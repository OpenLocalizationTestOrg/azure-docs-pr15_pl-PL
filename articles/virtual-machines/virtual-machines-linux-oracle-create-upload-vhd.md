<properties
    pageTitle="Tworzenie i przekazywanie Oracle Linux wirtualnego dysku twardego | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i przekazywać Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym Oracle Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Przygotowywanie maszyn wirtualnych Oracle Linux Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Wymagania wstępne ##

W tym artykule założono, że został zainstalowany system operacyjny Oracle Linux do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD, na przykład rozwiązanie wirtualizacji takich jak funkcji Hyper-V. Aby uzyskać instrukcje zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Uwagi dotyczące instalacji Linux Oracle

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Jądro zgodne funkcję czerwony programu Oracle i ich UEK3 (nierozerwalny jądro Enterprise) na funkcji Hyper-V i Azure są obsługiwane. Aby uzyskać najlepsze wyniki Pamiętaj o aktualizacji do najnowszej jądra podczas przygotowywania wirtualnego dysku twardego programu Oracle Linux.

- UEK2 programu Oracle nie jest obsługiwane na funkcji Hyper-V i Azure, ponieważ zawiera wymagane sterowniki.

- VHDX format nie jest obsługiwany w Azure, tylko **stałej wirtualnego dysku twardego**.  Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj format wirtualnego dysku twardego można konwertować na dysk.

- Podczas instalacji z systemem Linux zaleca się, że używasz standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.

- NUMA nie jest obsługiwane dla większych rozmiarów maszyn wirtualnych ze względu na błąd w wersji jądra Linux poniżej 2.6.37. Ten problem wpływa na przede wszystkim za pomocą poprzednie jądra czerwony funkcję 2.6.32. Instalacja ręczna agenta Azure Linux (waagent) automatycznie wyłączy NUMA w konfiguracji CHODNIKÓW jądrze Linux. Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux.  Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.

- Upewnij się, że `Addons` repozytorium jest włączona. Edytowanie pliku `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) lub `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) i zmień wiersz `enabled=0` do `enabled=1` w obszarze **[ol6_addons]** lub **[ol7_addons]** w tym pliku.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

Należy wykonać kroki określonej konfiguracji systemu operacyjnego maszyny wirtualnej do uruchamiania w Azure.

1. W środkowym okienku Menedżera funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno maszyny wirtualnej.

3. Odinstaluj NetworkManager, uruchamiając następujące polecenie:

        # sudo rpm -e --nodeps NetworkManager

    **Uwaga:** Jeśli pakiet nie jest już zainstalowany, to polecenie zakończy się niepowodzeniem z komunikatem o błędzie. Jest to planowane.

4.  Tworzenie pliku o nazwie **sieci** w `/etc/sysconfig/` katalog zawierający następujący tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Tworzenie pliku o nazwie **ifcfg eth0** w `/etc/sysconfig/network-scripts/` katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Modyfikowanie reguł udev, aby uniknąć generowania statyczne zasady interfejsy Ethernet. Te reguły może spowodować problemy, gdy klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Upewnij się, że usługa sieć zostanie uruchomiona w momencie uruchamiania, uruchamiając następujące polecenie:

        # chkconfig network on

8. Zainstaluj python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

9.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. W tym Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA ze względu na błąd jądra zgodne funkcję czerwony programu Oracle.

    Oprócz powyższych zalecane jest *usunięcie* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Opcja może być skonfigurowany w razie potrzeby lewej, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych 128MB lub więcej, które mogą być problemy na mniejszych maszyn wirtualnych.


10. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

11. Instalowanie agenta Linux Azure, uruchamiając następujące polecenie. Najnowsza wersja jest 2.0.15.

        # sudo yum install WALinuxAgent

    Zauważ, że instalacji pakietu WALinuxAgent spowoduje usunięcie NetworkManager i pakietów NetworkManager gnome Jeśli ta osoba nie zostały już usunięte zgodnie z opisem w kroku 2.

12. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Zmiany w Oracle Linux 7**

Przygotowywanie maszyn wirtualnych Oracle Linux 7 Azure jest bardzo podobne do 6 Linux Oracle, jednak istnieje kilka istotnych różnic warto zauważyć:

 - Zarówno jądrze zgodne funkcję czerwony i UEK3 programu Oracle są obsługiwane w Azure.  Zaleca się jądrze UEK3.
 - Pakiet NetworkManager nie jest już koliduje z agentem Azure Linux. Ten pakiet jest zainstalowana domyślnie i zaleca się, że nie jest usuwany.
 - GRUB2 teraz jest używany jako inicjującego domyślne, aby procedury do edycji parametrów jądra uległa zmianie (patrz poniżej).
 - XFS jest teraz domyślnego systemu plików. System plików ext4 nadal można użyć w razie potrzeby.


**Kroki konfiguracji**

1. W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

3.  Tworzenie pliku o nazwie **sieci** w `/etc/sysconfig/` katalog zawierający następujący tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Tworzenie pliku o nazwie **ifcfg eth0** w `/etc/sysconfig/network-scripts/` katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Modyfikowanie reguł udev, aby uniknąć generowania statyczne zasady interfejsy Ethernet. Te reguły może spowodować problemy, gdy klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Upewnij się, że usługa sieć zostanie uruchomiona w momencie uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

7. Zainstaluj pakiet python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

8.  Uruchom następujące polecenie, aby wyczyścić bieżące metadane yum i zainstaluj wszystkie aktualizacje:

        # sudo yum clean all
        # sudo yum -y update

9.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. W tym celu otwórz "/ itp domyślne i chodników" w edytorze tekstów i Edytuj `GRUB_CMDLINE_LINUX` parametru, na przykład:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Również powoduje wyłączenie nowe OEL 7 konwencje nazewnictwa dla nic. Oprócz powyższych zalecane jest *usunięcie* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Opcja może być skonfigurowany w razie potrzeby lewej, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych 128MB lub więcej, które mogą być problemy na mniejszych maszyn wirtualnych.


10. Po zakończeniu edycji "/ itp domyślne i chodników" na powyżej, uruchom następujące polecenie, aby odbudować konfiguracji chodników:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

12. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


## <a name="next-steps"></a>Następne kroki
Teraz możesz tworzyć nowe maszyn wirtualnych w Azure za pomocą VHD programu Oracle Linux. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).
