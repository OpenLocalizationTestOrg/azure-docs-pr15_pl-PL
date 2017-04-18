<properties 
    pageTitle="Przenoszenie danych z programu Teradata | Factory Azure danych" 
    description="Dowiedz się więcej o Teradata łącznik usługi Factory danych, który umożliwia przenoszenie danych z bazy danych programu Teradata" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Przenoszenie danych z przy użyciu Azure Factory danych Teradata

W tym artykule przedstawiono, jak można to działanie kopii w factory Azure danych przenoszenie danych z programu Teradata do innego danych przechowywania. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Factory danych obsługuje nawiązywanie połączeń lokalnych źródeł Teradata za pomocą bramy zarządzania danymi. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) , aby uzyskać informacje o brama zarządzania danymi i instrukcje krok po kroku dotyczące konfigurowania bramy. 

> [AZURE.NOTE]
Brama jest wymagane, nawet jeśli Teradata jest obsługiwany w maszyn wirtualnych IaaS Azure. Możesz zainstalować bramy na tym samym maszyn wirtualnych IaaS do przechowywania danych lub na inny maszyn wirtualnych jak bramy nawiązać połączenie z bazą danych. 

Factory danych obsługuje tylko przenoszenie danych z programu Teradata do innych sklepów danych, a nie z innych zbiorów danych do programu Teradata.

## <a name="installation"></a>Instalacji 

Dla bramy zarządzania danymi do połączenia z bazą danych programu Teradata należy zainstalować [Dostawca danych programu .NET dla programu Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) w tym samym systemie jako brama zarządzania danymi.

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z programu Teradata do dowolnego z magazynów sink obsługiwane jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Kopiowanie danych między Teradata magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Przykład: Kopiowanie danych z programu Teradata do obiektów Blob platformy Azure

W tym przykładzie pokazano, jak skopiować dane z bazy danych programu Teradata magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z wynik kwerendy w bazie danych programu Teradata do obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

Pierwszym krokiem konfiguracji bramy zarządzania danymi. Instrukcje znajdują się w artykule [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) .

**Usługa Teradata połączone:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Magazyn obiektów Blob platformy Azure połączone usługi:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Zestaw wprowadzania danych Teradata:**

Próbki założono utworzysz tabeli "Moja_tabela" Teradata i zawiera kolumnę o nazwie "sygnatura czasowa" czas serii danych.

Ustawienie "zewnętrzne": PRAWDA informuje usługę Factory danych, że tabeli zewnętrznej fabryki danych i nie jest tworzone przez działania w factory danych.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Obiektów Blob platformy Azure wyjściowych zestawu danych:**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Planowanej aktywnością kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **RelationalSource** i **sink** typ jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** zaznacza ostatniej godziny, aby skopiować dane.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Właściwości usługi Teradata połączone

Poniższa tabela zawiera opis specyficzne dla usługi Teradata połączone elementy JSON. 

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Typ | Ustaw właściwości Typ: **OnPremisesTeradata** | Tak
Serwer | Nazwa serwera programu Teradata. | Tak
authenticationType | Typ uwierzytelniania używany do łączenia się z bazą danych programu Teradata. Możliwe wartości to: anonimowe, podstawowe i systemu Windows. | Tak
Nazwa użytkownika | Określ nazwę użytkownika, jeśli jest używane uwierzytelnianie podstawowe i systemu Windows. | Brak 
hasło | Określ hasło do konta użytkownika, który został określony jako nazwa użytkownika. | Brak 
gatewayName | Nazwa bramy, która usługa Factory danych należy używać do połączenia z bazą danych Teradata lokalnego. | Tak

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych programu Teradata w lokalnym, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Właściwości typu zestawu danych Teradata

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. Obecnie nie istnieją żadne właściwości Typ obsługiwane dla zestawu danych Teradata. 


## <a name="teradata-copy-activity-type-properties"></a>Właściwości typ aktywności kopii Teradata

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło jest typu **RelationalSource** (obejmująca Teradata) w sekcji **typeProperties** są dostępne następujące właściwości:

Właściwość | Opis | Dopuszczalne wartości | Wymagane
-------- | ----------- | -------------- | --------
kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: zaznacz * z Moja_tabela. | Tak

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Mapowanie typu dla programu Teradata

Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) aktywności kopii wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do programu Teradata, następujące mapowania umożliwia z typu Teradata typ .NET.

Typ bazy danych programu Teradata | Typ .NET framework
----------------- | ---------------------------
Znak | Ciąg
CLOB | Ciąg
Grafika | Ciąg
VarChar | Ciąg
VarGraphic | Ciąg
Obiektów blob | Bajt]
Bajt | Bajt]
VarByte | Bajt]
BigInt | Int64
ByteInt | Int16
Dziesiętna | Dziesiętna
Naciśnij dwukrotnie | Naciśnij dwukrotnie
Liczba całkowita | Int32
Liczba | Naciśnij dwukrotnie
SmallInt | Int16
Data | Daty i godziny
Czas | Przedziału czasu
Czas przy użyciu strefy czasowej | Ciąg
Sygnatura czasowa | Daty i godziny
Sygnatura czasowa z strefy czasowej | DateTimeOffset
Dzień interwału | Przedziału czasu
Dzień interwału do godziny | Przedziału czasu
Dzień interwału minuty | Przedziału czasu
Dzień interwału do drugiego | Przedziału czasu
Godzina interwału | Przedziału czasu
Interwał godziny, minuty | Przedziału czasu
Interwał godzinę do drugiego | Przedziału czasu
Minuta interwału | Przedziału czasu
Minuta interwału do drugiego | Przedziału czasu
Interwał drugi | Przedziału czasu
Interwał roku | Ciąg
Interwał rok, miesiąc | Ciąg
Miesiąc interwału | Ciąg
Period(Date) | Ciąg
Period(Time) | Ciąg
Okres (czas ze strefą czasową) | Ciąg
Period(TimeStamp) | Ciąg
Okres (sygnatury czasowej ze strefą czasową) | Ciąg
XML | Ciąg

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.