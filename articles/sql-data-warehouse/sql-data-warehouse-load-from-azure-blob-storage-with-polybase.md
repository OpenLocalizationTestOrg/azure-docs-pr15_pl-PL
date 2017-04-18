<properties
   pageTitle="Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase) | Microsoft Azure"
   description="Dowiedz się, jak za pomocą PolyBase załadować dane z magazynem obiektów blob Azure do magazynu danych SQL. Załaduj kilka tabel z danych publicznych w schemacie Contoso detalicznej Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)

> [AZURE.SELECTOR]
- [Factory danych](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Aby załadować dane z magazynem obiektów blob Azure do magazynu danych SQL Azure, należy użyć poleceń PolyBase i T-SQL. 

Aby zachować prosty, ten samouczek ładuje dwie tabele z publicznej obiektów Blob miejsca do magazynowania Azure do schematu Contoso detalicznej Data Warehouse. Aby załadować pełny zestaw danych, można uruchomić przykład [Ładowanie pełny Contoso w sprzedaży detalicznej Data Warehouse][] z repozytorium programu Microsoft SQL Server próbki.

W tym samouczku dowiesz się:

1. Konfigurowanie PolyBase, aby załadować z magazynem obiektów blob Azure
2. Ładowanie danych publicznych do bazy danych
3. Wykonywanie optymalizacje, po zakończeniu ładowania.


## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby uruchomić ten samouczek, potrzebne jest konto Azure, która już zawiera bazy danych SQL Data Warehouse. Jeśli nie masz jeszcze to, zobacz [Tworzenie magazynu danych SQL][].

## <a name="1-configure-the-data-source"></a>1. Konfigurowanie źródła danych

PolyBase używa obiektów zewnętrznych T-SQL, aby zdefiniować lokalizację i atrybuty danych zewnętrznych. Definicje obiektu zewnętrznego są przechowywane w magazynie danych SQL. Dane są magazynowane zewnętrznie.

### <a name="11-create-a-credential"></a>1.1. Tworzenie poświadczenie

**Pomiń ten krok** , jeśli są ładowane danych publicznych firmy Contoso. Ponieważ jest już dostępne dla wszystkich nie trzeba bezpiecznego dostępu do danych publicznych.

Jeśli korzystasz z tego samouczka jako szablon ładowania własnych danych **nie Pomiń ten krok** . Dostęp do danych za pośrednictwem poświadczenie, poświadczenie występujące bazy danych za pomocą tego skryptu, a następnie użyj go podczas definiowania lokalizacji źródła danych.


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
```

Przejdź do kroku 2.

### <a name="12-create-the-external-data-source"></a>1.2. Tworzenie źródła danych zewnętrznych

Użyj tego polecenia [Utwórz zewnętrznego źródła danych][] do przechowywania lokalizacji danych i typ danych. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Jeśli użytkownik chce udostępnić usługi obiektów blob platformy azure kontenerów miejsca do magazynowania, należy pamiętać, że właściciel danych zostanie naliczona dla danych opłaty wyjściowego podczas dane pozostaną centrum danych. 

## <a name="2-configure-data-format"></a>2. Skonfiguruj format danych

Dane są przechowywane w plikach tekstowych w magazynie obiektów blob platformy Azure i każdego pola są rozdzielone ogranicznika. Polecenie [Utwórz zewnętrznych FORMAT pliku][] Określ format danych w plikach tekstowych. Danych firmy Contoso jest nieskompresowane i rozdzielany potoku.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. Tworzenie tabel zewnętrznych

Teraz, gdy określono danych źródłowych i format pliku, możesz przystąpić do tworzenia tabel zewnętrznych. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Utwórz schemat danych. 

Miejsce do przechowywania danych firmy Contoso w bazie danych, utworzyć schemat.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. Tworzenie tabel zewnętrznych. 

Uruchom ten skrypt w celu utworzenia tabeli zewnętrznej Wymiarprodukt i FactOnlineSales. Po wyświetleniu wszystkich robimy tutaj jest definiowania nazw kolumn i typów danych i powiązanie ich z lokalizacją i formatu plików magazyn obiektów blob platformy Azure. Definicji są przechowywane w magazynie danych SQL i dane są nadal w Azure Blob miejsca do magazynowania.

Parametr **Lokalizacja** jest to folder w folderze głównym w obiektów Blob miejsca do magazynowania Azure. Każda tabela znajduje się w innym folderze.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. załadować dane
Ma dostęp do danych zewnętrznych na różne sposoby.  Możesz kwerendy danych bezpośrednio z tabeli zewnętrznej, załadować dane do nowych tabel bazy danych lub dodawanie danych zewnętrznych do istniejących tabel bazy danych.  


### <a name="41-create-a-new-schema"></a>4.1. Tworzenie nowego schematu

CTAS tworzy nową tabelę, która zawiera dane.  Najpierw należy utworzyć schemat danych firmy contoso.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Ładowanie danych do nowych tabel

Aby załadować dane z magazynem obiektów blob Azure i zapisz go w tabeli w bazie danych, należy użyć instrukcji [Utwórz tabelę jako wybierz (Transact-SQL)][] . Ładowanie z CTAS wykorzystuje jednoznacznie określonym tabel zewnętrznych właśnie utworzonego. Aby załadować dane do nowych tabel, należy użyć jednej instrukcji [CTAS][] na tabelę. 

CTAS tworzy nową tabelę i wypełnia ją z wynikami instrukcji select. CTAS określa nowa tabela ma mieć takie same kolumny i typy danych jako wyniki instrukcji select. Zaznaczenie wszystkich kolumn z tabeli zewnętrznej nowej tabeli będzie replice kolumn i typy danych w tabeli zewnętrznej.

W tym przykładzie tworzymy zarówno wymiar i tabeli faktów jako skrótu rozłożone tabel. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 śledzić postęp ładowania

Możesz śledzić postęp do ładowania korzystanie z widoków dynamiczne zarządzanie (DMVs). 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. Optymalizuj kompresję columnstore

Domyślnie program SQL Data Warehouse przechowuje tabeli jako indeks columnstore grupowany. Po zakończeniu ładowania niektórych wierszy danych może nie można skompresować w columnstore.  Istnieje szereg powodów, dlaczego jest to możliwe. Aby uzyskać więcej informacji, zobacz [Zarządzanie columnstore indeksy][].

Aby zoptymalizować wydajność kwerend i kompresji columnstore po załadowaniu, odbudowanie tabeli, aby wymusić indeks columnstore, aby skompresować wszystkie wiersze. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Aby uzyskać więcej informacji na utrzymanie indeksy columnstore zobacz artykuł [Zarządzanie columnstore indeksy][] .

## <a name="6-optimize-statistics"></a>6. statystyki zoptymalizować

Najlepiej utworzyć statystyki jednokolumnowego natychmiast po załadowaniu. Istnieje kilka możliwości przeprowadzenia statystyki. Na przykład jeśli utworzysz statystyki jednokolumnowego w każdej kolumnie go może zająć dużo czasu odbudować wszystkich statystyk. Jeśli wiesz, że niektóre kolumny nie będą znajdować się w orzeczenia kwerendy, możesz pominąć tworzenie statystyki na podstawie tych kolumn.

Jeśli zdecydujesz się na tworzenie statystyk jednokolumnowego w każdej kolumnie każda tabela, możesz użyć procedury składowanej przykładowy kod `prc_sqldw_create_stats` w artykule [Statystyki][] .

Poniższy przykład to dobry punkt wyjścia do tworzenia statystyk. Tworzy statystyki jednokolumnowego na każdej z kolumn w tabeli wymiarów, a następnie na każdej łączących kolumny w tabeli faktów. Możesz zawsze dodać statystyki jednej lub wielu kolumn do innych kolumn w tabeli faktów później.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Osiągnięć odblokowywane!

Pomyślnie załadowano danych publicznych do magazynu danych SQL Azure. Wspaniale!

Możesz teraz rozpocząć badanie tabel za pomocą kwerendy podobnej do następującej:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Następne kroki
Aby załadować pełny danych firmy Contoso detalicznej Data Warehouse, aby uzyskać więcej porad rozwoju za pomocą skryptu, zobacz [Omówienie tworzenia magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Tworzenie magazynu danych SQL]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Omówienie tworzenia magazynu danych SQL]: sql-data-warehouse-overview-develop.md
[Zarządzanie columnstore indeksów]: sql-data-warehouse-tables-index.md
[Statystyki]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[TWORZENIE ZEWNĘTRZNEGO ŹRÓDŁA DANYCH]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[TWORZENIE ZEWNĘTRZNEGO FORMATU PLIKU]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Tworzenie tabeli jako wybierz pozycję (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Ładowanie pełnym magazynem danych sprzedaży detalicznej Contoso]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
