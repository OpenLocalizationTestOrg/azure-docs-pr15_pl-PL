<properties 
    pageTitle="Przekształcanie: Proces i przekształcania danych | Microsoft Azure" 
    description="Dowiedz się, jak Przekształcanie lub proces danych w Azure Factory danych przy użyciu Hadoop, nauki komputera lub Azure danych Lake analizy." 
    keywords="Przekształcenie danych, dane proces, przekształcania danych, przekształcenie aktywności"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Przekształcanie danych w Factory danych Azure
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Omówienie 
W tym artykule wyjaśniono aktywności przekształcenie danych w Factory danych Azure umożliwia przekształcenie, a następnie przetwarzania nieprzetworzonych danych do przewidywań oraz wnioski. Wykonuje działaniem przekształcenia w środowisku komputerowym, takich jak usługa Azure HDInsight klaster lub partii Azure. Szczegółowe informacje o każdej czynności przekształcenie go znajdują się łącza do artykułów.
 
Factory danych obsługuje następujące transformacja działań, które można dodać do [procesy](data-factory-create-pipelines.md) pojedynczo lub powiązane z innej działalności.

> [AZURE.NOTE] Aby skorzystać z instrukcji krok po kroku, zobacz artykuł [Tworzenie potok transformację gałęzi](data-factory-build-your-first-pipeline.md) .  

## <a name="hdinsight-hive-activity"></a>Gałąź HDInsight aktywności
Działanie gałąź HDInsight w potoku Factory danych wykonuje kwerendy gałęzi samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz artykuł [Gałęzi aktywności](data-factory-hive-activity.md) szczegółowe informacje na temat tego działania. 

## <a name="hdinsight-pig-activity"></a>Świnka HDInsight aktywności
Działania świnka HDInsight w potoku Factory danych wykonuje kwerendy świnka samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz artykuł [Aktywności świnka](data-factory-pig-activity.md) szczegółowe informacje na temat tego działania. 

## <a name="hdinsight-mapreduce-activity"></a>Usługa HDInsight MapReduce aktywności
Działanie HDInsight MapReduce w potoku Factory danych wykonuje programy MapReduce samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz artykuł [Aktywności MapReduce](data-factory-map-reduce.md) szczegółowe informacje na temat tego działania.

