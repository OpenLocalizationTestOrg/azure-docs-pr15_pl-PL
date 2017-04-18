<properties
   pageTitle="Tworzenie i zarządzanie nimi klastrze tkaninie usługi Azure autonomicznego | Microsoft Azure"
   description="Tworzenie i zarządzanie nimi klaster tkaninie usługi Azure dowolnego komputera (fizycznej lub wirtualnej) z systemem Windows Server, czy jest lokalnego w dowolnym chmury."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Tworzenie i zarządzanie nimi klastrze z systemem Windows Server

Tkaninie usługi Azure umożliwia tworzenie klastrów tkaninie usługi w dowolnym maszyn wirtualnych lub na komputerach z systemem Windows Server. Oznacza to, możesz wdrażanie i uruchamianie aplikacji usługi tkaninie w dowolnym środowisku, które zawiera zestaw wzajemnie połączonymi elementami komputerów Windows Server, w środowisku lokalnym i z dowolnego dostawcy chmury. Tkaninie Usługa udostępnia pakiet instalacyjny Tworzenie klastrów tkaninie usługi o nazwie pakiet Windows Server autonomiczny.

W tym artykule opisano kroki związane z tworzeniem klastrze przy użyciu pakietu autonomicznego tkaninie usługi w środowisku lokalnym, ale można łatwo dostosować we wszystkich środowiskach, takich jak innych dostawców usług w chmurze.

