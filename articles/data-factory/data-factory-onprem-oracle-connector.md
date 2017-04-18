<properties 
    pageTitle="Przenoszenie danych do/z Oracle przy użyciu danych Factory | Microsoft Azure" 
    description="Dowiedz się, jak przenieść dane z bazy danych programu Oracle, która jest lokalnego przy użyciu Factory danych Azure." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Przenoszenie danych z Oracle lokalnego przy użyciu Factory danych Azure 

W tym artykule opisano, jak factory danych w trybie offline kopii umożliwia przenoszenie danych z programu Oracle z innego danych przechowywanie. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

## <a name="installation"></a>Instalacji 
Usługi Azure danych Factory mieć możliwość połączenia z bazą danych Oracle lokalnego trzeba zainstalować następujące składniki: 

- Brama zarządzania danymi na tym samym komputerze obsługującym bazę danych lub na komputerze osobnych unikać uczestniczących w zawodach dla zasobów w bazie danych. Brama zarządzania danymi jest agenta klienta, połączony lokalnych źródeł danych z usług w chmurze w sposób bezpieczny i zarządzanie nimi. Zobacz artykuł [Przenoszenie danych między wdrożeniem lokalnym a chmurą](data-factory-move-data-between-onprem-and-cloud.md) szczegółowe informacje na temat bramy zarządzania danymi. 
- Dostawca danych programu Oracle dla środowiska .NET. Ten składnik znajduje się w [Oracle danych programu Access składniki dla systemu Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Zainstaluj odpowiednią wersję (32-64-bitowa) na komputerze obsługującym miejsce, w którym jest zainstalowana brama. [12.1 .NET dostawca danych programu Oracle](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) dostęp do bazy danych programu Oracle 10 g w wersji 2 lub nowszej.

    Jeśli wybierzesz "XCopy instalacji", wykonaj czynności w readme.htm. Zalecamy zapoznanie wybrać Instalatora z interfejsu użytkownika (non-XCopy jeden). 
 
    Po zainstalowaniu dostawcy ponowne uruchomienie usługi hosta bramy zarządzania danymi na komputerze za pomocą usług aplet (lub) Menedżer konfiguracji bramy zarządzania danymi.  

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane z bazą danych Oracle do dowolnego z magazynów sink obsługiwane jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane z bazą danych Oracle z magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Przykład: Kopiowanie danych z programu Oracle do obiektów Blob platformy Azure
W tym przykładzie pokazano, jak w celu skopiowania danych z bazą danych Oracle lokalnego magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) jako źródła i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) jako sink.

Próbki kopiuje dane z tabeli w bazie danych programu Oracle lokalnego do obiektów blob co godzinę. Aby uzyskać więcej informacji na różne właściwości używane w próbce zobacz dokumentację w sekcjach poniżej próbki.

**Usługa Oracle połączone:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Magazyn obiektów Blob platformy Azure połączone usługi:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Zestaw wprowadzania danych Oracle:**

Próbki założono utworzysz tabeli "Moja_tabela" Oracle i zawiera kolumnę o nazwie "timestampcolumn" dla czasu serii danych. 

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżkę i nazwę folderu dla obiektów blob dynamiczne są obliczane według czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.
    
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


**Planowanej aktywnością kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **OracleSource** i typ **sink** jest ustawiona na **BlobSink**.  Kwerenda SQL określony właściwość **oracleReaderQuery** zaznacza dane w ostatniej godziny do skopiowania.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Należy dopasować ciągu kwerendy według konfiguracji dat w bazie danych programu Oracle. Jeśli pojawi się następujący komunikat o błędzie: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

