<properties
   pageTitle="Uaktualnianie aplikacji tkaninie usługi przy użyciu programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano środowisko wdrażania aplikacji usługi tkaninie, zmieniając kod i wdrażania uaktualnienie przy użyciu programu PowerShell."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Uaktualnianie aplikacji usługi tkaninie przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Programu PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Programu Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Najczęściej używane i zalecane uaktualnienia jest monitorowane uaktualnienia stopniowego.  Azure tkaninie usługi monitoruje kondycję stosowania uaktualniany na podstawie zestawu zasad dotyczących kondycji. Gdy update domain (UD) zostanie uaktualniony, usługa tkaninie oblicza kondycji aplikacji i przechodzi do następnego domeny aktualizacji lub kończy się niepowodzeniem uaktualnienia w zależności od zasad dotyczących kondycji.

Uaktualnianie aplikacji monitorowane mogą być wykonywane za pomocą zarządzanych lub macierzystym interfejsy API, programu PowerShell lub umieść. Aby uzyskać instrukcje dotyczące uaktualniania przy użyciu programu Visual Studio zobacz [Uaktualnianie aplikacji przy użyciu programu Visual Studio](service-fabric-application-upgrade-tutorial.md).

Usługa tkaninie monitorowane uaktualnień stopniowych administrator aplikacji umożliwia konfigurowanie zasad oceny kondycji, które usługa tkaninie używa w celu ustalenia, czy aplikacja jest prawidłowy. Ponadto administrator może skonfigurować działania, jakie należy podjąć w przypadku niepowodzenia oceny kondycji (na przykład, wykonując automatycznego przywrócenia.) W tej sekcji opisano monitorowane uaktualnienia dla jednej z próbki SDK, których używa programu PowerShell.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Krok 1: Tworzenie i wdrażanie przykładowe obiekty wizualne


Tworzenie i publikowanie aplikacji w projekcie aplikacji, **VisualObjectsApplication,** prawym przyciskiem myszy i wybierając polecenie **Publikuj** .  Aby uzyskać więcej informacji zobacz [Aplikacja usługi tkaninie uaktualnienie samouczka](service-fabric-application-upgrade-tutorial.md).  Za pomocą programu PowerShell można także wdrażania aplikacji.

> [AZURE.NOTE] Aby jakichkolwiek poleceń tkaninie usługi mogą być używane w programie PowerShell, najpierw musisz połączyć się z klastrem przy użyciu `Connect-ServiceFabricCluster` polecenia cmdlet. Podobnie przyjmuje się, że klaster ma już został skonfigurowany na komputerze lokalnym. Zobacz artykuł o [konfigurowaniu środowiska programowania tkaninie usługi](service-fabric-get-started.md).

Po utworzeniu projektu w programie Visual Studio, można skopiować pakiet aplikacji do ImageStore za pomocą polecenia programu PowerShell **ServiceFabricApplicationPackage kopii** . Następnym krokiem jest zarejestrować tę aplikację do obsługi tkaninie usługi przy użyciu polecenia cmdlet **ServiceFabricApplicationPackage rejestru** . Ostatnim krokiem jest rozpoczęcie wystąpienie aplikacji przy użyciu polecenia cmdlet **New-ServiceFabricApplication** .  Odpowiednikiem przy użyciu polecenia menu **Rozmieszczanie** w programie Visual Studio są następujące trzy kroki.

