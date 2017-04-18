<properties
   pageTitle="Wywołać Chaos w klastrów tkaninie usługi | Microsoft Azure"
   description="Za pomocą uruchomienie błędów oraz interfejsy API usług Analysis klaster Zarządzanie Chaos w klastrze."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Wywołać sterowane Chaos w klastrów tkaninie usługi
Dużych systemów dystrybucji pozornie założenia mało wiarygodnych infrastruktury chmury. Tkaninie usługi Azure umożliwia programistom pisanie zaufanego usług u góry mało wiarygodnych infrastruktury. Napisać niezawodne funkcje usługi, deweloperzy muszą mieć możliwość powodować błędy przed takie mało wiarygodnych infrastruktury, aby przetestować stabilność swoich usług.

Uruchomienie błędów i usług Analysis klaster (nazywane także usługa analizy błędów) umożliwia deweloperów wywołać akcje błędów w celu przetestowania usług. Jednak docelowej błędów symulowany pomogą Ci tylko pory. Aby dodatkowo zastosować do badania, możesz użyć Chaos.

Chaos symuluje błędów stałego, przeplotem (bezpieczne i nieprawidłowego) w grupie przez dłuższy czas. Po skonfigurowaniu Chaos ze wskaźnikiem i rodzaju błędów, można uruchomić lub zatrzymać za pośrednictwem interfejsów API języka C# lub programu PowerShell, aby wygenerować błędów w klastrze i usługi.

Po uruchomieniu Chaos zapewnia różne zdarzenia, które przechwytywania stanu Uruchom w momencie. Na przykład ExecutingFaultsEvent zawiera wszystkie błędy, wykonanych w tym iteracji. ValidationFailedEvent zawiera szczegóły dotyczące awarii, który został znaleziony podczas sprawdzania poprawności klaster. Można wywołać interfejsu API GetChaosReportAsync w celu uzyskania raportu zostanie uruchomiona Chaos.

## <a name="faults-induced-in-chaos"></a>Błędy wywołane w Chaos
Chaos generuje błędy w całej klastrze tkaninie usługi i kompresuje błędy, które są widoczne w miesiącach lub latach na kilka godzin. Kombinacja przeplotem błędów ze wskaźnikiem błędu wysoka znajduje rogu sprawy, które w przeciwnym razie zostaną pominięte. Tego ćwiczenia Chaos prowadzi do znacznej poprawy jakości kod usługi.

Chaos wywołuje usterek z następujących kategorii:

 - Ponownie uruchom węzeł
 - Uruchom ponownie pakiet wdrożonym kodu
 - Usuwanie replice
 - Uruchom ponownie replice
 - Przenoszenie replice podstawowego (można konfigurować)
 - Przenoszenie replice pomocniczą (można konfigurować)

Chaos działa w wielu iteracji. Iteracji składa się z błędów i sprawdzanie poprawności klaster w podanym okresie. Można skonfigurować czas spędzony klaster stabilizację i sprawdzania poprawności powiodła się. Jeśli błąd znajduje się w klastrze sprawdzanie poprawności, Chaos generuje i będzie nadal występował ValidationFailedEvent z sygnatura czasowa UTC i szczegóły błędu.

Na przykład można rozważyć wystąpienie Chaos, który jest ustawiona do uruchamiania przez godzinę maksymalnie trzy równoczesne błędów. Chaos wywołuje trzy błędów, a następnie sprawdza kondycji klaster. Jego iterację poprzedni krok, aż znajdzie się jawnie zatrzymana za pośrednictwem interfejsu API StopChaosAsync lub godziny przekazuje. Jeśli klaster stanie się nieprawidłowe w dowolnym iteracji (oznacza to, że go nie stabilizacji w skonfigurowanym czasie) Chaos generuje ValidationFailedEvent. To zdarzenie wskazuje, że coś niepowodzenia i może być konieczne dalsze badania.

W swojej obecnej formie Chaos wywołuje tylko bezpieczne błędów. Oznacza to, że w przypadku braku błędów zewnętrznych, utraty kworum lub utratą danych nigdy nie wystąpiła.

## <a name="important-configuration-options"></a>Opcje konfiguracji ważne
 - **TimeToRun**: całkowity czas tego Chaos zostanie uruchomiona, przed zakończeniem pomyślnie. Możesz wyłączyć Chaos przed jego wykonaniu okres TimeToRun za pośrednictwem interfejsu API StopChaos.
 - **MaxClusterStabilizationTimeout**: maksymalnej ilości czasu oczekiwania klaster stają się prawidłowy przed sprawdzeniem na go ponownie. Jest to oczekiwania w celu zmniejszenia obciążenia w klastrze, gdy trwa odzyskiwanie. Testy wykonywane są następujące:
    - W przypadku kondycji klaster OK
    - W przypadku kondycji usługi OK
    - Jeśli w replice docelowej Ustaw rozmiar uzyskuje się dla partycją usługi
    - Czy istnieją nie repliki InBuild
 - **MaxConcurrentFaults**: Maksymalna liczba równoczesne błędy, które są wywołane w każdej iteracji. Wyższa liczba, tym bardziej rygorystyczne Chaos jest. Powoduje bardziej złożone praca awaryjna i kombinacje przejścia. Chaos gwarantuje, że w przypadku braku błędów zewnętrznych jest nie utraty kworum lub utraty danych, niezależnie od tego, jak duża wartość ma tej konfiguracji.
 - **EnableMoveReplicaFaults**: Włącza lub wyłącza błędów, powodujących repliki podstawowym lub pomocniczym, aby przenieść. Te błędy są domyślnie wyłączone.
 - **WaitTimeBetweenIterations**: czas oczekiwania między iteracji, oznacza to, że po round błędów i odpowiadające im sprawdzania poprawności.
 - **WaitTimeBetweenFaults**: czas oczekiwania między dwoma następujących po sobie błędów w iteracji.

## <a name="how-to-run-chaos"></a>Jak uruchomić Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
