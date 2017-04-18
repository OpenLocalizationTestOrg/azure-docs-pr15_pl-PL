<properties
    pageTitle="Role witryny sieci web i pracownik PHP | Microsoft Azure"
    description="Przewodnik dotyczący tworzenia PHP sieci web i pracownik ról w usłudze w chmurze Azure i skonfigurować środowisko uruchomieniowe PHP."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Jak utworzyć role PHP sieci web i pracownika

## <a name="overview"></a>Omówienie

Ten przewodnik będzie pokazano, jak utworzyć PHP sieci web lub pracownika ról w środowisku programowania systemu Windows, wybierz określonej wersji PHP z wersji "wbudowanych" dostępne, zmień konfigurację PHP, Włącz rozszerzenia, a na koniec Wdroż Azure. Również zawiera opis sposobu konfigurowania sieci web lub pracownika roli za pomocą PHP obsługi (z niestandardowej konfiguracji i rozszerzenia) podane.

## <a name="what-are-php-web-and-worker-roles"></a>Co to są role sieci web i pracownik PHP?

Azure oferuje trzy obliczyć modeli uruchamiania aplikacji: Usługa Azure aplikacji, maszyn wirtualnych Azure i usług w chmurze Azure. Wszystkich trzech modeli obsługuje PHP. Usługami w chmurze, obejmujące ról w sieci web i Pracownik zapewnia *platformę usług (PaaS)*. W ramach usługi w chmurze ról w sieci web zawiera dedykowane serwer sieci web Internetowe usługi informacyjne (IIS) do obsługi aplikacji frontonu sieci web. Rola pracownika można uruchamiać asynchroniczne, długim lub bezterminowo następujące zadania niezależnie od interakcji użytkownika lub danych wejściowych.

Aby uzyskać więcej informacji o tych opcjach zobacz [obliczyć hostingu dostępne Azure](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Pobierz Azure SDK dla PHP

[Azure SDK dla PHP] składa się z kilku części. W tym artykule użyje dwóch z nich: Azure programu PowerShell i emulatory Azure. Te dwa składniki można zainstalować za pomocą Instalatora platformy sieci Web firmy Microsoft. Aby uzyskać więcej informacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Tworzenie projektu usług w chmurze

Pierwszym krokiem przy tworzeniu roli sieci web lub pracownika PHP jest utworzenie projektu usługa Azure. Usługa Azure projektu służy jako kontener logiczny dla sieci web i pracownik ról i zawiera pliki projektu [definicji usługi (.csdef)] i [Konfiguracja usługi (.cscfg)] .

Aby utworzyć nowy projekt usługa Azure, Azure programu PowerShell, Uruchom jako administrator, a następnie wykonaj następujące polecenie:

    PS C:\>New-AzureServiceProject myProject

To polecenie spowoduje utworzenie nowego katalogu (`myProject`) do którego można dodać ról w sieci web i pracownika.

## <a name="add-php-web-or-worker-roles"></a>Dodawanie ról PHP sieci web lub pracownika

Aby dodać rolę web PHP do projektu, uruchom następujące polecenie z poziomu projektu głównego katalogu:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Dla roli Pracownik Użyj tego polecenia:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] `roleName` Parametr jest opcjonalny. Jeśli zostanie pominięty, nazwa roli zostaną automatycznie wygenerowane. Pierwszy rola sieci web utworzona będzie `WebRole1`, druga będzie `WebRole2`i tak dalej. Pierwszy utworzony rola Pracownik będzie `WorkerRole1`, druga będzie `WorkerRole2`i tak dalej.

## <a name="specify-the-built-in-php-version"></a>Określanie wbudowanych wersji PHP

Po dodaniu PHP roli sieci web lub pracowników do projektu pliki konfiguracji projektu są modyfikowane tak, aby PHP zostaną zainstalowane w każdym wystąpieniu sieci web lub pracownika aplikacji po wdrożeniu go. Aby wyświetlić wersję PHP zainstalowanym domyślnie, uruchom następujące polecenie:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Dane wyjściowe polecenia powyżej będzie wyglądać podobnie do tego, co przedstawiono poniżej. W tym przykładzie `IsDefault` flaga jest ustawiona na `true` dla PHP 5.3.17, wskazująca, że będzie on zainstalowanej wersji PHP domyślne.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Można ustawić wersji środowisko uruchomieniowe PHP z wersji PHP, które znajdują się. Na przykład, aby ustawić wersję PHP (dla roli o nazwie `roleName`) do 5.4.0, użyj następującego polecenia:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Dostępne wersje PHP może się zmienić w przyszłości.

