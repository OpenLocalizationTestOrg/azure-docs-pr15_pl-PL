<properties 
   pageTitle="Typowe polecenia programu PowerShell dla maszyny wirtualne | Microsoft Azure"
   description="Typowe polecenia programu PowerShell ułatwiające rozpoczęcie pracy, tworzenie i zarządzanie nimi z maszyny wirtualne platformy Azure w systemie Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Typowe polecenia programu PowerShell służące do tworzenia i zarządzania maszyny wirtualne

W tym artykule opisano niektóre polecenia programu PowerShell Azure, których można tworzyć i zarządzać nimi maszyn wirtualnych w ramach subskrypcji Azure.  Aby uzyskać dodatkową pomoc przy użyciu przełączniki wiersza polecenia i opcje umożliwia **Uzyskiwanie pomocy** *polecenia*.

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="create-a-vm"></a>Tworzenie maszyn wirtualnych

Zadanie | Polecenie
-------------- | -------------------------
Tworzenie konfiguracji maszyn wirtualnych | $vm = [New AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>Konfiguracja maszyn wirtualnych jest używana do definiowania lub aktualizowanie ustawień maszyn wirtualnych. Konfiguracja zainicjowano o nazwie maszyn wirtualnych i jego [rozmiar](virtual-machines-windows-sizes.md).
Dodaj ustawienia konfiguracji | $vm = [Set AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - maszyn wirtualnych $vm-Windows - ComputerName "nazwa_komputera"-poświadczeń $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Ustawienia systemu operacyjnego, w tym [poświadczenia](https://technet.microsoft.com/library/hh849815.aspx) są dodawane do uprzednio utworzony przy użyciu AzureRmVMConfig nowy obiekt konfiguracji.
Dodawanie karty sieciowej | $vm = [AzureRmVMNetworkInterface Dodaj](https://msdn.microsoft.com/library/mt619351.aspx) - maszyn wirtualnych $vm-identyfikator $ karty sieciowej. Identyfikator<BR></BR><BR></BR>Maszyny musi być [sieciowej](virtual-machines-windows-ps-create.md) można komunikować się w wirtualnej sieci. Za pomocą [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) do pobierania istniejącego obiektu interfejsu sieci.
Określ obraz platformy | $vm = [Set AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - maszyn wirtualnych $vm - PublisherName "publisher_name"-oferują "publisher_offer" — wersje produktu "product_sku"-"najnowszych" wersji<BR></BR><BR></BR>[Obraz informacje](virtual-machines-windows-cli-ps-findimage.md) są dodawane do uprzednio utworzony przy użyciu AzureRmVMConfig nowy obiekt konfiguracji. Obiekt zwrócony przez to polecenie jest używane tylko po ustawieniu dysku systemu operacyjnego, aby użyć obrazu platformy.
Ustawianie OS dysku, aby użyć obrazu platformy | $vm = [Set AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - maszyn wirtualnych $vm-- VhdUri "disk_name" Nazwa "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage<BR></BR><BR></BR>Nazwa dysku systemu operacyjnego i jego lokalizacji w [magazynie](../storage/storage-powershell-guide-full.md) jest dodawana do wcześniej utworzony obiekt konfiguracji.
Ustawianie dysku z systemem operacyjnym w systemie obraz uogólniony | $vm = set AzureRmVMOSDisk - maszyn wirtualnych $vm-- SourceImageUri "disk_name" Nazwa "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk. VHD {guid}"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-systemu Windows<BR></BR><BR></BR>Nazwa dysku system operacyjny, lokalizację obraz źródłowy i lokalizacji na dysku w [magazynie](../storage/storage-powershell-guide-full.md) jest dodawana do obiektu konfiguracji.
Ustawianie OS dysku w systemie specjalistyczne obrazu | $vm = set AzureRmVMOSDisk - maszyn wirtualnych $vm-dołączyć - CreateOption - VhdUri "name_of_disk" Nazwa "http://mystore1.blob.core.windows.net/vhds/" - systemu Windows
Tworzenie maszyn wirtualnych | [Nowy AzureRmVM]() - ResourceGroupName "resource_group_name"-lokalizacji "location_name" - maszyn wirtualnych $vm<BR></BR><BR></BR>Wszystkie zasoby są tworzone w [grupie zasobów](../powershell-azure-resource-manager.md). Maszyn wirtualnych muszą zostać utworzone w tej samej [lokalizacji](https://msdn.microsoft.com/library/azure/dn495177.aspx) , co grupa zasobów. Przed uruchomieniem tego polecenia Uruchom nowy AzureRmVMConfig, AzureRmVMOperatingSystem zestaw AzureRmVMSourceImage zestawu, Dodaj AzureRmVMNetworkInterface i AzureRmVMOSDisk zestawu.

## <a name="get-information-about-vms"></a>Uzyskiwanie informacji na temat maszyny wirtualne

Zadanie | Polecenie
-------------- | -------------------------
Lista maszyny wirtualne w subskrypcji| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Lista maszyny wirtualne w grupie zasobów | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Aby uzyskać listę grup zasobów w ramach subskrypcji, za pomocą [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Uzyskiwanie informacji na temat maszyny | Get-AzureRmVM - ResourceGroupName "resource_group_name"-Nazwa "vm_name"

## <a name="manage-vms"></a>Zarządzanie maszyny wirtualne

Zadanie | Polecenie
-------------- | -------------------------
Rozpoczynanie maszyny | [Rozpocznij AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-Nazwa "vm_name"
Zatrzymywanie maszyny | [Zatrzymaj AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-Nazwa "vm_name"
Uruchom ponownie uruchomionego maszyn wirtualnych | [Uruchom ponownie AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-Nazwa "vm_name"
Usuwanie maszyny | [Usuń AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-Nazwa "vm_name"
Generalize maszyny | [Ustawianie AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-nazwę "vm_name"-uogólniony<BR></BR><BR></BR>Uruchom to polecenie przed uruchomieniem Zapisz AzureRmVMImage.
Przechwytywanie maszyny | [Zapisz AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-Ścieżka "C:\filepath\filename.json"<BR></BR><BR></BR>Maszyny wirtualnej należy [zamknąć i uogólniony](virtual-machines-windows-generalize-vhd.md) ma być używany do tworzenia obrazu. Przed uruchomieniem tego polecenia Uruchom zestaw AzureRmVm.
Aktualizowanie maszyny | [Aktualizacja AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - maszyn wirtualnych $vm<BR></BR><BR></BR>Uzyskiwanie bieżącej konfiguracji maszyn wirtualnych przy użyciu Get-AzureRmVM, należy zmienić ustawienia konfiguracji na obiekcie maszyn wirtualnych, a następnie uruchom tego polecenia.
Dodawanie dyskiem danych do maszyn wirtualnych | [Dodaj AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - maszyn wirtualnych $vm-- VhdUri "disk_name" Nazwa "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - LUN #-buforowanie odczytu i zapisu - DiskSizeinGB # - CreateOption puste<BR></BR><BR></BR>Pobierz obiekt maszyn wirtualnych za pomocą Get-AzureRmVM. Określ numer LUN i rozmiaru dysku. Uruchom aktualizację-AzureRmVM, aby zastosować zmiany konfiguracji do maszyn wirtualnych. Dysk, na którym możesz dodać nie został zainicjowany. Aby dowiedzieć się, jak inicjowanie dysków, gdy są one dodawane zobacz [Zarządzanie maszyn wirtualnych Azure za pomocą Menedżera zasobów i programu PowerShell](virtual-machines-windows-ps-manage.md).
Usuwanie dysku danych z maszyny | [Usuń AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - maszyn wirtualnych $vm-nazwę "disk_name"<BR></BR><BR></BR>Pobierz obiekt maszyn wirtualnych za pomocą Get-AzureRmVM. Uruchom aktualizację-AzureRmVM, aby zastosować zmiany konfiguracji do maszyn wirtualnych.
Dodawanie rozszerzenia do maszyn wirtualnych | [Zestaw AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-lokalizacji "azure_location" - VMName "vm_name"-nazwę "extension_name"-"publisher_name" w programie Publisher-typu "extension_type" - TypeHandlerVersion "#. #"-Ustawienia $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Uruchom to polecenie odpowiednie [informacje o konfiguracji](virtual-machines-windows-extensions-configuration-samples.md) rozszerzenia, które chcesz zainstalować.
Usuwanie rozszerzenia maszyn wirtualnych | [Usuń AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-- VMName "extension_name" Nazwa "vm_name"

## <a name="next-steps"></a>Następne kroki

- Zobacz podstawowe etapy tworzenia maszyny wirtualnej w [Tworzenie maszyn wirtualnych systemu Windows przy użyciu programu PowerShell i Menedżera zasobów](virtual-machines-windows-ps-create.md).

