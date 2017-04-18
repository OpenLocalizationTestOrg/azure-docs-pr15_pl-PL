<properties
   pageTitle="Używanie MapReduce i zwinięcie z Hadoop w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak zdalnie uruchamiania zadań MapReduce z Hadoop na HDInsight przy użyciu zwinięcie."
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Uruchamianie zadań MapReduce z Hadoop na HDInsight przy użyciu zwinięcie

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

W tym dokumencie dowiesz się, jak za pomocą zwinięcie uruchamiania zadań MapReduce na Hadoop w klastrze HDInsight.

Zwinięcie służy do pokazano, jak możesz korzystać ze HDInsight przy użyciu pierwotne żądania HTTP do uruchamiania zadań MapReduce. Działa to przy użyciu WebHCat interfejsu API usługi REST (wcześniej znany jako Templeton) dostarczony przez klaster HDInsight.

> [AZURE.NOTE] Jeśli znasz już korzystanie z systemem Linux Hadoop serwerów, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz, [co należy wiedzieć o systemem Linux Hadoop na HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Hadoop w klastrze HDInsight (Linux lub opartych na systemie Windows)

* [Zwinięcie](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Uruchamianie MapReduce zadań przy użyciu zwinięcie

> [AZURE.NOTE] Jeśli używasz zwinięcie i wszelkie inne metody komunikacji RESZTA z WebHCat musi uwierzytelniania żądań przez podanie nazwy użytkownika administratora klaster HDInsight i hasła. Należy również użyć nazwy klaster jako część identyfikator URI, który jest używany do wysyłania żądania do serwera.
>
> Polecenia w tej sekcji Zamień **nazwa_użytkownika** użytkownika do uwierzytelniania klaster i **hasło** przy użyciu hasła dla konta użytkownika. Zamień **NAZWAKLASTRA** nazwę klaster.
>
> Interfejsu API usługi REST jest zabezpieczone za pomocą [uwierzytelniania podstawowego dostępu](http://en.wikipedia.org/wiki/Basic_access_authentication). Zawsze należy żądania za pomocą HTTPS, aby upewnić się, że poświadczenia są bezpieczne wysyłane na serwerze.

1. W wierszu polecenia wpisz następujące polecenie aby sprawdzić, czy możesz połączyć się ze klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Powinien zostać wyświetlony odpowiedzi podobny do następującego:

        {"status":"ok","version":"v1"}

    Parametry używane w tym poleceniu są następujące:

    * **-u**: wskazuje nazwę użytkownika i hasło używane do uwierzytelniania żądania
    * **-G**: wskazuje, że jest żądania GET

    Na początek identyfikatora URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**jest taka sama dla wszystkich żądań.

2. Aby przesłać zadanie MapReduce, użyj następującego polecenia:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Na koniec identyfikator URI (/ mapreduce/słoik) zawiera informację WebHCat, że to żądanie rozpocznie się zadanie MapReduce z klasy w pliku słoik. Parametry używane w tym poleceniu są następujące:

    * **-d**: `-G` nie jest używany, aby domyślnie żądania metody POST. `-d`Umożliwia określenie wartości danych, które są wysyłane z żądaniem.

        * **User.name**: użytkownika, który jest uruchomienie polecenia
        * **słoik**: uruchomiono lokalizacji pliku słoik, który zawiera klasa była
        * **klasy**: klasy, która zawiera logikę MapReduce
        * **argument**: argumenty, które zostaną przekazane do zadania MapReduce; w tym przypadku, plik tekstowy wprowadzania i katalogu, w którym są używane wyników

    To polecenie powinna zwrócić identyfikator zadania, który może być używany, aby sprawdzić stan zadania:

        {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia. Zamień **JOBID** wartość zwracana w poprzednim kroku. Na przykład zwrócona wartość `{"id":"job_1415651640909_0026"}`, następnie będzie JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jeśli zadanie jest wykonane, stan będzie można "powiodło się".

    > [AZURE.NOTE] To żądanie zwinięcie zwraca dokumentu JSON informacje o zadaniu; jq jest używana do pobierania wartości stan.

4. Gdy stan zadania zmieniła się **powiodło się**, można pobrać wyniki zadania z magazynem obiektów Blob platformy Azure. `statusdir` Parametr, które są przekazywane z kwerendy zawiera lokalizacji w pliku docelowym; w tym przypadku **wasbs: / / / przykład/zwinięcie**. Ten adres są przechowywane dane wyjściowe zadania w katalogu **przykładzie/zwinięcie** w kontenerze miejsca do magazynowania domyślne używane przez klaster HDInsight.

Można wyświetlić listę i Pobierz te pliki przy użyciu [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md). Aby na przykład listę plików w **przykładzie/zwinięcie**Użyj następującego polecenia:

    azure storage blob list <container-name> example/curl

Aby pobrać plik, użyj następujących opcji:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Podaj nazwę konta magazynu zawierającego to przy użyciu `-a` i `-k` parametry lub zestawu **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_dostępu\_klucza** zmiennych środowiska. Dowiedz się, [jak przekazać danych do HDInsight](hdinsight-upload-data.md) uzyskać więcej informacji.

##<a id="summary"></a>Podsumowanie

Jak pokazano w tym dokumencie, umożliwia nieprzetworzonych żądania HTTP uruchomić, monitorować i wyświetlić wyniki zadań gałęzi w klastrze HDInsight.

Aby uzyskać więcej informacji na temat interfejs pozostałych, który będzie używany w tym artykule zobacz [WebHCat odwołania](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o zadaniach MapReduce w HDInsight:

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)
