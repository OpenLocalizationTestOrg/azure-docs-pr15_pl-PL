<properties
   pageTitle="Praca z gałęzi na HDInsight (Hadoop) przy użyciu Beeline | Microsoft Azure"
   description="Dowiedz się, jak za pomocą SSH połączyć się z klastrem Hadoop w HDInsight, a następnie interakcyjnie Przekaż gałęzi kwerendy przy użyciu Beeline. Narzędzie do pracy z HiveServer2 nad JDBC jest beeline."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Używanie gałęzi z Hadoop w HDInsight z Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym artykule dowiesz się, jak używać Secure Shell (SSH) do nawiązania połączenia z klastrem systemem Linux HDInsight, a następnie interakcyjnie Przekaż gałęzi zapytań za pomocą narzędzia wiersza polecenia [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) .

> [AZURE.NOTE] Beeline łączy do gałęzi za pośrednictwem JDBC. Aby uzyskać więcej informacji na temat korzystania z programu Hive JDBC zobacz [Łączenie się gałęzi na HDInsight Azure za pomocą sterownika gałęzi JDBC](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Systemem Linux Hadoop w klastrze HDInsight.

* Klient SSH. Linux, Unix lub systemu Mac OS powinna pochodzić z klientem SSH. Użytkownicy systemu Windows musi pobrać klienta, takich jak [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Łączenie się z SSH

Nawiązać połączenie w pełni kwalifikowaną nazwę domeny (FQDN) programu klaster HDInsight przy użyciu polecenia SSH. Nazwy FQDN będzie nazwa nadana klaster, następnie **. azurehdinsight.net**. Na przykład poniższy chcesz ustanowić połączenie z klastrem o nazwie **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli opisane klucza certyfikatu uwierzytelniania SSH** po utworzeniu klaster HDInsight, może być konieczne Określ lokalizację klucz prywatny w systemie klienta:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Jeśli opisane hasło do uwierzytelniania SSH** po utworzeniu klaster HDInsight, będzie konieczne hasło po wyświetleniu monitu.

Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kit (klientów opartych na systemie Windows)

System Windows nie udostępnia wbudowane klienta SSH. Firma Microsoft zaleca używanie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji na temat korzystania z Kit Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Za pomocą polecenia Beeline

1. Po połączeniu, uruchom Beeline należy wykonać następujące kroki:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    To będzie Uruchom klienta Beeline i Połącz do adresu url JDBC. W tym miejscu `localhost` służy HiveServer2 działa na obu głowy węzły w klastrze i firma Microsoft używasz Beeline bezpośrednio na headnode podstawowego.
    
    Po wykonaniu polecenia zostaną dostarczone w `jdbc:hive2://localhost:10001/>` wiersza.

3. Polecenia beeline zwykle zaczynają się od `!` znak, na przykład `!help` Wyświetla Pomoc. Jednak `!` często mogą zostać pominięte. Na przykład `help` będą również działać.

    Jeśli możesz wyświetlić Pomoc, można zauważyć `!sql`, która umożliwia wykonywanie instrukcji HiveQL. Jednak HiveQL jest tak często używanych że można pominąć poprzednim `!sql`. Poniższe instrukcje dwoma mają odmienne wyniki; Wyświetlanie obecnie dostępne za pośrednictwem gałęzi tabel:
    
        !sql show tables;
        show tables;
    
    W klastrze nowej liście powinny pozostać tylko jedną tabelę: __hivesampletable__.

4. Wyświetlanie schemat hivesampletable należy wykonać następujące kroki:

        describe hivesampletable;
        
    Zostaną zwrócone następujące informacje:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Spowoduje to wyświetlenie kolumny w tabeli. Gdy firma Microsoft może wykonać kilka kwerend dla tych danych, zamiast tego tworzenie zupełnie nową tabelę do pokazano, jak załadować dane do gałęzi i stosować schemat.
    
5. Wprowadź następujące instrukcje, aby utworzyć nową tabelę o nazwie **log4jLogs** za pomocą dostarczonych z klastrem HDInsight przykładowych danych:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Poniższe instrukcje należy wykonać następujące czynności:

    * **DROP TABLE** — powoduje usunięcie tabeli i w pliku danych, w przypadku, gdy tabela już istnieje.
    * **Tworzenie tabeli zewnętrznej** — powoduje utworzenie nowej tabeli "zewnętrzne" w gałęzi. Zewnętrzne tabele zawierają tylko definicji tabeli w gałęzi. Dane pozostaje w pierwotnej lokalizacji.
    * **FORMAT wiersza** — informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.
    * **PRZECHOWYWANY w lokalizacji TEXTFILE** — informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i że są przechowywane jako tekst.
    * **Wybierz pozycję** - zaznacza liczbę wierszy miejsce, w którym kolumny **t4** zawiera wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ istnieją trzy wiersze, które zawierają wartość.
    * **INPUT__FILE__NAME takie jak "%.log"** - zawiera informację, który należy tylko zwróconych danych z plików kończącymi się gałęzi. dziennika. Zazwyczaj tylko trzeba danych za pomocą tego samego schematu w tym samym folderze po wykonywanie zapytań za pomocą gałęzi, jednak ten przykładowy plik dziennika są przechowywane dane w innych formatach.

    > [AZURE.NOTE] Jeśli oczekujesz danych źródłowych zostaną zaktualizowane przez źródło zewnętrzne, taką jak proces przekazywania danych lub przez inną operację MapReduce, ale zawsze ma gałęzi kwerendy do korzystania z najnowszych danych, należy użyć tabele zewnętrzne.
    >
    > Usuwanie tabeli zewnętrznej czy **nie** Usuń dane, tylko definicję tabeli.
    
    Wynik tego polecenia powinien być podobny do następującego:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Aby zakończyć Beeline, za pomocą `!quit`.

##<a id="file"></a>Uruchom plik HiveQL

Aby uruchomić plik, który zawiera instrukcje HiveQL można również beeline. Wykonaj następujące czynności, aby utworzyć plik, a następnie uruchom go przy użyciu Beeline.

1. Umożliwia utworzenie nowego pliku o nazwie __query.hql__następujące polecenie:

        nano query.hql
        
2. Po otwarciu edytora służą jako zawartość pliku. Ta kwerenda utworzy nową tabelę "wewnętrzną" o nazwie **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Tworzenie tabeli IF NOT EXISTS** - tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.
    * **PRZECHOWYWANE jako ORC** - są przechowywane dane w formacie zoptymalizowane wiersza kolumnowy (ORC). To jest wysoce zoptymalizowane i wydajną format do przechowywania danych gałęzi.
    * Zastąp **Wstawianie... Wybierz** - wybiera wiersze z tabeli **log4jLogs** , które zawierają **[Błąd]**, a następnie wstawia dane do tabeli **errorLogs** .
    
    > [AZURE.NOTE] W odróżnieniu od tabele zewnętrzne upuszczanie wewnętrznej tabeli spowoduje usunięcie danych źródłowych.
    
3. Aby zapisać plik, użyj __klawiszy Ctrl__+ ___X__, a następnie wprowadź __Y__, a na końcu __Enter__.

4. Uruchom plik przy użyciu Beeline należy wykonać następujące kroki. Zastąp __HOSTNAME__ uzyskany wcześniej węzła głównego i __hasło__ przy użyciu hasła do konta administratora:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] `-i` Parametru zaczyna się Beeline uruchamia stwierdzeń umieszczonych w pliku query.hql i pozostanie w Beeline w `jdbc:hive2://localhost:10001/>` wiersza. Możesz również uruchomić plik przy użyciu `-f` parametr, który powoduje powrót do imprezie po przetworzeniu pliku.

5. Aby sprawdzić, czy tabela **errorLogs** została utworzona, należy użyć następującej instrukcji zwraca wszystkie wiersze z **errorLogs**:

        SELECT * from errorLogs;

    Trzech wierszy danych powinna zostać zwrócona, zawierające wszystkie **[ERROR]** w t4 kolumny:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Więcej informacji na temat łączności Beeline

Kroki opisane w tym dokumentu użyj `localhost` nawiązywania połączenia z HiveServer2 uruchomionych headnode klaster. O ile możesz również użyć nazwa hosta lub w pełni kwalifikowaną nazwę domeny z headnode te wymagają dodatkowych czynności, aby proces (kroki, aby znaleźć nazwa hosta lub nazwy FQDN). Za pomocą `localhost` wystarcza podczas korzystania z headnode Beeline.

Jeśli masz węzeł krawędzi w klastrze, ze zainstalowany Beeline, będzie konieczne nawiązywanie połączenia za pomocą nazwa hosta lub nazwy FQDN headnode.

Jeśli masz zainstalowaną na komputerze klienckim spoza klaster Beeline, możesz nawiązać połączenie za pomocą następującego polecenia. Zamień __NAZWAKLASTRA__ nazwę klaster HDInsight. Zamień __hasło__ hasło do konta administratora (HTTP logowania).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Zauważ, że parametry URI jest inne niż podczas uruchamiania bezpośrednio na headnode lub węzeł krawędzi w klastrze. Jest to ponieważ z klastrem łączą się z Internetem używa publicznej bramy, która kieruje ruch na porcie 443. Także kilka innych usług są dostępne za pośrednictwem publicznego bramy na porcie 443, więc identyfikator URI różni się od podczas nawiązywania połączenia bezpośrednio. Podczas nawiązywania połączenia z Internetem, musisz również uwierzytelnienia sesji, podając hasło.

##<a id="summary"></a><a id="nextsteps"></a>Następne kroki

Jak widać, polecenie Beeline zapewnia łatwy sposób interakcyjne uruchamianie kwerend gałęzi w klastrze HDInsight.

Ogólne informacje na temat gałąź w HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje na inne sposoby pracy z Hadoop na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Jeśli używasz Tez z gałęzi, zobacz następujące dokumenty dla informacji debugowania:

* [Korzystanie z interfejsu użytkownika Tez na HDInsight systemu Windows](hdinsight-debug-tez-ui.md)

* [Korzystanie z widoku Ambari Tez na podstawie Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

