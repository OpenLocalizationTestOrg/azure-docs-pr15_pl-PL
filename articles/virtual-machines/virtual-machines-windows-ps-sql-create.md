<properties
    pageTitle="Tworzenie wirtualnych serwerze SQL w programie PowerShell Azure (Menedżer zasobów) | Microsoft Azure"
    description="Przedstawiono kroki i skryptów programu PowerShell tworzenia maszyn wirtualnych Azure SQL Server maszyn wirtualnych ze zdjęciami z galerii."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Inicjowanie obsługi maszyny wirtualnej programu SQL Server przy użyciu programu PowerShell Azure (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Programu PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Omówienie

Ten samouczek pokazano, jak utworzyć pojedynczy Azure maszyny wirtualnej przy użyciu modelu wdrożenia **Menedżera zasobów Azure** za pomocą poleceń cmdlet środowiska PowerShell Azure. W tym samouczku zostanie utworzony jednym komputerze wirtualnych przy użyciu pojedynczy dysk z obrazu w galerii SQL. Zostanie utworzony nowy dostawców dla miejsca do magazynowania, sieci i zasoby obliczeń, które będą używane przez maszyny wirtualnej. Jeśli masz istniejące dostawców dla każdego z tych zasobów, możesz użyć tych dostawców.

W razie potrzeby w klasycznej wersji programu w tym temacie, zobacz [Obsługa administracyjna maszyny wirtualnej programu SQL Server przy użyciu klasycznego programu PowerShell Azure](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek musisz:

- Konto Azure i subskrypcję przed rozpoczęciem pracy. Jeśli nie masz jedną Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- [Azure programu PowerShell)](../powershell-install-configure.md), minimalna wersja programu 1.4.0 lub nowszego (ten samouczek napisane przy użyciu wersji 1.5.0).
    - Aby pobrać wersji, wpisz **ListAvailable Azure Get-modułu**.

## <a name="configure-your-subscription"></a>Konfigurowanie subskrypcji

Otwieranie programu Windows PowerShell i ustanowić uzyskiwania dostępu do konta Azure, uruchamiając następujące polecenie cmdlet. Zostanie wyświetlona ze znakiem na ekranie, aby wprowadzić poświadczenia. Za pomocą samego adresu e-mail i hasła, którego używasz, aby zalogować się do portalu Azure.

    Add-AzureRmAccount

Po pomyślnym zalogowaniu się pewne informacje będą wyświetlane na ekranie, który zawiera SubscriptionId, z którym nie zostało to zrobione. Jest to subskrypcji, w którym zostaną utworzone zasoby dla tego samouczka, jeśli nie zmienisz do innej subskrypcji. Jeśli masz wiele SubscriptionIds, uruchom następujące polecenie cmdlet, aby powrócić do listy wszystkich usługi SubscriptionIds:

    Get-AzureRmSubscription

Aby zmienić na inną SubscriptionID, uruchom następujące polecenie cmdlet przy użyciu usługi odpowiedniej SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definiowanie zmiennych obrazu

Aby uprościć użyteczności i opis skryptu wykonanych z tego samouczka, firma Microsoft rozpocznie się ze definiując wiele zmiennych. Zmień wartości parametrów, ale Uważaj nazewnictwa ograniczenia dotyczące długości nazwy i znaków specjalnych podczas modyfikowania wartości podane.

### <a name="location-and-resource-group"></a>Lokalizacja i grupa zasobów
Umożliwia określenie obszaru danych i grupa zasobów, do którego ma zostać utworzony inne zasoby dla maszyny wirtualnej dwóch zmiennych.

Modyfikowanie stosownie do potrzeb, a następnie wykonaj następujące polecenia cmdlet, aby zainicjować zmienne.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Właściwości magazynu

Umożliwia określenie konta miejsca do magazynowania i typ używanego magazynu może być używany przez komputer wirtualnych następujących zmiennych.

Modyfikowanie stosownie do potrzeb, a następnie wykonaj następujące polecenie cmdlet zainicjować zmienne. Należy zauważyć, że w tym przykładzie użyto [Premium miejsca do magazynowania](../storage/storage-premium-storage.md), co jest zalecane dla obciążenia produkcji. Aby uzyskać szczegółowe informacje dotyczące tych wskazówek i innych zaleceń zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Właściwości sieci

Umożliwia określenie interfejsu sieciowego, Metoda alokacji TCP/IP, nazwa wirtualną sieć, nazwa wirtualna podsieci, zakres adresów IP dla wirtualnej sieci, zakres adresów IP dla podsieci i etykieta nazwy publiczną może być używany przez sieć maszyny wirtualnej następujących zmiennych.

Modyfikowanie stosownie do potrzeb, a następnie wykonaj następujące polecenie cmdlet zainicjować zmienne.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Właściwości maszyn wirtualnych

Umożliwia określenie nazwy maszyn wirtualnych, nazwę komputera, rozmiar maszyn wirtualnych i nazwa dysku systemu operacyjnego maszyny wirtualnej następujących zmiennych.

Modyfikowanie stosownie do potrzeb, a następnie wykonaj następujące polecenie cmdlet zainicjować zmienne.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Właściwości obrazu

Umożliwia określenie obraz, aby używać maszyny wirtualnej następujących zmiennych. W tym przykładzie jest używany obraz SQL Server 2016 Enterprise.

Modyfikowanie stosownie do potrzeb, a następnie wykonaj następujące polecenie cmdlet zainicjować zmienne.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Zwróć uwagę, że można uzyskać pełną listę ofert obrazu programu SQL Server przy użyciu polecenia Get-AzureRmVMImageOffer:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

I można zobaczyć SKU dostępne dla oferująca przy użyciu polecenia Get-AzureRmVMImageSku. Następujące polecenie pokazuje oferują wszystkich wersji produktów, które są dostępne dla **SQL2014SP1 WS2012R2** .

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Model wdrożenia Menedżera zasobów pierwszy utworzony obiekt jest grupa zasobów. Tworzenie grupy usługi Azure zasobów i jego zasobów z nazwy grupy zasobów i lokalizacji definiowane przez zmienne, które poprzednio zainicjowany użyjemy polecenia cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) .

