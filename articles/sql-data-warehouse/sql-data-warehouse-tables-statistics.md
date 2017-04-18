<properties
   pageTitle="Zarządzanie statystyk dotyczących tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do programu statystyk dotyczących tabel w magazynie danych SQL Azure."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Zarządzanie statystyk dotyczących tabel w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Im więcej magazynu danych SQL zna danych, tym szybciej można wykonywać kwerend dla danych.  Metodą informowanie magazynu danych SQL o Twoich danych jest zbierania statystyk danych.  O statystyki danych jest jednym z najważniejszych czynności, które można zrobić, aby zoptymalizować zapytań.  Statystyki pomoc magazynu danych SQL utworzyć najbardziej optymalny plan zapytań.  Jest to spowodowane Optymalizator kwerend SQL Data Warehouse jest optymalizatora na podstawie.  Oznacza to, że porównanie kosztów różnych planów kwerend i użytkownik wybiera plan z najniższych kosztów należy również plan, który będzie wykonywać najszybszy.

Statystyki można tworzyć w pojedynczej kolumnie, wiele kolumn lub na indeks tabeli.  Statystyki są przechowywane w postaci histogramu, która obejmuje zakres i selektywność wartości.  To jest znaczący, gdy Optymalizator należy ocenić sprzężenia, GROUP BY, HAVING i klauzul WHERE w kwerendzie.  Na przykład jeśli Optymalizator szacuje, że data filtrujesz w kwerendzie zwróci wiersz 1, może wybrać bardzo innego planu niż w przypadku jego szacuje, że ich daty można wybrano będzie zwracana 1 miliona wierszy.  Podczas tworzenia statystyk jest bardzo ważne, jest równie ważne, tym statystyki *dokładnie* odzwierciedlał bieżącego stanu tabeli.  Aktualne statystyki gwarantuje, że dobry plan jest zaznaczona opcja przez optymalizator.  Plany utworzone przez optymalizator są tylko jak dobrą statystyki dotyczące danych.

Proces tworzenia i aktualizowania statystyki jest obecnie proces ręczny, ale jest bardzo proste zrobić.  Jest to inaczej niż w przypadku programu SQL Server, który automatycznie tworzy i aktualizuje statystyki dotyczące pojedynczej kolumny i indeksy.  Korzystając z poniższych informacji, można znacznie zautomatyzować zarządzanie statystyki na danych. 

## <a name="getting-started-with-statistics"></a>Wprowadzenie do statystyki

 Tworzenie próbki statystyki dla każdej kolumny jest łatwym sposobem wprowadzenie statystyki.  Ponieważ jest równie ważne zapewnić aktualność statystyki, tradycyjne podejście może być aktualizowanie statystyk codziennie lub po każdym ładowania. Są zawsze korzystnych rozwiązań między wydajności i kosztów do tworzenia i aktualizowania statystyki.  Jeśli okaże się, że trwa zbyt długo, aby zachować wszystkie statystyk, warto wypróbować mają być dotyczące kolumny, które mają statystyki lub kolumny, które wymagają częstego aktualizowania.  Na przykład można zaktualizować kolumn daty codziennie, jak można dodać nowe wartości, a nie po każdej ładowania. Ponownie, możesz uzyskać najlepiej wykorzystać przez statystyki na kolumnami w sprzężeniach GROUP BY, HAVING i klauzul WHERE.  Jeśli masz tabelę zawierającą wiele kolumn, które można stosować tylko w klauzuli SELECT statystyk dotyczących tych kolumn nie na powyższej liście, a wydatków nieco więcej starań, aby zidentyfikować kolumny, gdzie pomoże statystyki umożliwia skrócenie czasu, aby zachować statystyk.

## <a name="multi-column-statistics"></a>Statystyki wielu kolumn

Oprócz tworzenia statystyki w pojedynczej kolumny, może się okazać, że kwerendy będą korzystać z wieloma kolumnami statystyki.  Statystyki tworzone na liście kolumn są statystyki wielokolumnowego.  Uwzględnić statystyki pojedynczej kolumny w pierwszej kolumnie na liście oraz niektóre informacje korelacji między kolumny o nazwie gęstości.  Na przykład jeśli masz tabelę, która łączy do innego na dwóch kolumn, może się okazać, że magazynu danych SQL może lepiej Optymalizowanie planu, jeśli siebie relacji między dwiema kolumnami.   Statystyki wielokolumnowego można zwiększyć wydajność kwerendy dla niektórych operacji, takich jak złożone sprzężenia i Grupuj według.

