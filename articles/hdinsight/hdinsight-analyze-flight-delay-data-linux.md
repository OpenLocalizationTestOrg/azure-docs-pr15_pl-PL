<properties 
    pageTitle="Analizowanie danych opóźnienia lotów z gałęzi na podstawie Linux HDInsight | Microsoft Azure" 
    description="Dowiedz się, jak używać gałęzi do analizowania danych lotu na podstawie Linux HDInsight, a następnie wyeksportuj dane do bazy danych SQL przy użyciu Sqoop." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analizowanie danych opóźnienia lotów przy użyciu gałąź w HDInsight

Dowiedz się, jak do analizowania danych opóźnienia lotów za pomocą gałęzi na podstawie Linux HDInsight, a następnie wyeksportuj dane do bazy danych SQL Azure przy użyciu Sqoop.

> [AZURE.NOTE] Chociaż poszczególne części tego dokumentu mogą być używane z klastrów HDInsight systemu Windows (Python i gałęzi, na przykład) są specyficzne dla klastrów systemem Linux kilku etapach. Instrukcje, które będą działać z klastrem bazującym na systemie Windows zobacz [Analiza samolotu opóźnienie danych przy użyciu gałąź w HDInsight](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Klaster HDInsight__. Instrukcje dotyczące tworzenia nowych klaster systemem Linux HDInsight, zobacz [Wprowadzenie do korzystania z Hadoop z gałąź w HDInsight w systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Baza danych SQL azure__. Bazy danych programu Azure SQL użyje przechowywania danych miejsca docelowego. Jeśli nie masz już z bazą danych SQL, zobacz [bazy danych SQL samouczek: tworzenie bazy danych SQL w minutach, po upływie](../sql-database/sql-database-get-started.md).

- __Polecenie azure__. Jeśli nie zainstalowano polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md) dla większej liczby kroków.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Pobieranie danych lotu

1. Przejdź do [badań i Administracja innowacyjnego technologii, Biuro statystyki transportu][rita-website].
2. Na stronie wybierz następujące wartości:

  	| Nazwa | Wartość |
  	| ---- | ---- |
  	| Filtr rok | 2013 |
  	| Filtrowanie okresem. | Styczeń |
  	| Pola | Rok, flightdate (datalotu), UniqueCarrier, przewoźnik, FlightNum, OriginAirportID, pochodzenia, OriginCityName, OriginState, DestAirportID, docelowy, DestCityName, DestState, DepDelayMinutes, ArrDelay, największe, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Wyczyść wszystkie pozostałe pola |

3. Kliknij przycisk **Pobierz**. 

##<a name="upload-the-data"></a>Przekazywanie danych

1. Aby przekazać plik zip do głowy węzła HDInsight, użyj następującego polecenia:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Zamień nazwę pliku zip __nazwy pliku__ . Zamień __nazwę użytkownika__ logowania SSH dla klastrów HDInsight. Zamień NAZWAKLASTRA nazwę klaster HDInsight.
    
    > [AZURE.NOTE] Jeżeli używasz hasła uwierzytelniania nazwy logowania, SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz publiczny, może być konieczne używanie `-i` parametru i określ ścieżkę do odpowiadającego mu klucz prywatny. Na przykład `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Po zakończeniu przekazywania nawiązywanie połączenia z klastrem przy użyciu SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Aby uzyskać więcej informacji na temat korzystania z SSH z systemem Linux HDInsight zobacz następujące artykuły:
    
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Po połączeniu Rozpakuj plik zip należy wykonać następujące kroki:

        unzip FILENAME.zip
    
    Spowoduje to Wyodrębnij plik CSV, który ma około 60MB rozmiar.
    
4. Utwórz nowy katalog na WASB (rozłożone przechowywanych danych przez HDInsight) należy wykonać następujące kroki, a następnie skopiuj plik:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-umieszczenie FILENAME.csv-samouczki/flightdelays/danych /
    
##<a name="create-and-run-the-hiveql"></a>Tworzenie i uruchamianie HiveQL

Wykonaj następujące czynności, aby zaimportować dane z pliku CSV do gałęzi tabeli o nazwie __opóźnienia__.

1. Tworzenie i edytowanie nowego pliku o nazwie __flightdelays.hql__należy wykonać następujące kroki:

        nano flightdelays.hql
        
    Zawartość tego pliku, należy użyć następującej składni:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Zapisz plik za pomocą __klawiszy Ctrl + X__, a następnie __Y__ .

3. Uruchom gałęzi i uruchom plik __flightdelays.hql__ należy wykonać następujące kroki:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] W tym przykładzie `localhost` jest używana, ponieważ są połączeni z węzła głównego klastrze HDInsight, czyli miejsce, w którym jest uruchomiony HiveServer2.

4. Użyj następującego polecenia, aby otworzyć sesję interakcyjnych Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Po otrzymaniu `jdbc:hive2://localhost:10001/>` należy korzystać z następujących do pobierania danych z danych opóźnienia lotów zaimportowanych.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Spowoduje to pobrać listę miast, w których wystąpił opóźnienia pogody, wraz z czasem średniej opóźnień i zapisz go do `/tutorials/flightdelays/output`. Później Sqoop odczytywanie danych z tej lokalizacji i go wyeksportować do bazy danych SQL Azure.

6. Aby zakończyć Beeline, wprowadź `!quit` w wierszu.

## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

Jeśli masz już z bazą danych SQL, należy najpierw uzyskać nazwę serwera. Można znaleźć to w [Azure Portal](https://portal.azure.com) , wybierając __Bazy danych SQL__, a następnie filtrowanie nazwy bazy danych, którego chcesz użyć. Nazwa serwera jest wyświetlane w kolumnie __serwera__ .

Jeśli nie masz już z bazą danych SQL, korzystając z informacji w [bazy danych SQL samouczek: tworzenie bazy danych SQL w minutach, po upływie](../sql-database/sql-database-get-started.md) go utworzyć. Należy zapisać nazwę serwera, używane do bazy danych.

##<a name="create-a-sql-database-table"></a>Tworzenie tabeli bazy danych SQL

> [AZURE.NOTE] Istnieją różne sposoby nawiązywania połączenia z bazą danych SQL, aby utworzyć tabelę. W poniższej procedurze użyto [FreeTDS](http://www.freetds.org/) z klaster HDInsight.

1. Nawiązywanie połączenia z systemem Linux HDInsight klastrem za pomocą SSH, a następnie uruchom następujące kroki z sesji SSH.

3. Aby zainstalować FreeTDS, użyj następującego polecenia:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Po zainstalowaniu FreeTDS Użyj następującego polecenia, aby połączyć się z serwerem bazy danych SQL. Zamień __nazwa_serwera__ nazwę serwera bazy danych SQL. Zamień __adminLogin__ i __adminPassword__ logowania bazy danych SQL. Zamień __databaseName__ nazwy bazy danych.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Zostanie wyświetlony wynik podobny do następującego:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. W `1>` monit, wprowadź następujące wiersze:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Gdy `GO` instrukcja jest wprowadzona, będzie obliczane poprzedniego instrukcji. Spowoduje to utworzenie nowej tabeli o nazwie __opóźnienia__z indeksem grupowany (wymagane przez bazy danych SQL).

    Sprawdź, czy zostały utworzone tabeli należy wykonać następujące kroki:

        SELECT * FROM information_schema.tables
        GO

    Powinien zostać wyświetlony wynik podobny do następującego:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Wprowadź `exit` w `1>` monitu wyjść z narzędzia tsql.
    
##<a name="export-data-with-sqoop"></a>Eksportowanie danych z Sqoop

2. Użyj następującego polecenia w celu zweryfikowania, że Sqoop będą widoczne dla bazy danych SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To powinna zwrócić listę baz danych, łącznie z bazy danych utworzonej tabeli opóźnienia w.

3. Aby wyeksportować dane z hivesampletable do tabeli mobiledata, użyj następującego polecenia:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    To powoduje, że Sqoop nawiązywanie bazy danych SQL do bazy danych zawierającej tabelę opóźnienia i eksportowanie danych z wasbs: / / / samouczki i flightdelays/wyjścia (możemy przechowywania zestawu wyników kwerendy gałęzi w starszej wersji) do tabeli opóźnienia.

4. Po zakończeniu polecenia połączenia z bazą danych przy użyciu TSQL należy wykonać następujące kroki:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Po nawiązaniu połączenia należy zastosować następujące instrukcje, aby zweryfikować, że został wyeksportowane dane w tabeli mobiledata:
    
        SELECT * FROM delays
        GO

    Powinna być widoczna lista danych w tabeli. Typ `exit` aby wyjść z narzędzia tsql.

##<a id="nextsteps"></a>Następne kroki

Teraz wiesz, jak przekazać plik z magazynem obiektów Blob platformy Azure, jak wypełnić tabelę programu Hive przy użyciu danych z magazynem obiektów Blob platformy Azure, uruchamianie kwerendy gałęzi i jak za pomocą Sqoop eksportować dane z HDFS do bazy danych programu Azure SQL. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Wprowadzenie do usługi HDInsight][hdinsight-get-started]
* [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
* [Używanie Oozie z usługi HDInsight][hdinsight-use-oozie]
* [Używanie Sqoop z usługi HDInsight][hdinsight-use-sqoop]
* [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
* [Można opracowywać programy Java MapReduce dla HDInsight][hdinsight-develop-mapreduce]
* [Można opracowywać Hadoop Python streaming programów dla HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
