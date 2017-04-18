<properties
    pageTitle="Program SQL Server aplikacji desenie maszyny wirtualne | Microsoft Azure"
    description="W tym artykule opisano wzorców aplikacji dla programu SQL Server na maszyny wirtualne Azure. Przewiduje rozwiązanie Twórcy architektury i deweloperzy foundation dobre architektura i projektu."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Desenie aplikacji i strategii rozwoju dla programu SQL Server w środowisku maszyn wirtualnych Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Podsumowanie:
Określanie, które deseniu aplikacji lub deseni pozwala za pomocą aplikacji oparte na serwerze SQL platformy Azure środowisko jest ważne decyzję i wymaga pełne zrozumienie istoty sposoby współdziałania programu SQL Server i każdy element infrastruktury Azure. Z programem SQL Server w usługach infrastruktury Azure można łatwo przeprowadzić migrację, obsługa i monitorowanie aplikacji programu SQL Server wbudowany w systemie Windows Server maszyn wirtualnych w Azure.

Celem tego artykułu jest zapewnienie rozwiązanie Twórcy architektury i deweloperzy foundation dobre architektura i projektu, w której można stosować podczas migracji istniejących aplikacji do Azure, a także opracowywania nowych aplikacji platformy Azure.

Dla każdego wzorca aplikacji spowoduje znalezienie scenariusz lokalnego, jego odpowiednich rozwiązanie obsługiwanych w chmurze i powiązanych zaleceniach technicznych. Ponadto, w tym artykule omówiono strategii rozwoju specyficzne dla Azure tak, aby aplikacji można zaprojektować poprawnie. Ze względu na wiele wzorców możliwości zastosowania zaleca się, że Twórcy architektury i deweloperzy należy wybrać najbardziej odpowiedni wzór ich aplikacji i użytkowników.

**Techniczne Współautorzy:** Śledź Vargas Artur Luis, Madhan Arumugam Ramakrishnan

**Recenzentów techniczne:** Szlifierki Corey Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Coriani Silvano, Schackow Stefana Michał Hickey, Michał Wieman, Xin Jin

## <a name="introduction"></a>Wprowadzenie

Dzięki oddzieleniu składników warstwy innej aplikacji na różnych komputerach, a także jak składniki można tworzyć różnego typu n warstwowych aplikacji. Na przykład możesz umieścić z aplikacją kliencką i firm reguł składników na jednym komputerze, warstwa frontonu sieci web i składniki warstwy danych programu access na innym komputerze i warstwa wewnętrznej bazy danych na innym komputerze. Ten rodzaj struktury pomaga wyodrębnienia każdej warstwy od siebie. Jeśli zmienisz, skąd pochodzą dane, nie musisz zmienić klienta lub aplikacji sieci web, ale tylko składniki poziomu dostępu do danych.

Aplikacja typowe *n warstwowych* zawiera warstwie prezentację, warstwa firm i warstwy danych:


| Warstwy              | Opis                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Prezentacji** | *Warstwa prezentacji* (warstwa, warstwa zewnętrzną) jest warstwy, w której użytkownicy interakcji z aplikacją.                                                                      |
| **Business**     | *Business warstwa* (środkowa warstwa) jest warstwy, która warstwa prezentacji i warstwy danych za pomocą komunikowania się między sobą i zawiera podstawowe funkcje systemu. |
| **Dane**         | *Warstwy danych* zasadniczo jest serwer, na którym są przechowywane dane aplikacji (na przykład na serwerze z programem SQL Server).                                                             |

