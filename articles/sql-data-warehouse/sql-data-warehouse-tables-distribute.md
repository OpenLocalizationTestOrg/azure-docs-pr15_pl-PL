<properties
   pageTitle="Rozpowszechnianie tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do rozpowszechniania tabel w magazynie danych SQL Azure."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Rozpowszechnianie tabel w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Program SQL Data Warehouse jest przetwarzanie znacznym stopniu równoległe (MPP) Rozproszony system bazy danych.  Dzieląc danych i możliwości przetwarzania w różnych węzłach, magazynu danych SQL mogą oferować dużego skalowalność — daleko poza dowolny pojedynczy system.  Określanie sposobu rozpowszechniania danych w magazynie danych SQL jest jednym z najważniejszych czynników do osiągnięcia optymalnej wydajności.   Klawisz, aby uzyskać optymalną wydajność jest minimalizowania przenoszenia danych i z kolei wybiera strategii dystrybucji prawy klawisz, aby zminimalizować przenoszenia danych.

## <a name="understanding-data-movement"></a>Opis przepływu danych

W systemie MPP dane z każdej tabeli jest dzielona przez kilka baz danych źródłowych.  Najbardziej zoptymalizowane zapytań w systemie MPP może po prostu przekazywane do wykonania na pojedyncze rozłożone bazy danych przy użyciu bez interakcji między innymi bazami danych.  Załóżmy na przykład, że masz bazy danych za pomocą danych sprzedaży, zawierający dwie tabele, sprzedaży i klientów.  Jeśli ma kwerendę, która ma dołączyć do tabeli sprzedaży do tabeli odbiorcy, możesz podzielić sprzedaży i tabele klienta w górę przez numer klienta, umieszczanie każdego z klientów w oddzielnej bazy danych można rozwiązać wszelkich kwerend, które powodują łączenie sprzedaży i obsługi klientów w każdej bazy danych za pomocą żadnych informacji o innych baz danych.  Z drugiej strony, jeśli dane sprzedaży podzielona przez numer zamówienia i dane o klientach według numeru klienta, następnie dowolnej bazy danych nie będą mieć odpowiadające im dane dla każdego z klientów, a więc jeśli chcesz dołączyć do danych sprzedaży do danych klienta, czy jest potrzebne do uzyskania danych dla każdego z klientów z innej bazy danych.  W tym przykładzie druga przenoszenia danych należy występują Aby przenieść dane klienta dane o sprzedaży, tak aby dwie tabele można łączyć.  

Przenoszenie danych nie zawsze jest nieprawidłowy element, czasami konieczne jest rozwiązuje kwerendy.  Jednak podczas tego dodatkowego kroku można uniknąć, sposób naturalny kwerenda działa szybciej.  Przenoszenie danych pojawia się najczęściej podczas tabele są sprzężone lub agregacji są wykonywane.  Często należy wykonać, aby podczas można zoptymalizować dla jeden scenariusz, takich jak sprzężenia, nadal trzeba przenoszenia danych ułatwiają rozwiązywanie innych scenariusza, takich jak agregacji.  Lewy jest oceniania, która jest mniej pracy.  W większości przypadków rozpowszechniania tabelami faktów dużych często sprzężonych kolumnę, na jest najskuteczniejszą metodą zmniejszania większość przenoszenia danych.  Rozpowszechnianie danych w kolumnach sprzężenia jest o wiele bardziej typowe metody w celu zmniejszenia przenoszenia danych niż dystrybucji danych w kolumnach biorących udział w agregacji.

## <a name="select-distribution-method"></a>Wybierz metodę dystrybucji

W tle magazynu danych SQL dzieli danych do 60 baz danych.  Każdej bazy danych pojedyncze nosi nazwę **rozkładu**.  Po załadowaniu danych do każdej tabeli, magazynu danych SQL ma się dowiedzieć, jak do dzielenia danych na tych 60 dystrybucji.  

Metoda dystrybucji jest zdefiniowana na poziomie tabeli i obecnie dostępne są dwie opcje:

1. **Okrężnego** , które rozpowszechnianie danych równomiernie, ale losowo.
2. **Distributed mieszania** które rozdziela danych w oparciu o mieszania wartości z pojedynczej kolumny

Domyślnie podczas definiowania nie metodę rozkładu danych tabeli rozdzielane przy użyciu metody dystrybucji **okrężnego** .  Jednak podczas staje się bardziej zaawansowane w implementacji, można brać pod uwagę przy użyciu **skrótu distributed** tabel, aby zminimalizować przenoszenia danych, która z kolei będzie zoptymalizowania wydajności kwerend.