## <a name="updating-statistics"></a>Aktualizowanie statystyki

Aktualizowanie statystyki jest ważną częścią procedury zarządzania z bazy danych.  Po zmianie rozkład danych w bazie danych statystyki należy zaktualizować.  Nieaktualne statystyki będzie powodowało częściowej optymalnego działania zapytań.

Jeden najlepszym rozwiązaniem jest aktualizację statystyk dotyczących kolumn daty każdego dnia, po dodaniu nowych dat.  Nowe wiersze każdej godziny są ładowane do magazynu danych, nowe daty ładowania lub dat transakcji są dodawane. Te zmiany rozkładu danych w celu statystyki nieaktualne. I odwrotnie statystyki kolumny Kraj w tabeli odbiorcy nigdy nie trzeba będzie aktualizowany zgodnie z rozkładem wartości zazwyczaj nie powoduje zmiany. Zakładając, że rozkład jest stała między klientami, dodawanie nowych wierszy do odmian tabeli nie jest zamiar zmienić rozkładu danych. Jednak jeśli magazynu danych zawiera tylko jeden kraj i udostępniania danych z nowego kraju, uzyskując danych z wielu krajów przechowywane, która z pewnością należy aktualizację statystyk dotyczących kolumny Kraj.

Jedną z pierwszej pytań podczas rozwiązywania problemów z kwerendy jest "Są aktualne statystyki?"

To pytanie nie jest takie, które należy odpowiedzieć według wieku danych. Obiekt na bieżąco statystyki może być bardzo starych, jeśli została brak materiałów zmian w danych źródłowych. Liczba wierszy zmienił się znacznie lub materiałowego zmiany w rozkład wartości dla określonej kolumny *następnie* go zawiera czas aktualizacji statystyk.  

Odwołanie, **SQL Server** (nie magazyn danych SQL) automatycznie aktualizuje statystyki w następujących sytuacjach:

- Jeśli masz zerowej liczby wierszy w tabeli, podczas dodawania wierszy, zostanie wyświetlony automatyczna aktualizacja statystyki
- Po dodaniu więcej niż 500 wierszy do tabeli, zaczynając od mniej niż 500 wierszy (np. na początku masz 499 i następnie dodać 500 wierszy może zawierać maksymalnie 999 wierszy), otrzymasz aktualizacji automatycznej 
- Po otwarciu ponad 500 wierszy, musisz dodać 500 dodatkowe wiersze + 20% rozmiaru tabeli przed automatyczna aktualizacja widoczne na statystykę

Ponieważ nie DMV ustalenie, jeśli dane w tabeli zmienił się od czasu ostatniego statystyki czasu zostały zaktualizowane, informacje o wieku statystyk może dostarczyć części obrazu.  Można użyć następującej kwerendy do określenia ostatniego statystyk w przypadku, gdy aktualizacja dla każdej tabeli.  

> [AZURE.NOTE] Pamiętaj, że w przypadku istotnej zmiany w dystrybucji wartości dla danej kolumny, należy zaktualizować statystykę niezależnie od czasu ostatniego zostały one zaktualizowane.  

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Kolumn daty w magazynie danych, na przykład, zazwyczaj trzeba często aktualizacji statystyki. Nowe wiersze każdej godziny są ładowane do magazynu danych, nowe daty ładowania lub dat transakcji są dodawane. Te zmiany rozkładu danych w celu statystyki nieaktualne.  I odwrotnie statystyki w kolumnie płeć na tabeli odbiorcy nigdy nie trzeba będzie aktualizowany. Zakładając, że rozkład jest stała między klientami, dodawanie nowych wierszy do odmian tabeli nie jest zamiar zmienić rozkładu danych. Jednak jeśli magazyn danych zawiera tylko jedną płeć i nowe wyniki wymagane w wielu określoną płeć ostatecznie należy zaktualizować statystykę w kolumnie płeć.