Teraz możesz użyć [Usługi tkaninie Eksploratora, aby wyświetlić klaster i aplikacji](service-fabric-visualizing-your-cluster.md). Aplikacja ma usługi sieci web, którą można nawigować, aby w programie Internet Explorer, wpisując [http://localhost: 8081-visualobjects](http://localhost:8081/visualobjects) na pasku adresu.  Powinien zostać wyświetlony niektórych obiektów przestawnych wizualne, poruszania się po ekranie.  Ponadto można użyć **Get-ServiceFabricApplication** Aby sprawdzić stan aplikacji.

## <a name="step-2-update-the-visual-objects-sample"></a>Krok 2: Aktualizowanie przykładowe obiekty wizualne

Może się okazać, że przy użyciu wersji, który został wdrożony w kroku 1, obiekty wizualne umieszczane nie obrócić. Załóżmy Uaktualnij tę aplikację do jednego miejsce, w którym obiekty wizualne umieszczane także obrócić.

Wybierz projekt VisualObjects.ActorService w rozwiązaniu VisualObjects i Otwórz plik StatefulVisualObjectActor.cs. W tym pliku, przejdź do metody `MoveObject`, komentarz `this.State.Move()`i Usuń komentarze `this.State.Move(true)`. Ta zmiana obraca obiekty po uaktualnieniu usługi.

Trzeba także zaktualizować plik *ServiceManifest.xml* (w obszarze PackageRoot) **VisualObjects.ActorService**projektu. Zaktualizuj *CodePackage* i usługa wersja 2.0 i odpowiednie wiersze w pliku *ServiceManifest.xml* .
Za pomocą opcji programu Visual Studio *Edytować pliki pojawiają* po kliknięciu prawym przyciskiem myszy na rozwiązanie, aby wprowadzić zmiany pliku manifestu.


Po wprowadzeniu zmian manifest powinna wyglądać następująco (wyróżnione fragmentów wyświetlanie zmian):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Teraz plik *ApplicationManifest.xml* (znajdujący się w obszarze Projekt **VisualObjects** w obszarze rozwiązanie **VisualObjects** ) jest aktualizowana do wersji 2.0 programu **VisualObjects.ActorService** project. Ponadto wersja aplikacji jest aktualizowana do 2.0.0.0 z 1.0.0.0. *ApplicationManifest.xml* powinna wyglądać podobnie do wstawkę kodu następujące czynności:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Teraz można utworzyć projekt wybranie tylko projektu **ActorService** , a następnie prawym przyciskiem myszy i wybierając opcję **tworzenia** w programie Visual Studio. Po wybraniu **wszystkich odbudowanie**, należy zaktualizować wersji dla wszystkich projektów, ponieważ kod może zostać zmieniona. Następny, Przejdźmy pakiet zaktualizowanej aplikacji przez kliknięcie prawym przyciskiem myszy ***VisualObjectsApplication***, wybierając z Menu tkaninie usług i wybierając **pakietu**. Ta akcja tworzy pakietu aplikacji, które mogą być wdrażane.  Aplikacja zaktualizowane jest gotowy do wdrożenia.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Krok 3: Wybór rodzaju zasady dotyczące kondycji i parametry uaktualnienia

Zapoznaj się z [aplikacji parametry uaktualnienia](service-fabric-application-upgrade-parameters.md) i [procesu uaktualniania](service-fabric-application-upgrade.md) uzyskanie dobrze zapoznać się z różnych parametrów uaktualnienia, limity czasu i kryterium kondycji zastosowane. Dla tego instruktażu kryterium oceny kondycji usługi jest ustawiona na wartość domyślna (i zalecane) wartości, które oznacza, że wszystkie usługi i wystąpienia powinna być _prawidłowy_ po uaktualnieniu.  

Jednak Przejdźmy zwiększyć *HealthCheckStableDuration* do 60 sekund (tak, aby te usługi są prawidłowy co najmniej 20 sekund przed uaktualnienie przechodzi do następnej aktualizacji domeny).  Załóżmy również ustawić *UpgradeDomainTimeout* być 1200 sekund i *UpgradeTimeout* do 3000 sekund.

Na koniec Załóżmy także ustawić *UpgradeFailureAction* wycofywania. Ta opcja wymaga tkaninie usługi przywrócić poprzednią wersję aplikacji w przypadku wykrycia problemów podczas uaktualniania. W związku z tym przy uruchamianiu uaktualnienia (w kroku 4) są określane następujących parametrów:

FailureAction = wycofywania

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Krok 4: Przygotowanie wniosku o uaktualnieniu

Teraz aplikacja jest gotowy i gotowy do uaktualnienia. Jeśli możesz otworzyć okno programu PowerShell jako administrator i typ **Get-ServiceFabricApplication**go należy wyświetlona informacja jest typ aplikacji 1.0.0.0 **VisualObjects** jest został wdrożony.  

Pakiet aplikacji znajduje się w obszarze następującą ścieżkę względną miejsce, w którym nieskompresowane SDK tkaninie usługi - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Folder "Pakiet" powinien znajdować się w tym katalogu, w której jest przechowywany pakiet aplikacji. Sprawdzanie sygnatury czasowe, aby upewnić się, że jest najnowszej kompilacji (może być konieczne odpowiednio zmodyfikuj te ścieżki także).

Teraz Skopiuj pakiet aplikacji zaktualizowane do ImageStore tkaninie usługi (gdzie pakietów aplikacji są przechowywane przez usługę tkanina). Parametr *ApplicationPackagePathInImageStore* informuje tkaninie usługi, gdzie można znaleźć pakiet aplikacji. Firma Microsoft wprowadziły zaktualizowaną aplikację "VisualObjects\_w wersji 2" przy użyciu następującego polecenia (może być konieczne ponownie odpowiednio zmodyfikuj ścieżek).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Następnym krokiem jest zarejestrować tę aplikację za pomocą tkaninie usług, które mogą być wykonywane przy użyciu następującego polecenia:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Jeśli poprzednie polecenie nie działa, prawdopodobnie trzeba Odbuduj wszystkich usług. Jak wskazano w kroku 2, może być konieczne zaktualizować swoją wersję WebService.

## <a name="step-5-start-the-application-upgrade"></a>Krok 5: Rozpoczynanie uaktualnienia aplikacji

Teraz możemy wszystko jest gotowe do Rozpoczynanie uaktualnienia aplikacji przy użyciu następującego polecenia:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Nazwa aplikacji jest taki sam opisane w pliku *ApplicationManifest.xml* . Tkaninie usługi używa tej nazwy do identyfikowania, której aplikacji jest wprowadzenie uaktualnione. Jeśli ustawisz limity czasu się zbyt krótki, można napotkać komunikat o błędzie z informacją, ten problem. Można znaleźć w sekcji rozwiązywania problemów lub Zwiększ limity czasu.

Teraz przychody uaktualnienia aplikacji, można monitorować za pomocą usługi tkaninie Eksploratora lub przy użyciu programu PowerShell następujące polecenia: **tkaninie Get-ServiceFabricApplicationUpgrade:-VisualObjects**.

W ciągu kilku minut, stan masz za pomocą powyższej polecenia programu PowerShell, należy Województwo, że wszystkie domeny aktualizacji zostały uaktualnione (wykonana). I powinien znajdować się obiekty wizualne umieszczane w oknie przeglądarki mają rozpoczętej obracanie!

Możesz spróbować uaktualnianie z wersji 2 do wersji 3 lub w wersji 2 do wersji 1 jako wykonywania. Przechodzenie z wersji 2 do wersji 1 jest traktowana jako uaktualnienia. Odtwarzanie z limity czasu i zasad dotyczących kondycji, aby z nich korzystać. Gdy wdrażania Azure klastrem parametry należy odpowiednio ustawione. Warto ustawić limity czasu umiarkowane zwiększenie rozmiaru.


## <a name="next-steps"></a>Następne kroki

[Uaktualnianie aplikacji przy użyciu programu Visual Studio](service-fabric-application-upgrade-tutorial.md) opisano uaktualnienia aplikacji przy użyciu programu Visual Studio.

Kontrolowanie, jak uaktualnienie aplikacji przy użyciu [Parametry uaktualnienia](service-fabric-application-upgrade-parameters.md).

Wprowadzić do uaktualnienia aplikacji zgodnych, Dowiedz się, jak używać [szeregowania danych](service-fabric-application-upgrade-data-serialization.md).

Dowiedz się, jak korzystać z zaawansowanych funkcji podczas uaktualniania aplikacji, odwołując się do [Zagadnienia zaawansowane](service-fabric-application-upgrade-advanced.md).

Rozwiązywanie typowych problemów w uaktualnienia aplikacji, odwołując się do kroki [uaktualnienia aplikacji Rozwiązywanie problemów](service-fabric-application-upgrade-troubleshooting.md).