Wykonaj następujące polecenie cmdlet do utworzenia nowej grupy zasobów.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

Maszyna wirtualna wymaga zasobów miejsca do magazynowania dla dysku systemu operacyjnego i plików danych i dziennika programu SQL Server. Dla uproszczenia zostanie utworzony jednego dysku dla obu. Można dołączyć dodatkowe dyski później przy użyciu polecenia cmdlet [Dysku Azure Dodaj](https://msdn.microsoft.com/library/azure/dn495252.aspx) aby umieścić dane programu SQL Server i pliki na dyskach dedykowane dziennika. Aby utworzyć konto standardowego magazynu w nowej grupy zasobów i nazwę konta magazynu, miejsca do magazynowania Sku nazwy i lokalizacji zdefiniowane przy użyciu zmiennych, które poprzednio zainicjowany użyjemy polecenia cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) .

Wykonaj następujące polecenie cmdlet, aby utworzyć nowe konto miejsca do magazynowania.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Tworzenie zasobów

Maszyna wirtualna wymaga wiele zasobów sieci łączności sieciowej.

- Każda maszyna wirtualna wymaga wirtualnej sieci.
- Wirtualna sieć musi mieć co najmniej jedną podsieć zdefiniowane.
- Karty sieciowej należy zdefiniować przy użyciu publicznego lub prywatny adres IP.

### <a name="create-a-virtual-network-subnet-configuration"></a>Tworzenie konfiguracji podsieci wirtualnej sieci

Firma Microsoft zostanie uruchomiony przez utworzenie konfiguracji podsieci dla naszych wirtualnej sieci. Nasz samouczek zostanie utworzony przy użyciu polecenia cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) podsieć domyślne. Nasze konfiguracji podsieci wirtualną sieć zostanie utworzony prefiksem podsieci nazwisko i adres zdefiniowane przy użyciu zmiennych, które poprzednio zainicjowany.

>[AZURE.NOTE] Można zdefiniować dodatkowe właściwości konfiguracji podsieci wirtualną sieć za pomocą tego polecenia cmdlet, ale który wykracza poza zakres tego samouczka.

