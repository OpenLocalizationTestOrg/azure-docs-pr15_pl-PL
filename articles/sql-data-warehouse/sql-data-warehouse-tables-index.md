<properties
   pageTitle="Indeksowanie tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do tabeli indeksowania w magazynie danych SQL Azure."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Indeksowanie tabel w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Magazynu danych SQL oferuje kilka opcji indeksowania, takich jak [Indeksy grupowany columnstore][], [Indeksy grupowany i zbudowania indeksów][].  Ponadto również oferuje indeks opcja Brak nazywane także [stosu][].  W tym artykule opisano, jak korzyści wynikających z każdego typu indeksu, a także porady dotyczące uzyskiwania z usługi indeksy większość optymalny. Zobacz [Tworzenie tabeli składni][] , aby uzyskać więcej szczegółów na temat tworzenia tabeli w magazynie danych SQL.

## <a name="clustered-columnstore-indexes"></a>Indeksy columnstore grupowany

Domyślnie magazynu danych SQL tworzy indeks grupowany columnstore Brak opcji indeksu są określane w tabeli. Tabele columnstore grupowany oferują zarówno na najwyższym poziomie kompresji danych, jak i zalecane ogólną wydajność kwerendy.  Tabele columnstore grupowany zazwyczaj będzie przewyższyć grupowany indeks lub stosu tabel i zazwyczaj jest to najlepszy wybór dla dużych tabel.  Z tego powodu grupowany columnstore najlepiej zacząć, gdy masz pewności, jak indeks tabeli.  

Aby utworzyć tabelę grupowany columnstore, po prostu określić GRUPOWANY indeks COLUMNSTORE w klauzuli WITH lub pozostawić klauzuli WITH wyłączone:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Istnieje kilka scenariuszy, w którym grupowany columnstore może nie być dobrym rozwiązaniem:

- Tabele Columnstore nie obsługują indeksów pomocniczych-grupowany.  Zamiast tego warto rozważyć sterta lub indeks grupowany tabel.
- Tabele Columnstore nie obsługują varchar(max), nvarchar(max) i varbinary(max).  Zamiast tego warto rozważyć sterta lub indeks grupowany.
- Tabele Columnstore może być mniej wydajna przejściowych danych.  Należy rozważyć, czy stosu i czasami nawet tymczasowe tabele.
- Małe tabele z mniej niż 100 milionów wierszy.  Należy rozważyć, czy tabele stosu.

## <a name="heap-tables"></a>Tabele stosu

Gdy dane są tymczasowo wyładunku w magazynie danych SQL, może się okazać, że przy użyciu tabeli stosu spowoduje, że całego procesu szybciej.  Jest to spowodowane obciążenia do stert są szybciej niż do tabel indeks, a w niektórych przypadkach można przeprowadzić kolejnych odczytu z pamięci podręcznej.  Jeśli są ładowane dane tylko do etapu ją przed uruchomieniem więcej przekształcenia, ładowania do stosu tabeli będzie szybciej niż ładowanie danych do tabeli columnstore grupowany. Ponadto ładowania danych do [tabeli tymczasowej][tymczasowe] będzie również znacznie szybsza niż ładowanie tabeli w celu stałego miejsca do magazynowania.  

Dla tabel odnośników małych, mniej niż 100 milionów wierszy, często tabel stosu miały znaczenie.  Tabele columnstore klaster rozpocząć uzyskanie optymalnej kompresji po więcej niż 100 milionów wierszy.

Aby utworzyć tabelę stosu, po prostu określić STOSU w klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Indeksy grupowany i zbudowania

Indeksy grupowany może przewyższyć grupowany columnstore tabel, po jednym wierszu trzeba szybko pobrać.  Dla kwerendy, gdzie jest wymagane do wydajności za pomocą skrajnych szybkość jednym lub niewielka liczba odnośników wiersza należy rozważyć, czy indeksu klaster lub zbudowania pomocniczej indeksu.  Wadą korzystania z indeksem grupowany jest skorzystają tylko kwerend, które zastosować filtr wysoce selektywne na kolumnę indeksu grupowany.  Aby poprawić filtru na innych kolumn zbudowania indeksu można dodawać do innych kolumn.  Jednak każdy indeks, który został dodany do tabeli doda zarówno miejsca i czas przetwarzania na obciążenia.

