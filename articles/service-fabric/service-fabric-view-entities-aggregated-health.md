<properties
   pageTitle="Jak wyświetlić podmioty tkaninie usługi Azure agregacją kondycji | Microsoft Azure"
   description="Informacje dotyczące kwerend, wyświetlanie i oceniania zagregowane kondycji podmioty tkaninie usługi Azure za pośrednictwem zdrowia i ogólne kwerend."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Wyświetlanie raportów dotyczących kondycji usługi tkaninie
Azure tkaninie usługi wprowadza [modelu](service-fabric-health-introduction.md) , który zawiera jednostki kondycji, które systemie watchdogs i składników zgłosić lokalne warunki, które są one monitorowania. [Przechowywanie kondycji](service-fabric-health-introduction.md#health-store) agreguje wszystkie dane dotyczące kondycji do określenia, czy jednostki są prawidłowy.

Okno klaster zostanie wypełniona raporty dotyczące kondycji wysyłane przez składniki systemu. Dowiedz się więcej na [Używanie raportów dotyczących kondycji systemu rozwiązywać problemy](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Usługa tkaninie oferuje wiele sposobów uzyskiwania zagregowane kondycji podmiotów:

- [Usługa tkaninie Eksploratora](service-fabric-visualizing-your-cluster.md) lub innych narzędzi do wizualizacji

- Kondycji kwerend (za pomocą programu PowerShell, interfejsu API lub pozostałych)

- Ogólne kwerendy to zwrotu listę obiektów, które mają kondycji w jednym z właściwości (za pomocą programu PowerShell, interfejsu API lub pozostałych)

Wykazać tych opcji, można użyć klaster lokalny z pięciu węzłów. Obok pozycji **tkaninie:-System** aplikacji (który istnieje poza pole), niektóre inne aplikacje są rozmieszczane. Jedną z nich jest **tkaninie:-WordCount**. Ta aplikacja zawiera stanowe usługi skonfigurowano siedem repliki. Ponieważ nie istnieją tylko pięć węzłów, składników systemu wyświetlany jest komunikat, partycją znajdujących się poniżej statystykę docelowej.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Kondycja w Eksploratorze tkaninie usługi
Usługa tkaninie Eksploratora wyświetla wizualne klaster. Na poniższej ilustracji można wyświetlić, które:

- Aplikacja **tkaninie:-WordCount** jest czerwony (w błąd), ponieważ ma ona zdarzenie błędu zgłaszane przez **MyWatchdog** właściwości **dostępności**.

- Jedną z jego usług **tkaninie: / WordCount/WordCountService** żółte (w ostrzeżenie). Ponieważ usługa jest skonfigurowana z replikami siedem, ta osoba nie wszystkie można umieścić, są tylko pięć węzły. Mimo że nie jest tutaj pokazano, partycją usługi jest żółty z powodu raportu systemu. Żółty partition uruchamia usługę żółty.

- Klaster jest czerwony ze względu na czerwony aplikacji.

Oceny używa domyślnych zasad z manifestu klaster i manifest aplikacji. Są ścisłe zasady i nieodpowiednie niezgodności.

Widok klaster w Eksploratorze tkaninie usługi:

![Widok klaster w Eksploratorze tkaninie usługi.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Dowiedz się więcej na temat [Eksploratora tkaninie usługi](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Kondycji kwerend
Usługa tkaninie udostępnia kondycji kwerend dla każdego z obsługiwanych [typów jednostek](service-fabric-health-introduction.md#health-entities-and-hierarchy). Dostępne za pośrednictwem interfejsu API (metody można znaleźć w **FabricClient.HealthManager**), polecenia cmdlet programu PowerShell i Zatrzymaj. Te kwerendy zwracają pełną kondycji informacji o jednostce: zagregowane kondycję, zdarzeń kondycji jednostki, stany kondycji podrzędny (w stosownych przypadkach) i ocen nieprawidłowe, jeśli jednostka nie działa prawidłowo.

> [AZURE.NOTE] Gdy zostanie całkowicie wypełniony w magazynie kondycji, jest zwracany podmiot kondycji. Jednostkę musi być aktywna (nie usunięte) i raport systemu. Jej podmioty nadrzędnej łańcucha hierarchii musi mieć również raportów systemu. Jeśli dowolny z tych warunków nie jest spełniony, kondycji kwerend zwracają wyjątek, który zawiera, dlaczego nie jest zwracane podmiotu.

Kwerendy kondycji należy przekazać identyfikator jednostki, która zależy od typu obiektu. Kwerendy Zaakceptuj parametry zasad kondycji opcjonalne. Jeśli podano żadne zasady dotyczące kondycji, [zasady dotyczące kondycji](service-fabric-health-introduction.md#health-policies) z manifestu klaster lub aplikacji są używane do obliczeń. Kwerendy również zaakceptować filtrów do zwracania tylko części dzieci lub zdarzenia — te, których przestrzegać określone filtry.

> [AZURE.NOTE] Filtry wyjściowe są stosowane po stronie serwera, aby w przypadku zmniejszenia rozmiaru odpowiedzi wiadomości. Zaleca się, że umożliwia ograniczenie danych zwróconych przez filtry wyjściowe, zamiast możesz zastosować filtry po stronie klienta.

Prawidłowość jednostki zawiera:

- Stan kondycji zagregowane jednostki. Obliczona w sklepie kondycji na podstawie raporty dotyczące kondycji jednostki, stany kondycji podrzędny (w stosownych przypadkach) i zasad dotyczących kondycji. Dowiedz się więcej na temat [oceny kondycji jednostki](service-fabric-health-introduction.md#entity-health-evaluation).  

- Zdarzenia kondycji w jednostce.

- Kolekcja stany kondycji wszystkich elementów podrzędnych dla obiektów, które mogą mieć elementów podrzędnych. Stany kondycji zawierają identyfikatory jednostki i stan kondycji zagregowane. Aby uzyskać pełną kondycji dla dzieci, połączenie kondycji kwerend dla typu jednostki podrzędne i przekaż identyfikator podrzędne.

- Nieprawidłowe ocen, które wskazują raportu, którego dotyczy stan obiektu, jeśli jednostka nie działa prawidłowo.

## <a name="get-cluster-health"></a>Uzyskiwanie klaster kondycji
Zwraca kondycji jednostki klaster i zawiera stany kondycji aplikacji i węzły (dzieci klaster). Danych wejściowych:

- [Opcjonalne] Zasady dotyczące kondycji klaster używane do analizowania węzły i zdarzeń klaster.

- [Opcjonalne] Aplikacja kondycji zasad mapy, zasady dotyczące kondycji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia, węzły i aplikacje, które Określ zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia, węzły i aplikacji są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji klaster, tworzenie `FabricClient` i wywołać metodę [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) w jego **HealthManager**.

Poniższe wywołanie otrzymuje kondycji klaster:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Poniższy kod pobiera kondycji klaster przy użyciu zasady kondycji klaster niestandardowych i filtry dla węzły i aplikacje. Tworzy [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), który zawiera dane wejściowe.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji klaster jest [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Stan klaster jest pięć węzły, aplikacja systemowa i tkaninie: / WordCount skonfigurowane zgodnie z opisem.

Następujące polecenie cmdlet pobiera kondycji klaster przy użyciu domyślnych zasad dotyczących kondycji. Stan kondycji zagregowane znajduje się na ostrzeżenie, ponieważ tkaninie: / WordCount aplikacja znajduje się ostrzeżenie. Należy pamiętać o tym, jak nieprawidłowe ocen szczegółowo warunki, które wyzwalane zagregowane kondycji.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Następujące polecenia cmdlet programu PowerShell otrzymuje kondycji klaster przy użyciu zasad niestandardową aplikację. Fragmentator filtruje wyników, aby uzyskać tylko błąd lub ostrzeżenie aplikacje i węzły. W wyniku bez węzłów są zwracane, ponieważ są one wszystkie prawidłowy. Tylko tkaninie: / WordCount aplikacji uwzględnia filtr aplikacji. Ponieważ zasad niestandardowych określa brać pod uwagę ostrzeżenia jako błędy tkaninie: / WordCount aplikacji, aplikacja jest szacowana, tak jak w błąd, a więc klaster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji klaster [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707669.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707696.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-node-health"></a>Uzyskiwanie węzeł kondycji
Zwraca kondycji podmiotu węzeł i zawiera zdarzenia kondycji zgłoszone w węźle. Danych wejściowych:

- [Wymagane] Nazwa węzła, który identyfikuje węzeł.

- [Opcjonalne] Ustawienia zasad kondycji klaster używane do oceniania kondycji.

- [Opcjonalne] Filtry dla zdarzenia, które Określ zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji węzeł za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) w jego HealthManager.

Poniższy kod pobiera kondycji węzeł nazwy określonego węzła:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Poniższy kod pobiera kondycji węzeł nazwy określonego węzła i przechodzi w filtrze zdarzeń i zasad niestandardowych [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji węzeł jest [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .
Następujące polecenie cmdlet pobiera kondycji węzeł, używając domyślnych zasad dotyczących kondycji:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Następujące polecenie cmdlet pobiera kondycji wszystkich węzłów w klastrze:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji węzeł [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707650.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707665.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-application-health"></a>Uzyskiwanie kondycji aplikacji
Zwraca kondycji Jednostka aplikacji. Zawiera on stany kondycji aplikacji i usług dzieci. Danych wejściowych:

- [Wymagane] Nazwa aplikacji (URI) identyfikujący aplikacji.

- [Opcjonalne] Zasady dotyczące kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia, usług i rozmieszczonych aplikacji określające zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia, usługi i rozmieszczonych aplikacji są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji aplikacji, tworzenie `FabricClient` i wywołać metodę [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) w jego HealthManager.

Poniższy kod pobiera kondycji aplikacji dla nazwy określonej aplikacji (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Poniższy kod pobiera kondycji aplikacji dla nazwy określonej aplikacji (URI), przy użyciu filtrów i zasad niestandardowych określonego przez [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji aplikacji jest [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Następujące polecenie cmdlet zwraca kondycji **tkaninie:-WordCount** aplikacji:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Następujące polecenia cmdlet programu PowerShell przekazuje w zasad niestandardowych. Umożliwia filtrowanie również dzieci i zdarzeń.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji aplikacji z [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707681.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707643.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-service-health"></a>Uzyskiwanie kondycja usługi
Zwraca kondycji usługi podmiotu. Zawiera on stany kondycji partycją. Danych wejściowych:

- [Wymagane] Nazwa usługi (URI) identyfikująca usługę.

- [Opcjonalne] Zasady dotyczące kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia i partycje określające zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie partycje i zdarzenia są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycja usługi za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) w jego HealthManager.

Poniższy przykład pobiera kondycji usługi przy użyciu określonej nazwy usługi (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Poniższy kod pobiera kondycji usługi dla określonej nazwy usługi (URI), określając filtrów i zasad niestandardowych przy użyciu [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji usługi jest [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Następujące polecenie cmdlet pobiera kondycji usługi, używając domyślnych zasad dotyczących kondycji:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycja usługi [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707609.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707646.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-partition-health"></a>Uzyskiwanie kondycji partition
Zwraca kondycji podmiotu partition. Zawiera on stany kondycji replice. Danych wejściowych:

- [Wymagane] Identyfikator (GUID), który identyfikuje partycją partycją.

- [Opcjonalne] Zasady dotyczące kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia i repliki określające zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia i repliki są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji partition za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) w jego HealthManager. Aby określić parametry opcjonalne, Utwórz [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet uzyskanie kondycji partycją jest [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Następujące polecenie cmdlet pobiera kondycji dla wszystkich partycje **tkaninie: / WordCount/WordCountService** usługi:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji partition [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707683.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707680.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-replica-health"></a>Uzyskiwanie replice kondycji
Zwraca kondycji replice stanowe usługi lub wystąpienia stateless usługi. Danych wejściowych:

- [Wymagane] Identyfikator GUID i replice identyfikator identyfikujący replice.

- [Opcjonalne] Parametry zasad kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia, które Określ zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji replice za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) w jego HealthManager. Aby określić parametry zaawansowane, za pomocą [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji replice jest [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Następujące polecenie cmdlet pobiera kondycji podstawowego replice dla wszystkie partycje usługi:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji replice [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707673.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707641.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-deployed-application-health"></a>Pobieranie aplikacji kondycji
Zwraca kondycji aplikacji wdrożone w jednostce węzeł. Zawiera on stany kondycji pakiet wdrożonym usługi. Danych wejściowych:

- [Wymagane] Nazwa aplikacji (URI) i nazwa węzła (ciąg) identyfikujące wdrożonej aplikacji.

- [Opcjonalne] Zasady dotyczące kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia i pakietów wdrożonym usług, określające zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia i pakietów wdrożonym usług są używane do oceny kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji aplikacji wdrożone w węźle za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) w jego HealthManager. Aby określić parametry opcjonalne, za pomocą [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet uzyskanie kondycji aplikacji jest [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) . Aby dowiedzieć się, gdzie wdrożyć aplikację, uruchom [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) i przeglądać dzieci wdrożonej aplikacji.

Następujące polecenie cmdlet pobiera kondycji **tkaninie:-WordCount** aplikacji wdrożone na **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycji aplikacji z [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707644.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707688.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="get-deployed-service-package-health"></a>Uzyskiwanie kondycja pakiet wdrożonym usługi
Zwraca kondycji podmiotu pakiet wdrożonym usługi. Danych wejściowych:

- [Wymagane] Nazwa aplikacji (URI), nazwa węzła (ciąg) i usługa pojawiają identyfikowanie pakietu wdrożonym z nazwy (ciąg).

- [Opcjonalne] Zasady dotyczące kondycji aplikacji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla zdarzenia, które Określ zapisy, które mogą być przydatne i powinny być zwrócone w wyniku (na przykład tylko błędy lub ostrzeżenia i błędy). Wszystkie zdarzenia są używane do oceniania kondycji jednostki agregacją, bez względu na to filtr.

### <a name="api"></a>INTERFEJS API
Aby uzyskać kondycji pakietu wdrożonym usług za pośrednictwem interfejsu API, Utwórz `FabricClient` i wywołać metodę [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) w jego HealthManager. Aby określić parametry opcjonalne, za pomocą [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet uzyskanie kondycji usługi wdrożonym pakiet jest [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) . Aby zobaczyć, gdzie wdrożyć aplikację, uruchom [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) i przyjrzyj się rozmieszczonych aplikacji. Aby zobaczyć, które usługi są pakiety w aplikacji, spójrz na dzieci pakietu usługi wdrożonym w wyniku [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

Następujące polecenie cmdlet pobiera kondycji pakiet usługi **WordCountServicePkg** **tkaninie:-WordCount** aplikacji wdrożone na **_Node_2**. Obiekt ma **System.Hosting** raportów dla pomyślnego pakietu usług i punktu wejścia aktywacji i pomyślnego rejestracji typ usługi.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać kondycja pakietu usługi wdrożonym [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/dn707677.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/dn707689.aspx) zawiera zasady dotyczące kondycji opisanych w treści.

## <a name="health-chunk-queries"></a>Kondycja fragmentu kwerendy
Kwerendy fragmentu kondycji może zwracać wielopoziomowych klaster dzieci (lokalizacji) na filtry. Filtry zaawansowane umożliwiające dużą elastyczność wyrazić które określonych elementów podrzędnych mają zostać zwrócone określoną przez jego unikatowy identyfikator lub innych identyfikator grupy lub stanu kondycji ją obsługuje. Domyślnie dzieci nie są uwzględniane, zamiast polecenia kondycji, które zawsze należy uwzględnić podrzędnymi pierwszego poziomu.

[Kondycji kwerend](service-fabric-view-entities-aggregated-health.md#health-queries) zwracają tylko podrzędnymi pierwszego poziomu jednostki określonej na wymagane filtry. Aby uzyskać dzieci dzieci, można zadzwonić kondycji dodatkowe interfejsy API dla każdego obiektu zainteresowania. Podobnie Aby uzyskać kondycji konkretne osoby, należy wywołać kondycji jednego interfejsu API dla każdej odpowiedniej jednostki. Kwerenda fragmentu zaawansowane filtrowanie pozwala na żądanie wielu elementy w jednej kwerendy zminimalizowania rozmiaru wiadomości i liczby wiadomości.

Kwerenda fragmentu wartość, którą można pobrać stanu kondycji, dla kolejne klastrze jednostki (potencjalnie wszystkich klaster jednostki począwszy od głównego wymagane) jednego połączenia. Kondycja złożonej kwerendy można wyrazić takich jak:

- Zwraca tylko te aplikacje na błędu i tych aplikacji zawiera wszystkie usługi przy ostrzeżenie | błędu. Zwrócone usług obejmuje wszystkie partycje.

- Zwraca tylko kondycji 4 aplikacji, określone przez ich nazwy.

- Zwraca tylko kondycji aplikacji typu żądaną aplikację.

- Zwraca wszystkie wdrożone podmioty w węźle. Zwraca wszystkie aplikacje wdrażane wszystkich aplikacji w określonym węźle i wszystkich pakietów usługi wdrożonym w tym węźle.

- Zwraca wszystkie repliki w błąd. Zwraca wszystkie aplikacje, usług partycje i tylko repliki w błąd.

- Zwraca wszystkie aplikacje. W przypadku określonej usługi zawiera wszystkie partycje.

Obecnie kondycji kwerend fragmentu są wyświetlane tylko dla jednostki klaster. Zwraca fragment kondycji klaster, który zawiera:

- Stan kondycji klaster agregacją.

- Lista fragmentu stan kondycji węzłów, które przestrzegać filtry.

- Lista fragmentu stan kondycji aplikacji, które przestrzegać filtry. Każdy fragment stan kondycji aplikacji zawiera listę fragmentu ze wszystkimi usługami, które przestrzegać filtry i lista fragmentu z wszystkich wdrożonym aplikacjami, które przestrzegać filtrów. Działa tak samo dla dzieci usług i aplikacji wdrożone. W ten sposób wszystkie obiekty w klastrze mogą być potencjalnie zwracane żądanie w sposób hierarchiczny.

### <a name="cluster-health-chunk-query"></a>Klaster kondycji fragmentu kwerendy
Zwraca kondycji jednostki klaster i zawiera fragmenty stan kondycji hierarchicznych wymagane elementy podrzędne. Danych wejściowych:

- [Opcjonalne] Zasady dotyczące kondycji klaster używane do analizowania węzły i zdarzeń klaster.

- [Opcjonalne] Aplikacja kondycji zasad mapy, zasady dotyczące kondycji umożliwia zastępują zasady manifestu aplikacji.

- [Opcjonalne] Filtry dla węzły i aplikacje określające zapisy, które mogą być przydatne i powinny zostać zwrócone w wyniku. Filtry specyficzne dla obiektu lub grupy obiektów lub mają zastosowanie do wszystkich obiektów na tym samym poziomie. Na liście filtrów może zawierać jeden filtr ogólne i/lub filtry dla określonej identyfikatory jednostek ziarno małe zwróconych przez kwerendę. W przypadku braku elementy podrzędne nie są zwracane domyślnie.
Dowiedz się więcej o filtrach [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) i [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Lokalizacji mogą filtry aplikacji, określ filtry zaawansowane dla dzieci.

Wynik fragmentu zawiera elementy podrzędne przestrzegać filtrów.

Obecnie kwerendy fragmentu nie zwraca nieprawidłowe ocen lub wydarzenia podmiotu. Dodatkowe informacje można uzyskać przy użyciu istniejącej kwerendy kondycji klaster.

### <a name="api"></a>INTERFEJS API
Aby uzyskać klaster kondycji fragmentu, Utwórz `FabricClient` i wywołać metodę [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) w jego **HealthManager**. Można przekazać w [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) w celu opisania zasady dotyczące kondycji i zaawansowanych filtrów.

Poniższy kod pobiera fragmentu kondycji klaster przy użyciu filtrów zaawansowanych.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>Programu PowerShell
Polecenia cmdlet, aby uzyskać kondycji klaster jest [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Najpierw połączyć się z klastrem za pomocą polecenia cmdlet [ServiceFabricCluster Połącz](https://msdn.microsoft.com/library/mt125938.aspx) .

Poniższy kod pobiera węzły tylko wtedy, gdy znajdują się one w błędu z wyjątkiem określonego węzła zawsze zostać zwrócone.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Następujące polecenie cmdlet pobiera fragmentu klaster filtrami aplikacji.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Następujące polecenie cmdlet zwraca wszystkie obiekty wdrożonym w węźle.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>POZOSTAŁE
Możesz uzyskać fragmentu kondycji klaster [Uzyskiwanie żądania](https://msdn.microsoft.com/library/azure/mt656722.aspx) lub [żądania POST](https://msdn.microsoft.com/library/azure/mt656721.aspx) zawiera zasady dotyczące kondycji i filtry zaawansowane opisane w treści.

## <a name="general-queries"></a>Kwerendy ogólne
Ogólne kwerendy zwracają wykaz podmiotów tkaninie usługi określonego typu. Są one dostępne za pośrednictwem interfejsu API (przy użyciu metody **FabricClient.QueryManager**), polecenia cmdlet programu PowerShell i Zatrzymaj. Te kwerendy agregują podkwerendy wiele składników. Jeden z nich jest [Przechowywanie kondycji](service-fabric-health-introduction.md#health-store), które wypełnia zagregowane kondycji dla poszczególnych wyników kwerendy.  

> [AZURE.NOTE] Ogólne kwerendy Zwraca zagregowaną kondycji jednostki i nie zawiera danych sformatowanego kondycji. Jeśli jednostka nie działa prawidłowo, możesz organizować przy użyciu kwerendy kondycji w celu uzyskania wszystkich jej informacji kondycji, takich jak zdarzenia, stany kondycji podrzędne i nieprawidłowe ocen.

Jeśli ogólne kwerend zwraca nieznany kondycję obiektu, jest to możliwe, że w magazynie kondycji nie ma pełnych danych o jednostce. Istnieje także możliwość, że podkwerendy w magazynie kondycji nie powiedzie (na przykład wystąpił błąd komunikacji lub w sklepie kondycji została ograniczenie). Flaga monitująca za pomocą kwerendy kondycji dla obiektu. W przypadku podkwerendę przejściowych błędów, takich jak w przypadku problemów z siecią monitowania kwerendy może się niepowodzeniem. Go może również możesz uzyskać więcej ze sklepu kondycji o Dlaczego jednostka nie jest dostępna.

Kwerendy, które zawierają **HealthState** dla obiektów są:

- Lista węzłów: zwraca listę węzłów w klastrze (stronicowanej).
  - Interfejs API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Lista aplikacji: zwraca listę aplikacji w klastrze (stronicowanej).
  - Interfejs API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Lista usług: zwraca listę usług w aplikacji (stronicowanej).
  - Interfejs API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Lista partition: zwraca listę partycje w usłudze (stronicowanej).
  - Interfejs API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Lista replice: zwraca listę repliki w partycją (stronicowanej).
  - Interfejs API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Wdrożony listy aplikacji: zwraca listę rozmieszczonych aplikacji w węźle.
  - Interfejs API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Wdrożony listą pakiet usług: zwraca listę pakietów usług we wdrożonej aplikacji.
  - Interfejs API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Niektóre kwerendy zwracają wyniki stronicowania. Zwrot te zapytania znajduje się lista pochodzących z [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Jeśli wyniki mieści się wiadomość, zwracany jest tylko strony i ContinuationToken, która śledzi miejsce, w którym zatrzymano wyliczenia. Powinny być nadal połączeń tej samej kwerendy i przekazywania tokenu kontynuacji z poprzedniej kwerendy w celu uzyskania wyników dalej.

### <a name="examples"></a>Przykłady

Poniższy kod w klastrze otrzymuje nieprawidłowe aplikacji:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Następujące polecenie cmdlet pobiera dane aplikacji tkaninie:-WordCount aplikacji. Należy zauważyć, że stan kondycji znajduje się na ostrzeżenie.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Następujące polecenie cmdlet pobiera usługi przy użyciu stanu kondycji ostrzeżenia:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Klaster i stosowanie uaktualnienia
Podczas uaktualniania monitorowane klaster i aplikacji usługi tkaninie sprawdza kondycji, aby upewnić się, czy wszystko jest prawidłowy. W przypadku nieprawidłowe obliczonego za pomocą zasad kondycji skonfigurowaną jednostka uaktualnienie dotyczy zasady specyficzne dla uaktualnienie do określenia następnej akcji. Uaktualnianie może być wstrzymane umożliwia interakcji użytkownika (na przykład rozwiązywanie błędów lub zmiana zasad) lub jej może automatycznie przywrócić poprzednią wersję warto.

Podczas uaktualniania *klaster* możesz uzyskać stanu uaktualniania klaster. Stan uaktualniania zawiera nieprawidłowe ocen, które wskazują co to jest nieprawidłowe w klastrze. Jeśli uaktualnienie zostanie przywrócona z powodu problemów z kondycji, stanu uaktualniania zapamiętuje ostatniej powodów nieprawidłowe. Te informacje mogą pomóc administratorom badanie na czym polega problem po uaktualnieniu wycofana lub zatrzymać.

Podobnie podczas uaktualniania *aplikacji* wszelkie nieprawidłowe oceny są zawarte w stanu uaktualniania aplikacji.

Poniżej pokazano stanu uaktualniania aplikacji tkaninie zmienione:-WordCount aplikacji. Watchdog zgłoszone komunikat o błędzie na jednym z jego repliki. Uaktualnianie jest stopniowych, ponieważ sprawdzanie kondycji nie zostaną zachowane.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Dowiedz się więcej o [aplikacji usługi tkaninie uaktualnienie](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą ocen kondycji
Zawsze, gdy występuje problem z klastrem lub aplikacji, spójrz na kondycji klaster lub aplikacji w celu określenia, na czym polega problem. Nieprawidłowe ocen zawierają szczegółowe opisy co wyzwalane bieżący stan nieprawidłowe. Jeśli chcesz, możesz Przechodzenie do szczegółów w obiektów podrzędnych nieprawidłowe do identyfikowania głównej przyczyny.

> [AZURE.NOTE] Nieprawidłowe ocen Pokaż pierwszy przyczyny jednostki są filtrowane w celu bieżący stan kondycji. Może być wiele zdarzeń wywołujących tego stanu, ale nie są odzwierciedlane w ocen. Aby uzyskać więcej informacji, przechodzenie do szczegółów podmioty kondycji ustalanie wszystkich raportów nieprawidłowe w klastrze.

## <a name="next-steps"></a>Następne kroki
[Używanie raportów dotyczących kondycji systemu rozwiązywać problemy](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Dodawanie niestandardowych raporty dotyczące kondycji usługi tkaninie](service-fabric-report-health.md)

[Jak zgłaszać i sprawdzanie kondycji usługi](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Monitorowanie i diagnozowanie usług lokalnie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md)
