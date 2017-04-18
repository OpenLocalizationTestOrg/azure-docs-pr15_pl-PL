<properties
    pageTitle="Automatyzowanie tkaninie usługi zarządzania aplikacjami przy użyciu programu PowerShell | Microsoft Azure"
    description="Wdrażanie, uaktualnianie, testowanie i usuwanie aplikacji usług tkaninie przy użyciu programu PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatyzowanie cyklu życia aplikacji przy użyciu programu PowerShell

Można zautomatyzować wiele aspektów [cyklu życia aplikacji usługi tkaninie](service-fabric-application-lifecycle.md) .  W tym artykule pokazano, jak za pomocą programu PowerShell Automatyzowanie typowych zadań dotyczących wdrażania, uaktualniania, usuwanie i testowania aplikacji tkaninie usługi Azure.  Zarządzania oraz interfejsy API HTTP dla aplikacji zarządzania są również dostępne. Zobacz [cyklu życia aplikacji](service-fabric-application-lifecycle.md) , aby uzyskać więcej informacji.  

## <a name="prerequisites"></a>Wymagania wstępne
Aby przejść do zadań w artykule należy:

+ Zapoznanie się z pojęcia tkaninie usług opisany w [techniczne omówienie usługi tkaninie](service-fabric-technical-overview.md).
+ [Zainstaluj środowisko uruchomieniowe, SDK i narzędzia](service-fabric-get-started.md), które jest instalowany również program **ServiceFabric** modułu programu PowerShell.
+ [Wykonywanie skryptu programu PowerShell włączyć](service-fabric-get-started.md#enable-powershell-script-execution).
+ Rozpocznij klaster lokalny.  Uruchamianie nowego okna programu PowerShell jako administrator, a następnie uruchom skrypt konfiguracji klaster z folderu SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Przed uruchomieniem dowolnego polecenia programu PowerShell w tym artykule najpierw nawiązać połączenie lokalne klaster tkaninie usługi przy użyciu [**Połącz ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Następujące zadania wymagają pakiet aplikacji w wersji 1 do wdrożenia i pakiet aplikacji w wersji 2 do uaktualnienia. Pobierz [aplikację przykładową **WordCount** ](http://aka.ms/servicefabricsamples) (znajdujące się w próbce wprowadzenie). Tworzenie i pakiet aplikacji w programie Visual Studio (kliknij prawym przyciskiem myszy na **WordCount** w Eksploratorze rozwiązań i wybierz **pakiet**). Skopiuj pakiet w wersji 1 w `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` do `C:\Temp\WordCount`. Kopiowanie `C:\Temp\WordCount` do `C:\Temp\WordCountV2`, tworzenie pakietu aplikacji w wersji 2 do uaktualnienia. Otwórz `C:\Temp\WordCountV2\ApplicationManifest.xml` w edytorze tekstów. W elemencie **ApplicationManifest** zmieniaj atrybutu **ApplicationTypeVersion** z "1.0.0" do "2.0.0", aby zaktualizować numer wersji aplikacji. Zapisz plik ApplicationManifest.xml zmienione.

## <a name="task-deploy-a-service-fabric-application"></a>Zadanie: Wdrożyć aplikację tkaninie usługi

Po skonstruowaniu i spakowany aplikacji (lub pobrać pakiet aplikacji), należy wdrożyć aplikację w lokalnym klaster tkaninie usług. Rozmieszczanie obejmuje przekazywanie pakietu aplikacji, rejestrowanie typ aplikacji i tworzenie aplikację. Aby wdrożyć nowej aplikacji do klastrów, wykonaj instrukcje w tej sekcji.

### <a name="step-1-upload-the-application-package"></a>Krok 1: Przekazać pakiet aplikacji
Przekazywanie pakietu aplikacji do sklepu obraz umieszcza go w lokalizacji dostępne dla wewnętrznych składników usługi tkaninie.  Pakiet aplikacji zawiera niezbędne konfiguracji manifest aplikacji, usługi manifesty oraz kod i pakiety danych do tworzenia aplikacji i wystąpień usługi.  Polecenie [**Kopiuj ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) przekazywanie pakietu. Na przykład:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Krok 2: Zarejestrować typ aplikacji
Rejestrowanie pakiet aplikacji sprawia, że typ aplikacji i wersja zgłoszone w manifeście aplikacji dostępny do użytku. System odczytuje pakietu przekazane w kroku 1, sprawdź pakietu (odpowiednik uruchomiony [**Test ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) lokalnie), przetwarzanie zawartości pakietu i skopiuj pakiet przetworzone do lokalizacji wewnętrznego systemu.  Uruchom polecenie cmdlet [**ServiceFabricApplicationType rejestru**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCount
```
Aby wyświetlić wszystkie typy aplikacji zarejestrowane w klastrze, uruchom polecenie cmdlet [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Krok 3: Tworzenie wystąpienia aplikacji
Aplikacja można tworzyć za pomocą dowolnej wersji typ aplikacji, który został pomyślnie zarejestrowany przy użyciu polecenia [**Nowy ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . Nazwa każdej aplikacji jest zadeklarowane na wdrażanie czasu i musi się zaczynać **tkaninie:** systemu i być unikatowa dla każdego wystąpienia aplikacji. Nazwa typu aplikacji i wersja typ aplikacji są deklarowane w pliku **ApplicationManifest.xml** pakietu aplikacji. Jeśli wszystkie domyślne usługi zdefiniowane w manifeście aplikacji typu aplikacji docelowej, te są tworzone w tej chwili.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Polecenie [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) Wyświetla listę wszystkich wystąpień aplikacji utworzonych pomyślnie, wraz z ich ogólny stan. Polecenie [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) Wyświetla listę wszystkich wystąpień usług, które zostały pomyślnie utworzone w przypadku wystąpienia danej aplikacji. Znajdują się domyślnych usług (jeśli istnieją).

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Zadanie: Uaktualnianie aplikacji tkaninie usługi
Można też uaktualnić wcześniej wdrożonej aplikacji usługi tkaninie z pakietu aplikacji zaktualizowane. To zadanie uaktualnienia aplikacji WordCount, który został wdrożony w "zadań: Wdrażanie aplikacji usługi tkaninie." Aby uzyskać więcej informacji, przeczytaj za pośrednictwem [aplikacji usługi tkaninie uaktualnienie](service-fabric-application-upgrade.md) .

Aby zachować elementy prostej w tym przykładzie, numer wersji aplikacji została zaktualizowana w WordCountV2 pakiet aplikacji utworzony w wymagania wstępne. Bliższy scenariusz będzie obejmować aktualizowanie plików kodu, konfiguracji lub dane usługi, a następnie odbudowanie i pakowanie aplikacji z numerami zaktualizowanej wersji.  

### <a name="step-1-upload-the-updated-application-package"></a>Krok 1: Przekazać pakiet aplikacji zaktualizowane
WordCount aplikacji w wersji 1 jest gotowy do uaktualnienia. Jeśli możesz otworzyć okno programu PowerShell jako administrator i typ [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)widać, że wdrożenie tej wersji 1.0.0 typu aplikacja WordCount.  

Teraz Skopiuj pakiet zaktualizowanej aplikacji do sklepu obraz tkaninie Service (gdzie pakietów aplikacji są przechowywane przez usługę tkaninie). Parametr **ApplicationPackagePathInImageStore** informuje tkaninie usługi, gdzie można znaleźć pakiet aplikacji. Następujące polecenie kopiuje pakiet aplikacji do **WordCountV2** w magazynie obrazu:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Krok 2: Zarejestrować typ zaktualizowanej aplikacji
Następnym krokiem jest zarejestrować nową wersję aplikacji za pomocą tkaninie usług, które mogą być wykonywane przy użyciu polecenia cmdlet [**ServiceFabricApplicationType rejestru**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Krok 3: Rozpoczynanie uaktualnienia
Różne parametry uaktualnienia, limity czasu i kryteria kondycji można stosować do uaktualnienia aplikacji. Zapoznaj się z dokumentów [Parametry uaktualnienia aplikacji](service-fabric-application-upgrade-parameters.md) i [proces uaktualniania](service-fabric-application-upgrade.md) , aby dowiedzieć się więcej. Wszystkich usług i wystąpienia powinny być _prawidłowy_ po uaktualnieniu.  Ustaw **HealthCheckStableDuration** 60 sekund (tak, aby te usługi są prawidłowy co najmniej 20 sekund przed uaktualnienie przechodzi do następnego uaktualnienia domeny).  Także ustawić **UpgradeDomainTimeout** 1200 sekundy i **UpgradeTimeout** 3000 sekund. Na koniec ustaw **UpgradeFailureAction** **wycofywania**, które żądania, że tkaninie usługi z rozwiniętymi informacjami ponownie aplikacji do poprzedniej wersji, jeśli wystąpią błędy podczas uaktualniania.

Możesz teraz rozpocząć uaktualnienie aplikacji przy użyciu polecenia cmdlet [**Start ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Zauważ, że nazwa aplikacji jest taki sam, jak nazwa aplikacji wcześniej wdrożonym v1.0.0 (tkaninie: / WordCount). Tkaninie usługi używa tej nazwy do identyfikowania, której aplikacji jest wprowadzenie uaktualnione. Jeśli ustawisz limity czasu się zbyt krótki, mogą wystąpić komunikat Błąd przekroczenia limitu czasu problemu. Zapoznaj się [uaktualnień aplikacji Rozwiązywanie problemów z](service-fabric-application-upgrade-troubleshooting.md)lub zwiększanie limitów czasu.

### <a name="step-4-check-upgrade-progress"></a>Krok 4: Postęp uaktualnienia wyboru
Za pomocą [Eksploratora tkaninie usługi](service-fabric-visualizing-your-cluster.md)lub za pomocą polecenia cmdlet [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) można monitorować postęp uaktualnienia aplikacji:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

W ciągu kilku minut, polecenia cmdlet [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) pokazano, że wszystkie uaktualnienia domeny zostały uaktualnione (wykonana).

## <a name="task-test-a-service-fabric-application"></a>Zadanie: Testowanie aplikacji tkaninie usługi

Napisać usług wysokiej jakości, deweloperzy muszą mieć możliwość powodować błędy mało wiarygodnych infrastruktury, aby przetestować stabilność swoich usług. Usługa tkaninie umożliwia deweloperów wywołać akcje błędów i przetestować usług w obecności błędy przy użyciu scenariuszy testowania chaos i pracy awaryjnej.  Przeczytaj [Omówienie pola](service-fabric-testability-overview.md) , aby uzyskać dodatkowe informacje.

### <a name="step-1-run-the-chaos-test-scenario"></a>Krok 1: Uruchamianie Scenariusz testów chaos
Scenariusz testów chaos generuje błędy w całej klastrze tkaninie usług. Scenariusz kompresuje błędów zazwyczaj widoczne w miesiącach lub latach kilka godzin. Kombinacja przeplotem błędów błędów Wysoka szybkość znajduje rogu sprawy, które w przeciwnym razie byłyby odebrane. Poniższy przykład jest uruchamiany Scenariusz testów chaos dla 60 minut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Krok 2: Uruchamianie Scenariusz testów pracy awaryjnej
Tym przełączeniu testowanie elementów docelowych scenariusz partycją określonej usługi, a nie cały klaster i pozostawia innych usług nie ma wpływu. Scenariusz iterację sekwencji symulowany usterek sprawdzania poprawności usług uruchomiony logiki biznesowej. Błąd podczas sprawdzania poprawności usług wskazuje problem, który wymaga dalszego badania. Testowanie pracy awaryjnej wywołuje tylko jeden błędów w danym momencie, a nie w scenariuszu test chaos może wywołać wielu błędów. Poniższy przykład uruchamia test pracy awaryjnej przez 60 minut przed tkaninie: / WordCount/usługi WordCountService.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Zadanie: Usuwanie aplikacji usług tkaninie
Można usunąć wystąpienie wdrożonej aplikacji, usunąć typ ustanawianie aplikacji z klastrem i usunąć pakiet aplikacji z ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Krok 1: Usuń wystąpienie aplikacji
Gdy wystąpienie aplikacji nie jest już potrzebna, można trwale usunąć go za pomocą polecenia cmdlet [**ServiceFabricApplication Usuń**](https://msdn.microsoft.com/library/azure/mt125914.aspx) . Spowoduje to również automatycznie usunięcie wszystkich usług należących do aplikacji trwałe usunięcie wszystkich stanu usługi. Nie można cofnąć tej operacji i nie można odzyskać stan aplikacji.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Krok 2: Unregister typ aplikacji
Gdy już nie potrzebujesz wersję określonego typu aplikacji, unregister go za pomocą polecenia cmdlet [**Unregister ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) . Wyrejestrowywanie nieużywane typy udostępnia ilości miejsca do magazynowania używane przez pakiet aplikacji w sklepie obrazu. Typ aplikacji może być wyrejestrowany, ile istnieje nie tworzenia wystąpień przed nim lub oczekujące uaktualnienia aplikacji go odwoływanie się do aplikacji.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Krok 3: Usuń pakiet aplikacji
Po typ aplikacji jest zarejestrowany, pakiet aplikacji może być usuwany ze sklepu obrazu przy użyciu polecenia cmdlet [**ServiceFabricApplicationPackage Usuń**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Następne kroki
[Cykl życia aplikacji tkaninie usługi](service-fabric-application-lifecycle.md)

[Wdrażanie aplikacji](service-fabric-deploy-remove-applications.md)

[Uaktualnianie aplikacji](service-fabric-application-upgrade.md)

[Azure tkaninie usługi poleceń cmdlet.](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure poleceń cmdlet pola tkaninie usługi](https://msdn.microsoft.com/library/azure/mt125844.aspx)