Aby utworzyć tabelę grupowany indeks, po prostu określić GRUPOWANY INDEKSU w klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Aby dodać indeks grupowany w tabeli, po prostu użyć następującej składni:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Indeks bez klastrów jest domyślnie tworzone użycia CREATE INDEX. Ponadto indeks bez klastrów jest dozwolony tylko w tabeli Wiersz miejsca do magazynowania (STOSU lub wykres kolumnowy GRUPOWANY indeks). Indeksy nie grupowany na bieżąco GRUPOWANY indeks COLUMNSTORE nie jest dozwolona w tej chwili.


## <a name="optimizing-clustered-columnstore-indexes"></a>Optymalizacja indeksy columnstore grupowany

Tabele grupowany columnstore są zorganizowane w danych na segmenty.  Niezwykle ważne osiągnięcie optymalnego działania zapytań w tabeli columnstore jest o części wysokiej jakości.  Jakość części można zmierzyć przez liczbę wierszy w grupie skompresowany wiersza.  Jakość segmentu jest najbardziej optymalny, gdy istnieją co najmniej 100 KB wierszy w wierszu skompresowany grupowania i uzyskać wydajności jako liczba wierszy w ujęciu grupowym wiersza 1 048 576 wierszy, która jest większość wierszy, które mogą zawierać grupy wierszy.

Poniżej pozycji widok można tworzyć i używany w systemie do obliczenia średniej wierszy w wierszu grupowania i identyfikowanie indeksach columnstore częściowej optymalnego klaster.  Ostatniej kolumny w tym widoku wygeneruje jako instrukcję SQL, które mogą być używane odbudowanie usługi indeksy.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Teraz, gdy został utworzony widoku, kwerenda, aby zidentyfikować tabele z grupy wierszy z mniej niż 100 KB wierszy.  Oczywiście można zwiększyć progu 100 KB, jeśli szukasz więcej jakości optymalnego części. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Po uruchomieniu kwerendy, którą można rozpocząć przeglądać dane i analizowanie wyników. W poniższej tabeli opisano, co należy szukać podczas analizy grupy wierszy.


