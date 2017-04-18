<properties
    pageTitle="Konfigurowanie zawsze na grupy dostępności w Azure maszyn wirtualnych przy użyciu programu PowerShell"
    description="Ten samouczek używa zasobów utworzone za pomocą modelu Klasyczny wdrożenia i użycia programu PowerShell w celu utworzenia zawsze na grupy dostępności platformy Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfigurowanie zawsze na grupy dostępności w Azure maszyn wirtualnych przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Menedżer zasobów: szablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Menedżer zasobów: ręczne](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasyczny: interfejs użytkownika](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasyczny: programu PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure wirtualnych maszyn może pomóc administratorom bazy danych do wykonania niższe koszty wysokiej dostępności systemu programu SQL Server. Ten samouczek pokazano, jak wdrażać grupy dostępności za pomocą programu SQL Server zawsze na końcu do końca w środowisku Azure. Na koniec samouczka rozwiązanie programu SQL Server zawsze na Azure będzie się składał z następujących elementów:

- Wirtualna sieć zawierająca wiele podsieci, łącznie z zewnętrzną i podsieć wewnętrznej

- Kontrolera domeny z domeną usługi Active Directory (AD)

- Dwa SQL Server maszyny wirtualne wdrożyć podsieci wewnętrznej i dołączony do domeny AD

- Klaster WSFC węzeł 3 z modelem kworum Większość węzłów

- Grupy dostępności z dwoma replikami synchroniczne Zatwierdź dostępność bazy danych

W tym scenariuszu jest wybierana dla jego uproszczenia, nie dla jego skuteczność kosztów i inne czynniki Azure. Na przykład można zminimalizować liczbę maszyny wirtualne dla grupy dostępność replice dwóch w celu zapisania na godziny obliczeń w Azure przy użyciu kontrolera domeny jako monitora udostępniania plików kworum w klastrze WSFC węzeł 2. Ta metoda zmniejsza statystykę maszyn wirtualnych przez jeden z podanych konfiguracji.

Ten samouczek ma przedstawiono kroki wymagane do skonfigurowania rozwiązanie opisany powyżej bez opracowanie na stronie szczegóły każdego kroku. W związku z tym zamiast z kroków konfiguracji Graficznym, używa programu PowerShell skryptów pozwalają szybko przez poszczególne kroki. Zakłada się następujące czynności:

- Masz już konto Azure z subskrypcji maszyn wirtualnych.

- Zainstalowano [Azure poleceń cmdlet](../powershell-install-configure.md).