## <a name="customize-the-built-in-php-runtime"></a>Dostosowywanie wbudowanego runtime PHP

Masz pełną kontrolę nad konfiguracji obsługi PHP jest zainstalowany, gdy wykonaj kroki opisane, łącznie z modyfikacji `php.ini` ustawienia i włączanie rozszerzenia.

Aby dostosować wbudowany runtime PHP, wykonaj następujące czynności:

1. Dodawanie nowego folderu o nazwie `php`, do `bin` systemu ról w sieci web. Dla roli pracownika należy ją dodać do roli katalogu.
2. W `php` folderu, utwórz inny folder o nazwie `ext`. Umieszczaj żadnego `.dll` rozszerzenia plików (np. `php_mongo.dll`), który chcesz włączyć w tym folderze.
3. Dodawanie `php.ini` pliku `php` folder. Włącz wszystkie niestandardowe rozszerzenia i ustaw dowolne dyrektywy PHP w tym pliku. Na przykład, jeśli chcesz wyłączyć `display_errors` na i Włącz `php_mongo.dll` rozszerzenia, zawartość z `php.ini` pliku zostaną wyświetlone następujące:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Wszystkie ustawienia, które wyraźnie nie ustawisz w `php.ini` pliku podane zostanie automatycznie można ustawić wartości domyślne. Jednak należy pamiętać, że możesz dodawać kompletne `php.ini` pliku.

## <a name="use-your-own-php-runtime"></a>Używanie własnych runtime PHP
W niektórych przypadkach zamiast wybierania wbudowanych runtime PHP i konfigurowania go w sposób opisany powyżej można podać własne środowisko uruchomieniowe PHP. Na przykład można użyć w tym samym czasie wykonywania PHP w sieci web lub pracownika roli używanej w środowisku rozwoju. Ułatwia zapewnienie, że aplikacja nie ulegnie zmianie zachowanie w środowisku produkcyjnym.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Konfigurowanie ról w sieci web umożliwia własne środowisko uruchomieniowe PHP

Aby skonfigurować ról w sieci web umożliwia runtime PHP, dostarczone, wykonaj następujące kroki:

1. Tworzenie projektu usługa Azure i dodać rolę web PHP opisane wcześniej w tym temacie.
2. Tworzenie `php` folderu w `bin` folder, w którym znajduje się w katalogu głównym swojej roli sieci web, a następnie dodaj programu runtime PHP (wszystkie pliki binarne, pliki konfiguracji, podfoldery itp.) do `php` folder.
3. (OPCJONALNIE) Jeśli [Sterowniki firmy Microsoft dla PHP dla programu SQL Server]korzysta z programu runtime PHP[sqlsrv drivers], będzie konieczne konfigurowania roli usługi sieci web do zainstalowania [programu SQL Server natywnych klienta 2012] [ sql native client] po zainicjowaniu obsługi administracyjnej. W tym celu należy dodać [Instalatora sqlncli.msi x64] `bin` folder w katalogu głównym swojej roli sieci web. Skrypt uruchamiania opisem w następnym kroku bezgłośną będzie uruchom Instalatora, gdy zainicjowano obsługę administracyjną roli. Użycie programu runtime PHP nie Drivers firmy Microsoft dla PHP dla programu SQL Server, należy usunąć następujący wiersz skryptu pokazane w następnym kroku:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definiowanie zadania uruchamiania, która konfiguruje [Internetowe usługi informacyjne (IIS)] [ iis.net] za pomocą programu runtime PHP obsługiwać żądań `.php` stron. Aby to zrobić, otwórz `setup_web.cmd` pliku (w `bin` plik katalogu Twoja rola sieci web) w edytorze tekstów i zastąpić zawartość tego skryptu:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Dodawanie plików aplikacji do katalogu głównego roli użytkownika sieci web. Są to katalog główny serwer sieci web.

