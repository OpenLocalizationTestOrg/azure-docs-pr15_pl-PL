<properties
    pageTitle="Konfigurowanie zasad protokołu SSL i kompleksowego SSL z bramy aplikacji | Microsoft Azure"
    description="Ten artykuł zawiera opis sposobu konfigurowania SSL kompleksowego bramy aplikacji przy użyciu programu PowerShell Menedżera zasobów Azure"
    services="application-gateway"
    documentationCenter="na"
    authors="georgewallace"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="application-gateway"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="gwallace"/>

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurowanie zasad protokołu SSL i kompleksowego SSL z bramy aplikacji przy użyciu programu PowerShell

## <a name="overview"></a>Omówienie

Brama aplikacji obsługuje szyfrowanie kompleksowego ruchu. Aplikacja bramy powinien się tym zająć przez zakończenie połączenia SSL na bramy aplikacji. Następnie bramy dotyczy reguł rozsyłania ruchu, ponownie szyfrowanie pakietu i przesyła pakiet do odpowiedniej wewnętrznej bazy danych na podstawie reguł rozsyłania zdefiniowane. Odpowiedzi na serwerze sieci web przechodzi przez ten sam proces powrót do użytkownika końcowego.

Funkcja innej bramy tej aplikacji obsługuje wyłączanie niektórych wersji protokołu SSL. Brama aplikacji obsługuje wyłączenie następujących wersji Protocol (protokół); TLSv1.0, TLSv1.1 i TLSv1.2.

> [AZURE.NOTE] SSL 2.0 i SSL 3.0 są domyślnie wyłączone i nie można włączyć. Są traktowane jako niezabezpieczoną i mogą nie być używane z aplikacji bramy

![Scenariusz obrazu][scenario]

## <a name="scenario"></a>Scenariusz

W tym scenariuszu możesz dowiedzieć się, jak utworzyć bramy aplikacji przy użyciu protokołu SSL kompleksowego przy użyciu programu PowerShell.

W tym scenariuszu będzie:

- Utwórz grupę zasobów o nazwie "appgw rg"
- Tworzenie wirtualnych sieci o nazwie "appgwvnet" z zastrzeżone blok CIDR 10.0.0.0/16.
- Tworzenie dwóch podsieci o nazwie "appgwsubnet" i "appsubnet".
- Tworzenie bramy mała aplikacja pomocniczych szyfrowania SSL kompleksowego, która powoduje wyłączenie niektórych protokoły SSL.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby skonfigurować kompleksowego SSL z bramy aplikacji, certyfikat jest wymagany dla bramy i certyfikaty są wymagane dla serwerów wewnętrznej bazy danych. Certyfikat bramy jest używany do szyfrowania i odszyfrowywania ruch wysłany do niej przy użyciu protokołu SSL. Certyfikat bramy musi mieć format wymiana informacji osobistych (pfx). W tym formacie plików pozwala na klucz prywatny do wyeksportowania, które są wymagane przez bramę aplikacji do wykonania do szyfrowania i odszyfrowywania ruchu.

Na potrzeby szyfrowania ssl kompleksowego wewnętrznej bazy danych musi być whitelisted aplikacji bramy. Jest to, przekazując publicznej certyfikat pomocą bramy aplikacji. Dzięki temu, że brama aplikacji tylko komunikuje wystąpienia znane wewnętrznej bazy danych. To jeszcze bardziej zabezpiecza komunikacji kompleksowego.

Ten proces został opisany w następujące czynności:

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

W tej sekcji przeprowadzi Cię przez proces tworzenia grupy zasobów, zawierającą bramę aplikacji.

### <a name="step-1"></a>Krok 1

Zaloguj się do konta Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Wybierz subskrypcję, aby użyć w tym scenariuszu.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Krok 3

Tworzenie grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie wirtualnych sieci i podsieci bramy aplikacji

Poniższy przykład tworzy wirtualnej sieci i dwóch podsieci. Jedna podsieć służy do przechowywania bramy aplikacji. Inne podsieci jest używana do pomocą hostingu aplikacji sieci web.

### <a name="step-1"></a>Krok 1

