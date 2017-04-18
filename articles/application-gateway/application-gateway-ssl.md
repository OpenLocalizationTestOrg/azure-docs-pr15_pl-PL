<properties
   pageTitle="Konfigurowanie bramy aplikacji dla odciążania SSL przy użyciu klasycznej wdrożenia | Microsoft Azure"
   description="Ten artykuł zawiera instrukcje, aby utworzyć bramy aplikacji za pomocą protokołu SSL Odciążanie przy użyciu modelu Azure wdrożenia klasyczny."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurowanie bramy aplikacji dla odciążania SSL przy użyciu modelu Klasyczny wdrażania

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Menedżera zasobów programu PowerShell](application-gateway-ssl-arm.md)
-[Azure klasycznego programu PowerShell](application-gateway-ssl.md)

Aby zakończyć sesję Secure Sockets Layer (SSL) u bramy, aby uniknąć kosztów zadań odszyfrowywania SSL działania na farmie sieci web można skonfigurować Azure bramy aplikacji. Odciążanie SSL upraszcza zarządzanie aplikacji sieci web i konfiguracji serwera frontonu.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [do pobrania strony](https://azure.microsoft.com/downloads/).
2. Upewnij się, że masz wirtualnej sieci pracy z prawidłową podsieci. Upewnij się, że nie maszyn wirtualnych lub we wdrożeniach w chmurze z podsieci. Brama aplikacji musi być samoczynnie w podsieci wirtualnej sieci.
3. Serwerów, które można konfigurować w celu za pomocą bramy aplikacji, musi istnieć lub przypisano ich punkty końcowe utworzonych w wirtualnej sieci lub z publicznej IP-VIP.

Aby skonfigurować odciążanie protokołu SSL na bramy aplikacji, wykonaj następujące czynności w podanej kolejności:

1. [Tworzenie bramy aplikacji](#create-an-application-gateway)
2. [Przekazywanie certyfikatów SSL](#upload-ssl-certificates)
3. [Konfigurowanie bramy](#configure-the-gateway)
4. [Ustawianie konfiguracji bramy](#set-the-gateway-configuration)
5. [Rozpoczynanie bramy](#start-the-gateway)
6. [Sprawdź stan bramy](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Aby utworzyć bramę, należy użyć polecenia cmdlet **Nowy AzureApplicationGateway** , zamieniając wartości na własny. Karta bramy nie jest uruchamiany w tym momencie. Rozliczenia rozpoczyna się w późniejszym etapie, po pomyślnym uruchomieniu bramy.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Potwierdzenie, że brama została utworzona, można użyć polecenia cmdlet **Get-AzureApplicationGateway** .

W próbki, *Opis*, *InstanceCount*i *GatewaySize* są parametry opcjonalne. Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Małe i duże są inne dostępne wartości. *VirtualIPs* i *Nazwa_dns* są wyświetlane jako puste, ponieważ bramy nie została jeszcze rozpoczęta. Wartości te są tworzone po brama jest w stanie uruchomienia.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Przekazywanie certyfikatów SSL

**Dodaj AzureApplicationGatewaySslCertificate** umożliwia przekazywanie certyfikatu serwera w formacie *pfx* do bramy aplikacji. Nazwa certyfikatu jest nazwą wybranych przez użytkownika i musi być unikatowa w bramy aplikacji. Ten certyfikat odwołuje się do tej nazwy w wszystkie operacje zarządzania certyfikatu na bramie aplikacji.

Poniżej pokazano polecenia cmdlet, to należy zamienić wartości w próbce z własnych.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Następnie sprawdź poprawność przekazywanie certyfikatu. Polecenie cmdlet **Get-AzureApplicationGatewayCertificate** .

To pokazano w pierwszym wierszu polecenia cmdlet następuje wynik.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Hasło certyfikatu musi należeć do przedziału od 4 do 12 znaków, liter i liczb. Znaki specjalne nie są akceptowane.

## <a name="configure-the-gateway"></a>Konfigurowanie bramy

Aplikacja konfiguracji bramy składa się z wielu wartości. Wartości można powiązanych ze sobą, aby utworzyć konfigurację.

Dostępne są następujące wartości:

- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do podsieci, wirtualną sieć lub powinny być publicznej IP-VIP.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Zewnętrzną portu:** Ten port jest publicznej otwarcia na bramie aplikacji. Ruch trafienia tego portu, a następnie przekierowaniem do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https, wartości te są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload).
- **Reguły:** Reguła powiąże odbiornika i puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika. Obecnie jest obsługiwana tylko *podstawowe* reguły. *Podstawowe* reguła jest dystrybucji obciążenia okrężny.

**Dodatkowa konfiguracja notatek**

Konfiguracja certyfikatów SSL protokołu w **HttpListener** powinien zmienić się na *Https* (jest uwzględniana wielkość liter). **SslCert** element jest dodawany do **HttpListener** wartość taką samą nazwę jak używanych w przekazywanie poprzedzających sekcji certyfikatów SSL. Port zewnętrzną powinny być aktualizowane do 443.

**Umożliwiające koligacji systemem plików cookie**: bramy aplikacji można skonfigurować tak, aby upewnić się, że żądania z sesją zawsze jest kierowane do samej maszyn wirtualnych w farmie sieci web. W tym scenariuszu jest wykonywane przez uruchomienie cookie sesji, umożliwiająca bramy skierować ruch prawidłowo. Aby włączyć koligacji systemem plików cookie, ustaw **CookieBasedAffinity** *włączone* w elemencie **BackendHttpSettings** .



Można utworzyć konfiguracji, tworząc obiekt konfiguracji lub za pomocą pliku XML konfiguracji.
Aby skonstruować konfiguracji za pomocą pliku XML konfiguracji, należy użyć Poniższy przykładowy:

**Przykładowy plik XML konfiguracji**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Ustawianie konfiguracji bramy

Następnie możesz ustawić bramy aplikacji. Za pomocą polecenia cmdlet **Set-AzureApplicationGatewayConfig** z obiektu konfiguracji lub z pliku XML konfiguracji.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Rozpoczynanie bramy

Skonfigurowane bramy należy użyć polecenia cmdlet **Start AzureApplicationGateway** , aby rozpocząć bramy. Karta bramy aplikacji rozpoczyna się po pomyślnym uruchomieniu bramy.


**Uwaga:** Polecenie cmdlet **Start AzureApplicationGateway** może potrwać do 15 20 minut do zakończenia.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Sprawdź stan bramy

Aby sprawdzić stan bramy, należy użyć polecenia cmdlet **Get-AzureApplicationGateway** . Jeśli **Start AzureApplicationGateway** zakończyła się pomyślnie w poprzednim kroku, *Stan* powinna być uruchomiona i *VirtualIPs* i *Nazwa_dns* powinny mieć prawidłowych wpisów.

W tym przykładzie pokazano bramy aplikacji działa, i chcesz skorzystać ruch.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Następne kroki


Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)