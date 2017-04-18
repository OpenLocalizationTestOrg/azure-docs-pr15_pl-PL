<properties
   pageTitle="Tabele tymczasowe w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do tabel tymczasowych w magazynie danych SQL Azure."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Tabele tymczasowe w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Tabele tymczasowe są przydatne podczas przetwarzania danych — zwłaszcza podczas przekształcania umiejscowienia przejściowych pośrednie wyniki. W magazynie danych SQL tabele tymczasowe istnieją na poziomie sesji.  Tylko będą one widoczne do sesji, w której zostały utworzone i są automatycznie usuwane po wylogowaniu tej sesji.  Tabele tymczasowe oferty poprawiać wydajność, ponieważ ich wyniki są zapisywane w lokalnym zamiast Magazyn zdalny.  Tabele tymczasowe są nieco inne w magazynie danych SQL Azure niż bazy danych SQL Azure, jak są one dostępne z dowolnego miejsca wewnątrz sesji, w tym zarówno i spoza niej procedura składowana.

Ten artykuł zawiera podstawowe wskazówki dotyczące korzystania z tabel tymczasowych i wyróżnienie zasady poziomu tabele tymczasowe sesji. Korzystając z informacji w tym artykule może ułatwić modularize kodzie poprawy zarówno ponownego użycia i ułatwienia konserwacji kodu.

## <a name="create-a-temporary-table"></a>Utwórz tabelę tymczasową

Tabele tymczasowe są tworzone po prostu dodając nazwę tabeli z `#`.  Na przykład:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Można także tworzyć tabele tymczasowe z `CTAS` za pomocą dokładnie to samo podejście:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`jest to polecenie wydajne i ma dodatkową zaletą jest bardzo wydajny przy jego użyciu miejsca w dzienniku transakcji. 


## <a name="dropping-temporary-tables"></a>Usuwanie tymczasowych tabel

Po utworzeniu nowej sesji musi istnieć żadnych tymczasowych tabel.  Jednak jeśli dzwonisz samej procedury składowanej powoduje tymczasowe z tą samą nazwą, upewnij się, że usługi `CREATE TABLE` instrukcje są pomyślnego prostego wyboru przed istnienia z `DROP` może być używany jako w poniższym przykładzie:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Kodowanie spójności, należy zaleca się używanie tego wzorca dla zarówno tabele i tabele tymczasowe.  Należy również zaleca się używanie `DROP TABLE` usunąć tabele tymczasowe po zakończeniu im w kodzie.  W procedurze przechowywanej rozwoju dość często, aby wyświetlić polecenia upuszczania powiązane ze sobą na końcu procedurę w celu zapewnienia tych obiektów są czyszczone.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Kod modularizing

Ponieważ tabele tymczasowe są widoczne dowolne miejsce w sesji użytkownika, to można wykorzystać ułatwiające modularize kodzie aplikacji.  Na przykład poniższa procedura składowana zgromadzono najważniejsze wskazówki, od góry do generowania DDL, które zaktualizuje wszystkie statystyki w bazie danych według nazwy statystyczny.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Na tym etapie, które tylko akcję, która wystąpił jest tworzenie procedury składowanej, który po prostu wygenerowanie tabeli tymczasowej stats_ddl #, z instrukcji DDL.  Ta procedura przechowywana powoduje usunięcie #stats_ddl, jeśli już istnieje, aby upewnić się, że nie zostanie w przypadku więcej niż jeden raz w sesji.  Jednak ponieważ istnieje nie `DROP TABLE` na końcu procedury składowanej po zakończeniu działania procedury składowanej pozostawia utworzonej tabeli tak, aby można ją odczytać poza procedury składowanej.  W magazynie danych SQL w przeciwieństwie do innych baz danych programu SQL Server, jest możliwe za pomocą tabeli tymczasowej poza procedury, w którym go utworzono.  Tabele tymczasowe magazynu danych SQL może być używane **miejsca** wewnątrz sesji. To może prowadzić do więcej kodu moduły i zarządzaniu, jak w poniższym przykładzie:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Ograniczenia w tabeli tymczasowej

Magazyn danych SQL nałożyć kilka ograniczeń, podczas wykonywania tabele tymczasowe.  Obecnie tylko sesji zakresu tabele tymczasowe są obsługiwane.  Globalne tabele tymczasowe nie są obsługiwane.  Ponadto widoków nie można utworzyć na tabelach tymczasowych.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej, zobacz artykuły [Omówienie tabeli][Omówienie], [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks], [podziału tabeli][Partition] i [Zachowywanie statystyki tabeli][Statystyki].  Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących zobacz [Najważniejsze wskazówki magazynu danych SQL][].

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

<!--MSDN references-->

<!--Other Web references-->
