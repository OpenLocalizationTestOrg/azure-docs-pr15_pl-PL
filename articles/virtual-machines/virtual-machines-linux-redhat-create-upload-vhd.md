<properties
    pageTitle="Tworzenie i przekazywanie czerwony funkcję Enterprise Linux wirtualnego dysku twardego do użycia w Azure"
    description="Dowiedz się, jak tworzyć i przekaż Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym Red funkcję Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Przygotowywanie maszyny wirtualnej oparte na czerwony funkcję Azure

W tym artykule dowiesz się, jak przygotować się do użycia w Azure maszyny wirtualnej czerwony funkcję Enterprise Linux (RHEL). Wersje programu RHEL, które zostały omówione w tym artykule są 6,7, 7.1 i 7.2. Monitorami przygotowania, które zostały omówione w tym artykule są funkcji Hyper-V, oparte na jądra maszyn wirtualnych (KVM) i VMware. Uzyskać więcej informacji o wymagania dotyczące uczestnictwo w programie dostęp w chmurze czerwony ustawień uprawnień zobacz [funkcję czerwony dostęp w chmurze witryny sieci Web](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) i [Uruchomiony RHEL Azure](https://access.redhat.com/articles/1989673).

[Przygotowywanie maszyny wirtualnej 6,7 RHEL z Menedżera funkcji Hyper-V](#rhel67hyperv)

[Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z Menedżera funkcji Hyper-V](#rhel7xhyperv)

[Przygotowywanie maszyny wirtualnej 6,7 RHEL z KVM](#rhel67kvm)

[Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z KVM](#rhel7xkvm)

[Przygotowywanie maszyny wirtualnej 6,7 RHEL z VMware](#rhel67vmware)

[Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z VMware](#rhel7xvmware)

[Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z pliku kickstart](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Przygotowywanie maszyny wirtualnej oparte na czerwony funkcję z Menedżera funkcji Hyper-V
### <a name="prerequisites"></a>Wymagania wstępne
W tej sekcji przyjęto założenie, że zainstalowano już obraz RHEL (z pliku ISO, który został pobrany z witryny sieci Web funkcję czerwony) do wirtualnego dysku twardego (VHD). Aby uzyskać więcej informacji na temat instalacji obrazu systemu operacyjnego za pomocą Menedżera funkcji Hyper-V zobacz [Instalowanie roli Hyper-V i konfigurowanie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

**Uwagi dotyczące instalacji RHEL**

- Aby uzyskać więcej porady dotyczące przygotowywania Linux oraz dla Azure również zobacz [Ogólne uwagi dotyczące instalacji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Nowszy format VHDX nie jest obsługiwana w Azure. Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet programu PowerShell **wirtualnego dysku twardego Konwertuj** można konwertować na dysk Format wirtualnego dysku twardego.

- Wirtualnych dysków twardych muszą zostać utworzone jako "stałe" — dynamiczne wirtualnych dysków twardych nie są obsługiwane.

- Jeśli instalujesz z systemem Linux, firma Microsoft zaleca używanie standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. LVM lub [RAID](virtual-machines-linux-configure-raid.md) może zostać zastosowane na dyskach danych.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Możesz skonfigurować agenta Linux, aby utworzyć plik wymiany na dysku zasobów. Więcej informacji na ten temat jest dostępna w ramach poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.

- Gdy używasz **qemu img** Aby przekonwertować obrazy dysków format wirtualnego dysku twardego Zauważ, że w wersjach qemu img 2.2.1 jest znane błąd lub nowszym. Ten błąd powoduje niewłaściwie sformatowany wirtualnego dysku twardego. Ten problem ma ustalana w nowej wersji qemu img. Teraz, zaleca się używanie qemu-img wersji 2.2.0 lub starszym.

### <a id="rhel67hyperv"> </a>Przygotować maszyny wirtualnej 6,7 RHEL z Menedżera funkcji Hyper-V###


1.  W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2.  Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

3.  Odinstaluj NetworkManager, uruchamiając następujące polecenie:

        # sudo rpm -e --nodeps NetworkManager

    Należy zauważyć, że jeśli pakiet nie jest już zainstalowany, to polecenie nie powiedzie się komunikat o błędzie. Jest to planowane.

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

6.  Przenoszenie (lub usunąć) udev reguł, aby uniknąć generowania statyczne reguły dla interfejsu Ethernet. Te reguły spowodować problemy, jeśli klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

8.  Zarejestruj swoją subskrypcję funkcję czerwony, aby umożliwić instalację pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, otwórz `/boot/grub/menu.lst` w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA z powodu błędu w wersji jądra jest używana przez RHEL 6.

    Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

11. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna. Modyfikowanie /etc/ssh/sshd_config, aby uwzględnić następujący wiersz:

        ClientAliveInterval 180

12. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Zauważ, że instalacji pakietu WALinuxAgent spowoduje usunięcie NetworkManager i pakietów NetworkManager gnome Jeśli ta osoba nie zostały już usunięte zgodnie z opisem w kroku 2.

13. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego.
Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Unregister subskrypcji (w razie potrzeby), uruchamiając następujące polecenie:

        # sudo subscription-manager unregister

15. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kliknij pozycję **akcji > Zamknij** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.
 

### <a id="rhel7xhyperv"> </a>Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z Menedżera funkcji Hyper-V###

1.  W Menedżerze funkcji Hyper-V wybierz maszyny wirtualnej.

2.  Kliknij przycisk **Połącz** , aby otworzyć okno konsoli dla maszyny wirtualnej.

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

5.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

6.  Zarejestruj swoją subskrypcję funkcję czerwony, aby umożliwić instalację pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, otwórz `/etc/default/grub` w edytorze tekstów i Edytuj parametr **GRUB_CMDLINE_LINUX** . Na przykład:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.
    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

8.  Po zakończeniu edycji `/etc/default/grub`, uruchom następujące polecenie, aby odbudować konfiguracji chodników:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna. Modyfikowanie `/etc/ssh/sshd_config` do uwzględnienia następujący wiersz:

        ClientAliveInterval 180

10. Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego. Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), zmodyfikuj ustawienia następujących parametrów w `/etc/waagent.conf` odpowiednio:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Jeśli chcesz unregister subskrypcji, uruchom następujące polecenie:

        # sudo subscription-manager unregister

14. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kliknij pozycję **akcji > Zamknij** w Menedżerze funkcji Hyper-V. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Przygotowywanie maszyny wirtualnej oparte na czerwony funkcję z KVM

### <a id="rhel67kvm"> </a>Przygotować maszyny wirtualnej 6,7 RHEL z KVM###


1.  Pobierz obraz KVM 6,7 RHEL z czerwonym funkcję.

2.  Ustawianie hasła głównego.

    Generowanie zaszyfrowane hasło, a następnie skopiuj dane wyjściowe polecenia:

        # openssl passwd -1 changeme

    Ustawianie hasła głównego z guestfish:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Zmienianie drugie pole administratora z "!" Aby zaszyfrowane hasło.

3.  Tworzenie maszyny wirtualnej w KVM z obrazu qcow2, Ustaw typ dysku **qcow2**i ustaw modelu urządzenia interfejsu wirtualnej sieci **virtio**. Następnie uruchom maszyny wirtualnej i zaloguj się jako głównego.

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

6.  Przenoszenie (lub usuwanie) z regułami udev, aby uniknąć generowania statyczne reguły dla interfejsu Ethernet. Te reguły spowodować problemy, jeśli klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # chkconfig network on

8.  Zarejestruj swoją subskrypcję funkcję czerwony, aby umożliwić instalację pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, otwórz `/boot/grub/menu.lst` w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA z powodu błędu w wersji jądra jest używana przez RHEL 6.

    Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.
    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

10. Dodawanie funkcji Hyper-V moduły do initramfs:  

    Edytowanie `/etc/dracut.conf` i dodawanie zawartości: += add_drivers "hv_vmbus hv_netvsc hv_storvsc"

    Odbudowywanie initramfs: # dracut – f - v

11. Odinstaluj inicjowania chmurze:

        # yum remove cloud-init

12. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania:

        # chkconfig sshd on

    Modyfikowanie /etc/ssh/sshd_config, aby uwzględnić następujące wiersze:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Uruchom ponownie sshd:

        # service sshd restart

13. Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w **/etc/waagent.conf** :

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister subskrypcji (w razie potrzeby), uruchamiając następujące polecenie:

        # subscription-manager unregister

17. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Zamknij maszyn wirtualnych w KVM.

19. Konwertowanie obrazu na qcow2 format wirtualnego dysku twardego.
    Najpierw przekonwertować obraz na format nieprzetworzonych:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Upewnij się, że rozmiar obrazu nieprzetworzonych jest wyrównany 1 MB. W przeciwnym razie zaokrąglić rozmiar wyrównać 1 MB: # MB = $((1024*1024)) rozmiar # = $(qemu img informacje -f nieprzetworzonych — wyjściowy json "rhel-6.7.raw" | \ gawk "zgodne (0 zł /"wirtualnej": ([0-9] +), /, val) {drukowanie val[1]}') # rounded_size = $((($size-$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Konwertowanie nieprzetworzonych dysku wirtualnego dysku twardego stałym rozmiarze:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Przygotować maszyny wirtualnej RHEL 7.1 i 7.2 z KVM###


1.  Pobierz obraz KVM RHEL 7.1 (lub 7.2) z czerwonym ustawień witryny sieci Web. Użyjemy RHEL 7.1 jako w tym przykładzie.

2.  Ustawianie hasła głównego.

    Generowanie zaszyfrowane hasło, a następnie skopiuj dane wyjściowe polecenia:

        # openssl passwd -1 changeme

    Ustawianie hasła głównego z guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Zmienianie drugie pole użytkownika z uprawnieniami administratora z "!" Aby zaszyfrowane hasło.

3.  Tworzenie maszyny wirtualnej w KVM z obrazu qcow2, Ustaw typ dysku **qcow2**i ustaw modelu urządzenia interfejsu wirtualnej sieci **virtio**. Następnie uruchom maszyny wirtualnej i zaloguj się jako głównego.

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

6.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # chkconfig network on

7.  Zarejestruj swoją subskrypcję funkcję czerwony, aby włączyć instalacji pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, otwórz `/etc/default/grub` w edytorze tekstów i Edytuj parametr **GRUB_CMDLINE_LINUX** . Na przykład:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.
    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

9.  Po zakończeniu edycji `/etc/default/grub`, uruchom następujące polecenie, aby odbudować konfiguracji chodników:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Dodawanie funkcji Hyper-V moduły do initramfs:

    Edytowanie `/etc/dracut.conf` i dodawanie zawartości:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Odbudowywanie initramfs:

        # dracut –f -v

11. Odinstaluj inicjowania chmurze:

        # yum remove cloud-init

12. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania:

        # systemctl enable sshd

    Modyfikowanie /etc/ssh/sshd_config, aby uwzględnić następujące wiersze:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Uruchom ponownie sshd:

        systemctl restart sshd

13. Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # yum install WALinuxAgent

    Włączanie usługi waagent:

        # systemctl enable waagent.service

15. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego. Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), zmodyfikuj ustawienia następujących parametrów w `/etc/waagent.conf` odpowiednio:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister subskrypcji (w razie potrzeby), uruchamiając następujące polecenie:

        # subscription-manager unregister

17. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Zamknij maszyny wirtualnej KVM.

19. Konwertowanie obrazu na qcow2 format wirtualnego dysku twardego.

    Najpierw przekonwertować obraz na format nieprzetworzonych:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Upewnij się, że rozmiar obrazu nieprzetworzonych jest wyrównany 1 MB. W przeciwnym razie zaokrąglić rozmiar wyrównać 1 MB:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Konwertowanie nieprzetworzonych dysku wirtualnego dysku twardego stałym rozmiarze:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Przygotowywanie maszyny wirtualnej oparte na czerwony funkcję z VMware
### <a name="prerequisites"></a>Wymagania wstępne
W tej sekcji założono, że został zainstalowany maszyny wirtualnej RHEL w VMware. Aby uzyskać szczegółowe informacje dotyczące sposobu zainstalowania systemu operacyjnego w VMware zobacz [Podręcznik instalacji systemu operacyjnego gościa VMware](http://partnerweb.vmware.com/GOSIG/home.html).

- Po zainstalowaniu systemu operacyjnego Linux, firma Microsoft zaleca używanie standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego maszyn wirtualnych dotyczących rozwiązywania problemów. LVM lub RAID mogą zostać zastosowane na dyskach danych.

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Możesz skonfigurować agenta Linux, aby utworzyć plik wymiany na dysku zasobów. Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Po utworzeniu wirtualnego dysku twardego wybierz **dysk wirtualny magazynu jako jeden plik**.



### <a id="rhel67vmware"> </a>Przygotować maszyny wirtualnej 6,7 RHEL z VMware###

1.  Odinstaluj NetworkManager, uruchamiając następujące polecenie:

         # sudo rpm -e --nodeps NetworkManager

    Należy zauważyć, że jeśli pakiet nie jest już zainstalowany, to polecenie nie powiedzie się komunikat o błędzie. Jest to planowane.

2.  Utwórz plik o nazwie **sieci** w katalogu/etc/sysconfig/zawierający następujący tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Tworzenie pliku o nazwie **ifcfg eth0** w /etc/sysconfig/network-scripts-katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Przenoszenie (lub usuwanie) z regułami udev, aby uniknąć generowania statyczne reguły dla interfejsu Ethernet. Te reguły spowodować problemy, jeśli klonowanie maszyny wirtualnej w programie Microsoft Azure lub funkcji Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

6.  Zarejestruj swoją subskrypcję funkcję czerwony, aby umożliwić instalację pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, Otwórz "-boot/grub/menu.lst" w edytorze tekstów i upewnij się, że domyślnego jądra obejmuje następujące parametry:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Spowoduje to wyłączenie NUMA z powodu błędu w wersji jądra jest używana przez RHEL 6.
    Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.
    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

9.  Dodawanie funkcji Hyper-V moduły do initramfs:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna. Modyfikowanie `/etc/ssh/sshd_config` do uwzględnienia następujący wiersz:

        ClientAliveInterval 180

11. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Nie twórz wymiany miejsca na dysku systemu operacyjnego:

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), zmodyfikuj ustawienia następujących parametrów w `/etc/waagent.conf` odpowiednio:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Unregister subskrypcji (w razie potrzeby), uruchamiając następujące polecenie:

        # sudo subscription-manager unregister

14. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Zamknij maszyn wirtualnych i przekonwertować plik VMDK plik VHD.

    Najpierw przekonwertować obraz na format nieprzetworzonych:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Upewnij się, że rozmiar obrazu nieprzetworzonych jest wyrównany 1 MB. W przeciwnym razie zaokrąglić rozmiar wyrównać 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Konwertowanie nieprzetworzonych dysku wirtualnego dysku twardego stałym rozmiarze:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Przygotować maszyny wirtualnej RHEL 7.1 i 7.2 z VMware###

1.  Utwórz plik o nazwie **sieci** w katalogu/etc/sysconfig/zawierający następujący tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Tworzenie pliku o nazwie **ifcfg eth0** w /etc/sysconfig/network-scripts-katalog zawierający następujący tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Upewnij się, że Usługa sieciowa będzie uruchamiane podczas uruchamiania, uruchamiając następujące polecenie:

        # sudo chkconfig network on

4.  Zarejestruj swoją subskrypcję funkcję czerwony, aby umożliwić instalację pakietów z repozytorium RHEL, uruchamiając następujące polecenie:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Modyfikowanie linii uruchamiania jądra w konfiguracji chodników w celu dołączenia jądra dodatkowe parametry Azure. Aby to zrobić, otwórz `/etc/default/grub` w edytorze tekstów i Edytuj parametr **GRUB_CMDLINE_LINUX** . Na przykład:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    To zapewni również, że wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów. Oprócz powyższych czynności zalecamy usunięcie następujących parametrów:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.
    Opcja crashkernel może być lewej skonfigurowany w razie potrzeby, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych przez 128 MB lub większej. Może to być problemy na mniejszych maszyn wirtualnych.

6.  Po zakończeniu edycji `/etc/default/grub`, uruchom następujące polecenie, aby odbudować konfiguracji chodników:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Dodawanie funkcji Hyper-V moduły do initramfs:

    Edytowanie `/etc/dracut.conf`, dodawanie zawartości:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Odbudowywanie initramfs:

        # dracut –f -v

8.  Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania. Zazwyczaj jest to wartość domyślna. Modyfikowanie `/etc/ssh/sshd_config` do uwzględnienia następujący wiersz:

        ClientAliveInterval 180

9.  Pakiet WALinuxAgent `WALinuxAgent-<version>` przypisany repozytorium dodatki funkcję czerwony. Włącz repozytorium dodatki, uruchamiając następujące polecenie:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Zainstaluj agenta Linux Azure, uruchamiając następujące polecenie:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Nie należy tworzyć wymiany miejsca na dysku systemu operacyjnego. Azure Linux Agent może automatycznie skonfigurować obszar wymiany przy użyciu dysku zasobów lokalnych, który jest dołączony do maszyn wirtualnych po maszyn wirtualnych jest włączona na Azure. Zwróć uwagę na dysk lokalny zasób jest dyskiem tymczasowe i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), zmodyfikuj ustawienia następujących parametrów w `/etc/waagent.conf` odpowiednio:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Jeśli chcesz unregister subskrypcji, uruchom następujące polecenie:

        # sudo subscription-manager unregister

13. Uruchom następujące polecenia deprovision maszyny wirtualnej i przygotować ją do inicjowania obsługi administracyjnej Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Zamknij maszyn wirtualnych i przekonwertować plik VMDK format wirtualnego dysku twardego.

    Najpierw przekonwertować obraz na format nieprzetworzonych:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Upewnij się, że rozmiar obrazu nieprzetworzonych jest wyrównany 1 MB. W przeciwnym razie zaokrąglić rozmiar wyrównać 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Konwertowanie nieprzetworzonych dysku wirtualnego dysku twardego stałym rozmiarze:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Przygotowywanie maszyny wirtualnej oparte na czerwony funkcję z ISO za pomocą pliku kickstart automatycznie


### <a id="rhel7xkickstart"> </a>Przygotowywanie maszyny wirtualnej RHEL 7.1 i 7.2 z pliku kickstart###


1.  Tworzenie pliku kickstart z zawartością poniżej, a następnie zapisz plik. Aby uzyskać szczegółowe informacje o instalacji kickstart zobacz [Przewodnik po instalacji Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Umieść plik kickstart w miejsce, w którym jest dostępna z systemu instalacji.

3.  Utwórz nowy maszyn wirtualnych w Menedżera funkcji Hyper-V. Na stronie **Nawiązywanie połączenia wirtualnego dysku twardego** wybierz pozycję **Dołącz wirtualny dysk twardy w dalszej części**i ukończyć Kreatora nowej maszyny wirtualnej.

4.  Otwieranie ustawień maszyn wirtualnych:

    .  Dołączanie nowy wirtualny dysk twardy do maszyn wirtualnych. Upewnij się wybrać **Format wirtualnego dysku twardego** i **Stały rozmiar**.

    b.  Dołączanie instalacji ISO do stacji dysków DVD.

    c.  Ustawianie BIOS z dysku CD.

5.  Rozpocznij maszyn wirtualnych. Gdy pojawi się w podręczniku instalacji, naciśnij klawisz **Tab** , aby skonfigurować opcje uruchamiania.

6.  ENTER `inst.ks=<the location of the kickstart file>` na końcu opcje uruchamiania, a następnie naciśnij klawisz **Enter**.

7.  Poczekaj na zakończenie instalacji. Po jego zakończeniu maszyn wirtualnych zostanie zamknięty automatycznie. Usługi Linux wirtualny dysk twardy jest teraz gotowy do przekazania do Azure.

## <a name="known-issues"></a>Znane problemy
Istnieją znane problemy podczas używania 7.1 RHEL w funkcji Hyper-V i Azure.

### <a name="disk-io-freeze"></a>Blokowanie We/Wy dysku

Ten problem może wystąpić podczas operacji We/Wy dysku częste miejsca do magazynowania z 7.1 RHEL w funkcji Hyper-V i Azure.   

Odtwórz kurs:

Ten problem występuje sporadycznie. Jednak występuje więcej często podczas częste operacji We/Wy dysku w funkcji Hyper-V i Azure.   


[AZURE.NOTE] Ten znany problem zostały omówione w kolorze czerwonym funkcję. Aby zainstalować skojarzone poprawki, uruchom następujące polecenie:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Sterownik funkcji Hyper-V nie mogły zawarte w początkowej dysku RAM, używając hiperwizora nienależących do funkcji Hyper-V

W niektórych przypadkach programów instalacyjnych Linux może nie obejmować sterowniki funkcji Hyper-v początkowej dysku RAM (initrd lub initramfs) o ile nie wykrywa, że jest on uruchomiony w środowisku funkcji Hyper-V.

Podczas korzystania z systemu różnych wirtualizacji (to znaczy Virtualbox, Xen itp.) do przygotowania obrazu Linux, może być konieczne odbudowanie initrd w celu zapewnienia, że co najmniej modułów jądra hv_vmbus i hv_storvsc są dostępne na początkowej dysku RAM. Jest to znany problem z co najmniej w systemach na podstawie nadrzędny rozmieszczenia funkcję czerwony.

Aby rozwiązać ten problem, należy dodać moduły funkcji Hyper-V do initramfs i jej odbudowanie:

Edytowanie `/etc/dracut.conf` i dodawanie zawartości:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Odbudowywanie initramfs:

        # dracut –f -v

Aby uzyskać więcej informacji zobacz informacje dotyczące [odbudowanie initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Następne kroki
Teraz możesz używać dysku twardego Red funkcję Enterprise Linux wirtualnych do tworzenia nowych maszyn wirtualnych platformy Azure. Jeśli jest to, że przesyłana pliku VHD Azure po raz pierwszy, zobacz kroki 2 i 3 w [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający system operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Aby uzyskać więcej informacji na temat monitorami, które są certyfikowane uruchamianie Red funkcję Enterprise Linux, zobacz [funkcję czerwony witryny sieci Web](https://access.redhat.com/certified-hypervisors).
