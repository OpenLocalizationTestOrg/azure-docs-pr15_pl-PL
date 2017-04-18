<properties
    pageTitle="Optymalizowanie MySQL na maszyny wirtualne Linux | Microsoft Azure"
    description="Dowiedz się, jak zoptymalizować MySQL uruchomionych Azure maszyny wirtualnej (maszyn wirtualnych) systemem Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optymalizacja wydajności MySQL na maszyny wirtualne Azure Linux

Istnieje wiele czynników, które wpływają na wydajność MySQL Azure, zarówno w konfiguracji sprzętu wirtualnego zaznaczenia i oprogramowania. W tym artykule omówiono optymalizacji wydajności za pomocą miejsca do magazynowania, system i konfiguracji bazy danych.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Korzystanie z RAID na Azure maszyn wirtualnych
Miejsce do magazynowania jest współczynnik klawiszy wpływa na wydajność bazy danych w środowisku chmury.  W porównaniu do jednego dysku, RAID zapewnia szybszy dostęp za pośrednictwem współbieżności.  Aby uzyskać więcej szczegółów odwołują się do [Standardowego poziomy RAID](http://en.wikipedia.org/wiki/Standard_RAID_levels) .   

Przepustowość dysku i czas reakcji we/wy w Azure można znacznie zwiększyć za pośrednictwem RAID. Testów ćwiczenia Pokaż We/Wy dysku może być podwójny przepustowość i czas reakcji we/wy można podzielić na pół średnio po podwoić jest liczba dysków RAID (od 2 do 4, 4-8 itp.). Aby uzyskać szczegółowe informacje, zobacz [dodatek A](#AppendixA) .  

Oprócz dyskowej wydajności MySQL zwiększa Zwiększ poziom RAID.  Aby uzyskać szczegółowe informacje, zobacz [Dodatek B](#AppendixB) .  

Warto też rozważyć rozmiar segmentu. Ogólnie Jeśli rozmiar fragmentu, zostanie wyświetlony dolnym nakład pracy szczególnie w przypadku dużych zapisu. Jednak jeśli rozmiar segmentu jest zbyt duża, może dodać dodatkowe obciążenie i nie można korzystać z RAID. Bieżący rozmiar domyślny jest 512KB, który wynosi optymalne dla większości środowisku produkcyjnym. Aby uzyskać szczegółowe informacje, zobacz [Dodatku C](#AppendixC) .   

Pamiętaj, że są limity na dyskach ile możesz dodać maszyn wirtualnych różnych typów. Limity te są wyszczególnione w [maszyn wirtualnych i rozmiarów usługi Cloud Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Konieczne będzie 4 dysków załączonych danych do opracowania przykład RAID w tym artykule, mimo że można skonfigurować RAID, używając mniej dysków.  

W tym artykule założono, zostały już utworzone maszyny wirtualnej Linux i MYSQL zainstalowaniu i skonfigurowaniu. Więcej informacji na temat rozpoczęcia pracy można znaleźć w sposób instalowania MySQL Azure.  

###<a name="setting-up-raid-on-azure"></a>Aby skonfigurować RAID Azure
Poniższe kroki pokazują sposób tworzenia RAID Azure za pomocą portalu klasyczny Azure. Możesz również skonfigurować RAID za pomocą skrypty środowiska Windows PowerShell.
W tym przykładzie skonfiguruje możemy z 4 dysków RAID 0.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Krok 1: Dodawanie dysku danych na komputerze wirtualnych  

Na stronie maszyn wirtualnych Azure portal klasyczny kliknij maszyny wirtualnej, do którego chcesz dodać dyskiem danych. W tym przykładzie wirtualna maszyna jest mysqlnode1.  

![][1]

Na stronie maszyny wirtualnej kliknij pozycję **pulpit nawigacyjny**.  

![][2]


Na pasku zadań kliknij przycisk **Dołącz**.

![][3]

A następnie kliknij przycisk **Dołącz pusty dysk**.  

![][4]

W przypadku dysków danych **Preferencji pamięci podręcznej hosta** powinna być równa **Brak**.  

Spowoduje to dodanie jeden pusty dysk do komputera wirtualnych. Powtórz ten krok trzy razy, dzięki czemu będziesz mieć 4 dyski danych dla RAID.  

Dodano dyski maszyny wirtualnej widać, sprawdzając dziennik komunikatów jądra. Na przykład aby zobaczyć to na Ubuntu, należy użyć następującego polecenia:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Krok 2: Tworzenie RAID z dodatkowe dyski
Należy wykonać w tym artykule, aby uzyskać szczegółowe instrukcje konfiguracji RAID:  

[Konfigurowanie oprogramowania RAID w systemie Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Jeśli korzystasz z systemu plików XFS, wykonaj poniższe czynności, po utworzeniu RAID.

Aby zainstalować XFS Debian, Ubuntu lub mennic Linux, wpisz następujące polecenie:  

    apt-get -y install xfsprogs  

Aby zainstalować XFS na Fedora, CentOS lub RHEL, wpisz następujące polecenie:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Krok 3: Konfigurowanie nowej ścieżki miejsca do magazynowania
Użyj następującego polecenia:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Krok 4: Skopiowania oryginalne dane do nowego miejsca do magazynowania
Użyj następującego polecenia:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Krok 5: Modyfikowanie uprawnień w celu zapewnienia dostępu MySQL (odczytu i zapisu) dysku danych
Użyj następującego polecenia:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Dostosowywanie algorytmu planowania We/Wy dysku
Linux wykonuje cztery typy planowania algorytmów we/wy:  

-   Algorytm aktualizujący nie działa (nr operacji)
-   Termin ostateczny algorytm (terminu)
-   Algorytm Kolejkowanie całkowicie projektu naukowego (CFQ)
-   Algorytm okresu budżetu (Anticipatory)  

Można wybierać różne pracownikom we/wy w różnych scenariuszach w celu zoptymalizowania wydajności. W środowisku całkowicie dostępie jest nie wielka różnica między CFQ i termin realizacji oferty algorytmów wydajności. Zalecane jest zazwyczaj Ustaw środowisku bazy danych MySQL ostatecznego stabilnością. Jeśli istnieje wiele kolejnych we/wy, CFQ może zmniejszyć wydajność wejścia/wyjścia dysku.   

SSD i innych urządzeń za pomocą aktualizujący nie działa lub termin ostateczny można uzyskać lepszą wydajność niż harmonogram domyślny.   

Z jądra 2,5 domyślny algorytm planowania we/wy jest terminu ostatecznego. Począwszy od jądrze 2.6.18, CFQ stał się domyślny algorytm planowania we/wy.  Można określić to ustawienie w momencie uruchamiania jądra lub dynamicznie zmodyfikować to ustawienie, gdy w systemie.  

Poniższy przykład przedstawiono sposób sprawdzić i ustawić harmonogram domyślny algorytm aktualizujący nie działa.  

Rozkład Debian rodziny:

###<a name="step-1view-the-current-io-scheduler"></a>Krok 1 widok bieżący/Wy harmonogramu
Użyj następującego polecenia:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Zostanie wyświetlony po produkcja, która wskazuje bieżący harmonogram.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Krok 2. Zmienianie obecne urządzenie We/Wy planowania algorytm (/ deweloperów/sda)
Użyj następujących poleceń:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Ustawienie to dla /dev/sda nie jest przydatne. Należy ustawić na wszystkich dyskach danych miejsce, w którym znajduje się baza danych.  

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

##<a name="configure-system-file-operations-settings"></a>Konfigurowanie ustawień operacji systemu plików
Jeden najlepszym rozwiązaniem jest aby wyłączyć funkcję rejestrowania atime w systemie plików. Atime jest podczas ostatniego dostępu do pliku. Gdy plik jest dostępny, system plików rejestruje sygnaturę czasową w dzienniku. Jednak tych informacji jest rzadko używana. Jeśli nie potrzebujesz, które będą zmniejszyć ogólny czas dostępu do dysku można ją wyłączyć.  

Aby wyłączyć rejestrowanie atime, należy zmodyfikować plik system konfiguracji plik/etc / fstab i dodawanie opcji **noatime** .  

Na przykład edytować plik /etc/fstab vim, dodając noatime, tak jak pokazano poniżej.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Następnie zainstaluj system plików przy użyciu następującego polecenia:  

    mount -o remount /RAID0

Testowanie zmienione wynik. Należy zauważyć, że po zmodyfikowaniu plik testowy czas dostępu nie jest aktualizowana.  

Przed przykład:     

![][5]

Po:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Zwiększanie maksymalna liczba systemu obsługuje dla wysoka współbieżności
MySQL jest wysoka współbieżności bazy danych. Domyślny numer równoczesne uchwyty jest 1024 Linux, której nie zawsze jest wystarczające. **Wykonaj następujące czynności, aby zwiększyć maksymalną uchwyty równoczesne systemu do obsługi wysoka współbieżności MySQL**.

###<a name="step-1-modify-the-limitsconf-file"></a>Krok 1: Zmodyfikuj limits.conf
Dodaj następujące cztery linie w pliku /etc/security/limits.conf, aby zwiększyć maksymalna dozwolona uchwyty jednocześnie. Należy zauważyć, że 65536 maksymalną liczbę obsługujące systemu.   

    * Wygładzone nofile 65536
    * słabo nofile 65536
    * Wygładzone nproc 65536
    * słabo nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Krok 2: Aktualizacja systemu dla nowych limitów
Uruchom następujące polecenia:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Krok 3: Upewnij się, że limity są aktualizowane w czasie uruchamiania
W pliku /etc/rc.local należy umieścić następujące polecenia uruchamiania, aby zaczną obowiązywać podczas każdego uruchamiania.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optymalizacja bazy danych MySQL
Tym samym strategii określania wydajności umożliwia konfigurowanie MySQL Azure jako na komputerze lokalnym.  

Główne zasady optymalizacji We/Wy są:   

-   Zwiększ rozmiar pamięci podręcznej.
-   Ograniczyć czas reakcji we/wy.  

Aby zoptymalizować ustawienia serwera MySQL, możesz zaktualizować plik my.cnf, który jest domyślny plik konfiguracji dla serwera i komputerów klienckich.  

Następujące elementy konfiguracji są główne czynniki, które mają wpływ na wydajność MySQL:  

-   **innodb_buffer_pool_size**: puli buforów zawiera buforowane dane i indeks. Jest to zazwyczaj równa 70% pamięci fizycznej.
-   **innodb_log_file_size**: jest to rozmiar dziennika wykonaj ponownie. Upewnij się, że operacje zapisu są szybkie, niezawodne i odzyskania po awarii za pomocą dzienników wykonaj ponownie. Ustawienie 512MB, co spowoduje wyświetlenie wystarczająco dużo miejsca do rejestrowania operacji zapisu.
-   **max_connections**: czasami aplikacji nie zostanie zamknięty połączeń poprawnie. Większe wartości da serwer więcej czasu, aby odtworzyć idled połączenia. Maksymalna liczba połączeń jest 10000, ale zalecana maksymalna liczba to 5000.
-   **Innodb_file_per_table**: to ustawienie Włączanie lub wyłączanie InnoDB możliwość przechowywania tabel w oddzielnym pliku. Włącz opcję zapewni efektywne zastosowano kilka czynności administracji zaawansowane. Z punktu widzenia wydajności można zwiększyć szybkość przekazywania miejsca tabeli i optymalizowanie zarządzania pozostałości. Dlatego to ustawienie zalecane jest włączone.</br>
    Z MySQL 5.6 ustawieniem domyślnym jest włączone. W związku z tym jest wymagane żadne działanie. Dla innych wersji, które są starsze niż 5.6, ustawienia domyślne jest wyłączone. Wymagane jest zmianę koloru tego włączone. I należy stosować go przed załadowaniu danych, ponieważ dotyczy tylko nowo utworzone tabele.
-   **innodb_flush_log_at_trx_commit**: wartością domyślną jest 1 w zakresie równa 0 ~ 2. Wartość domyślna to najbardziej odpowiednich opcji autonomicznego bazy danych MySQL. Ustawienie 2 umożliwia większość integralności danych oraz nadaje się do wzorca w klastrze MySQL. Ustawienie 0 umożliwia utracie danych, które mogą mieć wpływ na niezawodność, w niektórych przypadkach z lepszą wydajność i nadaje się do podrzędna w klastrze MySQL.
-   **Innodb_log_buffer_size**: Bufor dziennika umożliwia transakcje mogła być uruchamiana bez opróżnić dziennik na dysku, zanim zatwierdzenie transakcji. Jednak jeśli istnieje binarny obiekt lub duży rozmiar pola tekstowego, będzie bardzo szybko zużycie pamięci podręcznej i we/wy dysku częste będą wyświetlane. Jest lepiej zwiększyć rozmiar buforu, jeśli zmiennej stanu Innodb_log_waits nie jest 0.
-   **query_cache_size**: najlepszym rozwiązaniem jest, aby ją wyłączyć od początku. Ustaw query_cache_size 0 (teraz jest to domyślne ustawienie w MySQL 5.6) i innymi metodami, aby przyspieszyć kwerendy.  

Zobacz [Dodatku D](#AppendixD) do porównywania wydajności po optymalizacji.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Włączanie dziennika powolne kwerend MySQL do analizy gardła wydajności
Dziennik działa wolno kwerendy MySQL pomaga określić powolne kwerend dla MySQL. Po włączeniu dziennika kwerend działa wolno MySQL, MySQL narzędzi, takich jak **mysqldumpslow** służy do identyfikowania gardła wydajności.  

Należy pamiętać, że domyślnie to nie jest włączony. Włączanie dziennika powolne kwerend może być korzystanie ze niektóre zasoby Procesora. W związku z tym zaleca się włączenie to tymczasowo dotyczących rozwiązywania problemów gardła wydajności.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Krok 1: Zmodyfikuj my.cnf, dodając kolejne wiersze na końcu   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Krok 2: Uruchom ponownie serwer mysql
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Krok 3: Sprawdź, czy ustawienie trwa efekt za pomocą polecenia "Pokaż"

![][7]   

![][8]

W tym przykładzie widać włączona funkcja działa wolno kwerendy. Następnie za pomocą narzędzia **mysqldumpslow** do określenia gardła wydajności i optymalizacji wydajności, takie jak dodawanie indeksy.





##<a name="appendix"></a>Dodatek

Oto przykładowe wydajności testowymi danymi wyprodukowano w środowisku ćwiczenia docelowej, zapewniają ogólne na wydajność trendów danych z różnych wydajności dostosowywanie metod, jednak wyniki mogą się różnić w różnych wersjach środowiska lub produktu.

<a name="AppendixA"></a>Dodatek A:  
**Wydajność dysku (operacji i/o na SEKUNDĘ) z RAID różne poziomy**


![][9]

**Test polecenia:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Uwaga: Obciążenie pracą test wykorzystuje 64 wątki próby osiągnięcia górną granicę RAID.

<a name="AppendixB"></a>Dodatek B:  
**Porównanie wydajności (przepustowość) MySQL RAID różne poziomy**   
(System plików XFS)


![][10]  
![][11]

**Test polecenia:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Porównanie wydajności (OLTP) MySQL RAID różne poziomy**  
![][12]

**Test polecenia:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Dodatek C:   
**Porównanie wydajności (operacji i/o na SEKUNDĘ) dysku dla różnych rozmiarach**  
(System plików XFS)


![][13]

**Test polecenia:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Uwaga, rozmiar pliku do testowania ten jest odpowiednio 30GB i 1, z RAID 0(4 disks) XFS pole systemu.


<a name="AppendixD"></a>Dodatek c  
**Porównanie wydajności (przepustowość) MySQL przed i po optymalizacji**  
(XFS File System)


![][14]

**Test polecenia:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Ustawienie konfiguracji domyślne i optmization jest następująca:**

|Parametry |Domyślne    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Brak   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Bardziej szczegółowe optymalizacji parametrów konfiguracji, zapoznaj się z instrukcjami oficjalnym mysql.  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Środowisko testowania**  

|Sprzętowe   |Szczegóły
|-----------|-------
|Procesor    |AMD Opteron(tm) procesor 4171 HE / 4 rdzenie
|Pamięci |14G
|dysku   |10G/dysk
|system operacyjny |Ubuntu 14.04.1 KÓW



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
