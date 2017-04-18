<properties
    pageTitle="Klaster Spark na HDInsight Linux panelami Spark SQL z Jupyter na potrzeby analizy interakcyjnych | Microsoft Azure"
    description="Instrukcje krok po kroku szybko utworzyć iskry Apache klaster w HDInsight, a następnie użyj Spark SQL w notesach Jupyter uruchamianie kwerend interakcyjne."
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
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Rozpoczynanie pracy: tworzenie Apache Spark klaster w systemie HDInsight Linux i uruchamianie kwerend interakcyjnych, za pomocą Spark SQL

Dowiedz się, jak utworzyć klaster Apache Spark w HDInsight i uruchamianie kwerend interakcyjnych Spark SQL w klastrze Spark za pomocą [Jupyter](https://jupyter.org) notesu.

   ![Wprowadzenie do korzystania z Spark Apache w HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Rozpocząć korzystanie z Apache Spark w samouczku HDInsight. Kroki przedstawione: Tworzenie konta miejsca do magazynowania; utworzyć klaster; Uruchamianie instrukcji Spark SQL")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Przed rozpoczęciem tego samouczka, musisz mieć subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Klient Secure Shell (SSH)**: Linux, Unix lub OS X zachowywane systemów SSH klienta za pośrednictwem `ssh` polecenia. W przypadku systemu zalecamy [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Klucze bezpiecznego Shell (SSH) (opcjonalnie)**: można zabezpieczyć konto SSH, używane do łączenia się z klastrem za pomocą hasła lub klucz publiczny. Przy użyciu hasła pobiera szybkie rozpoczęcie pracy, a należy używać tej opcji, jeśli chcesz szybko utworzyć klaster i wykonywać niektóre zadania test. Przy użyciu klucza jest bezpieczniejsze, jednak wymaga dodatkowe ustawienia. Można użyć tej metody, podczas tworzenia klastrze produkcji. W tym artykule firma Microsoft korzysta z metody hasła. Aby uzyskać instrukcje dotyczące tworzenia i używania kluczy SSH z usługi HDInsight można znaleźć w następujących artykułach:

    -  Linux z komputera z systemem - [Użyj SSH z systemem Linux HDInsight (Hadoop) z Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Na komputerze z systemem Windows — [Użyj SSH z systemem Linux HDInsight (Hadoop) z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] W tym artykule użyto szablonu Menedżera zasobów Azure, aby utworzyć klaster Spark, który używa [Obiektów blob miejsca do magazynowania Azure przechowywania klaster](hdinsight-hadoop-use-blob-storage.md). Można także tworzyć klaster Spark, która używa [Magazynu Lake danych Azure](../data-lake-store/data-lake-store-overview.md) jako dodatkowego miejsca do magazynowania, oprócz blob miejsca do magazynowania Azure jako magazyn domyślny. Aby uzyskać instrukcje zobacz [Tworzenie klastrze HDInsight z magazynu Lake danych](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Utwórz klaster Spark

W tej sekcji można tworzyć klastrze wersji 3.4 HDInsight (Spark wersji 1.6.1) przy użyciu szablonu Menedżera zasobów Azure. Uzyskać informacje o wersji usługi HDInsight i ich poziomu zobacz [Przechowywanie wersji składnika HDInsight](hdinsight-component-versioning.md). Dla innych metod tworzenia klaster zobacz [Tworzenie HDInsight klastrów](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij, aby otworzyć szablon w Azure Portal następujące obraz.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Szablon znajduje się w kontenerze publicznej obiektów blob, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Z karta Parametry wprowadź następujące informacje:

    - **NazwaKlastra**: Wprowadź nazwę klaster Hadoop, który ma zostać utworzony.
    - **Klaster nazwę logowania i hasło**: domyślna nazwa logowania to administrator.
    - **SSH nazwy użytkownika i hasła**.
    
    Zapisz te wartości.  Należy je w dalszej części samouczka.

    > [AZURE.NOTE] SSH umożliwia zdalny dostęp do klaster HDInsight za pomocą wiersza polecenia. Nazwa użytkownika i hasło używane w tym miejscu jest używana podczas łączenia się z klastrem za pośrednictwem SSH. Ponadto SSH nazwa użytkownika musi być unikatowa, podczas tworzenia konta użytkownika na wszystkie węzły HDInsight. Poniżej przedstawiono kilka nazw kont zarezerwowana do użytku w usługach w klastrze, a nie można użyć jako nazwa użytkownika SSH:
    >
    > główny, hdiuser, burza, hbase, ubuntu, zookeeper, hdfs, przędzy, mapred, hbase, gałęzi, oozie, falcon, sqoop, administrator, tez, hcat, hdinsight zookeeper.

    > Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz jeden z następujących artykułów:

    > * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3.w kliknij **przycisk OK** , aby zapisać parametry.

4. z karta **wdrożenia niestandardowe** kliknij pole listy rozwijanej **Grupa zasobów** , a następnie kliknij **Nowy** , aby utworzyć nową grupę zasobów. Grupa zasobów jest kontenerem grupującego klastrem, konto zależne miejsca do magazynowania i innych zasobów połączonych.

5 kliknij **warunki prawne**, a następnie kliknij przycisk **Utwórz**.

6 kliknij przycisk **Utwórz**. Zostanie wyświetlony fragment zatytułowany wdrożenia Submitting wdrożenia szablonu. Wystarczy o około 20 minut utworzyć klaster i baza danych SQL.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Uruchamianie kwerend Spark SQL używaniem notesu Jupyter

W tej sekcji umożliwiają notesu Jupyter Uruchom na klaster Spark Spark SQL kwerendy. Klastrów HDInsight Spark zawierają dwa jądra, których można używać z notesu Jupyter. Są to:

* **PySpark** (w przypadku aplikacji napisana Python)
* **Spark** (w przypadku aplikacji napisana Scala)

W tym artykule należy użyć jądrze PySpark. W artykule [jądra dostępne na Jupyter notesów za pomocą Spark HDInsight klastrów](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) można znaleźć w szczegółowe informacje na temat korzyści wynikające z używania jądrze PySpark. Jednak kilka kluczowych korzyści wynikające z używania jądrze PySpark są:

* Nie należy ustawić konteksty iskrowym i gałęzi. Te są ustawiane automatycznie dla Ciebie.
* Można użyć magics komórki, takich jak `%%sql`, bezpośrednio uruchamianie kwerend SQL lub gałęzi bez dowolnego poprzedzającego wstawki kodu.
* Dane wyjściowe zapytania SQL lub gałąź jest automatycznie zwizualizować.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Tworzenie notesu Jupyter z jądra PySpark 

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu. Kliknij pozycję **Nowy**, a następnie kliknij pozycję **PySpark**.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Tworzenie nowego notesu Jupyter")

3. Nowy notes zostanie utworzony i otwarty o nazwie Untitled.pynb. Kliknij nazwę notesu, u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Podaj nazwę notesu")

4. Ponieważ utworzono Notes za pomocą jądrze PySpark, nie musisz utworzyć dowolny konteksty jawnie. Konteksty iskrowym i gałąź jest automatycznie tworzona automatycznie po uruchomieniu pierwszą komórkę kodu. Możesz rozpocząć, importując typy wymaganego w tym scenariuszu. Aby to zrobić, wklej następujący fragment kodu w komórce i naciśnij klawisze **SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Każdorazowo po uruchomieniu zadania w Jupyter tytułu okna przeglądarki sieci web pokazuje stan **(zajęty)** wraz z tytułu notesu. Pojawi się także pełne kółko obok tekstu **PySpark** w prawym górnym rogu. Po zakończeniu zadania spowoduje to zmianę okrąg.

     ![Stan zadania notesu Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Stan zadania notesu Jupyter")

4. Ładowanie przykładowych danych do tabeli tymczasowej. Po utworzeniu klastrze Spark w HDInsight przykładowy plik danych, **hvac.csv**są kopiowane do konta skojarzonego miejsca do magazynowania w ramach **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    W pustej komórce Wklej w poniższym przykładzie i naciśnij klawisze **SHIFT + ENTER**. W tym przykładzie kodu rejestruje dane w tabeli tymczasowej o nazwie **instalacji grzewczo-wentylacyjnych**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Ponieważ używasz jądra PySpark, można teraz bezpośrednio uruchomić zapytania SQL w tabeli tymczasowej **instalacji grzewczo-wentylacyjnych** , która została właśnie utworzona za pomocą `%%sql` magiczną. Aby uzyskać więcej informacji na temat `%%sql` magiczną, a także inne dostępne z jądrze PySpark magics zobacz [jądra dostępne na Jupyter notesów przy użyciu klastrów Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Po pomyślnym zakończeniu zadania jest domyślnie wyświetlany następujący wynik tabelaryczne.

    ![Tabela danych wyjściowych wynik kwerendy] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Tabela danych wyjściowych wynik kwerendy")

    Można też wyświetlić wyniki w także inne wizualizacje. Na przykład wykres warstwowy, aby uzyskać ten sam wynik będzie wyglądał jak na następującym przykładzie.

    ![Wykres warstwowy wynik kwerendy] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Wykres warstwowy wynik kwerendy")


6. Po zakończeniu uruchamiania aplikacji należy zamknięcia Notes, aby zwolnić zasoby. Aby to zrobić, w menu **plik** Notes, kliknij polecenie **Zamknij i zatrzymanie**. Ten zostanie zamknięty, a następnie zamknij notes.

##<a name="delete-the-cluster"></a>Usuwanie klaster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych Instalacja grzewczo-Wentylacyjna danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dziennika witryny sieci Web przy użyciu Spark w HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Aplikacja wglądu telemetrycznego analiza danych przy użyciu Spark w HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
