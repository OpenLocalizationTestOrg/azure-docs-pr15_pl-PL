<properties 
    pageTitle="Ponownie wdróż maszyn wirtualnych systemu Windows | Microsoft Azure" 
    description="W tym artykule opisano, jak ponownie rozmieścić maszyn wirtualnych systemu Windows zmniejszeniu problemy z połączeniem RDP." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ponownie wdróż maszyny wirtualnej do nowego węzła Azure

Jeśli zostały przeciwległych trudności Rozwiązywanie problemów z pulpitu zdalnego (RDP) połączenia lub aplikacji programu access oparte na systemie Windows Azure maszyn wirtualnych (Głosowa), ponowne rozmieszczanie maszyn wirtualnych może pomóc. Podczas ponownego wdrożenia maszyny maszyn wirtualnych jest przenoszony do nowego węzła w ramach infrastruktury Azure, a następnie włącza ją ponownie, zachowując wszystkie opcje konfiguracji i skojarzonych z nimi zasobów. W tym artykule pokazano, jak ponownie rozmieścić maszyny przy użyciu programu PowerShell Azure lub Azure portal.

> [AZURE.NOTE] Po ponownego wdrożenia maszyny dysku tymczasowym zostaje utracony i dynamiczne adresy IP skojarzonego z interfejsem wirtualnej sieci zostaną zaktualizowane. 

## <a name="using-azure-powershell"></a>Przy użyciu programu PowerShell Azure

Upewnij się, że masz najnowszą Azure programu PowerShell 1.x zainstalowana na tym komputerze. Aby uzyskać więcej informacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Użyj tego polecenia programu PowerShell Azure ponownie rozmieścić komputera wirtualnej:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Następne kroki
Jeśli masz problemów dotyczących łączenia się z maszyn wirtualnych można znaleźć pomoc w [rozwiązywaniu problemów z połączeniami RDP](virtual-machines-windows-troubleshoot-rdp-connection.md) lub [RDP szczegółowe kroki rozwiązywania problemów](virtual-machines-windows-detailed-troubleshoot-rdp.md). Jeśli nie masz dostępu do uruchamiania aplikacji na swojej maszyn wirtualnych, można również przeczytać [rozwiązywania problemów aplikacji](virtual-machines-windows-troubleshoot-app-connection.md).