<properties
   pageTitle="Jak wywołać utratą danych w usługach tkaninie usługi | Microsoft Azure"
   description="Informacje dotyczące używania utratą danych interfejsu api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Jak wywołać utratą danych w usługach

>[AZURE.WARNING] Ten dokument opisano spowodować utratę danych w swoich usług i powinny być używane z rozwagą.

## <a name="introduction"></a>Wprowadzenie
Można wywołać utracie danych na partycją usługi tkaninie usługi przez wywołania StartPartitionDataLossAsync().  Ten interfejs api używa uruchomienie błędów i Analysis Services wykonywanie pracy spowodowało warunków utracie danych.

## <a name="using-the-fault-injection-and-analysis-service"></a>Za pomocą uruchomienie błędów i Analysis Services

Uruchomienie błędów i Analysis Services obsługuje obecnie następujące interfejsy API w poniższej tabeli.  Po prawej stronie wykresu widać odpowiedniego polecenia cmdlet programu PowerShell.  Zapoznaj się z dokumentacją msdn w każdej interfejsu API, aby uzyskać więcej informacji na temat poszczególnych.

|           INTERFEJS API JĘZYKA C#                    |         Polecenia Cmdlet programu PowerShell                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Rozpocznij ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Rozpocznij ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Rozpocznij ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Omówienie uruchomienia polecenia

Uruchomienie błędów i usług Analysis używają asynchroniczne modelu miejsce, w którym można uruchomić polecenia przy użyciu jednego interfejsu API, określane jako interfejsu API "Start" w tym dokumencie, a następnie sprawdza postępu tego polecenia za pomocą "GetProgress" interfejsu API, aż osiągnie ona Państwie końcowych lub dopóki nie zostanie anulowana.
Aby uruchomić polecenia, zadzwoń interfejsu API "Start" dla odpowiedniego interfejsu API.  Ten interfejs API zwraca kiedy uruchomienie błędów i usług Analysis zaakceptuje zaproszenie.  Jednak nie oznacza ile zostało uruchomione polecenie, lub nawet wtedy, gdy została jeszcze rozpoczęta.  Aby sprawdzić postęp polecenia, zadzwoń interfejs API, który odpowiada interfejsu API "Start" wcześniej nazywane "GetProgress".  "GetProgress" interfejsu API zwróci obiektu wskazującym bieżący stan polecenia wewnątrz jego właściwość stan.  Czas nieokreślony poczekaj, aż zostanie uruchomione polecenie:

1.  Jego zakończy się pomyślnie.  Jeśli w tym przypadku połączeń "GetProgress" nad nim, zostanie ukończona stanu obiektu postępu.
2.  Napotkania błędu krytycznego.  Jeśli w tym przypadku połączenie "GetProgress" nad nim, stan obiektu postępu wystąpi błąd
3.  Subskrypcja zostanie anulowana przez [CancelTestCommandAsync]  [ cancel] interfejsu API lub [Zatrzymaj ServiceFabricTestCommand]  [ cancelps] polecenia cmdlet programu PowerShell.  Jeśli w tym przypadku połączeń "GetProgress" nad nim, stan obiektu postęp będzie odwołania lub ForceCancelled, w zależności od argument tego interfejsu API.  Zapoznaj się z dokumentacją dla [CancelTestCommandAsync]  [ cancel] uzyskać więcej szczegółowych informacji.


## <a name="details-of-running-a-command"></a>Szczegółowe informacje o uruchamianiu polecenia

Aby uruchomić polecenia, zadzwoń API rozpocząć z argumentami oczekiwany.  Wszystkich interfejsów API Rozpocznij ma wartość argumentu Guid o nazwie operationId.  Możesz należy zachować informacje o operationId argument, ponieważ jest używany w celu śledzenia postępu tego polecenia.  Muszą być przekazywane do "GetProgress" interfejsu API w celu śledzenia postępu polecenia.  OperationId musi być unikatowa.

Po pomyślnym wywołaniu interfejsu API rozpocząć, interfejsu API GetProgress powinna być wywoływana w pętli, dopóki nie zakończy się właściwość stan obiektu zwracane postępu.  Wszystkie [osoby FabricTransientException]  [ fte] i wykonana przez OperationCanceledException należy ponownie.
Po osiągnięciu końcowych stan (wykonane, Faulted lub odwołania) polecenie Właściwości obiektu postępu zwracany wynik uzyskuje dodatkowe informacje.  Jeśli stan zostanie zakończone, Result.SelectedPartition.PartitionId będzie zawierać identyfikator partition, który został wybrany.  Result.Exception będzie wartość null.  Jeśli wystąpił błąd stanu Result.Exception uzyskuje przyczyny uruchomienie błędów i Analysis Services umieszczone polecenia.  Result.SelectedPartition.PartitionId będą mieć identyfikator partition, który został wybrany.  W niektórych sytuacjach polecenia mogą nie mieć prowadzi daleko wybierz partycją.  W takim przypadku PartitionId będzie równa 0.  Jeśli stan zostanie anulowana, Result.Exception będzie wartość null.  Jak w przypadku Faulted Result.SelectedPartition.PartitionId będą mieć identyfikator partycją, która została wybrana, ale jeśli to polecenie nie prowadzi daleko to zrobić, będzie 0.  Również zajrzyj do poniższych przykładowych.

Przykładowe poniższy kod jak uruchomić, a następnie sprawdzić postęp polecenie może spowodować utracie danych w określonym partycją.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Poniżej pokazano jak za pomocą PartitionSelector wybierz partycją losowe określonej usługi:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Historia i obcinania

Po osiągnięciu stanu końcowych polecenia metadanych pozostanie w uruchomienie błędów i Analysis Services na określoną godzinę, zanim zostanie on usunięty, aby oszczędzać miejsce.  Jeśli "GetProgress" jest wywoływana przy użyciu operationId polecenia po jego usunięciu, zwróci FabricException z kod błędu KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
