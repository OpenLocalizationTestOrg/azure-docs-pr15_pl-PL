<properties
   pageTitle="Tworzenie i konfigurowanie bramy aplikacji z równoważenia obciążenia wewnętrzny (ILB) za pomocą Menedżera zasobów Azure | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowanie, Rozpoczynanie i usuwanie bramy Azure aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB) dla Menedżera zasobów Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB) za pomocą Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure czynności klasyczny](application-gateway-ilb.md)
- [Procedura programu PowerShell Menedżera zasobów](application-gateway-ilb-arm.md)

Azure bramy aplikacji można skonfigurować VIP Internecie lub punktu końcowego wewnętrznych, nie uwidaczniany z Internetem, nazywane także punktu końcowego usługi równoważenia (ILB) wewnętrznych ładowania. Konfigurowanie bramy przy użyciu ILB są przydatne w przypadku wewnętrznym aplikacjom programu LOB, które nie są dostępne w Internecie. Jest również przydatne w przypadku usług i aplikacji wielu poziomów, które znajdują się w granicę zabezpieczeń, który nie jest dostępny w Internecie, ale nadal wymaga okrężny ładowanie rozkładu, lepkość sesji lub zakończenie Secure Sockets Layer (SSL).

W tym artykule opisano czynności, aby skonfigurować bramy aplikacji z ILB.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [do pobrania strony](https://azure.microsoft.com/downloads/).
2. Tworzenie wirtualnych sieci i podsieci dla bramy aplikacji. Upewnij się, że nie maszyn wirtualnych lub we wdrożeniach w chmurze z podsieci. Brama aplikacji musi być samoczynnie w podsieci wirtualnej sieci.
3. Serwerów, które można konfigurować w celu za pomocą bramy aplikacji, musi istnieć lub przypisano ich punkty końcowe utworzonych w wirtualnej sieci lub z publicznej IP-VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co to jest wymagane do utworzenia bramy aplikacji?


- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do wirtualnej sieci, ale w innej podsieci bramy aplikacji lub powinny być publicznej IP-VIP.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Zewnętrzną portu:** Ten port jest publicznej otwarcia na bramie aplikacji. Ruch trafienia tego portu, a następnie przekierowaniem do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https są uwzględniania wielkości liter), a nazwa certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload).
- **Reguły:** Reguła powiąże odbiornika i puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika. Obecnie jest obsługiwana tylko *podstawowe* reguły. *Podstawowe* reguła jest dystrybucji obciążenia okrężny.



## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Różnica między przy użyciu klasycznej Azure i Menedżera zasobów Azure jest kolejność, w której można tworzyć bramy aplikacji oraz elementy, które trzeba skonfigurować.
Przy użyciu Menedżera zasobów wszystkie elementy, które ułatwiają bramy aplikacji skonfigurowane indywidualnie, a następnie umieszczony, aby utworzyć zasób bramy aplikacji.


Poniżej przedstawiono kroki, które są potrzebne do utworzenia bramy aplikacji:

1. Tworzenie grupy zasobów dla Menedżera zasobów
2. Tworzenie wirtualnych sieci i podsieci bramy aplikacji
3. Tworzenie obiektu konfiguracji bramy aplikacji
4. Tworzenie aplikacji bramy zasobów


## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla Menedżera zasobów

