<properties
    pageTitle="Ciągły dostarczenia chmury usługi z TFS platformy Azure | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować ciągły dostarczania w przypadku aplikacji chmura Azure. Przykłady kodu dla MSBuild instrukcji wiersza polecenia i skryptów programu PowerShell."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Ciągły dostawy dla usług w chmurze platformy Azure

Z procesu opisanego w tym artykule pokazano, jak skonfigurować ciągły dostarczania w przypadku aplikacji Azure chmury. Ten proces umożliwia automatyczne tworzenie pakietów i wdrażanie pakietu Azure po każdej kod ewidencjonowania. Opisane w tym artykule procesu tworzenia pakietu jest odpowiednikiem polecenia **pakietu** w programie Visual Studio i publikowania czynności są równoważne polecenia **Publikuj** w programie Visual Studio.
Artykuł obejmuje metod, których chcesz używać do tworzenia serwera kompilacji z instrukcji wiersza polecenia MSBuild i skrypty środowiska Windows PowerShell, a także zaprezentowano jak skonfigurować program Visual Studio Team Foundation Server — definicje Budowanie zespołu za pomocą poleceń MSBuild i skryptów programu PowerShell. Ten proces jest dostosowywanych budowania środowiska i środowiskach Azure docelowej.

Można również użyć programu Visual Studio Team Services, wersję TFS obsługiwanej w Azure, aby łatwiej. Aby uzyskać więcej informacji zobacz temat [Ciągły Delivery Azure przez przy użyciu programu Visual Studio Team Services][].

Przed rozpoczęciem należy opublikować aplikacji z programu Visual Studio.
Temu będziesz mieć pewność, że wszystkie zasoby są dostępne i zainicjowana przy próbie zautomatyzowanie procesów publikacji.

## <a name="1-configure-the-build-server"></a>1: Konfigurowanie serwera kompilacji

Aby można było tworzyć pakietu Azure za pomocą MSBuild, należy zainstalować narzędzia i oprogramowanie wymagane na serwerze kompilacji.

Programu Visual Studio nie jest wymagane do zainstalowania na serwerze kompilacji. Jeśli chcesz umożliwia zarządzanie serwera kompilacji programu Team Foundation tworzenie Service, postępuj zgodnie z instrukcjami w dokumentacji programu [Team Foundation tworzenie Service][] .

