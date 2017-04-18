<properties
    pageTitle="Tworzenie i przekazywanie wirtualnego dysku twardego Linux | Microsoft Azure"
    description="Tworzenie i przekazywanie Azure wirtualnego dysku twardego (wirtualny dysk twardy) z modelu Klasyczny wdrożenia, który zawiera system operacyjny Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Tworzenie i przekazywanie wirtualnego dysku twardego z systemem operacyjnym Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Można również [przekazać obraz niestandardowy dysku za pomocą Menedżera zasobów Azure](virtual-machines-linux-upload-vhd.md).

W tym artykule pokazano, jak tworzyć i przekazać wirtualnego dysku twardego (wirtualnego dysku twardego), aby użyć jako własnego obrazu do tworzenia maszyn wirtualnych w Azure. Dowiedz się, jak przygotować system operacyjny, więc można utworzyć wiele maszyn wirtualnych na podstawie tego obrazu. 

>  [AZURE.NOTE] Jeśli masz kilka chwil Pomóż nam ulepszać dokumentacji maszyn wirtualnych Linux Azure, nie podejmując tę [ankietę szybkie](https://aka.ms/linuxdocsurvey) środowiska usługi. Co odpowiedzi pomagają nam pomocy, których wykonanie pracy.

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że następujące elementy:

- **W pliku VHD jest zainstalowany system operacyjny Linux** — zainstalowano [rozkładu zatwierdzone Azure Linux](virtual-machines-linux-endorsed-distros.md) (lub wyświetlić [informacje o innych niż zatwierdzone dystrybucji](virtual-machines-linux-create-upload-generic.md)) do dysku wirtualnego w formacie wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia maszyn wirtualnych i wirtualnego dysku twardego:
    - Zainstaluj i skonfiguruj [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) lub [KVM](http://www.linux-kvm.org/page/RunningKVM), zwracając uwagę na używanie wirtualny dysk twardy w formacie obrazu. W razie potrzeby można [przekonwertować obraz](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) za pomocą `qemu-img convert`.
    - Możesz też użyć funkcji Hyper-V [w systemie Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) [w systemie Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Nowszy format VHDX nie jest obsługiwana w Azure. Po utworzeniu maszyny Określ wirtualnego dysku twardego jako format. W razie potrzeby można konwertować dyski VHDX do korzystania z wirtualnego dysku twardego [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) lub [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) polecenia cmdlet programu PowerShell. Ponadto Azure nie obsługuje przekazywania dynamiczne wirtualnych dysków twardych, więc należy konwertować takiego dysków na statyczne wirtualnych dysków twardych przed przekazaniem. Za pomocą narzędzi, takich jak [Azure wirtualnego dysku twardego narzędzia dla Przejdź](https://github.com/Microsoft/azure-vhd-utils-for-go) do konwertowania dyski dynamiczne podczas przekazywania Azure.

- **Interfejs wiersza polecenia azure** - zainstaluj najnowszą [interfejs wiersza polecenia Azure](../virtual-machines-command-line-tools.md) przekazywania wirtualnego dysku twardego.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Krok 1: Przygotowywanie obrazu do przekazania

Azure obsługuje różne dystrybucji Linux (zobacz [Zatwierdzone dystrybucji](virtual-machines-linux-endorsed-distros.md)). Następujące artykuły poprowadzą przygotowywania różnych dystrybucji Linux, które są obsługiwane na Azure. Po wykonaniu kroków w następujące przewodniki wezwaniem ponownie po umieszczeniu pliku wirtualny dysk twardy, który jest gotowy do przekazania Azure:

- **[Oparte na centOS dystrybucji](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Czerwone funkcję Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Inne — dystrybucji — zatwierdzone](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Platformy Azure SLA dotyczy środowisku maszyn wirtualnych systemu operacyjnego Linux tylko wtedy, gdy jedna z zatwierdzonego rozkład jest używana ze szczegółami konfiguracji zgodnie z ustawieniami w obszarze "Obsługiwane wersje" w [Linux na rozkład Azure-Endorsed](virtual-machines-linux-endorsed-distros.md). Wszystkich przydziałów Linux w galerii Azure obraz są zatwierdzonego rozkładów o konfiguracji.

Zobacz też **[Uwagi dotyczące instalacji Linux oraz](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** dla bardziej ogólne porady dotyczące przygotowywania obrazów Linux oraz dla Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Krok 2: Przygotowywanie połączenie Azure

Upewnij się, polecenie Azure są używane w modelu Klasyczny wdrożenia (`azure config mode asm`), następnie zaloguj się do swojego konta:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Krok 3: Przekazywanie obrazu do Azure

Wymagane jest konto miejsca do magazynowania, aby przekazać plik wirtualnego dysku twardego. Możesz wybrać albo istniejącego konta miejsca do magazynowania lub [Utwórz nowy](../storage/storage-create-storage-account.md).

Polecenie Azure umożliwia przekazywanie obrazu przy użyciu następującego polecenia:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

W poprzednim przykładzie:

- Adres URL dla konta miejsca do magazynowania, który ma zostać użyta jest **BlobStorageURL**
- **YourImagesFolder** jest kontenerze magazyn obiektów blob miejsce, w którym mają być przechowywane obrazy
- **VHDName** jest etykietę, która pojawia się w portalu do identyfikowania wirtualnego dysku twardego.
- **PathToVHDFile** jest pełną ścieżkę i nazwę pliku VHD na tym komputerze.

Poniżej przedstawiono przykład wykonane:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Krok 4: Tworzenie maszyn wirtualnych z obrazu
Tworzenia maszyn wirtualnych przy użyciu `azure vm create` w taki sam sposób jak zwykłe maszyn wirtualnych. Określ nazwę przekazała obrazu w poprzednim kroku. W poniższym przykładzie stosujemy nazwa obrazu **UbuntuLTS** podanych w poprzednim kroku:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Aby utworzyć własny maszyny wirtualne, podanie własnej nazwy użytkownika + hasła, lokalizacji, nazwa DNS i nazwa obrazu.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [informacje dotyczące polecenie Azure modelu Azure wdrożenia klasyczny](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
