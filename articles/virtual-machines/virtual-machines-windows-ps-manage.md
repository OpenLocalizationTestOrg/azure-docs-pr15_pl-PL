<properties
    pageTitle="Zarządzanie maszyny wirtualne za pomocą Menedżera zasobów i programu PowerShell | Microsoft Azure"
    description="Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Zarządzanie maszyn wirtualnych Azure za pomocą Menedżera zasobów i programu PowerShell

## <a name="install-azure-powershell"></a>Instalowanie programu PowerShell Azure
 
Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="set-variables"></a>Ustawianie zmiennych

Wszystkie polecenia w artykule wymagają nazwę grupy zasobów, w której znajduje się maszyny wirtualnej i nazwę maszyny wirtualnej do zarządzania. Zastąp wartość **$rgName** nazwą grupy zasobów, która zawiera maszyny wirtualnej. Zamień wartość **$vmName** nazwę maszyn wirtualnych. Tworzenie zmiennych.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Wyświetlanie informacji o maszyny wirtualnej

Uzyskiwanie informacji maszyn wirtualnych.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Zwraca wartość inną jak w tym przykładzie:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Zatrzymywanie maszyny wirtualnej

Zatrzymanie uruchomionego maszyny wirtualnej.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Ktoś poprosi Cię o potwierdzenie:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Wprowadź **Y** , aby zatrzymać maszyny wirtualnej.

Po kilku minutach zwraca coś w tym przykładzie:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Rozpoczynanie maszyny wirtualnej

Rozpocznij maszyny wirtualnej, jeśli zostanie zatrzymana.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Po kilku minutach zwraca coś w tym przykładzie:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Jeśli chcesz ponownie uruchomić maszyny wirtualnej, która jest już uruchomiona, użyj **AzureRmVM ponowne uruchomienie** opisane dalej.

## <a name="restart-a-virtual-machine"></a>Uruchom ponownie komputer wirtualnych

Uruchom ponownie uruchomionej maszyny wirtualnej.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Zwraca wartość inną jak w tym przykładzie:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Usuwanie maszyny wirtualnej

Usuwanie maszyny wirtualnej.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Możesz użyć **-życie** parametr, aby pominąć monit o potwierdzenie.

Jeśli nie używasz parametr życie wyświetleniu pytania o potwierdzenie:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Zwraca wartość inną jak w tym przykładzie:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Aktualizowanie maszyny wirtualnej

W tym przykładzie pokazano, jak zaktualizować rozmiar maszyny wirtualnej.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Zwraca wartość inną jak w tym przykładzie:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Aby uzyskać listę dostępnych rozmiarów dla maszyny wirtualnej, zobacz [rozmiarów maszyn wirtualnych w Azure](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Dodawanie dysku danych maszyn wirtualnych

W tym przykładzie pokazano sposób dodać dyskiem dane do istniejącego maszyn wirtualnych.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Dysk, na którym możesz dodać nie został zainicjowany. Aby zainicjować dysku, można zalogować się do niego i używać dysku zarządzania. Jeśli zainstalowano program WinRM i certyfikatu na nim po jego utworzeniu, można użyć zdalnej obsługi programu PowerShell zainicjować dysk. Umożliwia także rozszerzeniem skrypt niestandardowy: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Plik skryptu mogą zawierać podobny do tego kodu, aby zainicjować dyski:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Następne kroki

W przypadku problemów z wdrożeniu może przeglądać [wdrożeń grupa zasobów Rozwiązywanie problemów z Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
