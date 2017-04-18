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


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Tworzenie aplikacji maszynowego uczenia do uruchamiania w iskrowym Apache klastrów na HDInsight Linux

Dowiedz się, jak tworzyć maszynowego uczenia aplikacji przy użyciu klaster Apache Spark w HDInsight. W tym artykule pokazano, jak za pomocą notesu Jupyter dostępne z klastrem tworzenie i testowanie naszych aplikacji. Aplikacja używa przykładowych danych HVAC.csv, który jest dostępny na wszystkich klastrów domyślnie.

**Wymagania wstępne dotyczące:**

Użytkownik musi mieć następujące czynności:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Pokaż dane

Przed rozpoczęciem firma Microsoft, tworzenie aplikacji, powiadom nas opis struktury danych i rodzaju analizy, która będzie czynności na danych. 

W tym artykule firma Microsoft korzysta z pliku danych **HVAC.csv** próbki, który jest dostępna na koncie Azure miejsca do magazynowania, który skojarzony z klastrem HDInsight. W ramach konta miejsca do magazynowania plik znajduje się na **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Pobierz i Otwórz plik, aby uzyskać migawkę danych.  

![Instalacja grzewczo-Wentylacyjna migawkę danych] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Migawkę danych instalacji grzewczo-Wentylacyjnych")

Dane zawiera temperatury docelowej i rzeczywisty temperatury budynku zawierającą zainstalowanych systemów instalacji grzewczo-Wentylacyjnych. Załóżmy kolumny **systemowej** reprezentuje system ID, a kolumna **SystemAge** liczba lat system GRZEWCZO został wprowadzane w budynku.

Używamy tych danych do przewidywania, czy budynku będzie hotter lub colder oparte na temperatury docelowej, danej identyfikator systemu i wieku systemu.

##<a name="app"></a>Pisanie aplikację szkoleń komputera przy użyciu Spark MLlib

W tej aplikacji używamy potok Spark ML przeprowadzić klasyfikacja dokumentu. W potoku firma Microsoft dokumentu można podzielić wyrazy, konwertowanie wyrazy wektor liczbowe funkcji, a na koniec tworzenia modelu przewidywania za pomocą funkcji wektorów i etykiet. Wykonaj poniższe czynności, aby utworzyć aplikację.

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu. Kliknij pozycję **Nowy**, a następnie kliknij pozycję **PySpark**.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Tworzenie nowego notesu Jupyter")

3. Nowy notes zostanie utworzony i otwarty o nazwie Untitled.pynb. Kliknij nazwę notesu, u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Podaj nazwę notesu")

3. Ponieważ utworzono Notes za pomocą jądrze PySpark, nie musisz utworzyć dowolny konteksty jawnie. Konteksty iskrowym i gałąź jest automatycznie tworzona automatycznie po uruchomieniu pierwszą komórkę kodu. Możesz rozpocząć, importując typów, które są wymagane do tego scenariusza. Wklej poniższy fragment w pustej komórce, a następnie naciśnij klawisze **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Należy teraz ładowanie danych (hvac.csv), przeanalizuj je i używać go do przeszkolenie modelu. W tym należy zdefiniować funkcję, która sprawdza, czy rzeczywiste temperatury budynku jest większa niż temperatura docelowej. Jeśli rzeczywista temperatury jest większy, budynku jest ciepłej, oznaczona wartość **1.0**. Jeśli rzeczywista temperatura jest mniejsze, budynku jest zimnej, oznaczona wartość **0,0**. 

    Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfigurowanie procesu nauki komputera Spark, która składa się z trzech etapów: mechanizmu dzielenia na tokeny, hashingTF i lr. Aby uzyskać więcej informacji o tym, co potok i sposobu jej działania zobacz <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark maszynowego uczenia procesu</a>.

    Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Dopasuj planowana do dokumentu szkoleniowego. Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Sprawdź dokument szkolenie punkt kontrolny postępie z aplikacją. Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.

        training.show()

    To powinien podać wynik podobny do następującego:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Przejdź wstecz i sprawdź dane wyjściowe pod kątem zgodności nieprzetworzonych pliku CSV. Na przykład pierwszy wiersz pliku CSV zawiera te dane:

    ![Instalacja grzewczo-Wentylacyjna migawki danych] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Migawka danych instalacji grzewczo-Wentylacyjnych")

    Zwróć uwagę, jak rzeczywisty temperatury jest mniejsza niż docelowej temperatury sugerujący, że budynku jest zimnej. W związku z tym rezultaty szkolenie wartość **etykietę** w pierwszym wierszu jest **0,0**, co oznacza, że nie jest ciepłej budynku.

8.  Przygotowywanie zbioru danych do uruchomienia modelu przeszkolony pod kątem zgodności. Aby to zrobić, czy firma Microsoft przechodzą na identyfikator systemu i wiek system (oznaczona jako **SystemInfo** w wyniku szkolenie), a model będzie przewidywanie czy konstrukcyjnych za pomocą tego Identyfikatora systemu i wiek system będzie hotter (oznaczona 1.0) lub chłodniej (oznaczona 0,0).

    Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Na koniec umożliwienie przewidywań testowymi danymi. Wklej poniższy fragment w pustej komórce i naciśnij klawisze **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Powinien zostać wyświetlony wynik podobny do następującego:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    W pierwszym wierszu prognozowania widać, że systemu instalacji grzewczo-Wentylacyjnych z 20 identyfikator i system wiek 25 lat budynku będzie ciepłej (**przewidywania = 1,0**). Wartość w polu pierwszy DenseVector (0.49999) odpowiada przewidywania 0.0 i drugą wartość (0.5001) odpowiada przewidywania 1.0. W wyniku kwerendy, nawet jeśli druga wartość jest tylko nieznacznie wyższy modelu zawiera **przewidywania = 1,0**.

11. Po zakończeniu uruchamiania aplikacji należy zamknięty Notes, aby zwolnić zasoby. Aby to zrobić, w menu **plik** Notes, kliknij polecenie **Zamknij i zatrzymanie**. Ten zostanie zamknięty, a następnie zamknij notes.
           

##<a name="anaconda"></a>Za pomocą Anaconda scikit — informacje biblioteki szkoleniowe na komputerze

Klastrów Apache Spark na HDInsight zawiera bibliotek Anaconda. Obejmuje to także **scikit-informacje** biblioteki szkoleniowe na komputerze. Biblioteka zawiera również różnych zestawów danych, które służą do tworzenia aplikacji przykładowych bezpośrednio z notesu Jupyter. Przykłady przy użyciu scikit — Dowiedz się, biblioteki, zobacz [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Zobacz też

* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
