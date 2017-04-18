<properties
    pageTitle="SQL w pamięci, wprowadzenie | Microsoft Azure"
    description="Technologie SQL w pamięci znacznie zwiększyć wydajność transakcji i analizy obciążeń pracą. Dowiedz się, jak korzystać z tych technologii."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Wprowadzenie do programu w pamięci (wersja Preview) w bazie danych SQL

Funkcje w pamięci znacznie zwiększyć wydajność transakcji i analizy obciążeń pracą w prawo sytuacjach.

W tym temacie wyróżniane dwóch pokazów, jedną dla OLTP w pamięci i jedną dla analizy w pamięci. Każdy pokaz zawiera zawierający czynności i kod, należy uruchomić pokaz. Możesz:

- Aby przetestować odmiany, aby wyświetlić różnice w wyniki; przy użyciu kodu lub
- W tym artykule kod opis tego scenariusza, a także dowiedzieć się, jak tworzyć i wykorzystywać obiektów w pamięci.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Szybki Start 1: technologie OLTP w pamięci szybciej T-SQL wydajności](http://msdn.microsoft.com/library/mt694156.aspx) -jest inny artykuł ułatwiające rozpoczęcie pracy.

#### <a name="in-memory-oltp"></a>OLTP w pamięci

Dostępne są następujące funkcje w pamięci [OLTP](#install_oltp_manuallink) (przetwarzanie transakcji online):

- Pamięć zoptymalizowana tabel.
- Skompilowany oryginalnie procedur składowanych.


Pamięć zoptymalizowana tabeli z jednym reprezentacja się w aktywnej pamięci, oprócz standardowej reprezentacja na dysku twardym. Transakcji w odniesieniu do tabeli działa szybciej, ponieważ współdziałają bezpośrednio z tylko reprezentacją, który znajduje się w aktywnej pamięci.

Z OLTP w pamięci można uzyskać w górę do uzyskania 30 razy w przepustowość transakcji, w zależności od szczegółowe informacje na temat obciążenie pracą.


Oryginalnie skompilowany procedur składowanych wymaga mniejszej liczby instrukcji procesora podczas wykonywania niż tradycyjny interpretowany procedur składowanych. Firma Microsoft przejrzane wynik natywnych kompilacji w czasie trwania, które są 1/100 interpretowany czasu trwania.


#### <a name="in-memory-analytics"></a>Analizy w pamięci 

Ta funkcja w pamięci [analizy](#install_analytics_manuallink) jest:

Indeksy Columnstore zwiększyć wydajność programu raportów dotyczących kwerend i analizy. 


#### <a name="real-time-analytics"></a>W czasie rzeczywistym analizy

[W czasie rzeczywistym](http://msdn.microsoft.com/library/dn817827.aspx) analiz łączenie OLTP w pamięci i analiz, aby uzyskać:

- W czasie rzeczywistym informacje oparte na danych operacyjnych.


#### <a name="availability"></a>Dostępność


GA, ogólnodostępną:

- [Indeksy Columnstore](http://msdn.microsoft.com/library/dn817827.aspx) , które są *na dysku*.


Podgląd:

- OLTP w pamięci
- W czasie rzeczywistym analizy operacyjne


Zagadnienia dotyczące podczas funkcje w pamięci są dostępne w wersji Preview są opisane [w dalszej części tego tematu](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Te funkcje podglądu są dostępne tylko w przypadku baz danych [*Premium*](sql-database-service-tiers.md) nie w puli elastyczne, a nie jest dostępna dla żadnych Basic lub standardowe baz danych.  Obsługa Premium baz danych w pule elastyczne jest już wkrótce. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>ODPOWIEDZI. Instalowanie przykładowe OLTP w pamięci

Możesz utworzyć AdventureWorksLT [wersji 12] przykładowej bazy danych za pomocą kilku kliknięć w [Azure portal](https://portal.azure.com/). Następnie w tej sekcji wyjaśniono, jak mogą wzbogacić AdventureWorksLT bazy danych przy użyciu:

- Tabele w pamięci.
- Procedura składowana oryginalnie skompilowany.


#### <a name="installation-steps"></a>Kroki instalacji

1. W [portalu Azure](https://portal.azure.com/)tworzenie Premium bazy danych na serwerze w wersji 12. Ustawianie **źródła** na bazę danych przykładowej AdventureWorksLT [wersji 12].
 - Aby uzyskać szczegółowe instrukcje można zobacz [Tworzenie pierwszej bazy danych Azure SQL](sql-database-get-started.md).

2. Nawiązywanie połączenia z bazą danych z programu SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Skopiuj [skrypt w pamięci OLTP języku Transact-SQL](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) do Schowka.
 - Skrypt T-SQL tworzy obiekty niezbędne w pamięci AdventureWorksLT przykładowej bazy danych utworzonej w kroku 1.

4. Wklej skrypt T-SQL do SSMS, a następnie wykonać skrypt.
 - Kluczowe jest `MEMORY_OPTIMIZED = ON` instrukcja CREATE TABLE klauzuli, jak w:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Błąd 40536


Jeśli zostanie wyświetlony błąd 40536 po uruchomieniu skryptu T-SQL, uruchom następujący skrypt T-SQL w celu zweryfikowania, czy bazę danych obsługuje w pamięci:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Wynik **0** oznacza, że nie jest obsługiwana w pamięci, a 1 oznacza, że jest obsługiwana. Do diagnozowania problemu:

- Upewnij się, że baza danych została utworzona po funkcje OLTP w pamięci stał się aktywne podglądu.
- Upewnij się, że baza danych znajduje się na warstwie usługi Premium.


#### <a name="about-the-created-memory-optimized-items"></a>Temat utworzonych elementów zoptymalizowane pamięci

**Tabele**: próbka zawiera następujące tabele zoptymalizowane pamięci:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Można sprawdzić zoptymalizowana pod pamięci tabel przy użyciu **Eksploratora obiektów** w SSMS przez:

- Kliknij prawym przyciskiem myszy **tabele** > **Filtr** > **Ustawień filtrowania** > **Jest zoptymalizowany pamięci** jest równe 1.


Lub kwerendy można widoków wykazu takich jak:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Procedura składowana oryginalnie skompilowany**: SalesLT.usp_InsertSalesOrder_inmem może być kontrolowane za pomocą kwerendy widoku wykazu:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Uruchamianie obciążenie pracą OLTP próbki

Jedyną różnicą między następujących dwóch *procedur składowanych* to, że pierwszej procedury używa zoptymalizowane pamięci wersji tabele, a drugi procedura używa zwykłymi tabelami na dysku:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


W tej sekcji widać sposobu używania narzędzia przydatne **ostress.exe** do wykonywania dwóch procedur składowanych na poziomie stressful. Umożliwia porównywanie, jak długo trwa uruchamia dwa obciążenia do wykonania.


Po uruchomieniu ostress.exe, zaleca się pomyślnym zakończeniu wartości parametrów przeznaczona do obu:

- Uruchamianie dużej liczby równoczesne połączenia za pomocą - n100.
- Masz każdej pętli połączenia setki razy, przez przy użyciu parametru - r500.


Jednak warto rozpocząć z dużo mniejszymi wartościami, takich jak - n10 i - r50 zapewnienie wszystko działa.


### <a name="script-for-ostressexe"></a>Skrypt ostress.exe


W tej sekcji wyświetla skrypt T-SQL, który jest osadzony w naszym wiersza polecenia ostress.exe. Skrypt używa elementów, które zostały utworzone przez skrypt T-SQL, który został wcześniej zainstalowany.


Poniższy skrypt wstawia przykładowe zamówienia sprzedaży z pięciu pozycji do poniższych zoptymalizowane pamięci *tabel*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Aby wersji _ondisk poprzedniego T-SQL dla ostress.exe, po prostu chcesz zamienić obu wystąpienia podciąg *_inmem* z *_ondisk*. Te zastępuje wpływa na nazw tabel i procedur składowanych.


### <a name="install-rml-utilities-and-ostress"></a>Instalowanie narzędzia RML i ostress


Czy zamierzasz najlepiej uruchomić ostress.exe maszyn wirtualnych Azure. W tym samym Azure regionu geograficznego miejsce, w którym znajduje się AdventureWorksLT bazy danych należy utworzyć [maszyn wirtualnych Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) . Ale ostress.exe można uruchamiać zamiast na komputerze przenośnym.


Na maszyn wirtualnych lub na inną możesz udostępniać wybierz, należy zainstalować narzędzia powtarzania Markup Language (RML), które obejmują ostress.exe.

- Zapoznać się z omówieniem ostress.exe w [Przykładowej bazie danych dla OLTP w pamięci](http://msdn.microsoft.com/library/mt465764.aspx).
 - Lub [Przykładowa baza danych dla OLTP w pamięci](http://msdn.microsoft.com/library/mt465764.aspx).
 - Lub zobacz [Blog dotyczące instalowania ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Najpierw należy uruchomić obciążenie pracą obciążenia _inmem


Okno *RML Cmd tryb* służy do uruchamiania naszych ostress.exe wiersza polecenia. Parametry wiersza polecenia bezpośredni ostress do:

- Równoczesne połączenia 100 (-n100).
- Każdego połączenia uruchamianie skryptu T-SQL 50 godzin (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Aby uruchomić poprzedniego wiersza polecenia ostress.exe:


1. Resetuj zawartość danych do bazy danych, uruchamiając następujące polecenie w SSMS, aby usunąć wszystkie dane, które została wstawiona przez dowolnego poprzedniego zostanie uruchomiona:
```
EXECUTE Demo.usp_DemoReset;
```

2. Skopiuj tekst, poprzedniego wiersza polecenia ostress.exe do Schowka.

3. Zamienianie `<placeholders>` parametrów -S - U -P - d z prawidłowe wartości rzeczywistych.

4. Do edycji wiersza polecenia są uruchamiane w oknie RML Cmd.


#### <a name="result-is-a-duration"></a>Wynik to czas trwania


Po zakończeniu ostress.exe zapisuje uruchamianie czas trwania jako jego końcowego wiersza danych wyjściowych w oknie RML Cmd. Na przykład skrócić testowa trwał około 1,5 minut:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Resetowanie, edytować dla _ondisk, a następnie uruchom ponownie


Po utworzeniu wyniku Uruchom _inmem dla _ondisk uruchomić należy wykonać następujące czynności:


1. Zresetuj bazy danych, uruchamiając następujące polecenie w SSMS, aby usunąć wszystkie dane, które zostało wstawione w procesie poprzedniego:
```
EXECUTE Demo.usp_DemoReset;
```

2. Edytowanie poziomu wiersza polecenia ostress.exe, aby zamienić wszystkie *_inmem* *_ondisk*.

3. Uruchom ponownie ostress.exe po raz drugi, a Przechwytywanie wynik czas trwania.

4. Ponownie zresetować bazę danych, odpowiedzialny usuwania dużej ilości testowymi danymi, które można.


#### <a name="expected-comparison-results"></a>Wyniki porównania oczekiwanych

Testów w pamięci jest wyświetlane wydajność **9 razy** obciążenie simplistic z ostress uruchomionych maszyn wirtualnych Azure w tym samym regionie Azure jako bazę danych.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Instalowanie przykładowe analizy w pamięci


W tej sekcji możesz porównać wyniki Jo oraz statystyk podczas korzystania z indeksem columnstore a indeks tradycyjnych b drzewa.


W czasie rzeczywistym analizy na obciążenie pracą OLTP często jest najlepszy indeks zbudowania columnstore. Aby uzyskać szczegółowe informacje, zobacz [Opisany indeksy Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Przygotowywanie test analizy columnstore


1. Azure portal należy utworzyć nową bazą danych AdventureWorksLT z próbki.
 - Użyj takiej samej nazwie.
 - Wybierz dowolny warstwa usług Premium.

2. Kopiowanie [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) do Schowka.
 - Skrypt T-SQL tworzy obiekty niezbędne w pamięci AdventureWorksLT przykładowej bazy danych utworzonej w kroku 1.
 - Skrypt tworzy tabeli Wymiar i dwie tabele faktów. Tabele faktów są wypełniane milionów 3.5 wierszy.
 - Skrypt może potrwać 15 minut.

3. Wklej skrypt T-SQL do SSMS, a następnie wykonać skrypt.
 - Kluczowe jest słowo kluczowe **COLUMNSTORE** w instrukcji **CREATE INDEX** , jako:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Ustaw AdventureWorksLT poziom zgodności 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Poziom 130 nie są bezpośrednio związane w pamięci funkcji. Ale poziom 130 zazwyczaj zapewnia lepszą wydajność kwerendy, niż 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Kluczowe tabele i indeksy columnstore


- dbo. FactResellerSalesXL_CCI jest tabeli, która ma indeksu grupowany **columnstore** zaawansowane kompresji na poziomie *danych* .

- dbo. FactResellerSalesXL_PageCompressed jest tabeli, która ma równoważne zwykła grupowany indeksu, w którym jest skompresowany tylko na poziomie *strony* .


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Kluczowe kwerendy do porównywania indeks columnstore


[W tym miejscu](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) jest kilka typów kwerend T SQL, może zostać uruchomiony Aby wyświetlić ulepszenia dotyczące wydajności. W kroku 2 w obszarze skrypt T-SQL jest para zapytania, które mogą być przydatne bezpośredni. Dwóch zapytań różnią się tylko w jednym wierszu:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Indeks grupowany columnstore znajduje się w tabeli**_CCI** FactResellerSalesXL.

Poniższy fragment skrypt T-SQL drukowany statystyki Jo i godziny dla każdej tabeli kwerendy.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Podgląd zagadnienia związane z OLTP w pamięci


Funkcje OLTP w pamięci w bazie danych SQL Azure stał się [aktywne podglądu na 28 października 2015 r](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


W bieżącym podglądzie OLTP w pamięci jest obsługiwane tylko w przypadku:

- W przypadku baz danych, które są na warstwie usługi *Premium* .

- Bazy danych, które zostały utworzone po funkcje OLTP w pamięci stał się aktywny.
 - Nowej bazy danych nie obsługuje OLTP w pamięci, gdy zostanie przywrócony z bazy danych, która została utworzona przed funkcje OLTP w pamięci stał się aktywny.


W razie wątpliwości będzie można uruchamiać następujących wybierz T-SQL w celu sprawdzenia, czy baza danych obsługuje OLTP w pamięci. Wynikiem **1** oznacza, że baza danych obsługuje OLTP w pamięci:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Jeśli kwerenda zwraca wartość **1**, OLTP w pamięci jest obsługiwana w tej bazy danych i odpowiednie kopii bazy danych i przywracanie bazy danych utworzone na podstawie tej bazy danych.


#### <a name="objects-allowed-only-at-premium"></a>Obiekty dozwolone tylko na Premium


Jeśli baza danych zawiera dowolny z następujących rodzajów obiektów OLTP w pamięci lub typy, obniżenie warstwie usługi bazy danych z Premium podstawowe lub standardowy nie jest obsługiwane. Na starszą wersję bazy danych, należy najpierw usunąć tych obiektów:

- Pamięć zoptymalizowana tabel
- Typy pamięć zoptymalizowana tabel
- Moduły oryginalnie skompilowany


#### <a name="other-relationships"></a>Relacje


- OLTP w pamięci za pomocą funkcji z bazami danych w pule elastyczne nie jest obsługiwana w wersji Preview.
 - Aby przenieść bazie danych, które mają lub miały OLTP w pamięci obiektów do puli elastyczne, wykonaj następujące czynności:
  - 1. Usuwanie dowolnego zoptymalizowane pamięci tabel, typy tabel i oryginalnie skompilowany moduły T-SQL w bazie danych
  - 2. Zmienianie poziomu usług bazy danych na standardowy
  - 3. Przenoszenie bazy danych do puli elastyczne

- Za pomocą OLTP w pamięci z magazynu danych SQL nie jest obsługiwane.
 - Funkcja indeks columnstore analizy w pamięci jest obsługiwana w magazynie danych SQL.

- W sklepie zapytania nie przechwytywać kwerendy wewnątrz oryginalnie skompilowany moduły.

- Niektóre funkcje Transact-SQL nie są obsługiwane z OLTP w pamięci. Dotyczy to zarówno programu Microsoft SQL Server i bazy danych SQL Azure. Aby uzyskać szczegółowe informacje Zobacz:
 - [Obsługa języka Transact-SQL OLTP w pamięci](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Konstrukcji Transact-SQL nie są obsługiwane przez OLTP w pamięci](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Następne kroki


- Spróbuj [Użyj OLTP w pamięci w istniejącej aplikacji SQL Azure.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Dodatkowe zasoby

#### <a name="deeper-information"></a>Więcej informacji

- [Dowiedz się więcej o OLTP w pamięci, której dotyczy programu Microsoft SQL Server i bazy danych SQL Azure](http://msdn.microsoft.com/library/dn133186.aspx)

- [Więcej informacji na temat działania analizy w czasie rzeczywistym w witrynie MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Oficjalny dokument na [Typowe wzorce obciążenie pracą i zagadnienia migracji](http://msdn.microsoft.com/library/dn673538.aspx), który opisuje desenie obciążenie pracą, gdzie OLTP w pamięci często zapewnia znaczący wzrost wydajności.

#### <a name="application-design"></a>Projekt aplikacji

- [OLTP (Optymalizacja w pamięci) w pamięci](http://msdn.microsoft.com/library/dn133186.aspx)

- [Użyj OLTP w pamięci w istniejącej aplikacji SQL Azure.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Narzędzia

- [Podgląd narzędzia danych programu SQL Server (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), miesięczny najnowszą wersję.

- [Opis narzędzia powtarzania Markup Language (RML) dla programu SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Monitorowanie przechowywania w pamięci](sql-database-in-memory-oltp-monitoring.md) OLTP w pamięci.

