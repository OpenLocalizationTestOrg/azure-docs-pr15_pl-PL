<properties 
    pageTitle="Omówienie Spark Apache w HDInsight | Microsoft Azure" 
    description="Wprowadzenie do Spark Apache w HDInsight i scenariusze korzystania Spark na HDInsight w aplikacjach." 
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
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Omówienie: Apache Spark na HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> jest równolegle Otwórz źródło przetwarzania framework obsługującego przetwarzania w pamięci, aby zwiększyć wydajność aplikacji analityczny duży danych. Aparatu przetwarzania Spark jest przeznaczony dla szybkość łatwość użytkowania i zaawansowanych analiz. Możliwości obliczeń w pamięci w iskrowym ułatwiają dobrym rozwiązaniem iteracyjne algorytmów w obliczeniach maszynowego uczenia się i wykres. Spark również jest zgodny z magazynem obiektów Blob platformy Azure (WASB), aby istniejących danych przechowywanych w Azure łatwo mogą być przetwarzane za pośrednictwem Spark.

Po utworzeniu klastrze Spark w HDInsight możesz utworzyć zasoby Azure obliczeń z Spark zainstalowaniu i skonfigurowaniu. Aby utworzyć klaster Spark w HDInsight są tylko trwa około dziesięciu minut. Dane mają być przetwarzane są przechowywane w magazynie obiektów Blob platformy Azure. Zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight][hdinsight-storage].

