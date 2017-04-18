<properties
   pageTitle="Podział tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do tabeli podziału w magazynie danych SQL Azure."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Tabele podziału w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Podział jest obsługiwany we wszystkich typach tabeli SQL Data Warehouse; w tym grupowane columnstore, indeks grupowany i stosu.  Podział obsługiwane jest też we wszystkich typach dystrybucji, w tym zarówno skrótu lub okrężnego distributed.  Podział umożliwia do dzielenia danych na mniejszych grupach danych i w większości przypadków podziału jest wykonywane w kolumnie Data.

## <a name="benefits-of-partitioning"></a>Zalety podziału

Podział mogą korzystać wydajności Konserwacja i kwerendy danych.  Czy korzyści obie lub tylko jedną jest uzależniony od sposobu załadowaniu danych i czy tej samej kolumnie mogą być używane do obu celów, ponieważ podziału jest możliwe tylko w jednej kolumnie.

### <a name="benefits-to-loads"></a>Korzyści na obciążenia

Główną zaletą podziału w magazynie danych SQL jest poprawić efektywność i wydajność ładowania używanych danych przez usunięcia partition, przenoszenie i scalanie.  W większości przypadków danych jest podzielona według kolumny daty, która jest ściśle związany sekwencji, które załadowaniu danych do bazy danych.  Jedną z zalet największe sposób przechowywania danych za pomocą partycje go unikanie rejestrowania transakcji.  Po prostu Wstawianie, aktualizowanie lub usuwanie danych mogą być rozwiązaniem najbardziej proste, z nieco myśli i nakładu, za pomocą podziału podczas procesu ładowania znacznie zwiększyć wydajność.

Przełączanie partition można szybko usunąć lub zamienianie części tabeli.  Na przykład tabeli faktów sprzedaży może zawierać tylko dane przez okres 36 miesięcy.  Na końcu każdego miesiąca najstarszych miesiąc dane dotyczące sprzedaży zostanie usunięty z tabeli.  Dane można usunąć za pomocą instrukcja delete usunąć dane dla najstarszych miesiąca.  Jednak usuwanie dużej ilości danych wiersz po wierszu z instrukcja delete może trwać bardzo długo, a także tworzyć ryzyka duże transakcje, które może zająć dużo czasu wycofać, jeśli wystąpią problemy.  Bardziej odpowiednim rozwiązaniem jest po prostu zlikwiduj partycją najstarszych danych.  Miejsce, w którym usuwanie pojedynczych wierszy może zająć godzin, usunięcie całego partition może minąć sekund.

### <a name="benefits-to-queries"></a>Zalety do kwerend

Podział również można zwiększyć wydajność kwerendy.  Jeśli kwerenda zastosowanie filtru kolumny podzielonej, to ograniczyć skanowania tylko uprawniający partycje, które mogą być dużo mniejsze podzestawu danych, można uniknąć skanowania pełnego tabeli.  Wraz z wprowadzeniem indeksy grupowany columnstore korzyści wydajności predykatu usunięcia są mniej korzystne, ale w niektórych przypadkach może być korzyści kwerendy.  Na przykład tabeli faktów sprzedaży jest podzielone na 36 miesięcy przy użyciu pola Data sprzedaży, a następnie kwerendy ten filtr z datą sprzedaży można pominąć wyszukiwanie w partycje, które nie są zgodne filtr.

## <a name="partition-sizing-guidance"></a>Wskazówki dotyczące zmiany rozmiaru partition

Chociaż partycje mogą być używane w celu zwiększenia wydajności kilka scenariuszy, tworząc tabelę przy użyciu partycje **za dużo** może obniżyć wydajność w niektórych okolicznościach.  Tych problemów są szczególnie dla tabel columnstore grupowany.  Dzielony, aby być pomocne, należy pamiętać, kiedy należy używać podziału i liczba partycje, aby utworzyć.  Istnieje nie twardym szybkie reguła, ile partycje są zbyt dużą liczbą, to zależy od rodzaju danych i ile partycje są ładowane do jednocześnie.  Ale jako ogólne zasada, można traktować Dodawanie 10s do 100s partycje, nie 1000s.

