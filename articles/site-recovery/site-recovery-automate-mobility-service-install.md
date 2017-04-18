<properties
    pageTitle="Powielić maszyn wirtualnych VMware Azure za pomocą Odzyskiwanie witryny z DSC automatyzacji Azure | Microsoft Azure"
    description="Informacje dotyczące używania DSC automatyzacji Azure automatycznie wdrożenia usługi Azure witryny odzyskiwania mobilności i Azure agenta dla maszyn wirtualnych/fizycznej Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Replikacja VMware środowisku maszyn wirtualnych systemu Azure za pomocą Odzyskiwanie witryny z DSC automatyzacji Azure

W pakiecie zarządzania operacji otrzymasz kompleksowe tworzenie kopii zapasowych i rozwiązanie odzyskiwania danych używanego w ramach planu ciągłości firm.

Za pomocą funkcji Hyper-V replice możemy pracę tego kursu razem z funkcji Hyper-V. Ale zostały rozszerzone do obsługi niejednorodnymi konfiguracji, ponieważ klienci mają wieloma monitorami i platformach w ich chmury.

Jeśli korzystasz z obciążeń pracą VMware i/lub serwerów fizycznych dzisiaj, serwer zarządzania uruchamia wszystkie składniki Odzyskiwanie witryny Azure w środowisku obsługi replikacji komunikacji i danych z Azure, w przypadku Azure do miejsca docelowego.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Wdrażanie usługi mobilności odzyskiwania witryny przy użyciu DSC automatyzacji
Zacznijmy, wykonując podział szybkie działanie tego serwera zarządzania.

Serwer zarządzania uruchamia kilka ról serwerów. Jedną z tych ról jest *konfiguracji*, która współrzędnych komunikacji i zarządza procesami replikacji i odzyskiwanie danych.

Ponadto roli *proces* pełni rolę bramy replikacji. Ta rola otrzyma danych replikacji z komputerów zabezpieczonego źródła, optymalizuje go z pamięci podręcznej, kompresji i szyfrowania i przesyła go do konta usługi Azure miejsca do magazynowania. Jedna z funkcji dla roli proces jest również push instalacji usługi mobilności na komputerach chronionego i wykonać automatycznego wykrywania maszyny wirtualne VMware.

W przypadku awarii z platformy Azure rolę *wzorca docelową* będzie obsługiwany danych replikacji w ramach tej operacji.

W przypadku komputerów chronione możemy oparte na *mobilności usługi*. Ten składnik jest używany na każdym komputerze (maszyn wirtualnych VMware lub serwera fizycznego), który chcesz odtworzyć Azure. Jej rejestruje zapisywanie na komputerze i przekazuje je do serwera zarządzania (rola proces).

Już zajmujących się Zapewnianie ciągłości, należy zrozumieć z obciążeń pracą, infrastruktury i elementów związanych. Następnie można spełniają wymagania celem czasu odzyskiwania (RTO) i odzyskiwania punktów cel (RPO). W tym kontekście usługi mobilności jest kluczem do zapewnienia ochrony usługi obciążenia zgodnie z oczekiwaniami.

W jaki sposób można, w sposób zoptymalizowany zapewnić, że zaufanego konfiguracji chronionym przy pomocy niektóre składniki pakietu zarządzania operacje?

Ten artykuł zawiera przykład korzystaniem Azure automatyzacji potrzeby stan konfiguracji (DSC), razem z Odzyskiwanie witryny, aby upewnić się, że:

- Usługa mobilności i agenta maszyn wirtualnych Azure są wdrażane komputerów systemu Windows, które mają być chronione.
- Mobilność maszyn wirtualnych Azure agenta jest uruchomiona usługa i zawsze po Azure docelowej replikacji.

## <a name="prerequisites"></a>Wymagania wstępne

- Repozytorium do przechowywania wymagane instalacji
- Repozytorium do przechowywania wymagane hasło, aby zarejestrować na serwerze zarządzania

 > [AZURE.NOTE] Unikatowe hasło jest generowany dla każdego serwera zarządzania. Jeśli zamierzasz wdrożyć wielu serwerów zarządzania, masz upewnij się, że poprawne hasło jest przechowywana w pliku passphrase.txt.

