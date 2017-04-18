<properties
    pageTitle="Przenoszenie maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Przenoszenie maszyn wirtualnych systemu Windows do innej Azure grupy subskrypcji lub zasób w modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Przenoszenie maszyn wirtualnych systemu Windows do innej grupy Azure subskrypcji lub zasobów 

W tym artykule opisano sposób przenoszenia maszyn wirtualnych systemu Windows między grupami zasobów lub subskrypcji. Przechodzenie między subskrypcje mogą być bardzo przydatne pierwotnie utworzony maszyny w subskrypcji personal i teraz chcesz go przenieść do subskrypcji w firmie, aby kontynuować pracę.

> [AZURE.NOTE] Nowe identyfikatory zasobów zostanie utworzona w ramach Przenieś. Po przeniesieniu maszyn wirtualnych należy zaktualizować narzędzia i skrypty do użycia nowego zasobu identyfikatorów. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Przenoszenie maszyn wirtualnych przy użyciu programu Powershell

Aby przenieść maszyny wirtualnej do innej grupy zasobów, należy upewnić się, że możesz również przenieść wszystkie zasoby zależne. Aby użyć polecenia cmdlet Przenieś AzureRMResource, należy nazwę zasobu i typ zasobu. Możesz przejść z polecenia cmdlet AzureRMResource Znajdź.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Aby przenieść maszyny trzeba przenieść wiele zasobów. Firma Microsoft utwórz oddzielnych zmiennych dla każdego zasobu, a następnie na liście. W tym przykładzie zawiera większość podstawowych zasobów dla maszyny, ale można dodać więcej stosownie do potrzeb.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Aby przenieść zasobów do innej subskrypcji, należy dodać parametr **- DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Zostanie wyświetlony monit o potwierdzenie, że chcesz przenieść określonych zasobów. Wpisz **Y** , aby potwierdzić, że chcesz przenieść zasoby.

  
## <a name="next-steps"></a>Następne kroki

Wiele różnych typów zasobów można przenosić między subskrypcji i grup zasobów. Aby uzyskać więcej informacji zobacz [Przenoszenie zasobów do nowej grupy zasobów lub innej subskrypcji](../resource-group-move-resources.md).    