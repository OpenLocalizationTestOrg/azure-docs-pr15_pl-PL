<properties 
    pageTitle="Przenoszenie danych z Amazon prosty Usługa magazynu przy użyciu danych Factory | Microsoft Azure" 
    description="Informacje na temat sposobu przenoszenia danych z usługi Magazyn prosty Amazon (S3) przy użyciu Azure danych Factory." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Przenoszenie danych z Amazon prosty przestrzeni dyskowej usługi przy użyciu Factory danych Azure

W tym artykule przedstawiono, jak można to działanie kopii w factory Azure danych Aby przenieść dane mają być przechowywane w usłudze Amazon prosty miejsca do magazynowania (S3) do innego danych. W tym artykule opiera się na artykułu [działania przepływu danych](data-factory-data-movement-activities.md) , który przedstawia ogólne omówienie przenoszenia danych oraz listę obsługiwanych źródeł i sink magazynów aktywnością Kopiuj.  

Factory danych obecnie obsługuje tylko dane ruchomą, z Amazon S3 do innych sklepów danych, ale nie przenoszenie danych z innych baz danych do Amazon S3.

## <a name="required-permissions"></a>Wymagane uprawnienia

Aby skopiować dane z Amazon S3, upewnij się, że udzielono Ci poniższe uprawnienia:

- **S3:GetObject** i **s3:GetObjectVersion** dla operacji obiektu S3 Amazon
- **S3:ListBucket** i **s3:ListAllMyBuckets** (używany tylko kreatora kopiowania) dla operacji kolorem S3 Amazon 

[Określanie](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)uprawnień w zasadzie można znaleźć pełną listę Amazon S3 permisions ze szczegółami.

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia potok kopiuje dane z Amazon S3 jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Go pokazano, jak w celu skopiowania danych z Amazon S3 magazynem obiektów Blob platformy Azure. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Przykład: Kopiowanie danych z Amazon S3 do obiektów Blob platformy Azure
W tym przykładzie pokazano, jak w celu skopiowania danych z S3 Amazon magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

- Usługi połączone typu [AwsAccessKey](#linked-service-properties).
- Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AmazonS3](#dataset-type-properties).
- Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [FileSystemSource](#copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z Amazon S3 do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

**Usługi połączone S3 Amazon**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Zestaw danych wejściowych Amazon S3**

Ustawianie **"zewnętrznych": PRAWDA** usługę Factory danych informuje, że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych. Ustawiona na wartość PRAWDA na wprowadzania zestawu danych, który nie jest tworzone przez działania w potoku.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
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
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **FileSystemSource** i typ **sink** jest ustawiona na **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Właściwości usługi połączone

W poniższej tabeli przedstawiono opis dla określonych elementów JSON do Amazon S3 usługi połączone (**AwsAccessKey**).

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | Identyfikator klawisz dostępu poufne. | ciąg | Tak |
| secretAccessKey | Klawisz dostępu tajne się. | Zaszyfrowane tajny ciąg | Tak | 


## <a name="dataset-type-properties"></a>Właściwości typu zestawu danych

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasady są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **AmazonS3** (obejmująca Amazon S3 zestawu danych) zawiera następujące właściwości

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------- | ------ | 
| bucketName | Nazwa pakietu S3. | Ciąg | Tak |
| klawisz | Klucz obiektu S3. | Ciąg | Brak | 
| Prefiks | Prefiks klucza obiektu S3. Zostaną zaznaczone obiekty, których klawiszy początkowych ten prefiks. Dotyczy tylko wtedy, gdy klucza jest puste. | Ciąg | Brak | 
| Wersja | Wersja obiektu S3, jeśli jest włączone przechowywanie wersji S3. | Ciąg | Brak |  
| Formatowanie | Obsługiwane są następujące typy formatów: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Ustaw właściwości **Typ** w obszarze format jedną z następujących wartości. Zobacz [Określanie TextFormat](#specifying-textformat), [Określając AvroFormat](#specifying-avroformat) [Określającą JsonFormat](#specifying-jsonformat), [Określanie OrcFormat](#specifying-orcformat)i [Określanie ParquetFormat](#specifying-parquetformat) sekcji, aby uzyskać szczegółowe informacje. Jeśli chcesz skopiować pliki jako-jest między opartych na plikach magazynów (binarne kopii), możesz pominąć sekcji format w obu definicjach wejściowe i wyjściowe zestawu danych.| Brak
| stopień kompresji | Określ typ i stopień kompresji dla danych. Obsługiwane typy to: **GZip**i **Deflate**i **BZip2** poziomy obsługiwane są: **optymalna** i **najszybciej**. Obecnie ustawienia kompresji dla danych w **AvroFormat** lub **OrcFormat**nie są obsługiwane. Aby uzyskać więcej informacji, zobacz sekcję [kompresji pomocy technicznej](#compression-support) .  | Brak |

> [AZURE.NOTE] bucketName + klawisz Określa położenie obiektu S3, gdzie kolorem jest kontenera root S3 obiektów, a klawisz jest jego pełną ścieżkę do obiektu S3.

### <a name="sample-dataset-with-prefix"></a>Przykładowy zestaw danych z prefiksem

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Przykładowy zestaw danych (z wersja)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamiczne ścieżki S3

W próbce firma Microsoft korzysta z wartościami stałymi klucz i bucketName właściwości w zestawie danych Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Możesz mieć obliczanie klucz i bucketName dynamicznie w czasie rzeczywistym przy użyciu zmienne systemu, takie jak SliceStart Factory danych.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Jak taki sam dla właściwości prefiks zestaw Amazon S3. Aby uzyskać listę obsługiwanych funkcji i zmiennych, zobacz [Funkcje Factory danych i zmienne systemowe](data-factory-functions-variables.md) . 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Kopiowanie właściwości typ aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji **typeProperties** działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło w działaniu Kopiuj jest typu **FileSystemSource** (obejmująca Amazon S3), w sekcji typeProperties są dostępne następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- | 
| cykliczne | Określa, czy do lokalizacji lista S3 obiektów w katalogu. | PRAWDA i FAŁSZ | Brak | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

## <a name="next-steps"></a>Następne kroki
Zobacz następujące artykuły: 
- [Samouczek aktywności kopii](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) instrukcje krok po kroku dotyczące tworzenia potok aktywnością kopii. 