Działanie MapReduce służy do uruchamiania programów Spark w klastrze HDInsight Spark. Aby uzyskać szczegółowe informacje, zobacz [Programy wywołania Spark z fabryki danych Azure](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>Usługa HDInsight Streaming aktywności
HDInsight strumienia aktywności w potoku Factory danych wykonuje Hadoop Streaming programy samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz [HDInsight strumienia aktywności](data-factory-hadoop-streaming-activity.md) szczegółowe informacje na temat tego działania.

## <a name="machine-learning-activities"></a>Działania szkoleniowe komputera
Azure Factory danych pozwala na łatwe tworzenie kanał opublikowanych usługi web Azure maszynowego uczenia przewidywanych analiz. Za pomocą [Partii wykonywanie działań](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) w potoku Factory danych Azure, można wywołać usługi sieci web uczenia komputera do przeprowadzania przewidywań danych w partii.

Czasem przewidywanych modeli w nauki Machine wyników doświadczeń konieczne retrained, przy użyciu nowych baz danych wejściowych. Po zakończeniu z przeszkolenie, który chcesz zaktualizować wyników usługi sieci web z modelem retrained nauki komputera. [Aktualizowanie aktywności zasobów](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) umożliwia aktualizowanie usługi sieci web z modelem nowo przeszkolony.  

Szczegółowe informacje na temat działania tych maszynowego uczenia, zobacz [Używanie maszynowego uczenia działań](data-factory-azure-ml-batch-execution-activity.md) . 

## <a name="stored-procedure-activity"></a>Procedura składowana aktywności
Działanie procedura składowana programu SQL Server w potoku Factory danych służy do wywołania procedury przechowywanej w jednym z następujących magazynów: bazy danych SQL Azure, magazynu danych SQL Azure danych programu SQL Server w przedsiębiorstwie lub maszyn wirtualnych Azure. Zobacz artykuł [Przechowywane działania procedury](data-factory-stored-proc-activity.md) Aby uzyskać szczegółowe informacje.  

## <a name="data-lake-analytics-u-sql-activity"></a>Analizy Lake U-SQL danych w trybie offline
Dane Lake analizy U SQL aktywności jest uruchamiany skrypt U SQL w klastrze Azure danych Lake analizy. Zobacz artykuł [Analizy danych w trybie offline U SQL](data-factory-usql-activity.md) , aby uzyskać szczegółowe informacje. 

## <a name="net-custom-activity"></a>Działania niestandardowego .NET
Jeśli potrzebujesz przekształcania danych w taki sposób, aby nie jest obsługiwana przez Factory danych, możesz utworzyć niestandardowe działania z logiki przetwarzania danych i używana w potoku. Można skonfigurować niestandardowe działania .NET przy użyciu usługi Azure partii lub klaster Azure HDInsight. Zobacz artykuł [Używanie niestandardowe działania](data-factory-use-custom-activities.md) Aby uzyskać szczegółowe informacje. 

Możesz utworzyć niestandardowe działania na uruchamianie skryptów R w klastrze HDInsight ze zainstalowany R. Zobacz [Factory danych Azure za pomocą Uruchom skrypt R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Obliczanie środowiskach
Możesz utworzyć połączone usługi dla środowiska obliczeń, a następnie użyj powiązanych z, podczas definiowania działaniem transformacja. Istnieją dwa typy obliczeń środowiskach obsługiwanych przez Factory danych. 

1. **Na żądanie**: W tym przypadku środowisko komputerowe jest w pełni zarządzane przez Factory danych. Jego są tworzone przez usługę Factory danych przed zadanie jest przesłane do procesu danych i usuwane po zakończeniu zadania. Można skonfigurować i kontrolowania ustawień szczegółowego środowiska obliczeń na żądanie do wykonywania zadań, Zarządzanie klastrem i Inicjowanie akcji. 
2. **Przesuń własny**: W tym przypadku można zarejestrować własną środowiska komputerowego (na przykład HDInsight klaster) jako usługi połączone w Factory danych. Środowisko komputerowe odbywa się przez Ciebie i usługa Factory danych umożliwia wykonywanie działań. 

Zobacz artykuł [Obliczyć połączone usługi](data-factory-compute-linked-services.md) Aby uzyskać informacje o obliczyć usług obsługiwanych przez Factory danych. 


## <a name="summary"></a>Podsumowanie
Azure Factory danych obsługuje następujące działania przekształcania danych i środowiskach obliczeń działań. Działania transformacja można dodawać do procesy pojedynczo lub powiązane z innej działalności.

Przekształcanie danych w trybie offline |  Obliczanie środowiska 
:----------------------- | :--------------------
[Gałąź](data-factory-hive-activity.md) | Usługa HDInsight [Hadoop] 
[Świnka](data-factory-pig-activity.md) | Usługa HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | Usługa HDInsight [Hadoop]  
[Hadoop strumieniowego przesyłania](data-factory-hadoop-streaming-activity.md) | Usługa HDInsight [Hadoop]
[Stanowiska nauki działań: aktualizacja zasobów i wsadowe](data-factory-azure-ml-batch-execution-activity.md) | Azure maszyn wirtualnych 
[Procedura składowana](data-factory-stored-proc-activity.md) | Azure SQL, magazynu danych Azure SQL lub SQL Server |
[U Lake analizy danych SQL](data-factory-usql-activity.md) | Analizy Lake Azure danych 
[DotNet](data-factory-use-custom-activities.md) | Usługa HDInsight [Hadoop] lub partii Azure
   