Wykonaj następujące polecenie cmdlet Tworzenie konfiguracji podsieci wirtualną.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Tworzenie wirtualnych sieci

Następnie zostanie utworzony naszych wirtualnej sieci przy użyciu polecenia cmdlet [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . Nasze wirtualną sieć zostanie utworzony w nowej grupy zasobów, z nazwę, lokalizację i adres prefiks zdefiniowany przy użyciu zmiennych, które poprzednio zainicjowany i używanie konfiguracji podsieci, którą zdefiniowano w poprzednim kroku.

Wykonaj następujące polecenie cmdlet Tworzenie wirtualnych sieci.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Tworzenie publiczny adres IP

Teraz gdy mamy już naszych wirtualną sieć zdefiniowane, trzeba skonfigurować adres IP łączności maszyn wirtualnych. Ten samouczek zostanie utworzony publiczny adres IP przy użyciu dynamiczne IP adresowania do obsługi łączność z Internetem. Aby utworzyć publiczny adres IP, w grupie zasobów utworzone prevously i nazwę, lokalizację, Metoda alokacji i etykiety nazw domen DNS zdefiniowane przy użyciu zmiennych, które poprzednio zainicjowany użyjemy polecenia cmdlet [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) .

>[AZURE.NOTE] Można zdefiniować dodatkowe właściwości publiczny adres IP przy użyciu tego polecenia cmdlet, ale który wykracza poza zakres tego samouczka początkowej. Można również utworzyć adres prywatny lub adresu statycznego adresu, ale jest również poza zakres tego samouczka.

Wykonaj następujące polecenie cmdlet, aby utworzyć publiczny adres IP.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Tworzenie interfejsu sieciowego

Teraz możemy przystąpić do tworzenia interfejsu sieciowego, który użyje naszych maszyn wirtualnych. Tworzenie naszych sieciowej w grupie zasobów utworzone wcześniej i z nazwą, lokalizację, podsieci i publiczny adres IP wstępnie zdefiniowana użyjemy polecenia cmdlet [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) .

Wykonaj następujące polecenie cmdlet Tworzenie usługi sieciowej.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurowanie obiektu maszyn wirtualnych

Teraz, gdy mamy zasobów sieci i miejsca do magazynowania, zdefiniowane przez możemy przystąpić do definiowania obliczeń zasobów dla maszyny wirtualnej. Nasz samouczek firma Microsoft określić rozmiar maszyn wirtualnych i różne właściwości systemu operacyjnego, określ sieciowej, możemy wcześniej utworzonej przez definiowanie magazyn obiektów blob, a następnie określ dysku systemu operacyjnego.

### <a name="create-the-vm-object"></a>Tworzenie obiektu maszyn wirtualnych

Firma Microsoft rozpocznie się określając rozmiar maszyn wirtualnych. Ten samouczek możemy określania DS13. Aby utworzyć obiekt można konfigurować maszyn wirtualnych o nazwie i rozmiar zdefiniowane przy użyciu zmiennych, które poprzednio zainicjowany użyjemy polecenia cmdlet [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) .

Wykonaj następujące polecenie cmdlet, aby utworzyć obiekt maszyn wirtualnych.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Tworzenie obiektu poświadczeń, aby pomieścić nazwę i hasło dla poświadczenia administratora lokalnego

Przed możemy można ustawić właściwości systemu operacyjnego maszyny wirtualnej, należy podać poświadczenia dla lokalnego konta administratora jako ciąg bezpiecznego. Aby to zrobić, użyjemy polecenia cmdlet [Get-poświadczeń](https://technet.microsoft.com/library/hh849815.aspx) .

Wykonaj następujące polecenie cmdlet, a następnie w oknie programu Windows PowerShell poświadczeń żądania wpisz nazwę i hasło dla lokalnego konta administratora na komputerze wirtualnych systemu Windows.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Ustawianie właściwości systemu operacyjnego maszyny wirtualnej

Teraz możemy przystąpić do ustawiania właściwości systemu operacyjnego maszyny wirtualnej. Aby ustawić typ systemu operacyjnego jako systemu Windows, wymagają [agenta maszyn wirtualnych](virtual-machines-windows-classic-agents-and-extensions.md) w celu zainstalowania, określ, czy polecenia cmdlet umożliwia aktualizacje automatyczne i ustawić nazwę maszyn wirtualnych, nazwę komputera i poświadczeń przy użyciu zmiennych, które poprzednio zainicjowany użyjemy polecenia cmdlet [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) .

Wykonaj następujące polecenie cmdlet, aby ustawić właściwości systemu operacyjnego komputera wirtualnych.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Dodawanie interfejsu sieciowego maszyn wirtualnych

Następnie możemy dodać interfejsu sieciowego, który utworzone wcześniej maszyn wirtualnych. Aby dodać sieciowej przy użyciu zmiennej interfejsu sieci, którą wcześniej zdefiniowano użyjemy polecenia cmdlet [AzureRmVMNetworkInterface Dodaj](https://msdn.microsoft.com/library/mt619351.aspx) .

Wykonaj następujące polecenie cmdlet, aby ustawić sieciowej komputera wirtualnych.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Ustawianie lokalizacji przechowywania obiektów blob dysku może być używany przez komputer wirtualnych

Następnie możemy zostanie ustawiona lokalizacji przechowywania obiektów blob dysku może być używany przez komputer wirtualnych przy użyciu zmiennych, które wcześniej zdefiniowano.

Wykonaj następujące polecenie cmdlet, aby ustawić lokalizację magazyn obiektów blob.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Ustawianie właściwości dysku maszyny wirtualnej system operacyjny

Następnie firma Microsoft będzie właściwości systemu operacyjnego dysku dla maszyny wirtualnej. Aby określić, że system operacyjny dla maszyny wirtualnej będą pochodzić z obrazu, aby ustawić buforowania odczytać tylko (ponieważ program SQL Server są instalowane na tym samym dysku) i nazwa maszyn wirtualnych i dysku system operacyjny zdefiniowane przy użyciu zmiennych, które było wcześniej zdefiniowane użyjemy polecenia cmdlet [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) .

Wykonaj następujące polecenie cmdlet, aby ustawić właściwości dysku komputera wirtualnych systemu operacyjnego.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Określ obraz platformy dla maszyny wirtualnej

Nasze ostatni krok konfiguracji jest określenie obraz platformy dla naszych maszyn wirtualnych. Nasz samouczek użyto najnowszą obraz CTP programu SQL Server 2016. Aby użyć tego obrazu zdefiniowane zmienne zdefiniowane wcześniej użyjemy polecenia cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) .

Wykonaj następujące polecenie cmdlet, aby określić obraz platformy komputera wirtualnych.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Tworzenie maszyn wirtualnych SQL

Teraz, gdy skończysz czynności konfiguracyjne, możesz przystąpić do tworzenia maszyny wirtualnej. Użyjemy polecenia cmdlet [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) do tworzenia maszyny wirtualnej przy użyciu zmiennych zdefiniowane firma Microsoft.

Wykonaj następujące polecenie cmdlet, aby utworzyć komputera wirtualnych.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Maszyna wirtualna zostanie utworzona. Zwróć uwagę, że konto standardowego magazynu jest tworzone uruchamiania diagnostyki pakietu, ponieważ konto określonego miejsca do magazynowania dla dysku maszyny wirtualnej jest kontem miejsca do magazynowania premium.

Teraz można wyświetlać tego komputera w Portal Azure, aby wyświetlić [jego publiczny adres IP i jego w pełni kwalifikowaną nazwę domeny](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Przykładowy skrypt

Poniższy skrypt zawiera kompletny skrypt programu PowerShell dla tego samouczka. Przyjęto założenie, że masz już konfiguracji Azure subskrypcję za pomocą polecenia **Dodaj AzureRmAccount** i **Wybierz pozycję AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Następne kroki
Po utworzeniu maszyny wirtualnej można przystąpić do łączenie maszyn wirtualnych za pomocą połączenia RDP i konfiguracji. Aby uzyskać więcej informacji zobacz [Łączenie z programu SQL Server maszyny wirtualnej Azure (Menedżer zasobów)](virtual-machines-windows-sql-connect.md).
