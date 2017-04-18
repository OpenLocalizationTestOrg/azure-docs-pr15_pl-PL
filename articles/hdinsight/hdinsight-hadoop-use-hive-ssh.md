<properties
   pageTitle="Używanie powłoki gałęzi w HDInsight (Hadoop) | Microsoft Azure"
   description="Dowiedz się, jak używać powłoki gałęzi z klastrem HDInsight systemem Linux. Zostanie Dowiedz się, jak połączyć się z klastrem HDInsight przy użyciu SSh, a następnie za pomocą powłoki gałęzi interakcyjne uruchamianie kwerend."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Gałąź za pomocą Hadoop w HDInsight z SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym artykule dowiesz się, jak używać Secure Shell (SSH) nawiązywanie połączenia z Hadoop w klastrze Azure HDInsight, a następnie interakcyjne przesyłać gałęzi zapytań za pomocą interfejsu wiersza polecenia programu Hive (polecenie).

> [AZURE.IMPORTANT] Polecenie gałęzi są dostępne na systemem Linux HDInsight klastrów, należy rozważyć użycie Beeline. Beeline jest nowszego klienta do pracy z gałęzi i znajduje się w klastrze HDInsight. Aby uzyskać więcej informacji na temat korzystania z tego zobacz [Używanie gałęzi z Hadoop w HDInsight z Beeline](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Systemem Linux Hadoop w klastrze HDInsight.

* Klient SSH. Linux, Unix lub systemu Mac OS powinna pochodzić z klientem SSH. Użytkownicy systemu Windows musi pobrać klienta, takich jak [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Łączenie się z SSH

Nawiązać połączenie w pełni kwalifikowaną nazwę domeny (FQDN) programu klaster HDInsight przy użyciu polecenia SSH. Nazwy FQDN będzie nazwa nadana klaster, następnie **. azurehdinsight.net**. Na przykład poniższy chcesz ustanowić połączenie z klastrem o nazwie **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli opisane klucza certyfikatu uwierzytelniania SSH** po utworzeniu klaster HDInsight, może być konieczne Określ lokalizację klucz prywatny w systemie klienta:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli opisane hasło do uwierzytelniania SSH** po utworzeniu klaster HDInsight, będzie konieczne hasło po wyświetleniu monitu.

Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kit (klientów opartych na systemie Windows)

System Windows nie udostępnia wbudowane klienta SSH. Firma Microsoft zaleca używanie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji na temat korzystania z Kit Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Za pomocą polecenia gałęzi

2. Po połączeniu, uruchom polecenie gałęzi przy użyciu następującego polecenia:

        hive

3. Za pomocą interfejsu wiersza polecenia, wprowadź poniższe instrukcje, aby utworzyć nową tabelę o nazwie **log4jLogs** za pomocą przykładowych danych:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Poniższe instrukcje należy wykonać następujące czynności:

    * **DROP TABLE** — powoduje usunięcie tabeli i w pliku danych, w przypadku, gdy tabela już istnieje.
    * **Tworzenie tabeli zewnętrznej** — powoduje utworzenie nowej tabeli "zewnętrzne" w gałęzi. Zewnętrzne tabele zawierają tylko definicji tabeli w gałęzi. Dane pozostaje w pierwotnej lokalizacji.
    * **FORMAT wiersza** — informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.
    * **PRZECHOWYWANY w lokalizacji TEXTFILE** — informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i że są przechowywane jako tekst.
    * **Wybierz pozycję** - zaznacza liczbę wierszy miejsce, w którym kolumny **t4** zawiera wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ istnieją trzy wiersze, które zawierają wartość.
    * **INPUT__FILE__NAME takie jak "%.log"** - zawiera informację, który należy tylko zwróconych danych z plików kończącymi się gałęzi. dziennika. To ogranicza wyszukiwanie do pliku sample.log, który zawiera dane, a zachowany zwraca dane z innych przykładowe pliki danych, które nie pasuje do schematu, możemy zdefiniowanych przez.

    > [AZURE.NOTE] Jeśli oczekujesz danych źródłowych zostaną zaktualizowane przez źródło zewnętrzne, taką jak proces przekazywania danych lub przez inną operację MapReduce, ale zawsze ma gałęzi kwerendy do korzystania z najnowszych danych, należy użyć tabele zewnętrzne.
    >
    > Usuwanie tabeli zewnętrznej czy **nie** Usuń dane, tylko definicję tabeli.

4. Aby utworzyć nową tabelę "wewnętrzną" o nazwie **errorLogs**, należy zastosować następujące instrukcje:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Tworzenie tabeli IF NOT EXISTS** - tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.
    * **PRZECHOWYWANE jako ORC** - są przechowywane dane w formacie zoptymalizowane wiersza kolumnowy (ORC). To jest wysoce zoptymalizowane i wydajną format do przechowywania danych gałęzi.
    * Zastąp **Wstawianie... Wybierz** - wybiera wiersze z tabeli **log4jLogs** , które zawierają **[Błąd]**, a następnie wstawia dane do tabeli **errorLogs** .

    Aby sprawdzić, czy tylko wiersze zawierające w kolumnie t4 **[ERROR]** są zapisywane w tabeli **errorLogs** , należy użyć następującej instrukcji zwraca wszystkie wiersze z **errorLogs**:

        SELECT * from errorLogs;

    Trzech wierszy danych powinny być zwrócone, zawierające wszystkie **[ERROR]** w t4 kolumny.

    > [AZURE.NOTE] W odróżnieniu od tabele zewnętrzne upuszczanie wewnętrznej tabeli spowoduje usunięcie danych źródłowych.

##<a id="summary"></a>Podsumowanie

Jak widać, polecenie gałęzi zapewnia łatwy sposób interakcyjne uruchamianie kwerend gałęzi w klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a id="nextsteps"></a>Następne kroki

Ogólne informacje na temat gałąź w HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje na inne sposoby pracy z Hadoop na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Jeśli używasz Tez z gałęzi, zobacz następujące dokumenty dla informacji debugowania:

* [Korzystanie z interfejsu użytkownika Tez na HDInsight systemu Windows](hdinsight-debug-tez-ui.md)

* [Korzystanie z widoku Ambari Tez na podstawie Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

