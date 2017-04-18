<properties
   pageTitle="Tworzenie niestandardowego sondy bramy aplikacji przy użyciu programu PowerShell w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak utworzyć niestandardowe sondy bramy aplikacji przy użyciu programu PowerShell w Menedżerze zasobów"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Tworzenie niestandardowych sondy Azure aplikacji bramy przy użyciu programu PowerShell dla Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-probe-ps.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Krok 1

Uwierzytelnianie za pomocą AzureRmAccount logowania.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription

### <a name="step-3"></a>Krok 3

Wybranie Azure subskrypcji korzystać. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Krok 4

Tworzenie grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. Lokalizacja ta jest używana jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia bramy aplikacji za pomocą tej samej grupy zasobów.

W powyższym przykładzie możemy utworzyć grupę zasobów o nazwie "appgw RG" i lokalizacja "Zachód z NAMI".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie wirtualnych sieci i podsieci bramy aplikacji

Poniższe czynności Tworzenie wirtualnych sieci i podsieci bramy aplikacji.

### <a name="step-1"></a>Krok 1


Przypisz 10.0.0.0/24 zakres adres zmiennej podsieci może być używany do tworzenia wirtualnej sieci.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Krok 2

Tworzenie wirtualnych sieci o nazwie "appgwvnet" w zasobów grupy "appgw-rg" w regionie Zachód USA 10.0.0.0/16 prefiks za pomocą 10.0.0.0/24 podsieci.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Krok 3

Przypisz zmiennej podsieci następne kroki, które tworzą bramy aplikacji.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publiczny adres IP frontonu konfiguracji


Tworzenie publicznej zasobu IP "publicIP01" w zasobów grupy "appgw-rg" w regionie Zachód USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Tworzenie obiektu konfiguracji bramy aplikacji przy użyciu niestandardowej sondy

Przed utworzeniem bramy aplikacji możesz skonfigurować wszystkie pozycje konfiguracji. Poniższe kroki Tworzenie pozycji konfiguracji, które są wymagane przez zasób bramy aplikacji.

### <a name="step-1"></a>Krok 1

Tworzenie konfiguracji IP bramy dla aplikacji o nazwie "gatewayIP01". Po uruchomieniu aplikacji bramy przejmuje adresu IP z podsieci skonfigurowane i kierowanie ruchu sieciowego do adresów IP w puli adresów IP wewnętrznej. Należy pamiętać, że każde wystąpienie ma jeden adres IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Krok 2


Konfigurowanie puli adresów IP wewnętrznej o nazwie "pool01" z adresami IP "134.170.185.46, 134.170.188.221,134.170.185.50". Wartości te są adresy IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Zamień powyżej, aby dodać własne punkty końcowe adres IP aplikacji adresów IP.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Krok 3


Niestandardowe sondy jest skonfigurowany w tym kroku.

Parametry używane są następujące:

- **Interwał** - konfiguruje testy interwału sondy w sekundach.
- **Przekroczono limit czasu** — określa limit sondy sprawdzanie odpowiedzi HTTP.
- **-Hostname i ścieżki** - pełnego adresu URL ścieżkę, która jest wywoływana przez bramę aplikacji w celu ustalenia stanu wystąpienia. Na przykład jeśli masz http://contoso.com/ witryny sieci Web, następnie sondy niestandardowej można skonfigurować dla "http://contoso.com/path/custompath.htm" kontroli sondy ma pomyślną odpowiedź HTTP.
- **UnhealthyThreshold** — liczba negatywnych odpowiedzi HTTP, aby oznaczyć flagą wystąpienia wewnętrznej jako *nieprawidłowe*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Krok 4

Konfigurowanie ustawień bramy aplikacji "poolsetting01" dla ruchu w puli wewnętrznej. Ten krok ma także konfiguracji limitu czasu, czyli odpowiedź wewnętrznej puli aplikacji żądanie bramy. Gdy odpowiedź wewnętrznej trafienia limitu, Application Gateway anuluje żądanie. Ta wartość różni się od sondy limitu czasu, przeznaczone tylko do odpowiedzi wewnętrznej na sondy kontroli.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Krok 5

Skonfiguruj zewnętrzną port IP o nazwie "frontendport01" dla punktu końcowego usługi publicznych adresów IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Krok 6

Tworzenie zewnętrzną konfigurację IP o nazwie "fipconfig01" i skojarzyć publiczny adres IP zewnętrzną konfiguracji adresów IP.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Krok 7

Utwórz odbiornik nazwę "listener01" i skojarz frontonu port frontonu konfiguracji adresów IP.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Krok 8

Utwórz regułę routingu równoważenia obciążenia, o nazwie "rule01", która umożliwia skonfigurowanie zachowanie usługi równoważenia obciążenia.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Krok 9

Konfigurowanie rozmiaru wystąpienia bramy aplikacji.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Możesz wybrać między Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu AzureRmApplicationGateway nowy

Tworzenie bramy aplikacji z wszystkie pozycje konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Dodawanie sondy do bramy istniejącej aplikacji

Masz cztery kroki, aby dodać niestandardowe sondy do istniejącej bramy aplikacji.

### <a name="step-1"></a>Krok 1

Ładowanie zasobu bramy aplikacji do zmiennej programu PowerShell przy użyciu **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Dodawanie sondy do istniejącej konfiguracji bramy.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


W tym przykładzie niestandardowe sondy jest skonfigurowany do wyszukania contoso.com/path/custompath.htm ścieżkę adresu URL co 30 sekund. Limit czasu wynoszącej 120 sekund skonfigurowano maksymalna liczba 8 żądań sondy nie powiodło się.

### <a name="step-3"></a>Krok 3

Dodawanie sondy do puli wewnętrznej konfiguracji oraz limit czasu przy użyciu **— Ustawianie-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Krok 4

Zapisywanie konfiguracji bramy aplikacji przy użyciu **Zestawu AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Usuwanie sondy z istniejących bramy aplikacji

Poniżej przedstawiono czynności, aby usunąć niestandardowy sondy z bramy istniejącej aplikacji.

### <a name="step-1"></a>Krok 1

Ładowanie zasobu bramy aplikacji do zmiennej programu PowerShell przy użyciu **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Krok 2

Usuwanie konfiguracji sondy z bramy aplikacji przy użyciu **AzureRmApplicationGatewayProbeConfig Usuń**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Krok 3

Aktualizowanie puli wewnętrznej ustawienia, aby usunąć sondy i ustawienie limitu czasu przy użyciu **— Ustawianie – AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Krok 4

Zapisywanie konfiguracji bramy aplikacji przy użyciu **Zestawu AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Uzyskaj nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie komunikacji zewnętrznej. Gdy używasz publiczny adres IP, brama aplikacji wymaga przypisywany dynamicznie nazwa DNS, która nie jest przyjazny. Aby upewnić się, że użytkownicy końcowi naciśnij klawisz aplikacji bramy rekord CNAME może służyć do wskaż punkt końcowy publicznej bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). W tym celu należy pobrać szczegóły bramy aplikacji i jego skojarzony nazwy IP/DNS za pomocą elementu PublicIPAddress dołączone do bramy aplikacji. Należy użyć nazwy DNS bramy aplikacji do utworzenia rekordu CNAME, która wskazuje aplikacji sieci web dwóch do tej nazwy DNS. Użyj rekordów A nie jest zalecane, ponieważ VIP może się zmienić po ponownym uruchomieniu aplikacji bramy.
    
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

Dowiedz się, jak skonfigurować odciążanie protokołu SSL, odwiedź witrynę [Skonfigurować odciążanie protokołu SSL](application-gateway-ssl-arm.md)

