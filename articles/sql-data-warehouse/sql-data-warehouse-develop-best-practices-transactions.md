<properties
   pageTitle="Optymalizacja transakcji dla magazynu danych SQL | Microsoft Azure"
   description="Najlepsze praktyki wskazówki dotyczące pisania aktualizacje wydajność transakcji w magazynie danych SQL Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optymalizacja transakcji dla magazynu danych SQL

W tym artykule omówiono sposób optymalizacji wydajności transakcji kodu minimalizując ryzyko dla długich wycofywania.

## <a name="transactions-and-logging"></a>Rejestrowanie i transakcje

Transakcje są ważne część aparat relacyjnej bazy danych. Program SQL Data Warehouse transakcje jest używany podczas modyfikacji danych. Transakcje te mogą być jawne lub niejawne. Pojedynczy `INSERT`, `UPDATE` i `DELETE` instrukcje są we wszystkich przykładach niejawnej transakcji. Jawne transakcje są wyraźnie napisanego przez dewelopera przy użyciu `BEGIN TRAN`, `COMMIT TRAN` lub `ROLLBACK TRAN` i są zazwyczaj używane do wielu instrukcji modyfikacji muszą być powiązane razem w jednej jednostki Atomowej. 

Magazyn danych SQL Azure zatwierdza zmiany do bazy danych za pomocą dzienników transakcji. Każdy rozkład ma własny dziennik transakcji. Zapisuje dziennik transakcji są automatyczne. Nie jest wymagana żadna konfiguracja. Jednak podczas tego procesu gwarantuje zapisu dostosowanie wprowadza ogólnych w systemie. Wpływ ten można zminimalizować, pisanie kodu transakcyjnie wydajność. Kod transakcyjnie wydajność ogólnie przypada na dwie kategorie.

- Wykorzystać konstrukcji minimalne rejestrowanie, jeśli to możliwe
- Proces danych przy użyciu występujące partie, aby uniknąć pojedynczej długotrwałych transakcje
- Przyjęcie partycją przełączania wzorca dużych zmian w danym partycją

## <a name="minimal-vs-full-logging"></a>Minimalne a pełne rejestrowanie

W przeciwieństwie do w pełni zarejestrowane działań, które użyć dziennik transakcji do śledzenia każdej zmianie wiersza, minimalny zarejestrowane operacje aktualizowanie informacji o zakresie przydziałów i tylko zmiany metadane. W związku z tym, minimalne rejestrowanie obejmuje rejestrowania tylko informacje, które są wymagane do wycofywania transakcji w przypadku awarii lub jawne żądanie (`ROLLBACK TRAN`). Jak znacznie mniej informacje są śledzone w dzienniku transakcji, operacji minimalny zarejestrowane wykonuje się lepiej niż operacji w pełni zarejestrowane o podobnym rozmiarze. Ponadto ponieważ zapisy mniejszej liczby przejść dziennik transakcji, ilości mniejszej ilości danych dziennika jest generowany a więc więcej we/wy wydajność.

Limity dotyczące bezpieczeństwa transakcji dotyczą tylko operacje w pełni zarejestrowane.

>[AZURE.NOTE] Minimalny zarejestrowane operacje mogą uczestniczyć w jawne transakcje. Jak wszystkie zmiany w strukturze alokacji są śledzone, prawdopodobnie wycofać operacje minimalny zarejestrowane. Należy pamiętać, że zmiana jest "" rejestrowane nie jest usunięcie zarejestrowane.

## <a name="minimally-logged-operations"></a>Operacje minimalny zarejestrowane

Obsługuje są rejestrowane są następujące operacje:

