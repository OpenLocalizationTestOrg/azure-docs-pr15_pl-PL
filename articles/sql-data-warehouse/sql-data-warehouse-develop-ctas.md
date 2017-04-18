<properties
   pageTitle="Tworzenie tabeli jako wybierz (CTAS) w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące kodowania z Utwórz tabelę jako instrukcji select (CTAS) w magazynie danych SQL Azure dla opracowania rozwiązań."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Tworzenie tabeli jako wybierz pozycję (CTAS) w magazynie danych SQL
Tworzenie tabeli jako wybierz pozycję lub `CTAS` znajduje jedną z najważniejszych funkcji T-SQL. Jest w pełni parallelized operację, której powoduje utworzenie nowej tabeli oparte na danych wyjściowych instrukcji SELECT. `CTAS`jest najprostszym i najszybszym sposobem utworzenia kopii tabeli. Należy rozważyć należy wersję z doładowaniem `SELECT..INTO` Sł. Niniejszy dokument zawiera zarówno przykłady i najważniejsze wskazówki dotyczące `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Kopiowanie tabeli za pomocą CTAS

Być może jedno z najczęstszych zastosowań `CTAS` jest tworzenie kopii tabeli, można zmienić DDL. Jeśli na przykład pierwotnie utworzony tabeli jako `ROUND_ROBIN` i teraz chcę zmienić ją na tabeli rozpowszechniane na kolumnę, `CTAS` się, jak można zmienić kolumnę rozkładu. `CTAS`można również zmienić typ podziału, indeksowanie lub kolumny.

Załóżmy, że został utworzony w tej tabeli za pomocą domyślny typ rozkładu `ROUND_ROBIN` distributed, ponieważ nie kolumnę rozkładu został określony na `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Teraz chcesz utworzyć nową kopię w tej tabeli z indeksem Columnstore grupowany tak, aby można korzystać z działania tabel Columnstore wykres kolumnowy grupowany. Również dotycząca dystrybucji tej tabeli na klucz produktu, ponieważ są przewidywania sprzężenia w tej kolumnie i chcesz uniknąć przenoszenia danych podczas sprzężenia na ProductKey. Ponadto również chcesz dodać podział na OrderDateKey tak, aby można szybko usunąć stare dane przez upuszczanie stare partycje. Oto instrukcji CTAS, które chcesz skopiować starej tabeli do nowej tabeli.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Na koniec można zmienić tabel do Zamień w nowej tabeli, a następnie upuść starej tabeli.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Magazyn danych SQL Azure czy jeszcze nie pomocy technicznej automatyczne tworzenie lub automatycznej aktualizacji statystyki.  W celu uzyskania najlepszej wydajności należy przejść z zapytań, należy pamiętać, że statystyki można tworzyć na wszystkich kolumn wszystkie tabele po załadowaniu pierwszego lub wystąpienia jakichkolwiek znacznych zmian w danych.  Aby uzyskać szczegółowy opis statystyki zobacz temat [Statystyki][] w grupie opracowanie tematów.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Za pomocą CTAS w celu obejścia nieobsługiwane funkcje

`CTAS`można także w celu obejścia liczbę nieobsługiwane funkcje wymienione poniżej. To często może okazać się sytuacja Zysk/zysk nie tylko będzie kodzie spełniać, ale będzie często wykonywać szybciej w magazynie danych SQL. Jest to wyniku jej w pełni parallelized projekt. Scenariusze, które można pracować z CTAS wokół obejmują:

- WYBIERZ POZYCJĘ. DO
- SPRZĘŻENIA ANSI na aktualizacje
- Sprzężenia ANSI na usuwa
- SCALANIE poufności informacji

> [AZURE.NOTE] Pomyśl "CTAS pierwszy". Jeśli uważasz, że można rozwiązać problem przy użyciu `CTAS` następnie jest zazwyczaj najlepszym sposobem zbliżających się — nawet wtedy, gdy piszesz większej ilości danych w wyniku.
>

## <a name="selectinto"></a>WYBIERZ POZYCJĘ. DO
Może się okazać `SELECT..INTO` pojawi się w polu Liczba miejsc w rozwiązaniu.

Poniżej przedstawiono przykładowy `SELECT..INTO` instrukcji:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Aby przekonwertować powyżej, aby `CTAS` jest bardzo proste przekazywania:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS obecnie wymaga, aby określić kolumnę rozkładu.  Jeśli nie celowo chcesz zmienić kolumnę rozkładu usługi `CTAS` będą działać najszybciej zaznacz kolumnę rozkładu jest taka sama, jak tabela podstawowa, jak ta strategia pozwala uniknąć przenoszenia danych.  Jeśli tworzysz małą tabelę, której wydajność nie ma znaczenie, a następnie można określić `ROUND_ROBIN` można uniknąć konieczności decyduje o kolumnę rozkładu.

## <a name="ansi-join-replacement-for-update-statements"></a>Zastępuje sprzężenia ANSI instrukcji update

Może się okazać, że masz złożonych aktualizacji, która łączy więcej niż dwie tabele współpraca przy użyciu dołączania składnia ANSI przeprowadzić AKTUALIZACJĘ lub usunięcie.

Załóżmy, że musiał zaktualizować w tej tabeli:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Pierwotna kwerenda może sprawdzono podobną do następującej:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Ponieważ program SQL Data Warehouse nie obsługuje ANSI sprzężenia w `FROM` klauzuli `UPDATE` instrukcji, nie można skopiować kod nad zmieniając nieco.

