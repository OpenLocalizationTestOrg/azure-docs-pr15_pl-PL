<properties
   pageTitle="Tworzenie, Rozpoczynanie i usuwanie bramy aplikacji | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowanie, Rozpoczynanie i usuwanie bramy Azure aplikacji"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Tworzenie, Rozpoczynanie i usuwanie bramy aplikacji

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-gateway-arm.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-gateway.md)
- [Azure szablonu Menedżera zasobów](application-gateway-create-gateway-arm-template.md)
- [Polecenie Azure](application-gateway-create-gateway-cli.md)

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego. Brama aplikacji oferuje wiele funkcji aplikacji dostarczenia kontroler (ADC), w tym równoważenia obciążenia HTTP koligacji sesji plików cookie, Secure Sockets Layer (SSL) offload sondy zdrowia niestandardowe, obsługę wielu witryn i wielu innych. Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Omówienie bramy aplikacji](application-gateway-introduction.md)

W tym artykule opisano czynności, aby utworzyć, konfigurowanie, uruchom i Usuń bramy aplikacji.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [do pobrania strony](https://azure.microsoft.com/downloads/).
2. Jeśli masz istniejące wirtualnej sieci, wybierz istniejącą podsieć puste lub tworzenie nowych podsieci w istniejącej sieci wirtualne wyłącznie do użytku przez bramę aplikacji. Nie można wdrożyć bramę aplikacji do innej sieci wirtualnej niż zasoby, które chcesz wdrożyć za bramą aplikacji, chyba że zaglądanie vnet jest używana. Aby dowiedzieć się więcej odwiedź stronę [Zaglądanie Vnet](../virtual-network/virtual-network-peering-overview.md)
3. Upewnij się, że masz wirtualnej sieci pracy z prawidłową podsieci. Upewnij się, że nie maszyn wirtualnych lub we wdrożeniach w chmurze z podsieci. Brama aplikacji musi być samoczynnie w podsieci wirtualnej sieci.
3. Serwerów, które można konfigurować w celu za pomocą bramy aplikacji, musi istnieć lub przypisano ich punkty końcowe utworzonych w wirtualnej sieci lub z publicznej IP-VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co to jest wymagane do utworzenia bramy aplikacji?

Użycie polecenia **Nowy AzureApplicationGateway** utworzyć bramy aplikacji, konfiguracja nie jest ustawiona na tym etapie i nowo utworzony zasób są skonfigurowane przy użyciu XML lub obiekt konfiguracji.

Dostępne są następujące wartości:

- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do podsieci, wirtualną sieć lub powinny być publicznej IP-VIP.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Frontonu portu:** Ten port jest publicznej otwierany na bramie aplikacji. Ruch trafienia tego portu, a następnie jest kierowany do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https, wartości te są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload).
- **Reguły:** Reguła powiąże odbiornika i puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Aby utworzyć bramy aplikacji:

1. Tworzenie aplikacji zasobów bramy.
2. Tworzenie pliku XML konfiguracji lub obiekt konfiguracji.
3. Przekaż konfiguracji do nowo utworzonej aplikacji zasobu bramy.

>[AZURE.NOTE] Jeśli musisz skonfigurować niestandardowe sonda Centrum aplikacji, zobacz [Tworzenie bramy aplikacji z sondy niestandardowych przy użyciu programu PowerShell](application-gateway-create-probe-classic-ps.md). Zapoznaj się z [sondy niestandardowych i monitorowanie kondycji](application-gateway-probe-overview.md) więcej informacji.

![Przykład scenariusza][scenario]

### <a name="create-an-application-gateway-resource"></a>Tworzenie aplikacji bramy zasobów

Aby utworzyć bramę, należy użyć polecenia cmdlet **Nowy AzureApplicationGateway** , zamieniając wartości na własny. Karta bramy nie jest uruchamiany w tym momencie. Rozliczenia rozpoczyna się w późniejszym etapie, po pomyślnym uruchomieniu bramy.

Poniższy przykład tworzy bramy aplikacji przy użyciu wirtualną sieć o nazwie "testvnet1" i podsieć o nazwie "podsieci-1".


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Opis*, *InstanceCount*i *GatewaySize* są parametry opcjonalne.

Potwierdzenie, że brama została utworzona, można użyć polecenia cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Można wybrać mały, średni i duży.

*VirtualIPs* i *Nazwa_dns* są wyświetlane jako puste, ponieważ bramy nie została jeszcze rozpoczęta. Te są tworzone po brama jest w stanie uruchomienia.

## <a name="configure-the-application-gateway"></a>Konfigurowanie bramy aplikacji

Brama aplikacji przy użyciu XML lub obiekt konfiguracji.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfigurowanie bramy aplikacji przy użyciu XML

W poniższym przykładzie umożliwia pliku XML Konfigurowanie wszystkie ustawienia bramy aplikacji i przekazać je do zasobu bramy aplikacji.  

### <a name="step-1"></a>Krok 1  