>[AZURE.NOTE] Ten pakiet Windows Server autonomicznego może zawierać funkcje, które są obecnie w podglądzie i nie są obsługiwane do użytku komercyjnego. Aby wyświetlić listę funkcji dostępnych w oknie podglądu, zobacz "funkcje Preview dołączone do tego pakietu". Możesz również [pobrać kopię umowę licencyjną](http://go.microsoft.com/fwlink/?LinkID=733084) .


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Uzyskiwanie pomocy technicznej dla pakietu autonomicznego tkaninie usługi

- Zapytaj społeczność o pakiet autonomiczny tkaninie usługi dla systemu Windows Server na [forum tkaninie usługi Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Otwieranie bilet dla [Profesjonalna pomoc techniczna dla usługi tkaninie](http://support.microsoft.com/oas/default.aspx?prid=16146 ).  Dowiedz się więcej na temat [Profesjonalna pomoc techniczna firmy Microsoft](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Pobierz pakiet autonomiczny tkaninie usługi


[Pobierz pakiet autonomiczny dla usługi tkaninie dla systemu Windows Server 2012 R2 lub nowszy](http://go.microsoft.com/fwlink/?LinkId=730690), o nazwie Microsoft.Azure.ServiceFabric.WindowsServer. &lt;wersji&gt;zip.


Pobierz pakiet zawiera następujące pliki:

|**Nazwa pliku**|**Krótki opis**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Plik cab, który zawiera plików binarnych, które są wdrożone na każdym komputerze w klastrze.|
|ClusterConfig.Unsecure.DevCluster.json|Klaster konfiguracji przykładowy plik zawierający ustawienia niezabezpieczoną, trzy węzła pojedynczego komputera (lub maszyn wirtualnych) rozwoju klaster, włącznie z informacjami dla każdego węzła w klastrze. |
|ClusterConfig.Unsecure.MultiMachine.json|Klaster konfiguracji przykładowy plik zawierający ustawienia klaster niezabezpieczoną, wielu komputera (lub maszyn wirtualnych), włącznie z informacjami dla każdego komputera w grupie.|
|ClusterConfig.Windows.DevCluster.json|Klaster konfiguracji przykładowy plik zawierający wszystkie ustawienia bezpieczne, trzy węzła pojedynczego komputera (lub maszyn wirtualnych) rozwoju klastrze, włącznie z informacjami dla każdego węzła, który znajduje się w grupie. Klaster jest zabezpieczone przy użyciu [tożsamości systemu Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.json|Klaster konfiguracji przykładowy plik zawierający wszystkie ustawienia zabezpieczeń klaster wielu komputera (lub maszyn wirtualnych) przy użyciu zabezpieczeń systemu Windows, w tym informacje o każdej z nich znajduje się w grupie zabezpieczeń. Klaster jest zabezpieczone przy użyciu [tożsamości systemu Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.x509.DevCluster.json|Klaster konfiguracji przykładowy plik zawierający wszystkie ustawienia bezpieczne, trzy węzła pojedynczego komputera (lub maszyn wirtualnych) rozwoju klastrze, włącznie z informacjami dla każdego węzła w klastrze. Klaster jest chronione za pomocą x509 certyfikaty.|
|ClusterConfig.x509.MultiMachine.json|Klaster konfiguracji przykładowy plik zawierający wszystkie ustawienia klaster bezpiecznego, wielu komputera (lub maszyn wirtualnych), włącznie z informacjami dla każdego węzła w grupie zabezpieczeń. Klaster jest chronione za pomocą x509 certyfikaty.|
|EULA_ENU.txt|Postanowienia licencyjne dotyczące stosowania pakietu Microsoft Azure usługi tkaninie autonomicznego systemu Windows Server. Możesz [pobrać kopię umowę licencyjną](http://go.microsoft.com/fwlink/?LinkID=733084) .|
|Readme.txt|Łącze do wersji i podstawowe instrukcje. Jest podzbiór z instrukcjami podanymi w tym dokumencie.|
|CreateServiceFabricCluster.ps1|Skrypt programu PowerShell, który tworzy klaster przy użyciu ustawień w ClusterConfig.json.|
|RemoveServiceFabricCluster.ps1|Skrypt programu PowerShell, który usuwa klaster za pomocą ustawień w ClusterConfig.json.|
|ThirdPartyNotice.rtf |Powiadomienie o oprogramowania innej firmy, która znajduje się w pakiecie.|
|AddNode.ps1|Dodawanie węzła do istniejącego skrypt programu PowerShell wdrożone klaster.|
|RemoveNode.ps1|Skrypt programu PowerShell do usuwania węzła z istniejącego wdrożone klaster.|
|CleanFabric.ps1|Skrypt programu PowerShell do czyszczenia w wersji autonomicznej instalacji usługi tkaninie wyłączanie bieżącego komputera. Poprzednie instalacje MSI powinny zostać usunięte przy użyciu własnej skojarzone uninstallers.|
|TestConfiguration.ps1|Skrypt programu PowerShell do analizy infrastruktury zgodnie z Cluster.json.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Planowanie i przygotowywanie wdrożenia klaster
Przed utworzeniem klaster, należy wykonać następujące czynności.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Krok 1: Planowanie infrastruktury klaster
Zamierzasz utworzyć klaster tkaninie usługi na komputerach, których jesteś właścicielem, więc możesz zdecydować, jakiego rodzaju błędów ma klaster po. Na przykład czy potrzebujesz oddzielnych, power linii lub połączenia z Internetem do tych urządzeń? Ponadto należy rozważyć fizycznego zabezpieczenia tych urządzeń. Gdzie znajdują maszyny i kto ma do nich dostęp? Po wprowadzeniu tych decyzji maszyny można mapować logicznie z różnymi domenami błędów (zobacz krok 4). Planowania dla klastrów produkcji infrastruktury dotyczy więcej niż dla klastrów test.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Krok 2: Przygotowywanie komputerach, aby spełnić wymagania wstępne
Wymagania wstępne dla każdej z nich ma zostać dodany do klastrów:

- Zaleca się co najmniej 16 GB pamięci RAM.
- Zaleca się co najmniej 40 GB wolnego miejsca na dysku.
- Zaleca się 4 core lub większa Procesora.
- Połączenie z siecią bezpiecznego lub sieci dla wszystkich komputerów.
- Windows Server 2012 R2 lub Windows Server 2012 (musisz mieć zainstalowany [KB2858668](https://support.microsoft.com/kb/2858668) ).
- [.NET framework 4.5.1 lub wyższej](https://www.microsoft.com/download/details.aspx?id=40773), pełnej instalacji.
- [Środowiska Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- Powinny być uruchomiona [Usługa RemoteRegistry](https://technet.microsoft.com/library/cc754820) na wszystkich komputerach.

Administrator klastrów, wdrażania i konfigurowania klaster, musisz mieć [uprawnienia administratora](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) na wszystkich komputerach. Nie można zainstalować usługę tkaninie na kontrolerze domeny.

### <a name="step-3-determine-the-initial-cluster-size"></a>Krok 3: Określenia rozmiaru początkowego klaster
Każdy węzeł w klastrze tkaninie usługi autonomicznej ma tkaninie usługi środowisko uruchomieniowe wdrożony i jest członkiem klaster. We wdrożeniu produkcji istnieje jeden węzeł na wystąpienie systemu operacyjnego (fizycznej lub wirtualnej). Rozmiar klastrów zależy od potrzeb firmy. Jednak musisz mieć klaster minimalny rozmiar trzy węzły (maszyn lub maszyn wirtualnych).
Do celów programistycznych możesz mieć więcej niż jeden węzeł na danym komputerze. W środowisku produkcyjnym tkaninie usługi obsługuje tylko jeden węzeł na fizycznej lub maszyn wirtualnych.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Krok 4: Określanie liczby domen błędów i uaktualniania domen
*Domeny błędów* (FD) jest fizycznie jednostka błąd i są bezpośrednio związane z fizycznej infrastruktury w centrach danych. Domena błędów składa się z składniki sprzętowe (komputery, przełączniki, sieci i inne), które współużytkują pojedynczy punkt awarii. Mimo że nie jest mapowania 1:1 między domenami błędów i stojaków, luźno mówiąc, każdego ze stojaków można uznać domeny błędów. Zastanawiając węzłów w klastrze, zalecamy że węzły rozdzielana między co najmniej trzy domeny błędów.

Po określeniu FDs w ClusterConfig.json, możesz wybrać nazwę dla każdego FD. Usługa tkaninie obsługuje hierarchicznych FDs, więc można uwzględnić topologii infrastruktury w nich.  Na przykład następujące FDs są prawidłowe:

- "faultDomain": "fd: / Room1/Rack1/Komputer1"
- "faultDomain": "fd:-FD1"
- "faultDomain": "fd: / Room1/Rack1/PDU1 znajdującego i M1"


*Uaktualnianie domeny* (UD) jest logiczna jednostka węzłów. Podczas uaktualniania usługi tkaninie orchestrated (uaktualnienie aplikacji lub uaktualnienie klaster) wszystkie węzły UD są pobierane w dół do uaktualnienia podczas węzłów na inne UDs pozostają dostępne do obsługi żądania. Aktualizacje oprogramowania należy wykonać na komputery nie obsługują funkcji nakładania UDs, więc musisz je jednego komputera w czasie.

Najłatwiejszym sposobem wziąć pod uwagę następujące pojęcia jest brać pod uwagę FDs jako jednostki niezaplanowane błąd i UDs jednostką informacji o planowanej konserwacji.

Po określeniu UDs w ClusterConfig.json możesz wybrać nazwę dla każdego UD. Na przykład następujące nazwy są prawidłowe:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Niebieski"

Uzyskać bardziej szczegółowe informacje o a domenami uaktualnienia błędów zobacz [opisem klastrze tkaninie usługi](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Krok 5: Pobierz pakiet autonomiczny tkaninie usługi dla systemu Windows Server
[Pobierz pakiet autonomicznego tkaninie usługi dla systemu Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) i rozpakuj plik pakietu do komputera wdrożenia, który nie jest częścią klaster lub do jednego z komputerów, które będą częścią klaster. Może zmienić nazwę folderu rozpakowane `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Tworzenie klaster

Po przejściu planowanie i przygotowanie czynności można przystąpić do tworzenia klaster.

### <a name="step-1-modify-cluster-configuration"></a>Krok 1: Modyfikowanie konfiguracji klaster
Klaster opisano w ClusterConfig.json. Aby uzyskać szczegółowe informacje w sekcjach, w tym pliku zobacz [Ustawienia konfiguracji autonomicznej klaster systemu Windows](service-fabric-cluster-manifest.md).
Otwieranie pliku ClusterConfig.json z pakietu pobranego i modyfikowanie następujące ustawienia:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Ustawienie konfiguracji**|**Opis**|
|-----------------------|--------------------------|
|**NodeTypes**|Typy węzłów umożliwia dzielenie węzły klaster na różnych grup. Klaster musi mieć co najmniej jeden typ węzła. Wszystkie węzły w grupie mają następujące cechy wspólne: <br> **Nazwa** — jest to nazwa typu węzeł. <br>**Porty punktu końcowego** — są to różne nazwanych punkty końcowe (porty) skojarzonych z tego typu węzeł. Można użyć dowolnej numer portu, który chcesz, dopóki nie powodują one konfliktu z pozostałej zawartości w tym manifeście i nie są już używane przez inną aplikację uruchomione na komputerze/m. <br> **Właściwości położenie** — te opisano właściwości dla tego typu węzeł, która służy jako położenie ograniczenia usługi systemowe lub usług. Poniższe właściwości są pary klucz wartość zdefiniowane przez użytkownika, które zapewniają dodatkowe metadane dla danego węzła. Przykłady właściwości węzła będzie, czy węzeł ma dysku twardego lub karty graficznej, liczbę osi w jego dysk twardy, rdzenie i inne właściwości fizycznych. <br> **Możliwości** - węzeł możliwości Definiuj nazwę i ilość określonego zasobu, która ma określonego węzła dostępna do wykorzystania. Na przykład węzeł mogą określić, że ma możliwości metryki o nazwie "MemoryInMb" i czy 2048 MB dostępne jest domyślnie. Następujące możliwości są używane w czasie rzeczywistym, aby upewnić się, że usług, które wymagają określonego ilości zasobów są umieszczane w węzłach zawierające te zasoby, które są dostępne w wymagane wielkości.<br>**IsPrimary** — Jeśli masz więcej niż jeden typ węzła zdefiniowane upewnij się, że tylko jeden jest ustawiona do podstawowego z wartość *PRAWDA*, czyli miejsce, w którym działają usługi systemowe. Wszystkie inne typy węzeł powinna być ustawiona na wartość *FAŁSZ*|
|**Węzły**|Są to dane dla każdej z nich węzłów, które są częścią klaster (typ węzła, nazwa węzła, adres IP, domeny błędów i uaktualniania domeny węzła). Maszyny ma klaster ma zostać utworzony na potrzeby będą tu wymienione adresy IP. <br> Jeśli używasz tego samego adresu IP dla wszystkich węzłów klaster z nich pole tworzona jest, które służy do celów testowych. Nie należy używać jednego pola klastrów do wdrażania obciążenia produkcji.|


### <a name="step-2-run-the-testconfiguration-script"></a>Krok 2: Uruchamianie skryptu TestConfiguration

Skrypt TestConfiguration testów infrastruktury, zgodnie z definicją w cluster.json, aby upewnić się, wymagane uprawnienia są przypisywane, maszyny są połączone ze sobą i inne atrybuty są definiowane tak, aby wdrażanie można powiodła się. Zasadniczo jest mini analizatora najlepszych rozwiązań. Będziemy dodawać więcej reguły sprawdzania poprawności do tego narzędzia w czasie, aby nadać mu bardziej rozbudowany.

Ten skrypt można uruchamiać na każde urządzenie, które ma dostęp do wszystkich komputerów, na których są wyświetlane jako węzły w pliku konfiguracji klaster administrator. Komputer, uruchamianego tego skryptu nie ma być częścią klaster.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Krok 3: Uruchamianie skryptu klaster tworzenia
Po modyfikacji konfiguracji klaster w dokumencie JSON i dodane wszystkie informacje węzeł, uruchom tworzenia klaster skrypt programu PowerShell *CreateServiceFabricCluster.ps1* z folderu pakietu i przenieść ścieżkę do pliku konfiguracji JSON. Po zakończeniu należy zaakceptować umowę licencyjną.

Ten skrypt można uruchamiać na każde urządzenie, które ma dostęp do wszystkich komputerów, na których są wyświetlane jako węzły w pliku konfiguracji klaster administrator. Komputer, uruchamianego tego skryptu nie ma być częścią klaster.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Dzienniki wdrażania są dostępne lokalnie na maszyn wirtualnych komputera, na którym został uruchomiony CreateServiceFabricCluster PowerShell na. Znajdują się w podfolderze o nazwie DeploymentTraces w folderze, w którym zostało uruchomione polecenia programu PowerShell. Aby wyświetlić, jeśli usługa tkaninie wdrożonej poprawnie do komputera, zainstalowanych plików można znaleźć w katalogu C:\ProgramData i procesów FabricHost.exe i Fabric.exe są widoczne uruchomionego w Menedżerze zadań.

### <a name="step-4-connect-to-the-cluster"></a>Krok 4: Połączyć się z klastrem

Aby połączyć do bezpiecznego klastrów, zobacz [Usługa tkaninie nawiązania połączenia z klastrem bezpiecznego](service-fabric-connect-to-secure-cluster.md).

Aby połączyć się z klastrem niezabezpieczoną, uruchom następujące polecenia programu PowerShell:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Krok 5: Wywoływanie usługi tkaninie Eksploratora

Teraz możesz łączyć się klaster w Eksploratorze tkaninie usługi albo bezpośrednio na komputerach z http://localhost:19080/Explorer/index.html lub zdalnie http://<*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Dodawanie i usuwanie węzły

Możesz dodać lub usunąć węzły klaster tkaninie usługi autonomicznej, jak Twoja firma wymaga zmiany. Aby uzyskać szczegółowe instrukcje, zobacz [Dodawanie lub usuwanie węzły do klastrów autonomicznej usługi tkaninie](service-fabric-cluster-windows-server-add-remove-nodes.md) .


## <a name="remove-a-cluster"></a>Usuwanie klastrze

Aby usunąć klaster, uruchomić skrypt programu PowerShell *RemoveServiceFabricCluster.ps1* z folderu pakietu, a następnie przenieść ścieżkę do pliku konfiguracji JSON. Opcjonalnie można określić lokalizację dziennika usunięcie.

Skrypt można uruchamiać na każde urządzenie, które ma dostęp do wszystkich komputerów, na których są wyświetlane jako węzły w pliku konfiguracji klaster administrator. Komputer, uruchamianego tego skryptu nie ma być częścią klaster.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Dane telemetryczne zbierane i jak zaprzestać korzystania z go

Domyślnie produktu zbiera telemetrycznego na zastosowania tkaninie usługi, aby poprawić produktu. Najlepsze praktyki analizatora naśladujący części sprawdza instalacji połączenie [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Jeśli nie jest dostępne, instalacja nie powiedzie się, chyba że zrezygnować z telemetrycznego.

  1. Proces telemetrycznego próbuje przekazać następujące dane do [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) raz każdego dnia. Jest przekazywania optymalny i nie ma wpływu na funkcję klaster. Telemetrycznego są zgodne z węzła, który jest uruchamiany tym przełączeniu Menedżer podstawowego. Nie inne węzły wysyłają telemetrycznego.

  2. Telemetrycznego składa się z następujących czynności:

-            Liczba usług
-            Liczba ServiceTypes
-            Liczba aplikacji
-            Liczba ApplicationUpgrades
-            Liczba FailoverUnits
-            Liczba InBuildFailoverUnits
-            Liczba UnhealthyFailoverUnits
-            Liczba operacji wstawiania danych
-            Liczba InBuildReplicas
-            Liczba StandByReplicas
-            Liczba OfflineReplicas
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Liczba węzłów
-            IsContextComplete: PRAWDA i FAŁSZ
-            ClusterId: Jest to identyfikator GUID losowo generowane dla każdego klaster
-            ServiceFabricVersion
-             Adres IP maszyn wirtualnych lub komputera, z którego przekazaniu telemetrycznego


Aby wyłączyć telemetrycznego, Dodaj następujący *Właściwości* w pliku config klaster: *enableTelemetry: FAŁSZ*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>Funkcje podglądu dołączone do tego pakietu

Brak.

>[AZURE.NOTE] Nowe [wersji GA klaster autonomicznego dla systemu Windows Server (wersja 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), można też uaktualnić klaster do przyszłych wersjach ręcznie lub automatycznie. Ta funkcja nie jest dostępna dla wersji preview, należy utworzyć klaster przy użyciu wersji GA i przeprowadzić migrację danych oraz aplikacji z klaster Podgląd. Zamieścimy, aby uzyskać więcej informacji na temat tej funkcji.


## <a name="next-steps"></a>Następne kroki
- [Ustawienia konfiguracji autonomicznej klaster systemu Windows](service-fabric-cluster-manifest.md)
- [Dodawanie lub usuwanie węzły do klastrów tkaninie usługi autonomicznego](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Tworzenie klastrze tkaninie usługi autonomicznej z maszyny wirtualne Azure z systemem Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu X509 certyfikatów](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
