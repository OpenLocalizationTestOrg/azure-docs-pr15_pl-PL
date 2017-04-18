<properties 
    pageTitle="Przenoszenie danych do/z tabeli Azure | Microsoft Azure" 
    description="Dowiedz się, jak przenieść dane z magazynie tabel platformy Azure za pomocą Factory danych Azure." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Przenoszenie danych do i z tabeli Azure za pomocą Factory danych Azure

W tym artykule opisano, jak można to działanie kopii w factory Azure danych przenoszenie danych z tabeli Azure z innego magazynu danych. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych i danych obsługiwanych kombinacji ze sklepu z działaniem Kopiuj.

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane z magazynem tabel platformy Azure jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane do i z magazynem tabel platformy Azure i bazy danych obiektów Blob platformy Azure są pokazywane. Jednak dane mogą być skopiowane **bezpośrednio** z dowolnego źródła do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Przykład: Kopiowanie danych z tabeli Azure do obiektów Blob platformy Azure

Następujący przykład przedstawia:

1.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (używane do obiektów blob & tabeli).
2.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [Azure](#azure-table-dataset-type-properties).
3.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [AzureTableSource](#azure-table-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Próbki kopiuje dane należące do domyślną partycją tabeli Azure obiektów blob co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki.

**Usługa Azure magazynu połączone:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Factory danych obsługuje dwa typy usług magazynu Azure połączone: **AzureStorage** i **AzureStorageSas**. Dla pierwszą z nich możesz określić parametry połączenia, który zawiera klucz konta i dla nich później, określ identyfikator Uri udostępnione podpis programu Access (SA). Zobacz sekcję [Usługi połączone](#linked-services) , aby uzyskać szczegółowe informacje.  

**Azure tabeli danych wejściowych:**

Próbki założono, że została utworzona tabela "Moja_tabela" w tabeli Azure.
 
Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Obiektów Blob platformy Azure wyjściowych zestawu danych:**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Planowanej z działaniem kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **AzureTableSource** i typ **sink** jest ustawiona na **BlobSink**. Kwerenda SQL określony właściwość **AzureTableSourceQuery** wybiera dane z domyślną partycją co godzinę do skopiowania.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Przykład: Kopiowanie danych z obiektów Blob platformy Azure do tabeli platformy Azure

Następujący przykład przedstawia:

1.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (używane do obiektów blob & tabeli)
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [Azure](#azure-table-dataset-type-properties). 
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [AzureTableSink](#azure-table-copy-activity-type-properties). 


Kopie przykładowych szeregu czasowego danych z Azure blob Azure tabeli co godzina. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki.

**Usługa Azure miejsca do magazynowania (w przypadku tabeli Azure i Blob) połączone:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Factory danych obsługuje dwa typy usług magazynu Azure połączone: **AzureStorage** i **AzureStorageSas**. Dla pierwszą z nich możesz określić parametry połączenia, który zawiera klucz konta i dla nich później, określ identyfikator Uri udostępnione podpis programu Access (SA). Zobacz sekcję [Usługi połączone](#linked-services) , aby uzyskać szczegółowe informacje. 

**Azure Blob wprowadzania zestawu danych:**

Dane są pobierane z nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżkę i nazwę folderu dla obiektów blob dynamiczne są obliczane według czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc i dzień część czasu rozpoczęcia, a nazwa pliku używa godzinę część godziny rozpoczęcia. "zewnętrzne": "true" ustawienie usługę Factory danych informuje, że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Tabela Azure wyjściowych zestawu danych:**

Próbki kopiuje dane do tabeli o nazwie "Moja_tabela" w tabeli Azure. Tworzenie tabel platformy Azure z taką samą liczbę kolumn, zgodnie z oczekiwaniami pliku Blob CSV zawierać. Nowe wiersze zostaną dodane do tabeli co godzinę. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Planowanej z działaniem kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **BlobSource** i typ **sink** jest ustawiona na **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Usługi połączone
Istnieją dwa rodzaje połączonych usług, których możesz połączyć magazyn obiektów blob platformy Azure factory Azure danych. Są one: **AzureStorage** połączone usługi i **AzureStorageSas** połączone. Usługa Magazyn Azure połączone zapewnia factory danych programu access globalny do magazynu Azure. Połączone skojarzenia Azure miejsca do magazynowania zabezpieczeń (udostępnione podpis programu Access) usługi przewiduje factory danych z ograniczone/czas — powiązanych z dostępem do magazynu Azure. Istnieją inne różnice między te dwie usługi połączone. Wybierz usługę połączone, która odpowiada potrzebom użytkownika. W poniższych sekcjach przedstawiono szczegółowe informacje na temat tych dwóch połączonych usług.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure właściwości typu zestawu danych tabeli

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji **typeProperties** zestawu danych typu **Azure** zawiera następujące właściwości.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| tableName | Nazwa tabeli w wystąpieniu bazy danych tabeli Azure powiązanych z odwołujące się do. | Wartość Tak. Po określeniu tableName bez azureTableSourceQuery wszystkie rekordy z tabeli są kopiowane do miejsca docelowego. Jeśli określono także azureTableSourceQuery, rekordy z tabeli, która spełnia kwerendy są kopiowane do miejsca docelowego. |

### <a name="schema-by-data-factory"></a>Schemat przez Factory danych
W przypadku sklepów dane schematu przykład tabeli Azure usługę Factory danych ustala schematu w jednym z następujących sposobów:

1.  Jeśli użytkownik określi, struktury danych przy użyciu właściwości **struktury** w definicji zestawu danych, usługę Factory danych uwzględnia zdefiniowane struktury jako schematu. W tym przypadku jeśli wiersz zawiera wartość dla kolumny, wartość null są udostępniane dla niego.
2. Jeśli nie określisz struktury danych przy użyciu właściwości **struktury** w definicji zestawu danych, Factory danych ustala schematu przy użyciu pierwszego wiersza w danych. W tym przypadku pierwszego wiersza nie zawiera schematu pełny, spowoduje nieodebrane kilka kolumn w wyniku operacji kopiowania.

W związku z tym w przypadku źródeł danych wolny schematu najlepiej jest określenie struktury danych przy użyciu właściwości **struktury** .

## <a name="azure-table-copy-activity-type-properties"></a>Azure właściwości typu działania kopii tabeli

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe zestawy danych i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

**AzureTableSource** obsługuje następujące właściwości w sekcji typeProperties:

Właściwość | Opis | Dopuszczalne wartości | Wymagane
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Użyj kwerendy niestandardowej do odczytywania danych. | Tabela Azure ciągu kwerendy. Zobacz przykłady w następnej sekcji. | Wartość nie. Po określeniu tableName bez azureTableSourceQuery wszystkie rekordy z tabeli są kopiowane do miejsca docelowego. Jeśli określono także azureTableSourceQuery, rekordy z tabeli, która spełnia kwerendy są kopiowane do miejsca docelowego.
azureTableSourceIgnoreTableNotFound | Wskazuje, czy swallow wyjątek tabeli nie istnieje. | PRAWDA<br/>FAŁSZ | Brak |

### <a name="azuretablesourcequery-examples"></a>Przykłady azureTableSourceQuery

Jeśli kolumna tabeli Azure jest typu ciąg: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Jeśli kolumny tabeli Azure jest typu Data/Godzina: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** obsługuje następujące właściwości w sekcji typeProperties:


Właściwość | Opis | Dopuszczalne wartości | Wymagane  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Domyślne partition wartością klucza, która może być używany przez obiekt sink. | Wartość ciągu. | Brak 
azureTablePartitionKeyName | Określ nazwę kolumny, których wartości są używane jako partition klawiszy. Jeśli nie zostanie określony, AzureTableDefaultPartitionKeyValue jest używany jako klucz partycją. | Nazwa kolumny. | Brak |
azureTableRowKeyName | Określ nazwę kolumny, w których wartości kolumny są używane jako klucz wiersza. Jeśli nie zostanie określony, za pomocą identyfikator GUID dla każdego wiersza. | Nazwa kolumny. | Brak  
azureTableInsertType | Tryb do wstawiania danych do tabel platformy Azure.<br/><br/>Właściwość ta określa, czy wartości zastąpiony lub scalone zostać istniejących wierszy w tabeli wyników z dopasowanymi klawiszy partycją i wiersza. <br/><br/>Aby dowiedzieć się, jak działają te ustawienia (korespondencji seryjnej i zamień), zobacz tematy [Wstaw lub podmiotów korespondencji seryjnej](https://msdn.microsoft.com/library/azure/hh452241.aspx) i [Wstawianie lub zamienianie podmiotów](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> To ustawienie jest stosowane na poziomie wiersza, a nie na poziomie tabeli, a żadna z tych opcji powoduje usunięcie wierszy w tabeli wyników, których nie ma w danych wejściowych. | Scalanie (ustawienie domyślne)<br/>zamienianie | Brak 
writeBatchSize | Wstawia dane do tabeli Azure po naciśnięciu writeBatchSize lub writeBatchTimeout jest. | Liczba całkowita (liczba wierszy)| Nie (domyślny: 10000) 
writeBatchTimeout | Wstawia dane do tabeli Azure po naciśnięciu writeBatchSize lub writeBatchTimeout jest | przedziału czasu<br/><br/>Przykład: "00:20:00" (20 minut) | Nie (domyślnie limitu czasu domyślny klient miejsca do magazynowania wartość 90 s)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Zamapuj kolumnę źródłową kolumny docelowej przy użyciu translator właściwość JSON, zanim będzie można używać kolumnie docelowej jako azureTablePartitionKeyName.

W poniższym przykładzie kolumna źródłowa DivisionID są mapowane w kolumnie docelowej: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

DivisionID jest określony jako klucz partycją. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Typ mapowania tabel platformy Azure

Wymieniony w artykule [działania przepływu danych](data-factory-data-movement-activities.md) kopii aktywności wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujących rozwinięcie.

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych do i z tabeli Azure, następujące [mapowania zdefiniowane przez usługę Azure tabeli](https://msdn.microsoft.com/library/azure/dd179338.aspx) są używane z typów OData tabeli Azure typ .NET i na odwrót. 

| Typ danych OData | Typ .NET | Szczegóły |
| --------------- | --------- | ------- |
| Edm.Binary | bajt] | Tablica bajtów maksymalnie 64 KB. |
| Edm.Boolean | wartość logiczna | Wartość logiczna. |
| Edm.DateTime | Daty i godziny | Wartość 64-bitową, wyrażone jako uniwersalny czas koordynowany (UTC). Obsługiwane zakres daty i godziny zaczyna się od 12:00, a w 1 styczniu 1601 A.D. (R) UTC. Zakres kończy się 31 grudnia 9999. |
| Edm.Double | podwójne | 64-bitowa przestawny wartość punktu. |
| Edm.Guid | Identyfikator GUID | Globalnie unikatowy identyfikator 128-bitowego. |
| Edm.Int32 | Int32 lub int | Liczba całkowita 32-bitowa. |
| Edm.Int64 | Int64 lub długi | 64-bitowa liczba całkowita. |
| Edm.String | Ciąg | Wartość zakodowany UTF-16. Wartości ciągów może być maksymalnie 64 KB. |

### <a name="type-conversion-sample"></a>Przykładowe konwersji typu

Poniższy przykład jest kopiowanie danych z obiektów Blob platformy Azure do tabeli Azure z konwersje typów. 

Załóżmy, że obiektów Blob zestawu danych w formacie CSV zawiera trzy kolumny. Jeden z nich jest kolumną daty i godziny w formacie datetime niestandardowych przy użyciu skróconej francuskiego nazwy dnia tygodnia. 

Definiowanie zestawu danych źródłowych obiektów Blob następująco wraz z definicje typów kolumn.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Nadana mapowanie typu z typu OData tabeli Azure typ .NET, czy zdefiniować tabelę w tabeli Azure z następującego schematu. 

**Azure schematu tabeli:**

Nazwa kolumny | Typ
----------- | --------
Nazwa użytkownika | Edm.Int64
Nazwa | Edm.String 
lastlogindate | Edm.DateTime

Następnie określ zestaw danych tabeli Azure w następujący sposób. Nie musisz określić sekcji "struktury" informacji o typach, ponieważ informacje o typie jest już określony w magazynie danych źródłowych.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

W tym przypadku Factory danych automatycznie konwersji typów tym pole daty i godziny w formacie datetime niestandardowe przy użyciu kultury "fr-fr" podczas przenoszenia danych z obiektów Blob platformy Azure tabeli.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Aby uzyskać informacje o najważniejszych czynników wpływających na które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby, zobacz [wydajność działania kopii i dostosowywanie przewodnika](data-factory-copy-activity-performance.md).







