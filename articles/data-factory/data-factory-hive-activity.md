<properties 
    pageTitle="Gałąź aktywności" 
    description="Dowiedz się, jak za pomocą aktywności gałęzi w factory Azure danych do uruchomienia kwerendy gałęzi na żądanie/swój własny klaster HDInsight." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Gałąź aktywności
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

Aktywność gałąź HDInsight Factory danych [Planowana](data-factory-create-pipelines.md) wykonuje gałęzi kwerend w klastrze HDInsight systemu Windows i Linux oraz [własne](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub [na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . W tym artykule opiera się na artykuł [działania przekształcania danych](data-factory-data-transformation-activities.md) , w którym przedstawiono ogólne omówienie przekształcenie danych i działania transformacja obsługiwane.

## <a name="syntax"></a>W składni

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Szczegóły dotyczące składni

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Nazwa | Nazwa działania | Tak
Opis | Opis działania do czego służy | Brak
Typ | HDinsightHive | Tak
wartości wejściowych | Wartości wejściowych zużywanej przez aktywności gałęzi | Brak
Wyświetla | Wyjściowe tworzone przez aktywności gałęzi | Tak 
linkedServiceName | Odwołanie do klastrów HDInsight zarejestrowany jako usługi połączone w Factory danych | Tak 
skrypt | Określanie gałęzi w tekście skryptu | Brak
Ścieżka skryptu | Przechowywanie skrypt gałęzi w magazynie obiektów blob platformy Azure i wprowadź ścieżkę do pliku. Właściwość "skrypt" lub "Ścieżka_skryptu". Nie można używać razem. Nazwa pliku jest uwzględniana wielkość liter. | Brak 
Definiuje | Parametry jako pary klucz wartość odwoływanie się do skryptu gałęzi za pomocą "hiveconf"  | Brak

## <a name="example"></a>Przykład

Przypatrzmy się z przykładem gier dzienników analizy, w którym chcesz określić czas spędzony przez użytkowników gry uruchomione przez firmę. 

Dziennik następujące jest przykładowy dziennik nożna, który jest przecinek (`,`) oddzielone i zawiera następujące pola — atrybutem ProfileID, SessionStart, czas trwania, SrcIPAddress i GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Gałąź skrypt** przetwarzania danych:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Do wykonania tego skryptu programu Hive w potoku Factory danych, należy wykonać następujące czynności

1. Tworzenie połączonych usługi, aby zarejestrować [własną HDInsight obliczyć klaster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) lub skonfigurować [klaster obliczyć HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Załóżmy Zadzwoń do tej usługi połączone "HDInsightLinkedService".
2. Tworzenie [połączonych usługi](data-factory-azure-blob-connector.md) w celu skonfigurowania połączenia z magazynem obiektów Blob Azure, w którym znajdują się dane. Załóżmy połączeń tej usługi połączone "StorageLinkedService"
3. Tworzenie [zestawów danych](data-factory-create-datasets.md) , wskaż polecenie dane wejściowe i dane wyjściowe. Załóżmy połączeń danych wejściowych "HiveSampleIn" i dataset wynik "HiveSampleOut"
4. Kopiowanie kwerendy gałęzi jako plik magazynem obiektów Blob platformy Azure skonfigurowany w kroku #2. Jeśli Magazyn przechowującego dane różni się od nich hostingu tego pliku kwerendy, utworzyć osobne usługi Magazyn Azure połączone i odwołują się do niego w działaniu. Za pomocą **scriptPath **Określ ścieżkę do pliku kwerendy gałęzi i **scriptLinkedService** , aby określić Azure magazynowania, który zawiera plik skryptu. 

    > [AZURE.NOTE] Można również dołączyć skrypt gałęzi w tekście w definicji aktywności przy użyciu właściwości **skryptu** . Firma Microsoft nie zaleca tej metody jako wszystkie znaki specjalne w obszarze skrypt w JSON potrzeb dokumentu, które mają być wyjściowym i może powodować problemy z debugowania. Najlepiej jest wykonać krok #4.
5.  Tworzenie potok z działaniem HDInsightHive. Działania procesów i przekształceń poprawiona danych.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Wdrażanie proces. Zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) , aby uzyskać szczegółowe informacje. 
7.  Monitorowanie planowana przy użyciu widoków monitorowania i zarządzania factory danych. Zobacz [monitorowania i zarządzania nimi procesy Factory danych](data-factory-monitor-manage-pipelines.md) artykuł, aby uzyskać szczegółowe informacje. 


## <a name="specifying-parameters-for-a-hive-script"></a>Określanie parametrów skryptu gałęzi  
W tym przykładzie gier dzienniki są codziennie zasysanego do magazyn obiektów Blob platformy Azure i są przechowywane w folderze podzielony na daty i godziny. Chcesz Definiowanie parametrów skryptu gałęzi i przekazać do lokalizacji folderu wprowadzania dynamicznie podczas wykonywania również da wynik podzielony na daty i godziny.

Aby użyć parametryczne skryptu gałęzi, wykonaj następujące czynności

- Definiowanie parametrów w **definiuje**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- W obszarze skrypt gałęzi odwołują się do parametrów przy użyciu **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Zobacz też
- [Świnka aktywności](data-factory-pig-activity.md)
- [Działanie MapReduce](data-factory-map-reduce.md)
- [Hadoop strumienia aktywności](data-factory-hadoop-streaming-activity.md)
- [Wywołaj Spark programów](data-factory-spark.md)
- [Wywoływanie skryptów R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