### <a name="round-robin-tables"></a>Tabele okrężnego

Przy użyciu metody okrężnego dystrybucji danych jest bardzo, jak ją dźwięki.  Jak załadowaniu danych każdy wiersz po prostu są wysyłane do następnego rozkładu.  Ta metoda dystrybucji danych będzie zawsze losowo równomierne rozłożenie dane bardzo we wszystkich dystrybucji.  Oznacza to, że istnieje nie sortowania wykonane podczas procesu okrężnego umieszcza dane.  Rozkład okrężnego jest czasami nazywana losowe skrótu z tego powodu.  Z tabeli rozłożone karuzeli istnieje bez konieczności opis danych.  Z tego powodu tabel karuzeli często wprowadzane dobre ładowania elementów docelowych.

Domyślnie w żaden sposób dystrybucji jest zaznaczona, metody dystrybucji okrężnego zostanie użyty.  Jednak podczas okrężnego tabele są łatwe w użyciu, ponieważ dane losowo obejmowała systemu, który oznacza, że system nie może zagwarantować, które rozkładu każdy wiersz jest na.  W wyniku systemu czasami trzeba wywołania operacji przeniesienia danych, aby lepiej zorganizować swoje dane przed może rozpoznać kwerendy.  Ten krok dodatkowe mogą spowolnić zapytań.

Należy rozważyć użycie okrężnego rozkładu dla tabeli w następujących sytuacjach:

- Przy rozpoczynaniu jako punktu startowego prostych
- Jeśli nie występują oczywiste łączących klucza
- Jeśli nie jest kolumną nadawało się na mieszania rozpowszechniania tabeli
- Jeśli tabeli nie udostępnia wspólny klucz sprzężenia z innymi tabelami
- Jeśli jest to mniej istotnych niż inne sprzężeń w kwerendzie
- Gdy jest tymczasowe Tabela tymczasowy

Zarówno w tych przykładach utworzy tabelę okrężnego:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Gdy jest okrężnego domyślny typ tabeli jest jawne usługi DDL najbardziej skuteczne rozwiązanie uznaje zapewniając zamiaru układu tabeli Wyczyść, aby inne osoby.

### <a name="hash-distributed-tables"></a>Skrótu rozłożone tabel

Za pomocą algorytmu **mieszania distributed** do rozpowszechniania tabel można zwiększyć wydajność wielu scenariuszach zmniejszając przenoszenia danych w czasie kwerendy.  Skrót distributed tabele są tabel, które są podzielone między rozłożone bazy danych za pomocą algorytmu mieszania na jedną kolumnę, którą można wybrać.  Kolumnę rozkładu jest, co określa sposób podziału danych między rozłożone baz danych.  Funkcja mieszania używane kolumnę rozkładu do przypisywania wierszy podziału.  Algorytm mieszania i dystrybucji wyniku jest deterministyczna.  To jest tej samej wartości w tym samym typie danych będzie ma zawsze do tego samego rozkładu.    

W tym przykładzie utworzy tabelę rozpowszechniane na identyfikator:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Zaznacz kolumnę rozkładu

Po wybraniu do **rozpowszechniania mieszania** tabeli, będzie konieczne zaznacz kolumnę rozkładu pojedynczy.  Wybierając kolumnę rozkładu, istnieją trzy główne kwestie do rozważenia.  

Zaznacz jedną kolumnę, która będzie:

1. Nie można zaktualizować
2. Równomierne rozłożenie danych, można uniknąć danych skośność
3. Minimalizowanie przenoszenia danych

### <a name="select-distribution-column-which-will-not-be-updated"></a>Zaznacz kolumnę rozkładu, które nie zostaną zaktualizowane

Rozkład kolumn nie są można aktualizować, w związku z tym, zaznacz kolumnę z wartościami statyczne.  Jeśli kolumny muszą być aktualizowane, jest zazwyczaj nie candidate dobrej.  W przypadku sprawy miejsce, w którym należy zaktualizować kolumnę rozkładu, jest to możliwe, najpierw usuwanie wiersza, a następnie wstawiając nowy wiersz.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Zaznacz kolumnę rozkładu, który spowoduje równomierne rozłożenie danych

