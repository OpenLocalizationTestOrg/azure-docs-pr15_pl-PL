<properties
   pageTitle="Ograniczenie pojemności magazynu danych SQL | Microsoft Azure"
   description="Maksymalna liczba wartości dla połączenia, baz danych, tabel i kwerend dla magazynu danych SQL."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>Ograniczenie pojemności magazynu danych SQL

W poniższej tabeli wymieniono wartości maksymalne dozwolone dla różnych elementów magazynu danych SQL Azure.


## <a name="workload-management"></a>Zarządzanie pracą

| Kategoria            | Opis                                       | Maksymalna            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Jednostki magazynu danych (DWU)][]| Maksymalna liczba DWU dla pojedynczego magazynu danych SQL | 6000               |
| [Jednostki magazynu danych (DWU)][]| Maksymalna liczba DWU dla pojedynczego programu SQL server         | 6000 domyślnie<br/><br/> Domyślnie każdy programu SQL server (np. myserver.database.windows.net) ma normę DTU 45,000, dzięki czemu maksymalnie 6000 DWU. Przydział jest po prostu limit bezpieczeństwo. Można zwiększyć przydziału przez [Tworzenie bilet pomocy technicznej][] i wybranie *przydziału* jako typ żądania.  Do obliczania usługi DTU potrzebom, należy pomnożyć 7.5 przez razem, gdy potrzebne DWU. Możesz wyświetlać Twojej bieżącej zużycia DTU z karta SQL server w portalu. Zarówno wstrzymanych i usuwając wstrzymanych baz danych są wliczane do przydziału DTU. |
| Połączenie z bazą danych | Równoczesne otwartych sesji                          | 1024<br/><br/>Firma Microsoft obsługuje maksymalnie 1024 aktywne połączenia, z których każdy będzie przesyłanie żądania z bazą danych SQL Data Warehouse w tym samym czasie. Zwróć uwagę, że są ograniczenia dotyczące liczba kwerend, które rzeczywiście mogą być wykonywane jednocześnie. W przypadku przekroczenia limitu współbieżności, żądanie przechodzi do wewnętrznej kolejce, gdzie oczekuje przetwarzanie.|
| Połączenie z bazą danych | Maksymalna liczba pamięci dla przygotowanych instrukcji            | 20 MB              |
| [Zarządzanie pracą][] | Maksymalna liczba kwerend równoczesne                    | 32<br/><br/> Domyślnie magazynu danych SQL można wykonywać maksymalnie 32 równoczesne kwerendy i kolejek pozostała kwerendy.<br/><br/>Poziom współbieżności może się zmniejszyć, gdy użytkownicy są przypisane wyższej klasy zasobu lub SQL Data Warehouse skonfigurowano DWU niska. Niektóre kwerendy, takich jak kwerendy DMV, zawsze można uruchamiać.|
| [Tymczasowe][]          | Maksymalny rozmiar tymczasowe                                | 399 GB na DW100. W związku z tym na tymczasowe DWU1000 rozmiarem 3,99 TB |


## <a name="database-objects"></a>Obiekty bazy danych

| Kategoria          | Opis                                  | Maksymalna            |
| :---------------- | :------------------------------------------- | :----------------- |
| Bazy danych          | Maksymalny rozmiar                                     | 240 TB skompresowany na dysku<br/><br/>Niezależnie od miejsca na tymczasowe lub dziennik jest to miejsce i w związku z tym to miejsce jest przeznaczona do tablic stałych.  Kompresowanie grupowany columnstore szacuje się na 5 X.  Ten kompresji umożliwia bazy danych w celu usprawnienia około 1 PB, gdy wszystkie tabele są grupowane columnstore (domyślny typ tabeli).|
| Tabela             | Maksymalny rozmiar                                     | 60 TB skompresowany na dysku   |
| Tabela             | Tabele na bazę danych                          | miliardów 2          |
| Tabela             | Kolumn w tabeli                            | 1024 kolumn       |
| Tabela             | Bajtów na kolumnę                             | W zależności od kolumny [Typ danych][].  Limit wynosi 8000 danych typu char, 4000 dla nvarchar lub 2 GB danych typu MAX.|
| Tabela             | Bajtów na wiersz, zdefiniowany rozmiar                  | 8060 bajtów<br/><br/>Liczba bajtów na wiersz jest obliczana w taki sam sposób, podobnie jak w przypadku programu SQL Server przy użyciu kompresji strony. Przykład programu SQL Server SQL Data Warehouse obsługuje magazynowanie przepełnienia wierszy, dzięki czemu mogą być przesunięcie poza wiersz **kolumn zmiennej długości** . Zmienna długość wiersze są przesunięcie poza wiersz, tylko 24-bajtowa główny jest przechowywany w głównym rekordzie. Aby uzyskać więcej informacji zobacz artykuł w witrynie MSDN [przepełnienia wierszy danych przekraczają 8 KB][] .|
| Tabela             | Partycje na tabelę                    | 15 000<br/><br/>Wysoka wydajność, zaleca się zminimalizować liczbę podziałów potrzebne podczas nadal pomocniczych potrzeb biznesowych. W miarę liczbę partycje obciążenia operacje język DDL (Data Definition) i manipulowania języka DML (Data) rozwoju i powoduje spadek wydajności.|
| Tabela             | Znaków partition obramowanie wartości.| 4000 znaków |
| Indeks             | Wykres kolumnowy grupowany niebędący indeksów na tabelę.        | 999<br/><br/>Dotyczy tylko tabel rowstore.|
| Indeks             | Grupowany indeksów na tabelę.            | 1<br><br/>Dotyczy zarówno rowstore, jak i columnstore tabel.|
| Indeks             | Rozmiar klucza indeksu.                          | 900 bajtów.<br/><br/>Dotyczy tylko indeksy rowstore.<br/><br/>Jeśli istniejące dane w kolumnach nie przekracza 900 bajtów po utworzeniu indeksu można utworzyć indeksów kolumn varchar o maksymalnym rozmiarze więcej niż 900 bajtów. Jednak później WSTAWIAĆ lub AKTUALIZUJ akcje w kolumnach, powodujące całkowity rozmiar przekracza 900 bajtów zakończy się niepowodzeniem.|
| Indeks             | Kolumny klucza na indeks.                   | 16<br/><br/>Dotyczy tylko indeksy rowstore. Indeksy grupowany columnstore zawierać wszystkie kolumny.|
| Statystyki        | Rozmiar wartości Scalonej kolumny.      | 900 bajtów.         |
| Statystyki        | Kolumny dla każdego obiektu statystyki.           | 32                 |
| Statystyki        | Statystyki tworzone na kolumn w tabeli. | 30 000            |
| Procedury składowane | Maksymalna liczba poziomów zagnieżdżenia.               | 8                 |
| Widok              | Kolumn na widok                         | 1024             |


