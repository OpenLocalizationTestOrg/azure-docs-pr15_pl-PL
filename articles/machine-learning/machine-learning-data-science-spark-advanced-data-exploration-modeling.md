<properties
    pageTitle="Zaawansowane badań danych i modelowanie z Spark | Microsoft Azure"
    description="Za pomocą HDInsight Spark badań danych i przeszkolenie binarne modeli klasyfikacji i regresji przy użyciu optymalizacji krzyżowego sprawdzania poprawności i hyperparameter."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Poszukiwanie zaawansowane danych i modelowanie z Spark 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

W tym instruktażu wykorzystano HDInsight Spark badań danych i przeszkolenie binarne klasyfikacji i modeli regresji przy użyciu krzyżowego sprawdzania poprawności i optymalizacji hyperparameter próbki Warszawa taksówki podróży i taryfy 2013 zestawu danych. Przeprowadzi Cię przez etapy [Procesu nauki danych](http://aka.ms/datascienceprocess), end-to-end, przy użyciu iskry HDInsight klaster przetwarzania i obiektów blob platformy Azure do przechowywania danych i modelu. Proces opisuje spowoduje wizualizację danych zaimportowane z obiektów Blob platformy Azure miejsca do magazynowania i następnie przygotowanie dane, których chcesz utworzyć przewidywanych modele. Python został użyty do kodu rozwiązanie i, aby pokazać odpowiednich powierzchni. Te modele są tworzenie przy użyciu narzędzi Spark MLlib zrobić binarne klasyfikacji i regresji zadań modelowania. 

- Zadanie **binarne klasyfikacji** jest określenie, czy etykietkę zapłacono do podróży. 
- Zadanie **regresji** jest przewidywanie ilość Porada na podstawie innych funkcji porada. 

Procedura modelowania również zawierać kod przedstawiający, jak przeszkolenie, ocenianie i zapisywanie każdego typu modelu. Temat opisano niektóre z tym samym ziemi jako temat [badań danych i modelowanie z Spark](machine-learning-data-science-spark-data-exploration-modeling.md) . Ale jej jest bardziej "Zaawansowane" w tym również wykorzystuje krzyżowego sprawdzania poprawności w połączeniu z hyperparameter zmiennymi na szkolenie optymalnie szczegółowych modeli klasyfikacji i regresji. 

**Krzyżowego sprawdzania poprawności (OKS)** jest techniką ocenia, jak generalizes modelu ćwiczenie znane zestaw danych do przewidywania, korzystając z funkcji zestawy danych, w którym go nie po zapoznaniu. Ogólne ideą ta metoda jest model jest ćwiczenie zestawu danych znane danych na, a następnie dokładność jego przewidywań jest sprawdzany przed niezależnych zestawu danych. Implementacji używane w tym miejscu jest podzielić zestawu danych na zgięciach K, a następnie przeszkolenie modelu w sposób okrężny we wszystkich z wyjątkiem jednego składając je. 

**Optymalizacja Hyperparameter** jest to problem wybierania zestawu hyperparameters dla algorytmu szkoleniowe, zwykle za pomocą cel optymalizowania miary wydajności algorytmu o niezależnych zestaw danych. **Hyperparameters** są wartościami, które należy określić poza procedury szkolenie modelu. Założenia dotyczące tych wartości może mieć wpływ na elastyczność i dokładności modeli. Algorytmów mają hyperparameters, na przykład, takie jak odpowiedniej głębokości i liczba liśćmi w drzewie. Obsługa wektorowa maszyn (SVMs) wymagają ustawienie terminu kar błędną klasyfikację. 

Powszechne rozwiązanie w celu wykonania optymalizacji hyperparameter używane w tym miejscu jest wyszukiwania siatki lub **Wyczyść parametru**. Składa się z wyszukiwania pełnego przez wartości podzbiór określonego miejsca hyperparameter dla algorytmu szkoleniowe. Krzyżowego sprawdzania poprawności można podać metryki wydajności do sortowania optymalne wyniki tworzone przez algorytm wyszukiwania siatki. OKS używane z hyperparameter zmiennymi ułatwia limit problemom, takim jak overfitting modelu danych szkolenie tak, aby modelu zachowuje możliwości dotyczyć ogólny zestaw danych, z którego została wyodrębnionych dane szkolenia.

Modele, które firma Microsoft korzysta z obejmują regresji logistycznych i liniowy, losowe lasów i gradientu drzew zwiększane:

- [Regresji liniowej z SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) jest modelu regresji liniowej, który korzysta z metody stochastycznego zejścia gradientowego (SGD) i optymalizacji i funkcji skalowania przewidywanie kwoty Porada zapłacone. 
- Regresji [regresją z LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) lub "logit", to model regresji, który może być używany w przypadku zmienną zależną kategorii do klasyfikacji danych. LBFGS to algorytm optymalizacji quasi-Kowalski który zbliżona algorytmu Broyden — Fletcher — Goldfarb — Shanno (BFGS) przy użyciu ograniczone ilości pamięci komputera i powszechnie używany w nauki komputera.
- [Losowe lasów](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) są komplety drzewa decyzji.  Łączą wiele algorytmów, aby zmniejszyć ryzyko overfitting. Przypadkowe lasów służą do regresji i klasyfikacji i mogą obsługiwać funkcje kategorii i może zostać wydłużony z ustawieniem klasyfikacji multiclass. Ta osoba nie wymaga funkcji skalowania i mogą do przechwytywania nieliniowość i funkcji interakcji. Przypadkowe lasów stanowią jeden z najbardziej popularnych maszynowego uczenia modeli klasyfikacji i regresji.
- [Gradient zwiększane drzewa](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) są komplety drzewa decyzji. GBTs przeszkolenie algorytmów wielokrotnie powtarzane, aby zminimalizować utratę funkcji. GBTs służą do regresji i klasyfikacji i mogą obsługiwać funkcje kategorii, nie wymaga funkcji skalowania i będą mogli przechwycić nieliniowość i funkcji interakcji. Można też nimi ustawienie klasyfikacji multiclass.

Modelowanie przykłady użycia OKS i Hyperparameter przedstawiono wyczyść problem klasyfikacji binarne. Przykłady prostsze (bez parametrów wymazywanie) są prezentowane w tematu głównego dla zadań regresji. Jednak w dodatku, również są prezentowane przy użyciu elastyczne netto dla regresji liniowej i OKS przy użyciu parametru wyczyść przy regresji losowe las sprawdzania poprawności. **Elastyczne netto** jest umorzyć regresji metodą dopasowanie modeli regresji liniowej liniowo łączący metryki L1 i L2 jako kar [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) i [pierścieniem](https://en.wikipedia.org/wiki/Tikhonov_regularization) metod.   



>[AZURE.NOTE] Chociaż narzędzi Spark MLlib jest przeznaczona do pracy w dużych zestawów danych, stosunkowo niewielką próbki (~ 30 Mb przy użyciu 170K wierszy, około 0,1% oryginalnego zestawu danych Warszawa) jest używana w tym miejscu dla wygody. Wykonywanie podane tutaj działa efektywne (w około 10 minut) w klastrze HDInsight z 2 węzły pracownika. Ten sam kod ze zmianami pomocnicze, może służyć do procesu większe-zestawów danych, z właściwymi zmianami pamięci podręcznej danych przechowywanych w pamięci i zmieniania rozmiaru klaster.


## <a name="prerequisites"></a>Wymagania wstępne

Potrzebujesz konta usługi Azure i HDInsight Spark możesz potrzebna 1.6 Spark 3.4 HDInsight klaster do przeprowadzenia tego instruktażu. Zobacz [Omówienie danych nauki przy użyciu Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) Aby uzyskać instrukcje dotyczące spełnienia tych wymagań. Ten temat zawiera również zapoznać się z opisem taksówki 2013 Warszawa dane używane w tym miejscu i instrukcje dotyczące wykonanie kodu z notesu Jupyter w klastrze Spark. **PySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** Notes, który zawiera przykłady kodu, w tym temacie jest dostępna w [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


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
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**WYNIK**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Importowanie biblioteki

Importowanie niezbędne bibliotek za pomocą następującego kodu:

    # LOAD PYSPARK LIBRARIES
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


## <a name="data-ingestion-from-public-blob"></a>Spożyciu danych z publicznej obiektów blob: 

Pierwszy krok w procesie nauki danych jest mogły zjeść tej ostatniej dane, które mają być analizowane ze źródeł, w którym się ona znajduje do badań danych i środowisko modelowania. To środowisko jest Spark w tym instruktażu. Ta sekcja zawiera kod, aby ukończyć serii zadań:

- mogły zjeść tej ostatniej próbki danych, aby być w modelu
- Przeczytaj w zestawie danych wejściowych (przechowywane jako pliku .tsv)
- Formatowanie i Usuń dane
- Tworzenie i pamięci podręcznej obiektów (RDDs lub ramek danych) w pamięci
- Zarejestruj go jako tabelę temp w kontekście SQL.

Poniżej przedstawiono kod spożyciu danych.


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
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

Czas wykonywania powyżej komórki: 276.62 sekund


## <a name="data-exploration--visualization"></a>Poszukiwanie danych i wizualizacji 

Po wprowadzeniu danych w iskrowym, następnym krokiem w procesie nauki danych jest uzyskanie szczegółowego zrozumienia danych za pomocą badań i wizualizacji. W tej sekcji możemy zbadać danych taksówki przy użyciu zapytania SQL oraz wykreślanie docelowych zmiennych i potencjalnych funkcje kontroli wizualnych. W szczególności możemy wykreślić częstotliwość liczby osób w podróży taksówki, częstotliwość kwoty Porada i jak porady zależy od kwota płatności i wpisz.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Wykreśl histogram częstotliwości liczba osób w próbce taksówki podróży

Tego kodu i kolejne fragmenty umożliwia Magia SQL kwerendy próbki i lokalnych Magia kreślenia danych.

- **Magia SQL (`%%sql`)** Jądro HDInsight PySpark obsługuje łatwe w tekście zapytań HiveQL sqlContext. (-O nazwa_zmiennej) argument będzie nadal występował wyniki kwerendy SQL jako DataFrame Pandas na serwerze Jupyter. Oznacza to, że jest ona dostępna w trybie lokalnym.
- ** `%%local` Magiczną** umożliwia uruchomienie kodu lokalnie na serwerze Jupyter, czyli headnode klastrze HDInsight. Zazwyczaj `%%local` Magiczna w połączeniu z `%%sql` Magiczna parametru -o. Parametr -o czy utrzymują wynik kwerendy SQL lokalnie, a następnie %% lokalne Magia spowoduje następnego zestawu wstawkę kodu w celu uruchomienia lokalnie dane wyjściowe zapytania SQL, który jest zachowywane lokalnie

Wynik jest automatycznie zwizualizować po uruchomieniu kodu.

Ta kwerenda pobiera podróży według liczby osób. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Ten kod tworzy lokalne ramki danych z wyników kwerendy i zawiera dane. `%%local` Magia tworzy lokalne ramki danych, `sqlResults`, który może być używany do wykreślania z matplotlib. 

>[AZURE.NOTE] Ten Magia PySpark jest używany wiele razy w tym instruktażu. W przypadku dużej ilości danych, należy przykładowych Tworzenie danych — ramki, który można umieścić w lokalnej pamięci.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Oto kod kreślenia podróży według liczby osób

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**WYNIK**

![Częstotliwość podróży według liczby osób](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Za pomocą przycisków menu **Typ** w notesie, można wybrać spośród wielu różnych typów wizualizacji (tabela, kołowego, linia, obszar lub pasek). Wykres słupkowy pokazano poniżej.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Wykreśl histogram kwot Porada i jak kwota Porada zależy od ilości tekstu taryfy i liczba osób.

Używanie zapytania SQL do przykładowych danych.
    
    # SQL SQUERY
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**WYNIK:** 

![Porada podział kwot](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Porada kwota według liczby osób](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Porada kwota według taryfy kwota](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funkcja techniczny, przekształcania i danych przygotowanie do modelowania

W tej sekcji opisano i zawiera kod do procedur używane do przygotowywania danych do użycia w ML modelowania. Jest wyświetlany sposobów wykonywania następujących zadań:

- Utwórz nową funkcję, binning godziny do przedziałów czasu ruchu
- Indeksowanie i na hot kodowanie kategorii funkcji
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

**WYNIK**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Indeksowanie i hot jeden kodowanie kategorii funkcji

W tej sekcji przedstawiono sposób indeksowanie lub kodowanie kategorii Funkcje związane z wprowadzeniem do funkcji modelowania. Modelowania i przewidywania funkcji MLlib wymaga funkcji z indeksowanych lub kodowane przed użyciem kategorii danych wejściowych. 

W zależności od modelu musisz indeksowanie lub kodowanie je na różne sposoby. Na przykład Logistic i regresji liniowej modele wymagają hot jeden kodowanie miejsce, w którym, na przykład funkcja z trzech kategorii można rozwinąć na trzy kolumny funkcji, z każdego zawierające 0 lub 1, w zależności od kategorii obserwacji. MLlib zawiera [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkcji hot jednego kodowania. Ten koder mapy kolumny wskaźników etykiety do kolumny wektorów binarny z co najwyżej jednego jedną wartość. Kodowanie umożliwia algorytmów oczekiwać liczbowe wartości funkcji, takich jak regresją mają być stosowane do kategorii funkcji.

Poniżej przedstawiono kod do indeksu i kodowania kategorii funkcje:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
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


**WYNIK**

Czas wykonywania powyżej komórki: 3.14 sekund


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Tworzenie obiektów oznaczonych punktu wejścia do funkcji ML

Ta sekcja zawiera kod, który przedstawia sposób indeks danych tekstowych kategorii jako typ punktu etykietami danych i kodowania jej, tak aby może służyć do szkolenie i testowanie regresją MLlib i innych modeli klasyfikacji. Punkt oznaczona etykietą obiekty są mechanizm Distributed zestawów danych (RDD) sformatowane w sposób, który jest potrzebny najczęściej algorytmy ML MLlib jako danych wejściowych. [Etykietą punktu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) jest lokalne wektorowa gęstości lub rzadkie, skojarzone z etykiety i odpowiedzi.

Poniżej przedstawiono kod indeksu i kodowanie tekstu funkcje klasyfikacji binarne.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
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
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
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

**WYNIK**

Czas wykonywania powyżej komórki: 0.31 sekund


### <a name="feature-scaling"></a>Funkcja skalowania

Funkcja skalowania, nazywane także normalizacji danych uzyskanie, że funkcje z wartościami powszechnie rozchodów są nie przyznał nadmiarowe oceny w celu funkcji. Kod funkcji skalowania używa [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) przeskalować funkcje do Odchylenie jednostki. Działaniem MLlib do użytku w regresji liniowej z stochastycznego gradientowego zejścia (SGD), popularne algorytm szkoleniowe szeroką gamę innych maszynowego uczenia modeli, takich jak umorzyć strat lub maszyn wektorowa pomocy technicznej (SVM).   

>[AZURE.TIP] Znalezionych algorytm LinearRegressionWithSGD jako poufne do promowania skalowania.   

Poniżej przedstawiono kod do zmiennych skali do użytku z algorytmu regularized SGD liniowego.

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

**WYNIK**

Czas wykonywania powyżej komórki: 11.67 sekund


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

**WYNIK** 

Czas wykonywania powyżej komórki: 0,13 sekund


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Przewidywanie czy etykietkę zapłacono z modelami binarne klasyfikacji

W tej sekcji przedstawiono, jak Użyj trzech modeli dla zadania binarne klasyfikacji przewidywania czy etykietkę zapłacono w podróży taksówki. Modele przedstawione są:

- Regresją 
- Przypadkowe las
- Drzewa zwiększający gradientowego

Każdy model konstruowania sekcję kodu jest podzielony na czynności: 

1. **Szkolenie modelu** danych za pomocą jednego zestawu parametru
2. **Ocena modelu** z zestawem danych test zawierającym metryki
3. **Zapisywanie modelu** w obiekcie blob zużycia przyszłych

Jak wykonać krzyżowego sprawdzania poprawności (OKS) możemy wyświetlić z parametrem zmiennymi na dwa sposoby:

1. Przy użyciu **ogólnego** kodu niestandardowego, które mogą być stosowane do dowolnego algorytmu w MLlib i do każdego parametru ustawia w polu algorytm. 
1. Używanie **pySpark funkcja Planowana CrossValidator**. Należy zauważyć, że chociaż wygodny, na podstawie naszych doświadczeń CrossValidator ma kilka ograniczeń Spark 1.5.0: 

    - Potok modeli nie może być zapisany i trwała zużycia przyszłych.
    - Nie można użyć dla każdego parametru w modelu.
    - Nie można używać dla każdej algorytmu MLlib.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Rodzajowy krzyżowe sprawdzanie poprawności i kominów hyperparameter używane z algorytmu regresją klasyfikacji binarny

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i Zapisz model regresją [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , która wskazuje, czy etykietkę zapłacono w podróży w Warszawa taksówki podróży i taryfy zestawie danych. Model przygotowaniu przy użyciu krzyżowe sprawdzanie poprawności (OKS) i kominów hyperparameter wykonywane przy użyciu kodu niestandardowego, którą można zastosować do dowolnego algorytmów nauki w MLlib.   

>[AZURE.NOTE] Wykonanie tego kodu niestandardowego OKS może potrwać kilka minut.

**Przeszkolenie modelu regresją, używając OKS i kominów hyperparameter**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

Współczynniki: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Punkt przecięcia:-0.0111216486893

Czas wykonywania powyżej komórki: 14.43 sekund


**Ocena modelu klasyfikacji binarny z metryki standardowy**

Kod w tej sekcji pokazano, jak ma być obliczona modelu regresją przed test-zbioru danych, w tym wykres krzywej ROC.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
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
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

Obszarze PR = 0.985336538462

Obszarze ROC = 0.983383274312

Statystyka podsumowania

Dokładność = 0.984174341679

Odwoływanie = 0.984174341679

F1 Wynik = 0.984174341679

Czas wykonywania powyżej komórki: 2.67 sekund


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
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**WYNIK**

![Krzywa ROC regresją podejściu ogólne](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Zmiana jest zachowywana modelu w obiektów blob zużycia przyszłych**

Kod w tej sekcji pokazano, jak zapisać model regresją zużycia.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**WYNIK**

Czas wykonywania powyżej komórki: 34.57 sekund


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Funkcja osoby MLlib CrossValidator planowana z modelu regresją (elastyczne regresji)

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i Zapisz model regresją [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , która wskazuje, czy etykietkę zapłacono w podróży w Warszawa taksówki podróży i taryfy zestawie danych. Model przygotowaniu przy użyciu krzyżowe sprawdzanie poprawności (OKS) i kominów hyperparameter mając z funkcją planowana MLlib CrossValidator OKS wyczyść parametru.   


>[AZURE.NOTE] Wykonywanie tego kodu OKS MLlib może potrwać kilka minut.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**WYNIK**

Czas wykonywania powyżej komórki: 107.98 sekund


**Wykreślić krzywą ROC.**

*PredictionAndLabelsDF* jest zarejestrowany jako tabelę *tmp_results*w poprzedniej komórki. *tmp_results* może służyć do wykonania kwerendy i wynik do ramki danych sqlResults dla Rysowanie. Oto kod.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Poniżej przedstawiono kod, aby wykreślić krzywą ROC.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**WYNIK**

![Krzywa ROC regresją przy użyciu CrossValidator i MLlib](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Przypadkowe las klasyfikacji

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie regresji las losowej, która wskazuje, czy etykietkę zapłacono w podróży w Warszawa taksówki podróży i taryfy zestawie danych.


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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


**WYNIK**

Obszarze ROC = 0.985336538462

Czas wykonywania powyżej komórki: 26.72 sekund


### <a name="gradient-boosting-trees-classification"></a>Gradient zwiększający klasyfikacji drzewa

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenić i zapisać gradientu zwiększający modelu drzew, który wskazuje, czy w podróży w podróży taksówki Warszawa zapłacono porady i taryfy zestawu danych.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
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

**WYNIK**

Obszarze ROC = 0.985336538462

Czas wykonywania powyżej komórki: 28.13 sekund


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Przewidywanie kwota Porada z modelami regresji (bez użycia OKS)

W tej sekcji przedstawiono, jak trzech modeli użycia dla zadania regresji przewidywania ilość poradę opłacona wycieczki taksówki na podstawie innych funkcji porada. Modele przedstawione są:

- Umorzyć regresji liniowej.
- Przypadkowe las
- Drzewa zwiększający gradientowego

Te modele zostały opisane we wprowadzeniu. Każdy model konstruowania sekcję kodu jest podzielony na czynności: 

1. **Szkolenie modelu** danych za pomocą jednego zestawu parametru
2. **Ocena modelu** z zestawem danych test zawierającym metryki
3. **Zapisywanie modelu** w obiektów blob zużycia przyszłych   


>AZURE Uwaga: Krzyżowego sprawdzania poprawności nie jest używany z modelami trzy regresji w tej sekcji, ponieważ została ona wyświetlana szczegółowo dla modeli regresją. Przykład przedstawiający, jak za pomocą OKS elastyczne netto regresji liniowej znajduje się w dodatku w tym temacie.


>AZURE Uwaga: W naszym środowisko, mogą występować problemy ze zbliżania modeli LinearRegressionWithSGD i parametry należy zmienić i zoptymalizowane dokładnie w celu uzyskania prawidłowy model. Skalowanie zmiennych znacznie ułatwia spójności. Zamiast LinearRegressionWithSGD można także elastyczne regresji netto, przedstawiony w dodatku do tego tematu.


### <a name="linear-regression-with-sgd"></a>Z SGD linią regresji liniowej.

Kod w tej sekcji przedstawiono, jak za pomocą funkcji skalować przeszkolenie regresji liniowej używaną stochastycznego zejścia gradientowego (SGD) na potrzeby optymalizacji i jak wynik, ocenianie i zapisać model w magazyn obiektów Blob platformy Azure (WASB).

>[AZURE.TIP] W naszym środowisko mogą występować problemy ze zbliżania modeli LinearRegressionWithSGD i parametry należy zmienić i zoptymalizowane dokładnie w celu uzyskania prawidłowy model. Skalowanie zmiennych znacznie ułatwia spójności.


    # LINEAR REGRESSION WITH SGD 

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
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK**

Współczynniki: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

ODCIĘTA: 0.854507624459

RMSE = 1.23485131376

R sqr = 0.597963951127

Czas wykonywania powyżej komórki: 38.62 sekund


### <a name="random-forest-regression"></a>Las losowo regresji

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie modelu losowe las, który przewiduje kwota Porada dla danych podróży taksówki Warszawa.   

>[AZURE.NOTE] Sprawdzanie poprawności krzyżowe z parametrem zmiennymi przy użyciu kodu niestandardowego znajduje się w dodatku.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
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

**WYNIK**

RMSE = 0.931981967875

R sqr = 0.733445485802

Czas wykonywania powyżej komórki: 25.98 sekund


### <a name="gradient-boosting-trees-regression"></a>Gradient zwiększający regresji drzewa

Kod w tej sekcji przedstawiono sposób przeszkolenie, ocenianie i zapisywanie gradientu zwiększający modelu drzew, który przewiduje kwota Porada danych Warszawa taksówki podróży.

**Przeszkolenie i oceny**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

RMSE = 0.928172197114

R sqr = 0.732680354389

Czas wykonywania powyżej komórki: 20.9 sekund


**Kreśl**
    
*tmp_results* jest zarejestrowany jako tabelę programu Hive w poprzedniej komórki. Wyniki z tabeli są dane wyjściowe do ramki danych *sqlResults* dla Rysowanie. Oto kod

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Poniżej przedstawiono kod kreślenia danych przy użyciu serwera Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![A przewidziane — Porada kwoty rzeczywiste](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Dodatek: Regresji dodatkowe zadania krzyżowego sprawdzania poprawności przy użyciu parametru przeszukiwanie

Ten dodatek zawiera kod, jak wykonać OKS przy użyciu elastyczne netto dla regresji liniowej i sposobów wykonywania OKS z wyczyść parametrów przy użyciu kodu niestandardowego dla regresji las losowe.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Krzyżowe sprawdzanie poprawności przy użyciu elastyczne netto dla regresji liniowej.

Kod w tej sekcji pokazano, jak krzyżowe sprawdzanie poprawności przy użyciu elastyczne netto dla regresji liniowej i jak ma być obliczona modelu danych test.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

Czas wykonywania powyżej komórki: 161.21 sekund

**Szacowanie z metryki R SQR**

*tmp_results* jest zarejestrowany jako tabelę programu Hive w poprzedniej komórki. Wyniki z tabeli są dane wyjściowe do ramki danych *sqlResults* dla Rysowanie. Oto kod

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Poniżej przedstawiono kod, aby obliczyć R sqr.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**WYNIK**

R sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Krzyżowe sprawdzanie poprawności z wyczyść parametrów przy użyciu kodu niestandardowego dla regresji losowe las

Kod w tej sekcji przedstawiono sposobu krzyżowe sprawdzanie poprawności z wyczyść parametrów przy użyciu kodu niestandardowego dla regresji losowe las i ocena modelu przed testowymi danymi.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK**

RMSE = 0.906972198262

R sqr = 0.740751197012

Czas wykonywania powyżej komórki: 69.17 sekund


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Oczyść obiekty z pamięci i lokalizacji modelu wydruku

Używanie `unpersist()` usunięcie obiektów w pamięci podręcznej.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
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


**WYNIK**

PythonRDD [122] u RDD u PythonRDD.scala: 43


**Wydruku ścieżkę dostępu do plików modelu może być używany w notesie zużycie.** Aby używać i wynik niezależnych zestawu danych, należy skopiować i wkleić te nazwy pliku w "Notes zużycie".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**WYNIK**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Co to jest dalej?

Teraz, gdy Spark MlLib utworzony modeli regresji i klasyfikacji, możesz przystąpić do Dowiedz się, jak wynik i oceny tych modeli.

**Modelu zużycie:** Aby dowiedzieć się, jak wynik i oceny modeli klasyfikacji i regresji, utworzonych w tym temacie, zobacz [wynik i oceny modeli nauki wbudowany Spark komputera](machine-learning-data-science-spark-model-consumption.md).
