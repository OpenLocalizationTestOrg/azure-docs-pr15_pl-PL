<properties
   pageTitle="Integrowanie magazynu Lake danych z innych usług Azure | Microsoft Azure"
   description="Opis sposobu magazynu Lake danych można zintegrować z innych usług Azure"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrowanie magazynu Lake danych z innych usług Azure

Azure magazynu Lake danych może służyć w połączeniu z innymi usługami Azure umożliwiające szerszej grupie scenariuszy. Następujący artykuł zawiera listę usług, które można zintegrować z magazynu Lake danych.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Użyj magazynu Lake danych z usługi HDInsight platformy Azure

Umożliwia obsługę klaster [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) , która używa magazynu Lake danych zgodnych z HDFS przechowywania danych. W tej wersji dla klastrów Hadoop i Burza w systemach Windows i Linux, umożliwia magazynu Lake danych tylko jako dodatkowego miejsca do magazynowania. Takie klastrów nadal używać Azure przestrzeni dyskowej (WASB) jako domyślnego magazynu. Jednak w przypadku klastrów HBase w systemach Windows i Linux oraz magazynu Lake danych może zawierać jako magazynu domyślnego i/lub dodatkowego miejsca do magazynowania.

Aby uzyskać instrukcje dotyczące obsługi administracyjnej klastrze HDInsight z magazynu Lake danych zobacz:

* [Ustanawianie klaster HDInsight magazynu Lake danych przy użyciu Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Ustanawianie klaster HDInsight magazynu Lake danych przy użyciu programu PowerShell Azure](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Wolisz wideo?** Skorzystaj z łączy poniżej, aby obejrzeć klipy wideo z instrukcjami dotyczącymi sposobu używania magazynu Lake danych z usługi HDInsight klastrów.

* [Tworzenie klastrze HDInsight z dostępem do magazynu Lake danych](https://mix.office.com/watch/l93xri2yhtp2)
* Gdy klaster jest skonfigurowana, [danych programu Access w sklepie Lake danych za pomocą skryptów gałąź i świnka](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Używanie danych Lake Sklepu przy użyciu analizy Lake Azure danych

[Analizy Lake danych Azure](../data-lake-analytics/data-lake-analytics-overview.md) umożliwia pracę z danymi duży w chmurze skali. Dynamiczne przepisy zasoby i pozwala wykonywać analizy na terabajtów lub nawet eksabajtów dane, które mogą być przechowywane w liczbie obsługiwanych źródeł danych, jednego z nich magazynu Lake danych. Dane Lake analizy specjalnie jest zoptymalizowany do pracy z magazynu Lake Azure danych — dostarczając najwyższego poziomu wydajności i przepustowość i parallelization dla Ciebie obciążenia duży danych.

Aby uzyskać instrukcje dotyczące sposobu używania analizy Lake danych z magazynu Lake danych zobacz [Wprowadzenie do analiz Lake danych za pomocą magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Wolisz wideo?** Skorzystaj z łączy poniżej, aby obejrzeć klipy wideo z instrukcjami dotyczącymi sposobu używania magazynu Lake danych z usługi HDInsight klastrów.

* [Łączenie danych Azure Lake analizy z magazynu Lake danych Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Dostęp Azure danych Lake magazynu za pomocą analizy Lake danych](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Używanie danych Lake Sklepu przy użyciu danych Azure Factory

[Factory danych Azure](https://azure.microsoft.com/services/data-factory/) umożliwia mogły zjeść tej ostatniej danych z tabel Azure, bazy danych SQL Azure, Azure SQL DataWarehouse, blob miejsca do magazynowania Azure i lokalnych baz danych. Jest obywatelem pierwszej klasie w ekosystemie Azure, Azure Factory danych można dodać akompaniament spożyciu danych z tych źródeł do magazynu Lake danych Azure.

Aby uzyskać instrukcje dotyczące sposobu używania Azure Factory danych z magazynu Lake danych zobacz [Przenoszenie danych do i z magazynu Lake danych przy użyciu danych Factory](../data-factory/data-factory-azure-datalake-connector.md).

**Klipy wideo ponownie!** Zobacz [Aranżacji danych przy użyciu Azure fabryki danych dla magazynu Lake danych Azure](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych

Magazyn Lake danych Azure udostępnia narzędzia wiersza polecenia AdlCopy, umożliwiający skopiuj dane z magazynem obiektów Blob platformy Azure pod uwagę magazynu Lake danych. Aby uzyskać więcej informacji zobacz [Kopiowanie danych z obiektów blob miejsca do magazynowania Azure magazynu Lake danych](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopiowanie danych między bazy danych SQL Azure i magazynu Lake danych

Apache Sqoop umożliwia importowanie i eksportowanie danych między bazy danych SQL Azure i magazynu Lake danych. Aby uzyskać więcej informacji zobacz [Kopiowanie danych między magazynu Lake danych i baza danych Azure SQL za pomocą Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**Obejrzyj ten klip wideo** na [Przy użyciu Sqoop Apache przenoszenie danych między relacyjnych źródeł i magazynu Lake danych Azure](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Używanie danych Lake Sklepu przy użyciu analizy strumieniu

Magazynu Lake danych w jednym z wyjściowe służy do przechowywania danych strumieniowo przy użyciu analizy strumieniu Azure. Aby uzyskać więcej informacji zobacz [transmisji danych z obiektów Blob miejsca do magazynowania Azure do magazynu Lake danych za pomocą analizy strumieniu Azure](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Użyj magazynu Lake danych dzięki usłudze Power BI

Power BI umożliwia importowanie danych z konta magazynu Lake danych analizowanie i wizualizowanie danych. Aby uzyskać więcej informacji zobacz [Analiza danych w magazynie Lake danych przy użyciu usługi Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Użyj magazynu Lake danych dzięki wykazowi danych

Dane z magazynu Lake danych można zarejestrować do wykazu danych Azure w celu ułatwienia odnajdowania w całej organizacji danych. Aby uzyskać więcej informacji zobacz [Rejestrowanie danych z magazynu danych Lake w wykazie danych Azure](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Zobacz też

- [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)
- [Rozpoczynanie pracy z magazynu Lake danych za pomocą portalu](data-lake-store-get-started-portal.md)
- [Rozpoczynanie pracy z magazynu Lake danych przy użyciu programu PowerShell](data-lake-store-get-started-powershell.md)  