Podczas tworzenia podziału w tabelach **columnstore grupowane** , jest należy rozważyć, ile wierszy zostanie ziemi w każdym partycją.  Optymalnej kompresji i wydajność tabel columnstore grupowane co najmniej 1 miliona wierszy na dystrybucji i partycją jest potrzebny.  Przed utworzeniem partycje magazynu danych SQL już dzieli każdej tabeli do 60 rozłożone baz danych.  Wszystkie partycje dodane do tabeli jest oprócz dystrybucji utworzone w tle.  Przy użyciu w tym przykładzie, jeśli w tabeli faktów sprzedaży zawarte 36 partycje miesięczny i przyjmując, że program SQL Data Warehouse ma rozkład 60, następnie tabeli faktów sprzedaży powinien zawierać 60 milionów wierszy na miesiąc lub miliardów 2.1 wiersze po wszystkich miesięcy są wypełnione.  Jeśli tabela zawiera wiersze znacznie mniej niż zalecane minimalna liczba wierszy na partition, warto rozważyć użycie mniej partycje, aby zwiększyć liczbę wierszy na partycją.  Zobacz też [indeksowania]artykuł[indeks] , który zawiera kwerendy, które mogą być uruchamiane w magazynie danych SQL do oceny jakości klaster columnstore indeksy.

## <a name="syntax-difference-from-sql-server"></a>Różnica składni z programu SQL Server

Program SQL Data Warehouse wprowadzono uproszczone definicji partycje różni się nieco z programu SQL Server.  Podziału funkcje i schematów nie są używane w magazynie danych SQL, jak w programie SQL Server.  Zamiast tego jest wszystko, co należy zrobić, określ kolumny podzielonej i punktów obramowanie.  Podstawowe pojęcia składnia podziału może się nieco różnić z programu SQL Server, są takie same.  Jedna kolumna partition w tabeli, która może być w granicach partition pomocy technicznej programu SQL Server i magazynu danych SQL.  Aby dowiedzieć się więcej na temat podziału, zobacz [tabele na partycje i indeksy][].

Poniższym przykładzie instrukcję SQL Data Warehouse partycją [Tworzenie tabeli][] partycje tabeli FactInternetSales w kolumnie OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrowanie podziału z programu SQL Server

Aby przeprowadzić migrację definicje partition programu SQL Server do magazynu danych SQL po prostu:

- Usuń programu SQL Server [partycją schematu][].
- Dodawanie definicji [Funkcja partition][] do tworzenia tabeli.

Jeśli podczas migrowania tabeli podzielonej na partycje z wystąpieniem programu SQL Server poniżej SQL może ułatwić interrogate liczbę wierszy, które znajdują się w każdej partycją.  Należy pamiętać, że użycie samego szczegółowości podziału w magazynie danych SQL liczbę wierszy na partition zmniejszy przez współczynnik równy 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Zarządzanie pracą

Jeden element końcowy uwzględnienie współczynnik do decyzji partition tabeli jest [Zarządzanie pracą][].  Zarządzanie pracą w magazynie danych SQL jest przede wszystkim zarządzania pamięci i współbieżności.  W magazynie danych SQL klas której działalność zasobów jest Maksymalna pamięć przydzielone do każdej dystrybucji podczas wykonywania kwerend.  Najlepiej będzie o rozmiarze do partycje, biorąc pod uwagę innych czynników, takich jak tworzenie indeksów columnstore grupowany na potrzeby pamięci.  Wykres kolumnowy grupowany korzyści indeksy columnstore znacznie po są przydzielone więcej pamięci.  W związku z tym należy upewnić się, że odbudowywania indeksu partition nie jest zagłodzone pamięci. Zwiększenie ilości pamięci do kwerendy można uzyskać, możesz je przełączyć z roli domyślne smallrc, jedna z innych ról, takich jak largerc.

Informacje o alokacji pamięci dla rozkładu są dostępne za pomocą kwerend wysyłanych widoki dynamiczne zarządzanie Gubernator zasobów. W rzeczywistości usługi udzielanie pamięci wynosi mniej niż poniższej ilustracji. Jednak zapewnia poziom wskazówek, który można używać podczas zmiany rozmiaru z partycje operacji zarządzania danymi.  Należy unikać zmiany rozmiaru z partycje poza udzielanie pamięci udostępnianym przez klasę bardzo duże zasobów. Jeśli do partycje rosnąć poza na poniższym rysunku narażenie się na ryzyko ciśnienia pamięci, która z kolei prowadzi do mniej optymalnej kompresji.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Przełączanie partition

