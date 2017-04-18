<properties
   pageTitle="Świnka Hadoop za pomocą zwinięcie w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak używać zwinięcie do przeprowadzania zadań łaciński świnka w klastrze Hadoop w Azure HDInsight."
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

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Uruchamianie zadań świnka z Hadoop na HDInsight przy użyciu zwinięcie

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

W tym dokumencie dowiesz się, jak za pomocą zwinięcie uruchamiania zadań łaciński świnka na klastrze Azure HDInsight. Język programowania łaciński świnka umożliwia opisują przekształcenia, które są stosowane do wprowadzania danych do generowania odpowiedniej danych wyjściowych.

Zwinięcie służy do pokazano, jak możesz korzystać ze HDInsight przy użyciu pierwotne żądania HTTP, aby uruchomić, monitorować i pobrać wyniki świnka zadania. Działa to za pomocą WebHCat interfejsu API usługi REST (wcześniej znany jako Templeton) dostarczonego przez klaster HDInsight.

> [AZURE.NOTE] Jeśli znają korzystanie z systemem Linux Hadoop serwerów, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz [Porady HDInsight systemem Linux](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Klaster Azure HDInsight (Hadoop na HDInsight) (systemem Linux lub opartych na systemie Windows)

* [Zwinięcie](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Uruchamianie świnka zadań przy użyciu zwinięcie

> [AZURE.NOTE] Podczas korzystania z WebHCat zwinięcie i wszelkie inne metody komunikacji pozostałych, musi uwierzytelnić żądania przez podanie nazwy użytkownika administratora i hasła dla klastrów HDInsight. Należy również użyć nazwy klaster w ramach programu identyfikator URI (Uniform Resource) używany do wysyłania żądania do serwera.
>
> Polecenia w tej sekcji Zamień **nazwę użytkownika** z użytkownikiem w celu uwierzytelnienia z klastrem i zastąpić **hasło** hasło do konta użytkownika. Zamień **NAZWAKLASTRA** nazwę klaster.
>
> Interfejsu API usługi REST jest zabezpieczone za pomocą [uwierzytelniania podstawowego dostępu](http://en.wikipedia.org/wiki/Basic_access_authentication). Zawsze należy żądania za pomocą protokołu HTTP bezpiecznego (HTTPS) upewnij się, że poświadczenia są bezpieczne wysyłane na serwerze.

1. W wierszu polecenia wpisz następujące polecenie aby sprawdzić, czy możesz połączyć się ze klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Powinien zostać wyświetlony odpowiedzi podobny do następującego:

        {"status":"ok","version":"v1"}

    Parametry używane w tym poleceniu są następujące:

    * **-u**: nazwa użytkownika i hasło używane do uwierzytelniania żądania
    * **-G**: wskazuje, że jest żądania GET

    Na początek adres URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**jest taki sam dla wszystkich żądań. Ścieżka **/status/status**wskazuje, że żądania jest powoduje przywrócenie stanu WebHCat (nazywane także Templeton) na serwerze.

2. Aby przesłać zadanie łaciński świnka z klastrem, należy użyć następującego kodu:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Parametry używane w tym poleceniu są następujące:

    * **-d**: ponieważ `-G` nie jest używany, żądanie zostanie przyjęta wartość domyślna metody POST. `-d`Umożliwia określenie wartości danych, które są wysyłane z żądaniem.

        * **User.name**: użytkownika, który jest uruchomienie polecenia
        * **Wykonywanie**: łaciński świnka instrukcji do wykonania
        * **statusdir**: stan dla danego zadania, które zostaną zapisane w katalogu

    > [AZURE.NOTE] Należy zauważyć, że zastępuje spacje w instrukcjach łaciński świnka `+` znak użycia z zwinięcie.

    To polecenie powinna zwrócić identyfikator zadania, który może być używany, aby sprawdzić stan zadania, na przykład:

        {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia. Zamień **JOBID** wartość zwracana w poprzednim kroku. Na przykład zwrócona wartość `{"id":"job_1415651640909_0026"}`, a następnie będzie **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jeśli zadanie zostało ukończone, stan będzie **powiodło się**.

    > [AZURE.NOTE] To żądanie zwinięcie zwraca dokumentu notacji obiektu JavaScript (JSON) informacje o zadaniu, a jq jest używana do pobierania wartości stan.

##<a id="results"></a>Wyświetlanie wyników

Gdy stan zadania zmieniła się **powiodło się**, można pobrać wyniki zadania z magazynem obiektów Blob platformy Azure. `statusdir` Parametr przekazano z zapytaniem zawiera lokalizacji w pliku docelowym; w tym przypadku **wasbs: / / / przykład pigcurl**. Ten adres są przechowywane dane wyjściowe zadania w katalogu **przykładzie pigcurl** w kontenerze miejsca do magazynowania domyślne używane przez klaster HDInsight.

Można wyświetlić listę i Pobierz te pliki przy użyciu [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md). Aby na przykład listę plików w **przykładzie pigcurl**Użyj następującego polecenia:

    azure storage blob list <container-name> example/pigcurl

Aby pobrać plik, użyj następujących opcji:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Podaj nazwę konta magazynu zawierającego to przy użyciu `-a` i `-k` parametry lub zestawu **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_dostępu\_klucza** zmiennych środowiska.

##<a id="summary"></a>Podsumowanie

Jak pokazano w tym dokumencie, umożliwia nieprzetworzonych żądania HTTP uruchomić, monitorować i wyświetlić wyniki świnka zadań w klastrze HDInsight.

Aby uzyskać więcej informacji na temat interfejsu REST używanych w tym artykule zobacz [WebHCat odwołania](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat świnka na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)
