<properties
    pageTitle="Tworzenie i przekazywanie Linux wirtualnego dysku twardego platformy Azure"
    description="Dowiedz się, jak tworzyć i przekazywać Azure wirtualnego dysku twardego (VHD) z systemem operacyjnym Linux."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informacje dotyczące innych niż zatwierdzone dystrybucji #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Ważne**: SLA platformy Azure dotyczy środowisku maszyn wirtualnych systemu operacyjnego Linux tylko wtedy, gdy jest używany jeden z [zatwierdzony dystrybucji](virtual-machines-linux-endorsed-distros.md) . Wszystkich przydziałów Linux, które znajdują się w galerii Azure obraz są zatwierdzonego rozkładów o konfiguracji.

- [Linux Azure - zatwierdzone dystrybucji](virtual-machines-linux-endorsed-distros.md)
- [Obsługa obrazów Linux platformy Microsoft Azure](https://support.microsoft.com/kb/2941892)

Wszystkich przydziałów uruchomionych Azure muszą spełniać wymagania wstępne dotyczące mieć możliwość poprawnie działać na platformie.  W tym artykule w żadnym wypadku nie jest pełna, jak każdej rozkład jest inny; a jest bardzo możliwe, że nawet jeśli są spełnione wszystkie poniższe kryteria będzie nadal konieczne znacznie dostosować a systemem Linux, aby upewnić się, że poprawnie działa na platformie.

Jest dlatego zalecamy uruchamiania z jednym z naszych [Linux na rozkład zatwierdzone Azure](virtual-machines-linux-endorsed-distros.md) , jeśli to możliwe. Następujące artykuły przeprowadzi Cię przez przygotowywania różnych zatwierdzonego dystrybucji Linux, które są obsługiwane na Azure:

- **[Oparte na centOS dystrybucji](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Czerwone funkcję Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Dalszej części tego artykułu poświęcona ogólne wskazówki dotyczące uruchomionych Azure do dystrybucji Linux.


## <a name="general-linux-installation-notes"></a>Uwagi dotyczące instalacji Linux ogólne ##

- VHDX format nie jest obsługiwany w Azure, tylko **stałe wirtualnego dysku twardego**.  Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet wirtualnego dysku twardego Konwertuj format wirtualnego dysku twardego można konwertować na dysk. Jeśli korzystasz z VirtualBox oznacza to, wybierając **Stały rozmiar** zamiast domyślnego dynamicznie przydzielany podczas tworzenia dysku.

- Podczas instalacji z systemem Linux jest *zalecane* używanie standardowe partycje zamiast LVM (często domyślnego dla wielu instalacji). Pozwoli to uniknąć konfliktów nazw LVM z sklonowanym maszyny wirtualne, szczególnie jeśli dysk systemu operacyjnego kiedykolwiek musi zostać dołączony do innego identyczne maszyn wirtualnych dotyczących rozwiązywania problemów. [LVM](virtual-machines-linux-configure-lvm.md) lub [RAID](virtual-machines-linux-configure-raid.md) może być używany na dyskach danych.

- Obsługa jądra Instalowanie systemu plików UDF jest wymagane. Przy pierwszym uruchomieniu komputera Azure obsługi administracyjnej konfiguracji są przekazywane do maszyn wirtualnych Linux za pośrednictwem sformatowane UDF multimediów, podłączonego do gościa. Agent Azure Linux muszą mieć możliwość Zainstaluj system plików UDF do czytania swojej konfiguracji i obsługi administracyjnej maszyn wirtualnych.

- Linux wersjami jądra poniżej 2.6.37 nie obsługują NUMA na funkcji Hyper-V o dużym rozmiarze maszyn wirtualnych. Ten problem przede wszystkim skutki starsze dystrybucji przy użyciu poprzednie jądra czerwony funkcję 2.6.32 i został ustawiony w RHEL 6.6 (jądra-2.6.32-504). Z systemami niestandardowe jądra starsze niż 2.6.37 lub oparte na RHEL jądra starszych niż 2.6.32-504 należy ustawić parametr uruchamiania `numa=off` na jądrze wiersza polecenia w grub.conf. Aby uzyskać więcej informacji, zobacz funkcję czerwony [KB 436883](https://access.redhat.com/solutions/436883).

- Nieskonfigurowane partycją wymiany na dysku systemu operacyjnego. Aby utworzyć plik wymiany na dysku zasobów można skonfigurować agenta Linux.  Więcej informacji na ten temat można znaleźć w poniższych kroków.

- Wszystkie pliki VHD muszą mieć rozmiary, które są wielokrotności 1 MB.


### <a name="installing-linux-without-hyper-v"></a>Instalowanie Linux bez funkcji Hyper-V ###

W niektórych przypadkach programów instalacyjnych Linux może nie zawierać sterowniki funkcji Hyper-v początkowej dysk RAM (initrd lub initramfs) o ile nie wykrywa, że jest on uruchomiony środowisku funkcji Hyper-V.  Przygotuj obraz Linux za pomocą systemu różnych wirtualizacji (to znaczy Virtualbox, KVM itp.), może być konieczne odbudowanie initrd, aby upewnić się, że co najmniej `hv_vmbus` i `hv_storvsc` modułów jądra są dostępne na początkowej dysk RAM.  Jest to znany problem z co najmniej w systemach na podstawie nadrzędny rozmieszczenia funkcję czerwony.

Mechanizm odbudowanie obrazu initrd lub initramfs mogą się różnić w zależności od rozkładu. W dokumentacji usługi dystrybucyjnej lub obsługę prawidłowe procedury.  Oto przykład sposobu odbudowanie przy użyciu initrd `mkinitrd` narzędzia:

Najpierw należy utworzyć kopię zapasową istniejącego obrazu initrd:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Następnie odbudowanie initrd z `hv_vmbus` i `hv_storvsc` modułów jądra:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Zmienianie rozmiaru wirtualnych dysków twardych ###

Obrazy wirtualnego dysku twardego Azure musi mieć rozmiar wirtualny wyrównany do 1MB.  Zazwyczaj wirtualnych dysków twardych utworzony przy użyciu funkcji Hyper-V powinny być wyrównane poprawnie.  Jeśli wirtualny dysk twardy nie jest prawidłowo wyrównane następnie może zostać wyświetlony komunikat o błędzie podobny do następującego przy próbie utworzenia *obrazu* ze swojego wirtualnego dysku twardego:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Aby rozwiązać ten problem, można zmienić rozmiar maszyn wirtualnych, przy użyciu konsoli Menedżera funkcji Hyper-V lub polecenia cmdlet programu Powershell [Wirtualnego dysku twardego zmiany rozmiaru](http://technet.microsoft.com/library/hh848535.aspx) .  Jeśli nie używasz w środowisku Windows następnie zaleca się używanie qemu img do konwertowania (w razie potrzeby) i zmienianie rozmiaru wirtualnego dysku twardego.

> [AZURE.NOTE] Jest to błąd znane w wersjach qemu img > = 2.2.1 powodujące niewłaściwie sformatowany wirtualnego dysku twardego. Ten problem został rozwiązany w QEMU 2.6. Zaleca się używanie qemu-img 2.2.0 lub style niższego poziomu lub zaktualizować na 2.6 lub wyższą. Odwołanie: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Zmienianie rozmiaru wirtualny dysk twardy bezpośrednio przy użyciu narzędzia takich jak `qemu-img` lub `vbox-manage` może spowodować naliczenie będzie niemożliwy wirtualnego dysku twardego.  Dlatego zaleca się pierwszy Konwertuj wirtualny dysk twardy do obrazu dysku nieprzetworzonych.  Jeśli obraz maszyn wirtualnych został już utworzony jako obrazu dysku nieprzetworzonych (domyślnie dla niektórych monitorami, takich jak KVM) możesz pominąć ten krok:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Obliczanie wymagany rozmiar obrazu dysku, aby upewnić się, że rozmiar wirtualny jest wyrównany do 1MB.  Poniższy skrypt powłoki imprezie może pomóc to.  Skrypt używa "`qemu-img info`" do określenia wirtualnych rozmiar obrazu dysku, a następnie oblicza rozmiar do następnego 1MB:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Zmienianie rozmiaru nieprzetworzonych dysku przy użyciu $rounded_size zgodnie z powyższym skrypt:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Powrót do wirtualnego dysku twardego stałym rozmiarze teraz przekonwertuj nieprzetworzonych dysku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Wymagania dotyczące jądra Linux ##

Sterowniki Linux Integration Services (LIS) dla funkcji Hyper-V i Azure są tworzone bezpośrednio do nadrzędnego jądra Linux. Wiele dystrybucji, które obejmują ostatnich wersji jądra Linux (to znaczy 3.x) zawiera już te sterowniki lub inny sposób dostarczanych backported wersje tych sterowników z ich jądra.  Te sterowniki są stale aktualizowane w jądrze nadrzędnego z nowe poprawki i funkcje, więc jeśli to możliwe zaleca się uruchamianie [zatwierdzone rozkładu](virtual-machines-linux-endorsed-distros.md) zawierające te poprawki i aktualizacje.

Jeśli używasz wariant Red funkcję Enterprise Linux wersji **6.0 6.3**będą wymagania dotyczące instalacji najnowsze sterowniki LIS dla funkcji Hyper-V. Sterowniki znajduje się [w tej lokalizacji](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (i pochodnych) sterowniki LIS już są dostarczane z jądra, a więc żadnych pakietów dodatkowych instalacji są wymagane do uruchomienia systemy Azure.

Jeśli wymagane jest niestandardowe jądra, zaleca się przy użyciu nowszej wersji jądra (to znaczy **3.8 +**). Dla tych dystrybucji lub dostawców, którzy mają własne jądra wysiłku będą musieli regularnie Poprawka usterki systemu sterowniki LIS z nadrzędnego jądra do swojego niestandardowego jądra.  Nawet jeśli już używasz stosunkowo ostatnich wersji jądra, zdecydowanie zalecane jest do śledzenia dowolnego nadrzędny poprawki sterowniki LIS i poprawka usterki systemu tych stosownie do potrzeb. Lokalizacja pliku LIS sterownik źródła jest dostępne w pliku [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) w drzewie Linux jądra źródła:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

W bardzo minimalne braku następujących poprawek być znane powodujące problemy Azure i dlatego te muszą być zawarte w jądrze. Ta lista jest w pełnym lub wykonania wszystkich przydziałów:

- [ata_piix: odłożyć dysków sterowników funkcji Hyper-V domyślnie](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: konto w drodze pakietów w ścieżce Resetowanie](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: uniknąć zastosowania WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: Wyłącz PISANIE w tym samym RAID i sterowniki karty hosta wirtualnego](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: fix cofnięcia odwołania wskaźnik o wartości NULL](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: blokowanie we/wy może spowodować błędy bufor dzwonienia](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: chronić się przed podwójne wykonanie __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Azure agenta Linux ##

[Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) (waagent) jest wymagane poprawnie zainicjować maszyny wirtualnej Linux Azure. Można uzyskać najnowszą wersję, plik problemy lub przesyłanie żądania pobieraj [repo GitHub agenta Linux](https://github.com/Azure/WALinuxAgent).

- Linux agent jest publikowany w ramach licencji Apache 2.0. Wiele dystrybucji już znajdują się obrotów na MINUTĘ lub deb pakietów agenta, a więc w niektórych przypadkach to można instalować i aktualizacja bez trudu.

- Azure Linux Agent wymaga Python v2.6 +.

- Agent wymaga również moduł python pyasn1. Większości dystrybucji udostępnia ją jako oddzielny pakiet, która może być zainstalowana.

- W niektórych przypadkach agenta Linux Azure mogą nie być zgodne z NetworkManager. Wiele pakietów obrotów na MINUTĘ-Deb dostarczony przez dystrybucji skonfiguruj NetworkManager jako konflikt pakietu waagent, a więc spowoduje odinstalowanie NetworkManager, podczas instalowania pakietu agenta Linux.


## <a name="general-linux-system-requirements"></a>Wymagania systemowe Linux ogólne ##

- Modyfikowanie wiersza uruchamiania jądra CHODNIKÓW lub GRUB2, aby uwzględnić następujące parametry. Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Zapewni to także wszystkie wiadomości konsoli są wysyłane do pierwszego portu szeregowego mogą pomóc Azure pomocy technicznej z debugowania problemów.

    Oprócz powyższych zalecane jest *usunięcie* następujących parametrów Jeśli istnieją:

        rhgb quiet crashkernel=auto

    Graficzne i cichy uruchamiania nie są przydatne w środowisku chmury miejsce, w którym będą wszystkie dzienniki były wysyłane do portu szeregowego.

    `crashkernel` Opcja może być skonfigurowany w razie potrzeby lewej, ale notatki, że ten parametr powoduje zmniejszenie ilości dostępnej pamięci w maszyn wirtualnych 128MB lub więcej, które mogą być problemy na mniejszych maszyn wirtualnych.

- Instalowanie Azure agenta Linux

    Azure Linux Agent jest wymagane na potrzeby obsługi administracyjnej obraz Linux Azure.  Wiele dystrybucji zapewniają agenta jako pakiet obrotów na MINUTĘ lub Deb (pakiet jest zazwyczaj o nazwie "WALinuxAgent" lub "walinuxagent").  Agent można również zainstalować ręcznie, wykonując czynności opisane w [Przewodniku agenta Linux](virtual-machines-linux-agent-user-guide.md).

- Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie uruchamiania.  Zazwyczaj jest to wartość domyślna.

- Nie twórz wymiany miejsca na dysku systemu operacyjnego

    Azure Linux Agent może automatycznie skonfigurować obszar wymiany za pomocą dysku zasobów lokalnych podłączonego do maszyn wirtualnych po zainicjowaniu obsługi administracyjnej Azure. Uwaga dysku zasobów lokalnych jest *tymczasowy* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych. Po zainstalowaniu agenta Linux Azure (zobacz poprzedni krok), odpowiednio zmodyfikuj następujących parametrów w /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Jako ostatni krok Uruchom następujące polecenia, aby deprovision maszyny wirtualnej:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Na Virtualbox zobaczysz następujący komunikat o błędzie po uruchomieniu "waagent-wymusić - deprovision": `[Errno 5] Input/output error`. Ten komunikat o błędzie jest krytyczne i można je zignorować.

- Następnie należy zamknąć komputer wirtualnych i przekaż wirtualny dysk twardy Azure.
