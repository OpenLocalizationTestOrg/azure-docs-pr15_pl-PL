<properties
    pageTitle="Za pomocą interakcyjnego gałąź w HDInsight | Microsoft Azure"
    description="Dowiedz się, jak za pomocą interakcyjnego gałęzi (gałęzi na LLAP) w HDInsight."
    keywords=""
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
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Za pomocą interakcyjnego gałąź w HDInsight (wersja Preview)

Interakcyjne gałęzi (alias [Długości live a proces]( https://cwiki.apache.org/confluence/display/Hive/LLAP)). jest to nowy HDInsight [typu klaster]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Umożliwia interakcyjne gałęzi w pamięci, pamięci podręcznej, dzięki któremu kwerend gałęzi bardziej interakcyjne i szybsze. Tej nowej funkcji przekształca HDInsight na świecie większość performant, elastyczne, i otwórz rozwiązania duży danych w chmurze z w pamięci przechowuje w pamięci podręcznej (za pomocą gałęzi i Spark) i zaawansowanych analiz za pośrednictwem ścisła integracja z usługami R. 

Klaster interakcyjnych gałęzi różni się od klaster Hadoop. Zawiera ona tylko usługę gałęzi. 

> [AZURE.NOTE] MapReduce, świnka Sqoop, Oozie i innych usług zostaną usunięte z tego typu klaster wkrótce.
Usługa gałęzi w klastrze interakcyjne gałęzi jest dostępny tylko za pośrednictwem widoku gałąź Ambari, Beeline i gałęzi ODBC. Nie są dostępne za pośrednictwem konsoli gałęzi, Templeton, polecenie Azure i Azure programu PowerShell. 


 


## <a name="create-an-interactive-hive-cluster"></a>Tworzenie interakcyjnych gałęzi klaster

Interakcyjne klaster gałąź jest obsługiwana tylko na podstawie Linux klastrów. Aby uzyskać informacji na temat tworzenia klastrów HDInsight zobacz [klastrów opartych na tworzenie Linux Hadoop w HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Wykonywanie gałęzi z interakcyjnych gałęzi

Istnieją różne opcje, jak można wykonywać gałęzi kwerendy:

- Uruchamianie gałęzi przy użyciu widoku gałąź Ambari

    Aby uzyskać informacje o korzystaniu z widoku gałęzi zobacz [Używanie widoku gałęzi z Hadoop w HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Uruchamianie gałęzi przy użyciu Beeline

    Aby uzyskać informacje na temat korzystania z Beeline na HDInsight zobacz [Używanie gałęzi z Hadoop w HDInsight z Beeline](hdinsight-hadoop-use-hive-beeline.md).

    Możesz użyć Beeline z headnode lub węzeł pustych krawędzi.  Zaleca się przy użyciu Beeline z węzła pustych krawędzi.  Informacje na temat tworzenia klastrze HDInsight z pustym edgenode zobacz [Używanie pustych krawędzi węzłów na HDInsight](hdinsight-apps-use-edge-node.md).

- Uruchamianie gałęzi przy użyciu gałęzi ODBC

    Aby uzyskać informacje na temat korzystania z gałęzi ODBC zobacz [Nawiązywanie połączenia w programie Excel Hadoop za pomocą sterownika ODBC gałęzi firmy Microsoft](hdinsight-connect-excel-hive-odbc-driver.md).

**Aby znaleźć JDBC parametry połączenia:**

1.  Zaloguj się Ambari przy użyciu następujący adres URL: https://<ClusterName>. AzureHDInsight.net.
2.  Kliknij **gałąź** z menu po lewej stronie.
3.  Kliknij ikonę z wyróżnionym skopiuj adres URL:

    ![Usługa HDInsight Hadoop interakcyjnych gałąź LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Zobacz też
-   [Klaster oparte na tworzenie Linux Hadoop w HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Dowiedz się, jak tworzyć interaktywne gałęzi klastrów w HDInsight.
-   [Za pomocą gałęzi z Hadoop w HDInsight z Beeline](hdinsight-hadoop-use-hive-beeline.md): Dowiedz się, jak za pomocą Beeline przesyłać kwerendy gałęzi.
-   [Nawiązywanie połączenia w programie Excel Hadoop za pomocą sterownika ODBC gałęzi firmy Microsoft](hdinsight-connect-excel-hive-odbc-driver.md): Dowiedz się, jak połączyć program Excel do gałęzi.
