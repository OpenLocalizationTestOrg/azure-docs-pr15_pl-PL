<properties
   pageTitle="Projektowanie wzorców multitenant aplikacji władz akredytacji bezpieczeństwa i bazy danych SQL Azure | Microsoft Azure"
   description="W tym artykule omówiono wymagania i typowe wzorce Architektura danych multitenant bazy danych, które aplikacje w środowisku chmury potrzebne brać pod uwagę i różnych kompromisów skojarzone z tych deseni. Go też wyjaśniono, jak bazy danych SQL Azure z pul elastycznych i elastyczne narzędzia pomóc rozwiązania tych wymagań w sposób nie kompromisów."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Projektowanie wzorców multitenant aplikacji władz akredytacji bezpieczeństwa i bazy danych SQL Azure

W tym artykule można zapoznać wymagania i wspólne dane desenie architektura oprogramowania multitenant jako aplikacji bazy danych usług (SaaS), które działają w środowisku chmury. Wyjaśniono również czynników, które należy wziąć pod uwagę i korzystnych rozwiązań wzorców inny projekt. Elastyczne pul i elastyczne narzędzia bazy danych SQL Azure może ułatwić własnych wymagań bez utraty innych celów.

Deweloperów czasami ustawić opcje, które działają przed ich długotrwałe interesy zalecane podczas projektowania modeli dzierżawy dla poziomów danych multitenant aplikacji. Początkowo co najmniej deweloper może szybko wychwycić ułatwienia rozwoju i niższe koszty dostawcy usługi cloud jako ważne więcej niż izolacji dzierżawy lub skalowalność aplikacji. Ta opcja może prowadzić do problemów zadowolenie klienta i kosztów korekty kursu później.

Multitenant aplikacja jest obsługiwany w środowisku chmury aplikacji i który zawiera ten sam zestaw usług setki lub tysiące dzierżaw, którzy nie udostępnianie lub wyświetlić dane innych osób. Przykład jest aplikacja władz akredytacji bezpieczeństwa, która zapewnia usługi z dzierżawami w środowisku hostowana w chmurze.

## <a name="multitenant-applications"></a>Multitenant aplikacji

W aplikacjach multitenant danych i obciążenie pracą można łatwo rozdzielonymi. Można podzielić danych i obciążenie pracą, na przykład wzdłuż granic dzierżawy, ponieważ większość żądania pojawią się w obrębie dzierżawy. Ta właściwość jest związane z danych i obciążenie pracą, a jego ma wzorców aplikacji omawiane w tym artykule.

Deweloperów Użyj tego typu stosowania przez cały zakres aplikacje oparte na chmurze, w tym:

- Partner bazy danych aplikacji, które są jest zmigrowanych w chmurze jako aplikacje władz akredytacji bezpieczeństwa
- Aplikacje władz akredytacji bezpieczeństwa, utworzone dla chmury od podstaw w górę
- Bezpośrednie, skierowaną klienta aplikacji
- Aplikacje dla przedsiębiorstw skierowaną pracownika

Aplikacje władz akredytacji bezpieczeństwa, które są przeznaczone do chmury i z głównych zwykle partnera aplikacji baz danych są multitenant aplikacji. Te aplikacje władz akredytacji bezpieczeństwa dostarczanie specjalistyczne aplikacji jako usługa ich dzierżaw. Dzierżaw można uzyskać dostęp do usług aplikacji i pełne prawo własności skojarzone dane przechowywane jako część aplikacji. Jednak aby można było korzystać z zalet władz akredytacji bezpieczeństwa, dzierżaw musi przekazanie niektórych kontrolę nad własnych danych. Ufać usługodawcy władz akredytacji bezpieczeństwa, aby zapewnić bezpieczne i są one odizolowane danych z innych dzierżaw danych. Przykłady tego typu multitenant aplikacji władz akredytacji bezpieczeństwa są MYOB, SnelStart i Salesforce.com. Na każdej z nich można partycje wzdłuż ograniczenia dzierżawy i aplikacja projektowanie desenie, które omówimy w tym artykule pomocy technicznej.

