<properties 
    pageTitle="Hadoop strumienia aktywności" 
    description="Dowiedz się, jak za pomocą aktywności Streaming Hadoop w factory Azure danych do uruchamiania programów Hadoop Streaming na na żądanie/swój własny klaster HDInsight." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop strumienia aktywności
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

Używana HDInsightStreamingActivity wywołania Hadoop Streaming zadania z poziomu procesu Azure danych Factory. Poniższy fragment JSON przedstawiono składnię przy użyciu HDInsightStreamingActivity w pliku planowana JSON. 

HDInsight aktywność strumieniowego przesyłania danych Factory [Planowana](data-factory-create-pipelines.md) wykonuje programy Hadoop Streaming na [własny](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub [na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight systemu Windows i Linux. W tym artykule opiera się na artykuł [działania przekształcania danych](data-factory-data-transformation-activities.md) , w którym przedstawiono ogólne omówienie przekształcenie danych i działania transformacja obsługiwane.

## <a name="json-sample"></a>Przykładowe JSON
Klaster HDInsight jest automatycznie wprowadzana przykład programów (wc.exe i cat.exe) i danych (davinci.txt). Domyślnie nazwa kontenera, który jest używany przez klaster HDInsight to nazwa klastrem. Na przykład jeśli Twoja nazwa klaster to myhdicluster, nazwą kontenera obiektów blob skojarzone byłaby myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Zwróć uwagę następujące punkty:

1. Ustaw **linkedServiceName** nazwa usługi połączone, wskazującego na klaster HDInsight, na którym jest uruchomiona przesyłanie strumieniowe zadania mapreduce.
2. Ustaw typ działania **HDInsightStreaming**.
3. **Mapowanie** właściwości określ nazwę mapowania wykonywalny. W tym przykładzie cat.exe jest wykonywalny mapowania.
4. Dla właściwości **reduktorem** Określ nazwę reduktorem wykonywalny. W tym przykładzie wc.exe jest reduktorem wykonywalny.
5. Dla właściwości typ **input** Określ plik wejściowy (łącznie z lokalizacji) do mapowania. W tym przykładzie: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample jest kontenerem obiektów blob, Gutenberg przykład/danych jest folder i davinci.txt jest to.
6. Dla właściwości typ **danych wyjściowych** Określ plik docelowy (łącznie z lokalizacji) dla reduktorem. Dane wyjściowe Hadoop Streaming zadania są zapisywane w lokalizacji określonej dla tej właściwości.
7. W sekcji **filePaths** Określ ścieżki do mapowania i reduktorem pliki wykonywalne. W tym przykładzie: "adfsample/example/apps/wc.exe" adfsample jest kontenerem obiektów blob, przykład/aplikacji jest folder i wc.exe jest plik wykonywalny.
8. Właściwość **fileLinkedService** Określ Usługa Magazyn Azure połączone, która reprezentuje Azure przestrzeni dyskowej, zawierającego pliki określonych w sekcji filePaths.
9. Właściwość **argumenty** określ argumenty przesyłanie strumieniowe zadania.
10. Właściwość **getDebugInfo** jest opcjonalnego elementu. Gdy jest ustawiony na błąd, dzienniki są pobierane tylko w przypadku awarii. Po ustawieniu do wszystkich dzienników są zawsze pobierane niezależnie od stanu wykonanie.

> [AZURE.NOTE] Jak pokazano w przykładzie, można określić zestaw wynik działania Streaming Hadoop **Wyświetla** właściwości. Tego zestawu danych jest po prostu fikcyjna zestawu danych wymaganego do sterują harmonogramem procesu. Nie musisz określić dowolną wprowadzania zestawu danych dla aktywności właściwości **danych wejściowych** .  

    
## <a name="example"></a>Przykład
Proces, w tym instruktażu uruchamia program mapy/zmniejszanie przesyłanie strumieniowe Statystyka wyrazów w klastrze Azure HDInsight. 

### <a name="linked-services"></a>Usługi połączone

#### <a name="azure-storage-linked-service"></a>Azure Usługa magazynu połączone
Najpierw należy utworzyć połączone usługi, aby połączyć magazyn Azure, jest używana przez klaster Azure HDInsight do fabryki Azure danych. Jeśli możesz skopiować i wkleić poniższy kod, pamiętaj zamienić nazwę konta i klucz konta przy użyciu nazwy i klucza magazynu Azure. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Usługa Azure HDInsight połączone
Następnie możesz utworzyć połączone usługi, aby połączyć klaster Azure HDInsight factory Azure danych. Jeśli możesz skopiować i wkleić poniższy kod, zamień nazwę klaster HDInsight nazwę klaster HDInsight i zmień wartości nazwy i hasła użytkownika. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Zestawy danych

#### <a name="output-dataset"></a>Dane wyjściowe zestawu danych
Proces, w tym przykładzie nie prowadzi nakładów. Można określić zestaw wynik działania Streaming HDInsight. Tego zestawu danych jest po prostu fikcyjna zestawu danych wymaganego do sterują harmonogramem procesu. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Potok

Proces, w tym przykładzie zawiera tylko jedno działanie, które jest wartością typu liczba: **HDInsightStreaming**. 

Klaster HDInsight jest automatycznie wprowadzana przykład programów (wc.exe i cat.exe) i danych (davinci.txt). Domyślnie nazwa kontenera, który jest używany przez klaster HDInsight to nazwa klastrem. Na przykład jeśli Twoja nazwa klaster to myhdicluster, nazwą kontenera obiektów blob skojarzone byłaby myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Zobacz też
- [Gałąź aktywności](data-factory-hive-activity.md)
- [Świnka aktywności](data-factory-pig-activity.md)
- [Działanie MapReduce](data-factory-map-reduce.md)
- [Wywołania Spark programów](data-factory-spark.md)
- [Wywołania skryptów R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

