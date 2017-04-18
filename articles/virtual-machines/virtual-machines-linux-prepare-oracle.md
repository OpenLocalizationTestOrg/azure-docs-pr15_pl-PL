<properties
pageTitle="Przygotowywanie maszyn wirtualnych Linux Oracle Azure | Microsoft Azure"
description="Konfiguracja krok po kroku maszyn wirtualnych Oracle systemem Linux platformy Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Przygotowywanie maszyn wirtualnych Oracle Linux Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Przygotowywanie maszyn wirtualnych Linux Oracle 6.4 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Przygotowywanie maszyn wirtualnych Linux Oracle 7.0 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że został zainstalowany system operacyjny Oracle Linux do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD, na przykład rozwiązanie wirtualizacji takich jak funkcji Hyper-V. Aby uzyskać instrukcje, zobacz [instalacji funkcji Hyper-V i tworzenia maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

**Uwagi dotyczące instalacji Linux Oracle**

- Jądro zgodne funkcję czerwony programu Oracle i jego UEK3 (nierozerwalny jądro Enterprise) na funkcji Hyper-V i Azure są obsługiwane. Aby uzyskać najlepsze wyniki należy zaktualizować do najnowszej jądra podczas przygotowywania wirtualnego dysku twardego programu Oracle Linux.

- UEK2 programu Oracle nie jest obsługiwane na funkcji Hyper-V i Azure, ponieważ zawiera wymagane sterowniki.

- Nowszy format VHDX nie jest obsługiwana w Azure. Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj można konwertować na dysk Format wirtualnego dysku twardego.

- Jeśli instalujesz z systemem Linux, zaleca się używanie standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. LVM lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.

- NUMA nie jest obsługiwane dla większych rozmiarów maszyn wirtualnych ze względu na błąd w wersji jądra Linux poniżej 2.6.37. Ten problem przede wszystkim wpływa na rozkład, które używają poprzednie jądra czerwony funkcję 2.6.32. Instalacja ręczna agenta Azure Linux (waagent) automatycznie wyłączy NUMA w konfiguracji CHODNIKÓW jądrze Linux. Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux. Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.

- Upewnij się, że `Addons` repozytorium jest włączona. Wybierz pozycję, aby edytować plik `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) lub `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) i zmień wiersz `enabled=0` do `enabled=1` w obszarze **[ol6_addons]** lub **[ol7_addons]** w tym pliku.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Należy wykonać kroki określonej konfiguracji systemu operacyjnego maszyny wirtualnej do uruchamiania w Azure.

1. W środkowym okienku Menedżera funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno maszyny wirtualnej.

3. Odinstaluj NetworkManager, uruchamiając następujące polecenie:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Jeśli pakiet nie jest już zainstalowany, to polecenie zakończy się niepowodzeniem z komunikatem o błędzie. Jest to planowane.

4. Utwórz plik o nazwie **sieci** w katalogu/etc/sysconfig/zawierający następujący tekst:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Tworzenie pliku o nazwie **ifcfg eth0** w /etc/sysconfig/network-scripts-katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Przenoszenie (lub usunąć) udev reguł, aby uniknąć generowania statyczne reguły dla interfejsu Ethernet. Te reguły spowodować problemy, jeśli masz klonowanie maszyny wirtualnej w Azure lub funkcji Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # chkconfig network on

8.  Zainstaluj python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

9.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA ze względu na błąd jądra zgodne funkcję czerwony programu Oracle.

    Oprócz powyższych zalecamy, możesz *usunąć* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Może być włączona opcja lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

10.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna.

11.  Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # <a name="sudo-yum-install-walinuxagent"></a>Zainstaluj yum sudo WALinuxAgent

    Zauważ, że instalacji pakietu WALinuxAgent spowoduje usunięcie NetworkManager i pakietów NetworkManager gnome Jeśli ta osoba nie zostały już usunięte zgodnie z opisem w kroku 2.

12.  Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Należy zauważyć, że dysk lokalny zasób jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Uwaga: można ustawić inną należy się.

13.  Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-wymusić - deprovision
        # <a name="export-histsize0"></a>Eksportowanie HISTSIZE = 0
        # <a name="logout"></a>Wyloguj się

14.  Kliknij pozycję **Action -\> zamknąć** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Zmiany w Oracle Linux 7**

Przygotowywanie maszyn wirtualnych Oracle Linux 7 Azure jest bardzo podobne do procesu dla programu Oracle Linux 6. Istnieją jednak kilka istotnych różnic warto zauważyć:

-   Zarówno jądrze zgodne funkcję czerwony i UEK3 programu Oracle są obsługiwane w Azure. Zalecamy jądrze UEK3.

-   Pakiet NetworkManager nie jest już koliduje z agentem Azure Linux. Ten pakiet jest instalowany domyślnie i zaleca się, że nie jest usuwany.

-   GRUB2 teraz jest używany jako inicjującego domyślne, aby procedury do edycji parametrów jądra uległa zmianie (patrz poniżej).

-   XFS jest teraz domyślnego systemu plików. System plików ext4 nadal można użyć w razie potrzeby.

**Kroki konfiguracji**

1.  W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2.  Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

3.  Utwórz plik o nazwie **sieci** w katalogu/etc/sysconfig/zawierający następujący tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Tworzenie pliku o nazwie **ifcfg eth0** w /etc/sysconfig/network-scripts-katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Przenoszenie (lub usunąć) udev reguł, aby uniknąć generowania statyczne reguły dla interfejsu Ethernet. Te reguły spowodować problemy, jeśli masz klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-V.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

7.  Zainstaluj pakiet python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

8.  Uruchom następujące polecenie, aby wyczyścić bieżące metadane yum i zainstaluj wszystkie aktualizacje:

        # sudo yum clean all
        # sudo yum -y update

9.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "/ itp domyślne i chodników" w edytorze tekstów i Edytuj CHODNIKÓW\_WIERSZ_POLECENIA\_parametru LINUX, na przykład:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Oprócz powyższych zalecamy, możesz *usunąć* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Może być włączona opcja lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

10.  Po zakończeniu edycji "/ itp domyślne i chodników", uruchom następujące polecenie, aby odbudować konfiguracji chodników:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2 mkconfig - o /boot/grub2/grub.cfg

11.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna.

12.  Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # <a name="sudo-yum-install-walinuxagent"></a>Zainstaluj yum sudo WALinuxAgent

13.  Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Należy zauważyć, że dysk lokalny zasób jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Uwaga: można ustawić inną należy się.

14.  Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-wymusić - deprovision
        # <a name="export-histsize0"></a>Eksportowanie HISTSIZE = 0
        # <a name="logout"></a>Wyloguj się

15.  Kliknij pozycję **Action -\> zamknąć** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.
