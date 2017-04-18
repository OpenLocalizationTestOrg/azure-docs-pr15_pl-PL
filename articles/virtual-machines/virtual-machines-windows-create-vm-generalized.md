<properties
    pageTitle="Tworzenie maszyn wirtualnych z uniwersalny wirtualnego dysku twardego | Microsoft Azure"
    description="Dowiedz się, jak utworzyć maszyny wirtualnej systemu Windows z uogólniony obrazu wirtualnego dysku twardego przy użyciu programu PowerShell Azure, w modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Tworzenie maszyn wirtualnych z obraz uogólniony wirtualnego dysku twardego

Obraz uogólniony wirtualnego dysku twardego miał wszystkie informacje osobiste konto usunąć za pomocą [narzędzia Sysprep](virtual-machines-windows-generalize-vhd.md). Można utworzyć uniwersalny wirtualnego dysku twardego, uruchamiając Sysprep na Głosowa lokalnego, [przekazując wirtualny dysk twardy Azure](virtual-machines-windows-upload-image.md)lub narzędzie Sysprep zostanie uruchomione na istniejące Azure maszyn wirtualnych, a następnie [Kopiowanie wirtualnego dysku twardego](virtual-machines-windows-vhd-copy.md).

Jeśli chcesz utworzyć maszyny z specjalistyczne wirtualny dysk twardy, zobacz [Tworzenie maszyn wirtualnych z specjalistyczne wirtualnego dysku twardego](virtual-machines-windows-create-vm-specialized.md).

Najszybszym sposobem na tworzenie maszyn wirtualnych z uogólniony wirtualny dysk twardy jest użycie [szablon szybki start] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Wymagania wstępne

Jeśli zamierzasz użyć wirtualnego dysku twardego przekazane z Głosowa lokalnego, podobny do utworzony przy użyciu funkcji Hyper-V, należy upewnić się wykonaniu wskazówek dotyczących dojazdu przygotowywanie [Wirtualnego dysku twardego systemu Windows do przekazania Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Oba przekazane wirtualnych dysków twardych i wirtualnych dysków twardych istniejących Azure maszyn wirtualnych muszą uogólniony, aby można było tworzyć maszyny przy użyciu tej metody. Aby uzyskać więcej informacji zobacz [Generalize maszyny wirtualnej systemu Windows za pomocą programu Sysprep](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Ustawianie identyfikatora URI wirtualnego dysku twardego

Identyfikator URI dla wirtualnego dysku twardego umożliwia ma format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**VHD. W tym przykładzie wirtualny dysk twardy o nazwie **myVHD** znajduje się w **mystorageaccount** konta miejsca do magazynowania w kontenerze **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Tworzenie wirtualnych sieci

Tworzenie vNet i podsieci, [wirtualną sieć](../virtual-network/virtual-networks-overview.md).


1. Tworzenie podsieci. Poniższy przykład tworzy podsieć o nazwie **mySubnet** w grupie zasobów **myResourceGroup** z prefiksem adresu **10.0.0.0/24**.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Tworzenie wirtualnych sieci. Poniższy przykład tworzy wirtualną sieć o nazwie **myVnet** w lokalizacji **USA Zachodnia** z prefiksem adresu **10.0.0.0/16**.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Tworzenie publicznego adresu i sieci interfejsu IP

Aby umożliwić komunikację z maszyny wirtualnej w wirtualnej sieci, konieczne jest [publiczny adres IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i karty sieciowej.

1. Tworzenie publiczny adres IP. W tym przykładzie tworzy publiczny adres IP o nazwie **myPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Tworzenie karty sieciowej. W tym przykładzie tworzy NIC o nazwie **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Tworzenie grupy zabezpieczeń sieci i reguły RDP

Aby można było zalogować się do usługi maszyn wirtualnych przy użyciu RDP, należy następująco skonfigurować regułę zabezpieczeń, który umożliwia dostęp RDP na porcie 3389. 

W tym przykładzie tworzy NSG o nazwie **myNsg** , który zawiera regułę o nazwie **myRdpRule** , który umożliwia ruch RDP za pośrednictwem portu 3389. Aby uzyskać więcej informacji o NSGs zobacz [Otwieranie portów do maszyn wirtualnych w Azure przy użyciu programu PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Tworzenie zmiennej wirtualnej sieci

Tworzenie zmiennej złożonym wirtualnej sieci. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Tworzenie maszyn wirtualnych

Poniższy skrypt programu PowerShell przedstawiono sposoby ustawiania konfiguracji maszyn wirtualnych i użyć przekazanego obrazu maszyn wirtualnych jako źródła dla nowej instalacji.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Sprawdź, czy został utworzony maszyn wirtualnych 

Po zakończeniu powinna być widoczna nowo utworzonego maszyn wirtualnych w [Azure portal](https://portal.azure.com) w obszarze **Przeglądanie** > **maszyn wirtualnych**, albo za pomocą następujących poleceń programu PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Następne kroki

Aby zarządzać do nowego komputera wirtualnych z Azure programu PowerShell, zobacz [Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell](virtual-machines-windows-ps-manage.md).


