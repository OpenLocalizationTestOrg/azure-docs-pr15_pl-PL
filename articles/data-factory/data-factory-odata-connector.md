<properties 
    pageTitle="Przenoszenie danych ze źródeł OData | Factory Azure danych" 
    description="Informacje na temat przenieść dane ze źródeł OData przy użyciu Azure danych Factory." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Przenoszenie danych z OData źródła przy użyciu Factory danych Azure
W tym artykule opisano, jak można to działanie kopii w factory Azure danych przenoszenie danych ze źródła danych OData do innego magazynu danych. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

> [AZURE.NOTE] Ten łącznik OData Obsługa kopiowanie danych z obu chmury OData i lokalnych źródeł OData. Dla niego należy zainstalować bramę zarządzania danymi. Zobacz artykuł [Przenoszenie danych między wdrożeniem lokalnym a chmurą](data-factory-move-data-between-onprem-and-cloud.md) szczegółowe informacje na temat bramy zarządzania danymi.

## <a name="copy-data-wizard"></a>Kopiowanie Kreator danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane ze źródła danych OData jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane ze źródła danych OData magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Przykład: Kopiowanie danych ze źródła OData do obiektów Blob platformy Azure

W tym przykładzie pokazano, jak skopiować dane ze źródła danych OData magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OData](#odata-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [ODataResource](#odata-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](#odata-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z przetwarzanie kwerend dotyczących źródłem OData do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

**Usługi połączone OData** W tym przykładzie użyto uwierzytelniania podstawowego. Zobacz sekcję [OData połączone usługi](#odata-linked-service-properties) dla różnych typów uwierzytelniania, możesz użyć. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**Zestaw wprowadzania danych OData**

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

Określanie **ścieżki** w zestawie danych definicji jest opcjonalna. 


**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **RelationalSource** i typ **sink** jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** zaznacza najnowszych danych (od najnowszych) źródło OData.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


Określenie **zapytania** w definicji planowana jest opcjonalne. Jest używany adres **URL** , używaną przez usługę Factory danych do pobierania danych: adres URL określony w usłudze połączone (wymagany) + ścieżka zestawu danych (opcjonalnie) + kwerendy w potoku (opcjonalnie). 

## <a name="odata-linked-service-properties"></a>Właściwości usługi połączone OData

Poniższa tabela zawiera opis specyficzne dla usługi OData połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **OData** | Tak |
| adres URL| Adres URL usługi OData. | Tak |
| authenticationType | Typ uwierzytelniania używany do łączenia się ze źródłem OData. <br/><br/> Chmury OData możliwe wartości są anonimowe i Basic. W przypadku OData lokalnego możliwe wartości są anonimowe, podstawowe i systemu Windows. | Tak | 
| Nazwa użytkownika | Określ nazwę użytkownika, jeśli korzystasz z uwierzytelniania podstawowego. | Tak (tylko wtedy, gdy używasz uwierzytelnianie podstawowe) | 
| hasło | Określ hasło do konta użytkownika, który został określony jako nazwa użytkownika. | Tak (tylko wtedy, gdy używasz uwierzytelnianie podstawowe) | 
| gatewayName | Nazwa bramy, która usługa Factory danych należy używać do łączenia się z usługą OData lokalnego. Określ tylko, jeśli kopiujesz dane ze źródła OData lokalna. | Brak |

### <a name="using-basic-authentication"></a>Przy użyciu uwierzytelniania podstawowego

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Uwierzytelnianie anonimowe
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Przy użyciu funkcji uwierzytelniania systemu Windows, uzyskiwanie dostępu do lokalnego źródła OData

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>Właściwości typu zestawu danych OData

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **ODataResource** (obejmująca zestawu danych OData) zawiera następujące właściwości

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| Ścieżka | Ścieżkę do zasobu OData | Brak | 

## <a name="odata-copy-activity-type-properties"></a>Właściwości Typ OData kopii aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło jest typu **RelationalSource** (obejmująca OData) w sekcji typeProperties są dostępne następujące właściwości:

| Właściwość | Opis | Przykład | Wymagane |
| -------- | ----------- | -------------- | -------- |
| kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | "? $select = nazwa, opis i $top = 5" | Brak | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Typ mapowania OData

Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) kopii aktywności wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie na typ natywnych sink typ .NET

Gdy zapisuje przenoszenie danych z danych OData, typy danych OData są mapowane na typy .NET.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