Aby jeszcze bardziej informacje zobacz [statystykę][] w witrynie MSDN.

## <a name="implementing-statistics-management"></a>Implementacji statystyki zarządzania

Często jest dobrym pomysłem jest rozszerzanie danych proces, aby upewnić się, że statystyki są aktualizowane na końcu Załaduj ładowania. Ładowania danych jest w przypadku tabel najczęściej zmienić ich rozmiar i/lub ich rozkład wartości. Dlatego jest logiczna miejsce do wprowadzenia w życie niektórych procesów zarządzania.

Niektóre wytyczne przedstawiono poniżej w celu zaktualizowania statystyk podczas ładowania:

- Upewnij się, że każda tabela załadowanego zawiera co najmniej jeden obiekt statystyki zaktualizowane. Informacje o rozmiarze (liczba wierszy i liczba stron) tabel w ramach aktualizacji statystykę to aktualizacji.
- Skup się na kolumny wchodzące w klauzule sprzężenia, GROUP BY, ORDER BY i DISTINCT
- Należy rozważyć aktualizowanie "rosnąco klucza" kolumn, takie jak transakcji więcej często te wartości nie zostaną uwzględnione na histogramie statystyki daty.
- Należy rozważyć, często aktualizowanie kolumn statyczne rozkładu.
- Należy pamiętać, że każdy obiekt statystyczny jest aktualizowana w serii. Po prostu implementacji `UPDATE STATISTICS <TABLE_NAME>` może nie być idealne rozwiązanie — zwłaszcza dla wielu tabel z wieloma obiektami statystyki.

> [AZURE.NOTE] Więcej informacji na temat [rosnąco klucza] można znaleźć dokument modelu programu SQL Server 2014 kardynalności oszacowanie.

Aby jeszcze bardziej informacje zobacz [Oszacowania kardynalności][] w witrynie MSDN.

## <a name="examples-create-statistics"></a>Przykłady: Tworzenie statystyk

W tych przykładach przedstawiono sposoby używania różnych opcji do tworzenia statystyk. Opcje, których używasz dla każdej kolumny, zależy od właściwości danych i używania kolumny w kwerendach.

### <a name="a-create-single-column-statistics-with-default-options"></a>ODPOWIEDZI. Tworzenie statystyki jednokolumnowego przy użyciu opcji domyślnych

Aby utworzyć statystyki dla kolumny, po prostu Podaj nazwę obiektu statystyki i nazwę kolumny.

Tej składni używa wszystkich opcji domyślnych. Domyślnie program SQL Data Warehouse próbki 20 procent tabeli podczas tworzenia statystyki.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Na przykład:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Tworzenie statystyk jednokolumnowego sprawdzając każdy wiersz

Domyślna stawka przy próbkowaniu 20 procent wystarcza w większości sytuacji. Jednak można dostosować szybkość pobierania.

Aby przykładowe pełny tabeli, użyj następującej składni:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Na przykład:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Tworzenie statystyk jednokolumnowego, określając wielkością próbki

Ponadto można określić wielkością próbki jako procent:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Tworzenie statystyk jednokolumnowego tylko na niektórych wierszy

Inną opcją, możesz utworzyć statystyki na części wierszy w tabeli. Jest to statystyki filtrowanego.

Na przykład można filtrowane statystyki Planując kwerendy partycją określonej tabeli podzielonej na partycje duży. Tworząc statystyki na tylko wartości partycją, dokładność danych statystycznych będzie poprawić, a w związku z tym zwiększyć wydajność kwerendy.

W tym przykładzie tworzy statystyki w zakresie komórek. Aby pasuje do zakresu wartości w partycją łatwo należy zdefiniować wartości.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] Optymalizator kwerend brać pod uwagę przy użyciu filtrowane statystyki, gdy wybiera planu kwerend rozłożone kwerendy musi nadania definicji obiektu statystyki. W poprzednim przykładzie, kwerendy miejsce, w którym należy określić wartości Kol1 między 2000101 i 20001231 klauzuli.

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Tworzenie statystyk jednokolumnowego ze wszystkimi opcjami

