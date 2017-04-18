<properties
    pageTitle="Tworzenie i przekazywanie CentOS systemem Linux wirtualnego dysku twardego platformy Azure"
    description="Dowiedz się, jak tworzyć i przekaż Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym CentOS systemem Linux."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Przygotowywanie maszyny wirtualnej oparte na CentOS Azure

- [Przygotowywanie maszyny wirtualnej 6.x CentOS Azure](#centos-6x)
- [Przygotowywanie maszyny wirtualnej CentOS 7.0 + Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Wymagania wstępne ##

W tym artykule założono, że zainstalowano już CentOS (lub podobnych zależnych) systemu operacyjnego Linux wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD, na przykład rozwiązanie wirtualizacji takich jak funkcji Hyper-V. Aby uzyskać instrukcje zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).


**Uwagi dotyczące instalacji centOS**

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- VHDX format nie jest obsługiwany w Azure, tylko **stałej wirtualnego dysku twardego**.  Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj format wirtualnego dysku twardego można konwertować na dysk.

- Podczas instalacji z systemem Linux zaleca się, że używasz standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów.  [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.

- NUMA nie jest obsługiwane dla większych rozmiarów maszyn wirtualnych ze względu na błąd w wersji jądra Linux poniżej 2.6.37. Ten problem wpływa na przede wszystkim za pomocą poprzednie jądra czerwony funkcję 2.6.32. Instalacja ręczna agenta Azure Linux (waagent) automatycznie wyłączy NUMA w konfiguracji CHODNIKÓW jądrze Linux. Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux.  Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.


## <a name="centos-6x"></a>CentOS 6.x ##

1. W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2. Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

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

        # sudo chkconfig network on


8. **Tylko 6.3 centOS**: Instalowanie sterowników dla usług integracji Linux (LIS).

    **Ważne: Krok jest tylko prawidłowe dla CentOS 6.3 lub nowszy.**  W CentOS 6.4 + Linux Integration Services są *już dostępne w jądrze standardowy*.

    - Postępuj zgodnie z instrukcjami instalacji na [stronie pobierania LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) i zainstaluj liczbę obrotów na MINUTĘ na obrazie.  


9. Zainstaluj pakiet python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

10. Jeśli chcesz używać ich kopie OpenLogic, które są obsługiwane w ramach Azure centrach danych, Zamień plik /etc/yum.repos.d/CentOS-Base.repo z następujących repozytoriach.  Spowoduje to dodanie również repozytorium **[openlogic]** , zawierającej pakietów dla agenta Azure Linux:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Uwaga:** Pozostałą część tego przewodnika jest założenie, że korzystasz z co najmniej repo [openlogic], który będzie używany podczas instalacji agenta Azure Linux poniżej.


11. Dodaj następujący wiersz do /etc/yum.conf:

        http_caching=packages

    I **na tylko 6.3 CentOS** Dodaj poniższy wiersz:

        exclude=kernel*

12. Wyłączanie edycji pliku modułu yum "fastestmirror" "/ etc/yum/pluginconf.d/fastestmirror.conf" i w obszarze `[main]`, wpisz następujący ciąg:

        set enabled=0

13. Uruchom następujące polecenie, aby wyczyścić bieżące metadane yum:

        # yum clean all

14. **Na CentOS 6.3 tylko**aktualizacji jądra przy użyciu następującego polecenia:

        # sudo yum --disableexcludes=all install kernel

15. Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA ze względu na błąd w wersji jądra używanego przez CentOS 6.

    Oprócz powyższych zalecane jest *usunięcie* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Opcja może być skonfigurowany w razie potrzeby lewej, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych 128MB lub więcej, które mogą być problemy na mniejszych maszyn wirtualnych.


16. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

17. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent

    Zauważ, że instalacji pakietu WALinuxAgent spowoduje usunięcie NetworkManager i pakietów NetworkManager gnome Jeśli ta osoba nie zostały już usunięte zgodnie z opisem w kroku 2.

18. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Zmiany w CentOS 7 (i podobne pochodnych)**

Przygotowywanie maszyny wirtualnej CentOS 7 Azure jest bardzo podobne do 6 CentOS, jednak istnieje kilka istotnych różnic warto zauważyć:

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

6.  Modyfikowanie reguł udev, aby uniknąć generowania statyczne zasady interfejsy Ethernet. Te reguły może spowodować problemy, gdy klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Upewnij się, że usługa sieć zostanie uruchomiona w momencie uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

7. Zainstaluj pakiet python pyasn1, uruchamiając następujące polecenie:

        # sudo yum install python-pyasn1

8. Jeśli chcesz używać ich kopie OpenLogic, które są obsługiwane w ramach Azure centrach danych, Zamień plik /etc/yum.repos.d/CentOS-Base.repo z następujących repozytoriach.  Spowoduje to dodanie również repozytorium **[openlogic]** , zawierającej pakietów dla agenta Azure Linux:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Uwaga:** Pozostałą część tego przewodnika jest założenie, że korzystasz z co najmniej repo [openlogic], który będzie używany podczas instalacji agenta Azure Linux poniżej.

9.  Uruchom następujące polecenie, aby wyczyścić bieżące metadane yum i zainstaluj wszystkie aktualizacje:

        # sudo yum clean all
        # sudo yum -y update

10. Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "/ itp domyślne i chodników" w edytorze tekstów i Edytuj `GRUB_CMDLINE_LINUX` parametru, na przykład:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Również powoduje wyłączenie nowe CentOS 7 konwencje nazewnictwa dla nic. Oprócz powyższych zalecane jest *usunięcie* następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Opcja może być skonfigurowany w razie potrzeby lewej, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych 128MB lub więcej, które mogą być problemy na mniejszych maszyn wirtualnych.

11. Po zakończeniu edycji "/ itp domyślne i chodników" na powyżej, uruchom następujące polecenie, aby odbudować konfiguracji chodników:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

13. **Tylko w przypadku tworzenia obrazu z VMWare, VirtualBox lub KVM:** Dodawanie funkcji Hyper-V moduły do initramfs:

    Edytowanie `/etc/dracut.conf`, dodawanie zawartości:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Odbudowywanie initramfs:

        # dracut –f -v

14. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Kliknij pozycję **Akcja -> Zamknij w dół** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.

## <a name="next-steps"></a>Następne kroki
Teraz możesz używać dysku twardego CentOS Linux wirtualnych do tworzenia nowych maszyn wirtualnych platformy Azure. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).