Aplikacje, które klientom bezpośrednią usługę lub inną kategorię na zakres multitenant aplikacji są pracowników w obrębie organizacji (nazywane często użytkownicy, a nie dzierżaw). Klienci zasubskrybuj usługę i nie ma danych usługodawca gromadzi i przechowuje. Dostawcy usług mają mniej rygorystyczne wymagania dotyczące przechowywania danych swoich klientów odizolowane od siebie poza przepisami osobowych upoważnionych dla instytucji rządowych. Przykłady tego typu aplikacji multitenant skierowaną klienta są dostawców zawartości multimediów, takich jak Netflix, spotify osoba i Xbox LIVE. Inne przykłady łatwo partitionable aplikacje są skierowaną klienta, aplikacji skali Internet lub aplikacji Internet czynności (IoT) w których każdego klienta lub urządzenia może służyć jako. Ograniczenia partition oddzielić użytkowników i urządzeń.

Nie wszystkie aplikacje partition łatwo wzdłuż jednej właściwości, takie jak dzierżawy, klienta, użytkownika lub urządzenia. Zasób przedsiębiorstwa złożonych planowania aplikacji (ERP), na przykład ma produktów, zamówienia i klienci. Zazwyczaj jest złożone schemat o tysiące wysoce połączone tabele.

Nie strategii jedną partycją można Zastosuj do wszystkich tabel i pracy w całej obciążenie pracą aplikacji. W tym artykule omówiono multitenant aplikacji, które mają obciążenia i łatwo partitionable danych.

## <a name="multitenant-application-design-trade-offs"></a>Aplikacja multitenant projekt korzystnych rozwiązań

Wzorzec projektu multitenant deweloperem wybiera zazwyczaj opiera się pod uwagę następujące kwestie:

-   **Izolacji dzierżawy**. Deweloper musi upewnij się, że nie dzierżawy ma niepożądane dostęp do danych innych dzierżaw. Wymaganie izolacji rozszerza na inne właściwości, takie jak zapewnia ochronę z zakłócenia sąsiadów, możliwość przywracania danych dzierżawy i implementowania dostosowania specyficzne dla dzierżawy.
-   **Koszt zasobu w chmurze**. Aplikacja władz akredytacji bezpieczeństwa musi być konkurencyjności koszt. Multitenant deweloperem określić, aby zoptymalizować obniżenia kosztów w polu Użyj chmury zasoby, takie jak miejsca do magazynowania i obliczania kosztów.
-   **Ułatwienia DevOps**. Multitenant deweloperem musi włączyć ochronę izolacji, obsługa, monitorowanie kondycji ich stosowania i schemat bazy danych i rozwiązywanie problemów dzierżawy. Złożoność opracowywania aplikacji i działaniem przekłada się bezpośrednio na lepszą kosztów i wymagań z zadowolenie dzierżawy.
-   **Skalowalność**. Możliwość stopniowego dodawania więcej dzierżaw i pojemność dzierżaw, którzy potrzebują go jest Operacja pomyślna władz akredytacji bezpieczeństwa.

Każdego z tych czynników ma korzystnych rozwiązań w porównaniu do drugiego. Chmura najniższego kosztu oferowanie nie mogą oferować najwygodniejsze proces tworzenia. Należy dla deweloperów nawiązać właściwe opcje dotyczące tych opcji i ich korzystnych rozwiązań w procesie projektowania aplikacji.

Deseń popularne rozwoju jest dodatkiem Service pack kilka dzierżaw do jednej lub kilku baz danych. Korzyści wynikające z tej metody są niższe koszty, ponieważ zapłacić za kilka baz danych i uproszczenia względna pracy z ograniczoną liczbą baz danych. Ale czasem władz akredytacji bezpieczeństwa multitenant deweloperem będzie uznasz, że ta opcja ma istotne downsides izolacji dzierżawy i skalowalność. Jeśli izolacji dzierżawy jest ważne, dodatkowego nakładu pracy jest wymagane do ochrony danych dzierżawy w magazynie udostępnionym przed nieautoryzowanym dostępem lub zakłócenia sąsiadów. Ten dodatkowego nakładu pracy może być znacznie zwiększyć programistycznych i kosztów utrzymania izolacji. Podobnie jeśli wymagane jest dodanie dzierżaw tego wzorca projektu zazwyczaj wymaga Aby rozpowszechniać dzierżawy danych w bazach danych prawidłowo przeskalować warstwy danych aplikacji.  

