<properties
    pageTitle="Proces zespołu danych naukowych w działaniu: używanie Hadoop klastrów | Microsoft Azure"
    description="Przy użyciu procesu nauki danych zespołu do scenariusza końcu do końca zastosowanie klaster HDInsight Hadoop do tworzenia i wdrażania modelu za pomocą publicznej zestawu danych."
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Proces zespołu danych naukowych w działaniu: używania klastrów HDInsight Hadoop

W tym przykładzie używamy [Zespołu danych nauki proces (TDSP)](data-science-process-overview.md) w scenariuszu zakończenia do końca przy użyciu [klaster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) do przechowywania, eksplorowanie i funkcji odtwarzania danych z zestawu danych publicznie [Warszawa taksówki podróży](http://www.andresmh.com/nyctaxitrips/) i przykładowe dane w dół. Modele danych utworzono z Azure maszynowego nauki obsługi binarne multiclass klasyfikacji i regresji przewidywanych zadania i.

Aby uzyskać instrukcje przedstawiająca sposób obsługi większych zestawu danych (1 terabajtów) dla podobny scenariusz używania klastrów HDInsight Hadoop przetwarzania danych, zobacz [Proces zespołu do nauki danych — za pomocą Azure HDInsight Hadoop klastrów na 1 TB zestawu danych](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Użytkownik może również Notes IPython służy do wykonywania zadań prezentowane Instruktaż przy użyciu zestawu danych 1 TB. Użytkowników, którzy Aby skorzystać z tej metody należy można znaleźć w temacie [Instruktaż Criteo za pomocą połączenia ODBC gałęzi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Opis zestawu danych podróży taksówki Warszawa

Dane Warszawa taksówki podróży jest około 20GB plików skompresowany wartości rozdzielanych przecinkami (CSV) (~ 48GB nieskompresowane), zawierających więcej niż miliona 173 pojedynczych podróży i opłaty opłacona każdej podróży. Każdy podróży rekord zawiera odbiór i przyjmowania lokalizację i czas, zastąpione anonimowymi próba złamania (sterownik) liczbę licencji i liczba Medalionu (taksówki i unikatowy identyfikator). Dane obejmuje wszystkie podróży w roku 2013 i znajduje się w następujących dwa zestawy danych dla każdego miesiąca:

1. Pliki CSV "trip_data" zawierają szczegóły podróży, takie jak liczba osób, pobrania i dropoff punktów, czas trwania podróży i długość podróży. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Pliki CSV "trip_fare" zawierają szczegóły taryfy za każdym razem, takie jak typ płatności, kwota taryfy, opłaty dodatkowej i podatki, porady i drogowe i całkowitą kwotę zapłaconą. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Unikatowy klucz, aby dołączyć do podróży\_danych i podróży\_taryfy składa się z polami: Medalionu, próba złamania\_licencji oraz odbiór\_daty i godziny.

Aby uzyskać szczegółów dotyczącym określonego wycieczki, wystarczy dołączanie za pomocą trzech kluczy: "Medalionu", "Funkcja\_licencji" i "pobranie\_daty i godziny".

Gdy mamy ich przechowywania w tabelach gałęzi wkrótce opisano niektóre szczegółowe dane.

## <a name="mltasks"></a>Przykłady przewidywania zadań
Gdy zbliża się dane, określanie rodzaju przewidywań, które mają być na podstawie analizy pomaga wyjaśnienia zadania, które należy uwzględnić w procesie.
Oto trzy przykłady przewidywania problemów, które można rozwiązać w tym instruktażu, na podstawie których skład *Porada\_kwota*:

1. **Binarny klasyfikacji**: przewidywanie czy etykietkę zapłacono w podróży, to znaczy *Porada\_kwota* większą niż 0 zł jest dodatnia przykład, podczas gdy *Porada\_kwota* $0 jest ujemna przykład.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Klasyfikacja multiclass**: przewidywanie zakres kwoty Porada opłacona podróży. Dzielimy *Porada\_kwota* do pięciu przedziałów lub klas:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Zadanie regresji**: przewidywanie ilość Porada spłacona w podróży.  


## <a name="setup"></a>Konfigurowanie klaster HDInsight Hadoop analiz zaawansowane

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

Możesz skonfigurować środowisko Azure zaawansowanej analizy, której użyto klaster HDInsight w trzech kroków:

1. [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md): to konto miejsca do magazynowania jest używane do przechowywania danych w magazynie obiektów Blob platformy Azure. Dane używane w HDInsight klastrów znajdującą się poniżej.

2. [Dostosowywanie Azure HDInsight Hadoop klastrów proces zaawansowane analizy i technologii](machine-learning-data-science-customize-hadoop-cluster.md). Spowoduje to utworzenie HDInsight Hadoop platformy Azure klaster z 2.7 Python Anaconda 64-bitowej zainstalowany na wszystkich węzłach. Dostępne są dwie ważne czynności należy pamiętać podczas dostosowywania klaster HDInsight.

    * Pamiętaj połączyć konto miejsca do magazynowania utworzony w kroku 1 z klaster HDInsight podczas jej tworzenia. To konto miejsca do magazynowania jest używane dostępu do danych jest przetwarzana w klastrze.

    * Po utworzeniu klaster Włącz dostęp zdalny do głowy węzła klaster. Przejdź na kartę **Konfiguracja** , a następnie kliknij pozycję **Włącz zdalny**. W tym kroku Określa poświadczenia użytkownika do logowania zdalnego.

3. [Tworzenie obszaru roboczego Azure maszynowego uczenia](machine-learning-create-workspace.md): nauki ten komputer Azure obszar roboczy jest używany do utworzenia maszynowego uczenia modeli. Po zakończeniu badań danych początkowych i w dół przy próbkowaniu przy użyciu klaster HDInsight skierowana jest to zadanie.

## <a name="getdata"></a>Pobieranie danych ze źródła publiczne

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

Aby uzyskać zestaw danych [Warszawa taksówki podróży](http://www.andresmh.com/nyctaxitrips/) z jego lokalizacji publicznej, można użyć z metod opisanych w [Przenoszenie danych do i z magazynem obiektów Blob platformy Azure](machine-learning-data-science-move-azure-blob.md) Aby skopiować dane do komputera.

Poniżej opisano sposób za pomocą AzCopy transfer plików zawierających dane. Aby pobrać i zainstalować AzCopy, postępuj zgodnie z instrukcjami [wprowadzenie](../storage/storage-use-azcopy.md)do narzędzia wiersza polecenia AzCopy.

1. W oknie wiersza polecenia programu problemu następujące polecenia AzCopy, zamieniając *< path_to_data_folder >* docelowej:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Po zakończeniu kopiowania w sumie 24 zip pliki znajdują się w wybrany folder danych. Rozpakuj plik pobrane pliki do tego samego katalogu na komputerze lokalnym. Zanotuj folder, w którym znajdują się pliki nieskompresowane. Ten folder zostanie określone jako *< ścieżka\_do\_unzipped_data\_pliki\> * jest poniżej.


## <a name="upload"></a>Przekazywanie danych do domyślnego kontenera klaster Azure HDInsight Hadoop

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

W następujących poleceń AzCopy Zamień następujących parametrów na rzeczywiste wartości, które określone podczas tworzenia klaster Hadoop i rozpakować pliki danych.

* ***& #60; path_to_data_folder >*** katalogu (wraz z ścieżka) na komputerze, zawierające pliki rozpakowane danych  
* ***& #60; nazwę konta magazynu klastrze Hadoop >*** miejsca do magazynowania konta skojarzonego z klaster HDInsight
* ***& #60; domyślnego kontenera klaster Hadoop >*** domyślnego kontenera używane przez klaster. Zauważ, że nazwa domyślnego kontenera jest zazwyczaj taką samą nazwę jak klastrem. Na przykład jeśli klaster ma nazwę "abc123.azurehdinsight.net", domyślny kontener jest abc123.
* ***& #60; klucz konta miejsca do magazynowania >*** klucz konta magazynu używane przez klaster

Z wiersza polecenia lub okna programu Windows PowerShell na tym komputerze uruchom następujące dwa polecenia AzCopy.

To polecenie wydajnie przekazuje dane podróży do katalogu ***nyctaxitripraw*** w kontenerze domyślnym klaster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

To polecenie wydajnie przekazuje dane opłatę do katalogu ***nyctaxifareraw*** w kontenerze domyślnym klaster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Dane powinny się teraz w magazynie obiektów Blob platformy Azure i gotowe do zużycia w klastrze HDInsight.

## <a name="#download-hql-files"></a>Logowanie do węzła głównego klastrze Hadoop i i przygotować do analizy danych badawczych

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

Aby uzyskać dostęp do węzła głównego klastrze do analizy danych badawczych i w dół do pobierania danych, wykonaj procedurę opisaną w [programie Access węzeł głowy klaster Hadoop](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

W tym instruktażu przede wszystkim używamy kwerendy, napisane w [gałęzi](https://hive.apache.org/), językiem kwerend SQL podobne do wykonywania explorations wstępne dane. Kwerendy gałęzi są przechowywane w plikach .hql. Firma Microsoft następnie Strzałka w dół przykładowych danych do użytku w Azure maszynowego szkoleniowe dotyczące tworzenia modeli.

Aby przygotować klaster do analizy danych badawczych, firma Microsoft pobrać pliki .hql zawierające istotnych skryptów gałęzi z [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) do katalogu lokalnego (C:\temp) na węzła głównego. Aby to zrobić, otwórz **Wiersz polecenia** z poziomu węzła głównego klaster i problemu dwóch następujących poleceń:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Te dwa polecenia pobierze wszystkie pliki .hql potrzebne w tym instruktażu do katalogu lokalnego ***C:\temp & #92;*** w węzła głównego.

## <a name="#hive-db-tables"></a>Tworzenie bazy danych gałęzi i tabel podzielona według miesiąca

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

Teraz możemy przystąpić do tworzenia tabel gałąź dla naszych zestawu danych taksówki Warszawa.
W węzła głównego klaster Hadoop Otwórz ***Wiersz polecenia Hadoop*** na pulpicie węzła głównego i wprowadź katalogu gałęzi wpisując polecenie

    cd %hive_home%\bin

>[AZURE.NOTE] **Uruchamianie wszystkie polecenia gałęzi w tym instruktażu z powyższych pojemnika gałęzi i monit katalogu. To będzie zajmować się jakiekolwiek problemy ścieżkę automatycznie. Firma Microsoft korzysta z warunków "Gałęzi katalogu monit", "pojemnika gałęzi-katalogu monit" i "wiersza polecenia Hadoop" są używane zamiennie w tym instruktażu.**

W wierszu gałęzi katalogu wprowadź poniższe polecenie w Hadoop wierszu polecenia węzła głównego przesłania kwerendy gałęzi, aby utworzyć bazę danych gałęzi i tabele:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Oto zawartość ***C:\temp\sample\_gałąź\_tworzenie\_db\_i\_tables.hql*** pliku, który tworzy gałęzi bazy danych ***nyctaxidb*** i tabele ***podróży*** oraz ***taryfy***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Ten skrypt gałęzi tworzy dwóch tabel:

* Tabela "strony" zawiera szczegóły dotyczące podróży każdego innych (Szczegóły sterownika wraz z godzinami poczty, odległość podróży i godzin)
* Tabela "taryfy" zawiera szczegóły taryfy (taryfy kwota, kwota Porada, drogowe i opłaty).

Jeśli potrzebujesz dowolnej dodatkowej pomocy dotyczącej poniższe procedury lub konieczne jest analizowanie alternatywny z nich, zobacz sekcję [Przesyłanie gałęzi kwerendy bezpośrednio z poziomu Hadoop wiersza polecenia ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Ładowanie danych do tabel programu Hive przez partycje

>[AZURE.NOTE] Jest to zazwyczaj zadania **administracyjne** .

Zestaw danych taksówki Warszawa występują naturalne podziału według miesięcy, której użyjemy umożliwiające osiągać krótsze czasy ich przetwarzania i kwerendy. Polecenia programu PowerShell poniżej (wydać z katalogu gałęzi za pomocą **wiersza polecenia Hadoop**) ładowanie danych do tabel programu Hive "strony" i "opłata" podzielona według miesiąca.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

*Próbki\_gałąź\_ładowanie\_danych\_przez\_partitions.hql* plik zawiera następujące polecenia **ZAŁADUJ** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Należy zauważyć, że liczba kwerend gałęzi, które firma Microsoft korzysta z tutaj w procesie badań związane z jedną partycją lub tylko kilka partycje. Ale te zapytania, może uruchomić przez cały dane.

### <a name="#show-db"></a>Pokazywanie baz danych w grupie HDInsight Hadoop

Aby wyświetlić baz danych utworzonych w klastrze HDInsight Hadoop w oknie wiersza polecenia Hadoop, uruchom następujące polecenie w wierszu polecenia Hadoop:

    hive -e "show databases;"

### <a name="#show-tables"></a>Pokaż tabele gałęzi w bazie danych nyctaxidb

Aby pokazać tabele w bazie danych nyctaxidb, uruchom następujące polecenie w wierszu polecenia Hadoop:

    hive -e "show tables in nyctaxidb;"

Możemy potwierdzić, że tabele oddzielone od siebie wysyłając poniższe polecenie:

    hive -e "show partitions nyctaxidb.trip;"

Poniżej pokazano oczekiwane wyniki:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Podobnie możemy zapewnić, że tabela taryfy jest podzielona według poniższe polecenie:

    hive -e "show partitions nyctaxidb.fare;"

Poniżej pokazano oczekiwane wyniki:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Poszukiwanie danych i funkcji techniczny w gałęzi

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Badań danych i funkcji inżynierskich zadania związane z dane wprowadzone do tabel gałąź można wykonywać przy użyciu kwerendy gałęzi. Poniżej przedstawiono przykłady takie zadania, że wykonaniem w tej sekcji:

- Wyświetl rekordy 10 pierwszych w obu tabelach.
- Eksplorowanie danych podziału kilku pól w różnych okien czasu.
- Badanie jakości danych pól szerokości i długości geograficznej.
- Generowanie etykiet binarne i multiclass klasyfikacji na podstawie **Porada\_kwota**.
- Generowanie funkcje przez obliczenie odległości bezpośredni podróży.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Więcej informacji: Wyświetlanie rekordów 10 pierwszych w podróży tabeli

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Aby zobaczyć, jak wygląda dane, możemy zbadać 10 rekordy z każdej tabeli. Uruchom następujące dwa zapytania oddzielnie w wierszu katalogu gałęzi w konsoli wiersza polecenia Hadoop, aby sprawdzić rekordy.

Aby wyświetlić rekordów 10 pierwszych w tabeli "strony" w pierwszym miesiącu:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Aby uzyskać najważniejsze 10 rekordów w tabeli "opłata" w pierwszym miesiącu:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Warto często zapisuje je do pliku w celu wyświetlania wygodny. Zmiana małych powyższa kwerenda obejmuje to:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Więcej informacji: Wyświetlanie liczby rekordów w każdym z 12 partycje

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Odsetki jest jak Liczba podróży zmienia się w ciągu roku kalendarzowego. Grupowanie według miesięcy pozwala zobaczyć, jak wygląda rozpowszechnianie podróży.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Daje wynik:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

W tym miejscu pierwsza kolumna zawiera miesiąca, a drugim liczba podróży w danym miesiącu.

Firma Microsoft zliczyć całkowitą liczbę rekordów w zestawie danych naszych podróży wysyłając poniższe polecenie w wierszu polecenia katalogu gałęzi.

    hive -e "select count(*) from nyctaxidb.trip;"

Daje:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Za pomocą poleceń podobne do wyświetlania dla zestawu danych w podróży, możemy problemów gałęzi kwerendy w wierszu gałęzi katalogu dla zestawu danych taryfy sprawdzić poprawność liczbę rekordów.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Daje wynik:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Zauważ, że dla obu zestawów danych zwracany jest liczbą samej podróży na miesiąc. Dzięki temu pierwsze sprawdzenie, czy poprawnie załadowaniu danych.

Zliczanie całkowitej liczby rekordów w zestawie danych taryfy można zrobić za pomocą polecenia poniżej wierszu gałęzi katalogu:

    hive -e "select count(*) from nyctaxidb.fare;"

Daje:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Całkowita liczba rekordów w obu tabelach jest również. Dzięki temu drugiego weryfikacji, że poprawnie załadowaniu danych.

### <a name="exploration-trip-distribution-by-medallion"></a>Więcej informacji: Podróży rozpowszechniania przez Medalionu

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

W tym przykładzie identyfikuje Medalionu (taksówki liczby) z więcej niż 100 podróży w danym okresie. Zalety kwerendy z tabeli podzielonej na partycje programu access, ponieważ należy przygotować zmiennych partition **miesiąca**. Wyniki kwerendy są zapisywane w pliku lokalnego queryoutput.tsv w `C:\temp` na węzła głównego.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Oto zawartość *próbki\_gałąź\_podróży\_Statystyka\_przez\_medallion.hql* pliku inspekcji.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medalionu w zestawie danych taksówki Warszawa służy do identyfikowania unikatowych cab. Wskazano można, które OOZ są "zajęty" pytaniem, które z nich wykonane więcej niż liczba podróży w określonym przedziale czasu. Poniższy przykład identyfikuje OOZ wprowadzone podróży więcej niż sto pierwszego trzy miesiące i zapisywanie wyników kwerendy do pliku lokalnego C:\temp\queryoutput.tsv.

Oto zawartość *próbki\_gałąź\_podróży\_Statystyka\_przez\_medallion.hql* pliku inspekcji.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

W wierszu gałęzi katalogu wydaj polecenie poniżej:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Więcej informacji: Podróży rozkładu Medalionu i hack_license

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Gdy poznawanie zestawu danych, chcemy często Sprawdź liczbę wystąpień co grupy wartości. W tej sekcji zawierają przykład jak to zrobić dla OOZ i sterowników.

*Próbki\_gałąź\_podróży\_Statystyka\_przez\_Medalionu\_license.hql* plików grupy zestawu danych taryfy na "Medalionu" i "hack_license" i zwraca wartość liczby kombinacjami. Poniżej znajdują się jego zawartość.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Ta kwerenda zwraca cab i kombinacje określony sterownik uporządkowanych według malejącej Liczba podróży.

W wierszu gałęzi katalogu Uruchom:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Wyniki kwerendy są zapisywane w pliku lokalnym C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Więcej informacji: Oceny jakości danych, zaznaczając pole wyboru nieprawidłową długość i szerokość geograficzna rekordów

>[AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Typowe celem analizy danych badawczych jest chwastów nieprawidłowe lub nieprawidłowe rekordy. Przykład w tej sekcji określa, czy długość geograficzna albo szerokości pola zawierają wartość daleko poza obszarem Warszawa. Ponieważ jest to prawdopodobieństwo, że takie rekordy mają wartości błędnych szerokości geograficznej, chcemy Usuń je z jakiekolwiek dane, który ma być używany do modelowania.

Oto zawartość *próbki\_gałąź\_jakości\_assessment.hql* pliku inspekcji.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


W wierszu gałęzi katalogu Uruchom:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Argument *-S* zawarte w tym poleceniu pomija wydruk ekranu stanu zadań Mapa/zmniejszanie gałęzi. Jest to przydatne, ponieważ ułatwia ekranu drukowania z wyników kwerendy gałęzi bardziej czytelne.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Badań: Podział binarne klasy porady dotyczące podróży

> [AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Binarny klasyfikacji problemu opisane w sekcji [przykładów zadań przewidywania](machine-learning-data-science-process-hive-walkthrough.md#mltasks) warto wiedzieć, czy Porada na ten temat podano lub nie. Rozpowszechnianie porad jest binarne:

* Porada podane (klasy 1, porada\_kwota > 0 zł)  
* Brak porady (klasy 0, porada\_kwota = 0 zł).

*Próbki\_gałąź\_Przechylony\_frequencies.hql* pliku, jak pokazano poniżej powinien się tym zająć.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

W wierszu gałęzi katalogu Uruchom:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Więcej informacji: Rozkład zajęć ustawienie multiclass

> [AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Problemu multiclass klasyfikacji opisane w sekcji [przykładów zadań przewidywania](machine-learning-data-science-process-hive-walkthrough.md#mltasks) tym zestawie danych również pozwalają na naturalne klasyfikacji, w której chcemy przewidywanie ilość porady podane. Firma Microsoft korzysta przedziałów definiowaniu zakresów Porada w kwerendzie. Aby uzyskać rozkład zajęć dla różnych Porada zakresów, firma Microsoft korzysta z *próbki\_gałąź\_Porada\_zakres\_frequencies.hql* pliku. Poniżej znajdują się jego zawartość.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Z poziomu wiersza polecenia Hadoop konsoli, uruchom następujące polecenie:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Więcej informacji: Obliczanie bezpośredni odległość między dwiema lokalizacjami szerokości długości geograficznej

> [AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

O miarę bezpośredni odległość pozwala dowiedzieć się rozbieżności między tą kolumną i odległość podróży rzeczywiste. Firma Microsoft zmotywowania tę funkcję, wskazujące, że osób może być rzadziej Porada jeżeli ich wiadomo, że sterownik celowo miało ich drogą dłużej.

Aby zobaczyć porównanie odległość podróży rzeczywiste i [Haversine odległości](http://en.wikipedia.org/wiki/Haversine_formula) między dwoma punktami szerokości długość geograficzna (odległość "koła"), stosujemy dostępne funkcje trygonometryczne w gałęzi, więc:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

W kwerendzie powyżej R jest promieniem ziemi w mila i pi jest konwertowana na radiany. Należy zauważyć, że punkty szerokości geograficznej są "filtrowane" Aby usunąć wartości, które są daleko od obszaru Warszawa.

W tym przypadku możemy naszych wyników do zapisu katalogu o nazwie "queryoutputdir". Sekwencja poleceń, jak pokazano poniżej najpierw tworzy ten katalog dane wyjściowe, a następnie wykonuje polecenie gałęzi.

W wierszu gałęzi katalogu Uruchom:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Wyniki kwerendy są zapisywane w 9 Azure obiektów blob ***queryoutputdir-000000\_0*** do ***queryoutputdir-000008\_0*** w kontenerze domyślne klaster Hadoop.

Aby wyświetlić rozmiar poszczególnych obiektów blob, możemy uruchom następujące polecenie w wierszu katalogu gałęzi:

    hdfs dfs -ls wasb:///queryoutputdir

Aby wyświetlić zawartość danego pliku, na przykład 000000\_0, firma Microsoft korzysta z firmy Hadoop `copyToLocal` polecenia w ten sposób.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`mogą być bardzo wolne dla dużych plików, a nie jest zalecane do użytku z nimi.  

Przewaga o te dane znajdują się w obiektów blob platformy Azure to, że firma Microsoft może Eksplorowanie danych w ramach Azure nauki komputera przy użyciu [Importowanie danych] [ import-data] modułu.


## <a name="#downsample"></a>Przykładowe dane i tworzenie modeli w Azure maszynowego uczenia w dół

> [AZURE.NOTE] Jest to zazwyczaj zadania **Naukowca danych** .

Po fazie analizy danych badawczych możemy przystąpić do przykładowych danych do tworzenia modeli w Azure maszynowego uczenia w dół. W tej sekcji pokażemy, jak za pomocą kwerendy gałęzi w dół przykładowych danych, co jest następnie dostępny [Importowanie danych] [ import-data] moduł w Azure maszynowego uczenia.

### <a name="down-sampling-the-data"></a>W dół próbek danych

Istnieją dwa kroki w tej procedurze. Najpierw możemy łączenia tabel **nyctaxidb.trip** i **nyctaxidb.fare** na trzy klucze, które znajdują się we wszystkich rekordach: "Medalionu", "Funkcja\_licencji", a "pobrania\_datetime". Firma Microsoft następnie wygenerować etykiety z binarne klasyfikacji, **Przechylony** i etykiety klasyfikacji wielu klasy **Porada\_klasy**.

Aby można było Użyj pobrane dane bezpośrednio z [Importu danych] [ import-data] moduł w Azure maszynowego uczenia, należy przechowywać wyniki powyższa kwerenda do wewnętrznej tabeli gałęzi. W poniżej firma Microsoft Utwórz tabelę wewnętrzną gałęzi i wypełnij jego zawartość z sprzężonej i w dół próbki danych.

Kwerenda początkowa standardowych funkcji gałęzi bezpośrednio do wygenerowania godziny dnia, tygodnia w roku, dnia tygodnia (1 zawiera poniedziałek i 7 stojaki niedziela) "pobrania\_daty i godziny" pole i bezpośredni odległość między lokalizacjami odbiór i dropoff. Aby uzyskać pełną listę funkcji użytkowników można znaleźć [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) .

Kwerenda następnie Strzałka w dół próbki danych tak, aby wyniki kwerendy można umieścić w Studio nauki maszynowego Azure. Tylko o 1% oryginalny zestaw danych zostanie zaimportowana do Studio.

Poniżej przedstawiono zawartość *próbki\_gałąź\_przygotowywanie\_dla\_aml\_full.hql* pliku, który przygotowuje danych dla modelu tworzenia w Azure maszynowego uczenia.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Aby uruchomić tej kwerendy w wierszu gałęzi katalogu:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Teraz mamy wewnętrznej tabeli "nyctaxidb.nyctaxi_downsampled_dataset", który można uzyskać dostęp za pomocą [Importowanie danych] [ import-data] modułu Azure maszynowego uczenia. Ponadto możemy używać tego zestawu danych do tworzenia modeli nauki komputera.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Używanie modułu importowanie danych w Azure maszynowego uczenia uzyskiwać dostęp do danych próbki dół

Jako wymagania wstępne dotyczące wydawania gałęzi zapytań w oknie [Importowanie danych] [ import-data] moduł Azure maszynowego uczenia, potrzebujemy dostęp do komputera Azure nauki obszaru roboczego i dostęp do poświadczeń klaster i jego konto skojarzone miejsca do magazynowania.

Niektóre szczegółowych informacji na temat [Importowania danych] [ import-data] moduł i parametrów wejściowych:

**Identyfikator URI serwera HCatalog**: Jeśli nazwa klaster jest abc123, a następnie jest to po prostu: https://abc123.azurehdinsight.net

**Nazwa konta użytkownika Hadoop** : nazwa użytkownika wybrany dla klaster (**nie** nazwa użytkownika dostępu zdalnego)

**Hasło konta ser Hadoop** : hasło wybrany dla klaster (**nie** hasła dostępu zdalnego)

**Lokalizację danych wyjściowych** : wybrano to być Azure.

**Nazwę konta magazynu platformy Azure** : nazwę konta magazynu domyślnego skojarzone z klastrem.

**Nazwa kontenera Azure** : to jest domyślna nazwa kontenera klaster i zazwyczaj jest taki sam jak jego nazwy. Klaster o nazwie "abc123" to tylko abc123.

> [AZURE.IMPORTANT] **Życzymy kwerendy przy użyciu [Importowanie danych] tabeli[ import-data] moduł w Azure maszynowego uczenia musi być wewnętrznej tabeli.** Porada sprawdzania, czy tabela T w bazie danych D.db jest wewnętrznej tabeli jest w następujący sposób.

W wierszu gałęzi katalogu wydaj polecenie:

    hdfs dfs -ls wasb:///D.db/T

Jeśli tabela znajduje się wewnętrznej tabeli i jest wypełniana, jego zawartość musi Pokaż tutaj. Innym sposobem na określenie, czy tabela jest wewnętrznej tabeli jest za pomocą Eksploratora magazynu Azure. Przejdź do domyślnej nazwy kontenera klaster, a następnie filtrowanie według nazwy tabeli, użyj go. Jeśli w tabeli i jej zawartość jest widoczna, to potwierdzenie jest wewnętrznej tabeli.

Oto migawki kwerendy gałęzi i [Importowanie danych] [ import-data] modułu:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Uwaga Aby od naszych szczegółów próbki dane znajdują się w kontenerze domyślnym wyniku kwerendy gałęzi Azure maszynowego uczenia jest bardzo proste i jest po prostu "Wybierz * z nyctaxidb.nyctaxi\_próbkowane w dół\_danych".

Zestaw danych teraz można jako punktu wyjścia do tworzenia modeli nauki komputera.

### <a name="mlmodel"></a>Tworzenie modeli w nauki maszynowego Azure

Teraz możemy przejść do tworzenia modelu i wdrożenia modelu w [Azure maszynowego uczenia](https://studio.azureml.net). Dane są gotowe do nas korzystać w rozwiązywaniu problemów przewidywania określonych powyżej:

**1. klasyfikacji dwójkową**: przewidywanie czy etykietkę zapłacono w podróży.

**Się wzrokowo używane:** Regresją dwóch zajęć

. Ten problem naszych etykiety docelowej (lub klasy) jest "Przechylony". Nasze oryginalny zestaw danych pobrane szczegółów zawiera kilka kolumn, które są przecieki docelowej dla tego doświadczenia klasyfikacji. W szczególności: Porada\_klasy, porada\_kwota i sumowanie\_kwota ujawnianie informacji na temat etykiecie docelowej, która nie jest dostępny na testowanie czasu. Firma Microsoft umożliwia usunięcie tych kolumn z uwagę za pomocą [Wybieranie kolumn w zestawie danych] [ select-columns] modułu.

Migawki poniżej pokazano naszych doświadczenia przewidywanie czy etykietkę poniesiony na danej podróży.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Do tego doświadczenia naszych dystrybucji etykiety docelowej były około 1:1.

Migawka poniżej pokazuje rozkład Porada etykiety klasy problemu klasyfikacji binarne.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

W wyniku możemy uzyskać AUC 0.987, jak pokazano na poniższej ilustracji.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass klasyfikacji**: przewidywanie zakres kwoty Porada opłacona podróży przy użyciu wcześniej zdefiniowanych klas.

**Się wzrokowo używane:** Multiclass regresją

. Tego problemu jest naszych etykiety docelowej (lub klasy) "Porada\_zajęć" której można wykonać jedną z pięciu wartości (0,1,2,3,4). Podobnie jak w przypadku klasyfikacji binarne mamy kilka kolumn, które tego doświadczenia w nim nieszczelności docelowej. W szczególności: Przechylony, porada\_kwota całkowita\_kwota ujawnianie informacji na temat etykiecie docelowej, która nie jest dostępny na testowanie czasu. Usuwania tych kolumn przy użyciu [Wybierz kolumny w zestawie danych] [ select-columns] modułu.

Migawka poniżej przedstawiono naszych doświadczenia przewidywanie, w których Koszu Porada jest mogą należeć (klasy 0: Porada = 0 zł, klasy 1: Porada > 0 zł i Porada < = $5, klasy 2: Porada > $5 i Porada < = $10, klasy 3: Porada > 10 zł i Porada < = 20 zł, klasy 4: Porada > 20 zł)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Teraz wyświetlane naszych rozkład zajęć rzeczywistego badania wygląda tak:. Widać, że klasy 0 i 1 klasy są powszechnie znane, innych klas są rzadko.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Dla tego doświadczenia używamy macierzy zamieszania aby przyjrzeć się naszych dokładności przewidywania. Poniżej przedstawiono to.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Należy zauważyć, że podczas naszych dokładności zajęć na rozpowszechnione klasy jest bardzo dobra, model nie zadanie dobre "nauki" rzadkich klas.


**3. zadania regresji**: przewidywanie ilość Porada spłacona w podróży.

**Się wzrokowo używane:** Algorytm zwiększane

. Tego problemu jest naszych etykiety docelowej (lub klasy) "Porada\_kwota". Nasze przecieki docelowej w tym przypadku są: Przechylony, porada\_klasy sumy\_kwota; te zmienne ujawnienia informacji o wartość Porada, która jest zazwyczaj niedostępny testowania czasu. Usuwania tych kolumn przy użyciu [Wybierz kolumny w zestawie danych] [ select-columns] modułu.

Belows migawkę zawiera naszych doświadczenia przewidywanie ilość danego poradę.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. W przypadku problemów regresji możemy Zmierz dokładności naszych przewidywania sprawdzając błędu w przewidywań, współczynnik wyznaczania i tym podobne. Zostanie wyświetlona tych poniżej.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Widać, że o współczynnik wyznaczania jest 0.709, co oznacza około 71% wariancję tłumaczy naszych współczynników modelu.

> [AZURE.IMPORTANT] Aby dowiedzieć się więcej na temat Azure maszynowego uczenia się i uzyskiwaniu dostępu do i z niej korzystać, skorzystaj [Co to jest maszynowego uczenia?](machine-learning-what-is-machine-learning.md). Zasób bardzo przydatne do odtwarzania wyświetlonych doświadczeń nauki komputera po otrzymaniu komputera Azure jest [Galeria analizy Cortana](https://gallery.cortanaintelligence.com/). Galeria obejmuje gamę doświadczeń i udostępnia dokładne wprowadzenie do zakresu możliwości Azure maszynowego uczenia.

## <a name="license-information"></a>Informacje o licencji

W tym instruktażu próbki i jego towarzyszące skryptów są udostępniane przez firmę Microsoft w obszarze licencja. Sprawdź, czy plik LICENSE.txt w katalogu przykładowy kod na GitHub, aby uzyskać więcej informacji.

## <a name="references"></a>Odwołania

• [Podróży taksówki Warszawa Andrés Monroy pobieranie strony](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing osoby Warszawa taksówki podróży danych według Whong Tomasza](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksówki Warszawa oraz Limousine prowizji badań i statystyki](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
