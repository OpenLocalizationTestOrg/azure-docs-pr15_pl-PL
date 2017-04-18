<properties
    pageTitle="Tworzenie wirtualnych serwerze SQL w programie PowerShell Azure (klasyczny) | Microsoft Azure"
    description="Przedstawiono kroki i skryptów programu PowerShell tworzenia maszyn wirtualnych Azure SQL Server maszyn wirtualnych ze zdjęciami z galerii. W tym temacie korzysta z trybu klasycznego wdrożenia."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Inicjowanie obsługi maszyny wirtualnej programu SQL Server przy użyciu programu PowerShell Azure (klasyczny)

## <a name="overview"></a>Omówienie

Ten artykuł zawiera instrukcje dotyczące tworzenia maszyny wirtualnej programu SQL Server w Azure za pomocą poleceń cmdlet programu PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dla wersji Menedżera zasobów w tym temacie zobacz [Obsługa administracyjna maszyny wirtualnej programu SQL Server za pomocą Menedżera zasobów programu PowerShell Azure](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Instalowanie i konfigurowanie programu PowerShell:

1. Jeśli nie masz konta usługi Azure, odwiedź [Azure bezpłatny okres próbny](https://azure.microsoft.com/pricing/free-trial/).

2. [Pobierz i zainstaluj najnowszą polecenia programu PowerShell Azure](../powershell-install-configure.md).

3. Uruchom program Windows PowerShell i połączyć go z subskrypcji usługi Azure przy użyciu polecenia **Dodaj AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Określanie docelowego regionu Azure

Usługi SQL Server Virtual Machine będzie obsługiwana w usłudze w chmurze, który znajduje się określonego regionu Azure. Poniższe czynności ułatwiają ustalenie konto region, konto miejsca do magazynowania, a usługa, która będzie używana dla pozostałych samouczka w chmurze.

1. Określanie centrum danych, którego chcesz użyć do obsługi maszyn wirtualnych usługi SQL Server. Następujące polecenia programu PowerShell wyświetli dostępne regionów szczegółowo z listą podsumowań na końcu.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Po zostały zidentyfikowane swojej preferowanej lokalizacji, ustaw zmienną o nazwie **$dcLocation** do tego regionu.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Ustawianie konta subskrypcji i miejsca do magazynowania

1. Określanie Azure subskrypcję, którą chcesz użyć do nowej maszyny wirtualnej.

        (Get-AzureSubscription).SubscriptionName

1. Przypisz docelowego Azure subskrypcji do zmiennej **$subscr** . Następnie ustawić jako bieżącej subskrypcji Azure.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Sprawdź w przypadku istniejących kont miejsca do magazynowania. Poniższy skrypt zostaną wyświetlone wszystkie konta miejsca do magazynowania, znajdują się w danym regionie wybranym:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Jeśli jest wymagane do nowego konta z miejsca do magazynowania, najpierw utworzyć nazwę konta magazynu wszystkich liter przy użyciu polecenia Nowy AzureStorageAccount, tak jak w poniższym przykładzie: **New AzureStorageAccount - StorageAccountName "<storage account name>"-lokalizacji $dcLocation**

1. Przypisz nazwę konta magazynu docelowej do **$staccount**. Następnie użyj **Zestawu AzureSubscription** , aby ustawić subskrypcji i bieżącego konta miejsca do magazynowania.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Wybieranie obrazu maszyn wirtualnych programu SQL Server

1. Dowiedz się, na liście dostępnych obrazów maszyn wirtualnych programu SQL Server z galerii. Te wszystkie obrazy mają właściwość **ImageFamily** zaczynające się od "SQL". Poniższe zapytanie Wyświetla dostępne rodziny obraz do użytkownika, które mają preinstalowany programu SQL Server.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Gdy znajdziesz rodzinie obraz maszyn wirtualnych, w tym rodziny może istnieć wiele opublikowanych obrazów. Użyj tego skryptu, aby znaleźć najnowszą nazwę obrazu opublikowanych maszyn wirtualnych rodzinie zaznaczonego obrazu (na przykład **SQL Server 2016 RTM Enterprise w systemie Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Tworzenie maszyny wirtualnej

Na koniec Utwórz maszyny wirtualnej z programem PowerShell:

1. Tworzenie usługi w chmurze do obsługi nowych maszyn wirtualnych. Należy zauważyć, że użytkownik może również użyć istniejącej usługi w chmurze. Tworzenie nowych zmiennych **$svcname** z krótką nazwę usługi w chmurze.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Określ nazwę maszyn wirtualnych i rozmiar. Aby uzyskać więcej informacji na temat rozmiarów maszyn wirtualnych zobacz [Maszyn wirtualnych rozmiarów Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Określ lokalnego konta administratora i hasło.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Uruchom następujący skrypt w celu utworzenia maszyny wirtualnej.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Aby uzyskać dodatkowe objaśnienia i opcje konfiguracji zobacz sekcję **Utwórz zestaw poleceń** w [Użyj Azure programu PowerShell, aby utworzyć i wstępnie skonfigurować maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Przykładowy skrypt programu PowerShell

Poniższy skrypt zawiera i przykładowy skrypt wykonane, który tworzy maszyny wirtualnej **SQL Server 2016 RTM Enterprise w systemie Windows Server 2012 R2** . Jeśli używasz tego skryptu, konieczne jest dostosowanie zmienne początkowej, w oparciu o poprzednie kroki w tym temacie.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Łączenie się z pulpitu zdalnego

1. Tworzenie. RDP pliki w folderze dokument bieżącego użytkownika w celu uruchamiania tych maszyn wirtualnych, aby ukończyć konfigurację:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. W katalogu dokumenty Uruchom plik RDP. Nawiązywanie połączenia przy użyciu nazwy użytkownika i hasła, znajdujących się we wcześniejszej (na przykład jeśli VMAdmin swoją nazwę użytkownika, określić "\VMAdmin" jako użytkownika i hasło).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Kończenie konfiguracji komputera serwera SQL dla dostępu zdalnego

Po skonfigurowaniu logowanie się do komputera za pomocą pulpitu zdalnego, SQL Server zgodnie z instrukcjami podanymi w [instrukcje dotyczące konfigurowania programu SQL Server łączność w maszyn wirtualnych Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Następne kroki

Można znaleźć dodatkowe instrukcje dotyczące obsługi administracyjnej maszyn wirtualnych przy użyciu programu PowerShell w [środowisku maszyn wirtualnych systemu dokumentacji](virtual-machines-windows-classic-create-powershell.md). Dodatkowe skryptów związane z programu SQL Server i Premium miejsca do magazynowania zobacz [Używanie Azure Premium magazyn z programem SQL Server w środowisku maszyn wirtualnych systemu](virtual-machines-windows-classic-sql-server-premium-storage.md).

W większości przypadków następnym krokiem jest migracji baz danych do tego nowego maszyn wirtualnych serwera SQL. Orientacji migracji bazy danych zobacz [Migrowanie bazy danych programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-migrate-sql.md).

Jeśli chcesz również w tworzenie maszyn wirtualnych SQL za pomocą portalu Azure, zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md). Zauważ, że samouczek, który przeprowadzi Cię przez portalu tworzy maszyny wirtualne za pomocą modelu Menedżera zasobów zalecane zamiast modelu Klasyczny używane w tym temacie programu PowerShell.

Oprócz tych zasobów zalecamy zapoznanie się [innych tematów związanych z programem SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
