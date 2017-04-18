<properties
   pageTitle="Akcja pola | Microsoft Azure"
   description="Ten artykuł zawiera informacje o akcje pola znalezionych na tkaninie usługi Azure firmy Microsoft."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Akcje pola
W celu zasymulowania mało wiarygodnych infrastruktury, tkaninie usługi Azure umożliwia, dewelopera, przy użyciu metody symulacji różnych błędy rzeczywistych i stan przejścia. Dostępne są następujące jako pola akcje. Dostępne są następujące akcje niższego poziomu interfejsów API, które powodują uruchomienie określonych błędów, zmiana stanu lub sprawdzania poprawności. Łącząc te akcje, można zapisywać scenariuszy testowania Pełna dla usług.

Usługa tkaninie zawiera kilka typowych scenariuszy test składający się z te akcje. Zdecydowanie zalecamy są używane te wbudowane scenariuszy, które dokładnie chcą testowanie typowych przejścia stanów i przypadkach błąd. Jednak akcje mogą być używane do tworzenia scenariuszy testowania niestandardowe, gdy chcesz dodać pokrycia scenariuszy, które nie są objęte wbudowanych scenariusze jeszcze lub które są niestandardowe dostosowane do aplikacji.

C# implementacji akcje znajdują się w zestawie System.Fabric.dll. Moduł programu PowerShell tkaninie System znajduje się w zestawie Microsoft.ServiceFabric.Powershell.dll. W ramach instalacji środowisko uruchomieniowe modułu programu ServiceFabric PowerShell zainstalowano umożliwiające łatwość użytkowania.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Bezpieczne a błąd nieprawidłowego działania
Akcje pola dzieli się na dwa główne pakiety:

* Błędy nieprawidłowego: tych błędów symulacji awarii, takich jak ponowne uruchamianie komputera i przetwarzanie awarii. W takich przypadkach błędy kontekstu wykonania procesu przestaje nieprawidłowo. Oznacza to, że nie Oczyszczanie stan może zostać uruchomiony przed ponownym uruchomieniu aplikacji.

* Bezpieczne błędów: tych błędów zasymulowania bezpieczne akcji, takich jak przenosi replice i krople wyzwalane przez równoważenia obciążenia. W takich przypadkach usługa otrzymuje powiadomienie po zamknięciu i mogą oczyścić stan przed zakończeniem.

Lepsza kontrola jakości uruchomienie obciążenie pracą usługi i firmy podczas wywołania różnych bezpieczne i nieprawidłowego błędów. Błędy nieprawidłowego wykonuje scenariusze, w którym proces usługi nieprawidłowo zamknięciu środku niektórych przepływu pracy. Testy ścieżka odzyskiwania, po przywróceniu replice usługi przez usługę tkaninie. Dzięki temu testowanie spójności danych i tego, czy stan usługi jest obsługiwane poprawnie po awarii. Zestaw błędy (błędy bezpieczne) sprawdzić, czy w repliki przenoszenie przez usługę tkaninie przypadku poprawnie usługę. Obsługa anulowania to testów w metodzie RunAsync. Usługa musi Sprawdź token anulowania jest ustawiona, poprawnie zapisać stanu, a następnie zamknij metody RunAsync.

## <a name="testability-actions-list"></a>Lista akcji pola

