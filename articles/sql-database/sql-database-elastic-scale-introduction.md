<properties
    pageTitle="Skalowanie zewnętrzne z bazy danych SQL Azure | Microsoft Azure"
    description="Oprogramowanie jako deweloperów usługi (władz akredytacji bezpieczeństwa) można łatwo tworzyć elastyczne, skalowalna baz danych w chmurze za pomocą tych narzędzi"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Skalowanie zewnętrzne z bazy danych SQL Azure

Można łatwo skalować się baz danych Azure SQL za pomocą narzędzia **Elastyczne bazy danych** . Te narzędzia i funkcje umożliwiają tworzenie rozwiązań dla transakcji obciążenia, a zwłaszcza oprogramowania jako aplikacjami usług (władz akredytacji bezpieczeństwa) za pomocą zasoby nieograniczoną bazy danych z **Bazy danych SQL Azure** . Elastyczne funkcje bazy danych składają się z następujących czynności:

* [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md): Biblioteka klienta to funkcja, która umożliwia tworzenie i obsługę sharded baz danych.  Zobacz [Rozpoczynanie pracy z narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md).
* [Narzędzie korespondencji seryjnej podziału elastyczne bazy danych](sql-database-elastic-scale-overview-split-and-merge.md): przenosi dane między sharded baz danych. To jest przydatne w przypadku przenoszenia danych z wielu dzierżawy bazy danych do bazy danych pojedyncze dzierżawy (lub odwrotnie). Zobacz [elastyczne baza danych korespondencji seryjnej podziału narzędzie samouczka](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastyczne bazy danych zadania](sql-database-elastic-jobs-overview.md) (wersja preview): Zarządzanie dużą liczbą baz danych programu SQL Azure za pomocą zadań. Łatwe wykonywania operacji administracyjnych, takich jak zmiany schematu, zarządzania poświadczeń, aktualizacje danych odwołania, zbieranie danych dotyczących wydajności lub zbioru telemetrycznego dzierżawy (klienta) przy użyciu zadań.
* [Kwerenda elastyczne bazy danych](sql-database-elastic-query-overview.md) (wersja preview): umożliwia uruchomić kwerendę w języku Transact-SQL, która obejmuje wiele baz danych. Dzięki temu połączenia w celu raportowania narzędzi, takich jak program Excel, PowerBI, Tableau itp.
* [Elastyczne transakcji](sql-database-elastic-transactions-overview.md): Ta funkcja umożliwia uruchamianie transakcje obejmujące wiele baz danych w bazie danych SQL Azure. Transakcje elastyczne bazy danych są dostępne dla aplikacji .NET przy użyciu ADO .NET i integracji z znajome środowisko programowania przy użyciu [klas System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx).

Poniższy rysunek przedstawia architekturę, która zawiera **Funkcje elastyczne bazy danych** względem zbiór baz danych.

Na tej ilustracji kolory bazy danych reprezentują schematów. Bazy danych z tym samym kolorem udostępnianie tego samego schematu.

1. Zestaw **baz danych programu SQL Azure** znajdują się w Azure za pomocą architektura sharding.
2. **Biblioteka klienta elastyczne bazy danych** jest używana do zarządzania zestawu shard.
3. Podzestaw bazy danych są umieszczane w **puli elastyczne bazy danych**. (Zobacz [Co to jest puli?](sql-database-elastic-pool.md)).
4. Uruchomienie **elastyczne bazy danych zadania** według harmonogramu lub ad hoc T-SQL skrypty wszystkich bazach danych.
5. **Narzędzie do korespondencji seryjnej podziału** jest używany do przenieść dane z jednej shard.
6. **Kwerenda elastyczne bazy danych** umożliwia pisanie kwerendę, która obejmuje wszystkie bazy danych w zestawie shard.
7. **Elastyczne transakcje** pozwala na uruchamianie transakcje obejmujące wiele baz danych. 


![Elastyczne narzędzia bazy danych][1]


## <a name="why-use-the-tools"></a>Dlaczego warto używać narzędzi?

