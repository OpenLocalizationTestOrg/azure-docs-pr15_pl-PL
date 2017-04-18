<properties
   pageTitle="Omówienie tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do tabel magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Omówienie tabel w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Wprowadzenie do tworzenia tabel w magazynie danych SQL jest proste.  Podstawowa składnia [CREATE TABLE][] następuje typowych składnia najprawdopodobniej zna Praca z innymi bazami danych.  Aby utworzyć tabelę, po prostu musisz nazwy tabeli, nazwy kolumn i definiować typy danych dla każdej kolumny.  Jeśli zostały Tworzenie tabel w innych baz danych, to powinna wyglądać dobrze znany dla Ciebie.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Powyższy przykład tworzy tabelę o nazwie Customers z dwiema kolumnami, imię i nazwisko.  Każda kolumna jest definiowana o typie danych VARCHAR(25), który ogranicza dane do 25 znaków.  Tych podstawowych atrybutów tabeli, a także inne osoby, głównie to samo jak inne bazy danych.  Typy danych są definiowane dla każdej kolumny i zapewnienia integralności danych.  Aby zwiększyć wydajność przez zmniejszenie we/wy można dodawać indeksy.  Partycje można dodawać w celu zwiększenia wydajności, gdy trzeba zmodyfikować dane.

[Zmiana nazwy] [ RENAME] tabela SQL Data Warehouse wygląda następująco:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Rozłożone tabel

Nowy atrybut podstawowych wprowadzonego systemów dystrybucji, takich jak program SQL Data Warehouse jest **kolumnę rozkładu**.  Kolumnę rozkładu jest bardzo co go brzmi jak.  Jest kolumna, która określa sposób rozpowszechnianie lub dzielenie danych w tle.  Po utworzeniu tabeli bez określania kolumnę rozkładu tabeli jest automatycznie rozkładana według **okrężnego**.  Okrężnego tabele mogą być wystarczające w niektórych scenariuszach, definiowania kolumn dystrybucyjnej można znacznie zmniejszyć przenoszenia danych podczas wykonywania kwerend, więc optymalizacji wydajności.  Zobacz [rozpowszechniania tabeli][Rozłóż] Aby dowiedzieć się więcej na temat zaznacz kolumnę rozkładu.

## <a name="indexing-and-partitioning-tables"></a>Indeksowanie i podziału tabel

Stają się bardziej zaawansowane się przy użyciu magazynu danych SQL i chcesz zoptymalizować, chcesz dowiedzieć się więcej o projekt tabeli.  Aby uzyskać więcej informacji, zobacz artykuły w [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks] i [podziału tabeli][partycją].

## <a name="table-statistics"></a>Tabeli statystyki

Statystyki są bardzo ważne dla efektywnego najlepszą wydajność z magazynu danych SQL.  Ponieważ program SQL Data Warehouse nie jeszcze automatycznie utworzyć i statystyki aktualizacji dla Ciebie, jak mogą pochodzić można oczekiwać w bazie danych SQL Azure, czytania naszych artykułu w witrynie [Statystyki][] może być jednym z najważniejszych artykułów przeczytaj aby mieć pewność, że najlepszą wydajność z kwerend.

## <a name="temporary-tables"></a>Tabele tymczasowe

Tabele tymczasowe są tabele, które tylko istnieją na czas trwania logowania i nie są widoczne dla innych użytkowników.  Tymczasowe tabele mogą być dobrym sposobem, aby uniemożliwić innym osobom wyświetlanie wyników tymczasowe i również zmniejszyć potrzebę oczyszczanie.  Ponieważ tymczasowych tabel sieci Web magazynu lokalnego, oferują można zwiększyć wydajność dla niektórych operacji.  Zobacz artykuły[tymczasowe] [Tabela tymczasowa]uzyskać więcej szczegółowych informacji o tabelach tymczasowych.

## <a name="external-tables"></a>Tabele zewnętrzne

