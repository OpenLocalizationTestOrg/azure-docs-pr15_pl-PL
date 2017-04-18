<properties
    pageTitle="Nauka danych przy użyciu Scala i Spark Azure | Microsoft Azure"
    description="Jak używać Scala dla komputera pod nadzorem nauki zadań za pomocą Spark skalowalna MLlib i Spark ML pakiety w klastrze Azure HDInsight Spark."  
    services="machine-learning"
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Nauka danych przy użyciu Scala i Spark Azure

W tym artykule pokazano, jak używać Scala dla komputera pod nadzorem nauki zadań za pomocą Spark skalowalna MLlib i Spark ML pakiety w klastrze Azure HDInsight Spark. Przeprowadzi Cię przez zadania, które stanowią [proces nauki danych](http://aka.ms/datascienceprocess): spożyciu danych i badań, wizualizacji, funkcja techniczny, modelowania i zużycie modelu. Modeli w artykule obejmują regresji logistycznych i liniowy, losowe lasów i drzew zwiększane gradientu (GBTs), oprócz dwa typowe zadania nauki pod nadzorem komputera:

- Problem regresji: przewidywania kwota Porada ($) w podróży taksówki
- Binarny klasyfikacji: przewidywania porada lub brak porady (1-0) w podróży taksówki

Proces modelowania wymaga oceny zestawu danych testowych i wskaźniki odpowiednich dokładność i szkolenia. W tym artykule przedstawiono sposobu przechowywania tych modeli w magazynie obiektów Blob platformy Azure i wynik i oceny ich przewidywanych wydajności. W tym artykule opisano również bardziej zaawansowanych zagadnień jak zoptymalizować modeli przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru. Dane używane są próbki 2013 Warszawa taksówki podróży i taryfy zestawu danych dostępne na GitHub.

[Scala](http://www.scala-lang.org/), języka, w oparciu o środowiska Java integruje pojęcia języka obiektowych i funkcjonalne. Jest skalowalna języka, które sprawdzają się przetwarzanie rozproszone w chmurze, a działa na klastrów Azure Spark.

[Spark](http://spark.apache.org/) jest struktura przetwarzania równolegle Otwórz źródło, obsługującego przetwarzania w pamięci, aby zwiększyć wydajność aplikacji analizy danych duży. Aparatu przetwarzania Spark jest przeznaczony dla szybkość łatwość użytkowania i zaawansowanych analiz. Możliwości obliczeń rozłożone w pamięci w iskrowym ułatwiają dobrym rozwiązaniem iteracyjne algorytmów w obliczeniach maszynowego uczenia się i wykres. Pakiet [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) zawiera jednolity zestaw wysokiego poziomu interfejsów API utworzoną na danych ramki, które ułatwiają tworzenie i dostosowywanie praktyczne maszynowego uczenia procesy. [MLlib](http://spark.apache.org/mllib/) jest w iskrowym skalowalna maszynowego uczenia biblioteki, która udostępnia funkcje modelowania tego środowiska rozłożone.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) jest hostowana Azure oferowania Spark Otwórz źródło. Także obsługuje Jupyter Scala notesów w klastrze Spark i może zostać uruchomiony interakcyjnych zapytania Spark SQL Przekształcanie, filtrowania i wizualizowanie danych przechowywanych w magazynie obiektów Blob platformy Azure. Uruchom Scala wstawki kodu programu w tym artykule zawierające rozwiązania Pokaż odpowiednich powierzchni wizualizacji danych w notesach Jupyter zainstalowanym klastrów Spark. Modelowanie czynności opisane w poniższych tematach posiada kodu, którym pokazano, jak przeszkolenie, oceny, zapisywanie i używanie każdego typu modelu.

Kroki konfigurowania i kod w tym artykule dotyczą Azure HDInsight 3.4 Spark 1.6. Jednak kod w niniejszym artykule oraz w [Notesie Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) są ogólne i powinna działać w dowolnym klastrze Spark. Kroki konfiguracji i zarządzania klaster mogą się nieco różnić od tego, co przedstawiono w tym artykule, jeśli nie korzystasz z usługi HDInsight Spark.

> [AZURE.NOTE] Tematu, w którym pokazano, jak używać Python zamiast Scala do wykonania zadania związane z procesem nauki danych end-to-end zobacz [Nauki danych przy użyciu Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Wymagania wstępne

-   Musisz mieć subskrypcję usługi Azure. Jeśli nie masz już jeden [Uzyskiwanie Azure bezpłatna wersja próbna](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Potrzebujesz klaster Azure HDInsight 3.4 Spark 1.6 do wykonania poniższych procedur. Aby utworzyć klaster, należy zapoznać się z instrukcjami w [Rozpoczęcie pracy: tworzenie Spark Apache na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Ustaw typ klaster i wersję z menu **Wybierz typ klaster** .

![Konfiguracja typu klaster HDInsight](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Opis Warszawa taksówki podróży danych i instrukcje dotyczące wykonanie kodu z notesu Jupyter w klastrze Spark zobacz odpowiednie sekcje [Omówienie danych nauki przy użyciu Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Wykonanie kodu Scala z notesu Jupyter w klastrze Spark

Możesz uruchomić notesu Jupyter z portalem Azure. Znajdowanie klaster Spark na pulpicie nawigacyjnym, a następnie kliknij go, aby wprowadzić stronę Zarządzanie klaster. Następnie kliknij pozycję **Klaster pulpitów nawigacyjnych**, a następnie kliknij **Jupyter Notes** , aby otworzyć Notes skojarzone z klastrem Spark.

![Pulpit nawigacyjny klaster i Jupyter notesów](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Możesz również uzyskać dostęp do Jupyter notesów na https://&lt;NazwaKlastra&gt;.azurehdinsight.net/jupyter. Zamień *NazwaKlastra* nazwę klaster. Potrzebne hasło do konta administratora uzyskać dostęp do notesów Jupyter.

![Przejdź do Jupyter notesów przy użyciu nazwy klaster](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Wybierz pozycję **Scala** , aby wyświetlić katalog, który zawiera kilka przykładów paczkowanych notesy, które za pomocą interfejsu API PySpark. Modelowanie badań i punktacja przy użyciu Scala.ipynb Notes, który zawiera przykłady kodu tego pakietu tematy Spark jest dostępna na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Możesz przekazać notesu bezpośrednio z GitHub na serwerze Jupyter notesu w klastrze Spark. Na stronie głównej Jupyter kliknij przycisk **Przekaż** . W Eksploratorze plików Wklej adres URL (zawartość nieprzetworzonych) GitHub Scala notesu, a następnie kliknij przycisk **Otwórz**. Notes Scala jest dostępny pod adresem URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Konfiguracji: Konteksty iskrowym ustawienie wstępne i gałęzi, Spark magics i bibliotek Spark

### <a name="preset-spark-and-hive-contexts"></a>Wstępnie ustawione iskrowym i gałęzi konteksty

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Jądra Spark, które są dostarczane z notesami Jupyter mają wstępnie konteksty. Nie musisz jawnie ustawione Spark lub tworzenia konteksty gałęzi przed rozpoczęciem pracy z aplikacją. Wstępnie ustawionych konteksty są:

- `sc`Aby uzyskać SparkContext
- `sqlContext`Aby uzyskać HiveContext


### <a name="spark-magics"></a>Spark magics

Jądra Spark udostępnia kilka wstępnie zdefiniowanych "magics", które są specjalne polecenia wywołujących z `%%`. Obie te polecenia są używane w następujących przykładach kodu.

- `%%local`Umożliwia określenie lokalnie wykonany kod w kolejnych wierszach. Kod musi być prawidłowy kod Scala.
- `%%sql -o <variable name>`wykonuje kwerenda gałąź `sqlContext`. Jeśli `-o` przekazano parametr, wynik kwerendy jest umieszczany w `%%local` kontekstu Scala jako ramki Spark danych.

Aby uzyskać więcej informacji na temat jądra Jupyter notesów i ich wstępnie zdefiniowanych "magics" który nawiązywanie połączenia z `%%` (na przykład `%%local`), zobacz [jądra dostępne dla Jupyter notesów przy użyciu klastrów HDInsight Spark Linux na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Importowanie biblioteki

Importowanie Spark, MLlib i innych bibliotek, które będą potrzebne przy użyciu następującego kodu.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Spożyciu danych

Pierwszy krok w procesie nauki danych jest mogły zjeść tej ostatniej dane, które chcesz przeanalizować. Możesz zabrać ze sobą danych z zewnętrznych źródeł lub systemów miejsce, w którym znajduje się do środowiska badań i modelowanie danych. W tym artykule dane, które możesz z mogły zjeść tej ostatniej jest połączonych próbki 0,1% taksówki podróży i taryfy pliku (przechowywane jako pliku .tsv). Środowisko badań i modelowanie danych jest Spark. Ta sekcja zawiera kod, aby wykonać następujące serii zadań:

1. Ustawianie ścieżki katalogów do przechowywania danych i modelu.
2. W tym artykule w zestawie danych wejściowych (przechowywane jako pliku .tsv).
3. Definiowanie schemat danych i Usuń dane.
4. Tworzenie ramki oczyszczonych danych i pamięci podręcznej go w pamięci.
5. Rejestrowanie danych jako tabeli tymczasowej SQLContext.
6. Kwerendy tabeli, a następnie zaimportować wyniki do ramki danych.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Ustawianie ścieżki katalogu do lokalizacji przechowywania w magazynie obiektów Blob platformy Azure

Spark pozwoli odczytywać i pisać magazynem obiektów Blob platformy Azure. Można przetwarzać żadnych istniejących danych za pomocą Spark, a następnie ponownie zapisać wyniki w magazynie obiektów Blob.

Aby zapisać modeli lub pliki w magazynie obiektów Blob, musisz poprawnie Określ ścieżkę. Dokumentacja dotycząca domyślnego kontenera dołączone do klastrów Spark przy użyciu ścieżki, który zaczyna się od `wasb:///`. Dokumentacja dotycząca innych lokalizacji za pomocą `wasb://`.

Poniższy przykład kodu Określa lokalizację wprowadzania danych do odczytu i ścieżkę do magazyn obiektów Blob, podłączonego do klastrów Spark zapisywania modelu.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Importowanie danych, tworzenie RDD i definiowanie ramki danych według schematu

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: 8 sekund.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Kwerendy tabeli, a następnie zaimportować wyników w ramce danych

Następnie zapytanie w tabeli taryfy, osób i Porada danych; odfiltrowywania uszkodzony i źródło danych; i drukowanie kilku wierszy.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Wynik:**

fare_amount|passenger_count|tip_amount|Przechylony
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16.0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Poszukiwanie danych i wizualizacji

Po wprowadzenia danych do Spark, następnym krokiem w procesie nauki danych jest uzyskanie szczegółowego zrozumienia danych za pomocą badań i wizualizacji. W tej sekcji możesz sprawdzić dane taksówki za pomocą kwerend SQL. Następnie zaimportować wyniki do ramki danych kreślenia docelowych zmiennych i potencjalnych funkcje kontroli wizualnych za pomocą funkcji automatycznego wizualizacji Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Kreśl dane przy użyciu lokalnie i Magia SQL

Domyślnie dane wyjściowe dowolnego wstawkę kodu uruchamianego z notesu Jupyter jest dostępna w ramach sesji, który jest zachowywane w węzłach pracownika. Jeśli chcesz zapisać wycieczki węzły pracownika dla każdej obliczeń, a jeśli wszystkie dane potrzebne do obliczaniu jest dostępny lokalnie w węźle serwera Jupyter (czyli węzła głównego), możesz użyć `%%local` Magia do uruchomienia na serwerze Jupyter wstawkę kodu.

- **Magia SQL** (`%%sql`). Jądro HDInsight Spark obsługuje łatwe w tekście zapytań HiveQL SQLContext. (`-o VARIABLE_NAME`) Argument będzie nadal występował wyniki kwerendy SQL jako ramki danych Pandas na serwerze Jupyter. Oznacza to, że będzie on dostępny w trybie lokalnym.
- `%%local`**Magia**. `%%local` Magiczną uruchamia kod lokalnie na serwerze Jupyter, czyli węzła głównego klastrze HDInsight. Zazwyczaj `%%local` Magiczna w połączeniu z `%%sql` Magiczna z `-o` parametru. `-o` Parametru czy utrzymują wynik kwerendy SQL lokalnie, a następnie `%%local` Magia spowoduje następnego zestawu wstawkę kodu w celu uruchomienia lokalnie dane wyjściowe zapytania SQL, który jest zachowywane lokalnie.

### <a name="query-the-data-by-using-sql"></a>Kwerendy danych przy użyciu języka SQL
Ta kwerenda pobiera podróży taksówki kwota taryfy, liczba osób i kwota porada.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Poniższy kod `%%local` Magia tworzy ramki danych lokalnych sqlResults. Za pomocą sqlResults kreślenia przy użyciu matplotlib.

> [AZURE.TIP] Magia lokalne jest używany wiele razy w tym artykule. Jeśli zestaw danych jest duży, zobacz przykładowe Tworzenie ramki danych, który można umieścić w lokalnej pamięci.

### <a name="plot-the-data"></a>Kreśl dane

Można wykreślić przy użyciu kodu Python po ramki danych w kontekście lokalnego jako ramki danych Pandas.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Jądra Spark automatycznie spowoduje wizualizację dane wyjściowe zapytania SQL (HiveQL), po uruchomieniu kodu. Można wybrać kilka typów wizualizacji:
 
- Tabela
- Kołowy
- Wiersz
- Obszar
- Pasek

Poniżej przedstawiono kod kreślenia danych:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Wynik:**

![Porada kwota histogramu](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Porada kwota według liczby osób](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Porada kwota kwotą Taryfy](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Tworzenie funkcji Przekształcanie funkcje, a następnie przygotowywanie danych wprowadzeniem do funkcji modelowania

Dla drzewa modelowania funkcji Spark ML i MLlib trzeba przygotować funkcje i docelowej przy użyciu różnych technik, takich jak binning, indeksowania, hot jeden kodowanie i vectorization. Poniżej przedstawiono procedury, aby obserwować w tej sekcji:

1. Utwórz nową funkcję, **binning** godziny do przedziałów czasu ruchu.
2. Stosowanie **indeksowania i hot jednego kodowania** do kategorii funkcji.
3. **Próbki i podziału zestawu danych** do badania i szkolenia ułamki.
4. **Określanie zmiennej szkolenia i funkcje**, następnie utwórz indeksowanych lub hot jeden zakodowany, szkolenia i badania wprowadzania punktu oznaczona etykietą mechanizm distributed zestawów danych (RDDs) lub ramek danych.
5. Automatyczne **kategoryzowanie i vectorize funkcji i elementów docelowych** ma zostać użyte jako danych wejściowych dla komputera nauki modeli.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Utwórz nową funkcję, binning godziny do przedziałów czasu ruchu

Kod pokazano, jak utworzyć nową funkcję o binning godziny do przedziałów czasu ruchu i jak buforować wyniku ramki danych w pamięci. Miejsce, w którym ramki RDDs i dane są używane wielokrotnie, buforowanie prowadzi do ulepszone czasów wykonania. W związku z tym będzie buforować RDDs i ramek danych w kilku etapach poniższych procedur.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indeksowanie i hot jeden kodowanie kategorii funkcji

Modelowania i przewidywania funkcji MLlib wymaga funkcji z indeksowanych lub kodowane przed użyciem kategorii danych wejściowych. W tej sekcji pokazano, jak indeksowanie lub kodowanie kategorii Funkcje związane z wprowadzeniem do funkcji modelowania.

Musisz indeksowanie lub kodowanie modelach na różne sposoby, w zależności od modelu. Na przykład regresji logistycznych i liniowy modele wymagają hot jednego kodowania. Na przykład funkcja z trzech kategorii można rozwinąć na trzy kolumny, funkcja. Każda kolumna zawiera 0 lub 1, w zależności od kategorii obserwacji. MLlib zawiera funkcję [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) hot jednego kodowania. Ten koder mapy kolumny wskaźników etykiety do kolumny wektorów binarny z co najwyżej jednego jedną wartość. Z tego kodowania algorytmów oczekiwać liczbowe wartości funkcji, takich jak regresją, można stosować do kategorii funkcji.

W tym miejscu można przekształcać tylko cztery zmienne do pokazywania przykładowych, które są ciągi znaków. Możesz również indeksowanie innych czynników, takich jak dzień tygodnia, reprezentowany przez wartości liczbowe, jak zmienne kategorii.

Indeksowanie, za pomocą `StringIndexer()`i hot jednego kodowania za pomocą `OneHotEncoder()` funkcji MLlib. Poniżej przedstawiono kod do indeksu i kodowania kategorii funkcje:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: 4 sekundy.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Próbki i podziału zestawu danych do ułamki badania i szkolenia

Kod ten tworzy losową próbki danych (w tym przykładzie 25%). Mimo że przy próbkowaniu nie jest wymagane w tym przykładzie ze względu na rozmiar zestawu danych, artykule pokazano, jak można przykładowe tak, że wiesz, jak go używać własnych problemów w razie potrzeby. W przypadku dużych próbek to zapisać wiele czasu, gdy przeszkolenie modeli. Następnie można podzielić próbki części szkolenia (w tym przykładzie 75%) i testowania części (25%, w tym przykładzie) do użycia w klasyfikacji i modelowania regresji.

Dodaj liczbę losową z zakresu (od 0 do 1) do każdego wiersza (w kolumnie "LOS"), który można wybrać zgięcia krzyżowego sprawdzania poprawności podczas szkolenia.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: dwie sekundy.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Określanie zmiennej szkolenia i funkcje, a następnie utwórz indeksowanych lub hot jeden zakodowany szkoleń i testowania wprowadzania etykietą RDDs punkt lub danych ramek

Ta sekcja zawiera kod, którego pokazano, jak indeks danych tekstowych kategorii jako punkt oznaczona etykietą typ danych i kodowania jej, aby móc używać go przeszkolenie i przetestować regresją MLlib i innych modeli klasyfikacji. Punkt oznaczona etykietą obiekty są RDDs, które zostały sformatowane w sposób, który jest potrzebny najczęściej maszynowego uczenia algorytmy MLlib jako danych wejściowych. [Etykietą punktu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) jest lokalne wektorowa gęstości lub rzadkie, skojarzone z etykiety i odpowiedzi.

W tym kodzie Określ zmienną (zależnych) docelową i funkcje za pomocą przeszkolenie modeli. Następnie możesz utworzyć indeksowanych lub hot jeden zakodowany szkoleń i testowania wprowadzania etykietą ramki RDDs punkt lub danych.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: 4 sekundy.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatyczne kategoryzowanie i vectorize funkcji i elementów docelowych, aby użyć jako danych wejściowych dla maszynowego uczenia modeli

Za pomocą Spark ML docelowej i funkcje, które można używać w funkcji drzewa modelowania Kategoryzuj. Kod wykonuje dwa zadania:

-   Tworzy binarne docelowej klasyfikacji przypisując wartość 0 lub 1 do każdego punktu danych od 0 do 1 przy użyciu wartość progowa liczby 0,5.
- Automatycznie kategoryzuje funkcje. Jeśli liczba różnych wartości liczbowe dla dowolnej funkcji jest mniejsza niż 32, została sklasyfikowana tej funkcji.

Poniżej przedstawiono kod tych dwóch zadań.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Model klasyfikacji binarne: przewidywanie, czy należy zwrócić Porada na ten temat

W tej sekcji możesz tworzyć trzy typy modeli dwójkową klasyfikacji do prognozowania, czy należy zwrócić Porada na ten temat:

- **Model regresją** przy użyciu Spark ML `LogisticRegression()` funkcji
- **Model klasyfikacji las losową** przy użyciu Spark ML `RandomForestClassifier()` funkcji
- **Gradient zwiększający model klasyfikacji drzewa** za pomocą MLlib `GradientBoostedTrees()` funkcji

### <a name="create-a-logistic-regression-model"></a>Tworzenie modelu regresją

Następnie utwórz model regresją przy użyciu Spark ML `LogisticRegression()` funkcji. Tworzenie modelu tworzenia kodu w pewne czynności:

1. Ustawianie **pociągu modelu** danych za pomocą jednego parametru.
2. **Szacowanie modelu** zestawu danych test z metryki.
3. **Zapisz model** w magazynie obiektów Blob zużycia przyszłych.
4. **Wynik modelu** danych test.
5. **Kreśl wyniki** z słuchawka operacyjnym krzywe cechy (ROC).

Oto kod poniższe procedury:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Ładowanie, wynik i zapisać wyniki.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Wynik:**

ROC na testowymi danymi = 0.9827381497557599


Umożliwia Python na lokalnym ramek danych Pandas wykreślić krzywą ROC.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Wynik:**

![Porada lub nie krzywej ROC Porada](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Utwórz model klasyfikacji losowe las

Następnie utworzyć model klasyfikacji las losową przy użyciu Spark ML `RandomForestClassifier()` działać, a następnie Ocena modelu danych test.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Wynik:**

ROC na testowymi danymi = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Utwórz model klasyfikacji GBT

Następnie utworzyć model klasyfikacji GBT przy użyciu MLlib w `GradientBoostedTrees()` działać, a następnie Ocena modelu danych testowych.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Wynik:**

Obszar pod krzywą ROC: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Model regresji: przewidywanie kwota Porada

W tej sekcji można tworzyć dwa typy modeli regresji przewidzieć wielkość Porada:

- **Umorzyć modelu regresji liniowej** przy użyciu Spark ML `LinearRegression()` funkcji. Można zapisać model i ocena modelu danych test.
- **Zwiększenia gradient drzewa modelu regresji** przy użyciu Spark ML `GBTRegressor()` funkcji.


### <a name="create-a-regularized-linear-regression-model"></a>Tworzenie modelu umorzyć regresji liniowej.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: 13 sekund.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Wynik:**

R-sqr na testowymi danymi = 0.5960320470835743


Następnie wyniki testu kwerendy jako ramki danych i wizualizowanie go przy użyciu AutoVizWidget i matplotlib.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Kod tworzy ramkę dane lokalne z wyników kwerendy i zawiera dane. `%%local` Magia tworzy ramkę dane lokalne `sqlResults`, którego może wykreślić z matplotlib.

>[AZURE.NOTE] Ten Magia Spark jest używany wiele razy w tym artykule. W przypadku dużej ilości danych, należy przykładowych Tworzenie ramki danych, który można umieścić w lokalnej pamięci.

Tworzenie wykresów za pomocą Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Wynik:**

![Porada kwota: rzeczywiste a przewidywane](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Tworzenie modelu regresji GBT

Tworzenie modelu regresji GBT przy użyciu Spark ML `GBTRegressor()` działać, a następnie Ocena modelu danych testowych.

[Drzewa zwiększane gradientu](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) są komplety drzewa decyzji. GBTs przeszkolenie algorytmów wielokrotnie powtarzane, aby zminimalizować utratę funkcji. Za pomocą GBTs regresji i klasyfikacja. Ich mogą obsługiwać funkcje kategorii, nie wymaga funkcji skalowania i przechwycić nonlinearities oraz funkcji interakcji. Również ich zastosowań w ustawieniach multiclass klasyfikacji.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Wynik:**

Jest test R-sqr: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Narzędzia zaawansowane modelowanie zoptymalizować

W tej sekcji można za pomocą narzędzi nauki komputera używane w deweloperzy często Optymalizacja modelu. W szczególności przy użyciu parametru kominów i krzyżowego sprawdzania poprawności może optymalizować maszynowego uczenia modeli trzy sposoby:

-   Dane można podzielić zestawy pociągu i sprawdzanie poprawności, optymalizacja modelu przy użyciu funkcji hyper parametru kominów zestawu szkolenia i oceny zestawu sprawdzania poprawności (regresji liniowej.)
-   Optymalizowanie modelu przy użyciu krzyżowego sprawdzania poprawności oraz funkcji hyper parametru zmiennymi przy użyciu funkcji CrossValidator Spark ML (Klasyfikacja binarne)
-   Optymalizowanie modelu przy użyciu kodu niestandardowego krzyżowego sprawdzania poprawności i parametr kominów używać dowolnego maszynowego uczenia zestawu funkcji i parametrem (regresji liniowej.)


**Sprawdzanie poprawności krzyżowe** jest techniką ocenia, jak modelu ćwiczenie znane zestaw danych będzie generalize przewidywanie funkcji zestawów danych, na których go nie po zapoznaniu. Ogólne ideą tej techniki to, czy model jest ćwiczenie zbioru danych znanych danych, a następnie dokładność jego przewidywań jest sprawdzany przed niezależnych zestawu danych. Typowych wykonania jest dzielony zbioru danych *k*-zgięcia, a następnie przeszkolenie modelu w sposób okrężny we wszystkich z wyjątkiem jednego składając je.

**Optymalizacja parametr funkcji Hyper** jest to problem wybierania zestawu funkcji hyper-parametrów algorytmu szkoleniowe, zwykle za pomocą cel optymalizowania miary wydajności algorytmu o niezależnych zestaw danych. Funkcji hyper parametr jest wartością musisz określić poza procedury szkolenie modelu. Założenia dotyczące wartości parametru funkcji hyper może wpłynąć na elastyczność i dokładności modelu. Algorytmów mieć funkcji hyper parametry, na przykład, takie jak odpowiedniej głębokości i liczba liśćmi w drzewie. Należy ustawić terminu kar błędną klasyfikację dla komputera wektorowa pomocy technicznej (SVM).

Powszechne rozwiązanie w celu wykonania optymalizacji parametr funkcji hyper jest przy użyciu funkcji wyszukiwania siatki, nazywany **Wyczyść parametru**. W wynikach wyszukiwania siatki pełnego wyszukiwanie jest wykonywane za pośrednictwem wartości określony podzestaw funkcji hyper parametr miejsca Algorytm uczenia. Sprawdzanie poprawności między dostarczyć metryki wydajności do sortowania optymalne wyniki tworzone przez algorytm wyszukiwania siatki. Użycie krzyżowego sprawdzania poprawności parametru funkcji hyper kominów może pomóc limit problemom, takim jak overfitting modelu danych szkolenie. Dzięki temu modelu zachowuje możliwości dotyczyć ogólny zestaw danych, z którego została wyodrębnionych dane szkolenia.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optymalizowanie modelu regresji liniowej z kominów funkcji hyper parametru

Następnie można podzielić danych zestawy pociągu i sprawdzanie poprawności, użyj funkcji hyper parametru zmiennymi zestawu szkolenie zoptymalizować model oraz oceny zestawu sprawdzania poprawności (regresji liniowej.).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Wynik:**

Jest test R-sqr: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optymalizacja modelu klasyfikacji binarne przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru

W tej sekcji pokazano, jak zoptymalizować model klasyfikacji binarne przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru. Ta opcja stosuje Spark ML `CrossValidator` funkcji.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Wynik:**

Czas uruchomienia komórki: 33 sekundy.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optymalizowanie modelu regresji liniowej przy użyciu kodu niestandardowego krzyżowego sprawdzania poprawności i kominów parametru

Następnie Optymalizowanie modelu przy użyciu kodu niestandardowego i identyfikacji najlepszych parametrów modelu przy użyciu kryterium najwyższe dokładności. Następnie utwórz ostatecznego modelu, ocena modelu danych test i zapisać model w magazynie obiektów Blob. Na koniec ładowanie modelu, wynik badań i oceny dokładności.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Wynik:**

Czas uruchomienia komórki: 61 sekund.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Używanie modele nauki wbudowany Spark komputera automatycznie z Scala

Aby uzyskać omówienie tematy, które znajdziesz szczegółową zadania, które składają się proces nauki danych platformy Azure zobacz [Proces nauki danych zespołu](http://aka.ms/datascienceprocess).

[Proces nauki danych zespołu instruktaży](data-science-process-walkthroughs.md) zawiera opis innych instruktaży zakończenia do końca, który wykazać kroki w procesie nauki zespołu danych dla określonych scenariuszach. Instruktaży również przedstawiają sposób łączenia chmury i lokalnych narzędzia i usługi do przepływu pracy lub procesu, aby utworzyć aplikację inteligentnego.

[Wynik wbudowany Spark maszynowego uczenia modeli](machine-learning-data-science-spark-model-consumption.md) pokazano, jak za pomocą kodu Scala automatycznie ładowanie i wynik nowych zestawów danych w modelach maszynowego uczenia wbudowany w iskrowym i zapisane w magazynie obiektów Blob platformy Azure. Postępuj zgodnie z instrukcjami i po prostu zastąpić Python Scala kod w tym artykule automatyczne zużycie.
