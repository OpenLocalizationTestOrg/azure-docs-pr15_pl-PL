<properties
   pageTitle="Rozpoczynanie pracy z rozmieszczanie i uaktualnianie aplikacji w klastrze lokalne | Microsoft Azure"
   description="Konfigurowanie lokalnych klaster tkaninie usługi, wdrażanie istniejącą aplikację do niego i uaktualnienie tej aplikacji."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Wprowadzenie do wdrażania i uaktualnianie aplikacji w klastrze lokalne
SDK tkaninie usługi Azure zawiera środowisku projektowania pełnego lokalne umożliwiają szybkie rozpoczęcie pracy z wdrażanie i zarządzanie aplikacjami w klastrze lokalny. W tym artykule utworzyć klaster lokalny, wdrażanie istniejącą aplikację do niego i uaktualnienie aplikacji do nowej wersji, wszystkie z programu Windows PowerShell.

> [AZURE.NOTE] W tym artykule założono, że możesz już [Konfigurowanie środowiska programowania](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Tworzenie klaster lokalny
Klaster tkaninie usługi reprezentuje zestaw sprzętu zasobów, które można rozmieszczać aplikacje do. Zazwyczaj klaster składa się z dowolnego miejsca od 5 do tysiące komputerów. Usługa SDK tkaninie zawiera jednak konfiguracji klaster, która może zostać uruchomiony na jednym komputerze.

Aby dowiedzieć się, że klaster lokalny tkaninie usługa nie jest emulatora lub simulator ważne jest. Ten sam kod platformy, która znajduje się na wiele stanowiska klastrów działa. Jedyna różnica polega na wykonywania procesów platform, które zwykle rozciągnąć pięciu komputerach na komputerze.

Zestaw SDK zawiera dwa sposoby konfigurowania klaster lokalny: skrypt programu Windows PowerShell i Menedżer klaster lokalnej aplikacji obszar powiadomień systemu. W tym samouczku używamy skrypt programu PowerShell.

> [AZURE.NOTE] Jeśli utworzono już klaster lokalny wdrażając aplikacji z programu Visual Studio, możesz pominąć tę sekcję.


1. Uruchamianie nowego okna programu PowerShell jako administrator.

2. Uruchom skrypt konfiguracji klaster z folderu SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Konfigurowanie klastrów trwa kilka chwil. Po zakończeniu instalacji powinny zostać wyświetlone dane wyjściowe podobne do:

    ![Klaster ustawienia wyjściowe][cluster-setup-success]

    Teraz możesz przystąpić do wypróbowania wdrażanie aplikacji klaster.

## <a name="deploy-an-application"></a>Wdrażanie aplikacji
Usługa SDK tkaninie zawiera bogatego zestawu RAM i deweloper narzędzie do tworzenia aplikacji. Jeśli interesuje Cię Dowiedz się, jak do tworzenia aplikacji w programie Visual Studio, zobacz [Tworzenie pierwszej aplikacji usługi tkaninie w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

W tym samouczku użyjemy istniejącej aplikacji przykładowych (nazywane WordCount) tak, aby możemy skupić się na związanych z zarządzaniem platformy — w tym wdrażania, monitorowania i uaktualnianie.


1. Uruchamianie nowego okna programu PowerShell jako administrator.

2. Importowanie modułu programu PowerShell SDK tkaninie usługi.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Utwórz folder do przechowywania aplikację, którą można pobrać i zainstalować, takich jak C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Pobieranie aplikacji WordCount](http://aka.ms/servicefabric-wordcountapp) do lokalizacji został utworzony.  Uwaga: przeglądarki Microsoft Edge zapisuje plik z rozszerzeniem *zip* .  Zmień rozszerzenie pliku do *.sfpkg*.

5. Połącz się z klastrem lokalne:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Tworzenie nowej aplikacji za pomocą polecenia wdrażania zestawu SDK o nazwie i ścieżkę do pakiet aplikacji.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Jeśli wszystkie odbywa się również, powinny zostać wyświetlone wyniki podobne do następujących:

    ![Wdrażanie aplikacji Klaster lokalny][deploy-app-to-local-cluster]

7. Aby wyświetlić aplikacji w działaniu, uruchamianie przeglądarki i przejdź do [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Powinien zostać wyświetlony:

    ![Interfejs użytkownika aplikacji][deployed-app-ui]

    Aplikacja WordCount jest bardzo proste. Zawiera kod JavaScript po stronie klienta, aby wygenerować losową pięciu znaków "słowa", które następnie są przekazywane do aplikacji przy użyciu interfejsu API sieci Web programu ASP.NET. Usługa stanowe śledzi liczbę wyrazów zliczane. One oddzielone od siebie oparte na pierwszy znak wyrazu. Kod źródłowy można znaleźć aplikacji WordCount w [Wprowadzenie próbki](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    Aplikacja, z której wdrożone zawiera cztery partycje. Aby wyrazy rozpoczynające się od A do G są przechowywane w pierwszą partycją, wyrazy rozpoczynające się od H do N są przechowywane w drugim partycją i tak dalej.

## <a name="view-application-details-and-status"></a>Wyświetlanie szczegółów aplikacji i stanu
Teraz, gdy masz wdrożone aplikacji, Przyjrzyjmy się niektóre szczegóły aplikacji w programie PowerShell.

1. Kwerendy wszystkie wdrożone aplikacji w klastrze:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Przy założeniu, że tylko wdrożono aplikację WordCount, pojawi się Strona podobna do:

    ![Kwerendy wszystkie wdrożone aplikacji w programie PowerShell][ps-getsfapp]

2. Przejdź do następnego poziomu, kwerenda zestaw usług, które znajdują się w aplikacji WordCount.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Lista usług dla aplikacji w programie PowerShell][ps-getsfsvc]

    Aplikacja składa się z dwóch usług — frontonu sieci web i stanowe usługa, która zarządza wyrazy.

3. Na koniec zapoznaj się lista podziałów dla WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Przeglądanie usług w programie PowerShell][ps-getsfpartitions]

    Zestaw poleceń, których użyto, takie jak wszystkie polecenia programu PowerShell tkaninie usługi, są dostępne dla dowolnego klaster służący do lokalną lub zdalną.

    Bardziej wizualną umożliwia interakcję z klastrem można użyć narzędzia Eksploratora tkaninie usługi oparte na sieci web, przechodząc do [Http://localhost:19080-Eksploratora](http://localhost:19080/Explorer) w przeglądarce.

    ![Wyświetlanie szczegółów zgłoszenia w Eksploratorze tkaninie usługi][sfx-service-overview]

    > [AZURE.NOTE] Aby dowiedzieć się więcej na temat usługi tkaninie Eksploratora, zobacz [wizualizacji klaster w Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Uaktualnianie aplikacji
Usługa tkaninie zapewnia uaktualniania nie przestoje przez monitorowanie kondycji aplikacji zgodnie z rozwiniętymi informacjami w klastrze. Załóżmy wykonać proste uaktualnienia aplikacji WordCount.

Nową wersję teraz aplikacja zlicza tylko te wyrazy, które zaczynają się od samogłoski. Podczas uaktualniania z rozwiniętymi informacjami, zobaczymy, dwie zmiany zachowanie aplikacji. Najpierw spowalniać należy stopa co rozwoju statystykę, ponieważ zliczane są mniej wyrazów. Drugi ponieważ pierwszą partycją występują dwa samogłosek (A i E), a wszystkie inne partycje zawiera tylko jednej, jego liczbę po pewnym czasie powinny zacząć do outpace w innych widokach.

1. [Pobierz pakiet w wersji 2 WordCount](http://aka.ms/servicefabric-wordcountappv2) , w tym samym miejscu, w którym został pobrany pakiet w wersji 1.

2. Wróć do okna programu PowerShell i zarejestrować nową wersję w klastrze przy użyciu polecenia uaktualnienia zestawu SDK. Zacznij uaktualniania materiału:-WordCount aplikacji.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Powinien zostać wyświetlony wynik w programie PowerShell, o nazwach rozpoczynających się wygląd podobny do następującego jako uaktualnienie.

    ![Uaktualnianie postępu w programie PowerShell][ps-appupgradeprogress]

3. Podczas uaktualniania jest postępowania, użytkownik może ułatwić monitorowanie stanu w Eksploratorze tkaninie usługi. Okno przeglądarki i przejdź do [Http://localhost:19080-Eksploratora](http://localhost:19080/Explorer). Rozszerzanie **aplikacji** w drzewie po lewej stronie, a następnie wybierz pozycję **WordCount**i w końcu **tkaninie:-WordCount**. Na karcie podstawowe informacje dotyczące stanu uaktualniania będzie widoczna, jak ją przechodzą przez klaster uaktualnienia domen.

    ![Postęp uaktualnienia w Eksploratorze tkaninie usługi][sfx-upgradeprogress]

    W trakcie wykonywania uaktualnienia do każdej domeny kondycji są sprawdzane aby upewnić się, że aplikacja zachowuje się prawidłowo.

4. Jeśli ponowne uruchomienie kwerendy wcześniejszych zestawu usług w tym: / WordCount aplikacji, należy zauważyć, że wersja WordCountService zmieniona, ale wersji WordCountWebService nie:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Kwerenda usługi aplikacji po uaktualnieniu][ps-getsfsvc-postupgrade]

    Wyróżnienie to, jak usługa tkaninie zarządza uaktualnienia aplikacji. Dotknęła tylko zestaw usług (ani pakietów kod i konfiguracji zawartych w tych usług) zostały zmienione, dzięki czemu procesu uaktualniania szybszą i bardziej niezawodne.

5. Na koniec wróć do przeglądarki, aby zaobserwować zachowanie nowej wersji aplikacji. Zgodnie z oczekiwaniami, Zliczanie postępów wolniej, a pierwszą partycją znajdą się nieco więcej wielkości.

    ![Wyświetlanie nowej wersji aplikacji w przeglądarce][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Czyszczenie

Przed Zawijanie, należy pamiętać, że klaster lokalny jest prawdziwa. Aplikacje nadal działać w tle, dopóki nie zostaną usunięte.  W zależności od rodzaju aplikacji uruchomionego aplikacji może zająć znaczną zasobów na tym komputerze. Istnieje kilka sposobów zarządzania aplikacjami i klaster:

1. Aby usunąć pojedyncze aplikacji i wszystkie dane, uruchom następujące polecenie:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Możesz też usunąć aplikację z menu **Akcje** w Eksploratorze tkaninie usługi lub menu kontekstowe w okienku po lewej stronie widoku listy aplikacji.

    ![Usuwanie aplikacji jest Explorer tkaninie usługi][sfe-delete-application]

2. Po usunięciu aplikacji z klastrem, możesz unregister wersji 1.0.0 i 2.0.0 typu aplikacja WordCount. Usuwanie usuwa pakietów aplikacji, takich jak kod i konfiguracji ze sklepu obraz w grupie.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Lub w Eksploratorze tkaninie usług, wybierz **Typ wstrzymywanie obsługi administracyjnej** aplikacji.

3. Aby zamknąć klaster, ale zachować dane aplikacji i śledzenia, kliknij przycisk **Zatrzymaj klaster lokalny** w aplikacji obszar powiadomień systemu.

4. Aby całkowicie usunąć klaster, kliknij pozycję **Usuń klaster lokalny** w aplikacji obszar powiadomień systemu. Ta opcja spowoduje inne wdrożenie działa wolno przy następnym w programie Visual Studio naciśnij klawisz F5. Usuń klaster lokalny tylko wtedy, gdy nie chcesz używać ich do trochę czasu, lub jeśli chcesz odzyskać zasobów.

## <a name="1-node-and-5-node-cluster-mode"></a>Węzeł 1 i 5 trybu klaster węzeł

Podczas pracy z lokalnym klaster do tworzenia aplikacji, często okaże się, że szybkie iteracji pisania kodu, debugowanie, zmiana kodu, wykonując debugowania itp. Aby pomóc zoptymalizować tego procesu, klaster lokalny może zostać uruchomiony w dwóch trybach: węzeł 1 lub 5. Oba tryby klaster mają korzyści.
5 tryb klaster węzeł umożliwia pracę z klastrem rzeczywistą. Można przetestować scenariuszach pracy awaryjnej, Praca z więcej wystąpień i repliki usług.
Tryb klaster węzeł 1 jest zoptymalizowany do szybkiego rozmieszczania i rejestracji usług, ułatwiające szybkiego sprawdzenia poprawności kodu za pomocą środowisko uruchomieniowe usługi tkaninie.

Zarówno tryb klaster węzeł 1 i 5 tryb klaster węzeł nie jest emulatora lub simulator. Ten sam kod platformy, która znajduje się na wiele stanowiska klastrów działa.

> [AZURE.NOTE] Ta funkcja jest dostępna w przypadku SDK wersję 5,2 oraz powyżej.

Aby zmienić tryb klaster klastrze węzeł 1, użyj Menedżera klaster lokalny tkaninie usługi lub za pomocą programu PowerShell następująco:

1. Uruchamianie nowego okna programu PowerShell jako administrator.

2. Uruchom skrypt konfiguracji klaster z folderu SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Konfigurowanie klastrów trwa kilka chwil. Po zakończeniu instalacji powinny zostać wyświetlone dane wyjściowe podobne do:
    
    ![Klaster ustawienia wyjściowe][cluster-setup-success-1-node]

Jeśli korzystasz z Menedżer klaster lokalnej tkaninie usługi:

![Przełącz tryb klaster][switch-cluster-mode]

> [AZURE.WARNING] Po zmianie trybu klaster bieżący klaster jest usuwany z systemu i jest tworzony nowy klaster. Dane, które muszą mieć przechowywane w klastrze, zostaną usunięte, gdy zmienisz tryb klaster.

## <a name="next-steps"></a>Następne kroki
- Teraz, gdy masz wdrożony i uaktualnione niektórych gotowych aplikacji, możesz [wypróbować tworzenie własnych w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Wszystkie akcje wykonywane na klaster lokalny w tym artykule mogą być wykonywane na [klaster Azure](service-fabric-cluster-creation-via-portal.md) także.
- Uaktualnianie, możemy wykonywane w tym artykule została podstawowe. Zobacz [Uaktualnianie dokumentacji](service-fabric-application-upgrade.md) dowiedzieć się więcej o dodatku i elastyczność uaktualniania usługi tkaninie.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