Zewnętrzne tabele, nazywane także Polybase tabele są tabele, które mogą być wysyłane kwerendy z magazynu danych SQL, ale punkt danych zewnętrznych z magazynu danych SQL.  Na przykład można utworzyć tabeli zewnętrznej wskazującą do plików na magazyn obiektów Blob platformy Azure.  Aby uzyskać więcej informacji na temat tworzenia i przeszukiwania tabeli zewnętrznej zobacz [Ładowanie danych za pomocą Polybase][].  

## <a name="unsupported-table-features"></a>Nieobsługiwane funkcje tabel

Program SQL Data Warehouse zawiera wiele z tych samych funkcji tabeli oferowanych przez innych baz danych, wiążą się niektóre funkcje, które nie są jeszcze obsługiwane.  Poniżej przedstawiono listę niektóre funkcje tabel, które nie są jeszcze obsługiwane.

| Nieobsługiwane funkcje |
| --- |
|[Właściwość tożsamości][] (zobacz [Przypisywanie obejście klucza zastępczego][])|
|Klucz podstawowy, kluczy obcych unikatowych i wyboru [Powiązanych tabel][]|
|[Indeksy unikatowe][]|
|[Kolumny obliczane][]|
|[Rzadkie kolumn][]|
|[Typy zdefiniowane przez użytkownika][]|
|[Sekwencja][]|
|[Wyzwalaczy][]|
|[Widoki indeksowane][]|
|[Synonimy][]|

## <a name="table-size-queries"></a>Kwerendy rozmiaru tabeli

Najprostszy sposób identyfikowania miejsca i wiersze zużywanej przez tabeli w każdej z 60 dystrybucji, jest używanie [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Jednak przy użyciu polecenia DBCC może być bardzo ograniczenie.  Dynamiczne zarządzanie widokami (DMVs) umożliwiają wyświetlić znacznie więcej szczegółów, a także umożliwiają znacznie większą kontrolę nad wyników kwerendy.  Rozpoczyna się od utworzenia widoku zostaną określone przez wiele nasze przykłady w tym i inne artykuły.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Podsumowanie obszaru tabel

Ta kwerenda zwraca wierszy i miejsca w tabeli.  Jest doskonałe kwerendę, aby wyświetlić tabel, które są największą tabel i czy są one okrężnego lub mieszania distributed.  Dla tabel mieszania distributed zawiera także kolumnę rozkładu.  W większości przypadków największą tabel należy mieszania distributed z indeksem columnstore grupowany.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Obszar tabel według typu dystrybucji

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Obszar tabel według typu indeksu

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Podsumowanie miejsca rozkładu

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz artykuły w [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks], [podziału tabeli][Partition], [Zachowywanie statystyki tabeli][Statystyki] i [Tabele tymczasowe][tymczasowe].  Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Omówienie]: ./sql-data-warehouse-tables-overview.md
[Typy danych]: ./sql-data-warehouse-tables-data-types.md
[Rozpowszechnianie]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[Najważniejsze wskazówki magazynu danych SQL]: ./sql-data-warehouse-best-practices.md
[Ładowanie danych za pomocą Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TWORZENIE TABELI]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Właściwość tożsamości]: https://msdn.microsoft.com/library/ms186775.aspx
[Przypisywanie obejście klucza zastępczego]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Powiązanych tabel]: https://msdn.microsoft.com/library/ms188066.aspx
[Kolumny obliczane]: https://msdn.microsoft.com/library/ms186241.aspx
[Rzadkie kolumn]: https://msdn.microsoft.com/library/cc280604.aspx
[Typy zdefiniowane przez użytkownika]: https://msdn.microsoft.com/library/ms131694.aspx
[Sekwencja]: https://msdn.microsoft.com/library/ff878091.aspx
[Wyzwalaczy]: https://msdn.microsoft.com/library/ms189799.aspx
[Widoki indeksowane]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonimy]: https://msdn.microsoft.com/library/ms177544.aspx
[Indeksy unikatowe]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
