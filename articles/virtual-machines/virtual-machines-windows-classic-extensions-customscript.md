<properties
   pageTitle="Niestandardowy skrypt rozszerzenie maszyn wirtualnych systemu Windows | Microsoft Azure"
   description="Automatyzowanie zadań konfiguracji Azure maszyn wirtualnych przy użyciu rozszerzenia niestandardowego skryptu na uruchamianie skryptów programu PowerShell na zdalnym maszyn wirtualnych systemu Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Niestandardowy skrypt rozszerzenia dla maszyn wirtualnych systemu Windows

Ten artykuł zawiera omówienie sposobu używania rozszerzenia niestandardowego skryptu na maszyny wirtualne systemu Windows przy użyciu poleceń cmdlet środowiska PowerShell Azure interfejsy API zarządzania usługi Azure.

Rozszerzenia maszyn wirtualnych (maszyn wirtualnych) są utworzoną przez firmę Microsoft i Zaufani wydawcy innych firm, aby rozszerzyć funkcjonalność maszyn wirtualnych. Aby uzyskać omówienie rozszerzenia maszyn wirtualnych zobacz [rozszerzenia maszyn wirtualnych Azure i funkcje](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Omówienie rozszerzenia skryptu niestandardowego

Z rozszerzeniem niestandardowego skryptu dla systemu Windows można uruchamiać skryptów programu PowerShell na zdalnym maszyn wirtualnych bez konieczności logowania się do niego. Można uruchomić skrypty po zainicjowaniu obsługi administracyjnej maszyn wirtualnych lub w dowolnym momencie podczas zarządzania cyklem życia maszyn wirtualnych bez otwierania wszelkie dodatkowe porty. Najczęściej używane przypadków użycia uruchamiania rozszerzenia niestandardowego skryptu zawierać uruchomiony, instalowanie i konfigurowanie dodatkowego oprogramowania na maszyn wirtualnych, po zainicjowaniu obsługi administracyjnej.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Wymagania wstępne dotyczące uruchamiania rozszerzenia niestandardowego skryptu

1. Instalowanie <a href="http://azure.microsoft.com/downloads" target="_blank">Azure poleceń cmdlet</a> wersji 0.8.0 lub nowszej.
2. Jeśli chcesz skrypty do uruchomienia na istniejące maszyn wirtualnych, upewnij się, że Agent maszyn wirtualnych jest włączone maszyn wirtualnych. Jeśli nie jest zainstalowana, wykonaj następujące [kroki](virtual-machines-windows-classic-agents-and-extensions.md). Jeśli maszyn wirtualnych jest tworzona z Azure portal, Agent maszyn wirtualnych jest instalowana domyślnie.
3. Przekaż skrypty, które chcesz uruchomić na maszyn wirtualnych do magazynu Azure. Skrypty mogą pochodzić z kontenera jednego lub wielu kontenery miejsca do magazynowania.
4. Skrypt powinien tworzone, dlatego skryptu wpis, który jest uruchamiany przez rozszerzenie, zostanie uruchomiony innych skryptów.

## <a name="custom-script-extension-scenarios"></a>Niestandardowy skrypt rozszerzenia scenariuszy

### <a name="upload-files-to-the-default-container"></a>Przekazywanie plików do domyślnego kontenera

W poniższym przykładzie pokazano, jak można uruchomić skrypty na maszyn wirtualnych jeśli znajdują się w kontenerze miejsca do magazynowania domyślnego konta subskrypcji. Możesz przekazać skrypty do ContainerName. Domyślne konto miejsca do magazynowania można sprawdzać przy użyciu **Get-AzureSubscription — domyślne** polecenia.

Poniższy przykład tworzy maszyny, ale możesz również uruchomić tego samego scenariusza na istniejących maszyn wirtualnych.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Przekazywanie plików do kontenera magazynu innych niż domyślne

W tym scenariuszu przedstawiono sposób użycia kontenera miejsca do magazynowania niż domyślny w obrębie tej samej subskrypcji lub w innej subskrypcji dla przekazywanie skryptów i plików. W tym przykładzie przedstawiono istniejących maszyn wirtualnych, ale można przeprowadzić te same operacje podczas tworzenia maszyn wirtualnych.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Przekaż skrypty do wielu kontenerów na kontach innego miejsca do magazynowania

  Jeśli pliki skryptów są przechowywane w wielu kontenery, musisz podać adres URL podpisów (SA) pełny dostęp do udostępnionych plików uruchamiania skryptów.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Dodawanie numeru wewnętrznego niestandardowego skryptu z Azure portal

Przejdź do maszyn wirtualnych w <a href="https://portal.azure.com/ " target="_blank">Azure portal</a> i dodać rozszerzenie, określając plik skryptu do uruchomienia.

  ![Określ plik skryptu][5]


### <a name="uninstall-the-custom-script-extension"></a>Odinstalowywanie z rozszerzeniem niestandardowego skryptu

Po odinstalowaniu rozszerzenia niestandardowego skryptu z maszyn wirtualnych przy użyciu następującego polecenia.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Rozszerzenie niestandardowego skryptu za pomocą szablonów

Aby uzyskać informacje o korzystaniu z rozszerzeniem niestandardowego skryptu z szablonami Menedżera zasobów Azure, zobacz [Korzystanie z rozszerzenia niestandardowego skryptu dla systemu Windows maszyny wirtualne z szablonami Azure Menedżera zasobów](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
