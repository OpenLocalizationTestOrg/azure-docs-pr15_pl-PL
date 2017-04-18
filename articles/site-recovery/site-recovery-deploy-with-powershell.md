<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM przy użyciu programu PowerShell i odzyskiwanie witryny Azure | Microsoft Azure"
    description="Dowiedz się, jak zautomatyzować replikacji maszyn wirtualnych funkcji Hyper-V w chmury VMM przy użyciu programu PowerShell i odzyskiwanie witryny."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure przy użyciu programu Powershell — klasyczny

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasyczny portalu](site-recovery-vmm-to-azure-classic.md)
- [PowerShell — klasyczny](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Omówienie

Azure Odzyskiwanie witryny składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych w szeregu wdrożeń. Aby uzyskać pełną listę wdrożeń zobacz [Omówienie Azure witryny odzyskiwania](site-recovery-overview.md).

W tym artykule pokazano, jak za pomocą programu PowerShell Automatyzowanie typowych zadań, które należy wykonać podczas konfigurowania Odzyskiwanie witryny Azure replikacji maszyn wirtualnych funkcji Hyper-V w chmury VMM Centrum systemu Azure miejsca do magazynowania.

W tym artykule zawiera wymagania wstępne dotyczące tego scenariusza i pokazano, jak skonfigurować magazynu Odzyskiwanie witryny, instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM źródłowym, zarejestrować serwer w magazyn, Dodaj konto Azure miejsca do magazynowania, zainstalować agenta usługi Azure odzyskiwania na serwerach hosta funkcji Hyper-V, konfigurowanie ustawień ochrony dla chmur VMM, które będą stosowane do wszystkich chronionych maszyn wirtualnych , a następnie włączyć ochronę tych maszyn wirtualnych. Zakończyć, testując przełączanie awaryjne do upewnij się, że wszystko działa zgodnie z oczekiwaniami.

Jeśli wystąpią problemy, aby skonfigurować w tym scenariuszu ogłaszać pytania na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure występują dwa modele rozmieszczania do tworzenia i pracy z zasobami: [Menedżer zasobów i klasyczny](../resource-manager-deployment-model.md). W tym artykule opisano, jak przy użyciu modelu wdrożenia klasyczny.



## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały spełnione następujące wymagania wstępne w miejscu:

### <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

- Musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Musisz mieć konto Azure miejsca do magazynowania, aby zapisać zreplikowanej dane. Konto musi replikacji geo włączone. Powinien znajdować się w tym samym regionie jako magazynu Odzyskiwanie witryny Azure i zostaną skojarzone z tej samej subskrypcji. [Dowiedz się więcej o Azure miejsca do magazynowania](../storage/storage-introduction.md).
- Konieczne będzie upewnij się, że maszyn wirtualnych, które mają być chronione spełniają [wymagania wstępne dotyczące Azure maszyn wirtualnych](site-recovery-best-practices.md#virtual-machines).

### <a name="vmm-prerequisites"></a>Wymagania wstępne dotyczące VMM
- Konieczne będzie serwerem VMM System Centrum 2012 R2.
- Konieczne będzie chmurze co najmniej jeden na serwerze VMM, którą chcesz chronić. Chmura powinien zawierać:
    - Jedną lub więcej grup hosta VMM.
    - Jeden lub więcej serwerów głównych funkcji Hyper-V lub klastrów w każdej grupie hosta.
    - Co najmniej jeden maszyn wirtualnych na serwerze źródłowym funkcji Hyper-V.

### <a name="hyper-v-prerequisites"></a>Wymagania wstępne dotyczące funkcji Hyper-V

- Serwery hosta funkcji Hyper-V musi działać co najmniej **systemu Windows Server 2012** z roli Hyper-V lub **Microsoft Hyper-V Server 2012** i zainstalowano najnowsze aktualizacje.
- Jeśli używasz funkcji Hyper-V w notatce klaster tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. W tym celu w Menedżerze serwera > Menedżer klastrów pracy awaryjnej, połącz się z klastrem, kliknij **Konfigurowanie ról** i wybierz **Brokera replice funkcji Hyper-V** na ekranie **Wybierz rolę** Kreatora wysokiej dostępności.
- Serwer hosta funkcji Hyper-V ani klaster, dla której chcesz zarządzać ochrona ma być uwzględniana w chmurze VMM.

### <a name="network-mapping-prerequisites"></a>Wymagania wstępne dotyczące sieci mapowania
Przy ochrony maszyn wirtualnych w mapach mapowania Azure sieci między sieciami maszyn wirtualnych na serwerze VMM źródłowym i docelowych Azure sieciach, zapewniając następujące czynności:

- Wszystkie komputery, które nie nad w tej samej sieci połączyć się ze sobą, niezależnie od tego, który plan odzyskiwania znajdują się w.
- Jeśli brama sieci jest skonfigurowana w celu Azure sieci, maszyn wirtualnych można nawiązać pozostałych maszyn wirtualnych lokalnego.
- Jeśli nie skonfigurowano sieci mapowania tylko maszyn wirtualnych, które kończą się niepowodzeniem nad w tego samego planu odzyskiwania będzie mógł połączyć się ze sobą po przełączeniu Azure.

Jeśli chcesz wdrożyć mapowanie sieci najpierw następujące czynności:

- Maszyn wirtualnych, które mają być chronione na serwerze źródłowym VMM powinny być połączone z siecią maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
- Sieć Azure, do której można podłączyć zreplikowanej maszyn wirtualnych po przełączeniu. Możesz wybrać tej sieci w czasie pracy awaryjnej. W tym samym regionie jako subskrypcji Odzyskiwanie witryny Azure powinny być sieci.
- [Aby uzyskać więcej informacji](site-recovery-network-mapping.md) na temat mapowania sieci:

###<a name="powershell-prerequisites"></a>Wymagania wstępne dotyczące programu PowerShell
Upewnij się, że masz Azure programu PowerShell gotowa do wysłania. Jeśli już używasz programu PowerShell, konieczne będzie uaktualnienie do wersji 0.8.10 lub nowszej. Aby uzyskać informacje dotyczące konfigurowania programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Po skonfigurowaniu i skonfigurowany programu PowerShell można wyświetlić wszystkie dostępne polecenia cmdlet usługi [tutaj](https://msdn.microsoft.com/library/dn850420.aspx).

Aby uzyskać informacje o porady, które mogą ułatwić użyć poleceń cmdlet, takich jak jak wartości parametru, dane wejściowe i wyjściowe zwykle są obsługiwane w programie PowerShell Azure, zobacz [Wprowadzenie do poleceń cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Krok 1: Ustawianie subskrypcji

W programie PowerShell uruchom następujące polecenia cmdlet:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Zamień elementów w obrębie "< >" określonych informacji.

## <a name="step-2-create-a-site-recovery-vault"></a>Krok 2: Tworzenie magazynu Odzyskiwanie witryny

W programie PowerShell Zamień elementów w obrębie "< >" na określonych informacji i uruchom następujące polecenia:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Krok 3: Generuj klucz rejestru magazynu

Generowanie klucza rejestru w magazyn. Po pobraniu dostawca odzyskiwania witryny Azure i zainstalować go na serwerze VMM, aby zarejestrować serwer VMM w magazyn za pomocą tego klucza.

1.  Pobieranie pliku ustawień magazynu i ustawić kontekstu:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Ustaw kontekst magazynu, uruchamiając następujące polecenia:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalowanie dostawcy odzyskiwania Azure witryny

1.  Na komputerze VMM Utwórz katalogu, uruchamiając następujące polecenie:

    ```

    pushd C:\ASR\

    ```

2.  Wyodrębnianie pliki przy użyciu pobranego dostawcy, uruchamiając następujące polecenie

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Instalowanie dostawcy za pomocą następujących poleceń:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Poczekaj na zakończenie instalacji.

4.  Zarejestrować serwer w magazynu przy użyciu następującego polecenia:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Tworzenie konta magazynu platformy Azure

Jeśli nie masz konta usługi Azure miejsca do magazynowania, należy utworzyć konto replikacji geo włączone, uruchamiając następujące polecenie:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Należy zauważyć, że konto miejsca do magazynowania musi znajdować się w tym samym regionie jako usługa Azure witryny odzyskiwania i zostaną skojarzone z tej samej subskrypcji.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Instalowanie agenta usługi Azure odzyskiwania

Z portalu Azure zainstalować agenta usługi Azure odzyskiwania na wszystkich serwerach hosta funkcji Hyper-V znajduje się w chmury VMM, którą chcesz chronić.

Dla wszystkich hostów VMM, uruchom następujące polecenie:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurowanie chmury ustawień ochrony

1.  Tworzenie profilu ochrony chmura Azure, uruchamiając następujące polecenie:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Pobranie kontenera ochrony, uruchamiając następujące polecenia:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Rozpocznij skojarzenia kontenera ochrony z chmurą:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Po zakończeniu zadania, uruchom następujące polecenie:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Po zakończeniu przetwarzania zadania, uruchom następujące polecenie:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Konfigurowanie mapowanie sieci
Przed rozpoczęciem mapowanie sieci upewnij się, że maszyn wirtualnych na serwerze VMM źródłowym jest połączony z siecią maszyn wirtualnych. Ponadto utworzyć jeden lub więcej Azure wirtualnych sieci. Należy zauważyć, że wiele maszyn wirtualnych sieci mogą być mapowane na jednej sieci Azure.

Należy zauważyć, że jeśli sieci docelowej ma wiele podsieci i jedną z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła, a następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączeniu. Jeśli nie jest podsieci docelowej przy użyciu pasujące nazwy, maszyna wirtualna zostanie połączony z pierwszą podsieć w sieci.

Pierwsze polecenie pobiera serwery dla bieżącego magazynu Odzyskiwanie witryny Azure. Polecenie zapisuje serwery Odzyskiwanie witryny Microsoft Azure w zmiennej tablicy $Servers.

    $Servers = Get-AzureSiteRecoveryServer


Drugie polecenie otrzymuje sieci odzyskiwania witryny dla pierwszego serwera w tablicy $Servers. Polecenie zapisuje sieci w zmiennej $Networks.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Trzecie polecenie otrzymuje subskrypcji Azure za pomocą polecenia cmdlet Get-AzureSubscription, a następnie zapisanie tej wartości w zmiennej $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Czwarte polecenie otrzymuje Azure wirtualnych sieci przy użyciu polecenia cmdlet Get-AzureVNetSite, a następnie tę wartość w zmiennej $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



Polecenia cmdlet ostateczne tworzy mapowanie między sieci podstawowej i sieciami Azure maszyn wirtualnych. Polecenie cmdlet określa sieci podstawowej jako pierwszy element $Networks. Polecenie cmdlet określa maszyn wirtualnych sieci jako pierwszy element $AzureVmNetworks za pomocą jego identyfikatora. Polecenie zawiera swojego identyfikatora Azure subskrypcji.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Włączanie ochrony dla maszyn wirtualnych

Po serwerów, chmury i sieci zostały skonfigurowane poprawnie, można włączyć ochronę maszyn wirtualnych w chmurze. Pamiętaj o następujących kwestiach:

Maszyn wirtualnych musi spełniać [wymagania wstępne dotyczące Azure maszyn wirtualnych](site-recovery-best-practices.md#virtual-machines).

Aby włączyć ochronę systemu operacyjnego i system operacyjny właściwości dysku przeznaczona dla maszyny wirtualnej. Po utworzeniu maszyny wirtualnej w VMM przy użyciu szablonu maszyn wirtualnych można ustawić właściwość. Można także ustawić następujące właściwości istniejącego maszyn wirtualnych na kartach **Ogólne** i **Konfiguracji sprzętowej** właściwości maszyn wirtualnych. Jeśli nie ustawisz tych właściwości w VMM można skonfigurować je w portalu Odzyskiwanie witryny Azure.



1.  Aby włączyć ochronę, uruchom następujące polecenie, aby uzyskać kontenerze ochrony:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Uzyskiwanie jednostki ochrony (maszyn wirtualnych), uruchamiając następujące polecenie:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Włącz DR dla maszyn wirtualnych, uruchamiając następujące polecenie:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testowanie wdrożenia

Aby przetestować wdrożenie można uruchomić trybie awaryjnym test dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania składający się z wielu maszyn wirtualnych i uruchomić trybie awaryjnym test dla planu. Testowanie pracy awaryjnej symuluje z mechanizmu awaryjnego i odzyskiwania w odizolowanych sieci. Należy zauważyć, że:

- Jeśli chcesz połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego po przełączeniu włączyć Podłączanie pulpitu zdalnego maszyny wirtualnej przed uruchomieniem tym przełączeniu test.
- Po przełączeniu które będą używane publiczny adres IP połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego. Jeśli chcesz wykonać tę czynność, upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.

Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Tworzenie pliku XML jako szablonu w celu planu odzyskiwania przy użyciu danych poniżej, a następnie zapisz go jako "C:\RPTemplatePath.xml".
2. Zmienianie węzeł RecoveryPlan identyfikator, nazwisko, PrimaryServerId i SecondaryServerId.
3. Zmienianie węzeł ProtectionEntity PrimaryProtectionEntityId (vmid z VMM).
4. Możesz dodać więcej maszyny wirtualne, dodając więcej węzłów ProtectionEntity.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Wypełnianie danych w szablonie:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Tworzenie RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1.  Pobierz obiekt RecoveryPlan, uruchamiając następujące polecenie:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Uruchom tym przełączeniu test, uruchamiając następujące polecenie:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Monitorowanie aktywności

Monitorowanie aktywności za pomocą następujących poleceń. Zauważ, że musisz czekać między zadania do przetwarzania zakończyć.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](https://msdn.microsoft.com/library/dn850420.aspx) o Azure witryny odzyskiwania poleceń cmdlet. </a>.
