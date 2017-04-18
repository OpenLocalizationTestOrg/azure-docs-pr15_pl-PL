<properties
   pageTitle="Tworzenie, Rozpoczynanie i usuwanie bramy aplikacji przy użyciu Menedżera zasobów Azure | Microsoft Azure"
   description="Ta strona zawiera instrukcje tworzenie, konfigurowanie i rozpoczynanie oraz usuwanie bramy aplikacji Azure za pomocą Menedżera zasobów Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Tworzenie, Rozpoczynanie i usuwanie bramy aplikacji przy użyciu Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-gateway-arm.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-gateway.md)
- [Azure szablonu Menedżera zasobów](application-gateway-create-gateway-arm-template.md)
- [Polecenie Azure](application-gateway-create-gateway-cli.md)

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego. Brama aplikacji oferuje wiele funkcji aplikacji dostarczenia kontroler (ADC), w tym równoważenia obciążenia HTTP koligacji sesji plików cookie, Secure Sockets Layer (SSL) offload sondy niestandardowe kondycji, obsługę wielu witryn i wiele innych. Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Omówienie bramy aplikacji](application-gateway-introduction.md)

W tym artykule opisano czynności, aby utworzyć, konfigurowanie, uruchom i Usuń bramy aplikacji.

>[AZURE.IMPORTANT] Przed rozpoczęciem pracy z zasobami Azure jest pamiętać, że Azure obecnie ma dwa modele wdrażania: Menedżer zasobów i klasyczny. Upewnij się, opis [modeli wdrażania i narzędzi](../azure-classic-rm.md) przed rozpoczęciem pracy z dowolną Azure zasobów. Możesz wyświetlić dokumentację dla różnych narzędzi, klikając pozycję kart u góry tego artykułu. W tym dokumencie opisano tworzenie bramy aplikacji za pomocą Menedżera zasobów Azure. Aby użyć klasycznej wersji, przejdź do artykułu [Tworzenie wdrażanie klasyczny bramy aplikacji przy użyciu programu PowerShell](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [do pobrania strony](https://azure.microsoft.com/downloads/).
2. Jeśli masz istniejące wirtualnej sieci, wybierz istniejącą podsieć puste albo Tworzenie podsieci w istniejącej sieci wirtualnej wyłącznie do użytku przez bramę aplikacji. Nie można wdrożyć aplikację bramę do innego wirtualną sieć niż zasoby, które mają być wdrażanie za bramą aplikacji.
3. Serwerów, które można konfigurować w celu za pomocą bramy aplikacji, musi istnieć lub przypisano ich punkty końcowe utworzonych w wirtualnej sieci lub z publicznej IP-VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co to jest wymagane do utworzenia bramy aplikacji?

- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do podsieci, wirtualną sieć lub powinny być publicznej IP-VIP.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Zewnętrzną portu:** Ten port jest port publicznej, który zostanie otwarty w aplikacji bramy. Ruch trafienia tego portu, a następnie przekierowaniem do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https, wartości te są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload).
- **Reguły:** Reguła powiąże odbiornika puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Różnica między przy użyciu klasycznej Azure i Menedżera zasobów Azure jest kolejność, w której można tworzyć bramy aplikacji oraz elementy, które trzeba skonfigurować.

Przy użyciu Menedżera zasobów wszystkie elementy, które ułatwiają bramy aplikacji są skonfigurowane indywidualnie, a następnie umieść ze sobą, aby utworzyć zasób bramy aplikacji.

Poniżej przedstawiono kroki, które są potrzebne do utworzenia bramy aplikacji.

## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla Menedżera zasobów

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Dodatkowe informacje są dostępne po [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Zaloguj się do Azure

    Login-AzureRmAccount

Zostanie wyświetlony monit o poświadczenia uwierzytelniania.

### <a name="step-2"></a>Krok 2

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription

### <a name="step-3"></a>Krok 3

Wybranie Azure subskrypcji korzystać.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Krok 4

Tworzenie grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. Lokalizacja ta jest używana jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia bramy aplikacji używa tej samej grupy zasobów.

W powyższym przykładzie możemy utworzyć grupę zasobów o nazwie "appgw RG" i lokalizacja "Zachód z NAMI".

>[AZURE.NOTE] Jeśli musisz skonfigurować niestandardowe sonda Centrum aplikacji, zobacz [Tworzenie bramy aplikacji z sondy niestandardowych przy użyciu programu PowerShell](application-gateway-create-probe-ps.md). Zapoznaj się z [sondy niestandardowe i monitorowanie kondycji](application-gateway-probe-overview.md) więcej informacji.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie wirtualnych sieci i podsieci bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć wirtualną sieć za pomocą Menedżera zasobów.

### <a name="step-1"></a>Krok 1

Przypisz 10.0.0.0/24 zakres adresów do zmiennej podsieci można użyć do utworzenia wirtualnej sieci.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Krok 2

Tworzenie wirtualnych sieci o nazwie "appgwvnet" w zasobów grupy "appgw-rg" w regionie Zachód USA 10.0.0.0/16 prefiks za pomocą 10.0.0.0/24 podsieci.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Krok 3

Przypisz zmiennej podsieci następne kroki, które tworzą bramy aplikacji.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publiczny adres IP zewnętrzną konfiguracji

Tworzenie publicznej zasobu IP "publicIP01" w zasobów grupy "appgw-rg" w regionie Zachód USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji

Przed utworzeniem bramy aplikacji, musisz skonfigurować wszystkie pozycje konfiguracji. Poniższe kroki Tworzenie pozycji konfiguracji, które są wymagane przez zasób bramy aplikacji.

### <a name="step-1"></a>Krok 1

Tworzenie konfiguracji IP bramy dla aplikacji o nazwie "gatewayIP01". Po uruchomieniu aplikacji bramy przejmuje adres IP z podsieci skonfigurowane i kieruje ruch sieciowy do adresów IP w puli adresów IP wewnętrznej. Należy pamiętać, że każde wystąpienie ma jeden adres IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Krok 2

Konfigurowanie puli adresów IP wewnętrznej o nazwie "pool01" z adresami IP "134.170.185.46, 134.170.188.221,134.170.185.50". Te adresy IP są adresy IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Zamień poprzedniego adresy IP, aby dodać własne punkty końcowe adres IP aplikacji.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Krok 3

Skonfiguruj ustawienia bramy aplikacji "poolsetting01" dla ruchu sieciowego równoważenia obciążenia w puli wewnętrznej.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Krok 4

Skonfiguruj zewnętrzną port IP o nazwie "frontendport01" dla punktu końcowego usługi publicznych adresów IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Krok 5

Tworzenie zewnętrzną konfigurację IP o nazwie "fipconfig01" i kojarzenie publiczny adres IP z zewnętrzną konfiguracji adresów IP.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Krok 6

Utwórz odbiornik nazwę "listener01" i skojarz zewnętrzną port zewnętrzną konfiguracji adresów IP.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Krok 7

Utwórz regułę routingu równoważenia obciążenia, o nazwie "rule01", która umożliwia skonfigurowanie zachowanie usługi równoważenia obciążenia.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Krok 8

Konfigurowanie rozmiaru wystąpienia bramy aplikacji.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Możesz wybrać między Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu AzureRmApplicationGateway nowy

Tworzenie bramy aplikacji z wszystkie pozycje konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Krok 9

Pobieranie szczegółów DNS i VIP bramy aplikacji z publicznej zasobu IP dołączone do bramy aplikacji.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Usuwanie bramy aplikacji

Aby usunąć bramy aplikacji, wykonaj następujące kroki:

### <a name="step-1"></a>Krok 1

Uzyskaj obiekt bramy aplikacji i skojarzyć ją do zmiennej "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Użyj **AzureRmApplicationGateway Zatrzymaj** , aby zatrzymać bramy aplikacji.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Po bramy aplikacji jest w stanie zatrzymania, należy użyć polecenia cmdlet **AzureRmApplicationGateway Usuń** zamiar usunięcia usługi.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] **-Wymusić** przełącznik może być używany, aby pominąć komunikat potwierdzający Usuń.

Aby potwierdzić usunięcie usługi, można użyć polecenia cmdlet **Get-AzureRmApplicationGateway** . Ten krok nie jest wymagane.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>Uzyskaj nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie komunikacji zewnętrznej. Gdy używasz publiczny adres IP, brama aplikacji wymaga przypisywany dynamicznie nazwa DNS, która nie jest przyjazny. Aby upewnić się, że użytkownicy końcowi naciśnij klawisz aplikacji bramy rekord CNAME może służyć do wskaż punkt końcowy publicznej bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). W tym celu należy pobrać szczegóły bramy aplikacji i jej nazwę skojarzonej IP/DNS za pomocą elementu PublicIPAddress dołączone do bramy aplikacji. Należy użyć nazwy DNS bramy aplikacji do utworzenia rekordu CNAME, która wskazuje aplikacji sieci web dwóch do tej nazwy DNS. Użyj rekordów A nie jest zalecane, ponieważ VIP może się zmienić po ponownym uruchomieniu aplikacji bramy.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz [Konfigurowanie bramy aplikacji dla protokołu SSL offload](application-gateway-ssl.md).

Jeśli chcesz skonfigurować bramy aplikacji do użytku z równoważenia obciążenia wewnętrznych, zobacz [Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)](application-gateway-ilb.md).

Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)
