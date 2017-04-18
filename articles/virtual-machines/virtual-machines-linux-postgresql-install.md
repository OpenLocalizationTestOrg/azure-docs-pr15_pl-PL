<properties
    pageTitle="Konfigurowanie PostgreSQL na maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak zainstalować i skonfigurować PostgreSQL na komputerze wirtualnych Linux platformy Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Instalowanie i konfigurowanie PostgreSQL Azure

PostgreSQL zaawansowane Otwórz źródło baza danych jest podobne do Oracle i DB2. Zawiera gotowe enterprise funkcje, takie jak pełnego kwasu zgodności, niezawodne przetwarzanie transakcji i kontroli współbieżności wielu wersji. Obsługuje ona również standardy, takich jak języka SQL ANSI i SQL-MED (w tym otoki obcego danych dla programu Oracle, MySQL MongoDB i wielu innych). Jest wysoce extensible dzięki obsłudze języków proceduralnych ponad 12, indeksy GIN i GiST Obsługa przestrzenna danych i wiele funkcji NoSQL JSON lub aplikacji opartych na wartość klucza.

W tym artykule dowiesz się, jak zainstalować i skonfigurować PostgreSQL na Azure maszyn wirtualnych systemem Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>Instalowanie PostgreSQL

> [AZURE.NOTE] Musi być już Azure maszyn wirtualnych systemem Linux, aby można było użyć tego samouczka. Aby utworzyć i skonfigurować maszyny Linux przed kontynuowaniem, zobacz [Samouczek maszyn wirtualnych Linux Azure](virtual-machines-linux-quick-create-cli.md).

W tym przypadku korzysta z portu 1999 jako PostgreSQL port.  

Połącz ze Linux maszyn wirtualnych utworzone za pomocą Kit. Jeśli po raz pierwszy używasz maszyn wirtualnych Linux Azure, zobacz, [jak za pomocą SSH z systemem Linux Azure](virtual-machines-linux-mac-create-ssh-keys.md) Dowiedz się, jak używać Kit do łączenia się maszyny Linux.

1. Uruchom następujące polecenie, aby przełączyć się do katalogu głównego (Administrator):

        # sudo su -