| Kolumny                             | Jak używać tych danych                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Jeśli tabela jest podzielony na partycje, może oczekiwać wyświetlić zlicza wyższej grupy Otwórz wiersz. Każdy partition w dystrybucji może teoretycznie Otwórz wiersz grupa być skojarzone z nim. Współczynnik to do analiz. Niewielka tabela, która jest podzielony na partycje może być zoptymalizowany, usuwając całkowicie podziału zgodnie z efektem czy kompresji.                                                                        |
| [row_count_total]                  | Całkowita liczba wierszy w tabeli. Za pomocą tej wartości można na przykład obliczyć procent wierszy w stanie skompresowany.                                                                      |
| [row_count_per_distribution_MAX]   | Jeśli wszystkie wiersze są równomiernie tę wartość będzie docelowej liczba wierszy na rozkładu. Porównanie tej wartości z compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Całkowita liczba wierszy w formacie columnstore dla tabeli.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Jeżeli średnia liczba wierszy jest znacznie mniej niż maksymalna liczba wierszy dla grupy wierszy, należy rozważyć użycie CTAS lub zmienić ODBUDOWANIE INDEKSU będzie danych                     |
| [COMPRESSED_rowgroup_count]        | Liczba grup wierszy w formacie columnstore. Jeśli ta liczba jest bardzo wysoki względem tabeli jest wskaźnik brakuje gęstości columnstore.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Wiersze logicznie są usuwane w formacie columnstore. Jeśli liczba jest wysoka względem rozmiar tabeli, rozważ ponownego tworzenia partycją lub odbudowanie indeksu, jak spowoduje to usunięcie ich fizycznie. |
| [COMPRESSED_rowgroup_rows_MIN]     | Umożliwia w połączeniu z kolumnami średnia i MAX opis zakres wartości dla grupy wierszy w swojej columnstore. Niską wartość ponad próg obciążenia (102400 na rozkład partition wyrównane) sugeruje, że optymalizacji są dostępne podczas ładowania danych                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Jak powyżej                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Otwórz wiersz grupy są normalny. Czy jedną spodziewać jedną grupę Otwórz wiersz na dystrybucję tabel (60). Numery nadmiarowe sugeruje ładowania przez partycje danych. Sprawdź podziału strategii się upewnić, że jest dźwięku                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Każdej grupy wierszy może być 1 048 576 wierszy w nim maksymalnie. Zobacz, jak pełny grupy Otwieranie wierszy są obecnie za pomocą tej wartości                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Otwieranie grupy wskazuje, że dane jest strumieniem ładowane do tabeli lub że poprzedniego Załaduj rozrzucone pozostałych wierszy do tej grupy wierszy. Używanie MIN, MAX, średnia kolumn w celu wyświetlania, ilości danych jest znajdowało się w Otwórz wiersz grupy. W przypadku małych tabel może to być 100% wszystkich danych! W takim przypadku zmiany ODBUDOWANIE INDEKSU, aby wymusić dane columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Jak powyżej                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Jak powyżej                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Odszukaj wiersz zamknięta grupowanie wierszy w celu sprawdzenia wykonuje.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Liczba grup zamknięty wiersza powinna być niska, jeśli dowolne są widoczne na wszystkich. Grupy zamknięty wierszy można konwertować roups rowg skompresowany za pomocą ALTER INDEX... REORGANIZACJA polecenia. Jednak nie jest to zazwyczaj wymagane. Zamknięte grupy są automatycznie konwertowane na grupy wierszy columnstore przez proces "krotki przenoszenia" tła.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Grupy wierszy zamknięte powinny mieć stawki wypełnienia bardzo wysoki. Jeśli stopa wypełnienia dla grupy zamknięty wierszy jest niska, dalszej analizy columnstore jest wymagane.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Jak powyżej                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Jak powyżej                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | Kod SQL, aby odbudowanie indeksu columnstore dla tabeli                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Przyczyny columnstore niskiej jakości indeksu

W przypadku zidentyfikowania tabele z części niskiej jakości, należy Określ przyczynę.  Poniżej wymieniono niektóre typowe przyczyny niskiej części quaility:

1. Ciśnienia pamięci, gdy został utworzony indeks
2. Dużą liczbę operacji DML
3. Małe lub ścieknie operacji ładowania
4. Zbyt wiele partycje

Czynniki te mogą powodować indeks columnstore znacznie ma wartość mniejszą niż optymalnego 1 miliona wierszy dla każdej grupy wierszy.  Może to powodować również wierszy przejść do grupy wierszy różnicy zamiast grupy skompresowany wierszy. 

### <a name="memory-pressure-when-index-was-built"></a>Ciśnienia pamięci, gdy został utworzony indeks

Liczba wierszy dla każdej grupy wierszy skompresowany są bezpośrednio związane z szerokość wiersza i ilości pamięci do procesu grupy wierszy.  Gdy wiersze są zapisywane w tabelach columnstore w obszarze pamięci, może być mogą być columnstore części jakości.  W związku z tym najlepszym rozwiązaniem jest udzielić sesji, które zapisuje dostępu columnstore indeksu tabel do tak dużej ilości pamięci, jak to możliwe.  Ponieważ istnieje zależność między pamięci i współbieżności, wytyczne dotyczące przydziału pamięci prawo zależy od danych w każdym wierszu tabeli, ilość DWU zostały przydzielone do systemu i ilość gniazd współbieżności, które można przekazać sesji, które zapisuje dane do tabeli.  Zgodnie z zaleceniami dotyczącymi zaleca się ciągiem z xlargerc, jeśli korzystasz z DW300 lub mniej largerc, jeśli używasz DW400 DW600 i mediumrc, jeśli korzystasz z DW1000 i powyżej.

### <a name="high-volume-of-dml-operations"></a>Dużą liczbę operacji DML

