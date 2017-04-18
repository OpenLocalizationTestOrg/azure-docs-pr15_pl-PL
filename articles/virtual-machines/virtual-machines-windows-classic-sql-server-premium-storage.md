<properties
    pageTitle="Magazyn Azure Premium za pomocą programu SQL Server | Microsoft Azure"
    description="W tym artykule została użyta zasobów utworzone za pomocą modelu Klasyczny wdrożenia i udostępnia wskazówki na temat korzystania z programem SQL Server uruchomiony na maszyn wirtualnych Azure magazyn Premium Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Magazyn Azure Premium za pomocą programu SQL Server w środowisku maszyn wirtualnych systemu


## <a name="overview"></a>Omówienie

[Magazyn Premium Azure](../storage/storage-premium-storage.md) jest generacji magazyn, które zawiera krótki czas oczekiwania i wysokiej wydajności Jo. Działa najlepszy dla klucza Jo intensywnie obciążeń pracą, takich jak program SQL Server na IaaS [maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ten artykuł zawiera wskazówki dotyczące migrowania maszyny wirtualnej uruchomiony program SQL Server korzystania z magazynu Premium i planowania. Ta opcja uwzględnia Azure infrastruktury (sieci, Magazyn) i gościa czynności maszyn wirtualnych systemu Windows. W [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) przykładzie pełnego pełna migracji kompleksowego, jak przenieść większe maszyny wirtualne skorzystać ulepszone lokalne przechowywanie SSD przy użyciu programu PowerShell.

Należy zrozumieć proces zakończenia do końca medyczne przestrzeni dyskowej Premium Azure z programem SQL Server na IAAS maszyny wirtualne. Ta opcja uwzględnia:

- Identyfikator wymagania wstępne dotyczące korzystania z magazynu Premium.
- Przykłady wdrażania programu SQL Server na IaaS Premium miejsca do magazynowania dla nowych wdrożeń.
- Przykłady Migrowanie istniejącej wdrożeń autonomiczne serwery i wdrożenia przy użyciu SQL zawsze na dostępność grup.
- Metod możliwej migracji.
- Przykład pełnego zakończenia do końca pokazujący Azure, systemu Windows i programu SQL Server instrukcje dotyczące migrację istniejącej implementacji zawsze włączone.

Aby uzyskać więcej informacji o tła w programie SQL Server w maszyn wirtualnych Azure zobacz [Programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** Sol Krzysztof **recenzentów techniczne:** Śledź Vargas Artur Luis Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Wymagania wstępne dotyczące miejsca do magazynowania Premium

Istnieje kilka wymagania wstępne dotyczące korzystania z miejsca do magazynowania Premium.

### <a name="machine-size"></a>Rozmiar komputera

Dla magazynu Premium potrzebujesz za pomocą serii Zasadami maszyn wirtualnych (maszyn wirtualnych). Jeśli nie zastosowano maszyn serii DS w Twojej usłudze w chmurze przed, należy usunąć istniejące maszyn wirtualnych, Zachowaj dysków dołączonych i utworzenie nowej usługi w chmurze przed odtworzenia maszyn wirtualnych jako rozmiar roli Zasadami *. Aby uzyskać więcej informacji na temat rozmiarów maszyn wirtualnych zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Usług w chmurze

Zasadami * maszyny wirtualne można używać z nośnikami Premium tylko, tworząc w nowej usługi w chmurze. Jeśli korzystasz z programu SQL Server zawsze na platformy Azure zawsze na odbiornika będzie dotyczyć adres Azure wewnętrznych lub zewnętrznych IP usługi równoważenia obciążenia, który jest skojarzony z usługi w chmurze. W tym artykule omówiono sposób migracji przy zachowaniu dostępność w tym scenariuszu.

> [AZURE.NOTE] Seria Zasadami * musi być pierwszym maszyn wirtualnych, który jest wdrożony w nowej usługi w chmurze.

### <a name="regional-vnets"></a>VNETS regionalnych

Dla Zasadami * maszyny wirtualne należy skonfigurować wirtualna sieć (VNET) hostingu swojej maszyny wirtualne należy regionalne. To "rozszerzenie" VNET jest umożliwienie większe maszyny wirtualne można obsługi administracyjnej w innych klastrów i zezwolić na komunikację między nimi. W następujących zrzut ekranu wyróżnionej lokalizacji pokazuje regionalne VNETs pierwszego wyniku pokazuje VNET "wąskie".

![RegionalVNET][1]

Można podnieść bilet pomocy technicznej firmy Microsoft, aby przeprowadzić migrację do regionalnych VNET, Microsoft zostanie zmiany, a następnie przeprowadzić migrację do regionalnych VNETs zmienić właściwość AffinityGroup w konfiguracji sieci. Najpierw wyeksportować konfiguracji sieci w programie PowerShell, a następnie zastąpić właściwości **AffinityGroup** w elemencie **VirtualNetworkSite** właściwości **lokalizacji** . Określanie `Location = XXXX` miejsce, w którym `XXXX` jest Azure region. Następnie zaimportować nowej konfiguracji.

Na przykład uwzględniając następujące VNET:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Aby przenieść to regionalne VNET w Europie Zachód, należy zmienić konfigurację do następującego:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Konta miejsca do magazynowania

Konieczne będzie utworzenie nowego konta miejsca do magazynowania, który jest skonfigurowany do przechowywania Premium. Należy zauważyć, że wykorzystanie miejsca do magazynowania Premium jest na koncie miejsca do magazynowania, nie ma na poszczególnych wirtualnych dysków twardych, jednak podczas korzystania z maszyn wirtualnych serii Zasadami * z kont Premium i standardowego magazynu można dołączać wirtualnego na dysku twardego. Jeśli nie chcesz umieścić wirtualny dysk twardy OS na koncie magazynowania Premium można to rozważyć.

Następujące polecenie **Nowy AzureStorageAccountPowerShell** "Premium_LRS" **Typ** tworzy konto miejsca do magazynowania Premium:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Ustawienia pamięci podręcznej wirtualnych dysków twardych

Główna różnica pomiędzy tworzenia dysków, które są częścią konta magazynu Premium to ustawienie pamięci podręcznej dysku. W przypadku dysków głośność danych programu SQL Server zaleca się używać "**Buforowanie odczytu**". Dla liczby dziennik transakcji ustawienie buforowania dysku powinna być równa "**Brak**". To różni się od zalecenia dotyczące konta standardowego magazynu.

Po podłączeniu wirtualnych dysków twardych nie można zmienić ustawień pamięci podręcznej. Należy odłączyć i ponownie dołączyć wirtualny dysk twardy z ustawieniem zaktualizowane pamięci podręcznej.

### <a name="windows-storage-spaces"></a>Spacje miejsca do magazynowania systemu Windows

Można używać [Spacji miejsca do magazynowania systemu Windows](https://technet.microsoft.com/library/hh831739.aspx) , jak w przypadku poprzedniego standardowego magazynu, to będzie można przeprowadzić migrację maszyn wirtualnych, który jest już wykorzystanie miejsca do magazynowania spacje. W przykładzie w [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (krok 9 i przesyłanie dalej) pokazano kod programu Powershell, aby wyodrębnić i importowanie maszyn wirtualnych z wielu dołączony wirtualnych dysków twardych.

Puli miejsca do magazynowania były używane z konta miejsca do magazynowania standardowy Azure Aby zwiększyć wydajność i zmniejszyć opóźnienie. Podczas testowania puli miejsca do magazynowania z Premium miejsca do magazynowania dla nowych wdrożeń mogą okazać się wartość, ale ich dodać dodatkowe złożoność instalacji magazynu.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Jak znaleźć która mapa dysków wirtualnych Azure pul miejsca do magazynowania

Są różne pamięci podręcznej ustawienie zalecenia dotyczące dołączony wirtualnych dysków twardych, można zdecydować skopiować pliki VHD konto Premium miejsca do magazynowania. Jednak podczas podłącz je nowa seria Zasadami maszyn wirtualnych może być konieczne do zmiany ustawień pamięci podręcznej. Jest prostsze może zastosować magazynu Premium zalecanych ustawień pamięci podręcznej, gdy masz osobne wirtualnych dysków twardych dla plików danych programu SQL i plików dziennika (zamiast pojedynczego wirtualny dysk twardy, który zawiera zarówno).

> [AZURE.NOTE] Jeśli masz plików danych i dziennika programu SQL Server na tym samym głośność, wybrana opcja buforowania zależy od deseni dostępu Jo dla swojego obciążenia bazy danych. Testowanie tylko wykaże wybranej opcji buforowania jest zalecane dla tego scenariusza.

Jeśli korzystasz z systemu Windows spacje miejsca do magazynowania, które składają się z wielu wirtualnych dysków twardych należy przeglądać oryginalny skryptów do identyfikowania, której dołączona wirtualnych dysków twardych są jednak w określonej puli, więc można ustawić ustawienia pamięci podręcznej w związku z tym dla każdego dysku.

Jeśli nie masz oryginalny skrypt można wyświetlić, które wirtualnych dysków twardych mapowanie puli miejsca do magazynowania, można określić mapowanie puli miejsca i za pomocą następujące czynności.

Dla każdego dysku wykonaj następujące czynności:

1. Uzyskiwanie listy dysków dołączonych do maszyn wirtualnych przy użyciu polecenia **Get-AzureVM** :

    Get AzureVM - NazwaUsługi <servicename> -nazwa <vmname> | Get-AzureDataDisk

1. Uwaga Diskname i LUN.

    ![DisknameAndLUN][2]

1. Pulpit zdalny do maszyn wirtualnych. Następnie przejdź do sekcji **Zarządzanie komputerem** | **Menedżer urządzeń** | **stacji dysków**. Przyjrzyj się właściwości każdego "Dysków wirtualnych Microsoft"

    ![VirtualDiskProperties][3]

1. Tutaj numer LUN jest odwołaniem do liczby LUN zadanej podczas dołączania do maszyn wirtualnych wirtualnego dysku twardego.
1. Dla "Dysku wirtualnego Microsoft" Przejdź na kartę **Szczegóły** , następnie na liście **Właściwości** , przejdź do **Klucza sterownika**. W polu **wartość**Uwaga **Przesunięcie**, czyli 0002 w następujących zrzut ekranu. 0002 oznacza PhysicalDisk2, która odwołuje się puli miejsca do magazynowania.

    ![VirtualDiskPropertyDetails][4]

2. Dla każdej puli miejsca do magazynowania zrzutu się skojarzone dysków:

    Get StoragePool - FriendlyName AMS1pooldata | Dysk fizyczny Get

    ![GetStoragePool][5]

Teraz możesz użyć tych informacji w celu skojarzenia powiązana wirtualnych dysków twardych dysków fizycznych w puli miejsca do magazynowania.

Po obecnie wirtualnych dysków twardych dysków fizycznych w puli miejsca do magazynowania możesz następnie Odłączanie i kopiowane do konta Premium miejsca do magazynowania, a następnie dołączyć je z ustawieniem poprawne pamięci podręcznej. Zobacz przykład w [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)kroki od 8 do 12. Poniższe kroki przedstawiają jak wyodrębnić konfigurację dysku wirtualnego dysku twardego dołączone maszyn wirtualnych do pliku CSV, skopiuj wirtualnych dysków twardych, zmieniać ustawienia pamięci podręcznej konfiguracji dysku i na końcu ponownie wdróż maszyn wirtualnych jako serię Zasadami maszyn wirtualnych im wszystkich dołączonych dysków.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Przepustowości miejsca do magazynowania maszyn wirtualnych i wydajności magazynu wirtualnego dysku twardego

Ilość miejsca do magazynowania wydajność zależy od określony rozmiar Zasadami * maszyn wirtualnych i rozmiarów wirtualnego dysku twardego. Maszyny wirtualne mają różne dodatków dla liczby wirtualnych dysków twardych, które można dołączać i maksymalna przepustowość obsługują one (MB/s). Dla liczb określonych przepustowości zobacz [maszyn wirtualnych i rozmiarów chmury usługi Azure](virtual-machines-linux-sizes.md).

Zwiększona operacji i/o na SEKUNDĘ zostaną osiągnięte o dużym rozmiarze dysku. To należy rozważyć, gdy sądzisz o ścieżce migracji. Aby uzyskać szczegóły [można znaleźć w tabeli typy dysku i operacji i/o na SEKUNDĘ](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Ponadto należy rozważyć, że maszyny wirtualne przepustowości innym dysku maksymalną, będzie obsługiwanych dla wszystkich dysków dołączony. Dużym obciążeniu może nasycenia przepustowości dysku maksymalna liczba dostępnych dla tego rozmiaru roli maszyn wirtualnych. Na przykład Standard_DS14 będzie obsługiwać maksymalnie 512 MB/s; w związku z tym z trzech dysków P30 może nasycenia przepustowości dysku maszyn wirtualnych. Jednak w tym przykładzie limit przepustowości może zostać przekroczona w zależności od kombinacji odczytu i zapisu IOs.

## <a name="new-deployments"></a>Nowe wdrożenia

Następnych dwóch sekcjach pokazano, jak można wdrażać maszyny wirtualne programu SQL Server do magazynu Premium. Wymienione przed nie niekoniecznie należy umieścić dysku systemu operacyjnego na Premium miejsca do magazynowania. Można to zrobić, jeśli mają zamiar umieścić dowolny intensywnie obciążenia Jo na OS wirtualnego dysku twardego.

W pierwszym przykładzie pokazano, wykorzystując istniejących Azure galerii obrazów. Drugi przykład pokazano sposób Użyj obrazu niestandardowego maszyn wirtualnych, zainstalowanej w istniejącego konta standardowego magazynu.

> [AZURE.NOTE] W tych przykładach założono, że zostały już utworzone regionalnych VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Tworzenie nowych maszyn wirtualnych z magazynu Premium z obrazu z galerii

W poniższym przykładzie pokazano sposób umieścić wirtualny dysk twardy OS na magazynowania premium i dołączyć wirtualnych dysków twardych Premium miejsca do magazynowania. Można jednak także umieścić na dysku z systemem operacyjnym z konta standardowego magazynu, a następnie dołączyć wirtualnych dysków twardych znajdujących się na koncie Premium miejsca do magazynowania. Przedstawiono obu scenariuszach.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Krok 1: Utwórz konto miejsca do magazynowania Premium


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Krok 2: Tworzenie nowej usługi w chmurze

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Krok 3: Rezerwowanie VIP usługi Cloud (opcjonalnie)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Krok 4: Tworzenie kontenera maszyn wirtualnych
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Krok 5: Wprowadzanie OS wirtualnego dysku twardego na standardowy lub miejsca do magazynowania Premium
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Krok 6: Tworzenie maszyn wirtualnych
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Tworzenie nowych maszyn wirtualnych do przechowywania Premium za pomocą obraz niestandardowy

Ten scenariusz pokazuje, gdzie mają istniejących niestandardowych obrazów znajdujących się na koncie standardowego magazynu. Jak już wspomniano, jeśli chcesz umieścić wirtualny dysk twardy OS ilość miejsca do magazynowania Premium będzie konieczne skopiuj obraz, który istnieje w oknie konta standardowego magazynu i przenosi je do magazynowania Premium, zanim będzie można go używać. Jeśli masz obrazu lokalnego umożliwia także tej metody można użyć do skopiowania który bezpośrednio na koncie Premium miejsca do magazynowania.

#### <a name="step-1-create-storage-account"></a>Krok 1: Tworzenie konta miejsca do magazynowania
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Krok 2 usługi w chmurze
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Krok 3: Użyj istniejącego obrazu
Możesz użyć istniejącego obrazu. Można też [wykonać obraz istniejącego komputera](virtual-machines-windows-classic-capture-image.md). Uwaga komputera możesz obraz nie musi być Zasadami* komputera. Po umieszczeniu obrazu, poniższe kroki pokazują sposób skopiować go do konta Premium miejsca do magazynowania z * *Start AzureStorageBlobCopy** polecenia programu PowerShell.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Krok 4: Kopiowanie Blob między kontami miejsca do magazynowania
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Krok 5: Regularnie sprawdzić stan kopii:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Krok 6: Dodaj dysk z obrazami na dysku Azure repozytorium subskrypcji
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Może się okazać, że mimo że odpowiednie raporty o stanie jako sukcesu, można nadal uzyskać, błąd dzierżawy dysku. W tym przypadku poczekaj około 10 minut.

#### <a name="step-7--build-the-vm"></a>Krok 7: Tworzenie maszyn wirtualnych
W tym miejscu tworzysz maszyn wirtualnych z obrazu i dołączanie wirtualnych dysków twardych dwóch Premium miejsca do magazynowania:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Istniejące wdrożonych nie należy używać zawsze na grupy dostępności

> [AZURE.NOTE] W przypadku istniejących wdrożeń najpierw zobacz sekcję [wymagania wstępne dotyczące](#prerequisites-for-premium-storage) tego tematu.

Istnieją różne aspekty w przypadku wdrożeń programu SQL Server, których nie należy używać zawsze na grupy dostępności i tych, które wykonaj. Jeśli nie jest używany zawsze włączone i masz istniejące autonomicznego programu SQL Server, można też uaktualnić do magazynowania Premium przy użyciu nowego konta usługi i miejsca do magazynowania chmury. Należy rozważyć następujące opcje:

- **Tworzenie nowych maszyn wirtualnych serwera SQL**. Możesz utworzyć nowy maszyny serwera SQL korzystającego z konta Premium miejsca do magazynowania, opisane w nowych wdrożeniach. Następnie kopia zapasowa i przywracanie bazy danych konfiguracji i użytkownika programu SQL Server. Aplikacja muszą zostać zaktualizowane, aby odwołać nowego programu SQL Server, jeśli jest on dostępny wewnętrznie lub zewnętrznie. Należy skopiować wszystkie obiekty "poza db" tak, jakby były wykonanie migracji programu SQL Server (udostępniania) obok siebie. Ta opcja uwzględnia obiekty, takie jak logowania, certyfikaty i serwerów połączonych.
- **Migrowanie istniejącej maszyn wirtualnych serwera SQL**. Wymaga to przełączanie maszyn wirtualnych serwera SQL do trybu offline, a następnie przenoszenia go do nowej usługi w chmurze, które zawierają skopiowanie wszystkich jej dołączony wirtualnych dysków twardych z tym kontem Premium miejsca do magazynowania. Gdy maszyn wirtualnych do trybu online, aplikacja będzie odwoływać się do nazwy hosta serwera jako przed. Należy pamiętać, że rozmiar istniejącego dysku wpłynie na parametrów. Na przykład dysk 400 GB jest zaokrąglana w górę do P20. Jeśli wiadomo, że nie wymagają tego wydajności dysku, a następnie można odtworzyć maszyn wirtualnych jako maszyny serii Zasadami i dołączyć Premium miejsca do magazynowania wirtualnych dysków twardych specyfikacji rozmiaru i wydajności, który jest wymagane. Następnie można odłączyć i ponownie dołączyć pliki bazy danych SQL.

> [AZURE.NOTE] Kopiowanie dysków wirtualny dysk twardy, należy pamiętać o rozmiarze w zależności od rozmiaru będzie oznaczała rodzaju dysku magazynu Premium objęte są, określa Specyfikacja wydajności dysku. Azure będzie zaokrąglanie do najbliższej dysku rozmiaru, jeśli masz dyskiem 400 GB, to zostanie zaokrąglona do P20. W zależności od istniejącego wymagań Jo wirtualnego dysku twardego systemu operacyjnego mogą nie musisz przeprowadzić migrację to konto Premium miejsca do magazynowania.

Jeśli program SQL Server jest dostępny zewnętrznie, VIP usługi cloud ulegnie zmianie. Będzie również musisz punkty końcowe aktualizacji, ACL i DNS ustawienia.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Istniejące wdrożonych zawsze na grupy dostępności

> [AZURE.NOTE] W przypadku istniejących wdrożeń najpierw zobacz sekcję [wymagania wstępne dotyczące](#prerequisites-for-premium-storage) tego tematu.

Początkowo w tej sekcji, firma Microsoft będzie wyglądać na zawsze na interakcji z sieci Azure. Firma Microsoft zostanie następnie Podziel migracji w celu dwa scenariusze: migracji miejsce, w którym można dopuszczalne niektórych przestoje i miejsce, w którym należy osiągnięcia minimalnego przestoje podczas migracji.

Lokalnego programu SQL Server zawsze na grupy dostępności za pomocą odbiornika lokalnego które rejestruje wirtualnych nazwę DNS oraz adres IP są udostępniane między co najmniej jednego serwera SQL. Gdy klienci łączą routingu IP odbiornika do podstawowego programu SQL Server. To jest serwera, który jest właścicielem zasobu zawsze na IP w tym czasie.

![DeploymentsUseAlways na][6]

Platformy Microsoft Azure może zawierać tylko jeden adres IP przypisane do karty Sieciowej w maszyn wirtualnych tak aby osiągnąć tej samej warstwie abstrakcji jako lokalnego, Azure wykorzystuje adres IP, który jest przypisana do wewnętrznego/zewnętrznego urządzenia do równoważenia obciążenia (ILB-ELB). Zasób IP, które mają być udostępniane między serwerami jest ustawiona na tym samym IP jako ILB-ELB. To jest opublikowany w systemie DNS, a ruch klientów jest przekazywany za pośrednictwem ILB-ELB replice podstawowego programu SQL Server. ILB-ELB wie, która programu SQL Server jest podstawowy, ponieważ użyto sondy do sondy zasobu zawsze na IP. W poprzednim przykładzie go sondy węzeł każdego punktu końcowego odwołuje się ELB-ILB, zależnie od odpowiedzi jest podstawowy programu SQL Server.

> [AZURE.NOTE] ILB i ELB są przypisane do usługi w chmurze Azure określonego, dlatego każdej migracji chmury platformy Azure prawdopodobnie oznacza zmieni IP usługi równoważenia obciążenia.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrowanie zawsze na wdrożeń umożliwiające niektórych przestoje

Istnieją dwie strategie przeprowadzić migrację wdrożeń zawsze włączone, zezwalające na niektórych przestojów:

1. **Dodawanie więcej repliki pomocniczej z istniejącym zawsze na klastrem**
1. **Migrowanie do nowego zawsze w klastrze**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Dodaj więcej repliki pomocniczej z istniejącym zawsze na klastrem

Jedną ze strategii jest dodanie więcej pomocnicze zawsze na dostępność grupy. Należy je w nowej usługi w chmurze dodawania i aktualizowania odbiornika z nowym od usługi równoważenia obciążenia.

##### <a name="points-of-downtime"></a>Punkty przestoje:

- Sprawdzanie poprawności klaster.
- Test praca awaryjna zawsze na nowe pomocnicze.

Jeśli korzystasz z systemu Windows puli miejsca do magazynowania w ramach maszyn wirtualnych dla wyższej przepustowości Jo, następnie tych zostanie pobrana offline podczas pełnego poprawności klaster. Po dodaniu węzły z klastrem wymagane jest test sprawdzania poprawności. Czas potrzebny na testu może być różna, więc należy przetestować tę w środowisku testowym przedstawiciela, aby uzyskać przybliżonego czasu jak długo potrwa.

Należy zapewnić czas miejsce, w którym można wykonywać ręczne awaryjnego i chaos badania na nowo dodanym węzły zapewnienie zawsze na wysoką dostępność funkcji zgodnie z oczekiwaniami.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Należy zatrzymać wszystkie wystąpienia programu SQL Server, w których są używane puli miejsca do magazynowania przed uruchomieniem sprawdzania poprawności.
##### <a name="high-level-steps"></a>Czynności wysokiego poziomu

1. Utwórz dwa nowe serwery SQL w nowej usługi w chmurze z masowej Premium.
1. Skopiuj na pełne kopie zapasowe i Przywróć bez **odzyskiwania**.
1. Kopiowanie przez "poza użytkownik DB" zależne obiekty, takie jak logowania itp.
1. Umożliwia utworzenie nowej wewnętrznych ładowania równoważenia (ILB) lub za pomocą zewnętrznej usługi równoważenia obciążenia (ELB), a następnie skonfiguruj równoważenia obciążenia w punkty końcowe w obu nowych węzłów.
> [AZURE.NOTE] Sprawdź, czy wszystkie węzły mają poprawnej konfiguracji punktu końcowego, przed kontynuowaniem

1. Zatrzymywanie użytkownika/aplikacji dostępu do programu SQL Server (Jeśli za pomocą pul miejsca do magazynowania).
1. Zatrzymywanie usług SQL Server aparat we wszystkich węzłach (Jeśli za pomocą pul miejsca do magazynowania).
1. Dodawanie nowych węzłów na klaster i uruchamianie pełnego sprawdzania poprawności.
1. Po weryfikacji zakończyło się pomyślnie, uruchom wszystkich usług SQL Server Services.
1. Wykonaj kopię zapasową dzienników transakcji i przywracanie baz danych użytkowników.
1. Dodawanie nowych węzłów do zawsze na dostępność grupy i umieść replikacji do **synchroniczne**.
1. Dodawanie zasobu adresu IP z nowego chmury usługi ILB-ELB przy użyciu programu PowerShell dla zawsze na przykład wielu elementów witryny w [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage). W systemie Windows klaster, ustaw **możliwych właścicieli** zasobu **Adres IP** nowe węzły stare. Zobacz sekcję "Dodawanie zasobu adresu IP w tej samej podsieci" [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Awaryjnego do jednego z nowych węzłów.
1. Wprowadź nowe węzły praca awaryjna automatycznej pracy awaryjnej partnerów i test.
1. Usuń oryginalny węzły z grupy dostępności.

##### <a name="advantages"></a>Zalety

- Nowe serwery SQL może być sprawdzany (program SQL Server i aplikacja), zanim zostaną one dodane do zawsze włączone.
- Można zmienić rozmiar pamięci Wirtualnej i dostosowywanie do własnych potrzeb dokładnie przechowywania. Jednak może być korzystne zachować wszystkie ścieżki pliku SQL, bez zmian.
- Możesz sterować uruchomienia transferu do pomocniczej replik kopie zapasowe bazy danych. To różni się od kopiowanie za pomocą polecenia Azure **Start AzureStorageBlobCopy** wirtualnych dysków twardych, ponieważ jest to kopia asynchroniczne.

##### <a name="disadvantages"></a>Wady
- Podczas korzystania z systemu Windows puli miejsca do magazynowania, istnieje klaster przestoje podczas pełnego sprawdzania poprawności klaster dla nowych węzłów dodatkowe.
- W zależności od wersję programu SQL Server i istniejący numer replik pomocniczym nie można dodać więcej repliki pomocniczą bez usuwania istniejącej pomocnicze.
- Może być dużo czasu transfer danych SQL podczas konfigurowania pomocnicze.
- Istnieje dodatkowe koszty podczas migracji, gdy masz nowe komputerów z systemem równolegle.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. migrację do nowego zawsze w klastrze

Kolejną metodą jest tworzenie nowym zawsze w klastrze z nowym węzły w nowej usługi w chmurze i Przekierowanie klientom z niej korzystać.

##### <a name="points-of-downtime"></a>Punkty przestoje

Istnieje przestoje podczas przenoszenia aplikacji i użytkowników do nowego odbiornika zawsze włączone. Przestoje zależy od:

- Aby przywrócić kopie zapasowe dziennika transakcji ostateczne baz danych na serwerach nowy czas.
- Aby zaktualizować aplikacjach klienckich umożliwia nowy odbiornik zawsze na czas.

##### <a name="advantages"></a>Zalety

- Istnieje możliwość przetestowania środowisku produkcyjnym rzeczywiste, SQL Server i zmiany kompilacji systemu operacyjnego.
- Istnieje możliwość dostosowywania przechowywania i potencjalnie zmniejszyć rozmiar maszyn wirtualnych. Może to spowodować obniżanie kosztów.
- W trakcie tego procesu, możesz zaktualizować Twoja kompilacji programu SQL Server lub wersja. Można też uaktualnić System operacyjny.
- Poprzedni zawsze na klaster może działać jako docelowej wycofywania pełne.

##### <a name="disadvantages"></a>Wady

- Należy zmienić nazwę DNS nasłuchującego, jeśli chcesz klastrów działających jednocześnie w obu zawsze na. Spowoduje to dodanie Administracja obciążenie podczas migracji jako ciągi aplikacji klienta musi odpowiadać nową nazwę odbiornika.
- Należy zaimplementować mechanizmu synchronizacji między dwoma środowiskami je jak najbliżej zminimalizować wymagania ostateczne synchronizacji przed migracją.
- Zostanie dodany koszt podczas migracji gdy masz nowe środowisko uruchamiania.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrowanie zawsze na wdrożeń przestojów minimalnego

Istnieją dwie strategie dotyczące Migrowanie wdrożeń zawsze na przestojów minimalnego:

1. **Wykorzystanie istniejących pomocniczego: pojedynczej witryny**
1. **Wykorzystanie istniejących Replica(s) pomocnicza: wiele witryn**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. wykorzystanie istniejących pomocniczego: pojedynczej witryny

Jeden strategia minimalnego przestojów jest zrobić istniejących chmury pomocniczej i usunąć go z bieżącym usługi w chmurze. Następnie skopiuj wirtualnych dysków twardych do nowego konta magazynu Premium i Tworzenie maszyn wirtualnych w nowej usługi w chmurze. Następnie zaktualizuj odbiornika w klaster i pracy awaryjnej.

##### <a name="points-of-downtime"></a>Punkty przestoje

- Po zaktualizowaniu ostatni węzeł z punktem końcowym równoważenia obciążenia jest przestoje.
- Ponowne swojego klienta może się wydłużyć w zależności od konfiguracji klienta/DNS.
- Jeśli chcesz wykonać grupę zawsze na klaster w trybie offline, aby zmienić adresów IP jest dodatkowe przestoje. Można tego uniknąć, przy użyciu lub zależności i możliwych właścicieli dodano zasobu adres IP. Zobacz sekcję "Dodawanie zasobu adresu IP w tej samej podsieci" [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Węzeł dodane do partake w partnerem zawsze na pracy awaryjnej, należy należy dodać punkt końcowy Azure z odwołaniem do ładowania strategie zestawu. Po uruchomieniu polecenia **Dodaj AzureEndpoint** w tym celu bieżące połączenia pozostać otwarte, ale nowego połączenia odbiornika nie będzie mógł zostać ustanowione, dopóki zaktualizowane równoważenia obciążenia. Podczas testowania to było widoczne do ostatniego 120seconds 90, to należy przetestować.

##### <a name="advantages"></a>Zalety

- Bez dodatkowych kosztów poniesionych podczas migracji.
- Jeden migracji.
- Złożoności.
- Pozwala na lepszą operacji i/o na SEKUNDĘ przez wersji Premium miejsca do magazynowania produktów. Gdy dyski od maszyn wirtualnych skopiowane do nowej usługi w chmurze firmy 3rd narzędzie można zwiększyć rozmiar wirtualny dysk twardy, który zapewnia wyższą przepustowość. Zwiększenia rozmiarów wirtualny dysk twardy, zobacz [forum dyskusji](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Wady

- Istnieje Tymczasowa utrata HA i DR podczas migracji.
- Tak jak to migracji 1:1, konieczne będzie używanie minimalny rozmiar pamięci Wirtualnej, obsługujące numeru wirtualnych dysków twardych, więc może być możliwe downsize pośrednictwem usługi SMS.
- W tym scenariuszu należy użyć polecenia Azure **AzureStorageBlobCopy rozpoczęcia** , która jest asynchroniczne. Po zakończeniu kopiowania jest nie Umowa dotycząca poziomu usług. Czas kopii jest zmienny, gdy to zależy od oczekiwania w kolejce, który go też zależy od ilości danych, aby przenieść. Podczas kopiowania zwiększa, jeśli przeniesienie ma inną centrum danych Azure, które pozwala na przechowywanie Premium w innym regionie. Jeśli masz tylko węzły 2, należy rozważyć, czy łagodzenia to możliwe w przypadku kopii trwa dłużej niż podczas testowania. Może to być następujące pomysłów.
    - Dodawanie tymczasowe 3 węzeł programu SQL Server dla HA przed migracją z uzgodnione przestoje.
    - Przeprowadzenie migracji poza Azure zaplanowanych.
    - Upewnij się, że poprawnie skonfigurowany do kworum klaster.  

##### <a name="high-level-steps"></a>Czynności wysokiego poziomu

Ten dokument nie przedstawiają rozbudowany przykład kompleksowego, jednak [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) zawiera szczegółowe informacje, których można użyć do wykonywania tego.

![MinimalDowntime][8]

- Gromadzić konfiguracji dysku, a następnie usuń węzeł (nie usuwaj dołączony wirtualnych dysków twardych).
- Tworzenie konta magazynu Premium i kopiowanie wirtualnych dysków twardych z konta standardowego magazynu
- Tworzenie nowej usługi w chmurze i ponownie wdróż maszyn wirtualnych SQL2 w tej usłudze w chmurze. Tworzenie maszyn wirtualnych przy użyciu skopiowany oryginalny OS wirtualnego dysku twardego i dołączanie skopiowany wirtualnych dysków twardych.
- Konfigurowanie ILB-ELB i dodawanie punktów końcowych.
- Aktualizacja odbiornika przez:
    - Przełączanie zawsze w grupie do trybu offline i aktualizowania zawsze na odbiornika nowe ILB / adres ELB IP.
    - Czy dodanie zasobu adresu IP z nowego chmury usługi ILB-ELB przy użyciu programu PowerShell do klastrów systemu Windows. Następnie ustaw możliwych właścicieli zasobu adres IP do węzła migracją, SQL2 i ustawić jako zależności lub w polu Nazwa sieciowa. Zobacz sekcję "Dodawanie zasobu adresu IP w tej samej podsieci" [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Sprawdzanie konfiguracji DNS i propagowania klientom.
- Migrowanie maszyn wirtualnych SQL1 i przechodzenie przez kroki 2 – 4.
- Jeśli używasz 5ii czynności, następnie dodać SQL1 jako możliwy właściciel dodano zasobu adresu IP
- Testowanie pracy awaryjnej.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. wykorzystanie istniejących replica(s) pomocnicza: wiele witryn

Jeśli masz węzły w więcej niż jednym centrum danych Azure (kontrolera domeny) lub jeśli masz środowiska hybrydowego, można zawsze o konfiguracji w tym środowisku aby zminimalizować przestoje.

To podejście jest aby zmienić synchronizacji zawsze na synchroniczne lokalnego lub pomocnicza Azure kontrolera domeny, a następnie przełączanie awaryjne nad do tego programu SQL Server. Następnie skopiuj wirtualnych dysków twardych na koncie magazynowania Premium i ponownie wdróż komputera do nowej usługi w chmurze. Zaktualizuj odbiornika, a następnie powrócić.

##### <a name="points-of-downtime"></a>Punkty przestoje

Przestoje składa się z czasu na przełączanie awaryjne do alternatywnego kontrolera domeny i Wstecz. To również zależy od konfiguracji klienta/DNS i ponowne swojego klienta mogą być opóźnione.
Należy rozważyć następujący przykład hybrydowych zawsze na konfiguracji:

![MultiSite1][9]

##### <a name="advantages"></a>Zalety

- Można użyć istniejącej infrastruktury.
- Istnieje możliwość sprzed uaktualnienia Azure miejsca do magazynowania na kontrolerze domeny DR Azure najpierw.
- Magazyn DR Azure kontrolera domeny można skonfigurować.
- Istnieje co najmniej dwa praca awaryjna podczas migracji, z wyłączeniem praca awaryjna test.
- Nie trzeba przenieść dane programu SQL Server z kopii zapasowych i przywracanie.

##### <a name="disadvantages"></a>Wady

- W zależności od dostępu klienta z programem SQL Server mogą występować opóźnienia większe uruchamianiem programu SQL Server w alternatywnych kontrolera domeny do aplikacji.
- Podczas kopiowania wirtualnych dysków twardych do magazynu Premium mogą być długie. Może wpłynąć na decyzję dotyczącą czy chcesz zachować węzeł w grupie dostępności. Należy wziąć pod uwagę to po dziennika pracy intensywnie obciążenia są uruchomione podczas migracji jest wymagane, ponieważ węzeł podstawowego należy zachować Niezreplikowane transakcje w jej dziennik transakcji. W związku z tym to może powiększyć znacznie.
- W tym scenariuszu należy użyć polecenia Azure **AzureStorageBlobCopy rozpoczęcia** , która jest asynchroniczne. Po zakończeniu jest nie Umowa dotycząca poziomu usług. Zmienia się czas kopie, gdy to zależy od oczekiwania w kolejce go też zależy od ilości danych, aby przenieść. W związku z tym wystarczy jeden węzeł 2 centrum danych, należy wykonać odpowiednie czynności łagodzenia, w przypadku kopii trwa dłużej niż podczas testowania. Może to być następujące pomysłów.
    - Dodawanie tymczasowe 2 węzeł SQL dla HA przed migracją z uzgodnione przestoje.
    - Przeprowadzenie migracji poza Azure zaplanowanych.
    - Upewnij się, że poprawnie skonfigurowany do kworum klaster.

W tym scenariuszu założono, że masz opisano instalacją i znasz odwzorowania miejsca do magazynowania w celu zmiany ustawień pamięci podręcznej dysków optymalnej.

##### <a name="high-level-steps"></a>Czynności wysokiego poziomu
![Multisite2][10]

- Wprowadź w lokalnej / alternatywne podstawowego serwera SQL Azure kontrolera domeny, a był innych automatycznej pracy awaryjnej partnera (AFP).
- Gromadzenie informacji o konfiguracji dysku z SQL2, a następnie usuń węzeł (nie usuwaj dołączony wirtualnych dysków twardych).
- Tworzenie konta magazynu Premium i skopiuj wirtualnych dysków twardych z konta standardowego magazynu.
- Tworzenie nowej usługi w chmurze i Tworzenie maszyn wirtualnych SQL2 z jego dysków w magazynie premii dołączony.
- Konfigurowanie ILB-ELB i dodawanie punktów końcowych.
- Aktualizowanie zawsze na odbiornika z nowego ILB-ELB IP adresu i testowanie pracy awaryjnej.
- Sprawdź konfigurację DNS.
- Zmienianie AFP SQL2 i przeprowadzić migrację SQL1 i obsłużone kroki od 2 do 5.
- Testowanie pracy awaryjnej.
- Przełącz AFP do SQL1 i SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Dodatek: Migrowanie wielooddziałowość zawsze w klastrze do magazynowania Premium

Pozostała część tego tematu zawiera szczegółowy przykład konwersji klastrze zawsze w wielu witrynach Premium miejsca do magazynowania. Ponadto konwertuje odbiornika możliwość korzystania z usługi równoważenia obciążenia zewnętrznych (ELB) do równoważenia obciążenia wewnętrzny (ILB).

### <a name="environment"></a>Środowisko

- Windows 2k 12-SQL 2k 12
- Pliki 1 DB na SP
- 2 x puli miejsca do magazynowania na węzeł

![Appendix1][11]

### <a name="vm"></a>MASZYNA WIRTUALNA:

W tym przykładzie chwilę celu zademonstrowania przechodzenie z ELB do ILB. ELB były dostępne przed ILB, dlatego to pokazano, jak przełączyć się do tego podczas migracji.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Czynności wstępne: Nawiązywanie połączenia z subskrypcji

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Krok 1: Tworzenie nowego konta magazynu i usług w chmurze
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Krok 2: Zwiększ dozwolonych błędy zasobów<Optional>
Na niektórych zasobów, które należą do zawsze na dostępność grupy istnieją ograniczenia na jak wiele błędów, które mogą wystąpić w okresie, gdzie usługa klaster próbuje ponownie uruchomić grupa zasobów. Zaleca się, że to zwiększenie jednocześnie możesz są Instruktaż tej procedury, ponieważ w przeciwnym razie ręcznie awaryjnego i wyzwalacza praca awaryjna zamykając maszyn, które otrzymujesz Zamknij ten limit.

Możesz ją rozsądne dwukrotnie odliczenie błąd, aby wykonać tę czynność w Menedżerze klaster pracy awaryjnej przejdź do pozycji właściwości grupy zasobów zawsze na:

![Appendix3][13]

Zmienianie maksymalna liczba porażek do 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Krok 3: Adres IP dodanie zasobów dla grupy klastrów<Optional>

Jeśli masz tylko jeden adres IP dla grupy klastrów i to jest wyrównany do chmury podsieci, należy pamiętać, jeśli przypadkowo w tryb offline wszystkie węzłach w chmurze tej sieci następnie zasobu Klaster IP i nazwa sieciowa klaster nie będzie można do trybu online. W przypadku tego uniemożliwi aktualizacje dla innych zasobów klastrów.

#### <a name="step-4-dns-configuration"></a>Krok 4: Konfiguracji systemu DNS

Do wykonania sprawne przejścia zależy od sposobu jest DNS użycia oraz zaktualizowane.
Zawsze włączony jest zainstalowany, który tworzy grupę zasobów klaster systemu Windows podczas otwierania Menedżer klastrów pracy awaryjnej, zobaczysz, że co najmniej będzie miał trzy zasoby, są tymi, które dokument odwołuje się do:

- Wirtualna Network Name (VNN) — jest to nazwa DNS klienta łączyć, gdy chcesz połączyć się z serwerami SQL za pośrednictwem zawsze na.
- Zasób adresu IP — to jest adres IP, który skojarzony z VNN, może mieć więcej niż jeden, a w konfiguracji wielooddziałowości będą mieć na witryny i podsieć adresów IP.

Podczas łączenia się programu SQL Server, klienta programu SQL Server sterownik będą pobierane rekordy DNS skojarzone z odbiornika i spróbuj połączyć się każdego zawsze na skojarzone z adresem IP, pod omówimy niektórych czynników, które wpływają na to.

Liczba równoczesne rekordy DNS, które są skojarzone z nazwą odbiornika zależy od nie tylko na liczbę adresów IP skojarzonych, ale "RegisterAllIpProviders'setting w awaryjnej dla zasobu zawsze włączone VNN.

Przy umieszczaniu zawsze na Azure istnieją różne kroki w celu utworzenia odbiornika i adresy IP, należy ręcznie skonfigurować RegisterAllIpProviders 1, to różny w wersji lokalnej wdrożenia zawsze na miejsce, w którym już jest ustawiona na 1.

Jeśli "RegisterAllIpProviders" jest równa 0, a następnie będziesz widzieć tylko jeden rekord DNS w systemie DNS skojarzone z odbiornika:

![Appendix4][14]

"RegisterAllIpProviders" w przypadku 1:

![Appendix5][15]

Kod poniżej będzie zrzut się ustawienia VNN i ustawić dla Ciebie, pamiętaj, aby zmiany zostały zastosowane, należy przełączyć do trybu offline VNN i przybrało z powrotem do trybu online, to określone odbiornika offline powoduje zakłócenia łączności klienta.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

W późniejszym etapie migracji zostanie musisz zaktualizować odbiornika zawsze na adres IP zaktualizowane będzie odwoływać się do równoważenia obciążenia obejmuje usuwania zasobów adres IP i dodanie. Po aktualizacji adresów IP należy się upewnić, nowy adres IP został zaktualizowany w strefie DNS i klienci aktualizowania ich lokalnej pamięci podręcznej DNS.

Jeśli klientów znajdują się w części innej sieci i odwołanie innego serwera DNS, należy rozważyć, co się stanie, o Transfer strefy DNS podczas migracji, po ponownym podłączeniu aplikacja będzie ograniczone przez co najmniej strefy godzina przełączyć nowe adresy IP odbiornika. Jeśli jesteś w obszarze poniżej ograniczenia czasu, należy omówić i przetestować wymuszania przyrostowy transfer stref w zespołach do systemu Windows, a także umieścić rekord hosta DNS do dolnej czasu wygaśnięcia (TTL), dlatego klienci aktualizują. Aby uzyskać więcej informacji zobacz [Przyrostowe transferów stref](https://technet.microsoft.com/library/cc958973.aspx) i [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Domyślnie TTL rekordu DNS, który jest skojarzony z odbiornika w zawsze na Azure to 1200 sekund. Możesz zmniejszyć to, jeśli jesteś w obszarze czas ograniczenia podczas migracji do zapewnienia klienci aktualizowanie ich DNS o adresie IP zaktualizowane odbiornika. Można wyświetlić i modyfikowanie konfiguracji przez zatapianie się w konfiguracji VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Należy pamiętać dolnym "HostRecordTTL", wystąpi wyższy stopień ruchu DNS.

##### <a name="client-application-settings"></a>Ustawienia aplikacji klienta

Jeśli aplikacja klienta SQL obsługuje .net 4,5 Klient SQL, możesz użyć "MULTISUBNETFAILOVER = TRUE" słów kluczowych, jest to zalecane mają być stosowane jako umożliwia szybsze połączenia SQL zawsze na grupy dostępności podczas pracy awaryjnej. Wylicza wszystkie adresy IP skojarzonego z odbiornika zawsze na równolegle, a wykonuje skuteczniejsze szybkość ponów próbę połączenia TCP w trybie awaryjnym.

Aby uzyskać więcej informacji na temat ustawień powyżej można znaleźć [słowo kluczowe MultiSubnetFailover i skojarzonych z funkcji](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Zobacz też [Klient SQL obsługa wysokiej dostępności, awarii](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Krok 5: Ustawienia kworum klaster

Jak mają być pobieranie co najmniej jeden program SQL Server w dół naraz, należy zmodyfikować ustawienie klaster kworum, jeśli z 2 węzły przy użyciu monitora udostępniania plików (FSW), należy ustawić kworum, aby zezwolić na Większość węzłów i są używane dynamiczne głosowanie i to umożliwia dla jednego węzła pozostać miejsc stojących.


    Set-ClusterQuorum -NodeMajority  

Aby uzyskać więcej informacji na temat zarządzania i konfigurowania kworum klaster zobacz [Konfigurowanie i zarządzanie kworum w klastrze pracy awaryjnej systemu Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Krok 6: Wyodrębnić istniejące punkty końcowe i listy kontroli dostępu
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Zapisywanie ich do pliku tekstowego.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Krok 7: Zmienianie partnerów pracy awaryjnej i trybów replikacji

Jeśli masz więcej niż 2 serwerów SQL, należy zmienić awaryjnego przeniesienia innego pomocniczego w innej kontrolera domeny lub lokalnego "Synchroniczne" i był automatycznego partnera pracy awaryjnej (AFP), to jest, aby zachować HA podczas wprowadzania zmian. Można to zrobić za pomocą TSQL: modyfikowanie jednak SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Krok 8: Usuwanie pomocniczych maszyn wirtualnych usługi w chmurze

Należy planowane migrowanie węźle pomocniczym w chmurze, jeśli jest obecnie podstawowego, należy inicjować ręczne przejęcie awaryjne.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 9: Zmienianie dyskowej pamięci podręcznej ustawienia w pliku CSV i zapisywanie

Dla ilości danych tych powinna być równa tylko do odczytu.

Do przedstawienia wielkości TLOG tych powinna być równa Brak.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Krok 10: Kopiowanie wirtualnych dysków TWARDYCH
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Możesz sprawdzić stan kopii wirtualnych dysków twardych z tym kontem magazynowania Premium:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Poczekaj, aż wszystkie te są rejestrowane jako sukcesu.

Aby uzyskać informacje dotyczące poszczególnych obiektów blob:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Krok 11: Dysk rejestru systemu operacyjnego

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Krok 12: Importowanie pomocniczej do nowej usługi w chmurze

Kod poniżej także używa opcji dodane w tym miejscu można zaimportować komputera i używać retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Krok 13: Tworzenie ILB na nowy chmury Svc, Dodaj ładowania strategie punkty końcowe i listy kontroli dostępu
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Krok 14: Zawsze zaktualizować
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Teraz usunąć stare usług w chmurze adres IP.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Krok 15: Sprawdzanie aktualizacji DNS

Teraz należy sprawdzić serwerów DNS sieci klienta programu SQL Server i upewnij się, że klaster został dodany rekord hosta dodatkowe dodano adres IP. Jeśli te serwery DNS nie zostały zaktualizowane, należy rozważyć, czy wymuszania transfer strefy DNS i upewnij się, że klienci w podsieci będą mogli rozpoznawać oba zawsze na adresy IP, to jest, aby nie trzeba czekać na automatyczne replikacji DNS.

#### <a name="step-16-reconfigure-always-on"></a>Krok 16: Skonfigurowanie zawsze na

W tym momencie poczekać pomocniczej tego węzła, który został migrowane do w pełni zsynchronizować z węzła lokalnego i przełącz się do węzła synchroniczne replikacji i był AFP.  

#### <a name="step-17-migrate-second-node"></a>Krok 17: Migrowanie drugiego węzła
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 18: Zmienianie dyskowej pamięci podręcznej ustawienia w pliku CSV i zapisywanie

Dla ilości danych tych powinna być równa tylko do odczytu.

Do przedstawienia wielkości TLOG tych powinna być równa Brak.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Krok 19: Tworzenie nowego konta niezależnych miejsca do magazynowania dla węzła pomocniczej
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Krok 20: Kopiowanie wirtualnych dysków TWARDYCH
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Możesz sprawdzić stan kopii wirtualnego dysku twardego dla wszystkich wirtualnych dysków twardych: ForEach ($disk w $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Etykieta_dysku $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Poczekaj, aż wszystkie te są rejestrowane jako sukcesu.

Aby uzyskać informacje dotyczące poszczególnych obiektów blob: #Check induvidual obiektów blob stanu Get-AzureStorageBlobCopyState-Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd"-kontener $containerName-kontekstu $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Krok 21: Dysk rejestru systemu operacyjnego
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Krok 22: Dodawanie ładowania strategie punkty końcowe i listy kontroli dostępu
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Krok 23: Pracy awaryjnej Test

Teraz należy powiadomić węzeł migracją synchronizowanie z lokalnego zawsze na węzeł, umieść go w trybie synchroniczne replikacji i poczekaj, aż jest synchronizowane. Następnie awaryjne z lokalna w pierwszym węźle migracji, która jest AFP. Po której pracował, zmienić ostatniego węzła migracją AFP.

Należy przetestować praca awaryjna między wszystkie węzły i uruchamiania, jeśli poprawnie testów chaos zapewnienie praca awaryjna pracy i w odpowiednim manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Krok 24: Przywrócić ustawienia kworum klaster-DNS TTL i pracy awaryjnej Pntrs i ustawień synchronizacji
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Dodawanie zasobu adresu IP w tej samej podsieci

Jeśli masz tylko 2 serwery SQL i chcesz przeprowadzić migrację je do nowej usługi w chmurze, ale chcesz zachować je w tej samej podsieci, można uniknąć trwa odbiornika w trybie offline Usuń oryginalny zawsze na adres IP i Dodaj nowy adres IP. Jeśli maszyny wirtualne są migrowane do innej podsieci będzie trzeba wykonaj następujące czynności, ponieważ będzie sieć kolejne klastrze odwołującego się podsieci.

Po przeniesione migracją monitora pomocniczego i dodane w nowego zasobu adres IP dla nowej usługi w chmurze przed pracy awaryjnej istniejących podstawowych należy wykonaj następujące czynności w ramach Menedżer pracy awaryjnej klaster:

Aby dodać w polu adres IP, zobacz [dodatek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), krok 14.

1. Dla bieżącego zasobu adres IP Zmienianie właściciela do"istniejące podstawowego programu SQL Server", w poniższym przykładzie, "dansqlams4":

    ![Appendix13][23]

1. Dla nowego zasobu adres IP Zmienianie właściciela możliwe do"Migrated pomocniczej programu SQL Server", w poniższym przykładzie, "dansqlams5":

    ![Appendix14][24]

1. Po ustawieniu to można pracy awaryjnej, a podczas migracji ostatniego węzła możliwych właścicieli powinny być edytowane, więc węzeł zostanie dodane jako możliwy właściciel:

    ![Appendix15][25]

## <a name="additional-resources"></a>Dodatkowe zasoby
- [Azure Premium miejsca do magazynowania](../storage/storage-premium-storage.md)
- [Maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machines/)
- [Program SQL Server w środowisku maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
