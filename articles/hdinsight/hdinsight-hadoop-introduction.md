 <properties
    pageTitle="Wprowadzenie do Hadoop - co to jest Hadoop na HDInsight? | Microsoft Azure"
    description="Wprowadzenie do Hadoop, rozłożone duży przetwarzania danych i analiza oraz składniki ekosystemu Hadoop w chmurze na HDInsight."
    keywords="Analiza danych duży, wprowadzenie do hadoop, co to jest hadoop, stos technologii hadoop, ekosystemu hadoop"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Wprowadzenie do ekosystemie Hadoop na Azure HDInsight

 Ten artykuł zawiera wprowadzenie do Hadoop Azure HDInsight, jego ekosystemu i duży danych. Informacje na temat składników Hadoop, wspólna terminologia i scenariusze duży analizy.

## <a name="what-is-hadoop-on-hdinsight"></a>Co to jest Hadoop na HDInsight?

Hadoop odwołuje się do ekosystem oprogramowanie open source ramy rozłożone przetwarzania, przechowywania i analizy dużych zestawów danych dotyczących klastrów komputerów. Usługa Azure HDInsight udostępnia składniki Hadoop z rozkładem **Hortonworks danych platformy (HDP)** w chmurze, wdraża i przepisy zarządzanych klastrów z wysoka niezawodność i dostępność.  

