<properties 
   pageTitle="Tworzenie i konfigurowanie bramy aplikacji z usługi równoważenia obciążenia wewnętrzny (ILB) w wirtualnej sieci | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące konfigurowania bramy aplikacji Azure z punktem końcowym wewnętrznych równoważenia obciążenia"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Tworzenie bramy aplikacji przy użyciu usługi równoważenia obciążenia wewnętrzny (ILB)

> [AZURE.SELECTOR]
- [Azure czynności klasyczny](application-gateway-ilb.md)
- [Procedura programu Powershell Menedżera zasobów](application-gateway-ilb-arm.md)

Brama aplikacji można skonfigurować internetowej przeciwległych wirtualny adres IP lub punkt końcowy wewnętrznych nie udostępniana w Internecie, nazywane także punktu końcowego usługi równoważenia obciążenia wewnętrzny (ILB). Konfigurowanie bramy przy użyciu ILB są przydatne w przypadku wewnętrznym aplikacjom programu LOB udostępniana internet. Jest również przydatne w przypadku usług i warstw w aplikacji wielu, który znajduje się w granicę zabezpieczeń udostępniana internet, ale nadal wymaga dystrybucji obciążenia okrężnego, lepkość sesji lub zakończenie SSL. W tym artykule opisano czynności, aby skonfigurować bramy aplikacji z ILB.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Azure za pomocą Instalatora platformy sieci Web. Możesz pobrać i zainstalować najnowszą wersję z sekcji **Programu Windows PowerShell** [pobieranie strony](https://azure.microsoft.com/downloads/).
2. Upewnij się, że masz wirtualnej sieci pracy z prawidłową podsieci.
3. Sprawdź, czy masz serwery wewnętrznej bazy danych w wirtualnej sieci lub z publicznej IP-VIP przydzielone.


Aby utworzyć bramy aplikacji, wykonaj następujące czynności w określonej kolejności. 

1. [Tworzenie bramy aplikacji](#create-a-new-application-gateway)
2. [Konfigurowanie bramy](#configure-the-gateway)
3. [Ustawianie konfiguracji bramy](#set-the-gateway-configuration)
4. [Rozpoczynanie bramy](#start-the-gateway)
4. [Sprawdź bramy](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji:

**Aby utworzyć bramę**, użyj `New-AzureApplicationGateway` polecenia cmdlet, zamieniając wartości na własny. Należy zauważyć, że karta bramy nie jest uruchamiana na tym etapie. Rozliczenia rozpoczyna się w późniejszym etapie, po pomyślnym uruchomieniu bramy.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Aby sprawdzić poprawność** że brama została utworzona, można użyć `Get-AzureApplicationGateway` polecenia cmdlet. 

W próbki, *Opis*, *InstanceCount*i *GatewaySize* są parametry opcjonalne. Wartość domyślna *InstanceCount* jest 2 z maksymalną wartość 10. Wartość domyślna *GatewaySize* jest średni. Małe i duże są inne dostępne wartości. *VIP* i *Nazwa_dns* są wyświetlane jako puste, ponieważ bramy nie została jeszcze rozpoczęta. Te są tworzone po brama jest w stanie uruchomienia. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Konfigurowanie bramy

Aplikacja konfiguracji bramy składa się z wielu wartości. Wartości można powiązanych ze sobą, aby utworzyć konfigurację.
 
Dostępne są następujące wartości:

- **Puli serwera wewnętrznej bazy danych:** Lista adresów IP serwerów wewnętrznej bazy danych. Adresy IP wymienione albo powinna należeć do podsieci VNet lub powinny być publicznej IP-VIP. 
- **Ustawienia puli serwera wewnętrznej bazy danych:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Port serwera sieci Web:** Ten port jest publicznej otwarty na bramie aplikacji. Ruch trafienia tego portu, a następnie przekierowaniem do jednego z serwerów wewnętrznej bazy danych.
- **Odbiornika:** Odbiornik ma port serwera sieci Web, protokół (Http lub Https są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload). 
- **Reguły:** Reguła powiąże odbiornika i puli serwera wewnętrznej bazy danych i definiuje puli serwera wewnętrznej bazy danych, które dane powinny być kierowane do, gdy trafienia w szczególności odbiornika. Obecnie jest obsługiwana tylko *podstawowe* reguły. *Podstawowe* reguła jest dystrybucji obciążenia okrężny.

Można utworzyć konfiguracji, tworząc obiekt konfiguracji lub pliku XML konfiguracji. Aby skonstruować konfiguracji za pomocą pliku XML konfiguracji, wykonaj poniższe przykładowe.



Pamiętaj o następujących kwestiach:


- *FrontendIPConfigurations* element opisano ILB dotyczące konfigurowania aplikacji bramy za pomocą ILB. 

- IP serwera sieci Web *typu* powinna być równa "Prywatny"

- *StaticIPAddress* powinna być równa odpowiednie bramy, na którym otrzymuje ruch wewnętrznych adresów IP. Należy zauważyć, że *StaticIPAddress* element jest opcjonalna. Jeśli nie jest zestaw dostępnych wewnętrznych adresów IP z wdrożonym podsieci została wybrana. 

- Wartość elementu *nazwę* określonego w *FrontendIPConfiguration* należy w elemencie *FrontendIP* HTTPListener odwołują się do FrontendIPConfiguration.

 **Przykładowy plik XML konfiguracji**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Ustawianie konfiguracji bramy

Następnie skonfigurujesz bramy aplikacji. Możesz użyć `Set-AzureApplicationGatewayConfig` polecenia cmdlet z obiektu konfiguracji lub z pliku XML konfiguracji. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Rozpoczynanie bramy

Po skonfigurowaniu bramy za pomocą `Start-AzureApplicationGateway` polecenia cmdlet, aby rozpocząć bramy. Karta bramy aplikacji rozpoczyna się po pomyślnym uruchomieniu bramy. 


> [AZURE.NOTE] `Start-AzureApplicationGateway` Polecenia cmdlet może potrwać do 15-20 minut. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Sprawdź stan bramy

Używanie `Get-AzureApplicationGateway` polecenia cmdlet, aby sprawdzić stan bramy. Jeśli *Start AzureApplicationGateway* zakończyła się pomyślnie w poprzednim kroku, stan powinien być *uruchomiony*i Vip i Nazwa_dns powinny mieć prawidłowych wpisów. To pokazano w pierwszym wierszu polecenia cmdlet następuje wynik. W tym przykładzie brama jest uruchomiona i jest gotowy do wykonać ruch. 

> [AZURE.NOTE] Brama aplikacji jest skonfigurowany do akceptowania ruchu w punkcie skonfigurowaną końcowym ILB 10.0.0.10 w tym przykładzie.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Następne kroki


Więcej informacji na temat ogólnego ładowanie opcje bilansowania, zobacz:

- [Usługi równoważenia obciążenia Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Menedżer ruchu](https://azure.microsoft.com/documentation/services/traffic-manager/)