1.  Na serwerze kompilacji Zainstaluj [.NET Framework 4.5.2][], który oferuje MSBuild.
2.  Zainstaluj najnowszą [Narzędzia do tworzenia Azure dla środowiska .NET](https://azure.microsoft.com/develop/net/).
3.  Zainstaluj [Azure bibliotek dla środowiska .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Skopiuj plik Microsoft.WebApplication.targets z instalacją programu Visual Studio do serwera kompilacji.

    Na komputerze z programem Visual Studio zainstalowany ten plik znajduje się w katalogu C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Należy skopiować go do tego samego katalogu na serwerze kompilacji.
5.  Instalowanie [Narzędzia Azure programu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: tworzenie pakiet za pomocą polecenia MSBuild

W tej sekcji opisano, jak utworzyć polecenie MSBuild, które tworzy pakiet Azure. Uruchom ten krok na serwerze Konstruuj, aby sprawdzić, czy wszystko jest poprawnie skonfigurowany i że polecenie MSBuild wykonuje, co ma być wykonaj. Możesz dodać ten wiersz polecenia do istniejących skryptów Konstruuj na serwerze kompilacji lub można użyć wiersza polecenia w definicji tworzenie TFS opisane w następnej sekcji. Aby uzyskać więcej informacji na temat parametrów wiersza polecenia i MSBuild zobacz [Informacje dotyczące wiersza polecenia MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Jeśli programu Visual Studio jest zainstalowana na serwerze kompilacji, Znajdź i wybierz pozycję **Visual Studio polecenie Monituj** w folderze **Programu Visual Studio Tools** w systemie Windows.

    Jeśli nie zainstalowano programu Visual Studio na serwerze kompilacji, otwórz wiersz polecenia i upewnij się, że MSBuild.exe jest dostępny na ścieżce. Przy użyciu programu .NET Framework w ścieżkę % WINDIR % jest zainstalowany program MSBuild\\Microsoft.NET\\Framework\\*wersji*. Na przykład aby dodać MSBuild.exe do zmiennej środowiska PATH, gdy masz zainstalowany program .NET Framework 4, w wierszu polecenia wpisz następujące polecenie:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  W wierszu polecenia przejdź do folderu zawierającego plik Azure projektu, który chcesz utworzyć.

3.  Prowadzenie MSBuild w / target: publikowanie opcja tak jak w poniższym przykładzie:

        MSBuild /target:Publish

    Tej opcji można stosować skrót /t: publikowanie. Opcja /t:Publish w MSBuild nie należy mylić z polecenia Publikuj dostępne w programie Visual Studio, gdy masz SDK Azure zainstalowany. /T: publikowanie kompilacjach tylko opcja Azure pakietów. Nie wdrożenia pakietów jak polecenia Publikuj w programie Visual Studio.

    Opcjonalnie możesz określić nazwę projektu jako parametr MSBuild. Jeśli nie zostanie określony, jest używany bieżący katalog. Aby uzyskać więcej informacji na temat MSBuild opcje wiersza polecenia zobacz [Informacje dotyczące wiersza polecenia MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Zlokalizuj dane wyjściowe. Domyślnie to polecenie tworzy katalog względem folderu głównego dla projektu, na przykład *ProjectDir*\\pojemnika\\*konfiguracji*\\app.publish\\. Podczas tworzenia projektu Azure wygenerować dwa pliki, w samym pliku pakietu i towarzyszące pliku konfiguracji:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Domyślnie każdy Azure projekt zawiera jeden usług konfiguracji pliku (.cscfg) dla lokalnego kompilacjach (debugowania), a innej kompilacjach chmury (tymczasowym lub produkcji), ale można dodać lub usunąć pliki konfiguracji usługi, stosownie do potrzeb. Jeśli konstruujesz pakietu w programie Visual Studio, zostanie wyświetlony monit plików konfiguracji usługi do uwzględnienia razem z pakietem pakietu.

5.  Określ plik konfiguracyjny usługi. Po utworzeniu pakietu przy użyciu MSBuild domyślnie znajduje się plik konfiguracji lokalnej usługi. Aby dołączyć plik konfiguracji innej usługi, należy ustawić właściwość TargetProfile polecenia MSBuild, tak jak w poniższym przykładzie:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Określ lokalizację danych wyjściowych. Ustawianie ścieżki przy użyciu /p:PublishDir =*katalog* \\ opcji, w tym końcowe separator ukośnik odwrotny, tak jak w poniższym przykładzie:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Po wykonane i testowanych odpowiedniego wiersza polecenia MSBuild do tworzenia projektów i połączyć je w pakiecie Azure, możesz dodać ten wiersz polecenia do skryptów kompilacji. Jeśli serwer kompilacji korzysta z własnych skryptów, ten proces zależy szczegółowe informacje na temat usługi procesu tworzenia niestandardowych. Jeśli korzystasz z TFS jako środowisko budowania, można postępuj zgodnie z instrukcjami w następnym kroku Dodawanie kompilacji Azure pakiet do procesu kompilacji.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: Tworzenie pakietu TFS Budowanie zespołu

Jeśli masz skonfigurować jako kontroler kompilacji i serwera kompilacji Team Foundation Server (TFS) skonfigurować jako komputerze kompilacji TFS, a następnie możesz opcjonalnie można skonfigurować automatyczne tworzenie pakietu Azure. Aby uzyskać informacje dotyczące konfigurowania i używania programu Team Foundation server jako system kompilacji zobacz [Skala się systemu kompilacji][]. W szczególności w poniższej procedurze przyjęto, że skonfigurowano serwera kompilacji, zgodnie z opisem w [rozmieszczanie i konfigurowanie serwera kompilacji][], i utworzeniu zespołu projektu, projekt usługi cloud utworzony w programie project zespołu.

Aby skonfigurować TFS tworzenia pakietów Azure, wykonaj następujące czynności:

1.  W programie Visual Studio na komputerze dewelopera w menu Widok wybierz pozycję **Eksplorator zespołów**, lub wybierz kombinację klawiszy Ctrl +\\, Ctrl + M. W oknie Eksplorator zespołu rozwiń węzeł **tworzy** lub wybierz **tworzy** strony, a następnie wybierz **Nową definicję tworzenie**.

    ![Nowa opcja tworzenia definicji][0]

2.  Wybierz kartę **wyzwalacza** , a następnie określ odpowiednie warunki, gdy chcesz, aby pakiet, który ma być wbudowane. Na przykład określić **Ciągły integracji** do utworzenia pakietu po zmianie ewidencjonowania kontrolki źródła.

3.  Wybierz kartę **Ustawienia źródeł** i upewnij się, folder projektu znajduje się w kolumnie **Źródło formantu Folder** i stanu jest **aktywna**.

4.  Wybierz kartę **Tworzenie domyślne** , a w obszarze kontroler kompilacji, sprawdź nazwę serwera kompilacji.  Ponadto wybierz opcję **Kopiuj tworzenie wyjścia do folderu upuszczania** i określ odpowiedniej lokalizacji.

5.  Wybierz kartę **proces** . Na karcie proces wybierz szablon domyślny, w obszarze **Tworzenie**, wybierz projekt, jeśli nie jest zaznaczona, a rozwijanie sekcji **Zaawansowane** w sekcji **Tworzenie** siatki.

6.  Wybierz **Argumenty MSBuild**, a następnie ustaw odpowiednie argumenty wiersza polecenia MSBuild, zgodnie z opisem w kroku 2 powyżej. Na przykład wprowadź **/t: publikowanie /p:PublishDir =\\\\nazwa_serwera\\pomija\\ ** Aby utworzyć pakiet i skopiować pliki pakietu do lokalizacji \\ \\nazwa_serwera\\pomija\\:

    ![Argumenty MSBuild][2]

    **Uwaga:** Kopiowanie plików do udziału ułatwia ręcznie wdrożenia pakietów ze swojego komputera rozwoju.

5.  Testowanie sukcesu Twojej kroku kompilacji, zaznaczając pole wyboru zmianę w projekcie, lub twórz nowe kompilacji. Aby ustawić nową kompilację w Eksploratorze zespołu kolejkę kliknij prawym przyciskiem myszy **Wszystkie definicje tworzenie** , a następnie wybierz pozycję **Tworzenie nowej kolejki**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: publikowanie pakiet za pomocą skryptu programu PowerShell

W tej sekcji opisano, jak utworzyć skrypt programu Windows PowerShell, który będą publikować dane wyjściowe pakietu aplikacji chmury Azure za pomocą parametrów. Ten skrypt można wywołać po kompilacja krok w swojej automatyzacji Tworzenie niestandardowego. Może ona również wywołana z szablonu procesu działania przepływu pracy w Visual Studio TFS zespołu umożliwia tworzenie.

1.  Instalowanie [Azure poleceń cmdlet][] (v0.6.1 lub nowszy).
    Podczas fazy instalacji polecenia cmdlet wybierz pozycję, aby zainstalować jako przystawki. Należy zauważyć, że tej wersji oficjalnego wsparcia zastępuje starszą wersję dostępna za pośrednictwem witrynie CodePlex, chociaż poprzednich wersji zostały numerowane 2.x.x.

2.  Uruchom program PowerShell Azure za pomocą Start menu lub stronę początkową. Po uruchomieniu w ten sposób zostanie załadowana polecenia cmdlet programu PowerShell Azure.

3.  W wierszu polecenia programu PowerShell Sprawdź, czy poleceń cmdlet środowiska PowerShell są załadowane przez wprowadzenie polecenia częściowego `Get-Azure` i naciskając klawisz Tab do instrukcji.

    Po naciśnięciu klawisza Tab wielokrotnie, należy wyświetlić różne polecenia programu PowerShell Azure.

4.  Upewnij się, że możesz połączyć się do subskrypcji usługi Azure, importowanie informacji o subskrypcji z pliku .publishsettings.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Następnie wprowadź polecenie

    `Get-AzureSubscription`

    Spowoduje to wyświetlenie informacji o subskrypcji. Upewnij się, że wszystko jest poprawne.

4.  Zapisywanie szablonu skrypt podany na końcu tego artykułu do folderu skryptów jako c:\\skryptów\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Zapoznaj się z sekcją Parametry skryptu. Dodawanie lub modyfikowanie wartości domyślne. Te wartości zastąpione zawsze przekazanie jawne parametry.

6.  Upewnij się, są tworzone prawidłowych chmury konta usługi i magazynowania w ramach subskrypcji, które będą udostępniane przez skrypt Publikuj. Konto miejsca do magazynowania (magazyn obiektów blob) będzie używana do przekazywania i tymczasowego przechowywania plików wdrażania pakietu i konfiguracji podczas tworzenia wdrożenia.

    -   Aby utworzyć nowy usługi w chmurze, możesz połączeń tego skryptu lub korzystać z [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Nazwa usługi cloud zostanie użyty jako prefiks w pełni kwalifikowaną nazwę domeny, a więc musi być unikatowa.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Aby utworzyć nowe konto miejsca do magazynowania, można ten scenariusz rozmowy lub korzystać z [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Nazwę konta magazynu zostanie użyty jako prefiks w pełni kwalifikowaną nazwę domeny, a więc musi być unikatowa. Możesz spróbować użyć tej samej nazwie jak usługa w chmurze.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Połączenia skrypt bezpośrednio z poziomu programu PowerShell Azure lub szkielety się ten skrypt do kompilacji hosta automatyzację wykonywane po kompilacji pakietu.

    >[AZURE.IMPORTANT] Skrypt będzie zawsze Usuń lub Zamień istniejące wdrożeń domyślnie, jeśli zostaną wykryte żadne. Jest to konieczne umożliwienie ciągły dostarczania z automatyzacji, gdy jest możliwe nie monitowania użytkownika.

    **Przykładowy scenariusz 1:** ciągły wdrożenia tymczasowy środowiska usługi:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    To zazwyczaj zostanie użyte testem Uruchom weryfikacji, a następnie zamień VIP. Zamienianie VIP jest możliwe za pośrednictwem [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885) lub przy użyciu polecenia cmdlet Przenieś wdrożenia.

    **Przykładowy scenariusz 2:** ciągły wdrożenia środowisku produkcyjnym usługi dedykowane test

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Pulpit zdalny:**

    Jeśli pulpit zdalny jest włączony w projekcie Azure będzie konieczne wykonanie dodatkowych kroków jednorazowego, aby upewnić się, że poprawne certyfikatu usługi Cloud jest przekazane do wszystkich usług w chmurze odwołuje tego skryptu.

    Znajdź wartości odcisku palca certyfikatu oczekiwanego przez poszczególnych ról. Wartości odcisku palca są widoczne w sekcji certyfikatów w chmurze pliku config (to znaczy ServiceConfiguration.Cloud.cscfg). Jest również wyświetlane w oknie dialogowym Konfiguracja usług pulpitu zdalnego w programie Visual Studio po wyświetleniu opcji i wyświetlanie wybranego certyfikatu.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Przekaż certyfikaty pulpitu zdalnego krokiem jednorazowej konfiguracji za pomocą skryptu następujące polecenie cmdlet:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Na przykład:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Można wyeksportować plik PFX certyfikat z kluczem prywatnym i przekaż certyfikaty dla każdej usługi cloud docelowej za pomocą [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885). 
    Przeczytaj następujący artykuł, aby dowiedzieć się więcej: [[http://msdn.microsoft.com/library/windowsazure/gg443832.aspx]].

    **Uaktualnianie wdrożenie wdrożenia a Usuń —\> nowe wdrażania**

    Skrypt będzie domyślnie wykonywać wdrożenia usługi uaktualnienie ($enableDeploymentUpgrade = 1) po Brak parametru jest przekazywana lub jawnie przekazywana wartość 1. Dla jednego wystąpienia tego przewaga trwa mniej czasu niż pełne wdrożenie. Dla wystąpienia, które wymagają wysokiej dostępności to również przewaga zamykania niektórych przypadkach uruchomiony, a inne są uaktualniane (analizowanie domeny update) oraz usługi VIP nie zostaną usunięte.

    Wdrożenie uaktualnienia może zostać wyłączona w skrypt ($enableDeploymentUpgrade = 0) lub przekazując *enableDeploymentUpgrade 0* jako parametr, który zmienia zachowanie skrypt, aby najpierw usunąć wszelkie istniejące wdrożenia, a następnie utworzyć nowy wdrożenia.

    >[AZURE.IMPORTANT] Skrypt będzie zawsze Usuń lub Zamień istniejące wdrożeń domyślnie, jeśli zostaną wykryte żadne. Jest to konieczne umożliwienie ciągły dostarczania z automatyzacji, gdy jest możliwe nie monitowania użytkownika operatora.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publikowanie pakiet za pomocą TFS Budowanie zespołu

Ten krok opcjonalny łączy TFS zespołu tworzenia skryptom utworzony w kroku 4, obsługujący publikowania kompilacji pakietu Azure. Dzięki temu modyfikowania szablonu procesu używane przez usługi definicji kompilacji tak, aby była uruchamiana działaniem Publikuj na końcu przepływu pracy. Działanie Publikuj będzie wykonywane polecenia programu PowerShell przekazując w parametrach z kompilacji. Wynik MSBuild celów i publikowanie skrypt będzie przekazywany w potoku do wyjścia standardowa kompilacji.

1.  Edytowanie definicji tworzenie odpowiedzialny za ciągły wdrażanie.

2.  Wybierz kartę **proces** .

3.  Wykonaj [te instrukcje](http://msdn.microsoft.com/library/dd647551.aspx) , aby dodać projekt aktywności szablonu proces kompilacji, pobrać szablon domyślny, dodaj go do projektu i to sprawdzić. Nadaj szablonowi proces kompilacji pod inną nazwą, takich jak AzureBuildProcessTemplate.

3.  Powróć do kartę **proces** i wyświetlić listę szablonów procesów kompilacji dostępne za pomocą **Pokaż szczegóły** . Wybierz przycisk **Nowy...** , a następnie przejdź do projektu po prostu dodać i zaewidencjonowany. Zlokalizuj szablon została właśnie utworzona i wybierz **przycisk OK**.

4.  Otwórz wybrany szablon proces do edycji. Możesz otworzyć bezpośrednio w Projektancie przepływów pracy lub w edytorze XML do pracy z XAML.

5.  Dodaj następujący wykaz nowych argumentów jako oddzielne pozycje na karcie argumenty projektanta przepływów pracy. Wszystkie argumenty mają kierunek = w, a następnie wpisz ciąg =. Te posłuży do parametry przepływu z definicji kompilacji do przepływu pracy, które następnie Uzyskaj używane do połączeń skrypt Publikuj.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Listy argumentów.][3]

    Odpowiednie XAML wygląda następująco:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Dodawanie nowej sekwencji na końcu Uruchom na agenta:

    1.  Zacznij od dodania Wyszukaj plik skryptu prawidłowe działanie instrukcji Jeżeli. Warunek do tej wartości:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  W polu następnie przypadku instrukcji Jeśli Dodaj nowe działanie sekwencji. Ustawienia nazwy wyświetlanej "Publikowanie rozpoczęcia"

    3.  Z początkiem Opublikuj sekwencji nadal zaznaczony, Dodaj następujący wykaz nowe zmienne jako oddzielne pozycje na karcie Zmienne projektanta przepływów pracy. Wszystkie zmienne powinny mieć typ zmiennej = ciąg, a zakres = rozpoczęcie publikowania. Te posłuży do parametry przepływu z definicji kompilacji do przepływu pracy, które następnie Uzyskaj używane do połączeń skrypt Publikuj.

        -   SubscriptionDataFilePath typu ciąg

        -   PublishScriptFilePath typu ciąg

            ![Nowe zmienne][4]

    4.  Jeśli korzystasz z TFS 2012 lub z wcześniejszymi wersjami, Dodaj działanie ConvertWorkspaceItem na początku nowej sekwencji. Jeśli korzystasz z TFS 2013 lub nowszym, Dodaj działanie GetLocalPath na początku nowej sekwencji. ConvertWorkspaceItem ustawić właściwości w następujący sposób: kierunek = ServerToLocal, DisplayName = "Konwertuj opublikować nazwę pliku skryptu", wprowadzania = "PublishScriptLocation", wynik = "PublishScriptFilePath", obszar roboczy = "Obszar roboczy". Działania GetLocalPath należy ustawić właściwość IncomingPath do "PublishScriptLocation", a wynik "PublishScriptFilePath". To działanie konwertuje ścieżkę skryptu Publikuj z TFS lokalizacji serwera (jeśli dotyczy) na ścieżkę standardowy dysk lokalny.

    5.  Jeśli używasz TFS 2012 lub z wcześniejszymi wersjami dodać innego działania ConvertWorkspaceItem na końcu nowej sekwencji. Kierunek = ServerToLocal, DisplayName = "Przekonwertować filename subskrypcję", wprowadzania = "SubscriptionDataFileLocation", wynik = "SubscriptionDataFilePath", obszar roboczy = "Obszar roboczy". Jeśli korzystasz z TFS 2013 lub nowszym, dodać innego GetLocalPath. IncomingPath = "SubscriptionDataFileLocation", a wynik = "SubscriptionDataFilePath".

    6.  Dodaj działanie InvokeProcess na końcu nową kolejność elementów.
        To działanie połączeń PowerShell.exe z przekazanych przez tworzenie definicji argumentów.

        1.  Argumenty = String.Format ("-pliku""{0}" "- NazwaUsługi {1} - storageAccountName \{2\} - tabelę packageLocation""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6}-środowisko""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, tabelę PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, środowisko)

        2.  DisplayName = wykonywanie publikowanie skryptu

        3.  FileName = "Programu PowerShell" (zawiera cudzysłowów)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  W sekcji **Obsługi wyjścia standardowego** tekstowym InvokeProcess, ustaw wartość pola tekstowego na "dane". To jest zmienną do przechowywania danych wyjściowych standardowy.

    8.  Dodaj działanie WriteBuildMessage tuż poniżej sekcji **Obsługiwać standardowych danych wyjściowych** . Ustawianie wysoki priorytet = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High", jak i wiadomość = "dane". Dzięki temu standardowe dane wyjściowe skrypt zostaną zapisane w wyniku kompilacji.

    9.  W sekcji **Obsługiwać komunikaty o błędach wyświetlane** tekstowym InvokeProcess, ustaw wartość pola tekstowego na "dane". To jest zmienną do przechowywania danych błędu standardowego.

    10. Dodaj działanie WriteBuildError tuż poniżej sekcji **Obsługiwać komunikaty o błędach wyświetlane** . Ustawianie wiadomości = "dane". Dzięki temu błędy standardowe skryptu zostaną zapisane w wyniku błąd kompilacji.

    11. Poprawiać błędy, wskazywana przez niebieski znaczniki wykrzyknika. Umieść wskaźnik na znaczniki wykrzyknika, aby uzyskać wskazówki dotyczące błędu. Zapisywanie oczywiste błędy w przepływie pracy.

    Ostateczny wynik działania przepływu pracy Publikuj będzie wyglądać następująco w Projektancie:

    ![Działania przepływu pracy][5]

    Ostateczny wynik działania przepływu pracy Publikuj będzie wyglądać następująco w języku XAML:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Zapisz przepływ pracy szablonu procesu konstruowania i zaewidencjonuj ten plik.

8.  Edytowanie definicji Konstruuj (Zamknij, jeśli jest otwarta) i kliknij przycisk **Nowy** , jeśli jeszcze nie widzisz nowego szablonu na liście szablonów procesów.

9.  Ustaw parametr wartości właściwości w sekcji różne w następujący sposób:

    1.  CloudConfigLocation = "c:\\pomija\\app.publish\\ServiceConfiguration.Cloud.cscfg" *tę wartość pochodzi od: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  Tabelę PackageLocation = "c:\\pomija\\app.publish\\ContactManager.Azure.cspkg" *tę wartość pochodzi od: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = "c:\\skryptów\\WindowsAzure\\PublishCloudService.ps1"

    4.  Nazwausługi = "mycloudservicename" *Użyj poniżej nazwy usługi odpowiednie chmury*

    5.  Środowisko = "Tymczasowego"

    6.  StorageAccountName = "mystorageaccountname" *Użyj tutaj nazwę konta magazynu odpowiednie*

    7.  SubscriptionDataFileLocation = "c:\\skryptów\\WindowsAzure\\Subscription.xml"

    8.  SubscriptionName = "domyślne"

    ![Wartości parametru][6]

10. Zapisz zmiany do tworzenia definicji.

11. Kolejka kompilacji do wykonania kompilacji pakietu i publikowanie. Jeśli masz wyzwalacza Ustaw ciągły integracji, będzie wykonywać to zachowanie na każdej ewidencjonowania.

### <a name="publishcloudserviceps1-script-template"></a>Szablon skrypt PublishCloudService.ps1

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Następne kroki

Aby włączyć debugowanie zdalnego podczas korzystania z ciągłym dostarczenia, zobacz [Włączanie zdalnego debugowania podczas korzystania z ciągłym dostarczenia publikowanie Azure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Ciągły dostarczenia Azure za pomocą usług zespołu programu Visual Studio]: cloud-services-continuous-delivery-use-vso.md  
  [Usługa kompilacji Team Foundation]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Możliwość skalowania systemu kompilacji]: https://msdn.microsoft.com/library/dd793166.aspx
  [Wdrażanie i konfigurowanie serwera kompilacji]: https://msdn.microsoft.com/library/ms181712.aspx
  [Polecenia cmdlet programu PowerShell Azure]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