może być konieczne zmodyfikowanie kwerendy, jak pokazano w poniższym przykładzie (za pomocą funkcji to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Przykład: Kopiowanie danych z obiektów Blob platformy Azure do bazy danych Oracle
W tym przykładzie pokazano sposób skopiuj dane z magazynem obiektów Blob platformy Azure z bazą danych Oracle lokalnego. Jednak dane mogą być skopiowane **bezpośrednio** z dowolnego źródła podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ze źródła [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) jako sink.

Próbki kopiuje dane z obiektu blob do tabeli w bazie danych programu Oracle lokalnego co godzinę. Aby uzyskać więcej informacji na różne właściwości używane w próbce zobacz dokumentację w sekcjach poniżej próbki.

**Usługa Oracle połączone:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Magazyn obiektów Blob platformy Azure połączone usługi:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure zestawu danych wejściowych obiektów Blob**

Dane są pobierane z nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżkę i nazwę folderu dla obiektów blob dynamiczne są obliczane według czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc i dzień część czasu rozpoczęcia, a nazwa pliku używa godzinę część godziny rozpoczęcia. "zewnętrzne": "true" ustawienie usługę Factory danych informuje, że tej tabeli zewnętrznej fabryki danych i nie jest tworzone przez działania w factory danych.

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

**Oracle dane wyjściowe zestawu danych:**

Próbki założono, że została utworzona tabela "Moja_tabela" w Oracle. Tworzenie tabeli w Oracle z taką samą liczbę kolumn, zgodnie z oczekiwaniami pliku CSV obiektów Blob zawierać. Nowe wiersze zostaną dodane do tabeli co godzinę.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Planowanej aktywnością kopii:**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **BlobSource** i typ **sink** jest ustawiona na **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Właściwości usługi Oracle połączone

Poniższa tabela zawiera opis JSON elementy specyficzne dla programu Oracle połączone usługi. 

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Typ | Ustaw właściwości Typ: **OnPremisesOracle** | Tak
connectionString | Określanie informacji potrzebnych do połączenia z wystąpieniem bazy danych programu Oracle dla właściwości connectionString. | Tak 
gatewayName | Nazwa bramy, który jest używany do łączenia się z serwerem programu Oracle lokalnego | Tak

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych programu Oracle lokalnego, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Właściwości typu zestawu danych Oracle

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Oracle, obiektów blob platformy Azure, Azure tabeli itp.).
 
W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu OracleTable ma następujące właściwości:

Właściwość | Opis | Wymagane
-------- | ----------- | --------
tableName | Nazwa tabeli w powiązanych z odwołuje się do bazy danych Oracle. | Brak (jeśli jest określony **oracleReaderQuery** **OracleSource** )

## <a name="oracle-copy-activity-type-properties"></a>Właściwości typ aktywności kopii bazy danych Oracle

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

> [AZURE.NOTE]
Działanie kopii trwa tylko jedno wejście i tworzy tylko jeden wynik.

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

### <a name="oraclesource"></a>OracleSource
W działaniu Kopiuj gdy źródłem jest typu **OracleSource** następujące właściwości są dostępne w sekcji **typeProperties** :

Właściwość | Opis |Dopuszczalne wartości | Wymagane
-------- | ----------- | ------------- | --------
oracleReaderQuery | Użyj kwerendy niestandardowej do odczytywania danych. | Ciąg zapytania SQL. Na przykład: zaznacz *z Moja_tabela <br/> <br/>Jeśli nie zostanie określony, instrukcji SQL, która jest wykonywana: zaznacz* z Moja_tabela | Brak (jeśli jest określony **tableName** **zestawu danych** )

### <a name="oraclesink"></a>OracleSink
**OracleSink** obsługuje następujące właściwości:

Właściwość | Opis | Dopuszczalne wartości | Wymagane
-------- | ----------- | -------------- | --------
writeBatchTimeout | Czas dla operacji wstawiania partii na zakończenie przekroczenie limitu czasu oczekiwania. | przedziału czasu<br/><br/> Przykład: 00:30:00 (30 minut). | Brak
writeBatchSize | Wstawia dane do tabeli SQL, gdy rozmiar buforu osiągnie writeBatchSize.   | Liczba całkowita (liczba wierszy)| Nie (domyślny: 10000)  
sqlWriterCleanupScript | Określić kwerendę wykonania kopii wykonać tak, aby dane określonych wycinek jest wyczyszczone. | Instrukcja kwerendy. | Brak
sliceIdentifierColumnName | Określ nazwę kolumny wykonania kopii można wypełnić generowana automatycznie identyfikator wycinek, który jest używany do oczyszczania danych określonych wycinek, gdy ponownie. | Nazwa kolumny kolumny z typem danych binary(32). | Brak


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Mapowanie typu dla programu Oracle