Często izolacji dzierżawy jest wymagane podstawowych w aplikacjach multitenant władz akredytacji bezpieczeństwa, dbających dla firm i organizacji. Deweloper może mieć ochotę przez znane zalet w prostoty i koszt izolacji dzierżawy i skalowalność. Takie rozwiązanie może udowodnić złożone i drogich rozwoju usługę i wymagania izolacji dzierżawy stają się bardziej ważne i zarządzanych w warstwie aplikacji. Jednak w aplikacjach multitenant, które oferują usługę bezpośrednie, dla klientów indywidualnych skierowaną do klientów, izolacji dzierżawy może być niższy priorytet niż optymalizowania kosztu zasobu w chmurze.

## <a name="multitenant-data-models"></a>Modele danych multitenant

Typowe rozwiązania projektu dotyczących umieszczania danych dzierżawy wykonaj trzech modeli odrębnych pokazano na rysunku 1.


  ![Modele danych aplikacji multitenant](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 rysunek 1: typowe rozwiązania projektu w przypadku modeli danych multitenant

-   **Bazy danych na dzierżawcę**. Każdy dzierżawy ma swoją własną bazę danych. Wszystkie dane specyficzne dla dzierżawy jest ograniczona do bazy danych dzierżawy i samodzielnie z innymi dzierżaw i ich dane.
-   **Udostępnione sharded bazy danych**. Kilka dzierżaw udostępnić jeden z wielu baz danych. Unikatowy zestaw dzierżaw jest przypisany do każdej bazy danych za pomocą podziału strategii, takich jak skrótu, zakresu lub listy podziału. Ta strategia rozkładu danych często nosi nazwę sharding.
-   **Udostępnianie bazy danych pojedynczy**. Pojedynczy, czasami duży bazy danych zawiera dane dla wszystkich dzierżaw, które są sobie w kolumnie Identyfikator dzierżawy.

> [AZURE.NOTE] Aby umieścić różnych dzierżaw w schematach inną bazę danych, a następnie użyj nazwy schematu, aby odróżnić różne dzierżaw warto wybrać Deweloper aplikacji. Zaleca się tej metody, ponieważ zazwyczaj wymaga użycia dynamicznego kodu SQL, a nie może być skuteczne w pamięci podręcznej planu. W dalszej części tego artykułu możemy skupić się na metodę tabeli udostępnione dla tej kategorii multitenant aplikacji.

## <a name="popular-multitenant-data-models"></a>Modele popularnych multitenant danych

Należy oceny różnych rodzajów modeli danych multitenant za pomocą aplikacji projekt korzystnych rozwiązań, które już określiliśmy. Następujących czynników pomoc, charakteryzującą trzy najczęściej używane multitenant modelami danych z opisanych wcześniej i ich zastosowania bazy danych, jak pokazano na rysunku 2.

-   **Izolacji**. Stopień izolacji między dzierżawami może być pewien stopień izolacji dzierżawy, ile uzyskuje modelu danych.
-   **Koszt zasobu w chmurze**. Ilość zasobu udostępnianie między dzierżawami może optymalizować kosztu zasobu w chmurze. Zasób można zdefiniować jako koszt obliczeń i miejsca do magazynowania.
-   **Koszt DevOps**. Ułatwienia opracowywania aplikacji, wdrażania i łatwości zarządzania zmniejsza całkowity koszt operacji władz akredytacji bezpieczeństwa.  

Rysunek 2 oś Y pokazuje stopień izolacji dzierżawy. Osi X pokazano poziom współużytkowanie zasobów. Szary, ukośną strzałkę w środku wskazuje kierunek kosztów DevOps sytuacjach zwiększyć lub zmniejszyć.

![Desenie projektu popularnych aplikacji multitenant](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) rysunek 2: modeli popularnych multitenant danych

Ćwiartka prawym dolnym rogu na rysunku 2 pokazano wzorzec aplikacji używa potencjalnie duże, udostępnionego jednej bazie danych i udostępnionych tabeli (lub osobne schematu) podejście. Zaleca dla zasobu udostępniania, ponieważ wszystkie dzierżaw używać tych samych zasobów bazy danych (Procesor, pamięć, wejście i wyjście) w jednej bazie danych. Ale izolacji dzierżawy jest ograniczona. Może być konieczne podjąć dodatkowe kroki w celu ochrony dzierżaw od siebie w warstwie aplikacji. Następujące dodatkowe kroki można znacznie zwiększyć koszt DevOps rozwijania i Zarządzanie aplikacją. Skalowalność jest ograniczona przez skali sprzęt, który znajduje się baza danych.

Ćwiartka w lewym dolnym na rysunku 2 przedstawiono kilka dzierżaw sharded przez wiele baz danych (zazwyczaj innego sprzętu skalowanie jednostki). Podzestaw dzierżaw, którego dotyczy problem dotyczący skalowalność innych wzorców znajduje się w każdej bazy danych. W przypadku wymagane na potrzeby dzierżaw więcej możliwości, możesz łatwo umieścić dzierżaw w nowych baz danych zostało przydzielonych do nowej jednostki skali sprzętu. Jednak ilość współużytkowanie zasobów jest ograniczona. Tylko dzierżaw umieszczony na tej samej jednostki skali współużytkowanie zasobów. Ta metoda zapewnia nieco usprawnienie dzierżawa izolacji, ponieważ wielu dzierżaw są nadal współistniejącego wdrożenia wersji bez automatycznie chronione od działań innych osób. Złożoność aplikacji pozostaje wysoka.

Ćwiartka lewej górnej na rysunku 2 jest trzecim podejściem. Danych każdej dzierżawy pozwala umieszczać w swoją własną bazę danych. Ta metoda ma właściwości dobre izolacji dzierżawy, ale ogranicza współużytkowanie zasobów środków własnych dedykowane po każdej bazy danych. Ta metoda jest zalecana, jeśli wszystkie dzierżaw mają przewidywalne obciążenia. Jeśli mniej przewidywalne obciążenia dzierżawy, dostawca nie może optymalizować współużytkowanie zasobów. Nieprzewidywalność są często władz akredytacji bezpieczeństwa aplikacji. Dostawca musi nadmiernej zapewnienie spełnienia wymagań albo dolnym zasobów. Każda z tych operacji powoduje wyższych kosztów lub dolnym zadowolenie dzierżawy. Wyższy poziom zasobów, udostępnianie dzierżaw staje się pożądane ustanowienie rozwiązanie redukcji kosztów. Zwiększenie liczby baz danych zwiększa także koszt DevOps do wdrożenia i obsługi aplikacji. Pomimo tych problemów ta metoda zapewnia najlepszych i najprostszym izolacji dla dzierżaw.

Czynniki te również wpływ modelu, który wybiera klienta:

-   **Własności danych dzierżawy**. Aplikacja, w której dzierżaw utrzymać własności własnych danych ma strukturze jednej bazie danych na dzierżawcę.
-   **Skala**. Aplikacja, która jest przeznaczony dla setki tysiące lub miliony dzierżaw ma udostępniania metod, takich jak sharding bazy danych. Wymagania dotyczące izolacji nadal mogą stanowić problemach.
-   **Wartość i procesów biznesowych**. Jeśli aplikacja na dzierżawy zysk Jeśli małych (mniej niż PLN), wymagania izolacji stają się mniej ważnych i udostępniona baza danych jest właściwe rozwiązanie. W przypadku złotych kilka lub więcej zysk na dzierżawy modelu bazy danych na dzierżawcę jest bardziej możliwe. Zmniejszanie kosztów opracowywania warto skorzystać.

Mieć projektu skutków pokazany na rysunku 2, model multitenant idealny musi uwzględniać właściwości izolacji dobre dzierżawy z zasobem optymalnego udostępnianie między dzierżawami. Ten model mieści się w kategorii opisanego w Ćwiartka prawego górnego rysunku 2.

## <a name="multitenancy-support-in-azure-sql-database"></a>Obsługa multitenancy w bazie danych SQL Azure

Baza danych SQL Azure obsługuje wszystkie desenie multitenant aplikacji przedstawionych na rysunku 2. Elastyczne pul obsługuje także deseń aplikacji, który łączy współużytkowanie zasobów warto i izolacji zalety bazy danych na na dzierżawy dojechać (zobacz Ćwiartka prawego górnego na rysunku 3). Elastyczne bazy danych narzędzia i funkcje w bazie danych SQL zmniejszyć koszty tworzy i wykorzystuje aplikację, która zawiera wiele baz danych (jak pokazano w cieniowanym obszarem na rysunku 3). Te narzędzia ułatwiają tworzenie i zarządzanie aplikacje, które używają tych wzorców wielu bazy danych.

![Desenie w bazie danych SQL Azure](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) rysunek 3: wzorców Multitenant aplikacji w bazie danych SQL Azure

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Modelu bazy danych na dzierżawy za pomocą narzędzi i pule elastyczne

Elastyczne pul w bazie danych SQL połączyć izolacji dzierżawy z zasobów udostępniania między baz danych dzierżawy lepszą obsługę ujęciu bazy danych na dzierżawcę. Baza danych SQL jest rozwiązaniem Warstwa danych dotyczących dostawców władz akredytacji bezpieczeństwa, którzy tworzą multitenant aplikacji. Ciężar współużytkowania między dzierżawami zasobów zostanie przeniesiony z warstwy aplikacji do warstwy usługi bazy danych. Złożoność zarządzania i wykonywanie zapytań w skali w bazach danych jest uproszczone z elastyczne zadania, elastyczne kwerendy, transakcje elastycznych i Biblioteka klienta elastyczne bazy danych.

| Wymagania dotyczące aplikacji | Funkcje bazy danych SQL |
| ------------------------ | ------------------------- |
| Izolacji dzierżawy i współużytkowanie zasobów | [Elastyczne pul](sql-database-elastic-pool.md): przydzielanie pulę zasobów bazy danych SQL i współużytkowanie zasobów między różnymi bazami danych. Ponadto pojedyncze bazy danych można narysować tyle zasobów z puli, stosownie do potrzeb, aby zezwalały na tych najwyższych wartościach pojemności żądanie ze względu na zmiany w obciążenia dzierżawy. Elastyczne puli się można skalować w górę lub w dół, stosownie do potrzeb. Elastyczne pul zapewniają również ułatwienia zarządzania i monitorowania i rozwiązywania problemów na poziomie puli. |
| Ułatwienia DevOps całej bazy danych | [Elastyczne pul](sql-database-elastic-pool.md): jak wspomniano wcześniej.|
||[Elastyczne kwerend](sql-database-elastic-query-horizontal-partitioning.md): kwerendy w bazach danych raportowania dzierżawy krzyżowe — analizy.|
||[Elastyczne zadania](sql-database-elastic-jobs-overview.md): pakiet i niezawodne wdrażanie operacji konserwacji bazy danych lub zmiany schematu bazy danych do wielu baz danych.|
||[Elastyczne transakcje](sql-database-elastic-transactions-overview.md): proces zmieni się na kilka baz danych w sposób Atomowej i są one odizolowane. Elastyczne transakcje są wymagane, gdy potrzebne gwarancje "wszystko lub nic" przez kilka operacji bazy danych. |
||[Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md): Mapa dzierżaw do baz danych i zarządzanie nimi rozkład danych. |

## <a name="shared-models"></a>Udostępnione modele

Opisany wcześniej, większość dostawców władz akredytacji bezpieczeństwa, podejście udostępniony model mogą stanowić problemów z dzierżawy izolacji problemów i złożoności opracowywania aplikacji i konserwacja. Jednak dla multitenant aplikacji, które oferują usługę bezpośrednio do konsumentów, wymagania izolacji dzierżawy nie mogą być wysoki priorytet jako zminimalizowaniu kosztów. Może być możliwość pakowanie dzierżaw w jedną lub więcej baz danych o wysokiej gęstości, aby zmniejszyć koszty. Modele udostępnioną bazą danych przy użyciu jednej bazie danych lub wielu sharded baz danych może spowodować dodatkowe korzyści, udostępnianie i ogólny kosztów zasobów. Baza danych SQL Azure zawiera niektóre funkcje, które zapewniają klientom tworzenie izolacji udoskonalone zabezpieczenia i zarządzania w skali w warstwie danych.

| Wymagania dotyczące aplikacji | Funkcje bazy danych SQL |
| ------------------------ | ------------------------- |
| Funkcje zabezpieczeń izolacji | [Zabezpieczenia na poziomie wiersza](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Schemat bazy danych](https://msdn.microsoft.com/library/dd207005.aspx) |
| Ułatwienia DevOps całej bazy danych | [Elastyczne kwerendy](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastyczne zadania](sql-database-elastic-jobs-overview.md) |
|| [Elastyczne transakcje](sql-database-elastic-transactions-overview.md) |
|| [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md) |
|| [Dzielenie bazy danych elastyczne i scalanie](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Podsumowanie

Wymagania dotyczące izolacji dzierżawy są istotne w przypadku większości aplikacji multitenant władz akredytacji bezpieczeństwa. Najlepsze opcję, aby zapewnić izolacji intensywnie leans w ujęciu bazy danych na dzierżawcę. Inne metody wymagają inwestycji w warstwy złożonej aplikacji, które wymagają personelu opracowującego specjalistycznej zapewnienie oddzielnie, co znacznie zwiększa kosztu i ryzyka. Jeśli izolacji nie uwzględnia wczesnym etapie rozwoju usługi, może być bardziej kosztów w pierwszych dwóch modelach modernizacji je. Wady głównym, skojarzone z modelu bazy danych na dzierżawcę odnoszą się do chmury lepszą koszty zasobów z powodu ograniczona udostępniania i zachowywanie i zarządzanie wiele baz danych. Deweloperów aplikacji władz akredytacji bezpieczeństwa często wystąpić problemy po wysłaniu tych korzystnych rozwiązań.

Chociaż korzystnych rozwiązań może być głównych progi o większość dostawców usług bazy danych w chmurze, bazy danych SQL Azure eliminuje przeszkód z puli elastycznych i możliwości elastyczne bazy danych. Deweloperów władz akredytacji bezpieczeństwa można łączyć cech izolacji modelu bazy danych na dzierżawy i zoptymalizować współużytkowanie zasobów i łatwość wiele baz danych za pomocą pul elastycznych i powiązane z nim narzędzia.

Dostawcy multitenant aplikacji, która ma nie wymagania izolacji dzierżawy i kto pakietów dzierżaw w bazie danych o wysokiej gęstości może się okazać, że dane udostępnione modele wynik dodatkowe efektywności w współużytkowanie zasobów i zmniejszyć całkowity koszt. Narzędzia elastyczne bazy danych bazy danych SQL Azure, bibliotek sharding i funkcje zabezpieczeń pomóc dostawców władz akredytacji bezpieczeństwa, tworzenie i zarządzanie aplikacjami multitenant.

## <a name="next-steps"></a>Następne kroki

[Wprowadzenie do narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md) przy użyciu aplikacji przykładowych, demonstrujący Biblioteka klienta.

Tworzenie [puli elastyczne niestandardowe pulpit nawigacyjny do władz akredytacji bezpieczeństwa](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) z aplikację próbki, która używa pul elastyczne rozwiązania efektywne pod względem kosztów, skalowalna bazę danych.

Za pomocą narzędzia bazy danych SQL Azure, aby [przeprowadzić migrację istniejącej bazy danych w celu skalowania](sql-database-elastic-convert-to-use-elastic-tools.md).

Wyświetlanie Nasz samouczek na temat [tworzenia elastycznych puli](sql-database-elastic-pool-create-portal.md).  

Więcej sposobów [monitorowania i zarządzania nimi puli elastyczne](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Co to jest Azure puli elastyczne?](sql-database-elastic-pool.md)
- [Skalowanie zewnętrzne z bazy danych SQL Azure](sql-database-elastic-scale-introduction.md)
- [Multitenant aplikacji za pomocą narzędzia bazy danych elastycznych i zabezpieczenia na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Uwierzytelnianie w aplikacjach multitenant za pomocą usługi Azure Active Directory i łączenie OpenID](../guidance/guidance-multitenant-identity-authenticate.md)
- [Firma ankiet aplikacji](../guidance/guidance-multitenant-identity-tailspin.md)
- [Rozwiązanie Szybkie uruchamianie](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Pytania i funkcji

Pytania Znajdź dla z nami na [forum bazy danych SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Dodaj żądania funkcji na [forum opinii bazy danych SQL](https://feedback.azure.com/forums/217321-sql-database/).
