<properties
   pageTitle="Rozwiązania do odzyskiwania danych - SQL Replikacja bazy danych Active Geo — w chmurze | Microsoft Azure"
   description="Dowiedz się, jak zaprojektować chmury awarii odzyskiwania rozwiązań biznesowych ciągłości planowania przy użyciu replikacji geo dla aplikacji kopii zapasowych danych z bazy danych SQL Azure."
   keywords="odzyskiwanie, rozwiązania odzyskiwania danych, tworzenia kopii zapasowych danych aplikacji, geo replikacji w chmurze planowania ciągłości biznesowego"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Projektowanie aplikacji chmury awarii przy użyciu aktywnej replikacji Geo w bazie danych SQL


> [AZURE.NOTE] [Replikacja Geo Active](sql-database-geo-replication-overview.md) jest teraz dostępny dla wszystkich baz danych ze wszystkich warstw.



Dowiedz się, jak za pomocą [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md) w bazie danych SQL projekt bazy danych aplikacji mechanizm regionalnych błędy i dostawie losowych. Dla firm ciągłości planowanie, należy rozważyć, czy topologia wdrażania aplikacji, Umowa dotycząca poziomu usług docelowej przeglądarki używasz, opóźnienie ruchu i koszty. W tym artykule firma Microsoft Spójrz na typowe wzorce aplikacji i omówić korzyści i możliwości poszczególnych opcji. Aby uzyskać informacje o aktywnej Geo replikacji z pul elastyczne zobacz [elastyczne puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Wzór projektu 1: wdrożenie pasywne aktywne chmury awarii z bazą danych znajdują się

Ta opcja jest najlepiej w przypadku aplikacji o następujących właściwościach:

+ Aktywne wystąpienia w jednym regionie Azure
+ Silne zależność odczytu i zapisu (RW) dostęp do danych
+ Łączność między region logiki aplikacji i w bazie danych nie jest do przyjęcia ze względu na czas oczekiwania i ruch kosztu    

W tym przypadku topologia wdrażania aplikacji jest zoptymalizowana pod kątem obsługi awarii regionalne, gdy wszystkie składniki aplikacji ma wpływ i konieczności pracy awaryjnej jako jednostki. W celu zapewnienia nadmiarowości geograficzne logiki aplikacji i bazy danych są replikowane do innego regionu, ale nie są używane do obciążenie pracą aplikacji w normalnych warunkach. Aplikacji w regionie pomocniczej należy skonfigurowany do używania parametrów połączenia SQL do pomocniczej bazy danych. Menedżer ruchu jest skonfigurowana do używania [routingu metody pracy awaryjnej](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [AZURE.NOTE] [Menedżer Azure ruchu](../traffic-manager/traffic-manager-overview.md) jest używana w tym artykule tylko do celów ilustracji. Możesz użyć dowolnego równoważenia obciążenia rozwiązanie, które obsługuje metody routingu pracy awaryjnej.    

Oprócz wystąpienia głównej aplikacji należy rozważyć wdrażanie małą [Pracownik roli aplikacji](cloud-services-choose-me.md#tellmecs) , którą monitoruje podstawową bazą danych, wysyłając okresowych poleceń T SQL tylko do odczytu (RO). Umożliwia on automatycznie uruchamiać awaryjnym przeniesieniu na generowanie alertu na konsoli administracyjnej aplikacji lub obu. W celu zapewnienia, że monitorowania nie ma wpływu na przy dostawie całego regionu, należy wdrożyć monitorowania wystąpień aplikacji do każdego regionu i każdego wystąpienie połączony z podstawowego baz danych w regionie podstawowy i pomocniczy w regionie lokalne. 

> [AZURE.NOTE] Zarówno monitorowania aplikacji powinny być aktywna i sondy podstawowych i pomocniczych baz danych. Te ostatnie może służyć wykrywanie awarii w ustawieniach regionalnych pomocniczej i alert, gdy aplikacja nie jest chroniony.     

Na poniższym diagramie przedstawiono tej konfiguracji przed awarii.

![Konfiguracja Geo-Replikacja bazy danych SQL. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Po awarii w regionie podstawowego monitorowania aplikacji wykrywa, że podstawowej bazy danych nie jest dostępny i rejestruje alertu. W zależności od usługi SLA aplikacji można określić liczbę następujących po sobie sondy monitorowania ulegnie awarii przed deklaruje awaria bazy danych. Aby osiągnąć skoordynowanego awaryjnego przeniesienia końcowy aplikacji i w bazie danych, powinien mieć monitorowania aplikacji, wykonaj następujące czynności:

1. [Aktualizuj stan podstawowego końcowy](https://msdn.microsoft.com/library/hh758250.aspx) , aby wyzwolić pracy awaryjnej końcowy.
2. Zadzwoń do pomocniczego bazy danych, aby [zainicjować pracy awaryjnej bazy danych](sql-database-geo-replication-portal.md).

Po przełączeniu aplikacja będzie przetwarzał żądania użytkowników w regionie pomocniczej, ale będą nadal znajdują się w bazie danych, ponieważ podstawowej bazy danych będzie się teraz w regionie pomocniczą. W tym scenariuszu przedstawiono przy następnym diagramu. Wszystkich diagramów linie ciągłe sygnalizuje aktywnych połączeń, linie kropkowane wskazują zawieszonego połączenia i znaki tabulatora wskazać wyzwalaczy akcji.


![Replikacja Geo: Awaryjnego do pomocniczego bazy danych. Wykonywanie kopii zapasowej danych aplikacji.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

W przypadku awarii w regionie pomocniczym, łącza replikacji między głównego i pomocniczego bazy danych zostało zawieszone i monitorowania aplikacji rejestruje alertu, że są wyświetlane podstawowej bazy danych. Nie ma wpływu na wydajność aplikacji, a aplikacja będzie działać dostępne w związku z tym na wyższe ryzyko w przypadku, gdy dla obu regionów niepowodzenie kolejno.

> [AZURE.NOTE] Zaleca się tylko konfiguracji wdrażania z pojedynczego obszaru DR. Jest tak, ponieważ większość Azure regionach dwóch regionów. Te konfiguracje nie chroni aplikacji losowych awarii dla obu regionów. W przypadku prawdopodobne takiej awarii można odzyskać baz danych w regionie trzecim przy użyciu [geo Przywracanie](sql-database-disaster-recovery.md#recovery-using-geo-restore).

Po awarii jest nieco ograniczane, pomocniczego bazy danych jest automatycznie synchronizowana z podstawową. Podczas synchronizacji wykonywania podstawowych może mieć nieco negatywny wpływ w zależności od ilości danych, który ma być synchronizowane. Na poniższym diagramie przedstawiono awarii w regionie pomocniczą.

![Pomocniczego bazy danych są synchronizowane z głównego. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


Kluczowe **zalety** tego modelu są następujące:

+ Parametry połączenia SQL jest ustawiona podczas wdrażania aplikacji w każdym regionie i nie zmienia się po przełączeniu.
+ Wydajność aplikacji nie dotyczy pracy awaryjnej jako aplikacja i bazy danych są zawsze znajdują się.

Z głównego **zależnościami** jest zbędne aplikację w regionie pomocniczej jest używany tylko do awarii.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Wzór projektu 2: Wdrażanie aktywny aktywny równoważenia obciążenia aplikacji
Ta opcja odzyskiwania po awarii chmury najlepiej nadaje się dla aplikacji o następujących właściwościach:

+ Odczytuje wysokim stosunku bazy danych do zapisu
+ Opóźnienie zapisu bazy danych nie ma wpływu na przez użytkownika końcowego  
+ Logika tylko do odczytu mogą być oddzielone od logiki odczytu i zapisu przy użyciu parametrów połączenia różnych
+ Logika tylko do odczytu nie zależy od danych jest w pełni zsynchronizowana z najnowszymi aktualizacjami  

Jeśli aplikacje te właściwości, równoważenia połączeń użytkowników końcowych w wielu wystąpień aplikacji w różnych regionach poprawić wydajność i przez użytkownika końcowego. Aby zastosować równoważenia obciążenia, każdego regionu ma aktywne wystąpienie aplikacji logiczny (RW) odczytu i zapisu połączony z podstawową bazą danych w regionie podstawowego. Tylko do odczytu w logiczny (RO) powinny być połączone pomocniczej bazy danych w tym samym regionie jako aplikację. Menedżer ruchu powinna być równa za pomocą [routingu karuzeli](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) lub [wydajności routingu](../traffic-manager/traffic-manager-configure-performance-routing-method.md) [monitorowania końcowy](../traffic-manager/traffic-manager-monitoring.md) włączone dla każdego wystąpienia aplikacji.

Jak wzorzec #1 należy rozważyć wdrażanie podobnej aplikacji monitorowania. Ale w przeciwieństwie do wzorca #1, monitorowania aplikacja nie będzie odpowiedzialny za powodujące tym przełączeniu końcowy.

> [AZURE.NOTE] Podczas tego wzorca korzysta z więcej niż jednego pomocniczego bazy danych, tylko jeden pomocnicze może służyć do przełączania awaryjnego do powodów, dla których wspomniano wcześniej. Ponieważ tego wzorca wymaga dostępu tylko do odczytu do pomocniczej, wymaga aktywnej replikacji Geo.

Menedżer ruchu należy skonfigurować dla wydajności kierowaniu do bezpośredniego połączenia użytkownika z wystąpieniem aplikacji najbliżej lokalizacji geograficznej użytkownika. Na poniższym diagramie przedstawiono tej konfiguracji przed awarii.
![Nie awarii: wydajności kierowaniu do najbliższej aplikacji. Replikacja Geo.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Wykrycie awarii bazy danych w obszarze podstawowy inicjuje przejęcie awaryjne podstawowej bazy danych do jednego z regionów pomocniczym, zmiany lokalizacji podstawowej bazy danych. Menedżer ruchu automatycznie wyłącza offline końcowy z tabeli routingu, ale nadal routing ruchu użytkowników końcowych do pozostałych wystąpień online. Ponieważ podstawowej bazy danych jest obecnie w innym regionie, wszystkie wystąpienia online, musisz zmienić ich parametry połączenia SQL odczytu i zapisu nawiązać nowe podstawowa. Należy pamiętać, że chcesz wprowadzić tę zmianę przed podjęciem tym przełączeniu bazy danych. Parametry połączenia tylko do odczytu SQL powinny pozostać niezmienione, jak zawsze wskazywać, do bazy danych w tym samym regionie. Kroki pracy awaryjnej są:  

1. Zmień parametry połączenia SQL odczytu i zapisu, wskaż polecenie nowy podstawowy.
2. Połączenie wyznaczonych pomocniczego bazy danych w celu [pracy awaryjnej inicjowania bazy danych](sql-database-geo-replication-portal.md).

Na poniższym diagramie przedstawiono nowej konfiguracji po tym przełączeniu.
![Konfiguracja po przełączeniu. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

W przypadku awarii w jednym z regionów pomocniczym Menedżer ruchu automatycznie usuwa offline końcowy w danym regionie z tabeli routingu. Replikacja kanału pomocniczego bazy danych w danym regionie zostało zawieszone. Ponieważ pozostałe regiony uzyskać dodatkowe ruchu danych, w tym scenariuszu, aplikacji jest utworzenie wpływ na wydajność podczas awarii. Po awarii jest nieco ograniczane, pomocniczego bazy danych w regionie ryzyko natychmiast jest synchronizowana z podstawową. Podczas synchronizacji wydajności podstawowego może mieć nieco negatywny wpływ w zależności od ilości danych, który ma być synchronizowane. Na poniższym diagramie przedstawiono awarii w jednym z regionów pomocniczą.

![Awaria w regionie pomocniczą. Odzyskiwanie - Geo replikacji w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Najważniejszych **zalet** tego modelu to, że można skalować obciążenie pracą aplikacji u wielu pomocnicze uzyskanie wydajności optymalne. **Kompromisów** tej opcji są następujące:

+ Odczytu i zapisu połączenia między wystąpień aplikacji i bazy danych mają różne opóźnienie i koszt
+ Aplikacja jest utworzenie wpływ na wydajność podczas awarii
+ Wystąpień aplikacji są wymagane do dynamicznej zmiany parametrów połączenia SQL po przełączeniu bazy danych.  

> [AZURE.NOTE] Podobne podejście może służyć odciążania specjalistyczne obciążeń pracą, takich jak Raportowanie zadań, narzędzi do analizy biznesowej lub kopie zapasowe. Zazwyczaj obciążenie zajmują zasoby znaczną bazy danych w związku z tym zaleca się wskazanie jednego z pomocniczym baz danych dla nich poziom wydajności dopasowane do przewidywanych obciążenie pracą.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Modelu 3: wdrożenie pasywne aktywne konserwacji danych  
Ta opcja jest najlepiej w przypadku aplikacji o następujących właściwościach:

+ Utraty danych jest duży ryzyko. Tym przełączeniu bazy danych można używać tylko ostatecznym w przypadku awarii trwały.
+ Aplikacja może działać w trybie"tylko do odczytu" przez okres.

W tym deseniu aplikacji powoduje przejście do trybu tylko do odczytu, gdy połączenie z bazą danych pomocniczą. Logikę aplikacji w obszarze podstawowy jest znajdują się w podstawowej bazy danych i działa w trybie odczytu i zapisu (RW). Logikę aplikacji w regionie pomocniczej jest znajdują się w tym pomocniczego bazy danych i jest gotowe do rozpoczęcia pracy w trybie tylko do odczytu (RO).  Menedżer ruchu powinna być równa za pomocą [routingu pracy awaryjnej](../traffic-manager/traffic-manager-configure-failover-routing-method.md) [monitorowania końcowy](../traffic-manager/traffic-manager-monitoring.md) dla obu wystąpień aplikacji.

Na poniższym diagramie przedstawiono tej konfiguracji przed awarii.
![Wdrożenie pasywne aktywne przed pracy awaryjnej. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Gdy Menedżer ruchu wykryje błąd łączności podstawowego region, automatycznie przełącza ruch użytkownika do aplikację w regionie pomocniczą. Z tego wzorca ważne jest tej **nie** Inicjowanie pracy awaryjnej bazy danych po awarii zostanie wykryta. Aplikacja w regionie pomocniczej zostało aktywowane i działa w trybie tylko do odczytu przy użyciu pomocniczego bazy danych. Przedstawia poniższy diagram.

![Awaria: Aplikacja w trybie tylko do odczytu. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Po awarii w obszarze podstawowy jest nieco ograniczane, Menedżer ruchu wykrywa przywrócenia połączenia w regionie podstawowego i przełączniki ruch użytkownika do aplikację w regionie podstawowego. To wystąpienie aplikacji życiorysy i działa w trybie odczytu i zapisu przy użyciu podstawowego bazy danych.

> [AZURE.NOTE] Ponieważ tego wzorca wymaga dostępu tylko do odczytu do pomocniczej wymaga aktywnej replikacji Geo.

W przypadku awarii w regionie pomocniczym Menedżer ruchu oznacza końcowy aplikacji w regionie podstawowego o obniżonej wydajności i kanału replikacji zostało zawieszone. Jednak to awarii nie wpływ na wydajność aplikacji podczas awarii. Po awarii jest nieco ograniczane, pomocniczego bazy danych od razu jest synchronizowana z podstawową. Podczas synchronizacji wydajności podstawowego może mieć nieco negatywny wpływ w zależności od ilości danych, który ma być synchronizowane.

![Awaria: Pomocniczego bazy danych. Odzyskiwanie w chmurze.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Tego wzorca projekt ma wiele **zalet**:

+ Podczas tymczasowych dostawie go pozwala uniknąć utraty danych.
+ Nie wymaga o wdrożyć aplikację monitorowania, jak odzyskiwania zostanie wywołana przez Menedżera ruch.
+ Przestoje zależy od tylko szybkiego Menedżer ruchu wykrywa niepowodzenia łączności można konfigurować.

**Kompromisów** są:

+ Aplikacja musi być działać w trybie tylko do odczytu.
+ Konieczne jest dynamicznie przełączyć do niego, gdy jest połączony z bazą danych tylko do odczytu.
+ Wznowienie operacji odczytu i zapisu wymaga odzyskiwania podstawowego regionu, co oznacza, że dostęp do pełnego danych mogą być wyłączone dla godzin lub nawet dni.

> [AZURE.NOTE] W przypadku awarii usługi trwały w regionie należy ręcznie aktywować pracy awaryjnej bazy danych i zaakceptować utratą danych. Aplikacja będzie działać w regionie pomocniczej z dostępem do odczytu i zapisu do bazy danych.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Planowanie ciągłości Business: Wybierz projekt aplikacji awarii chmury

Strategii odzyskiwania danych do chmury określonych można połączyć lub rozszerzanie deseni projektu najlepiej stosownie do potrzeb aplikacji.  Jak wspomniano wcześniej, strategii, która zostanie wybrana opcja jest oparty na Umowa dotycząca poziomu usług chcesz do oferowania klientów i topologia wdrażania aplikacji. Aby ułatwić decyzję, w poniższej tabeli porównano opcje, w zależności od utraty danych szacowany lub odzyskiwanie punktu cel (RPO) i szacowany czas odzyskiwania (Wstaw).

| Wzór | RPO | WSTAW
| :--- |:--- | :---
| Wdrożenie pasywne aktywne awarii z dostępem znajdują się bazy danych | Dostęp do odczytu i zapisu < 5 s | Czas wykrywania wystąpienia błędu + interfejsu API pracy awaryjnej połączeń test weryfikacji aplikacji
| Wdrożenie aktywny aktywny do równoważenia obciążenia aplikacji | Dostęp do odczytu i zapisu < 5 s | Czas wykrywania wystąpienia błędu + połączenia pracy awaryjnej interfejsu API + parametry połączenia SQL Zmienianie test weryfikacji aplikacji
| Wdrożenie pasywne aktywne konserwacji danych | Dostęp tylko do odczytu, < odczytu i zapisu 5 s = 0 | Dostęp tylko do odczytu = czas wykrywania awarii łączności + test weryfikacji aplikacji <br>Dostęp do odczytu i zapisu = czas zmniejszeniu awarii

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
- Aby uzyskać informacje o aktywnej Geo replikacji z pul elastyczne zobacz [elastyczne puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
