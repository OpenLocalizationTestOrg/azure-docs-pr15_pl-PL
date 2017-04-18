<properties
   pageTitle="Ładowanie danych za pomocą Factory danych Azure | Microsoft Azure"
   description="Dowiedz się, jak załadować dane z Factory danych Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Ładowanie danych za pomocą fabrycznych Azure danych 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Factory danych](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Ten samouczek pokazano, jak utworzyć potok w Azure fabryki danych, aby przenieść dane z obiektów Blob miejsca do magazynowania Azure do magazynu danych SQL Azure. Poniższe czynności spowoduje:

+ Konfigurowanie przykładowych danych w obiektów Blob platformy Azure miejsca do magazynowania.
+ Łączenie zasobów do fabryki danych Azure.
+ Tworzenie procesu, aby przenieść dane z magazyn obiektów blob magazynu danych SQL.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby zapoznać się z Factory danych Azure, zobacz [Wprowadzenie do Azure danych Factory][].

### <a name="create-or-identify-resources"></a>Tworzenie lub zidentyfikuj zasoby

Przed rozpoczęciem tego samouczka, musisz mieć następujące zasoby:

   + **Azure miejsca do magazynowania Blob**: samouczku obiektów Blob miejsca do magazynowania Azure jako źródła danych procesu Azure Factory danych, a więc musisz dysponować jedną dostępnych do przechowywania danych przykładowych. Jeśli nie masz już, Dowiedz się, jak [Tworzenie konta miejsca do magazynowania][].

   + **Program SQL Data Warehouse**: ten samouczek przenosi dane z obiektów Blob miejsca do magazynowania Azure SQL Data Warehouse i dlatego należy udzielić danych magazynu online zostanie załadowana z przykładowymi danymi AdventureWorksDW. Jeśli nie masz już magazynu danych, Dowiedz się, jak inicjować [obsługę jedną][Create a SQL Data Warehouse]. Jeśli masz magazynu danych, ale nie obsługi administracyjnej z przykładowymi danymi, możesz ją [Załaduj go ręcznie][Load sample data into SQL Data Warehouse].

   + **Azure danych Factory**: Factory danych Azure kończy rzeczywiste obciążenie, a więc musisz mieć taki, który umożliwia tworzenie procesu przepływu danych. Jeśli nie masz już, Dowiedz się, jak utworzyć w kroku 1 [wprowadzenie Azure danych fabrycznych (Edytor fabrycznych danych)][].

   + **AZCopy**: należy AZCopy, aby skopiować przykładowych danych z lokalnego klienta do obiektów Blob usługi Azure miejsca do magazynowania. Aby uzyskać instrukcje dotyczące instalacji zapoznaj się z [dokumentacją AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Krok 1: Skopiuj dane przykładowe do obiektów Blob miejsca do magazynowania Azure

Po umieszczeniu fragmenty gotowa, możesz przystąpić do skopiuj dane przykładowe do obiektów Blob usługi Azure miejsca do magazynowania.

1. [Pobieranie przykładowych danych][]. Te dane dodaje innego trzech latach danych sprzedaży do AdventureWorksDW danych przykładowych.

2. Użyj tego polecenia AZCopy, aby skopiować trzech latach danych do obiektów Blob usługi Azure miejsca do magazynowania.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Krok 2: Łączenie zasobów do fabryki danych Azure

Teraz, gdy dane znajdują się w miejscu możemy utworzyć potok Factory danych Azure przenieść dane z magazynem obiektów blob Azure do magazynu danych SQL.

Aby rozpocząć, otwórz [Azure portal][] i wybierz firmie danych z menu po lewej stronie.

### <a name="step-21-create-linked-service"></a>Krok 2.1: Tworzenie usługi połączone

Połączyć swoje konto Azure miejsca do magazynowania i magazynu danych SQL firmie danych.  

1. Najpierw należy rozpocząć proces rejestracji, klikając w sekcji "Usługi połączone" firmie danych, a następnie kliknij przycisk "nowe dane zawierają". Wybierz nazwę, aby zarejestrować magazynie azure w obszarze wybierz magazyn Azure jako typ, a następnie wpisz nazwę konta i klucz konta.

2. Aby zarejestrować magazynu danych SQL przejdź do sekcji "Autor i rozmieszczanie", wybierz pozycję "Magazynu danych nowy" i "Skład danych SQL Azure". Skopiuj i Wklej w tym szablonie, a następnie wypełnij określonych informacji.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Krok 2.2: Definiowanie zestawu danych

Po utworzeniu połączonej usług, mamy Definiowanie zestawów danych.  W tym miejscu oznacza to, Definiowanie struktury danych, który jest przenoszony z Twoim magazynie do magazynu danych.  Więcej informacji o tworzeniu

1. Uruchom ten proces, przejdź do sekcji "Autor i rozmieszczanie" firmie danych.

2. Kliknij przycisk "Nowy zestaw danych", a następnie "Magazyn obiektów Blob platformy Azure" utworzyć łącze z magazynu firmie danych.  Możesz użyć poniżej skrypt, aby zdefiniować dane w magazynie obiektów Blob platformy Azure:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Teraz możemy Definiowanie naszych zestawu danych dla magazynu danych SQL. Firma Microsoft Uruchom w ten sam sposób, klikając "Nowy zestaw danych", a następnie "Skład danych SQL Azure".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Krok 3: Tworzenie i uruchamianie planowaną sprzedażą

Na końcu możemy skonfigurować i uruchomić proces w Azure danych Factory.  To jest operację, która wykonuje przepływu danych rzeczywistych.  Cały widok operacji, które można wykonać przy użyciu magazynu danych SQL i Azure danych Factory można znaleźć [tutaj][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

W sekcji "Autor i rozmieszczanie" kliknij przycisk więcej poleceń, a następnie "Nowego procesu".  Po utworzeniu proces, można użyć poniżej kod, aby przenieść dane do magazynu danych:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej, zacznij od wyświetlania:

- [Ścieżka nauki azure danych fabrycznych][].
- [Łącznik magazynu danych azure SQL][]. To jest temat podstawowe informacje dotyczące korzystania z Azure Factory danych z magazynu danych SQL Azure.


Następujące tematy zawierają szczegółowe informacje dotyczące Azure danych Factory. Rozmawiają bazy danych SQL Azure lub usługi HDInsight, ale informacje dotyczą również magazynu danych SQL Azure.

- [Samouczek: wprowadzenie do programu Factory danych Azure][] To jest samouczek core przetwarzania danych z Azure danych fabrycznych. W tym samouczku zostanie utworzona pierwszego planowaną korzystającego z usługi HDInsight Przekształcanie i analizowania dzienniki sieci web co miesiąc. Należy zauważyć, że nic się nie dzieje kopię w tym samouczku.
- [Samouczek: kopiowanie danych z obiektów Blob miejsca do magazynowania Azure do bazy danych SQL Azure][]. W tym samouczku możesz utworzyć potok w Azure fabrycznych danych do skopiowania Azure Blob miejsca do magazynowania danych do bazy danych SQL Azure.

<!--Image references-->

<!--Article references-->
[Dokumentacja AZCopy]: ../storage/storage-use-azcopy.md
[Łącznik magazynu danych Azure SQL]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Tworzenie konta miejsca do magazynowania]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Wprowadzenie do programu Factory danych Azure (Edytor Factory danych)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Wprowadzenie do fabryki Azure danych]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Samouczek: Kopiowanie danych z obiektów Blob miejsca do magazynowania Azure do bazy danych SQL Azure]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Samouczek: Wprowadzenie do programu Factory danych Azure]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Ścieżka nauki Azure Factory danych]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Pobieranie przykładowych danych]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