Dużej liczby operacje DML aktualizowanie i usuwanie wierszy można wprowadzać braku efektywności do columnstore. Jest szczególnie modyfikacji większość wiersze w grupie wiersza.

- Tylko w logiczny sposób usunięcia wiersza z grupy skompresowany wierszy oznacza wiersz jako usunięte. Wiersz pozostaje w grupy skompresowany wierszy, dopóki nie zostanie odtworzony partycją lub tabeli.
- Wstawiania wiersza dodaje wiersza do tabeli wewnętrznych rowstore o nazwie grupy wierszy różnicy. Wstawionym wierszu nie jest konwertowany na columnstore, aż grupy wierszy różnicy jest pełny i zostanie oznaczone jako zamknięte. Grupy wierszy są zamknięte po ich osiągnięcia maksymalnej pojemności 1 048 576 wierszy. 
- Aktualizacja wiersza w formacie columnstore jest przetwarzane jako logiczne Usuń, a następnie Wstaw. Wstawionym wierszu mogą być przechowywane w magazynie różnicy.

Przetwarzany wsadowo aktualizacji i wstawić operacji, które przekracza progu zbiorcze 102 400 wierszy na partition wyrównanymi rozkładu zostaną zapisane bezpośrednio do formatu columnstore. Jednak przy założeniu Rozdziel, trzeba modyfikować więcej niż miliona 6.144 wierszy w jednej operacji dla to możliwe. Jeśli wyrównane liczbę wierszy dla danego partition rozkład jest mniejsza niż 102400 wiersze będzie przejdź do sklepu różnicy i pozostanie tam do momentu wystarczających wierszy zostały wstawione lub modyfikować tak, aby zamknąć grupy wierszy lub zostały odbudowany indeks.

### <a name="small-or-trickle-load-operations"></a>Małe lub ścieknie operacji ładowania

Mały ładuje, że przepływ do magazynu danych SQL czasami nazywa się ścieknie obciążenia. Zazwyczaj reprezentują najbliższego stałej strumienia danych jest wchłonięte przez system. Jednak tym strumieniu znajduje się w pobliżu ciągły liczby wierszy nie jest szczególnie duży. Najczęściej dane są mocno poniżej wartości progowej, wymagane bezpośrednie obciążenia columnstore format.

W następujących sytuacjach warto często najpierw ziemi dane w magazynie obiektów blob platformy Azure i pozwolić na jego zebrać przed ładowania. Ta metoda jest często nazywane *tworzeniu partii micro*.

### <a name="too-many-partitions"></a>Zbyt wiele partycje

Niekiedy brać pod uwagę jest wpływ podziału w tabelach columnstore grupowany.  Przed dzielony, magazynu danych SQL już dzieli danych do 60 baz danych.  Dalsze podziału dzieli danych.  Jeśli dzielenia danych, następnie można brać pod uwagę, że **Każdy** partition muszą mieć co najmniej 1 miliona wierszy do korzystania z indeksu columnstore grupowany.  Jeśli tabela jest podziału na partycje 100, następnie tabeli muszą mieć co najmniej 6 miliardów wierszy do korzystania z indeksu grupowany columnstore (wiersze *partycje 100* milionów 1 dystrybucji 60). Jeśli tabela 100 partition nie zawiera wiersze 6 miliardów, Zmniejsz liczbę partycje lub tabeli stosu warstwowy.

Gdy tabel załadowano zawierającego dane, wykonaj kroki do identyfikowania i odbudowanie tabel z częściowej optymalnego klaster columnstore indeksy.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Odbudowywania indeksów, aby poprawić jakość części

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Krok 1: Identyfikowanie lub Utwórz użytkownika, który używa klasy zasobu w prawo

