<properties
   pageTitle="Hadoop Sqoop za pomocą zwinięcie w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak zdalnie przesyłać zadania Sqoop z usługą hdinsight za pomocą zwinięcie."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Wykonywanie zadań Sqoop z Hadoop w HDInsight z zwinięcie

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Dowiedz się, jak używać zwinięcie do przeprowadzania zadań Sqoop w klastrze Hadoop w HDInsight.

Zwinięcie służy do pokazano, jak możesz korzystać ze HDInsight przy użyciu pierwotne żądania HTTP, aby uruchomić, monitorować i pobrać wyniki Sqoop zadania. Działa to przy użyciu WebHCat interfejsu API usługi REST (wcześniej znany jako Templeton) dostarczony przez klaster HDInsight.

> [AZURE.NOTE] Jeśli znają korzystanie z systemem Linux Hadoop serwerów, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz [informacje o korzystaniu z usługi HDInsight w systemie Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Hadoop w klastrze HDInsight (Linux lub opartych na systemie Windows)

* [Zwinięcie](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Przesyłanie Sqoop zadań przy użyciu zwinięcie

> [AZURE.NOTE] Podczas korzystania z WebHCat zwinięcie i wszelkie inne metody komunikacji pozostałych, podając nazwę użytkownika i hasło administratora klastrów HDInsight wymagane jest uwierzytelnienie żądania Należy również użyć nazwy klaster w ramach programu identyfikator URI (Uniform Resource) służy do wysyłania żądania na serwerze.
>
> Polecenia w tej sekcji Zamień **nazwę użytkownika** z użytkownikiem w celu uwierzytelnienia z klastrem i zastąpić **hasło** hasło do konta użytkownika. Zamień **NAZWAKLASTRA** nazwę klaster.
>
> Interfejsu API usługi REST jest zabezpieczone za pomocą [uwierzytelniania podstawowego](http://en.wikipedia.org/wiki/Basic_access_authentication). Zawsze należy żądania za pomocą protokołu HTTP bezpiecznego (HTTPS) upewnij się, że poświadczenia są bezpieczne wysyłane na serwerze.

1. W wierszu polecenia wpisz następujące polecenie aby sprawdzić, czy możesz połączyć się ze klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Powinien zostać wyświetlony odpowiedzi podobny do następującego:

        {"status":"ok","version":"v1"}

    Parametry używane w tym poleceniu są następujące:

    * **-u** - nazwę użytkownika i hasło używane do uwierzytelnienia wezwanie.
    * **-G** - wskazuje, że jest żądania GET.

    Na początek adres URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**jest taki sam dla wszystkich żądań. Ścieżka **/status/status**wskazuje, że żądania jest aby przywrócić stan WebHCat (nazywane także Templeton) na serwerze. 

2. Prześlij zadanie sqoop należy wykonać następujące kroki:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Parametry używane w tym poleceniu są następujące:

    * **-d** - od `-G` nie jest używany, żądanie zostanie przyjęta wartość domyślna metody POST. `-d`Umożliwia określenie wartości danych, które są wysyłane z żądaniem.

        * **user.name** - użytkownika, który jest uruchomienie polecenia.

        * **polecenie** - Sqoop polecenie do wykonania.

        * **statusdir** - katalogu, w którym zostanie zapisany stan dla danego zadania.

    To polecenie powinna zwrócić identyfikator zadania, który może być używany, aby sprawdzić stan zadania.

        {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia. Zamień **JOBID** wartość zwracana w poprzednim kroku. Na przykład zwrócona wartość `{"id":"job_1415651640909_0026"}`, a następnie będzie **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jeśli zadanie zostało ukończone, stan będzie **powiodło się**.

    > [AZURE.NOTE] To żądanie zwinięcie zwraca dokumentu notacji obiektu JavaScript (JSON) informacje o zadaniu; jq jest używana do pobierania wartości stan.

4. Gdy stan zadania zmieniła się **powiodło się**, można pobrać wyniki zadania z magazynem obiektów Blob platformy Azure. `statusdir` Parametr przekazano z zapytaniem zawiera lokalizacji w pliku docelowym; w tym przypadku **wasbs: / / / przykład/zwinięcie**. Ten adres są przechowywane dane wyjściowe zadania w katalogu **przykładzie/zwinięcie** kontenera magazynu domyślne używane przez klaster HDInsight.

    Można wyświetlić listę i Pobierz te pliki przy użyciu [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md). Aby na przykład listę plików w **przykładzie/zwinięcie**Użyj następującego polecenia:

        azure storage blob list <container-name> example/curl

    Aby pobrać plik, użyj następujących opcji:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Należy określić nazwę konta magazynu zawierającego to przy użyciu `-a` i `-k` parametry lub zestawu **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_dostępu\_klucza** zmiennych środowiska. Zobacz < odwołanie = "hdinsight przekazywania data.md" target = "_blank", aby uzyskać więcej informacji.

##<a name="limitations"></a>Ograniczenia

* Zbiorcze export - systemem Linux z usługi HDInsight, łącznik Sqoop umożliwia eksportowanie danych do programu Microsoft SQL Server lub bazy danych SQL Azure nie obsługuje obecnie wstawia zbiorczo.

* Tworzeniu partii — z systemem Linux HDInsight podczas korzystania z `-batch` przełączanie podczas wykonywania instrukcji INSERT, Sqoop wykona wielu wstawia zamiast tworzeniu partii operacje wstawiania.

##<a name="summary"></a>Podsumowanie

Jak pokazano w tym dokumencie, umożliwia nieprzetworzonych żądania HTTP uruchomić, monitorować i wyświetlić wyniki Sqoop zadań w klastrze HDInsight.

Uzyskać więcej informacji o interfejsie pozostałych używanych w tym artykule zobacz <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Przewodnik po interfejsie API usługi REST Sqoop</a>.

##<a name="next-steps"></a>Następne kroki

Aby uzyskać ogólne informacje na gałęzi z usługi HDInsight:

* [Sqoop za pomocą Hadoop na HDInsight](hdinsight-use-sqoop.md)

Aby uzyskać informacje na inne sposoby pracy z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

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


