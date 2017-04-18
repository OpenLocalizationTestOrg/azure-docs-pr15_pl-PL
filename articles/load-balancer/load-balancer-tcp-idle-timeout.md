<properties
   pageTitle="Konfigurowanie limitu czasu bezczynności protokołu TCP równoważenia obciążenia | Microsoft Azure"
   description="Konfigurowanie limitu czasu bezczynności protokołu TCP równoważenia obciążenia"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia Azure

W konfiguracji domyślnej równoważenia obciążenia Azure ma ustawienie limitu czasu bezczynności 4 minut. Jeśli pewnym okresie nieaktywności jest dłuższa niż wartość limitu czasu, nie jest gwarantowana sesji TCP i HTTP są obsługiwane między klientem a usługa w chmurze.

Po zamknięciu połączenia z aplikacją kliencką może zostać wyświetlony następujący komunikat o błędzie: "Połączenie podstawowe zostało zakończone: połączenie oczekiwano życiu została zamknięta przez serwer."

Wspólnych ćwiczenia jest użycie TCP aktywności. Metoda ta pozostawia połączenie jako aktywne przez dłuższy czas. Aby uzyskać więcej informacji zobacz w tych [przykładach .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Włączone aktywności, pakiety są wysyłane okresach braku aktywności przez połączenie. Te pakiety aktywności upewnij się, że nigdy nie jest osiągnięta wartość limitu czasu bezczynności i połączenia są obsługiwane przez dłuższy.

To ustawienie ma zastosowanie tylko w przypadku połączeń przychodzących. Aby uniknąć utraty połączenia, możesz skonfigurować porty TCP aktywności interwał mniejsza niż ustawienie limitu czasu bezczynności lub zwiększ wartość limitu czasu bezczynności. Aby obsługiwać scenariusze tego typu, Dodaliśmy obsługę można skonfigurować limit czasu bezczynności. Teraz można ustawić czas trwania 4 do 30 minut.

Port TCP aktywności sprawdza się w przypadku scenariuszy, gdzie baterii nie jest ograniczenia. Nie jest zalecane dla aplikacji dla urządzeń przenośnych. Za pomocą TCP aktywności w aplikacji mobilnej można szybciej Opróżnij baterii urządzenia.

![Limit czasu TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

W poniższych sekcjach opisano, jak zmienić ustawienia czasu bezczynności w środowisku maszyn wirtualnych i usług w chmurze.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Konfigurowanie limit czasu TCP dla swojego poziomu wystąpienia publiczny adres IP na 15 minut

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`jest opcjonalna. Jeśli nie zostanie ustawiona, domyślny limit czasu jest 4 minut. Zakres dopuszczalny limit czasu jest 4 do 30 minut.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Ustawianie limitu czasu bezczynności podczas tworzenia punktu końcowego Azure na komputerze wirtualnych

Aby zmienić ustawienie limitu czasu dla punktu końcowego, użyj następujących opcji:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Aby pobrać konfiguracji bezczynności, użyj następującego polecenia:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Ustawianie limitu czasu TCP zestawu równoważenia obciążenia punktu końcowego

Jeśli punkty końcowe są częścią zestawu równoważenia obciążenia punkt końcowy, należy ustawić limit czasu TCP na stronie Ustaw punkt końcowy równoważenia obciążenia. Na przykład:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Zmienianie ustawień limitu czasu dla usług w chmurze

Azure SDK umożliwia aktualizowanie usługi w chmurze. Wybierz punkt końcowy ustawienia dla usług w chmurze w pliku .csdef. Aktualizowanie limit czasu TCP do wdrożenia usługi w chmurze wymaga uaktualnienia wdrożenia. Wyjątkiem jest, jeśli tylko dla publiczny adres IP jest określony limit czasu TCP. Ustawienia w publicznej IP znajdują się w pliku .cscfg, a następnie zaktualizuje ich tak, za pośrednictwem wdrażania aktualizacji i uaktualnianie.

Zmiany .csdef dla ustawienia punktu końcowego są następujące:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Zmiany .cscfg dla ustawienie limitu czasu na publiczne adresy IP są następujące:

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

## <a name="rest-api-example"></a>Przykład interfejsu API usługi REST

Limit czasu bezczynności protokołu TCP można skonfigurować za pomocą interfejsu API Menedżera usługi. Upewnij się, że `x-ms-version` nagłówek jest ustawiona na wersję `2014-06-01` lub nowszym. Zaktualizuj konfigurację określonego równoważenia obciążenia wprowadzania punkty końcowe w przypadku wszystkich maszyn wirtualnych we wdrożeniu.

### <a name="request"></a>Żądanie

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Odpowiedź

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Następne kroki

[Omówienie równoważenia obciążenia wewnętrznych](load-balancer-internal-overview.md)

[Przed rozpoczęciem konfigurowania równoważenia obciążenia z Internetu](load-balancer-get-started-internet-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)