Osiągnięcie elastyczność i skala dla aplikacje w chmurze została proste maszyny wirtualne i magazyn obiektów blob — po prostu Dodawanie lub odejmowanie jednostek lub zwiększanie power. Ale pozostają wezwanie stanowe przetwarzania danych w relacyjnych baz danych. Wyzwania pojawiło się w następujących sytuacjach:

* Rozwija i zmniejszanie możliwości części relacyjnej bazy danych z pracą.
* Zarządzanie punkty aktywne, które mogą się pojawić wpływających na określony podzestaw danych — takich jak szczególnie zajęty końcowego odbiorcy (dzierżawy).

Zazwyczaj scenariusze, takie jak te dotyczą poświęcania na serwerach dużą skalę bazy danych do obsługi aplikacji. Jednak ta opcja jest ograniczona w chmurze, gdzie przetwarzania się dzieje na sprzęcie wstępnie zdefiniowanych towaru. Zamiast tego dystrybucji danych i przetwarzanie przez wiele baz danych, które są identyczne strukturę (poza skalowanie deseniu znana pod nazwą "sharding") stanowi alternatywę dla tradycyjnych metod Skala up zarówno pod względem kosztów i elastyczność.

## <a name="horizontal-and-vertical-scaling"></a>Skalowanie poziome i pionowe

Na poniższym rysunku przedstawiono wymiary poziome i pionowe skalowania, które podstawowe sposoby, który może być skalowany elastyczne baz danych.

![Pozioma i pionowa Scaleout][2]

Skalowanie w poziomie odwołuje się do dodawania lub usuwania bazy danych w celu Dostosowywanie wydajności lub ogólnej wydajności. Jest to także nazywane "skalowania". Sharding, w którym danych jest podzielona przez zbiór identyczne strukturalnych bazy danych, to powszechne rozwiązanie w celu wdrożenia skalowanie w poziomie.  

Skalowanie pionowe odwołuje się do zwiększenie lub zmniejszenie poziom wydajności bazy danych pojedyncze — jest to także nazywane "Skalowanie wewnętrzne".

Większość aplikacji baz danych w chmurze skali użyje kombinacji tych dwóch strategii. Na przykład oprogramowanie jako aplikacji usługi może umożliwia skalowanie w poziomie do zapewniania obsługi nowych klientów końcowych i skalowanie pionowe możliwe bazą danych każdego odbiorcy końcowego powiększanie lub zmniejszanie zasobów, stosownie do potrzeb obciążenie pracą.

* Skalowanie w poziomie odbywa się za pośrednictwem [biblioteki klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md).

* Skalowanie pionowe jest wykonywane przy użyciu poleceń cmdlet środowiska PowerShell Azure zmienić warstwa usług lub przez umieszczenie baz danych w puli elastyczne.

## <a name="sharding"></a>Sharding

*Sharding* to technika do rozpowszechniania dużych ilości danych tak samo strukturę przez liczba niezależnych baz danych. Jest szczególnie popularne z chmury deweloperów tworzenie oprogramowania jako oferty usług (SAAS) klientów końcowych i firmy. Tych klientów końcowych są często nazywane "dzierżaw". Sharding mogą być wymagane na potrzeby różnych sytuacjach:  

* Całkowita ilość danych jest zbyt duża, aby dopasować w ramach ograniczeń jednej bazie danych
* Przepustowość transakcji ogólnego obciążenie pracą przekracza możliwości jednej bazie danych
* Dzierżaw może wymagać fizycznie oddzielnie, więc oddzielne bazy danych są wymagane dla każdego dzierżawy
* Różne części bazy danych może być konieczne znajdują się w różnych regionach zgodności, wydajności lub geopolitycznych powodów.

W innych sytuacjach, takie jak spożywanie danych z rozkładem urządzeń sharding może służyć do wypełnienia zestaw baz danych, które są zorganizowane tymczasowe. Na przykład oddzielnej bazy danych mogą być przeznaczone dla każdego dnia lub tygodnia. W takim przypadku klucz sharding może być liczbą całkowitą reprezentującą datę (istnieje we wszystkich wierszach tabel sharded) i kwerend pobieranie informacji dla zakresu dat muszą być kierowane przez aplikację do podzbioru baz danych obejmujących danego zakresu.

