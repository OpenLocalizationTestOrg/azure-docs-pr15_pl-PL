<properties
    pageTitle="Co to jest HBase w HDInsight? | Microsoft Azure"
    description="Wprowadzenie do HBase Apache w HDInsight tworzenie bazy danych NoSQL na Hadoop. Więcej informacji na temat przypadków użycia i porównać HBase z innych klastrów Hadoop."
    keywords="bigtable nosql, co to jest hbase"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Co to jest HBase w HDInsight: NoSQL bazy danych, która zapewnia możliwości przypominających BigTable Hadoop

Apache HBase jest bazy NoSQL Otwórz źródło danych oparty na Hadoop i modelowania po Google BigTable. HBase zawiera dostępie i spójność znaczący dla dużych ilości danych niestrukturalne i semistrukturalne schemaless zorganizowane według kolumny rodziny bazy danych.

Dane są przechowywane w wierszach tabeli i rodzina kolumny zgrupowaniem danych w wierszu. HBase jest schemaless bazy danych, w tym znaczeniu, że typ danych przechowywanych w nich ani kolumn trzeba zdefiniować przed ich użyciem. Kod źródłowy Otwórz liniowo skale obsługę petabytes danych na tysiącach węzłów. Może korzystać nadmiarowości danych, przetwarzanie wsadowe i inne funkcje, które są udostępniane przez aplikacje rozproszone w ekosystemie Hadoop.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Jak HBase jest zaimplementowana w Azure HDInsight?

Usługa HDInsight HBase jest oferowane jako zarządzaną klaster, który jest zintegrowany z Azure środowiska. Klastrów są skonfigurowane do przechowywania danych bezpośrednio w magazynie obiektów Blob platformy Azure, która udostępnia krótki czas oczekiwania i zwiększona elastyczność w opcje dotyczące wydajności i koszt. Dzięki temu klienci tworzenia interakcyjnych witryn sieci Web Praca z dużych zestawów danych do tworzenia usług, w których są przechowywane dane czujnik i telemetrycznego z milionów punkty końcowe i analizy danych z zadaniami Hadoop. HBase i Hadoop to dobre punktów dla projektu duży danych wyjściowych w Azure; w szczególności może udostępnić w czasie rzeczywistym aplikacji do współdziałania z dużych zestawów danych.

Implementacja HDInsight wykorzystuje skali architektura HBase o podanie automatyczne sharding tabel, silne spójności do odczytu i zapisu oraz automatyczne przejście. Zwiększa wydajność przez buforowanie Odczyt i wysokiej przepustowości dla zapisu w pamięci. Wirtualna sieć inicjowania obsługi administracyjnej jest także dostępny dla HDInsight HBase. Aby uzyskać szczegółowe informacje, zobacz [klastrów świadczenia usługi HDInsight w wirtualnej sieci Azure] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Jak odbywa się danych w HDInsight HBase?

Dane można zarządzać w HBase przy użyciu `create`, `get`, `put`, i `scan` polecenia z powłoki HBase. Dane są zapisywane w bazie danych przy użyciu `put` i za pomocą `get`. `scan` Polecenie jest używane w celu uzyskania danych z wielu wierszy w tabeli. Dane można również zarządzać przy użyciu HBase C# interfejsu API, który zawiera bibliotekę klienta na bieżąco interfejsu API usługi REST HBase. Bazy danych programu HBase może również przeszukiwać przy użyciu gałęzi. Wprowadzenie do tych modelach programowania, zobacz [Wprowadzenie do korzystania z HBase z Hadoop w HDInsight][hbase-get-started]. Współtworzenie procesory są również dostępne, umożliwiające przetwarzania danych w węzłach obsługujących bazy danych.


## <a name="scenarios-use-cases-for-hbase"></a>Scenariusze: Używanie przypadków dla HBase
Przypadek użycia kanonicznych, dla których BigTable (i przez rozszerzenie HBase) został utworzony został wyszukiwanie w sieci web. Aparaty wyszukiwania tworzyć indeksy, które mapowanie terminów do stron sieci web, które zawierają je. Istnieją większości przypadków użycia, w których HBase nadaje się do — niektóre z nich są wyszczególnione w tej sekcji.

- Wartość klucza magazynu

    HBase może być używany jako magazynu wartość klucza, a jego nadaje się do zarządzania systemami wiadomości. Facebook używa HBase dla ich systemu wiadomości oraz idealnie nadają się do przechowywania i zarządzanie komunikacja w Internecie. WebTable używa HBase wyszukiwanie i zarządzać nimi, tabel, które są wyodrębniane ze stron sieci Web.

- Dane czujnika

    HBase służy do rejestrowania danych zebranych w porządku rosnącym z różnych źródeł. Ta opcja uwzględnia analizy społecznościowe, szeregu czasowego, aktualizowanie interakcyjne pulpity nawigacyjne z trendów i liczników i zarządzanie systemami dziennika inspekcji. Jako przykład można wymienić podmiot Bloomberg końcowych i otwartej czasu serii bazy danych (OpenTSDB) są przechowywane i zapewnia dostęp do metryki zbierane o kondycji systemu serwera.

- Zapytania w czasie rzeczywistym

    [Phoenix](http://phoenix.apache.org/) jest aparatu zapytania SQL w celu Apache HBase. Jest dostępny jako sterownik JDBC oraz umożliwia kwerend i zarządzanie HBase tabel przy użyciu języka SQL.

- HBase jako platformy

    Aplikacje można uruchamiać na bieżąco HBase przy użyciu go jako magazynu danych. Jako przykład można wymienić Olsztyn, OpenTSDB, Kiji i Titan. Aplikacje także można zintegrować z HBase. Jako przykład można wymienić gałęzi, świnka Solr, burza, Flume, Impala, Spark, trójdzielnego i zwijania i rozwijania szczegółów.


##<a name="next-steps"></a>Następne kroki

- [Wprowadzenie do korzystania z Hadoop w HDInsight HBase][hbase-get-started]
- [Inicjowanie obsługi klastrów HDInsight w wirtualnej sieci Azure] [hbase-provision-vnet]
- [Konfigurowanie replikacji HBase w HDInsight](hdinsight-hbase-geo-replication.md)
- [Analizowanie upodobania Twitter z HBase w HDInsight][hbase-twitter-sentiment]
- [Używanie środowiska Maven tworzenie aplikacji Java HBase za pomocą usługi HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>Zobacz też

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: System rozłożone miejsca do magazynowania danych strukturalnych](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