- Tworzenie tabeli jako wybierz pozycję ([CTAS][])
- WSTAWIANIE. WYBIERZ POZYCJĘ
- TWORZENIE INDEKSU
- INSTRUKCJA ALTER ODBUDOWYWANIA INDEKSU
- USUWANIE INDEKSU
- OBCINANIE TABELI
- USUWANIE TABELI
- INSTRUKCJA ALTER TABELI PRZEŁĄCZ PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Operacje przepływu danych wewnętrznych (takie jak `BROADCAST` i `SHUFFLE`) nie dotyczy limit bezpieczeństwo transakcji.

## <a name="minimal-logging-with-bulk-load"></a>Minimalne rejestrowanie z ładowanie zbiorcze

`CTAS`i `INSERT...SELECT` zarówno zbiorczo operacji ładowania. Jednak zarówno są zależne od definicji tabeli docelowej i zależą od tego scenariusza ładowania. Poniżej przedstawiono tabelę, która w tym miejscu wyjaśniono, jeśli operacja zbiorcze zostanie całkowicie lub minimalny zarejestrowane:  

| Indeks główny               | Scenariusz ładowania                                            | Tryb rejestrowania |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Stos                        | Dowolny                                                      | **Minimalne**  |
| Indeks grupowany             | Tabela docelowa puste                                       | **Minimalne**  |
| Indeks grupowany             | Wiersze załadowanego nie pokrywają się z istniejących stron w docelowej | **Minimalne**  |
| Indeks grupowany             | Nakładanie załadowanego wierszy z istniejących stron w docelowej        | Pełne         |
| Indeks Columnstore grupowany | Rozmiar partii > = 102400 na partition wyrównana do dystrybucji | **Minimalne**  |
| Indeks Columnstore grupowany | Partii rozmiar < 102400 na partition wyrównana do dystrybucji  | Pełne         |

Warto zauważyć, wszelkie zapisy zaktualizować pomocniczą lub bez klastrów indeksów będzie zawsze operacje w pełni zarejestrowane.

> [AZURE.IMPORTANT] Program SQL Data Warehouse ma rozkład 60. W związku z tym, przy założeniu wszystkie wiersze są równomiernie i wyładunku w jedną partycją, usługi partii muszą zawierać wiersze 6,144,000 lub większe zalogowania się minimalny podczas zapisywania indeks Columnstore wykres kolumnowy grupowany. Jeśli tabela jest podzielona i partycją ograniczenia zakresu wierszy zostanie wstawiony, jest potrzebna 6,144,000 wierszy na krawędzi partition przy założeniu Rozdziel danych. Każdy partition w każdym dystrybucji niezależnie przekraczać progu 102 400 wiersza do wstawiania minimalny zalogowania się do dystrybucji.

Ładowanie danych do tabeli niepustych z indeksem grupowany często może zawierać z różnych wierszy w pełni zarejestrowane i minimalny zarejestrowane. Grupowany indeks jest strategii drzewa (b drzewa) stron. Jeśli na stronie zapisywane już zawiera wiersze z innej transakcji, następnie zapisuje te będą w pełni rejestrowane. Jednak jeśli strony jest pusta następnie zapisu do tej strony zostanie minimalny zarejestrowane.

## <a name="optimizing-deletes"></a>Optymalizacja usuwa

`DELETE`jest w pełni zarejestrowane operacji.  Jeśli chcesz usunąć dużej ilości danych w tabeli lub partycją często warto więcej `SELECT` chcesz zachować, dane, które mogą być uruchamiane jako operacji minimalny zarejestrowane.  Aby to zrobić, należy utworzyć nową tabelę z [CTAS][].  Po utworzeniu umożliwia [Zmienianie nazwy][] zamieniać starej tabeli z nowo utworzona tabela.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optymalizacja aktualizacji

`UPDATE`jest w pełni zarejestrowane operacji.  Jeśli musisz zaktualizować dużej liczby wierszy w tabeli lub partycją często może być znacznie bardziej efektywne, użyj operacji minimalny zarejestrowane, takich jak [CTAS][] to zrobić.