Skopiuj następujący tekst do Notatnika.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Umożliwia edytowanie wartości w nawiasach pozycji konfiguracji. Zapisz plik z rozszerzeniem XML.

>[AZURE.IMPORTANT] Element protokołu Http lub Https jest uwzględniana wielkość liter.

W poniższym przykładzie pokazano, jak za pomocą pliku konfiguracji Konfigurowanie bramy aplikacji. Załaduj przykład sald ruch HTTP na porcie publicznej 80 i wysyła ruch sieciowy do wewnętrznej port 80 między dwoma adresami IP.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Krok 2

Następnie należy ustawić bramy aplikacji. Polecenie cmdlet **Set-AzureApplicationGatewayConfig** za pomocą pliku XML konfiguracji.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Konfigurowanie bramy aplikacji przy użyciu obiektu konfiguracji

W poniższym przykładzie pokazano, jak skonfigurować bramę aplikacji przy użyciu obiektów konfiguracji. Wszystkie pozycje konfiguracji musi skonfigurować indywidualnie, a następnie jest dodawana do obiektu konfiguracji bramy aplikacji. Po utworzeniu obiektu konfiguracji, polecenie **Ustaw AzureApplicationGateway** na zatwierdzenie konfiguracji do zasobu bramy wcześniej utworzonej aplikacji.

>[AZURE.NOTE] Przed przypisywania wartości do każdego obiektu konfiguracji, należy zadeklarować, jakiego rodzaju obiekt programu PowerShell używa magazynu. Jakie Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (nazwa obiektu) określa pierwszego wiersza w celu utworzenia poszczególne elementy są używane.

### <a name="step-1"></a>Krok 1

Utwórz wszystkie poszczególne pozycje konfiguracji.

Tworzenie zewnętrzną IP, jak pokazano w poniższym przykładzie.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Tworzenie zewnętrzną portu, jak pokazano w poniższym przykładzie.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Tworzenie puli wewnętrznej serwera.

 Definiowanie adresy IP, które są dodawane do puli serwerze wewnętrznym, jak pokazano w przykładzie dalej.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Obiekt $server umożliwia dodawanie wartości do obiektu puli wewnętrznej ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Tworzenie ustawienie puli wewnętrznej serwera.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Tworzenie odbiornika.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Utwórz regułę.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Krok 2

Przypisz wszystkie poszczególne pozycje konfiguracji do obiektu konfiguracji bramy aplikacji ($appgwconfig).

Dodawanie zewnętrzną adresów IP w konfiguracji.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Dodaj zewnętrzną port konfiguracji.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Dodawanie puli wewnętrznej serwera konfiguracji.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Dodaj ustawienie wewnętrznej puli w konfiguracji.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Dodawanie odbiornika w konfiguracji.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Dodawanie reguły do konfiguracji.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Krok 3

Przekaż obiekt konfiguracji do zasobu bramy aplikacji przy użyciu **Zestawu AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Rozpoczynanie bramy

Po skonfigurowaniu bramy użyć polecenia cmdlet **Start AzureApplicationGateway** zacząć bramy. Karta bramy aplikacji rozpoczyna się po pomyślnym uruchomieniu bramy.

> [AZURE.NOTE] Polecenie cmdlet **Start AzureApplicationGateway** może potrwać do 15 20 minut do zakończenia.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Sprawdź stan bramy

Aby sprawdzić stan bramy, należy użyć polecenia cmdlet **Get-AzureApplicationGateway** . Jeśli **Start AzureApplicationGateway** zakończyła się pomyślnie w poprzednim kroku, *Stan* powinna być uruchomiona i *Vip* i *Nazwa_dns* powinni mieć prawidłowych wpisów.

W poniższym przykładzie pokazano bramy aplikacji, w górę, w pracy, i chcesz wykonać ruch przeznaczone do `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Usuń bramy aplikacji

Aby usunąć bramy aplikacji:

1. Aby zatrzymać bramy, należy użyć polecenia cmdlet **AzureApplicationGateway tabulatora** .
2. Aby usunąć bramę, należy użyć polecenia cmdlet **AzureApplicationGateway Usuń** .
3. Upewnij się, że brama została usunięta przy użyciu polecenia cmdlet **Get-AzureApplicationGateway** .

W poniższym przykładzie pokazano polecenia cmdlet **AzureApplicationGateway tabulatora** w pierwszym wierszu, a następnie wynik.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Po bramy aplikacji jest w stanie zatrzymania, należy użyć polecenia cmdlet **AzureApplicationGateway Usuń** zamiar usunięcia usługi.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Aby potwierdzić usunięcie usługi, można użyć polecenia cmdlet **Get-AzureApplicationGateway** . Ten krok nie jest wymagane.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz [Konfigurowanie bramy aplikacji dla protokołu SSL offload](application-gateway-ssl.md).

Jeśli chcesz skonfigurować bramy aplikacji do użytku z równoważenia obciążenia wewnętrznych, zobacz [Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)](application-gateway-ilb.md).

Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png