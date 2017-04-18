<properties
   pageTitle="Wdrażanie maszyny wirtualne NIC wielu przy użyciu programu PowerShell w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne NIC wielu przy użyciu programu PowerShell w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Wdrażanie maszyny wirtualne NIC wielu przy użyciu programu PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Obecnie nie mogą mieć maszyny wirtualne z jednym NIC i maszyny wirtualne z kart w ten sam zestaw dostępności. W związku z tym należy wdrożyć serwerem wewnętrznym w różnych grupach zasobów niż inne składniki. Grupa zasobów o nazwie *IaaSStory* dla grupy zasobów głównym i *Wewnętrznej bazy danych IaaSStory* dla serwerów wewnętrznej za pomocą poniższych kroków.

## <a name="prerequisites"></a>Wymagania wstępne

Przed wdrożeniem serwerem wewnętrznym, należy wdrożyć grupy zasobów głównym z wszystkie niezbędne zasoby dla tego scenariusza. Aby wdrożyć te zasoby, wykonaj poniższe czynności.

1. Przejdź do [strony szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Na stronie szablonu z prawej strony **nadrzędnej, grupa zasobów**kliknij pozycję **Deploy Azure**.
3. W razie potrzeby zmień wartości parametrów, a następnie postępuj zgodnie z instrukcjami portal Azure Podgląd, aby wdrożyć grupa zasobów.

> [AZURE.IMPORTANT] Upewnij się, że nazwy konta przestrzeni dyskowej są unikatowe. Nie może zawierać zduplikowane magazynowania nazwy kont Azure.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Wdrażanie wewnętrznej maszyny wirtualne

Maszyny wirtualne wewnętrznej bazy danych zależy od utworzenia zasobów wymienionych poniżej.

- **Konto miejsca do magazynowania dla dyski danych**. Lepszą wydajność dyski danych na serwerze bazy danych będzie używany stanie stałym technologii dysk (SSD), która wymaga konta programu premium miejsca do magazynowania. Upewnij się, lokalizację Azure Wdroż obsługuje magazynowania premium.
- **Nic**. Każdy maszyn wirtualnych ma dwa nic, jedną dla dostępu do bazy danych, a drugi do zarządzania.
- **Ustawianie dostępności**. Wszystkie serwery mają bazy danych zostanie dodany do jednej dostępność ustawić, aby upewnić się, że co najmniej jeden z pośrednictwem SMS jest uruchomiony i uruchamiania podczas konserwacji.  

### <a name="step-1---start-your-script"></a>Krok 1 — Uruchom skrypt

Możesz pobrać pełny skrypt programu PowerShell używane [w tym miejscu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Wykonaj poniższe czynności, aby zmienić skrypt do pracy w środowisku.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Zmienianie wartości poniżej zmiennych na podstawie do istniejącej grupy zasobów wdrożona powyżej w [wymagania wstępne](#Prerequisites).

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Zmienianie wartości poniżej zmiennych na podstawie wartości, których chcesz używać dla wdrożenia wewnętrznej bazy danych.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Pobieranie istniejących zasobów potrzebne do wdrożenia.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 — Tworzenie niezbędnych zasobów dla swojego maszyny wirtualne

Należy utworzyć nową grupę zasobów, konto miejsca do magazynowania dysków danych i dostępności, ustaw dla wszystkich maszyny wirtualne. Alos konieczne poświadczenia konta lokalnego administratora dla każdego maszyn wirtualnych. Aby utworzyć te zasoby, wykonaj następujące czynności.

1. Tworzenie nowej grupy zasobów.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Tworzenie nowego konta miejsca do magazynowania premium w grupie zasobów utworzony w poprzednim przykładzie.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Tworzenie nowego zestawu dostępności.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Uzyskaj poświadczenia konta ma być używany dla każdego maszyn wirtualnych administrator lokalny.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Krok 3 — Tworzenie nic i wewnętrznej bazy danych maszyny wirtualne

Należy możliwie jak najwięcej maszyny wirtualne, a tworzenie niezbędne nic i maszyny wirtualne w pętli za pomocą pętli. Aby utworzyć nic i maszyny wirtualne, wykonaj następujące czynności.

1. Rozpoczynanie `for` pętli Powtarzaj polecenia służące do tworzenia maszyn wirtualnych i dwóch nic tyle razy, stosownie do potrzeb, na podstawie wartości z `$numberOfVMs` zmiennej.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Tworzenie NIC na potrzeby dostępu do bazy danych.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Tworzenie NIC używane dla dostępu zdalnego. Zwróć uwagę, jak tej karty Sieciowej ma NSG skojarzonego z nim.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Tworzenie `vmConfig` obiektu.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Utwórz dwa dyski danych na maszyn wirtualnych. Zwróć uwagę, że dyski danych znajdują się w przestrzeni dyskowej nominalnej utworzony wcześniej.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Konfigurowanie systemu operacyjnego, a obraz ma być używany dla maszyn wirtualnych.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Dodawanie dwóch sieciowe utworzony w poprzednim przykładzie do `vmConfig` obiektu.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Tworzenie dysku systemu operacyjnego i Utwórz maszyn wirtualnych. Powiadomienie o `}` kończące się `for` pętli.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Krok 4 — uruchamianie skryptu

Teraz, gdy pobranego i zmienić skrypt, w zależności od potrzeb runt on skrypt tworzenie wewnętrzna maszyny wirtualne bazy danych przy użyciu kart.

1. Zapisz skrypt i uruchom go z poziomu wiersza polecenia **programu PowerShell** lub **Środowiska PowerShell ISE**. Zostanie wyświetlona początkowej wynik, jak pokazano poniżej.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Po upływie kilku minut Wypełnij wierszu poświadczeń, a następnie kliknij **przycisk OK**. Wynik poniżej reprezentuje pojedynczy maszyn wirtualnych. Powiadomienie o całego procesu miał 8 minut.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
