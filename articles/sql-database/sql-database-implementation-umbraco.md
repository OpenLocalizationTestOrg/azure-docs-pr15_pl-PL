<properties
   pageTitle="Analiza przypadku Azure - Umbraco bazy danych Azure SQL | Microsoft Azure"
   description="Więcej informacji na temat usługi skali tysiące dzierżaw w chmurze i jak Umbraco używane bazy danych SQL szybko inicjowania obsługi"
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
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco używa bazy danych SQL Azure szybko świadczenia skali usługi i tysiące dzierżaw w chmurze

![Umbraco Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco to popularne Otwórz źródło zarządzania zawartością system (CMS) który może zostać uruchomiony nic z małych kampanii lub broszura witryn do skomplikowanych aplikacji dla przedsiębiorstw magazyn Fortune 500 i globalnej nośnika witryn sieci Web. 

> "Mamy bardzo dużych społeczność deweloperów, którzy systemu, za pomocą więcej niż 100 000 deweloperów na forach i więcej niż 350,000 witryn, które są aktywne, uruchomiony Umbraco".

> — Umbraco Witold Christensen, kierownika technicznego

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Aby uprościć wdrożeń klienta, Umbraco dodane Umbraco jako z usługi (UaaS): oferująca oprogramowania jako z usługi (władz akredytacji bezpieczeństwa), eliminujący potrzebę wdrożenia lokalne zawiera wbudowane skalowania i usuwa zarządzania obciążenie, umożliwiając deweloperów skoncentrować się na zarządzanie innowacje zamiast rozwiązanie produktów. Umbraco będzie mógł przekazać te korzyści Polegaj na elastyczne model (PaaS) platformy jako usługi oferowane przez Microsoft Azure.

UaaS udostępnia klientom władz akredytacji bezpieczeństwa wykorzystaniu możliwości Umbraco CMS, które zostały wcześniej przed ich. Tych klientów zainicjowano obsługę administracyjną ze środowiskiem CMS pracy, która zawiera produkcyjnej bazy danych. Klientów można dodać do dwóch dodatkowych baz danych dla rozwoju i środowiska wzorcowego, w zależności od ich wymagania. W przypadku zażądania nowego środowiska, zautomatyzowany proces przypisuje klienta bazy danych automatycznie. Nowe baza danych jest przygotowana w sekundach, ponieważ bazy danych już wstępnie zainicjowano przez Umbraco z puli elastyczne Azure dostępnych baz danych (patrz rysunek 1).


![Cykl życia obsługi administracyjnej Umbraco](./media/sql-database-implementation-umbraco/figure1.png)

Rysunek 1. Inicjowanie obsługi administracyjnej cyklu życia dla Umbraco jako usługa (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure pul elastycznych i automatyzacji uprościć wdrożenia

Z bazy danych SQL Azure i innych usług Azure Umbraco klienci mogą własnym zakresie ich środowiska i Umbraco łatwo monitorowanie i zarządzanie bazami danych w ramach przepływu pracy intuicyjny:

1.  Obsługa administracyjna

    Umbraco przechowuje pojemności 200 dostępnych wstępnie ustanawianie baz danych z zestawów elastyczne. Po nowym klientem zarejestrowaniu dla UaaS, Umbraco zawiera klienta z nowym środowisku CMS w czasie rzeczywistym najbliższego przez przypisywanie ich bazy danych z puli dostępności.

    Po puli dostępność osiągnięciu progu, utworzeniu nowej puli elastycznych i nowych baz danych są wstępnie ustanawianie przydzielania klientom stosownie do potrzeb.

    Implementacja jest w pełni zautomatyzować, używając biblioteki zarządzania C# i kolejki Bus usługi Azure.

2.  Wykorzystanie

    Klienci za pomocą jednego do trzech środowiskach (w przypadku produkcji, tymczasowego lub rozwoju), każdy swoją własną bazę danych. Bazy danych klienta są w pule elastyczne bazy danych, które umożliwia Umbraco zapewnienie efektywnego skalowania bez konieczności nadmiernie obsługi administracyjnej.

    ![Przegląd projektów Umbraco](./media/sql-database-implementation-umbraco/figure2.png)

    ![Szczegóły projektu Umbraco](./media/sql-database-implementation-umbraco/figure3.png)

    Rysunek 2. Umbraco jako z usługi (UaaS) klienta witryny sieci Web, przedstawiający Przegląd projektów i podania ich szczegółów

    Baza danych SQL Azure używa jednostki transakcji bazy danych (DTUs) w celu zaprezentowania względne uprawnienia wymagane dla transakcje bazy danych rzeczywistych. W przypadku klientów UaaS baz danych zwykle działania na 10 DTUs, ale każda ma elastyczność przeskalować na żądanie. Oznacza to, że UaaS zagwarantować, że klienci zawsze mieli niezbędne zasoby, nawet w czasie Szczyt. Na przykład podczas ostatnie zdarzenia sports niedzielę wieczorem jeden UaaS doświadczonych bazy danych klientów pików DTUs do 100 na czas trwania gry. Azure pul elastyczne możliwe Umbraco do obsługi wymagające wysokiej bez spadek wydajności.

3.  Monitorowanie

    Monitory Umbraco bazy danych działań przy użyciu pulpitów nawigacyjnych w portalu Azure, wraz z alertów własnego adresu e-mail.

4.  Odzyskiwanie

    Azure oferuje dwie opcje odzyskiwania (DR): aktywne replikacji Geo i ich przywracanie Geo. Firmy, należy zaznaczyć odpowiednią opcję DR zależy od jego [celów ciągłości](sql-database-business-continuity.md).

    Replikacja Geo Active zapewnia najszybszy poziom odpowiedzi w wypadku przestoje. Używanie aktywnej replikacji Geo, możesz utworzyć do czterech pomocnicze czytelny na serwerach w różnych regionów i przełączanie awaryjne do dowolnego z pomocnicze mogą następnie inicjować w przypadku awarii.

    Umbraco nie wymaga Geo replikacji, ale jej korzystać z Azure Geo-Przywróć, aby zapewnić minimalne przestoje wypadku awarii. Przywracanie Geo zależy od kopie zapasowe bazy danych w magazynie Azure geo zbędne. Umożliwia użytkownikom przywracanie z kopii zapasowej, gdy wystąpi awarii w regionie podstawowego.

5.  Usuwanie

    Po usunięciu środowiska projektu podczas Oczyszczanie kolejki Bus usługi Azure zostaną usunięte wszystkie skojarzone bazy danych (rozwój, tymczasowy lub live). Ten proces automatycznego przywraca nieużywane baz danych do puli elastyczne dostępność bazy danych i Umbraco, udostępniania na potrzeby przyszłych inicjowania obsługi administracyjnej przy zachowaniu maksymalne wykorzystanie.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastyczne pul Zezwalaj UaaS przeskalować z łatwością

Korzystając z pul Azure elastyczne bazy danych, Umbraco można Zoptymalizuj klientom bez konieczności przez lub niedoboru świadczenia. Umbraco ma obecnie prawie 3000 baz danych przez 19 pule elastyczne bazy danych, możliwość łatwego skalowania stosownie do potrzeb, aby zezwalały na dowolną z jej obecnych klientów 325,000 lub nowe klienci, którzy są gotowe do wdrożenia CMS w chmurze.

W rzeczywistości według Christensen Witold, techniczne potencjalnego klienta na Umbraco, "UaaS teraz występuje wzrostu około 30 nowi klienci dziennie. Naszym klientom są zachwycony wygody można dodawać nowych projektów w sekundach, natychmiast publikowanie aktualizacji do witryn live ze środowiska programowania przy użyciu "jednym kliknięciem wdrożenie" i wprowadź zmiany, tak jak szybko, jeśli one znaleźć błędy."

Jeśli odbiorca nie wymaga już środowisku drugiego i trzeciego, wystarczy po prostu usunąć tych środowiskach. Który zwolnienia zasobów, które mogą być używane dla innych klientów w ramach puli Umbraco elastyczne dostępność bazy danych.

![Architektura wdrażania Umbraco](./media/sql-database-implementation-umbraco/figure4.png)

Rysunek 3. Architektura wdrażania UaaS Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Ścieżka z centrum danych do chmury

Gdy deweloperzy Umbraco początkowo decyzji, aby przejść do modelu władz akredytacji bezpieczeństwa, ich wiedzieli, że powinny efektywne pod względem kosztów i skalowalna sposobem tworzenia usługi.

> "Pul elastyczne bazy danych są dokładne dopasowanie naszych władz akredytacji bezpieczeństwa oferowania, ponieważ firma Microsoft można dołączyć, dzwoniąc wydajność i rozwijaniu stosownie do potrzeb. Inicjowania obsługi administracyjnej jest łatwe, a nasz instalację, możemy powiadamiać wykorzystania maksymalnie."

> — Umbraco Witold Christensen, kierownika technicznego

"Trzeba poświęcać czasu na rozwiązywaniu problemów klientów nie zarządzania infrastrukturą. Firma Microsoft chce ułatwić klientom uzyskiwanie najbardziej wartości"mówi Niels Hartvig, założyciele Umbraco. "Początkowo uwzględniane hostingu z serwerami nad, ale planowania pojemności mogą być nightmare". Coincidentally Umbraco nie zawiera dowolny Administratorzy bazy danych, które podkreśla propozycji wartości klucza dotyczące korzystania z UaaS.

Jeden cel ważne dla deweloperów Umbraco było umożliwiają klientom UaaS obsługi administracyjnej środowiskach szybko i bez ograniczeń pojemności. Ale dostarczając dedykowane hostowana usługa w centrach danych Umbraco czy masz wymagane dużej pojemności nadmiarowe obsługę seria przetwarzania. Który chcesz mieć są przeznaczone Dodawanie infrastruktury znaczące obliczeń, którą chcesz regularnie niedociążony.

Ponadto zespołu opracowującego Umbraco utworzyć rozwiązanie, które umożliwi ich ponowne możliwie największą część kodu istniejących, jak to możliwe. Jako deweloper Umbraco Mikkel Madsen stanowi "możemy były zadowalająca narzędzi programistycznych firmy Microsoft, które już zostały firma Microsoft zna, takich jak program Microsoft SQL Server, Microsoft Azure SQL Database, ASP.net i Internet Information Services (IIS). Przed poświęcania IaaS lub PaaS w chmurze rozwiązanie, trzeba upewnij się, że go czy obsługuje nasze narzędzia Microsoft i platform, więc nie udostępniamy do wprowadzania zmian duże naszemu kodowi podstawowej. "

Aby spełnić wszystkie jej kryteria, Umbraco tablicą partnera chmury z następujące kwalifikacje:

-   Wystarczające wydajność i niezawodność
-   Obsługa narzędzi rozwoju firmy Microsoft, tak, aby Umbraco inżynierów będą nie musieli całkowicie reinvent ich środowisko projektowania
-   Obecność we wszystkich rynkach geograficznych, w których UaaS konkuruje (potrzeby firmy upewnij się, że ich dostęp do danych szybko i że ich dane są przechowywane w lokalizacji, która spełnia ich regionalne regulacji)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Dlaczego Umbraco wybrany Azure dla UaaS

Zgodnie z Christensen Witold "po ustalając nasze możliwości, wybraliśmy Azure ponieważ spełnia wszystkie naszych kryteria z zarządzaniem i skalowalność znajomości i efektywności. Firma Microsoft Konfigurowanie środowiska na maszyny wirtualne Azure i każdego środowiska ma własne wystąpienie bazy danych SQL Azure z wszystkich wystąpień w puli elastyczne bazy danych. Dzięki oddzieleniu baz danych między rozwoju, organizowanie i środowiskach produkcyjnych, możemy zaoferować naszym klientom niezawodne funkcje wydajności izolacji dopasowane do skali — duży zysk. "

Witold będzie nadal występował, "przed było konieczne ręczne obsługi administracyjnej serwerów baz danych sieci web. Teraz możemy nie trzeba pamiętać o go. Wszystko jest zautomatyzowany — z inicjowania obsługi administracyjnej do oczyszczania. "

Witold jest również powinno ze skalowaniem możliwości oferowane przez Azure. "Pul elastyczne bazy danych są dokładne dopasowanie naszych władz akredytacji bezpieczeństwa oferowania, ponieważ firma Microsoft można dołączyć, dzwoniąc wydajność i rozwijaniu stosownie do potrzeb. Inicjowania obsługi administracyjnej jest łatwe, a nasz konfiguracji z użyciem nam zapewnić wykorzystania maksymalnie." Zjednoczone Witold, "uproszczenia pul elastyczne, oraz zapewnienie na podstawie poziomu usługi DTUs daje możliwości obsługi administracyjnej nowych pul zasobów na żądanie. W ostatnim daszenie kalenicowe jeden z naszych dużych klientów do 100 DTUs w jego rzeczywistym środowisku. Przy użyciu Azure, naszych elastyczne pul udostępniana baz danych klienta z zasobów, które są potrzebne w czasie rzeczywistym bez konieczności przewidywanie wymagania DTU. Krótko mówiąc, naszym klientom Uzyskaj Włącz wokół czas spodziewają i firma Microsoft spełniają naszych umów poziom obsługi wydajności."

Mikkel Madsen sumuje je w górę: "Firma Microsoft została przyjętych algorytmu Azure zaawansowanych łączy typowy scenariusz władz akredytacji bezpieczeństwa (ułatwiającej rozpoczęcie korzystania nowych klientów w czasie rzeczywistym w skali) naszych wzorca aplikacji (wstępnie inicjowania obsługi administracyjnej baz danych, obie rozwoju i live) u góry technologii (w połączeniu z bazą danych SQL Azure za pomocą kolejek Bus usługi Azure)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Z Azure UaaS przekracza oczekiwania klientów

Po wybraniu Azure jako swojego partnera w chmurze, Umbraco udało się dostarczyć klientom UaaS zoptymalizowaną wydajność zarządzania zawartością, bez inwestycji IT zasobu wymagane od siebie rozwiązanie. Zgodnie z opisem Witold, "przyłączyć Deweloper wygody i skalowalność Azure daje i klientami są thrilled z funkcjami i niezawodności. Ogólnie mówiąc był doskonałe zysk dla nas!"
 
## <a name="more-information"></a>Więcej informacji

- Aby dowiedzieć się więcej na temat pul Azure elastyczne bazy danych, zobacz [pul elastyczne bazy danych](sql-database-elastic-pool.md).

- Aby dowiedzieć się więcej na temat Bus usługi Azure, zobacz [Bus usługi Azure](https://azure.microsoft.com/services/service-bus/).

- Aby dowiedzieć się więcej na temat ról w sieci Web i Pracownik, zobacz [Role pracownika](../fundamentals-introduction-to-azure.md#compute). 

- Aby dowiedzieć się więcej na temat wirtualnej sieci, zobacz [wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Aby dowiedzieć się więcej na temat i przywracania kopii zapasowych, zobacz [ciągłości](sql-database-business-continuity.md).  

- Aby dowiedzieć się więcej na temat monitorowania ppols, zobacz [Monitorowanie pul](sql-database-elastic-pool-manage-portal.md). 

- Aby dowiedzieć się więcej na temat Umbraco jako usługi, zobacz [Umbraco](https://umbraco.com/cloud).

