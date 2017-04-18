<properties
    pageTitle="Wynik modeli nauki wbudowany Spark komputera | Microsoft Azure"
    description="Jak modeli nauki wynik, które były przechowywane w magazyn obiektów Blob platformy Azure (WASB)."
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

# <a name="score-spark-built-machine-learning-models"></a>Wynik modeli nauki wbudowany Spark komputera 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

W tym temacie opisano sposób ładowania maszynowego nauki (ML) modeli, w których został utworzony przy użyciu Spark MLlib i przechowywane w magazyn obiektów Blob platformy Azure (WASB) oraz sposobu ich wynik z zestawów danych również były przechowywane w WASB. Informuje jak wstępnie przetwarzanie danych wejściowych, przekształcanie funkcje za pomocą funkcji indeksowania i kodowania w MLlib zestaw narzędzi, jak utworzyć punkt oznaczona etykietą obiektu danych, który może być używany jako dane wejściowe wyników z modelami ML. Modele wyników obejmują regresji liniowej, regresją, losowe modeli las i gradientu zwiększenia modeli drzewa.


## <a name="prerequisites"></a>Wymagania wstępne

1. Potrzebujesz konta usługi Azure i HDInsight Spark możesz potrzebna 1.6 Spark 3.4 HDInsight klaster do przeprowadzenia tego instruktażu. Zobacz [Omówienie danych nauki przy użyciu Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) Aby uzyskać instrukcje dotyczące spełnienia tych wymagań. Ten temat zawiera również zapoznać się z opisem taksówki 2013 Warszawa dane używane w tym miejscu i instrukcje dotyczące wykonanie kodu z notesu Jupyter w klastrze Spark. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** Notes, który zawiera przykłady kodu, w tym temacie jest dostępna w [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Musisz też utworzyć maszynowego uczenia modeli punktowanych w tym miejscu pracy za pomocą [badań danych i modelowanie z Spark](machine-learning-data-science-spark-data-exploration-modeling.md) tematu.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Konfiguracji: lokalizację, bibliotek i kontekst Spark wstępnie ustawionych

Spark będzie mógł odczytywanie i zapisywanie do Azure magazyn obiektów Blob (WASB). Aby żadnych istniejących danych przechowywanych mogą być przetwarzane przy użyciu iskrowym i ponownie przechowywane w WASB wyniki.

Aby zapisać modeli lub pliki w WASB, ścieżka nie trzeba określać poprawnie. Kontener domyślny dołączone do klastrów Spark można odwoływać się przy użyciu ścieżki począwszy od: *"wasb--"*. Poniższy przykład kodu Określa lokalizację danych do odczytu i ścieżkę katalogu miejsca do magazynowania modelu, do którego jest zapisany model danych wyjściowych. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Ustawianie ścieżki katalogu do lokalizacji przechowywania w WASB

Modele są zapisywane w: "wasb: / / / użytkownika/remoteuser/NYCTaxi/modeli". Jeśli ta ścieżka nie jest ustawiona prawidłowo, modeli nie są ładowane wyników.

Scored wyniki zostały zapisane w: "wasb: / / / użytkownika/remoteuser/NYCTaxi/ScoredResults". Jeśli ścieżka do folderu jest niepoprawny, wyniki nie są zapisywane w tym folderze.   


>[AZURE.NOTE] Lokalizacje plików ścieżki można kopiować i wklejać do symboli zastępczych w kod z wyników ostatniej komórki **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notesu.   


Oto kod, aby ustawić ścieżki katalogów: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**WYNIK:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Importowanie biblioteki

Ustaw kontekst spark i zaimportować niezbędne bibliotek za pomocą następującego kodu

    #IMPORT LIBRARIES
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

Jądra PySpark, które są dostarczane z notesami Jupyter mają wstępnie ustawionych kontekst. Dlatego nie trzeba ustawić Spark lub tworzenia konteksty gałęzi jawnie przed rozpoczęciem pracy z aplikacją. Są domyślnie dostępne dla Ciebie. Te konteksty są:

- SC - dla Spark 
- sqlContext - gałęzi

Jądro PySpark udostępnia kilka wstępnie zdefiniowanych "magics", które są specjalne polecenia wywołujących z %%. Istnieją dwa polecenia, które są używane w tych przykładach kodu.

- **%% lokalny** Określić, że kod w kolejnych wierszach jest wykonywane lokalnie. Kod musi być prawidłowy kod Python.
- **%% sql -o<variable name>** 
- Wykonuje gałęzi zapytanie sqlContext. Jeśli parametr -o zostanie przekazany, wynik kwerendy jest umieszczany w %% lokalnych warunków Python jako Pandas dataframe.
 

Aby uzyskać więcej informacji na temat jądra Jupyter notesów i wstępnie zdefiniowanych "magics" które zapewniają, zobacz [jądra dostępne dla Jupyter notesów przy użyciu klastrów HDInsight Spark Linux na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Mogły zjeść tej ostatniej danych i Tworzenie ramki oczyszczonych danych

Ta sekcja zawiera kod serii zadań wymaganych do mogły zjeść tej ostatniej dane, które mają być uzyskał. Przeczytaj w próbce 0,1% sprzężonych taksówki podróży i taryfy pliku (przechowywane jako pliku .tsv), format danych, a następnie tworzy ramki Oczyść danych.

Pliki podróży i taryfy taksówki zostały podstawie połączonych za pomocą procedury opisane w: [proces nauki danych zespołu w działaniu: używania klastrów HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) tematu.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 46.37 sekund


## <a name="prepare-data-for-scoring-in-spark"></a>Przygotowywanie danych wyników w iskrowym 

W tej sekcji przedstawiono sposób indeks, kodowanie i skalowanie kategorii funkcje ich przygotowanie do użycia w MLlib nadzorowany nauki algorytmów klasyfikacji i regresji.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Funkcja przekształcenie: indeks i kodowanie kategorii funkcje o wprowadzenie informacji w modelach wyników. 

W tej sekcji przedstawiono sposób indeksu kategorii danych przy użyciu `StringIndexer` i funkcje kodowanie `OneHotEncoder` do modeli.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kodowane kolumny ciąg etykiet do kolumny wskaźników etykiety. Indeksy są uporządkowane według częstotliwości etykiety. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapy kolumny wskaźników etykiety do kolumny wektorów binarny z co najwyżej jednego jedną wartość. Kodowanie umożliwia algorytmów oczekiwać ciągły wartości funkcji, takich jak regresją mają być stosowane do kategorii funkcji.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 5.37 sekund


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Tworzenie obiektów RDD z tablicami funkcji podanie danych wejściowych do modeli

Ta sekcja zawiera kod, którego pokazano, jak indeks danych tekstowych kategorii jako obiekt RDD i hot jeden kodowanie go, aby można było używać przeszkolenie i przetestować regresją MLlib i modeli drzewa. Indeksowane dane są przechowywane w obiektach [Mechanizm Distributed zestawu danych (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Poniżej przedstawiono podstawowe abstrakcji w iskrowym. Obiekt RDD reprezentuje niezmienne kolekcję podzielone na partycje elementy, które mogą być stosowane równolegle z Spark.

Również zawiera kod, który przedstawia sposób do przeskalowania danych z `StandardScalar` podany przez MLlib do użytku w regresji liniowej z stochastycznego gradientu zejścia (SGD), popularne algorytm szkoleniowe szeroką gamę modeli nauki komputera. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) jest używana do skalowania funkcje do Odchylenie jednostki. Funkcja skalowania, nazywane także normalizacji danych uzyskanie, że funkcje z wartościami powszechnie rozchodów są nie przyznał nadmiarowe oceny w celu funkcji. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**WYNIK:**

Czas wykonywania powyżej komórki: 11.72 sekund


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Wynik z modelem regresji logistycznych i zapisywanie danych wyjściowych blob

Kod w tej sekcji przedstawiono sposób ładowanie modelu regresji logistyczne, który został zapisany w magazynie obiektów blob platformy Azure i używać go do prognozowania, czy etykietkę zapłacono w podróży taksówki, wynik go z metryki standardowej klasyfikacji, a następnie zapisz i kreślenia wyników, aby magazyn obiektów blob. Scored wyniki są przechowywane w obiektach RDD. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**WYNIK:**

Czas wykonywania powyżej komórki: 19.22 sekund


## <a name="score-a-linear-regression-model"></a>Wynik modelu regresji liniowej.

Umożliwia możemy przeszkolenie modelu regresji liniowej przewidywanie ilość Porada opłaconej przy użyciu stochastycznego gradientowego zejścia (SGD) dla optymalizacji [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) . 

Kod w tej sekcji przedstawiono sposób ładowanie modelu regresji liniowej z magazynem obiektów blob Azure, wynik przy użyciu zmiennych skalować, a następnie zapisz wyniki obiektów blob.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**WYNIK:**

Czas wykonywania powyżej komórki: 16.63 sekund


## <a name="score-classification-and-regression-random-forest-models"></a>Wynik losowe modeli las klasyfikacji i regresji

Kod w tej sekcji przedstawiono sposób załadować zapisaną klasyfikacji i regresji losowe las modeli zapisane w magazynie obiektów blob platformy Azure wynik ich wydajności za pomocą standardowej klasyfikatora i miar regresji, a następnie zapisz wyniki magazyn obiektów blob.

[Losowe lasów](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) są komplety drzewa decyzji.  Łączą wiele algorytmów, aby zmniejszyć ryzyko overfitting. Przypadkowe lasów może obsługiwać kategorii funkcji rozszerzenia z ustawieniem multiclass klasyfikacji, nie wymaga funkcji skalowania i będą mogli przechwycić nieliniowość i funkcji interakcji. Przypadkowe lasów to jeden z najbardziej popularnych maszynowego uczenia modeli klasyfikacji i regresji.

[Spark.mllib](http://spark.apache.org/mllib/) obsługuje losowe lasów klasyfikacji binarne i multiclass i regresji, za pomocą funkcji zarówno ciągły, jak i kategorii. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**WYNIK:**

Czas wykonywania powyżej komórki: 31.07 sekund


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Wynik klasyfikacji i regresji gradientu zwiększenia drzewa modeli

Kod w tej sekcji przedstawiono sposób ładowania klasyfikacji i regresji gradientu zwiększenia drzewa modeli z magazynem obiektów blob Azure, wynik ich wydajności za pomocą standardowej klasyfikatora i miar regresji, a następnie zapisz wyniki magazyn obiektów blob. 

**Spark.mllib** obsługuje GBTs klasyfikacji binarne i regresji, za pomocą funkcji zarówno ciągły, jak i kategorii. 

[Drzewa zwiększający gradientowego](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) są komplety drzewa decyzji. GBTs przeszkolenie algorytmów wielokrotnie powtarzane, aby zminimalizować utratę funkcji. GBTs mogą obsługiwać funkcje kategorii, nie wymaga funkcji skalowania i będą mogli przechwycić nieliniowość i funkcji interakcji. Można też nimi ustawienie klasyfikacji multiclass.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**WYNIK:**

Czas wykonywania powyżej komórki: 14.6 sekund


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Oczyść obiekty z pamięci i Drukuj uzyskał lokalizacje plików

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**WYNIK:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Używanie modeli Spark za pośrednictwem interfejsu sieci web

Spark udostępnia mechanizm zdalne przesyłanie zadań lub interakcyjnych kwerend za pośrednictwem interfejsu REST z składnik o nazwie Livy. Livy jest domyślnie włączona w iskrowym HDInsight klaster. Aby uzyskać więcej informacji na temat Livy zobacz: [Przesyłanie Spark zadań przy użyciu Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Umożliwia Livy zdalne przesyłanie zadanie wsadowe wyniki plik, który jest przechowywany w obiektów blob platformy Azure, a następnie zapisuje wyniki do innego obiektów blob. W tym celu należy przekazać skrypt Python  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) obiektów blob klastrze Spark. Narzędzie, takie jak **Microsoft Azure miejsca do magazynowania Explorer** lub **AzCopy** umożliwia Skopiuj skrypt do obiektów blob klaster. W naszym przypadku możemy przekazać skrypt do ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Klawisze dostępu, które możesz potrzebujesz można znaleźć w portalu dla konta magazynu skojarzone z klastrem Spark. 


Po przesłaniu do tej lokalizacji tego skryptu uruchamiania w klastrze Spark w kontekście rozłożone. Ładuje modelu i uruchomić przewidywań nad plikami wprowadzania danych oparty na modelu.  

Ten skrypt można wywołać zdalnie dokonując prostego żądania HTTPS i umieść na Livy.  Oto polecenia zwinięcie do utworzenia żądania HTTP zdalnego wywołania skrypt Python. Zamień CLUSTERLOGIN, CLUSTERPASSWORD, NAZWAKLASTRA odpowiednie wartości dla klaster Spark.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Można używać dowolnego języka w systemie zdalnym do wywołania zadania Spark za pośrednictwem Livy dokonując prostego połączenia HTTPS z uwierzytelniania podstawowego.   


>[AZURE.NOTE] Jest łatwe w użyciu biblioteki żądania Python, dokonując to połączenie HTTP, ale nie jest obecnie zainstalowany domyślnie w funkcjach Azure. Dlatego zamiast tego używane są starsze bibliotek HTTP.   


Oto kod Python połączenia HTTP:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Można również dodać kod Python [Funkcje Azure](https://azure.microsoft.com/documentation/services/functions/) wyzwalać przesyłania zadania Spark, że wyniki obiektów blob oparte na różnych zdarzeń, takich jak czasomierz, tworzenie lub aktualizowanie obiektów blob. 

Jeśli wolisz środowisko bezpłatnego klienta kod umożliwia [Azure logiki aplikacji](https://azure.microsoft.com/documentation/services/app-service/logic/) wywołania partii Spark wyników definiujące akcji HTTP **Projektanta aplikacji logiki** i ustawiając określonych parametrów. 

- W portalu Azure utworzenia nowej aplikacji logiczny, wybierając pozycję **+ Nowe** -> **Web + Mobile** -> **Logiczny aplikacji**. 
- Aby wyświetlić **Projektanta aplikacji logiczny**, wprowadź nazwę aplikacji logiki i Planowanie usługi aplikacji.
- Wybierz akcję HTTP i wprowadź odpowiednie parametry pokazano na poniższym rysunku:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Co to jest dalej? 

**Krzyżowego sprawdzania poprawności i hyperparameter zmiennymi**: zobacz [Zaawansowane badań danych i modelowanie z Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) jak można modeli szkolenia przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru.
