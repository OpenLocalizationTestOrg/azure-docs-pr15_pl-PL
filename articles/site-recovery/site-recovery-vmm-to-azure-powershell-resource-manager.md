<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM przy użyciu programu PowerShell (Menedżer zasobów) i odzyskiwanie witryny Azure | Microsoft Azure"
    description="Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM przy użyciu programu PowerShell i odzyskiwanie witryny Azure"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure za pomocą programu PowerShell i Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasyczny portalu](site-recovery-vmm-to-azure-classic.md)
- [PowerShell — klasyczny](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Omówienie

Azure Odzyskiwanie witryny składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych w szeregu wdrożeń. Aby uzyskać pełną listę wdrożeń zobacz [Omówienie Azure witryny odzyskiwania](site-recovery-overview.md).

W tym artykule pokazano, jak za pomocą programu PowerShell Automatyzowanie typowych zadań, które należy wykonać podczas konfigurowania Odzyskiwanie witryny Azure replikacji maszyn wirtualnych funkcji Hyper-V w chmury VMM Centrum systemu Azure miejsca do magazynowania.

W tym artykule zawiera wymagania wstępne dotyczące tego scenariusza i przedstawiono

- Jak skonfigurować magazynu usługi odzyskiwania
- Instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM źródłowym
- Zarejestrować serwer w magazyn, Dodaj konto Azure miejsca do magazynowania
- Instalowanie agenta usługi Azure odzyskiwania na serwerach hosta funkcji Hyper-V
- Konfigurowanie ustawień ochrony dla chmury VMM, które będą stosowane do wszystkich chronionych maszyn wirtualnych
- Włączanie ochrony dla tych maszyn wirtualnych.
- Testowanie przełączaniem, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Jeśli wystąpią problemy, aby skonfigurować w tym scenariuszu ogłaszać pytania na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów i klasyczny](../resource-manager-deployment-model.md). W tym artykule opisano, jak przy użyciu modelu wdrożenia Menedżera zasobów.

## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały spełnione następujące wymagania wstępne w miejscu:

### <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

- Musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Jeśli nie masz, uruchomić przy użyciu [bezpłatnego konta](https://azure.microsoft.com/free). Ponadto można przeczytać o [Menedżer odzyskiwania witryny Azure ceny](https://azure.microsoft.com/pricing/details/site-recovery/).
- Musisz subskrypcji dostawcy, jeśli próbujesz się replikacji do scenariusza subskrypcji dostawcy. Dowiedz się więcej o programie dostawcy w [sposób rejestrowania w programie dostawcy](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Konieczne będzie konto Azure w wersji 2 miejsca do magazynowania (Menedżer zasobów) do przechowywania danych replikować do Azure. Konto musi replikacji geo włączone. Powinny znajdować się w tym samym regionie jako usługa Azure witryny odzyskiwania i być skojarzone z tej samej subskrypcji lub subskrypcji dostawcy. Aby dowiedzieć się więcej o konfigurowaniu Azure miejsca do magazynowania, zobacz [Wprowadzenie do programu Microsoft Azure miejsca do magazynowania](../storage/storage-introduction.md) dla odwołania.
- Konieczne będzie upewnij się, że maszyn wirtualnych, które mają być chronione spełniają [wymagania wstępne dotyczące Azure maszyn wirtualnych](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] Obecnie tylko operacje poziomu maszyn wirtualnych jest możliwe przy użyciu programu Powershell. Obsługa operacji poziomu planu odzyskiwania będą wkrótce.  Teraz możesz są ograniczone do wykonywania przejęcia nie powiodło się tylko na szczegółowości "chroniony maszyn wirtualnych", a nie na poziomie planowanie odzyskiwania.

### <a name="vmm-prerequisites"></a>Wymagania wstępne dotyczące VMM
- Konieczne będzie serwerem VMM System Centrum 2012 R2.
- Dowolny serwer VMM zawierający maszyn wirtualnych, które mają być chronione musi być zainstalowany dostawca odzyskiwania witryny Azure. To jest instalowany podczas wdrażania Odzyskiwanie witryny Azure.
- Konieczne będzie chmurze co najmniej jeden na serwerze VMM, którą chcesz chronić. Chmura powinien zawierać:
    - Jedną lub więcej grup hosta VMM.
    - Jeden lub więcej serwerów głównych funkcji Hyper-V lub klastrów w każdej grupie hosta.
    - Co najmniej jeden maszyn wirtualnych na serwerze źródłowym funkcji Hyper-V.
- Dowiedz się więcej o konfigurowaniu VMM chmury:
    - Dowiedz się więcej o prywatne chmury VMM w [Co nowego w chmurze prywatne z systemu Centrum 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) i [VMM 2012 do chmury](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Informacje na temat [konfigurowania tkaninie chmury VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Po obowiązują elementów tkaninie chmury więcej informacji o tworzeniu prywatne chmury tworzenie [prywatnych chmury w VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) i w [Instruktaż: tworzenie prywatnych chmury za pomocą systemu Centrum 2012 z dodatkiem SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Wymagania wstępne dotyczące funkcji Hyper-V

- Serwery hosta funkcji Hyper-V musi działać co najmniej **systemu Windows Server 2012** z roli Hyper-V lub **Microsoft Hyper-V Server 2012** i zainstalowano najnowsze aktualizacje.
- Jeśli używasz funkcji Hyper-V w notatce klaster tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. Aby uzyskać
- Aby uzyskać instrukcje, zobacz [jak skonfigurować brokera replice funkcji Hyper-V](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Serwer hosta funkcji Hyper-V ani klaster, dla której chcesz zarządzać ochrona ma być uwzględniana w chmurze VMM.

### <a name="network-mapping-prerequisites"></a>Wymagania wstępne dotyczące sieci mapowania
Przy ochrony maszyn wirtualnych w Azure, mapy mapowania sieci maszyny wirtualnej sieci na serwerze VMM źródłowym i docelowych Azure sieci wykonywanie następujących czynności:

- Wszystkie komputery, które nie nad w tej samej sieci połączyć się ze sobą, niezależnie od tego, który plan odzyskiwania znajdują się w.
- Jeśli brama sieci jest skonfigurowana w celu Azure sieci, maszyn wirtualnych można nawiązać pozostałych maszyn wirtualnych lokalnego.
- Jeśli nie skonfigurowano mapowanie sieci, maszyn wirtualnych, których nie powiodło się nad w tego samego planu odzyskiwania będzie mógł połączyć się ze sobą po przełączaniem Azure.

Jeśli chcesz wdrożyć mapowanie sieci najpierw następujące czynności:

- Maszyn wirtualnych, które mają być chronione na serwerze źródłowym VMM powinny być połączone z siecią maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
- Sieć Azure, do której można podłączyć zreplikowanej maszyn wirtualnych po przełączaniem. Możesz wybrać tej sieci w momencie przełączaniem. W tym samym regionie jako subskrypcji Odzyskiwanie witryny Azure powinny być sieci.

Dowiedz się więcej o mapowanie sieci w

- [Jak skonfigurować sieci logicznych w VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Jak skonfigurować maszyn wirtualnych sieci i bramy w VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Jak skonfigurować i monitorowanie wirtualnych sieci platformy Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>Wymagania wstępne dotyczące programu PowerShell
Upewnij się, że masz Azure programu PowerShell gotowa do wysłania. Jeśli już używasz programu PowerShell, konieczne będzie uaktualnienie do wersji 0.8.10 lub nowszej. Aby uzyskać informacje dotyczące konfigurowania programu PowerShell zobacz [Przewodnik po zainstalowaniu i skonfigurowaniu programu PowerShell Azure](../powershell-install-configure.md). Po skonfigurowaniu i skonfigurowany programu PowerShell można wyświetlić wszystkie dostępne polecenia cmdlet usługi [tutaj](https://msdn.microsoft.com/library/dn850420.aspx).

Aby uzyskać informacje o porady, które mogą ułatwić użyć poleceń cmdlet, takich jak jak wartości parametru, dane wejściowe i wyjściowe zwykle są obsługiwane w programie PowerShell Azure, zobacz [Przewodnik Wprowadzenie do poleceń cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Krok 1: Ustawianie subskrypcji

1. Z programu powershell Azure, zaloguj się do konta Azure: za pomocą następujące polecenia cmdlet

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Pobierz listę subskrypcji. Spowoduje to również listy subscriptionIDs dla każdego z subskrypcji. Zanotuj subscriptionID subskrypcji, w której chcesz utworzyć magazynu usługi odzyskiwania

        Get-AzureRmSubscription

3. Ustawianie subskrypcji, w którym ma być utworzony podając identyfikator subskrypcji magazynu usługi odzyskiwania

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2: Tworzenie magazynu usługi odzyskiwania

1. Tworzenie grupy zasobów w Menedżerze zasobów Azure, jeśli nie masz już

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Tworzenie nowego magazynu usługi odzyskiwania i zapisywanie utworzony obiekt magazynu automatycznego odzyskiwania systemu w zmiennej (posłuży później). Możesz również pobrać Tworzenie wpisu obiektów magazynu automatycznego odzyskiwania systemu przy użyciu polecenia cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Ustaw kontekst odzyskiwania usług magazynu

1.  Ustaw kontekst magazynu, uruchamiając poniżej polecenia.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalowanie dostawcy odzyskiwania Azure witryny

1.  Na komputerze VMM Utwórz katalogu, uruchamiając następujące polecenie:

        New-Item c:\ASR -type directory

2.  Wyodrębnianie pliki przy użyciu pobranego dostawcy, uruchamiając następujące polecenie

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Instalowanie dostawcy za pomocą następujących poleceń:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Poczekaj na zakończenie instalacji.

4.  Zarejestrować serwer w magazynu przy użyciu następującego polecenia:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Tworzenie konta magazynu platformy Azure

1. Jeśli nie masz konta usługi Azure miejsca do magazynowania, należy utworzyć konto replikacji geo włączone w tym samym geo jako magazyn, uruchamiając następujące polecenie:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Należy zauważyć, że konto miejsca do magazynowania musi znajdować się w tym samym regionie jako usługa Azure witryny odzyskiwania i zostaną skojarzone z tej samej subskrypcji.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Instalowanie agenta usługi Azure odzyskiwania

1. Agent usługi Azure odzyskiwania na [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) go pobrać i zainstalować na wszystkich serwerach hosta funkcji Hyper-V znajduje się w chmury VMM, którą chcesz chronić.

2. Dla wszystkich hostów VMM, uruchom następujące polecenie:

    /nu/q marsagentinstaller.exe

## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurowanie chmury ustawień ochrony

1.  Tworzenie zasad replikacji Azure, uruchamiając następujące polecenie:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Pobranie kontenera ochrony, uruchamiając następujące polecenia:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Szczegóły zasady zmiennej przy użyciu zadania, który został utworzony i uzyskać tworzenie wzmianki o nazwie przyjazny zasad:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Rozpocząć skojarzenia kontenera ochrony zasady replikacji:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Po zakończeniu zadania, uruchom następujące polecenie:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Po zakończeniu przetwarzania zadania, uruchom następujące polecenie:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Konfigurowanie mapowanie sieci

Przed rozpoczęciem mapowanie sieci upewnij się, że maszyn wirtualnych na serwerze VMM źródłowym jest połączony z siecią maszyn wirtualnych. Ponadto można utworzyć jeden lub więcej Azure wirtualnych sieci.

Dowiedz się więcej o tworzeniu wirtualnej sieci przy użyciu Menedżera zasobów Azure i programu PowerShell, w [Tworzenie wirtualnej sieci się przez połączenie VPN witryny do witryny za pomocą Menedżera zasobów Azure i programu PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Należy zauważyć, że wiele maszyn wirtualnych sieci mogą być mapowane na jednej sieci Azure. Jeśli jeden z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła sieci docelowej zawiera wiele podsieci, następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączaniem. W przypadku nie podsieć docelowej z odpowiednia nazwa maszyny wirtualnej zostanie połączony z pierwszą podsieć w sieci.

1. Pierwsze polecenie pobiera serwery dla bieżącego magazynu Odzyskiwanie witryny Azure. Polecenie zapisuje serwery Odzyskiwanie witryny Microsoft Azure w zmiennej tablicy $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Drugie polecenie otrzymuje sieci odzyskiwania witryny dla pierwszego serwera w tablicy $Servers. Polecenie zapisuje sieci w zmiennej $Networks.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Trzecie polecenie otrzymuje Azure wirtualnych sieci, a następnie tę wartość w zmiennej $AzureVmNetworks.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Polecenia cmdlet wersję ostateczną tworzy mapowanie między sieci podstawowej i sieciami Azure maszyn wirtualnych. Polecenie cmdlet określa sieci podstawowej jako pierwszy element $Networks. Polecenie cmdlet określa maszyn wirtualnych sieci jako pierwszy element $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Włączanie ochrony dla maszyn wirtualnych

Po serwerów, chmury i sieci zostały skonfigurowane poprawnie, można włączyć ochronę maszyn wirtualnych w chmurze.

 Pamiętaj o następujących kwestiach:

 - Maszyn wirtualnych musi spełniać wymagania Azure. Zaznacz te [wymagania wstępne i pomocy technicznej](site-recovery-best-practices.md) w podręczniku planowania.

 - Aby włączyć ochronę, system operacyjny i systemu operacyjnego właściwości dysku przeznaczona dla maszyny wirtualnej. Po utworzeniu maszyny wirtualnej w VMM przy użyciu szablonu maszyn wirtualnych można ustawić właściwość. Można także ustawić następujące właściwości istniejącego maszyn wirtualnych na kartach **Ogólne** i **Konfiguracji sprzętowej** właściwości maszyn wirtualnych. Jeśli nie ustawisz tych właściwości w VMM można skonfigurować je w portalu Odzyskiwanie witryny Azure.


  1. Aby włączyć ochronę, uruchom następujące polecenie, aby uzyskać kontenerze ochrony:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Uzyskiwanie jednostki ochrony (maszyn wirtualnych), uruchamiając następujące polecenie:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Włącz DR dla maszyn wirtualnych, uruchamiając następujące polecenie:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Testowanie wdrożenia

Aby przetestować wdrożenie można uruchomić test przełączaniem dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania składający się z wielu maszyn wirtualnych i uruchomić test przełączaniem planu. Test przełączaniem symuluje z przełączaniem i odzyskiwanie mechanizmu odizolowanych sieci. Należy zauważyć, że:

- Jeśli chcesz połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego po przełączaniem włączyć Podłączanie pulpitu zdalnego maszyny wirtualnej przed uruchomieniem tym przełączeniu test.
- Po przełączaniem które będą używane publiczny adres IP połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego. Jeśli chcesz wykonać tę czynność, upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.

Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).


### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1.  Uruchom tym przełączeniu test, uruchamiając następujące polecenie:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Uruchamianie zaplanowanych trybie awaryjnym

1. Rozpocznij tym przełączeniu planowanej, uruchamiając następujące polecenie:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Uruchamianie nieplanowanego przełączania awaryjnego

1. Uruchom nieplanowanego przełączania awaryjnego, uruchamiając następujące polecenie:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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

[Dowiedz się więcej](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Odzyskiwanie witryny przy użyciu poleceń cmdlet środowiska PowerShell Menedżera zasobów Azure.
