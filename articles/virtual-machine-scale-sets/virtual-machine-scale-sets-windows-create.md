<properties
    pageTitle="Tworzenie maszyn wirtualnych skali zestawu przy użyciu programu PowerShell | Microsoft Azure"
    description="Tworzenie maszyn wirtualnych skali zestawu przy użyciu programu PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Utwórz zestaw przy użyciu programu PowerShell Azure skalę maszyn wirtualnych systemu Windows

Te kroki należy wykonać podejście wypełniania-w--pustych komórek do tworzenia zestawu skali Azure maszyn wirtualnych. Zobacz [Omówienie ustawia skali maszyn wirtualnych](virtual-machine-scale-sets-overview.md) , aby dowiedzieć się więcej na temat zestawów skali.

Należy trwa około 30 minut wykonaj kroki opisane w tym artykule.

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalowanie programu PowerShell Azure

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="step-2-create-resources"></a>Krok 2: Tworzenie zasobów

Tworzenie zasoby, które są wymagane przez swój nowy zestaw Skala.

### <a name="resource-group"></a>Grupa zasobów

Ustawianie skali maszyn wirtualnych muszą znajdować się w grupie zasobów.

1. Uzyskać listę dostępnych lokalizacji, w których możesz umieścić zasoby:

        Get-AzureLocation | Sort Name | Select Name

2. Wybierz lokalizację, która najbardziej Ci odpowiada, Zamień wartość **$locName** z tej nazwy lokalizacji, a następnie utwórz zmiennej:

        $locName = "location name from the list, such as Central US"

3. Zastąp wartość **$rgName** nazwę, którą chcesz użyć dla nowej grupy zasobów, a następnie utworzyć zmienną: 

        $rgName = "resource group name"
        
4. Tworzenie grupy zasobu:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Powinien zostać wyświetlony coś w tym przykładzie:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Konto magazynu

Konto miejsca do magazynowania jest używane przez komputer wirtualnych do przechowywania dysku systemu operacyjnego i dane diagnostyczne używane do skalowania. Jeśli to możliwe, jest najlepszym rozwiązaniem ma konta miejsca do magazynowania dla każdej maszyny wirtualnej utworzone w zestawie Skala. Jeśli nie jest to możliwe, planowanie nie więcej niż 20 maszyny wirtualne dla każdego konta miejsca do magazynowania. W przykładzie, w tym artykule przedstawiono trzy konta miejsca do magazynowania tworzony dla trzech maszyn wirtualnych.

1. Zamień wartość **$saName** nazwę konta magazynu. Testowanie nazwę unikatowość. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Jeśli odpowiedź jest **prawdziwe**, nazwę proponowana jest unikatowy.

3. Zamień wartość **$saType** typu konta miejsca do magazynowania, a następnie utwórz zmiennej:  

        $saType = "storage account type"
        
    Możliwe wartości to: Standard_LRS, Standard_GRS, Standard_RAGRS lub Premium_LRS.
        
4. Utwórz konto:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Powinien zostać wyświetlony coś w tym przykładzie:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Powtórz kroki od 1 do 4, aby utworzyć trzy konta miejsca do magazynowania, na przykład myst1, myst2 i myst3.

### <a name="virtual-network"></a>Wirtualna sieć

Wirtualna sieć jest wymagane na potrzeby maszyn wirtualnych w zestawie Skala.

1. Zastąp wartość **$subnetName** nazwę, którą chcesz użyć dla tej podsieci wirtualnej sieci, a następnie utworzyć zmienną: 

        $subnetName = "subnet name"
        
2. Tworzenie konfiguracji podsieci:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Prefiks adresu mogą się różnić w wirtualnej sieci.

3. Zastąp wartość **$netName** nazwę, którą chcesz używać dla wirtualnej sieci, a następnie utworzyć zmienną: 

        $netName = "virtual network name"
        
4. Tworzenie wirtualnych sieci:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfiguracja zestawu skali

Masz wszystkich zasobów, których potrzebujesz dla skali konfiguracji, więc warto go utworzyć.  

