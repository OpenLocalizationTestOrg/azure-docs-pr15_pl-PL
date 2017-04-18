Azure Factory danych obsługuje następujące transformacja czynności, jakie można dodać do procesy pojedynczo lub powiązane z innej działalności.

Przekształcanie danych w trybie offline |  Obliczanie środowiska 
:----------------------- | :--------------------
[Gałąź](../articles/data-factory/data-factory-hive-activity.md) | Usługa HDInsight [Hadoop] 
[Świnka](../articles/data-factory/data-factory-pig-activity.md) | Usługa HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | Usługa HDInsight [Hadoop]  
[Hadoop strumieniowego przesyłania](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | Usługa HDInsight [Hadoop]
[Stanowiska nauki działań: aktualizacja zasobów i wsadowe](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure maszyn wirtualnych 
[Procedura składowana](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, magazynu danych Azure SQL lub SQL Server |
[U Lake analizy danych SQL](../articles/data-factory/data-factory-usql-activity.md) | Analizy Lake Azure danych 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | Usługa HDInsight [Hadoop] lub partię Azure
   
> [AZURE.NOTE] 
> Działanie MapReduce służy do uruchamiania programów Spark w klastrze HDInsight Spark. Szczegółowe informacje znajdują się w [Programy wywołania Spark z fabryki danych Azure](../articles/data-factory/data-factory-spark.md) .
> Możesz utworzyć działania niestandardowego w celu uruchamiania skryptów R w klastrze HDInsight ze zainstalowany R. Zobacz [Factory danych Azure za pomocą Uruchom skrypt R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).