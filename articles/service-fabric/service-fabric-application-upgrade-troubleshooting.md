<properties
   pageTitle="Rozwiązywanie problemów z uaktualnienia aplikacji | Microsoft Azure"
   description="W tym artykule opisano niektóre typowe problemy wokół uaktualniania aplikacji usługi tkaninie i jak je rozwiązać."
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

# <a name="troubleshoot-application-upgrades"></a>Rozwiązywanie problemów z uaktualnienia aplikacji

W tym artykule opisano niektóre typowe problemy wokół uaktualniania aplikacji tkaninie usługi Azure i jak je rozwiązać.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Rozwiązywanie problemów z uaktualnienia aplikacji

Po uaktualnieniu nie powiedzie się, dane wyjściowe polecenia **Get-ServiceFabricApplicationUpgrade** zawiera dodatkowe informacje dotyczące debugowania błędu.  Na poniższej liście podano, jak można uzyskać dodatkowe informacje:

1. Określ typ błędu.
2. Identyfikowanie z powodu błędu.
3. Wyodrębnianie jeden lub więcej składników Niepowodzenie w celu dalszego zbadania.

Te informacje są dostępne, gdy tkaninie usługi wykryje błąd niezależnie od tego, czy jest **FailureAction** Aby przywrócić lub zawieszenie uaktualnienia.

### <a name="identify-the-failure-type"></a>Określ typ awarii

W wynikach **Get-ServiceFabricApplicationUpgrade** **FailureTimestampUtc** identyfikuje sygnatury czasowej (w czasie UTC), których wykrył błąd uaktualniania usługi tkaninie oraz wyzwolone **FailureAction** . **FailureReason** identyfikuje jeden z trzech potencjalne przyczyny wysokiego poziomu błędu:

1. UpgradeDomainTimeout - wskazuje, że określonej domeny uaktualnienia trwała zbyt długo, aby ukończyć i **UpgradeDomainTimeout** wygasła.
2. OverallUpgradeTimeout - wskazuje, że ogólny uaktualnienie trwała zbyt długo, aby ukończyć i **UpgradeTimeout** wygasła.
3. Test kondycji - wskazuje, że po uaktualnieniu domeny aktualizacji, pozostała nieprawidłowe zgodnie z zasadami kondycji określonej aplikacji i **HealthCheckRetryTimeout** wygasła.

Te wpisy tylko widoczne w wyniku po uaktualnieniu kończy się niepowodzeniem i zaczyna się wycofywanie. Dalsze informacje są wyświetlane w zależności od typu błędu.

### <a name="investigate-upgrade-timeouts"></a>Badanie uaktualnienia przekroczenia limitu czasu

Uaktualnij przekroczenia limitu czasu, które błędy są najczęściej powodowane przez problemy z dostępnością usługi. Dane wyjściowe dalej w tym temacie są typowe uaktualniania miejsce, w którym replik usługi lub wystąpienia mogły zostać uruchomione w nowej wersji kodu. Pole **UpgradeDomainProgressAtFailure** przechwytuje migawki starań uaktualnienia Oczekiwanie w czasie awarii.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

