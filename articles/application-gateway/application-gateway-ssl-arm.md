<properties
   pageTitle="Konfigurowanie bramy aplikacji dla odciążania SSL przy użyciu Menedżera zasobów Azure | Microsoft Azure"
   description="Ta strona zawiera instrukcje, aby utworzyć bramy aplikacji za pomocą protokołu SSL Odciążanie przy użyciu Menedżera zasobów Azure"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurowanie bramy aplikacji dla odciążania SSL przy użyciu Menedżera zasobów Azure

> [AZURE.SELECTOR]
-[Azure Portal](application-gateway-ssl-portal.md)
-[Azure PowerShell Menedżera zasobów](application-gateway-ssl-arm.md)
-[Azure klasycznego programu PowerShell](application-gateway-ssl.md)

 Aby zakończyć sesję Secure Sockets Layer (SSL) u bramy, aby uniknąć kosztów zadań odszyfrowywania SSL działania na farmie sieci web można skonfigurować Azure bramy aplikacji. Odciążanie SSL upraszcza zarządzanie aplikacji sieci web i konfiguracji serwera frontonu.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [do pobrania strony](https://azure.microsoft.com/downloads/).
2. Możesz utworzyć wirtualnej sieci i podsieci bramy aplikacji. Upewnij się, że nie maszyn wirtualnych lub we wdrożeniach w chmurze z podsieci. Brama aplikacji musi być samoczynnie w podsieci wirtualną sieć.
3. Serwery można konfigurować w celu za pomocą bramy aplikacji, musi istnieć lub przypisano ich punkty końcowe utworzonych w wirtualnej sieci lub z publicznej IP-VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co to jest wymagane do utworzenia bramy aplikacji?


- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do podsieci, wirtualną sieć lub powinny być publicznej IP-VIP.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Frontonu portu:** Ten port jest publicznej otwierany na bramie aplikacji. Ruch trafienia tego portu, a następnie jest kierowany do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https, te ustawienia są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload).
- **Reguły:** Reguła powiąże odbiornika i puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika. Obecnie jest obsługiwana tylko *podstawowe* reguły. *Podstawowe* reguła jest dystrybucji obciążenia okrężny.

**Dodatkowa konfiguracja notatek**

Konfiguracja certyfikatów SSL protokołu w **HttpListener** powinien zmienić się na *Https* (jest uwzględniana wielkość liter). **SslCertificate** element jest dodawany do **HttpListener** z wartością zmiennej skonfigurowane dla certyfikatu SSL. Port frontonu powinny być aktualizowane do 443.

**Umożliwiające koligacji systemem plików cookie**: bramy aplikacji można skonfigurować tak, aby upewnić się, że żądania z sesją zawsze jest kierowane do samej maszyn wirtualnych w farmie sieci web. W tym scenariuszu jest wykonywane przez uruchomienie cookie sesji, umożliwiająca bramy skierować ruch prawidłowo. Aby włączyć koligacji systemem plików cookie, ustaw **CookieBasedAffinity** *włączone* w elemencie **BackendHttpSettings** .


## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Różnica między przy użyciu Azure Klasyczny model wdrażania i Menedżera zasobów Azure jest kolejności, w jakiej Tworzenie bramy aplikacji oraz elementy, które trzeba skonfigurować.

Przy użyciu Menedżera zasobów wszystkie składniki bramy aplikacji są skonfigurowane indywidualnie, a następnie umieść ze sobą, aby utworzyć zasób bramy aplikacji.


Poniżej przedstawiono kroki potrzebne do utworzenia bramy aplikacji:

1. Tworzenie grupy zasobów dla Menedżera zasobów
2. Tworzenie wirtualnych sieci, podsieci i publiczny adres IP bramy aplikacji
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

Tworzenie grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. To ustawienie jest używany jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia bramy aplikacji używa tej samej grupy zasobów.

W powyższym przykładzie możemy utworzyć grupę zasobów o nazwie "appgw RG" i lokalizacja "Zachód z NAMI".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie wirtualnych sieci i podsieci bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć wirtualną sieć za pomocą Menedżera zasobów:

### <a name="step-1"></a>Krok 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

W tym przykładzie przypisuje 10.0.0.0/24 zakres adres zmienną podsieci do można użyć do utworzenia wirtualnej sieci.

### <a name="step-2"></a>Krok 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

W tym przykładzie tworzy wirtualną sieć o nazwie "appgwvnet" w zasobów grupy "appgw-rg" w regionie Zachód USA 10.0.0.0/16 prefiks za pomocą 10.0.0.0/24 podsieci.

### <a name="step-3"></a>Krok 3

    $subnet = $vnet.Subnets[0]

W tym przykładzie przypisuje obiektu podsieci zmiennej $subnet do następnych kroków.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publiczny adres IP frontonu konfiguracji

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

W tym przykładzie tworzy publicznej zasobu IP "publicIP01" w zasobów grupy "appgw-rg" w regionie Zachód USA.


## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji

### <a name="step-1"></a>Krok 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

W tym przykładzie zostanie utworzona konfiguracja IP bramy dla aplikacji o nazwie "gatewayIP01". Po uruchomieniu aplikacji bramy przejmuje adresu IP z podsieci skonfigurowane i kierowanie ruchu sieciowego do adresów IP w puli adresów IP wewnętrznej. Należy pamiętać, że każde wystąpienie ma jeden adres IP.

### <a name="step-2"></a>Krok 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

W tym przykładzie konfiguruje puli adresów IP wewnętrznej o nazwie "pool01" z adresami IP "134.170.185.46, 134.170.188.221,134.170.185.50." Wartości te są adresy IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Adresy IP z poprzedniego przykładu należy zastąpić adresy IP punktów końcowych aplikacji sieci web.

### <a name="step-3"></a>Krok 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

W tym przykładzie konfiguruje ustawienia bramy aplikacji "poolsetting01" do równoważenia obciążenia ruchu sieciowego w puli wewnętrznej.

### <a name="step-4"></a>Krok 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

W tym przykładzie konfiguruje frontonu portu IP o nazwie "frontendport01" dla punktu końcowego usługi publicznych adresów IP.

### <a name="step-5"></a>Krok 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

W tym przykładzie konfiguruje certyfikatu użytego dla połączenia SSL. Certyfikat musi się znajdować w formacie pfx, a hasło musi zawierać od 4 do 12 znaków.

### <a name="step-6"></a>Krok 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

W tym przykładzie tworzy zewnętrzną konfigurację IP o nazwie "fipconfig01" i kojarzy publiczny adres IP przy użyciu zewnętrzną konfiguracji adresów IP.

### <a name="step-7"></a>Krok 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


W tym przykładzie tworzy nazwę odbiornika "listener01" i kojarzy zewnętrzną portu zewnętrzną konfiguracji IP i certyfikat.

### <a name="step-8"></a>Krok 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

W tym przykładzie tworzy regułę routingu równoważenia obciążenia, o nazwie "rule01", która umożliwia skonfigurowanie zachowanie usługi równoważenia obciążenia.

### <a name="step-9"></a>Krok 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

W tym przykładzie konfiguruje rozmiar wystąpienie bramy aplikacji.

>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Możesz wybrać między Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu AzureApplicationGateway nowy

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

W tym przykładzie powoduje bramy aplikacji z wszystkie pozycje konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".

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

Jeśli chcesz skonfigurować bramy aplikacji do użytku z równoważenia obciążenia wewnętrzny (ILB), zobacz [Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)](application-gateway-ilb.md).

Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)
