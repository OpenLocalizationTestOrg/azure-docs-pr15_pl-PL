<properties
   pageTitle="Analiza przypadku Azure - Daxko/CSI bazy danych Azure SQL | Microsoft Azure"
   description="Więcej informacji na temat jak Daxko/CSI używa bazy danych SQL, aby przyspieszyć cyklu opracowywania i zwiększyć jego działu pomocy technicznej i wydajność"
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
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI używane Azure przyspieszyć cyklu opracowywania i zwiększyć jego działu pomocy technicznej i wydajność

![Logo Daxko/CSI](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Oprogramowanie Daxko/CSI wypadek wezwanie: jej klientów centra przydatności i odtworzenie została rosnących szybko, dzięki powodzenia jego rozwiązanie pełna oprogramowania przedsiębiorstwa, ale zatrzymując potrzeb infrastruktury informatycznej dla tego klienta rosnących podstawowej została testowania firmy personelem INFORMATYCZNYM. Firmy rosnącej złożoności zostało ograniczone przez wzrost ogólnych operacji, szczególnie w celu zarządzania rosnących bazy danych. Odpowiadającą ogólnych tej operacji została Wycinanie do rozwoju zasobów dla nowych inicjatywy, takie jak nowe funkcje mobilności dla oprogramowania firmy.

Według David Molina, dyrektor rozwoju produktu na Daxko/CSI Azure do dyspozycji oprogramowania CSI model (PaaS) platformy jako usług, który jest potrzebne uprościć zarządzanie bazą danych, zwiększyć skalowalność i zwolnić zasoby skoncentrować się na oprogramowanie zamiast ops. "Świetne rozwiązanie dla nas jest baza danych SQL azure. Nie ma martwić się o zachowaniu programu SQL Server, klaster pracy awaryjnej i wszystkie inne infrastruktury musi jest idealnym rozwiązaniem nam."

Po migracji do Azure, oprogramowanie CSI musi Operatorzy tylko dwa radzić ponad 600 klienta baz danych. Firma używa pul elastyczne bazy danych SQL Azure, aby przenieść baz danych klienta na podstawie rozmiaru i potrzebujesz.

Molina będzie nadal występował, "naszym klientom mieli świadomość zmiany natychmiast. Przed elastyczne pul od czasu do czasu miały limity czasu i innych problemów w okresach serii. Z Azure pule elastyczne mogą serii zgodnie z potrzebami i używać oprogramowania bezproblemowe."

Ponadto, oprócz ulepszenia wydajności dla klientów, Azure elastyczne bazy danych pul nasz zasoby oprogramowania CSI skoncentrować się na opracowywania nowych usług i funkcji, zamiast sposób postępowania z pracy i zarządzania. Te zasoby IT pomógł oprogramowania CSI poprawić jego przedsiębiorstwa oprogramowanie, SpectrumNG ułatwiające prowadzenie siłowni członków, poprawić wydajność personelu i nadaj pracowników i członków dostępu mobilnego dla zadań interakcyjnych i powiadomień w czasie rzeczywistym.

Azure pomogło również oprogramowania CSI przyspieszenia i poprawić rozwoju i cykl jakości (QA), umożliwiając opcje automatyzacji. W przypadku implementacji Azure firmy kompilacji menedżerowie mogą grupowanie składniki za pomocą kliknięcia przycisku. Jak opisano Molina "w ramach cyklu wersji, pytania i odpowiedzi — jest teraz możliwość wdrażanie w środowisku testowym w Azure, której blisko symuluje naszych stos produkcji. Firma Microsoft natychmiastowe wdrażanie kompilacjach do naszych środowisku testowym do wybiera zmiany. To duży zysk dla nas, ponieważ nie udostępniamy spójności badań przed nim."

## <a name="offloading-to-the-cloud"></a>Odciążanie w chmurze

Przed przejściem w chmurze, CSI oprogramowania zostały pomyślnie utworzono własnej multitenant infrastruktury w lokalnym centrum danych w Houston w górę. Jak rozwinięta firmy, wypadek rosnącymi rosnący problemy z zakupu, inicjowania obsługi administracyjnej i zachowywanie wszystkich sprzęt i oprogramowanie wymagane do obsługi klientów. IT personelu obsługę operacji stał się innego gardło, które doprowadziły do spowolnienia inicjowania obsługi administracyjnej nowych zasobów i wdrażania nowych usług klientom.

Oprogramowanie CSI elementu w chmurze opcje usuwaniu tego ogólnych tak, aby może skoncentrować się na jego kod, a nie jego operacji. Firmie wykryte wielu dostawców górny chmury tylko oferują infrastruktury jako usługi (IaaS) rozwiązania, które wymagają jeszcze dużych personelem INFORMATYCZNYM, aby zarządzać stos IaaS. Dzięki temu oprogramowania CSI określić, że rozwiązanie Azure PaaS były najlepsze dopasowanie do swoich potrzeb. Wyjaśniono Molina "Azure otrzymuje przeszkadza, oprogramowanie sprzętowe i systemowe, więc możemy skupić się na zakupione oprogramowanie, redukując naszych ogólnych IT."

## <a name="making-the-transition-to-azure"></a>Przejścia Azure

Po wybraniu Azure jego rozwiązania PaaS, oprogramowanie CSI rozpoczęcia migracji jej infrastruktury wewnętrznej bazy danych i baz danych w chmurze. Przed Azure klienci SpectrumNG wymagane do zainstalowania aplikacji, które przekazują za pomocą usługi Windows Communication Foundation (WCF) na końcu Wstecz. Według Molina "mimo że niektórzy klienci hostowanej wszystko w ich centrach danych, możemy utworzono się produktu, który ma być multitenant. Firma Microsoft hostowana publikowane w centrum danych w Houston, przy użyciu programu SQL Server do przechowywania danych.

"Produkt oferuje również uwzględniane członka skierowaną portalu (napisana ASP.net), który ma być etykietą biały klienta witrynę internetową i API SOAP do pomocy technicznej online stron i integracji innych firm".

Migracja w chmurze nie długo architektury. Według Molina, "Większość nakładu zająć zmianę sposobu możemy odczytywać informacje o plikach konfiguracji, zmianę scentralizowane parametry połączenia i automatyzowanie opakowanie, przekazywanie i wdrażania naszych wersjach".

Umożliwiają automatyzację kompilacji inżynierów oprogramowania CSI umożliwia Azure programu PowerShell oraz interfejsy API pozostałych tworzenie pakietów i przekazać je do środowiska wzorcowego wydania dziennie.
Ogólna przejścia do wdrożenia usługi Azure opartej na chmurze polega szybko i płynnie dla zespołu IT oprogramowania CSI. Wyjaśniono Molina "we wszystkich, było środowisku beta w chmurze w trzech do czterech tygodni pracy nad projektem. Który był zaskakująco zysk dla nas."

Po Konfigurowanie i testowanie środowiska, oprogramowanie CSI rozpoczęcia migracji klientów. Klienci korzystający z już hostingu oprogramowania CSI przejścia była niemal bezproblemowa. W przypadku migracji z lokalnego wdrożenia klientów, zajęła trochę czasu, dodatkowe migracji w chmurze, ale był nadal przede wszystkim sytuacji bezpłatne dla klientów i CSI oprogramowania.

Dla odbiorców CSI oprogramowania przez pracowników działu informatycznego zgodne z następującym procesem do płycie ich Azure:

1.  Azure skryptów programu PowerShell są używane do obracania się nowej bazy danych dla klienta; wszystkich odbiorców rozpocząć w warstwie premium, zapewniające za mało początkowej przepustowość przejścia.
2.  Jeśli to możliwe, oprogramowanie CSI Przenoszenie istniejących danych do wystąpienia bazy danych SQL Azure za pomocą Kreatora migracji SQL Azure.
3.  Na koniec programu Microsoft SQL Server Integration Services (SSIS) są używane do uzgadnianie niezgodności w danych lub wykonywanie oczyszczania danych zgodnie z wymaganiami.

Obecnie około 99 procent klientów CSI oprogramowania są obsługiwane w Azure, między czterema regionalnych centrach danych (Ameryka Północna centralnej, Południowej Central Wschód i zachód). Konfigurując centrach danych w regionu geograficznego każdego z klientów, opóźnienie jest ograniczone do minimum.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Pule Azure elastyczne bazy danych zwolnienia zasobów IT

Kilka funkcji Azure Pomoc oprogramowania CSI shift przed infrastruktury i operacje ograniczony do funkcji i rozwoju ograniczony. Być może największych korzyści został z pul elastyczne bazy danych.

Oprogramowanie CSI zapewnia obecnie około 550 baz danych dla klientów. Przed elastyczne pul było trudne do zarządzania tym wiele baz danych w strukturze warstwy. Menedżerowie OPS musiała przypisywanie poziomów wydajności na podstawie potrzeb serii klientów, które znaczną IT-zasób obciążenie. Z zestawów elastyczne bazy danych menedżerów można przypisać dzierżaw premium lub standardowy puli, stosownie do potrzeb i następnie przenoszenie klientów w oparciu o rozmiarze i potrzebujesz. Klienci mieli niemal natychmiast; świadomość skutków pul elastyczne bazy danych przed elastyczne pul klientów sprzedał limity czasu i innych problemów w okresach zastosowania serii, ale z elastycznych pul klientów, mogą występować seria aktywności stosownie do potrzeb, a one nadal korzystać SpectrumNG bez problemów.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active replikacji Geo przyspiesza raportowania

Kilka klientów CSI oprogramowania są również skorzystać z Azure Active Geo replikacji. Z aktywnej Geo replikacji, maksymalnie cztery czytelne pomocniczej bazy danych można skonfigurować w regionach takich samych lub różnych centrum danych. Oprogramowanie CSI korzysta aktywne Geo-replikacji na dwa sposoby: najpierw pomocniczej bazy danych są dostępne w przypadku awaria centrum danych lub brak możliwości połączenia podstawowego bazy danych. i drugi, pomocniczej bazy danych można odczytać i może służyć do offload obciążenia tylko do odczytu, takich jak Raportowanie zadań. Za pomocą tego świadczenia niektórzy klienci oprogramowania CSI przyspieszenie raportowania przepływy pracy.

## <a name="csi-software-application-logic-and-architecture"></a>Architektura i oprogramowania CSI logiki aplikacji

SpectrumNG używa role w sieci web. Ponieważ aplikacja jest wiele dzierżawy, usługa WCF jest używana do obsługi początkowe żądanie połączenia od klientów. Jak Molina stany, "żądanie służy do identyfikowania każdego z klientów, które następnie pozwoli utworzyć parametry połączenia się z ich bazami danych czynność, niezależnie od trzeba wykonać."

Dla poziomu sieci web jej usługi oprogramowanie CSI korzysta z Azure automatyczne skalowanie, oparte na datę i godzinę. Dostępne zasoby są automatycznie zwiększana, aby zezwalały na większym użyciem w godzinach pracy według strefę czasową dla każdego regionalnym centrum danych. Zasoby są także ustawić skali w weekendy, gdy potrzeb klienta są niższe.

     
![Architektura Daxko/CSI](./media/sql-database-implementation-daxko/figure1.png)

Rysunek 1. Roli Pracownik usługi cloud pobiera dane strukturalne z bazy danych SQL Azure i półstrukturalnych danych z magazynu w tabeli. Użytkownicy SpectrumNG interakcję, że danych za pomocą chmurę usługi ról w sieci web.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Przy użyciu aplikacji sieci web i warstwa plan sieci web dla aplikacji dla urządzeń przenośnych

Przy użyciu nasz zasobów dla oprogramowania CSI bazy danych SQL Azure, aby włączyć nowe inicjatywy, w tym pełną platformę urządzeń przenośnych na podstawie interfejs API niestandardowych hostowanych w aplikacjach sieci web Azure. Platforma umożliwia siłowni członków i personelu, aby sprawdzić harmonogramy, rezerwowanie klas i odbierać wiadomości za pomocą urządzeń przenośnych.

Platformy używa opartych na usługach architektura (SOA) podjęcie jeden składnik — system sprzedaży (punkt sprzedaży) lub sprzedaży systemu, takich jak — przenieść go na bieżąco do innego planu sieci web, a następnie pokrętła usługi do obsługi tego składnika, pozostawiając innych możliwościach na oryginalnego planu sieci web. Taką możliwość zapewnia elastyczność imponujące CSI oprogramowania i pomaga zachować kosztów w dół.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure umożliwia oprogramowania CSI deweloperów fokusu na aplikacji i usług

Baza danych SQL Azure nie jest po prostu zwiększa to SpectrumNG klientów, którzy korzystają z usługi szybkie, niezawodne, jest również duży zysk dla oprogramowania CSI personelem INFORMATYCZNYM i deweloperów. Przenosząc ops Azure w chmurze, oprogramowanie CSI zmniejszone jego obciążenie zasobów i infrastruktury, znacznie skrócić jego programistycznych, a nie jest już potrzeby micromanage baz danych, aby zoptymalizować dla jego dzierżaw.

## <a name="more-information"></a>Więcej informacji

- Aby dowiedzieć się więcej na temat pul Azure elastyczne bazy danych, zobacz [pul elastyczne bazy danych](sql-database-elastic-pool.md).

- Aby dowiedzieć się więcej na temat narzędzia bazy danych i elastycznym skalowania, zobacz [Narzędzia elastyczne bazy danych i skalowanie elastyczne](sql-database-elastic-scale-get-started.md).

- Aby dowiedzieć się więcej na temat migrowania bazy danych programu SQL Server, zobacz [Kreator migracji SQL Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Aby dowiedzieć się więcej o aktywnej Geo replikacji, zobacz [Replikacja Geo Active](sql-database-geo-replication-overview.md).

- Aby dowiedzieć się więcej na temat ról w sieci Web i Pracownik, zobacz [Role pracownika](../fundamentals-introduction-to-azure.md#compute). 

- Aby dowiedzieć się więcej na temat Bus usługi Azure, zobacz [Bus usługi Azure](https://azure.microsoft.com/services/service-bus/).

- Aby dowiedzieć się więcej na temat Automatyczne skalowanie, zobacz [Skalowanie usług w chmurze](../cloud-services/cloud-services-how-to-scale.md).
