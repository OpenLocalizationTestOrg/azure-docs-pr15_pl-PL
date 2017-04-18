<properties
   pageTitle="Rozwiązywanie problemów z raportów dotyczących kondycji systemu | Microsoft Azure"
   description="W tym artykule opisano raporty dotyczące kondycji wysyłane przez składniki tkaninie usługi Azure i ich zastosowania klaster rozwiązywania problemów lub problemów aplikacji."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Używanie raportów dotyczących kondycji systemu rozwiązywać problemy

Azure tkaninie usługi składniki Raport gotowych na wszystkich obiektów w grupie. [Przechowywanie kondycji](service-fabric-health-introduction.md#health-store) tworzy i usuwa jednostek opartych na raportów systemu. Go też porządkuje je w hierarchii, na którym interakcji jednostki.

> [AZURE.NOTE] Aby zrozumieć pojęcia dotyczące kondycji, przeczytaj więcej w [modelu kondycji usługi tkaninie](service-fabric-health-introduction.md).

Raporty dotyczące kondycji systemu zapewniają wgląd klaster i funkcji aplikacji i flagi problemów za pośrednictwem kondycji. Dla aplikacji i usług raporty dotyczące kondycji systemu sprawdź, czy jednostki są wykonywane i są poprawnie zachowuje z perspektywy tkaninie usługi. Raportów nie zawierają dowolne monitorowanie kondycji warunków logicznych usługi lub wykrywania zawieszanie procesów. Usług użytkowników mogą wzbogacić dane dotyczące kondycji informacje dotyczące ich.

> [AZURE.NOTE] Raporty dotyczące kondycji watchdogs są widoczne tylko *po* składniki systemu utworzyć obiekt. Jednostka zostanie usunięty, magazynie kondycja automatycznie usuwa wszystkie raporty dotyczące kondycji skojarzone z nim. Dotyczy to także po utworzeniu nowego wystąpienia jednostki (na przykład nowego wystąpienia replice usługi jest tworzony). Wszystkie raporty skojarzone z stare wystąpienie są usuwane i czyszczone ze sklepu.

Raporty składnika systemu są oznaczane źródło, w którym rozpoczyna się od "**System.**" prefiks. Watchdogs nie można użyć ten sam prefiks ich źródeł, jak będą odrzucane raportów przy użyciu nieprawidłowych parametrów.
Przyjrzyjmy się niektóre raporty systemu, aby dowiedzieć się, co wywołujących je i rozwiązania możliwych problemów, które reprezentują.

> [AZURE.NOTE] Usługa tkaninie w dalszym ciągu dodawać raporty na warunkach zainteresowania, które poprawić wgląd co się dzieje w klastrze i aplikacji.

## <a name="cluster-system-health-reports"></a>Raporty dotyczące kondycji systemu klaster
Jednostki kondycji klaster jest tworzony automatycznie w magazynie kondycji. Jeśli wszystko działa poprawnie, nie zawiera on raport systemu.

### <a name="neighborhood-loss"></a>Utrata klubu
**System.Federation** zgłasza błąd, gdy wykrywa utratę klubu. Raport jest na pojedynczych węzłów i identyfikator węzła znajduje się nazwa właściwości. Jeden klubu utraconych całego Dzwoń tkaninie usługi zwykle można oczekiwać dwa zdarzenia (obie strony raportu odstęp). W przypadku utraty więcej kluby istnieje kolejnych zdarzeń.

Raport określa limit czasu dzierżawy globalnej jako czas wygaśnięcia. Raport jest płatność każdej połowie TTL Czas trwania, jak warunek pozostaje aktywna. Wydarzenia są usuwane automatycznie po jej wygaśnięciu. W przypadku wygasłe zachowanie gwarantuje, że raport jest wyczyszczone ze sklepu kondycji poprawnie, nawet jeśli węzeł raportowania usunąć.

- **SourceId**: System.Federation
- **Właściwość**: zaczyna się od **klubu** oraz informacje węzeł
- **Następne kroki**: badanie, dlaczego klubu zostaną utracone (na przykład sprawdzić komunikacji między węzłach).

## <a name="node-system-health-reports"></a>Raporty dotyczące kondycji systemu węzeł
**System.FM**, która reprezentuje usługę Menedżer pracy awaryjnej, jest urząd, która zarządza informacji na temat węzłach. Każdy węzeł powinien mieć jeden raport z System.FM przedstawiający stanu. Jednostki węzeł są usuwane po usunięciu stan węzła (zobacz [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Węzeł wzrost spadek
System.FM raportów jako przycisk OK, gdy węzeł dołączy dzwonienia (jest to i rozpocząć pracę). Zgłasza błąd, gdy węzeł odjazdem Dzwoń (jest dół albo uaktualniania lub po prostu ponieważ nie powiodło się). Hierarchia kondycji dostępne w sklepie kondycji przejmuje akcji wdrożonym jednostki korelacji System.FM węzeł raportów. Uważa węzeł element nadrzędny wirtualny wszystkich jednostek wdrożonym. Jednostki wdrożonym w tym węźle są dostępne za pomocą kwerendy, jeśli węzeł jest zgłoszone w górę przez System.FM, w tym samym wystąpieniu jako wystąpienia skojarzonego z podmiotów. Kiedy System.FM zgłasza, że węzeł nie działa lub ponowne uruchomienie (nowego wystąpienia), magazynie kondycji automatycznie czyści wdrożonym obiekty, które mogą znajdować się tylko na dół węzeł lub poprzednie wystąpienie węzła.

- **SourceId**: System.FM
- **Właściwość**: stan
- **Następne kroki**: Jeśli węzeł nie działa w przypadku uaktualnienia, go należy przywracane po została uaktualniona. W tym przypadku stan kondycji powinny się przełączyć ponownie przycisk OK. Jeśli węzeł nie wróci lub go nie powiedzie się, problem wymaga więcej dochodzenia.

W poniższym przykładzie pokazano zdarzenie System.FM o kondycji OK węzła w górę:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Certyfikat wygaśnie
**System.FabricNode** raportów ostrzeżenie w przypadku certyfikaty używane przez węzeł u wygaśnięcia. Istnieją trzy certyfikatów dla węzła: **Certificate_cluster**, **Certificate_server**i **Certificate_default_client**. Po co najmniej dwa tygodnie, po upływie stan kondycji raport jest przycisk OK. Jeśli po upływie do dwóch tygodni, typ raportu jest ostrzeżenie. TTL te zdarzenia jest nieograniczony, i są usuwane po węzeł opuszcza klaster.

- **SourceId**: System.FabricNode
- **Właściwość**: zaczyna się od **certyfikatu** i zawiera więcej informacji na temat typ certyfikatu
- **Następne kroki**: aktualizowanie certyfikatów, jeśli znajdują się obok wygaśnięcia.

### <a name="load-capacity-violation"></a>Naruszenie wydajność ładowania
Usługi równoważenia obciążenia tkaninie raportów ostrzeżenie, jeśli wykryje naruszenie zdolności węzeł.

 - **SourceId**: System.PLB
 - **Właściwość**: zaczyna się od **pojemności**
 - **Następne kroki**: sprawdzanie pod warunkiem metryki i wyświetlanie możliwości bieżącej w węźle.

## <a name="application-system-health-reports"></a>Raporty dotyczące kondycji systemu aplikacji
**System.CM**, która reprezentuje usługę Menedżera klaster, jest urząd, która zarządza informacji na temat aplikacji.

### <a name="state"></a>Województwo
System.CM raportów jako OK aplikacja została utworzona lub zaktualizowana. Informuje sklepu kondycji usunięcie aplikacji tak, aby może być usuwany ze sklepu.

- **SourceId**: System.CM
- **Właściwość**: stan
- **Następne kroki**: Jeśli aplikacja została utworzona, powinien zawierać raport dotyczący kondycji Menedżer klastrów. W przeciwnym razie sprawdź stan aplikacji wysyłając kwerendę (na przykład programu PowerShell polecenia cmdlet * *Get-ServiceFabricApplication ApplicationName *applicationName***).

W poniższym przykładzie pokazano zdarzenie stanu na **tkaninie:-WordCount** aplikacji:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Raporty dotyczące kondycji systemu usługi
**System.FM**, która reprezentuje usługę Menedżer pracy awaryjnej, jest urząd, która zarządza informacji o usługach.

### <a name="state"></a>Województwo
System.FM raportów jako OK po utworzeniu usługę. Usuwa jednostki ze sklepu kondycji po usunięciu usługę.

- **SourceId**: System.FM
- **Właściwość**: stan

W poniższym przykładzie pokazano zdarzenie stanu w usłudze **tkaninie: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Naruszenie Nierozmieszczone repliki
**System.PLB** raportów ostrzeżenie, jeśli nie możesz znaleźć położenie dla jednej lub większej liczbie replik usługi. Raport są usuwane po jej wygaśnięciu.

- **SourceId**: System.FM
- **Właściwość**: stan
- **Następne kroki**: sprawdzanie ograniczeń usługi i bieżący stan położenie.

W poniższym przykładzie pokazano naruszenia usługi skonfigurowano 7 repliki docelowej w klastrze z 5 węzłów:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Raporty dotyczące kondycji systemu partition
**System.FM**, która reprezentuje usługę Menedżer pracy awaryjnej, jest urząd, która zarządza informacji na temat partycje usługi.

### <a name="state"></a>Województwo
System.FM raportów jako OK po partycją została utworzona i nie jest uszkodzony. Usuwa jednostki ze sklepu kondycji po usunięciu partycją.

W przypadku partycją poniżej replice minimalna liczba, zgłasza błąd. Jeśli partycją jest poniżej replice minimalna liczba, ale jest mniejsza liczba replice docelowej, raportów ostrzeżenie. W przypadku partycją utratę kworum, System.FM zgłasza błąd.

Gdy zmianę konfiguracji trwa dłużej niż oczekiwano, a gdy Kompilacja trwa dłużej niż oczekiwano, innych ważnych wydarzeń zawierać ostrzeżenie. Oczekiwany czas kompilacji i ponowna konfiguracja są konfigurowane na podstawie scenariuszy usług. Na przykład w przypadku usługi terabajtów stanu, takich jak baza danych SQL, Kompilacja trwa dłużej niż usługi z niewielkiej ilości stan.

- **SourceId**: System.FM
- **Właściwość**: stan
- **Następne kroki**: Jeśli stan kondycji nie jest przycisk OK, jest możliwe, że niektóre repliki nie zostały utworzone, otwarte lub promowane głównego i pomocniczego poprawnie. W wielu przypadkach głównej przyczyny jest to błąd usługi w wersji protokołu open lub zmień rolę.

W poniższym przykładzie pokazano partycją prawidłowy:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

W poniższym przykładzie pokazano kondycji partycją jest mniejsza liczba replice docelowej. Następnym krokiem jest, aby uzyskać opis partition pokazano, jak jest skonfigurowany: **MinReplicaSetSize** jest dwa i **TargetReplicaSetSize** 7. Następnie uzyskać liczby węzłów w klastrze: pięć. Aby w tym przypadku dwie repliki nie można umieścić.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Naruszenie ograniczenia replice
**System.PLB** raportów ostrzeżenie, jeśli wykryje naruszenie ograniczenia replice i nie można umieścić repliki partycją.

- **SourceId**: System.PLB
- **Właściwość**: zaczyna się od **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Raporty dotyczące kondycji systemu replice
**System.RA**, reprezentujące zmiany konfiguracji składnika agent jest Urząd stanu replice.

### <a name="state"></a>Województwo
**System.RA** raportów jako OK po utworzeniu replice.

- **SourceId**: System.RA
- **Właściwość**: stan

W poniższym przykładzie pokazano replice prawidłowy:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Stan otwarcia replice
Opis ten raport dotyczący kondycji zawiera czas rozpoczęcia (uniwersalny czas koordynowany) podczas połączenia interfejsu API.

**System.RA** raporty ostrzeżenie, jeśli replice otwieranie trwa dłużej niż skonfigurowanego okresu (domyślny: 30 minut). Jeśli API wpływa na dostępność usługi, raport jest wydawany znacznie szybsze (można konfigurować interwał, domyślnie 30 sekund). Czas zawiera czas potrzebny na Otwórz Replikator i otwórz usługi. Po zakończeniu Otwórz właściwość zmienia się na przycisk OK.

- **SourceId**: System.RA
- **Właściwość**: **ReplicaOpenStatus**
- **Następne kroki**: Jeśli stan kondycji nie jest przycisk OK, badanie, dlaczego replice otwieranie trwa dłużej niż oczekiwano.

### <a name="slow-service-api-call"></a>Usługa działa wolno interfejsu API połączenia
Raport **System.RAP** i **System.Replicator** ostrzeżenie, jeśli połączenia do użytkownika kod usługi trwa dłużej niż skonfigurowanego czasu. To ostrzeżenie jest wyczyszczone, po zakończeniu połączenia.

- **SourceId**: System.RAP lub System.Replicator
- **Właściwość**: Nazwa powolne interfejsu API. Opis zawiera szczegółowe informacje o czasie API zostało oczekiwanie.
- **Następne kroki**: badanie, dlaczego połączenie trwa dłużej niż oczekiwano.

W poniższym przykładzie pokazano partycją utratę kworum i kroki postępowania gotowe, aby dowiedzieć się, dlaczego. Ma jednej z replik stanowi kondycji ostrzeżenie, więc zostanie wyświetlony jego kondycji. Informuje, że operacji usługi trwa dłużej niż oczekiwano, zdarzenie zgłaszane przez System.RAP. Po odebraniu tych informacji, następnym krokiem jest Spójrz na kod usługi i badanie. W tym przypadku **RunAsync** stosowania usługę stanowe generuje nieobsługiwany wyjątek. Odtwarzanie są repliki, więc mogą nie być widoczne wszystkie repliki w stanie ostrzeżenia. Możesz ponowić próbę wyświetlany stan kondycji i poszukaj różnice w replice identyfikatora. W niektórych przypadkach ponowne próby można uzyskać wskazówek.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Po uruchomieniu błędnej aplikacji w obszarze debugowania zdarzeń diagnostycznych systemu windows Pokaż wyjątek z RunAsync:

![Visual Studio 2015 zdarzeń diagnostycznych: błąd RunAsync tkaninie:-HelloWorldStatefulApplication.][1]

Visual Studio 2015 zdarzeń diagnostycznych: błąd RunAsync **tkaninie:-HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Pełna kolejki replikacji
**System.Replicator** raportów ostrzeżenie, jeśli kolejki replikacji jest pełny. Na podstawową zwykle dzieje się tak ponieważ jeden lub więcej pomocniczej repliki są powolne potwierdzenia operacji. Na pomocniczym zwykle dzieje gdy usługa działa wolno stosowanie operacji. Ostrzeżenie jest wyczyszczone, gdy kolejka już nie jest pełna.

- **SourceId**: System.Replicator
- **Właściwość**: **PrimaryReplicationQueueStatus** lub **SecondaryReplicationQueueStatus**, w zależności od roli replice

### <a name="slow-naming-operations"></a>Powolne operacje nazw

**System.NamingService** raportów kondycji w jego podstawowego replice podczas operacji nazw trwa dłużej niż dopuszczalne. Przykłady nazw operacji są [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) lub [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Więcej metod można znaleźć w FabricClient, na przykład w obszarze [metody zarządzania usługi](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) lub [metody zarządzania właściwości](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Usługa nazw jest rozpoznawana jako nazwy usług do lokalizacji w klastrze i umożliwia użytkownikom zarządzanie nazwy usług i właściwości. Jest to tkaninie usługi partycją trwała usługi. Jedną z partycje reprezentuje właściciel urząd zawiera metadane dotyczące wszystkich usług i nazwy tkaninie usługi. Nazwy tkaninie usługi są mapowane na różne partycje o nazwie partycje właściciela, dlatego extensible usługę. Dowiedz się więcej o [usłudze nazw](service-fabric-architecture.md).

Podczas operacji nazw trwa dłużej niż oczekiwano, operacja jest oflagowane z raportem w formie ostrzeżenie *podstawowego replice partycją usługi nazw służy tej operacji*. Jeśli operacja zakończy się pomyślnie, ostrzeżenie jest wyczyszczone. Po zakończeniu operacji z powodu błędu, raport dotyczący kondycji zawiera szczegółowe informacje o błędzie.

- **SourceId**: System.NamingService
- **Właściwość**: rozpoczyna się prefiks **Duration_** i służy do identyfikowania wolne działanie i nazwę tkaninie usługi, na którym jest stosowany operacji. Na przykład jeśli utworzyć usługi na tkaninie Nazwa: / MyApp/Moja_usługa trwa zbyt długo, właściwość jest Duration_AOCreateService.fabric:/MyApp/MyService. AO punktów do roli partycją nazw dla tej nazwy i operacji.
- **Następne kroki**: Sprawdź, dlaczego nazw kończy się niepowodzeniem. Kolejne operacje mogą zawierać różne przyczyn. Na przykład usunąć usługi mogą zostać zatrzymane w węźle, ponieważ hosta aplikacji przechowuje awarii w węźle ze względu na błąd użytkownika w kodzie usługi.

W poniższym przykładzie pokazano tworzenie operacji usługi. Czynność ma być dłuższa niż skonfigurowanego czasu trwania. AO prób i wysyła pracy wartość nie. NIE ukończono ostatniej operacji limitu czasu. W tym przypadku samej replice jest dokumentem głównym zarówno AO i żadnych ról.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>Raporty dotyczące kondycji systemu DeployedApplication
**System.Hosting** jest urząd na wdrożonym podmiotów.

### <a name="activation"></a>Aktywacja
System.Hosting raportów jako OK, gdy aplikacja został pomyślnie aktywowany w węźle. W przeciwnym razie zgłasza błąd.

- **SourceId**: System.Hosting
- **Właściwość**: aktywacji, w tym wersję rozmieszczenia
- **Następne kroki**: Jeśli aplikacja jest nieprawidłowe, badanie niepowodzenia aktywacji.

W poniższym przykładzie pokazano Aktywacja powiodła:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Plik do pobrania
**System.Hosting** zgłasza błąd, jeśli Pobierz pakiet aplikacji nie powiedzie się.

- **SourceId**: System.Hosting
- **Właściwość**: * *Pobieranie:*RolloutVersion***
- **Następne kroki**: badanie niepowodzenia pobierania w węźle.

## <a name="deployedservicepackage-system-health-reports"></a>Raporty dotyczące kondycji systemu DeployedServicePackage
**System.Hosting** jest urząd na wdrożonym podmiotów.

### <a name="service-package-activation"></a>Aktywacja pakietu usługi
System.Hosting raportów jako OK, jeśli aktywacja pakietu usługi w węźle zostanie przeprowadzone pomyślnie. W przeciwnym razie zgłasza błąd.

- **SourceId**: System.Hosting
- **Właściwość**: aktywacji
- **Następne kroki**: badanie niepowodzenia aktywacji.

### <a name="code-package-activation"></a>Aktywacja pakietu kodu
**System.Hosting** raportów jako OK dla każdego pakietu kod Jeśli aktywacja zostanie przeprowadzone pomyślnie. Jeśli aktywacja nie powiedzie się, raportów ostrzeżenie zgodnie z konfiguracją. Jeśli **CodePackage** kończy się niepowodzeniem aktywować lub kończy się komunikat o błędzie większa niż skonfigurowaną **CodePackageHealthErrorThreshold**, hostingu zgłasza błąd. Jeśli pakiet usługa zawiera wiele pakietów kodu, Generowanie raportu aktywacji dla każdego z nich.

- **SourceId**: System.Hosting
- **Właściwość**: korzysta z prefiksem **CodePackageActivation** i zawiera nazwę pakietu kod i punkt wejścia jako * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint-punktu wejścia* ** (na przykład **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Rejestracja typu usług
**System.Hosting** raportów jako OK, jeśli typ usługi został pomyślnie zarejestrowany. Jeśli rejestracji nie została wykonana w czasie (zgodnie z konfiguracją przy użyciu **ServiceTypeRegistrationTimeout**), zgłasza błąd. Jeśli typ usługi jest zarejestrowany z węzła, wynika to czas został zamknięty. Obsługa raportów ostrzeżenie.

- **SourceId**: System.Hosting
- **Właściwość**: korzysta z prefiksem **ServiceTypeRegistration** i zawiera nazwę typu (na przykład **ServiceTypeRegistration:FileStoreServiceType**)

W poniższym przykładzie pokazano pakiet prawidłowy wdrożonym usługi:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Plik do pobrania
**System.Hosting** zgłasza błąd, jeśli usługa pobrania pakietu nie powiodła się.

- **SourceId**: System.Hosting
- **Właściwość**: * *Pobieranie:*RolloutVersion***
- **Następne kroki**: badanie niepowodzenia pobierania w węźle.

### <a name="upgrade-validation"></a>Weryfikacja uaktualnienia
Jeśli sprawdzanie poprawności podczas uaktualniania kończy się niepowodzeniem, lub Jeśli uaktualnienie kończy się niepowodzeniem w węźle, **System.Hosting** zgłasza błąd.

- **SourceId**: System.Hosting
- **Właściwość**: korzysta z prefiksem **FabricUpgradeValidation** i zawiera uaktualnionej wersji
- **Opis**: wskazuje wystąpił błąd

## <a name="next-steps"></a>Następne kroki
[Wyświetlanie raportów dotyczących kondycji usługi tkaninie](service-fabric-view-entities-aggregated-health.md)

[Jak zgłaszać i sprawdzanie kondycji usługi](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Monitorowanie i diagnozowanie usług lokalnie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md)
