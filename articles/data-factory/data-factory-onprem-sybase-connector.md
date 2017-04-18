<properties 
    pageTitle="Przenoszenie danych z programu Sybase | Factory Azure danych" 
    description="Informacje na temat przenoszenia danych z bazy danych programu Sybase przy użyciu Azure danych Factory." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Przenoszenie danych z programu Sybase przy użyciu Factory danych Azure 

W tym artykule opisano, jak można to działanie kopii w factory Azure danych do przenoszenia danych z programu Sybase do innego magazynu danych. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Usługa Factory danych obsługuje nawiązywanie połączeń lokalnych źródeł programu Sybase za pomocą bramy zarządzania danymi. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) , aby uzyskać informacje o brama zarządzania danymi i instrukcje krok po kroku dotyczące konfigurowania bramy. 

> [AZURE.NOTE]
> Brama jest wymagane, nawet jeśli baza danych programu Sybase jest hostowana w maszyn wirtualnych IaaS Azure. Możesz zainstalować bramy na tym samym maszyn wirtualnych IaaS do przechowywania danych lub na inny maszyn wirtualnych jak bramy nawiązać połączenie z bazą danych. 

Dane factory obecnie obsługuje tylko przenoszenie danych z programu Sybase do innych sklepów danych, a nie z innych magazynów z serwerem Sybase.

## <a name="installation"></a>Instalacji

Dla bramy zarządzania danymi do połączenia z bazą danych programu Sybase należy zainstalować [dostawcy danych programu Sybase](http://go.microsoft.com/fwlink/?linkid=324846) w tym samym systemie jako brama zarządzania danymi.

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie Kreator danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane z bazy danych programu Sybase do dowolnego z magazynów sink obsługiwane jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane z bazy danych programu Sybase magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Przykład: Kopiowanie danych z programu Sybase do obiektów Blob platformy Azure
W tym przykładzie pokazano, jak skopiować dane z bazy danych programu Sybase magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Usługa liked typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z wynik kwerendy w bazie danych programu Sybase do obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

Pierwszym krokiem konfiguracji bramy zarządzania danymi. Instrukcje znajdują się w artykule [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) .

**Usługa programu Sybase połączone:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Zestaw wprowadzania danych programu Sybase:**

Próbki założono utworzysz tabeli "Moja_tabela" Sybase i zawiera kolumnę o nazwie "sygnatura czasowa" czas serii danych.

Ustawienie "zewnętrzne": PRAWDA informuje usługę Factory danych, że tego zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych. Należy zauważyć, że ustawiono **Typ** usługi połączone: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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

Dane są zapisywane do nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **RelationalSource** i typ **sink** jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** wybiera dane z REGUŁĘ. Tabelę zamówień w bazie danych.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Właściwości usługi połączone programu Sybase

Poniższa tabela zawiera opis specyficzne dla usługi Sybase połączone elementy JSON.

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Typ | Ustaw właściwości Typ: **OnPremisesSybase** | Tak
Serwer | Nazwa serwera programu Sybase. | Tak
bazy danych | Nazwa bazy danych programu Sybase. | Tak 
schematu  | Nazwa schematu w bazie danych. | Brak
authenticationType | Typ uwierzytelniania używany do łączenia się z bazą danych programu Sybase. Możliwe wartości to: anonimowe, podstawowe i systemu Windows. | Tak
Nazwa użytkownika | Określ nazwę użytkownika, jeśli jest używane uwierzytelnianie podstawowe i systemu Windows. | Brak
hasło | Określ hasło do konta użytkownika, który został określony jako nazwa użytkownika. |  Brak
gatewayName | Nazwa bramy, która usługa Factory danych należy używać do połączenia z bazą danych programu Sybase lokalnego. | Tak 

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych programu Sybase lokalnego, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Właściwości typu zestawu danych programu Sybase

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji **typeProperties** zestawu danych typu **RelationalTable** (obejmująca zestawu danych programu Sybase) zawiera następujące właściwości:

Właściwość | Opis | Wymagane
-------- | ----------- | --------
tableName | Nazwa tabeli w wystąpieniu bazy danych programu Sybase powiązanych z odwołujące się do. | Brak (jeśli jest określony **kwerendy** **RelationalSource** )

## <a name="sybase-copy-activity-type-properties"></a>Właściwości typ aktywności kopii programu Sybase 

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działań, zobacz artykuł [Tworzenie rurociągi](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło jest typu **RelationalSource** (obejmująca Sybase) w sekcji **typeProperties** są dostępne następujące właściwości:

Właściwość | Opis | Dopuszczalne wartości | Wymagane
-------- | ----------- | -------------- | --------
kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: zaznacz * z Moja_tabela. | Brak (jeśli jest określony **tableName** **zestawu danych** )

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Typ mapowania programu Sybase

Wymieniony w artykule [Działania przepływu danych](data-factory-data-movement-activities.md) aktywności kopii wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Sybase obsługuje T-SQL i typy T-SQL. Dla tabeli Mapowanie typów sql typu .NET, zobacz [Łącznik SQL Azure](data-factory-azure-sql-connector.md) artykuł.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.