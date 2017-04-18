<properties
    pageTitle="Poszukiwanie danych i modelowanie z Spark | Microsoft Azure"
    description="Ilustrację możliwości badań i modelowanie danych Spark MLlib zestaw narzędzi."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="data-exploration-and-modeling-with-spark"></a>Poszukiwanie danych i modelowanie z Spark

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

W tym instruktażu została użyta HDInsight Spark do badań danych i binarne klasyfikacji regresji modelowania zadania, na przykład Warszawa taksówki podróży i taryfy 2013 zestawu danych.  Przeprowadzi Cię przez etapy [Procesu nauki danych](http://aka.ms/datascienceprocess), end-to-end, przy użyciu iskry HDInsight klaster przetwarzania i obiektów blob platformy Azure do przechowywania danych i modelu. Proces opisuje spowoduje wizualizację danych zaimportowane z obiektów Blob platformy Azure miejsca do magazynowania i następnie przygotowanie dane, których chcesz utworzyć przewidywanych modele. Te modele są tworzenie przy użyciu narzędzi Spark MLlib zrobić binarne klasyfikacji i regresji zadań modelowania.

- Zadanie **binarne klasyfikacji** jest określenie, czy etykietkę zapłacono do podróży. 
- Zadanie **regresji** jest przewidywanie ilość Porada na podstawie innych funkcji porada. 

Modele, które firma Microsoft korzysta z obejmują regresji logistycznych i liniowy, losowe lasów i gradientu drzew zwiększane:

- [Regresji liniowej z SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) jest modelu regresji liniowej, który korzysta z metody stochastycznego zejścia gradientowego (SGD) i optymalizacji i funkcji skalowania przewidywanie kwoty Porada zapłacone. 
- Regresji [regresją z LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) lub "logit", to model regresji, który może być używany w przypadku zmienną zależną kategorii robić klasyfikacji danych. LBFGS to algorytm optymalizacji quasi-Kowalski który zbliżona algorytmu Broyden — Fletcher — Goldfarb — Shanno (BFGS) przy użyciu ograniczone ilości pamięci komputera i powszechnie używany w nauki komputera.
- [Losowe lasów](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) są komplety drzewa decyzji.  Łączą wiele algorytmów, aby zmniejszyć ryzyko overfitting. Przypadkowe lasów służą do regresji i klasyfikacji i mogą obsługiwać funkcje kategorii i może zostać wydłużony z ustawieniem klasyfikacji multiclass. Ta osoba nie wymaga funkcji skalowania i mogą do przechwytywania nieliniowość i funkcji interakcji. Przypadkowe lasów to jeden z najbardziej popularnych maszynowego uczenia modeli klasyfikacji i regresji.
- [Gradient zwiększane drzewa](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) są komplety drzewa decyzji. GBTs przeszkolenie algorytmów wielokrotnie powtarzane, aby zminimalizować utratę funkcji. GBTs służą do regresji i klasyfikacji i mogą obsługiwać funkcje kategorii, nie wymaga funkcji skalowania i będą mogli przechwycić nieliniowość i funkcji interakcji. Można też nimi ustawienie klasyfikacji multiclass.

Procedura modelowania również zawierać kod przedstawiający, jak przeszkolenie, ocenianie i zapisywanie każdego typu modelu. Python został użyty do kodu rozwiązanie i, aby pokazać odpowiednich powierzchni.   


>[AZURE.NOTE] Chociaż narzędzi Spark MLlib jest przeznaczona do pracy w dużych zestawów danych, stosunkowo niewielką próbki (~ 30 Mb przy użyciu 170K wierszy, około 0,1% oryginalnego zestawu danych Warszawa) jest używana w tym miejscu dla wygody. Wykonywanie podane tutaj działa efektywne (w około 10 minut) w klastrze HDInsight z 2 węzły pracownika. Ten sam kod ze zmianami pomocnicze, może służyć do procesu większe-zestawów danych, z właściwymi zmianami pamięci podręcznej danych przechowywanych w pamięci i zmieniania rozmiaru klaster.

## <a name="prerequisites"></a>Wymagania wstępne

Potrzebujesz konta usługi Azure i HDInsight Spark możesz potrzebna 1.6 Spark 3.4 HDInsight klaster do przeprowadzenia tego instruktażu. Zobacz [Omówienie danych nauki przy użyciu Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) Aby uzyskać instrukcje dotyczące spełnienia tych wymagań. Ten temat zawiera również zapoznać się z opisem taksówki 2013 Warszawa dane używane w tym miejscu i instrukcje dotyczące wykonanie kodu z notesu Jupyter w klastrze Spark. **PySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** Notes, który zawiera przykłady kodu, w tym temacie jest dostępna w [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Konfiguracji: lokalizację, bibliotek i kontekst Spark wstępnie ustawionych

Spark będzie mógł odczytywać i zapisywać obiektów Blob usługi Azure miejsca do magazynowania (nazywane także WASB). Aby żadnych istniejących danych przechowywanych mogą być przetwarzane przy użyciu iskrowym i ponownie przechowywane w WASB wyniki.

Aby zapisać modeli lub pliki w WASB, ścieżka nie trzeba określać poprawnie. Kontener domyślny dołączone do klastrów Spark można odwoływać się przy użyciu ścieżki począwszy od: "wasb: / / /". Odwołuje się innych lokalizacji "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Ustawianie ścieżki katalogu do lokalizacji przechowywania w WASB

Poniższy przykład kodu Określa lokalizację danych do odczytu i ścieżkę katalogu miejsca do magazynowania model, do którego jest zapisany wynik modelu:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Importowanie biblioteki

Konfigurowanie wymaga także importowanie niezbędne bibliotek. Ustaw kontekst spark i importowanie niezbędne bibliotek za pomocą następującego kodu:


    # IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Wstępnie ustawione kontekst Spark i PySpark magics

Jądra PySpark, które są dostarczane z notesami Jupyter mają wstępnie ustawionych kontekst. Dlatego nie trzeba ustawić Spark lub tworzenia konteksty gałęzi jawnie przed rozpoczęciem pracy z aplikacją. Te konteksty są domyślnie dostępne dla Ciebie. Te konteksty są:

- SC - dla Spark 
- sqlContext - gałęzi

Jądro PySpark udostępnia kilka wstępnie zdefiniowanych "magics", które są specjalne polecenia wywołujących z %%. Istnieją dwa polecenia, które są używane w tych przykładach kodu.

- **%% lokalny** Określa, że kod w kolejnych wierszach ma zostać wykonana lokalnie. Kod musi być prawidłowy kod Python.
- **%%sql -o <variable name>** Wykonuje gałęzi zapytanie sqlContext. Jeśli parametr -o zostanie przekazany, wynik kwerendy jest umieszczany w %% lokalnych warunków Python jako Pandas DataFrame.
 

Aby uzyskać więcej informacji na temat jądra Jupyter notesów i wstępnie zdefiniowanych "magics" które zapewniają, zobacz [jądra dostępne dla Jupyter notesów przy użyciu klastrów HDInsight Spark Linux na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>Spożyciu danych z publicznej obiektów blob

Pierwszym krokiem w procesie nauki danych jest mogły zjeść tej ostatniej dane, które mają być analizowane źródłach miejsce, w którym się znajdują się do środowiska badań i modelowanie danych. Środowisko jest Spark w tym instruktażu. Ta sekcja zawiera kod, aby ukończyć serii zadań:

- mogły zjeść tej ostatniej próbki danych, aby być w modelu
- Przeczytaj w zestawie danych wejściowych (przechowywane jako pliku .tsv)
- Formatowanie i Usuń dane
- Tworzenie i pamięci podręcznej obiektów (RDDs lub ramek danych) w pamięci
- Zarejestruj go jako tabelę temp w kontekście SQL.

Poniżej przedstawiono kod spożyciu danych.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**WYNIK:**

Czas wykonywania powyżej komórki: 51.72 sekund


## <a name="data-exploration--visualization"></a>Poszukiwanie danych i wizualizacji

Po wprowadzeniu danych w iskrowym, następnym krokiem w procesie nauki danych jest uzyskanie szczegółowego zrozumienia danych za pomocą badań i wizualizacji. W tej sekcji możemy zbadać danych taksówki przy użyciu zapytania SQL oraz wykreślanie docelowych zmiennych i potencjalnych funkcje kontroli wizualnych. W szczególności możemy wykreślić częstotliwość liczby osób w podróży taksówki, częstotliwość kwoty Porada i jak porady zależy od kwota płatności i wpisz.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Wykreśl histogram częstotliwości liczba osób w próbce taksówki podróży

Tego kodu i kolejne fragmenty umożliwia Magia SQL kwerendy próbki i lokalnych Magia kreślenia danych.

- **Magia SQL (`%%sql`)** Jądro HDInsight PySpark obsługuje łatwe w tekście zapytań HiveQL sqlContext. (-O nazwa_zmiennej) argument będzie nadal występował wyniki kwerendy SQL jako DataFrame Pandas na serwerze Jupyter. Oznacza to, że jest ona dostępna w trybie lokalnym.
- ** `%%local` Magiczną** umożliwia uruchomienie kodu lokalnie na serwerze Jupyter, czyli headnode klastrze HDInsight. Zazwyczaj `%%local` Magiczna w połączeniu z `%%sql` Magiczna parametru -o. Parametr -o czy utrzymują wynik kwerendy SQL lokalnie, a następnie %% lokalne Magia spowoduje następnego zestawu wstawkę kodu w celu uruchomienia lokalnie dane wyjściowe zapytania SQL, który jest zachowywane lokalnie

Wynik jest automatycznie zwizualizować po uruchomieniu kodu.

Ta kwerenda pobiera podróży według liczby osób. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Ten kod tworzy lokalne ramki danych z wyników kwerendy i zawiera dane. `%%local` Magiczną tworzy lokalne ramki danych, `sqlResults`, który może być używany do wykreślania z matplotlib. 

>[AZURE.NOTE] Ten Magia PySpark jest używany wiele razy w tym instruktażu. W przypadku dużej ilości danych, należy przykładowych Tworzenie danych — ramki, który można umieścić w lokalnej pamięci.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Oto kod kreślenia podróży według liczby osób

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**WYNIK:**

![Częstotliwość podróży według liczby osób](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Za pomocą przycisków menu **Typ** w notesie, można wybrać spośród wielu różnych typów wizualizacji (tabela, kołowego, linia, obszar lub pasek). Wykres słupkowy pokazano poniżej.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Wykreśl histogram kwot Porada i jak kwota Porada zależy od ilości tekstu taryfy i liczba osób.

Użyj zapytania SQL do przykładowych danych.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Ta komórka kodu użyto kwerendy SQL, aby utworzyć wykresy trzy dane.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**WYNIK:** 

![Porada podział kwot](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Porada kwota według liczby osób](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Porada kwota kwotą Taryfy](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funkcja techniczny, przekształcania i danych przygotowanie do modelowania
W tej sekcji opisano i zawiera kod do procedur używane do przygotowywania danych do użycia w ML modelowania. Jest wyświetlany sposobów wykonywania następujących zadań:

- Utwórz nową funkcję, binning godziny do przedziałów czasu ruchu
- Indeksowanie i kodowanie kategorii funkcji
- Tworzenie obiektów oznaczonych punktu wejścia do funkcji ML
- Tworzenie losowe pobieranie próbek zastępczych danych i podzielić je na szkoleń i testowania zestawy
- Funkcja skalowania
- Obiekty pamięci podręcznej w pamięci


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Utwórz nową funkcję, binning godziny do przedziałów czasu ruchu

Kod pokazano, jak utworzyć nową funkcję o binning godziny do przedziałów czasu ruch, a następnie jak buforować wyniku ramki danych w pamięci. W przypadku mechanizm Distributed zestawów danych (RDDs) i ramek danych wielokrotnie pamięci podręcznej prowadzi do ulepszone czasów wykonania. W związku z tym firma Microsoft pamięci podręcznej RDDs i ramek danych w kilku etapach Instruktaż. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**WYNIK:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Indeksowanie i kodowanie kategorii Funkcje związane z wprowadzeniem do funkcji modelowania

W tej sekcji przedstawiono sposób indeksowanie lub kodowanie kategorii Funkcje związane z wprowadzeniem do funkcji modelowania. Modelowania i przewidywania funkcji MLlib wymaga funkcji z indeksowanych lub kodowane przed użyciem kategorii danych wejściowych. W zależności od modelu musisz indeksowanie lub kodowanie je na różne sposoby:  

- **Drzewa modelowania** wymaga kategorie, aby być zakodowany jako wartości liczbowe (na przykład funkcja o trzy kategorie mogą być kodowane z 0, 1, 2). To jest udostępniany przez osoby MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funkcji. Ta funkcja kodowane kolumny ciąg etykiet do kolumny wskaźników etykiety, zamówionych przez częstotliwości etykiety. Mimo że indeksowane z wartościami liczbowymi wprowadzania i obsługi danych, algorytmy oparte na drzewa można określić je odpowiednio traktować jako kategorie. 

- **Logistic i regresji liniowej modele** wymagają hot jeden kodowanie, gdzie, na przykład funkcja z trzech kategorii można rozwinąć na trzy kolumny funkcji, z każdego zawierające 0 lub 1, w zależności od kategorii obserwacji. MLlib zawiera [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkcji hot jednego kodowania. Ten koder mapy kolumny wskaźników etykiety do kolumny wektorów binarny z co najwyżej jednego jedną wartość. Kodowanie umożliwia algorytmów oczekiwać liczbowe wartości funkcji, takich jak regresją mają być stosowane do kategorii funkcji.

Poniżej przedstawiono kod do indeksu i kodowania kategorii funkcje:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 1,28 sekund

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Tworzenie obiektów oznaczonych punktu wejścia do funkcji ML

Ta sekcja zawiera kod, który przedstawia sposób indeks danych tekstowych kategorii jako punkt oznaczona etykietą typ danych i kodowania jej, tak aby może służyć do szkolenie i testowanie regresją MLlib i innych modeli klasyfikacji. Punkt oznaczona etykietą obiekty są mechanizm Distributed zestawów danych (RDD) sformatowane w sposób, który jest potrzebny najczęściej algorytmy ML MLlib jako danych wejściowych. [Etykietą punktu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) jest lokalne wektorowa gęstości lub rzadkie, skojarzone z etykiety i odpowiedzi.  

Ta sekcja zawiera kod, który przedstawia sposób indeks danych tekstowych kategorii jako typ danych [etykietą punktu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) i kodowania jej, tak aby może służyć do szkolenie i testowanie regresją MLlib i innych modeli klasyfikacji. Punkt oznaczona etykietą obiekty są mechanizm Distributed zestawów danych (RDD) składający się z etykietą (zmienna docelowej odpowiedzi) i wektorowej funkcja. W tym formacie jest wymagane jako danych wejściowych przez wiele algorytmów ML w MLlib.

Poniżej przedstawiono kod indeksu i kodowanie tekstu funkcje klasyfikacji dwójkową.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Poniżej przedstawiono kod kodowanie i indeksu funkcji kategorii tekstu na potrzeby analizy regresji liniowej.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Tworzenie losowe pobieranie próbek zastępczych danych i podzielić je na szkoleń i testowania zestawy

Kod ten tworzy losową próbki danych (25% jest używany jako poniżej). Mimo że nie jest wymagane w tym przykładzie ze względu na rozmiar zestawu danych, możemy pokazano, jak próbkę można pobrać tutaj, możesz dowiedzieć się, jak używać tej funkcji własne problemu w razie potrzeby. W przypadku dużych próbek to zaoszczędzić czas istotne podczas szkolenia modeli. Następnie firma Microsoft można podzielić próbki szkolenia (75% poniżej) i testowania (poniżej 25%) do użycia w klasyfikacji i modelowania regresji.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 0,24 sekund


### <a name="feature-scaling"></a>Funkcja skalowania

Funkcja skalowania, nazywane także normalizacji danych uzyskanie, że funkcje z wartościami powszechnie rozchodów są nie przyznał nadmiarowe oceny w celu funkcji. Kod funkcji skalowania używa [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) przeskalować funkcje do Odchylenie jednostki. Działaniem MLlib do użytku w regresji liniowej z stochastycznego gradientowego zejścia (SGD), popularne algorytm szkoleniowe szeroką gamę innych maszynowego uczenia modeli, takich jak umorzyć strat lub maszyn wektorowa pomocy technicznej (SVM).

>[AZURE.NOTE] Znalezionych algorytm LinearRegressionWithSGD jako poufne do promowania skalowania.

Poniżej przedstawiono kod do zmiennych skali do użytku z algorytmu regularized SGD liniowego.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 13.17 sekund


### <a name="cache-objects-in-memory"></a>Obiekty pamięci podręcznej w pamięci

Czas szkoleń i testowania algorytmów ML można zmniejszyć przez buforowanie ramki danych wejściowych obiektów na potrzeby klasyfikacji, regresji i skalowany funkcje.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:** 

Czas wykonywania powyżej komórki: 0,15 sekund


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Przewidywanie czy etykietkę zapłacono z modelami binarne klasyfikacji

W tej sekcji przedstawiono, jak Użyj trzech modeli dla zadania binarne klasyfikacji przewidywania czy etykietkę zapłacono w podróży taksówki. Modele przedstawione są:

- Umorzyć regresją 
- Model losowej las
- Drzewa zwiększający gradientowego

Każdy model konstruowania sekcję kodu jest podzielony na czynności: 

1. **Szkolenie modelu** danych za pomocą jednego zestawu parametru
2. **Ocena modelu** z zestawem danych test zawierającym metryki
3. **Zapisywanie modelu** w obiektów blob zużycia przyszłych

### <a name="classification-using-logistic-regression"></a>Za pomocą regresją klasyfikacji

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i Zapisz model regresją [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , która wskazuje, czy etykietkę zapłacono w podróży w Warszawa taksówki podróży i taryfy zestawie danych.

**Przeszkolenie modelu regresją, używając OKS i kominów hyperparameter**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK:** 

Współczynniki: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Punkt przecięcia:-0.0111216486893

Czas wykonywania powyżej komórki: 14.43 sekund

**Ocena modelu klasyfikacji binarny z metryki standardowy**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**WYNIK:** 

Obszarze PR = 0.985297691373

Obszarze ROC = 0.983714670256

Statystyka podsumowania

Dokładność = 0.984304060189

Odwoływanie = 0.984304060189

F1 Wynik = 0.984304060189

Czas wykonywania powyżej komórki: 57.61 sekund

**Wykreślić krzywą ROC.**

*PredictionAndLabelsDF* jest zarejestrowany jako tabelę *tmp_results*w poprzedniej komórki. *tmp_results* może służyć do wykonania kwerendy i wynik do ramki danych sqlResults dla Rysowanie. Oto kod.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Poniżej przedstawiono kod cennych oraz Wykreślanie krzywej ROC.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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
    

**WYNIK:**

![Curve.png ROC regresją](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Przypadkowe las klasyfikacji

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie modelu losowe las, który wskazuje, czy etykietkę zapłacono w podróży w Warszawa taksówki podróży i taryfy zestawie danych.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Obszarze ROC = 0.985297691373

Czas wykonywania powyżej komórki: 31.09 sekund


### <a name="gradient-boosting-trees-classification"></a>Gradient zwiększający klasyfikacji drzewa

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenić i zapisać gradientu zwiększający modelu drzew, który wskazuje, czy w podróży w podróży taksówki Warszawa zapłacono porady i taryfy zestawu danych.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK:**

Obszarze ROC = 0.985297691373

Czas wykonywania powyżej komórki: 19.76 sekund


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Przewidywanie Porada kwoty taksówki podróży z modelami regresji

W tej sekcji przedstawiono, jak trzech modeli użycia dla zadania regresji przewidywania ilość poradę opłacona wycieczki taksówki na podstawie innych funkcji porada. Modele przedstawione są:

- Umorzyć regresji liniowej.
- Przypadkowe las
- Drzewa zwiększający gradientowego

Te modele zostały opisane we wprowadzeniu. Każdego modelu tworzenia sekcję kodu jest podzielony na czynności: 

1. **Szkolenie modelu** danych za pomocą jednego zestawu parametru
2. **Ocena modelu** z zestawem danych test zawierającym metryki
3. **Zapisywanie modelu** w obiektów blob zużycia przyszłych

### <a name="linear-regression-with-sgd"></a>Z SGD linią regresji liniowej. 

Kod w tej sekcji przedstawiono, jak za pomocą funkcji skalować przeszkolenie regresji liniowej używaną stochastycznego zejścia gradientowego (SGD) na potrzeby optymalizacji i jak wynik, ocenianie i zapisać model w magazyn obiektów Blob platformy Azure (WASB).

>[AZURE.TIP] W naszym środowisko mogą występować problemy ze zbliżania modeli LinearRegressionWithSGD i parametry należy zmienić i zoptymalizowane dokładnie w celu uzyskania prawidłowy model. Skalowanie zmiennych znacznie ułatwia spójności. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Współczynniki: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

ODCIĘTA: 0.853872718283

RMSE = 1.24190115863

R sqr = 0.608017146081

Czas wykonywania powyżej komórki: 58.42 sekund


### <a name="random-forest-regression"></a>Las losowo regresji

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie regresji losowe las, który przewiduje kwota Porada danych Warszawa taksówki podróży.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

RMSE = 0.891209218139

R sqr = 0.759661334921

Czas wykonywania powyżej komórki: 49.21 sekund


### <a name="gradient-boosting-trees-regression"></a>Gradient zwiększający regresji drzewa

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie gradientu zwiększający modelu drzew, który przewiduje kwota Porada danych Warszawa taksówki podróży.

**Przeszkolenie i oceny**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

RMSE = 0.908473148639

R sqr = 0.753835096681

Czas wykonywania powyżej komórki: 34.52 sekund

**Kreśl**

*tmp_results* jest zarejestrowany jako tabelę programu Hive w poprzedniej komórki. Wyniki z tabeli są dane wyjściowe do ramki danych *sqlResults* dla Rysowanie. Oto kod

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Poniżej przedstawiono kod kreślenia danych przy użyciu serwera Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**WYNIK:**

![A przewidziane — Porada kwoty rzeczywiste](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Oczyść obiekty z pamięci

Używanie `unpersist()` usunięcie obiektów w pamięci podręcznej.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Rekord lokalizację modeli zużycia i wyników

Używanie i wynik zestaw niezależnych opisanego w [wynik i oceny modeli wbudowany Spark maszynowego uczenia](machine-learning-data-science-spark-model-consumption.md) tematu, należy skopiować i wkleić te nazwy pliku zawierającego zapisany modele utworzone tutaj do notesu Jupyter zużycie. Poniżej przedstawiono kod, aby wydrukować ścieżki do modelu pliki, które potrzebujesz.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**WYNIK**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Co to jest dalej?

Teraz, gdy Spark MlLib utworzony modeli regresji i klasyfikacji, możesz przystąpić do Dowiedz się, jak wynik i oceny tych modeli. Poszukiwanie zaawansowane danych i modelowanie notesu dives szczegółowego w tym krzyżowego sprawdzania poprawności, funkcji hyper parametru zmiennymi i modelu oceny. 

**Modelu zużycie:** Aby dowiedzieć się, jak wynik i oceny modeli klasyfikacji i regresji, utworzonych w tym temacie, zobacz [wynik i oceny modeli nauki wbudowany Spark komputera](machine-learning-data-science-spark-model-consumption.md).

**Krzyżowego sprawdzania poprawności i hyperparameter zmiennymi**: zobacz [Zaawansowane badań danych i modelowanie z Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) jak można modeli szkolenia przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru



