<properties
   pageTitle="Gałąź kwerendy przy użyciu narzędzia Hadoop programu Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak używać gałęzi z Hadoop w HDInsight za pomocą narzędzia Visual Studio Hadoop."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Uruchamianie kwerend gałęzi przy użyciu narzędzia HDInsight programu Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym artykule dowiesz się, jak przesyłać kwerendy gałęzi z klastrem HDInsight za pomocą narzędzi HDInsight programu Visual Studio.

> [AZURE.NOTE] Ten dokument nie umożliwia szczegółowy opis wykonaj instrukcje HiveQL używane w przykładach. Aby uzyskać informacji na temat HiveQL, który jest używany w tym przykładzie zobacz [Gałęzi korzystanie z Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy.

* Klaster Azure HDInsight (Hadoop na HDInsight) (Linux lub opartych na systemie Windows)

* Programu Visual Studio (jeden z następujących wersji):

    Visual Studio 2013 społeczności i Professional i Premium i Ultimate w przypadku [aktualizacji 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (społeczności i przedsiębiorstw)

- Usługa HDInsight narzędzia programu Visual studio. Aby uzyskać informacje o instalowaniu i konfigurowaniu narzędzia, zobacz [Wprowadzenie do korzystania z narzędzia Visual Studio Hadoop dla HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a id="run"></a>Uruchamianie kwerend gałęzi przy użyciu narzędzia HDInsight programu Visual Studio

1. Otwórz **Program Visual Studio** i wybierz pozycję **Nowy** > **projektu** > **HDInsight** > **Gałęzi aplikacji**. Podaj nazwę dla tego projektu.

2. Otwórz plik **Script.hql** , który jest tworzony z tego projektu i Wklej w poniższych stwierdzeń HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Usuwanie tabeli**: usuwa tabeli i w pliku danych, jeśli tabela już istnieje.
    * **Tworzenie tabeli zewnętrznej**: tworzy nową tabelę "zewnętrzne" w gałęzi. Zewnętrzne tabele zawierają tylko definicja tabeli w gałęzi (danych pozostaje w pierwotnej lokalizacji).

        > [AZURE.NOTE] Tabele zewnętrzne powinien być używany, gdy można się spodziewać danych źródłowych, które mają być aktualizowane przez zewnętrznego źródła (na przykład procesu przekazywania danych) lub inna operacja MapReduce, ale zawsze ma gałęzi kwerendy do korzystania z najnowszych danych.
        >
        > Usuwanie tabeli zewnętrznej czy **nie** Usuń dane, tylko definicję tabeli.

    * **Formatowanie wierszy**: informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.
    * **PRZECHOWYWANE jako lokalizacji TEXTFILE**: informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i są przechowywane jako tekst.
    * **Wybierz pozycję**: Wybierz liczbę wierszy miejsce, w którym kolumny **t4** zawierają wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ nie istnieją trzy wiersze, które zawierają wartość.
    * **INPUT__FILE__NAME takie jak "%.log"** - zawiera informację, który należy tylko zwróconych danych z plików kończącymi się gałęzi. dziennika. To ogranicza wyszukiwanie do pliku sample.log, który zawiera dane, a zachowany z zwraca dane z innych przykładowe pliki danych, które nie pasuje do schematu, możemy zdefiniowanych przez.

3. Na pasku narzędzi wybierz **Klaster HDInsight** , który ma być używany dla tej kwerendy, a następnie wybierz **Prześlij, aby WebHCat** , aby uruchomić instrukcje jako zadanie programu Hive przy użyciu WebHCat. Możesz również przesłać zadania za pomocą przycisku __wykonywanie za pośrednictwem HiveServer2__ , jeśli HiveServer2 jest dostępna w wersji klaster. **Gałąź podsumowanie zadań** pojawia się i są wyświetlane informacje o zadaniu uruchomiony. Aby odświeżyć informacje o zadaniu, aż zmieni **Stan zadania** **wykonane**, należy skorzystać z łącza **odświeżanie** .

4. Łącze **Dane wyjściowe zadania** umożliwia wyświetlanie danych wyjściowych tego zadania. Powinien być wyświetlany `[ERROR] 3`, która jest wartość zwrócona przez instrukcji SELECT.

5. Można również uruchomić kwerendy gałęzi bez tworzenia projektu. Za pomocą **Eksploratora serwera**, rozwiń **Azure** > **HDInsight**, kliknij prawym przyciskiem myszy serwer usługi HDInsight, a następnie wybierz **napisać kwerendę gałęzi**.

6. W dokumencie **temp.hql** , który zostanie wyświetlony Dodaj następujące instrukcje HiveQL:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Tworzenie tabeli IF NOT EXISTS**: tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.

        > [AZURE.NOTE] W przeciwieństwie do tabel **zewnętrznych** upuszczanie wewnętrznej tabeli powoduje także usunięcie danych źródłowych.

    * **PRZECHOWYWANE jako ORC**: przechowuje dane w wierszu zoptymalizowane tabelarycznego (ORC). To jest wysoce zoptymalizowanej i wydajną format do przechowywania danych gałęzi.
    * Zastąp **Wstawianie... Wybierz pozycję**: umożliwia zaznaczenie wierszy z tabeli **log4jLogs** zawierające **[ERROR]**, następnie wstawia dane do tabeli **errorLogs** .

7. Na pasku narzędzi wybierz pozycję **Prześlij** do wykonywania zadania. **Stan zadania** umożliwia określenie, czy zadanie zakończyło się pomyślnie.

8. Aby sprawdzić, czy zadania wykonane i utworzyć nową tabelę, za pomocą **Eksploratora serwera** , a następnie rozwiń węzeł **Azure** > **HDInsight** > klaster HDInsight > **Gałęzi bazy danych** > i **domyślny**. Powinny być widoczne w tabeli **errorLogs** i tabeli **log4jLogs** .

##<a id="summary"></a>Podsumowanie

Jak widać, narzędzia HDInsight programu Visual Studio umożliwiają łatwe uruchamianie kwerend gałęzi w klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat gałąź w HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Aby uzyskać więcej informacji na temat narzędzia HDInsight programu Visual Studio:

* [Wprowadzenie do narzędzia HDInsight programu Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
