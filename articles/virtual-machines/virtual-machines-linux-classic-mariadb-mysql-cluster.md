<properties
    pageTitle="Uruchomiony klaster MariaDB (MySQL) Azure"
    description="Tworzenie MariaDB + Galera MySQL klaster na maszyn wirtualnych Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>Klaster MariaDB (MySQL) - samouczek Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  Klaster MariaDB przedsiębiorstwa jest obecnie dostępna w Azure Marketplace.  Nowa oferta zostanie automatycznie wdrożyć klastrze MariaDB Galera w CHMURZE. Należy używać nowego produktu z https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

Firma Microsoft tworzenia wielu wzorców klastrze [Galera](http://galeracluster.com/products/) [MariaDBs](https://mariadb.org/en/about/), zastępuje niezawodne, skalowalna i wiarygodnych dzięki wsuwanej konstrukcji MySQL do pracy w środowisku maszyn wirtualnych Azure na wysokiej dostępności.

## <a name="architecture-overview"></a>Przegląd architektury

W tym temacie wykonuje następujące czynności:

1. Utworzyć klaster węzeł 3
2. Oddzielanie dyski danych z dysku systemu operacyjnego
3. Tworzenie dysków danych RAID-0-rozłożone ustawienie, aby zwiększyć operacji i/o na SEKUNDĘ
4. Obciążenie dla węzłów 3 przy użyciu usługi równoważenia obciążenia Azure
5. Aby zminimalizować powtarzających się zadań, utworzyć obraz maszyn wirtualnych zawierające MariaDB + Galera i za jej pomocą tworzyć innych klaster maszyny wirtualne.

![Architektura](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  W tym temacie korzysta narzędzia [Polecenie Azure](../xplat-cli-install.md) , dlatego należy się upewnić, pobierz je i ich łączenia się z subskrypcji usługi Azure zgodnie z instrukcjami. Jeśli potrzebujesz odwołanie do poleceń dostępnych w polecenie Azure, zapoznaj się z to łącze, aby [polecenie Azure polecenie odwołania](../virtual-machines-command-line-tools.md). Zostaną również do [utworzenia klucza SSH uwierzytelniania] i zanotuj **lokalizację pliku .pem**.


## <a name="creating-the-template"></a>Tworzenie szablonu

### <a name="infrastructure"></a>Infrastruktura

1. Tworzenie grupy koligacji do przechowywania razem z zasobów

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Tworzenie wirtualnych sieci

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Utwórz konto miejsca do magazynowania do obsługi wszystkich naszych dysków. Notatki, że użytkownik nie powinny wprowadzenie do ponad 40 intensywnie używany dysków na tym samym kontem miejsca do magazynowania unikać naciśnięcie 20 000 limitu konta przestrzeni dyskowej operacji i/o na SEKUNDĘ. W tym przypadku jesteśmy daleko z tego numeru, będą przechowywane wszystkie elementy na to samo konto w celu uproszczenia

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Znajdź nazwę obrazu maszyn wirtualnych 7 CentOS

        azure vm image list | findstr CentOS
Spowoduje to wyjściowy podobną do `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Użyj nazwy w poniższym kroku.

4. Tworzenie szablonu maszyn wirtualnych, zamieniając **/path/to/key.pem** ścieżkę lokalizacji przechowywania klucza SSH wygenerowane .pem

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. Dołączanie dyski danych 4 x 500GB do maszyn wirtualnych do użycia w konfiguracji RAID

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH do szablonu maszyn wirtualnych, którego została utworzona w **mariadbhatemplate.cloudapp.net:22** i nawiązać połączenie za pomocą klucza prywatnego.

### <a name="software"></a>Oprogramowanie

1. Uzyskać katalogu głównego

        sudo su

2. Zainstaluj RAID pomocy technicznej:

     - Instalowanie mdadm

                yum install mdadm

     - Tworzenie konfiguracji RAID0-pasek z systemem plików EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Tworzenie katalogu punktu instalacji

                mkdir /mnt/data

     - Pobieranie UUID nowo utworzonego urządzenia RAID

                blkid | grep /dev/md0

     - Edytowanie /etc/fstab

                vi /etc/fstab

     - Dodawanie urządzenia w celu włączyć automatyczne mouting przy ponownym uruchomieniu, zamieniając UUID wartość uzyskanego z poleceniem **blkid** przed

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Instalowanie nowej partition

                mount /mnt/data

3. Zainstaluj MariaDB:

     - Utwórz plik MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Wypełnij ją z poniżej zawartości

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Usuwanie istniejącej przyrostek i mariadb bibliotekami, aby uniknąć konfliktów

            yum remove postfix mariadb-libs-*

     - Instalowanie MariaDB przy użyciu Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Przenoszenie katalogu danych MySQL na urządzeniu blok RAID

     - Skopiuj bieżącego katalogu MySQL do nowej lokalizacji i usuń starą katalogu

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Odpowiednio ustawić uprawnienia dotyczące nowego katalogu

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Tworzenie łącza symbolicznego wskazująca katalogu stare w nowe miejsce na partycją RAID

            ln -s /mnt/data/mysql /var/lib/mysql

5. Ponieważ [SELinux zakłóca operacje klaster](http://galeracluster.com/documentation-webpages/configuration.html#selinux), należy je wyłączyć dla bieżącej sesji (aż pojawi się zgodnej wersji). Edytowanie `/etc/selinux/config` je wyłączyć dla kolejnych ponownego uruchamiania:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Sprawdź poprawność zostanie uruchomiona MySQL

    - Rozpoczynanie MySQL

            service mysql start

    - Secure instalacji MySQL, ustaw hasła root, Usuń użytkowników anonimowych, wyłączania logowania zdalnym i usuwanie testowej bazy danych

            mysql_secure_installation

    - Tworzenie użytkownika w bazie danych dla operacji klaster i opcjonalnie aplikacji

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - Zatrzymywanie MySQL

            service mysql stop

7. Tworzenie konfiguracji symbolu zastępczego

    - Edytowanie Konfiguracja MySQL, aby utworzyć symbol zastępczy ustawienia klaster. Nie zamieniaj **`<Vairables>`** lub Usuń komentarze teraz. Który się stanie po tworzymy maszyn wirtualnych przy użyciu tego szablonu.

            vi /etc/my.cnf.d/server.cnf

    - Edytowanie sekcji **[galera]** i czyszczenie go

    - Edytowanie sekcji **[mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Otwórz wymagane porty w zaporze (za pomocą FirewallD na CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - TSI GALERA:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Załaduj ponownie zapory:`firewall-cmd --reload`

9.  Optymalizacja systemu wydajności. Zapoznaj się z tym artykułem [strategii określania wydajności](virtual-machines-linux-classic-optimize-mysql.md) , aby uzyskać więcej informacji

    - Ponownie edytować plik konfiguracji MySQL

            vi /etc/my.cnf.d/server.cnf

    - Edytowanie sekcji **[mariadb]** i dołączanie poniżej

    > [AZURE.NOTE] Zalecane jest **innodb\_buforu\_pool_size** 70% pamięci do maszyn wirtualnych. Została ustawiona na poniżej 2.45GB dla maszyn wirtualnych Azure średni 3,5 GB pamięci RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Zatrzymywanie MySQL, wyłączyć usługę MySQL uruchamiania podczas uruchamiania, aby uniknąć konieczności modyfikowania w górę klaster podczas dodawania nowego węzła i deprovision komputera.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Przechwytywanie maszyn wirtualnych za pośrednictwem portalu. (Obecnie [problem 1268 # w Azure interfejsu wiersza polecenia] narzędzia opisano fakt, że obrazów tworzonych przez narzędzia polecenie Azure nie przechwytywać dysków załączonych danych).

    - Zamknięcie komputera za pośrednictwem portalu
    - Kliknij na przechwytywanie i określić nazwę obrazu jako **mariadb galera obrazu** i podaj opis a "Mają Uruchom waagent".
    ![Przechwytywanie maszyny wirtualnej](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![Przechwytywanie maszyny wirtualnej](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Tworzenie klaster

Tworzenie 3 maszyny wirtualne wylogowywanie się z szablonu, możesz po prostu utworzone skonfigurować i uruchomić klaster.

1. Tworzenie pierwszego CentOS 7 maszyn wirtualnych z obrazu **mariadb galera obrazu** został utworzony, zapewniające nazwy wirtualnej sieci **mariadbvnet** i podsieci **mariadb**komputera rozmiar **Średni**przekazywania w usłudze w chmurze nazwy należy **mariadbha** (lub dowolną inną przy użyciu mariadbha.cloudapp.net), ustawianie nazwy tego komputera **mariadb1** i nazwa użytkownika należy **azureuser**i włączanie dostępu SSH i przekazywanie SSH certyfikatów plik .pem i zamieniania **/path/to/key.pem** ścieżką miejsce, w którym możesz przechowywane klucz SSH wygenerowane .pem.

    > [AZURE.NOTE] Poniżej polecenia są podzielić na wiele wierszy dla jasności, ale należy wprowadzić jako jeden wiersz.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. Tworzenie za pomocą _połączenia_ 2 więcej maszyn wirtualnych im aktualnie utworzonych **mariadbha** usługi w chmurze, zmieniając **nazwy maszyn wirtualnych** , a także **SSH port** do portu unikatowe nie powoduje konflikt z innymi maszyny wirtualne w tym samym usługi w chmurze.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
i MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Należy uzyskać wewnętrzny adres IP każdego z 3 maszyny wirtualne do następnego kroku:

    ![Uzyskiwanie adresu IP](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH do 3 maszyny wirtualne i i edytowania pliku konfiguracji na każdym

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** i **`wsrep_cluster_address`** , usuwając **#** na początku i sprawdzanie poprawności są to w rzeczywistości odpowiednie.
    Ponadto Zamień **`<ServerIP>`** w **`wsrep_node_address`** i **`<NodeName>`** w **`wsrep_node_name`** z maszyn wirtualnych IP adresu i nazwy odpowiednio i Usuń komentarze linie, a także.

5. Klaster na MariaDB1 i umożliwić jego uruchamianie podczas uruchamiania

        sudo service mysql bootstrap
        chkconfig mysql on

6. Uruchom MySQL na MariaDB2 i MariaDB3 i umożliwić jego uruchamianie podczas uruchamiania

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Klaster równoważenia obciążenia
Po utworzeniu grupowany maszyny wirtualne zostały dodane do ich dostępności Ustawianie nazywane **clusteravset** upewnij się, że są one wprowadzane na różnych błędów i zaktualizować domeny i że Azure przeze mnie oznacza konserwacji na wszystkich komputerach jednocześnie. Ta konfiguracja spełnia wymagania obsługiwane przez ten Azure usługi umową dotyczącą poziomu (SLA).

Teraz umożliwia równoważenia obciążenia Azure saldo żądaniami nasz węzły 3.

Uruchamianie poniżej poleceń na tym komputerze za pomocą interfejsu wiersza polecenia Azure.
Struktura parametrów polecenia jest:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Na koniec ponieważ polecenie ustawia interwał sondy równoważenia obciążenia 15 sekundach (które mogą być nieco zbyt długo), zmienisz go w portalu w obszarze **punkty końcowe** dla każdego z maszyny wirtualne

![Edytowanie punktu końcowego](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

następnie wybierz polecenie Ustaw Reconfigure The Load-Balanced i przejść

![Ustawianie strategii ładowanie ponownej konfiguracji](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Zmień interwał sondy do 5 sekund, a następnie zapisz

![Zmienianie interwału sondy](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Sprawdzanie poprawności klaster

Należy wytężonej pracy. Klaster powinna być teraz dostępna na `mariadbha.cloudapp.net:3306` której zostaną trafienie równoważenia obciążenia i kierowanie żądaniami 3 maszyny wirtualne sprawnie i wydajnie.

Ulubione klienta MySQL umożliwia łączenie lub po prostu nawiązywanie połączenia z jednej z pośrednictwem SMS, aby sprawdzić, czy ten klaster działa.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Utwórz nową bazę danych i wypełniania danych

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Spowoduje w poniższej tabeli

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

W tym artykule są tworzone 3 węzeł MariaDB + Galera klaster wysokiej dostępności na Azure maszyn wirtualnych z systemem CentOS 7. Maszyny wirtualne są obciążenia konto usługi równoważenia obciążenia Azure.

Być może zechcesz zapoznaj się [w inny sposób klaster MySQL w systemie Linux](virtual-machines-linux-classic-mysql-cluster.md) i sposobów [Optymalizowanie i testowanie wydajności MySQL na maszyny wirtualne Linux Azure](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[utworzenie klucza SSH uwierzytelniania]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[problem #1268 polecenie Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268