Warstwy aplikacji opisują logiczne grupowanie funkcji i składników w aplikacji; Dlatego poziomów opisują rozkład fizycznie funkcji i składników na osobne serwerów fizycznych, komputerów, sieci lub lokalizacji zdalnych. Warstwy aplikacji mogą znajdować się na tym samym komputerze fizycznym (tej samej warstwie) lub rozłożoną różnych komputerach (n warstwa), a składniki w każdej warstwie komunikacji ze składnikami na innych warstwach za pośrednictwem interfejsów przejrzyste. Możesz myśleć poziomu terminów jako o desenie fizycznie dystrybucji takich jak dwupoziomowy trzeciej warstwy i n warstwowych. **Wzór aplikacji poziomu 2** zawiera dwie warstwy aplikacji: serwer aplikacji i serwer bazy danych. Bezpośrednie komunikacji odbywa się między serwer aplikacji i serwer bazy danych. Serwer aplikacji zawiera zarówno poziomu sieci web i firm warstwa składniki. Za pomocą **aplikacji poziomu 3 deseniu**, są trzy warstwy aplikacji: server, serwera aplikacji, który zawiera warstwa logiki biznesowej i/lub składniki dostępu do danych warstwy firm i serwer bazy danych w sieci web. Komunikacji między serwerem sieci web i serwer bazy danych ma miejsce na serwerze aplikacji. Aby uzyskać szczegółowe informacje o warstwy i warstw aplikacji zobacz [Podręcznik Architektura aplikacji Microsoft](https://msdn.microsoft.com/library/ff650706.aspx).

Przed rozpoczęciem pracy w tym artykule, należy użyć wiedzę na podstawowe pojęcia programu SQL Server i Azure. Aby uzyskać informacje zobacz [Dokumentacji SQL Server — książki Online](https://msdn.microsoft.com/library/bb545450.aspx), [Programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md) i [Azure.com](https://azure.microsoft.com/).

Ten artykuł zawiera opis kilku wzorców aplikacji, które mogą być odpowiednie dla prostych aplikacji, a także aplikacje skomplikowanej przedsiębiorstwa. Przed szczegółów każdego wzorca, zaleca się, że należy zapoznać się z usługami miejsca do magazynowania danych dostępne w Azure, takich jak [Magazyn Azure](../storage/storage-introduction.md), [Bazy danych SQL Azure](../sql-database/sql-database-technical-overview.md)i [SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md). Aby najlepsze decyzje projektowe dla aplikacji, zrozumieć, kiedy należy używać wyraźnie usługę miejsca do magazynowania danych.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Wybieranie programu SQL Server w Azure maszyny wirtualnej, gdy:

- Potrzebujesz kontrolki programu SQL Server i systemu Windows. Na przykład to mogą zawierać wersja programu SQL Server, specjalne poprawki, konfigurację wydajności itd.

- Potrzebujesz pełnej zgodności z programu SQL Server w środowisku lokalnym i chcesz przenieść istniejące aplikacje Azure jako-jest.

- Użytkownik chce wykorzystać możliwości środowiska Azure, ale bazy danych SQL Azure nie obsługuje wszystkich funkcji, które wymagają aplikacji. Może to dotyczyć następujących obszarów:

    - **Rozmiar bazy danych**: W tym czasie, w tym artykule została zaktualizowana, baza danych SQL obsługuje bazę danych do 1 TB danych. Jeśli aplikacja wymaga więcej niż 1 TB danych i nie chcesz, aby zaimplementować sharding niestandardowych rozwiązań, zaleca się używanie programu SQL Server w maszyn wirtualnych Azure. Aby uzyskać najnowsze informacje zobacz [Skalowanie się Azure baz danych programu SQL](https://msdn.microsoft.com/library/azure/dn495641.aspx) i [warstwy usługi bazy danych SQL Azure i poziomy wydajności](../sql-database/sql-database-service-tiers.md).
    - **Zgodność HIPAA**: klienci ochrony zdrowia i niezależnych dostawców oprogramowania (ISV) warto wybrać [Programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md) zamiast [Bazy danych SQL Azure](../sql-database/sql-database-technical-overview.md) , ponieważ program SQL Server w maszyn wirtualnych Azure jest objęta przez HIPAA firm skojarzyć umowy (BAA). Aby uzyskać informacje na temat zgodności, zobacz [Microsoft Azure Trust Center: zgodność](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Funkcje na poziomie wystąpienie**: W tym momencie bazy danych SQL nie obsługuje funkcji, które live poza bazy danych (takich jak serwerów połączonych agenta zadań, FileStream, pośrednik usługi, itp.). Aby uzyskać więcej informacji zobacz [wskazówki bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>warstwy 1 (proste): pojedynczy maszyn wirtualnych

Wzór tej aplikacji należy wdrożyć aplikację programu SQL Server i bazy danych usługi maszyny wirtualnej autonomicznego Azure. Tym samym komputerze wirtualnych zawiera aplikacji sieci web i klienta, składniki biznesowej, Warstwa dostępu do danych i serwer bazy danych. Prezentacja, firm i kod dostępu danych logicznie rozdzielono, ale fizycznie znajdują się w jednym serwerze. Większość klientów początkowych tego wzorca aplikacji, a następnie skalować się, dodając więcej role w sieci web lub maszyn wirtualnych do systemu.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz wykonać proste migrację do platformy Azure czy platformie odpowiedzi wymagania dotyczące aplikacji, czy nie.

- Chcesz zachować wszystkie warstwy aplikacji obsługiwany w tym samym komputerze wirtualnych pośrodku Azure danych w celu zmniejszenia opóźnienia między warstwy.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

Poniższy diagram przedstawia prostą lokalnego scenariusz i jak można wdrażać jego rozwiązanie chmury włączone w jednym komputerze wirtualnej platformy Azure.

![wzór aplikacji poziomu 1](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Wdrażanie warstwy business (reguł biznesowych i składniki dostępu do danych) w tej samej warstwie fizycznej jako Warstwa prezentacji można zmaksymalizować wydajność aplikacji, chyba że należy użyć osobnych warstwa ze względu na skalowalność lub zabezpieczeń.

Ponieważ jest to często deseń rozpocząć, mogą być przydatne następujący artykuł migracji przenoszenia danych do maszyn wirtualnych usługi SQL Server: [Migrowanie bazy danych programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>warstwy 3 (proste): wiele maszyn wirtualnych

Tego wzorca aplikacji należy wdrożyć aplikację poziomu 3 platformy Azure po umieszczeniu każdej warstwy aplikacji na innym komputerze wirtualnych. Zapewnia elastyczne środowisko do łatwe scenariusze Skala up i skala w nowym oknie. Gdy maszyn wirtualnych zawiera aplikacji sieci web i klienta, drugi obsługuje składniki firm i obsługuje drugi serwer bazy danych.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz przeprowadzanie migracji złożonych bazy danych aplikacji do maszyn wirtualnych Azure.

- Chcesz poziomów innej aplikacji, jakie mają być przechowywane w różnych regionach. Na przykład może udostępnieniu bazy danych, które są rozmieszczane do wielu regionów w raportach.

- Chcesz przenieść aplikacje dla przedsiębiorstw z obsługą lokalnego platform do maszyn wirtualnych Azure. Szczegółowe omówienie w aplikacjach dla przedsiębiorstw zobacz [Co to jest aplikacji dla przedsiębiorstw](https://msdn.microsoft.com/library/aa267045.aspx).

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

Poniższy diagram przedstawia, jak umieścić prostych aplikacji poziomu 3 w Azure po umieszczeniu każdej warstwy aplikacji na innym komputerze wirtualnych.

![wzór aplikacji poziomu 3](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

W tym deseniu aplikacji istnieje tylko jedna maszyna wirtualna (maszyn wirtualnych) w każdej warstwie. Jeśli masz wiele maszyny wirtualne w Azure zaleca się konfigurowanie wirtualnej sieci. [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md) tworzy krawędzi zaufanego i umożliwia maszyny wirtualne do komunikowania się między sobą w prywatny adres IP. Ponadto zawsze upewnij się, że wszystkie połączenia z Internetem tylko przejdź do poziomu prezentacji. Po tym deseniu aplikacji Zarządzanie zasad grupy zabezpieczeń sieci do kontrolowania dostępu. Aby uzyskać więcej informacji zobacz [Zezwalaj na zewnętrznym dostęp do Twojej maszyn wirtualnych za pomocą portalu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Na diagramie protokołów może być TCP, UDP, HTTP lub HTTPS.

>[AZURE.NOTE] Konfigurowanie sieci wirtualnej platformy Azure jest dostępne bezpłatnie. Jednak są naliczane bramy sieci VPN, który łączy do lokalnego. Opłata jest oparty na ilość czasu, połączenie to jest ustanawianie i jest dostępny.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>Warstwa 2 i 3-warstwa, z prezentacji warstwa skali

W tym deseniu aplikacji należy wdrożyć poziomu 2 lub 3 Warstwa bazy danych aplikacji do maszyn wirtualnych Azure po umieszczeniu każdej warstwy aplikacji na innym komputerze wirtualnych. Ponadto możesz skalowania Warstwa prezentacji ze względu na zwiększenie wielkości przychodzących żądań.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz przenieść aplikacje dla przedsiębiorstw z obsługą lokalnego platform do maszyn wirtualnych Azure.

- Chcesz skalowania Warstwa prezentacji ze względu na zwiększenie wielkości przychodzących żądań.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

- Chcesz właścicielem środowisko infrastruktury, które można skalować w górę lub w dół na żądanie.

Poniższy diagram przedstawia, jak możesz umieścić warstwy aplikacji w środowisku maszyn wirtualnych wielu platformy Azure przez Skalowanie zewnętrzne Warstwa prezentacji ze względu na zwiększenie wielkości przychodzących żądań. Jak widać na diagramie, równoważenia obciążenia Azure jest odpowiedzialny za dystrybucję ruchu wiele maszyn wirtualnych, a także określanie, które serwer sieci web, aby nawiązać połączenie. O wielu wystąpień serwerów sieci web za równoważenia obciążenia zapewnia wysoki poziom dostępności Warstwa prezentacji.

![Wzór aplikacji - Skala Warstwa prezentacji się](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Najważniejsze wskazówki dotyczące poziomu 2, 3 warstwy lub n warstwowych wzorców, które mają wiele maszyny wirtualne w jednej warstwy

Zaleca się, że umieszczać maszyn wirtualnych, które należą do tej samej warstwie, w tym samym usługi w chmurze i w tym samym dostępność zestawu. Na przykład umieść zestawu serwerów sieci web w **CloudService1** i **AvailabilitySet1** zestawu serwerów baz danych w **CloudService2** i **AvailabilitySet2**. Dostępność w Azure umożliwia umieścić węzły wysoką dostępność w osobnym błędów domen i uaktualnianie domen.

Aby korzystać z wielu wystąpień maszyn wirtualnych warstwy, musisz skonfigurować równoważenia obciążenia Azure między warstwy aplikacji. Konfigurowanie usługi równoważenia obciążenia w każdej warstwy, należy utworzyć punkt końcowy równoważenia obciążenia na każdej warstwy maszyny wirtualne oddzielnie. Dla określonego poziomu należy najpierw utworzyć maszyny wirtualne w tym samym usługi w chmurze. Dzięki temu mają ten sam adres IP wirtualnego publicznej. Następnie należy utworzyć punkt końcowy jednej maszyn wirtualnych w tej warstwie. Następnie przypisz samego punktu końcowego do pozostałych maszyn wirtualnych w tej warstwie do równoważenia obciążenia. Tworząc zestaw równoważenia obciążenia, dystrybucji ruchu na wielu komputerach wirtualnych, a także umożliwić usługi równoważenia obciążenia określić, które węzeł, aby połączyć w przypadku niepowodzenia węzeł maszyn wirtualnych wewnętrznej bazy danych. Na przykład o wielu wystąpień serwerów sieci web za równoważenia obciążenia zapewnia wysoki poziom dostępności poziomu prezentacji.

Zgodnie z zaleceniami dotyczącymi zawsze upewnij się, że wszystkie połączenia z Internetem najpierw należy przejść do poziomu prezentacji. Warstwy prezentację uzyskuje dostęp do poziomu firm, a następnie warstwie firm uzyskuje dostęp do warstwy danych. Aby uzyskać więcej informacji na temat Zezwalaj na dostęp do warstwy prezentacji zobacz [Zezwalaj na zewnętrznym dostęp do Twojej maszyn wirtualnych za pomocą portalu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Należy zauważyć, że usługi równoważenia obciążenia platformy Azure działa podobnie do ładowania urządzenia do równoważenia w środowisku lokalnym. Aby uzyskać więcej informacji zobacz [równoważenia obciążenia dla usług Azure infrastruktury](virtual-machines-windows-load-balance.md).

Ponadto zaleca się, że możesz skonfigurować sieć prywatną na potrzeby maszyn wirtualnych przy użyciu Azure wirtualnej sieci. Pozwala na komunikowania się między sobą za pośrednictwem prywatny adres IP. Aby uzyskać więcej informacji zobacz [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>Warstwa 2 i 3-warstwa, z firm warstwa skali

W tym deseniu aplikacji należy wdrożyć poziomu 2 lub 3 Warstwa bazy danych aplikacji do maszyn wirtualnych Azure po umieszczeniu każdej warstwy aplikacji na innym komputerze wirtualnych. Ponadto można rozpowszechnianie składników serwera aplikacji do wielu maszyn wirtualnych ze względu na złożoność aplikacji.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz przenieść aplikacje dla przedsiębiorstw z obsługą lokalnego platform do maszyn wirtualnych Azure.

- Chcesz rozpowszechniać składników serwera aplikacji do wielu maszyn wirtualnych ze względu na złożoność aplikacji.

- Chcesz przenieść ciężkich logiczny firm aplikacji FIRMOWYCH (linia business) do maszyn wirtualnych Azure lokalnego. Aplikacji FIRMOWYCH to zbiór aplikacje komputerowe krytycznych, które są niezbędne do uruchamiania przedsiębiorstwa, takich jak księgowe, kadry (h), płac, zarządzaniu łańcuch dostaw i aplikacje do planowania zasobów.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

- Chcesz właścicielem środowisko infrastruktury, które można skalować w górę lub w dół na żądanie.

Poniższy diagram ilustruje scenariusz lokalnego i jego rozwiązanie chmury włączone. W tym scenariuszu należy zaznaczyć warstwy aplikacji w środowisku maszyn wirtualnych wielu platformy Azure przez Skalowanie zewnętrzne warstwy firm zawiera warstwa logiki biznesowej i składniki dostępu do danych. Jak widać na diagramie, równoważenia obciążenia Azure jest odpowiedzialny za dystrybucję ruchu wiele maszyn wirtualnych, a także określanie, które serwer sieci web, aby nawiązać połączenie. O wielu wystąpień serwerów aplikacji za równoważenia obciążenia zapewnia wysoki poziom dostępności warstwie firm. Aby uzyskać więcej informacji zobacz [Najważniejsze wskazówki dotyczące poziomu 2, 3 warstwy lub desenie n warstwowych aplikacji, które mają wiele maszyn wirtualnych w jednej warstwie](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Wzór aplikacji ze skalą warstwa firm się](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>Warstwa 2 i 3-warstwa, z prezentacji i firm poziomów skali w nowym oknie oraz HADR

Wzór tej aplikacji należy wdrożyć aplikację poziomu 2 lub 3 Warstwa bazy danych do maszyn wirtualnych Azure przekazując Warstwa prezentacji (serwer sieci web) i składniki firm warstwy (serwer aplikacji) do wielu maszyn wirtualnych. Ponadto wdrożenie rozwiązania odzyskiwania wysokiej dostępności i awarii baz danych w środowisku maszyn wirtualnych Azure.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz przenieść aplikacji przedsiębiorstwa z lokalne wirtualne platformy Azure z zastosowaniem programu SQL Server wysoką dostępność i zastosowanie funkcji.

- Chcesz skalować Warstwa prezentacji i warstwie firm ze względu na zwiększenie wielkości przychodzących żądań i złożoność aplikacji.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

- Chcesz właścicielem środowisko infrastruktury, które można skalować w górę lub w dół na żądanie.

Poniższy diagram ilustruje scenariusz lokalnego i jego rozwiązanie chmury włączone. W tym scenariuszu skalowanie Warstwa prezentacji i składniki warstwy biznesowych w wielu maszyn wirtualnych w Azure. Ponadto można zaimplementować wysokiej dostępności i awarii technik odzyskiwania (HADR) dla baz danych programu SQL Server Azure.

Działa wiele kopii aplikacji w różnych maszyny wirtualne upewnij się, czy jesteś równoważenia żądania w notatkach. Jeśli masz wiele maszyn wirtualnych, należy upewnić się, że wszystkie maszyny wirtualne są dostępne i uruchomionego w jednym punkcie w czasie. Jeśli skonfigurujesz równoważenia obciążenia, usługi równoważenia obciążenia Azure umożliwia śledzenie kondycji maszyny wirtualne i kieruje połączeń przychodzących do prawidłowy węzłów maszyn wirtualnych działa prawidłowo. Aby uzyskać informacje dotyczące konfigurowania równoważenia obciążenia maszyn wirtualnych zobacz [równoważenia obciążenia dla usługi Azure infrastruktury](virtual-machines-windows-load-balance.md). O wielu wystąpień serwerów sieci web i aplikacji za równoważenia obciążenia zapewnia wysokiej dostępności poziomów prezentacji i małych firm.

![Poza skalowanie i wysoka dostępność](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Najważniejsze wskazówki dotyczące wzorców aplikacji wymagających SQL HADR

Po skonfigurowaniu programu SQL Server wysoką dostępność i rozwiązań odzyskiwania po awarii w maszyn wirtualnych Azure konfigurowania wirtualną sieć na potrzeby maszyn wirtualnych przy użyciu [Azure wirtualną sieć](../virtual-network/virtual-networks-overview.md) jest wymagane.  Maszyn wirtualnych sieci wirtualnej będą mieć stałe prywatny adres IP nawet po przestojów, więc można uniknąć czas aktualizacji rozpoznawania nazw DNS. Ponadto wirtualną sieć umożliwia rozszerzenie sieci lokalnej Azure i tworzy granicę zaufanego. Na przykład jeśli aplikacja ma ograniczenia domeny firmy (na przykład, uwierzytelnianie systemu Windows, usługi Active Directory), skonfigurowanie [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md) jest to konieczne.

Większość klientów, którzy korzystają z kodu produkcji Azure, są pamiętając repliki zarówno głównego i pomocniczego Azure.

Szczegółowe informacje i materiały szkoleniowe dotyczące wysokiej dostępności i technik odzyskiwania danych zobacz [wysoką dostępność i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>warstwy 2 i 3-warstwa przy użyciu maszyny wirtualne Azure i usług w chmurze

W tym deseniu aplikacji wdrażania aplikacji poziomu 2 lub 3 warstwa Azure za pomocą obu [Usług w chmurze Azure](../cloud-services/cloud-services-choose-me.md#tellmecs) (sieci web i pracownik role - platformy usług (PaaS)) i [maszyn wirtualnych Azure](virtual-machines-windows-about.md) (infrastruktury jako usługa (IaaS)). Korzystanie z [Usług w chmurze Azure](https://azure.microsoft.com/documentation/services/cloud-services/) warstwa biznesowym i warstwy prezentacji i program SQL Server w [środowisku maszyn wirtualnych systemu Azure](virtual-machines-windows-about.md) dla warstwy danych jest przydatne w przypadku większości aplikacje na Azure. Przyczyną tego jest to, że masz w przypadku wystąpienia obliczeń uruchomionych usług w chmurze udostępnia łatwe zarządzanie, wdrażania, monitorowania i skala w nowym oknie.

Z usługami w chmurze Azure przechowuje infrastruktury dla Ciebie, wykonuje konserwacja poprawki systemy operacyjne i pozwala podjąć próbę odzyskania z błędów usługi i sprzętu. Gdy aplikacja wymaga skala w nowym oknie, automatyczne i ręczne poza skalowanie dostępnych opcji projektu usługi cloud poprzez zwiększenie lub zmniejszenie liczby wystąpień lub maszyn wirtualnych, które są używane przez aplikację. Ponadto lokalnego programu Visual Studio umożliwia wdrażanie aplikacji do projektu usługi cloud platformy Azure.

Podsumowując Jeśli nie chcesz, aby właścicielem rozległa poziomu zadania administracyjne dla prezentacji i firm i aplikacja nie wymaga złożonej konfiguracji oprogramowania lub system operacyjny, za pomocą usług w chmurze Azure. Jeśli baza danych SQL Azure nie obsługuje wszystkich funkcji, którego szukasz, użyj programu SQL Server w maszyn wirtualnych Azure dla warstwy danych. Uruchamianie aplikacji na usług w chmurze Azure i przechowywania danych w maszyn wirtualnych Azure łączy zalety obiema tymi usługami. Szczegółowe porównanie zobacz sekcję w tym temacie [Opis](#comparing-web-development-strategies-in-azure)strategii rozwoju platformy Azure.

W tym deseniu aplikacji Warstwa prezentacji zawiera roli sieci web, która działa składnik usług w chmurze w środowisku wykonanie Azure i obsługiwana przez usługi IIS i ASP.NET jest dostosowana programowania aplikacji sieci web. Warstwa firmy lub wewnętrznej bazy danych zawiera roli Pracownik, która działa składnik usług w chmurze w środowisku wykonanie Azure i są przydatne w przypadku uogólniony rozwoju i może wykonywać przetwarzanie w tle dla ról w sieci web. Warstwa bazy danych znajduje się w maszyny wirtualnej programu SQL Server Azure. Komunikacji między Warstwa prezentacji i Warstwa bazy danych ma miejsce bezpośrednio lub za pośrednictwem warstwie business — pracownik roli składniki.

Ten wzorzec aplikacji jest przydatny, gdy:

- Chcesz przenieść aplikacji przedsiębiorstwa z lokalne wirtualne platformy Azure z zastosowaniem programu SQL Server wysoką dostępność i zastosowanie funkcji.

- Chcesz właścicielem środowisko infrastruktury, które można skalować w górę lub w dół na żądanie.

- Baza danych SQL Azure nie obsługuje wszystkich funkcji, których potrzebuje aplikacji lub bazie danych.

- Chcesz wykonać obciążenia sprawdzanie występowania różnych poziomów obciążenie pracą, ale w tym samym czasie nie chcesz własności i obsługa wielu fizycznych komputerów cały czas.

Poniższy diagram ilustruje scenariusz lokalnego i jego rozwiązanie chmury włączone. W tym scenariuszu należy zaznaczyć Warstwa prezentacji w sieci web ról, warstwie firm ról pracownika, ale warstwy danych w środowisku maszyn wirtualnych w Azure. Uruchomiony wiele kopii Warstwa prezentacji w sieci web różne role zapewnia, aby załadować żądania salda w notatkach. Połączenie usługi Cloud Azure z maszyn wirtualnych Azure zaleca się również skonfigurować pocztę [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md) . [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md)można połączyć stabilny i trwałych prywatnych adresów IP w tej samej usługi w chmurze w chmurze. Po zdefiniowaniu wirtualną sieć dla maszyn wirtualnych i usług w chmurze, można uruchomić, komunikowania się między sobą przez prywatny adres IP. Ponadto o maszyn wirtualnych i ról Azure web/pracownika w tej samej [Azure wirtualną sieć](../virtual-network/virtual-networks-overview.md) zawiera krótki czas oczekiwania i zabezpieczyć łączności. Aby uzyskać więcej informacji zobacz [Co to jest usługa w chmurze](../cloud-services/cloud-services-choose-me.md).

Jak pokazano na diagramie, równoważenia obciążenia Azure rozdziela ruchu w wielu maszyn wirtualnych i określa również serwer sieci web lub serwera aplikacji, aby nawiązać połączenie. O wielu wystąpień serwerów sieci web i aplikacji za równoważenia obciążenia zapewnia wysoki poziom dostępności Warstwa prezentacji i warstwa firm. Aby uzyskać więcej informacji zobacz [Najważniejsze wskazówki dotyczące wzorców aplikacji wymagających SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Desenie aplikacji z usługami w chmurze](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Innym sposobem zaimplementowania deseniem tej aplikacji jest użycie rolę skonsolidowane sieci web, która zawiera zarówno Warstwa prezentacji i składniki firm warstwy, tak jak pokazano na poniższym diagramie. Ten wzorzec aplikacji jest przydatne w przypadku aplikacji, które wymagają stanowe projektu. Ponieważ Azure udostępnia węzły stateless obliczeń na rolach sieci web i Pracownik, zaleca się stosowanie logiki do przechowywania stanu sesji przy użyciu jednej z następujących technologii: [Azure pamięci podręcznej](https://azure.microsoft.com/documentation/services/redis-cache/), [Magazyn tabel platformy Azure](../storage/storage-dotnet-how-to-use-tables.md) lub [Bazy danych SQL Azure](../sql-database/sql-database-technical-overview.md).

![Desenie aplikacji z usługami w chmurze](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Deseń z Azure maszyny wirtualne, baza danych SQL Azure i Azure aplikacji usługi (aplikacje sieci Web)

Podstawowym celem tego wzorca aplikacji jest pokazano, jak połączyć Azure infrastruktury jako składniki usługi (IaaS) z Azure składniki platformy jako usługi (PaaS) w rozwiązaniu. Ten wzorzec jest ograniczony do bazy danych SQL Azure do przechowywania danych relacyjnych. Nie zawiera programu SQL Server w Azure maszyny wirtualnej, która stanowi część Azure infrastruktury jako oferująca usługi.

W ten sposób aplikacji należy wdrożyć aplikacji bazy danych Azure, umieszczając poziomów prezentacji i małych firm w tym samym komputerze wirtualnych i dostęp do bazy danych na serwerach Azure SQL Database (baza danych SQL). Warstwa prezentacji można zaimplementować przy użyciu rozwiązania tradycyjnych sieci web usług IIS. Lub scalonej prezentacji i warstwa firm można zaimplementować przy użyciu [Aplikacji sieci Web Azure](https://azure.microsoft.com/documentation/services/app-service/web/).

Ten wzorzec aplikacji jest przydatny, gdy:

- Masz już istniejącej bazy danych programu SQL server jest skonfigurowany w Azure i chcesz szybko przetestować aplikację.

- Chcesz przetestować możliwości Azure środowiska.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Składniki firm logikę i dane programu access mogą być niezależne w obrębie aplikacji sieci web.

Poniższy diagram ilustruje scenariusz lokalnego i jego rozwiązanie chmury włączone. W tym scenariuszu należy zaznaczyć warstwy aplikacji w jednym komputerze wirtualnych Azure i dostępu do danych w bazie danych SQL Azure.

![Wzór mieszanych aplikacji](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Jeśli wybierzesz zaimplementować Scalonej sieci web i warstwa aplikacji przy użyciu aplikacji sieci Web Azure, zaleca się zachować warstwie aplikacji lub warstwy środkowej jako biblioteki dołączanej dynamicznie (dll) w kontekście aplikacji sieci web.

Ponadto Przejrzyj zalecenia podane w sekcji [Opis web opracowywania strategii platformy Azure](#comparing-web-development-strategies-in-azure) na końcu tego artykułu, aby dowiedzieć się więcej na temat programowania technik.

## <a name="n-tier-hybrid-application-pattern"></a>Hybrydowe N warstwowych aplikacji deseniu

Wzór aplikacji hybrydowych n warstwowych implementacji aplikacji w wielu poziomów rozdzielone lokalnego i Azure. Dlatego utworzyć systemu hybrydowych elastycznych i przeznaczone do wielokrotnego użytku, które można modyfikować lub dodawać określonego poziomu bez zmieniania innych warstw. Aby rozszerzyć tej samej sieci firmowej w chmurze, możesz za pomocą usługi [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md) .

Ten wzorzec aplikacji hybrydowego jest przydatny, gdy:

- Chcesz tworzyć aplikacje, które Uruchom częściowo w chmurze i częściowo lokalnego.

- Chcesz przeprowadzić migrację niektórych lub wszystkich elementów istniejącej aplikacji lokalnej w chmurze.

- Chcesz przenieść aplikacji przedsiębiorstwa z lokalnym z obsługą platformy Azure.

- Chcesz właścicielem środowisko infrastruktury, które można skalować w górę lub w dół na żądanie.

- Chcesz szybko obsługi administracyjnej rozwoju i przetestować środowiskach przez krótki czas.

- Chcesz kosztów skuteczny sposób podjęcie kopie zapasowe aplikacji dla przedsiębiorstw bazy danych.

Poniższy diagram przedstawia wzorzec aplikacji hybrydowych n warstwowych obejmuje lokalnego i Azure. Jak pokazano na diagramie, infrastruktura lokalnego zawiera [Usług domenowych Active Directory](https://technet.microsoft.com/library/hh831484.aspx) kontrolera domeny do obsługi uwierzytelniania użytkowników i autoryzacji. Należy zauważyć, że diagram przedstawia scenariusza, w której niektórych części warstwy danych są przechowywane w centrum danych lokalnych należy niektórych części warstwy danych są przechowywane w Azure. W zależności od potrzeb aplikacji można zaimplementować kilka scenariuszy hybrydowego. Na przykład może przechowywać Warstwa prezentacji i warstwa firm w środowisku lokalnym, ale warstwy danych platformy Azure.

![Wzór N warstwowych aplikacji](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

W Azure można użyć usługi Active Directory jako autonomicznego katalogu chmury dla Twojej organizacji lub istniejącej usługi Active Directory w lokalnej również można zintegrować z [Usługą Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Jak pokazano na diagramie, składniki warstwy firm mogą uzyskiwać dostęp do wielu źródeł danych, takich jak do [Serwera SQL Azure](virtual-machines-windows-sql-server-iaas-overview.md) za pośrednictwem prywatne wewnętrzny adres IP, do lokalnego programu SQL Server za pośrednictwem [Wirtualnych sieci Azure](../virtual-network/virtual-networks-overview.md)lub z [Bazą danych SQL](../sql-database/sql-database-technical-overview.md) przy użyciu technologii dostawcy danych .NET Framework. W tym diagramie bazy danych SQL Azure to usługa miejsca do magazynowania danych opcjonalnych.

Wzór aplikacji n warstwowych hybrydowe można zaimplementować następujące przepływu pracy w określonej kolejności:

1. Identyfikowanie enterprise bazy danych aplikacji, która musi mają zostać przeniesione w górę do przy użyciu [zestawu narzędzi planowania (tablica) oceny firmy Microsoft i](http://microsoft.com/map)w chmurze. Zestaw narzędzi Mapa zbiera dane zapasami i wydajności z komputerów, który zamierzasz wirtualizacji i zawiera wskazówki dotyczące zdolności i ocena planowania.

1. Planowanie zasobów i konfiguracji potrzebne do platformy Azure, takich jak konta miejsca do magazynowania i maszyn wirtualnych.

1. Konfigurowanie łączność sieciowa między sieci firmowej lokalnego i [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md). Aby skonfigurować połączenie między sieci firmowej lokalnego i maszyny wirtualnej Azure, użyj jednej z następujących dwóch metod:

    1. Nawiązują połączenie między wdrożeniem lokalnym a Azure za pomocą publicznej punkty końcowe maszyny wirtualnej platformy Azure. Ta metoda zawiera Łatwa konfiguracja i umożliwia uwierzytelnianie programu SQL Server na tym komputerze wirtualnych. Ponadto skonfiguruj reguł grupy zabezpieczeń sieci do sterowania ruchu publicznego do maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [Zezwalaj na zewnętrznym dostęp do Twojej maszyn wirtualnych za pomocą portalu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Nawiąż połączenie między wdrożeniem lokalnym a Azure za pośrednictwem Azure wirtualnych prywatne tunelem sieci (VPN). Ta metoda pozwala na rozszerzenie zasady domeny maszyn wirtualnych w Azure. Ponadto można skonfigurować reguły zapory i uwierzytelniania systemu Windows na tym komputerze wirtualnych. Obecnie Azure obsługuje bezpieczne połączenie sieci VPN witryny do witryny i połączenia VPN punktu do witryny:

        - Z bezpiecznego połączenia witryny do witryny możesz ustanowić połączenie sieciowe między sieci lokalnej i wirtualna sieć platformy Azure. Zalecane jest Azure nawiązywania połączenia z lokalnym środowiskiem centrum danych.

        - Z bezpiecznego połączenia punkt do witryny możesz ustanowić połączenie sieciowe między wirtualnej sieci platformy Azure i poszczególne komputery z dowolnego miejsca. Zaleca się głównie na potrzeby projektowania i testowania.

    Aby uzyskać informacje na temat nawiązywania połączenia z programu SQL Server w Azure zobacz [Łączenie z programu SQL Server maszyny wirtualnej Azure](virtual-machines-windows-classic-sql-connect.md).

1. Skonfiguruj zaplanowanych zadań i alertów, których kopię zapasową danych lokalnych w dyskiem maszyn wirtualnych w Azure. Aby uzyskać więcej informacji zobacz [SQL Server wykonywanie kopii zapasowych i przywracanie z usługi Magazyn obiektów Blob platformy Azure](https://msdn.microsoft.com/library/jj919148.aspx) i [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md).

1. W zależności od potrzeb aplikacji można zaimplementować jedną z następujących trzech typowe scenariusze:

    1. Możesz zachować serwer sieci web, serwera aplikacji i danych wielkość liter na serwerze bazy danych platformy Azure przechowywać poufne dane lokalne.

    1. Możesz zachować serwer sieci web i aplikacji lokalnego serwera należy serwer bazy danych w maszyny wirtualnej Azure.

    1. Możesz zachować swój serwer bazy danych, serwer sieci web i aplikacji lokalnego serwera przechowywać repliki bazy danych w środowisku maszyn wirtualnych w Azure. To ustawienie umożliwia lokalne serwery sieci web i aplikacjach do raportowania dostępu repliki bazy danych platformy Azure. W związku z tym można uzyskać, aby zmniejszyć obciążenie pracą w bazie danych lokalnych. Firma Microsoft zaleca, aby zaimplementować w tym scenariuszu dla ciężkich odczytywanie obciążenia i rozwoju celów. Aby uzyskać informacje na temat tworzenia repliki bazy danych platformy Azure Zobacz grupy dostępności (AlwaysOn) w [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Porównanie strategii rozwoju sieci web platformy Azure

Aby zaimplementowania i wdrożenia wielu warstwy aplikacji oparte na serwerze SQL Azure, można użyć jednej z następujących dwóch metod programowania:

- Konfigurowanie serwera sieci web tradycyjnych (IIS - Internet Information Services) w Azure i dostępu do bazy danych programu SQL Server w maszyn wirtualnych Azure.

- Wdrożenie i wdrażanie usługi w chmurze Azure. Następnie należy upewnić się, że usługa w chmurze można uzyskać dostęp do baz danych programu SQL Server w środowisku maszyn wirtualnych Azure. Usługa w chmurze może zawierać wiele ról w sieci web i pracowników.

Poniższa tabela zawiera porównanie programowania tradycyjnych sieci web z usługami w chmurze Azure i aplikacje sieci Web Azure w odniesieniu do programu SQL Server w maszyn wirtualnych Azure. Tabela zawiera aplikacje sieci Web Azure, jak można używać programu SQL Server w maszyn wirtualnych Azure jako źródła danych dla aplikacji sieci Web Azure za pośrednictwem jego publicznej wirtualny adres IP lub nazwę DNS.

||Opracowywanie tradycyjnych sieci web w maszyn wirtualnych Azure|Usług w chmurze platformy Azure|Hostingu z aplikacjami Azure Web|
|---|---|---|---|
|**Migracja aplikacji ze źródeł lokalnych**|Istniejące aplikacje jako-jest.|Aplikacje muszą ról w sieci web i pracownika.|Istniejące aplikacje jako — ale nadaje się do samodzielne aplikacji i usług sieci web, wymagających szybkiego skalowalność.|
|**Opracowanie i wdrożenie**|Programu Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, Menedżera IIS, programu PowerShell.|Programu Visual Studio SDK Azure, TFS, programu PowerShell. Każdy usługi w chmurze występują dwa środowiska, do których można wdrażać z pakietu usługi i konfiguracji: roboczych i produkcyjnych. Środowiska wzorcowego ją przetestować przed przenosząc je produkcji można wdrożyć usługi w chmurze.|Programu Visual Studio, WebMatrix, deweloper wizualne sieci Web, FTP, CYFRA, BitBucket, witrynie CodePlex, DropBox, GitHub, Mercurial, TFS, sieci Web wdrożyć, programu PowerShell.|
|**Administracja i konfigurowanie**|Odpowiadają dla zadań administracyjnych w aplikacji, dane, reguły zapory, wirtualną sieć i system operacyjny.|Odpowiadają dla zadań administracyjnych w aplikacji, dane, reguły zapory i wirtualnej sieci.|Odpowiadają dla zadań administracyjnych w aplikacji, a tylko dane.|
|**Wysoką dostępność i odzyskiwanie (HADR)**|Zaleca się umieszczenie maszyn wirtualnych w ten sam zestaw dostępność i w tym samym usługi w chmurze. Zachowanie pośrednictwem usługi SMS, w tym samym dostępność zestawu umożliwia Azure, aby umieścić węzły wysoką dostępność w osobnym błędów domen i uaktualnianie domen. Podobnie pamiętając pośrednictwem usługi SMS samej usługi w chmurze umożliwia równoważenia obciążenia i maszyny wirtualne można komunikować się bezpośrednio ze sobą sieci lokalnej w centrum danych Azure.<br/><br/>Odpowiadają stosowania wysoką dostępność i rozwiązanie odzyskiwania danych dla programu SQL Server w maszyn wirtualnych Azure unikać dowolnego przestoje. Dla obsługiwanych technologiach HADR zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Jesteś odpowiedzialny za wykonywanie kopii zapasowej własnych danych i aplikacji.<br/><br/>Azure można przenieść maszyn wirtualnych, jeśli na komputerze obsługującym w centrum danych kończy się niepowodzeniem z powodu problemów ze sprzętem. Ponadto może istnieć planowane przestoje usługi maszyn wirtualnych podczas aktualizacji zabezpieczeń lub oprogramowania na komputerze obsługującym aktualizacje. Dlatego zaleca się zachować co najmniej dwa pośrednictwem SMS w każdej warstwie aplikacji do zapewnienia dostępności ciągły. Azure nie umożliwia Umowa dotycząca poziomu usług dla jednego komputera wirtualnych. Aby uzyskać więcej informacji zobacz [wskazówki techniczne elastyczność Azure](../resiliency/resiliency-technical-guidance.md).|Azure zarządza błędy wynikające z podstawowej oprogramowania sprzętu lub systemu operacyjnego. Firma Microsoft zaleca stosowanie wielu wystąpień roli zapewniające wysoki poziom dostępności aplikacji sieci web lub pracownika. Aby uzyskać informacje zobacz [usług w chmurze, maszyn wirtualnych i umowy o poziomie usług wirtualnej sieci](http://www.microsoft.com/download/details.aspx?id=38427) i [Odzyskiwanie i wysoką dostępność dla aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Jesteś odpowiedzialny za wykonywanie kopii zapasowej własnych danych i aplikacji.<br/><br/>Baz danych znajdujących się w bazie danych programu SQL Server w maszyn wirtualnych Azure odpowiadasz stosowania wysoka rozwiązania odzyskiwania dostępności i danych w celu uniknięcia dowolnego przestoje. Dla obsługiwanych technologiach HDAR Zobacz wysokiej dostępności i odzyskiwanie dla programu SQL Server w maszyn wirtualnych Azure.<br/><br/>**Odzwierciedlające bazy danych serwera SQL**: Korzystanie z usług w chmurze Azure (web pracownik role). Maszyny wirtualne programu SQL Server i projektu usługi cloud mogą być w tej samej sieci wirtualnej Azure. Jeśli maszyn wirtualnych programu SQL Server nie znajduje się w tej samej sieci wirtualnej, należy utworzyć Alias serwera SQL, aby skierować komunikacji do wystąpienia programu SQL Server. Ponadto aliasu musi odpowiadać nazwie programu SQL Server.|Wysoce pochodzi z role Azure pracownik, magazyn obiektów blob platformy Azure i bazy danych SQL Azure. Na przykład magazyn Azure obsługuje trzy repliki wszystkich obiektów blob, tabele i dane kolejki. W dowolnym momencie, bazy danych SQL Azure zachowuje trzy repliki danych uruchomiony — jednej replice podstawowego i dwie repliki pomocniczą. Aby uzyskać więcej informacji zobacz [Magazyn Azure](https://azure.microsoft.com/documentation/services/storage/) i [Bazy danych SQL Azure](../sql-database/sql-database-technical-overview.md).<br/><br/>Jeśli używasz programu SQL Server w maszyn wirtualnych Azure jako źródła danych dla aplikacji sieci Web Azure, należy pamiętać o tym, Azure aplikacje sieci Web nie obsługuje Azure wirtualną sieć. Innymi słowy wszystkich połączeń z aplikacji sieci Web Azure maszyny wirtualne serwera SQL platformy Azure musi przejść publicznej punkty końcowe maszyn wirtualnych. Może to spowodować pewne ograniczenia wysokiej dostępności i scenariuszy odzyskiwania po awarii. Na przykład z aplikacją kliencką w aplikacjach sieci Web Azure nawiązywanie maszyn wirtualnych programu SQL Server przy użyciu odzwierciedlające bazy danych może okazać się niemożliwe nawiązywania połączenia z nowym serwerem podstawowym odzwierciedlające bazy danych wymaga Konfigurowanie Azure wirtualną sieć między maszyny wirtualne hosta programu SQL Server w Azure. W związku z tym aplikacje sieci Web Azure za pomocą **Odzwierciedlające programu SQL Server bazy danych** nie jest obsługiwane obecnie.<br/><br/>**Grupy dostępności (AlwaysOn) serwera SQL**: możesz skonfigurować grupy dostępności (AlwaysOn) podczas korzystania z aplikacji sieci Web Azure z maszyny wirtualne serwera SQL platformy Azure. Ale musisz skonfigurować odbiornika grupy dostępności (AlwaysOn) do kierowania komunikacji podstawowego replice za pośrednictwem publicznego równoważenia obciążenia punktów końcowych.|
|**Łączność między lokalna**|Wirtualna sieć Azure umożliwia łączenie do lokalnego.|Wirtualna sieć Azure umożliwia łączenie do lokalnego.|Azure wirtualnej sieci jest obsługiwana. Aby uzyskać więcej informacji zobacz [Integracji wirtualnej sieci aplikacji sieci Web](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Skalowalność**|Strzałka w górę skala jest dostępna przez zwiększenie rozmiarów maszyn wirtualnych lub dodawania dysków. Aby uzyskać więcej informacji na temat rozmiarów maszyn wirtualnych zobacz [Maszyn wirtualnych rozmiarów Azure](virtual-machines-windows-sizes.md).<br/><br/>**Dla serwera bazy danych**: w nowym oknie Skala jest dostępna za pośrednictwem technik podziału bazy danych i grupy dostępności (AlwaysOn) programu SQL Server.<br/><br/>Dla odczytu złożonych zadań można używać [Grup dostępności (AlwaysOn)](https://msdn.microsoft.com/library/hh510230.aspx) na wiele węzłów pomocniczej, a także replikacji SQL Server.<br/><br/>Poziomy podziału danych obciążeń pracą intensywnie zapisu, można zaimplementować na wielu serwerach fizycznej zapewniające aplikacji skala w nowym oknie.<br/><br/>Ponadto można zaimplementować poza skalowanie przy użyciu [Programu SQL Server zależne routingu danych](https://technet.microsoft.com/library/cc966448.aspx). Przy użyciu danych zależne routingu (DDR), należy zaimplementować mechanizmu podziału w kliencie, zwykle w warstwie warstwa firm do kierowania żądań bazy danych do wielu węzłów programu SQL Server. Warstwa firm zawiera mapowania jak partycją danych i który węzeł zawiera dane.<br/><br/>Można skalować aplikacje, które są uruchomione w środowisku maszyn wirtualnych systemu. Aby uzyskać więcej informacji zobacz [jak skalowanie aplikacji](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Uwaga**: funkcja **AutoScale** platformy Azure umożliwia automatycznie zwiększyć lub zmniejszyć maszyn wirtualnych, które są używane przez aplikację. Ta funkcja gwarantuje, że przez użytkownika końcowego nie jest negatywny wpływ okresach Alokacja maksymalna, i maszyny wirtualne są zamknięte, gdy brakuje żądanie. Zaleca się, że nie ustawiono opcję AutoScale dla usługi w chmurze, jeśli zawiera maszyny wirtualne programu SQL Server. Przyczyną tego jest to, że funkcja AutoScale umożliwia Azure wyłączyć na komputerze wirtualnych, gdy użycie Procesora, w tym maszyn wirtualnych jest wyższy niż niektórych próg, a także wyłączyć maszyny wirtualnej, gdy użycie Procesora niższej niż go. Funkcja AutoScale jest przydatne w przypadku aplikacji bezstanowych, takich jak serwery sieci web, w której wszelkie maszyn wirtualnych można zarządzać obciążenie pracą bez żadnych odwołań do dowolnego poprzedniego stanu. Funkcja AutoScale jest przydatne w przypadku stanowe aplikacjach, takich jak SQL Server, gdy tylko jedno wystąpienie umożliwia zapisywanie do bazy danych.|Strzałka w górę skala jest dostępna przy użyciu wielu ról w sieci web i pracowników. Aby uzyskać więcej informacji na temat rozmiarów maszyn wirtualnych dla ról w sieci web i pracownik zobacz [Konfigurowanie rozmiarów usług w chmurze](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Podczas korzystania z **Usług w chmurze**, można zdefiniować wiele ról do rozpowszechniania przetwarzania i również osiągnięcia elastyczność skalowania aplikacji. Każda usługa w chmurze zawiera role w sieci web lub role pracownika, każda z plików aplikacji i konfiguracji. Możesz Skala up usługi w chmurze, zwiększając liczbę wystąpień roli (maszyn wirtualnych) używany dla roli i skali szczegółów usługi w chmurze zmniejszając liczbę wystąpień roli. Aby uzyskać szczegółowe informacje zobacz [Azure wykonanie modeli](../cloud-services/cloud-services-choose-me.md).<br/><br/>Poza skalowanie jest dostępne za pośrednictwem obsługi wbudowanych Azure wysokiej dostępności za pomocą [usług w chmurze, maszyn wirtualnych umowy o poziomie usług wirtualnej sieci](http://www.microsoft.com/download/details.aspx?id=38427) i równoważenia obciążenia.<br/><br/>W przypadku aplikacji wielu zalecamy nawiązywanie połączenia sieci web i pracownik role aplikacji do bazy danych serwera maszyny wirtualne za pośrednictwem wirtualnych sieci Azure. Ponadto Azure udostępnia równoważenia obciążenia dla maszyny wirtualne w tym samym usługi w chmurze, rozłożenie ich żądania użytkowników. Maszyn wirtualnych połączonych w ten sposób można komunikować się bezpośrednio ze sobą sieci lokalnej w centrum danych Azure.<br/><br/>Możesz skonfigurować **AutoScale** na portalu klasyczny Azure, jak i godziny harmonogramu. Aby uzyskać więcej informacji zobacz [jak skalowanie aplikacji](../cloud-services/cloud-services-how-to-scale.md).|**Skalowanie w górę i w dół**: użytkownik może zwiększenie/zmniejszenie rozmiaru wystąpienia (maszyn wirtualnych) zarezerwowane dla witryny sieci web.<br/><br/>Skala się: możesz dodać więcej zastrzeżone wystąpienia (pośrednictwem SMS) dla witryny sieci web.<br/><br/>Możesz skonfigurować **AutoScale** w portalu, jak i godziny harmonogramu. Aby uzyskać więcej informacji zobacz [jak skala aplikacji sieci Web](../app-service-web/web-sites-scale.md).|

Aby uzyskać więcej informacji o wybierając jedną z następujących metod programowania, zobacz [Azure aplikacji sieci Web, usługami w chmurze i maszyny wirtualne: kiedy należy używać](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat uruchamiania programu SQL Server w środowisku maszyn wirtualnych Azure zobacz [Programu SQL Server na podglądzie maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