W tym przykładzie uaktualniania nie powiodło się w domenie uaktualnienia *MYUD1* i zostały zatrzymane dwie partycje (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* i *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Partycje zostały zatrzymane powodu nie można umieścić podstawowy repliki (*WaitForPrimaryPlacement*) w docelowej węzłach *Węzeł1* i *Węzeł4*runtime.

Polecenie **Get-ServiceFabricNode** może służyć do weryfikacji, że oba te węzły znajdują się w domenie uaktualnienia *MYUD1*. *UpgradePhase* mówi *PostUpgradeSafetyCheck*, co oznacza kontroli bezpieczeństwa występuje po wszystkich węzłów na uaktualnienia domeny Ukończono uaktualnianie. Wszystkie te informacje wskazuje potencjalne problem z nową wersją kod aplikacji. Najczęstsze problemy są błędy usługi w polu Otwórz lub promocji ścieżki kod podstawowy.

*UpgradePhase* *PreUpgradeSafetyCheck* oznacza, że wystąpiły problemy przygotowanie uaktualnienia domeny przed została wykonana. Najczęstsze problemy, w tym przypadku są błędy usługi Zamknij lub obniżanie z ścieżek kod podstawowy.

Bieżący **UpgradeState** jest *RollingBackCompleted*, więc uaktualnianie oryginalny musi wykonać z cofaniem **FailureAction**, która automatycznie wycofana uaktualnienie w przypadku awarii. W przypadku uaktualniania oryginalny z ręcznego **FailureAction**, następnie Uaktualnianie chcesz zamiast tego być w stanie wstrzymania, aby zezwolić na żywo debugowania aplikacji.

### <a name="investigate-health-check-failures"></a>Badanie błędy wyboru kondycji

Błędy wyboru kondycji może być uruchomiona przez różne problemy, jakie mogą wystąpić po zakończeniu wszystkich węzłów na uaktualnienia domeny, uaktualnianie i przekazywanie wszystkich kontroli bezpieczeństwa. Dane wyjściowe dalej w tym temacie są typowe uaktualnienia awarii z powodu sprawdzanie kondycji nie powiodło się. Pole **UnhealthyEvaluations** przechwytuje migawki sprawdzanie kondycji, które nie zostały w czasie uaktualniania zgodnie z określonymi [zasadami kondycji](service-fabric-health-introduction.md).

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Najpierw analizuje błędy wyboru kondycji wymaga znajomości modelu kondycji usługi tkaninie. Ale nawet bez takich szczegółowy opis, są wyświetlane dwie usługi są nieprawidłowe: *tkaninie: / DemoApp/Svc3* i *tkaninie: / DemoApp/Svc2*, oraz raporty dotyczące kondycji błędu ("InjectedFault" w tym przypadku). W tym przykładzie dwie z czterech usług są nieprawidłowe, czyli poniżej wartości docelowej domyślne 0% nieprawidłowe (*MaxPercentUnhealthyServices*).

Uaktualnianie zostało zawieszone po przełączeniu, określając **FailureAction** podręcznika podczas uruchamiania uaktualnienia. W tym trybie umożliwia badanie live systemu w stanie awarii przed podjęciem dalszych działań.

### <a name="recover-from-a-suspended-upgrade"></a>Odzyskiwanie z zawieszone uaktualnienia

Wycofywanie **FailureAction**jest odzyskać potrzebna, ponieważ uaktualnienie automatycznie z rozwiniętymi informacjami ponownie po przełączeniu. Ręczne **FailureAction**istnieje kilka opcji odzyskiwania:

1. Ręczne wyzwalacza wycofywania
2. Postępuj zgodnie z instrukcjami do końca uaktualnienie ręcznie
3. Wznawianie monitorowane uaktualnienia

Polecenie **Rozpocznij ServiceFabricApplicationRollback** można w dowolnej chwili uruchomić wycofywanie aplikacji. Po polecenie zwraca pomyślnie, żądanie wycofywania została zarejestrowana w systemie i rozpoczęcie wkrótce później.

Polecenie **ServiceFabricApplicationUpgrade Życiorys** może służyć do postępuj zgodnie z instrukcjami do końca uaktualnienie ręcznie, jedną domenę uaktualnienia naraz. W tym trybie tylko bezpieczeństwa są sprawdzane przez system. Już kondycji są sprawdzane. To polecenie można używać tylko w przypadku, gdy *RollingForwardPending*, co oznacza bieżącą domenę uaktualnienia zakończenie uaktualniania przedstawia *UpgradeState* , ale następnego nie rozpoczęło (zaległe).

Polecenie **Aktualizuj ServiceFabricApplicationUpgrade** może służyć do Życiorys monitorowane uaktualnienia obu bezpieczeństwa i kontroli zdrowia wykonywana.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Uaktualnianie z uaktualnienia domeny miejsce, w którym ostatnio zostało zawieszone i użyj samo uaktualnienie parametrów i zasad dotyczących kondycji jako przed będzie nadal występował. W razie potrzeby parametry uaktualnienia i zasad dotyczących kondycji pokazano w powyższej wynik można zmienić w tym samym poleceniu podczas uaktualniania zostało przywrócone. W tym przykładzie uaktualnienie zostało przywrócone w trybie monitorowane, przy użyciu parametrów i zasad dotyczących kondycji bez zmian.

## <a name="further-troubleshooting"></a>Dalsze kroki

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Usługa tkaninie nie obserwuje zasady dotyczące określonej kondycji

Możliwa przyczyna 1:

Usługa przekłada się wszystkie wartości procentowych w rzeczywistej liczby jednostek (na przykład repliki, partycje i usługi) dla oceny kondycji i zawsze zaokrągla liczbę w górę do całego podmiotów. Na przykład jeśli istnieje pięć repliki maksymalna _MaxPercentUnhealthyReplicasPerPartition_ jest 21%, następnie tkaninie usługi umożliwia maksymalnie dwie repliki nieprawidłowe (tej is,'Math.Ceiling (5\*0.21)). W związku z tym zasady dotyczące kondycji należy skonfigurować odpowiednio.

Możliwa przyczyna 2:

Zasady dotyczące kondycji są określane w odniesieniu do wartości procentowych sumy usług i wystąpienia nie określonych usług. Na przykład przed uaktualnieniem, jeśli aplikacja ma cztery usługi wystąpienia A, B, C i D, gdzie usługa D są nieprawidłowe, ale efektownych nieco do aplikacji. Chcemy ignorować znanej usługi nieprawidłowe D podczas uaktualniania i ustaw parametr *MaxPercentUnhealthyServices* 25%, przy założeniu, A, B i C, muszą być prawidłowy.

Jednak podczas uaktualniania D może stać się prawidłowy podczas C staje się nieprawidłowe. Uaktualnianie będzie nadal powiodła się, ponieważ tylko 25% usług są nieprawidłowe. Jednak może powodować nieoczekiwane błędy spowodowane C jest nieoczekiwanie nieprawidłowe zamiast D. W tej sytuacji D należy zaprojektowany jako typ innej usługi a, B i C. Ponieważ zasady dotyczące kondycji podano dla typów usług, progi różnych nieprawidłowe wartości procentowej mogą być stosowane do różnych usług. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Nie określono zasadę uaktualnienia aplikacji, ale uaktualnienie nadal nie niektóre limity czasu, które nigdy nie określone

Zasady dotyczące kondycji nie są udostępniane na żądanie uaktualnienia, będą pobierane z *ApplicationManifest.xml* bieżącej wersji aplikacji. Na przykład są używane uaktualniania X aplikacji w wersji 1.0 do wersji 2.0, zasady dotyczące kondycji aplikacji parametrów w wersji 1.0. Jeśli zasady kondycji różne powinien być używany do uaktualnienia, zasady musi być określony jako części połączenia interfejsu API uaktualnienia aplikacji. Zasady zdefiniowanego połączenia interfejsu API mają zastosowanie tylko podczas uaktualniania. Po zakończeniu uaktualniania zasady określone w *ApplicationManifest.xml* są używane.

### <a name="incorrect-time-outs-are-specified"></a>Podano niepoprawne limity czasu

Może mieć zastanawiasz się o co się dzieje, gdy niespójnie limity czasu. Na przykład może być *UpgradeTimeout* , która jest mniejsza niż *UpgradeDomainTimeout*. Odpowiedź brzmi, jest zwracany błąd. Jeśli *UpgradeDomainTimeout* jest mniejsza niż suma *HealthCheckWaitDuration* i *HealthCheckRetryTimeout*lub *UpgradeDomainTimeout* jest mniejsza niż suma *HealthCheckWaitDuration* i *HealthCheckStableDuration*są zwracane błędy.

### <a name="my-upgrades-are-taking-too-long"></a>Moje aktualizacje trwa zbyt długo

Podczas uaktualniania do wykonania zależy od tego, sprawdzanie kondycji i interakcja z danymi określone. Sprawdzanie kondycji i limity czasu są zależy od tego, jak długo trwa kopiowanie, wdrażania i stabilizację aplikacji. Jest zbyt rygorystyczne z limity czasu może oznaczać więcej nie powiodło się uaktualnień, dlatego jest zalecane, umiarkowane zwiększenie rozmiaru począwszy od dłużej limity czasu.

Oto szybki przypomnieć na interakcja limitów czasu w czasie uaktualniania:

Uaktualnienie do uaktualnienia domeny nie można ukończyć szybciej niż *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Błąd uaktualniania nie może być szybciej niż *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Podczas uaktualniania dla uaktualnienia domeny jest ograniczona przez *UpgradeDomainTimeout*.  Jeśli *HealthCheckRetryTimeout* i *HealthCheckStableDuration* są zera i kondycji aplikacji zachowuje przełączania i z powrotem, następnie uaktualnienie po pewnym czasie przekroczenie limitu czasu na *UpgradeDomainTimeout*. *UpgradeDomainTimeout* uruchomiony, licząc w dół, po rozpoczęciu uaktualnienia dla bieżącej domeny uaktualnienia.

## <a name="next-steps"></a>Następne kroki

[Uaktualnianie do aplikacji przy użyciu programu Visual Studio](service-fabric-application-upgrade-tutorial.md) opisano uaktualnienia aplikacji przy użyciu programu Visual Studio.

[Uaktualnianie programu Powershell przy użyciu aplikacji](service-fabric-application-upgrade-tutorial-powershell.md) opisano uaktualnienia aplikacji przy użyciu programu PowerShell.

Kontrolowanie, jak uaktualnienie aplikacji przy użyciu [Parametrów uaktualnienia](service-fabric-application-upgrade-parameters.md).

Wprowadzić do uaktualnienia aplikacji zgodnych, Dowiedz się, jak używać [Szeregowania danych](service-fabric-application-upgrade-data-serialization.md).

Dowiedz się, jak korzystać z zaawansowanych funkcji podczas uaktualniania aplikacji, odwołując się do [Zagadnienia zaawansowane](service-fabric-application-upgrade-advanced.md).

Rozwiązywanie typowych problemów w uaktualnienia aplikacji, odwołując się do kroki [Rozwiązywania problemów uaktualnienia aplikacji](service-fabric-application-upgrade-troubleshooting.md).
 