6. Publikowanie aplikacji zgodnie z opisem w poniższej sekcji [Publikowanie aplikacji](#how-to-publish-your-application) .

> [AZURE.NOTE] `download.ps1` Skryptu (w `bin` folderu katalogu głównego ról w sieci web) można usunąć po wykonaniu kroków opisanych powyżej dotyczące korzystania z własnych runtime PHP.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Konfigurowanie za pomocą własnego runtime PHP roli pracownika

Aby skonfigurować rolę pracownika do obsługi PHP podane za pomocą, wykonaj następujące kroki:

1. Tworzenie projektu usługa Azure i dodać rolę Pracownik PHP opisane wcześniej w tym temacie.
2. Tworzenie `php` folder w katalogu głównym roli pracownika, a następnie dodaj programu runtime PHP (wszystkie pliki binarne, pliki konfiguracji, podfoldery itp.) do `php` folder.
3. (OPCJONALNIE) Jeśli programu runtime PHP używa [Sterowniki firmy Microsoft dla PHP dla programu SQL Server][sqlsrv drivers], należy skonfigurować Rola pracownika do zainstalowania [programu SQL Server natywnych klienta 2012] [ sql native client] po zainicjowaniu obsługi administracyjnej. W tym celu należy dodać [Instalatora sqlncli.msi x64] katalog główny roli pracownika. Skrypt uruchamiania opisem w następnym kroku bezgłośną będzie uruchom Instalatora, gdy zainicjowano obsługę administracyjną roli. Użycie programu runtime PHP nie Drivers firmy Microsoft dla PHP dla programu SQL Server, należy usunąć następujący wiersz skryptu pokazane w następnym kroku:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definiowanie zadania uruchamiania, który zapewnia usługi `php.exe` wykonywalny program roli Pracownik ścieżki środowiska zmiennej zainicjowano obsługę administracyjną roli. Aby to zrobić, otwórz `setup_worker.cmd` plik (w katalogu głównym roli Pracownik) w edytorze tekstów i zastąpić zawartość tego skryptu:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Dodawanie plików aplikacji do katalogu głównego Twoja Rola pracownika.

6. Publikowanie aplikacji zgodnie z opisem w poniższej sekcji [Publikowanie aplikacji](#how-to-publish-your-application) .

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uruchamianie aplikacji w emulatory obliczeń i miejsca do magazynowania

Azure emulatory zapewniają środowisku lokalnym, można przetestować aplikację Azure, przed wdrożeniem w chmurze. Istnieją pewne różnice między emulatory i Azure środowiska. Aby lepiej zrozumieć to, zobacz [Używanie emulatora Azure miejsca do magazynowania dla projektowania i testowania](./storage/storage-use-emulator.md).

Należy zauważyć, że muszą mieć zainstalowany lokalnie, aby użyć emulatora obliczeń PHP. Przy użyciu lokalnej instalacji PHP emulatora obliczeń uruchomienie aplikacji.

Aby uruchomić projektu w emulatory, wykonaj następujące polecenie z projektu głównego katalogu:

    PS C:\MyProject> Start-AzureEmulator

Zostaną wyświetlone informacje podobnie do następującej:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Widać, aplikacja działa w emulatorze, otwierając przeglądarkę sieci web i przejdź do lokalnego adresu wyświetlana w danych wyjściowych (`http://127.0.0.1:81` w wyniku przykład powyżej).

Aby zatrzymać emulatory, wykonywanie tego polecenia:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Publikowanie aplikacji

Aby opublikować aplikacji, należy najpierw zaimportować z ustawienia publikowania za pomocą polecenia cmdlet [AzurePublishSettingsFile importu](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Następnie możesz opublikować aplikacji przy użyciu polecenia cmdlet [AzureServiceProject Publikuj](https://msdn.microsoft.com/library/azure/dn495166.aspx) . Aby uzyskać informacje na temat logowania się zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów PHP](/develop/php/).

[Azure SDK dla PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definicji usługi (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Konfiguracja usługi (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[Instalator sqlncli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
