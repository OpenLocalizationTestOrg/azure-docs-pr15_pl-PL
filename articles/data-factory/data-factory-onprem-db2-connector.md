<properties 
    pageTitle="Przenoszenie danych z DB2 | Factory Azure danych" 
    description="Dowiedz się, jak przenieść dane z DB2 bazy danych przy użyciu Factory danych Azure" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Przenoszenie danych z DB2 przy użyciu Factory danych Azure
W tym artykule przedstawiono, jak można to działanie kopii w factory Azure danych przenoszenie danych z DB2 do innego danych przechowywania. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Factory danych obsługuje nawiązywanie połączeń lokalnych źródeł DB2 za pomocą bramy zarządzania danymi. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) , aby uzyskać informacje o brama zarządzania danymi i instrukcje krok po kroku dotyczące konfigurowania bramy. 

> [AZURE.NOTE]
Nawiązywanie połączenia z DB2, nawet jeśli jest ona hostowana w maszyny wirtualne IaaS Azure za pomocą bramy. Jeśli chcesz się połączyć się z wystąpieniem DB2 obsługiwany w chmurze, możesz zainstalować wystąpienie bramy w IaaS maszyn wirtualnych.

Dane factory obecnie obsługuje tylko przenoszenie danych z DB2 do innych sklepów danych, a nie z innych magazynów do DB2. 

## <a name="installation"></a>Instalacji 

Brama zarządzania danymi zapewnia wbudowane sterownika DB2, która obsługuje następujące czynności: 

- SQLAM 9 / 10 / 11
- DB2 dla LUW (Linux, Unix, Windows)
- DB2 z/OS i DB2 dla i (czyli jako / 400)

W związku z tym nie potrzebujesz już ręcznie zainstalować sterowniki podczas kopiowania danych z DB2.

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z bazy danych DB2 jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane z bazy danych DB2 i magazyn obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Przykład: Kopiowanie danych z DB2 do obiektów Blob platformy Azure

W tym przykładzie pokazano, jak w celu skopiowania danych z bazą danych DB2 w lokalnym magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Próbki kopiuje dane z wynik kwerendy w bazie danych DB2 do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

Pierwszym krokiem Instalowanie i konfigurowanie bramy zarządzania danymi. Instrukcje znajdują się w artykule [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) .

**Usługa DB2 połączone:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
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

**Zestaw danych wejściowych DB2:**

Próbki założono utworzysz tabeli "Moja_tabela" DB2 i zawiera kolumnę o nazwie "sygnatury czasowej" czasu serii danych.

Ustawienie "zewnętrzne": PRAWDA informuje usługę Factory danych, że tego zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych. Należy zauważyć, że **Typ** jest ustawiona na **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
            "typeProperties": {},
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
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Planowanej aktywnością kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **RelationalSource** i **sink** typ jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** wybiera dane z tabeli zamówienia.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Właściwości usługi DB2 połączone

Poniższa tabela zawiera opis specyficzne dla usługi DB2 połączone elementy JSON. 

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **OnPremisesDB2** | Tak |
| Serwer | Nazwa serwera DB2. | Tak |
| bazy danych | Nazwa bazy danych DB2. | Tak |
| schematu | Nazwa schematu w bazie danych. Nazwa schematu jest uwzględniana wielkość liter. | Brak |
| authenticationType | Typ uwierzytelniania używany do łączenia się z bazą danych DB2. Możliwe wartości to: anonimowe, podstawowe i systemu Windows. | Tak |
| Nazwa użytkownika | Określ nazwę użytkownika, jeśli jest używane uwierzytelnianie podstawowe i systemu Windows. | Brak |
| hasło | Określ hasło do konta użytkownika, który został określony jako nazwa użytkownika. | Brak |
| gatewayName | Nazwa bramy, która usługa Factory danych należy używać do połączenia z bazą danych DB2 lokalnego. | Tak |

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych DB2 w lokalnym, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>Właściwości typu zestawu danych DB2

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu RelationalTable (obejmująca zestawu danych DB2) zawiera następujące właściwości.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| tableName | Nazwa tabeli w wystąpieniu bazy danych DB2 powiązanych z odwołujące się do. Element tableName jest uwzględniana wielkość liter. | Brak (jeśli jest określony **kwerendy** **RelationalSource** ) |

## <a name="db2-copy-activity-type-properties"></a>Właściwości typ aktywności kopii DB2

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Wykonania kopii gdy źródłem jest typu **RelationalSource** (obejmująca DB2) następujące właściwości są dostępne w sekcji typeProperties:


| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------- | -------------- |
| kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: `"query": "select * from "MySchema"."MyTable""`. | Brak (jeśli jest określony **tableName** **zestawu danych** )|

> [AZURE.NOTE] Nazwy schematu i tabeli jest uwzględniana wielkość liter. Nazwy ujęte w "" (cudzysłów podwójny) w kwerendzie.  

**Przykład:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Mapowanie typu dla DB2
Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) aktywności kopii wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do DB2, następujące mapowania umożliwia z typu DB2 typ .NET.

Typ danych DB2 | Typ .NET framework 
----------------- | ------------------- 
SmallInt | Int16
Liczba całkowita | Int32
BigInt | Int64
Liczba rzeczywista | Pojedynczy
Naciśnij dwukrotnie | Naciśnij dwukrotnie
Przestawne | Naciśnij dwukrotnie
Dziesiętna | Dziesiętna
DecimalFloat | Dziesiętna
Numeryczne | Dziesiętna
Data | Daty i godziny
Czas | Przedziału czasu
Sygnatura czasowa | Daty i godziny
XML | Bajt]
Znak | Ciąg
VarChar | Ciąg
LongVarChar | Ciąg
DB2DynArray | Ciąg
Binarny | Bajt]
Zmiennej liczby dwójkowej | Bajt]
LongVarBinary | Bajt]
Grafika | Ciąg
VarGraphic | Ciąg
LongVarGraphic | Ciąg
CLOB | Ciąg
Obiektów blob | Bajt]
DbClob | Ciąg
SmallInt | Int16
Liczba całkowita | Int32
BigInt | Int64
Liczba rzeczywista | Pojedynczy
Naciśnij dwukrotnie | Naciśnij dwukrotnie
Przestawne | Naciśnij dwukrotnie
Dziesiętna | Dziesiętna
DecimalFloat | Dziesiętna
Numeryczne | Dziesiętna
Data | Daty i godziny
Czas | Przedziału czasu
Sygnatura czasowa | Daty i godziny
XML | Bajt]
Znak | Ciąg


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.


