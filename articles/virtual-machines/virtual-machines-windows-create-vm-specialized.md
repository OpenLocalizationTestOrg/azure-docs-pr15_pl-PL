<properties
    pageTitle="Tworzenie kopii maszyn wirtualnych usługi Windows | Microsoft Azure"
    description="Dowiedz się, jak utworzyć kopię z systemem Windows, w modelu wdrożenia Menedżera zasobów usługi specjalistyczne maszyn wirtualnych Azure."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Tworzenie maszyn wirtualnych z specjalistyczne wirtualnego dysku twardego

Tworzenie nowych maszyn wirtualnych dołączając specjalistyczne wirtualnego dysku twardego jako dysk systemu operacyjnego przy użyciu programu Powershell. Specjalistyczne wirtualny dysk twardy przechowuje konta użytkowników, aplikacji i innych danych o stanie ze swojego oryginalnego maszyn wirtualnych. 

Jeśli chcesz utworzyć maszyny z uniwersalny wirtualnego dysku twardego, zobacz [Tworzenie maszyn wirtualnych z uogólniony obrazu wirtualnego dysku twardego](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Tworzenie podsieci i vNet

Tworzenie vNet i podsieci, [wirtualną sieć](../virtual-network/virtual-networks-overview.md).

1. Tworzenie podsieci. W tym przykładzie tworzy podsieć o nazwie **mySubNet**w grupie zasobów **myResourceGroup**i ustawia prefiks adresu podsieci **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Tworzenie vNet. W tym przykładzie nazwa wirtualnej sieci **myVnetName**, lokalizację **Zachód Stanów Zjednoczonych**i prefiks adresu wirtualnej sieci **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Tworzenie publiczny adres IP i NIC

Aby umożliwić komunikację z maszyny wirtualnej w wirtualnej sieci, konieczne jest [publiczny adres IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i karty sieciowej.

1. Tworzenie publiczny adres IP. W tym przykładzie Nazwa publiczna adres IP jest równa **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Tworzenie karty sieciowej. W tym przykładzie nazwa sieciowa jest ustawiona na **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Tworzenie grupy zabezpieczeń sieci i reguły RDP

Aby można było zalogować się do usługi maszyn wirtualnych przy użyciu RDP, należy następująco skonfigurować regułę zabezpieczeń, zezwalające na dostęp RDP na porcie 3389. Ponieważ wirtualnego dysku twardego dla nowych maszyn wirtualnych został utworzony na podstawie istniejącego specjalistyczne Głosowa, po utworzeniu maszyn wirtualnych, możesz użyć istniejącego konta z komputera wirtualnych źródła mająca uprawnienia, aby zalogować się przy użyciu RDP.

W tym przykładzie nazwa NSG **myNsg** i nazwa reguły RDP do **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Aby uzyskać więcej informacji na temat reguł NSG i punkty końcowe zobacz [Otwieranie portów do maszyn wirtualnych w Azure przy użyciu programu PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Tworzenie konfiguracji maszyn wirtualnych

Ustawianie konfiguracji maszyn wirtualnych, aby dołączyć skopiowany wirtualnego dysku twardego jako OS wirtualny dysk twardy.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Jeśli masz dyski danych, które muszą być dołączone do maszyn wirtualnych, należy również dodać następujące czynności: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Dane i adresy URL dysku system operacyjny przypominać następującą tabelę: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. To w portalu można znaleźć, przechodząc do kontenera docelowego miejsca do magazynowania, klikając pozycję system operacyjny lub danych wirtualny dysk twardy, który został skopiowany, a następnie skopiować zawartość adresu URL.


## <a name="create-the-vm"></a>Tworzenie maszyn wirtualnych

Tworzenie maszyn wirtualnych, przy użyciu konfiguracji, które właśnie utworzony.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Jeśli to polecenie zakończyło się pomyślnie, zostanie wyświetlony wynik w następujący sposób:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Sprawdź, czy został utworzony maszyn wirtualnych 
 
Nowo utworzony maszyn wirtualnych powinna być widoczna w [Azure portal](https://portal.azure.com), w obszarze **Przeglądanie** > **maszyn wirtualnych**, albo za pomocą następujących poleceń programu PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Następne kroki

Aby zalogować się do nowego komputera wirtualnych, przejdź do maszyn wirtualnych w [portalu](https://portal.azure.com), kliknij przycisk **Połącz**i Otwórz plik Remote Desktop RDP. Za pomocą poświadczeń konta oryginalnego komputera wirtualnych do zalogowania się do nowego komputera wirtualnych. Aby uzyskać więcej informacji zobacz [jak połączyć się i zalogować się do Azure maszyn wirtualnych z systemem Windows](virtual-machines-windows-connect-logon.md).







