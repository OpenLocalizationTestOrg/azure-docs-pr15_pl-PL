<properties
    pageTitle="Debugowanie i analizowania usługi Hadoop przy użyciu zrzuty stosu | Microsoft Azure"
    description="Automatyczne zbieranie zrzuty stosu usługi Hadoop i umieść wewnątrz konta magazyn obiektów Blob platformy Azure debugowania i analizy."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Zbieranie stosu dokonuje zrzutu w magazynie obiektów Blob debugowania i analizowania usługi Hadoop

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Zrzuty stosu zawierają migawki pamięci aplikacji, w tym wartości zmiennych w czasie utworzone zrzut. Tak, aby były bardzo przydatne w przypadku diagnozowanie problemów występujących w czasie wykonywania. Zrzuty stosu można automatycznie zbierane usługi Hadoop i umieszczony wewnątrz konta magazyn obiektów Blob platformy Azure użytkownika w obszarze HDInsightHeapDumps /. 

Kolekcja zrzuty stosu dla różnych usług musi być włączona dla usług na poszczególnych klastrów. Domyślne dla tej funkcji jest wyłączone dla klastrów. Te zrzuty stosu mogą być duże, więc zaleca się monitorowanie konta magazyn obiektów Blob gdzie one są zapisywane po włączeniu kolekcji.

> [AZURE.NOTE] Informacje w tym artykule dotyczą tylko HDInsight systemu Windows. Aby uzyskać informacje na podstawie Linux HDInsight zobacz [Włączanie zrzuty stosu usługi Hadoop na podstawie Linux HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Usług dla zrzuty stosu

Możesz włączyć zrzuty stosu dla następujących usług:

*  **hcatalog** - tempelton
*  **gałąź** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **przędza** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfiguracja elementów, które umożliwiają zrzuty stosu

Aby włączyć zrzuty stosu usługi, należy ustawić elementy odpowiednią konfigurację w sekcji w tej usłudze, określonym przez **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Wartość **service_name** może być dowolna z powyższych usług: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, lub namenode.

## <a name="enable-using-azure-powershell"></a>Włączanie przy użyciu programu PowerShell Azure

Na przykład aby włączyć zrzuty stosu przy użyciu programu PowerShell Azure dla jobhistoryserver, należy następujące czynności:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Włączanie przy użyciu zestawu SDK .NET

Na przykład aby włączyć zrzuty stosu, używając Azure HDInsight .NET SDK jobhistoryserver, należy następujące czynności:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
