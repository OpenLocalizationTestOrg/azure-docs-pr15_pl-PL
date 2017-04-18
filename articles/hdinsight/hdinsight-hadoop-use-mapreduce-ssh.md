<properties
   pageTitle="MapReduce i SSH połączenie z Hadoop w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak używać SSH do przeprowadzania zadań MapReduce przy użyciu Hadoop na HDInsight."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce za pomocą Hadoop na HDInsight z SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

W tym artykule dowiesz się, jak używać Secure Shell (SSH) nawiązywanie połączenia z Hadoop w klastrze HDInsight, a następnie przesyłać MapReduce zadań za pomocą poleceń Hadoop.

> [AZURE.NOTE] Jeśli znasz już korzystanie z systemem Linux Hadoop serwerów, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz [porady HDInsight systemem Linux](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Klaster systemem Linux HDInsight (Hadoop na HDInsight)

* Klient SSH. Systemy operacyjne Linux, Unix i Mac powinna pochodzić z klientem SSH. Użytkownicy systemu Windows musi pobrać klienta, takich jak [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Łączenie się z SSH

Nawiązać połączenie w pełni kwalifikowaną nazwę domeny (FQDN) programu klaster HDInsight przy użyciu polecenia SSH. Kwalifikowaną będzie nazwa nadana klaster, a następnie **. azurehdinsight.net**. Na przykład poniższy chcesz ustanowić połączenie z klastrem o nazwie **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli opisane klucza certyfikatu uwierzytelniania SSH** po utworzeniu klaster HDInsight, może być konieczne Określ lokalizację klucz prywatny w systemie klienta, na przykład:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli opisane hasło do uwierzytelniania SSH** po utworzeniu klaster HDInsight, będzie konieczne hasło po wyświetleniu monitu.

Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Kit (klientów systemu Windows)

System Windows nie udostępnia wbudowane klienta SSH. Firma Microsoft zaleca używanie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji na temat korzystania z Kit Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Za pomocą poleceń Hadoop

1. Po połączeniu z klastrem HDInsight, użyj następującego polecenia **Hadoop** , aby rozpocząć zadanie MapReduce:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Zostanie uruchomiony klasy **wordcount** , która znajduje się w pliku **hadoop-mapreduce-examples.jar** . Jako dane wejściowe, używa dokumentu **wasbs://example/data/gutenberg/davinci.txt** , a wynik jest przechowywany w **wasbs: / / / przykład/danych i WordCountOutput**.

    > [AZURE.NOTE] Aby uzyskać więcej informacji o tym zadaniu MapReduce i przykładowymi danymi zobacz [Używanie MapReduce w Hadoop na HDInsight](hdinsight-use-mapreduce.md).

2. Zadanie emituje szczegóły przetwarza, a po zakończeniu zadania informacje zwraca podobny do następującego:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po zakończeniu zadania, użyj następującego polecenia, aby wyświetlić listę plików wyjściowych, które są przechowywane w **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    To powinien być wyświetlany dwa pliki, **_SUCCESS** i **części r-00000**. Plik **części r-00000** zawiera dane wyjściowe dla tego zadania.

    > [AZURE.NOTE] Niektóre zadania MapReduce może podzielić wyniki na wielu plików **części r-###** . Jeśli tak, za pomocą ### sufiks, aby wskazać kolejność plików.

4. Aby wyświetlić wynik, użyj następującego polecenia:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Spowoduje to wyświetlenie listy wyrazów, które są zawarte w pliku **wasbs://example/data/gutenberg/davinci.txt** i liczba wystąpienia każdego wyrazu. Oto przykład dane, które będą zawarte w pliku:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Podsumowanie

Jak widać, polecenia Hadoop umożliwiają łatwe uruchamianie zadań MapReduce w klastrze HDInsight, a następnie wyświetlenie wyniku zadania.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o zadaniach MapReduce w HDInsight:

* [Używanie MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)
