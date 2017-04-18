<properties
   pageTitle="Publikowanie w środowiskach testowych i standardowego za pomocą skryptów programu PowerShell systemu Windows | Microsoft Azure"
   description="Dowiedz się, jak opublikować do rozwoju i przetestować środowiskach za pomocą skrypty środowiska Windows PowerShell z programu Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Publikowanie deweloperów i przetestować środowiskach przy użyciu skrypty środowiska Windows PowerShell

Po utworzeniu aplikacji sieci web w programie Visual Studio istnieje możliwość wygenerowania skrypt programu Windows PowerShell, który umożliwia później zautomatyzować publikowanie witryny sieci Web Azure jako aplikacji sieci Web w usłudze Azure aplikacji lub maszyny wirtualnej. Możesz edytować i rozszerzanie skrypt programu Windows PowerShell w Edytorze Visual Studio do własnych potrzeb lub Integracja skrypt z istniejących kompilacji, test i publikowania skryptów.

Za pomocą skryptów, można zapewnić dostosowanych wersji (nazywane także środowiskach deweloperów i przetestuj) witryny tymczasowo. Na przykład skonfiguruj określonej wersji witryny sieci Web na Azure maszyn wirtualnych lub tymczasowy przedział w witrynie sieci Web na uruchamianie pakietu testów, Odtwórz błąd, testowanie poprawki błędów, wersji próbnej proponowane zmiany lub Konfigurowanie środowiska niestandardowy pokaz lub prezentacji. Po utworzeniu skrypt, który publikuje projektu można odtworzyć identyczne środowiskach ponownie uruchamiając skrypt stosownie do potrzeb lub uruchomić skrypt za pomocą własnego tworzenie aplikacji sieci web, aby utworzyć niestandardowe środowisko do testowania.

## <a name="what-you-need"></a>Co jest potrzebne

- Azure SDK 2.3 lub nowszy. Aby uzyskać więcej informacji, zobacz [Pobieranie programu Visual Studio](http://go.microsoft.com/fwlink/?LinkID=624384) .

Nie ma potrzeby SDK Azure, aby wygenerować skrypty do projektów sieci web. Ta funkcja jest dla projektów sieci web, nie web ról w usług w chmurze.

- Azure programu PowerShell 0.7.4 lub nowszym. Aby uzyskać więcej informacji, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md) .

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) lub nowszym.

## <a name="additional-tools"></a>Dodatkowe narzędzia

