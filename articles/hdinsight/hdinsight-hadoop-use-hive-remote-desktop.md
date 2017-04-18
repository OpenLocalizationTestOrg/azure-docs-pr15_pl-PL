<properties
   pageTitle="Korzystanie z gałęzi Hadoop i pulpitu zdalnego w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak połączyć się klaster Hadoop w HDInsight przy użyciu pulpitu zdalnego, a następnie uruchom gałęzi kwerendy przy użyciu interfejsu wiersza polecenia gałęzi."
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

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Gałąź za pomocą Hadoop na HDInsight z pulpitu zdalnego

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym artykule będzie Dowiedz się, jak połączyć się z klastrem HDInsight przy użyciu pulpitu zdalnego, a następnie uruchom gałęzi zapytań za pomocą interfejsu wiersza polecenia gałęzi (polecenie).

> [AZURE.NOTE] Ten dokument nie umożliwia szczegółowy opis wykonaj instrukcje HiveQL, które są używane w przykładach. Aby uzyskać informacji na temat HiveQL, który jest używany w tym przykładzie zobacz [Gałęzi korzystanie z Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Klaster HDInsight systemu Windows (Hadoop na HDInsight)

* Komputer kliencki z systemem Windows 10, okno 8 lub Windows 7

##<a id="connect"></a>Łączenie się z pulpitu zdalnego

Włączanie pulpitu zdalnego dla klastrów HDInsight, a następnie postępując zgodnie z instrukcjami na [Nawiązywanie połączenia przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp)się z nim połączyć.

##<a id="hive"></a>Za pomocą polecenia gałęzi

Po podłączeniu do pulpitu dla klastrów HDInsight, wykonaj następujące czynności do pracy z gałęzi:

1. Na komputerze HDInsight można rozpocząć **wiersza polecenia Hadoop**.

2. Wpisz następujące polecenie, aby uruchomić polecenie gałęzi:

        %hive_home%\bin\hive

    Po rozpoczęciu polecenie, pojawi się monit polecenie gałęzi: `hive>`.

3. Za pomocą interfejsu wiersza polecenia, wprowadź poniższe instrukcje, aby utworzyć nową tabelę o nazwie **log4jLogs** za pomocą przykładowych danych:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Usuwanie tabeli**: usuwa tabeli i w pliku danych, jeśli tabela już istnieje.

    * **Tworzenie tabeli zewnętrznej**: tworzy nową tabelę "zewnętrzne" w gałęzi. Zewnętrzne tabele zawierają tylko definicję tabeli w gałęzi (danych pozostaje w pierwotnej lokalizacji).

        > [AZURE.NOTE] Tabele zewnętrzne powinien być używany, gdy można się spodziewać danych źródłowych, które mają być aktualizowane przez zewnętrznego źródła (na przykład procesu przekazywania danych) lub inna operacja MapReduce, ale zawsze ma gałęzi kwerendy do korzystania z najnowszych danych.
        >
        > Usuwanie tabeli zewnętrznej czy **nie** Usuń dane, tylko definicję tabeli.

    * **Formatowanie wierszy**: informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.

    * **PRZECHOWYWANE jako lokalizacji TEXTFILE**: informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i są przechowywane jako tekst.

    * **Wybierz pozycję**: zaznacza liczbę wierszy miejsce, w którym kolumny **t4** zawiera wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ nie istnieją trzy wiersze, które zawierają wartość.

    * **INPUT__FILE__NAME takie jak "%.log"** - zawiera informację, który należy tylko zwróconych danych z plików kończącymi się gałęzi. dziennika. To ogranicza wyszukiwanie do pliku sample.log, który zawiera dane, a zachowany zwraca dane z innych przykładowe pliki danych, które nie pasuje do schematu, możemy zdefiniowanych przez.


4. Aby utworzyć nową tabelę "wewnętrzną" o nazwie **errorLogs**, należy zastosować następujące instrukcje:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Tworzenie tabeli Jeżeli NOT EXISTS**: tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.

        > [AZURE.NOTE] W odróżnieniu od tabele **zewnętrzne** upuszczanie wewnętrznej tabeli powoduje także usunięcie danych źródłowych.

    * **PRZECHOWYWANE jako ORC**: przechowuje dane w wierszu zoptymalizowane tabelarycznego (ORC). To jest format wysoce zoptymalizowane i wydajną do przechowywania danych gałęzi.

    * Zastąp **Wstawianie... Wybierz pozycję**: wybiera wiersze z tabeli **log4jLogs** , które zawierają **[ERROR]**, następnie wstawia dane do tabeli **errorLogs** .

    Aby sprawdzić, czy tylko te wiersze, które zawierają **[ERROR]** w kolumnie t4 są zapisywane w tabeli **errorLogs** , należy użyć następującej instrukcji zwraca wszystkie wiersze z **errorLogs**:

        SELECT * from errorLogs;

    Trzech wierszy danych powinny być zwrócone, zawierające wszystkie **[Błąd]** w t4 kolumny.

##<a id="summary"></a>Podsumowanie

Jak widać, polecenie gałęzi zapewnia łatwy sposób interakcyjne uruchamianie kwerend gałęzi w klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat gałąź w HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Jeśli używasz Tez z gałęzi, zobacz następujące dokumenty dla informacji debugowania:

* [Korzystanie z interfejsu użytkownika Tez na HDInsight systemu Windows](hdinsight-debug-tez-ui.md)

* [Korzystanie z widoku Ambari Tez na podstawie Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

