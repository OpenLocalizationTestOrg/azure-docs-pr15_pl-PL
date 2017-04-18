<properties
    pageTitle="Tworzenie i przekazywanie SUSE Linux wirtualnego dysku twardego platformy Azure"
    description="Dowiedz się, jak tworzyć i przekazywać Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym SUSE Linux."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Przygotowywanie maszyny wirtualnej SLES lub openSUSE Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Wymagania wstępne ##

W tym artykule założono, że zainstalowano już SUSE lub openSUSE system operacyjny Linux do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD, na przykład rozwiązanie wirtualizacji takich jak funkcji Hyper-V. Aby uzyskać instrukcje zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES-openSUSE uwagi dotyczące instalacji

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- VHDX format nie jest obsługiwany w Azure, tylko **stałej wirtualnego dysku twardego**.  Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj format wirtualnego dysku twardego można konwertować na dysk.

- Podczas instalacji z systemem Linux zaleca się, że używasz standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux.  Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.


## <a name="use-suse-studio"></a>Za pomocą SUSE Studio
[SUSE Studio](http://www.susestudio.com) łatwo tworzyć i zarządzać obrazy SLES i openSUSE Azure i funkcji Hyper-V. Jest to zalecane podejście do dostosowywania własnych obrazów SLES i openSUSE.

Alternatywnie do tworzenia własnych wirtualny dysk twardy SUSE publikuje obrazy BYOS (wyświetlić swój własny subskrypcji) dla SLES u [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Przygotowywanie SUSE Linux Enterprise Server 11 SP4 ##

1. W środkowym okienku Menedżera funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno maszyny wirtualnej.

3. Zarejestruj się w systemie SUSE Linux Enterprise, aby zezwolić na pobieranie aktualizacji i zainstalować pakiety.

4. Aktualizacja systemu przy użyciu najnowszych poprawek:

        # sudo zypper update

5. Zainstaluj agenta Linux Azure z repozytorium SLES:

        # sudo zypper install WALinuxAgent

6. Sprawdź, czy waagent jest ustawiona na "na" w chkconfig, a nie ją włączyć dla autostart:
               
        # sudo chkconfig waagent on

7. Określ, czy usługa waagent jest uruchomiona i jeśli nie, uruchom go: 

        # sudo service waagent start
                
8. Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. W tym Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dzięki wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów.

9. Upewnij się, że /boot/grub/menu.lst i /etc/fstab zarówno odwołanie dysku przy użyciu jego UUID (przez uuid) zamiast identyfikator dysku (przez identyfikator). 

    Uzyskiwanie dysku UUID
    
        # ls /dev/disk/by-uuid/

    Jeśli /dev/disk/by-id / jest używana, aktualizowanie fstab zarówno /boot/grub/menu.lst i -itp-z wartością przez uuid pisane z wielkiej litery

    Przed zmianą
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Po zmianie
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Modyfikowanie reguł udev, aby uniknąć generowania statyczne zasady interfejsy Ethernet. Te reguły może spowodować problemy, gdy klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Jest zalecane, aby edytować plik "/ itp/sysconfig/network/dhcp" i zmienianie `DHCLIENT_SET_HOSTNAME` parametr następujące czynności:

        DHCLIENT_SET_HOSTNAME="no"

12. W "/ itp/sudoers" komentarz lub usunąć następujące wiersze, jeśli istnieją:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

14. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


----------

## <a name="prepare-opensuse-131"></a>Przygotowywanie openSUSE 13.1 + ##

1. W środkowym okienku Menedżera funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno maszyny wirtualnej.

3. W powłoce, uruchom polecenie "`zypper lr`". Jeśli to polecenie zwraca wynik podobny do następującego, a następnie przechowywania są skonfigurowane zgodnie z oczekiwaniami — nie zmiany są konieczne (należy zauważyć, że mogą być różne numery wersji):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Jeśli polecenie zwraca "Nie repozytoria zdefiniowane..." użyć następujących poleceń, aby dodać te umowy odkupu:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Następnie sprawdź, przechowywania zostały dodane, uruchamiając polecenie "`zypper lr`" ponownie. W przypadku, gdy jedna z repozytoria odpowiednich aktualizacji nie jest włączona, należy ją włączyć przy użyciu następującego polecenia:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Aktualizacja jądra do najnowszej wersji:

        # sudo zypper up kernel-default

    Lub zaktualizować system z najnowszych poprawek:

        # sudo zypper update

5.  Zainstaluj Azure agenta Linux.

        # sudo zypper install WALinuxAgent

6.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dzięki wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Ponadto usunąć następujących parametrów w wierszu uruchamiania jądra, jeśli istnieją:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Jest zalecane, aby edytować plik "/ itp/sysconfig/network/dhcp" i zmienianie `DHCLIENT_SET_HOSTNAME` parametr następujące czynności:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Ważne:** W "/ itp/sudoers" komentarz lub usunąć następujące wiersze, jeśli istnieją:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

10. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Upewnij się, że Azure Linux Agent jest uruchamiany podczas uruchamiania:

        # sudo systemctl enable waagent.service

13. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.

## <a name="next-steps"></a>Następne kroki
Teraz możesz używać dysku twardego wirtualnych SUSE Linux do tworzenia nowych maszyn wirtualnych w Azure. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).
