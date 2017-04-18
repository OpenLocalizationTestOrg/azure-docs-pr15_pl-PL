<properties 
    pageTitle="Wywoływanie programu MapReduce z fabryki Azure danych" 
    description="Dowiedz się, jak przetwarzanie danych, uruchamiając programy MapReduce w klastrze Azure HDInsight z fabryki Azure danych." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Wywołaj programy MapReduce z fabryki danych
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

Aktywność HDInsight MapReduce Factory danych [Planowana](data-factory-create-pipelines.md) wykonuje programy MapReduce na [własny](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub [na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight systemu Windows i Linux. W tym artykule opiera się na artykuł [działania przekształcania danych](data-factory-data-transformation-activities.md) , w którym przedstawiono ogólne omówienie przekształcenie danych i działania transformacja obsługiwane.

## <a name="introduction"></a>Wprowadzenie 
Potok w factory Azure danych przetwarza danych w usługach magazynowania połączonych za pomocą usług połączonych obliczeń. Zawiera on kolejność miejsce, w którym każdej czynności wykonuje operację przetwarzania określone czynności. Ten artykuł zawiera opis przy użyciu aktywności MapReduce HDInsight.
 
Zobacz [świnka](data-factory-pig-activity.md) i [gałęzi](data-factory-hive-activity.md) szczegółowe informacje na temat uruchamiania skryptów świnka-gałęzi w klastrze HDInsight systemu Windows i Linux oraz z potok przy użyciu świnka HDInsight i gałęzi czynności do wykonania. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON wykonania HDInsight MapReduce 

W definicji JSON aktywności HDInsight: 
 
1. Ustaw **Typ** **działań** związanych z usługą **HDInsight**.
3. Określ nazwę klasy **Nazwa_klasy** właściwości.
4. Określ ścieżkę do pliku SŁOIK, łącznie z nazwą pliku dla właściwości **jarFilePath** .
5. Określ połączonych usługa, która odwołuje się do magazyn obiektów Blob platformy Azure, zawierającego plik SŁOIK **jarLinkedService** właściwości.   
6. Określ argumenty programu MapReduce w sekcji **argumenty** . W czasie wykonywania, zobacz kilka dodatkowe argumenty (na przykład: mapreduce.job.tags) w ramach MapReduce. Aby odróżnić jego argumenty z argumentami MapReduce, warto rozważyć użycie zarówno opcja i wartość jako argumenty, jak pokazano w poniższym przykładzie (- s, — danych wejściowych, — dane wyjściowe itd., są opcje bezpośrednio po według ich wartości).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Działanie MapReduce HDInsight służy do uruchamiania dowolnego pliku słoik MapReduce w klastrze HDInsight. W poniższej definicji JSON przykładowe potok aktywności HDInsight jest skonfigurowany do uruchomienia Mahout JAR pliku.

## <a name="sample-on-github"></a>Przykładowy na GitHub
Możesz pobrać próbki dotyczące korzystania z aktywności MapReduce HDInsight: [Próbki Factory danych na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Uruchomienie programu Statystyka wyrazów
Proces, w tym przykładzie uruchamia program wyrazów mapy/zmniejszanie w klastrze Azure HDInsight.   

### <a name="linked-services"></a>Usługi połączone
Najpierw należy utworzyć połączone usługi, aby połączyć magazyn Azure, jest używana przez klaster Azure HDInsight do fabryki Azure danych. Jeśli możesz skopiować i wkleić poniższy kod, pamiętaj zamienić nazwę i klucz magazynu Azure **klucz konta** i **nazwę konta** . 

#### <a name="azure-storage-linked-service"></a>Azure Usługa magazynu połączone

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
Następnie możesz utworzyć połączone usługi, aby połączyć klaster Azure HDInsight factory Azure danych. Jeśli możesz skopiować i wkleić poniższy kod, Zamień **nazwę klaster HDInsight** nazwę klaster HDInsight i zmień wartości nazwy i hasła użytkownika.   

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
Proces, w tym przykładzie nie prowadzi nakładów. Można określić zestaw wynik działania MapReduce HDInsight. Tego zestawu danych jest po prostu fikcyjna zestawu danych wymaganego do sterują harmonogramem procesu.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Potok
Proces, w tym przykładzie zawiera tylko jedno działanie, które jest wartością typu liczba: HDInsightMapReduce. Ważne właściwości w formacie JSON, należą: 

Właściwość | Notatki
:-------- | :-----
Typ | Typ musi być równa **HDInsightMapReduce**. 
Nazwa_klasy | Nazwa klasy to: **wordcount**
jarFilePath | Ścieżka do pliku słoik zawierającego klasę. Jeśli możesz skopiować i wkleić poniższy kod, nie zapomnij zmienić nazwę grupie. 
jarLinkedService | Azure magazynowania połączona usługa, która znajduje się plik słoik. Ta usługa połączonych odwołuje się do magazynowania, który jest skojarzony z klastrem HDInsight. 
argumenty | Wordcount program przyjmuje dwa argumenty wejściowe i wyjściowe. Plik wejściowy jest plik davinci.txt.
częstotliwość/interwał | Wartości dla tych właściwości są zgodne z zestawu danych dane wyjściowe. 
linkedServiceName | odwołuje się do usługi HDInsight połączone była wcześniej utworzoną.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Uruchamianie programów Spark
Działanie MapReduce służy do uruchamiania programów Spark w klastrze HDInsight Spark. Aby uzyskać szczegółowe informacje, zobacz [Programy wywołania Spark z fabryki danych Azure](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Zobacz też
- [Gałąź aktywności](data-factory-hive-activity.md)
- [Świnka aktywności](data-factory-pig-activity.md)
- [Hadoop strumienia aktywności](data-factory-hadoop-streaming-activity.md)
- [Wywołaj Spark programów](data-factory-spark.md)
- [Wywoływanie skryptów R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
