<properties
   pageTitle="Tworzenie niestandardowego sondy bramy aplikacji przy użyciu programu PowerShell w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak utworzyć niestandardowe sondy bramy aplikacji przy użyciu programu PowerShell w modelu Klasyczny wdrażania"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Tworzenie niestandardowego sondy Azure aplikacji bramy (klasyczny) przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-probe-ps.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Aby utworzyć bramy aplikacji:

1. Tworzenie aplikacji zasobów bramy.
2. Tworzenie pliku XML konfiguracji lub obiekt konfiguracji.
3. Przekaż konfiguracji do nowo utworzonej aplikacji zasobu bramy.

### <a name="create-an-application-gateway-resource"></a>Tworzenie aplikacji bramy zasobów

Aby utworzyć bramę, należy użyć polecenia cmdlet **Nowy AzureApplicationGateway** , zamieniając wartości na własny. Karta bramy nie jest uruchamiany w tym momencie. Rozliczenia rozpoczyna się w późniejszym etapie, po pomyślnym uruchomieniu bramy.

Poniższy przykład tworzy bramy aplikacji przy użyciu wirtualną sieć o nazwie "testvnet1" i podsieć o nazwie "podsieci-1".

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Potwierdzenie, że brama została utworzona, można użyć polecenia cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Można wybrać mały, średni i duży.

 *VirtualIPs* i *Nazwa_dns* są wyświetlane jako puste, ponieważ bramy nie została jeszcze rozpoczęta. Te są tworzone po brama jest w stanie uruchomienia.

## <a name="configure-an-application-gateway"></a>Konfigurowanie bramy aplikacji

Brama aplikacji przy użyciu XML lub obiekt konfiguracji.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurowanie bramy aplikacji przy użyciu XML

W poniższym przykładzie umożliwia pliku XML Konfigurowanie wszystkie ustawienia bramy aplikacji i przekazać je do zasobu bramy aplikacji.  

### <a name="step-1"></a>Krok 1

Skopiuj następujący tekst do Notatnika.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Umożliwia edytowanie wartości w nawiasach pozycji konfiguracji. Zapisz plik z rozszerzeniem XML.

W poniższym przykładzie pokazano, jak skonfigurować bramę aplikacji ruch HTTP na porcie publicznej 80 równoważenia obciążenia i wysyłać ruch sieciowy wewnętrznej port 80 między dwoma adresami IP przy użyciu niestandardowej sondy za pomocą pliku konfiguracji.

>[AZURE.IMPORTANT] Element protokołu Http lub Https jest uwzględniana wielkość liter.

Nowy element konfiguracji \<sondy\> zostanie dodany do skonfigurowania sondy niestandardowe.

Parametry konfiguracji są:

- **Nazwa** - Nazwa odwołania sondy niestandardowe.
- **Protocol (protokół)** - protokół używany (możliwe wartości są HTTP lub HTTPS).
- **Host** i **ścieżkę** - pełnego adresu URL ścieżkę, która jest wywoływana przez bramę aplikacji w celu ustalenia stanu wystąpienia. Na przykład jeśli masz http://contoso.com/ witryny sieci Web, następnie sondy niestandardowej można skonfigurować dla "http://contoso.com/path/custompath.htm" kontroli sondy ma pomyślną odpowiedź HTTP.
- **Interwał** - konfiguruje testy interwału sondy w sekundach.
- **Przekroczono limit czasu** - określa limit czasu sondy sprawdzanie odpowiedzi HTTP.
- **UnhealthyThreshold** - liczba negatywnych odpowiedzi HTTP, aby oznaczyć flagą wystąpienia wewnętrznej jako *nieprawidłowe*.

Odwołuje się nazwa sondy <BackendHttpSettings> konfiguracja, aby przypisać które puli wewnętrznej ustawień niestandardowych sondy.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Dodawanie konfiguracji niestandardowych sondy do bramy istniejącej aplikacji

Zmienianie bieżącej konfiguracji bramy aplikacji wymaga wykonania trzech kroków: pobieranie bieżącego pliku konfiguracji XML, modyfikowanie ma niestandardowe sondy i skonfigurować bramę aplikacji z nowymi ustawieniami XML.

### <a name="step-1"></a>Krok 1

Uzyskaj plik XML przy użyciu get-AzureApplicationGatewayConfig. To eksportuje konfigurację XML można modyfikować tak, aby dodać ustawienie sondy.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Krok 2

Otwórz plik XML w edytorze tekstów. Dodawanie `<probe>` sekcji po `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

W sekcji backendHttpSettings XML Dodaj nazwę sondy, jak pokazano w poniższym przykładzie:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Zapisz plik XML.

### <a name="step-3"></a>Krok 3

Aktualizowanie konfiguracji bramy aplikacji z nowego pliku XML przy użyciu **Zestawu AzureApplicationGatewayConfig**. Centrum aplikacji to aktualizacje z nową konfiguracją.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować odciążanie Secure Sockets Layer (SSL), zobacz [Konfigurowanie bramy aplikacji dla protokołu SSL offload](application-gateway-ssl.md).

Jeśli chcesz skonfigurować bramy aplikacji do użytku z równoważenia obciążenia wewnętrznych, zobacz [Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)](application-gateway-ilb.md).
