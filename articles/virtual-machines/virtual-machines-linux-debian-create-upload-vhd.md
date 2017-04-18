<properties
    pageTitle="Przygotowywanie Debian wirtualnego dysku twardego Linux | Microsoft Azure"
    description="Dowiedz się, jak utworzyć Debian 7 i 8 plików wirtualnego dysku twardego do wdrożenia platformy Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Przygotowywanie Debian wirtualnego dysku twardego Azure

## <a name="prerequisites"></a>Wymagania wstępne
W tej sekcji przyjęto założenie, że został zainstalowany system operacyjny Debian Linux z pliku ISO pobierane z [Debian witryny sieci Web](https://www.debian.org/distrib/) do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD; Funkcji Hyper-V jest tylko jeden przykład. Aby za pomocą funkcji Hyper-V, zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Uwagi dotyczące instalacji

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .
- Nowszy format VHDX nie jest obsługiwana w Azure. Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet **wirtualnego dysku twardego Konwertuj** Format wirtualnego dysku twardego można konwertować na dysk.
- Podczas instalacji z systemem Linux zaleca się używanie standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.
- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Azure Linux. Więcej informacji na ten temat można znaleźć w poniższych kroków.
- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Tworzenie wirtualnych dysków twardych Debian przy użyciu Zarządzanie Azure

Dostępne są narzędzia do generowania Debian wirtualnych dysków twardych dla Azure, takich jak [azure — Zarządzanie](https://github.com/credativ/azure-manage) skryptów z [credativ](http://www.credativ.com/). Jest to zalecane podejście i tworzenie obrazu od podstaw. Na przykład, aby utworzyć Debian 8 wirtualnego dysku twardego uruchom następujące polecenia do pobrania azure zarządzać (i zależności) i uruchamianie skryptu azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Ręczne przygotowywanie Debian wirtualnego dysku twardego

1. W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

3. Komentarz się wiersz dla **CD-ROM deb** w `/etc/apt/source.list` po zdefiniowaniu maszyn wirtualnych przed pliku ISO.

4. Edytowanie `/etc/default/grub` plików i modyfikowanie parametru **GRUB_CMDLINE_LINUX** następująco w celu dołączenia jądra dodatkowe parametry Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Odbudowywanie chodników i uruchom:

        # sudo update-grub

6. Dodaj osoby Debian Azure repozytoria do /etc/apt/sources.list Debian 7 lub 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Joasia"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Zainstaluj Azure agenta Linux:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 jest wymagane do uruchomienia jądrze oparte na 3.16 z repozytorium wheezy backports. Najpierw należy utworzyć w pliku o nazwie /etc/apt/preferences.d/linux.pref z następujące zagadnienia:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Następnie uruchom "Zainstaluj stanie get sudo linux — obraz amd64" do zainstalowania nowych jądra.

8. Deprovision maszyny wirtualnej i przygotowywanie go do inicjowania obsługi administracyjnej Azure i uruchom:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Kliknij przycisk **Akcja** -> Zamknij w dół w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


## <a name="next-steps"></a>Następne kroki

Teraz możesz utworzyć nowy maszyn wirtualnych w Azure za pomocą Debian wirtualnego dysku twardego. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).