## <a name="loads"></a>Obciążenia

| Kategoria          | Opis                                  | Maksymalna            |
| :---------------- | :------------------------------------------- | :----------------- |
| Obciążenia Polybase    | Bajtów na wiersz                                | 32 768<br/><br/>Polybase obciążenia są ograniczone do ładowania wierszy obu mniejszych niż 32 KB i nie można załadować VARCHR(MAX), NVARCHAR(MAX) lub VARBINARY(MAX).  Gdy ten limit istnieje już dziś, zostanie on usunięty dość szybko.<br/><br/>


## <a name="queries"></a>Kwerendy

| Kategoria          | Opis                                  | Maksymalna            |
| :---------------- | :------------------------------------------- | :----------------- |
| Kwerendy             | Kolejce kwerendy dla tabel użytkownika.               | 1000.               |
| Kwerendy             | Równoczesne kwerendy w widokach systemu.          | 100                |
| Kwerendy             | Kolejce kwerend w widokach systemu               | 1000.               |
| Kwerendy             | Maksymalna liczba parametrów                           | 2098               |
| Partii             | Maksymalny rozmiar                                 | 65, 536 * 4096        |
| Wybierz pozycję wyników    | Kolumn w poszczególnych wierszach                              | 4096<br/><br/>W wyniku wybierz pozycję nigdy nie może być używana więcej niż 4096 kolumn w poszczególnych wierszach. Nie jest gwarantowana, że możesz zawsze mieć 4096. Jeśli plan zapytania wymaga tabeli tymczasowej, może zastosować 1024 kolumn w tabeli maksymalna.|
| WYBIERZ POZYCJĘ            | Podkwerendy zagnieżdżone                            | 32<br/><br/>W instrukcji SELECT nigdy nie może być używana więcej niż 32 podkwerendy zagnieżdżone. Nie jest gwarantowana, że możesz zawsze mieć 32. Na przykład sprzężenia można wprowadzać podkwerendy do planu kwerend. Liczba podkwerend również mogły zostać ograniczone przez dostępną pamięć.|
| WYBIERZ POZYCJĘ            | Kolumn na sprzężenia                             | 1024 kolumn<br/><br/>W sprzężenia nigdy nie może być używana więcej niż 1024 kolumn. Nie jest gwarantowana, że możesz zawsze mieć 1024. Jeśli plan sprzężenia wymaga tymczasowe Tabela zawierająca więcej kolumn niż wynik sprzężenia, 1024 limit dotyczy tabeli tymczasowej. |
| WYBIERZ POZYCJĘ            | Liczbę bajtów na Grupuj według kolumn.                  | 8060<br/><br/>Kolumny w klauzuli GROUP BY może zawierać maksymalnie 8060 bajtów.|
| WYBIERZ POZYCJĘ            | Bajtów w kolejności według kolumn                   | 8060 bajtów.<br/><br/>Kolumny w klauzuli ORDER BY może zawierać maksymalnie 8060 bajtów.|
| Identyfikatory i stałych na instrukcję | Liczba identyfikatorów odwołania i stałe. | 65 535<br/><br/>Program SQL Data Warehouse ograniczyła liczbę identyfikatorów i stałych, które mogą być zawarte w wyrażeniu pojedynczej kwerendy. Ten limit wynosi 65 535. Powyżej tego numeru spowoduje błąd programu SQL Server 8632. Aby uzyskać więcej informacji, zobacz [Błąd wewnętrzny: Osiągnięto limit usług wyrażenie][].|


## <a name="metadata"></a>Metadane

| Widok systemowy                        | Maksymalna liczba wierszy |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10 000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Łączna liczba pracowników systemie dla ostatnio 1000 żądań SQL. |
| sys.dm_pdw_errors                  | 10 000       |
| sys.dm_pdw_exec_requests           | 10 000       |
| sys.dm_pdw_exec_sessions           | 10 000       |
| sys.dm_pdw_request_steps           | Całkowita liczba procedura ostatnio żądania SQL 1000, które są przechowywane w sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs           | 10 000       |
| sys.dm_pdw_sql_requests            | Ostatnio żądania SQL 1000 przechowywanych w sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacje zobacz [Omówienie magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Jednostki magazynu danych (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Omówienie magazynu danych SQL]: ./sql-data-warehouse-overview-reference.md
[Zarządzanie pracą]: ./sql-data-warehouse-develop-concurrency.md
[Tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[Typ danych]: ./sql-data-warehouse-tables-data-types.md
[Tworzenie bilet pomocy technicznej]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Dane przepełnienia wiersza przekraczają 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Błąd wewnętrzny: Osiągnięto limit usług wyrażenia]: https://support.microsoft.com/kb/913050
