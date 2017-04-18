<properties
    pageTitle="Przesyłanie zadań Spark zdalnie przy użyciu Livy | Microsoft Azure"
    description="Dowiedz się, jak za pomocą Livy HDInsight klastrów zdalnie przesyłać Spark zadania."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Przesyłanie zadań Spark zdalnie z klastrem Apache Spark na HDInsight Linux przy użyciu Livy

Klaster Apache Spark na Azure HDInsight zawiera Livy, interfejsu REST do przesyłania zadań zdalnie do klastrów Spark. Aby uzyskać szczegółowe dokumentacji zobacz [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Za pomocą Livy uruchomienia interakcyjne Spark powłoki lub przesyłania zadań mają być wykonywane w iskrowym. Ten artykuł zawiera informacje o przesyłanie zadań za pomocą Livy. Składnia poniżej używa zwinięcie na potrzeby połączeń pozostałych do punktu końcowego Livy.

**Wymagania wstępne dotyczące:**

Użytkownik musi mieć następujące czynności:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Przesyłanie zadanie wsadowe klaster

Przed wysłaniem zadanie wsadowe należy przekazać słoik aplikacji w magazynie klaster skojarzone z klastrem. Za pomocą [**AzCopy**](../storage/storage-use-azcopy.md), narzędzie wiersza polecenia, aby to zrobić. Istnieje wiele innych klientów, które umożliwiają przekazywanie danych. Można znaleźć więcej informacji na temat ich na [przekazywanie danych dla zadań Hadoop w HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Przykłady**:

* Jeśli plik słoik znajduje się w magazynie klaster (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Jeśli chcesz przekazać pliku słoik i nazwa_klasy jako część wprowadzania pliku (w tym przykładzie dane_wejściowe.txt zgodnie)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Uzyskiwanie informacji na temat uruchamiania w klastrze partie

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Przykłady**:

* Jeśli chcesz pobrać wszystkie partie uruchamiania w klastrze:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Jeśli chcesz pobrać określonej partii o danej batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Usuń zadanie wsadowe

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Przykład**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livy i wysokiej dostępności

Livy przewiduje wysokiej dostępności Spark zadania uruchamiania w klastrze. Oto kilka przykładów.

* Jeśli usługa Livy awarii po zostały przesłane zadanie zdalnie do klastrów Spark, zadania będzie działać w tle. Gdy Livy jest wykonywanie kopii zapasowej, przywraca stan zadania i raporty ponownie.

* Notesy Jupyter dla HDInsight są obsługiwane przez Livy w wewnętrznej bazy danych. Jeśli notes jest uruchomiony zadanie Spark i otrzymuje ponowne uruchomienie usługi Livy, Notes będzie uruchamianie komórek kodu. 

## <a name="show-me-an-example"></a>Pokaż przykład

W tej sekcji możemy Spójrz na przykłady na jak za pomocą Livy złożyć wniosek Spark, monitorować postęp aplikacji, a następnie usuń zadanie. Aplikacja używanych w tym przykładzie jest jedną opracowanych w artykule [Tworzenie autonomicznego Scala aplikacji i do uruchamiania w klastrze HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md). Poniższe kroki przyjęto założenie, następujące czynności:

* Na słoik aplikacji zostały już skopiowane konto miejsca do magazynowania, związane z klastrem.
* Masz zwinięcie zainstalowany na komputerze, w której próbujesz następujące kroki.

Wykonaj następujące czynności.

1. Pozwól nam najpierw upewnij się, że Livy jest uruchomiona w klastrze. Firma Microsoft zrobić uzyskując listę uruchomionych partie. Jeśli jest to po raz pierwszy uruchamiasz zadanie, używając Livy powinien zwrócić zero.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Należy uzyskać wynik podobny do następującego:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Zwróć uwagę, jak ostatni wiersz w wyniku kwerendy z opisem **sumy: 0**, co sugeruje nie uruchomionego partie.

2. Pozwól nam przesłać zadanie wsadowe. Poniżej wstawek używa plik wejściowy (dane_wejściowe.txt zgodnie) w celu przekazania nazwę słoik i nazwa klasy jako parametrów. Jest to zalecane podejście, jeśli są uruchomione następujące kroki komputera z systemem Windows.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Parametry w pliku **dane_wejściowe.txt zgodnie** są definiowane następująco:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Powinien zostać wyświetlony wynik podobny do następującego:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Zwróć uwagę, jak ostatni wiersz danych wyjściowych z opisem **Stan: uruchamianie**. Również z opisem, **identyfikator: 0**. To jest identyfikator partii.

3. Teraz można pobrać stanu tej partii określonych przy użyciu identyfikatora partii.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Powinien zostać wyświetlony wynik podobny do następującego:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Teraz wyświetlany **Stan: Sukces**, co sugeruje, że zadanie zakończyło się.

4. Jeśli chcesz, możesz teraz usunąć partię.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Powinien zostać wyświetlony wynik podobny do następującego:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Ostatni wiersz wyniku zawiera partii został pomyślnie usunięty. Jeśli usuniesz zadanie, gdy jest uruchomiony, zasadniczo będzie skasować co zadania. Po usunięciu zadania, która została ukończona pomyślnie lub w inny sposób usuwa informacje o zadaniu całkowicie.

## <a name="seealso"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych Instalacja grzewczo-Wentylacyjna danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dziennika witryny sieci Web przy użyciu Spark w HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji

* [Tworzenie autonomiczną aplikację za pomocą Scala](hdinsight-apache-spark-create-standalone-application.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia

* [Tworzenie i przesyłanie Spark Scala aplikacji za pomocą dodatku Narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Zdalne debugowanie aplikacji Spark za pomocą wtyczki narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Notesy Zeppelin za pomocą klaster Spark na HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jądra dostępne dla notesu Jupyter w klastrze Spark dla HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