Przypisywanie zakres adresów dla podsieci można używać samej bramy aplikacji.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podsieci skonfigurowane dla bramy aplikacji należy prawidłowo rozmiar. Bramy aplikacji można skonfigurować dla maksymalnie 10 wystąpień. Każde wystąpienie ma adres IP 1 z podsieci. Zbyt małą podsieci może negatywnie wpłynąć na Skalowanie zewnętrzne bramy aplikacji.

### <a name="step-2"></a>Krok 2

Przypisywanie zakres adresów ma być używany dla puli adresów wewnętrznej bazy danych.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Krok 3

Tworzenie wirtualnych sieci z podsieci określone w poprzednich krokach.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Krok 4

Pobieranie, wirtualną sieć zasobów i zasoby podsieci może być używany w następującej procedurze:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publiczny adres IP zewnętrzną konfiguracji

Tworzenie publicznej zasobu IP ma być używany dla bramy aplikacji. Ten publiczny adres IP jest używane następujące czynności.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Brama aplikacji nie obsługuje stosowania publiczny adres IP utworzone za pomocą etykiety domeny zdefiniowane. Jest obsługiwana tylko publiczny adres IP z etykietą dynamicznie utworzony domeny. Jeśli wymagane dns przyjazną nazwę bramy aplikacji, zaleca się za pomocą rekord cname jako alias.

## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji

Przed utworzeniem bramy aplikacji, musisz skonfigurować wszystkie pozycje konfiguracji. Poniższe kroki Tworzenie pozycji konfiguracji, które są wymagane przez zasób bramy aplikacji.

### <a name="step-1"></a>Krok 1

Tworzenie aplikacji konfigurację IP bramy, to ustawienie konfiguruje podsieci, jakie brama aplikacji używa. Po uruchomieniu aplikacji bramy przejmuje z podsieci skonfigurowane adresu IP i kieruje ruch sieciowy do adresów IP w puli wewnętrznej IP. Należy pamiętać, że każde wystąpienie ma jeden adres IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Krok 2

Tworzenie zewnętrzną konfiguracji adresów IP, to ustawienie mapy adresu ip prywatny lub publiczny do zewnętrznej bramy aplikacji. Następny krok przypisuje publiczny adres IP w poprzednim kroku zewnętrzną konfiguracji adresów IP.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Krok 3

Konfigurowanie puli adresów IP wewnętrznej przy użyciu adresów IP serwerów wewnętrznej bazy danych sieci web. Te adresy IP są adresy IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Możesz zastąpić następujące adresy IP, aby dodać własne punkty końcowe adres IP aplikacji.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] W pełni kwalifikowaną nazwę domeny (FQDN) jest również prawidłową wartość zamiast adresu ip dla serwerów wewnętrznej bazy danych za pomocą przełącznika - BackendFqdns.

### <a name="step-4"></a>Krok 4

Skonfiguruj zewnętrzną port IP dla punktu końcowego usługi publicznych adresów IP. Ten port jest numer portu, którego użytkownicy połączenie.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Krok 5

Konfigurowanie certyfikatu bramy aplikacji. Ten certyfikat jest używany odszyfrowywanie i ponownie zaszyfrować ruch na bramie aplikacji.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] W tym przykładzie konfiguruje certyfikatu użytego dla połączenia SSL. Certyfikat musi się znajdować w formacie pfx, a hasło musi zawierać od 4 do 12 znaków.

### <a name="step-6"></a>Krok 6

Tworzenie odbiornik HTTP bramy aplikacji. Przypisywanie zewnętrzną ip konfiguracji, port i ssl certyfikat.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Krok 7

Przekazywanie certyfikatu może być używany na ssl włączone wewnętrznej bazy danych puli zasobów.

