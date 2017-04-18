<properties 
    pageTitle="Świnka aktywności" 
    description="Dowiedz się, jak za pomocą aktywności świnka w factory Azure danych na uruchamianie skryptów świnka na żądanie/swój własny klaster HDInsight." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Świnka aktywności
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

Aktywność świnka HDInsight Factory danych [Planowana](data-factory-create-pipelines.md) wykonuje kwerendy świnka na [własny](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub [na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight systemu Windows i Linux. W tym artykule opiera się na artykuł [działania przekształcania danych](data-factory-data-transformation-activities.md) , w którym przedstawiono ogólne omówienie przekształcenie danych i działania transformacja obsługiwane.

## <a name="syntax"></a>W składni

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Szczegóły dotyczące składni

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Nazwa | Nazwa działania | Tak
Opis | Opis działania do czego służy | Brak
Typ | HDinsightPig | Tak
wartości wejściowych | Jeden lub więcej danych wejściowych zużywanej przez aktywności świnka | Brak
Wyświetla | Co najmniej jeden wyjściowe tworzone przez aktywności świnka | Tak
linkedServiceName | Odwołanie do klastrów HDInsight zarejestrowany jako usługi połączone w Factory danych | Tak
skrypt | Określanie świnka w tekście skryptu | Brak
Ścieżka skryptu | Przechowywanie skrypt świnka w magazynie obiektów blob platformy Azure i wprowadź ścieżkę do pliku. Właściwość "skrypt" lub "Ścieżka_skryptu". Nie można używać razem. Nazwa pliku jest uwzględniana wielkość liter. | Brak
Definiuje | Parametry jako pary klucz wartość odwoływanie się do skryptu świnka | Brak

## <a name="example"></a>Przykład

Przypatrzmy się z przykładem gier dzienników analizy, w której chcesz określić czas spędzony przez uczestników gry uruchomione przez firmę.
 
Następujące przykładowego dziennika gier jest plikiem przecinkami (,). Zawiera następujące pola — atrybutem ProfileID, SessionStart, czas trwania, SrcIPAddress i GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Skrypt świnka** przetwarzania danych:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Aby wykonać ten skrypt świnka w potoku Factory danych, wykonaj następujące czynności:

1. Tworzenie połączonych usługi, aby zarejestrować [własną HDInsight obliczyć klaster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub skonfigurować [klaster obliczyć HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Załóżmy Zadzwoń do tej usługi połączone **HDInsightLinkedService**.
2.  Tworzenie [połączonych usługi](data-factory-azure-blob-connector.md) w celu skonfigurowania połączenia z magazynem obiektów Blob Azure, w którym znajdują się dane. Załóżmy Zadzwoń do tej usługi połączone **StorageLinkedService**.
3.  Tworzenie [zestawów danych](data-factory-create-datasets.md) wskazująca dane wejściowe i dane wyjściowe. Załóżmy połączeń danych wejściowych **PigSampleIn** i dataset wynik **PigSampleOut**.
4.  Kopiowanie kwerendy świnka w pliku magazyn obiektów Blob platformy Azure skonfigurowany w kroku #2. Jeśli Azure magazynu, w którym są przechowywane dane różni się od tej, która znajduje się plik kwerendy, należy utworzyć osobne usługi Magazyn Azure połączone. Zapoznaj się z usługi połączone w konfiguracji aktywności. Za pomocą **scriptPath **Określ ścieżkę do pliku skryptu świnka i **scriptLinkedService**. 
    
    > [AZURE.NOTE] Możesz także podać tekście skrypt świnka w definicji aktywności przy użyciu właściwości **skryptu** . Jednak firma Microsoft nie zaleca tej metody jako wszystkie znaki specjalne w potrzeb skryptu, które mają być wyjściowym i może powodować problemy z debugowania. Najlepiej jest wykonać krok #4.
5. Utwórz proces z działaniem HDInsightPig. To działanie przetwarzania danych wejściowych, uruchamiając skrypt świnka w klastrze HDInsight.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Wdrażanie proces. Zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) , aby uzyskać szczegółowe informacje. 
7. Monitorowanie planowana przy użyciu widoków monitorowania i zarządzania factory danych. Zobacz [monitorowania i zarządzania nimi procesy Factory danych](data-factory-monitor-manage-pipelines.md) artykuł, aby uzyskać szczegółowe informacje.

## <a name="specifying-parameters-for-a-pig-script"></a>Określanie parametrów skryptu świnka 

Poniżej podano przykład: dzienniki gier są codziennie zasysanego do magazyn obiektów Blob platformy Azure i przechowywane w folderze podzielone na partycje na podstawie daty i godziny. Chcesz Definiowanie parametrów skryptu świnka i przekazać do lokalizacji folderu wprowadzania dynamicznie podczas wykonywania również da wynik podzielony na daty i godziny.
 
Aby użyć parametryczne skryptu świnka, wykonaj następujące czynności:

- Definiowanie parametrów w **definiuje**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- W obszarze skrypt świnka odwołują się do parametrów przy użyciu "**$parameterName**", jak pokazano w poniższym przykładzie:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Zobacz też
- [Gałąź aktywności](data-factory-hive-activity.md)
- [Działanie MapReduce](data-factory-map-reduce.md)
- [Hadoop strumienia aktywności](data-factory-hadoop-streaming-activity.md)
- [Wywołania Spark programów](data-factory-spark.md)
- [Wywołania skryptów R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


