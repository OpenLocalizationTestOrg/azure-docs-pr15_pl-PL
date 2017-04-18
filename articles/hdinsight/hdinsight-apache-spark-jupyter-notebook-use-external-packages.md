<properties 
    pageTitle="Korzystanie z notesów Jupyter w iskrowym Apache klastrów na HDInsight zewnętrznych pakietów | Azure"
    description="Instrukcjami krok po kroku dotyczące konfigurowania notesów Jupyter dostępne z klastrów HDInsight Spark za pomocą zewnętrznego pakietów Spark." 
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
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Korzystanie z notesów Jupyter w iskrowym Apache klastrów na HDInsight Linux pakietów zewnętrznych

>[AZURE.NOTE] Ten artykuł dotyczy Spark 1.5.2 na HDInsight 3.3 i Spark 1.6.2 na HDInsight 3.4. 

Dowiedz się, jak skonfigurować Notes Jupyter w iskrowym Apache klaster na HDInsight (Linux), aby użyć zewnętrznego, pakietów przyczynić się społeczności, które nie są wyświetlane w nowym polu w klastrze. 

Możesz wyszukać [repozytorium środowiska Maven](http://search.maven.org/) , aby uzyskać pełną listę pakietów, które są dostępne. Listę dostępnych pakietów można uzyskać również z innych źródeł. Na przykład pełną listę przyczynić się społeczności pakietów jest dostępna w [Iskrowym pakietów](http://spark-packages.org/).

W tym artykule dowiesz się, jak używać pakietu [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) z notesu Jupyter.

##<a name="prerequisites"></a>Wymagania wstępne

Użytkownik musi mieć następujące czynności:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Korzystanie z notesów Jupyter pakietów zewnętrznych 

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij przycisk **Szybkie łącza**, a następnie z karta **Pulpit nawigacyjny klaster** kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu. Kliknij pozycję **Nowy**, a następnie kliknij **Spark**.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Tworzenie nowego notesu Jupyter")

3. Nowy notes zostanie utworzony i otwarty o nazwie Untitled.pynb. Kliknij nazwę notesu, u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Podaj nazwę notesu")

4. Będą `%%configure` magiczną, aby skonfigurować Notes, aby użyć zewnętrznego pakietu. W notesach korzystające z opakowania zewnętrznego, upewnij się, połączenia `%%configure` Magiczna w pierwszej komórce kodu. Dzięki temu, że jądro jest skonfigurowany do używania pakietu, przed uruchomieniem sesji.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Jeśli zapomnisz Konfigurowanie jądrze w pierwszej komórce, możesz użyć `%%configure` z `-f` parametr, ale będzie Uruchom ponownie sesję i wszystkie informacje o postępie zostaną utracone.

5. W powyższych wstawek `packages` oczekuje wykaz współrzędnych środowiska maven w centralnym repozytorium środowiska Maven. W tym wstawek `com.databricks:spark-csv_2.10:1.4.0` jest współrzędnych środowiska maven pakietu **spark csv** . Poniżej opisano, jak utworzyć współrzędne pakietu.

    . Zlokalizuj pakiet w repozytorium środowiska Maven. Ten samouczek używamy [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Z repozytorium gromadzenie wartości dla **Identyfikator grupy**, **ArtifactId**i **wersji**.

    ![Używanie zewnętrznych pakietów z Jupyter notesu] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Używanie zewnętrznych pakietów z Jupyter notesu")

    c. Łączenia trzech wartości, dwukropkiem (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Uruchamianie kodu komórki z `%%configure` magiczną. Skonfiguruj źródłowych sesji Livy, aby użyć pakietu podana. W kolejnych komórek w notesie możesz teraz używać pakietu, tak jak pokazano poniżej.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Następnie możesz uruchomić wstawki, takich jak pokazano poniżej, aby wyświetlić dane z dataframe został utworzony w poprzednim kroku.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych Instalacja grzewczo-Wentylacyjna danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