![Spark Apache na usługa Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Spark Apache na usługa Azure HDInsight")


**Chcesz rozpocząć pracę z Spark Apache na Azure HDInsight?** Zobacz [Szybki Start: tworzenie klastrze Spark na HDInsight Linux i uruchamianie przykładowych aplikacji przy użyciu Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Aby uzyskać listę znanych problemów i ograniczenia z bieżącej wersji zobacz [znane problemy Spark Apache w Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Dlaczego warto używać Spark na Azure HDInsight? 

Usługa Azure HDInsight oferuje w pełni zarządzaną usługę Spark. Korzyści wynikające z używania Spark na HDInsight są następujące:

| Funkcja                             | Opis       |
|-------------------------------------|-------------------|
| Łatwość tworzenia klastrów            | Możesz utworzyć nowy klaster Spark na HDInsight w minutach za pomocą portalu zarządzania Azure, Azure programu PowerShell lub HDInsight .NET SDK. Zobacz [Rozpoczynanie pracy z klastrem Spark w HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Łatwość użytkowania                     | Spark w klastrów HDInsight zawiera notesów Jupyter wstępnie skonfigurowane. Możesz użyć tych interakcyjnych przetwarzania danych i wizualizacji. Adresy URL jest https://CLUSTERNAME.azurehdinsight.net/jupyter. Zamień __NAZWAKLASTRA__ nazwę klaster Spark HDInsight.|
| Interfejsy API REST                       | Spark w HDInsight zawiera [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server),-interfejsu API usługi REST podstawie serwer zadań Spark zdalne przesyłanie i Monitoruj uruchomionego zadania. |
| Obsługa magazynu Lake danych Azure | Spark na HDInsight może być skonfigurowany do używania magazynu Lake danych Azure jako dodatkowego miejsca do magazynowania. Aby uzyskać więcej informacji na magazynu Lake danych zobacz [Omówienie z Azure Lake magazynu danych](../data-lake-store/data-lake-store-overview.md).
| Integracja z usługami Azure | Spark na HDInsight zawiera łącznika do koncentratorów zdarzenia Azure. Klienci mogą tworzyć strumieniowo aplikacje przy użyciu biegami zdarzenia, oprócz [Kafka](http://kafka.apache.org/), który jest już dostępny jako część Spark. |
| Obsługa serwera R  | Można ustawić serwer R w klastrze HDInsight Spark uruchomić rozłożone obliczeń R z prędkości zapewnienie z klastrem Spark. Aby uzyskać więcej informacji zobacz [Wprowadzenie do korzystania z serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integracja z IntelliJ ogólny obraz  | Dodatek HDInsight dla IntelliJ służy do tworzenia i przesyłania aplikacji z klastrem HDInsight Spark. Aby uzyskać więcej informacji, zobacz [Dodatek Narzędzia HDInsight użycia dla IntelliJ POMYSŁEM utworzenie Spark wniosków o klaster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Równoczesne kwerendy              | Spark w HDInsight obsługuje równoczesne kwerendy. Dzięki temu wielu kwerend z jednego użytkownika lub wielu kwerend z różnych użytkowników i aplikacjom współużytkowanie tych samych zasobów klaster. |
| Buforowanie na twarde SSD                 | Istnieje możliwość danych z pamięci podręcznej w pamięci lub w twarde SSD dołączone do przez klaster. Pamięci podręcznej w pamięci zapewnia najlepszą wydajność kwerendy, ale może to być drogich; buforowanie w twarde SSD umożliwia świetne rozwiązanie dotyczące zwiększania wydajności kwerend bez konieczności utworzyć klaster o rozmiarze, który jest wymagany do dopasowania całego zestawu danych w pamięci.|
| Integracja z narzędzi analizy Biznesowej       | Spark dla HDInsight zawiera łączniki narzędzi analizy Biznesowej, takich jak [Usługi Power BI](http://www.powerbi.com/) i [Tableau](http://www.tableau.com/products/desktop) do analizy danych.|
| Wstępnie załadowanego bibliotek Anaconda        | Spark klastrów na HDInsight pochodzić z bibliotekami Anaconda preinstalowane. [Anaconda](http://docs.continuum.io/anaconda/) przewiduje zbliżony 200 bibliotek maszynowego uczenia, analizy danych, wizualizacji itp.|
| Skalowalność                     | Mimo że można określić liczby węzłów w klastrze, podczas tworzenia, warto powiększanie lub zmniejszanie klaster zgodnie z pracą. Wszystkich klastrów HDInsight umożliwiają zmienianie liczby węzłów w klastrze. Ponadto klastrów Spark można usunąć bez utraty danych, ponieważ wszystkie dane są przechowywane w magazynie obiektów Blob platformy Azure. |
| Pomoc techniczna 24-7                    | Spark na HDInsight zawiera pomoc techniczna 24-7 na poziomie przedsiębiorstwa i SLA wydłużyć czas 99,9%.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Co to są przypadków użycia dla Spark na HDInsight?

Spark Apache w HDInsight udostępnia następujące kluczowe scenariusze.

### <a name="interactive-data-analysis-and-bi"></a>Analiza danych interakcyjnych i analizy Biznesowej

[Przyjrzyj się samouczek](hdinsight-apache-spark-use-bi-tools.md)

Spark Apache w HDInsight przechowuje dane w obiektów blob platformy Azure. Ekspertów biznesowych i producenci najważniejszych decyzji można analizować i tworzenie raportów w danych i tworzenie interakcyjnych raportów z analizy danych za pomocą usługi Microsoft Power BI. Analitycy można rozpocząć niestrukturalne półprzezroczysty strukturę danych w magazynie Azure, definiowanie schemat danych przy użyciu notesy, a następnie skonstruuj modeli danych przy użyciu usługi Microsoft Power BI. Spark w HDInsight obsługuje także liczbę narzędzia innych producentów analizy Biznesowej, takich jak Tableau, Qlikview i Lumira SAP, dzięki czemu idealny platformy dla analityków danych, ekspertów biznesowych i producenci najważniejszych decyzji.

### <a name="iterative-machine-learning"></a>Nauka iteracyjne komputera

[Przyjrzyj się samouczek: przewidywanie tworzenia temperatury uisng instalacji grzewczo-Wentylacyjnych danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Przyjrzyj się samouczek: przewidywanie żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark zawiera [MLlib](http://spark.apache.org/mllib/), maszynowego uczenia biblioteki wbudowane na wierzchu Spark. Oprócz tego Spark na HDInsight zawiera Anaconda, rozkładu Python z różnymi pakiety szkoleniowe na komputerze. To połączenie przy użyciu wbudowanych funkcji Jupyter notesów, a masz środowisku wiersza do tworzenia aplikacji nauki komputera.  

### <a name="streaming-and-real-time-data-analysis"></a>Analiza danych w czasie rzeczywistym i przesyłanie strumieniowe

[Przyjrzyj się samouczek](hdinsight-apache-spark-eventhub-streaming.md)

Analiza danych w czasie rzeczywistym służy do scenariuszy od skracanie czasu do wglądu danych przez przetwarzanie danych, jak wyładowuje, do tworzenia rzeczywistego streaming rozwiązanie. Spark w HDInsight oferuje sformatowanego obsługę tworzenie rozwiązań analizy w czasie rzeczywistym. Podczas Spark już łączników, aby mogły zjeść tej ostatniej danych z wielu źródeł, takich jak Kafka, Flume, Twitter, ZeroMQ lub port TCP sockets, Spark w HDInsight dodaje najlepszych obsługę ingesting danych z koncentratorów zdarzenia Azure. Koncentratory wydarzenia są najczęściej używanych Kolejkowanie usługi Azure. Czy powoduje, że o pomocy technicznej w nowym polu dla zdarzenia koncentratorów wzmóc w HDInsight platformy idealne rozwiązanie do tworzenia procesu analizy w czasie rzeczywistym.

##<a name="next-steps"></a>Jakie składniki są zawarte klastrze Spark?

Spark w HDInsight zawiera następujące składniki, które są dostępne na klastrów domyślnie.

- [Podstawowe Spark](https://spark.apache.org/docs/1.5.1/). Obejmuje Spark Core, Spark SQL, Spark streaming interfejsy API, GraphX i MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter notesu](https://jupyter.org)

Spark w HDInsight udostępnia [sterownik ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) dla łączności do klastrów Spark w HDInsight z narzędzi analizy Biznesowej, takich jak Microsoft Power BI i Tableau.

## <a name="where-do-i-start"></a>Gdzie uruchomić?

Rozpocznij tworzenie klastrze Spark na HDInsight Linux. Zobacz [Szybki Start: tworzenie klastrze Spark na HDInsight Linux i uruchamianie przykładowych aplikacji przy użyciu Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Następne kroki

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

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
