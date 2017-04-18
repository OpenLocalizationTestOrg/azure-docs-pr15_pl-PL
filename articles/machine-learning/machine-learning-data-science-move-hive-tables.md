<properties
    pageTitle="Tworzenie i ładowanie danych do tabel gałęzi z magazynem obiektów Blob | Microsoft Azure"
    description="Tworzenie tabel gałęzi i ładowanie danych w obiekcie blob do tabel"
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Tworzenie i ładowanie danych do tabel gałęzi z magazynem obiektów blob Azure

W tym temacie przedstawiono ogólne zapytania gałęzi, które Tworzenie tabel gałęzi i załadować dane z magazynem obiektów blob Azure. Poradnika znajduje się również na podziału gałąź Tabele i przy użyciu zoptymalizowane wiersza kolumnowy (ORC) formatowanie, aby zwiększyć wydajność kwerendy.

Ten **menu** linki do tematy opisujące mogły zjeść tej ostatniej danych w środowiskach docelowej miejsce, w którym dane można przechowywane i przetwarzane podczas procesu nauki danych zespołu (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że:

* Utworzone konto Azure miejsca do magazynowania. Aby uzyskać instrukcje, zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). 
* Obsługi administracyjnej dostosowane klaster Hadoop z usługą HDInsight.  Aby uzyskać instrukcje, zobacz [Dostosowywanie Azure HDInsight Hadoop klastrów w celu zaawansowanej analizy](machine-learning-data-science-customize-hadoop-cluster.md).
* Włączony dostęp zdalny z klastrem zalogowany i otworzyć konsoli wiersza polecenia Hadoop. Aby uzyskać instrukcje, zobacz [dostępu węzeł głowy klaster Hadoop](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Przekazywanie danych z magazynem obiektów blob platformy Azure
Jeśli utworzono Azure maszyn wirtualnych, postępując zgodnie z instrukcjami podanymi w [Konfigurowanie Azure maszyn wirtualnych dla zaawansowanej analizy](machine-learning-data-science-setup-virtual-machine.md), ten plik skryptu należy zostały pobrane na *C:\\użytkowników\\\<nazwa użytkownika\>\\dokumenty\\skryptów nauki danych* katalogu na komputerze wirtualnych. Te kwerendy gałąź jest wymagane tylko podłączeniu własny schemat danych i konfiguracji magazyn obiektów blob platformy Azure w odpowiednich polach będzie gotowa do przesyłania.

Przyjęto założenie, że dane dla tabel gałąź jest w formacie tabelarycznym **nieskompresowane** i że dane zostały przekazane do domyślnej (lub dodatkowy) kontenera używana przez klaster Hadoop konta miejsca do magazynowania.

Jeśli chcesz ćwiczenia **Warszawa taksówki podróży danych**, należy:

- **Pobierz** 24 pliki [Danych podróży taksówki Warszawa](http://www.andresmh.com/nyctaxitrips) , (12 pliki podróży i 12 pliki taryfy),
- **Rozpakuj plik** wszystkie pliki do plików CSV, a następnie
- **Przekaż** je do domyślnego (lub odpowiednim kontenerze) konta Azure miejsca do magazynowania, które zostało utworzone zgodnie z procedurą konspektu w temacie [klastrów Dostosowywanie Azure HDInsight Hadoop proces zaawansowane analizy i technologii](machine-learning-data-science-customize-hadoop-cluster.md) . Proces przekazywania plików CSV do kontenera domyślnego konta magazynu znajdują się na tej [stronie](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Sposób przesyłania kwerend gałęzi

Gałąź zapytań mogły być przesyłane za pomocą:

1. [Przesyłanie gałęzi zapytań za pomocą wiersza polecenia Hadoop w headnode Hadoop klaster](#headnode)
2. [Przesyłanie gałęzi zapytań za pomocą edytora gałęzi](#hive-editor)
3. [Przesyłanie kwerend gałęzi za pomocą polecenia programu PowerShell Azure](#ps)

Gałąź kwerendy są podobne SQL. Jeśli znasz SQL, może się okazać [gałęzi arkusza Cheat użytkowników SQL](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) przydatne.

Podczas przesyłania kwerendy gałęzi, można także kontrolować miejsce docelowe danych wyjściowych kwerendy gałęzi, czy jest na ekranie lub plik lokalny w węźle głowy lub obiektów blob platformy Azure.


###<a name="headnode"></a>1. gałąź zapytań za pomocą wiersza polecenia Hadoop w headnode Hadoop klaster przesyłanie

W przypadku złożonych kwerend gałęzi przesłaniem go bezpośrednio w węźle głowy Hadoop klaster zwykle prowadzi do szybciej Włączanie niż przesłaniem go za pomocą edytora gałęzi lub Azure programu PowerShell skryptów.

Zaloguj się do węzła głównego klastrze Hadoop, otwórz wiersz polecenia Hadoop na pulpicie węzła głównego i wprowadź polecenie `cd %hive_home%\bin`.

Dostępne są trzy sposoby przesyłania gałęzi kwerendy w wierszu polecenia Hadoop:

* bezpośrednio
* Korzystanie z plików .hql
* przy użyciu konsoli poleceń gałęzi

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Przesyłanie kwerend gałęzi bezpośrednio w Hadoop wiersza polecenia.

Można uruchomić polecenie, takie jak `hive -e "<your hive query>;` przesłania prostych kwerend gałęzi bezpośrednio w Hadoop wiersza polecenia. Oto przykład, gdzie pole czerwony omówiono polecenia przesyła kwerendy gałęzi i zielone pole zawiera dane wyjściowe z kwerendy gałęzi.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Przesyłanie kwerend gałęzi w plikach .hql

Gdy kwerenda gałąź jest bardziej skomplikowana i ma wiele wierszy, edytowania kwerend w wierszu polecenia lub konsoli poleceń gałęzi nie jest praktyczne. Alternatywy należy zapisać zapytania gałęzi w pliku .hql w katalogu lokalnym głowy węzła przy użyciu edytora tekstu w węźle głowy klaster Hadoop. Następnie kwerendę gałęzi w pliku .hql będą mogły być przesyłane za pomocą `-f` argument w następujący sposób:

    hive -f "<path to the .hql file>"

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Pomiń drukowanie ekranu stan postępu gałęzi kwerend**

Domyślnie po przesłaniu kwerendy gałęzi w wierszu polecenia Hadoop postępu zadania mapy/zmniejszanie jest drukowany na ekranie. Aby wyłączyć drukowanie ekranu mapy/zmniejszanie postęp zadania, można użyć argumentu `-S` ("S" pisany wielkimi literami) w tym poleceniu wiersza w następujący sposób:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Przesyłanie kwerend gałęzi w konsoli poleceń gałęzi.

Możesz też najpierw wprowadzić konsoli poleceń gałęzi, uruchamiając polecenie `hive` w wierszu polecenia Hadoop, a następnie przesyłać zapytania gałęzi w konsoli poleceń gałęzi. Oto przykład. W tym przykładzie dwa pola czerwone wyróżnianie polecenia służące do wprowadzania konsoli poleceń gałęzi i kwerendy gałęzi przesłane w konsoli poleceń gałęzi, odpowiednio. Pole zielone wyróżnienie dane wyjściowe z kwerendy gałęzi.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Poprzednich przykładach wyjściowy bezpośrednio gałęzi wyników kwerendy na ekranie. Można także napisać wynik plik lokalny na węzła głównego lub obiektów blob platformy Azure. Następnie w innych narzędzi umożliwia dalsze analizowanie danych wyjściowych gałęzi kwerend.

**Wyjściowy gałęzi wyników kwerendy do pliku lokalnego.**

Celu wyprowadzenia gałęzi wyników kwerendy do katalogu lokalnego na węzła głównego, należy przesłać gałęzi kwerendy w wierszu polecenia Hadoop w następujący sposób:

    hive -e "<hive query>" > <local path in the head node>

W poniższym przykładzie wyniki kwerendy gałęzi są zapisywane w pliku `hivequeryoutput.txt` w katalogu `C:\apps\temp`.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Wyniki kwerendy gałęzi wyjścia do obiektów blob platformy Azure**

Można również wyjściowy gałęzi wyników kwerendy do obiektów blob platformy Azure, w kontenerze domyślne klaster Hadoop. Kwerenda gałęzi to jest w następujący sposób:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

W poniższym przykładzie wyniki kwerendy gałęzi są zapisywane w katalogu obiektów blob `queryoutputdir` w kontenerze domyślne klaster Hadoop. W tym miejscu wystarczy podać nazwę katalogu bez nazwy obiektów blob. Jeśli podanie nazwy zarówno w katalogu, jak i obiektów blob, takich jak, jest generowany błąd `wasb:///queryoutputdir/queryoutput.txt`.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Po otwarciu domyślnego kontenera klaster Hadoop za pomocą Eksploratora magazynu Azure widać wyniki kwerendy gałęzi, jak pokazano na poniższym rysunku. Możesz zastosować filtr, (wyróżniony w polu czerwony) tylko pobrać obiektów blob określonych liter w nazwach.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. przesyłanie gałęzi zapytań za pomocą edytora gałęzi

Można też użyć konsoli kwerendy (gałęzi Editor), wprowadzenie adresu URL formularza *https://&#60; Nazwa klaster Hadoop >.azurehdinsight.net/Home/HiveEditor* w przeglądarce sieci web. Użytkownik musi być zalogowany Zobacz konsoli i dlatego poświadczenia klaster Hadoop są wymagane.

###<a name="ps"></a>3. przesyłanie kwerend gałęzi za pomocą polecenia programu PowerShell Azure

Za pomocą programu PowerShell do przesyłania kwerend gałęzi. Aby uzyskać instrukcje zobacz [Przesyłanie gałęzi zadań przy użyciu programu PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Tworzenie bazy danych gałęzi i tabel

Kwerendy gałęzi są udostępniane w [repozytorium Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) i można pobrać z niego.

Poniżej przedstawiono kwerendy gałęzi, który tworzy tabelę programu Hive.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Poniżej przedstawiono opisy pól, które należy podłączyć i innych konfiguracji:

- **& #60; Nazwa bazy danych >**: nazwy bazy danych, który chcesz utworzyć. Jeśli chcesz użyć domyślnej bazy danych, można pominąć query *Tworzenie... bazy danych* .
- **& #60; nazwę tabeli >**: Nazwa tabeli, która ma zostać utworzony w określonej bazie danych. Jeśli chcesz użyć domyślnej bazy danych tabeli może być bezpośrednio odwołują *& #60; nazwę tabeli >* bez & #60; Nazwa bazy danych >.
- **& #60; separator pola >**: separatora, który rozgranicza pól z pliku danych do przekazania do tabeli gałęzi.
- **& #60; separator wiersz >**: separatora, który rozgranicza wierszy w pliku danych.
- **& #60; lokalizacji przechowywania >**: lokalizacji przechowywania Azure, aby zapisać dane gałęzi tabel. Jeśli nie określisz *lokalizacji & #60; lokalizacji przechowywania >*, bazy danych i tabele są przechowywane w *gałęzi magazyn-* katalogu w kontenerze domyślnym klaster gałęzi domyślnie. Jeśli chcesz określić miejsce przechowywania lokalizacji przechowywania musi być w kontenerze domyślny dla bazy danych i tabele. Tej lokalizacji ma być określane jako lokalizacji względem domyślnego kontenera klaster w formacie *' wasb: / / / & #60; katalogu 1 >-"* lub *' wasb: / / / & #60; katalogu 1 >-& #60; katalogu 2 >-"*itp. Po wykonaniu kwerendy względne katalogi są tworzone w kontenerze domyślne.
- **TBLPROPERTIES("SKIP.Header.line.Count"="1")**: Jeśli plik danych zawiera wiersz nagłówka, należy dodać tej właściwości **na końcu** kwerendy *Tworzenie tabeli* . W przeciwnym razie wiersz nagłówka zostanie załadowana jako rekordu do tabeli. Jeśli plik danych nie ma wiersz nagłówka, można pominąć tę konfigurację, w kwerendzie.

## <a name="load-data"></a>Ładowanie danych do tabel gałęzi
Oto kwerenda gałęzi załadowaniu danych do tabeli gałęzi.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; ścieżkę dostępu do danych typu blob >**: Jeśli plik obiektów blob należy przekazać do tabeli gałęzi znajduje się w kontenerze domyślnym klaster HDInsight Hadoop *& #60; ścieżkę dostępu do danych typu blob >* powinny mieć format *"wasb: / / / & #60; katalogu, w tym kontenerze >-& #60; nazwy pliku obiektów blob >"*. Plik obiektów blob może również być kontenerem kolejne klastrze HDInsight Hadoop. W tym przypadku *& #60; ścieżkę dostępu do danych typu blob >* powinny mieć format *' wasb: / / & #60; kontener name>@&#60;storage nazwę konta >.blob.core.windows.net/ & #60; nazwy pliku obiektów blob > "*.

    >[AZURE.NOTE] Dane blob do przekazania do tabelę programu Hive musi być w domyślnej lub dodatkowego kontenera konta miejsca do magazynowania dla klastrów Hadoop. W przeciwnym razie zapytanie *Ładowania danych* kończy się niepowodzeniem, strona skarżąca, że nie można uzyskać dostęp do danych.


## <a name="partition-orc"></a>Tematy dotyczące zaawansowanego: na partycje tabeli i magazynu danych gałęzi w formacie ORC

Jeśli danych jest duży, podziału tabeli jest przydatne w przypadku kwerend, które trzeba tylko skanowanie kilka partycje tabeli. Na przykład jest rozsądne dzielenia danych dziennika witryny sieci web według daty.

Oprócz dzielony gałąź tabele, jest również przechowywane dane gałęzi w formacie zoptymalizowane wiersza kolumnowy (ORC). Aby uzyskać więcej informacji na temat formatowania ORC zobacz <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">pliki przy użyciu ORC zwiększona wydajność podczas gałąź jest do czytania, zapisywania i przetwarzanie danych</a>.

### <a name="partitioned-table"></a>Tabeli podzielonej na partycje
Poniżej przedstawiono kwerendy gałęzi, która tworzy tabelę podzielone na partycje i załadowanie danych do niego.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Podczas wysyłania kwerend tabel podzielonych na partycje, zaleca się na **początku** przyciski Dodaj warunek partition `where` klauzula jako to zwiększa skuteczność wyszukiwania znacznie.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Przechowywanie danych gałęzi w formacie ORC

Nie można bezpośrednio załadowanie danych z magazynem obiektów blob do gałęzi tabel, które są przechowywane w formacie ORC. Poniżej przedstawiono kroki, które należy wykonać, aby załadować dane z Azure BLOB do tabel gałęzi zapisane w formacie ORC.

Utworzenie tabeli zewnętrznej **PRZECHOWYWANE jako TEXTFILE** i ładowania danych z magazynem obiektów blob do tabeli.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Utwórz tabelę wewnętrzną z ten sam schemat jako tabeli zewnętrznej w kroku 1, przy użyciu tego samego ogranicznika pola i przechowywania danych gałęzi w formacie ORC.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Zaznacz dane z tabeli zewnętrznej w kroku 1 i wstawić do tabeli ORC

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Jeśli w tabeli TEXTFILE *& #60; Nazwa bazy danych >. & #60; nazwy tabeli zewnętrznej textfile >* występują partycje w KROKU 3 `SELECT * FROM <database name>.<external textfile table name>` polecenie zaznacza zmiennej partycją jako pola w zestawie danych zwróconych. Wstawiany *& #60; Nazwa bazy danych >. & #60; nazwy tabeli ORC >* kończy się niepowodzeniem, ponieważ *& #60; Nazwa bazy danych >. & #60; nazwy tabeli ORC >* nie ma zmiennej partycją jako pola w schemacie tabeli. W tym przypadku należy wybrać pola, które mają być wstawiony w celu *& #60; Nazwa bazy danych >. & #60; nazwy tabeli ORC >* w następujący sposób:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Bezpiecznie upuść *& #60; nazwy tabeli zewnętrznej textfile >* po przy użyciu następującej kwerendy po wszystkich danych został wstawiony do *& #60; Nazwa bazy danych >. & #60; nazwy tabeli ORC >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Po wykonaniu tej procedury, należy mieć tabeli z danymi w formacie ORC gotowe do użycia.  
