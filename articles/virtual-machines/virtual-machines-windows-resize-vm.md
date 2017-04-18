<properties
    pageTitle="Zmienianie rozmiaru maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Zmienianie rozmiaru utworzone w modelu wdrożenia Menedżera zasobów, przy użyciu programu Powershell Azure maszyny wirtualnej systemu Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Zmienianie rozmiaru maszyn wirtualnych systemu Windows

W tym artykule przedstawiono sposób zmieniania rozmiaru maszyn wirtualnych systemu Windows, utworzone przy użyciu programu Powershell Azure modelu wdrożenia Menedżera zasobów.

Po utworzeniu maszyny wirtualnej (maszyn wirtualnych) można skalować maszyn wirtualnych w górę lub w dół, zmieniając rozmiar pamięci Wirtualnej. W niektórych przypadkach należy najpierw cofnąć maszyn wirtualnych. To może się zdarzyć, jeśli nowy rozmiar nie jest dostępna w klastrze sprzętu obecnie obsługujący maszyn wirtualnych.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Zmienianie rozmiaru maszyn wirtualnych systemu Windows nie ma w zestawie dostępności

1. Lista rozmiarów maszyn wirtualnych, które są dostępne w klastrze sprzętu miejsce, w którym znajduje się maszyn wirtualnych. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Jeśli wymieniono Zmień rozmiar okna, uruchom następujące polecenia w celu zmiany rozmiaru maszyn wirtualnych. Jeśli żądanego rozmiaru nie ma na liście, przejdź do kroku 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Jeśli żądanego rozmiaru nie ma na liście, uruchom następujące polecenia, aby zwolnić Głosowa, zmienić jego rozmiar, a następnie ponownie uruchom maszyn wirtualnych.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Cofanie przydziału maszyn wirtualnych aktualizacje dynamiczne adresy IP przypisane do maszyn wirtualnych. Nie wpływa na dyskach systemu operacyjnego i danych. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Zmienianie rozmiaru maszyn wirtualnych systemu Windows w zestawie dostępności

Jeśli nowy rozmiar maszyn wirtualnych w zestawie dostępność nie jest dostępna w klastrze sprzętu obecnie hostingu maszyn wirtualnych, wszystkie maszyny wirtualne w zestawie dostępność będzie konieczne przydzielenia w celu zmiany rozmiaru maszyn wirtualnych. Możesz również może być konieczne zaktualizowanie rozmiar inne maszyny wirtualne dostępności, po jednym maszyn wirtualnych został zmieniony. Aby zmienić rozmiar maszyn wirtualnych w zestawie dostępność, wykonaj następujące czynności.

1. Lista rozmiarów maszyn wirtualnych, które są dostępne w klastrze sprzętu miejsce, w którym znajduje się maszyn wirtualnych.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Jeśli wymieniono Zmień rozmiar okna, uruchom następujące polecenia w celu zmiany rozmiaru maszyn wirtualnych. Jeśli nie ma na liście, przejdź do kroku 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Jeśli żądanego rozmiaru nie ma na liście, przejdź do następujące czynności, aby cofnąć wszystkie maszyny wirtualne w zestawie dostępność, zmienianie rozmiaru maszyny wirtualne i uruchomić je ponownie.

4.  Zatrzymanie wszystkich maszyny wirtualne w zestawie dostępności.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Zmienianie rozmiaru i uruchom ponownie maszyny wirtualne w zestawie dostępności.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać dodatkowe skalowalność uruchamianie wielu wystąpień maszyn wirtualnych i skalowania. Aby uzyskać więcej informacji zobacz [Automatyczne skalowanie komputerów systemu Windows w zestawie maszyn wirtualnych Skala](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



