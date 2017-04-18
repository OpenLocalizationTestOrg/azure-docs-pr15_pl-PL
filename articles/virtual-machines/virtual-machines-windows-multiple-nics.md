<properties
   pageTitle="Tworzenie maszyn wirtualnych systemu Windows przy użyciu kart | Microsoft Azure"
   description="Dowiedz się, jak utworzyć maszyn wirtualnych systemu Windows przy użyciu kart dołączone do niej przy użyciu szablonów programu PowerShell Azure lub Menedżer zasobów."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Tworzenie maszyn wirtualnych systemu Windows za pomocą kart
Możesz utworzyć maszyny wirtualnej (maszyn wirtualnych) platformy Azure zawierającej wiele interfejsów wirtualnych sieci (NIC) dołączone do niego. Typowy scenariusz będzie mieć różnych podsieci łączności frontonu i wewnętrznej lub sieci przeznaczone do rozwiązania monitorowania lub kopii zapasowej. Ten artykuł zawiera szybkie polecenia do tworzenia maszyn wirtualnych z kart dołączone do niego. Aby uzyskać szczegółowe informacje, tym ich tworzenie kart w ramach własnych skryptów programu PowerShell więcej informacji o [wdrażaniu maszyny wirtualne NIC wielokrotne](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Różne [rozmiary maszyn wirtualnych](virtual-machines-windows-sizes.md) obsługi różną liczbę kart sieciowych, więc odpowiednio rozmiar do maszyn wirtualnych.

>[AZURE.WARNING] Po utworzeniu maszyny - nic nie można dodać do istniejącego maszyn wirtualnych, należy dołączyć kart. Możesz [utworzyć maszyny oparte na oryginalnym dyskach wirtualnych](virtual-machines-windows-vhd-copy.md) i tworzyć kart wdrażanie maszyn wirtualnych.

## <a name="create-core-resources"></a>Tworzenie podstawowego zasobów
Upewnij się, że masz [najnowszą Azure programu PowerShell zainstalowaniu i skonfigurowaniu](../powershell-install-configure.md). Zaloguj się do konta Azure:

```powershell
Login-AzureRmAccount
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `mystorageaccount`, i `myVM`.

Najpierw należy utworzyć grupy zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUs` lokalizacji:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Utwórz konto miejsca do magazynowania do przechowywania pośrednictwem usługi SMS. Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Tworzenie wirtualnych sieci i podsieci
Definiowanie dwóch podsieci wirtualną sieć — jedną dla zewnętrznej ruchu i jedną dla ruchu wewnętrznej. W poniższym przykładzie zdefiniowano dwa podsieci, o nazwie `mySubnetFrontEnd` i `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Tworzenie wirtualnych sieci i podsieci. Poniższy przykład tworzy wirtualną sieć o nazwie `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Tworzenie kart
Utwórz dwa nic, dołączania jednego NIC do zewnętrznej podsieci i jedna karta NIC do podsieci wewnętrznej. W poniższym przykładzie tworzone dwa nic, o nazwie `myNic1` i `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Zwykle można także tworzyć [grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) lub [Usługa równoważenia obciążenia](../load-balancer/load-balancer-overview.md) , aby pomóc w zarządzaniu i rozłożenie ruch w Twojej maszyny wirtualne. Artykuł [bardziej szczegółowe NIC wiele maszyn wirtualnych](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) prowadzi użytkownika przez tworzenie grupy zabezpieczeń sieci i przypisywanie nic.


## <a name="create-the-virtual-machine"></a>Tworzenie maszyny wirtualnej
Teraz zacząć tworzyć konfiguracji maszyn wirtualnych. Każdy rozmiar pamięci Wirtualnej ma limit całkowitej liczby nic, które można dodać do maszyny. Dowiedz się więcej na temat [rozmiarów maszyn wirtualnych systemu Windows](virtual-machines-windows-sizes.md). 

Najpierw Ustaw poświadczenia maszyn wirtualnych `$cred` zmiennej w następujący sposób:

```powershell
$cred = Get-Credential
```

W poniższym przykładzie zdefiniowano maszyny o nazwie `myVM` i używa rozmiar pamięci Wirtualnej, obsługującego do dwóch nic (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Tworzenie pozostałą część pliku config maszyn wirtualnych. Poniższy przykład tworzy maszyn wirtualnych systemu Windows Server 2012 R2:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Dołączanie dwóch sieciowe zostało wcześniej utworzone:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Konfigurowanie miejsca do magazynowania i dysku wirtualnego dla swojego nowa maszyna wirtualna:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Na koniec Utwórz maszyny:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Tworzenie kart przy użyciu szablonów Menedżera zasobów
Azure szablony Menedżera zasobów umożliwia definiowanie środowiska deklaracyjnych pliki JSON. [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)można czytać. Szablony Menedżera zasobów umożliwia tworzenie wielu wystąpień zasobu podczas wdrażania, takich jak tworzenie kart. Określ liczbę wystąpień, aby utworzyć za pomocą *kopiowania* :

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Dowiedz się więcej o [tworzeniu wielu wystąpień za pomocą *kopiowania*](../resource-group-create-multiple.md). 

Można również użyć `copyIndex()` następnie dołączyć numeru do nazwy zasobów, która umożliwia tworzenie `myNic1`, `MyNic2`itp. Poniżej przedstawiono przykład dołączanie wartość indeksu:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Można przeczytać pełny przykład [tworzenia kart przy użyciu szablonów Menedżera zasobów](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Następne kroki
Upewnij się przejrzeć [rozmiarów maszyn wirtualnych systemu Windows](virtual-machines-windows-sizes.md) , podczas próby Tworzenie maszyn wirtualnych za pomocą kart. Należy zwrócić uwagę na maksymalna liczba nic obsługuje każdego rozmiar pamięci Wirtualnej. 

Należy pamiętać, że nie możesz dodać dodatkowe nic do istniejącego maszyn wirtualnych, należy utworzyć wszystkie sieciowe podczas wdrażania maszyn wirtualnych. Zwrócić uwagę podczas planowania wdrożeń aby upewnić się, że wszystkie łączność sieciowa wymagane od początku.