| Akcja | Opis | Zarządzany interfejs API | Polecenia cmdlet programu PowerShell | Bezpieczne nieprawidłowego błędów |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Usuwa wszystkie stan testu z klaster w przypadku nieprawidłowych zamknięcia sterownika test. | CleanTestStateAsync | Usuń ServiceFabricTestState | Nie dotyczy |
| InvokeDataLoss | Powoduje utratę danych do partycją usługi. | InvokeDataLossAsync | Wywołania ServiceFabricPartitionDataLoss | Bezpieczne |
| InvokeQuorumLoss | Umieszczenie partycją danej usługi stanowe w utratę kworum. | InvokeQuorumLossAsync | Wywołania ServiceFabricQuorumLoss | Bezpieczne |
| Przenoszenie podstawowa | Przenosi określony replice podstawowego stanowe usługi do określonego węzła. | MovePrimaryAsync | Przenieś ServiceFabricPrimaryReplica | Bezpieczne |
| Przenoszenie pomocniczego | Przenosi bieżący replice pomocniczej stanowe usługi do innego węzła. | MoveSecondaryAsync | Przenieś ServiceFabricSecondaryReplica | Bezpieczne |
| RemoveReplica | Symuluje awarii replice przez usunięcie replice z klastrem. To zostanie zamknięty replice i spowoduje przejście do roli "Brak", usunięcie wszystkich stanu z klastrem. | RemoveReplicaAsync | Usuń ServiceFabricReplica | Bezpieczne |
| RestartDeployedCodePackage | Symuluje awaria procesu pakiet kod, uruchamiając pakiet kod wdrożony w węźle w klastrze. Proces pakiet kod, który zostanie uruchomiony ponownie wszystkie repliki usługa użytkownika hostowana w ten proces to przerwana. | RestartDeployedCodePackageAsync | Uruchom ponownie ServiceFabricDeployedCodePackage | Nieprawidłowego |
| RestartNode | Symuluje węzła klaster tkaninie usługi, uruchamiając węzeł. | RestartNodeAsync | Uruchom ponownie ServiceFabricNode | Nieprawidłowego |
| RestartPartition | Symuluje centrum danych na kolor czarny lub klaster na kolor czarny scenariusza, uruchamiając niektórych lub wszystkich replik partycją. | RestartPartitionAsync | Uruchom ponownie ServiceFabricPartition | Bezpieczne |
| RestartReplica | Błąd replice symuluje ponowne uruchomienie replice trwałe w klastrze, zamykając replice i otwierając go ponownie. | RestartReplicaAsync | Uruchom ponownie ServiceFabricReplica | Bezpieczne |
| Węzeł_początkowy | Zostanie uruchomiony węzeł w klastrze, który już został zatrzymany. | StartNodeAsync | Rozpocznij ServiceFabricNode | Nie dotyczy |
| StopNode | Symuluje awarii węzła przez zatrzymanie węzła w klastrze. Węzeł pozostanie w dół do momentu nosi nazwę Węzeł_początkowy. | StopNodeAsync | Zatrzymaj ServiceFabricNode | Nieprawidłowego |
| ValidateApplication | Sprawdza dostępności i kondycji wszystkich usług tkaninie usługi aplikacji, zwykle po wywołania niektórych błędów w systemie. | ValidateApplicationAsync | Test ServiceFabricApplication | Nie dotyczy |
| ValidateService | Sprawdza dostępności i kondycji usługi tkaninie usługi, zwykle po wywołania niektórych błędów w systemie. | ValidateServiceAsync | Test ServiceFabricService | Nie dotyczy |

## <a name="running-a-testability-action-using-powershell"></a>Uruchamianie akcji pola, przy użyciu programu PowerShell

Tym samouczku pokazano, jak uruchomić akcję pola przy użyciu programu PowerShell. Dowiesz uruchamianie akcji pola przed lokalne klaster (z nich pole) lub Azure klaster. Microsoft.Fabric.Powershell.dll — modułu programu PowerShell tkaninie usługi — zostanie zainstalowany automatycznie podczas instalacji MSI tkaninie usług firmy Microsoft. Moduł załadowaniu automatycznie podczas otwierania wierszu polecenia programu PowerShell.

Samouczki segmentów:

