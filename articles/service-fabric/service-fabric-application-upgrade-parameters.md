
<properties
   pageTitle="Uaktualnianie aplikacji: uaktualnianie parametrów | Microsoft Azure"
   description="W tym artykule opisano parametrów związanych z uaktualnianiem aplikacji tkaninie usługę, w tym sprawdzanie kondycji do wykonania i zasady, aby cofnąć automatyczne uaktualnienie."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Parametry uaktualnienia aplikacji

W tym artykule opisano różne parametry stosowane podczas uaktualniania aplikacji tkaninie usługi Azure. Parametry zawierają nazwę i wersję aplikacji. Są one pokręteł, które sterują limity czasu i sprawdzanie kondycji, które są stosowane podczas uaktualniania i określają zasady, które muszą być stosowane w przypadku niepowodzenia uaktualnienia.


<br>

| Parametr | Opis |
| --- | --- |
| ApplicationName | Nazwa aplikacji, która jest uaktualniany. Przykłady: tkaninie:-VisualObjects, tkaninie:-ClusterMonitor  |
| TargetApplicationTypeVersion | Typ wersji aplikacji, które uaktualnienia elementów docelowych. |
| FailureAction | Działań podjętych przez usługę tkaninie podczas uaktualniania nie powiedzie się. Aplikacja może zostać przywrócona do wersji wstępnej aktualizacji (wycofywania) lub uaktualnienie może być zatrzymana na bieżącą domenę uaktualnienia. W ostatnim przypadku trybu uaktualniania także zostanie zmieniony ręcznie. Dozwolone wartości są wycofywania i ręcznie. |
| HealthCheckWaitDurationSec | Czas oczekiwania (w sekundach), po zakończeniu uaktualniania w domenie uaktualnienia przed tkaninie usługi oblicza kondycji aplikacji. Ten czas trwania również można uznać za razem, gdy aplikacja powinna być uruchomiona, zanim można traktować prawidłowy. Przekroczeniu kontroli kondycji procesu uaktualniania przechodzi do następnego uaktualnienia domeny.  Jeśli sprawdzanie kondycji nie powiedzie się, usługa tkaninie upływie dla interwału (UpgradeHealthCheckInterval) ponownie ponawianie próby lekarskie, aż do osiągnięcia HealthCheckRetryTimeout. Domyślne i zalecana wartość to 0 sekund. |
| HealthCheckRetryTimeoutSec | Czas trwania (w sekundach) tkaninie tej usługi w dalszym ciągu wykonywać oceny kondycji przed deklarowanie uaktualnienie jako nie powiodło się. Wartość domyślna to 600 sekund. Ten czas trwania zaczyna się po osiągnięciu HealthCheckWaitDuration. W tym HealthCheckRetryTimeout tkaninie usługi może wykonać wiele sprawdzanie kondycji zdrowia aplikacji. Wartość domyślna wynosi 10 minut i powinny być odpowiednio dostosowane aplikacji. |
| HealthCheckStableDurationSec | Czas trwania (w sekundach) aby sprawdzić, czy aplikacja jest stabilny przed przeniesieniem do następnego uaktualnienia domeny lub zakończeniu uaktualniania. Ten czas oczekiwania jest używany do uniemożliwić wprowadzenie zmian niewykryte zdrowia prawo po wykonaniu kontroli kondycji. Wartość domyślna to 120 sekund, a powinny być odpowiednio dostosowane aplikacji. |
| UpgradeDomainTimeoutSec | Maksymalny czas (w sekundach) uaktualniania domena uaktualnienia. Po osiągnięciu tego limitu czasu uaktualnienia przestaje i jest przeprowadzane zgodnie z ustawieniem dla UpgradeFailureAction. Wartość domyślna to nigdy (nieograniczony) i powinny być odpowiednio dostosowane aplikacji. |
| UpgradeTimeout | Limit czasu (w sekundach) zastosowany do całego uaktualnienia. Po osiągnięciu tego limitu czasu zostanie wywołana stopnie uaktualnienia i UpgradeFailureAction. Wartość domyślna to nigdy (nieograniczony) i powinny być odpowiednio dostosowane aplikacji. |
| UpgradeHealthCheckInterval | Częstotliwość, że kondycja jest zaznaczone. Ten parametr jest określony w sekcji ClusterManager _klaster_ _pojawiają_, a nie określono jako część polecenia cmdlet uaktualniania. Wartością domyślną jest 60 sekund.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Ocenianie kondycji usługi podczas uaktualniania aplikacji

<br>
Kryteria oceny kondycji są opcjonalne. Jeśli nie zostaną określone kryteria oceny kondycji, wraz z uaktualnienie, tkaninie usługa używa zasady dotyczące kondycji aplikacji określonego w ApplicationManifest.xml wystąpień aplikacji.


<br>

