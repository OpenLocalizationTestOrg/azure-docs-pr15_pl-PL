### <a name="type-conversion-sample"></a>Przykładowe konwersji typu
Poniższy przykład jest kopiowania danych z obiektów Blob SQL Azure z konwersje typów.

Załóżmy, że obiektów Blob zestawu danych w formacie CSV zawiera kolumny 3. Jeden z nich jest kolumną daty i godziny w formacie datetime niestandardowych przy użyciu skróconej francuskiego nazwy dnia tygodnia.

Określi zestawu danych źródłowych obiektów Blob następująco wraz z definicje typów kolumn.

    {
        "name": "AzureBlobTypeSystemInput",
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

Podane SQL typu do tabeli mapowania typu .NET powyżej, które chcesz zdefiniować tabelę Azure SQL z następującego schematu.

| Nazwa kolumny | Typ SQL |
| ----------- | -------- |
| Nazwa użytkownika | bigint |
| Nazwa | tekst |
| lastlogindate | daty i godziny |

Następny zdefiniujesz zestaw danych Azure SQL w następujący sposób. Uwaga: Nie musisz określić sekcji "struktury" informacji o typach, ponieważ informacje o typie już zostało określone w magazynie danych źródłowych.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

W tym przypadku factory danych będzie automatycznie wykonywać odpowiedni typ konwersji pola daty/godziny przy użyciu niestandardowej daty/godziny w tym sformatować przy użyciu kulturą fr-fr podczas przenoszenia danych z obiektów Blob SQL Azure.


