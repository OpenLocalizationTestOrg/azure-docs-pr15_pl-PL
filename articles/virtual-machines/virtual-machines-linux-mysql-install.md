<properties
    pageTitle="Konfigurowanie MySQL na maszyny Linux | Microsoft Azure "
    description="Dowiedz się, jak zainstalować stos MySQL na komputerze wirtualnych Linux (Ubuntu i RedHat rodzina OS) platformy Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Jak zainstalować MySQL Azure


W tym artykule dowiesz się, jak zainstalować i skonfigurować MySQL na Azure maszyn wirtualnych systemem Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>Instalowanie na komputerze wirtualnych MySQL

> [AZURE.NOTE] Musi być już maszyny wirtualnej Microsoft Azure systemem Linux, aby można było użyć tego samouczka. Zobacz [Samouczek maszyn wirtualnych Linux Azure](virtual-machines-linux-quick-create-cli.md) utworzyć i skonfigurować maszyny Linux z `mysqlnode` jako nazwę maszyn wirtualnych i `azureuser` jako użytkownika przed kontynuowaniem.

W tym przypadku korzysta z portu 3306 jako MySQL port.  

Połącz ze Linux maszyn wirtualnych utworzone za pomocą Kit. Jeśli używasz maszyn wirtualnych Linux Azure po raz pierwszy, Dowiedz się, jak używać Kit nawiązywanie połączenia z maszyny Linux [tutaj](virtual-machines-linux-mac-create-ssh-keys.md).

Użyjemy repozytorium pakietu do zainstalowania MySQL5.6 jako przykład w tym artykule. W rzeczywistości MySQL5.6 ma więcej poprawy wydajności niż MySQL5.5.  Więcej informacji [w tym miejscu](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Jak zainstalować MySQL5.6 na Ubuntu
W tym miejscu użyjemy maszyn wirtualnych Linux z Ubuntu z platformy Azure.

- Krok 1: Instalowanie MySQL serwera 5.6 Przełącz do `root` użytkownika:

            #[azureuser@mysqlnode:~]sudo su -

    Zainstaluj serwer mysql 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Podczas instalacji pojawi się okno dialogowe okna poping do z pytaniem, czy można ustawić hasła root MySQL poniżej, na które muszą ustawić hasło tutaj.

    ![Obraz](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Wprowadź hasło ponownie, aby potwierdzić.

    ![Obraz](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Krok 2: Serwer MySQL logowania

    Po zakończeniu instalacji serwer MySQL MySQL zostanie uruchomiona automatycznie. Możesz logować się serwer MySQL z `root` użytkownika.
    Użyj poniżej polecenia hasło logowania i danych wejściowych.

             #[root@mysqlnode ~]# mysql -uroot -p

- Krok 3: Zarządzanie uruchomionej usługi MySQL

    () sprawdzić stan usługi MySQL

             #[root@mysqlnode ~]# service mysql status

    (b) uruchom usługę MySQL

             #[root@mysqlnode ~]# service mysql start

    (c) Zatrzymaj usługę MySQL

             #[root@mysqlnode ~]# service mysql stop

    (d) ponownie uruchomić usługę MySQL

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Jak zainstalować MySQL na czerwony funkcję OS rodziny takich jak CentOS, Oracle Linux
W tym miejscu użyjemy maszyn wirtualnych Linux z CentOS i Linux oraz Oracle.

- Krok 1: Dodawanie repozytorium MySQL Yum Przełącz do `root` użytkownika:

            #[azureuser@mysqlnode:~]sudo su -

    Pobierz i zainstaluj pakiet wersji MySQL:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Krok 2: Edytuj poniżej plik, aby włączyć Pobieranie pakietu MySQL5.6 repozytorium MySQL.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Zaktualizuj wartości tego pliku poniżej:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Krok 3: Instalowanie MySQL z repozytorium MySQL MySQL instalacji:

           #[root@mysqlnode ~]#yum install mysql-community-server

    Pakiet obrotów na MINUTĘ MySQL i wszystkie powiązane pakiety zostanie zainstalowana.

- Krok 4: Zarządzanie uruchomionej usługi MySQL

    () sprawdź stan usługi Serwer MySQL:

           #[root@mysqlnode ~]#service mysqld status

    (b) sprawdź, czy serwer domyślny port MySQL działa:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) start serwer MySQL:

           #[root@mysqlnode ~]#service mysqld start

    (d) zatrzymać serwer MySQL:

           #[root@mysqlnode ~]#service mysqld stop

    (e) zestawu MySQL do uruchamiania podczas uruchamiania systemu zapasowej:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Jak zainstalować MySQL na SUSE Linux
W tym miejscu użyjemy maszyn wirtualnych Linux z OpenSUSE.

- Krok 1: Pobieranie i instalowanie serwer MySQL

    Przełączanie do `root` użytkownika za pomocą poniżej polecenia:  

           #sudo su -

    Pobierz i zainstaluj pakiet MySQL:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Krok 2: Zarządzanie uruchomionej usługi MySQL

    () sprawdź stan serwer MySQL:

           #[root@mysqlnode ~]# rcmysql status

    (b) wyboru czy domyślny port serwer MySQL:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) start serwer MySQL:

           #[root@mysqlnode ~]# rcmysql start

    (d) zatrzymać serwer MySQL:

           #[root@mysqlnode ~]# rcmysql stop

    (e) zestawu MySQL do uruchamiania podczas uruchamiania systemu zapasowej:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Następny krok
Znajdź więcej użycia i informacje dotyczące MySQL [tutaj](https://www.mysql.com/).
