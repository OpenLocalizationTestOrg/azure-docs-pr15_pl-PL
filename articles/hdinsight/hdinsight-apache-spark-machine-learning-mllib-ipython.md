<properties 
    pageTitle="Tworzenie aplikacji nauki komputera na HDInsight za pomocą Apache Spark | Microsoft Azure" 
    description="Instrukcje krok po kroku dotyczące sposobu używania notesów z Apache Spark do tworzenia aplikacji nauki komputera" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Komputer nauki: analiza przewidywanych na żywność kontroli danych przy użyciu MLlib z Apache Spark klaster w systemie HDInsight Linux

> [AZURE.TIP] Ten samouczek jest także dostępny jako notesu Jupyter w klastrze Spark (Linux), który można tworzyć w HDInsight. Środowisko notesu pozwala na uruchamianie wstawki Python z notesu. Aby wykonać samouczek z poziomu programu Notes, utworzyć klaster Spark, uruchamianie notesu Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a następnie uruchom Notes **uczenie się komputera Spark - przewidywanych analizy danych inspekcji żywność MLLib.ipynb** w folderze **Python** .


W tym artykule przedstawiono sposób użycia **MLLib**, maszynowego wbudowanych w iskrowym uczenia bibliotek do wykonywania prostych analiz przewidywanych na otwartej zestawu danych. MLLib to podstawowa Biblioteka Spark, który zapewnia różne narzędzia, które są przydatne w przypadku komputera nauki zadań, takich jak narzędzia, które są odpowiednie dla:

* Klasyfikacja

* Regresji

* Klaster

* Modelowanie tematu

* Dekompozycji pojedynczą wartość (SVD) i głównych części analizy (UPW)

* Hipoteza badania i obliczania statystyk próbki

W tym artykule przedstawiono proste podejście do *klasyfikacji* za pośrednictwem regresją.

## <a name="what-are-classification-and-logistic-regression"></a>Co to są klasyfikacji i regresją?

*Klasyfikacji*, często maszynowego uczenia zadania, to proces sortowania danych wejściowych na kategorie. Jest to zadanie algorytmu klasyfikacji, aby dowiedzieć się, jak przypisać "etykiety" do wprowadzania danych, które można wprowadzić. Na przykład może wziąć pod algorytmu nauki maszynowego akceptuje giełdowe jako danych wejściowych i zasoby są podzielone na dwie kategorie: zasoby, które powinny sprzedaży i zasobów, które należy zachować.

