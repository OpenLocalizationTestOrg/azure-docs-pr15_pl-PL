<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM przy użyciu programu PowerShell (Menedżer zasobów) | Microsoft Azure"
    description="W tym artykule opisano jak wdrożyć serwer Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwania maszyny wirtualne funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM przy użyciu programu PowerShell (Menedżer zasobów)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM przy użyciu programu PowerShell (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-vmm.md)
- [Klasyczny portalu](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Odzyskiwanie Azure witryny — Zapraszamy! Ten artykuł, aby odtworzyć funkcji Hyper-V w lokalnym środowisku maszyn wirtualnych systemu zarządzania w chmury System Center Virtual Machine Manager (VMM) do pomocniczej witryny do użycia. 

W tym artykule pokazano, jak za pomocą programu PowerShell Automatyzowanie typowych zadań, które należy wykonać podczas konfigurowania Odzyskiwanie witryny Azure replikacji maszyn wirtualnych funkcji Hyper-V w chmury VMM Centrum systemu chmury VMM Centrum systemu w witrynie pomocniczą.

W tym artykule zawiera wymagania wstępne dotyczące tego scenariusza i przedstawiono 

- Jak skonfigurować magazynu usługi odzyskiwania
- Instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM źródłowym i na serwerze VMM docelowej
- Rejestrowanie serwery VMM w Magazyn
- Konfigurowanie zasad replikacji chmury VMM. Ustawienia replikacji w zasady zostaną zastosowane do wszystkich chronionych maszyn wirtualnych 
- Włącz ochronę dla maszyn wirtualnych. 
- Testowanie awaryjnego przeniesienia maszyny wirtualne pojedynczo lub jako część planu odzyskiwania, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.
- Wykonywanie planowane lub nieplanowanego przełączania awaryjnego maszyny wirtualne pojedynczo lub jako część planu odzyskiwania, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Jeśli wystąpią problemy, aby skonfigurować w tym scenariuszu ogłaszać pytania na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia. W tym artykule opisano, jak model wdrożenia Menedżera zasobów.



## <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

Oto, co będzie potrzebne w witrynach głównego i pomocniczego lokalnego wdrożenia w tym scenariuszu:

**Wymagania wstępne** | **Szczegóły** 
--- | ---
**VMM** | Zalecamy zapoznanie wdrożyć serwer VMM w podstawowej witryny i serwer VMM w witrynie pomocniczej.<br/><br/> Możesz także [replikować między chmury na serwerze VMM](site-recovery-single-vmm.md). W tym celu musisz co najmniej dwa chmury skonfigurowany na serwerze VMM.<br/><br/> Serwery VMM powinna działać co najmniej systemu Centrum 2012 z dodatkiem SP1 z najnowszymi aktualizacjami.<br/><br/> Każdy serwer VMM musi mieć na jednym lub więcej chmury skonfigurowane i wszystkich chmur muszą mieć profil zdolności funkcji Hyper-V, ustaw. <br/><br/>Chmury musi zawierać jedną lub więcej grup hosta VMM.<br/><br/>Dowiedz się więcej o konfigurowaniu chmury VMM [Konfigurowanie tkaninie VMM w chmurze](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [Instruktaż: tworzenie prywatnych chmury za pomocą systemu Centrum 2012 z dodatkiem SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Serwery VMM powinny mieć dostęp do Internetu. 
**Funkcji Hyper-V** | Serwery funkcji Hyper-V musi działać co najmniej systemu Windows Server 2012 z roli Hyper-V i zainstalowano najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V powinien zawierać co najmniej jeden maszyny wirtualne.<br/><br/>  Serwery hosta funkcji Hyper-V powinien znajdować się w grupach hosta w chmury VMM głównego i pomocniczego.<br/><br/> Jeśli używasz funkcji Hyper-V w klastrze w systemie Windows Server 2012 R2 należy zainstalować [aktualizacji 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Jeśli używasz funkcji Hyper-V w klastrze na Uwaga systemu Windows Server 2012 tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. [Dowiedz się więcej](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Dostawcy** | Podczas wdrażania Odzyskiwanie witryny dostawca odzyskiwania witryny Azure zainstalowanie na serwerach VMM. Dostawca komunikuje się z Odzyskiwanie witryny przez 443 HTTPS, aby dodać akompaniament replikacji. Dane replikacji między serwerami funkcji Hyper-V głównego i pomocniczego w sieci LAN lub połączenie VPN.<br/><br/> Dostawca na serwerze VMM musi mieć dostęp do następujących adresów URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Ponadto zezwolić na komunikację zapory z serwerów VMM [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i umożliwić protokołu HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Wymagania wstępne dotyczące sieci mapowania
Sieć mapy mapowania między sieciami maszyn wirtualnych VMM na serwerach VMM głównego i pomocniczego, aby:

- Optymalnie umieść po przełączeniu maszyny wirtualne replice pomocniczej hosts funkcji Hyper-V.
- Połącz replice maszyny wirtualne do odpowiednich maszyn wirtualnych sieci.
- Jeśli nie skonfigurowano sieci mapowanie replice maszyny wirtualne nie można połączyć dowolna sieć, po przełączeniu.
- Jeśli chcesz skonfigurować sieć mapowanie podczas odzyskiwania witryny wdrożenia w tym miejscu jest co będzie potrzebne:

    - Upewnij się, że maszyny wirtualne na serwerze hosta funkcji Hyper-V źródła jest połączony z siecią VMM maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
    - Sprawdź, czy chmurze pomocniczą, która będzie używana dla odzyskiwania ma odpowiednie maszyn wirtualnych sieci skonfigurowanej. Maszyn wirtualnych sieci powinny być połączone z sieci logicznej, który jest skojarzony z chmurą pomocniczej.


Dowiedz się więcej o konfigurowaniu sieci VMM poniżej artykuły

- [Jak skonfigurować sieci logicznych w VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Jak skonfigurować maszyn wirtualnych sieci i bramy w VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Aby uzyskać więcej informacji](site-recovery-network-mapping.md) o sposobie działania mapowanie sieci.

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

1. Tworzenie grupy zasobów Azure Menedżera zasobów, jeśli nie masz już

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Tworzenie nowego magazynu usługi odzyskiwania i zapisywanie utworzony obiekt magazynu automatycznego odzyskiwania systemu w zmiennej (posłuży później). Możesz również pobrać Tworzenie wpisu obiektów magazynu automatycznego odzyskiwania systemu przy użyciu polecenia cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Ustaw kontekst odzyskiwania usług magazynu

1.  Jeśli masz magazynu już utworzony, uruchom poniżej polecenia magazyn.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Ustaw kontekst magazynu, uruchamiając poniżej polecenia.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Krok 5: Tworzenie i kojarzenie zasad replikacji

1.  Tworzenie zasad replikacji funkcji Hyper-V 2012 R2, uruchamiając następujące polecenie:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] W chmurze VMM może zawierać hosts funkcji Hyper-V z różnymi wersjami systemu Windows Server (wymieniony w wymagania wstępne dotyczące funkcji Hyper-V), ale zasady replikacji jest określonej wersji systemu operacyjnego. Jeśli masz różne hosty uruchomionych dla wersji systemów operacyjnych, następnie utwórz zasady osobnych replikacji dla każdego typu wersja systemu operacyjnego. Dla np: Jeśli pięć hosts uruchomionych 2012 serwery systemu Windows i trzy na systemie Windows Server 2012 R2, utworzyć dwa zasady replikacji — jednego dla każdego typu wersji systemu operacyjnego.

2.  Uzyskiwanie podstawowego ochrony kontenerach (podstawowy VMM chmury) i odzyskiwania ochrony (odzyskiwania VMM w chmurze), uruchamiając następujące polecenia:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Pobieranie zasady, które zostało utworzone w kroku 1 przy użyciu przyjazna nazwa zasady

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Rozpocząć skojarzenia kontenera ochrony (VMM chmury) z zasad replikacji:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Poczekaj na ukończenie zadania skojarzenia zasad. Możesz sprawdzić, jeśli zadanie zostało ukończone, za pomocą następujących wstawkę kodu programu PowerShell.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Po zakończeniu przetwarzania zadania, uruchom następujące polecenie:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurowanie mapowanie sieci

1. Pierwsze polecenie pobiera serwery dla bieżącego magazynu Odzyskiwanie witryny Azure. Polecenie zapisuje serwery Odzyskiwanie witryny Microsoft Azure w zmiennej tablicy $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Poniżej poleceń uzyskać sieci odzyskiwania witryny z serwerem źródłowym VMM i serwer VMM docelowy.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Serwer źródłowy VMM może być pierwszy lub drugi w tablicy serwerów. Sprawdzanie nazw serwerów VMM i odpowiednio się sieci


4. Polecenia cmdlet wersję ostateczną tworzy mapowanie między sieci podstawowej i sieciami odzyskiwania. Polecenie cmdlet określa sieci podstawowej jako pierwszy element $PrimaryNetworks oraz sieci odzyskiwania jako pierwszy element $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Krok 6: Skonfiguruj mapowanie miejsca do magazynowania

1. Poniżej polecenia pobiera listę klasyfikacje miejsca do magazynowania w zmiennej $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Poniżej poleceń uzyskiwanie klasyfikacji źródła do zmiennej i docelowej klasyfikacji $SourceClassificaion do zmiennej $TargetClassification. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Klasyfikacje źródłowej i docelowej może być dowolny element w tablicy. Odwołują się do wyników poniżej polecenie, aby rysunek indeks źródłowej i docelowej klasyfikacje w tablicy $storageclassifications. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - FriendlyName właściwość, identyfikator | Formatowanie tabeli


3. Poniżej polecenia cmdlet tworzy mapowanie między klasyfikacji źródła i klasyfikacji docelowej. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Włącz ochronę dla maszyn wirtualnych

Po serwerów, chmury i sieci zostały skonfigurowane poprawnie, można włączyć ochronę maszyn wirtualnych w chmurze. 


  1. Aby włączyć ochronę, uruchom następujące polecenie, aby uzyskać kontenerze ochrony:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Uzyskiwanie jednostki ochrony (maszyn wirtualnych), uruchamiając następujące polecenie:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Włączanie replikacji maszyn wirtualnych, uruchamiając następujące polecenie:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testowanie wdrożenia

Aby przetestować wdrożenie można uruchomić trybie awaryjnym test dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania składający się z wielu maszyn wirtualnych i uruchomić trybie awaryjnym test dla planu. Testowanie pracy awaryjnej symuluje z mechanizmu awaryjnego i odzyskiwania w odizolowanych sieci. 

> [AZURE.NOTE] Możesz utworzyć plan odzyskiwania aplikacji w Azure portal.

Aby sprawdzić na wykonanie operacji, wykonaj czynności opisane w [Monitorowanie aktywności](#monitor).


### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1.  Uruchamianie poniżej polecenia cmdlet, aby uzyskać pośrednictwem usługi SMS do sieci maszyn wirtualnych, do którego chcesz przetestować pracy awaryjnej.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Wykonaj test awaryjnego przeniesienia maszyny, wykonując następujące czynności:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Wykonaj test trybie awaryjnym planu odzyskiwania, wykonując następujące czynności:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Uruchamianie zaplanowanych trybie awaryjnym

1. Wykonywanie planowanych trybie awaryjnym: maszyny, wykonując następujące czynności:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Wykonywanie planowanych trybie awaryjnym planu odzyskiwania, wykonując następujące czynności:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Uruchamianie nieplanowanego przełączania awaryjnego

1. Wykonywanie nieplanowanego przełączania awaryjnego maszyny, wykonując następujące czynności:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. wykonywać nieplanowanego przełączania awaryjnego planu odzyskiwania, wykonując następujące czynności:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

