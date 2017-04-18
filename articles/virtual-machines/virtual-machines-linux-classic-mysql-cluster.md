<properties
    pageTitle="Clusterize MySQL równoważenia obciążenia zestawów | Microsoft Azure"
    description="Konfigurowanie równoważenia obciążenia wysokiej dostępności klaster programu Linux MySQL utworzone za pomocą modelu Klasyczny wdrożenia Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Za pomocą równoważenia obciążenia zestawy clusterize MySQL w systemie Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Celem tego artykułu jest Eksplorowanie i przedstawić różnych metod można wdrożyć wysokiej dostępności systemem Linux usługi Microsoft Azure, poznawanie serwer MySQL wysokiej dostępności jako Elementarz. Klip wideo przedstawiający ta metoda jest dostępna na [kanału 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Możemy utworzyć konspekt udostępnione żaden element nie dwóch węzłach punktowe MySQL wysokiej dostępności rozwiązania opartego na DRBD, Corosync i rozrusznik. MySQL jest uruchomiony tylko jeden węzeł naraz. Czytanie i wysyłanie z zasobu DRBD również jest ograniczone do tylko jeden węzeł naraz.

Istnieje potrzeba rozwiązania VIP, takich jak LV, ponieważ używamy zestawy równoważenia obciążenia Microsoft Azure w celu włączenia funkcji okrężny i wykrywania punktu końcowego, usuwania i skuteczne odzyskanie VIP. VIP jest globalnie routingu adres IP protokołu IPv4 przypisane przez Microsoft Azure, gdy najpierw tworzymy usług w chmurze.

