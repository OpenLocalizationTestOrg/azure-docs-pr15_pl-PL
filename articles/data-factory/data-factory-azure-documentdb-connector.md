<properties 
    pageTitle="Przenoszenie danych do/z DocumentDB | Microsoft Azure" 
    description="Dowiedz się, jak przenieść dane do/z kolekcji Azure DocumentDB za pomocą Factory danych Azure" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Przenoszenie danych do i z DocumentDB przy użyciu Factory danych Azure

W tym artykule opisano, jak można to działanie kopii w factory Azure danych w celu przeniesienia danych do Azure DocumentDB z innego danych przechowywanie i przenoszenie danych z DocumentDB do innego magazynu danych. W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

W poniższych przykładach pokazano, jak skopiować dane do i z Azure DocumentDB i magazyn obiektów Blob platformy Azure. Jednak dane mogą być skopiowane **bezpośrednio** z dowolnego źródła do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.  

> [AZURE.NOTE] Kopiowania danych z o lokalnej Azure IaaS dane są przechowywane do Azure DocumentDB i odwrotnie są obsługiwane z bramy zarządzania danymi w wersji 2.1 lub wyższej.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Przykład: Kopiowanie danych z DocumentDB do obiektów Blob platformy Azure

Poniżej pokazano:

1. Usługi połączone typu [DocumentDb](#azure-documentdb-linked-service-properties).
2. Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Próbki kopiuje dane w Azure DocumentDB do obiektów Blob platformy Azure. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki.

**Usługa Azure DocumentDB połączone:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Magazyn obiektów Blob platformy Azure połączone usługi:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DB dokumentu wprowadzania zestawu danych:**

Próbki przyjęto założenie, że masz kolekcję o nazwie **osoby** w bazie danych programu Azure DocumentDB.
 
Ustawienie "zewnętrzne": "prawda" i określanie externalData informacji o zasadach usługi Azure danych Factory że tabeli zewnętrznej fabryki danych i nie są tworzone przez działania w factory danych.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Obiektów Blob platformy Azure wyjściowych zestawu danych:**

Dane są kopiowane do nowych obiektów blob co godzinę ścieżką dla blob odzwierciedlające określone daty/godziny z szczegółowości godzinę.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Przykładowy dokument JSON w zbiorze osoby w bazie danych DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB dokumentów podczas badania nad dokumentami JSON hierarchicznych przy użyciu SQL składni. 

Przykład: Wybierz pozycję Person.Name.Middle Person.PersonId, imię jako Person.Name.First jako MiddleName, nazwisko jako Person.Name.Last od osoby

Proces następujące kopiuje dane z kolekcji osoby w bazie danych DocumentDB do obiektów blob platformy Azure. W ramach działania Kopiuj dane wejściowe i wyjściowe określono zestawy danych.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Przykład: Kopiowanie danych z obiektów Blob platformy Azure do Azure DocumentDB

Poniżej pokazano:

1. Usługi połączone typu [DocumentDb](#azure-documentdb-linked-service-properties).
2. Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Próbki kopiuje dane z Azure obiektów blob do Azure DocumentDB. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki.

**Magazyn obiektów Blob platformy Azure połączone usługi:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Usługa Azure DocumentDB połączone:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob wprowadzania zestawu danych:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB wyjściowych zestawu danych:**

Próbki kopiuje dane do kolekcji o nazwie "Osoby".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Proces następujące kopiuje dane z obiektów Blob platformy Azure kolekcji osoby w DocumentDB. W ramach działania Kopiuj dane wejściowe i wyjściowe określono zestawy danych. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
W przypadku obiektów blob przykładowe dane wejściowe jako 

    1,John,,Doe

Następnie dane wyjściowe JSON w DocumentDB będą jako:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB jest magazynem NoSQL dokumentów JSON, gdzie są dozwolone struktury zagnieżdżone. Azure Factory danych umożliwia użytkownikowi oznaczenia hierarchii za pośrednictwem **nestingSeparator**, czyli "." w tym przykładzie. Z separatorem aktywności kopii generuje "Nazwa" obiekt z elementami trzy elementy podrzędne, drugie imię i nazwisko, zgodnie z "Name.First", "Name.Middle" i "Name.Last" w definicji tabeli.

## <a name="azure-documentdb-linked-service-properties"></a>Azure właściwości DocumentDB usługi połączone

Poniższa tabela zawiera opis specyficzne dla usługi Azure DocumentDB połączone elementy JSON. 

| **Właściwość** | **Opis** | **Wymagane** |
| -------- | ----------- | --------- |
| Typ | Ustaw właściwości Typ: **DocumentDb** | Tak |
| connectionString | Określanie informacji potrzebnych do połączenia z bazą danych Azure DocumentDB. | Tak |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure właściwości typu DocumentDB zestawu danych

Pełną listę sekcji i właściwości dostępnych do definiowania zestawów danych można znaleźć w artykule [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).
 
W sekcji typeProperties różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **DocumentDbCollection** zawiera następujące właściwości.

| **Właściwość** | **Opis** | **Wymagane** |
| -------- | ----------- | -------- |
| nazwa_kolekcji | Nazwa zbioru dokumentów DocumentDB. | Tak |


Przykład:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schemat przez Factory danych
W przypadku sklepów schematu dane takie jak DocumentDB usługę Factory danych ustala schematu w jednym z następujących sposobów:  

1.  Jeśli użytkownik określi, struktury danych przy użyciu właściwości **struktury** w definicji zestawu danych, usługę Factory danych uwzględnia zdefiniowane struktury jako schematu. W tym przypadku jeśli wiersz zawiera wartość dla kolumny, wartość null zostaną udostępnione dla niego.
2.  Jeśli nie określisz struktury danych przy użyciu właściwości **struktury** w definicji zestawu danych, usługę Factory danych ustala schematu przy użyciu pierwszego wiersza w danych. W tym przypadku jeśli pierwszy wiersz zawiera pełny schemat, niektóre kolumny będą widoczne w wyniku operacji kopiowania.

W związku z tym w przypadku źródeł danych wolny schematu najlepiej jest określenie struktury danych przy użyciu właściwości **struktury** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure właściwości typu DocumentDB kopii aktywności

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania przeczytaj artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań.
 
**Uwaga:** Działanie kopii trwa tylko jedno wejście i tworzy tylko jeden wynik.

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony, zależą od każdego typu działania i w przypadku działań kopii różnią się w zależności od rodzaju źródeł i ujść.

W przypadku działań kopii przypadku źródła typu **DocumentDbCollectionSource** następujące właściwości są dostępne w sekcji **typeProperties** :

| **Właściwość** | **Opis** | **Dopuszczalne wartości** | **Wymagane** |
| ------------ | --------------- | ------------------ | ------------ |
| kwerendy | Określ zapytania, aby odczytać danych. | Ciąg obsługiwanych przez DocumentDB kwerendy. <br/><br/>Przykład:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Brak <br/><br/>Jeśli nie zostanie określony, instrukcję SQL, która jest wykonywana:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Znak specjalny, aby wskazać, że dokument jest zagnieżdżone | Dowolny znak. <br/><br/>DocumentDB jest magazynem NoSQL dokumentów JSON, gdzie są dozwolone struktury zagnieżdżone. Azure Factory danych umożliwia użytkownikowi oznaczenia hierarchię za pośrednictwem nestingSeparator, czyli "." w powyższym przykładzie. Z separatorem aktywności kopii generuje "Nazwa" obiekt z elementami trzy elementy podrzędne, drugie imię i nazwisko, zgodnie z "Name.First", "Name.Middle" i "Name.Last" w definicji tabeli. | Brak

**DocumentDbCollectionSink** obsługuje następujące właściwości:

| **Właściwość** | **Opis** | **Dopuszczalne wartości** | **Wymagane** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Znak specjalny w nazwę kolumny źródłowej, aby wskazać zagnieżdżonych dokumentu jest potrzebny. <br/><br/>Na przykład powyżej: `Name.First` w wyniku kwerendy tabeli tworzy następującą strukturę JSON w dokumencie DocumentDB:<br/><br/>"Nazwa": {<br/>  "Imię": "Jan"<br/>}, | Znak, który jest używany do oddzielania poziomów zagnieżdżenia.<br/><br/>Wartość domyślna to `.` (kropka). | Znak, który jest używany do oddzielania poziomów zagnieżdżenia. <br/><br/>Wartość domyślna to `.` (kropka). | Brak | 
| writeBatchSize | Liczba żądań równoległych do usługi DocumentDB do tworzenia dokumentów.<br/><br/>Podczas kopiowania danych z DocumentDB za pomocą tej właściwości można dostosować wydajność. Zwiększanie writeBatchSize, ponieważ więcej równoległe żądania DocumentDB są wysyłane można oczekiwać lepszą wydajność. Jednak musisz uniknąć ograniczania, który może zostać zgłoszony komunikat o błędzie: "Żądanie stawka jest duży".<br/><br/>Ograniczanie decyduje wiele czynników, w tym rozmiar dokumentów, liczbę dokumentów, indeksowania zasad docelowym zbiorze itp. Do operacji kopiowania umożliwia lepsze zbioru (na przykład S3) mają najbardziej przepustowość dostępna (2500 żądania jednostki na sekundę). | Liczba całkowita | Nie (domyślny: 10000) |
| writeBatchTimeout | Czas na zakończenie przed przekroczenie limitu czasu operacji oczekiwania. | przedziału czasu<br/><br/> Przykład: "00: 30:00" (30 minut). | Brak |
 
## <a name="appendix"></a>Dodatek
1. **Pytanie:**  
    czy aktualizacją obsługi kopiowania aktywności istniejących rekordów?

    **Odpowiedź:**  
    nie.

2. **Pytanie:**  
    skąd ponowienie próby z kopią dotyczą DocumentDB już skopiowany rekordów?

    **Odpowiedź:**  
    rekordy mają pola "Identyfikator", operacja kopiowania próby wstawienia rekordu z tej samej operacji kopiowania zgłasza błąd.  
 
3. **Pytanie:** 
    Factory danych obsługuje [zakresu lub danych oparty na procesie mieszania podziału](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Odpowiedź:** 
    nie. 
4. **Pytanie:** 
    można określić więcej niż jednej kolekcji DocumentDB dla tabeli?
    
    **Odpowiedź:** 
    nie. W tej chwili można określić tylko jedną kolekcję.
     
## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.