Regresją jest algorytmu używanego klasyfikacji. W iskrowym regresją interfejsu API są przydatne w przypadku *klasyfikacji binarne*lub klasyfikacji dane wejściowe do jednego z dwóch grup. Aby uzyskać więcej informacji na temat logistyczne strat zobacz [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Podsumowując proces regresją tworzy *logistyczne funkcji* , która umożliwia przewidywanie prawdopodobieństwo, że wprowadzania wektor należy w jednej grupie lub w drugim.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Co możemy wykonywanego w tym artykule?

Spark użyje do wykonywania niektórych przewidywanych analiz na danych inspekcji żywność (**Food_Inspections1.csv**), który został kupiony za pośrednictwem [portalu danymi Miasto Chicago](https://data.cityofchicago.org/). Tego zestawu danych zawiera informacje o kontroli żywność, które przeprowadzono w Chicago, w tym informacje o każdej działalności żywność, które zostało inspekcji, naruszenia, które zostały odnalezione (jeśli istnieje) i wyniki inspekcji. Plik CSV danych jest już dostępna na koncie miejsca do magazynowania skojarzone z klastrem w **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

W ramach poniższych kroków należy opracować model, aby zobaczyć, co jest potrzebne do powodzenie lub Niepowodzenie inspekcji żywność. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Rozpoczęcie tworzenia aplikację nauki komputera przy użyciu Spark MLlib

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu. Kliknij pozycję **Nowy**, a następnie kliknij pozycję **PySpark**.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Tworzenie nowego notesu Jupyter")

3. Nowy notes zostanie utworzony i otwarty o nazwie Untitled.pynb. Kliknij nazwę notesu, u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Podaj nazwę notesu")

3. Ponieważ utworzono Notes za pomocą jądrze PySpark, nie musisz utworzyć dowolny konteksty jawnie. Konteksty iskrowym i gałąź jest automatycznie tworzona automatycznie po uruchomieniu pierwszą komórkę kodu. Można rozpocząć tworzenie komputera nauki aplikacji, importując typy wymaganego w tym scenariuszu. Aby to zrobić, umieść kursor w komórce i naciśnij klawisze **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Konstruowanie wprowadzania dataframe

Czy firma Microsoft korzysta z `sqlContext` do wykonywania przekształcenia w danych strukturalnych. Pierwsze zadanie jest ładowanie przykładowych danych ((**Food_Inspections1.csv**)) do Spark SQL *dataframe*. 

1. Ponieważ nieprzetworzonych danych jest w formacie CSV, należy używać kontekstu Spark do pobrania każdy wiersz pliku do pamięci jako tekst niestrukturalne; następnie za pomocą biblioteki CSV i Python indywidualnie analizować każdego wiersza. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Teraz mamy pliku CSV jako RDD. Pozwól nam pobrać jeden wiersz z RDD, aby poznać schemat danych.


        inspections.take(1)


    Powinny zostać wyświetlone wyniki podobne do następujących:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Powyższe wyniki daje uzyskać ogólny obraz tego schemat wprowadzania pliku; plik zawiera nazwę każdej przedsiębiorstwa, typ przedsiębiorstwa, adres, dane inspekcji i lokalizacji, między innymi. Po zaznaczeniu kilku kolumn, które będą przydatne do naszych przewidywanych analizy i grupowanie wyników jako dataframe, której użyjemy następnie utwórz tabelę tymczasową.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Teraz mamy *dataframe* `df` którego oferujemy nasze analizy. Mamy także tabelę tymczasową połączeń **CountResults**. Zostały włączone 4 kolumnach zainteresowanie dataframe: **identyfikator**, **nazwę** **wyników**i **naruszenia**. 
    
    Załóż mały przykładowy danych:

        df.show(5)

    Powinny zostać wyświetlone wyniki podobne do następujących:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Opis danych

1. Zacznijmy uzyskanie zorientować się, co zawiera naszych zestawu danych. Na przykład jakie są różne wartości w kolumnie **wyników** ?


        df.select('results').distinct().show()

    
    Powinny zostać wyświetlone wyniki podobne do następujących:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Szybkie wizualizacji pomagają nam powodu o rozkładu dla tych wyników. Mamy już dane w tabeli tymczasowej **CountResults**. Poniższe zapytanie SQL może zostać uruchomiony w odniesieniu do tabeli w celu uzyskania lepszego zrozumienia rozkład wyniki.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql` Magia następuje `-o countResultsdf` gwarantuje, że wyniki kwerendy jest zachowywane lokalnie na serwerze Jupyter (zazwyczaj headnode klaster). Wynik jest zachowywane jako dataframe [Pandas](http://pandas.pydata.org/) , przy użyciu określonej nazwy **countResultsdf**.
    
    Powinny zostać wyświetlone wyniki podobne do następujących:
    
    ![Wyniki kwerendy SQL] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "Wyniki kwerendy SQL")

    Aby uzyskać więcej informacji na temat `%%sql` magiczną, a także inne dostępne z jądrze PySpark magics zobacz [jądra dostępne na Jupyter notesów przy użyciu klastrów Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Za pomocą Matplotlib, bibliotece użyte do utworzenia wizualizacji danych, aby utworzyć wykres. Ponieważ powierzchni muszą zostać utworzone z dataframe lokalnie trwałe **countResultsdf** , wstawki muszą zaczynać się `%%local` magiczną. Dzięki temu, że jest on uruchamiany lokalnie na serwerze Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Powinny zostać wyświetlone wyniki podobne do następujących:

    ![Wynik wynik](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Możesz zobaczyć, czy 5 odrębnych wyników zawierające inspekcji:
    
    * Business nie znajduje się 
    * Fail (niepowodzenie)
    * Przebieg
    * Pomocy technicznej z warunków, i
    * Wylogowywanie się z firm 

    Pozwól nam opracować model, który można odgadnięcie wyniki inspekcji żywność, podane naruszenia. Ponieważ regresją jest metoda binarne klasyfikacji, jest to właściwe rozwiązanie, aby pogrupować dane na dwie kategorie: **kończą się niepowodzeniem** i **przekazać**. "Zaliczone z warunków" nadal jest przebiegu, gdy mamy przeszkolenie modelu, firma Microsoft będzie uwzględniać dwóch wyniki równoważne. Dane z innych wyników ("Nie znaleziono firm", "z firm") nie są przydatne, więc możemy spowoduje usunięcie ich z naszych zestaw szkoleniowy. Powinien to być poprawny, ponieważ te dwie kategorie składają się bardzo mały stopień wyniki mimo to.

5. Pozwól nam wtyczce i przekonwertuj naszych istniejących dataframe (`df`) do nowej dataframe, gdzie każdej kontroli jest reprezentowane jako parę naruszenia etykiety. W naszym przypadku etykietę `0.0` reprezentuje to błąd etykietę `1.0` reprezentuje sukcesu i etykietę `-1.0` reprezentuje niektóre wyniki oprócz tych dwóch. Firma Microsoft zostaną odfiltrowane wyniki te podczas obliczania nowej ramki danych.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Załóżmy pobrać jeden wiersz z etykietami danych, aby zobaczyć, jak wygląda.


        labeledData.take(1)


    Powinny zostać wyświetlone wyniki podobne do następujących:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Tworzenie modelu regresją z dataframe wprowadzania danych

Nasze ostatnim zadaniem jest konwertowanie oznaczona etykietą danych do formatu, który może być analizowany przy regresją. Dane wejściowe algorytm regresją powinny być zestaw *funkcji etykiet wektor pary*"wektorowej funkcja" w przypadku wektor liczb, reprezentująca punktu wejścia w określony sposób. Tak potrzebujemy umożliwia konwertowanie kolumny "naruszenia", która jest półstrukturalnych i zawiera wiele komentarzy w niezależnej, do tablicy liczb rzeczywistych, które łatwo zrozumiałej komputera. 

Na jednym komputerze standardowy nauki podejście do przetwarzania w języku naturalnym jest przypisywanie każdego wyrazu różnią się "index", a następnie przekazać wektor na tym komputerze nauki algorytmu w taki sposób, że każdy indeks wartość zawiera względne częstotliwości tego wyrazu w ciągu tekstowym. 

MLLib umożliwia łatwe do wykonania tej operacji. Najpierw firma Microsoft będzie "tokenize" Każdy ciąg naruszenia, aby uzyskać pojedyncze wyrazy w każdy ciąg, a następnie użyjemy `HashingTF` do konwertowania każdy zestaw tokeny na wektorowej funkcja, który można następnie przekazać do algorytmu regresją tworzenia modelu. Zostaną wszystkie kroki przeprowadza się przy użyciu "potok".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Ocena modelu na osobnego badania zestawu danych

Firma Microsoft korzysta z modelu utworzone wcześniej *przewidzieć* jakie wyniki będą nowej inspekcji według naruszenia, które zostały obserwowane. Firma Microsoft szkolenia tego modelu w zestawie danych **Food_Inspections1.csv**. Pozwól nam za pomocą drugi zestaw danych, **Food_Inspections2.csv**do *oceny* siły tego modelu na nowe dane. Drugi zestaw danych (**Food_Inspections2.csv**) powinna już być w kontenerze miejsca do magazynowania domyślnym skojarzone z klastrem.

1. Poniżej wstawek tworzy nowe dataframe, **predictionsDf** , zawierający przewidywania generowane przez model. Wstawkę kodu tworzy również tabelę tymczasową podstawą **przewidywań** dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Powinny zostać wyświetlone wyniki podobne do następujących:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Przyjrzyj się jedną przewidywań. Uruchom ten fragment:

        predictionsDf.take(1)

    Zostanie wyświetlona prognozowania dla pierwszego wpisu w zestawie danych test.

3. `model.transform()` Metody dotyczą przekształcenia wszystkie nowe dane z tego samego schematu, a obciążony przewidywania sposobu klasyfikowania danych. Możemy kilka prostych statystyki uzyskanie zorientować się, jak dokładny zostały naszych przewidywań:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Wynik wygląda następująco:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Regresją za pomocą Spark daje dokładności modelu relacji między opisy naruszenia w języku angielskim i czy danej firmy czy powodzenie lub Niepowodzenie inspekcji żywność. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Tworzenie będący wizualną reprezentacją prognozowania

Firma Microsoft teraz skonstruować ostateczne wizualizacji ułatwi powodu o wyników test. 

1. Możemy uruchomić wyodrębnianie różnych przewidywań i wyniki z tabeli tymczasowej **przewidywań** utworzony wcześniej. Następujące kwerendy oddzielne wynik jako *true_positive*, *false_positive*, *true_negative*i *false_negative*. W tych zapytaniach możemy wyłączanie wizualizacji za pomocą `-q` , a także zapisać dane wyjściowe (za pomocą `-o`) jako dataframes, który można następnie używać za pomocą `%%local` magiczną. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Na koniec użyć następujących wstawki kodu w celu wygenerowania kreślenia przy użyciu **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Powinien zostać wyświetlony następujący wynik.
    
    ![Wynik prognozowania](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Na tym wykresie wynik "dodatni" odwołuje się do inspekcji jedzenie nie powiodło się, a wynik ujemny odwołuje się do przekazanego inspekcji.

## <a name="shut-down-the-notebook"></a>Zamknij Notes

Po zakończeniu uruchamiania aplikacji należy zamknięcia Notes, aby zwolnić zasoby. Aby to zrobić, w menu **plik** Notes, kliknij polecenie **Zamknij i zatrzymanie**. Ten zostanie zamknięty, a następnie zamknij notes.


## <a name="seealso"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych Instalacja grzewczo-Wentylacyjna danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dziennika witryny sieci Web przy użyciu Spark w HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji

* [Tworzenie autonomiczną aplikację za pomocą Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Zdalne uruchamianie zadania w klastrze Spark przy użyciu Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia

* [Tworzenie i przesyłanie Spark Scala aplikacji za pomocą dodatku Narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Zdalne debugowanie aplikacji Spark za pomocą wtyczki narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Notesy Zeppelin za pomocą klaster Spark na HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jądra dostępne dla notesu Jupyter w klastrze Spark dla HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
