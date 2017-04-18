## <a name="invoking-stored-procedure-for-sql-sink"></a>Wywoływanie procedury składowanej dla zlew SQL

Podczas kopiowania danych do programu SQL Server lub bazy danych serwera SQL/SQL Azure, procedura składowana może skonfigurować i wywoływana z dodatkowe parametry określona przez użytkownika. 

Procedura składowana można użyć podczas kopiowania wbudowane mechanizmy nie służą. Zazwyczaj jest to osobie, podczas przetwarzania dodatkowe (Scalanie kolumn, wyszukiwanie dodatkowe wartości wstawiania w wielu tabelach...) należy przed ostateczne wstawiania danych źródłowych w tabeli docelowej. 

Procedura składowana wybranym może wywołać. Poniższy przykład pokazuje, jak wykonywać za pomocą procedury składowanej prosty wstawiania do tabeli w bazie danych. 

**Dane wyjściowe zestawu danych**

W tym przykładzie ustawiono typ: SqlServerTable. Ustaw AzureSqlTable do użytku z bazą danych Azure SQL. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Definiowanie sekcji SqlSink w działaniu kopii JSON w następujący sposób. Aby zadzwonić do procedura składowana Wstawianie danych, potrzebne są zarówno SqlWriterStoredProcedureName, jak i SqlWriterTableType właściwości.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

W bazie danych należy zdefiniować procedura składowana o takiej samej nazwie jako SqlWriterStoredProcedureName. Dane wejściowe z określonego źródła i wstaw go obsługuje do tabeli wyników. Zwróć uwagę, nazwa parametru procedury składowanej powinny być taka sama jak tableName zdefiniowane w pliku tabeli JSON.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

W bazie danych można zdefiniować typ tabeli o takiej samej nazwie jak SqlWriterTableType. Należy zauważyć, że schemat tabeli powinien być taki sam jak schemat zwrócony przez wprowadzania danych.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Funkcja procedura składowana korzysta z [Parametrów Table-Valued](https://msdn.microsoft.com/library/bb675163.aspx).