- [Uruchamianie akcji przed klastrze jednym polu](#run-an-action-against-a-one-box-cluster)
- [Uruchamianie akcji przed klastrze Azure](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Uruchamianie akcji przed klastrze jednym polu

Aby uruchomić akcję pola przed klaster lokalny, połącz się z klastrem i otwórz wierszu polecenia programu PowerShell w trybie administratora. Pozwól nam Przyjrzyj się akcji **ServiceFabricNode ponowne uruchomienie komputera** .

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

W tym miejscu Akcja **Uruchom ponownie ServiceFabricNode** zostanie uruchomione na węzeł o nazwie "Node1". Tryb zakończenia określa, że także należy sprawdza, czy akcja Uruchom ponownie węzeł faktycznie zakończyła się pomyślnie. Określanie tryb zakończenia jako "Zweryfikować" spowoduje, że należy sprawdzić, czy akcja Uruchom ponownie faktycznie zakończyła się pomyślnie. Zamiast bezpośredniego określania węzeł przez jego nazwę, można ją określić za pomocą klawisza partycją i rodzaju replice, w następujący sposób:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Uruchom ponownie ServiceFabricNode** należy ponownie uruchomić węzeł tkaninie usługi w klastrze. Spowoduje to zatrzymanie procesu Fabric.exe, w którym będzie Uruchom ponownie wszystkie system usługi i użytkownika usługi repliki hostowana w tym węźle. Za pomocą tego interfejsu API usługi ułatwia odkrywanie usterek wzdłuż ścieżki odzyskiwania pracy awaryjnej. Pomaga symulacji awarii węzła w klastrze.

Następujące zrzucie ekranu pokazano **ServiceFabricNode ponowne uruchomienie** polecenia pola w działaniu.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Wynik pierwszy **Get-ServiceFabricNode** (polecenia cmdlet z modułu programu PowerShell tkaninie Service) pokazuje, że klaster lokalny zawiera pięć węzły: Node.1 do Node.5. Po wykonaniu akcji pola (polecenia cmdlet) **ServiceFabricNode ponowne uruchomienie komputera** w węzeł o nazwie Node.4, zobaczymy zresetowaniu węzła czas pracy.

### <a name="run-an-action-against-an-azure-cluster"></a>Uruchamianie akcji przed klastrze Azure

Uruchomienie akcji pola (przy użyciu programu PowerShell) przed Azure klaster jest podobny do uruchamiania akcji przed klaster lokalny. Jedyna różnica polega na tym, że przed uruchomieniem akcji, zamiast nawiązywanie połączenia z klastrem lokalnych, należy najpierw utworzyć połączenie z klastrem Azure.

## <a name="running-a-testability-action-using-c35"></a>Uruchamianie akcji pola, przy użyciu C & #35;

Aby uruchomić akcję pola przy użyciu języka C#, należy najpierw połączyć się z klastrem przy użyciu FabricClient. Następnie uzyskać parametry, aby uruchomić akcję. Aby uruchomić tę samą czynność można użyć różnych parametrów.
Jednym ze sposobów go uruchomić przeglądająca akcji RestartServiceFabricNode, jest, korzystając z informacji węzeł (nazwa węzła i identyfikator wystąpienia węzła) w grupie.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Objaśnienie parametr:

- **CompleteMode** Określa, że tryb nie należy sprawdzić, czy akcja Uruchom ponownie faktycznie zakończyła się pomyślnie. Określanie tryb zakończenia jako "Zweryfikować" spowoduje, że należy sprawdzić, czy akcja Uruchom ponownie faktycznie zakończyła się pomyślnie.  
- **OperationTimeout** ustawia ilość czasu na zakończenie przed wyjątek TimeoutException operacji.
- **CancellationToken** umożliwia oczekiwanie na numer telefonu, aby je anulować.

Zamiast bezpośredniego określania węzeł przez jego nazwę, można ją określić za pomocą klawisza partycją i rodzaju replice.

Aby uzyskać więcej informacji zobacz [PartitionSelector i ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector i ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector jest Pomocnik widocznych w pola i służy do wyboru partycją określonych, na którym ma wykonywać żadnych akcji pola. Może służyć do wybierz określone partition, jeśli wcześniej znany jest identyfikator partition. Można także umożliwiają klucz partycją i operacji rozwiąże identyfikator wewnętrznie. Masz również wybrać partycją losowe.

Aby użyć tego Pomocnik, Utwórz obiekt PartitionSelector i zaznacz partycją, stosując jedną z metod Select *. Następnie przekazać w obiekcie PartitionSelector API, która go wymaga. Jeśli opcja nie jest zaznaczona, domyślnie partycją losowe.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector jest Pomocnik widocznych w pola i jest używany przy wyborze replice, na którym ma wykonywać żadnych akcji pola. Może służyć do wybierz określone replice, jeśli wcześniej znany jest identyfikator replice. Ponadto istnieje możliwość wybrania replice podstawowego lub przypadkowe pomocniczym. ReplicaSelector pochodzi z PartitionSelector, więc należy wybrać zarówno replice i partycją, na którym chcesz wykonać operację pola.

Aby użyć tej pomocy, tworzenie obiektu ReplicaSelector i ustawić tak, aby wybrać replice i partycją. Można następnie przekazać je do interfejsu API, która go wymaga. Jeśli opcja nie jest zaznaczona, domyślnie losowe replice i partition losowe.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Następne kroki

- [Scenariusze pola](service-fabric-testability-scenarios.md)
- Jak przetestować usługi
   - [Symulacji awarii podczas obciążeń pracą usługi](service-fabric-testability-workload-tests.md)
   - [Błędy komunikacji do usługi](service-fabric-testability-scenarios-service-communication.md)