Program SQL Data Warehouse obsługuje partition dzielenia, łączenia i przełączania. Każda z tych funkcji jest excuted za pomocą instrukcja [ALTER TABLE][] .

Aby przełączyć partycje między dwiema tabelami, należy się upewnić, że partycje wyrównanie w ich granicach odpowiednich i zgodność definicji tabel. Ograniczenia check nie są dostępne dla Wymuszaj zakres wartości w tabeli z tabeli źródłowej musi zawierać samej ograniczenia partycją jako tabeli docelowej. Jeśli nie jest wielkość liter, a następnie przełącz partition zakończy się niepowodzeniem, jak metadanych partition nie zostaną zsynchronizowane.

### <a name="how-to-split-a-partition-that-contains-data"></a>Sposób podziału partycją, która zawiera dane

Najskuteczniejszą metodą dzielenie partition, która już zawiera dane jest użycie `CTAS` instrukcji. W przypadku tabeli podzielonej na partycje grupowany columnstore to partycją tabeli musi być puste przed można podzielić.

Poniżej znajduje się tabela podzielone na partycje columnstore próbki, zawierająca jeden wiersz w każdej partition:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Tworząc obiekt statystyczny, firma Microsoft upewnij się, że metadane tabeli jest bardziej odpowiednia. Firma Microsoft pominięcia tworzenia statystyki, magazynu danych SQL użyje wartości domyślne. Aby uzyskać szczegółowe informacje dotyczące statystyk Przejrzyj [Statystyki][].

Firma Microsoft może następnie wyszukiwać przy użyciu liczba wierszy `sys.partitions` widoku katalogu:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Jeśli próbie dzielenie w tej tabeli, firma Microsoft będzie wyświetlany komunikat o błędzie:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, 15 poziom stan 1 wiersz PODZIELONY 44 klauzuli instrukcja ALTER PARTITION nie powiodło się, ponieważ nie jest pusta.  Tylko puste partycje można podzielić w, gdy istnieje indeks columnstore w tabeli. Rozważ wyłączenie indeksu columnstore przed wydaniem instrukcja ALTER PARTITION, a następnie odbudowanie indeksu columnstore po ukończeniu ALTER PARTITION.

Jednak firma Microsoft może zawierać `CTAS` Aby utworzyć nową tabelę do przechowywania naszych danych.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Jak ograniczenia partition są wyrównane przełącznika jest dozwolone. Spowoduje to pozostawienie tabeli źródłowej z partycją puste, który firma Microsoft może następnie dzielić.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Wszystkie opcje, które jest pozostałej do wykonania jest wyrównanie naszych danych do nowego ograniczenia partition przy użyciu `CTAS` i ponownie naszych danych do tabeli głównej

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Po zakończeniu przepływu danych jest dobrym pomysłem jest odświeżanie statystyki w tabeli docelowej, aby upewnić się, że dokładnie odzwierciedlać nowy rozkład danych w ich odpowiednich partycje:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabela podziału kontrolki źródła

Aby uniknąć do definicji tabeli z **uszkodzona** w źródłowym systemie sterowania można brać pod uwagę następujące podejście:

1. Tworzenie tabeli jako tabeli podzielonej na partycje, ale bez wartości partition

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`Tabela w ramach procesu wdrażania:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Z tej metody pozostaje statyczna kod w formancie źródła i podziału wartości granicznych mogą być dynamiczne; rozwijających się z magazynu w czasie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz artykuły w [Tabeli omówienie][Omówienie], [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks], [Zachowaniu statystyki tabeli][Statystyki] i [Tabele tymczasowe][tymczasowe].  Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Omówienie]: ./sql-data-warehouse-tables-overview.md
[Typy danych]: ./sql-data-warehouse-tables-data-types.md
[Rozpowszechnianie]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[Zarządzanie pracą]: ./sql-data-warehouse-develop-concurrency.md
[Najważniejsze wskazówki magazynu danych SQL]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Tabele podzielone na partycje i indeksy]: https://msdn.microsoft.com/library/ms190787.aspx
[INSTRUKCJA ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TWORZENIE TABELI]: https://msdn.microsoft.com/library/mt203953.aspx
[Funkcja Partition]: https://msdn.microsoft.com/library/ms187802.aspx
[Schemat partition]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