Wymienione w aktywności kopii artykuł [działania przepływu danych](data-factory-data-movement-activities.md) wykonuje konwersje typów automatyczne z typów źródeł, aby zatopić typy z następujące podejście krok 2:

1. Konwertowanie typów źródeł natywnych na typ .NET
2. Konwertowanie typu .NET na typ sink natywnych

Podczas przenoszenia danych z programu Oracle, następujące mapowania są używane z typem danych programu Oracle typ .NET i na odwrót.

Typ danych Oracle | Typ danych .NET framework
---------------- | ------------------------
BPLIK | Bajt]
OBIEKTÓW BLOB | Bajt]
ZNAK | Ciąg
CLOB | Ciąg
DATA | Daty i godziny
PRZESTAWNE | Dziesiętna
LICZBA CAŁKOWITA | Dziesiętna
INTERWAŁ ROK, MIESIĄC | Int32
DZIEŃ INTERWAŁU SEKUNDĘ | Przedziału czasu
DŁUGIE | Ciąg
DŁUGI NIEPRZETWORZONYCH | Bajt]
NCHAR | Ciąg
NCLOB | Ciąg
LICZBA | Dziesiętna
NVARCHAR2 | Ciąg
NIEPRZETWORZONYCH | Bajt]
WŁAŚCIWOŚĆ ROWID | Ciąg
SYGNATURA CZASOWA | Daty i godziny
SYGNATURA CZASOWA Z LOKALNEJ STREFY CZASOWEJ | Daty i godziny
SYGNATURA CZASOWA Z STREFY CZASOWEJ | Daty i godziny
LICZBA CAŁKOWITA BEZ ZNAKU | Liczba
VARCHAR2 | Ciąg
XML | Ciąg

## <a name="troubleshooting-tips"></a>Porady dotyczące rozwiązywania problemów

**Problemu:** Zostanie wyświetlony następujący **komunikat o błędzie**: kopiowanie działanie spełnione w przypadku nieprawidłowych parametrów: "UnknownParameterName" szczegółowego komunikatu: nie można odnaleźć żądanej .net Framework dostawca danych. Może nie być zainstalowane".  

**Możliwe przyczyny:**

1. Dostawca danych programu .NET Framework dla standardu Oracle nie został zainstalowany.
2. Dostawca danych programu .NET Framework dla standardu Oracle został zainstalowany na .NET Framework 2.0 i nie znajduje się w folderach programu .NET Framework 4.0. 

**Rozdzielczość i rozwiązania:**

1. Jeśli jeszcze nie zainstalowano dostawcę .NET dla programu Oracle, [Zainstaluj go](http://www.oracle.com/technetwork/topics/dotnet/downloads/) i spróbuj ponownie tego scenariusza. 
2. Jeśli zostanie wyświetlony komunikat o błędzie nawet po zainstalowaniu dostawcy, wykonaj następujące czynności: 
    1. Otwórz konfiguracji komputera .NET 2.0 z folderu: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Wyszukiwanie **Dostawca danych programu Oracle dla środowiska .NET**, a powinno być możliwe znaleźć wpis, jak pokazano w poniższej unwn próbki w następujących przykładowych NZder **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Skopiuj ten wpis do pliku machine.config w następującym folderze 4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, a następnie zmień jego wersję 4.xxx.x.x.
3.  Instalowanie "< ścieżka instalacji ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" do globalnej pamięci podręcznej zestawów (GAC), uruchamiając `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.
