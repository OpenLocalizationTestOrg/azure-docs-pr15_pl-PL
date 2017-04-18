<properties 
   pageTitle="Scenariusze danych dotyczących magazynu Lake danych | Microsoft Azure" 
   description="Opis różnych scenariuszach i narzędzi, za pomocą dane, które można wchłonięte, przetwarzania pobierane i zwizualizować w magazynie Lake danych" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Przy użyciu magazynu Lake danych Azure wymagań duży danych

Istnieją cztery etapy klucza duży przetwarzania danych:

* Ingesting dużych ilości danych do magazynu danych, w czasie rzeczywistym lub partiami
* Przetwarzanie danych
* Pobieranie danych
* Wizualizowanie danych

W tym artykule przyjrzymy się te etapy w odniesieniu do magazynu Lake danych Azure, aby poznać opcje i narzędzi dostępnych dla potrzeb duży danych.

## <a name="ingest-data-into-data-lake-store"></a>Mogły zjeść tej ostatniej danych do magazynu Lake danych

W tej sekcji przedstawiono różnych źródeł danych i różne sposoby, w którym dane można wchłonięte pod uwagę magazynu Lake danych.

![Ingest danych do magazynu Lake danych] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest danych do magazynu Lake danych")

### <a name="ad-hoc-data"></a>Spontaniczne danych

Jest to mniejszy zestawów danych, które są używane do tworzenia prototypów aplikacji duży danych. Istnieją różne sposoby ingesting danych ad hoc w zależności od źródła danych.

