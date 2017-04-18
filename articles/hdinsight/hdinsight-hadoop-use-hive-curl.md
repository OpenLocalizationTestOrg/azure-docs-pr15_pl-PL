<properties
   pageTitle="Gałąź Hadoop za pomocą zwinięcie w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak zdalnie przesyłać zadania świnka z usługą hdinsight za pomocą zwinięcie."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Uruchamianie kwerend gałęzi z Hadoop w HDInsight z zwinięcie

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym dokumencie dowiesz się, jak za pomocą zwinięcie uruchamianie kwerend gałęzi na Hadoop w klastrze Azure HDInsight.

Zwinięcie służy do pokazano, jak możesz korzystać ze HDInsight przy użyciu pierwotne żądania HTTP, aby uruchomić, monitorować i pobrać wyniki kwerendy gałęzi. Działa to przy użyciu WebHCat interfejsu API usługi REST (wcześniej znany jako Templeton) dostarczony przez klaster HDInsight.

> [AZURE.NOTE] Jeśli znają korzystanie z systemem Linux Hadoop serwerów, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz, [co należy wiedzieć o Hadoop na podstawie Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Hadoop w klastrze HDInsight (Linux lub opartych na systemie Windows)

* [Zwinięcie](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Uruchamianie kwerend gałęzi przy użyciu zwinięcie

> [AZURE.NOTE] Podczas korzystania z WebHCat zwinięcie i wszelkie inne metody komunikacji pozostałych, podając nazwę użytkownika i hasło administratora klastrów HDInsight wymagane jest uwierzytelnienie żądania Należy również użyć nazwy klaster w ramach programu identyfikator URI (Uniform Resource) służy do wysyłania żądania na serwerze.
>
> Polecenia w tej sekcji Zamień **nazwę użytkownika** z użytkownikiem w celu uwierzytelnienia z klastrem i zastąpić **hasło** hasło do konta użytkownika. Zamień **NAZWAKLASTRA** nazwę klaster.
>
> Interfejsu API usługi REST jest zabezpieczona za pomocą [uwierzytelniania podstawowego](http://en.wikipedia.org/wiki/Basic_access_authentication). Zawsze należy żądania za pomocą protokołu HTTP bezpiecznego (HTTPS) upewnij się, że poświadczenia są bezpieczne wysyłane na serwerze.

1. W wierszu polecenia wpisz następujące polecenie aby sprawdzić, czy możesz połączyć się ze klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Powinien zostać wyświetlony odpowiedzi podobny do następującego:

        {"status":"ok","version":"v1"}

    Parametry używane w tym poleceniu są następujące:

    * **-u** - nazwę użytkownika i hasło używane do uwierzytelnienia wezwanie.
    * **-G** - wskazuje, że jest żądania GET.

    Na początek adres URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**jest taki sam dla wszystkich żądań. Ścieżka **/status/status**wskazuje, że żądania jest aby przywrócić stan WebHCat (nazywane także Templeton) na serwerze. Możesz również zażądać wersji gałęzi przy użyciu następującego polecenia:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    To powinna zwrócić odpowiedź podobną do następującej:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Tworzenie nowej tabeli o nazwie **log4jLogs**należy wykonać następujące kroki:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Parametry używane w tym poleceniu są następujące:

    * **-d** - od `-G` nie jest używany, żądanie zostanie przyjęta wartość domyślna metody POST. `-d`Umożliwia określenie wartości danych, które są wysyłane z żądaniem.

        * **user.name** - użytkownika, który jest uruchomienie polecenia.

        * **Wykonywanie** — HiveQL instrukcji w celu wykonania.

        * **statusdir** - katalogu, w którym zostanie zapisany stan dla danego zadania.

    Poniższe instrukcje należy wykonać następujące czynności:

    * **DROP TABLE** — powoduje usunięcie tabeli i w pliku danych, jeśli tabela już istnieje.

    * **Tworzenie tabeli zewnętrznej** — powoduje utworzenie nowej tabeli "zewnętrzne" w gałęzi. Zewnętrzne tabele zawierają tylko definicję tabeli w gałęzi. Dane pozostaje w pierwotnej lokalizacji.

        > [AZURE.NOTE] Jeśli oczekujesz danych źródłowych zostaną zaktualizowane przez źródło zewnętrzne, taką jak proces przekazywania danych lub przez inną operację MapReduce, ale zawsze ma gałęzi kwerendy do korzystania z najnowszych danych, należy użyć tabele zewnętrzne.
        >
        > Usuwanie tabeli zewnętrznej czy **nie** Usuń dane, tylko definicję tabeli.

    * **FORMAT wiersza** — informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.

    * **PRZECHOWYWANE jako lokalizacji TEXTFILE** — informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i że są przechowywane jako tekst.

    * **Wybierz pozycję** - zaznacza liczbę wierszy miejsce, w którym kolumny **t4** zawiera wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ istnieją trzy wiersze, które zawierają wartość.

    > [AZURE.NOTE] Należy zauważyć, że spacje między HiveQL instrukcje są zamieniane `+` znak użycia z zwinięcie. Cudzysłowie wartości, które zawierają miejsca, na przykład ogranicznik, nie należy zastąpić `+`.

    * **INPUT__FILE__NAME LIKE "% 25.log"** - tym limity wyszukiwania tylko korzystanie z plików kończącymi się. dziennika. Jeśli to nie jest dostępna, gałęzi spróbuje wyszukać wszystkie pliki w tym katalogu i jego podkatalogów, w tym pliki, które nie są zgodne ze schematem kolumny zdefiniowane dla tej tabeli.

    > [AZURE.NOTE] Należy zauważyć, że 25% jest adres URL zakodowany formularza %, dlatego stan aktualny `like '%.log'`. % Musi być zakodowany, adres URL jest traktowany jako znak specjalny w adresy URL.

    To polecenie powinna zwrócić identyfikator zadania, który może być używany, aby sprawdzić stan zadania.

        {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia. Zamień **JOBID** wartość zwracana w poprzednim kroku. Na przykład zwrócona wartość `{"id":"job_1415651640909_0026"}`, a następnie będzie **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jeśli zadanie zostało ukończone, stan będzie **powiodło się**.

    > [AZURE.NOTE] To żądanie zwinięcie zwraca dokumentu notacji obiektu JavaScript (JSON) informacje o zadaniu; jq jest używana do pobierania wartości stan.

4. Gdy stan zadania zmieniła się **powiodło się**, można pobrać wyniki zadania z magazynem obiektów Blob platformy Azure. `statusdir` Parametr przekazano z zapytaniem zawiera lokalizacji w pliku docelowym; w tym przypadku **wasbs: / / / przykład/zwinięcie**. Ten adres są przechowywane dane wyjściowe zadania w katalogu **przykładzie/zwinięcie** w kontenerze miejsca do magazynowania domyślne używane przez klaster HDInsight.

    Można wyświetlić listę i Pobierz te pliki przy użyciu [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md). Aby na przykład listę plików w **przykładzie/zwinięcie**Użyj następującego polecenia:

        azure storage blob list <container-name> example/curl

    Aby pobrać plik, użyj następujących opcji:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Należy określić nazwę konta magazynu zawierającego to przy użyciu `-a` i `-k` parametry lub zestawu **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_dostępu\_klucza** zmiennych środowiska. Zobacz < odwołanie = docelowy "hdinsight przekazywania data.md" = "_blank", aby uzyskać więcej informacji.

6. Aby utworzyć nową tabelę "wewnętrzną" o nazwie **errorLogs**, należy zastosować następujące instrukcje:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Poniższe instrukcje należy wykonać następujące czynności:

    * **Tworzenie tabeli IF NOT EXISTS** - tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.

        > [AZURE.NOTE] W odróżnieniu od tabele zewnętrzne upuszczanie wewnętrznej tabeli spowoduje usunięcie danych źródłowych.

    * **PRZECHOWYWANE jako ORC** - są przechowywane dane w formacie zoptymalizowane wiersza kolumnowym (ORC). To jest wysoce zoptymalizowanej i wydajną format do przechowywania danych gałęzi.
    * Zastąp **Wstawianie... Wybierz** - wybiera wiersze z tabeli **log4jLogs** , które zawierają **[Błąd]**, a następnie wstawia dane do tabeli **errorLogs** .
    * **Wybierz pozycję** - zaznacza wszystkie wiersze z nowej tabeli **errorLogs** .

7. Za pomocą identyfikator zadania, zwracany, aby sprawdzić stan zadania. Po pomyślnym, umożliwia polecenie Azure dla komputerów Mac, Linux i Windows opisane wcześniej pobrać i wyświetlić wyniki. Wynik powinien zawierać trzy wiersze, które zawierają **[Błąd]**.


##<a id="summary"></a>Podsumowanie

Jak pokazano w tym dokumencie, umożliwia nieprzetworzonych żądania HTTP uruchomić, monitorować i wyświetlić wyniki zadań gałęzi w klastrze HDInsight.

Uzyskać więcej informacji o interfejsie pozostałych używanych w tym artykule zobacz <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">Odwołanie WebHCat</a>.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na gałęzi z usługi HDInsight:

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




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


