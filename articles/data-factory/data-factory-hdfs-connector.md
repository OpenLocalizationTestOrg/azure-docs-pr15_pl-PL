<properties 
    pageTitle="Przenoszenie danych z lokalnego HDFS | Factory Azure danych" 
    description="Informacje na temat przenoszenia danych z HDFS lokalnego przy użyciu Azure danych Factory." 
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

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Przenoszenie danych z HDFS lokalnego przy użyciu Factory danych Azure
W tym artykule opisano, jak można to działanie kopii w factory Azure danych do przenoszenia danych z lokalnego HDFS do innego magazynu danych. W tym artykule opiera się na artykułu [działania przepływu danych](data-factory-data-movement-activities.md) , który będzie zawierał ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Factory danych obecnie obsługuje tylko dane ruchomą, z HDFS lokalnego do innych sklepów danych, ale nie przenoszenie danych z innych baz danych do HDFS lokalnego.


## <a name="enabling-connectivity"></a>Włączanie połączenia
Usługa Factory danych obsługuje nawiązywanie połączeń z lokalnym HDFS za pomocą bramy zarządzania danymi. Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) , aby uzyskać informacje o brama zarządzania danymi i instrukcje krok po kroku dotyczące konfigurowania bramy. Nawiązywanie połączenia z HDFS, nawet jeśli jest ona hostowana w maszyn wirtualnych IaaS Azure za pomocą bramy. 

Gdy bramy można zainstalować na tym samym komputerze lokalnym lub maszyn wirtualnych Azure jako HDFS, zaleca się zainstalowanie bramy w osobnym komputera Azure IaaS maszyn wirtualnych. Masz bramy na komputerze oddzielnych zmniejsza konfliktu zasobów i zwiększa wydajność. Po zainstalowaniu bramy na komputerze osobnych komputera powinno być możliwe dostęp do komputera z HDFS. 


## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z lokalnego HDFS jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Kopiowanie danych między HDFS lokalnego magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Przykład: Kopiowanie danych z lokalnego HDFS do obiektów Blob platformy Azure

W tym przykładzie pokazano, jak w celu skopiowania danych z lokalnego HDFS magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