- Masz już pełny opis grup zawsze na dostępność dla lokalnego rozwiązania. Aby uzyskać więcej informacji zobacz [Zawsze na grupy dostępności (program SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Nawiązywanie połączenia z subskrypcji usługi Azure i tworzenie wirtualnych sieci

1. W oknie programu PowerShell na komputerze lokalnym zaimportować moduł Azure, Pobierz plik ustawień publikowania na komputerze i łączenie sesji programu PowerShell do subskrypcji usługi Azure przez importowanie pobranych ustawień publikowania.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Polecenie **Get-AzurePublishgSettingsFile** umożliwia automatyczne generowanie zarządzania certyfikat z Azure pobiera go na komputerze. Przeglądarki zostanie otwarty automatycznie i zostanie wyświetlony monit o podanie poświadczeń konta Microsoft Azure subskrypcji. Plik pobrany **.publishsettings** zawiera wszystkie informacje potrzebne do zarządzania Azure subskrypcji. Po zapisaniu tego pliku do katalogu lokalnego, zaimportuj go za pomocą polecenia **Importuj AzurePublishSettingsFile** .

    >[AZURE.NOTE] Plik publishsettings zawiera poświadczenia (niezakodowany) używane do administrowania Azure subskrypcje i usług. Ze względów bezpieczeństwa dla tego pliku jest zapisać go tymczasowo spoza Twojej katalogi źródłowe (na przykład w folderze Libraries\Documents), a następnie usuń je po zakończeniu importowania. Złośliwy użytkownik uzyska dostęp do pliku publishsettings można edytować, tworzenie i usuwanie usługi Azure.

1. Definiowanie serii czynników, które będzie używane do tworzenia usługi infrastruktury chmury.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Zwróć uwagę na następujące czynności, aby upewnić się, że polecenia powiedzie później:

    - Zmienne **$storageAccountName** i **$dcServiceName** muszą być unikatowe, ponieważ są one używane do identyfikowania chmury miejsca do magazynowania konta i chmura serwera, odpowiednio w Internecie.

    - Nazwy określone dla zmiennych **$affinityGroupName** i **$virtualNetworkName** są skonfigurowane w dokumencie konfiguracji wirtualnej sieci, który będzie późniejszego użycia.

    - **$sqlImageName** nazwa zaktualizowane obrazu maszyn wirtualnych, która zawiera SQL Server 2012 z dodatkiem Service Pack 1 Enterprise Edition.

    - Dla uproszczenia **Contoso! 000** jest to samo hasło używane w całym cały samouczek.

1. Tworzenie grupy koligacji.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Tworzenie wirtualnych sieci, importując plik konfiguracji.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Plik konfiguracyjny zawiera następujący dokument XML. Krótko mówiąc Określa on, wirtualną sieć o nazwie **ContosoNET** w grupie koligacji o nazwie **ContosoAG**i ma miejsce adres **10.10.0.0/16** i ma dwóch podsieci, **10.10.1.0/24** i **10.10.2.0/24**, które są podsieci przedniej i z powrotem podsieci, odpowiednio. Przednia podsieć jest miejsce, w którym możesz umieścić aplikacji klienta, takich jak Microsoft SharePoint i ponownie podsieci będą umieszczane są maszyny wirtualne serwera SQL. Jeśli zmienisz zmienne **$affinityGroupName** i **$virtualNetworkName** wcześniej, możesz również zmienić odpowiadającymi im nazwami poniżej.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Utwórz konto miejsca do magazynowania, który jest skojarzony z grupą koligacji została utworzona i jest ustawiony jako konta miejsca do magazynowania w ramach subskrypcji.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Tworzenie serwera kontrolera domeny w nowy zestaw usług i dostępności chmury.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Tę serię gazociągami polecenia wykonaj następujące czynności:

    - **Nowy AzureVMConfig** tworzy konfigurację maszyn wirtualnych.

    - **Dodaj AzureProvisioningConfig** zwraca parametry konfiguracji serwera autonomicznego systemu Windows.

    - **Dodaj AzureDataDisk** dodaje dysku danych używaną do przechowywania danych usługi Active Directory z pamięci podręcznej opcję Brak.

    - **Nowy AzureVM** tworzy nowe usługi w chmurze oraz nowe maszyn wirtualnych Azure w nowej usługi w chmurze.

1. Poczekaj, aż nowe maszyn wirtualnych w pełni być przygotowana i pobierania pliku z pulpitu zdalnego do katalogu pracy. Ponieważ nowa maszyna wirtualna Azure trwa długo udzielenia, pętla ankieta nowa maszyna wirtualna, dopóki nie jest gotowa do użycia w dalszym ciągu.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Serwer kontrolera domeny jest teraz pomyślnie zainicjowano obsługę administracyjną. Następnie skonfiguruj usługi Active Directory na tym serwerze kontrolera domeny. Pozostaw otwarte okno programu PowerShell na komputerze lokalnym. Użyje go później utworzyć dwa maszyny wirtualne serwera SQL.

## <a name="configure-the-domain-controller"></a>Konfigurowanie kontrolera domeny

1. Łączenie się z serwerem kontrolera domeny przez uruchamianie pliku z pulpitu zdalnego. Korzystanie z administratorem komputera AzureAdmin nazwy użytkownika i hasła **Contoso! 000**, określonej podczas tworzenia nowych maszyn wirtualnych.

1. Otwórz okno programu PowerShell w trybie administratora.

1. Uruchom następujące polecenie **DCPROMO. EXE** polecenie, aby skonfigurować domenę **corp.contoso.com** , z katalogów danych znajdujących się na dysku M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Po wykonaniu polecenia automatycznie uruchamia ponownie maszyn wirtualnych.

1. Nawiązywanie połączenia z serwerem kontrolera domeny ponownie uruchamiając pliku z pulpitu zdalnego. Tym razem, zaloguj się jako **CORP\Administrator**.

1. Otwórz okno programu PowerShell w trybie administratora i zaimportować moduł Active Directory programu PowerShell przy użyciu następującego polecenia:

        Import-Module ActiveDirectory

1. Uruchom następujące polecenia, aby dodać trzech użytkowników do domeny.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** jest używany do konfigurowania wszystkie związane z wystąpienia usługi SQL Server, klaster WSFC i grupy dostępności. **CORP\SQLSvc1** i **CORP\SQLSvc2** są używane jako konta usług SQL Server dla dwóch maszyny wirtualne serwera SQL.

1. Następnie uruchom następujące polecenia, aby nadać **CORP\Install** uprawnienia do tworzenia obiektów komputera w domenie.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Identyfikator GUID wymienione powyżej jest GUID dla komputera typu obiektu. Konto **CORP\Install** musi uprawnieniami **Odczyt wszystkich właściwości** i **Tworzenie obiektów typu komputer** w celu utworzenia aktywnego bezpośredni obiektów w celu klaster WSFC. Uprawnień **Odczyt wszystkich właściwości** jest już używana CORP\Install domyślnie, dzięki czemu nie trzeba udzielić go jawnie. Aby uzyskać więcej informacji o uprawnieniach potrzebne do utworzenia klaster WSFC, zobacz [Przewodnik krok po kroku klaster pracy awaryjnej: Konfigurowanie konta w usłudze Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Teraz, gdy skończysz konfigurowania usługi Active Directory i obiektów użytkownika, utworzysz dwa maszyny wirtualne serwera SQL i połączyć je do tej domeny.

## <a name="create-the-sql-server-vms"></a>Tworzenie maszyny wirtualne serwera SQL

1. Przejdź do za pomocą okna programu PowerShell, który jest otwarty na komputerze lokalnym. Definiowanie następujące dodatkowe zmienne:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Adres IP **10.10.0.4** zazwyczaj jest przypisana do pierwszego maszyn wirtualnych można tworzyć w podsieci **10.10.0.0/16** Azure wirtualnej sieci. Należy sprawdzić, czy jest to adres serwera kontrolera domeny, uruchamiając **IPCONFIG**.

1. Uruchom następujące polecenia gazociągami, aby utworzyć pierwszy maszyn wirtualnych w klastrze WSFC o nazwie **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Weź pod uwagę następujące dotyczące powyżej polecenia:

    - **Nowy AzureVMConfig** tworzy konfiguracji maszyn wirtualnych o nazwie ustaw odpowiednią dostępność. Kolejne maszyny wirtualne zostanie utworzony o takiej samej nazwie zestawu dostępność tak, aby ich dołączenia do tego samego zestawu dostępności.

    - **Dodaj AzureProvisioningConfig** łączy maszyn wirtualnych do domeny usługi Active Directory, który został utworzony.

    - **Ustawianie AzureSubnet** umieszczać maszyn wirtualnych w podsieci Wstecz.

    - **Nowy AzureVM** tworzy nowe usługi w chmurze oraz nowe maszyn wirtualnych Azure w nowej usługi w chmurze. Parametr **DnsSettings** Określa, że serwer DNS dla serwerów w nowej usługi w chmurze ma IP adres **10.10.0.4**, który jest adres IP serwera kontrolera domeny. Ten parametr jest potrzebny umożliwiające nowe maszyny wirtualne w usłudze w chmurze do przyłączenia się do usługi Active Directory pomyślnie. Ten parametr nie zostanie ręczne konfigurowanie ustawień IP protokołu IPv4 w swojej maszyn wirtualnych do korzystania z serwera kontrolera domeny jako podstawowy serwer DNS po zainicjowano obsługę administracyjną maszyn wirtualnych, a następnie dołącz maszyn wirtualnych z domeną usługi Active Directory.

1. Uruchom następujące polecenia gazociągami tworzenie maszyny wirtualne serwera SQL, o nazwie **ContosoSQL1** i **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Weź pod uwagę następujące dotyczące poleceń powyżej:

    - **Nowy AzureVMConfig** używa tej samej nazwy zestawu dostępność jako serwera kontrolera domeny, a następnie używa obrazu SQL Server 2012 z dodatkiem Service Pack 1 Enterprise Edition w galerii maszyn wirtualnych. Określa też dysku systemu operacyjnego do odczytu buforowania tylko (nie buforowanie zapisu). Zaleca się migrowanie plików bazy danych na dysk odrębne dołączenia do maszyn wirtualnych i skonfiguruj go z odczytu lub zapisu pamięci podręcznej. Następnym krokiem jest jednak do usunięcia, buforowanie zapisu na dysku systemu operacyjnego, ponieważ nie można usunąć więcej pamięci podręcznej na dysku systemu operacyjnego.

    - **Dodaj AzureProvisioningConfig** łączy maszyn wirtualnych do domeny usługi Active Directory, który został utworzony.

    - **Ustawianie AzureSubnet** umieszczać maszyn wirtualnych w podsieci Wstecz.

    - **Dodaj AzureEndpoint** dodaje punkty końcowe programu access, co aplikacje klienckie można uzyskać dostęp do tych wystąpień usług SQL Server w Internecie. Różne porty podano ContosoSQL1 i ContosoSQL2.

    - **Nowy AzureVM** tworzy nowe maszyn wirtualnych serwera SQL w samej usługi w chmurze jako ContosoQuorum. Maszyny wirtualne należy umieścić w tym samym usługi w chmurze, jeśli mają się znajdować się w ten sam zestaw dostępności.

1. Czekaj na poszczególnych maszyn wirtualnych w pełni być przygotowana i pobierania jej pliku z pulpitu zdalnego do katalogu pracy. Pętla przewijanie trzy nowe maszyny wirtualne i wykonuje polecenia podanego w nawiasach klamrowych najwyższego poziomu dla każdego z nich.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    Teraz zainicjowano pośrednictwem SQL Server SMS i uruchomiony, ale są instalowane z programem SQL Server przy użyciu opcji domyślnych.

## <a name="initialize-the-wsfc-cluster-vms"></a>Inicjowanie maszyny wirtualne klaster WSFC

W tej sekcji należy zmodyfikować trzech serwerów, które w grupie WSFC i instalacji programu SQL Server. W szczególności:

- (Wszystkie serwery mają) Należy zainstalować funkcję **Awaryjnej** .

- (Wszystkie serwery mają) Musisz dodać **CORP\Install** jako **administrator**komputera.

- (ContosoSQL1 i ContosoSQL2 tylko) Musisz dodać **CORP\Install** rolę **administratora systemu** w domyślnej bazy danych.

- (ContosoSQL1 i ContosoSQL2 tylko) Musisz dodać **NT\SYSTEM** jako nazwa logowania z następujących uprawnień:

    - Zmienić wszystkie grupy dostępności

    - Łączenie SQL

    - Wyświetlanie stanu serwera

- (ContosoSQL1 i ContosoSQL2 tylko) Protokołu **TCP** jest już włączone maszyn wirtualnych serwera SQL. Nadal potrzebny otworzyć zaporę dla dostępu zdalnego programu SQL Server.

Teraz możesz przystąpić do uruchomienia. Począwszy od **ContosoQuorum**, wykonaj następujące czynności:

1. Podłącz do **ContosoQuorum** , uruchamiając pliki pulpitu zdalnego. Korzystanie z administratorem komputera **AzureAdmin** nazwy użytkownika i hasła **Contoso! 000**, określonej podczas tworzenia maszyny wirtualne.

1. Upewnij się, że komputery zostały pomyślnie dołączyli do **corp.contoso.com**.

1. Poczekaj na zakończenie uruchamianie zadań automatycznego inicjowania przed kontynuowaniem instalacji programu SQL Server.

1. Otwórz okno programu PowerShell w trybie administratora.

1. Zainstaluj funkcję awaryjnej systemu Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Dodaj **CORP\Install** jako administrator lokalny.

        net localgroup administrators "CORP\Install" /Add

1. Wyloguj się z ContosoQuorum. Po zakończeniu tego serwera teraz.

        logoff.exe

Następnie zainicjować **ContosoSQL1** i **ContosoSQL2**. Wykonaj poniższe czynności, które są identyczne w obu maszyny wirtualne serwera SQL.

1. Podłącz do dwóch maszyny wirtualne serwera SQL uruchamiając pliki pulpitu zdalnego. Korzystanie z administratorem komputera **AzureAdmin** nazwy użytkownika i hasła **Contoso! 000**, określonej podczas tworzenia maszyny wirtualne.

1. Upewnij się, że komputery zostały pomyślnie dołączyli do **corp.contoso.com**.

1. Poczekaj na zakończenie uruchamianie zadań automatycznego inicjowania przed kontynuowaniem instalacji programu SQL Server.

1. Otwórz okno programu PowerShell w trybie administratora.

1. Zainstaluj funkcję awaryjnej systemu Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Dodawanie **CORP\Install** jako administrator lokalny

        net localgroup administrators "CORP\Install" /Add

1. Importowanie dostawca programu PowerShell programu SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Dodaj **CORP\Install** rolę administratora systemu dla domyślnego wystąpienia programu SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Dodawanie **NT\SYSTEM** jako nazwa logowania z trzech uprawnienia opisane powyżej.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Otwórz zaporę dla dostępu zdalnego programu SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Wyloguj się z obu maszyny wirtualne.

        logoff.exe

Na koniec możesz przystąpić do konfigurowania grupy dostępności. Dostawca programu PowerShell programu SQL Server użyje do wykonywania pracy na **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Konfigurowanie grupy dostępności

1. Ponownie nawiązać **ContosoSQL1** uruchamiając pliki pulpitu zdalnego. Zamiast logowanie się przy użyciu konta komputera, zaloguj się przy użyciu **CORP\Install**.

1. Otwórz okno programu PowerShell w trybie administratora.

1. Definiowanie następujących czynników:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Importowanie dostawca programu PowerShell programu SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Zmienianie konta usługi programu SQL Server dla ContosoSQL1 do CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Zmienianie konta usługi programu SQL Server dla ContosoSQL2 do CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Pobierz **CreateAzureFailoverCluster.ps1** z [Utworzyć klaster WSFC zawsze na dostępność grup w Azure maszyn wirtualnych](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) do katalogu lokalnego pracy. Ten skrypt użyje ułatwia tworzenie funkcjonalności klaster WSFC. Aby uzyskać ważnych informacji na temat interakcji WSFC z sieci Azure zobacz [wysoką dostępność i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).

1. Zmiana katalog roboczy i utworzyć klaster WSFC pobrany scenariusz.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Włącz zawsze na grupy dostępność dla domyślnego wystąpienia programu SQL Server na **ContosoSQL1** i **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Tworzenie kopii zapasowej katalogu i udzielanie uprawnień dla kont usług SQL Server. Ten katalog będzie umożliwia przygotowanie dostępność bazy danych w replice pomocniczą.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Tworzenie bazy danych na **ContosoSQL1** o nazwie **MyDB1**, zarówno pełną kopię zapasową, jak i kopia zapasowa dziennika i ich przywracania na **ContosoSQL2** z opcją **Z NORECOVERY** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Tworzenie grupy punkty końcowe dostępności na maszyny wirtualne serwera SQL i ustaw odpowiednie uprawnienia na punkty końcowe.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Tworzenie replik dostępności.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Na koniec Utwórz grupę dostępność i dołączać replice pomocniczej do grupy dostępności.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Następne kroki
Możesz teraz pomyślnie wdrożono program SQL Server zawsze włączone, tworząc grupy dostępności platformy Azure. Aby skonfigurować detektor dla tej grupy dostępności, zobacz [Konfigurowanie detektor ILB zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Aby uzyskać inne informacje o korzystaniu z programu SQL Server w Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
