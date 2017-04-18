<properties 
    pageTitle="Przenoszenie danych do/z bazy danych SQL Azure | Microsoft Azure" 
    description="Dowiedz się, jak przenieść dane z bazy danych SQL Azure za pomocą Azure danych Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Przenoszenie danych do i z bazy danych SQL Azure za pomocą Factory danych Azure
W tym artykule opisano, jak można to działanie kopii w factory Azure danych przenoszenie danych z bazy danych SQL Azure z innego magazynu danych. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu. 

## <a name="supported-sources-and-sinks"></a>Obsługiwanych źródeł i ujść
[Obsługiwane magazynów](data-factory-data-movement-activities.md#supported-data-stores-and-formats) znaleźć w tabeli lista magazynów danych jako źródła lub pochłaniacze obsługiwane przez aktywności kopii. Można przenieść dane z dowolnym obsługiwane źródło magazynu danych do bazy danych SQL Azure lub z bazy danych SQL Azure do dowolnego magazynu danych obsługiwanych sink.

## <a name="create-pipeline"></a>Tworzenie procesu
Możesz utworzyć potok aktywnością kopii powoduje przeniesienie danych z bazy danych programu Azure SQL za pomocą różnych narzędzi i interfejsów API.  

- Kreator kopii
- Azure portal
- Programu Visual Studio
- Azure programu PowerShell
- INTERFEJS API PROGRAMU .NET
- INTERFEJSU API USŁUGI REST

Zobacz [Samouczek aktywności kopii](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) instrukcje krok po kroku dotyczące tworzenia potok aktywnością kopii na różne sposoby.   

## <a name="copy-data-wizard"></a>Kopiowanie Kreator danych
Najprostszym sposobem utworzenia potok skopiowanie danych z bazy danych SQL Azure jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane do i z bazy danych SQL Azure i magazyn obiektów Blob platformy Azure są pokazywane. Jednak dane mogą być skopiowane **bezpośrednio** z dowolnego źródła do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Przykład: Kopiowanie danych z bazy danych SQL Azure do obiektów Blob platformy Azure

Taki sam definiuje następujące jednostki Factory danych:

1. Usługi połączone typu [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [SqlSource](#azure-sql-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje szeregu czasowego danych (godzina, dzienny, itp.) z tabeli w bazie danych Azure SQL do obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki.  

**Usługi połączone SQL Azure**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Zobacz sekcję [Połączonych usługa SQL Azure](#azure-sql-linked-service-properties) na liście właściwości obsługiwane przez tę usługę połączone. 

**Usługa magazynu połączone w usłudze Azure obiektów Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Zobacz artykuł [Obiektów Blob platformy Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) na liście właściwości obsługiwane przez tę usługę połączone. 

**Azure wprowadzania zestawu danych SQL**

Próbki założono utworzeniu tabeli "Moja_tabela" w języku SQL Azure i zawiera kolumnę o nazwie "timestampcolumn" dla czasu serii danych. 

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usługa Azure danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

Zobacz sekcję [Właściwości typu zestawu danych Azure SQL](#azure-sql-dataset-type-properties) na liście właściwości obsługiwane przez ten typ zestawu danych.  

**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane do nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Zobacz sekcję [Właściwości typu Azure Blob zestawu danych](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) na liście właściwości obsługiwane przez ten typ zestawu danych.  

**Planowanej aktywnością Kopiuj**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **SqlSource** i typ **sink** jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **SqlReaderQuery** zaznacza ostatniej godziny, aby skopiować dane.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

W tym przykładzie **sqlReaderQuery** jest określona dla SqlSource. Działanie kopii działa to zapytanie w odniesieniu do źródło bazy danych SQL Azure, aby pobrać dane. Można też określić procedura składowana, określając **sqlReaderStoredProcedureName** i **storedProcedureParameters** (Jeśli procedura składowana pobiera parametry).

Jeśli nie określisz sqlReaderQuery lub sqlReaderStoredProcedureName, kolumny w sekcji Struktura zestawu danych JSON są używane do utworzenia uruchomienie kwerendy w bazie danych SQL Azure. Na przykład: `select column1, column2 from mytable`. Jeśli definicja zestawu danych nie ma struktury, z tabeli zostaną zaznaczone wszystkie kolumny. 


Zobacz sekcję [Źródła Sql](#sqlsource) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) na liście właściwości obsługiwane przez SqlSource i BlobSink. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Przykład: Kopiowanie danych z obiektów Blob platformy Azure do bazy danych SQL Azure

Próbki definiuje następujące jednostki Factory danych:  

1.  Usługi połączone typu [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Kopie przykładowych szeregu czasowego danych (godzina, dzienny, itp.) z Azure blob do tabeli w języku SQL Azure bazy danych, co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 


**Usługi połączone SQL Azure**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Zobacz sekcję [Połączonych usługa SQL Azure](#azure-sql-linked-service-properties) na liście właściwości obsługiwane przez tę usługę połączone. 

**Usługa magazynu połączone w usłudze Azure obiektów Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Zobacz artykuł [Obiektów Blob platformy Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) na liście właściwości obsługiwane przez tę usługę połączone.

**Azure zestawu danych wejściowych obiektów Blob**

Dane są pobierane z nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżkę i nazwę folderu dla obiektów blob dynamicznie są obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu została użyta rok, miesiąc i dzień części godzina rozpoczęcia i godzina części czas rozpoczęcia używa nazwy pliku. "zewnętrzne": "true" ustawienie informuje usługę Factory danych, że tej tabeli zewnętrznej fabryki danych i nie jest tworzone przez działania w factory danych.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
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

Zobacz sekcję [Właściwości typu Azure Blob zestawu danych](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) na liście właściwości obsługiwane przez ten typ zestawu danych.

**Dataset dane wyjściowe SQL Azure**

Próbki kopiuje dane do tabeli o nazwie "Moja_tabela" w języku SQL Azure. Tworzenie tabeli w języku SQL Azure z taką samą liczbę kolumn, zgodnie z oczekiwaniami pliku CSV obiektów Blob zawierać. Nowe wiersze zostaną dodane do tabeli co godzinę. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Zobacz sekcję [Właściwości typu zestawu danych Azure SQL](#azure-sql-dataset-type-properties) na liście właściwości obsługiwane przez ten typ zestawu danych.

**Planowanej aktywnością Kopiuj**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **BlobSource** i typ **sink** jest ustawiona na **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Zobacz sekcję [Sql Sink](#sqlsink) i [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) na liście właściwości obsługiwane przez SqlSink i BlobSource. 


## <a name="azure-sql-linked-service-properties"></a>Właściwości usługi połączone SQL Azure
W próbkach trzeba użyć usługi połączone typu **AzureSqlDatabase** do łączenie bazy danych programu Azure SQL factory danych. Poniższa tabela zawiera opis specyficzne dla usługi Azure SQL połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| Typ | Ustaw właściwości Typ: **AzureSqlDatabase** | Tak |
| connectionString | Podaj informacje, aby połączyć się z wystąpieniem bazy danych SQL Azure właściwości connectionString. | Tak |

> [AZURE.NOTE] Konfigurowanie [Zapory bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) serwer bazy danych, aby [umożliwić usługi Azure dostępu do serwera](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Ponadto jeśli kopiujesz danych do bazy danych SQL Azure z zewnętrznego Azure łącznie z lokalnych źródeł danych za pomocą bramy factory danych, należy skonfigurować odpowiedni zakres adresów IP dla komputera, na którym jest wysyłanie danych do bazy danych SQL Azure. 

## <a name="azure-sql-dataset-type-properties"></a>Właściwości typu zestawu danych w usłudze Azure SQL
W próbkach zestaw danych typu **AzureSqlTable** użyto funkcji reprezentować tabeli w bazie danych programu Azure SQL. 

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.). 

W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji **typeProperties** zestawu danych typu **AzureSqlTable** ma następujące właściwości:

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| tableName | Nazwa tabeli w przypadku bazy danych SQL Azure, powiązanych z odwołujące się do. | Tak |

## <a name="azure-sql-copy-activity-type-properties"></a>Właściwości typ aktywności kopii Azure SQL
Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań.

> [AZURE.NOTE] Działanie kopii trwa tylko jedno wejście i tworzy tylko jeden wynik.

Właściwości, które są dostępne w sekcji **typeProperties** działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść. 

Jeśli chcesz przenieść dane z bazy danych programu Azure SQL, należy ustawić typ źródła w działaniu Kopiuj do **SqlSource**. Podobnie jeśli chcesz przenieść dane do bazy danych programu Azure SQL, należy ustawić typ sink w działaniu Kopiuj do **SqlSink**. Ta sekcja zawiera listę właściwości obsługiwane przez SqlSource i SqlSink. 

### <a name="sqlsource"></a>SqlSource

W działaniu Kopiuj gdy źródłem jest typu **SqlSource**następujące właściwości są dostępne w sekcji **typeProperties** :

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Przykład: `select * from MyTable`.  | Brak |
| sqlReaderStoredProcedureName | Nazwa procedury składowanej, który odczytuje dane z tabeli źródłowej. | Nazwa procedury składowanej. | Brak |
| storedProcedureParameters | Parametry procedury składowanej. | Pary nazwa/wartość. Nazwy i obudowy parametrów musi odpowiadać nazwy i obudowy parametrów procedura składowana. | Brak |

Jeśli określono **sqlReaderQuery** SqlSource, działania kopii uruchamia tę kwerendę względem źródła bazy danych SQL Azure uzyskiwanie danych. Można też określić procedura składowana, określając **sqlReaderStoredProcedureName** i **storedProcedureParameters** (Jeśli procedura składowana pobiera parametry). 

Jeśli nie określisz sqlReaderQuery lub sqlReaderStoredProcedureName, kolumny w sekcji Struktura zestawu danych JSON są używane skonstruować kwerendę (`select column1, column2 from mytable`) w celu uruchomienia bazy danych SQL Azure. Jeśli definicja zestawu danych nie ma struktury, z tabeli zostaną zaznaczone wszystkie kolumny. 

> [AZURE.NOTE] Użycie **sqlReaderStoredProcedureName**, należy określić wartość właściwości **tableName** w zestawie danych JSON. Nie istnieją żadne reguły sprawdzania poprawności przeprowadzana w tej tabeli, ale. 

### <a name="sqlsource-example"></a>Przykład SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Definicja procedura składowana:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** obsługuje następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Czas dla operacji wstawiania partii na zakończenie przekroczenie limitu czasu oczekiwania. | przedziału czasu<br/><br/> Przykład: "00: 30:00" (30 minut). | Brak | 
| writeBatchSize | Wstawia dane do tabeli SQL, gdy rozmiar buforu osiągnie writeBatchSize. | Liczba całkowita (liczba wierszy)| Nie (domyślny: 10000)
| sqlWriterCleanupScript | Określić kwerendę dla kopii czynności do wykonania w taki sposób, że dane określone wycinek jest wyczyszczone. Aby uzyskać więcej informacji zobacz [sekcję powtarzalności](#repeatability-during-copy). | Instrukcja kwerendy.  | Brak |
| sliceIdentifierColumnName | Określ nazwę kolumny wykonania kopii można wypełnić generowana automatycznie identyfikator wycinek, który jest używany do oczyszczania danych określonych wycinek, gdy ponownie. Aby uzyskać więcej informacji zobacz [sekcję powtarzalności](#repeatability-during-copy). | Nazwa kolumny kolumny z typem danych binary(32). | Brak |
| sqlWriterStoredProcedureName | Nazwy procedury składowanej danych upserts (aktualizacje i wstawia) do tabeli docelowej. | Nazwa procedury składowanej. | Brak |
| storedProcedureParameters | Parametry procedury składowanej. | Pary nazwa/wartość. Nazwy i obudowy parametrów musi odpowiadać nazwy i obudowy parametrów procedura składowana. | Brak | 
| sqlWriterTableType | Określ nazwę typu tabeli może być używany w procedurze przechowywanej. Kopiowanie działanie udostępnia danych jest przenoszony w temp tabeli z tej tabeli. Procedura składowana kod następnie można łączyć dane kopiowana z istniejących danych. | Wpisz nazwę tabeli. | Brak |

#### <a name="sqlsink-example"></a>Przykład SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Kolumny tożsamości w docelowej bazie danych
Ta sekcja zawiera przykład kopiowanie danych z tabeli źródłowej bez kolumny tożsamości do tabeli docelowej z kolumną tożsamości. 

**Tabela źródłowa:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Tabela docelowa:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Zwróć uwagę, że tabela docelowa zawiera kolumny tożsamości. 

**Definicja JSON zestawu danych źródłowych**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Definicja JSON zestawu danych miejsca docelowego**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Należy zauważyć, że masz inny schemat jako tabeli źródłowej i docelowej (docelowej ma dodatkowej kolumny przy użyciu tożsamości). W tym scenariuszu musisz określić właściwości **struktury** w definicji zestawu danych docelowej nie zawiera kolumny tożsamości. 

Następnie można mapować kolumn z zestawu źródła danych do kolumn w docelowym zestawie danych. Zobacz sekcję [próbki mapowania kolumn](#column-mapping-samples) , na przykład. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Mapowanie typu dla programu SQL Server i bazy danych SQL Azure

Wymienione w aktywności kopii artykuł [działania przepływu danych](data-factory-data-movement-activities.md) wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do i z programu SQL Azure, SQL server, Sybase następujące mapowania są używane z typu SQL na typ .NET i odwrotnie.

Mapowanie jest taki sam jak mapowanie typ danych programu SQL Server dla ADO.NET.

| Typ aparatu bazy danych programu SQL Server | Typ .NET framework |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| format binarny | Bajt] |
| bit | Wartość logiczna |
| znak | Ciąg znaków] |
| Data | Daty i godziny |
| Daty i godziny | Daty i godziny |
| datetime2 | Daty i godziny |
| Datetimeoffset | DateTimeOffset |
| Dziesiętna | Dziesiętna |
| Atrybut FILESTREAM (varbinary(max)) | Bajt] |
| Przestawne | Naciśnij dwukrotnie |
| Obraz | Bajt] | 
| int | Int32 | 
| pieniądze | Dziesiętna |
| nchar | Ciąg znaków] |
| ntext | Ciąg znaków] |
| liczbowe | Dziesiętna |
| nvarchar | Ciąg znaków] |
| liczba rzeczywista | Pojedynczy |
| ROWVERSION | Bajt] |
| smalldatetime | Daty i godziny |
| smallint | Int16 |
| smallmoney | Dziesiętna | 
| sql_variant | Obiekt * |
| tekst | Ciąg znaków] |
| czas | Przedziału czasu |
| Sygnatura czasowa | Bajt] |
| tinyint | Bajt |
| uniqueidentifier | Identyfikator GUID |
| zmiennej liczby dwójkowej |  Bajt] |
| varchar | Ciąg znaków] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.