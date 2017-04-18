<properties
   pageTitle="Konfigurowanie zapory aplikacji sieci Web w nowym lub istniejącym bramy aplikacji | Microsoft Azure"
   description="Ten artykuł zawiera wskazówki dotyczące sposobu uruchamiania za pomocą zapory aplikacji sieci web na istniejącym lub nowym bramy aplikacji."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurowanie zapory aplikacji sieci Web w nowym lub istniejącym bramy aplikacji

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-web-application-firewall-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-web-application-firewall-powershell.md)

Zapory aplikacji sieci web (WAF) w polu Brama aplikacji Azure chroni aplikacji sieci web z typowych atakami oparte na sieci web, takich jak uruchomienie SQL, atakami skryptów między witrynami i hijacks sesji.

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego. Aplikacji oferuje wiele funkcji aplikacji dostarczenia kontroler (ADC), w tym równoważenia obciążenia HTTP koligacji sesji plików cookie, Secure Sockets Layer (SSL) offload sondy niestandardowe zdrowia, obsługę wielu witryn i wiele innych. Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę omówienie bramy aplikacji

Ten artykuł pokazuje sposób [dodawania zapory aplikacji sieci web do istniejącej bramy aplikacji](#add-web-application-firewall-to-an-existing-application-gateway) i [Tworzenie bramy aplikacji, która korzysta z zapory aplikacji sieci web](#create-an-application-gateway-with-web-application-firewall).

![Scenariusz obrazu][scenario]

## <a name="waf-configuration-differences"></a>Różnice konfiguracji WAF

Jeśli podczas czytania [Tworzenie bramy aplikacji przy użyciu programu PowerShell](application-gateway-create-gateway-arm.md)rozumiesz ustawienia SKU, aby skonfigurować podczas tworzenia bramy aplikacji. WAF zawiera dodatkowe ustawienia, aby zdefiniować podczas konfigurowania używaną WERSJĘ bramy aplikacji. Nie istnieją żadne dodatkowe zmiany wprowadzone na samej bramie aplikacji.

**SKU** - normalnego zastosowania bramy nie obsługuje WAF **Standardowy\_małych**, **Standardowy\_średni**, i **Standardowy\_duży** rozmiarów. Wprowadzenie WAF, istnieją dwie dodatkowe SKU, **WAF\_średni** i **WAF\_duży**. WAF nie jest obsługiwane na małych aplikacji bramy.

**Warstwa** — **Standardowy** lub **WAF**są dostępne wartości. Gdy używasz zapory aplikacji sieci Web, musi być wybrana **WAF** .

**Tryb** — to ustawienie jest tryb WAF. dozwolone wartości są **Wykrywanie** i **Zapobieganie**. Po skonfigurowaniu WAF w trybie wykrywania, wszelkie zagrożenia są przechowywane w pliku dziennika. W tryb zapobiegania nadal są rejestrowane zdarzenia, ale tej otrzymuje 403 nieautoryzowanego odpowiedź z bramy aplikacji.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Dodatkowe informacje są dostępne po [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Zaloguj się do konta Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Wybierz subskrypcję, aby użyć w tym scenariuszu.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Krok 3

Pobieranie bramy, które dodajesz zapory aplikacji sieci Web.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Krok 4

Konfigurowanie sku zapory aplikacji sieci web. Dostępne rozmiary są **WAF\_duży** i **WAF\_średni**. Gdy używana jest Zapora aplikacji sieci web warstwie musi być **WAF**, muszą być potwierdzone możliwości, ustawiając używaną wersję.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Krok 5

Konfigurowanie ustawień WAF, zgodnie z definicją w poniższym przykładzie:

Dla ustawienia **WafMode** wartości dostępne są zapobiegania i wykrywania.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Krok 6

Zaktualizuj bramę aplikacji z ustawieniami określonymi w poprzednim kroku.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

To polecenie aktualizacji bramy aplikacji z zapory aplikacji sieci Web. Zalecane jest, aby wyświetlić [Diagnostyki bramy aplikacji](application-gateway-diagnostics.md) , aby dowiedzieć się, jak wyświetlać dzienniki Centrum aplikacji. Ze względu na zabezpieczenia rodzaj WAF dzienniki konieczne przejrzenia regularnie zrozumienie postawie zabezpieczeń aplikacji sieci web.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Tworzenie bramy aplikacji z zapory aplikacji sieci Web

Poniższe kroki przejście całego procesu od początku do końca Tworzenie bramy aplikacji za pomocą zapory aplikacji sieci Web.

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Krok 4

Tworzenie grupy zasobów (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. Lokalizacja ta jest używana jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia bramy aplikacji używa tej samej grupy zasobów.

W poprzednim przykładzie możemy utworzyć grupę zasobów o nazwie "appgw RG" i lokalizacja "Zachód z NAMI".

>[AZURE.NOTE] Jeśli musisz skonfigurować niestandardowe sonda Centrum aplikacji, zobacz [Tworzenie bramy aplikacji z sondy niestandardowych przy użyciu programu PowerShell](application-gateway-create-probe-ps.md). Zapoznaj się z [sondy niestandardowych i monitorowanie kondycji](application-gateway-probe-overview.md) więcej informacji.

### <a name="step-5"></a>Krok 5

Przypisywanie zakres adresów dla podsieci można używać samej bramy aplikacji.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podsieć dla aplikacji powinny mieć co najmniej 28 bitów maskę. Ta wartość pozostawia 10 dostępnych adresów w podsieć wystąpień bram aplikacji. Mniejsze podsieci nie można dodać więcej wystąpienie Centrum aplikacji w przyszłości.

### <a name="step-6"></a>Krok 6

Przypisywanie zakres adresów ma być używany dla puli adresów wewnętrznej bazy danych.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Krok 7

Tworzenie wirtualnej sieci z poprzedniego podsieci w grupie zasobów utworzony w kroku: [Tworzenie grup zasobów](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Krok 8

Pobieranie, wirtualną sieć zasobów i zasoby podsieci może być używany w następującej procedurze:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Krok 9

Tworzenie publicznej zasobu IP ma być używany dla bramy aplikacji. Ten publiczny adres IP jest używana w jednej z następujących czynności:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Brama aplikacji nie obsługuje stosowania publiczny adres IP utworzone za pomocą etykiety domeny zdefiniowane. Jest obsługiwana tylko publiczny adres IP z etykietą dynamicznie utworzony domeny. Jeśli wymagane dns przyjazną nazwę bramy aplikacji, zaleca się za pomocą rekord cname jako alias.

### <a name="step-10"></a>Krok 10

Przed utworzeniem bramy aplikacji, musisz skonfigurować wszystkie pozycje konfiguracji. Poniższe kroki Tworzenie pozycji konfiguracji, które są wymagane przez zasób bramy aplikacji.

Tworzenie aplikacji konfigurację IP bramy, to konfiguruje podsieci, jakie brama aplikacji używa. Po uruchomieniu aplikacji bramy przejmuje adres IP z podsieci skonfigurowane i kieruje ruch sieciowy do adresów IP w puli adresów IP wewnętrznej. Należy pamiętać, że każde wystąpienie ma jeden adres IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Krok 11

Konfigurowanie puli adresów IP wewnętrznej przy użyciu adresów IP serwerów wewnętrznej bazy danych sieci web. Te adresy IP są adresy IP, które odbieranie ruchu sieciowego, pochodzącej z zewnętrzną końcowy adresów IP. Możesz zastąpić następujące adresy IP, aby dodać własne punkty końcowe adres IP aplikacji.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Krok 12

Przekazywanie certyfikatu może być używany na ssl włączone wewnętrznej bazy danych puli zasobów.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Kroku 13

Konfigurowanie ustawień protokołu http wewnętrznej bramy aplikacji. Przypisz certyfikat przekazane w poprzednim kroku ustawienia protokołu http.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Krok 14

Skonfiguruj zewnętrzną port IP dla punktu końcowego usługi publicznych adresów IP. Ten port jest numer portu, którego użytkownicy połączenie.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Krok 15

Tworzenie zewnętrzną konfiguracji adresów IP, to ustawienie mapy adresu ip prywatny lub publiczny do zewnętrznej bramy aplikacji. Następny krok przypisuje publiczny adres IP w poprzednim kroku zewnętrzną konfiguracji adresów IP.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Krok 16

Konfigurowanie certyfikatu bramy aplikacji. Ten certyfikat jest używany odszyfrowywanie i ponownie zaszyfrować ruch na bramie aplikacji.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Krok 17

Tworzenie odbiornik HTTP bramy aplikacji. Przypisywanie zewnętrzną ip konfiguracji, port i ssl certyfikat.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Krok 18

Utwórz regułę routingu równoważenia obciążenia, która konfiguruje zachowanie usługi równoważenia obciążenia. W tym przykładzie jest tworzona reguła podstawowe okrężnego.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Krok 19

Konfigurowanie rozmiaru wystąpienia bramy aplikacji.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Możesz wybrać jeden z **WAF\_średni** i **WAF\_duży**, warstwie przy użyciu WAF jest zawsze **WAF**. Wydajność jest liczbą od 1 do 10.

### <a name="step-20"></a>Krok 20

Konfigurowanie trybu dla WAF, dopuszczalne wartości są **zapobiegania** i **wykrywania**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Krok 21

Tworzenie bramy aplikacji z wszystkie pozycje konfiguracji z powyższych kroków. W tym przykładzie bramy aplikacji jest nazywane "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Dowiedz się, jak skonfigurować rejestrowanie diagnostyczne logowania zdarzenia, które wykryty lub braku możliwości zapory aplikacji sieci Web, odwiedź witrynę [Aplikacji bramy diagnostyki](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png