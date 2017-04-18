<properties
    pageTitle="Zarządzanie maszyny wirtualne w zestawie skali maszyn wirtualnych | Microsoft Azure"
    description="Zarządzanie maszyn wirtualnych w skali maszyn wirtualnych zestaw przy użyciu programu PowerShell Azure."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Zarządzanie maszyn wirtualnych w zestawie skali maszyn wirtualnych

Zarządzanie maszyn wirtualnych w zestawie skali maszyn wirtualnych przy użyciu zadania w tym artykule.

Większość zadań, które obejmują zarządzanie maszyny wirtualnej w zestawie skali wymagają, że wiesz, identyfikator wystąpienia komputera, którego chcesz zarządzać. Korzystanie z [Eksploratora zasobów Azure](https://resources.azure.com) , aby znaleźć identyfikator wystąpienia maszyny wirtualnej w zestawie Skala. Aby sprawdzić stan zadania, które można zakończyć są również użyć Eksploratora zasobów.

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="display-information-about-a-scale-set"></a>Wyświetlanie informacji na temat zestawu skali

Można uzyskać ogólne informacje o zestawie skali, określany także jako widok wystąpienie. Lub można uzyskać bardziej szczegółowe informacje, takie jak informacje o zasobach w zestawie Skala.

Zamienić wartości cudzysłowie nazwę lub grupa zasobów i skala Ustaw, a następnie uruchom polecenie:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Zwraca mniej więcej tak:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Zamień wartości cudzysłowie nazwę zestawu grupy i skalę zasobów. Zamienianie *#* identyfikatorem wystąpienia maszyny wirtualnej, którą chcesz uzyskać informacje, a następnie uruchom go:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Zwraca wartość inną jak w tym przykładzie:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Rozpoczynanie maszyny wirtualnej w zestawie skali

Zamień wartości cudzysłowie nazwę zestawu grupy i skalę zasobów. Zamienianie *#* z identyfikatorem maszyny wirtualnej, który chcesz uruchomić, a następnie uruchom go:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

W Eksploratorze zasobów widać, że stan wystąpienia jest **uruchomiony**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Możesz rozpocząć maszyn wirtualnych w skali określonych przez nie przy użyciu parametru - identyfikator wystąpienia.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Zatrzymywanie maszyny wirtualnej w zestawie skali

Zamień wartości cudzysłowie nazwę zestawu grupy i skalę zasobów. Zamienianie *#* z identyfikatorem maszyny wirtualnej, którą chcesz zatrzymać, a następnie uruchom go:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

W Eksploratorze zasobów widać, że stan wystąpienia jest **alokację**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Aby zatrzymać maszyny wirtualnej i nie można jej cofnąć, należy użyć parametru - StayProvisioned. Możesz wyłączyć maszyn wirtualnych w zestawie, nie korzystając z parametru - identyfikator wystąpienia.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Uruchom ponownie maszyny wirtualnej w zestawie skali

Zamień cudzysłowie wartości nazwę swojej grupy zasobów i zestaw Skala. Zamienianie *#* z identyfikatorem maszyny wirtualnej, który chcesz ponownie uruchomić, a następnie uruchom go:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Można uruchomić ponownie maszyn wirtualnych w zestawie, nie korzystając z parametru - identyfikator wystąpienia.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Usuwanie maszyny wirtualnej z zestawu skali

Zamień cudzysłowie wartości nazwę swojej grupy zasobów i zestaw Skala. Zamienianie *#* z identyfikatorem maszyny wirtualnej, który chcesz usunąć, a następnie uruchom go:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Możesz usunąć zestaw skali maszyn wirtualnych w całym dokumencie, nie przy użyciu parametru - identyfikator wystąpienia.

## <a name="change-the-capacity-of-a-scale-set"></a>Zmienianie możliwości zestawu skali

Można dodać lub usunąć maszyn wirtualnych, zmieniając możliwości zestawu. Pobierz zestaw skali, który chcesz zmienić, Ustawianie wydajności, która ma się znaleźć, a następnie zaktualizuj skali zestaw z możliwości nowy. W tych poleceniach należy zamienić wartości cudzysłowie z nazwy grupy zasobów i Ustawianie skali.

  $vmss = get-AzureRmVmss - ResourceGroupName "Nazwa grupy zasobów" - VMScaleSetName "Nazwa zestawu skali" $vmss.sku.capacity = 5 aktualizacji AzureRmVmss - ResourceGroupName "Nazwa grupy zasobów"-Nazwa "skalowanie Nazwa zestawu" - VirtualMachineScaleSet $vmss 

Usunięcie maszyn wirtualnych z zestawu skali, najpierw są usuwane maszyn wirtualnych identyfikatorami najwyższe.