Szybkim sposobem natychmiast poprawić jakość segmentu jest odbudowanie indeksu.  Zwrócone przez powyżej widoku SQL zwróci instrukcji ALTER ODBUDOWANIE INDEKSU, które mogą być używane odbudowanie usługi indeksy.  Odbudowywanie usługi indeksy, należy pamiętać, przydzielenie pamięci do sesji, która będzie odbudowanie indeksu.  W tym celu należy zwiększyć klasy zasobu użytkownika, który ma uprawnienia do odbudowanie indeksu w tej tabeli zalecane minimum.  Nie można zmienić klasy zasobów użytkownika właściciela bazy danych, więc jeśli nie została utworzona przez użytkownika w systemie, konieczne będzie wykonanie tego najpierw.  Minimalna zalecane jest xlargerc Jeśli używasz DW300 lub mniej largerc Jeśli używasz DW400 DW600 i mediumrc, jeśli korzystasz z DW1000 i powyżej.

Oto przykładowy sposób przydzielić więcej pamięci do użytkownika, zwiększając jego klasy zasobu.  Aby uzyskać więcej informacji o zasobie klasy i sposobu tworzenia nowego użytkownika można znaleźć w [zarządzania współbieżności i obciążenie pracą] [ Concurrency] artykuł.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Krok 2: Odbudowanie indeksy columnstore grupowany z wyższymi użytkownika klasy zasobów
Zaloguj się jako użytkownika z kroku 1 (np. LoadUser), która jest teraz za pomocą wyższej klasy zasobów i wykonać instrukcje ALTER INDEX.  Upewnij się, że ten użytkownik ma uprawnienia ALTER do tabel, gdzie odbudowany indeks.  W tych przykładach pokazano, jak odbudowanie indeksu całego columnstore lub jak odbudować jedną partycją. W dużych tabel jest więcej praktycznych odbudowanie indeksy jedną partycją naraz.

Alternatywnie zamiast odbudowanie indeksu, możesz skopiować tabelę do nowej tabeli przy użyciu [CTAS][].  Jaki sposób jest najlepsza? Dla dużych ilości danych [CTAS][] jest zazwyczaj szybciej niż [ALTER INDEX][]. Mniejsze ilości danych [ALTER INDEX][] jest łatwiejsze w użyciu i nie trzeba zamieniać tabeli.  Więcej informacji na temat sposobu odbudowywania indeksów z CTAS, zobacz **Indeksy Rebuilding z CTAS i przełączanie partition** poniżej.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Odbudowanie indeksu w magazynie danych SQL jest operacji w trybie offline.  Aby uzyskać więcej informacji na temat odbudowywania indeksów sekcja zmiany ODBUDOWANIE INDEKSU w [Columnstore indeksy defragmentacji][] i tematu składnia [ALTER INDEX][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Krok 3: Sprawdź, czy występują zwiększenia jakości części columnstore grupowany
Uruchom kwerendę tabeli, która zidentyfikowanych z słabej segmentu jakości i sprawdź jakość części została ulepszona.  Jeśli nie poprawić jakość części, może to być, że wiersze w tabeli są bardzo szeroki.  Warto rozważyć użycie wyższej klasy zasobu lub DWU podczas odbudowywania indeksów usługi.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Odbudowywania indeksów z CTAS i przełączanie partition

W tym przykładzie użyto [CTAS][] i partycją przechodzenia do odbudowanie partycją tabeli. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Aby uzyskać więcej informacji o tworzeniu ponownie partycje za pomocą `CTAS`, zobacz artykuł [partycją][] .

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz artykuły w [Tabeli Przegląd][Przegląd], [Typy danych tabeli][Typów danych], [rozpowszechniania tabeli][Rozłóż], [podziału tabeli][Partition], [Zachowaniu statystyki tabeli][Statystyki] i [Tabele tymczasowe][tymczasowe].  Aby dowiedzieć się więcej o najważniejszych wskazówkach dotyczących, zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Omówienie]: ./sql-data-warehouse-tables-overview.md
[Typy danych]: ./sql-data-warehouse-tables-data-types.md
[Rozpowszechnianie]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Najważniejsze wskazówki magazynu danych SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[INSTRUKCJA ALTER INDEKSU]: https://msdn.microsoft.com/library/ms188388.aspx
[stos]: https://msdn.microsoft.com/library/hh213609.aspx
[indeksy grupowany i zbudowania indeksów]: https://msdn.microsoft.com/library/ms190457.aspx
[Tworzenie tabeli składni]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indeksy defragmentacji]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[indeksy columnstore grupowany]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