| Parametr | Opis |
| --- | --- |
| ConsiderWarningAsError | Wartość domyślna to False. Traktuje ostrzeżenia dotyczące kondycji stosowania jako błędy podczas obliczania kondycji aplikacji podczas uaktualniania. Domyślnie tkaninie usługa nie daje kondycji ostrzeżenia jako błędy (błędy), więc uaktualnianie można kontynuować, nawet jeśli istnieją ostrzeżenia.   |
| MaxPercentUnhealthyDeployedApplications | Domyślne i zalecana wartość jest równa 0. Określ maksymalną liczbę rozmieszczonych aplikacji (zobacz [sekcję kondycja](service-fabric-health-introduction.md)), które mogą być nieprawidłowe, zanim aplikacja jest traktowany jako nieprawidłowe i kończy się niepowodzeniem, uaktualnianie. Ten parametr określa kondycji aplikacji w węźle i pomaga wykrywać problemy podczas uaktualniania. Zazwyczaj repliki aplikacji Uzyskaj równoważenia obciążenia do innego węzła, co umożliwia aplikacji są wyświetlane prawidłowy, umożliwiając uaktualnienie kontynuować. Określając ściśle kondycji MaxPercentUnhealthyDeployedApplications usługi można szybko wykrywa problem z pakietem aplikacji i pomoc warzywa błędów szybkie uaktualnienia. |
| MaxPercentUnhealthyServices | Domyślne i zalecana wartość jest równa 0. Określ maksymalną liczbę usług w przypadku aplikacji, które mogą być nieprawidłowe, zanim aplikacja jest traktowany jako nieprawidłowe i kończy się niepowodzeniem, uaktualnianie. |
| MaxPercentUnhealthyPartitionsPerService | Domyślne i zalecana wartość jest równa 0. Określ maksymalną liczbę partycje usługi, który może być nieprawidłowe, zanim usługa jest traktowany jako nieprawidłowe. |
| MaxPercentUnhealthyReplicasPerPartition | Domyślne i zalecana wartość jest równa 0. Określ maksymalną liczbę repliki w partition, które mogą być nieprawidłowe, zanim partycją jest traktowany jako nieprawidłowe. |
| UpgradeReplicaSetCheckTimeout | **Usługa stateless**— w pojedynczej domenie uaktualnienia, tkaninie usługi próbuje upewnij się, że dostępne są dodatkowe wystąpienia usługi. Jeśli liczba wystąpienie docelowej jest więcej niż jedno, tkaninie usługa oczekuje na więcej niż jedno wystąpienie był dostępny maksymalnie maksymalny limit czasu. Ten limit czasu jest określana za pomocą właściwości UpgradeReplicaSetCheckTimeout. Jeżeli limit czasu wygaśnie, tkaninie usługi przechodzą uaktualnienie, bez względu na liczbę wystąpień usługi. Jeśli liczba wystąpienie docelowej, usługa nie czeka i przechodzi natychmiast po uaktualnieniu. **Akumulującej stan usługi**— w pojedynczej domenie uaktualnienia usługi tkaninie próbuje upewnij się, że zestawie ma kworum. Usługa tkaninie oczekuje kworum był dostępny, aż do maksymalny limit czasu (określona przez właściwość UpgradeReplicaSetCheckTimeout). Jeżeli limit czasu wygaśnie, tkaninie usługi przechodzą uaktualnienie, bez względu na kworum. To ustawienie jest zestaw jak nigdy (bez zatrzymania) wdrażania do przodu i 900 sekund podczas wycofywanie. |
| ForceRestart | Po zaktualizowaniu konfiguracji lub pakiet danych bez aktualizowania kod usługi ponownym uruchomieniu usługi tylko wtedy, gdy właściwość ForceRestart jest ustawiona na PRAWDA. Po zakończeniu aktualizacji usługi tkaninie powiadamia usługę konfiguracji pakiet lub pakiet danych jest dostępna. Usługa jest odpowiedzialny za zastosowanie zmian. W razie potrzeby usługę można uruchomić ponownie się. |



<br>
<br>
Kryteria MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService i MaxPercentUnhealthyReplicasPerPartition można określić dla typów usług dla wystąpienie aplikacji. Ustawienie tych parametrów na usługi zezwala do zawierają typy różnych usługach z zasady oceny różnych aplikacji. Na przykład typ usługi stateless bramy może mieć MaxPercentUnhealthyPartitionsPerService, który różni się od typu usługi stanowe aparat w przypadku wystąpienia określonej aplikacji.

## <a name="next-steps"></a>Następne kroki

[Uaktualnianie do aplikacji przy użyciu programu Visual Studio](service-fabric-application-upgrade-tutorial.md) opisano uaktualnienia aplikacji przy użyciu programu Visual Studio.

[Uaktualnianie programu Powershell przy użyciu aplikacji](service-fabric-application-upgrade-tutorial-powershell.md) opisano uaktualnienia aplikacji przy użyciu programu PowerShell.

Wprowadzić do uaktualnienia aplikacji zgodnych, Dowiedz się, jak używać [Szeregowania danych](service-fabric-application-upgrade-data-serialization.md).

Dowiedz się, jak korzystać z zaawansowanych funkcji podczas uaktualniania aplikacji, odwołując się do [Zagadnienia zaawansowane](service-fabric-application-upgrade-advanced.md).

Rozwiązywanie typowych problemów w uaktualnienia aplikacji, odwołując się do kroki [Rozwiązywania problemów uaktualnienia aplikacji](service-fabric-application-upgrade-troubleshooting.md).
 