Inne możliwe architektury dla MySQL tym klaster NBD, Percona i Galera, a także kilka rozwiązań pośredniczącym, łącznie z co najmniej jeden są dostępne jako maszyn wirtualnych w [Magazynie maszyn wirtualnych](http://vmdepot.msopentech.com). Jak długo te rozwiązania można odtworzyć w emisji pojedynczej a multiemisji lub emisji i nie zależy od miejsca do magazynowania udostępnionej lub wiele interfejsów sieciowych, scenariusze powinny być łatwa do wdrożenia na Microsoft Azure.

Oczywiście tych klastrów architektury może zostać wydłużony do innych produktów, takich jak PostgreSQL i OpenLDAP w podobny sposób. Na przykład obciążenie równoważenia procedury udostępnionego żaden element nie został pomyślnie badania przy użyciu wielu wzorca OpenLDAP, a które można oglądać w naszym blogu kanału 9.

## <a name="getting-ready"></a>Przygotowanie

Konieczne będzie konto Microsoft Azure ważna Subskrypcja można tworzyć maszyny wirtualne co najmniej dwa (2) (znaków x był używany w tym przykładzie), ustawić podsieci, grupa koligacji i udostępniania w sieci, a także możliwość tworzenia nowych wirtualnych dysków twardych w tym samym regionie jako usługa w chmurze oraz dołączone do maszyny wirtualne Linux.

### <a name="tested-environment"></a>Środowisko sprawdzania

- Ubuntu 13.10
  - DRBD
  - Serwer MySQL
  - Corosync i rozrusznik

### <a name="affinity-group"></a>Grupa koligacji

Grupa koligacji rozwiązania jest tworzona, logując się do Azure klasyczny portalu przewijania w dół do pozycji ustawienia i tworzenie nowej grupy koligacji. Przydzielone zasoby utworzone później są przypisywane do tej grupy koligacji.

### <a name="networks"></a>Sieci

Zostanie utworzona nowa sieć i podsieć zostanie utworzona wewnątrz sieci. Wybraliśmy sieci 10.10.10.0/24 za pomocą podsieci tylko jeden /24 wewnątrz.

### <a name="virtual-machines"></a>Maszyn wirtualnych

Pierwszy maszyn wirtualnych 13.10 Ubuntu jest utworzony przy użyciu Endorsed Ubuntu Galeria i o nazwie `hadb01`. Nowe usługi w chmurze jest tworzona w procesie o nazwie hadb. Nazywamy je ten sposób, aby przedstawić charakter współużytkowane, równoważenia obciążenia, zawierających usługę po dodajemy większej ilości zasobów. Tworzenie `hadb01` jest procesu i wykonanych za pomocą portalu. Punkt końcowy dla SSH jest tworzony automatycznie, a następnie naszej sieci utworzone jest zaznaczone. Firma Microsoft także wybrać opcję utworzenia dostępność nowych ustawieniem maszyny wirtualne.

Po utworzeniu pierwszego maszyn wirtualnych (technicznego, podczas tworzenia usług w chmurze) możemy rozpocząć tworzenie drugiego Głosowa, `hadb02`. Dla drugiego maszyn wirtualnych również użyjemy maszyn wirtualnych 13.10 Ubuntu z Galerii za pomocą portalu, ale zostanie wybrany do przy użyciu usługi cloud istniejących `hadb.cloudapp.net`, zamiast tworzenia nowej witryny. Konfigurowanie sieci i dostępność powinna już być automatycznie zaznaczona us. Punkt końcowy SSH zostanie utworzony, zbyt.

Po utworzeniu obu maszyny wirtualne możemy wziąć pod uwagę port SSH dla `hadb01` (TCP 22) i `hadb02` (przypisany automatycznie przez Azure)

### <a name="attached-storage"></a>Masowej

Firma Microsoft Dołączanie nowego dysku do obu maszyny wirtualne i Utwórz nowe dyski 5 GB w procesie. Dysków będzie obsługiwana w kontenerze wirtualnego dysku twardego używany dla naszych dysków głównym systemu operacyjnego. Gdy dyski są tworzone i dołączone nie jest konieczne dla nas o ponowne uruchomienie Linux oraz jak jądrze zobaczą nowego urządzenia (zazwyczaj `/dev/sdc`, możesz sprawdzić `dmesg` wyników)

Na każdym maszyn wirtualnych możemy rozpocząć tworzenie nowej za pomocą partition `cfdisk` (podstawowy, Linux partycją) i wpisz nową tabelę partycją. **Nie należy tworzyć plików na tym partycją** .

## <a name="setting-up-the-cluster"></a>Aby skonfigurować klaster

W obu Ubuntu maszyny wirtualne trzeba zainstalować Corosync, rozrusznik i DRBD za pomocą APT. Za pomocą `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Nie można zainstalować MySQL w tej chwili** . Debian i skryptów instalacji Ubuntu inicjowania katalog danych MySQL w `/var/lib/mysql`, ale ponieważ katalogu zostaną zastąpione plików DRBD, trzeba to zrobić później.

W tym momencie również weryfikacji (za pomocą `/sbin/ifconfig`) że oba maszyny wirtualne z adresów w podsieci 10.10.10.0/24 i że one wzajemnie ping według nazwy. W razie potrzeby można również użyć `ssh-keygen` i `ssh-copy-id` aby się upewnić, że oba maszyny wirtualne komunikować się przez SSH bez konieczności hasła.

### <a name="setting-up-drbd"></a>Aby skonfigurować DRBD

Zostanie utworzony zasób DRBD, która korzysta z podstawową `/dev/sdc1` partition da `/dev/drbd1` zasób może być sformatowany przy użyciu ext3 i używane w węzłach podstawowych i pomocniczych. Aby to zrobić, otwórz `/etc/drbd.d/r0.res` i skopiuj następującą definicję zasobów. W obu maszyny wirtualne, zrób tak:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Po w ten sposób inicjowania przy użyciu zasobu `drbdadm` w obu maszyny wirtualne:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

I na koniec podstawowych (`hadb01`) wymusić własności (podstawowa) zasobu DRBD:

    sudo drbdadm primary --force r0

Jeśli Sprawdź zawartość i procesora-drbd (`sudo cat /proc/drbd`) na obu maszyny wirtualne, powinna być widoczna `Primary/Secondary` na `hadb01` i `Secondary/Primary` na `hadb02`, zgodnie z rozwiązanie w tym momencie. 5 GB dysku zostaną zsynchronizowane przez sieć 10.10.10.0/24 bezpłatnie dla klientów.

Dysk jest synchronizowane można tworzyć systemu plików na `hadb01`. Do celów testowych użyliśmy ext2, ale poniższe instrukcje spowoduje utworzenie plików ext3:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Instalowanie zasobu DRBD

Na `hadb01` firma Microsoft jest gotowa do zainstalowania zasoby DRBD. Używanie debian i pochodnych `/var/lib/mysql` jako katalog danych MySQL firmy. Ponieważ firma Microsoft nie został zainstalowany MySQL, firma Microsoft będzie utworzyć katalogu i zainstalować zasobu DRBD. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>Aby skonfigurować MySQL

Teraz możesz zainstalować MySQL na `hadb01`:

    sudo apt-get install mysql-server

Aby uzyskać `hadb02`, masz do wyboru dwie opcje. Możesz zainstalować serwer mysql, który będzie utworzyć /var/lib/mysql i wypełnij ją z katalogiem nowych danych, a następnie usunąć zawartość. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Jest to druga opcja przełączanie awaryjne do `hadb02` , a następnie zainstaluj serwer mysql (skryptów instalacyjnych zauważyć istniejącej instalacji i nie można go dotknij)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Jeśli nie planujesz pracy awaryjnej DRBD teraz, pierwszą opcję jest łatwiejsze jednak mieć prawdopodobnie mniej eleganckie. Po skonfigurowaniu tak, można rozpocząć pracy z bazy danych MySQL. Na `hadb02` (lub niezależnie od serwerów jest aktywne, zgodnie z DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Ostrzeżenie**: ten ostatnim wyłącza uwierzytelniania dla administratora w tej tabeli. To powinna zostać zastąpiona produkcji niskiej jakości udzielanie instrukcje i znajduje się tylko do celów opisowy.

Trzeba również włączyć sieć w MySQL jeśli mają być kwerendy z zewnątrz maszyny wirtualne, czyli przeznaczenia tego przewodnika. W obu maszyny wirtualne, otwórz `/etc/mysql/my.cnf` i przejdź do `bind-address`, zmienianie go z 127.0.0.1 0.0.0.0. Po zapisaniu pliku, wydać `sudo service mysql restart` na Twojej bieżącej podstawowa.

### <a name="creating-the-mysql-load-balanced-set"></a>Tworzenie Załaduj MySQL strategie zestawu

Firma Microsoft będzie wróć do portalu i przejdź do `hadb01` maszyn wirtualnych, a następnie punktów końcowych. Firma Microsoft będzie utworzyć nowy punkt końcowy, wybierz z listy rozwijanej i znaczników w oknie dialogowym *Tworzenie nowego ładowania strategie zestawu* MySQL (port TCP 3306). Będzie nazywamy naszego punktu końcowego równoważenia obciążenia `lb-mysql`. Firma Microsoft pozostawi większość opcji tylko z wyjątkiem czas, jaki będzie możemy zmniejszyć do 5 (w sekundach, minimalna)

Po utworzeniu punkt końcowy możemy przejść do `hadb02`, punkty końcowe i utworzyć nowy punkt końcowy, ale zostanie wybrany `lb-mysql`, następnie wybierz z menu rozwijanego MySQL. Za pomocą interfejsu wiersza polecenia Azure w tym kroku.

W tym momencie mamy wszystko, co jest potrzebne do ręcznego działania klaster.

### <a name="testing-the-load-balanced-set"></a>Testowanie Załaduj strategie zestawu

Testy mogą być wykonywane z zewnętrznego komputera, przy użyciu dowolnego klienta MySQL, a także aplikacji (na przykład phpMyAdmin działającego jako Azure witryny sieci Web) w tym przypadku użyliśmy MySQL w wierszu polecenia narzędzia na innym polu Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Ręczne awarii przez

Możesz teraz symulować praca awaryjna, wyłączając MySQL, przełączanie podstawowego i DRBD i ponownie uruchomić MySQL.

Na hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Następnie na hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Po pracy awaryjnej możesz ręcznie można powtórzyć zdalnego kwerendy i należy dokładnie pracy.

## <a name="setting-up-corosync"></a>Aby skonfigurować Corosync

Corosync jest podstawowej infrastruktury klaster wymagane dla rozrusznik do pracy. Dla użytkowników w wersji 1 i 2 weryfikowania (i innych metod, takich jak Ultramonkey) Corosync jest dzielony funkcji CRM rozrusznik pozostaje więcej podobne do impulsów w funkcji.

Główne ograniczenie Corosync Azure jest Corosync preferowany multiemisji nad emisji nad komunikacji emisji pojedynczej, że sieci Microsoft Azure obsługuje tylko emisji pojedynczej.

Na szczęście Corosync ma trybu emisji pojedynczej pracy i tylko rzeczywistą ograniczenie jest, ponieważ wszystkie węzły nie komunikują się między sobą *automagically*, należy zdefiniować węzły w plikach konfiguracji, łącznie z ich adresy IP. Możemy użyć plików przykład Corosync dla emisji pojedynczej i zmień tylko wiążą adres, list węzeł i katalog rejestrowania (korzysta z Ubuntu `/var/log/corosync` podczas przykładzie pliki używają `/var/log/cluster`) i włączanie narzędzia kworum.

**Notatki `transport: udpu` dyrektywy poniżej i ręcznie zdefiniowanych adresów IP dla węzłów**.

Na `/etc/corosync/corosync.conf` dla obu węzłów:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Firma Microsoft skopiuj ten plik konfiguracyjny w obu maszyny wirtualne i rozpocząć Corosync w oba węzły:

    sudo service start corosync

Wkrótce po rozpoczęciu usługę klaster należy wprowadzić w bieżącym Dzwoń i kworum powinien zostać utworzony. Firma Microsoft Sprawdź ta funkcja przeglądając dzienniki lub:

    sudo corosync-quorumtool –l

Należy wykonać dane wyjściowe, podobnie jak na poniższej ilustracji:

![corosync quorumtool - l przykładowe dane wyjściowe](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Aby skonfigurować rozrusznik

Rozrusznik używa klaster w celu monitorowania zasobów, określić, kiedy kolory podstawowe Przejdź w dół i przełączanie tych zasobów do pomocnicze. Zasoby można zdefiniować z zestawu dostępnych skryptów lub najmniej Znaczących skrypty (inicjowania jak), między innymi opcjami.

Chcemy rozrusznik "własności" zasobu DRBD punktu instalacji oraz usługę MySQL. Jeśli rozrusznik można włączać i wyłączać DRBD, zainstalować go / zakończeniu umount go i Rozpocznij i Zatrzymaj MySQL w odpowiedniej kolejności, gdy coś nieprawidłowe się dzieje z podstawową, nasz konfiguracji.

Po zainstalowaniu rozrusznik konfiguracji powinny być prosta, przykład:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Sprawdź, uruchamiając `sudo crm configure show`. Teraz utworzyć plik (na przykład, `/tmp/cluster.conf`) z następujących zasobów:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

I teraz załadować program do konfiguracji (tylko musisz to zrobić w jeden węzeł):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Upewnij się również, że rozrusznik ma być uruchamiany przy uruchomieniu w obu węzłach:

    sudo update-rc.d pacemaker defaults

Po kilku sekundach i za pomocą `sudo crm_mon –L`, sprawdź, czy jedną węzły stał się główny klaster i działa wszystkich zasobów. Aby sprawdzić, czy zasoby są uruchomione, można użyć instalacji i ps.

Zrzut ekranu poniższej `crm_mon` z jednego węzła zatrzymano (Zamknij za pomocą kontrolki C)

![węzeł crm_mon zatrzymane](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

I tym zrzucie ekranu pokazano oba węzły z jednego i jeden podrzędna:

![crm_mon operacyjne wzorca/podrzędny](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testowanie

Służymy dla symulacji automatyczne przejście. Istnieją dwa sposoby w ten sposób: wygładzone i trudne. Wygładzone sposobem jest użycie funkcji zamknięcia klaster: ``crm_standby -U `uname -n` -v on``. Dzięki we wzorcu podrzędnej będą miały. Należy pamiętać o to cofnięcie wyłączone (crm_mon informuje o jeden węzeł jest w stanie wstrzymania w inny sposób)

Twardych sposób jest zamykanie podstawowego maszyn wirtualnych (hadb01) za pośrednictwem portalu lub zmiana uruchamiania przełącznika/RL na maszyn wirtualnych (to znaczy zatrzymania, zamknięcia), a następnie chcesz możemy pomóc Corosync i rozrusznik przez sygnalizacji rozpoczęcie wzorca w dół. Firma Microsoft przetestować tę (przydatne dla konserwacja systemu windows), ale możemy również wymusić tego scenariusza, po prostu blokując maszyn wirtualnych.

## <a name="stonith"></a>STONITH

Powinno być możliwe problemu zamknięcia maszyn wirtualnych za pośrednictwem interfejsu wiersza polecenia Azure zamiast skrypt STONITH, który steruje urządzenia fizycznego. Możesz użyć `/usr/lib/stonith/plugins/external/ssh` jako podstawa i Włącz STONITH w konfiguracji klaster. Polecenie Azure powinna zostać zainstalowana globalnie i ustawienia profilu publikowania powinny być załadowane dla użytkownika w grupie.

Przykładowy kod dostępne na [GitHub](https://github.com/bureado/aztonith)zasobów. Musisz zmienić konfigurację klaster, dodając poniższe czynności, aby `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Uwaga:** skrypt nie działa wzrost-spadek kontroli. Oryginalny zasobów SSH miał 15 testy ping, ale czasu dla maszyn wirtualnych Azure mogą być więcej zmiennej.

## <a name="limitations"></a>Ograniczenia

Następującym ograniczeniom:

- Skrypt zasobów DRBD linbit, który zarządza DRBD jako zasobu w zastosowaniach rozrusznik `drbdadm down` zamknięcia węzeł, nawet jeśli węzeł tylko będzie wstrzymania. Jest to idealne rozwiązanie, ponieważ podrzędnej będzie nie być synchronizowanie zasobów DRBD podczas wzorcu otrzymuje zapisu. Jeśli wzorzec nie kończy się niepowodzeniem graciously, podrzędnej może być w stanie starszych plików. Istnieją dwa sposoby potencjalne rozwiązywania następująco:
  - Wymuszanie `drbdadm up r0` na wszystkich węzłach za pośrednictwem lokalnych watchdog (nie clusterized), lub,
  - Edytowanie linbit DRBD skryptów, upewniając się, że `down` nie jest wywoływana, w `/usr/lib/ocf/resource.d/linbit/drbd`.
- Usługi równoważenia obciążenia wymaga co najmniej 5 sekund, aby odpowiedzieć na wiadomość, aby aplikacje powinny być klastrów, być bardziej elastyczne limitu czasu; inne architektury pomaga, na przykład kolejkach w aplikacji, middlewares kwerendy itp.
- Dostosowywanie MySQL jest zapewnienie pisania jest wykonywana w tempie sane i pamięci podręcznej jest opróżniany na dysku, jak często, jak to możliwe, aby minimalizować straty pamięci
- Pisanie wydajność będzie zależne w maszyn wirtualnych połączenie w wirtualnej przełączanie jako mechanizmu używane przez DRBD do replikacji na urządzeniu