Opcje można oczywiście łączyć ze sobą. W poniższym przykładzie tworzy obiekt filtrowane statystyki o rozmiarze próbki niestandardowy:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Aby uzyskać pełne odwołanie zobacz [Tworzenie statystyki][] w witrynie MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Tworzenie statystyk wielu kolumn

Aby utworzyć statystyki wielokolumnowego, po prostu za pomocą poprzednich przykładach jednak określić więcej kolumn.

> [AZURE.NOTE] Histogram, która jest używana do szacowania liczba wierszy w wyniku kwerendy, jest dostępna tylko dla pierwszej kolumny w definicji obiektu statystyki na liście.

W tym przykładzie histogram znajduje się na *produktu\_kategorii*. Statystyki granic kolumn są obliczane na *produktu\_kategorii* i *produktu\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Ponieważ istnieje związek między *produktu\_kategorii* i *produktu\_sub\_kategorii*, stan wielokolumnowego może być przydatne, jeśli tych kolumn są dostępne w tym samym czasie.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Tworzenie statystyk na wszystkich kolumn w tabeli

Jednym ze sposobów tworzenia statystyki ma problemy z polecenia Utwórz statystyki po utworzeniu tabeli.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H Tworzenie statystyk na wszystkich kolumn w bazie danych za pomocą procedury składowanej

Program SQL Data Warehouse nie ma systemowa procedura składowana równoważna [[sp_create_stats]] w programie SQL Server. Ta procedura składowana tworzy obiekt statystyki pojedynczej kolumny dla każdej kolumny bazy danych, który nie ma jeszcze statystyki.

To pomoże Ci rozpocząć pracę z projektu bazy danych. Zachęcamy dopasować go do własnych potrzeb.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Aby utworzyć statystyki na wszystkich kolumn w tabeli z tej procedury, wystarczy zadzwonić procedury.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Przykłady: Aktualizacja statystyki

Aby zaktualizować statystykę, można wykonywać następujące czynności:

1. Zaktualizuj jeden obiekt statystyki. Określ nazwę obiektu statystyki, który chcesz zaktualizować.
2. Zaktualizuj wszystkie obiekty statystyki w tabeli. Określ nazwę tabeli, a nie jednego obiektu poszczególnych statystyk.


### <a name="a-update-one-specific-statistics-object"></a>ODPOWIEDZI. Aktualizowanie jeden obiekt dane ###
Aby zaktualizować obiektu poszczególnych statystyk, należy użyć następującej składni:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Na przykład:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Aktualizując obiektów dane, możesz zminimalizować czas i zasoby wymagane do zarządzania statystyki. W tym celu rozwagą, jednak aby wybrać polecenie Najlepsze obiekty statystyki aktualizacji.


### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualizowanie wszystkich statystyk w tabeli ###
Spowoduje to wyświetlenie prostą metodę aktualizowania wszystkich obiektów w tabeli statystyki.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Na przykład:

```sql
UPDATE STATISTICS dbo.table1;
```

To wyrażenie jest łatwe w użyciu. Po prostu Pamiętaj to aktualizacje wszystkich statystyk w tabeli i w związku z tym może wykonać więcej pracy, niż jest to konieczne. Jeśli działanie nie jest problem, która z pewnością jest najłatwiejszym i najbardziej kompletne sposobem gwarantuje, że statystyki są aktualne.

> [AZURE.NOTE] Aktualizowanie wszystkich statystyk w tabeli, magazynu danych SQL wykonuje skanowania próby tabeli statystyki każdego. Jeśli tabela jest duży, ma wiele kolumn i wiele statystyki, może to być bardziej efektywne, aby zaktualizować statystyki poszczególnych zgodnie z potrzebami.

Do wykonania `UPDATE STATISTICS` procedury można znaleźć w artykule[tymczasowe] [Tabele tymczasowe]. Metody wdrażania różni się nieco do `CREATE STATISTICS` powyższej procedury, ale w wyniku jest taki sam.

Informacje na temat składni pełny zobaczyć [Statystykę aktualizacji][] w witrynie MSDN.

## <a name="statistics-metadata"></a>Statystyki metadanych
Istnieje kilka widoku systemu i funkcje, które umożliwia znajdowanie informacji na temat statystyk. Na przykład widać, jeśli obiekt statystyki może być nieaktualne przez wyświetlanie statystyki były ostatnio utworzona lub zaktualizowana za pomocą funkcji date Statystyka.

