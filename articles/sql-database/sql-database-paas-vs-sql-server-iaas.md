<properties
    pageTitle="SQL (PaaS) bazy danych a programu SQL Server w chmurze na maszyny wirtualne (IaaS) | Microsoft Azure"
    description="Dowiedz się więcej opcji programu SQL Server chmury pasuje do Twojej aplikacji: bazy danych SQL Azure (PaaS) lub SQL Server w chmurze na maszyn wirtualnych Azure."
    services="sql-database, virtual-machines"
    keywords="Chmura programu SQL Server, program SQL Server w chmurze, PaaS bazy danych, w chmurze DBaaS programu SQL Server"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Wybierz pozycję chmury opcja programu SQL Server: bazy danych SQL Azure (PaaS) lub SQL Server na Azure maszyny wirtualne (IaaS)

Azure ma dwie możliwości obsługi obciążenia programu SQL Server w programie Microsoft Azure:

* [Bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/): natywne w chmurze, nazywane także platformy jako bazy danych usługi (PaaS) lub bazy danych jako usługa (DBaaS), która jest zoptymalizowana pod kątem opracowywania aplikacji (władz akredytacji bezpieczeństwa) oprogramowania jako usługi bazy danych SQL. Zapewnia on zgodność z większość funkcji programu SQL Server. Aby uzyskać więcej informacji o PaaS zobacz [Co to jest PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [Program SQL Server na maszyn wirtualnych Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server zainstalowane i przechowywane w chmurze na systemie Windows Server wirtualnych maszyn uruchomionych Azure, nazywane także infrastrukturę jako usługa (IaaS).
Program SQL Server w przypadku Azure maszyn wirtualnych jest zoptymalizowana pod kątem Migrowanie istniejących aplikacji programu SQL Server. Wszystkie wersje i wersje programu SQL Server są dostępne. Zapewnia on 100% zgodności z programem SQL Server, umożliwiając obsługi tyle baz danych jako wymagane i wykonywanie transakcji między bazami danych. Zapewnia pełną kontrolę nad programu SQL Server i systemu Windows.

Dowiedz się, jak poszczególnych opcji ogólnej platformy danych firmy Microsoft i uzyskiwanie pomocy pasujących odpowiedniej opcji do własnych potrzeb biznesowych. Czy można ustalać ich priorytety, oszczędności lub minimalnego Administracja związaną z wcześniejszym innych możliwościach, w tym artykule może ułatwić zdecydować, które rozwiązanie udostępnia przed wymagań, że Cię interesuje najczęściej.


## <a name="microsofts-data-platform"></a>Platformy danych firmy Microsoft

Jedną z pierwszych rzeczy, aby dowiedzieć się, w dowolnej dyskusji Azure i baz danych programu SQL Server lokalnego jest, której można je wszystkie. Platforma danych firmy Microsoft korzysta z technologii programu SQL Server i udostępnia fizycznie lokalnego na komputerach, środowiska chmury prywatne, środowiska chmury prywatne hostowanej innych firm i publicznej chmury. Program SQL Server na Azure mchines wirtualnych umożliwia potrzeb firmy unikatowych i różnorodną za pomocą kombinacji lokalnego i wdrożenia hostowana w chmurze, używając ten sam zestaw produktów serwerowych, narzędzi do tworzenia i specjalizacji w tych środowiskach.

   ![Opcje programu SQL Server w chmurze: programu SQL server na IaaS lub SQL władz akredytacji bezpieczeństwa bazy danych w chmurze.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak pokazano na diagramie, poszczególnych wersji można określony przez poziomie administracji nad infrastruktury (na osi X) i stopnia wydajności kosztów osiągnąć konsolidacji poziomu bazy danych i automatyzacji (na oś Y).

Podczas projektowania aplikacji, cztery podstawowe opcje są dostępne do obsługi programu SQL Server część aplikacji:

- Program SQL Server na-obsługą fizycznych komputerów
- Program SQL Server w środowisku lokalnym z obsługą maszyn (chmury prywatnych)
- Program SQL Server w maszyn wirtualnych Azure (publicznej chmury firmy Microsoft)
- Baza danych SQL Azure (publicznych chmury firmy Microsoft)

W poniższych sekcjach, Poznaj program SQL Server w chmurze publicznej firmy Microsoft: bazy danych SQL Azure i SQL Server na maszyny wirtualne Azure. Ponadto zapoznaj się typowych motivators firm do określenia, które opcja działa najlepiej z aplikacji.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Dokładniejsze przedstawienie bazy danych SQL Azure i SQL Server na maszyny wirtualne Azure

**Bazy danych SQL Azure** to relacyjne bazy danych — jako z usługa (DBaaS) przechowywane w chmurze Azure, która znajduje się w kategorii branży *Oprogramowania jako z usługi (władz akredytacji bezpieczeństwa)* i *Platformy jako z usługi (PaaS)*. [Baza danych SQL](sql-database-technical-overview.md) jest oparty na standardowych sprzętu i oprogramowania jest własnością hostowanej i obsługiwany przez firmę Microsoft. Z bazy danych SQL można tworzyć bezpośrednio w usłudze przy użyciu wbudowanych funkcji i funkcji. Podczas korzystania z bazy danych SQL, możesz płatne z opcjami umożliwiającymi skali większa power identycznego w górę lub na zewnątrz.

**Program SQL Server na Azure wirtualnych maszyn** należy do kategorii branżowe *Infrastruktury jako z usługi (IaaS)* i umożliwia uruchomienie programu SQL Server wewnątrz maszyny wirtualnej w chmurze. Podobnie jak baza danych SQL, jest oparte na standardowy sprzęt własnością, hostowana i obsługiwany przez firmę Microsoft. Używając programu SQL Server na maszyny, można albo płatności — jak możesz przenośna dla licencji programu SQL Server już zastosowana obraz programu SQL Server lub łatwo przy użyciu istniejących licencji. Możesz również łatwo skali, w górę/w dół i Wstrzymaj/Wznów maszyn wirtualnych stosownie do potrzeb.

Zazwyczaj te dwie opcje SQL są zoptymalizowane dla różnych celów:

- **Baza danych SQL** jest zoptymalizowana pod ograniczenie zbiorowy koszt minimum inicjowania obsługi administracyjnej i zarządzania nimi wiele baz danych. Ogranicza go koszty prowadzonego administrowania, ponieważ nie masz do zarządzania maszyn wirtualnych, system operacyjny ani oprogramowania bazy danych. Nie masz Zarządzanie uaktualnienia, wysokiej dostępności lub [kopie zapasowe](sql-database-automated-backups.md). Na ogół bazy danych SQL Azure można znacznie zwiększyć liczbę baz danych zarządzane przez jeden IT lub rozwoju zasobów.
- **Program SQL Server uruchomionych maszyny wirtualne Azure** jest zoptymalizowana pod kątem migracja aplikacji Azure lub rozszerzanie aplikacji lokalnej w chmurze we wdrożeniach hybrydowych. Ponadto można programu SQL Server w maszyny wirtualnej opracowywać i testować tradycyjne aplikacje programu SQL Server. Z programu SQL Server na maszyny wirtualne Azure masz prawa pełnego administracyjne nad dedykowane wystąpienie programu SQL Server i maszyn wirtualnych opartej na chmurze. Jest to doskonałe rozwiązanie, gdy organizacja ma już IT zasobów dostępnych do obsługi maszyn wirtualnych. Te funkcje umożliwiają tworzenie przystosowanych systemu powinien opracować określonych wydajność i wymagań dotyczących dostępności aplikacji.

W poniższej tabeli przedstawiono główne cechy bazy danych SQL i programu SQL Server na maszyny wirtualne Azure:

|       | Baza danych SQL | Program SQL Server w Azure maszyn wirtualnych|
| -------------- | ------------ | ---------------------- |
| **Najlepsza dla:** | Nowe zaprojektowane w chmurze aplikacje, które mają czasu ograniczenia w zakresie opracowywania i obrót. |Istniejące aplikacje, które wymagają szybkiego migracji w chmurze z minimalnymi zmiany. Szybkiego tworzenia i testowania scenariusze gdy nie chcesz kupić sprzęt programu SQL Server nie produkcji lokalnej. |
|| Zespoły, które muszą wbudowanych wysokiej dostępności, odzyskiwanie i uaktualnianie bazy danych. |Zespoły, które można konfigurować i zarządzać wysoką dostępność, odzyskiwanie i poprawianie dla programu SQL Server. Niektóre pod warunkiem, że funkcje automatycznego znacznie uprościć to. |
||Zespoły, których nie chcesz, aby zarządzać system operacyjny i ustawienia konfiguracji.| Jeśli potrzebujesz dostosowane środowisko z uprawnieniami pełnej administratora.|
||Do 1 TB baz danych, lub większych baz danych, które mogą być [poziomo lub pionowo na partycje](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) za pomocą deseń skala w nowym oknie.|Wystąpienia programu SQL Server przy użyciu maksymalnie 64 TB przestrzeni dyskowej. Wystąpienie może obsługiwać dowolną liczbę baz danych zgodnie z potrzebami. |
||[Konstrukcyjnych oprogramowania — jako a-(władz akredytacji bezpieczeństwa) aplikacjami usług](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migrowanie i tworzenia aplikacji w wersji enterprise i hybrydowego.|
|||||
|**Zasoby:**|Nie chcesz, aby zastosować zasobów IT konfiguracji i zarządzania infrastrukturą źródłowych, ale chcesz skupić się na warstwie aplikacji.|Masz kilka zasobów IT konfiguracji i zarządzania. Niektóre pod warunkiem, że funkcje automatycznego znacznie uprościć to.|
|**Całkowity koszt użytkowania:**|Eliminuje kosztów sprzętu i zmniejsza koszty administracyjne.|Eliminuje koszty sprzętu.|
|**Zapewnianie ciągłości:**|Oprócz wbudowane odporności na uszkodzenia infrastruktury możliwości bazy danych SQL Azure udostępnia funkcje, takie jak [Automatyczne kopie zapasowe](sql-database-automated-backups.md), [W chwili przywrócić](sql-database-recovery-using-backups.md#point-in-time-restore) [Przywróć Geo](sql-database-recovery-using-backups.md#geo-restore)i [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md) Aby zwiększyć ciągłości. Aby uzyskać więcej informacji zobacz [Omówienie ciągłości firm bazy danych SQL](sql-database-business-continuity.md).|Program SQL Server na maszyny wirtualne Azure umożliwia ustawianie wysoki rozwiązania odzyskiwania dostępności i danych do określonych potrzeb bazy danych. W związku z tym czy masz system wysoce przeznaczonego dla aplikacji. Można przetestować i uruchomić praca awaryjna przez siebie w razie potrzeby. Aby uzyskać więcej informacji zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server na maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Chmura hybrydowych:**|Aplikacja lokalnego na dostęp do danych w bazie danych SQL Azure.|Z programu SQL Server na pośrednictwem Azure SMS możesz mieć aplikacje, które Uruchom częściowo w chmurze i częściowo lokalnego. Na przykład można rozszerzyć sieci lokalnej i domena usługi Active Directory w chmurze za pośrednictwem [Wirtualnych sieci Azure](../virtual-network/virtual-networks-overview.md). Ponadto można przechowywać pliki danych lokalnych w magazynie Azure za pomocą [Pliki danych programu SQL Server w Azure](http://msdn.microsoft.com/library/dn385720.aspx). Aby uzyskać więcej informacji zobacz [Wprowadzenie do programu SQL Server 2014 hybrydowych chmury](http://msdn.microsoft.com/library/dn606154.aspx).|
||Obsługa [replikacji transakcji programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako replikacji danych.|W pełni obsługuje [transakcji replikacji programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [Grupy dostępności (AlwaysOn)](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), usług Integration Services i dziennika wysyłki replikacji danych. Ponadto tradycyjnych wykonywania kopii zapasowych programu SQL Server są w pełni obsługiwane|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motywacji firm dotyczące wybierania bazy danych SQL Azure lub SQL Server na maszyny wirtualne Azure

### <a name="cost"></a>Koszt

Czy jesteś uruchamiania, który jest strapped gotówkowych lub zespołu w firmie wskazanych, działająca w obszarze ograniczenia przylegające budżetu ograniczone funduszy jest zazwyczaj sterownik podstawowy decyzje dotyczące obsługi baz danych. W tej sekcji informacje o rozliczenie i licencjonowanie podstawowych zadań w Azure w odniesieniu do tych dwóch opcji relacyjnej bazy danych: bazy danych SQL i programu SQL Server na maszyny wirtualne Azure. Również informacje o wykonywaniu obliczeń koszt całkowity aplikacji.

#### <a name="billing-and-licensing-basics"></a>Rozliczenia i licencjonowanie — informacje podstawowe

**Baza danych SQL** sprzedaży dla klientów usług, nie z licencją.  [Program SQL Server na maszyny wirtualne Azure](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) sprzedaży przy uwzględniane licencji, którą możesz zapłacić na minutę. Jeśli masz licencję istniejących, można go.  

Obecnie **Bazy danych SQL** jest dostępna w kilka poziomów usług, które wystawiona w co godzinę przy stałej stawce według poziomu usług i poziom wydajności, które możesz wybrać. Ponadto konta dla ruchu wychodzącego Internet na zwykłą [szybkość przesyłania danych](https://azure.microsoft.com/pricing/details/data-transfers/). Warstwy usługi podstawowe, Standard i Premium są przeznaczone do przeprowadzania przewidywalne wydajności z wieloma poziomami wydajności zgodnie z wymaganiami Szczyt aplikacji. Możesz zmienić między warstwy usługi i poziomy wydajności zgodnie z potrzeb różnej przepustowość aplikacji. Jeśli baza danych zawiera dużą transakcji i musi obsługiwać wielu użytkowników jednocześnie, zalecamy warstwa usług Premium. Aby uzyskać najnowsze informacje o bieżącym poziomów obsługiwanej usługi zobacz [Warstwy usługi bazy danych SQL Azure](sql-database-service-tiers.md). Można także tworzyć [pul elastyczne bazy danych](sql-database-elastic-pool.md) na udostępnienie zasobów wydajności między wystąpień bazy danych.

Z **Bazy danych SQL**oprogramowania bazy danych jest automatycznie skonfigurowany, poprawkami i uaktualnione przez firmę Microsoft, co pozwala zmniejszyć koszty administracji. Oprócz możliwości [wbudowanych kopii zapasowej](sql-database-automated-backups.md) ułatwiają osiągnięcia znaczące oszczędności, zwłaszcza w przypadku, gdy masz wiele baz danych.

Z **Programu SQL Server na maszyny wirtualne Azure**można użyć dowolnej z dostarczonego przez platformy obrazy programu SQL Server (które obejmuje licencję) lub wyświetlić licencja programu SQL Server. Wszystkie obsługiwane wersje programu SQL Server (2008R2, 2012, 2014, 2016) i wersji (Deweloper, Express, sieci Web, Standard, Enterprise) nie są dostępne. Ponadto Przesuń i-właścicielem-licencji wersje (BYOL) obrazy są dostępne. Jeśli przy użyciu Azure podane obrazy, koszty operacyjne zależy rozmiar pamięci Wirtualnej i wersję programu SQL Server, możesz wybrać. Bez względu na rozmiar pamięci Wirtualnej lub wersji programu SQL Server możesz zapłacić koszt licencji na minutę programu SQL Server i systemu Windows Server, wraz z kosztem magazyn Azure dysków maszyn wirtualnych. Opcja rozliczeń na minutę umożliwia jak potrzebnych licencji bez zakupu dodatku programu SQL Server za pomocą programu SQL Server dla. Jeśli licencja programu SQL Server Azure, są naliczane dla systemu Windows Server i koszty miejsca do magazynowania. Aby uzyskać więcej informacji dotyczących wyświetlić swój własne licencji zobacz [Mobilności licencji poprzez Software Assurance Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Obliczanie kosztu sumy aplikacji

Podczas uruchamiania przy użyciu platformy chmury, koszt uruchomiona aplikacja obejmuje koszty administracyjne i rozwoju oraz koszty usługi platformy publicznej chmury.

Poniżej przedstawiono szczegółowe koszt obliczeń dla aplikacji uruchomionych w bazie danych SQL i SQL Server Azure maszyny wirtualne:

**Podczas korzystania z bazy danych SQL Azure:**

*Całkowity koszt stosowania = kosztów administracyjnych wysoce zminimalizowanym + koszty opracowywania oprogramowania + koszty serwisu bazy danych SQL*

**Podczas używania programu SQL Server na maszyny wirtualne Azure:**

*Całkowity koszt stosowania = koszt rozwoju oprogramowania wysoce zminimalizowanym + koszty administracyjne + koszty licencjonowania programu SQL Server i systemu Windows Server + koszty Azure miejsca do magazynowania*

Aby uzyskać więcej informacji dotyczących ceny zobacz następujące zasoby:

- [Cennik bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/)
- [Cennik maszyn wirtualnych](https://azure.microsoft.com/pricing/details/virtual-machines/) dla [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) i dla [systemu Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure ceny kalkulatora](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Istnieje mały podzbiór funkcji w programie SQL Server, które nie są stosowane lub nie są dostępne z bazą danych SQL. Uzyskać więcej informacji, zobacz [ograniczenia ogólne bazy danych SQL i wskazówki](sql-database-general-limitations.md) i [informacje bazy danych SQL języku Transact-SQL](sql-database-transact-sql-information.md) . Jeśli chcesz przenieść istniejącego rozwiązania programu SQL Server w chmurze, zobacz [Migrowanie bazy danych programu SQL Server do bazy danych SQL Azure](sql-database-cloud-migrate.md). Podczas migracji istniejącą aplikację programu SQL Server lokalnego do bazy danych SQL, warto rozważyć aktualizowanie aplikacji, aby korzystać z funkcji, które usługi w chmurze. Na przykład można rozważyć hosta usługi warstwy aplikacji, aby zwiększyć korzyści koszt za pomocą [Usługi aplikacji sieci Web Azure](https://azure.microsoft.com/services/app-service/web/) lub [Usług w chmurze Azure](https://azure.microsoft.com/services/cloud-services/) .

### <a name="administration"></a>Administracja

W wielu firmach decyzji o przejście do usługi w chmurze jest tak jak dużo o Odciążanie złożoność administracji jego koszt. Z **Bazy danych SQL**Microsoft zarządza używanego sprzętu. Microsoft automatycznie replikuje wszystkich danych w celu zapewnienia wysokiej dostępności, konfiguruje aktualizacje oprogramowania bazy danych, zarządza równoważenia obciążenia i czy przezroczysty pracy awaryjnej, jeśli występuje błąd serwera. Możesz nadal administrowanie bazy danych, ale nie są już potrzebne do zarządzania aparat bazy danych, systemów operacyjnych i sprzętu.  Przykładami elementy, które można kontynuować administrowanie baz danych i logowania, indeks i Dostosowywanie kwerendy, a inspekcji i zabezpieczeń.

Z **Programu SQL Server na maszyny wirtualne Azure**masz pełną kontrolę nad systemem operacyjnym i Konfiguracja wystąpienia programu SQL Server. Z Głosowa jest możesz zdecydować, kiedy aktualizacji i uaktualnianie systemu operacyjnego i oprogramowania bazy danych, a kiedy zainstalowanie dodatkowego oprogramowania takie jak oprogramowania antywirusowego. Niektóre funkcje automatycznego znajdują się znacznie ułatwia poprawiania, kopii zapasowej i wysoka dostępność. Ponadto można określić rozmiar maszyn wirtualnych, liczba dysków i ich konfiguracji przestrzeni dyskowej. Azure umożliwia zmianę rozmiaru maszyny, stosownie do potrzeb. Aby uzyskać informacje zobacz [maszyn wirtualnych i rozmiarów chmury usługi Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Umowa dotycząca poziomu usług (SLA)

W przypadku wielu działów informatycznych spotkania zobowiązań wydłużyć czas z umowy poziomu usług (SLA) jest najwyższy priorytet. W tej sekcji przyjrzymy się Umowa dotycząca poziomu usług stosuje do każdej bazy danych hostingu opcji.

W przypadku Basic **Bazy danych SQL** , Standard i Premium warstwy usługi Microsoft udostępnia dostępność SLA 99,99%. Aby uzyskać najnowsze informacje zobacz [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/sql-database/). Najnowsze informacje na bazy danych SQL warstwy usługi i plany ciągłości dla obsługiwanych firm zobacz [Warstwy usługi](sql-database-service-tiers.md).

**Program SQL Server uruchomionych maszyny wirtualne Azure**firma Microsoft udostępnia dostępność SLA % 99,95, obejmującą tylko maszyny wirtualnej. Ta umowa dotycząca poziomu usług nie obejmuje procesów (na przykład programu SQL Server) uruchomionych na maszyn wirtualnych i wymaga, aby udostępnić co najmniej dwa wystąpienia maszyn wirtualnych w zestawie dostępność. Aby uzyskać najnowsze informacje zobacz [Maszyn wirtualnych Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Baza danych wysokiej dostępności (HA) w maszyny wirtualne należy skonfigurować jedną z opcji szybkiej obsługiwane w programie SQL Server, takich jak [Grupy dostępności (AlwaysOn)](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Za pomocą opcji obsługiwanych wysokiej dostępności nie udostępnia dodatkowe SLA, ale umożliwia uzyskanie > 99,99% dostępność bazy danych.

### <a name="market"></a>Czas na rynku

**Baza danych SQL** jest właściwe rozwiązanie w przypadku aplikacji zaprojektowane w chmurze, gdy wydajność deweloperów i szybkie czasu na rynku są krytyczne. Z funkcje programistyczne przypominających Zarejestrowana jest idealny do chmury Twórcy architektury i deweloperzy jak go obniża konieczności zarządzania źródłowych systemu operacyjnego i bazy danych. Na przykład można użyć [Interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dn505719.aspx) i [Poleceń cmdlet środowiska PowerShell](http://msdn.microsoft.com/library/azure/dn546726.aspx) Automatyzowanie i zarządzać nimi operacji administracyjnych tysięcy baz danych. Funkcje, takie jak [Elastyczne pul bazy danych](sql-database-elastic-pool.md) umożliwia skoncentrowanie się na warstwie aplikacji i dostarczanie rozwiązania rynku szybciej.

**Program SQL Server uruchomionych maszyny wirtualne Azure** jest doskonałe, jeżeli istniejące czy nowe aplikacje wymagają dużych baz danych, powiązanych baz danych lub dostęp do wszystkich funkcji programu SQL Server lub w systemie Windows. Należy również dobrze, gdy chcesz przeprowadzić migrację istniejącej lokalnej aplikacji i baz danych Azure jako-jest. Ponieważ nie trzeba zmienić prezentacji, aplikacji i danych warstwy, zaoszczędzić czas i budżet na rearchitecting istniejące rozwiązania. Zamiast tego możesz skoncentrować się na migrowanie wszystkich rozwiązanie Azure i wykonując kilka optymalizacji wydajności, które mogą być wymagane przez platformę Azure. Aby uzyskać więcej informacji zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server na maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Podsumowanie

Ten artykuł zbadane bazy danych SQL i programu SQL Server na Azure wirtualnych maszyn i omówiony typowych motivators firm, które mogą mieć wpływ na swoją decyzję. Poniżej przedstawiono podsumowanie sugestii dotyczących brać pod uwagę:

Wybierz pozycję **Baza danych SQL Azure** , jeśli:

- Tworzysz nowe aplikacje oparte na chmurze, aby można było korzystać z oszczędności i podaj optymalizacji wydajności usług w chmurze. Ta metoda zalety usługi w chmurze w pełni zarządzane, pomaga w dolnym początkowej godziny do rynku i umożliwiają długoterminowe optymalizacji kosztów.

- Chcesz mieć Microsoft wykonywania typowych operacji zarządzania na baz danych i wymagają silniejsze dostępność zwiększany baz danych.



Wybierz pozycję **programu SQL Server na Azure pośrednictwem SMS** , jeśli:

- Masz istniejące aplikacje lokalnych, których chcesz przeprowadzić migrację lub rozszerzania w chmurze, lub jeśli chcesz utworzyć aplikacji przedsiębiorstwa większym niż 1 TB. Ta metoda zapewnia korzyści, jakie zgodności SQL 100%, pojemność dużej bazy danych, pełną kontrolę nad programu SQL Server i systemu Windows i bezpiecznego tunelowania do lokalnego. Tej metody minimalizuje koszty rozwoju i modyfikacji istniejących aplikacji.

- Masz istniejących zasobów IT i ostatecznie mogą być właścicielami poprawki, zapasowych i wysoką dostępność bazy danych. Zwróć uwagę, że niektóre funkcje automatycznego znacznie uprościć tych operacji. 


## <a name="next-steps"></a>Następne kroki
- Zobacz [bazy danych SQL samouczek: tworzenie bazy danych SQL w minutach za pomocą portalu Azure](sql-database-get-started.md) rozpocząć pracę z bazą danych SQL.
- [Cenach bazy danych SQL] (https://azure.microsoft.com/pricing/details/sql-database/).
- Zobacz wprowadzenie do programu SQL Server na maszyny wirtualne Azure [należy maszyny wirtualnej programu SQL Server Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) .
- Zobacz [programu SQL Server na Azure maszyn wirtualnych: ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
