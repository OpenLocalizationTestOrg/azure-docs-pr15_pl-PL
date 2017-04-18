<properties
    pageTitle="Tworzenie i przekazywanie VHD Linux Ubuntu platformy Azure"
    description="Dowiedz się, jak tworzyć i przekazywać Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym Ubuntu Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Przygotowywanie maszyn wirtualnych Ubuntu Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Obrazy Ubuntu oficjalnym w chmurze
Ubuntu publikuje teraz oficjalnym Azure wirtualnych dysków twardych pobrać [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Jeśli potrzebne do utworzenia własnego specjalistyczne obrazu Ubuntu dla Azure, raczej niż wykonaj poniższą procedurę ręcznego zaleca się zaczynać się te znane pracy wirtualnych dysków twardych i dostosowywanie stosownie do potrzeb. Najnowszej wersji obrazu zawsze można znaleźć w następujących miejscach:

 - Ubuntu 12.04-precyzyjnie: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04-Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że został zainstalowany system operacyjny Ubuntu Linux do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD, na przykład rozwiązanie wirtualizacji takich jak funkcji Hyper-V. Aby uzyskać instrukcje zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

**Uwagi dotyczące instalacji Ubuntu**

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .
- VHDX format nie jest obsługiwany w Azure, tylko **stałej wirtualnego dysku twardego**.  Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj format wirtualnego dysku twardego można konwertować na dysk.
- Podczas instalacji z systemem Linux zaleca się, że używasz standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.
- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux.  Więcej informacji na ten temat można znaleźć w poniższych kroków.
- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.


## <a name="manual-steps"></a>Ręczne

> [AZURE.NOTE] Przed utworzeniem własnych niestandardowych obrazu Ubuntu dla Azure, należy rozważyć użycie obrazów z [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) zamiast.


1. W środkowym okienku Menedżera funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno maszyny wirtualnej.

3.  Zamień bieżący repozytoria w obraz do użycia w Ubuntu Azure umowy odkupu. Kroki się nieco różnić w zależności od wersji Ubuntu.

    Przed rozpoczęciem edytowania /etc/apt/sources.list, zaleca się wykonanie kopii zapasowej:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Obrazy Ubuntu Azure teraz następujące jądrze *aktywacji sprzętu* (HWE). Aktualizacja systemu operacyjnego do najnowszej jądra, uruchamiając następujące polecenia:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Modyfikowanie linii uruchamiania jądra chodników w celu dołączenia jądra dodatkowe parametry Azure. W tym celu otwórz "/ itp domyślne i chodników" w edytorze tekstów, Znajdź zmienną o nazwie `GRUB_CMDLINE_LINUX_DEFAULT` (lub Dodaj go w razie potrzeby) i edytowanie go, aby uwzględnić następujące parametry:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Zapisz i zamknij ten plik, a następnie uruchom "`sudo update-grub`". Temu będziesz mieć pewność, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomoc techniczna debugowania problemów.

6.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

7.  Zainstaluj Azure agenta Linux:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Uwaga tej instalacji `walinuxagent` usunie pakiet `NetworkManager` i `NetworkManager-gnome` pakiety, jeśli są one zainstalowane.

8.  Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


## <a name="next-steps"></a>Następne kroki
Teraz możesz używać dysku twardego Ubuntu Linux wirtualnych do tworzenia nowych maszyn wirtualnych platformy Azure. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Odwołania ##

Jądro aktywacji (HWE) sprzętu Ubuntu:

- [http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