- Management Framework WMF (Windows) 5.0 zainstalowana na komputerach, które chcesz włączyć ochronę (wymaganie DSC automatyzacji)

 > [AZURE.NOTE] Jeśli chcesz używać komputerów DSC dla systemu Windows, na których jest zainstalowany 4.0 WMF, zobacz sekcję [Za pomocą DSC w środowiskach Rozłączono](#Use DSC in disconnected environments).

Usługa mobilności można zainstalować za pomocą wiersza polecenia i akceptuje kilka argumentów. Dlatego konieczne jest plików binarnych (po wyodrębnianie je z ustawień) i przechowywać je w miejscu, gdzie można je odzyskać przy użyciu konfiguracji DSC.

## <a name="step-1-extract-binaries"></a>Krok 1: Pliki binarne Wyodrębnij

1. Aby wyodrębnić pliki, których potrzebujesz dla tej konfiguracji, przejdź do następującego katalogu na serwerze zarządzania:

    **\Microsoft Recovery\home\svsystems\pushinstallsvc\repository azure witryny**

    W tym folderze powinna być widoczna plik MSI o nazwie:

    **ASR_UA_version_Windows_GA_date_Release.exe firmy Microsoft**

    Użyj następującego polecenia, aby wyodrębnić Instalatora pakietu:

    **/X:C:\Users\Administrator\Desktop\Mobility_Service\Extract/q.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe**

2. Wybierz pozycję wszystkie pliki i wysłać je do folder skompresowany (zip).

Teraz masz plików binarnych, potrzebne do zautomatyzowania instalacji usługi mobilności przy użyciu DSC automatyzacji.

### <a name="passphrase"></a>Hasło

Następnie należy określić miejsce, w którym chcesz umieścić ten folder zip. Za pomocą konta Azure miejsca do magazynowania, jak pokazano później do przechowywania hasło, które potrzebne do konfiguracji. Agent następnie zarejestruj się z serwerem zarządzania w ramach procesu.

Hasło, które masz po wdrożeniu serwera zarządzania mogą być zapisywane w pliku tekstowym jako passphrase.txt.

Umieść folderze zip i hasło w kontenerze dedykowane na koncie Azure miejsca do magazynowania.

![Lokalizacja folderu](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Jeśli wolisz zachować te pliki w udziale w sieci, możesz to zrobić. Po prostu należy się upewnić, że zasób DSC, który będzie używany później ma dostęp i można uzyskać instalacja i hasło.

## <a name="step-2-create-the-dsc-configuration"></a>Krok 2: Tworzenie konfiguracji DSC

Ustawienia zależy od WMF 5.0. Na komputerze pomyślnie zastosować konfigurację za pośrednictwem DSC automatyzacji WMF 5.0 musi znajdować się.

Środowisko używa następującej konfiguracji DSC przykładzie:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Konfiguracja będzie wykonaj następujące czynności:

- Zmienne informuje konfiguracji, gdzie można uzyskać plików binarnych usługę mobilności i agenta maszyn wirtualnych Azure, skąd można uzyskać hasło i miejsca przechowywania danych wyjściowych.
- Konfiguracji zaimportuje zasobów xPSDesiredStateConfiguration DSC, dzięki czemu można użyć `xRemoteFile` do pobierania plików z repozytorium.
- Konfiguracja utworzy katalog miejsce, w którym chcesz przechowywać pliki binarne.
- Zasób archiwum zostanie Wyodrębnij pliki z folderu zip.
- Pakiet zasobów instalacji instaluje usługę mobilności z UNIFIEDAGENT. EXE Instalatora z określonych argumentów. (Zmiennych, które skonstruować argumenty konieczne zostać zmieniony w celu dostosowania środowiska).
- Pakiet zasobów AzureAgent instaluje agenta maszyn wirtualnych Azure, co jest zalecane w każdej maszyn wirtualnych uruchamianej platformy Azure. Agent maszyn wirtualnych Azure umożliwia także dodać rozszerzenia do maszyn wirtualnych po przełączeniu.
- Zapewnia usługi lub zasobów zawsze używać powiązanych usług mobilności i usług Azure.

Zapisywanie konfiguracji jako **ASRMobilityService**.

>[AZURE.NOTE] Pamiętaj zamienić CSIP w konfiguracji z uwzględnieniem serwera zarządzania rzeczywiste, dzięki czemu agent zostanie poprawnie połączone i użyje poprawne hasło.

## <a name="step-3-upload-to-automation-dsc"></a>Krok 3: Przekazywanie do automatyzacji DSC

Spowoduje to zaimportowanie konfiguracji DSC, które zostały wprowadzone wymagane moduł zasobów DSC (xPSDesiredStateConfiguration), należy więc do zaimportowania tego modułu w automatyzacji przed przekazaniem konfiguracji DSC.

Zaloguj się do swojego konta automatyzacji, przejdź do **aktywów** > **moduły**i kliknij pozycję **Przeglądaj w galerii**.

W tym miejscu możesz wyszukiwać moduł i zaimportuj go do swojego konta.

![Importowanie modułu](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Gdy skończysz, przejdź do komputera, w którym masz moduły Menedżera zasobów Azure zainstalowany i przejdź do importowania konfiguracji DSC nowo utworzony.

### <a name="import-cmdlets"></a>Polecenia cmdlet importu

W programie PowerShell Zaloguj się do subskrypcji usługi Azure. Modyfikowanie polecenia cmdlet, aby odzwierciedlała środowiska i przechwytywanie informacji o koncie automatyzacji w zmiennej:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Przekaż konfiguracji do automatyzacji DSC przy użyciu następującego polecenia cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Można skompilować konfiguracji w DSC automatyzacji

Następnie należy skompilować konfiguracji w DSC automatyzacji tak, aby rozpocząć rejestrowanie węzły do niego. Który osiągnąć, uruchamiając następujące polecenie cmdlet:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Może to zająć kilka minut, ponieważ zasadniczo jest instalowany konfiguracji hostowanej usłudze pobieraj DSC.

Po skompilowaniu konfiguracji przy użyciu programu PowerShell (Get-AzureRmAutomationDscCompilationJob) lub za pomocą [Azure portal](https://portal.azure.com/)można pobrać informacje o zadaniu.

![Pobieranie zadania](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Teraz pomyślnie opublikowany i przekazane konfiguracji DSC do DSC automatyzacji.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Krok 4: Wbudowany komputerach, aby DSC automatyzacji
>[AZURE.NOTE] Jedną z wymagania wstępne dotyczące wykonywania tego scenariusza jest to, że komputery systemu Windows są aktualizowane z najnowszą wersją WMF. Możesz pobrać i zainstalować poprawną wersję dla swojej platformy z [Centrum pobierania](https://www.microsoft.com/download/details.aspx?id=50395).

Teraz utworzysz metaconfig DSC, którego będzie dotyczyć węzły. Aby pomyślnie z tymi musisz uzyskać adres URL punktu końcowego i klucza podstawowego dla wybranego konta automatyzacji platformy Azure. Można znaleźć te wartości w obszarze **klawiszy** na karta **wszystkie ustawienia** konta automatyzacji.

![Wartości klucza](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

W tym przykładzie masz serwera fizycznego Windows Server 2012 R2, który ma być chroniona za pomocą Odzyskiwanie witryny.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Sprawdź oczekujących operacje zmiany nazwy plików w rejestrze

Przed rozpoczęciem skojarzenia na serwerze z punktem końcowym DSC automatyzacji, zalecamy Sprawdzanie oczekujących operacje zmiany nazwy plików w rejestrze. Mogą one uniemożliwić konfiguracji Kończenie ze względu na oczekiwanie Uruchom ponownie komputer.

Uruchom następujące polecenie cmdlet, aby sprawdzić, czy jest Uruchom ponownie komputer nie oczekiwanie na serwerze:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Spowoduje to wyświetlenie puste, są OK, aby kontynuować. W przeciwnym razie zwracają ten przez ponownego uruchomienia serwera w oknie konserwacji.

Aby zastosować konfigurację na serwerze, uruchom środowisko zintegrowane skryptów w PowerShell (ISE) i uruchom następujący skrypt. To jest zasadniczo DSC lokalnej konfiguracji, która będzie poinstruuj aparat Menedżer konfiguracji lokalnej do rejestrowania w usłudze automatyzacji DSC i pobrać określonej konfiguracji (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Ta konfiguracja spowoduje, że aparat Menedżer konfiguracji lokalnej, aby zarejestrować się z DSC automatyzacji. Również określi, jak aparat powinna działać, co on zrobić, jeśli jest odchylenie konfiguracji (ApplyAndAutoCorrect) i jak go należy kontynuować konfigurowanie Jeśli wymagane jest ponowne.

Po uruchomieniu tego skryptu węzeł powinna rozpoczynać się zarejestrować za pomocą DSC automatyzacji.

![Węzeł rejestracji w toku](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Jeśli możesz przejść z powrotem do portalu Azure, zobaczysz, że węzeł nowo zarejestrowanych okazało się teraz w portalu.

![Węzeł zarejestrowanych w portalu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Na serwerze można wykonać następujące polecenia cmdlet programu PowerShell, aby zweryfikować, że węzeł został zarejestrowany poprawnie:

```powershell
Get-DscLocalConfigurationManager
```

Po ukończeniu pobierane konfiguracji i zastosowane na serwerze, możesz to sprawdzić, uruchamiając następujące polecenie cmdlet:

```powershell
Get-DscConfigurationStatus
```

Wynik pokazano, że serwer został pomyślnie pobierane konfigurację:

![Wynik](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Ponadto ustawienia usługi mobilności ma własny dziennika, który można znaleźć w *SystemDrive*\ProgramData\ASRSetupLogs.

To wszystko. Teraz pomyślnie wdrożony i zarejestrowana usługę mobilności na komputerze, który ma być chroniona za pomocą Odzyskiwanie witryny. DSC będzie upewnij się, że wymagane usługi są uruchomione zawsze.

![Pomyślnego wdrożenia](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Po serwer zarządzania wykryje pomyślnego wdrożenia, można skonfigurować ochrony i włączanie replikacji na komputerze za pomocą Odzyskiwanie witryny.

## <a name="use-dsc-in-disconnected-environments"></a>Używanie DSC w środowiskach Rozłączono

Jeśli komputery nie masz połączenia z Internetem, możesz nadal polega na DSC wdrażać i skonfigurować usługę mobilności na obciążenia, które mają być chronione.

Można utworzyć własny serwer pobieraj DSC w środowisku zapewnienie zasadniczo takie same możliwości, które otrzymujesz z DSC automatyzacji. Oznacza to, że klienci będą pobierać konfiguracji (po jest zarejestrowany) do punktu końcowego DSC. Innym rozwiązaniem jest jednak ręcznie push konfiguracji DSC programu komputerach lokalnie lub zdalnie.

Należy zauważyć, że w tym przykładzie jest dodane parametru dla nazwy komputera. Pliki zdalne teraz znajdują się w udziale zdalnym, która powinna być dostępna przez komputery, które mają być chronione. Koniec skryptu enacts konfiguracji, a następnie uruchamia się w konfiguracji DSC do komputera docelowego.

### <a name="prerequisites"></a>Wymagania wstępne

Upewnij się, że modułu programu PowerShell xPSDesiredStateConfiguration jest zainstalowany. W przypadku komputerów systemu Windows z zainstalowanym WMF 5.0 możesz zainstalować moduł xPSDesiredStateConfiguration, uruchamiając następujące polecenie cmdlet na komputerach docelowych:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Możesz również pobrać i Zapisz moduł, w przypadku, gdy trzeba rozpowszechnić go na komputerach systemu Windows, które mają WMF 4.0. Uruchom tego polecenia cmdlet na komputerze, w których ma PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Również 4.0 WMF się upewnić, że [System Windows 8.1 aktualizowanie KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) jest zainstalowany na komputerach.

Następujące może zostać przeniesiony do WMF 4.0 i WMF 5.0 komputerach systemu Windows:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Jeśli chcesz utworzyć własny serwer pobieraj DSC w sieci firmowej symulowanie możliwości, jakie możesz przejść z DSC automatyzacji, zobacz [Konfigurowanie DSC pobieraj serwera](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Opcjonalnie: Wdrażanie konfiguracji DSC za pomocą szablonu Azure Menedżera zasobów

Ten artykuł zawiera dotyczyła sposób tworzenia własną konfigurację DSC, aby automatycznie wdrożyć usługę mobilności i agenta maszyn wirtualnych Azure — i upewnij się, że działają na komputerach, które mają być chronione. Ponadto mamy Menedżera zasobów Azure szablonu, który zostanie wdrożony tej konfiguracji DSC do nowego lub istniejącego konta automatyzacji Azure. Szablon zostanie umożliwia utworzenie aktywów automatyzacji, zawierające zmienne w środowisku usługi parametrów wejściowych.

Po wdrożeniu szablonu to odwołanie do kroku 4 w tym przewodniku płycie po prostu komputery.

Szablon zostanie wykonaj następujące czynności:

1. Użyj istniejącego konta automatyzacji lub Utwórz nową
2. Sporządzanie parametrów wejściowych dla:
    - ASRRemoteFile — lokalizację, w której jest przechowywany ustawienia usługi dla firm — mobilność
    - ASRPassphrase — lokalizację, w której jest przechowywany plik passphrase.txt
    - ASRCSEndpoint — adres IP serwera zarządzania
3. Importowanie modułu programu PowerShell xPSDesiredStateConfiguration
4. Tworzenie i kompilowania konfiguracji DSC

Poprzednie kroki nastąpi w odpowiedniej kolejności, tak aby można uruchomić ułatwiającej rozpoczęcie korzystania z urządzenia do ochrony.

Szablon z instrukcjami do wdrożenia, znajduje się na [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Wdrażanie szablon przy użyciu programu PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Następne kroki

Po wdrożeniu agentów usługi mobilności, możesz [włączyć replikacji](site-recovery-vmware-to-azure.md#step-6-replicate-applications) dla maszyn wirtualnych.
