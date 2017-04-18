<properties
    pageTitle="Proces zespołu danych naukowych w działaniu: za pomocą HDInsight Hadoop klastrów zestawu danych Criteo 1 TB | Microsoft Azure"
    description="Przy użyciu procesu nauki danych zespołu do scenariusza końcu do końca zastosowanie klaster HDInsight Hadoop do tworzenia i wdrażania modelu przy użyciu dużego (1 TB) publicznie zestawu danych"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Proces zespołu danych naukowych w działaniu — za pomocą Azure HDInsight Hadoop klastrów na 1 TB zestawu danych

W tym instruktażu możemy wykazać przy użyciu procesu zespołu danych naukowych w scenariuszu zakończenia do końca z [klaster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) do przechowywania, eksplorowanie odtwarzania funkcji i przykładowe dane z jednej z dostępnych publicznie zestawy danych [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) w dół. Firma Microsoft korzysta z Azure maszynowego uczenia tworzenia modelu klasyfikacji binarne na tych danych. Zostanie wyświetlona również jak publikować jednego z tych modeli jako usługi sieci Web.

Użytkownik może również Notes IPython służy do wykonywania zadań przedstawione w tym instruktażu. Użytkowników, którzy Aby skorzystać z tej metody należy można znaleźć w temacie [Instruktaż Criteo za pomocą połączenia ODBC gałęzi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Opis zestawu danych Criteo

Criteo, które dane są zestawu danych przewidywania kliknij około 370 GB pliki TSV gzip skompresowany (~1.3TB nieskompresowane), zawierających więcej niż 4.3 miliardy rekordów. Jest przyjmowana z 24 dni kliknij pozycję dane udostępnione przez [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Dla wygody naukowców danych możemy mieć rozpakowane danych dostępnych do nas wypróbowanie.

Każdy rekord w tym zestawie danych zawiera 40 kolumn:

- Pierwsza kolumna zawiera kolumnę etykieta, która wskazuje, czy użytkownik kliknie przycisk **Dodaj** (wartość 1) lub nie kliknij jeden z (wartość 0)
- Następny 13 kolumny to liczbowe, a
- ostatni 26 są kolumny kategorii

Kolumny zostaną zastąpione anonimowymi i używanie serii wyliczenia nazw: "Kol1" (w przypadku kolumna etykieta) do "Col40" (w przypadku ostatnia kolumna kategorii).            

Oto fragment pierwszych 20 kolumny dwóch obserwacji (wierszy) z tego zestawu danych:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Istnieje brakujące wartości w kolumnach liczbowych i kategorii w tym zestawie danych. Opisano prostą metodę obsługi brakujące wartości. Dodatkowe informacje dotyczące dane zostały opisane po ich przechowywane w tabelach gałęzi.

**Definicji:** *Stawka liczbą kliknięć (kont.):* Jest to wartość procentową kliknięć w danych. W tym zestawie danych Criteo kont to około 3,3% lub 0.033.

## <a name="mltasks"></a>Przykłady przewidywania zadań
Dwa przykładowe przewidywania problemy zostały opisane w tym instruktażu:

1. **Binarny klasyfikacji**: wskazuje, czy użytkownik kliknie Dodaj:
    - Kategoria 0: Nie ma możliwości kliknięcia
    - Klasy 1: kliknij pozycję

2. **Regresji**: przewiduje prawdopodobieństwo kliknij ad z funkcji użytkownika.


## <a name="setup"></a>Ustawianie konta HDInsight Hadoop klaster do nauki danych

**Uwaga:** Jest to zazwyczaj zadania **administracyjne** .

Konfigurowanie środowiska Azure danych nauki dla tworzenie rozwiązań przewidywanych analizy z klastrów HDInsight w trzech kroków:

1. [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md): to konto miejsca do magazynowania jest używane do przechowywania danych w magazynie obiektów Blob platformy Azure. Dane używane w klastrów HDInsight znajduje się poniżej.

2. [Dostosowywanie Azure HDInsight Hadoop klastrów w celu nauki danych](machine-learning-data-science-customize-hadoop-cluster.md): w tym kroku tworzy klaster Azure HDInsight Hadoop z 2.7 Python Anaconda 64-bitowej zainstalowany na wszystkich węzłach. Istnieją dwie ważne czynności (opisane w tym temacie) do wykonania podczas dostosowywania klaster HDInsight.

    * Należy połączyć utworzony w kroku 1 z klaster HDInsight po utworzeniu konta miejsca do magazynowania. To konto magazynu jest używana do uzyskiwania dostępu do danych, które mogą być przetwarzane w klastrze.

    * Należy włączyć dostęp zdalny do głowy węzła klaster po jego utworzeniu. Zapamiętaj poświadczenia dostępu zdalnego określone w tym miejscu (inny niż określone dla klastrów podczas jej tworzenia): potrzebne do wykonania poniższych procedur.

3. [Tworzenie obszaru roboczego Azure ML](machine-learning-create-workspace.md): nauki ten komputer Azure obszaru roboczego służy do tworzenia modeli nauki komputera, po badań danych początkowych i w dół przy próbkowaniu w klastrze HDInsight.

## <a name="getdata"></a>Pobieranie i używanie danych ze źródła publiczne

Zestaw danych [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) są dostępne, klikając łącze, akceptując warunki użytkowania i podanie nazwy. Migawki to wygląda jak pokazano poniżej:

![Zaakceptuj postanowienia dotyczące Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Kliknij przycisk **Kontynuuj, aby pobrać** dowiedzieć się więcej o zestawu danych i ich dostępność.

Dane znajdują się w miejscu publicznym [Azure blob miejsca do magazynowania](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" odwołuje się do lokalizacji przechowywania obiektów Blob platformy Azure. 

1. Dane w tym magazyn obiektów blob publicznej składa się z trzech podfoldery rozpakowane danych.

    1. Podfolder *nieprzetworzonych Statystyka-* zawiera pierwszych 21 dni danych — od dnia\_00 do dnia\_20
    2. Podfolder *nieprzetworzonych pociągu-* składa się z jednego dnia danych, dnia\_21
    3. Podfolder *nieprzetworzonych/testowanie-* składa się z dwóch dni danych, dnia\_22 i dzień\_23

2. Dla osób, którym chcesz rozpocząć od gzip nieprzetworzonych danych, są również dostępne w folderze głównym *nieprzetworzonych-* jako day_NN.gz, gdzie NN przechodzi od 00 do 23.

Alternatywne podejście do dostępu, zapoznaj się, a model, który te dane, które nie wymagają wszystkie pliki lokalne jest opisano w dalszej części tego instruktażu, gdy tworzymy gałęzi tabel.

## <a name="login"></a>Zaloguj się do headnode klaster

Aby zalogować się do headnode klastrze, zlokalizuj klaster przy użyciu [Azure portal](https://ms.portal.azure.com) . Kliknij ikonę Słoń HDInsight po lewej stronie, a następnie kliknij dwukrotnie nazwę klaster. Przejdź na kartę **Konfiguracja** , kliknij dwukrotnie ikonę POŁĄCZ u dołu strony i wprowadź poświadczenia dostępu zdalnego po wyświetleniu monitu. Powoduje przejście do headnode klastrze.

Oto typowego dziennika pierwszego w headnode klaster wygląda tak:

![Zaloguj się do klastrów](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Po lewej stronie widzimy "Hadoop wiersza polecenia", czyli naszych najważniejszą metodą roboczą badań danych. Widzimy również dwóch przydatnych adresy URL - "Hadoop przędzy stan" i "Hadoop nazwę węzła". Adres URL stanu przędzy przedstawiające postęp zadania i nazwa adresu URL węzeł przedstawiono szczegółowe informacje na konfiguracji klaster.

Teraz możemy jest skonfigurowana i gotowa do rozpoczęcia pierwsza część Instruktaż: badań danych za pomocą gałęzi i przygotowanie do nauki maszynowego Azure danych.

## <a name="hive-db-tables"></a>Tworzenie bazy danych gałęzi i tabel

Tworzenie tabel gałąź dla naszych Criteo zestawu danych, otwórz ***Wiersz polecenia Hadoop*** na pulpicie węzła głównego i wprowadź katalog gałęzi wpisując polecenie

    cd %hive_home%\bin

>[AZURE.NOTE] Uruchom wszystkie polecenia gałęzi w tym instruktażu z Kosza gałęzi i monit katalogu. To przejście istotnych problemów ścieżkę automatycznie. Firma Microsoft korzysta z warunków "Gałęzi katalogu monit", "pojemnika gałęzi-katalogu monit" i "wiersza polecenia Hadoop" są używane zamiennie.

>[AZURE.NOTE]  Aby wykonać dowolną kwerendę gałęzi, jedną zawsze używaj następujących poleceń:

        cd %hive_home%\bin
        hive

Po wyświetleniu REPL gałęzi z "gałęzi >"Zaloguj, po prostu wycinanie i wklejanie zapytania, aby ją wykonać.

Poniższy kod tworzy bazę danych "criteo", a następnie generuje 4 tabel:


* *Tabela do wygenerowania liczby* utworzoną w dniu dni\_00 do dnia\_20,
* *w tabeli jako pociągu zestawu danych* oparty na dzień\_21, i
* dwóch *tabel za pomocą jako test zestawy danych* oparty na dzień\_22 i dzień\_23 odpowiednio.

Nasze test zestawu danych Firma Microsoft podzielić na dwie różne tabele, ponieważ jeden z tych dni przypada święto i chcemy określić modelu można wykryć różnice między świąt i nie święta z stawki liczbą kliknięć.

Skrypt [& #95 przykładowe; gałęzi & #95; tworzenie & #95; criteo & #95; & #95; bazy danych i & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) jest wyświetlany poniżej dla wygody:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Firma Microsoft Zauważ, że te tabele są zewnętrznych, jak możemy po prostu wskaż lokalizacje magazyn obiektów Blob platformy Azure (wasb).

**Istnieją dwa sposoby na wykonywanie gałęzi dowolny kwerendę, którą teraz wspomnieć.**

1. **Za pomocą wiersza polecenia REPL gałęzi**: pierwszego ma problemu polecenie "gałęzi" i Kopiuj i Wklej kwerendę z wiersza polecenia REPL gałęzi. Aby to zrobić, należy wykonać:

        cd %hive_home%\bin
        hive

    Teraz w REPL wiersza polecenia, wycinanie i wklejanie kwerendy wykonuje go.

2. **Zapisywanie kwerendy do pliku i wykonywania polecenia**: drugi jest zapisanie kwerendy do pliku .hql ([& #95 przykładowe; gałęzi & #95; tworzenie & #95; criteo & #95; & #95; bazy danych i & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)), a następnie Wydaj następujące polecenie, aby wykonać kwerendę:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Potwierdzanie tworzenia bazy danych i tabeli

Następnie potwierdzamy utworzenia bazy danych przy użyciu następującego polecenia z pojemnika gałęzi i monit katalogu:

        hive -e "show databases;"

Dzięki temu:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Tworzenie nowej bazy danych "criteo" to potwierdzenie.

Aby wyświetlić tabele utworzone, możemy po prostu wydaj polecenie tutaj z pojemnika gałęzi i monit katalogu:

        hive -e "show tables in criteo;"

Następnie widzimy następujący wynik:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Poszukiwanie danych w gałęzi

Teraz możemy przystąpić do niektórych badań podstawowych danych w gałęzi. Firma Microsoft rozpoczynają się od liczby przykładach w pociągu i testowanie tabel danych.

### <a name="number-of-train-examples"></a>Wiele przykładów pociągu

Zawartość [& #95 przykładowe; gałęzi & #95; liczba & #95; pociągu & #95; tabeli & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) przedstawiono poniżej:

        SELECT COUNT(*) FROM criteo.criteo_train;

Daje:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternatywnie jedną może także wydać następujące polecenie z pojemnika gałęzi i monit katalogu:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Liczba przykłady test w dwóch zestawów danych testowych

Firma Microsoft teraz policzyć przykładów w dwóch zestawów danych test. Zawartość [& #95 przykładowe gałęzi & #95 Statystyka & #95; criteo & #95; testowych & #95; dzień & #95; 22 & #95; tabeli & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) się tu:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Daje:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Jak zwykle, może również nazywamy skrypt z pojemnika gałęzi / katalogu monit przez wydanie polecenia:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Na koniec możemy Sprawdź liczbę przykłady test w zestawie danych test według dnia\_23.

Polecenie w tym celu jest podobny do tego, po prostu wyświetlana (zobacz [Przykładowy & #95; gałęzi & #95; liczba & #95; criteo & #95; test & #95; dzień & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Dzięki temu:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Rozkład etykiety w zestawie danych pociągu

Rozkład etykiety w zestawie danych pociągu jest zainteresowania. Aby wyświetlić, możemy wyświetlić zawartość [Przykładowy & #95; gałęzi & #95; criteo & #95; & #95 etykiety; rozkładu & #95; pociągu & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

To daje rozkład etykiet:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Należy zauważyć, że wartość procentową dodatnie etykiet około 3,3% (zgodne z oryginalnym zestawu danych).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram podziału niektórych zmiennych liczbowych w zestawie danych pociągu

Firma Microsoft korzysta z macierzystego gałęzi "histogram\_liczbowe" funkcji, aby dowiedzieć się, jak wygląda rozkład zmiennych liczbowych. Poniżej przedstawiono zawartość [próbki & #95; gałęzi & #95; criteo & #95; histogramu & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Daje następujące czynności:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

BOCZNE widok — rozłożenie kombinację gałęzi służy do generowania danych wyjściowych przypominających SQL zamiast zwykłych listy. Należy zauważyć, że w tej tabeli, pierwszą kolumnę odpowiada Centrum Kosz i drugi częstotliwości Kosz.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Przybliżona percentylu niektórych zmiennych liczbowych w zestawie danych pociągu

Zainteresowania przy użyciu zmiennych liczbowych jest również do obliczenia przybliżonego percentylu. Gałąź jest natywnych "percentyl\_w przybliżeniu" powinien się tym zająć dla nas. Zawartość [& #95 przykładowe; gałęzi #95; criteo & #95; przybliżonego & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) są:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Daje:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Firma Microsoft Oznacz, że rozkład percentylu jest ściśle związana z rozkładem histogramu dowolnej zmiennej liczbowe zazwyczaj.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Znajdź numer wartości unikatowe dla niektórych kategorii kolumn w zestawie danych pociągu

Kontynuowanie badań danych, teraz okaże się, w przypadku niektórych kategorii kolumn, liczba unikatowych wartości, które przyjmować. Aby to zrobić, możemy wyświetlić zawartość [& #95 przykładowe; gałęzi #95; criteo & #95; unikatowe & #95; wartości & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Daje:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Firma Microsoft Zwróć uwagę, że Col15 zawiera wartości unikatowe 19M! Przy użyciu prostego technik, takich jak "hot jeden kodowanie" do kodowania takich wymiarowe wysoki zmiennych kategorii jest praktyce. W szczególności możemy wyjaśnić i wykazać techniką zaawansowanych, niezawodne o nazwie [Nauki z zlicza](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) dla efektywne rozwiązania problemu ten problem.

W tej sekcji podrzędny możemy zakończyć, wyświetlając pod numerem wartości unikatowe dla niektórych innych kategorii kolumnach. Zawartość [& #95 przykładowe; gałęzi #95; criteo & #95; unikatowe & #95; & #95 wartości wielokrotności & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) są:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Daje:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Ponownie widać, że z wyjątkiem Col20, wszystkie pozostałe kolumny mają wiele wartości unikatowe.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Współtworzenie wystąpienie zlicza par kategorii zmiennych w zestawie danych pociągu

Współtworzenie wystąpienie liczby pary zmiennych kategorii jest również zainteresowania. Można to ustalić za pomocą kodu w [& #95 przykładowe; gałęzi #95; criteo & #95; iloczynów & #95; kategorii & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Firma Microsoft odwrotnej Uporządkuj liczby elementów według ich wystąpienia i Szukaj u góry 15 w tym przypadku. Dzięki temu z nami:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Przykładowe zestawy danych szkoleniowe maszynowego Azure w dół

Masz zbadane zestawy danych i wykazać, firma Microsoft może jak tego typu badań dla zmiennych (łącznie kombinacje), firma Microsoft teraz w dół przykładowe zestawy danych tak, aby ułatwi nam tworzenie modeli w Azure maszynowego uczenia. Odwoływanie będącej problem, możemy skoncentrować się na: nadać zestaw atrybutów przykład (funkcja wartości z Kol2 - Col40), firma Microsoft przewidywanie Jeśli Kol1 jest wartość 0 (nie ma możliwości kliknięcia) lub 1 (kliknij).

Strzałka w dół przykładowe naszych pociągu i przetestować zestawy danych 1% oryginalnego rozmiaru, możemy funkcja gałęzi natywnych RAND(). Następny skrypt [Przykładowy & #95; gałęzi & #95; criteo & #95; próbkowanie & #95; pociągu & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) powinien się tym zająć dla zestawu danych pociągu:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Daje:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Skrypt [Przykładowy & #95; gałęzi & #95; criteo & #95; próbkowanie & #95; testowych & #95; dzień & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) oznacza dla testowymi danymi dnia\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Daje:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Na koniec skryptu [Przykładowy & #95; gałęzi & #95; criteo & #95; próbkowanie & #95; test & #95; dzień & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) oznacza danych test dnia\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Daje:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Dzięki temu możemy przystąpić do naszych dół pociągu próbki i testów zestawów danych do tworzenia modeli w Azure maszynowego uczenia.

Przed możemy przejść do nauki maszynowego Azure, czyli dotyczy tabeli liczba jest składnik ważne ostateczne. W następnej sekcji podrzędny omówimy to niektóre szczegóły.

##<a name="count"></a>Krótki dyskusji w tabeli liczba

Firma Microsoft pokazano wiele zmiennych kategorii mają wymiarze bardzo wysoki. W naszym przykładzie firma Microsoft przedstawia techniką zaawansowanych o nazwie [Nauki z zlicza](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) do kodowania zmienne w sposób wydajne, efektywne. Więcej informacji na temat tej techniki jest łącza podanego.

**Uwaga:** W tym instruktażu możemy skupić się na przy użyciu tabel Statystyka da niewielkie reprezentacje wymiarowe wysoki funkcje kategorii. Nie jest jedynym sposobem kodowanie kategorii funkcji; Aby uzyskać więcej informacji na temat innych metod zainteresowanych użytkowników zapoznaj się [z nich hot kodowanie](http://en.wikipedia.org/wiki/One-hot) i [funkcji mieszania](http://en.wikipedia.org/wiki/Feature_hashing).

Do utworzenia liczba tabel danych Liczba, firma Microsoft korzysta z danych w folderze nieprzetworzonych statystykę. W sekcji modelowania możemy wyjaśnianie użytkownikom, jak tworzyć w poniższych tabelach liczba kategorii funkcji od podstaw, lub być tabeli gotowych liczba ich explorations. W jakich następujące po określane "Liczba gotowych tabele", firma Microsoft oznacza przy użyciu tabel liczby, które udostępniamy. Szczegółowe informacje na temat uzyskiwania dostępu w poniższych tabelach znajdują się w następnej sekcji.

## <a name="aml"></a>Tworzenie modelu z nauki maszynowego Azure

Naszego modelu tworzenia procesu w Azure maszynowego uczenia obejmuje następujące kroki:

1. [Pobieranie danych z tabel gałęzi do nauki maszynowego Azure](#step1)
2. [Tworzenie doświadczenia: oczyścić danych, wybierz uczeń i featurize z tabelami liczba](#step2)
3. [Przeszkolenie modelu](#step3)
4. [Wynik modelu danych testowych](#step4)
5. [Szacowanie modelu](#step5)
6. [Publikowanie modelu jako usługi sieci web ma zostać zużyta](#step6)

Teraz możemy przystąpić do tworzenia modeli w studio Azure maszynowego uczenia. Nasze szczegółów próbki dane zostaną zapisane jako tabele gałęzi w klastrze. Firma Microsoft korzysta z modułu Azure maszynowego uczenia **Importowanie danych** do odczytu tych danych. Poświadczenia, aby uzyskać dostęp do konta miejsca do magazynowania klaster znajdują się w poniżej.

### <a name="step1"></a>Krok 1: Pobieranie danych z tabel gałęzi do nauki maszynowego Azure za pomocą modułu importowanie danych i zaznacz go na komputerze nauki doświadczenia

Rozpoczęcie, wybierając pozycję **+ Nowe** -> **doświadczenia** -> **Doświadczenia puste**. Następnie w polu **wyszukiwania** w lewym górnym rogu Wyszukaj "Importowanie danych". Przeciągnij i upuść moduł **Importowanie danych** do obszaru roboczego doświadczenia (środkowej części ekranu) jest użycie modułu uzyskać dostęp do danych.

Oto jak wygląda **Importowanie danych** podczas pobierania danych z tabeli gałęzi:

![Importowanie danych pobiera dane](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Moduł **Importowanie danych** wartości parametrów, które znajdują się w grafice są tylko przykłady sortowania wartości, które należy podać. Poniżej podano pewne ogólne wskazówki na temat wypełniania parametr ustawiony dla modułu **Importowanie danych** .

1. Wybierz pozycję "Kwerenda gałęzi" dla **Źródła danych**
2. W polu **gałęzi zapytania bazy danych** , prosta instrukcja SELECT * od < do\_bazy danych\_name.your\_tabeli\_nazwa > — wystarcza.
3. **Identyfikator URI serwera Hcatalog**: Jeśli klaster jest "abc", a następnie jest to po prostu: https://abc.azurehdinsight.net
4. **Nazwa konta użytkownika Hadoop**: nazwa użytkownika wybrany w momencie oddanie klaster. (Nie dostęp zdalny do użytkownika nazwa!)
5. **Hasło do konta użytkownika Hadoop**: hasło dla nazwy użytkownika, wybrany w momencie oddanie klaster. (Nie hasło dostępu zdalnego!)
6. **Lokalizację danych wyjściowych**: wybierz pozycję "Azure"
7. **Nazwę konta magazynu platformy Azure**: konto miejsca do magazynowania skojarzone z klastrem
8. **Azure magazynowania klucz konta**: klucz konta magazynu skojarzone z klastrem.
9. **Nazwa kontenera Azure**: Jeśli nazwa klaster jest "abc", a następnie po prostu "abc", jest to zazwyczaj.


Po zakończeniu **Importowania danych** wprowadzenie danych (wyświetlony zielony osi w Module) zapisać te dane jako zestaw danych (o nazwie wybranych przez użytkownika). Co to wygląda:

![Importowanie danych zapisywanie danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Kliknij prawym przyciskiem myszy port wyjściowy modułu **Importowanie danych** . To powoduje wyświetlenie opcję **Zapisz jako zestaw danych** i **Wizualizacja** . Po kliknięciu opcji **Wizualizacja** Wyświetla 100 wierszy danych, wraz z prawym panelu, które są przydatne w przypadku niektórych statystyk podsumowania. Aby zapisać dane, wybierz polecenie **Zapisz jako zestaw danych** i postępuj zgodnie z instrukcjami.

Aby zaznaczyć zapisanego zestawu danych do użycia w eksperyment nauki komputera, zlokalizuj zestawy danych, korzystając z pola **wyszukiwania** pokazano na poniższym rysunku. Wystarczy wpisać się nazwa nadana zestawu danych częściowo do dostępu i przeciągnij panelu głównym zestawu danych. Upuszczenie go na panelu głównym zaznacza go do użytku w maszynowego uczenia modelowania.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] W tym zarówno pociągu i zestawy danych test. Ponadto Pamiętaj, aby użyć nazwy bazy danych i nazwy tabel, których można użyć w tym celu. Wartości używane na rysunku są przeznaczone wyłącznie dla ilustracji purposes.* *

### <a name="step2"></a>Krok 2: Tworzenie prostego doświadczenia w Azure maszynowego uczenia przewidywanie kliknięć-nie kliknięcia

Nasze doświadczenia Azure ML wygląda następująco:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Teraz możemy zbadać kluczowe składniki tego doświadczenia. Jako przypomnienie trzeba przeciągnij naszych zapisanego pociągu i najpierw sprawdź zestawy danych było naszych Kanwa doświadczenia.

#### <a name="clean-missing-data"></a>Oczyszczanie brakujących danych

Moduł **Oczyść brakujące dane** czy sugeruje nazwa: go czyści brakujących danych w sposób, który może być zdefiniowane przez użytkownika. Wyszukiwania w module, widzimy następująco:

![Oczyść brakujących danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

W tym miejscu Wybraliśmy zamienić wszystkie brakujące wartości 0. Istnieją inne opcje zostaną także, które są wyświetlane sprawdzając list rozwijanych w module.

#### <a name="feature-engineering-on-the-data"></a>Funkcja techniczny danych.

Może być miliony wartości unikatowe w przypadku niektórych kategorii funkcji dużych zestawów danych. Za pomocą prostego metod, takich jak hot jeden kodowanie odpowiadające takich wymiarowe wysoki kategorii funkcji jest całkowicie praktyce. W tym instruktażu możemy pokazano sposób korzystać z funkcji count przy użyciu wbudowanych modułów Azure maszynowego uczenia do generowania niewielkie reprezentacje zmienne kategorii wymiarowe wysoki. Wynik końcowy jest mniejszy rozmiar modelu, osiągać krótsze czasy szkolenia i wskaźniki dość porównywalna do korzystania z innych technik.

##### <a name="building-counting-transforms"></a>Tworzenie, licząc przekształceń

Aby utworzyć funkcji count, firma Microsoft korzysta z **Tworzenie zliczania Przekształcanie** modułu, który jest dostępny w Azure maszynowego uczenia. Moduł wygląda następująco:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Uwaga** : W polu **Liczba kolumn** możemy wprowadź tych kolumn, które możemy chcesz wykonać liczników na. Zazwyczaj są to (wymienione) wymiarowe wysoki kategorii kolumn. Na początku, wspomniano, że Criteo zestawu danych zawiera kolumny kategorii 26: od Col15 do Col40. Tutaj możemy zliczanie na każdy z nich i ich wskaźników (od 15 do 40, rozdzielając je przecinkami, jak pokazano).

Aby użyć modułu w trybie MapReduce (odpowiednie dla dużych zestawów danych), potrzebujemy dostęp do klastrów HDInsight Hadoop (używane do badań funkcji może nastąpić w tym celu także) i swoje poświadczenia. Poprzedniej ilustracji przedstawiono wypełnionymi wartości, jak wygląda (zamiast wartości podane jako ilustracja z tymi, które są odpowiednie dla własnego przypadków użycia).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Na powyższym rysunku zostanie wyświetlona jak wprowadź lokalizację wprowadzania obiektów blob. Tej lokalizacji ma zarezerwowane dla liczby tabel w oparciu o dane.


Po zakończeniu tego modułu przekształcenie dla można zapisać później, klikając prawym przyciskiem myszy folder i wybierając opcję **Zapisz jako przekształcenie** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

W naszych architektury doświadczenia, jak pokazano powyżej zestaw danych "ytransform2" odpowiada dokładnie przekształcenie zapisanego Statystyka. Do końca tego doświadczenia przyjęto założenie, użyto moduł **Tworzenie zliczania Przekształcanie** na niektóre dane w celu wyświetlenia liczby czytelnika, a następnie można użyć tych liczników do generowania funkcji count w pociągu i przetestować zestawy danych.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Wybieranie, jakie Statystyka funkcje dołączyć jako część pociągu i badania zestawy danych

Gdy mamy już zestawienia Przekształcanie gotowe, użytkownik może określić funkcji obejmować ich pociągu i przetestować zestawy danych za pomocą modułu **Modyfikowanie parametrów tabeli Statystyka** . Ten moduł pokażemy, po prostu tutaj kompletności, ale w celu uproszczenia nie rzeczywiście używać go w naszym doświadczeniu.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

W tym przypadku jako są widoczne, możemy wybrano do używania tylko prawdopodobieństwo dziennika i ignorowanie wycofywania kolumny. Firma Microsoft można także ustawić parametrów takich jak próg pojemnika śmieci, ile artykule poprzednich przykłady, aby dodać wygładzanie, albo użyć dowolnej hałasu Laplacian, czy nie. Wszystkie te są zaawansowane funkcje i zauważyć, że wartości domyślne to dobry punkt wyjścia dla użytkowników, którzy nowych funkcji Generowanie na tego typu.

##### <a name="data-transformation-before-generating-the-count-features"></a>Przekształcenie danych przed wygenerowaniem funkcji count

Teraz możemy skoncentrować się na punkt ważne informacje o przekształcaniu naszych pociągu i testowanie danych przed wygenerowaniem faktycznie funkcji ile.liczb. Należy zauważyć, że istnieją dwa moduły **Wykonywanie skryptów R** używany przed stosujemy przekształcenie Statystyka naszych danych.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Oto pierwszego skryptu R:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


W tym polu skrypt R firma Microsoft Zmień nazwy naszych kolumn na nazwy "Kol1" do "Col40". Jest to spowodowane przekształcenie Statystyka oczekuje nazwy w tym formacie.

W drugim skryptu R, możemy saldo podziału między klas dodatnie i ujemne (klasy 1 i 0 odpowiednio) przy próbkowaniu ujemne zajęć. Skrypt R poniżej pokazano, jak to zrobić:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

W tym prosty skrypt R, firma Microsoft korzysta z "punktu sprzedaży\_minus\_stosunek" do ustawiania wielkości równowagi między dodatnie i ujemne klasy. Jest to ważne ponieważ poprawy równowagi klasy zazwyczaj zawiera wydajność dla problemów klasyfikacji w przypadku rozkład zajęć pochylony (odwołanie w naszym przypadku mając klasy dodatnia 3,3% i klasy ujemne 96.7%).

##### <a name="applying-the-count-transformation-on-our-data"></a>Stosowanie przekształcenia zliczanie w naszych danych

Na koniec firma Microsoft może być moduł **Stosowanie transformacji** stosowanie przekształceń Statystyka na naszych pociągu i przetestować zestawy danych. Moduł ten trwa Przekształcanie Statystyka zapisany jako jedno wejście i test lub pociągu zestawy danych jako danych wejściowych i zwraca dane za pomocą funkcji ile.liczb. Przedstawiono poniżej:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Wyglądać fragment z funkcji count

Jest istotne, aby zobaczyć, jak wyglądają funkcji count w naszym przypadku. Poniżej pokazano fragment następująco:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

W tym fragment pokazano, że dla kolumn, które udostępniamy liczone na, możemy dostęp liczby elementów i logowania prawdopodobieństwo oprócz wszelkie odpowiednie backoffs.

Teraz możemy przystąpić do tworzenia modelu nauki maszynowego Azure za pomocą tych przekształconych zestawy danych. W następnej sekcji pokazano, jak to zrobić.

#### <a name="azure-machine-learning-model-building"></a>Tworzenie modelu maszynowego uczenia Azure

##### <a name="choice-of-learner"></a>Wybór uczeń

Najpierw należy wybrać uczeń. Firma Microsoft zamiar jako naszych uczeń za pomocą dwóch klasy zwiększane drzewa decyzji. Poniżej przedstawiono opcji domyślnych dla tego uczeń:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Dla naszych doświadczenia chwilę o wybranie wartości domyślnej. Firma Microsoft Zauważ, że ustawienia domyślne są zwykle zrozumiałej i dobrym sposobem na uzyskiwanie szybkiego plany bazowe na wydajność. Aby poprawić na wydajność, należy zmiennymi parametry, jeśli wybierzesz po zapisaniu planu bazowego.

#### <a name="train-the-model"></a>Przeszkolenie modelu

Szkolenia, możemy po prostu wywołać moduł **Modelu pociągu** . Dwóch danych wejściowych do niego są uczeń drzewa decyzji zwiększane dwóch zajęć i naszych dataset pociągu. To jest następujący:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Wynik modelu

Po mamy modelu przeszkolony możemy przystąpić do wyniku w zestawie danych testowych i do oceny ich działanie. Firma Microsoft to zrobić przy użyciu modułu **Modelu wynik** pokazano na poniższym rysunku, wraz z modułu **Oceny modelu** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Krok 5: Oceny modelu

Na koniec chcemy analizowanie wydajności modelu. Zwykle dwa problemy (binarne) klasyfikacji zajęć, miara warto jest AUC. To wizualizować, możemy nawiązać połączenie moduł **Modelu wynik** moduł **Oceny modelu** dla tego. Kliknięcie **Wizualizacja** w module **Modelu ocenianie** daje grafikę podobny do następującego:

![Ocena modelu BDT modułu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

W postaci dwójkowej (lub dwie klasy) klasyfikacji problemy, warto miarę dokładności przewidywania jest obszar w obszarze krzywej (AUC). W poniżej zostanie wyświetlona naszych wyników przy użyciu tego modelu w zestawie danych naszych test. Aby uzyskać dostęp do tej, kliknij prawym przyciskiem myszy port wyjściowy modułu **Modelu oceny** , a następnie **Wizualizacja**.

![Wizualizowanie moduł oceny modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Krok 6: Publikowanie modelu jako usługi sieci Web
Publikowanie modelu Azure maszynowego uczenia jako usługi sieci web z co najmniej problemów jest funkcją przydatne dokonywania powszechnie dostępny. Po wykonaniu tej operacji każdy nawiązywania połączeń z usługi sieci web z wprowadzania dane są potrzebne przewidywań dla, a usługa sieci web komunikuje modelu zwraca te przewidywań.

Aby to zrobić, możemy najpierw zapisać naszego przeszkolony modelu jako obiekt przeszkolony modelu. Jest to, klikając modułu **Modelu pociągu** i za pomocą opcji **Zapisz jako przeszkolony modelu** .

Następnie należy utworzyć dane wejściowe i wyjściowe porty nasze usługi sieci web:

* port wejściowy pobiera dane w tym samym formularzu jako dane, które administrator powinien przewidywań dla
* port wyjściowy zwraca uzyskał etykiet i prawdopodobieństwa.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Zaznaczanie kilku wierszy danych wejściowych portu

Rozwiązaniem jest moduł **Stosowanie transformacji SQL** umożliwia wybranie tylko 10 wierszy służyć jako danych wejściowych portu. Zaznacz tylko te wiersze danych dla naszych port wejściowy przy użyciu kwerendy SQL pokazano poniżej:

![Port wprowadzania danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Usługa sieci Web
Teraz możemy przystąpić do uruchomienia eksperyment małych, który może być używany do publikowania z naszych usług sieci web.

#### <a name="generate-input-data-for-webservice"></a>Generowanie danych wejściowych dla webservice

Krokiem zeroth od tabeli liczba jest duży, możemy wykonać kilka wierszy danych z badań i wygenerować danych wyjściowych z niego przy użyciu funkcji ile.liczb. To może służyć jako formatu danych wejściowych dla naszych webservice. To jest następujący:

![Tworzenie BDT wprowadzania danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Format danych wejściowych używamy są teraz wynik moduł **Featurizer Statystyka** . Po zakończeniu tego doświadczenia zapisać dane wyjściowe z modułu **Count Featurizer** jako zestaw danych. Tego zestawu danych jest używany do wprowadzania danych w webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Wyników doświadczenia dla webservice publikowania

Najpierw zostanie wyświetlona, tak jak wygląda. Struktura podstawowe jest moduł **Modelu wynik** akceptującym naszych obiekt modelu przeszkolony i kilka wierszy danych wejściowych, wygenerowania w poprzednich krokach za pomocą modułu **Featurizer Statystyka** . "Wybierz kolumny w zestawie danych" Firma Microsoft korzysta z projektem etykiety Scored i prawdopodobieństwa wynik.

![Zaznacz kolumny w zestawie danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Zwróć uwagę, jak moduł **Wybieranie kolumn w zestawie danych** można używać do "odfiltrowywania" danych z zestawu danych. Możemy wyświetlić zawartość poniżej:

![Filtrowanie z Wybieranie kolumn w module zestawu danych](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Aby uzyskać blue dane wejściowe i wyjściowe porty, możesz po prostu kliknij **Przygotowywanie webservice** w prawym dolnym rogu. Uruchamianie tego doświadczenia również umożliwia publikowanie usługi sieci web: kliknij ikonę **Publikowanie usługi sieci WEB** w prawym dolnym rogu, pokazano poniżej:

![Publikowanie usługi sieci Web](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Po opublikowaniu webservice możemy uzyskać przekierowanie do strony, która wygląda tak:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Po lewej stronie widać dwa łącza dla usługi sieci Web:

* **Żądanie/odpowiedź** usługi (lub RR) jest przeznaczony dla jednego przewidywań i jest możemy korzystać w tym workshop.
* Usługa **WSADOWE** (BES) służy do przewidywań partii i wymaga nawiązać przewidywań znajdują się w magazynie obiektów Blob platformy Azure danych wejściowych.

Klikając łącze, które **Żądanie i odpowiedź** przejście z nami do strony, którą daje wstępnie puszkach kod w języku C#, python i R. Kod, jaki można wygodny sposób nawiązywania połączenia, aby usługa sieci Web. Zauważ, że klucz interfejsu API na tej stronie musi być używany do uwierzytelniania.

To wygodny skopiować kod python nad do nowej komórki w notesie IPython.

Tutaj pokazano część kodu python przy użyciu poprawnego klucza interfejsu API.

![Kod Python](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Zwróć uwagę, możemy zastąpić klucz interfejsu API domyślny klucz interfejsu API nasze usługi sieci Web. Klikając polecenie **Uruchom** w tej komórce w notesie IPython daje następującą odpowiedź:

![Odpowiedź IPython](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Widać, że dwa przykłady test, który możemy pytania (w ramach JSON skrypt python), firma Microsoft wrócić odpowiedzi w postaci "Scored etykiet, prawdopodobieństwa Scored". Należy zauważyć, że w tym przypadku Wybraliśmy na domyślne wartości, która zawiera wstępnie puszkach kod (0 dla wszystkich kolumn liczbowych i ciąg "wartość" dla wszystkich kategorii kolumn).

Zakończenie naszych Instruktaż zakończenia do końca przedstawiająca sposób obsługi dużych zestawów danych przy użyciu Azure maszynowego uczenia. Firma Microsoft wprowadzenie terabajtów danych, tworzona w modelu przewidywania i wdrożyć go jako usługi sieci web w chmurze.
