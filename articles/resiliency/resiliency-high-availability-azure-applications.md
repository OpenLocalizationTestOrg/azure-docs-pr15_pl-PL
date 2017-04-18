<properties
   pageTitle="Wysoka dostępność dla aplikacji Azure | Microsoft Azure"
   description="Omówienie kwestii technicznych i szczegółowe informacje dotyczące projektowania i tworzenia aplikacji wysokiej dostępności w programie Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Wysoka dostępność dla aplikacji utworzonych na Microsoft Azure

Wysokiej dostępności aplikacji pochłania zmianami dostępności, załaduj i tymczasowe błędy w usługach zależne i sprzętu. Aplikacja będzie nadal działać, poziomie dopuszczalne użytkownika i odpowiedź systemowych, zdefiniowane przez wymagań lub umów poziom obsługi aplikacji (poziomu).

##<a name="azure-high-availability-features"></a>Funkcje dostępności Azure

Azure oferuje wiele funkcji wbudowanych platformy, obsługujące aplikacje o wysokiej dostępności. W tej sekcji opisano niektóre z tych najważniejszych funkcji. Aby uzyskać bardziej szczegółowe analizy platformę zobacz [wskazówki techniczne elastyczność Azure](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Kontroler tkaninie

Kontroler Azure tkaninie przepisy i monitoruje stan wystąpienia Azure obliczeń. Kontroler tkaninie sprawdza stan sprzętu i oprogramowania wystąpień komputera hosta i gościa. Gdy wykrywa błąd wymusza zwiększany przez automatyczne przenoszenie wystąpienia maszyn wirtualnych. Koncepcja dodatkowo błędów i uaktualnianie domen obsługuje SLA obliczeń.

Podczas rozmieszczania wielu wystąpień rolę usługi w chmurze Azure wdraża te wystąpienia błędów innej domeny. Obramowanie domeny błędów zasadniczo jest stojaków innego sprzętu, w tym samym regionie. Błąd domen zmniejszyć prawdopodobieństwo, że błąd sprzętowy zlokalizowanym zakłóci usługi aplikacji. Nie można zarządzać liczby domen błędów, przydzielonych do pracowników lub role w sieci web. Kontroler tkaninie używa dedykowane zasoby, które są oddzielone od hostowanej Azure aplikacji. Ponieważ służy jako jądro systemu Azure ma czas pracy w 100 procentach. Monitoruje i zarządza wystąpienia roli domen błędów.

Na poniższym diagramie przedstawiono Azure zasobach współużytkowanych, że kontroler tkaninie wdrożenie i zarządza domen różnych błędów.

![Uproszczony widok izolacji domeny błędów](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Uaktualnianie domen są podobne do domen błędów w funkcji, ale obsługuje uaktualnienia zamiast błędów. Uaktualnianie domeny jest jednostka logiczna odstęp wystąpienie, która określa, które wystąpienia w określonej usługi zostaną uaktualnione w punkcie w czasie. Domyślnie dla danego wdrożenia usług hostingowych pięć uaktualnienia domeny są definiowane. Jednak możesz zmienić tę wartość w pliku definicji usługi. Załóżmy na przykład, że masz wystąpienia osiem ról w sieci web. Będą dwa wystąpienia w trzech domenach uaktualnienia i dwa wystąpienia w jedną domenę uaktualnienia. Azure definiuje kolejność aktualizacji, ale jest oparty na liczby domen uaktualnienia. Aby uzyskać więcej informacji na uaktualnienia domeny Zobacz [Aktualizowanie usługi w chmurze](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Funkcje w innych usług

Oprócz obsługujące obliczeń wysoka dostępność funkcji platformy Azure umożliwia osadzenie funkcje wysokiej dostępności do jego innych usług. Na przykład magazyn Azure obsługuje trzy repliki wszystkich obiektów blob, tabele i dane kolejki. Umożliwia także opcja replikacji geo do przechowywania kopii zapasowych obiektów blob i tabel w regionie pomocniczą. Sieci dostarczania zawartości Azure umożliwia obiektów blob w pamięci podręcznej na świecie zarówno nadmiarowości i skalowalność. Baza danych SQL Azure przechowuje również wiele replik.

Oprócz [wskazówki techniczne elastyczność](https://aka.ms/bctechguide) seria artykułów zobacz [Najważniejsze wskazówki dotyczące projektowania pełnej usług na usług w chmurze Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) dokument. Te dokumenty zawierają Dyskusja szczegółowego dostępności platformy Azure.

Chociaż Azure udostępnia wiele funkcji, które zwiększają wysokiej dostępności, jest zrozumieć ich ograniczeń:

- Obliczeń Azure gwarantuje, że poszczególnych ról są dostępne i działają, ale nie wiesz, jeśli aplikacja jest uruchomiony lub przeciążony.
- Bazy danych SQL Azure dane są replikowane synchronicznie w regionie. Możesz wybrać aktywne geo replikacji, dzięki czemu maksymalnie czterech kopii dodatkowej bazy danych, w tym samym regionie (lub różnych regionów). Te repliki bazy danych nie są w chwili wykonywania kopii zapasowych. Bazy danych SQL zapewniają możliwość tworzenia kopii zapasowych w chwili. Aby dowiedzieć się więcej o funkcjach SQL bazy danych — chwili, przeczytaj [Punktu bazy danych SQL Azure w polu Czas Przywróć](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Do przechowywania Azure dane tabeli i obiektów blob są replikowane domyślnie alternatywny region. Jednak nie można uzyskać dostępu repliki do momentu Microsoft wybiera przechodzić do alternatywnego witryny. Awaria region wystąpiła tylko w przypadku przerwania długotrwałego usługi region, a istnieje nie SLA raz geo przypadku awarii. Jest również należy zauważyć, że uszkodzenie danych szybko strony widzące do replik.

Z tego powodu powinny uzupełnić platformy dostępności z dostępności aplikacji. Funkcje dostępności aplikacji obejmują funkcję migawkę obiektów blob do tworzenia kopii zapasowych w chwili obiektów blob danych.

###<a name="availability-sets-for-azure-virtual-machines"></a>Dostępność ustawia dla maszyn wirtualnych Azure

Większość w tym artykule omówiono usługami w chmurze, korzystające z platformy jako model usługi (PaaS). Istnieją jednak również określonych dostępności dla Azure maszyn wirtualnych, które używa infrastruktury jako model usługi (IaaS). Aby osiągnąć wysoką dostępność maszyn wirtualnych, należy użyć zestawy dostępności. Konfigurowanie dostępności służy podobne funkcje do błędów i uaktualnianie domen. W zestawie dostępność Azure umieszcza maszyn wirtualnych w sposób, który uniemożliwia błędów zlokalizowanym sprzętu i działania związane z obsługą wyłączania wszystkich komputerów w tej grupie. Zestawy dostępności są wymagane do osiągnięcia Umowa dotycząca poziomu usług Azure dostępność maszyn wirtualnych.

Poniższy diagram zawiera reprezentację dwa zestawy dostępność tej sieci web grupy i maszyn wirtualnych programu SQL Server, odpowiednio.

![Dostępność ustawia dla maszyn wirtualnych Azure](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Na powyższym diagramie programu SQL Server jest zainstalowana i uruchomiona w środowisku maszyn wirtualnych systemu. To różni się od poprzedniej dyskusji bazy danych SQL Azure, która udostępnia bazy danych jako zarządzanych usług.

##<a name="application-strategies-for-high-availability"></a>Strategie aplikacji wysokiej dostępności

Większość strategii aplikacji wysokiej dostępności obejmują nadmiarowości lub usunięcie słabo zależności między składnikami aplikacji. Projekt aplikacji powinien obsługiwać odporność na uszkodzenia podczas sporadycznie przestoje Azure lub usług innych firm. W poniższych sekcjach opisano wzorców aplikacji na zwiększenie dostępności usług w chmurze.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynchroniczne komunikacji i trwałe kolejki

Należy rozważyć, czy asynchroniczne komunikacji między usługami luźno powiązanych, aby zwiększyć dostępność w aplikacjach Azure. W tym deseniu redagowanie wiadomości do miejsca do magazynowania kolejki lub kolejek Bus usługi Azure do późniejszego przetworzenia. Podczas pisania wiadomości do kolejki sterowania natychmiast zwraca nadawcy wiadomości. Innej warstwie aplikacji obsługuje wiadomości przetwarzania, zwykle wdrożonej rolę pracownika. Jeśli roli Pracownik przejście w dół, w kolejce kumulują się wiadomości, dopóki Usługa przetwarzania zostanie przywrócony. Jak kolejki jest dostępny, istnieje nie bezpośrednia zależność między zewnętrzną nadawcy i procesor wiadomości. Dzięki temu wymagania połączeń synchroniczne usług, które mogą być gardło przepustowość w aplikacjach rozłożone.

Odmiany tego używa magazyn Azure (BLOB, tabele, kolejek) lub kolejki Bus usługi jako miejsce pracy awaryjnej połączenia bazy danych nie powiodło się. Na przykład połączenie obraz w aplikacji do innej usługi (na przykład baza danych SQL Azure) zakończy się niepowodzeniem wielokrotnie. Można szeregować te dane do magazynu trwałego. W pewnym momencie później w przypadku usługi lub bazy danych trybu online aplikacji można przesłać ponownie żądanie z magazynu. Różnica w tym modelu jest miejsce pośrednie nie stałą część aplikacji przepływu pracy. To jest używane jedynie w scenariuszy awarii.

W obu przypadkach asynchroniczne komunikacji i przechowywanie tymczasowe uniemożliwić działających wewnętrznej usługi wyłączania całej aplikacji. Kolejki służyć jako pośrednik logiczne. Więcej porad dotyczących wybierania poprawna Usługa Kolejkowanie zobacz [kolejki Azure i kolejki Bus usługi Azure — porównanie i porównanie oznaczonymi](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Logika wykrywania i spróbuj ponownie błędów

Kluczowym elementem w projekcie wysokiej dostępności aplikacji jest użycie ponów próbę logicznych kodem bezpieczne obsłużenie usługa, która tymczasowo działa. [Blokowanie aplikacji obsługi błędów przejściowych](https://msdn.microsoft.com/library/hh680934.aspx), opracowanych przez zespół Patterns firmy Microsoft i wskazówki dotyczące pomaga deweloperów aplikacji w ten proces. Wyraz "zmiennych" oznacza, że stan tymczasowy, który trwa tylko stosunkowo krótki czas. W kontekście tego artykułu obsługi błędów przejściowych jest częścią opracowywania aplikacji wysokiej dostępności. Warunki przejściowych Przykładami błędów sieciowych przerw i utracone połączenia z bazą danych.

Blok przejściowych aplikacji obsługi błędów jest uproszczone sposób obsługi błędów w kodzie w sposób bezpieczne. Można je poprawić dostępność aplikacji, dodając niezawodne przejściowych logiki obsługi błędów. W większości przypadków ponów próbę logicznych obsługuje krótkie przerwy i ponownie połączy nadawcy i odbiorcy po co najmniej jeden niepomyślne. Próba pomyślnego ponów próbę zazwyczaj pozwala przejść niezauważona użytkowników aplikacji.

Deweloperów masz trzy opcje służące do zarządzania ich Logika ponawiania: interwał przyrostowe, stałych i wykładniczy. Przyrostowe czeka już przed każdego ponowienia w rosnącymi liniowo (na przykład 1, 2, 3 i 4 sekundy). Stały zakres czeka tę samą ilość czasu między kolejnymi próbami (na przykład dwie sekundy). Przypadkowe więcej opcji wykładniczego wstecz wyłączenia czeka już między kolejnymi próbami. Jednak używa zachowanie wykładniczego (na przykład 2, 4, 8 i 16 sekund).

Strategia wysokiego poziomu z kodem jest:

1. Definiowanie polityki i strategii ponów próbę.
1. Spróbuj wykonać operację, która może spowodować błąd przejściowych.
1. W przypadku wystąpienia błędów przejściowych wywołać zasady ponów próbę.
1. Jeśli nie wszystkie próby połączenia, efektywnej ostateczne wyjątek.

Testowanie logiki ponów próbę symulowany błędy, aby upewnić się, że ponowne próby w kolejnych czynności nie powodują nieoczekiwane długie opóźnienie. W tym przed podjęciem decyzji niepowodzenie ogólne zadanie.

###<a name="reference-data-pattern-for-high-availability"></a>Wzorzec danych odwołania wysokiej dostępności

Dane źródłowe jest tylko do odczytu danych aplikacji. Te dane udostępnia kontekstu firm, w którym aplikacja generuje dane transakcji w trakcie operacji biznesowych. Transakcji danych jest funkcją w chwili danych źródłowych. Dlatego jej integralności zależy od migawkę danych źródłowych w momencie transakcji. Jest to nieco luźno definicji, ale należy go wystarczające w celu naszych tutaj.

Dane odwołania w kontekście aplikacji są niezbędne do działania aplikacji. Z odpowiednich aplikacji tworzenie i Obsługa danych referencyjnych; systemy zarządzania (MDM) dane główne często tej funkcji. Systemy te są odpowiedzialne za cyklu życia danych źródłowych. Przykładami danych źródłowych wykazu produktów, podstawowe pracownika, wzorzec części i wyposażenia wzorzec. Dane źródłowe można pochodzą spoza organizacji, na przykład kody pocztowe lub stawek podatkowych. Strategie dotyczące zwiększenie dostępności danych źródłowych utrudniają zwykle mniej niż transakcji danych. Dane źródłowe ma przeważnie prowadzoną niezmienne.

Istnieje możliwość Azure sieci web i pracownik role, które używają danych źródłowych autonomicznego w czasie wykonywania wdrażając danych źródłowych wraz z aplikacji. Jeśli rozmiar magazynu lokalnego zezwala takiej instalacji, to idealne rozwiązanie w stanie. Osadzone bazy danych (SQL, NoSQL) lub pliki XML używany lokalny system plików pomoże niezależności skali Azure obliczeń. Jednak trzeba mechanizmu aktualizowanie danych w poszczególnych ról bez konieczności ponownego rozmieszczania. Aby to zrobić, umieść wszystkie aktualizacje do danych źródłowych punkt końcowy miejsca do magazynowania chmurze (na przykład magazyn obiektów Blob platformy Azure lub bazy danych SQL). Dodawanie kodu do każdej roli, który pobiera aktualizacje danych w węzłach do uruchamiania podczas uruchamiania roli. Możesz również dodać kod, który umożliwia administratorowi wykonywanie wymuszonego pobieranie do wystąpienia roli.

Aby zwiększyć dostępność, ról powinien też zawierać zbiór danych źródłowych w przypadku, gdy miejsce do magazynowania jest w dół. Dzięki temu role rozpoczynać podstawowy zestaw danych źródłowych, dopóki nie zasób magazynowania będzie dostępne aktualizacje.

![Wysokiej dostępności aplikacji przez węzły autonomicznego obliczeń](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Jeden uwagę tego wzorca jest wdrożenie i szybkości uruchamiania dla poszczególnych ról. Jeśli są wdrażanie lub pobieranie dużych ilości danych źródłowych po ponownym uruchomieniu komputera, to można zwiększyć czas potrzebny na pokrętła wdrożenia nowej lub wystąpienia roli. Może to być akceptowane zależnościami dla niezależności o danych źródłowych natychmiast dostępne na poszczególnych ról, a nie w zależności od usług zewnętrznych magazynu.

###<a name="transactional-data-pattern-for-high-availability"></a>Wzorzec danych transakcji wysokiej dostępności

Transakcji dane znajdują się dane, które aplikacja generuje w kontekście firm. Dane transakcji to kombinacja zestaw procesów biznesowych, które wykonuje aplikacji i danych źródłowych, która obsługuje te procesy. Przykłady transakcji danych może zawierać zamówienia, zaawansowane uwagi wysyłki faktur i możliwości zarządzania (klientami CRM) relacji klienta. Transakcji danych, a więc generowanych będzie rana do systemów zewnętrznych dotyczących przechowywania danych lub dalszego przetwarzania.

Należy pamiętać, że odwołanie, które danych można zmienić w systemach, które są odpowiedzialne za te dane. Z tego powodu transakcji danych należy zapisać kontekstu danych w chwili odwołania, aby minimalnego zależności zewnętrznych jego semantyczny spójności. Na przykład można rozważyć usuwania produktu z katalogu kilka miesięcy po zamówienie jest zamknięte. Najlepiej jest osadzenie tyle kontekstu danych odwołania jako możliwe do transakcji. Zachowuje znaczenie skojarzone z transakcji, nawet jeśli ulegnie zmianie danych źródłowych, gdy transakcja jest rejestrowany.

Jak już wspomniano, architektury używające luźno sprzężenia i komunikacji asynchroniczne redagowanie wyższy poziom dostępności. Dotyczy także transakcji danych, ale wykonania jest bardziej złożona. Tradycyjne pojęcia transakcji zwykle oparte na zagwarantowanie transakcji bazy danych. Po wprowadzeniu pośrednie warstwy, kod aplikacji poprawnie musi obsługiwać dane w różnym warstwom zapewnienie wystarczających spójności i wytrzymałości.

Następującej opisano oddzielający Przechwytywanie transakcji danych od ich przetwarzanie przepływu pracy:

1. Węzeł obliczeń w sieci Web: prezentowanie danych referencyjnych.
1. Zewnętrzna: zapisywanie pośrednie danych transakcji.
1. Węzeł obliczeń w sieci Web: Kończenie transakcji przez użytkownika końcowego.
1. Węzeł obliczeń w sieci Web: wysyłanie złożonym danych transakcji, wraz z kontekst danych odwołania do tymczasowy obszar trwały jest gwarantowana przewidywalne odpowiedzi.
1. Węzeł obliczeń w sieci Web: sygnału na wykonanie użytkowników końcowych transakcji.
1. Tło obliczyć węzeł: wyodrębnianie danych transakcji, po przetworzeniu je w razie potrzeby i wyślij go do lokalizacji przechowywania końcowego w systemie.

Na poniższym diagramie przedstawiono jednej możliwości stosowania tego projektu w usłudze w chmurze hostowanej Azure.

![Wysoką dostępność dzięki luźno sprzężenia](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Kreskowane strzałki na powyższym diagramie wskazują Przetwarzanie asynchroniczne. Roli frontonu sieci web nie są znane z tym Przetwarzanie asynchroniczne. Prowadzi to do przechowywania transakcji w jej przeznaczenia w odniesieniu do bieżącego systemu. Ze względu na czas oczekiwania wprowadza asynchroniczne modelu transakcji dane nie są od razu dostępne dla kwerendy. W związku z tym każdej jednostki transakcji danych musi być zapisywane w pamięci podręcznej lub musi sesji użytkownika spotkania natychmiastowej interfejsu użytkownika.

Rola w sieci web jest niezależny od pozostałej części infrastruktury. Profil jej dostępność to kombinacja ról w sieci web i kolejce Azure i nie całej infrastruktury. Oprócz wysoką dostępność ta metoda pozwala ról w sieci web do skalowanie w poziomie, niezależnie od miejsca do magazynowania wewnętrznej. Ten model wysokiej dostępności mogą mieć wpływ na zarządzanie operacji. Dodatkowe składniki, takich jak kolejki Azure i ról pracownik może wpłynąć na miesięczne koszty użycia.

Należy zauważyć, że poprzedniego diagram przedstawia jeden stosowania tej metody rozłączone transakcji danych. Istnieje wiele możliwych implementacji. Poniższa lista zawiera niektóre rozwiązania alternatywne:

 * Rola Pracownik może znajdować się między ról w sieci web i kolejki miejsca do magazynowania.
 * Kolejka Bus usługi można używać zamiast kolejkę magazyn Azure.
 * Przeznaczenia może być magazyn Azure lub dostawcy inną bazę danych.
 * Azure pamięci podręcznej może służyć w warstwie sieci web podania natychmiastowej wymagania buforowania po transakcji.

###<a name="scalability-patterns"></a>Desenie skalowalność

Należy pamiętać, że skalowalność usług w chmurze bezpośrednio wpływa na dostępność. Jeśli zwiększona ładowania powoduje usługi odpowiadać, wrażenia użytkownika jest aplikacji w dół. Wykonaj najważniejsze wskazówki dotyczące skalowalność, w zależności od obciążenia oczekiwanych aplikacji i przyszłych oczekiwań. Skala najwyższe obejmuje wiele zagadnienia, takie jak wykorzystanie jedno-i wiele kont miejsca do magazynowania, udostępnianie wielu baz danych i buforowanie strategii. Aby szczegółowo przyjrzeć deseni zobacz [Najważniejsze wskazówki dotyczące usługi projektowania pełnej na Azure usług w chmurze](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii artykułów dotyczyła [Odzyskiwanie i wysoką dostępność dla aplikacji utworzonych na platformy Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Następny artykuł z tej serii jest [awarii dla aplikacji utworzonych na platformy Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
