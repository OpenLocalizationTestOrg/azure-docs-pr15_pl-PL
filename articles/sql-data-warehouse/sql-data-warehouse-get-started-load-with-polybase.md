<properties
   pageTitle="PolyBase w samouczku magazynu danych SQL | Microsoft Azure"
   description="Dowiedz się, co to jest PolyBase i jak z niego korzystać dla danych składu scenariuszy."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Ładowanie danych za pomocą PolyBase w magazynie danych SQL

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Factory danych](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Ten samouczek pokazano, jak załadować dane do magazynu danych SQL przy użyciu AzCopy i PolyBase. Gdy skończysz, będziesz wiedzieć, jak:

- Skopiuj dane z magazynem obiektów blob platformy Azure za pomocą AzCopy
- Tworzenie obiektów bazy danych, aby zdefiniować dane
- Uruchamianie kwerendy T-SQL, aby załadować dane

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Wymagania wstępne

Aby kroków ten samouczek, należy

- Baza danych SQL magazynu danych.
- Konto Azure magazynowania typu Standardowy lokalnie zbędne przestrzeni dyskowej (LRS standardowe), standardowego magazynu nadmiarowe Geo (Standard GRS) lub standardowego magazynu nadmiarowe Geo odczytu (Standard RAGRS).
- Narzędzie wiersza polecenia AzCopy. Pobierz i zainstaluj [najnowszą wersję programu AzCopy][] , która jest zainstalowany wraz z narzędzia Microsoft Azure miejsca do magazynowania.

    ![Narzędzia Azure miejsca do magazynowania](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Krok 1: Dodawanie przykładowych danych z magazynem obiektów blob platformy Azure

Aby załadować dane, należy umieścić kilka przykładowych danych do magazyn obiektów blob platformy Azure. W tym kroku możemy Wypełnij obiektów blob platformy Azure miejsca do magazynowania z przykładowymi danymi. Później użyjemy PolyBase załadować tych przykładowych danych do bazy danych SQL magazynu danych.

### <a name="a-prepare-a-sample-text-file"></a>ODPOWIEDZI. Przygotowywanie przykładowy plik tekstowy

Aby przygotować przykładowy plik tekstowy:

1. Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku. Zapisz to w katalogu lokalnym tymczasowym jako % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Znajdowanie punktu końcowego usługi obiektów blob usługi

Aby znaleźć punktu końcowego usługi obiektów blob usługi:

1. Azure Portal zaznacz **Przejdź** > **Kont miejsca do magazynowania**.
2. Kliknij konto miejsca do magazynowania, który ma być używany.
3. Karta konta miejsca do magazynowania kliknij obiektów blob

    ![Kliknij pozycję obiektów blob](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Zapisywanie adresu URL punktu końcowego usługi obiektów blob na później.

    ![Punkt końcowy usługi obiektów blob](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Znajdowanie klucza Azure miejsca do magazynowania

Aby znaleźć swój klucz Azure miejsca do magazynowania:

1. Azure Portal zaznacz **Przejdź** > **Kont miejsca do magazynowania**.
2. Kliknij konto miejsca do magazynowania, dla którego chcesz użyć.
3. Zaznacz **wszystkie ustawienia** > **klawiszy dostępu**.
4. Kliknij pole Kopiuj, aby skopiować jeden z klawiszy dostępu do Schowka.

    ![Kopiowanie klucza Azure magazynowania](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Skopiuj przykładowy plik z magazynem obiektów blob platformy Azure

Aby skopiować dane z magazynem obiektów blob platformy Azure:

1. Otwórz wiersz polecenia, a następnie zmień katalogów do katalogu instalacyjnego AzCopy. To polecenie zmienia się w domyślnym katalogu instalacji na komputerze klienckim systemu Windows w 64-bitowej.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Uruchom następujące polecenie, aby przekazać plik. Określanie adresu URL punktu końcowego usługi obiektów blob dla <blob service endpoint URL> i klucz konta Azure miejsca do magazynowania dla < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Zobacz też: [Wprowadzenie do narzędzia wiersza polecenia AzCopy][].

### <a name="e-explore-your-blob-storage-container"></a>E. Poznawanie programu kontenera magazynu obiektów blob

Aby wyświetlić plik przekazane do magazyn obiektów blob:

1. Wróć do swojego karta usługi obiektów Blob.
2. W obszarze kontenery kliknij dwukrotnie **datacontainer**.
3. Eksplorowanie ścieżkę dostępu do danych, kliknij folder **datedimension** , a zobaczysz przekazywanego pliku **DimDate2.txt**.
4. Aby wyświetlić właściwości, kliknij przycisk **DimDate2.txt**.
5. Należy zauważyć, że w karta właściwości obiektów Blob, można pobrać lub usuń go.

    ![Widok blob Azure miejsca do magazynowania](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Krok 2: Tworzenie tabeli zewnętrznej dla przykładowych danych

W tej sekcji tworzymy definiujące przykładowych danych tabeli zewnętrznej.

PolyBase używa zewnętrzne tabele, aby uzyskać dostęp do danych w magazynie obiektów blob platformy Azure. Ponieważ dane nie są przechowywane w obrębie magazynu danych SQL, PolyBase obsługuje uwierzytelnianie z danymi zewnętrznymi za pomocą poświadczenie występujące bazy danych.

Przykład w tym kroku używa tych instrukcji Transact-SQL do utworzenia tabeli zewnętrznej.

- [Tworzenie klucza głównego (Transact-SQL)][] do szyfrowania hasła bazy danych ograniczone poświadczeń.
- [Tworzenie bazy danych występujące poświadczeń (Transact-SQL)][] Określ informacje uwierzytelniające dla Twojego konta Azure miejsca do magazynowania.
- [Tworzenie zewnętrznego źródła danych (w języku Transact-SQL)][] , aby określić lokalizację magazyn obiektów blob platformy Azure.
- [Tworzenie zewnętrznych Format pliku (Transact-SQL)][] Określ format danych.
- [Tworzenie tabeli zewnętrznej (Transact-SQL)][] , aby określić definicji tabel i lokalizacji danych.

Uruchom tę kwerendę bazy danych SQL magazynu danych. Utworzy tabeli zewnętrznej o nazwie DimDate2External w schemacie dbo wskazującego na dane przykładowe DimDate2.txt w magazynie obiektów blob platformy Azure.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


W Eksploratorze obiektów serwera SQL w programie Visual Studio widać na format pliku zewnętrznych, zewnętrznego źródła danych i tabeli DimDate2External.

![Widok tabeli zewnętrznej](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Krok 3: Ładowanie danych do magazynu danych SQL

Po utworzeniu tabeli zewnętrznej można załadować dane do nowej tabeli lub wstawić go do istniejącej tabeli.

- Aby załadować dane do nowej tabeli, zestawienia [Utwórz tabelę jako wybierz (Transact-SQL)][] . Nowa tabela ma kolumn określona w kwerendzie. Typy danych w kolumnach będą zgodne typy danych w definicji tabeli zewnętrznej.
- Aby załadować dane do istniejącej tabeli, użyj [Wstawianie... Wybierz pozycję (Transact-SQL)][] instrukcji.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Tworzenie statystyk na załadowaniu danych

Program SQL Data Warehouse nie automatyczne tworzenie lub automatycznej aktualizacji statystyki. W związku z tym uzyskanie kwerendy wysokiej wydajności, należy utworzyć statystyki dla każdej kolumny w każdej tabeli po pierwszego obciążenia. Należy także zaktualizować statystykę po istotnych zmian w danych.

W tym przykładzie tworzy statystyki jednokolumnowego na nowej tabeli DimDate2.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Aby uzyskać więcej informacji, zobacz [Statystyka][].  


## <a name="next-steps"></a>Następne kroki
Zobacz [Przewodnik PolyBase][] , aby uzyskać więcej informacji, których warto wiedzieć, jak utworzyć rozwiązanie, które używa PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Przewodnik PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Wprowadzenie do narzędzia wiersza polecenia AzCopy]: ../storage/storage-use-azcopy.md
[Najnowsza wersja pakietu AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Tworzenie zewnętrznego źródła danych (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[Tworzenie zewnętrznego formatu pliku (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[Tworzenie tabeli zewnętrznej (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Tworzenie tabeli jako wybierz pozycję (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[WSTAW... Wybierz pozycję (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Tworzenie klucza głównego (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Tworzenie bazy danych POLU POŚWIADCZEŃ (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
