## <a name="column-mapping-with-translator-rules"></a>Mapowanie kolumny z regułami Minitłumacz
Mapowania kolumn można określić, jak kolumn określona w "strukturze" mapy tabeli źródła do kolumn określona w "strukturze" sink tabeli. Właściwość **mapowanie kolumny** jest dostępna w sekcji **typeProperties** działania Kopiuj.

Mapowanie kolumny obsługuje następujących scenariuszy:

- Wszystkie kolumny z tabeli źródłowej "struktury" są mapowane na wszystkich kolumn w tabeli sink "struktury".
- Podzestaw kolumn z tabeli źródłowej "struktury" są mapowane na wszystkich kolumn w tabeli sink "struktury".

Następujące warunki błędu i spowoduje wyjątków:

- Mniejsza liczba kolumn lub więcej kolumn w "strukturze" tabeli sink niż określona w mapowanie.
- Mapowanie zduplikowane.
- Wynik kwerendy SQL nie ma nazwę kolumny, określonego w mapowanie.

## <a name="column-mapping-samples"></a>Przykłady mapowania kolumn
> [AZURE.NOTE] Poniżej pokazano są Azure SQL i obiektów Blob platformy Azure, ale mają zastosowanie do dowolnego magazynu danych obsługującego prostokątnym zestawy danych. Musisz dostosować zestawu danych i definicji usługi połączone w przykładach poniżej, aby wskazywały danych w źródle danych. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Przykładowy 1 — kolumny mapowania z Azure SQL obiektów blob platformy Azure
W tym przykładzie Tabela wejściowa ma strukturę i wskazywała do tabeli w bazie danych programu Azure SQL SQL.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
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

W tym przykładzie tabela wyników ma strukturę i wskazuje obiektów blob w magazynie obiektów blob platformy Azure.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
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
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Poniżej przedstawiono JSON działania. Kolumny ze źródła zamapowane kolumn w sink (**columnMappings**) przy użyciu właściwości **Translator** .

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Przepływ mapowania kolumn:**

![Przepływ mapowania kolumn](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Przykładowy 2 — kolumny mapowania z zapytania SQL z Azure SQL do obiektów blob platformy Azure
W tym przykładzie zapytania SQL służy do wyodrębniania danych Azure SQL zamiast po prostu określenie nazwy tabeli i nazwy kolumn w sekcji "struktury". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

W tym przypadku wyników kwerendy najpierw są mapowane na kolumn określonych w "Struktura" źródła. Następnie kolumn ze źródła "struktury" są mapowane na kolumn w sink "struktury" z regułami określonego w columnMappings.  Załóżmy, że kwerenda zwraca 5 kolumn, dwie dodatkowe kolumny, a następnie określone w "Struktura" źródła.

**Przepływ mapowania kolumn**

![Mapowanie kolumny przepływu-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







