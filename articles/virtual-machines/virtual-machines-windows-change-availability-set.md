<properties
    pageTitle="Zmienianie zestawu dostępność maszyny wirtualne | Microsoft Azure"
    description="Dowiedz się, jak zmienić dostępność ustawieniem maszyn wirtualnych przy użyciu programu PowerShell Azure i model wdrożenia Menedżera zasobów."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>Zmienianie dostępności, ustaw dla maszyn wirtualnych systemu Windows

W poniższej procedurze opisano, jak zmienić zestaw dostępność maszyn wirtualnych przy użyciu programu PowerShell Azure. Maszyny można dodawać tylko dostępności Ustawianie po jego utworzeniu. Aby zmienić dostępność, należy usunąć i ponownie utworzyć maszyny wirtualnej. 

## <a name="change-the-availability-set-using-powershell"></a>Zmienianie dostępności zestaw przy użyciu programu PowerShell

1. Przechwytywanie następujące kluczowe informacje z maszyn wirtualnych do zmodyfikowania.

    Nazwa maszyn wirtualnych
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    Rozmiar pamięci Wirtualnej
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Interfejsu sieci podstawowej sieci i opcjonalnie interfejsów, jeśli znajdują się one na maszyn wirtualnych
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    Profil dysku systemu operacyjnego

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Profile dysku dla każdego dysku danych 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    Zainstalowano rozszerzenia maszyn wirtualnych 
    
    ```powershell
    $vm.Extensions
    ```

2. Usuwanie maszyn wirtualnych nie usuwając dysków lub interfejsów sieci.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Tworzenie dostępności, jeśli jeszcze nie istnieje

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Ponowne tworzenie maszyn wirtualnych, przy użyciu nowego zestawu dostępności

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Dodawanie dyski danych i rozszerzenia. Aby uzyskać więcej informacji zobacz [Dołączanie dysku danych do maszyn wirtualnych](virtual-machines-windows-attach-disk-portal.md) i [Rozszerzenia konfiguracji próbki](virtual-machines-windows-extensions-configuration-samples.md). Dyski danych i rozszerzenia można dodać do maszyn wirtualnych, przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure.

## <a name="example-script"></a>Przykładowy skrypt

Poniższy skrypt zawiera przykład gromadzenia wymagane informacje, usunięcie oryginalnej maszyn wirtualnych i odtwarzanie go w nowy zestaw dostępności.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Następne kroki

Dodawanie dodatkowego miejsca do magazynowania do swojego maszyn wirtualnych, dodając dodatkowego [dysku danych](virtual-machines-windows-attach-disk-portal.md).