2. Niektóre dystrybucji ma zależności, które należy zainstalować przed zainstalowaniem PostgreSQL. Sprawdzanie dostępności usługi distro na tej liście i uruchomić odpowiednie polecenie:

    - Czerwony funkcję podstawowej Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian Linux podstawowej:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Pobierz PostgreSQL w katalogu głównym, a następnie Rozpakuj plik pakietu:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Powyższe jest na przykład. [Indeks/pub-źródła-](https://ftp.postgresql.org/pub/source/)można znaleźć bardziej szczegółowe adres plik do pobrania.

4. Aby rozpocząć tworzenie, uruchom następujące polecenia:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Jeśli chcesz utworzyć wszystkie zasoby, które mogą być wbudowane, w tym dokumentację (stron HTML i man) i moduły dodatkowe (contrib), uruchom następujące polecenie:

        # gmake install-world

    Powinny otrzymać następujący komunikat z potwierdzeniem:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Konfigurowanie PostgreSQL

1. (Opcjonalnie) Tworzenie łącza symbolicznego, aby skrócić nawiązania PostgreSQL nie zawiera numer wersji:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Utwórz katalog bazy danych:

        # mkdir -p /opt/pgsql_data

3. Tworzenie administratora- i modyfikowanie profilu użytkownika. Następnie przełącz się do tego nowego użytkownika (nazywane *postgres* w naszym przykładzie):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Ze względów bezpieczeństwa PostgreSQL używa administratora — aby zainicjować, rozpocząć lub zamknij bazę danych.


4. Edytowanie pliku *bash_profile* przez wprowadzenie do poleceń poniżej. Te wiersze zostaną dodane na końcu pliku *bash_profile* :

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Wykonanie pliku *bash_profile* :

        $ source .bash_profile

6. Sprawdź poprawność instalacji przy użyciu następującego polecenia:

        $ which psql

    Jeśli instalacja powiedzie się, zobaczysz następującą odpowiedź:

        /opt/pgsql/bin/psql

7. Możesz również sprawdzić wersję PostgreSQL:

        $ psql -V

8. Inicjowanie bazy danych:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Powinien zostać wyświetlony następujący komunikat:

![Obraz](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Konfigurowanie PostgreSQL

<!--    [postgres@ test ~]$ exit -->

Uruchom następujące polecenia:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Modyfikowanie dwóch zmiennych w pliku /etc/init.d/postgresql. Prefiks jest ustawiona na ścieżce instalacji PostgreSQL: **/opt/pgsql**. PGDATA jest ustawiona na ścieżkę miejsca do magazynowania danych PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Obraz](./media/virtual-machines-linux-postgresql-install/no2.png)

Zmienianie plików nawiązać wykonywalny:

    # chmod +x /etc/init.d/postgresql

Rozpocznij PostgreSQL:

    # /etc/init.d/postgresql start

Sprawdź, czy punkt końcowy PostgreSQL jest na:

    # netstat -tunlp|grep 1999

Powinien zostać wyświetlony następujący komunikat:

![Obraz](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Nawiązywanie połączenia z bazą danych Postgres

Przełączanie do użytkownika postgres ponownie:

    # su - postgres

Tworzenie bazy danych Postgres:

    $ createdb events

Połączenia z bazą danych zdarzenia, która została właśnie utworzona:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Tworzenie i usuwanie tabeli Postgres

Teraz, gdy połączono z bazą danych, można utworzyć tabelę z nim.

Na przykład utworzyć nową tabelę Postgres przykład przy użyciu następującego polecenia:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Teraz skonfigurowane cztery kolumny tabeli za pomocą następujących nazw kolumn i ograniczeń:

1. Kolumna "Nazwa" ma został ograniczony za pomocą polecenia VARCHAR mieć mniej niż 20 znaków.
2. Kolumna "żywność" wskazuje spożywczemu, pozwalających każdej osoby. VARCHAR ogranicza ten tekst ma być w obszarze 30 znaków.
3. Kolumna "potwierdzone" rekordów, czy dana osoba jest RSVP'd na przyjęcie składkowe. Dopuszczalne wartości są "Y" i "N".
4. Przedstawia kolumny "date" podczas tworzenia konta w dla zdarzenia. Postgres wymaga zapisać daty w postaci yyyy-mm-domenie odnajdowania.

Powinien zostać wyświetlony poniższy po pomyślnym utworzeniu tabeli:

![Obraz](./media/virtual-machines-linux-postgresql-install/no4.png)

Możesz również sprawdzić strukturę tabeli przy użyciu następującego polecenia:

![Obraz](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Dodawanie danych do tabeli

Najpierw należy wstawić informacje do wiersza:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Powinien zostać wyświetlony ten wynik:

![Obraz](./media/virtual-machines-linux-postgresql-install/no6.png)

Możesz dodać kilka dodatkowych osób do tabeli. Oto kilka opcji lub można tworzyć własne:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Pokaż tabele

Aby pokazać tabelę, użyj następującego polecenia:

    select * from potluck;

Wynik to:

![Obraz](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Usuwanie danych w tabeli

Aby usunąć dane w tabeli, użyj następującego polecenia:

    delete from potluck where name=’John’;

Spowoduje to usunięcie wszystkich informacji w wierszu "Jan". Wynik to:

![Obraz](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Aktualizowanie danych w tabeli

Użyj następującego polecenia, aby zaktualizować dane w tabeli. Dla tego typu Sandy potwierdził, że jest ona uczestnictwa, więc zostanie zmieniona jej RSVP z "N" do "Y":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Więcej informacji o PostgreSQL
Teraz, gdy zakończeniu instalacji PostgreSQL w maszyn wirtualnych Linux Azure, można korzystać ze korzystania z niego w Azure. Aby dowiedzieć się więcej na temat PostgreSQL, odwiedź [witrynę sieci Web PostgreSQL](http://www.postgresql.org/).