Apache Hadoop był oryginalny projekt Otwórz źródło duży przetwarzania danych. Następujące był rozwoju oprogramowania związane z i narzędzi traktowane jako część Hadoop stos technologii, łącznie z gałęzi Apache, Apache HBase Apache Spark i wiele innych. Aby uzyskać szczegółowe informacje, zobacz [Omówienie ekosystemie Hadoop w HDInsight](#overview) .

## <a name="what-is-big-data"></a>Co to jest duży danych?

Duży danych w tym artykule opisano wszystkie duże treści informacji cyfrowych z tekstu w serwisie Twitter kanał czujnika informacji z urządzenia przemysłowe do informacji na temat przeglądania klienta i zakupów w witrynie sieci Web. Duży, może być historycznych (to znaczy przechowywanych danych) lub w czasie rzeczywistym (to znaczy strumieniowo bezpośrednio ze źródła). Duży nie zbiera się danych w kiedykolwiek zamocowaniem wielkości, o rosnącej złożoności wyższą prędkości i rozwijanie różnych formatów.

Duży danych o podanie sankcji analizy lub wglądu możesz zbieranie danych i prawy pytania. Ponadto należy się upewnić, że dane są dostępne, czyszczone, analizowane, a następnie przedstawione w wygodny sposób. To, gdzie mogą pomóc duży analizy na Hadoop w HDInsight.

## <a name="overview"></a>Omówienie ekosystemie Hadoop w HDInsight

Usługa HDInsight jest rozkład chmury firmy Microsoft Azure szybko rozwijanym stos technologii Apache Hadoop do analizy danych duży. Zawiera implementacji Apache Spark, HBase, burza, świnka, gałęzi, Sqoop, Oozie, Ambari i tak dalej. Usługa HDInsight również można zintegrować z narzędzi analiz biznesowych, takich jak usługi Power BI, Excel, SQL Server Analysis Services i SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase Spark, Burza i dostosowany klastrów

Usługa HDInsight zawiera konfiguracje klastrów Apache Hadoop, Spark, HBase lub Burza. Można też [dostosować klastrów z akcjami skrypt](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: umożliwia przechowywanie danych z [HDFS](#hdfs)i prosty model programowania [MapReduce](#mapreduce) w celu przetwarzania i analizowania danych równolegle.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: ramy przetwarzanie równoległe, które obsługuje przetwarzanie w pamięci, aby zwiększyć wydajność aplikacji analizy danych duży wzmóc works dla SQL strumieniowego przesyłania danych i urządzenia szkoleniowe. Zobacz [Omówienie: co to jest Apache Spark w HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: NoSQL bazy danych oparty na Hadoop udostępniający dostępie i spójność silnych dużych ilości niestrukturalne danych strukturalnych i półstrukturalnych - potencjalnie miliardów wierszy czasu miliony kolumn. Zobacz [Omówienie HBase na HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Burza Apache</a>**: system obliczeń rozłożone, w czasie rzeczywistym szybkiego przetwarzania dużych strumienie danych. Burza jest oferowane jako zarządzaną klaster w HDInsight. Zobacz [Analiza danych w czasie rzeczywistym czujnika przy użyciu Burza i Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Przykład dostosowywania skryptów

Akcje skryptu są skrypty uruchamiania podczas inicjowania obsługi administracyjnej klaster i można zainstalować dodatkowe składniki w klastrze. W przypadku klastrów z systemem Linux są imprezie skryptów.

Poniższe przykładowe skrypty są dostarczane przez zespół HDInsight:

* [Odcień](hdinsight-hadoop-hue-linux.md): A zestaw aplikacji sieci web umożliwiające interakcję z klastrem. Tylko klastrów Linux.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): wykres przetwarzania modelu relacje między rzeczy lub osób.

* [R](hdinsight-hadoop-r-scripts-linux.md): Otwórz źródło języka i środowisko statystyczne przetwarzania danych używane w nauki komputera.

* [Solr](hdinsight-hadoop-solr-install-linux.md): platformą wyszukiwania przedsiębiorstwa skali umożliwiający wyszukiwanie pełnotekstowe na danych.

Aby uzyskać informacje dotyczące tworzenia własnych skryptów akcji zobacz [opracowywania skrypt akcji z usługi HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Co to są składniki Hadoop i narzędzia?

Na HDInsight klastrów znajdują się następujące składniki i narzędzi.

* **[Ambari](#ambari)**: klaster inicjowania obsługi administracyjnej, zarządzania, monitorowania i narzędzi.

* **[Avro](#avro)** (Microsoft .NET biblioteki Avro): szeregowania danych w środowisku programu Microsoft .NET.

* **[Gałąź & HCatalog](#hive)**: języka SQL (Structured Query) — takie jak kwerend i warstwy zarządzania tabeli i miejsca do magazynowania.

* **[Mahout](#mahout)**: dla komputera skalowalna nauki aplikacji.

* **[MapReduce](#mapreduce)**: starsza wersja strukturę Hadoop distributed zarządzania zasobami i przetwarzania. Zobacz [PRZĘDZY](#yarn)framework generacji zasobów.

* **[Oozie](#oozie)**: Zarządzanie przepływami pracy.

* **[Phoenix](#phoenix)**: warstwy relacyjnej bazy danych na HBase.

* **[Świnka](#pig)**: prostsze skrypty przekształcenia MapReduce.

* **[Sqoop](#sqoop)**: danych importowanie i eksportowanie.

* **[Tez](#tez)**: umożliwia uruchamianie wydajność w skali procesów intensywną danych.

* **[Przędza](#yarn)**: część Hadoop Podstawowa biblioteka i generacji framework oprogramowaniem MapReduce.

* **[ZooKeeper](#zookeeper)**: można je zsynchronizować procesów w systemów dystrybucji.

> [AZURE.NOTE] Aby uzyskać informacje dotyczące określonych składników i informacje o wersji zobacz [składniki Hadoop, przechowywanie wersji i oferty usług w HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Jest Apache Ambari inicjowania obsługi administracyjnej i monitorowania klastrów Apache Hadoop i zarządzania. Zawiera intuicyjny zestaw narzędzi operator i zestaw interfejsów API ukrycie złożoności Hadoop upraszczanie operacji klastrów. HDInsight z systemem Linux oferowanych klastrów, oba Ambari sieci web interfejsu użytkownika i interfejsu API usługi REST Ambari podczas klastrów opartych na systemie Windows stanowią podzbiór interfejsu API usługi REST. Widoki Ambari na HDInsight klastrów Zezwalaj wtyczki możliwości interfejsu użytkownika.

Zobacz [Zarządzanie HDInsight klastrów przy użyciu Ambari](hdinsight-hadoop-manage-ambari.md) (tylko Linux), [Monitor Hadoop klastrów w HDInsight za pomocą interfejsu API Ambari](hdinsight-monitor-use-ambari-api.md)i <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a>.

### <a name="avro"></a>Avro (biblioteki programu Microsoft .NET Avro)

Biblioteka programu Microsoft .NET dla Avro zawiera format wymiany niewielkie dane binarne Apache Avro szeregowania dla środowiska programu Microsoft .NET. Aby zdefiniować schematu niezależne od języka, w którym ubezpiecza współdziałanie języków używa <a target="_blank" href="http://www.json.org/">Notacji obiektu JavaScript (JSON)</a> , znaczenia danych seryjnych w jednym języku można odczytywać w innym. Szczegółowe informacje na temat formatu można znaleźć w < docelowy = "pusty" odwołanie _ = "http://avro.apache.org/docs/current/spec.html" > Specyfikacja Avro Apache</a>.
Format plików Avro obsługuje rozłożone MapReduce model programowania. Pliki są "masowej", co oznacza można szukanie dowolnego miejsca w pliku i rozpocząć odczytu z określonego bloku. Aby dowiedzieć się, jak, zobacz [Dane seryjne z biblioteki programu Microsoft .NET Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Pliku usługi Hadoop rozproszony System (HDFS) jest rozproszony system plików który z MapReduce i przędza jest podstawową ekosystemu Hadoop. HDFS jest system plików standardowego dla klastrów Hadoop na HDInsight.

### <a name="hive"></a>Gałąź & HCatalog

<a target="_blank" href="http://hive.apache.org/">Gałąź Apache</a> to oprogramowanie magazyn danych oparty na Hadoop umożliwiający kwerend oraz zarządzanie dużych zestawów danych w magazynie rozłożone przy użyciu języka SQL przypominających o nazwie HiveQL. Gałąź, takich jak świnka, jest analizę u góry MapReduce. Po uruchomieniu kwerendy gałęzi przekłada się na serii zadań MapReduce. Gałąź jest koncepcji bliżej system zarządzania relacyjnymi bazami danych niż świnka i w związku z tym jest odpowiedni dla bardziej uporządkowanego danych. Dane bez struktury świnka jest lepszym wyborem. Zobacz [Używanie gałęzi z Hadoop w HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> jest warstwę zarządzania tabeli i miejsca do magazynowania dla Hadoop przedstawiającego użytkowników z relacyjnych widok danych. W HCatalog można czytać i zapisywać pliki w dowolnym formacie, dla których mogą być zapisywane SerDe gałęzi (serializatora Deserializator).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> jest skalowalna biblioteka maszynowego uczenia algorytmów, które działają w Hadoop. Korzystając z zasad statystyki, maszynowego uczenia aplikacji nauki systemów, aby dowiedzieć się z danych i używanie wcześniejszych wyników, aby określić zachowanie przyszłych. Zobacz [Generuj recommendations filmu przy użyciu Mahout na Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce jest ramach starszych wersji oprogramowania Hadoop dotyczące pisania aplikacji do partii proces dużych zestawów danych jednocześnie. Zadanie MapReduce dzieli dużych zestawów danych i organizowania danych w pary klucz wartość do przetwarzania.

[Przędza](#yarn) jest framework menedżera i aplikacji zasobów generacji Hadoop i nosi nazwę MapReduce 2.0. MapReduce zadania wykonywane na PRZĘDZY.

Aby uzyskać więcej informacji o MapReduce zobacz <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> na stronie typu Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> jest system można je zsynchronizować przepływu pracy, która zarządza Hadoop zadania. Go jest zintegrowany z stos Hadoop i obsługuje zadania Hadoop o MapReduce, świnka, gałęzi i Sqoop. Można również umożliwia za planowanie zadania specyficzne dla systemu, takich jak programy Java lub skryptów powłoki. Zobacz [Oozie korzystanie z Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> przekracza HBase warstwy relacyjnej bazy danych. Phoenix zawiera sterownik JDBC, który umożliwia użytkownikom wykonywania kwerend i zarządzania tabele SQL bezpośrednio. Phoenix tłumaczy kwerend oraz innych instrukcji do natywnej NoSQL interfejsu API — zamiast za pomocą MapReduce — dzięki czemu szybciej aplikacje u góry NoSQL sklepy. Zobacz [Używanie Apache Phoenix i SQuirreL z HBase klastrów](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Świnka
<a  target="_blank" href="http://pig.apache.org/">Świnka Apache</a> jest wysokiego poziomu platformę, która umożliwia wykonywanie złożonych przekształcenia MapReduce na bardzo dużych zestawów danych przy użyciu prostego języka skryptowego o nazwie świnka łaciński. Świnka tłumaczy skryptów łaciński świnka, będą działały w ramach Hadoop. Możesz utworzyć funkcje zdefiniowane przez użytkownika (UDF), aby rozszerzyć łaciński świnka. Zobacz [Używanie świnka z Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> to narzędzie, że przeniesienia zbiorczo danych między Hadoop i relacyjnych baz danych programu SQL lub innych sklepów danych strukturalnych tak efektywnie. Zobacz [Sqoop korzystanie z Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> jest AIF oparty na PRZĘDZY Hadoop, wykonującego wykresy złożone, acykliczne ogólne przetwarzania danych. Jest bardziej elastyczne i zaawansowane następnik RAM MapReduce umożliwiający procesów intensywną danych, takich jak gałęzi skuteczniejsze wykonywanie w skali. Zobacz ["Użyj Tez Apache zwiększa wydajność" Użyj gałęzi i HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>PRZĘDZA
Przędza Apache jest generacji MapReduce (MapReduce 2.0 lub MRv2) i obsługuje scenariusze przetwarzania danych poza przetwarzanie z większą skalowalność i w czasie rzeczywistym przetwarzanie wsadowe MapReduce. Przędza udostępnia zarządzania zasobami i rozłożone AIF. MapReduce zadania wykonywane na PRZĘDZY.

Aby uzyskać informacje o PRZĘDZY, zobacz <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (przędza)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> współrzędne procesy w dużych systemów dystrybucji za pomocą udostępnionego hierarchiczne obszary nazw rejestruje danych (znodes). Znodes zawierają niewielkich ilości informacji meta potrzebnych koordynowanie procesów: stan, lokalizację, konfiguracji i tak dalej.

## <a name="programming-languages-on-hdinsight"></a>Języki programowania na HDInsight

Usługa HDInsight klastrów — Hadoop, HBase, Burza i Spark klastrów — obsługuje wiele języków programowania, ale niektóre nie są instalowane domyślnie. W przypadku bibliotek modułów lub nie zainstalowano domyślnie pakietów za pomocą akcji skryptu Aby zainstalować składnik. Zobacz [opracowywanie akcji skryptów z usługi HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Domyślna obsługa języka programowania

Domyślnie HDInsight klastrów pomocy technicznej:

* Java

* Python

Dodatkowe języki, można zainstalować za pomocą skryptu akcje: [opracowywanie akcji skryptów z usługi HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Języki maszyn wirtualnych (maszyny wirtualnej Java) języka Java

Wiele języków innych niż Java mogą być uruchamiane przy użyciu środowiska Java (maszyny wirtualnej Java); jednak uruchomienie niektóre z tych języków, może wymagać zainstalowanych w klastrze składników dodatkowych.

Tych językach opartych na alfabecie maszyny wirtualnej Java są obsługiwane na klastrów HDInsight:

* Clojure

* Jython (Python dla języka Java)

* Scala

### <a name="hadoop-specific-languages"></a>Języki specyficzne dla Hadoop

Usługa HDInsight klastrów obsługuje następujących języków, które są specyficzne dla ekosystemu Hadoop:

* Łaciński świnka świnka zadań

* HiveQL zadań gałęzi i SparkSQL


## <a name="advantage"></a>Zalety Hadoop w chmurze

W ramach ekosystemu chmura Azure Hadoop w HDInsight oferują szereg korzyści między nimi:

* Automatyczne inicjowania obsługi administracyjnej klastrów Hadoop. Usługa HDInsight klastrów są znacznie ułatwia tworzenie niż ręczne konfigurowanie klastrów Hadoop. Aby uzyskać szczegółowe informacje zobacz [Hadoop świadczenia klastrów w HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Składniki Hadoop stan clipart. Aby uzyskać szczegółowe informacje, zobacz [Hadoop składniki, przechowywanie wersji i oferty usług w HDInsight][component-versioning].

* Wysoka dostępność i niezawodność klastrów. Aby uzyskać szczegółowe informacje, zobacz [dostępności i niezawodności klastrów Hadoop w HDInsight](hdinsight-high-availability-linux.md) .

* Magazynowanie danych wydajność i ekonomicznych z magazynem obiektów Blob platformy Azure, opcja zgodnego z programem Hadoop. Aby uzyskać szczegółowe informacje, zobacz [Magazyn obiektów Blob platformy Azure korzystanie z Hadoop w HDInsight](hdinsight-hadoop-use-blob-storage.md) .

* Integracja z innymi usługami Azure, w tym [aplikacji sieci Web](https://azure.microsoft.com/documentation/services/app-service/web/) i [Baza danych SQL](https://azure.microsoft.com/documentation/services/sql-database/).

* Dodatkowe rozmiary maszyn wirtualnych i typy uruchamiania klastrów HDInsight. Zobacz [składniki Hadoop, przechowywanie wersji i oferty usług w HDInsight] [ component-versioning] Aby uzyskać szczegółowe informacje.

* Klaster skalowania. Klaster Skalowanie umożliwia zmianę liczby węzłów klastrze HDInsight uruchomiony bez konieczności ponownie utworzyć lub usunąć.

* Wirtualna sieć pomocy technicznej. Usługa HDInsight klastrów może służyć z Azure wirtualną sieć do obsługi izolacji chmurze zasobów lub scenariuszy hybrydowego, zawierających łącza chmury zasoby z znajdującymi się w centrum danych.

* Niska wprowadzania kosztów. Rozpoczynanie [bezpłatnej wersji próbnej](https://azure.microsoft.com/free/)lub zwróć się [szczegółowe informacje o cenach HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Aby uzyskać więcej informacji na temat korzyści na Hadoop w HDInsight, zobacz na [stronie funkcje Azure HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>Usługa HDInsight Standard i HDInsight Premium

Usługa HDInsight zawiera oferty chmury duży danych w dwóch kategoriach Standard i Premium. HDInsight Standard zawiera klaster skali przedsiębiorstwa organizacje mogą używać do uruchamiania ich obciążenie pracą duży danych. Usługa HDInsight Premium tworzy na tym, a także zaawansowane analizy i możliwości zabezpieczeń dla klastrów HDInsight. Aby uzyskać więcej informacji zobacz [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Zasoby, aby uzyskać więcej informacji na temat analizy danych duży, Hadoop i HDInsight

Tworzenie na to wprowadzenie do Hadoop w chmurze i analizy dużych danych z poniższych zasobów.


### <a name="hadoop-documentation-for-hdinsight"></a>Dokumentacji Hadoop HDInsight

* [Dokumentacja HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): strona dokumentacji dla Hadoop na Azure HDInsight zawierająca łącza do artykułów, klipów wideo i więcej zasobów.

* [Rozpoczynanie pracy z Hadoop w HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): samouczek szybki start dla inicjowania obsługi administracyjnej klastrów HDInsight Hadoop i uruchamianie kwerend gałęzi próbki.

* [Rozpoczynanie pracy z Spark w HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): samouczek szybki start dla klastrów Spark tworzenie i uruchamianie zapytania Spark SQL interakcyjnych.

* [Użyj serwera R na HDInsight](hdinsight-hadoop-r-server-get-started.md): Rozpoczynanie korzystania z serwera R w HDInsight Premium.

* [Obsługa administracyjna HDInsight klastrów](hdinsight-hadoop-provision-linux-clusters.md): Dowiedz się, jak inicjować obsługę klaster HDInsight Hadoop przez Azure portal, polecenie Azure lub Azure programu PowerShell.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Dowiedz się więcej o bibliotece oprogramowania Apache Hadoop to struktura umożliwiająca rozłożone przetwarzania dużych zestawów danych w wielu klastrów komputerów.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Dowiedz się więcej o architektury i projektowania Hadoop Distributed systemu plików, system podstawowy używany przez aplikacje Hadoop.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">Samouczek MapReduce</a>: Dowiedz się więcej o platformy programowej pomocne przy tworzeniu Hadoop aplikacjom szybkiego przetwarzania dużych ilości danych równolegle na dużych klastrów węzłów obliczeń.


### <a name="microsoft-business-intelligence"></a>Funkcje analizy biznesowej firmy Microsoft

Narzędzia analiz biznesowych znanych — takich jak program Excel PowerPivot, SQL Server Analysis Services i SQL Server Reporting Services — pobierania, analizowanie i raportowanie danych zintegrowany z usługi HDInsight za pomocą dodatek Power Query lub sterownik ODBC gałęzi firmy Microsoft.

Tych narzędzi analizy Biznesowej może pomóc w analizy dużych danych:

* [Nawiązywanie połączenia w programie Excel Hadoop dodatku Power Query](hdinsight-connect-excel-power-query.md): Dowiedz się, jak połączyć program Excel z kontem magazyn Azure, w którym są przechowywane dane skojarzone z klaster HDInsight przy użyciu dodatku Microsoft Power Query dla programu Excel. Wymagane roboczej systemu Windows. Współpraca z systemem Windows i Linux oraz klaster.

* [Nawiązywanie połączenia w programie Excel Hadoop za pomocą sterownika ODBC gałęzi firmy Microsoft](hdinsight-connect-excel-hive-odbc-driver.md): Dowiedz się, jak zaimportować dane z usługi HDInsight za pomocą sterownika ODBC gałęzi firmy Microsoft. Wymagane roboczej systemu Windows. Współpraca z systemem Windows i Linux oraz klaster.

* [Platformy chmury firmy Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Dowiedz się więcej o usłudze Power BI dla Office 365, pobrać wersję próbną programu SQL Server i konfigurowanie programu SharePoint Server 2013 i analizy Biznesowej programu SQL Server.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [Usługi SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
