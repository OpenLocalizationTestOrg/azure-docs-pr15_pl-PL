<properties
   pageTitle="Konfigurowanie usługi równoważenia obciążenia rozkładu trybu | Microsoft Azure"
   description="Jak skonfigurować Azure równoważenia dystrybucji w trybie do obsługi koligacji IP źródła"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfigurowanie trybu rozkładu dla równoważenia obciążenia

## <a name="hash-based-distribution-mode"></a>Tryb dystrybucji skrótu

Algorytm rozkładu domyślny jest 5-krotki (źródłowy adres IP, port źródłowy docelowy adres IP, port docelowy protokołu typu) skrótu do zamapować ruch dostępnych serwerów. Umożliwia lepkość tylko w sesji transportu. Pakiety w tej samej sesji będzie kierowany do tego samego wystąpienia IP (DIP) centrum danych za punktem końcowym równoważenia obciążenia. Przy uruchamianiu klienta nowej sesji z tym samym adres IP źródła, port źródłowy zmieni i ruch danych przejść do innego końcowy DIP.

![Skrót podstawie równoważenia obciążenia](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Rysunek 1-5-krotki rozkładu

## <a name="source-ip-affinity-mode"></a>Tryb koligacji IP źródła

Mamy innego trybu dystrybucji o nazwie koligacji IP źródła (nazywane także koligacji sesji lub koligacji IP klienta). Azure równoważenia obciążenia można skonfigurować do zamapować ruch dostępnych serwerów za pomocą krotki 2 (adres IP źródła, docelowy adres IP) lub 3-krotki (adres IP źródła, docelowy adres IP, Protocol). Za pomocą koligacji adres IP źródła, połączenia zainicjowane na tym samym komputerze klienckim prowadzi do samego punktu końcowego DIP.

Na poniższym diagramie przedstawiono konfiguracji krotki 2. Zwróć uwagę, działania krotki 2 za pośrednictwem usługi równoważenia obciążenia maszyn wirtualnych 1 (VM1), który jest następnie kopii zapasowej, VM2 i VM3.

![Koligacja sesji](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Rysunek 2-2-krotki rozkładu

Źródła IP koligacji rozwiązuje niezgodności między równoważenia obciążenia Azure i bramy pulpitu zdalnego (RD). Teraz można tworzyć farmy bramy usług pulpitu zdalnego w usłudze w chmurze pojedynczy.

Inny scenariusz użycia jest przekazywanie multimediów miejsce, w którym ma miejsce przekazywanie danych za pośrednictwem UDP, ale płaszczyzny sterowania uzyskuje się za pośrednictwem protokołu TCP:

- Klient najpierw inicjuje sesję TCP publiczny adres równoważenia obciążenia, otrzymuje kierowane do określonych DIP ten kanał pozostaje aktywna monitorowanie kondycji połączenia
- Nową sesję UDP z tej samej komputer kliencki została zainicjowana z tym samym końcowym publicznej równoważenia obciążenia, w tym miejscu oczekuje się, że to połączenie jest również kierowane do samego punktu końcowego DIP zgodnie z poprzedniego połączenia TCP tak, aby przekazać multimediów mogą być wykonywane w wysokiej wydajności przy zachowaniu również kanału kontroli za pośrednictwem protokołu TCP.

>[AZURE.NOTE] Podczas zmiany zestawu równoważenia obciążenia (usuwanie lub dodawanie maszyny wirtualnej) rozkład żądań jest ponownie przeliczana. Nie są zależne od nowych połączeń z istniejących klientów koniec ten sam serwer. Ponadto za pomocą adres IP źródła trybie rozkładu koligacji może spowodować nierówne dystrybucji ruchu. Uruchomionym za serwery proxy mogą być widoczne jako jeden aplikacji klienckiej unikatowych.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurowanie ustawień koligacji adres IP źródła dla Usługa równoważenia obciążenia

Dla maszyn wirtualnych można użyć programu PowerShell, aby zmienić ustawienia limitu czasu:

Dodawanie punktu końcowego Azure maszyn wirtualnych i ustaw tryb rozkładu równoważenia obciążenia

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution można ustawić na sourceIP równoważenia, sourceIPProtocol równoważenia obciążenia 3-krotki (adres IP źródła, docelowy adres IP, protocol) lub Brak Jeśli chcesz, aby domyślne zachowanie równoważenia obciążenia krotki 5 obciążenia krotki 2 (adres IP źródła, docelowy adres IP).

Pobieranie konfiguracji punktu końcowego ładowania równoważenia dystrybucji tryb za pomocą następujących czynności:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Jeśli brakuje elementu LoadBalancerDistribution równoważenia obciążenia Azure używa algorytmu krotki 5 domyślne.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Ustawianie trybu rozkładu na Ustaw punkt końcowy równoważenia obciążenia

Jeśli punkty końcowe są częścią Ustaw punkt końcowy równoważenia obciążenia, tryb rozkładu należy ustawić na stronie Ustaw punkt końcowy równoważenia obciążenia:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Konfiguracja usługi, aby zmienić tryb rozkładu w chmurze

Korzystać z SDK Azure dla .NET 2,5 (mają zostać udostępnione listopada), aby zaktualizować usługi w chmurze. Ustawienia punktu końcowego dla usług w chmurze są wprowadzane w .csdef. Aby zaktualizować tryb rozkładu równoważenia obciążenia wdrożenia usług w chmurze, wymagane jest uaktualnienie wdrożenia.
Oto przykład .csdef zmiany ustawień punkt końcowy:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
      <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
      </PublicIPs>
    </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="api-example"></a>Przykład interfejsu API

Możesz skonfigurować rozkład równoważenia obciążenia za pomocą interfejsu API Menedżera usługi. Upewnij się dodać `x-ms-version` nagłówek jest ustawiona na wersję `2014-09-01` lub nowszym.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Zaktualizuj konfigurację określonego zestawu równoważenia obciążenia we wdrożeniu

#### <a name="request-example"></a>Przykład żądanie

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Wartość LoadBalancerDistribution może być sourceIP koligacji krotki 2, sourceIPProtocol koligacji krotki 3 lub Brak (Brak koligacji. to znaczy krotki-5)

#### <a name="response"></a>Odpowiedź

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Następne kroki

[Omówienie równoważenia obciążenia wewnętrznych](load-balancer-internal-overview.md)

[Przed rozpoczęciem konfigurowania dostępnych równoważenia obciążenia przez Internet](load-balancer-get-started-internet-arm-ps.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
