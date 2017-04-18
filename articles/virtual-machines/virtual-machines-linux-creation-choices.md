<properties
    pageTitle="Różne sposoby tworzenia maszyny Linux | Microsoft Azure"
    description="Więcej sposobów tworzenia maszyny wirtualnej Linux Azure, w tym łącza do narzędzi i samouczki dla każdej z tych metod."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Różne sposoby tworzenia maszyny wirtualnej Linux platformy Azure

Masz elastyczność w Azure do tworzenia maszyny wirtualnej Linux (maszyn wirtualnych) za pomocą narzędzia i przepływy pracy doświadczenia dla Ciebie. W tym artykule przedstawiono następujące różnice i przykłady dotyczące tworzenia usługi maszyny wirtualne Linux.


## <a name="azure-cli"></a>Polecenie Azure 

Polecenie Azure jest dostępna na platformach za pośrednictwem pakietu npm, pakietów dostarczonego przez distro lub Docker kontener. Można znaleźć więcej na temat [instalowania i konfigurowania polecenie Azure](../xplat-cli-install.md). Następujące samouczki zawierają przykłady przy użyciu interfejsu wiersza polecenia Azure. Każdy artykuł więcej informacji na temat polecenia Szybki start polecenie pokazywane:

- [Tworzenie maszyny Linux z polecenie Azure dla deweloperów i test](virtual-machines-linux-quick-create-cli.md)
    - Poniższy przykład tworzy maszyny CoreOS przy użyciu klucza publicznego o nazwie `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Tworzenie zabezpieczonego maszyn wirtualnych Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Poniższy przykład tworzy maszyny przy użyciu szablonu przechowywanego na GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Utworzenie pełną środowiska Linux, za pomocą interfejsu wiersza polecenia Azure](virtual-machines-linux-create-cli-complete.md)
    - Zawiera tworzenia równoważenia obciążenia i wielu maszyny wirtualne w zestawie dostępności.

- [Dodanie dysku do maszyny Linux](virtual-machines-linux-add-disk.md)
    - W poniższym przykładzie dodawane dysku 5Gb do istniejącego maszyn wirtualnych o nazwie `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portal

[Azure portal](https://portal.azure.com) pozwala szybko utworzyć maszyny, ponieważ istnieje nic, aby zainstalować na komputerze. Umożliwia tworzenie maszyn wirtualnych Azure portal:

- [Tworzenie maszyny Linux przy użyciu Azure portal](virtual-machines-linux-quick-create-portal.md) 
- [Dołączanie dysk przy użyciu Azure portal](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>System operacyjny i opcji obrazu
Podczas tworzenia maszyny, możesz wybrać obrazu w zależności od systemu operacyjnego, który chcesz uruchomić. Azure i partnerów oferują wiele obrazów, które należą aplikacji i narzędzi preinstalowane. Lub przekazać jeden z własnych obrazów (zobacz [następną sekcję](#use-your-own-image)).

### <a name="azure-images"></a>Obrazy Azure
Używanie `azure vm image` poleceń interfejsu wiersza polecenia, aby sprawdzić, jakie opcje są dostępne, publisher, distro wersji i kompilacjach.

Lista dostępnych wydawcy w następujący sposób:

```bash
azure vm image list-publishers --location WestUS
```

Lista dostępnych produktów (ofert) dla danego programu publisher w następujący sposób:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Lista dostępnych wersji produktu (distro wersjach) danej oferty w następujący sposób:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Lista wszystkich obrazów dostępne dla danej wersji następujący:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Więcej przykładów na temat przeglądania i korzystania z dostępnych obrazów zobacz [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów za pomocą interfejsu wiersza polecenia Azure](virtual-machines-linux-cli-ps-findimage.md).

`azure vm quick-create` i `azure vm create` polecenia mają aliasy służy do szybkiego dostępu do najczęściej distros i jego najnowszej wersji. Używanie aliasów często jest szybsze niż określenie programu publisher, oferty, SKU i wersji każdym utworzeniu maszyny:

| Alias     | Program Publisher | Oferty        | JEDNOSTKA SKU         | Wersja |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | najnowsze  |
| CoreOS    | CoreOS    | CoreOS       | Stały      | najnowsze  |
| Debian    | credativ  | Debian       | 8           | najnowsze  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnowsze  |
| RHEL      | RedHat    | RHEL         | 7.2         | najnowsze  |
| SLES      | SLES      | SLES         | 12-Z DODATKIEM SP1      | najnowsze  |
| UbuntuLTS | Kanoniczny | UbuntuServer | 14.04.4-LTS | najnowsze  |

### <a name="use-your-own-image"></a>Za pomocą własnego obrazu

Jeśli potrzebujesz specjalnych dostosowań, możesz użyć obrazu na podstawie istniejącego maszyn wirtualnych Azure *Przechwytywanie* tego maszyn wirtualnych. Możesz również przekazać obraz utworzony lokalnego. Aby uzyskać więcej informacji o obsługiwanych distros i jak używać własnych obrazów zobacz następujące artykuły:

- [Azure zatwierdzone dystrybucji](virtual-machines-linux-endorsed-distros.md)

- [Informacje dotyczące innych niż zatwierdzone dystrybucji](virtual-machines-linux-create-upload-generic.md)

- [Jak przechwytywać maszyny wirtualnej Linux jako szablon Menedżera zasobów](virtual-machines-linux-capture-image.md).
    - Szybki start przykład poleceń, aby przechwycić istniejących maszyn wirtualnych:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Następne kroki

- Tworzenie maszyny Linux [portal](virtual-machines-linux-quick-create-portal.md), za pomocą [interfejsu wiersza polecenia](virtual-machines-linux-quick-create-cli.md)lub przy użyciu [szablonu Azure Menedżera zasobów](virtual-machines-linux-cli-deploy-templates.md).

- Po utworzeniu Głosowa Linux, [dodać dysk danych](virtual-machines-linux-add-disk.md).

- Szybkie kroki, aby [zresetować hasło lub klawiszy SSH i zarządzanie użytkownikami](virtual-machines-linux-using-vmaccess-extension.md)
