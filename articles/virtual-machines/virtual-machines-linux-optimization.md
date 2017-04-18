<properties
    pageTitle="Optymalizacja usługi maszyn wirtualnych Linux Azure | Microsoft Azure"
    description="Dowiedz się kilka porad dotyczących optymalizacji aby upewnić się, że skonfigurowano usługi maszyn wirtualnych Linux umożliwiające uzyskanie optymalnej wydajności Azure"
    keywords="maszyn wirtualnych Linux, linux maszyn wirtualnych maszyn wirtualnych ubuntu" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optymalizacja usługi maszyn wirtualnych Linux Azure

Tworzenie maszyny wirtualnej Linux (maszyn wirtualnych) jest łatwe, można wykonać z poziomu wiersza polecenia lub z portalu. Ten samouczek pokazano, jak zapewnić skonfigurowano go w celu zoptymalizowania jego wydajności platformy Microsoft Azure. W tym temacie używa maszyn wirtualnych Ubuntu serwera, ale możesz również utworzyć przy użyciu [obrazy jako szablonów](virtual-machines-linux-create-upload-generic.md)maszyn wirtualnych Linux.  

## <a name="prerequisites"></a>Wymagania wstępne

W tym temacie założono już działa Azure subskrypcji ([bezpłatnej wersji próbnej zapisywania](https://azure.microsoft.com/pricing/free-trial/)) [zainstalowany polecenie Azure](../xplat-cli-install.md) i już masz obsługi administracyjnej maszyny do subskrypcji usługi Azure. Przed zrobić cokolwiek z Azure - masz do uwierzytelnienia do swojej subskrypcji. Aby to zrobić przy użyciu interfejsu wiersza polecenia Azure, wystarczy wpisać `azure login` aby rozpocząć proces interakcyjne. 

## <a name="azure-os-disk"></a>Azure dysku systemu operacyjnego

Po utworzeniu maszyny Linux platformy Azure występują dwa dyski skojarzone z nim. /dev/SDA jest dysk systemu operacyjnego, /dev/sdb jest tymczasowy dysku.  Nie używaj głównym dysku OS (/ deweloperów/sda) do wszelkich działań z wyjątkiem systemu operacyjnego jest zoptymalizowana pod kątem szybkiego czasu uruchamiania maszyn wirtualnych, a nie umożliwia dobrej wydajności dla usługi obciążenia. Aby dołączyć jeden lub więcej dysków do swojego maszyn wirtualnych uzyskanie trwałych i zoptymalizowany miejsca do magazynowania dla określonego typu danych. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Dodawanie dysków w poszukiwaniu wydajność i rozmiar elementów docelowych 

W zależności od rozmiar pamięci Wirtualnej, można dołączać do 16 dodatkowe dyski serii A, 32 dyskach na serię D i 64 dyskach na serię G komputera — każdy do 1 TB w polu rozmiar. Możesz dodać dodatkowe dyski, stosownie do potrzeb na obszarze i wymagania dotyczące operacji i/o na sekundę. Każdy dysk ma docelowy wydajności operacji i/o na sekundę 500 dla standardowego magazynu i do maksymalnie 5000 operacji i/o na sekundę na dysku do przechowywania Premium.  Aby uzyskać więcej informacji na temat dysków w magazynie Premium odwołują się do [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla maszyny wirtualne Azure](../storage/storage-premium-storage.md)

Aby osiągnąć najwyższą operacji i/o na sekundę na dyskach Premium miejsca do magazynowania, gdzie swoje ustawienia pamięci podręcznej ustawiono "Tylko do odczytu" lub "Brak", musisz wyłączyć "przeszkód" podczas instalowania systemu plików w Linux. Nie ma potrzeby przeszkód, ponieważ zapisywania dysków w magazynie Premium kopii są trwałe tych ustawień pamięci podręcznej.

- Jeśli używasz **reiserFS**, wyłącz przeszkód za pomocą opcji instalacji "przegrody = Brak" (umożliwiających przeszkód za pomocą "przegrody = Dosunięty do")
- Jeśli używasz **ext3-ext4**, wyłącz przeszkód za pomocą opcji instalacji "przegrody = 0" (umożliwiających przeszkód za pomocą "przegrody = 1")
- Jeśli używasz **XFS**, wyłącz przeszkód za pomocą opcji instalacji "nobarrier" (umożliwiających przeszkód, użyj opcji "przegrody")

## <a name="storage-account-considerations"></a>Uwagi dotyczące konta miejsca do magazynowania

Po utworzeniu maszyn wirtualnych usługi Linux w Azure możesz upewnij się, że dołączyć dyski z magazynu kont znajdujących się w tym samym regionie jako usługi maszyn wirtualnych w celu zapewnienia pobliżu i zminimalizowania czasu oczekiwania w sieci.  Każdego konta standardowego magazynu może zawierać maksymalnie 20 k operacji i/o na sekundę i pojemności rozmiar 500 TB.  Dzieje do około 40 dysków mocno, włączając zarówno dysku systemu operacyjnego i wszelkie dyski danych, które można tworzyć. W przypadku kont Premium miejsca do magazynowania istnieje limit maksymalny operacji i/o na sekundę, ale istnieje limit rozmiaru 32 TB. 

Gdy sposób postępowania z wysoki obciążenia operacji i/o na sekundę i wybrano standardowy miejsca do magazynowania dla dysków, może być konieczne podzielić na dyskach na wiele kont przestrzeni dyskowej, aby upewnić się, że nie trafisz 20 000 operacji i/o na sekundę termin standardowego magazynu kont. Usługi maszyn wirtualnych może zawierać kombinację dysków z wielu kont innego miejsca do magazynowania i ich typów magazynowania uzyskanie optymalnej konfiguracji. 

## <a name="your-vm-temporary-drive"></a>Dysk tymczasowe maszyn wirtualnych

Domyślnie podczas tworzenia Głosowa, Azure oferuje OS dysku (-deweloperów-sda) i tymczasowe (/ deweloperów/sdb).  Wszystkie dodatkowe dyski, które możesz dodać wyświetlany jako /dev/sdc, /dev/sdd, /dev/sde i tak dalej. Wszystkie dane na dysku tymczasowe (/ deweloperów/sdb) nie są trwałe i mogą zostać utracone, jeśli określone zdarzenia, takie jak maszyn wirtualnych zmiany rozmiaru, ponownego rozmieszczenia, lub konserwacji wymusza ponowne uruchomienie usługi maszyn wirtualnych.  Rozmiar i typ dysku tymczasowe dotyczy rozmiar pamięci Wirtualnej, wybrana w czasie rozmieszczania. W przypadku dowolnej wielkości premium maszyny wirtualne (seria DS, G i DS_V2) tymczasowe dysk jest przechowywana w lokalnym SSD dodatkowe wydajności 48 k operacji i/o na sekundę. 

## <a name="linux-swap-file"></a>Plik wymiany Linux

W przypadku maszyn wirtualnych usługi Azure z Ubuntu lub CoreOS obrazu, można użyć CustomData wysyłanie cloud-config do inicjowania chmury. Jeśli [przekazać obraz niestandardowy Linux](virtual-machines-linux-upload-vhd.md) korzystającego z chmury inicjowania, możesz także skonfigurować wymiany partycje za pomocą inicjowania chmury.

Na obrazach chmury Ubuntu można użyć inicjowania chmury celu skonfigurowania partycją wymiany. Zobacz następującą stronę w wiki Ubuntu, aby uzyskać więcej informacji: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

W przypadku obrazów bez chmury inicjowania obsługi maszyn wirtualnych Linux Agent zintegrowany z systemem operacyjnym jest obrazów maszyn wirtualnych wdrożony z Azure Marketplace. Ten agent umożliwia maszyn wirtualnych do współdziałania z różnych usług Azure. Zakładając, że wdrożono standardowy obraz z Azure Marketplace, trzeba wykonaj poniższe czynności, aby poprawnie skonfigurować ustawień pliku wymiany Linux:

Znajdowanie i modyfikowanie dwa wpisy w pliku **/etc/waagent.conf** . Sterowanie ich istnienia plik wymiany dedykowane i rozmiar pliku wymiany. Parametry, której szukasz, aby zmodyfikować są `ResourceDisk.EnableSwap=N` i`ResourceDisk.SwapSizeMB=0` 

Należy je zmienić na następujące czynności:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size w MB stosownie do potrzeb} 

Po zostały wprowadzone zmiany, będzie konieczne ponowne uruchomienie waagent lub ponowne uruchamianie usługi maszyn wirtualnych Linux, aby uwzględnić zmiany.  Znasz zostały wprowadzone zmiany i utworzono plik wymiany, korzystając z `free` polecenie, aby wyświetlić ilość wolnego miejsca. W poniższym przykładzie ma utworzonych w wyniku modyfikowania pliku waagent.conf plik wymiany 512MB.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>Algorytm planowania wejścia/wyjścia do przechowywania Premium

Z 2.6.18 jądra Linux, domyślny planowania algorytm We/Wy została zmieniona z terminem na CFQ (całkowicie projektu naukowego algorytmu Kolejkowanie). Dla wzorców we/wy dostępie są nieznaczne wydajności różnice między CFQ i termin realizacji oferty.  W przypadku opartych na SSD dysków umożliwiającej głównie sekwencyjne wzorca we/wy dysku przełączanie z powrotem do algorytmu aktualizujący nie działa lub termin ostateczny można uzyskać lepszą wydajność wejścia/wyjścia.

### <a name="view-the-current-io-scheduler"></a>Wyświetlanie bieżącego harmonogramu wejścia/wyjścia

Użyj następującego polecenia:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Zostanie wyświetlony po produkcja, która wskazuje bieżący harmonogram.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Zmienianie obecne urządzenie We/Wy planowania algorytm (/ deweloperów/sda)

Użyj następujących poleceń:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Ustawienie to dla /dev/sda nie jest przydatne. Należy ustawić na wszystkich dyskach danych miejsce, w którym sekwencyjne we/wy dominuje wzorca we/wy.  

Powinien zostać wyświetlony następujący wynik, wskazująca, że ma zostały pomyślnie odbudowany tego grub.cfg i że harmonogram domyślny została zaktualizowana do aktualizujący nie działa.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

W przypadku Redhat rozkładu rodziny wystarczy następujące polecenie:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Za pomocą oprogramowania RAID uzyskanie wyższej I-Ops

Jeśli z obciążeń pracą wymagają operacji więcej i/o na sekundę niż jednego dysku, należy użyć konfiguracji RAID oprogramowania z wielu dysków. Ponieważ Azure wykonuje już dysku elastyczność w warstwie tkaninie lokalnych, uzyskania najwyższego poziomu wydajności z konfiguracji rozkładanie RAID 0.  Musisz obsługi administracyjnej i tworzenie nowych dysków w środowisku Azure i dołączyć je do swojego maszyn wirtualnych Linux przed podziału, formatowanie i instalowanie dysków.  Więcej informacji na temat konfigurowania konfigurację RAID oprogramowania w swojej maszyn wirtualnych Linux platformy azure znajdują się w dokumencie **[Konfigurowanie RAID oprogramowania w systemie Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Następne kroki

Pamiętaj, że zgodnie z wszystkich dyskusji optymalizacji, trzeba wykonać testy przed i po każdej zmianie do pomiaru wpływu posiadanych zmiany.  Optymalizacja jest proces krok po kroku, w którym mają różne wyniki na różnych komputerach w środowisku.  Co odpowiada jedna konfiguracja może nie działać przez inne osoby.

Niektóre przydatne łącza do dodatkowych zasobów: 

- [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych](../storage/storage-premium-storage.md)
- [Przewodnik użytkownika Azure agenta Linux](virtual-machines-linux-agent-user-guide.md)
- [Optymalizacja wydajności MySQL na maszyny wirtualne Azure Linux](virtual-machines-linux-classic-optimize-mysql.md)
- [Konfigurowanie oprogramowania RAID w systemie Linux](virtual-machines-linux-configure-raid.md)