### <a name="catalog-views-for-statistics"></a>Widoków statystyki dla wykazu
Te widoki systemu zawierają informacje o statystyki:

| Widoku katalogu | Opis |
| :----------- | :---------- |
| [sys.Columns][]  | Jeden wiersz dla każdej kolumny. |
| [sys.Objects][]  | Jeden wiersz dla każdego obiektu w bazie danych. |  |
| [sys.schemas][]  | Jeden wiersz dla każdego schematu w bazie danych. |  |
| [sys.stats][] | Jeden wiersz dla każdego obiektu statystyki. |
| [sys.stats_columns][] | Jeden wiersz dla każdej kolumny w obiekcie statystyki. Łącza do sys.columns. |
| [sys.Tables][] | Jeden wiersz dla każdej tabeli (obejmuje tabele zewnętrzne). |
| [sys.table_types][] | Jeden wiersz dla każdego typu danych. |


### <a name="system-functions-for-statistics"></a>Funkcje systemu dla statystyki
Funkcje systemu są przydatne w przypadku pracy z statystyki:

| Funkcja systemu | Opis |
| :-------------- | :---------- |
| [STATS_DATE][]    | Data ostatniej aktualizacji obiektu statystyki. |
| [DBCC SHOW_STATISTICS][] | Zawiera podsumowanie poziomu i szczegółowe informacje na temat rozpowszechniania wartości jako zrozumiałe dla obiektu statystyki. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Łączenie kolumn statystyki i funkcje w jednym widoku

W tym widoku powoduje kolumn, które dotyczą statystyki, a wynikiem funkcji [] [STATS_DATE()] razem.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Przykłady DBCC SHOW_STATISTICS()

DBCC SHOW_STATISTICS() są wyświetlane dane przechowywane w obiekcie statystyki. Te dane składa się z trzech części.

1. Nagłówek
2. Gęstość wektorowa
3. Histogram

Metadane nagłówek temat statystyk. Histogram Wyświetla rozkład wartości w pierwszej kolumnie klucza obiektu statystyki. Wektorowa gęstości środków korelacji między kolumnami. SQLDW oblicza szacuje kardynalności z jakichkolwiek danych w obiekcie statystyki.

### <a name="show-header-density-and-histogram"></a>Pokaż nagłówek, gęstości i histogramu

Ten prosty przykład zawiera trzy części obiektu statystyki.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Na przykład:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Pokazywanie jednej lub kilku części DBCC SHOW_STATISTICS();

Jeśli interesuje Cię tylko wyświetlanie określonych części, za pomocą `WITH` klauzula i określ części, które mają być wyświetlane:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Na przykład:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>Różnice DBCC SHOW_STATISTICS()
DBCC SHOW_STATISTICS() więcej ściśle jest zaimplementowana w magazynie danych SQL w porównaniu z programem SQL Server.

1. Nieudokumentowanych funkcje nie są obsługiwane.
- Nie można używać Stats_stream
- Nie mogę dołączyć do wyników dla określonych podzbiorów danych statystyki np. (STAT_HEADER sprzężenie DENSITY_VECTOR)
2. Nie można ustawić NO_INFOMSGS dla zwalczaniu wiadomości
3. Nawiasy kwadratowe statystyki nazwy nie mogą być używane.
4. Nie można użyć nazwy kolumn do identyfikowania statystyki obiektów
5. Błąd niestandardowy 2767 nie jest obsługiwane.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [DBCC SHOW_STATISTICS][] w witrynie MSDN.  Aby uzyskać więcej informacji, zobacz artykuły w [Omówienie tabeli][Omówienie], [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks], [podziału tabeli][partycją] i [Tabele tymczasowe][tymczasowe].  Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących zobacz [Najważniejsze wskazówki magazynu danych SQL][].  

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
[Szacowanie kardynalności]: https://msdn.microsoft.com/library/dn600374.aspx
[TWORZENIE STATYSTYK]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statystyki]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.Columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.Objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.Tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[AKTUALIZOWANIE STATYSTYKI]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
