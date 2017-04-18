<properties
   pageTitle="Przekazywanie dużych ilości danych do magazynu Lake danych trybu offline metodami | Microsoft Azure"
   description="Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych za pomocą narzędzia AdlCopy"
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
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Aby uzyskać kopię danych do magazynu Lake danych w trybie offline za pomocą usługi Importuj/Eksportuj Azure

W tym artykule dowiesz się o kopiowaniu dużego zestawów danych (> 200GB) do magazynu Lake Azure danych przy użyciu metod kopii w trybie offline, takich jak [Usługa Azure Importuj/Eksportuj](../storage/storage-import-export-service.md). W szczególności plik używany jako przykład w tym artykule jest 339,420,860,416 bajtów to znaczy około 319GB na dysku. Załóżmy połączeń 319GB.tsv ten plik.

Usługa Azure Importuj/Eksportuj umożliwia bezpieczne przesyłanie dużych ilości danych z magazynem obiektów blob platformy Azure poprzez wysyłanie dysków twardych do centrum danych Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Magazyn azure konta**.

- **Konto azure danych Lake analizy (opcjonalnie)** — zobacz [Wprowadzenie do analizy Lake danych Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Aby uzyskać instrukcje dotyczące tworzenia konta magazynu Lake danych.


## <a name="preparing-the-data"></a>Przygotowywanie danych

Przed rozpoczęciem korzystania z usługi Importuj/Eksportuj, możemy podziału zostać przeniesione **do kopii, które są mniejsze niż 200 GB** rozmiar pliku danych. Jest to ponieważ narzędzie importowania nie działa z plikami większa niż 200GB. Do wykonania, w tym samouczku możemy można podzielić plik fragmentów 100GB bajtów. Można to zrobić łatwo przy użyciu [programów Cygwin](https://cygwin.com/install.html). Programów Cygwin obsługuje polecenia Linux, które pozwala na łatwe wykonaj następujące czynności. W przypadku naszych firma Microsoft korzysta z następującego polecenia.

    split -b 100m 319GB.tsv

Operację podziału tworzy pliki z nazwami poniżej.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Przygotowywanie dysków z danymi

Postępuj zgodnie z instrukcjami w [Za pomocą usług Importuj/Eksportuj Azure](../storage/storage-import-export-service.md) (w sekcji **Przygotowywanie dysków** ), aby przygotować dysku twardego. Oto ogólna przepływu na jak przygotować się na dysku.

1. Zaopatrzenie dysk twardy, który spełnia wymagania może być używany dla Auzre usługi Importuj/Eksportuj.

2. Identyfikowanie konta magazynu platformy Azure miejsce, w którym dane zostaną skopiowane po jego tożsamości usługi wysłane do centrum danych Azure.

3. Użyj [Narzędzia Importuj/Eksportuj Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), narzędzie wiersza polecenia. Oto fragment próbki dotyczące korzystania z narzędzia.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Zobacz [Usługa Importuj/Eksportuj Azure za pomocą](../storage/storage-import-export-service.md) dla więcej wstawki przykładowy o tym, jak korzystać z **Narzędzia Azure Importuj/Eksportuj**.

4. Powyższe polecenie tworzy plik dziennika w określonej lokalizacji. Ten plik dziennika użyje do utworzenia zadania importu w [Klasycznym Portal Azure](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Tworzenie zadania importu

Teraz można utworzyć zadanie importu zgodnie z instrukcjami zawartymi w [Za pomocą usług Importuj/Eksportuj Azure](../storage/storage-import-export-service.md) (w sekcji **Tworzenie zadania importu** ). Dla tego zadania Importuj z innych szczegółów zapewniają również pliku dziennika utworzone podczas przygotowywania stacji dysków.

## <a name="physically-ship-the-disks"></a>Fizycznie wysłać dysków

Teraz można fizycznie wysłać dysków do centrum danych Azure miejsce, w którym dane są kopiowane na do blob miejsca do magazynowania Azure dostępne podczas tworzenia zadania importu. Ponadto podczas tworzenia zadania, jeśli wybrał podać informacje o śledzeniu później, możesz teraz wrócić do zadania importu i zaktualizowane numerem identyfikacyjnym.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych Azure

Po stan zadania importu jest wyświetlany ukończone, można sprawdzić, czy dane są dostępne w blob miejsca do magazynowania Azure zostały określone. Następnie za pomocą różnych metod przenieść dane z obiektami blob miejsca do magazynowania Azure magazynu Lake danych Azure. Wszystkie dostępne opcje pobierania danych zobacz [Ingesting danych do magazynu Lake danych](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

W tej sekcji otrzymasz z definicjami JSON, które umożliwia tworzenie potoku Factory danych Azure kopiowania danych. Możesz użyć tych definicji JSON [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Źródło połączone usługi (Blob Azure miejsca do magazynowania)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Docelowa połączonych usługi (Azure danych Lake magazyn)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Zestaw danych wejściowych
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Dane wyjściowe zestawu danych
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Potok (Kopiuj czynność)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Aby uzyskać więcej informacji na temat Przenoszenie danych z obiektów Blob miejsca do magazynowania Azure i magazynu Lake danych Azure za pomocą Factory danych Azure zobacz [Przenoszenie danych z obiektów Blob miejsca do magazynowania Azure magazynu Lake danych Azure za pomocą Azure danych Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Odtworzyć pliki danych w magazynie Lake danych Azure

Firma Microsoft pracę z plikiem, który był 319GB w rozmiarze i uniemożliwia ją w dół do plików o rozmiarze mniejszym tak, aby mogą być przenoszone, za pomocą usługi Azure Importuj/Eksportuj. Teraz, gdy dane znajdują się w magazynie Lake danych Azure, możemy reconstrcut pliku oryginalnego rozmiaru. Za pomocą następujących cmldts Azure programu PowerShell to zrobić.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
