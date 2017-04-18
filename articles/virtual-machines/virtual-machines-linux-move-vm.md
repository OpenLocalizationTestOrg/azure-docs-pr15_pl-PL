<properties
    pageTitle="Przenoszenie maszyny Linux | Microsoft Azure"
    description="Przenoszenie maszyny Linux do innej Azure grupy subskrypcji lub zasób w modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Przenoszenie maszyny Linux do innej grupy subskrypcji lub zasobów

W tym artykule opisano sposób przenoszenia maszyny Linux między grupami zasobów lub subskrypcji. Przenoszenie maszyny między mogą być bardzo przydatne, jeśli utworzono maszyny osobistych subskrypcji i teraz chcesz go przenieść do subskrypcji firmy.

> [AZURE.NOTE] Nowe identyfikatory zasobów są tworzone jako część Przenieś. Po przeniesieniu maszyn wirtualnych musisz zaktualizować narzędzia i skrypty do użycia nowego zasobu identyfikatorów. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Przenoszenie maszyn wirtualnych za pomocą interfejsu wiersza polecenia Azure 

Aby pomyślnie przenieść maszyny, musisz przenieść maszyn wirtualnych i jego obsługi zasobów. Aby wyświetlić listę wszystkich zasobów w grupę zasobów i ich identyfikatorami za pomocą polecenia **Pokaż azure grupy** . Pomaga w potoku wynik tego polecenia do pliku, więc możesz skopiować i wkleić identyfikatory poleceń później.

    azure group show <resourceGroupName>

Aby przenieść maszyny i jego zasobów do innej grupy zasobów, użyj polecenia polecenie **Przenieś azure zasobów** . W poniższym przykładzie pokazano sposób przenoszenia maszyn wirtualnych i najczęściej używane zasoby, które wymaga. Firma Microsoft użyć parametru **-i** i przebiegu na liście przecinkami (bez spacji) identyfikatorów zasobów, aby przenieść.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Jeśli chcesz przejść do innej subskrypcji maszyn wirtualnych i jego zasobów, dodawanie **— miejsce docelowe subscriptionId & #60; destinationSubscriptionID & #62;** parametru do określenia subskrypcji elementu docelowego.

Jeśli pracujesz z wiersza polecenia na komputerze z systemem Windows, musisz dodać **$** przed nazwach zmiennych podczas ich zadeklarować. Nie jest to potrzebne w Linux.

Zostanie wyświetlony monit o potwierdzenie przenoszenie określonego zasobu. Wpisz **Y** , aby potwierdzić, że chcesz przenieść zasoby.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Następne kroki

Wiele różnych typów zasobów można przenosić między subskrypcji i grup zasobów. Aby uzyskać więcej informacji zobacz [Przenoszenie zasobów do nowej grupy zasobów lub innej subskrypcji](../resource-group-move-resources.md).    