Sharding sprawdza się najlepiej, gdy każdej transakcji w aplikacji może być ograniczona do pojedynczej wartości klucza sharding. Która zapewnia wszystkie transakcje będą lokalne do bazy danych.

## <a name="multi-tenant-and-single-tenant"></a>Wiele dzierżawy i pojedyncze dzierżawy

Niektóre aplikacje używają najprostsze podejście tworzenia oddzielnej bazy danych dla każdego dzierżawy. Jest to **deseniu sharding jednej dzierżawy** , który udostępnia izolacji, możliwość tworzenia kopii zapasowych i przywracania i zasobów skalowania u szczegółowości dzierżawy. Z jednej dzierżawy sharding każdej bazy danych jest skojarzony z identyfikatorem określonych dzierżawy (lub wartości klucza klienta), ale tego klawisza nie muszą zawsze znajdować się w samych danych. Odpowiada aplikacji kierowanie każdego żądania do odpowiedniej bazy danych — i Biblioteka klienta można uprościć to.

![Pojedynczy dzierżawy i wielu dzierżawy][4]

Inne scenariusze pakietów kilka dzierżaw razem do baz danych, zamiast izolowanie ich do oddzielnych baz danych. Jest to typowy **Deseń wielu dzierżawy sharding** — i mogą być prowadzone przez fakt, że aplikacja zarządza dużej liczby dzierżaw bardzo mały. W sharding dzierżawy wielu wierszy w tabelach bazy danych wszystkie mają klucz, który identyfikuje identyfikator dzierżawy lub klucza sharding. Ponownie warstwie aplikacji jest odpowiedzialny za routing żądania dzierżawy do odpowiednią bazę danych, a to mogą być obsługiwane przez Biblioteka klienta elastyczne bazy danych. Ponadto zabezpieczenia na poziomie wiersza może służyć do filtrowania wierszy, które każdego dzierżawy dostępem — Aby uzyskać szczegółowe informacje, zobacz [dzierżawy wielu aplikacji przy użyciu narzędzia elastyczne bazy danych i zabezpieczeń na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md). Dystrybucji danych między bazami danych mogą być potrzebne ze wzorcem sharding wielu dzierżawy, a przebiega sprawniej, za pomocą narzędzia korespondencji seryjnej podziału elastyczne bazy danych. Aby dowiedzieć się więcej o wzorców projektu dla aplikacji władz akredytacji bezpieczeństwa za pomocą pul elastyczne, zobacz [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Przenoszenie danych z wielu do dzierżawy pojedyncze baz danych

Podczas tworzenia aplikacji władz akredytacji bezpieczeństwa, jest typowe do oferowania potencjalnych klientów wersję próbną oprogramowania. W tym przypadku jest efektywne pod względem kosztów korzystanie z dzierżawą wielu bazy danych dla danych. Jednak podczas dot, bazy danych pojedyncze dzierżawy lepiej jest ponieważ zapewnia lepszą wydajność. Jeśli klienta utworzono danych podczas okresu próbnego, użyj [Narzędzia do korespondencji seryjnej podziału](sql-database-elastic-scale-overview-split-and-merge.md) przenieść dane z wielu dzierżawy nowej bazy danych pojedyncze dzierżawy.

## <a name="next-steps"></a>Następne kroki

Dla aplikacji dla próbki zaprezentowano biblioteki klienta zobacz [Wprowadzenie do narzędzia elastyczną Datababase](sql-database-elastic-scale-get-started.md).

Aby przekonwertować istniejące bazy danych za pomocą narzędzi, zobacz [Migrowanie istniejących baz danych do skali w nowym oknie](sql-database-elastic-convert-to-use-elastic-tools.md).

Aby wyświetlić szczegółowe informacje na temat puli elastyczne bazy danych, zobacz [Wydajność i cena zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md)lub Utwórz nową pulę za pomocą [samouczka](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

