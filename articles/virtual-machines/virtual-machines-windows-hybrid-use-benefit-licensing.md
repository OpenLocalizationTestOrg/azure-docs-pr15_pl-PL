<properties
   pageTitle="Korzyści Użyj Azure hybrydowego dla Windows Server | Microsoft Azure"
   description="Dowiedz się, jak zwiększyć swoimi usługami Windows Server Software Assurance do lokalnego licencje Azure"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Korzyści Użyj Azure hybrydowego dla systemu Windows Server

Dla klientów korzystających z systemem Windows Server z pakietem Software Assurance można wyświetlić swoje lokalnie licencji Windows Server Azure i uruchomić maszyny systemu Windows Server wirtualne platformy Azure kosztem ograniczona. Korzyści Użyj hybrydowych Azure umożliwia uruchomieniu maszyny wirtualne serwera systemu Windows Azure i w tylko pobieranie wystawiona dla kursu podstawowych obliczeń. Aby uzyskać więcej informacji zobacz [Azure hybrydowych Użyj korzyści Licencjonowanie strony](https://azure.microsoft.com/pricing/hybrid-use-benefit/). W tym artykule wyjaśniono, jak wdrożyć maszyny wirtualne serwera systemu Windows w Azure, aby użyć tego świadczenia licencjonowania.

> [AZURE.NOTE] Obrazy Azure Marketplace nie można używać do wdrożenia maszyny wirtualne serwera Windows wykorzystanie korzyści za pomocą hybrydowych Azure. Należy wdrożyć usługi maszyny wirtualne przy użyciu szablonów programu PowerShell lub Menedżer zasobów, aby poprawnie zarejestrować pośrednictwem usługi SMS jako kwalifikujące się do podstawowych obliczeń stopy dyskontowej.

## <a name="pre-requisites"></a>Wymagania wstępne
Istnieje kilka wymagań wstępnych, aby można było korzystać Azure hybrydowych Użyj korzyści dla systemu Windows Server maszyny wirtualne platformy Azure:

- Masz zainstalowany moduł Azure programu PowerShell
- Usługi Windows serwera wirtualnego dysku twardego przekazano do magazynowania Azure

### <a name="install-azure-powershell"></a>Instalowanie programu PowerShell Azure
Upewnij się, że masz [zainstalowany i skonfigurowany najnowszą Azure programu PowerShell](../powershell-install-configure.md). Nawet jeśli zamierzasz wdrożyć usługi maszyny wirtualne korzystania z szablonów Menedżera zasobów, nadal potrzebujesz Azure programu PowerShell zainstalowany przekazywania usługi Windows serwera wirtualnego dysku twardego (zobacz następny krok).

### <a name="upload-a-windows-server-vhd"></a>Przekazywanie systemu Windows Server wirtualnego dysku twardego

Aby wdrożyć maszyn wirtualnych systemu Windows Server w Azure, najpierw należy utworzyć wirtualny dysk twardy, zawierający do podstawowej kompilacji systemu Windows Server. Ten wirtualny dysk twardy musi być odpowiednio przygotowany przez Sysprep przed przekazaniem Azure. Możesz [Dowiedz się więcej o wymagania wirtualnego dysku twardego i procesu Sysprep](./virtual-machines-windows-upload-image.md) i [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Wykonywanie kopii zapasowej maszyn wirtualnych przed uruchomieniem narzędzia Sysprep. Gdy już przygotowane do wirtualnego dysku twardego, Przekaż wirtualny dysk twardy za pomocą konta magazynu platformy Azure `Add-AzureRmVhd` polecenia cmdlet w następujący sposób:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, program SharePoint Server i Dynamics mogą także skorzystać z licencjonowania Software Assurance. Nadal trzeba przygotować obraz systemu Windows Server, instalowania aplikacji składniki i odpowiednio dostarczając kluczy licencji, a następnie przekazywanie obrazu dysku Azure. Przejrzyj odpowiednią dokumentację uruchamiania Sysprep z aplikacji, takich jak [Zagadnienia dotyczące instalowania programu SQL Server za pomocą programu Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) lub [utworzyć obraz wzorcowy programu SharePoint Server 2016 (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Można również uzyskać więcej informacji na temat [przekazywania wirtualny dysk twardy Azure procesu](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] W tym artykule omówiono wdrażanie maszyny wirtualne serwera systemu Windows. Można także wdrożyć maszyny wirtualne klienta systemu Windows w taki sam sposób. W poniższych przykładach, możesz zastąpić `Server` z `Client` prawidłowo.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Wdrażanie maszyny za pośrednictwem Szybki Start programu PowerShell
Wdrażając maszyn wirtualnych usługi Windows Server przy użyciu programu PowerShell, mają dodatkowy parametr dla `-LicenseType`. Po umieszczeniu do wirtualnego dysku twardego przekazane do Azure, tworzenia nowego przy użyciu maszyn wirtualnych `New-AzureRmVM` i określić typ licencjonowania w następujący sposób:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Można [przeczytać bardziej szczegółowe instrukcje dotyczące wdrażania maszyn wirtualnych w Azure za pomocą programu PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) poniżej lub więcej bardziej szczegółowy przewodnik na różne kroki, aby [utworzyć maszyn wirtualnych systemu Windows przy użyciu programu PowerShell i Menedżera zasobów](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Wdrażanie maszyn wirtualnych za pomocą Menedżera zasobów
W szablonach Menedżera zasobów dodatkowy parametr dla `licenseType` można określić. Więcej o [tworzenia szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md). Po umieszczeniu do wirtualnego dysku twardego przekazane do Azure edytować szablon Menedżera zasobów, aby umieścić typu licencji dostawcę obliczeń i wdrażanie szablonu w zwykły sposób:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Upewnij się, że jest korzystanie z usługi maszyn wirtualnych licencjonowania korzyści
Po wdrożeniu programu maszyn wirtualnych za pośrednictwem metody wdrażania programu PowerShell lub Menedżer zasobów, sprawdź typu licencji z `Get-AzureRmVM` w następujący sposób:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Wynik jest podobny do następującego:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

To kontrastujący z następujących maszyn wirtualnych rozmieszczony bez korzyści Użyj hybrydowych Azure Licencjonowanie, takich jak maszyny wdrożony bezpośrednio z galerii Azure:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Szczegółowe instrukcje programu PowerShell

Następujące uzyskać szczegółowe instrukcje programu PowerShell Pokaż pełne wdrożenie maszyny. Można przeczytać dodatkowy kontekst rzeczywisty poleceń cmdlet i różnych składników tworzonych w [Tworzenie maszyn wirtualnych systemu Windows przy użyciu programu PowerShell i Menedżera zasobów](./virtual-machines-windows-ps-create.md). Kroków tworzenia grupa zasobów, konto miejsca do magazynowania i wirtualnej sieci, a następnie zdefiniuj usługi maszyn wirtualnych i na końcu Utwórz do maszyn wirtualnych.
 
Najpierw bezpieczne Uzyskaj poświadczenia, należy ustawić lokalizację i nazwę grupy zasobów:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Tworzenie publiczny adres IP:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definiowanie podsieci, NIC i VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Nazwa Twojej maszyn wirtualnych i Utwórz konfiguracji maszyn wirtualnych:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definiowanie system operacyjny:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Dodawanie usługi NIC do maszyn wirtualnych:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definiowanie konto przestrzeni dyskowej, aby użyć:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Przekazać do wirtualnego dysku twardego, odpowiednio przygotowana i dołączyć do swojego maszyn wirtualnych do użycia:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Na koniec Utwórz usługi maszyn wirtualnych i typ licencjonowania korzystanie z Azure hybrydowych Użyj korzyści:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [licencjonowaniu Azure hybrydowych Użyj korzyści](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Dowiedz się więcej na temat [korzystania z szablonów Menedżera zasobów](../azure-resource-manager/resource-group-overview.md).
