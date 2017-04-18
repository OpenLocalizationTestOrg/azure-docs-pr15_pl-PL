<properties
    pageTitle="Omówienie zapytań elastyczne bazy danych w usłudze Azure baza danych SQL | Microsoft Azure"
    description="Omówienie funkcji elastyczne kwerendy"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure baza danych SQL elastyczne baza danych kwerendy Przegląd (wersja preview)

Funkcja kwerendy elastyczne bazy danych (w wersji preview) umożliwia uruchomić kwerendę w języku Transact-SQL, która obejmuje wiele baz danych w bazie danych SQL Azure (SQLDB). Możliwość wykonywania kwerend między bazami danych, aby uzyskać dostęp do tabel zdalnych i na łączenie się narzędzia Microsoft i innych firm (Excel, PowerBI, Tableau, itd.), kwerenda wykonywana poziomów danych z wielu baz danych. Za pomocą tej funkcji można skalowania kwerendy do warstwy dużych danych w bazie danych SQL i wizualizowanie wyniki są wyświetlane w raportach analiz biznesowych.

## <a name="documentation"></a>Dokumentacja

* [Wprowadzenie do kwerend między bazami danych](sql-database-elastic-query-getting-started-vertical.md)
* [Raportowanie w chmurze skalowanej baz danych](sql-database-elastic-query-getting-started.md)
* [Kwerenda wykonywana w chmurze sharded baz danych (poziomo na partycje)](sql-database-elastic-query-horizontal-partitioning.md)
* [Kwerenda wykonywana w chmurze baz danych z różnych schematów (podzielona w pionie)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_wykonywanie \_zdalnego](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Dlaczego warto używać elastyczne zapytania?

**Baza danych SQL Azure**

Kwerenda wykonywana baz danych programu SQL Azure całkowicie w T-SQL. Dzięki temu tylko do odczytu kwerend zdalne bazy danych. Zapewnia opcji dla bieżącej lokalnego programu SQL Server klientów przeprowadzić migrację aplikacji przy użyciu trzech i czterech niepełnym nazw lub serwera połączonego bazy danych SQL.

**Dostępne w warstwie standardowy** Elastyczne kwerendy jest obsługiwana w warstwie standardowy wydajności oprócz warstwie wydajności Premium. W sekcji ograniczeń podglądu poniżej ograniczeń wydajności dla niższych poziomów wydajności.

**Przekazać do zdalnych baz danych**

Elastyczne zapytania można teraz push parametrów SQL do zdalnego baz danych do wykonania.

**Wykonywanie procedury składowanej**

Wykonywanie połączeń zdalnej procedury składowanej lub funkcji zdalnego za pomocą [sp\_wykonywanie \_zdalnego](https://msdn.microsoft.com/library/mt703714).

**Elastyczność**

Zewnętrzne tabele z kwerendy elastyczne teraz skrót w odniesieniu do tabel zdalnych przy użyciu innego schematu lub nazwa tabeli.

## <a name="elastic-database-query-scenarios"></a>Scenariusze kwerendy elastyczne bazy danych

Celem jest ułatwienie podczas badania scenariusze, w którym wiele baz danych do współtworzenia wierszy w jeden wynik ogólny. Kwerendy mogą się składać albo przez użytkownika lub aplikację bezpośrednio lub pośrednio za pomocą narzędzi, które są połączone z bazą danych. To jest szczególnie przydatne podczas tworzenia raportów przy użyciu narzędzia integracji komercyjnego BI lub danych — lub dowolnej aplikacji, której nie można zmienić. Przy użyciu kwerendy elastyczne kwerendy można przez kilka baz danych przy użyciu znajome środowisko łączności programu SQL Server w narzędzi, takich jak program Excel, PowerBI, Tableau lub Cognos.
Elastyczne zapytania umożliwia łatwy dostęp do całego zbioru baz danych za pomocą kwerendy wydawane przez program SQL Server Management Studio lub Visual Studio i ułatwia między bazami danych kwerendy z Framework jednostki lub innych środowisk ORM. Rysunek 1 pokazuje scenariusz miejsce, w którym tworzy istniejącą aplikację chmury (używający [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md)) w warstwie skalowanej danych i elastyczne kwerendy jest używane w raportach między bazami danych.

**Rysunek 1** Kwerenda elastyczne bazy danych używane w warstwie skalowanej danych

![Kwerenda elastyczne bazy danych używane w warstwie skalowanej danych][1]

Scenariusze klienta kwerend elastyczne są określony przez następujących topologii:

* **Pionowy podział — kwerendy między bazami danych** (Topologii 1): danych jest podzielona w pionie między liczbą baz danych w warstwie danych. Zazwyczaj różnych zestawów tabelach znajdują się na różnych baz danych. Oznacza to, że schemat jest inaczej na różnych baz danych. Na przykład wszystkie tabele dla zapasów są w jednej bazie danych podczas wszystkich tabel powiązanych księgowe znajdują się na innej bazy danych. Typowe przypadki użycia z tej topologii wymagane do kwerenda wykonywana lub skompilować raportów tabel w kilku baz danych.
* **Poziomy podział — Sharding** (Topologii 2): danych jest podzielona w poziomie do rozpowszechniania wierszy między skalować się danych warstwy. Z tej metody schemat jest identyczne na wszystkich uczestniczących baz danych. Ta metoda jest nazywany "sharding". Sharding mogą być wykonywane i zarządzanych (1 elastyczne bazy danych za pomocą narzędzia bibliotek lub (2) na własny sharding. Elastyczne kwerenda jest używana do kwerendy lub opracowania raportów przez wielu odłamki.

> [AZURE.NOTE] Elastyczne kwerendę najlepiej pasuje do rzadkie raportowania scenariusze, w którym większość przetwarzania mogą być wykonywane w warstwie danych. W przypadku złożonych zadań raportowania lub scenariuszy magazynowanie danych z bardziej złożonych kwerend również warto rozważyć użycie [Magazynu danych SQL Azure](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastyczne topologii kwerendy bazy danych

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologia 1: Pionowy podział — między bazami danych kwerendy

Aby rozpocząć kodowania, zobacz [Wprowadzenie do kwerend między bazami danych (pionowy podziału)](sql-database-elastic-query-getting-started-vertical.md).

Elastyczne kwerendy można nawiązać dane znajdujące się w bazie danych SQLDB dostępne dla innych baz danych SQLDB. Dzięki temu kwerendy z jednej bazy danych, aby odwołać się do tabel w inne zdalnego SQLDB bazy danych. Pierwszym krokiem jest określenie zewnętrznego źródła danych dla każdego zdalna baza danych. Zewnętrzne źródło danych jest zdefiniowane w lokalnej bazy danych, z którego chcesz uzyskać dostęp do tabel znajdujących się w zdalna baza danych. Żadne zmiany są konieczne na zdalna baza danych. W przypadku typowego pionowego podziału scenariuszy której różnych baz danych mają różne schematy elastyczne kwerend może służyć do wdrożenia typowych przypadków użycia, takie jak dostęp do danych źródłowych i między bazami danych kwerendy.

**Dane źródłowe**: 1 topologii jest używana do zarządzania danymi odwołania. Na poniższej ilustracji dwóch tabel (T1 i T2) z danych źródłowych są przechowywane w dedykowanej bazie danych. Przy użyciu kwerendy elastyczne, możesz teraz otwierać tabele T1 i T2 zdalnie z innej bazy danych, jak pokazano na ilustracji. Użyj topologii 1 w przypadku tabel odwołań small Business lub zdalnego kwerendy do tabeli odwołanie ma selektywne orzeczenia.

**Rysunek 2** Pionowy podział — przy użyciu danych elastyczne odwołanie do kwerendy

![Pionowy podział — przy użyciu danych elastyczne odwołanie do kwerendy][3]

**Kwerenda między bazami danych**: elastyczne kwerendy Włącz przypadków użycia, które wymagają wykonywanie zapytań w kilku SQLDB baz danych. Rysunek 3 zawiera cztery różne bazy danych: CRM, zapasów, HR i produktów. Kwerendy wykonywane w jednym z bazy danych także muszą mieć dostęp do jednego lub wszystkich innych baz danych. Korzystanie z kwerendy elastyczne, można skonfigurować bazy danych dla tej sprawy, uruchamiając kilka prostych instrukcji DDL dla wszystkich czterech baz danych. Po tej konfiguracji jednorazowego dostęp do tabeli zdalnej jest tak proste jak odwołujące się do lokalnej tabeli z kwerendy T SQL lub narzędzi do analizy Biznesowej. Ta metoda jest zalecane, jeśli zdalnych kwerend nie zwracają wyników duży.

**Rysunek 3** Pionowy podział — przy użyciu elastyczne kwerendy do różnych baz danych

![Pionowy podział — przy użyciu elastyczne kwerendy do różnych baz danych][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologia 2: Poziomy podział — sharding

Wykonywanie zadań raportowania nad sharded, to znaczy, poziomo na partycje, za pomocą kwerendy elastyczne warstwy danych wymaga [mapy shard elastyczne bazy danych](sql-database-elastic-scale-shard-map-management.md) , aby reprezentować baz danych w warstwie danych. Zazwyczaj mapy pojedynczy shard jest używana w tym scenariuszu i dedykowane bazy danych za pomocą kwerendy elastyczne możliwości służy jako punkt wejścia raportowaniu kwerendy. Tylko ta dedykowane baza danych musi mieć dostęp do mapy shard. Rysunek 4 ilustruje tej topologii i konfiguracji z planem elastyczne kwerendy bazy danych i shard. Bazy danych w warstwie danych może być wersji bazy danych SQL Azure lub edition. Aby uzyskać więcej informacji o bibliotece klienta elastyczne bazy danych i tworzenia map shard zobacz [Shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md).

**Rysunek 4** Poziomy podział — przy użyciu kwerendy elastyczne raportowaniu nad poziomów sharded danych

![Poziomy podział — przy użyciu kwerendy elastyczne raportowaniu nad poziomów sharded danych][5]

> [AZURE.NOTE] Baza danych kwerendy dedykowane elastyczne bazy danych musi być bazy danych w wersji 12 bazy danych SQL. Nie ma żadnych ograniczeń na odłamki, się.

Aby rozpocząć kodowania, zobacz [Wprowadzenie do zapytań elastyczne bazy danych do poziomej podziału (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Wykonywania kwerend elastyczne bazy danych

W poniższych sekcjach omówiono kroki w celu wykonania kwerendy elastycznych scenariuszy podziału pionowe i poziome. Odnoszą się również do bardziej szczegółowej dokumentacji w różnych scenariuszach podziału.

### <a name="vertical-partitioning---cross-database-queries"></a>Pionowy podział - między bazami danych kwerendy

Poniższe czynności należy skonfigurować kwerendy elastyczne bazy danych dla pionowego podziału scenariuszy, które wymagają dostępu do tabeli znajduje się na zdalnym SQLDB bazy danych przy użyciu tego samego schematu:

*    [Tworzenie klucza](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Tworzenie bazy danych występujące POŚWIADCZEŃ](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Tworzenie i UPUŚĆ zewnętrznego źródła danych](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **RDBMS**
*    [Tworzenie i UPUŚĆ tabeli zewnętrznej](https://msdn.microsoft.com/library/dn935021.aspx) Moja_tabela

Po uruchomieniu instrukcji DDL, tak, jakby była lokalnej tabeli można uzyskać dostęp do tabeli zdalnej "Moja_tabela". Baza danych SQL Azure automatycznie otwiera połączenie zdalna baza danych, przetwarza wezwanie na zdalna baza danych i zwraca wynik.
Więcej informacji na temat kroki wymagane do pionowego podziału scenariusza można znaleźć w [elastyczną kwerendy do podziału w pionie](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Poziomy podział - sharding

Poniższe czynności należy skonfigurować kwerendy elastyczne bazy danych dla poziomy podziału scenariuszy, które wymagają dostępu do wielu tabel znajdujących się na (zwykle) kilku zdalnych SQLDB baz danych:

*    [Tworzenie klucza](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Tworzenie bazy danych występujące POŚWIADCZEŃ](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Tworzenie [mapy shard](sql-database-elastic-scale-shard-map-management.md) reprezentującą do warstwy danych przy użyciu Biblioteka klienta elastyczne bazy danych.   
*    [Tworzenie i UPUŚĆ zewnętrznego źródła danych](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **SHARD_MAP_MANAGER**
*    [Tworzenie i UPUŚĆ tabeli zewnętrznej](https://msdn.microsoft.com/library/dn935021.aspx) Moja_tabela

Po wykonaniu tych kroków, masz dostęp do tabeli poziomie podzielone na partycje "Moja_tabela" tak, jakby była lokalnej tabeli. Baza danych SQL Azure automatycznie otwiera wielu równoległych połączeń zdalnych baz danych, której tabele są przechowywane fizycznie, przetwarzania wezwań na zdalnych baz danych i zwraca wynik.
Więcej informacji na temat kroki wymagane do poziomej scenariusza podziału można znaleźć w [elastyczną kwerendy do podziału w poziomie](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL kwerendy
Po zdefiniowaniu z zewnętrznych źródeł danych i zewnętrznych tabel umożliwia regularne parametry połączenia programu SQL Server nawiązywanie połączenia z bazy danych w miejsce, w którym zdefiniowane zewnętrznych tabel. Następnie możesz uruchomić instrukcji T SQL w tabelach zewnętrznych na to połączenie z ograniczeniami przedstawionych poniżej. Dodatkowe informacje i przykłady T-SQL kwerendy można znaleźć w dokumentacji tematy na [partycje pozioma](sql-database-elastic-query-horizontal-partitioning.md) i [Pionowa podziału](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Łączność dla narzędzi
Zwykłe parametry połączenia programu SQL Server umożliwia łączenie aplikacji i narzędzia integracji BI lub danych do bazy danych, które mają tabele zewnętrzne. Upewnij się, czy program SQL Server jest obsługiwany jako źródła danych narzędzia. Po nawiązaniu połączenia można znaleźć elastyczne kwerendy bazy danych i tabele zewnętrzne w bazie danych tak, jak chcesz zrobić z dowolnego innego programu SQL Server bazę danych, którą możesz połączyć się z narzędziem.

> [AZURE.IMPORTANT] Uwierzytelnianie za pomocą usługi Azure Active Directory z kwerendami elastyczne nie jest obecnie obsługiwane.

## <a name="cost"></a>Koszt

Elastyczne kwerendy jest uwzględniana w koszt baz danych bazy danych SQL Azure. Należy zauważyć, że topologii umiejscowienia zdalnego baz danych w centrum danych innym niż punkt końcowy elastyczne kwerendy są obsługiwane, ale dane wyjściowe ze zdalnych baz danych są naliczane zwykłe [Azure stawki](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Ograniczenia w wersji Preview
* Z pierwszej kwerendy elastyczne może potrwać do kilka minut na warstwie zadania. Ten czas jest konieczne załadowanie funkcjonalności elastyczne kwerendy. zwiększa wydajność ładowania podczas z wyższych poziomów wydajności.
* Wykonywanie skryptów zewnętrznych źródeł danych lub zewnętrzne tabele z SSMS lub SSDT nie jest jeszcze obsługiwane.
* Importuj/Eksportuj do bazy danych SQL nie obsługuje jeszcze zewnętrznych źródeł danych i tabele zewnętrzne. Jeśli musisz użyć Importuj/Eksportuj upuść tych obiektów przed wyeksportowaniem, a następnie odtworzyć je po zaimportowaniu.
* Elastyczne kwerendę obecnie obsługuje tylko dostęp tylko do odczytu do tabel zewnętrznych. Można jednak, używając pełną funkcjonalność T-SQL w bazie danych miejsce, w którym jest definiowana tabeli zewnętrznej. Może to być przydatne, aby np utrzymują tymczasowych wyników przy użyciu, np., wybierz OPCJĘ < column_list > do < local_table > lub Definiowanie procedur składowanych elastyczne kwerendy bazy danych, które odwołują się do tabel zewnętrznych.
* Z wyjątkiem nvarchar(max) typy LOB nie są obsługiwane w definicji tabeli zewnętrznej. Obejść ten problem można utworzyć widok Zdalna baza danych rzutuje LOB wpisywanego nvarchar(max), definiowanie tabeli zewnętrznej w widoku zamiast tabeli podstawowej i zrzutowania go do oryginalnego typu LOB w kwerendach.
* Statystyki kolumny na zewnętrzne tabele nie są obecnie obsługiwane. Statystyki tabele są obsługiwane, ale muszą zostać utworzone ręcznie.

## <a name="feedback"></a>Opinie
Należy udostępnić opinie dotyczące środowiska elastyczne kwerendy z nami na Disqus poniżej, na forum w witrynie MSDN lub zdarzeń Stackoverflow. Firma Microsoft interesuje wszystkie rodzaje opinii na temat usługi (wady, nierówne krawędzie, funkcja przerw).

## <a name="more-information"></a>Więcej informacji

Więcej informacji na temat kwerend między bazami danych i pionowego podziału scenariusze można znaleźć w następujących dokumentach:

* [Badanie między bazami danych i pionowego podziału — omówienie](sql-database-elastic-query-vertical-partitioning.md)
* Wypróbuj nasz samouczek krok po kroku, aby pełny przykład pracy działa w minutach, po upływie: [Wprowadzenie do kwerend między bazami danych (pionowy podziału)](sql-database-elastic-query-getting-started-vertical.md).


Więcej informacji na temat poziomy podział i sharding scenariusze jest dostępny tutaj:

* [Poziomy podział i sharding — omówienie](sql-database-elastic-query-horizontal-partitioning.md)
* Wypróbuj nasz samouczek krok po kroku, aby pełny przykład pracy działa w minutach, po upływie: [Wprowadzenie do zapytań elastyczne bazy danych do poziomej podziału (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
