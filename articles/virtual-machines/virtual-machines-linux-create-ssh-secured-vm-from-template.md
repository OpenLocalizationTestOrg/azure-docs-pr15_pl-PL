<properties
    pageTitle="Tworzenie maszyny Linux przy użyciu szablonu Azure | Microsoft Azure"
    description="Tworzenie maszyny Linux Azure za pomocą szablonu Azure Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Tworzenie maszyny Linux przy użyciu szablonu Azure

W tym artykule pokazano, jak szybko wdrażanie maszyny wirtualnej Linux Azure za pomocą szablonu Azure.  Wymaga tego artykułu:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).

- [Polecenie Azure](../xplat-cli-install.md) podczas logowania `azure login`.

- Polecenie Azure _muszą znajdować się w_ Menedżera zasobów Azure tryb `azure config mode arm`.

Można też szybko wdrożyć szablonu Linux maszyn wirtualnych za pomocą [Azure portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Polecenie Szybkie podsumowania

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

Szablony umożliwiają tworzenie maszyny wirtualne Azure z ustawień, które można dostosować podczas pierwszego uruchomienia, ustawienia, takie jak nazwy użytkowników i nazwy hostów. W tym artykule udostępnieniem szablonu Azure wykorzystanie maszyn wirtualnych Ubuntu wraz z grupy zabezpieczeń sieci (NSG) z portem 22 otwarte dla SSH.

Azure Menedżera zasobów szablony są to pliki JSON, które mogą być używane do prostego jednorazowe zadań, takich jak uruchamianie maszyn wirtualnych Ubuntu jako gotowy w tym artykule.  Azure szablony można również skonstruować złożone Azure konfiguracje całego środowiska jak testowania, standardowego lub zastosowania stos wdrożenia.

## <a name="create-the-linux-vm"></a>Tworzenie maszyn wirtualnych Linux

W poniższym przykładzie pokazano sposób nawiązać połączenie `azure group create` utworzyć grupę zasobów i wdrażania maszyn wirtualnych zabezpieczone SSH Linux w tym samym czasie przy użyciu [tego szablonu Azure Menedżera zasobów](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Należy pamiętać, że w tym przykładzie, że należy użyć nazwy, które są unikatowe dla środowiska. W tym przykładzie użyto `myResourceGroup` jako nazwa grupy zasobów i `myVM` jako nazwę maszyn wirtualnych.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Wynik powinna wyglądać podobnie do następujących blok wyjściowy:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Ten przykład wdrożony maszyn wirtualnych przy użyciu `--template-uri` parametru.  Możesz również pobrać lub tworzenie szablonu lokalnie i przekazywane za pomocą szablonu `--template-file` parametru ze ścieżką do pliku szablonu jako argument. Polecenie Azure monituje o podanie parametrów wymaganych w szablonie.

## <a name="next-steps"></a>Następne kroki

Wyszukiwanie w [galerii szablonów](https://azure.microsoft.com/documentation/templates/) do znalezienia jakie struktury aplikacji do wdrożenia dalej.
