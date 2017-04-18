<properties 
    pageTitle="Przenoszenie danych z serwera FTP | Microsoft Azure" 
    description="Informacje na temat Przenoszenie danych z serwera FTP przy użyciu Azure danych Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Przenoszenie danych z serwera FTP przy użyciu Factory danych Azure
W tym artykule omówiono, jak za pomocą aktywności kopii w Factory danych Azure przenoszenie danych z serwera FTP do magazynu danych obsługiwanych sink. W tym artykule tworzy się w artykule [działania przepływu danych](data-factory-data-movement-activities.md) , który przedstawia ogólne omówienie przenoszenia danych z kopii aktywności i Lista sklepów danych jako źródeł i ujść obsługiwane. 

Dane factory obecnie obsługuje tylko dane ruchomą, z serwera FTP do innych sklepów danych, ale nie przenoszenie danych z innych baz danych na serwerze FTP. Obsługuje on zarówno w lokalnej i w chmurze serwerów FTP. 

Jeśli chcesz przenieść dane z serwera **lokalnego** FTP do magazynu danych w chmurze (przykład: magazyn obiektów Blob platformy Azure), instalowanie i używanie bramy zarządzania danymi. Brama zarządzania danymi jest agenta klienta, który jest zainstalowany na komputerze lokalnym umożliwia usług w chmurze nawiązywania połączenia z zasobów lokalnych. Aby uzyskać szczegółowe informacje na bramie, zobacz [Bramy zarządzania danymi](data-factory-data-management-gateway.md) . Zobacz artykuł [Przenoszenie danych między lokalizacjami lokalnego i chmury](data-factory-move-data-between-onprem-and-cloud.md) instrukcje krok po kroku dotyczące konfigurowania bramy i korzystania z niego. Nawiązywanie połączenia z serwerem FTP, nawet jeśli serwer znajduje się na Azure IaaS maszyn wirtualnych (maszyn wirtualnych) za pomocą bramy. 

Możesz zainstalować bramy na tym samym komputerze lokalnym lub maszyn wirtualnych IaaS Azure, co na serwerze FTP. Jednak zaleca się zainstalowanie bramy na osobne komputera lub osobnych maszyn wirtualnych IaaS Azure, aby uniknąć konfliktu zasobów i aby zapewnić lepszą wydajność. Po zainstalowaniu bramy na komputerze osobnych komputera powinno być możliwe do uzyskania dostępu do serwera FTP. 

## <a name="copy-data-wizard"></a>Kopiowanie kreatora danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane z serwera FTP jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższe przykłady zawierają definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Przykład: Kopiowanie danych z serwera FTP obiektów blob platformy Azure

W tym przykładzie pokazano, jak skopiować dane z serwera FTP magazynem obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** do dowolnego z pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  
 
Próbka obejmuje następujące jednostki factory danych:

- Usługi połączone typu [SerwerFTP](#ftp-linked-service-properties).
- Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [lokalizacji](#fileshare-dataset-type-properties).
- Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [FileSystemSource](#ftp-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane z serwera FTP do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. 

**Usługi połączone FTP** W tym przykładzie użyto uwierzytelniania podstawowego za pomocą nazwy użytkownika i hasła w formacie zwykłego tekstu. Można także użyć jednego z następujących sposobów: 

- Uwierzytelnianie anonimowe 
- Uwierzytelnianie podstawowe z szyfrowanymi poświadczeniami
- FTP na SSL/TLS (FTPS)

Zobacz sekcję [FTP połączone usługi](#ftp-linked-service-properties) dla różnych typów uwierzytelniania, możesz użyć. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**Zestaw FTP wprowadzania danych** Tego zestawu danych odwołuje się do folderu FTP `mysharedfolder` i plik `test.csv`. Proces kopiuje plik do miejsca docelowego. 

Ustawienie "zewnętrzne": "true" zostanie wyświetlona informacja, usług danych Factory że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane nowe blob co godzinę (częstotliwość: godzina, interwał: 1). Ścieżka folderu to jest dynamicznie obliczane na podstawie czasu rozpoczęcia wycinek, który jest przetwarzana. Ścieżka folderu używa rok, miesiąc, dzień i godzin części godziny rozpoczęcia.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
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
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Właściwości połączone witryny FTP

Poniższa tabela zawiera opis specyficzne dla FTP powiązanych z elementów JSON.

| Właściwość | Opis | Wymagane | Domyślne |
| -------- | ----------- | -------- | ------- | 
| Typ | Właściwość Typ musi być równa SerwerFTP | Tak | &nbsp;
| hosta | Nazwa lub adres IP serwera FTP | Tak | &nbsp;
| authenticationType | Określ typ uwierzytelniania | Tak | Podstawowe i anonimowe |
| Nazwa użytkownika | Użytkownik ma dostęp do serwera FTP | Brak | &nbsp;
| hasło | Hasło użytkownika (nazwa użytkownika) | Brak | &nbsp;
| encryptedCredential | Zaszyfrowane poświadczenia dostępu do serwera FTP | Brak | &nbsp;
| gatewayName | Nazwa bramy bramy zarządzania danymi w celu nawiązania połączenia z serwerem FTP lokalnego | Brak    | &nbsp;
| Port | Port, na którym oczekuje się na serwerze FTP | Brak | 21 |
| enableSsl | Określanie, czy należy używać FTP za pośrednictwem kanału SSL/TLS | Brak | wartość PRAWDA. | 
| enableServerCertificateValidation | Określ, czy włączyć sprawdzanie poprawności certyfikatu SSL serwera w przypadku używania FTP na kanału SSL/TLS | Brak | wartość PRAWDA. | 

### <a name="using-anonymous-authentication"></a>Uwierzytelnianie anonimowe

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Przy użyciu nazwy użytkownika i hasła w formacie zwykłego tekstu do uwierzytelniania podstawowego
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Korzystanie z portu, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Przy użyciu encryptedCredential dla uwierzytelniania i bramy

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Aby uzyskać szczegółowe informacje o ustawianiu poświadczeń dla źródła danych FTP lokalnego, zobacz [ustawić poświadczenia i zabezpieczenia](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Kopia FTP właściwości typ aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii właściwości typu różnią się w zależności od rodzaju źródeł i ujść.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.

## <a name="next-steps"></a>Następne kroki
Zobacz następujące artykuły: 

- [Samouczek aktywności kopii](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) instrukcje krok po kroku dotyczące tworzenia potok aktywnością kopii. 