Upewnij się, czy przełączyć tryb programu PowerShell, aby użyć poleceń cmdlet Menedżera zasobów Azure. Dodatkowe informacje są dostępne po [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription

Zostanie wyświetlony monit o poświadczenia uwierzytelniania.<BR>

### <a name="step-3"></a>Krok 3

Wybranie Azure subskrypcji korzystać. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Krok 4

Tworzenie nowej grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. To jest używany jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia bramy aplikacji używa tej samej grupy zasobów.

W powyższym przykładzie możemy utworzyć grupę zasobów o nazwie "appgw rg" i lokalizacja "Zachód z NAMI".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie wirtualnych sieci i podsieci bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć wirtualną sieć za pomocą Menedżera zasobów:

### <a name="step-1"></a>Krok 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

W ten sposób 10.0.0.0/24 zakres adresów do zmiennej podsieci może być używany do tworzenia wirtualną sieć.

### <a name="step-2"></a>Krok 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Spowoduje to utworzenie wirtualną sieć o nazwie "appgwvnet" w zasobów grupy "appgw-rg" w regionie Zachód USA 10.0.0.0/16 prefiks za pomocą 10.0.0.0/24 podsieci.

### <a name="step-3"></a>Krok 3

    $subnet = $vnet.subnets[0]

W ten sposób obiektu podsieci zmiennej $subnet do następnych kroków.

## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji

### <a name="step-1"></a>Krok 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Spowoduje to utworzenie konfiguracji IP bramy dla aplikacji o nazwie "gatewayIP01". Po uruchomieniu aplikacji bramy przejmuje adres IP z podsieci skonfigurowane i kierowanie ruchu sieciowego do adresów IP w puli adresów IP wewnętrznej. Należy pamiętać, że każde wystąpienie ma jeden adres IP.


### <a name="step-2"></a>Krok 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

To konfiguruje puli adresów IP wewnętrznej o nazwie "pool01" z adresami IP "134.170.185.46, 134.170.188.221,134.170.185.50". Znajdują się adresów IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Zamień powyżej, aby dodać własne punkty końcowe adresów IP aplikacji adresów IP.

### <a name="step-3"></a>Krok 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Ruch sieciowy bramy ustawienie "poolsetting01" dla obciążenia strategie aplikacji to konfiguruje w puli wewnętrznej.

### <a name="step-4"></a>Krok 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

To konfiguruje zewnętrzną portu IP o nazwie "frontendport01" dla ILB.

### <a name="step-5"></a>Krok 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Tworzy zewnętrzną konfigurację IP o nazwie "fipconfig01", a przypisuje prywatne IP z bieżącej podsieci wirtualną sieć.

### <a name="step-6"></a>Krok 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Tworzy detektora o nazwie "listener01", a kojarzy frontonu port frontonu konfiguracji adresów IP.

### <a name="step-7"></a>Krok 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Spowoduje to utworzenie reguły routingu równoważenia obciążenia, o nazwie "rule01", która umożliwia skonfigurowanie zachowanie usługi równoważenia obciążenia.

### <a name="step-8"></a>Krok 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

To konfiguruje rozmiar wystąpienie bramy aplikacji.

>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Możesz wybrać między Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu AzureApplicationGateway nowy

Tworzy bramy aplikacji z wszystkie pozycje konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Spowoduje to utworzenie bramy aplikacji z wszystkie elementy konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".


## <a name="delete-an-application-gateway"></a>Usuwanie bramy aplikacji

Aby usunąć bramy aplikacji, musisz wykonać następujące czynności w kolejności:

1. Aby zatrzymać bramy, należy użyć polecenia cmdlet **AzureRmApplicationGateway tabulatora** .
2. Aby usunąć bramę, należy użyć polecenia cmdlet **AzureRmApplicationGateway Usuń** .
3. Upewnij się, że brama została usunięta przy użyciu polecenia cmdlet **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Krok 1

Uzyskaj obiekt bramy aplikacji i skojarzyć ją do zmiennej "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Użyj **AzureRmApplicationGateway Zatrzymaj** , aby zatrzymać bramy aplikacji. Pokazano polecenia cmdlet **AzureRmApplicationGateway tabulatora** w pierwszym wierszu, to następuje wynik.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Po bramy aplikacji jest w stanie zatrzymania, należy użyć polecenia cmdlet **AzureRmApplicationGateway Usuń** zamiar usunięcia usługi.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] **-Wymusić** przełącznik może być używany, aby pominąć komunikat potwierdzający Usuń.


Aby potwierdzić usunięcie usługi, można użyć polecenia cmdlet **Get-AzureRmApplicationGateway** . Ten krok nie jest wymagane.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz [Konfigurowanie bramy aplikacji dla protokołu SSL offload](application-gateway-ssl.md).

Jeśli chcesz skonfigurować bramy aplikacji do użytku z ILB, zobacz [Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)](application-gateway-ilb.md).

Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)