Dodatkowe narzędzia i zasoby dotyczące pracy z programem PowerShell w programie Visual Studio rozwoju Azure są dostępne. Zobacz [Narzędzia programu PowerShell dla programu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Generowanie skryptów publikowania

Istnieje możliwość wygenerowania skrypty Publikuj do maszyny wirtualnej, która obsługuje witryny sieci Web, podczas tworzenia nowego projektu przez następujące [te instrukcje](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Możesz także [wygenerować publikowanie skryptów dla aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Skrypty generuje Visual Studio

Program Visual Studio generuje poziom rozwiązanie folder o nazwie **PublishScripts** zawiera dwa pliki programu Windows PowerShell, skrypt Publikuj maszyn wirtualnych lub witryny sieci Web i modułu, który zawiera funkcje, których można używać w skryptów. Program Visual Studio generuje również plik w formacie JSON, który określa szczegółowe informacje dotyczące projektu, który instalujesz.

### <a name="windows-powershell-publish-script"></a>Skrypt publikowanie programu Windows PowerShell

Skrypt publikowania zawiera Publikuj określone czynności podczas wdrażania do witryny sieci Web lub maszyn wirtualnych. Program Visual Studio zawiera kolorowania rozwoju środowiska Windows PowerShell. Pomoc dla funkcji jest dostępny i funkcji w obszarze skrypt do własnych potrzeb Zmienianie swobodnego można edytować.

### <a name="windows-powershell-module"></a>Moduł programu Windows PowerShell

Moduł programu Windows PowerShell, generowany przez program Visual Studio zawiera funkcje, które używa skryptu Publikuj. Te są funkcje Azure programu PowerShell i nie są przeznaczone do zmodyfikowania. Aby uzyskać więcej informacji, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>Plik konfiguracyjny JSON

Plik JSON zostanie utworzony w folderze **konfiguracji** i zawiera dane konfiguracji, które określa dokładnie zasoby, które zostaną wdrażanie Azure. Nazwa pliku, generowany przez program Visual Studio jest projektu — nazwa WAWS-dev.json Jeśli podczas tworzenia witryny sieci Web lub projektu nazwa-maszyn wirtualnych dev.json, jeśli utworzono maszyny wirtualnej. Poniżej przedstawiono przykładowy plik konfiguracji JSON, który jest generowany podczas tworzenia witryny sieci Web. Większość wartości jest oczywiste. Nazwa witryny sieci Web jest generowany przez Azure, więc mogą nie odpowiadać nazwy projektu.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Po utworzeniu maszyny wirtualnej pliku konfiguracji JSON wygląda podobny do następującego. Należy zauważyć, że usługa w chmurze jest tworzony jako kontenera dla maszyny wirtualnej. Maszyny wirtualnej zawiera zwykle punkty końcowe dla sieci web dostęp za pomocą protokołu HTTP i HTTPS, a także wdrożyć sieci Web, która umożliwia publikowanie w witrynie sieci Web z komputera lokalnego, pulpitu zdalnego i programu Windows PowerShell punktów końcowych.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Możesz edytować konfigurację JSON, którą chcesz zmienić, co się dzieje podczas uruchamiania skryptów Publikuj. `cloudService` i `virtualMachine` sekcje są wymagane, ale można usunąć `databases` sekcji, jeśli nie jest potrzebny. Właściwości, które są puste w pliku konfiguracji domyślnej generowany przez program Visual Studio są opcjonalne; wymagane są te, które mają wartości w pliku konfiguracji domyślnej.

Jeśli masz witryny sieci Web z wielu środowiskach rozmieszczania (nazywanego gniazda) zamiast w jednym miejscu produkcji platformy Azure, możesz umieścić przedział nazwę witryny sieci Web w pliku konfiguracji JSON. Na przykład jeśli masz witrynę sieci Web który o nazwie **Moja witryna** i przedział dla niego o nazwie **Testowanie** następnie identyfikator URI jest test.cloudapp.net w witrynie Moja witryna, ale mysite(test) jest prawidłową nazwę do użycia w pliku konfiguracji. Można wykonywać tylko tym jeśli witryny sieci Web i gniazda istnieje już w ramach subskrypcji. Jeśli nie istnieją, Utwórz witryny sieci Web, uruchamiając skrypt bez określania przedział, a następnie utworzyć przedział w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885), a następnie uruchom skrypt o nazwie modyfikacji witryny sieci Web. Aby uzyskać więcej informacji na temat wdrażania gniazda dla aplikacji sieci web zobacz [Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Jak uruchomić skrypty publikowania

Jeśli nigdy nie uruchomiono skrypt programu Windows PowerShell, należy najpierw ustawić zasady wykonanie umożliwiające na uruchamianie skryptów. Jest to funkcja zabezpieczeń, aby zapobiec uruchamianiu skrypty środowiska Windows PowerShell, jeśli znajdują się na złośliwe oprogramowanie lub wirusów, które wymagają wykonywanie skryptów.

### <a name="run-the-script"></a>Uruchamianie skryptu

1. Tworzenie pakietu wdrażania sieci Web dla projektu. Pakiet wdrażanie sieci Web jest skompresowany archiwum (pliku zip), zawierających pliki, które chcesz skopiować do witryny sieci Web lub maszyn wirtualnych. Wdrażanie sieci Web pakietów można utworzyć w programie Visual Studio dla dowolnej aplikacji sieci web.

![Utwórz sieć Web wdrożyć pakiet](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Aby uzyskać więcej informacji, zobacz [jak: Tworzenie pakietu wdrażania sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Można także zautomatyzować tworzenie pakietu wdrażania sieci Web zgodnie z opisem w sekcji **Dostosowywanie i rozszerzanie skryptów Publikuj** w dalszej części tego tematu.

1. W **Eksploratorze rozwiązań**Otwórz menu kontekstowe skrypt, a następnie wybierz **Otwórz za pomocą programu PowerShell ISE**.

1. Jeśli po raz pierwszy, na tym komputerze uruchomieniu skrypty środowiska Windows PowerShell, Otwórz okno wiersza polecenia z uprawnieniami administratora i wpisz następujące polecenie:

`Set-ExecutionPolicy RemoteSigned`

1. Zaloguj się do Azure za pomocą następującego polecenia.

`Add-AzureAccount`

Gdy zostanie wyświetlony monit, należy podać nazwę użytkownika i hasło.

Należy zauważyć, że podczas automatyzacji skrypt tej metody udostępniania poświadczeń Azure będzie działać. Zamiast tego należy użyć pliku .publishsettings o podanie poświadczeń. Jeden raz, możesz pobranie pliku z platformy Azure za pomocą polecenia **Get-AzurePublishSettingsFile** , a następnie zaimportować plik przy użyciu **Importu AzurePublishSettingsFile** . Aby uzyskać szczegółowe instrukcje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md).

1. (Opcjonalnie) Jeśli chcesz utworzyć Azure zasobów, takich jak maszyn wirtualnych, bazy danych i witryny sieci Web bez publikowania aplikacji sieci web, użyj polecenia **Publikuj WebApplication.ps1** z **-konfiguracji** argumentu równa pliku konfiguracji JSON. Ten wiersz polecenia używa pliku konfiguracji JSON do określenia, które zasoby, aby utworzyć. Ponieważ korzysta ona ustawienia domyślne dla innych argumenty wiersza polecenia, tworzy zasobów, ale nie publikowanie aplikacji sieci web. — Pełne opcji zawiera więcej informacji na temat co się dzieje.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Polecenie **Publikuj WebApplication.ps1** przedstawione w jednym z poniższych przykładach wywołania skrypt i publikowanie aplikacji sieci web. Jeśli chcesz zastąpić ustawienia domyślne dla wszystkich innych argumenty, takie jak nazwa subskrypcji, publikowanie nazwa pakietu, maszyn wirtualnych poświadczenia lub poświadczeń serwera bazy danych, możesz określić tych parametrów. Używanie **— pełne** opcję, aby uzyskać więcej informacji o postępie procesu publikowania.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Jeśli tworzysz maszyny wirtualnej polecenie wygląda następująco. W tym przykładzie przedstawiono również sposób określania poświadczenia dla wielu baz danych. Dla maszyn wirtualnych tworzonych przez te skrypty certyfikat SSL nie pochodzi z zaufanego głównego urzędu. W związku z tym należy skorzystać z opcji **— AllowUntrusted** .

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Skrypt można utworzyć baz danych, ale nie zostanie utworzona serwerów baz danych. Jeśli chcesz utworzyć serwer bazy danych, możesz użyć funkcji **AzureSqlDatabaseServer nowy** w Azure module.

## <a name="customizing-and-extending-the-publish-scripts"></a>Dostosowywanie i rozszerzanie skryptów publikowania

Możesz dostosować skrypt Publikuj i plik konfiguracyjny JSON. Funkcje modułu programu Windows PowerShell **AzureWebAppPublishModule.psm1** nie są przeznaczone do zmodyfikowania. Jeśli chcesz określić inną bazę danych lub zmienić niektóre z właściwości maszyny wirtualnej, Edytuj plik konfiguracji JSON. Jeśli chcesz rozszerzyć funkcjonalność skryptu, aby zautomatyzować tworzenie i sprawdzanie projektu, można zaimplementować fragmentami funkcja **WebApplication.ps1 Publikuj**.

Aby zautomatyzować tworzenie projektu, Dodaj kod, który wymaga MSBuild do `New-WebDeployPackage` przedstawionych w tym przykładzie kodu. Ścieżka do polecenia MSBuild różni się w zależności od wersji programu Visual Studio zainstalowano. Za pomocą funkcji **Get-MSBuildCmd**, jak pokazano w tym przykładzie uzyskanie poprawną ścieżkę.

### <a name="to-automate-building-your-project"></a>Aby zautomatyzować tworzenie projektu

1. Dodawanie `$ProjectFile` parametr w sekcji globalnej parametrów.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Skopiuj funkcję `Get-MSBuildCmd` do pliku skryptu.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Zamienianie `New-WebDeployPackage` z poniższy kod i zamienianie symboli zastępczych w tworzeniu linii `$msbuildCmd`. Ten kod jest Visual Studio 2015 r. Jeśli używasz programu Visual Studio 2013 zmienić właściwość poniżej **VisualStudioVersion** `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Połączenie `New-WebDeployPackage` funkcji przed wierszem: `$Config = Read-ConfigFile $Configuration` dla aplikacji sieci web lub `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` dla maszyn wirtualnych.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Wywołaj dostosowanego skryptu z wiersza polecenia przy użyciu przekazywanie `$Project` argument, tak jak w przykładzie następującego polecenia.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Aby zautomatyzować, testowanie aplikacji, Dodaj kod, aby `Test-WebApplication`. Pamiętaj o Usuń komentarze linii **Publikuj WebApplication.ps1** miejsce, w którym są nazywane te funkcje. Jeśli nie podasz implementacja, możesz ręcznie utworzyć projektu za pomocą programu Visual Studio, a następnie uruchom skrypt Publikuj publikowanie Azure.

## <a name="publishing-function-summary"></a>Podsumowanie funkcji publikowania

Aby uzyskać pomoc dotyczącą funkcji można używać w wierszu polecenia środowiska Windows PowerShell, użyj polecenia `Get-Help function-name`. Pomoc obejmuje pomoc parametru i przykłady. Ten sam tekst pomocy jest również pliki źródłowe skrypt, **AzureWebAppPublishModule.psm1** i **WebApplication.ps1 Publikuj**. Skrypt i pomocy są zlokalizowane w języku Visual Studio.

**AzureWebAppPublishModule**

|Nazwa funkcji|Opis|
|---|---|
|Dodawanie AzureSQLDatabase|Umożliwia utworzenie nowej bazy danych Azure SQL.|
|Dodawanie AzureSQLDatabases|Tworzy baz danych programu SQL Azure na podstawie wartości w generowany przez program Visual Studio plik konfiguracyjny JSON.|
|Dodawanie AzureVM|Tworzy Azure maszyny wirtualnej i zwraca adres URL wdrożonym maszyn wirtualnych. Funkcja konfiguruje wymagania wstępne, a następnie wywołuje funkcję **AzureVM nowy** (moduł Azure) do utworzenia nowej maszyny wirtualnej.|
|Dodawanie AzureVMEndpoints|Dodaje nowe wprowadzania punkty końcowe maszyn wirtualnych i zwraca maszyny wirtualnej z nowy punkt końcowy.|
|Dodawanie AzureVMStorage|Tworzy nowe konto Azure miejsca do magazynowania w bieżącej subskrypcji. Nazwa konta zaczyna się od "devtest", po której następuje ciąg alfanumeryczny unikatowy. Funkcja zwraca nazwę nowego konta miejsca do magazynowania. Należy określić lokalizację lub grupę koligacji dla nowego konta z miejsca do magazynowania.|
|Dodawanie AzureWebsite|Tworzy witrynę sieci Web przy użyciu określonej nazwy i lokalizacji. Ta funkcja wywołuje funkcję **AzureWebsite nowy** w Azure module. Jeśli subskrypcja nie zawiera już witrynę sieci Web o podanej nazwie, ta funkcja umożliwia utworzenie witryny sieci Web i zwraca obiekt witryny sieci Web. W przeciwnym razie zwraca `$null`.|
|Wykonywanie kopii zapasowych subskrypcji|Umożliwia zapisanie bieżącej subskrypcji Azure w `$Script:originalSubscription` zmiennych w zakresie skrypt. Ta funkcja umożliwia zapisanie bieżącej subskrypcji Azure (uzyskana przy `Get-AzureSubscription -Current`) i jego konta miejsca do magazynowania, a subskrypcji, która zostanie zmieniony przez tego skryptu (przechowywane w zmiennej `$UserSpecifiedSubscription`) i jego konto miejsca do magazynowania w zakresie skrypt. Zapisując wartości, można użyć funkcji, takich jak `Restore-Subscription`, aby przywrócić oryginalny obecnego konta subskrypcji i miejsca do magazynowania bieżący stan zmiana bieżący stan.|
|Znajdowanie AzureVM|Pobiera określoną Azure maszyny wirtualnej.|
|Format DevTestMessageWithTime|Dołącza daty i godziny do wiadomości. Ta funkcja jest przeznaczona dla wiadomości zapisane strumienie błędu i pełne.|
|Get-AzureSQLDatabaseConnectionString|Zbierane parametry połączenia nawiązywania połączenia z bazą danych Azure SQL.|
|Get-AzureVMStorage|Zwraca nazwę pierwsze konto miejsca do magazynowania z wzorca nazwy "devtest*" (jest uwzględniana wielkość liter) w określonej lokalizacji w grupie koligacji. Jeśli "devtest*" magazynowania konta nie są zgodne z lokalizacji lub grupy koligacji, funkcja ignoruje ją. Należy określić lokalizację lub grupę koligacji.|
|Get-MSDeployCmd|Zwraca polecenie, aby uruchomić narzędzie MsDeploy.exe.|
|Nowy AzureVMEnvironment|Umożliwia znalezienie wyrazów lub utworzenie maszyny wirtualnej w subskrypcji, która jest zgodna z wartości w pliku konfiguracji JSON.|
|Publikowanie WebPackage|Przy użyciu MsDeploy.exe i sieci web publikowanie pakietu. Plik zip wdrożenia zasobów do witryny sieci Web. Ta funkcja nie generuje żadnego wyniku. Jeśli połączenie MSDeploy.exe kończy się niepowodzeniem, funkcja zgłasza wyjątek. Aby uzyskać bardziej szczegółowe dane wyjściowe, użyj **— pełne** opcji.|
|Publikowanie WebPackageToVM|Sprawdza wartości parametrów, a następnie wywołuje funkcję **Publikowania WebPackage** .|
|Przeczytaj ConfigFile|Sprawdza plik konfiguracji JSON i zwraca tabelę mieszania wybranych wartości.|
|Przywracanie subskrypcji|Resetuje bieżącej subskrypcji do oryginalnej subskrypcji.|
|Test AzureModule|Zwraca `$true` w przypadku wersji zainstalowany moduł Azure 0.7.4 lub nowszym. Zwraca `$false` Jeśli moduł nie jest zainstalowany lub starszej wersji. Ta funkcja nie ma parametrów.|
|Test AzureModuleVersion|Zwraca `$true` w przypadku wersję modułu Azure 0.7.4 lub nowszym. Zwraca `$false` Jeśli moduł nie jest zainstalowany lub starszej wersji. Ta funkcja nie ma parametrów.|
|Test HttpsUrl|Konwertuje wprowadzania adresu URL do obiektu System.Uri. Zwraca `$True` Jeśli adres URL jest bezwzględne i jego schemat jest https. Zwraca `$false` Jeśli adres URL jest względne, jego schematu nie jest HTTPS lub ciąg wejściowy nie można przekonwertować adresu URL.|
|Test członka|Zwraca `$true` Jeśli właściwość lub metoda jest członkiem obiektu. W przeciwnym razie zwraca `$false`.|
|Wpisywanie ErrorWithTime|Zapisuje komunikat o błędzie z prefiksem bieżącą godzinę. Ta funkcja wywołuje funkcję **Format DevTestMessageWithTime** być dołączana czasu, zanim pisanie wiadomości w strumieniu błędu.|
|Wpisywanie HostWithTime|Zapisuje wiadomość do programu host (**Host zapisu**) prefiksem bieżącą godzinę. Zmienia się efekt zapisywania w programie hosta. Większość programów obsługujących środowiska Windows PowerShell zapisać te wiadomości pojawi się.|
|Wpisywanie VerboseWithTime|Zapisuje pełne wiadomości prefiksem bieżącą godzinę. Ponieważ wywołuje **Pełne zapisu**, zostanie wyświetlony komunikat tylko wtedy, gdy skrypt zostanie uruchomiony przy użyciu parametru **pełne** lub gdy w preferencjach **VerbosePreference** jest ustawiona na **Kontynuuj**.|

**Publikowanie aplikacji sieci Web**

|Nazwa funkcji|Opis|
|---|---|
|Nowy AzureWebApplicationEnvironment|Tworzy Azure zasoby, takie jak maszyn wirtualnych lub witryny sieci Web.|
|Nowy WebDeployPackage|Ta funkcja nie jest zaimplementowana. W tej funkcji tworzenia projektu można dodać polecenia.|
|Publikowanie AzureWebApplication|Publikuje aplikacji sieci web Azure.|
|Publikowanie aplikacji sieci Web|Tworzy i wdrożenie aplikacji sieci Web, maszyn wirtualnych baz danych programu SQL i kont miejsca do magazynowania dla projektu sieci web programu Visual Studio.|
|Test-aplikacji sieci Web|Ta funkcja nie jest zaimplementowana. Polecenia można dodawać w tej funkcji, aby przetestować aplikację.|

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o skryptów programu PowerShell, czytając [skryptów w programie Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) i zobacz innych skryptów programu PowerShell Azure w [Centrum skryptów](https://azure.microsoft.com/documentation/scripts/).