1.  Usługi połączone typu [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [lokalizacji](#hdfs-dataset-type-properties).
4.  Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [FileSystemSource](#hdfs-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z HDFS lokalnego do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

Pierwszym krokiem Konfigurowanie bramy zarządzania danymi. Instrukcje w artykule [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) . 

**Usługi połączone HDFS** W tym przykładzie użyto uwierzytelniania systemu Windows. Zobacz sekcję [HDFS połączone usługi](#hdfs-linked-service-properties) dla różnych typów uwierzytelniania, możesz użyć. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS wprowadzania danych** Tego zestawu danych odwołuje się do folderu HDFS DataTransfer/UnitTest /. Proces kopiuje wszystkie pliki w tym folderze do miejsca docelowego. 

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania tych wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **FileSystemSource** i typ **sink** jest ustawiona na **BlobSink**. Kwerenda SQL określona dla właściwości **kwerendy** zaznacza ostatniej godziny, aby skopiować dane.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Właściwości HDFS usługi połączone

Poniższa tabela zawiera opis JSON elementy specyficzne dla HDFS połączone usługi.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **Hdfs** | Tak | 
| Adres URL | Adres URL umożliwiający HDFS | Tak |
| encryptedCredential | Wynik [AzureRMDataFactoryEncryptValue nowy](https://msdn.microsoft.com/library/mt603802.aspx) poświadczeń programu access. | Brak |
| Nazwa użytkownika | Uwierzytelnianie nazwy użytkownika dla systemu Windows. | Tak (w przypadku uwierzytelniania systemu Windows)
| hasło | Hasło dla uwierzytelniania systemu Windows. | Tak (w przypadku uwierzytelniania systemu Windows)
| authenticationType | Systemu Windows, lub anonimowych. | Tak |
| gatewayName | Nazwa bramy, które powinny być używane przez usługę Factory danych nawiązać HDFS. | Tak |   

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla lokalnego HDFS, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Uwierzytelnianie anonimowe

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Przy użyciu funkcji uwierzytelniania systemu Windows
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Właściwości typu HDFS zestawu danych

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **lokalizacji** (obejmująca zestawu danych HDFS) zawiera następujące właściwości

Właściwość | Opis | Wymagane
-------- | ----------- | --------
ścieżkafolderu | Ścieżka do folderu. Przykład:`myfolder`<br/><br/>Używanie znaku anulowania "\" dla znaków specjalnych w ciągu. Na przykład: folder\subfolder, określ folder\\\\podfolder i d:\samplefolder, określ d:\\\\folder_przykładowy.<br/><br/>Tej właściwości można połączyć z **partitionBy** mają ścieżki folderów według wycinek daty i godziny rozpoczęcia i zakończenia. | Tak
Nazwa pliku | Określ nazwę pliku w **ścieżkafolderu** , jeśli chcesz, aby tabeli, aby odwołać się do określonego pliku w folderze. Jeśli nie określisz wartości tej właściwości tabeli wskazuje wszystkie pliki w folderze.<br/><br/>Jeśli nazwa pliku nie jest określona dla zestawu danych dane wyjściowe, nazwa wygenerowany plik jest w następującym formacie: <br/><br/>Dane. <Guid>txt (na przykład:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Brak
partitionedBy | partitionedBy można określić ścieżkafolderu dynamiczne, nazwę czasu serii danych. Przykład: ścieżkafolderu sparametryzowane dla każdej godziny danych. | Brak
obiektu fileFilter | Określ filtru w celu umożliwia wybranie podzbioru plików w ścieżkafolderu zamiast wszystkie pliki. <br/><br/>Dozwolone wartości są: `*` (wielu znaków) i `?` (pojedynczy znak).<br/><br/>Przykłady 1:`"fileFilter": "*.log"`<br/>Przykład 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Uwaga**: obiektu fileFilter ma zastosowanie do wprowadzania danych lokalizacji | Brak
| Formatowanie | Obsługiwane są następujące typy formatów: **TextFormat**, **AvroFormat** **JsonFormat**, **OrcFormat**i **ParquetFormat**. Ustaw właściwości **Typ** w obszarze format jedną z następujących wartości. Zobacz [Określanie TextFormat](#specifying-textformat), [Określając AvroFormat](#specifying-avroformat) [Określającą JsonFormat](#specifying-jsonformat), [Określanie OrcFormat](#specifying-orcformat)i [Określanie ParquetFormat](#specifying-parquetformat) sekcji, aby uzyskać szczegółowe informacje. Jeśli chcesz skopiować pliki jako-jest między opartych na plikach magazynów (binarne kopii), możesz pominąć sekcji format w obu definicjach wejściowe i wyjściowe zestawu danych. | Brak 
| stopień kompresji | Określ typ i stopień kompresji dla danych. Obsługiwane typy to: **GZip**i **Deflate**i **BZip2** poziomy obsługiwane są: **optymalna** i **najszybciej**. Obecnie ustawienia kompresji dla danych w **AvroFormat** lub **OrcFormat**nie są obsługiwane. Aby uzyskać więcej informacji, zobacz sekcję [kompresji pomocy technicznej](#compression-support) .  | Brak |



> [AZURE.NOTE] Nie można jednocześnie używać nazwy i obiektu fileFilter.


### <a name="using-partionedby-property"></a>Przy użyciu właściwości partionedBy

Jak wspomniano w poprzedniej sekcji, możesz określić ścieżkafolderu dynamiczne, nazwę czasu serii danych za pomocą partitionedBy. Aby to zrobić przy użyciu makra danych Factory i zmiennej systemowej SliceStart, SliceEnd, wskazujące logiczne okres dla danego wycinek. 

Aby dowiedzieć się więcej o zestawy czasu serii danych, planowania i wycinki, zobacz artykuły [Tworzenie zestawów danych](data-factory-create-datasets.md), [Planowanie i wykonanie](data-factory-scheduling-and-execution.md)i [Procesy tworzenia](data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Przykład 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

W tym przykładzie {wycinek} jest zastępowany wartość zmiennej system Factory danych SliceStart w formacie (YYYYMMDDHH) określone. SliceStart odwołuje się do wycinek czas rozpoczęcia. Ścieżkafolderu różni się dla każdego wycinka. Na przykład: wikidatagateway-wikisampledataout-2014100103 lub wikidatagateway-wikisampledataout-2014100104.

#### <a name="sample-2"></a>Przykład 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

W tym przykładzie rok, miesiąc, dzień i godzinę SliceStart są wyodrębniane do oddzielnych zmiennych, które są używane przez właściwości ścieżkafolderu i nazwę pliku.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Właściwości Typ HDFS kopii aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Wykonania kopii gdy źródłem jest typu **FileSystemSource** następujące właściwości są dostępne w sekcji typeProperties:

**FileSystemSource** obsługuje następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| cykliczne | Wskazuje, czy dane czytania lokalizacji z folderów sub lub tylko z określonego folderu. | PRAWDA, FAŁSZ (ustawienie domyślne)| Brak |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

