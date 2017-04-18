<properties
   pageTitle="MapReduce i pulpitu zdalnego z Hadoop w HDInsight | Microsoft Azure"
   description="Informacje o sposobie korzystania z pulpitu zdalnego do łączenia się Hadoop na HDInsight i MapReduce zadań."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Używanie MapReduce w Hadoop na HDInsight z pulpitu zdalnego

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

W tym artykule dowiesz się, jak połączyć się Hadoop w klastrze HDInsight przy użyciu pulpitu zdalnego, a następnie uruchom MapReduce zadań za pomocą polecenia Hadoop.

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Klaster HDInsight systemu Windows (Hadoop na HDInsight)

* Komputer kliencki z systemem Windows 10, Windows 8 lub Windows 7

##<a id="connect"></a>Łączenie się z pulpitu zdalnego

Włączanie pulpitu zdalnego dla klastrów HDInsight, a następnie postępując zgodnie z instrukcjami na [Nawiązywanie połączenia przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp)się z nim połączyć.

##<a id="hadoop"></a>Za pomocą polecenia Hadoop

Po nawiązaniu połączenia na pulpicie dla klastrów HDInsight, wykonaj następujące czynności, aby uruchomić zadanie MapReduce przy użyciu polecenia Hadoop:

1. Na komputerze HDInsight można rozpocząć **wiersza polecenia Hadoop**. Spowoduje to otwarcie nowego wiersza polecenia w **c:\apps\dist\hadoop-&lt;numer wersji >** katalogu.

    > [AZURE.NOTE] Numer wersji zmienia się jako Hadoop zostanie zaktualizowany. Zmienna środowiska **HADOOP_HOME** można znaleźć ścieżki. Na przykład `cd %HADOOP_HOME%` zmian katalogów do katalogu Hadoop, bez konieczności ustalić numer wersji.

2. Aby użyć polecenia **Hadoop** , aby uruchomić zadanie MapReduce przykład, użyj następującego polecenia:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Zostanie uruchomiony klasy **wordcount** , która znajduje się w pliku **hadoop-mapreduce-examples.jar** w bieżącym katalogu. Jako dane wejściowe, używa dokumentu **wasbs://example/data/gutenberg/davinci.txt** , a wynik jest przechowywany w: **wasbs: / / / przykład/danych i WordCountOutput**.

    > [AZURE.NOTE] Aby uzyskać więcej informacji o tym zadaniu MapReduce i przykładowymi danymi zobacz <a href="hdinsight-use-mapreduce.md">Używanie MapReduce w HDInsight Hadoop</a>.

2. Zadanie emituje informacji jest przetwarzana, a po zakończeniu zadania informacje zwraca podobny do następującego:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po ukończeniu zadania użyj następującego polecenia, aby wyświetlić listę plików wyjściowych przechowywane w **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    To powinien być wyświetlany dwa pliki, **_SUCCESS** i **części r-00000**. Plik **części r-00000** zawiera dane wyjściowe dla tego zadania.

    > [AZURE.NOTE] Niektóre zadania MapReduce może podzielić wyniki na wielu plików **części r-###** . Jeśli tak, za pomocą ### sufiks wskazującego pliki.

4. Aby wyświetlić wynik, użyj następującego polecenia:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Spowoduje to wyświetlenie listy wyrazów, które znajdują się w pliku **wasbs://example/data/gutenberg/davinci.txt** , wraz z liczba wystąpienia każdego wyrazu. Oto przykład dane, które będą zawarte w pliku:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Podsumowanie

Jak widać, polecenie Hadoop zapewnia łatwy sposób uruchamiania zadań MapReduce na klastrze HDInsight, a następnie Wyświetl wynik zadania.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o zadaniach MapReduce w HDInsight:

* [Używanie MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)