Ponieważ rozproszony system wykonuje tylko tak szybko, jak jego najmniejszą dystrybucji, należy równomiernie dzielenie pracy przez dystrybucji w celu wykonania strategii systemie.  Sposób pracy jest podzielony na Rozproszony system jest oparty na miejsce, w którym znajdują się dane dla każdego rozkładu.  Dzięki temu ważne zaznacz kolumnę rozkładu prawo do dystrybucji danych, aby każdy rozkładu pracy równa i zajmie tym samym czasie, aby ukończyć jego część pracy.  Gdy praca jest dzielona również systemie, danych jest strategie przez dystrybucji.  Gdy dane nie jest równomierne rozłożenie, nazywamy to **pochylić danych**.  

Aby równomiernie dzielenie danych i uniknąć skośność danych, należy uwzględnić poniższe zagadnienia po wybraniu kolumny dystrybucyjnej:

1. Zaznacz kolumnę, która zawiera znaczną liczbę unikatowych wartości.
2. Unikaj dystrybucji danych w kolumnach z kilku różnych wartości. 
3. Unikaj dystrybucji danych dla kolumn o wysokiej częstotliwości o wartości null.
4. Unikaj dystrybucji danych w kolumnach daty.

Ponieważ każda wartość jest mieszany 1 60 dystrybucji, aby osiągnąć Rozdziel należy wybierz kolumnę, która jest wysoce unikatowy i zawiera więcej niż 60 unikatowych wartości.  Aby przedstawić, należy rozważyć przypadek miejsce, w którym kolumny zawiera tylko 40 unikatowych wartości.  Jeśli ta kolumna została wybrana jako klucz Dystrybucja, dane dla tej tabeli czy grunt na 40 dystrybucji co najwyżej opuszczania dystrybucji 20 z żadnych danych i żadne przetwarzanie robić.  I odwrotnie 40 dystrybucji wymaga więcej pracy, aby to zrobić, jeśli dane została równomiernie ponad 60 dystrybucji.  W tym scenariuszu przedstawiono przykładowy skośność danych.

W systemie MPP każdy krok zapytania czeka na wszystkich przydziałów do wykonania udziału w pracy.  Jeśli jednym rozkładzie robi większej ilości pracy od innych, następnie zasobów innych dystrybucji są zasadniczo oszczędności tylko oczekiwanie na rozkład zajęty.  Podczas pracy nie jest równomiernie rozciągnąć wszystkich przydziałów, nazywamy to **funkcja skośność przetwarzania**.  Funkcja skośność przetwarzanie spowoduje, że kwerendy do działać wolniej niż obciążenie pracą można być równomiernie rozmieścić w dystrybucji.  Skośność danych powoduje przejście do procesu przetwarzania skośność.

Aby uniknąć rozpowszechniania w kolumnie wysoce wartości null jako wartości null zostanie najpierw na tego samego rozkładu. Ponieważ wszystkie dane dla określonej daty zostanie najpierw na tego samego rozkładu rozpowszechniania w kolumnie Data może powodować również pochylanie przetwarzania. Jeśli kilku użytkowników wykonywania kwerend wszystkie filtry na tę samą datę, a następnie tylko 1 60 dystrybucji będzie wykonanie pracy od podanej daty będzie tylko w jednym rozkładzie. W tym scenariuszu kwerend, prawdopodobnie spowoduje uruchomienie 60 razy mniejsza niż dane zostały równomiernie rozmieścić nad dystrybucji. 

Gdy bez kolumn nadawało istnieje, a następnie warto rozważyć użycie okrężnego jako metodę dystrybucji.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Zaznacz kolumnę rozkładu, która zostanie zminimalizowane przenoszenia danych

Minimalizowanie przenoszenia danych, wybierając kolumnę rozkładu prawo jest jednym z najważniejszych strategie dotyczące optymalizowania wydajności magazynu danych SQL.  Przenoszenie danych pojawia się najczęściej podczas tabele są sprzężone lub agregacji są wykonywane.  Kolumny używane w `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` i `HAVING` klauzule wszystkie ułatwiają **dobre** skrótu kandydatów rozkładu. 

Z drugiej strony, kolumny w `WHERE` klauzuli **nie** tworzącej dla kandydatów kolumny warto skrótu powoduje przetwarzanie pochylić ponieważ one ograniczyć, które dystrybucji uczestniczyć w kwerendzie.  Przykładem kolumnę, która może być kuszące do dystrybucji na, ale często może powodować to przetwarzanie skośność jest kolumną daty.

Ogólnie mówiąc Jeśli masz dwie tabele faktów dużych często związane w sprzężenia będzie uzyskać większość wydajności przekazując obie tabele jednej z kolumn sprzężenia.  Jeśli masz tabelę, która nigdy nie jest dołączony do innej tabeli faktów dużych, Znajdź do kolumn, które są często w `GROUP BY` klauzuli.