> [AZURE.NOTE] Sonda domyślne pobiera klucz publiczny z powiązanie SSL **domyślny** adres IP wewnętrznej firmy i porównanie wartość klucza publicznego otrzymywanych na wartość klucza publicznego podane tutaj. Klucz publiczny pobrane może być zawsze zamierzonego witryny, do którego ruch będzie przepływał, **Jeśli** korzystasz z nagłówków hosta i SNI na wewnętrznej. W razie wątpliwości, odwiedź stronę https://127.0.0.1/ na końcach wstecz o potwierdzenie, który certyfikat jest używany dla powiązanie SSL **domyślny** . Użyj klucz publiczny z tego wniosek w tej sekcji. Jeśli korzystasz z nagłówków hosta i SNI od powiązań HTTPS, a nie otrzymasz odpowiedź i certyfikat od żądania przeglądarki ręcznego do https://127.0.0.1/ na końcach Wstecz, możesz skonfigurować powiązanie SSL domyślne na końcach Wstecz. Jeśli nie zrobisz, sondy zakończy się niepowodzeniem i wewnętrznej można nie będą whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Certyfikat opisane w tym kroku należy klucz publiczny certyfikatu pfx Prezentuj do wewnętrznej bazy danych. Eksportowanie certyfikatu (nie certyfikat główny) zainstalowane na serwerze wewnętrznej bazy danych. CER formatowanie i używać go w tym kroku. Ten krok whitelists wewnętrznej bazy danych z bramy aplikacji.

### <a name="step-8"></a>Krok 8

Konfigurowanie ustawień protokołu http wewnętrznej bramy aplikacji. Przypisz certyfikat przekazane w poprzednim kroku ustawienia protokołu http.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Krok 9

Utwórz regułę routingu równoważenia obciążenia, która konfiguruje zachowanie usługi równoważenia obciążenia. W tym przykładzie jest tworzona reguła podstawowe okrężnego.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Krok 10

Konfigurowanie rozmiaru wystąpienia bramy aplikacji.  Są dostępne formaty **Standardowy\_małych**, **Standardowy\_średni**, i **Standardowy\_duży**.  Wydajność dostępne wartości są od 1 do 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Wystąpienia liczba 1 jest dostępny do celów testowych. Należy wiedzieć, że wszelkie licznik wystąpień w dwóch przypadkach nie są objęte umowa dotycząca poziomu usług i dlatego nie zaleca się. Małe bramy są ma być używany dla deweloperów przetestuj, a nie do celów produkcyjnych.

### <a name="step-11"></a>Krok 11

Konfigurowanie zasad protokołu SSL do użytku na bramie aplikacji. Brama aplikacji obsługuje możliwość wyłączenia niektórych wersji protokołu SSL.

Poniżej przedstawiono wartości listy wersji Protocol (protokół), które mogą być wyłączone.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Poniższy przykład wyłącza TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Tworzenie bramy aplikacji

Za pomocą powyższych kroków, Tworzenie bramy aplikacji. Tworzenie bramy jest procesem długotrwałych.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Wyłączanie wersji protokołu SSL na istniejące bramy aplikacji

Poprzednie kroki wykonać podczas tworzenia aplikacji za pomocą protokołu ssl kompleksowe i wyłączanie niektórych wersji protokołu SSL. Poniższy przykład wyłącza pewnych zasad protokołu SSL dla istniejących bramy aplikacji.

### <a name="step-1"></a>Krok 1

Pobieranie aplikacji bramę do aktualizacji.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Krok 2

Opracowanie zasad protokołu SSL. W poniższym przykładzie TLSv1.0 i TLSv1.1 są wyłączone.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Krok 3

Na koniec zaktualizuj bramę. Należy zauważyć, że ten ostatni krok jest długotrwałych zadania. Po zakończeniu, kompleksowego ssl jest skonfigurowany dla bramy aplikacji.

    $gw | Set-AzureRmApplicationGateway

## <a name="get-application-gateway-dns-name"></a>Uzyskaj nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie komunikacji zewnętrznej. Gdy używasz publiczny adres IP, brama aplikacji wymaga przypisywany dynamicznie nazwa DNS, która nie jest przyjazny. Aby upewnić się, że użytkownicy końcowi naciśnij klawisz aplikacji bramy rekord CNAME może służyć do wskaż punkt końcowy publicznej bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). W tym celu należy pobrać szczegóły bramy aplikacji i jego skojarzony nazwy IP/DNS za pomocą elementu PublicIPAddress dołączonym do bramy aplikacji. Należy użyć nazwy DNS bramy aplikacji do utworzenia rekordu CNAME, która wskazuje aplikacji sieci web dwóch do tej nazwy DNS. Użyj rekordów A nie jest zalecane, ponieważ VIP może się zmienić po ponownym uruchomieniu aplikacji bramy.
    
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

Dowiedz się więcej o Wzmacnianie zabezpieczeń aplikacji sieci web z zapory aplikacji sieci Web przez bramę aplikacji na stronie [Omówienie zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