1. Zastąp wartość **$ipName** nazwę, którą chcesz użyć w konfiguracji adresów IP, a następnie utworzyć zmienną: 

        $ipName = "IP configuration name"
        
2. Tworzenie konfiguracji IP:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Zastąp wartość **$vmssConfig** nazwę, którą chcesz używać konfiguracji zestawu skali, a następnie utworzyć zmienną:   

        $vmssConfig = "Scale set configuration name"
        
3. Tworzenie konfiguracji zestawu skali:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    W tym przykładzie przedstawiono zestawu skalę tworzony trzy maszyn wirtualnych. Zobacz [Omówienie ustawia skali maszyn wirtualnych](virtual-machine-scale-sets-overview.md) szczegółowe informacje na temat możliwości zestawów Skala. W tym kroku zawiera również ustawianie rozmiaru (nazywane SkuName) maszyn wirtualnych w zestawie. Aby znaleźć odpowiedni rozmiar, która odpowiada Twoim potrzebom, spójrz na [rozmiarów maszyn wirtualnych](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Dodawanie Konfiguracja interfejsu sieciowego do konfiguracji zestawu skali:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Powinien zostać wyświetlony coś w tym przykładzie:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>System operacyjny profilu

1. Zastąp wartość **$computerName** prefiks nazwy komputera, którego chcesz użyć, a następnie utwórz zmiennej: 

        $computerName = "computer name prefix"
        
2. Zamień wartość **$adminName** nazwę konta administratora w przypadku maszyn wirtualnych, a następnie utwórz zmiennej:

        $adminName = "administrator account name"
        
3. Zamień wartość **$adminPassword** hasło konta, a następnie utwórz zmiennej:

        $adminPassword = "password for administrator accounts"
        
4. Tworzenie profilu system operacyjny:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Profil miejsca do magazynowania

1. Zastąp wartość **$storageProfile** nazwę, którą chcesz używać dla profilu miejsca do magazynowania, a następnie utworzyć zmienną:  

        $storageProfile = "storage profile name"
        
2. Tworzenie zmiennych, które definiują obraz, aby użyć:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Aby znaleźć informacje o innych obrazów do użycia, spójrz na [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów przy użyciu programu Windows PowerShell i polecenie Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Zamień wartość **$vhdContainers** z listy, która zawiera ścieżki miejsce, w którym są przechowywane wirtualnych dyskach twardych, takie jak "https://mystorage.blob.core.windows.net/vhds", a następnie utwórz zmiennej:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Tworzenie profilu miejsca do magazynowania:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Ustawianie skali maszyn wirtualnych

Ponadto można utworzyć zestaw Skala.

1. Zamień wartość **$vmssName** nazwę zestawu skali maszyn wirtualnych, a następnie utwórz zmiennej:

        $vmssName = "scale set name"
        
2. Tworzenie zestawu skali:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Powinien zostać wyświetlony podobny do tego przykładu, która zawiera pomyślnego wdrożenia:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Krok 3: Przegląd zasobów

Eksplorowanie zestaw skali maszyn wirtualnych utworzony przy użyciu następujących zasobów:

- Azure portal - ograniczoną ilość informacji jest dostępny, za pomocą portalu.
- [Eksplorator zasobów azure](https://resources.azure.com/) — to narzędzie jest najlepszy dla uzyskiwanie informacji o bieżącym stanie zestawu Skala.
- Azure PowerShell — Użyj tego polecenia, aby uzyskać informacje o:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Następne kroki

- Zarządzanie zestawu skali właśnie utworzony przy użyciu informacji w [Zarządzanie maszyn wirtualnych w zestawie skali maszyn wirtualnych](virtual-machine-scale-sets-windows-manage.md)
- Należy rozważyć skonfigurowanie automatyczne skalowanie na skalę ustawić przy użyciu informacji w [Ustawia automatyczne skalowanie i maszyn wirtualnych skali](virtual-machine-scale-sets-autoscale-overview.md)
- Dowiedz się więcej o skalowanie pionowe przeglądając [Pionowa autoscale z zestawami skali maszyn wirtualnych](virtual-machine-scale-sets-vertical-scale-reprovision.md)
