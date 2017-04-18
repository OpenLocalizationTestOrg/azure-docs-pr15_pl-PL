<properties 
    pageTitle="Ponownie wdróż maszyn wirtualnych Linux | Microsoft Azure" 
    description="W tym artykule opisano, jak ponownie rozmieścić maszyn wirtualnych Linux zmniejszeniu problemy z połączeniem z SSH." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ponownie wdróż maszyny wirtualnej do nowego węzła Azure

Jeśli masz już przeciwległych trudności, które mogą pomóc SSH rozwiązywania problemów lub dostęp do aplikacji Azure maszyn wirtualnych (Głosowa), ponowne rozmieszczanie maszyn wirtualnych. Podczas ponownego wdrożenia maszyny maszyn wirtualnych jest przenoszony do nowego węzła w ramach infrastruktury Azure, a następnie włącza ją ponownie, zachowując wszystkie opcje konfiguracji i skojarzonych z nimi zasobów. W tym artykule pokazano, jak ponownie rozmieścić maszyny za pomocą interfejsu wiersza polecenia Azure lub Azure portal.

> [AZURE.NOTE] Po ponownego wdrożenia maszyny dysku tymczasowym zostaje utracony i dynamiczne adresy IP skojarzonego z interfejsem wirtualnej sieci zostaną zaktualizowane. 


## <a name="using-azure-cli"></a>Za pomocą interfejsu wiersza polecenia Azure

Upewnij się, masz [Najnowsze polecenie Azure zainstalowany](../xplat-cli-install.md) na komputerze i jest włączony tryb Menedżera zasobów (`azure config mode arm`).

Użyj następującego polecenia polecenie Azure ponownie rozmieścić komputera wirtualnej:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Wyświetlanie stanu Zmień maszyn wirtualnych odbywa się przez proces ponownego wdrażania. `PowerState` Maszyn wirtualnych przechodzi "Uruchamianie" do "Aktualizacja", "Uruchom", a na koniec "działanie" odbywa się przez proces ponowne rozmieszczanie do nowego hosta. Sprawdzanie stanu maszyny wirtualne w grupie zasobów z:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Następne kroki
Jeśli masz problemów dotyczących łączenia się z maszyn wirtualnych można znaleźć pomoc w [rozwiązywaniu problemów z połączeniami SSH](virtual-machines-linux-troubleshoot-ssh-connection.md) lub [SSH szczegółowe kroki rozwiązywania problemów](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Jeśli nie masz dostępu do uruchamiania aplikacji na swojej maszyn wirtualnych, można również przeczytać [rozwiązywania problemów aplikacji](virtual-machines-linux-troubleshoot-app-connection.md).