W przykładzie poniżej tabeli Pełna aktualizacja przekonwertowany na `CTAS` tak, aby minimalne rejestrowanie jest możliwe.

W tym przypadku możemy retrospektywnie dodajesz kwota rabatu sprzedaży w tabeli:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Ponowne tworzenie dużych tabel można korzystać z funkcji zarządzania pracą magazynem danych SQL. Więcej informacji można znaleźć w sekcji Zarządzanie obciążenie pracą w artykule [współbieżności][] .

## <a name="optimizing-with-partition-switching"></a>Optymalizacja z przełączanie partition

W przypadku zastosowania modyfikacje dużą skalę wewnątrz [tabeli partition][], partycją przełączanie wzór stanowi wiele właściwe rozwiązanie. Jeśli modyfikacji danych ma znaczenie i obejmuje wiele partycje, następnie po prostu Iterowanie nad partycje uzyskuje ten sam wynik.

Czynności do wykonania przełącznika partition są następujące:
1. Tworzenie pustego się partition
2. Pełnienie CTAS tej aktualizacji
3. Przełączanie się z istniejących danych do tabeli out
4. Przełączanie w nowych danych
5. Czyszczenie danych

Jednak do identyfikowania partycje, aby przełączyć możemy najpierw należy utworzyć procedurę Pomocnik, taki jak przedstawiony poniżej. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Ta procedura maksymalizuje kod ponownego użycia i zachowuje partycją przełączanie przykład bardziej zwarty.

Kod poniżej przedstawiono pięć kroków wymienionych powyżej do uzyskania pełnego partition, przełączanie procedury.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Minimalizowanie rejestrowania z małych partii

W przypadku operacji modyfikacji danych może być zrozumiałe podziału operacji na fragmenty lub partie do ograniczania zakresu jednostka pracy.

Poniżej znajduje się przykład pracy. Rozmiar partii ustawiono uproszczony liczby do wyróżnienia techniki. W rzeczywistości rozmiar partii może być znacznie większa. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Wstrzymaj i skalowanie wskazówki

Magazyn danych SQL Azure pozwala wstrzymać, wznowić i skalowanie magazynu danych na żądanie. Po wstrzymaniu lub przeskalować magazynu danych SQL ważne jest, aby zrozumieć natychmiast; zakończenia wszelkich lotu transakcji powoduje cofnięcie transakcji otwartych. Jeśli z pracą zostały wydane długo uruchomiony i niekompletna modyfikacji danych przed rozpoczęciem operacji Wstrzymaj lub Skala, następnie prac będzie konieczne można cofnąć. To może mieć wpływ na czas potrzebny na wstrzymać lub przeskalować bazy danych magazynu danych SQL Azure. 

> [AZURE.IMPORTANT] Obie `UPDATE` i `DELETE` są w pełni zarejestrowane operacji i tak tych cofanie i ponowne wykonywanie operacji można znacznie dłużej niż równowartość rejestrowane w operacji. 

Scenariusz najlepszego to aby umożliwić transakcji modyfikacji danych lotu wykonania przed wstrzymanie lub skalowania magazynu danych SQL. Jednak to mogą nie być praktycznych. Aby zmniejszyć ryzyko wycofywania długi, należy rozważyć jedną z następujących opcji:

- Ponowne używanie [CTAS][] długotrwałe operacje
- Podziel operację na fragmenty; działający na podzbiór wierszy

## <a name="next-steps"></a>Następne kroki

Wyświetlanie [transakcji w magazynie danych SQL][] , aby dowiedzieć się więcej na temat poziomów izolacji i ograniczenia transakcji.  Zawiera omówienie dotyczące innych najważniejszych wskazówek zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Transakcje w magazynie danych SQL]: ./sql-data-warehouse-develop-transactions.md
[Tabela partition]: ./sql-data-warehouse-tables-partition.md
[Współbieżności]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Najważniejsze wskazówki magazynu danych SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[ZMIENIANIE NAZWY]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

