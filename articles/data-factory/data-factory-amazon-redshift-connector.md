<properties 
    pageTitle="Przenoszenie danych z Amazon Redshift przy użyciu danych Factory | Microsoft Azure" 
    description="Informacje na temat przenoszenia danych z Redshift Amazon za pomocą Azure danych Factory." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Przenoszenie danych z Redshift Amazon za pomocą Factory danych Azure

W tym artykule przedstawiono, jak można to działanie kopii w factory Azure danych przenoszenie danych z Redshift Amazon do innego danych przechowywania. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i lista źródła sink magazynów.  

Factory danych obecnie obsługuje tylko dane ruchomą, z Amazon Redshift do innych sklepów danych, ale nie przenoszenie danych z innych baz danych do Amazon Redshift.

## <a name="prerequisites"></a>Wymagania wstępne

- Jeśli chcesz przenieść dane do magazynu danych lokalnych, przyznać bramy zarządzania danymi (Użyj adres IP komputera) dostęp do klastrów Amazon Redshift. Aby uzyskać instrukcje, zobacz [Autoryzuj dostęp z klastrem](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Jeśli chcesz przenieść dane do magazynu danych Azure, zobacz [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653) dla obliczenia zakresy adresów IP (w tym zakresy SQL) używane przez centrach danych Microsoft Azure. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z Amazon Redshift jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Go pokazano, jak w celu skopiowania danych z Amazon Redshift magazynem obiektów Blob platformy Azure. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Przykład: Kopiowanie danych z Amazon Redshift do obiektów Blob platformy Azure
W tym przykładzie pokazano, jak w celu skopiowania danych z bazy danych programu Amazon Redshift magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

- Usługi połączone typu [AmazonRedshift](#linked-service-properties).
- Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [RelationalTable](#dataset-type-properties).
- Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](#copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z wynik kwerendy w Amazon Redshift do obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

**Usługi połączone Redshift Amazon**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
            }
        }
    }


**Azure Usługa magazynu połączone**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Zestaw Amazon Redshift wprowadzania danych**

Ustawianie **"zewnętrznych": PRAWDA** usługę Factory danych informuje, że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych. Ustawiona na wartość PRAWDA na wprowadzania zestawu danych, który nie jest tworzone przez działania w potoku.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Planowanej aktywnością Kopiuj**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **RelationalSource** i **sink** typ jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** zaznacza ostatniej godziny, aby skopiować dane.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Właściwości usługi połączone

Poniższa tabela zawiera opis specyficzne dla usługi Redshift Amazon połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **AmazonRedshift**. | Tak |
| Serwer | Adres IP lub host nazwę serwera Amazon Redshift. | Tak |
| Port | Liczba port TCP używany przez serwer Amazon Redshift, aby odsłuchać dla połączeń klienta. | Nie, wartość domyślna: 5439 |
| bazy danych | Nazwa bazy danych Amazon Redshift. | Tak |
| Nazwa użytkownika | Nazwa użytkownika, kto ma dostęp do bazy danych.| Tak |
| hasło | Hasło do konta użytkownika.| Tak |


## <a name="dataset-type-properties"></a>Właściwości typu zestawu danych

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasady są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **RelationalTable** (obejmująca dataset Amazon Redshift) zawiera następujące właściwości

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| tableName | Nazwa tabeli w bazie danych Amazon Redshift powiązanych z odwołujące się do. | Brak (jeśli jest określony **kwerendy** **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Kopiowanie właściwości typ aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji **typeProperties** działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródła działania Kopiuj jest typu **RelationalSource** (obejmująca Amazon Redshift) w sekcji typeProperties są dostępne następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: zaznacz * z Moja_tabela. | Brak (jeśli jest określony **tableName** **zestawu danych** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Typ mapowania Amazon Redshift

Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) kopii aktywności wykonuje konwersje typów automatyczne z typów źródeł tonięcia typy z następujących rozwinięcie na:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do Amazon Redshift, następujące mapowania posłuży typów Amazon Redshift do typów .NET.

Typ Redshift Amazon | Typ podstawie .NET
-------------------- | ---------------
SMALLINT | Int16
LICZBA CAŁKOWITA | Int32
BIGINT | Int64
DZIESIĘTNA | Dziesiętna
LICZBA RZECZYWISTA | Pojedynczy
PODWÓJNA PRECYZJA | Naciśnij dwukrotnie
WARTOŚĆ LOGICZNA | Ciąg
ZNAK | Ciąg
VARCHAR | Ciąg
DATA | Daty i godziny
SYGNATURA CZASOWA | Daty i godziny
TEKST | Ciąg



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

## <a name="next-steps"></a>Następne kroki
Zobacz następujące artykuły: 
- [Samouczek aktywności kopii](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) instrukcje krok po kroku dotyczące tworzenia potok aktywnością kopii. 