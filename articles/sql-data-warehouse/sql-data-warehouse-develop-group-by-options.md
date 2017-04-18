<properties
   pageTitle="Opcje w magazynie danych SQL grupowania według | Microsoft Azure"
   description="Porady dotyczące wykonywania grupy opcji w magazynie danych SQL Azure do opracowania rozwiązań."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Grupowanie według opcji w magazynie danych SQL

Klauzula [GROUP BY][] jest używany do agregowanie danych do podsumowania zestaw wierszy. Ponadto wprowadzono kilka opcji, które rozszerzają jej funkcje, które musisz pracować wokół jako nie są bezpośrednio obsługiwane przez magazynu danych SQL Azure.

Te opcje są
- Grupowanie według z funkcją ROLLUP
- ZESTAWY GRUPOWANIA
- GROUP BY z modułu

## <a name="rollup-and-grouping-sets-options"></a>Zestawienie i grupowanie zestawów opcji
Najłatwiejszym opcji w tym miejscu jest użycie `UNION ALL` zamiast tego przeprowadzić pakietu zamiast Polegaj na jawne składni. Wynikiem jest dokładnie taki sam

Poniżej przedstawiono przykładowy grupy przy użyciu instrukcji `ROLLUP` opcję:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Za pomocą zestawienia możemy zażądał agregacji następujące czynności:
- Kraju i regionu
- Kraj
- Suma końcowa

Aby zamienić to będzie konieczne używanie `UNION ALL`; Określanie agregacji jawnie wymagane do zwracała te same wyniki:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Dla wszystkich należy wykonać zestawów GRUPOWANIA jest przyjąć takie same kapitału, ale tylko tworzenie UNION ALL sekcji poziomów agregacji, których chcemy zobacz

## <a name="cube-options"></a>Opcje modułu
Istnieje możliwość tworzenia GROUP BY WITH CUBE metoda UNION ALL. Problem polega na kod szybko może być kłopotliwe i niewygodna. Ograniczanie to umożliwia to bardziej zaawansowane metody.

Można użyć w powyższym przykładzie.

Pierwszym krokiem jest określenie "modułu" definiujące wszystkich poziomów agregacji, który chcemy utworzyć. Należy pamiętać o dołączanie krzyżowe dwóch tabel pochodnych. Spowoduje to wygenerowanie wszystkich poziomów dla nas. Pozostałe kod występuje naprawdę formatowania.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Wyniki CTAS są widoczne poniżej:

![][1]

Drugim krokiem jest określenie tabeli docelowej do przechowywania wyników pośrednich:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Trzecim krokiem jest pętli naszych modułu kolumn podczas wykonywania polecenia agregację. Kwerenda będzie uruchamiane raz dla każdego wiersza w tabeli tymczasowej #Cube i przechowywać wyniki w tabeli temp #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Ponadto można zwróconych wyników, po prostu czytając z tabeli tymczasowej #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Rozdzielanie kodu sekcje i generowanie konstrukcji pętli kod staje się bardziej w zarządzaniu i utrzymania.


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GRUPOWANIE WEDŁUG]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