| Źródła danych        | Mogły go przy użyciu zjeść tej ostatniej                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Komputer lokalny     | <ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure programu PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Polecenie między platformami Azure](data-lake-store-get-started-cli.md)</li> <li>[Przy użyciu narzędzia Lake danych dla programu Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Azure magazyn obiektów Blob | <ul> <li>[Factory Azure danych](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[Narzędzie AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp uruchamiania w klastrze HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Strumienia danych

Jest to dane, które mogą być generowane przez różnych źródeł, takich jak aplikacje, urządzenia, czujniki itp. Dane można wchłonięte w magazynie Lake danych za pomocą różnych narzędzi. Te narzędzia będzie zwykle przechwytywanie i przetwarzanie danych na podstawie zdarzenia zdarzeń w czasie rzeczywistym, a następnie wpisz zdarzenia partiami do magazynu Lake danych tak, aby były można kontynuować pracę. 

Poniżej przedstawiono narzędzi, których można używać:
 
* [Analizy strumieniu azure] (.. / strumienia analizy danych — lake wyjścia) - zdarzeń zasysanego do koncentratorów zdarzenie może zostać zapisana na Azure Lake danych przy użyciu magazynu Lake Azure danych wyjściowych.
* [Azure HDInsight Burza](../hdinsight/hdinsight-storm-write-data-lake-store.md) — można zapisywać dane bezpośrednio do magazynu Lake danych z klaster Burza.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) — możesz odbierać zdarzenia z koncentratorów zdarzeń i następnie zapisz je w magazynie Lake danych przy użyciu [Danych Lake sklepu .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>W relacyjnej bazie danych

Można również źródła danych z relacyjnych baz danych. W okresie czasu relacyjnych baz danych zbieranie dużej ilości danych, które zapewniają klucza wniosków przetwarzanie potoku duży danych. Następujące narzędzia umożliwia przenoszenie tych danych do magazynu Lake danych.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Factory Azure danych](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Dane dzienników serwera sieci Web (Przekaż przy użyciu aplikacji niestandardowe)

Ten rodzaj danych jest wymienionym specjalnie, ponieważ analiza danych dziennika serwera sieci web jest typowych przypadków użycia dla aplikacji duży danych i wymaga dużych ilości pliki dziennika do przekazania do magazynu Lake danych. Za pomocą dowolnej z następujących narzędzi programu tworzenia własnych skryptów lub aplikacji do przekazania tych danych.

* [Polecenie między platformami Azure](data-lake-store-get-started-cli.md)
* [Azure programu PowerShell](data-lake-store-get-started-powershell.md)
* [Zestaw SDK programu .NET magazynu Lake danych Azure](data-lake-store-get-started-net-sdk.md)
* [Factory Azure danych](../data-factory/data-factory-data-movement-activities.md)

Do przekazywania danych dziennika serwera sieci web, a także do przekazywania innych typów danych (np. Dane społecznościowe opinie) jest dobrym sposobem zapisać własne niestandardowe skrypty i aplikacje, ponieważ umożliwia elastyczne do uwzględnienia danych przekazywania składnik jako część większych aplikacji duży danych. W niektórych przypadkach kod może formę skryptu lub narzędzia prosty wiersza polecenia. W innych przypadkach kod może służyć do integracji duży przetwarzania danych aplikacji biznesowych lub rozwiązanie.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Dane skojarzone z klastrów Azure HDInsight

Usługa HDInsight klaster większości (Hadoop, HBase, Burza) obsługuje magazynu Lake danych jako repozytorium miejsca do magazynowania danych. Usługa HDInsight klastrów dostęp do danych z Azure magazyn obiektów blob (WASB). Lepszą wydajność możesz skopiować dane z WASB do konta magazynu Lake danych skojarzone z klastrem. Aby skopiować dane, można użyć następujących narzędzi.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [Usługa AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
* [Factory Azure danych](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Dane przechowywane w lokalnego lub IaaS Hadoop klastrów

Duże ilości danych mogą być przechowywane w istniejących klastrów Hadoop lokalnie na komputerach przy użyciu HDFS. Klastrów Hadoop może być w wdrożenia lokalnego lub może znajdować się w klastrze IaaS Azure. Może być wymagania, aby skopiować tych danych do magazynu Lake danych Azure metody jednorazowej lub w sposób cykliczne. Istnieją różne opcje, które są dostępne, aby osiągnąć ten cel. Poniżej przedstawiono listę alternatywy i skojarzone korzystnych rozwiązań.

| Metoda  | Szczegóły | Zalety   | Zagadnienia dotyczące  |
|-----------|---------|--------------|-----------------|
| Aby skopiować dane bezpośrednio z klastrów Hadoop do magazynu Lake danych Azure za pomocą Azure Factory danych (ADF) | [ADF obsługuje HDFS jako źródła danych](../data-factory/data-factory-hdfs-connector.md) | ADF obsługuje w nowym polu HDFS i zarządzania pierwszej klasy zakończenia do końca i monitorowania | Wymaga bramy zarządzania danymi do wdrożenia lokalnego lub w klastrze IaaS |
| Eksportowanie danych z Hadoop jako pliki. Następnie skopiuj pliki do magazynu Lake danych Azure przy użyciu odpowiedniego mechanizmu.                                   | Pliki można kopiować do magazynu Lake danych Azure za pomocą: <ul><li>[Azure programu PowerShell dla systemu Windows z systemem operacyjnym](data-lake-store-get-started-powershell.md)</li><li>[Polecenie między platformami Azure OS nie systemu Windows](data-lake-store-get-started-cli.md)</li><li>Za pomocą dowolnego SDK sklepu Lake danych aplikacji niestandardowej</li></ul> | Szybkie rozpoczęcie pracy. Możliwości przekazywania niestandardowych                                                   | Proces wielu krok, który obejmuje wiele technologii. Zarządzanie i monitorowanie wzrośnie do być wyzwaniem czasie charakter niestandardowych narzędzi |
| Aby skopiować dane z Hadoop do magazynu Azure za pomocą Distcp. Następnie skopiuj dane z magazynu Azure do magazynu Lake danych przy użyciu odpowiedniego mechanizmu. | Możesz skopiować dane z magazynu Azure do korzystania z magazynu Lake danych: <ul><li>[Factory Azure danych](../data-factory/data-factory-data-movement-activities.md)</li><li>[Narzędzie AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp Apache uruchomionych dla klastrów HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Za pomocą narzędzia Otwórz źródło. | Proces wielu krok, który obejmuje wiele technologii |

### <a name="really-large-datasets"></a>Naprawdę dużych zestawów danych

Do przekazywania zestawów danych w kilku terabajtów, przy użyciu metod opisanych powyżej może być czasem powolne i kosztów. W takich przypadkach można użyć poniższych opcji.

* **Za pomocą Azure ExpressRoute**. Azure ExpressRoute pozwala na tworzenie prywatnych połączeń między Azure centrach danych oraz infrastrukturę w swojej siedzibie. Dzięki temu zaufanego opcja przesyłania dużych ilości danych. Aby uzyskać więcej informacji zobacz [dokumentację Azure ExpressRoute](../expressroute/expressroute-introduction.md).


* **"Offline" przekazywanie danych**. Jeśli przy użyciu Azure ExpressRoute nie jest dowolnego powodu możliwe, możesz wysłać dysk twardy o dane centrum danych Azure za pomocą [usługi Azure Importuj/Eksportuj](../storage/storage-import-export-service.md) . Dane najpierw jest przekazane do obiektów blob miejsca do magazynowania Azure. Następnie służy [Factory danych Azure](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) lub [Narzędzia AdlCopy](data-lake-store-copy-data-azure-storage-blob.md) do skopiowania danych z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych.

    >[AZURE.NOTE] Podczas korzystania z usługi Importuj/Eksportuj rozmiarów plików na dyskach, które wysyłasz do centrum danych Azure nie powinna być większa niż 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Przetwarzanie danych przechowywanych w magazynie Lake danych

Gdy dane są dostępne w sklepie Lake danych można uruchamiać analizy danych za pomocą aplikacji obsługiwanych duży danych. Obecnie umożliwia Azure HDInsight i analizy Lake danych Azure uruchamianie zadań analizy danych na karcie dane przechowywane w magazynie Lake danych. 

![Analizowanie danych w magazynie Lake danych] (./media/data-lake-store-data-scenarios/analyze-data.png "Analizowanie danych w magazynie Lake danych")

Zapoznanie się z poniższych przykładach.

* [Tworzenie klastrze HDInsight z magazynu Lake danych jako miejsca do magazynowania](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Pobieranie danych z magazynu Lake danych

Można także pobrać lub przenoszenie danych z magazynu Lake danych Azure w sytuacjach, takich jak:

* Przenoszenie danych do innych repozytoria interfejsem za pomocą usługi istniejące procesy przetwarzania danych. Na przykład można przenieść dane z magazynu Lake danych do bazy danych SQL Azure lub lokalnego programu SQL Server.
* Pobieranie danych z komputerem lokalnym przetwarzania w środowiskach IDE podczas tworzenia prototypów aplikacji.

![Dane wyjściowe z magazynu Lake danych] (./media/data-lake-store-data-scenarios/egress-data.png "Dane wyjściowe z magazynu Lake danych")

W takich przypadkach, użyj jednej z następujących opcji:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Factory Azure danych](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Za pomocą poniższych metod tworzenia własnych skryptów/application do pobierania danych z magazynu Lake danych.

* [Polecenie między platformami Azure](data-lake-store-get-started-cli.md)
* [Azure programu PowerShell](data-lake-store-get-started-powershell.md)
* [Zestaw SDK programu .NET magazynu Lake danych Azure](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Wizualizowanie danych w magazynie Lake danych

Połączenie usługi służy do tworzenia graficznych reprezentacji dane przechowywane w magazynie Lake danych.

![Wizualizacja danych w magazynie Lake danych] (./media/data-lake-store-data-scenarios/visualize-data.png "Wizualizacja danych w magazynie Lake danych")

* Można uruchomić za pomocą [Factory danych Azure przenoszenie danych z magazynu Lake danych do magazynu danych SQL Azure](../data-factory/data-factory-data-movement-activities.md#supported-data-stores)
* Po wykonaniu tej można [zintegrować z magazynu danych SQL Azure usługi Power BI](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) utworzyć wizualną reprezentacją danych.