Można użyć kombinacji `CTAS` i niejawnych Dołącz, aby zamienić ten kod:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>Instrukcje usunąć przyklejanych sprzężenia ANSI
Czasami najlepszym rozwiązaniem usuwania danych jest użycie `CTAS`. Zamiast usuwania danych po prostu zaznacz dane, które chcesz zachować. Ten szczególnie dla `DELETE` instrukcji używających ansi łączących składni, ponieważ program SQL Data Warehouse nie obsługuje ANSI sprzężenia w `FROM` klauzuli `DELETE` instrukcji.

Przykład przekonwertowanej instrukcja DELETE znajduje się poniżej:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Zamienianie stwierdzeń korespondencji seryjnej
Instrukcje korespondencji seryjnej można zastąpić, co najmniej w części, za pomocą `CTAS`. Można konsolidować `INSERT` oraz `UPDATE` do pojedynczej instrukcji. Rekordy usunięte musi być zamknięte w drugim instrukcji.

Przykład `UPSERT` znajduje się poniżej:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>Zalecenie CTAS: jawnie określać typ danych i opcji wyjścia dopuszczania wartości null

Podczas migracji kodu mogą okazać się uruchomieniu przez ten typ deseniu kodowania:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instynktownie może być zastanowić, należy zainstalować ten kod CTAS a możesz są poprawne. Jednak jest to ukryte problem.

Poniższy kod nie daje taki sam wynik:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Zwróć uwagę, kolumna "wynik" prowadzi do przodu danych typu i opcja dopuszczania wartości null wartości wyrażenia. To może prowadzić do delikatne wariancje wartości, jeśli nie masz zachować ostrożność.

Spróbuj wykonać poniższe czynności, na przykład:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Wartości przechowywanej w wyniku różni się. Trwałe wartość w kolumnie wynik jest używany w innych wyrażenia błąd staje się bardziej znaczące.

![][1]

Jest to szczególnie istotne w przypadku migracji danych. Mimo że drugiego zapytania jest mieć prawdopodobnie dokładniejszego występuje problem. Dane będzie różne w porównaniu z w źródłowym systemie i powodująca pytania integralności w procesie migracji. Jest to jeden z tych rzadkich przypadkach, gdy "problem" odpowiedzi jest faktycznie właściwą domenę!

Przyczynę widzimy tej różnicy między dwoma wynikami jest do rzutowania niejawne typu. W pierwszym przykładzie tabeli określa definicji kolumny. Gdy zostanie wstawiona występuje niejawna konwersja typu. W drugim przykładzie istnieje nie niejawna konwersja typu wyrażenie definiuje typ danych kolumny. Zwróć uwagę, że kolumny w drugim przykładzie zdefiniowano jako kolumna wartości null należy w pierwszym przykładzie nie. Wyraźnie zdefiniowano utworzenia tabeli w pierwszym przykładzie kolumna opcja dopuszczania wartości null. W drugim przykładzie ją po prostu pozostał wyrażenie i domyślnie to doprowadziłaby definicji wartość NULL.  

Dotyczące rozwiązywania tych problemów należy jawnie ustawione konwersji typów i opcja dopuszczania wartości null w `SELECT` części `CTAS` instrukcji. Nie można skonfigurować te właściwości w obszarze Utwórz tabelę.

W poniższym przykładzie pokazano, jak naprawić kod:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Pamiętaj o następujących kwestiach:
- CAST lub CONVERT może używano
- Można wymusić opcja dopuszczania wartości null nie ŁĄCZONEJ ISNULL
- ISNULL jest funkcją zewnętrzne
- Druga część ISNULL jest stałą to znaczy 0

> [AZURE.NOTE] Opcja dopuszczania wartości null poprawnie można ustawić jest niezbędne do za pomocą `ISNULL` i nie `COALESCE`. `COALESCE`jest funkcją deterministyczne i dlatego wynikiem wyrażenia zawsze będzie NULLable. `ISNULL`różni się. Jest deterministyczna. Dlatego po drugiej części `ISNULL` funkcja jest stałą lub literał, a następnie będzie wartość wynikowa nie wartość NULL.

Ta porada przydaje się nie tylko w celu zapewnienia integralności obliczeń. Ważne jest również służące do przełączania partition tabeli. Załóżmy, że masz w tej tabeli zdefiniowane jako usługi faktów:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Jednak pola wartości to wyrażenie obliczeniowych nie jest częścią danych źródłowych.

Aby utworzyć usługi podzielone na partycje zestawu danych warto wykonaj następujące czynności:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Kwerendy można uruchomić idealnie poprawnie. Problem jest podczas próby wykonywania pracy z wersją partycją. Definicje tabeli nie są zgodne. Aby definicji tabel, Uwzględnij CTAS wymaga zmodyfikowania.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

W związku z tym można zobaczyć, że typ spójności i konserwowanie właściwości opcja dopuszczania wartości null na CTAS jest zalecane najlepsze odtwarzania. Pomaga zachować integralność w obliczeniach, a także sprawia, że przełączanie partycją jest możliwe.

Można znaleźć w witrynie MSDN Aby uzyskać więcej informacji na temat korzystania z [CTAS][]. Jest jednym z najważniejszych instrukcje w magazynie danych SQL Azure. Upewnij się, że wiesz dokładnie.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