Istnieje kilka kluczowych kryteria, które muszą być spełnione w celu uniknięcia przenoszenia danych podczas dołączania do:

1. Tabele uczestniczące w sprzężenia musi być mieszania rozpowszechniane na **jednej** kolumny wchodzące w sprzężenia.
2. Typy danych w kolumnach sprzężenia musi odpowiadać między obu tabelach.
3. Kolumny muszą być dołączone z operatorem równa się.
4. Typ sprzężenia nie może być `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Dane dotyczące rozwiązywania problemów skośność

Gdy dane w tabeli jest rozkładana według rozkładu mieszania to prawdopodobieństwo, że niektóre podziału zostanie pochylony mają nieproporcjonalnie większej ilości danych niż inne. Funkcja skośność nadmiarowe dane można wpływ na wydajność zapytania, ponieważ wynik końcowy rozłożone kwerendy, należy poczekać rozkład najdłuższy uruchomionego zakończyć. W zależności od stopnia pochylanie danych może być konieczne ją usunąć.

### <a name="identifying-skew"></a>Identyfikowanie funkcja skośność

Prosty sposób, aby zidentyfikować tabelę, jako pochylony jest użycie `DBCC PDW_SHOWSPACEUSED`.  Jest to bardzo szybki i prosty sposób, aby wyświetlić liczbę wierszy tabeli, które są przechowywane w każdej z 60 podziału bazy danych.  Należy pamiętać, że do wykonywania najbardziej strategii wierszy w tabeli rozłożone powinny być równomiernie przez wszystkich przydziałów.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Jednak jeśli kwerendy magazynu danych SQL Azure dynamiczne widoki zarządzania (DMV) można wykonywać bardziej szczegółowej analizy.  Aby rozpocząć, Utwórz [dbo.vTableSizes][] widoku za pomocą SQL z artykuł[Omówienie] [Tabeli].  Po utworzeniu widoku Uruchom tę kwerendę do identyfikowania tabel, które mają więcej niż 10% danych pochylanie.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Rozwiązywanie danych skośność

Nie wszystkie skośność wystarcza, aby zagwarantować poprawki.  W niektórych przypadkach wydajności tabeli w niektórych kwerend może być większe niż uszkodzenie danych skośność.  Zdecydować, jeśli należy rozwiązać danych skośność w tabeli, zapoznaj się ile to możliwe, informacje o ilości danych i kwerend w z pracą.   Jednym ze sposobów Spójrz na wpływ funkcja skośność jest monitorowanie wpływu pochylanie na wydajność kwerendy, a w szczególności wpływ na czas kwerendy wykonać, aby ukończyć na poszczególnych dystrybucji, wykonaj czynności podane w artykule [Monitorowania kwerendy][] .

Rozpowszechnianie danych polega na znajdowania prawo równowagi między zminimalizowaniem pochylanie danych i zminimalizowania przenoszenia danych. Te mogą przeciwstawne cele, a czasem można zachowanie danych skośność w celu zmniejszenia przenoszenia danych. Na przykład gdy kolumnę rozkładu często jest udostępnionym kolumnie w sprzężeń i agregacje, można będzie można zminimalizować przenoszenia danych. O konieczności przenoszenia minimalnego danych może być większe niż wpływ danych pochylić. 

Typowe sposobem rozwiązania pochylanie danych jest ponownie utwórz tabelę z kolumną innego przydziału. Ponieważ nie można zmienić kolumnę rozkładu na podstawie istniejącej tabeli, tak, aby zmienić rozmieszczenie tabeli go, aby utworzyć je ponownie przy [[CTAS]].  Oto dwa przykłady sposobu rozwiązywania skośność danych:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Przykład 1: Ponownie utworzyć tabelę z nową kolumnę rozkładu

W tym przykładzie użyto [[CTAS]], aby ponownie utworzyć tabelę zawierającą kolumnę rozkładu innego skrótu. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Przykład 2: Ponowne tworzenie tabeli przy użyciu okrężnego rozkładu

W tym przykładzie użyto [[CTAS]], aby ponownie utworzyć tabelę z okrężnego zamiast rozkładu skrótu. Ta zmiana zostanie zwrócona danych Rozdziel związany z przepływu danych. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat projektu tabeli, zobacz [Rozłóż][], [indeks][], [Partition][], [Typów danych][], [Statystyki][] i artykuły[tymczasowe] [Tabele tymczasowe].

Zawiera omówienie dotyczące najważniejszych wskazówek zobacz [Najważniejsze wskazówki magazynu danych SQL][].


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
[Monitorowanie kwerendy]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
