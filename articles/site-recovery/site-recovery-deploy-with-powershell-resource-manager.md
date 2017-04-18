<properties
    pageTitle="Ochrona serwerów Azure przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure | Microsoft Azure"
    description="Ochrona serwerów Azure z Odzyskiwanie witryny Azure zautomatyzować przy użyciu programu PowerShell i Menedżera zasobów Azure."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikowane między funkcji Hyper-V w lokalnym środowisku maszyn wirtualnych systemu i Azure przy użyciu programu PowerShell i Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell — Menedżera zasobów](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klasyczny portalu](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Omówienie

Azure Odzyskiwanie witryny składa się na strategii odzyskiwania ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych w szeregu wdrożeń. Aby uzyskać pełną listę scenariuszy wdrażania zobacz [Omówienie Azure witryny odzyskiwania](site-recovery-overview.md).

Azure PowerShell to modułu, który zawiera polecenia cmdlet, aby zarządzać Azure za pomocą programu Windows PowerShell. Można pracować z dwa typy modułów: moduł profilu Azure lub moduł Azure Menedżera zasobów.

Witryny odzyskiwania poleceń cmdlet programu PowerShell, dostępne w przypadku Azure programu PowerShell dla Menedżera zasobów Azure pomagają chronić i odzyskiwanie serwery platformy Azure.

W tym artykule opisano, jak za pomocą programu Windows PowerShell, razem z Menedżera zasobów Azure wdrażanie Odzyskiwanie witryny do konfigurowania i dodać akompaniament ochrony server Azure. Przykład przedstawiony w tym artykule pokazano, jak chronić, Niepowodzenie nad i odzyskiwanie maszyn wirtualnych na hoście funkcji Hyper-V Azure, przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure.

> [AZURE.NOTE] Polecenia cmdlet programu PowerShell odzyskiwania witryny umożliwia obecnie skonfigurować następujące elementy: jednej witryny Menedżer maszyn wirtualnych do innego, Menedżer maszyn wirtualnych Azure do witryny funkcji Hyper-V Azure.

Nie musisz być PowerShell ekspertów do korzystania z tego artykułu, ale należy zrozumieć podstawowe pojęcia, takie jak moduły, polecenia cmdlet i sesji. Aby uzyskać więcej informacji na temat programu Windows PowerShell zobacz [Wprowadzenie do programu Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Można również uzyskać więcej informacji o [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Partnerów firmy Microsoft, które są częścią programu chmury rozwiązanie dostawcy (dostawcy) można konfigurować i zarządzać ochrony swoich klientów serwerów do swoich klientów odpowiednich dostawcy subskrypcji (subskrypcje dzierżawy).

## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały spełnione następujące wymagania wstępne w miejscu:

- Konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). Ponadto można przeczytać o [cenach Menedżer odzyskiwania witryny Azure](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure programu PowerShell 1.0. Aby uzyskać informacje o tej wersji i jak ją zainstalować, zobacz [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- Moduły [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) i [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . Możesz wyświetlić najnowsze wersje tych modułów z [galerii programu PowerShell](https://www.powershellgallery.com/)

W tym artykule pokazano, jak Azure za pomocą programu Powershell przy użyciu Menedżera zasobów Azure konfigurowania i zarządzania nimi ochrony serwerów. Przykład przedstawiony w tym artykule pokazano, jak chronić maszyny wirtualnej, uruchamiania na hoście funkcji Hyper-V, aby Azure. W tym przykładzie dotyczą następujące wymagania wstępne. Zestaw bardziej szczegółowe wymagania dotyczące różnych scenariuszach Odzyskiwanie witryny można znaleźć w dokumentacji odnoszących się do tego scenariusza.

- Host funkcji Hyper-V z systemem Windows Server 2012 R2 lub zawierająca jeden lub więcej maszyn wirtualnych systemu Microsoft Hyper-V Server 2012 R2.
- Serwery funkcji Hyper-V jest połączony z Internetem, bezpośrednio lub za pośrednictwem serwera proxy.
- Maszyn wirtualnych, które mają być chronione powinny być zgodne z [wymagania wstępne dotyczące maszyn wirtualnych](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Krok 1: Zaloguj się do konta Azure


1. Otwieranie konsoli programu PowerShell i uruchom to polecenie, aby zalogować się do konta Azure. Polecenie cmdlet spowoduje wyświetlenie strony sieci web, który monituje o podanie poświadczeń konta.

        Login-AzureRmAccount

    Alternatywnie możesz także dołączyć swoje poświadczenia konta jako parametr `Login-AzureRmAccount` polecenia cmdlet, za pomocą `-Credential` parametru.

    Jeśli jesteś partnera dostawcy pracy imieniu dzierżawy, określić klienta jako dzierżawy, przy użyciu ich nazwa domeny podstawowej tenantID lub dzierżawy.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Konta można ma kilka subskrypcje, więc należy skojarzyć z subskrypcją, której chcesz używać z kontem.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Sprawdź, czy Twoja subskrypcja została zarejestrowana do używania Azure dostawców usług odzyskiwania i odzyskiwanie witryny za pomocą następujących poleceń:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    W wynikach te polecenia Jeśli **RegistrationState** jest ustawiona wartość **zarejestrowano**możesz przejść do kroku 2. W przeciwnym razie należy zarejestrować Brak dostawcy w ramach subskrypcji.

    Aby zarejestrować Azure dostawcy usług odzyskiwania i odzyskiwanie witryny, uruchom następujące polecenia:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Sprawdź, czy dostawcy pomyślnie zarejestrowane za pomocą następujących poleceń: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` i `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Krok 2: Konfigurowanie magazynu usługi odzyskiwania

1. Utwórz grupę zasobów Menedżer zasobów Azure, w którym można będzie utworzyć magazyn, lub użyj istniejącej grupy zasobów. Można utworzyć nową grupę zasobów przy użyciu następującego polecenia:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    miejsce, w którym zmiennej $ResourceGroupName zawiera nazwę grupy zasobów, które chcesz utworzyć, a zmienna $Geo Azure region, w której chcesz utworzyć grupę zasobów (na przykład "Brazylia południe").

    Lista grup zasobów w ramach subskrypcji można uzyskać przy użyciu `Get-AzureRmResourceGroup` polecenia cmdlet.

2. Utworzenie nowego magazynu usługi odzyskiwania Azure w następujący sposób:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Można pobrać listę istniejących magazynów przy użyciu `Get-AzureRmRecoveryServicesVault` polecenia cmdlet.

> [AZURE.NOTE] W razie potrzeby do wykonywania operacji na utworzony przy użyciu portalu klasyczny lub modułu programu PowerShell usługi Azure usługi zarządzania magazynami Odzyskiwanie witryny, możesz pobrać listę takich magazynów przy użyciu `Get-AzureRmSiteRecoveryVault` polecenia cmdlet. Czy utworzyć nowy magazynu usługi odzyskiwania dla wszystkich nowych operacji. Z magazynów Odzyskiwanie witryny, utworzony wcześniej są obsługiwane, ale nie masz najnowszych funkcji.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Ustaw kontekst magazynu usługi odzyskiwania

1.  Ustaw kontekst magazynu, uruchamiając następujące polecenie:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Krok 4: Tworzenie witryny funkcji Hyper-V i Generuj nowy klucz rejestru magazynu dla witryny.

1. Tworzenie nowej witryny funkcji Hyper-V w następujący sposób:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    To polecenie cmdlet uruchamia zadanie Odzyskiwanie witryny, aby utworzyć witrynę i zwraca obiekt zadania Odzyskiwanie witryny. Czekaj na zadanie do wykonania i sprawdź, czy zadanie zostało zakończone pomyślnie.

    Można pobrać obiekt zadania i tym samym sprawdzić bieżący stan zadania, za pomocą polecenia cmdlet Get-AzureRmSiteRecoveryJob.
2. Generowanie i Pobierz klucz rejestru dla witryny, w następujący sposób:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopiowanie klucz pobrany hosta funkcji Hyper-V. Potrzebujesz klawisz, aby zarejestrować hosta funkcji Hyper-V do witryny.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Krok 5: Instalowanie Azure Odzyskiwanie witryny dostawca i agenta usługi Azure odzyskiwania na hoście funkcji Hyper-V

1. Pobierz instalatora dla najnowszej wersji dostawcy firmy [Microsoft](https://aka.ms/downloaddra).

2. Uruchom Instalatora pakietu na hoście funkcji Hyper-V, a na koniec instalacji przejdź do kroku rejestracji.

3. Po wyświetleniu monitu podaj klucz rejestru pobrany witryny i zakończenie rejestracji hosta funkcji Hyper-V do witryny.

4. Sprawdź, czy host funkcji Hyper-V jest zarejestrowany do witryny za pomocą następującego polecenia:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Krok 6: Tworzenie zasad replikacji i kojarzenie go z tym kontenerem ochrony

1. Tworzenie zasad replikacji w następujący sposób:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Sprawdź zwracane zadania, aby upewnić się, że tworzenie zasad replikacji zakończyło się powodzeniem.

    >[AZURE.IMPORTANT] Konta miejsca do magazynowania określonego powinny znajdować się w tym samym regionie Azure jako do magazynu usługi odzyskiwania i powinien mieć replikacji geo włączone.
    >
    > - Jeśli określone konto miejsca do magazynowania odzyskiwania jest typu Magazyn Azure (klasyczny), awaryjnego przeniesienia chronionego maszyn odzyskać komputer Azure IaaS (klasyczny).
    > - Jeśli określone konto miejsca do magazynowania odzyskiwania jest typu Magazyn Azure (Menedżer zasobów Azure), awaryjnego przeniesienia chronionego maszyn odzyskać komputer Azure IaaS (Menedżer zasobów Azure).

2. Uzyskiwanie kontenerze ochrony odpowiadające do witryny, w następujący sposób:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Rozpocząć skojarzenia kontenera ochrony z zasad replikacji w następujący sposób:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Czekaj na ukończenie zadania skojarzenia i upewnij się, że ukończona pomyślnie.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Włącz ochronę dla maszyn wirtualnych

1. Uzyskiwanie jednostki ochrony odpowiadające maszyn wirtualnych, który chcesz chronić, w następujący sposób:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Rozpocznij ochrona maszyny wirtualnej w następujący sposób:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Konta miejsca do magazynowania określonego powinny znajdować się w tym samym regionie Azure jako do magazynu usługi odzyskiwania i powinien mieć replikacji geo włączone.
    >
    > - Jeśli określone konto miejsca do magazynowania odzyskiwania jest typu Magazyn Azure (klasyczny), awaryjnego przeniesienia chronionego maszyn odzyskać komputer Azure IaaS (klasyczny).
    > - Jeśli określone konto miejsca do magazynowania odzyskiwania jest typu Magazyn Azure (Menedżer zasobów Azure), awaryjnego przeniesienia chronionego maszyn odzyskać komputer Azure IaaS (Menedżer zasobów Azure).

    > Jeśli maszyn wirtualnych, są ochrona zawiera więcej niż jeden dysk dołączony do niego, określ dysku systemu operacyjnego przy użyciu parametru *OSDiskName* .

3. Poczekaj, aż maszyn wirtualnych do osiągnięcia obecnym stanie po początkowej replikacji. Może to zająć trochę czasu, w zależności od czynników, takich jak ilości danych, które powinny być replikowane i przepustowość nadrzędny Azure. Stan zadania oraz StateDescription zostaną zaktualizowane w następujący sposób na maszyn wirtualnych, osiągnięcia obecnym stanie.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Aktualizowanie właściwości odzyskiwania, takie jak rozmiar roli maszyn wirtualnych i Azure sieci, aby dołączyć maszyny wirtualnej karty sieciowe do po pracy awaryjnej.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Uruchom w trybie awaryjnym test

1. Uruchom zadanie pracy awaryjnej testowych w następujący sposób:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Upewnij się, że test maszyn wirtualnych, zostanie utworzona w Azure. (Zadania testowego pracy awaryjnej zostało zawieszone, po utworzeniu test maszyn wirtualnych w Azure. Zadanie kończy czyszcząc wynikające utworzonych po wznawianie zadania, jak pokazano w następnym kroku.)

3. Tym przełączeniu test, należy wykonać w następujący sposób:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Odzyskiwanie witryny przy użyciu poleceń cmdlet środowiska PowerShell Menedżera zasobów Azure.
