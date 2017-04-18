<properties
    pageTitle="Tworzenie i Przekaż obraz niestandardowy Linux | Microsoft Azure"
    description="Tworzenie i przekazywanie wirtualnego dysku twardego (wirtualnego dysku twardego) Azure z obrazem Linux niestandardowych przy użyciu modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Przekazywanie i tworzenie maszyny Linux z obrazu dysku niestandardowe

W tym artykule pokazano, jak przekazywać wirtualnego dysku twardego (wirtualnego dysku twardego) Azure przy użyciu modelu wdrożenia Menedżera zasobów i tworzyć maszyny wirtualne Linux z tego obrazu niestandardowego. Ta funkcja umożliwia Zainstaluj i skonfiguruj distro Linux do własnych potrzeb oraz szybko utworzyć Azure wirtualnych maszyn za pomocą tego wirtualnego dysku twardego.

## <a name="quick-commands"></a>Szybkie polecenia
Jeśli chcesz szybko wykonywać zadania, na następujące szczegóły sekcji podstawy polecenia służące do przekazania maszyny Azure. Pozostała część dokumentu, [Uruchamianie w tym miejscu](#requirements)można znaleźć bardziej szczegółowych informacji i kontekst dla każdego kroku.

Upewnij się, że masz [Polecenie Azure](../xplat-cli-install.md) zalogowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `mystorageaccount`, i `myimages`.

Najpierw należy utworzyć grupy zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUs` lokalizacji:

```bash
azure group create myResourceGroup --location "WestUS"
```

Utwórz konto miejsca do magazynowania do przechowywania dysków wirtualnych. Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Listy klawiszy dostępu do konta miejsca do magazynowania. Zanotuj `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Tworzenie kontenera w ramach konta miejsca do magazynowania przy użyciu klucza miejsca do magazynowania, który został pobrany. Poniższy przykład tworzy kontenera o nazwie `myimages` przy użyciu miejsca do magazynowania klucza wartości `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Na koniec przekazać do wirtualnego dysku twardego w kontenerze utworzony. Określanie lokalną ścieżkę do swojego wirtualnego dysku twardego w obszarze `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Teraz można utworzyć maszyn wirtualnych usługi przekazane dysku wirtualnego [przy użyciu szablonu Menedżera zasobów](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Można też użyć polecenie, określając identyfikator URI na dysku (`--image-urn`). Poniższy przykład tworzy maszyny o nazwie `myVM` za pomocą dysku wirtualnego przekazane wcześniej:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Konta docelowego miejsca do magazynowania musi być taka sama jak miejsce, w którym zostały przekazane wirtualny dysk. Należy określić lub odpowiedzi monituje o podanie dodatkowych parametrów wymaganych przez `azure vm create` poleceń, takich jak wirtualnej sieci, publiczny adres IP, nazwę użytkownika i SSH klawiszy. Więcej o [dostępnych parametrach polecenie Menedżer zasobów](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Wymagania dotyczące
Aby wykonać poniższe czynności, należy następująco:

- **Zainstalowany system operacyjny Linux w pliku VHD** - instalowanie [rozkładu zatwierdzone Azure Linux](virtual-machines-linux-endorsed-distros.md) (lub wyświetlić [informacje o innych niż zatwierdzone dystrybucji](virtual-machines-linux-create-upload-generic.md)) do dysku wirtualnego w formacie wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia maszyn wirtualnych i wirtualnego dysku twardego:
    - Zainstaluj i skonfiguruj [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) lub [KVM](http://www.linux-kvm.org/page/RunningKVM), zwracając uwagę na używanie wirtualny dysk twardy w formacie obrazu. W razie potrzeby można [przekonwertować obraz](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) za pomocą `qemu-img convert`.
    - Możesz też użyć funkcji Hyper-V [w systemie Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) [w systemie Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Nowszy format VHDX nie jest obsługiwana w Azure. Po utworzeniu maszyny Określ wirtualnego dysku twardego jako format. W razie potrzeby można konwertować dyski VHDX do korzystania z wirtualnego dysku twardego [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) lub [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) polecenia cmdlet programu PowerShell. Ponadto Azure nie obsługuje przekazywania dynamiczne wirtualnych dysków twardych, więc należy konwertować takiego dysków na statyczne wirtualnych dysków twardych przed przekazaniem. Za pomocą narzędzi, takich jak [Azure wirtualnego dysku twardego narzędzia dla Przejdź](https://github.com/Microsoft/azure-vhd-utils-for-go) do konwertowania dyski dynamiczne podczas przekazywania Azure.

- Maszyny wirtualne utworzone na podstawie obraz niestandardowy musi znajdować się na tym samym koncie miejsca do magazynowania jako samego obrazu
    - Tworzenie konta miejsca do magazynowania i kontener do przechowywania obu obraz niestandardowy i utworzonego maszyny wirtualne
    - Po utworzeniu wszystkich maszyny wirtualne można bezpiecznie usunąć obrazu

Upewnij się, że masz [Polecenie Azure](../xplat-cli-install.md) zalogowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `mystorageaccount`, i `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Przygotowywanie obrazu do przekazania

Azure obsługuje różne dystrybucji Linux (zobacz [Zatwierdzone dystrybucji](virtual-machines-linux-endorsed-distros.md)). Następujące artykuły poprowadzą przygotowywania różnych dystrybucji Linux, które są obsługiwane na Azure:

- **[Oparte na centOS dystrybucji](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Czerwone funkcję Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Inne — dystrybucji — zatwierdzone](virtual-machines-linux-create-upload-generic.md)**

Zobacz też **[Uwagi dotyczące instalacji Linux oraz](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** dla bardziej ogólne porady dotyczące przygotowywania obrazów Linux oraz dla Azure.

> [AZURE.NOTE] [Platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) dotyczy maszyny wirtualne systemem Linux tylko wtedy, gdy jedna z zatwierdzonego rozkład jest używana ze szczegółami konfiguracji zgodnie z ustawieniami w obszarze "Obsługiwane wersje" w [Linux na rozkład Azure-Endorsed](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Grupy zasobów umożliwiają powiązanie logicznie wszystkich Azure zasobów do obsługi maszyn wirtualnych, takich jak wirtualnej sieci i miejsca do magazynowania. Dowiedz się więcej o [grupach Azure zasobu w tym miejscu](../azure-resource-manager/resource-group-overview.md). Przed przekazaniem obrazu niestandardowego dysku i tworzenia maszyny wirtualne, najpierw należy utworzyć grupę zasobów. 

Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` lokalizacji:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania
Maszyny wirtualne są przechowywane jako blob strony w ramach konta usługi miejsca do magazynowania. Dowiedz się więcej o [magazynem obiektów blob Azure tutaj](../storage/storage-introduction.md#blob-storage). Możesz utworzyć konto miejsca do magazynowania dla obrazu niestandardowego dysku i maszyny wirtualne. Wszelkie maszyny wirtualne utworzone w programie obrazu niestandardowego dysku muszą być tego samego konta miejsca do magazynowania jako obrazu.

Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount` w grupie zasobów utworzone wcześniej:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Lista miejsca do magazynowania konta klawiszy
Azure generuje dwóch klawiszy dostępu 512-bitowej dla każdego konta miejsca do magazynowania. Klawisze dostępu są używane podczas uwierzytelniania na rachunku miejsca do magazynowania, takich jak do wykonywania operacji zapisu. Dowiedz się więcej o [zarządzaniu dostępu do magazynu w tym miejscu](../storage/storage-create-storage-account.md#manage-your-storage-account). Można wyświetlić klawiszy dostępu z `azure storage account keys list` polecenia.

Wyświetlanie klawisze dostępu dla utworzone konto miejsca do magazynowania:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Wynik jest podobne do:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Zanotuj `key1` jako użyje do współdziałania z konta miejsca do magazynowania w następnych kroków.

## <a name="create-a-storage-container"></a>Tworzenie kontenera miejsca do magazynowania
W ten sam sposób tworzenia różnych katalogach logicznie organizowanie lokalnego systemu plików możesz utworzyć kontenerów znajdujących się wewnątrz konto miejsca do magazynowania do organizowania dysków wirtualnych i obrazów. Konto miejsca do magazynowania może zawierać dowolną liczbę kontenerów. 

Poniższy przykład tworzy kontenera o nazwie `myimages`, określając klawisz dostępu uzyskanego w poprzednim kroku (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Przekazywanie wirtualnego dysku twardego
Teraz możesz faktycznie przekazać obraz niestandardowy dysku. Jako wszystkich dysków wirtualnych za pośrednictwem SMS, możesz przekazać i zawierają obrazu dysku niestandardowego jako blob strony.

Określ klawisz dostępu, kontener, który został utworzony w poprzednim kroku, a następnie ścieżkę do obrazu niestandardowego dysku na komputerze lokalnym:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Tworzenie maszyn wirtualnych na podstawie niestandardowego obrazu
Po utworzeniu maszyny wirtualne z obrazu dysku niestandardowych określ identyfikator URI obrazu dysku. Upewnij się, że sposób przechowywania obrazu niestandardowego dysku dopasowania konta docelowego miejsca do magazynowania. Możesz utworzyć do maszyn wirtualnych przy użyciu szablonu polecenie Azure lub JSON Menedżera zasobów.


### <a name="create-a-vm-using-the-azure-cli"></a>Tworzenie maszyn wirtualnych za pomocą interfejsu wiersza polecenia Azure
Możesz określić `--image-urn` parametr `azure vm create` polecenie wskaż obraz niestandardowy dysku. Upewnij się, że `--storage-account-name` pasuje do przechowywania obrazu dysku niestandardowe konto miejsca do magazynowania. Nie masz używać tym samym kontenerze jako obrazu dysku niestandardowych do przechowywania pośrednictwem usługi SMS. Upewnij się utworzyć wszelkie dodatkowe kontenery w taki sam sposób jak w poprzednich krokach przed przekazaniem obrazy niestandardowe dysku.

Poniższy przykład tworzy maszyny o nazwie `myVM` z obrazu dysku niestandardowy:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Nadal trzeba określić lub odpowiedzi monituje o podanie dodatkowych parametrów wymaganych przez `azure vm create` poleceń, takich jak wirtualnej sieci, publiczny adres IP, nazwę użytkownika i SSH klawiszy. Dowiedz się więcej o [dostępnych parametrach polecenie Menedżer zasobów](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Tworzenie maszyn wirtualnych przy użyciu szablonu JSON
Azure szablony Menedżera zasobów są plików notacji obiektu JavaScript (JSON), które definiują środowisko, który chcesz utworzyć. Szablony są w podziale do innego zasobu dostawców, takich jak obliczeń lub sieci. Możesz używać istniejących szablonów lub Napisz własny. Dowiedz się więcej o [korzystaniu z Menedżera zasobów i szablony](../azure-resource-manager/resource-group-overview.md).

W ramach `Microsoft.Compute/virtualMachines` dostawcy szablonu, masz `storageProfile` węzeł zawierający szczegóły konfiguracji dla swojego maszyn wirtualnych. Są dwa główne parametrów do edytowania `image` i `vhd` identyfikatory URI, które wskazują obrazu niestandardowego dysku i nowa maszyna wirtualna wirtualny dysk. Poniżej przedstawiono przykład JSON dotyczące korzystania z obrazu niestandardowego dysku:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Można użyć [tego istniejącego szablonu w celu tworzenia maszyn wirtualnych z obraz niestandardowy](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) lub przeczytaj informacje o [tworzeniu własnych szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md). 

Po utworzeniu szablonu skonfigurowanego Tworzenie usługi maszyny wirtualne za pomocą `azure group deployment create` polecenia. Podaj identyfikator URI szablon JSON przy użyciu `--template-uri` parametr:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Jeśli masz JSON pliku zapisanego lokalnie na komputerze, możesz użyć `--template-file` parametru zamiast tego:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Następne kroki
Po przygotowane i przekazane dysku wirtualnego niestandardowej można więcej informacji o [korzystaniu z Menedżera zasobów i szablony](../azure-resource-manager/resource-group-overview.md). Może też chcesz dodanie [dysku danych](virtual-machines-linux-add-disk.md) do nowej maszyny wirtualne. Jeśli masz aplikacje na pośrednictwem usługi SMS, których chcesz uzyskać dostęp, należy [otworzyć porty i protokoły punkty końcowe](virtual-machines-linux-nsg-quickstart.md).