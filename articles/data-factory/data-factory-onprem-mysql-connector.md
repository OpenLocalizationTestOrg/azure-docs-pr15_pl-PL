<properties 
    pageTitle="Przenoszenie danych z MySQL | Factory Azure danych" 
    description="Informacje na temat Przenoszenie danych z bazy danych MySQL przy użyciu Azure danych Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Przenoszenie danych z MySQL przy użyciu Factory danych Azure

W tym artykule przedstawiono, jak można to działanie kopii w factory Azure danych przenoszenie danych z MySQL do innego danych przechowywania. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Usługa Factory danych obsługuje nawiązywanie połączeń lokalnych źródeł MySQL za pomocą bramy zarządzania danymi. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) , aby uzyskać informacje o brama zarządzania danymi i instrukcje krok po kroku dotyczące konfigurowania bramy. 

> [AZURE.NOTE] Brama jest wymagane, nawet jeśli bazy danych MySQL jest obsługiwany w Azure IaaS maszyn wirtualnych (maszyn wirtualnych). Możesz zainstalować bramy na tym samym maszyn wirtualnych do przechowywania danych lub na inny maszyn wirtualnych jak bramy nawiązać połączenie z bazą danych.  

Dane factory obecnie obsługuje tylko dane ruchomą, z MySQL do innych sklepów danych, ale nie przenoszenie danych z innych baz danych do MySQL.

## <a name="installation"></a>Instalacji 
Dla bramy zarządzania danymi do połączenia z bazą danych MySQL należy zainstalować [MySQL łącznik/netto 6.6.5 dla systemu Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) na tym samym systemie co bramy zarządzania danymi.

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z bazą danych MySQL do dowolnego z magazynów sink obsługiwane jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok. Do wykonania skorzystać z instrukcji krok po kroku zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) . Dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Przykład: Kopiowanie danych z MySQL do obiektów Blob platformy Azure
W tym przykładzie pokazano, jak w celu skopiowania danych z bazy danych MySQL lokalnego magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  

> [AZURE.IMPORTANT] W tym przykładzie przedstawiono wstawki JSON. Nie zawiera instrukcje krok po kroku dotyczące tworzenia factory danych. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) instrukcje krok po kroku. 
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z wynik kwerendy w bazie danych MySQL do obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

Pierwszym krokiem konfiguracji bramy zarządzania danymi. Instrukcje znajdują się w artykule [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) . 

**Usługi połączone MySQL**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**Zestaw wprowadzania danych MySQL**

Próbki założono utworzysz tabeli "Moja_tabela" MySQL i zawiera kolumnę o nazwie "timestampcolumn" dla czasu serii danych.

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że tabeli zewnętrznej fabryki danych i nie jest tworzone przez działania w factory danych.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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



**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Właściwości usługi MySQL połączone

Poniższa tabela zawiera opis specyficzne dla usługi MySQL połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **OnPremisesMySql** | Tak |
| Serwer | Nazwa serwera MySQL. | Tak |
| bazy danych | Nazwa bazy danych MySQL. | Tak | 
| schematu  | Nazwa schematu w bazie danych. | Brak | 
| authenticationType | Typ uwierzytelniania używany do łączenia się z bazą danych MySQL. Możliwe wartości to: anonimowe, podstawowe i systemu Windows. | Tak | 
| Nazwa użytkownika | Określ nazwę użytkownika, jeśli jest używane uwierzytelnianie podstawowe i systemu Windows. | Brak | 
| hasło | Określ hasło do konta użytkownika, który został określony jako nazwa użytkownika. | Brak | 
| gatewayName | Nazwa bramy, która usługa Factory danych należy używać do połączenia z bazą danych MySQL lokalnego. | Tak |

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych MySQL lokalnego, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>Właściwości typu zestawu danych MySQL

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **RelationalTable** (obejmująca zestawu danych MySQL) zawiera następujące właściwości

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| tableName | Nazwa tabeli w wystąpieniu bazy danych MySQL, powiązanych z odwołujące się do. | Brak (jeśli jest określony **kwerendy** **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>Właściwości typ aktywności kopii MySQL

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis i wejściowe i wyjściowe tabele są zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji **typeProperties** działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło w działaniu Kopiuj jest typu **RelationalSource** (obejmująca MySQL) w sekcji typeProperties są dostępne następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: zaznacz * z Moja_tabela. | Brak (jeśli jest określony **tableName** **zestawu danych** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Typ mapowania MySQL

Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) kopii aktywności wykonuje konwersje typów automatyczne z typów źródeł tonięcia typy z następujących rozwinięcie na:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do MySQL, następujące mapowaniami umożliwia typów MySQL typy .NET.

| Typ danych MySQL | Typ .NET framework |
| ------------------- | ------------------- | 
| bigint niepodpisane | Dziesiętna |
| bigint | Int64 |
| bit | Dziesiętna |
| obiektów blob | Bajt] |
| wartość logiczna |  Wartość logiczna | 
| znak | Ciąg |
| Data | Daty i godziny |
| daty i godziny | Daty i godziny |
| dziesiętna | Dziesiętna |
| Podwójna precyzja | Naciśnij dwukrotnie |
| podwójne | Naciśnij dwukrotnie |
| wyliczenia | Ciąg |
| Przestaw | Pojedynczy |
| int niepodpisane | Int64 |
| int | Int32 |
| Liczba całkowita bez znaku | Int64 |
| Liczba całkowita | Int32 | 
| długie zmiennej liczby dwójkowej | Bajt] |
| długie varchar | Ciąg |
| longblob | Bajt] |
| LONGTEXT | Ciąg | 
| mediumblob |  Bajt] | 
| mediumint niepodpisane | Int64 |
| mediumint | Int32 | 
| mediumtext | Ciąg |
| liczbowe | Dziesiętna |
| liczba rzeczywista |  Naciśnij dwukrotnie |
| Ustawianie | Ciąg |
| smallint niepodpisane | Int32 |
| smallint | Int16 |
| tekst | Ciąg |
| czas | Przedziału czasu |
| Sygnatura czasowa | Daty i godziny |
| tinyblob | Bajt] |
| tinyint niepodpisane |  Int16 | 
| tinyint | Int16 |
| tinytext | Ciąg | 
| varchar | Ciąg | 
| rok | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

