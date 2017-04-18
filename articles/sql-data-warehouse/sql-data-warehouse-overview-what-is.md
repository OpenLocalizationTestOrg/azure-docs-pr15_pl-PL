<properties
   pageTitle="Co to jest magazynu danych SQL Azure? | Microsoft Azure"
   description="Klasy korporacyjnej rozkładana możliwościach przetwarzania petabyte ilości danych relacyjnych i -relacyjne bazy danych. Jest branżowe pierwszej cloud danych magazynu z zwiększania, Zmniejsz i zatrzymaj wskaźnik myszy w sekundach."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Co to jest magazynu danych SQL Azure?

Magazyn danych SQL Azure jest oparte na chmurze, poza skalowanie bazy danych do przetwarzania dużych ilości danych relacyjnych i -relacyjnych. Oparte na naszej architektury przetwarzanie znacznym stopniu równoległe (MPP), magazynu danych SQL może obsługiwać obciążenie pracą Twojej organizacji.

Magazyn danych SQL:

- Łączy relacyjnej bazy danych programu SQL Server z funkcjami poza skalowanie Azure chmury. Można zwiększyć, Zmniejsz, Zatrzymaj wskaźnik myszy lub wznowienie obliczeń w sekundach. Możesz zapisać koszty Skalowanie zewnętrzne Procesora, gdy ich potrzebujesz, a przy wycinaniu ponownie zastosowania w godzinach innych niż Szczyt.
- Wykorzystuje platformie Azure. Jest łatwa do wdrożenia, bezproblemowo obsługiwane i pełni odporność na uszkodzenia ze względu na automatyczne ups Wstecz.
- Uzupełnia ekosystemie programu SQL Server. Można tworzyć za pomocą narzędzi i znanych SQL Server Transact-SQL (T-SQL).

Ten artykuł zawiera opis najważniejszych funkcji programu SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Architektura przetwarzania znacznym stopniu równoległego

Program SQL Data Warehouse jest przetwarzanie znacznym stopniu równoległe (MPP) Rozproszony system bazy danych. Dzieląc danych i możliwości przetwarzania w różnych węzłach, magazynu danych SQL mogą oferować dużego skalowalność — daleko poza dowolny pojedynczy system.  W tle magazynu danych SQL strony widzące danych w wielu udostępnionych żaden element nie miejsca do magazynowania i jednostek. Dane są przechowywane w magazynie lokalnie zbędne Premium i połączone do obliczenia węzły wykonywania zapytań. Z tej architektury magazynu danych SQL przetwarza podejście "Dzielenie i Podbój" systemem obciążenia i złożonych kwerend. Żądania są odbierane przez węzeł formantu, zoptymalizowane i następnie przekazywane do węzłów obliczeń w pracy równolegle.

Łącząc architektura MPP i możliwości Azure miejsca do magazynowania, magazynu danych SQL wykonywać następujące czynności:

- Powiększanie lub zmniejszanie przestrzeni dyskowej niezależnie od obliczeń.
- Powiększanie lub zmniejszanie obliczeń bez przenoszenia danych.
- Wstrzymaj wydajności przy zachowaniu danych bez zmian.
- Życiorys obliczyć zdolności w momencie powiadomienie.

Na poniższym diagramie przedstawiono architektura bardziej szczegółowo.

![Architektura magazynu danych SQL][1]


**Węzeł sterowania:** Węzeł formantu zarządza i optymalizuje kwerendy. Jest frontonu, który współdziała z wszystkie aplikacje i połączenia. W magazynie danych SQL węzeł formantu jest obsługiwany przez bazy danych SQL i połączyć się z nią wygląda i zdania tak samo. W obszarze powierzchnię węzeł formantu współrzędne wszystkie przenoszenia danych i obliczeń wymagane do uruchamiania zapytania równolegle z rozkładem danymi. Po przesłaniu kwerendy T SQL do magazynu danych SQL węzeł formantu przekształca je w oddzielne kwerendy stosowanych w każdym węźle obliczeń równolegle.

**Obliczyć węzły:** Węzły obliczeń służyć jako power za magazynu danych SQL. Są one baz danych SQL przechowywanie danych i procesu kwerendy. Podczas dodawania danych SQL Data Warehouse rozdziela wiersze węzły obliczeń. Węzły obliczeń są pracowników, stosowanych zapytania równolegle w danych. Po przetworzeniu, przekazują wyniki do węzła kontrolki. Aby zakończyć kwerendę, węzeł formantu agreguje wyniki i zwraca wynik końcowy.

**Miejsca do magazynowania:** Dane są przechowywane w magazynie obiektów Blob platformy Azure. Węzły obliczeń interakcji z danymi, pisanie i przeczytaj bezpośrednio do i z magazynem obiektów blob. Ponieważ Azure magazynowania powoduje rozszerzenie przezroczyste i znacznie, magazynu danych SQL można zrobić to samo. Ponieważ niezależnych obliczeń i miejsca do magazynowania, magazynu danych SQL może automatyczne skalowanie miejsca do magazynowania niezależnie od skalowania obliczeń i na odwrót. Magazyn obiektów Blob platformy Azure jest również w pełni odporność na uszkodzenia i upraszcza proces i przywracania kopii zapasowych.

**Usługi przepływu danych:** Usługa przepływu danych (systemie) przenosi dane między węzły. Systemie zapewnia dostęp węzły obliczeń do danych, które są potrzebne dla sprzężeń i agregacji. Systemie nie jest usługą Azure. Jest usługa Windows uruchamiana wraz z bazą danych SQL na wszystkich węzłach. Ponieważ systemie działa w tle, użytkownik nie będzie interakcji z go bezpośrednio. Jednak po wyświetleniu planów kwerend Zauważ, że zawierają niektóre operacje systemie, ponieważ przenoszenia danych jest niezbędne do równolegle każda kwerenda.


## <a name="optimized-for-data-warehouse-workloads"></a>Zoptymalizowany pod kątem obciążenia magazynu danych

Metody MPP jest wspomaganie liczby danych składu optymalizacji wydajności zależnych, w tym:

- Optymalizator zapytań rozłożone i zbioru statystyki złożonych przez wszystkie dane. Za pomocą informacji na danych, rozmiar i dystrybucji usługi będzie mógł zoptymalizować kwerendy według oceny kosztów operacji określonej rozłożone kwerendy.

- Zaawansowane algorytmy i technik zintegrowane w procesie przepływu danych wydajne przenoszenie danych między przetwarzania zasobów stosownie do potrzeb, aby uruchomić kwerendę. Te operacje przepływ danych jest wbudowany w, a wszystkie optymalizacje do usługi przepływu danych zostanie przeprowadzona automatycznie.

- Wykres kolumnowy grupowany indeksy **columnstore** domyślnie. Korzystając z magazynu opartego na kolumny, magazynu danych SQL otrzymuje średnio 5 zyski kompresji x na tradycyjne zorientowana miejsca do magazynowania i do maksymalnie 10 x lub więcej zwiększenie wydajności zapytania. Kwerendy analizy, które zeskanować wielu doskonałe indeksów columnstore pracy wierszy.


## <a name="predictable-and-scalable-performance"></a>Wydajność przewidywalne i skalowalność

Program SQL Data Warehouse rozdzielający miejsca do magazynowania i obliczeń, dzięki czemu każdy przeskalować niezależnie. SQL Data Warehouse szybkie i łatwe przeskalować dodawać obliczeń dodatkowe zasoby w momencie powiadomienie. To uzupełnienie jest użycie magazyn obiektów Blob platformy Azure. Obiektów blob udostępniać nie tylko trwałe, zreplikowanej miejsca do magazynowania, ale również infrastruktury łatwe rozszerzenia w niskiej cenie. Przy użyciu tej kombinacji cloud skalą miejsca do magazynowania i Azure obliczeń, magazynu danych SQL umożliwia zapłacić za wydajności kwerend i miejsca do magazynowania, gdy ich potrzebujesz. Zmiany liczby obliczeń jest tak proste jak suwaka w portalu Azure do lewej lub do prawej lub mogą również planowane, przy użyciu programu PowerShell i T-SQL.

Wraz z możliwości w pełni kontrolować liczbę obliczeń niezależnie od miejsca do magazynowania magazynu danych SQL pozwala w pełni wstrzymania magazynu danych, co oznacza, że nie możesz zapłacić za obliczeń, po jej nie potrzebujesz. Przy zachowaniu z magazynu w miejscu, wszystkie obliczeń jest publikowany w puli głównym Azure, możesz oszczędności. W razie potrzeby, po prostu Życiorys obliczeń mają danych i obliczenia dostępne dla Twojej obciążenie pracą.

## <a name="data-warehouse-units"></a>Jednostki magazynu danych

Alokacja zasobów do magazynu danych SQL jest mierzony w jednostki magazynu danych (DWUs). DWUs jest miarą źródłowych zasoby takie jak Procesora i pamięci, operacji i/o na SEKUNDĘ, które zostało przydzielonych do magazynu danych SQL. Zwiększenie liczby DWUs zasobów i zwiększa wydajność. W szczególności DWUs pomocy, upewnij się, że:

- Masz możliwość skalowania magazynu danych, nie martwiąc się oprogramowania lub sprzętu źródłowej.

- Zwiększenie wydajności na poziomie DWU można prognozować przed zmianą rozmiaru magazynu danych.

- Podstawowe sprzętu i oprogramowania do wystąpienia można zmienić lub przenieść bez wpływu na wydajność obciążenie pracą.

- Microsoft można dostosować do podstawowej architektury usługi bez wpływu na wydajność usługi Obciążenie pracą.

- Microsoft szybko zwiększyć wydajność w magazynie danych SQL, w sposób, który jest skalowalna i równomiernie efekty systemu.

Jednostki magazynu danych zapewniają miarę trzy precyzyjnie metryk, które są ściśle powiązane z danymi składu obciążenie pracą wydajności. Celem jest to, że następujące metryki klucza obciążenie pracą będzie przebieg liniowy z DWUs, wybranych dla magazynu danych.

**Skanowania i agregacji:** Ten obciążenie pracą Metryka standardowych danych składu kwerendę, która skanuje dużej liczby wierszy, a następnie złożonych agregacji. To jest intensywnie operacji wejścia/wyjścia oraz Procesora.

**Obciążenia:** Ta metryka środków możliwość mogły zjeść tej ostatniej danych w usłudze. Obciążenia, są wykonywane z PolyBase ładowania przedstawiciela zestawu danych z magazynem obiektów Blob platformy Azure. Ta metryka jest przeznaczony do sieci obciążenia i Procesora aspektów usługę.

**Tworzenie tabeli jako wybierz pozycję (CTAS):** CTAS środków możliwość kopiowania tabeli. Wymaga to odczytywania danych z magazynu, dystrybucję węzły urządzenia i ponownie wpisywać do magazynu. Jest operacji intensywnie Procesora, Jo i sieci.

## <a name="pause-and-scale-on-demand"></a>Wstrzymaj i skalę na żądanie

Jeśli chcesz szybsze uzyskiwanie wyników, Zwiększ usługi DWUs i zapłacić w celu zwiększenia wydajności. Po potrzebujesz mniej obliczyć power, Zmniejsz usługi DWUs i zapłacić tylko w przypadku tego, czego potrzebujesz. Może wziąć pod uwagę zmieniania swojego DWUs w następujących sytuacjach:

- Kiedy nie musisz uruchomić kwerendy, być może wieczory lub w weekendy, przełączanie w stan spoczynku zapytań. Następnie zatrzymaj wskaźnik myszy zasobów obliczeń w celu uniknięcia płacisz za DWUs, gdy nie są potrzebne.

- Jeśli systemie jest niska żądanie, warto rozważyć skrócenia DWU niewielki rozmiar. Nadal można korzystać z danych, ale znaczące oszczędności.

- Podczas wykonywania operacji ładowania lub przekształcenie dużą ilością danych, być może zechcesz rozbudowy tak, aby szybciej danych jest dostępna.

Aby dowiedzieć się, co to jest idealnym wartość DWU, spróbuj skalowania w górę lub w dół i uruchamianie kwerend kilka po załadowaniu danych. Ponieważ skalowania jest szybkie, możesz spróbować szereg różnych poziomów wydajności, godziny lub mniej.  Należy pamiętać, że magazynu danych SQL jest przeznaczony do przetwarzania dużych ilości danych i, aby wyświetlić jego PRAWDA możliwości skalowania, zwłaszcza w większej skali, którego oferujemy, należy użyć duży zestaw danych, które zbliża się lub przekracza 1 TB.


## <a name="built-on-sql-server"></a>Dostępne w programie SQL Server

Program SQL Data Warehouse jest oparty na aparacie relacyjnej bazy danych programu SQL Server i zawiera wiele funkcji, które można się spodziewać z magazynu danych przedsiębiorstwa. Jeśli znasz już T-SQL, jest proste przenieść swoją wiedzę do magazynu danych SQL. Czy są zaawansowane lub zaczynasz korzystać, przykładów w dokumentacji pomocy, rozpocząć. Ogólnie mówiąc może wziąć pod uwagę tak, że firma Microsoft zostały wykonane elementy języka SQL Data Warehouse w następujący sposób:

- Program SQL Data Warehouse używa składni języka SQL T wiele operacji. Obsługuje ona również szeroki tradycyjnych konstrukcji SQL, takie jak procedury składowane, funkcje zdefiniowane przez użytkownika, podziału tabeli, indeksy i sortowania.

- Program SQL Data Warehouse również zawiera liczbę nowe funkcje programu SQL Server, w tym: grupowany indeksy **columnstore** , integracja PolyBase i dane inspekcji (wraz z ocenę zagrożeń).

- Niektóre elementy języka T-SQL, które są mniej powszechne danych składu obciążenia lub nowszej do programu SQL Server mogą być obecnie niedostępne. Aby uzyskać więcej informacji zapoznaj się z [dokumentacją migracji][].

Przy użyciu funkcji i języku Transact-SQL wspólny dla obu programu SQL Server, program SQL Data Warehouse bazy danych SQL i analizy platformy systemu można utworzyć rozwiązanie, który odpowiada Twoim potrzebom danych. Można decydowanie, gdzie zachować dane, na podstawie wydajności, zabezpieczeń i wymagania skali, a następnie przenieść dane w razie potrzeby między różnymi systemami.

## <a name="data-protection"></a>Ochrona danych

Magazynu danych SQL są przechowywane wszystkie dane w magazynie lokalnie zbędne Azure Premium. Wiele kopii synchroniczne dane są zachowywane w centrum danych lokalnych zagwarantowanie ochrony danych przezroczyste w przypadku zlokalizowanym błędy. Ponadto magazynu danych SQL automatycznie tworzy kopię zapasową aktywne (nie wstrzymanych) baz danych w regularnych odstępach czasu przy użyciu migawek miejsca do magazynowania Azure. Aby dowiedzieć się więcej na temat tworzenia kopii zapasowych i przywracanie działa, zobacz [Omówienie i przywracania kopii zapasowych][].

## <a name="integrated-with-microsoft-tools"></a>Zintegrowany z narzędzia Microsoft

Magazyn danych SQL integruje także wiele narzędzi tego użytkownicy mogą mieć znanych z programu SQL Server. W tym:

**Narzędzia tradycyjnych programu SQL Server:** Program SQL Data Warehouse jest w pełni zintegrowany z usług SQL Server Analysis Services, usług Integration Services i usług Reporting Services.

**Narzędzia oparte na chmurze:** Program SQL Data Warehouse można używać razem z pakietem liczba nowych narzędzi w Azure, w tym Factory danych, analizy strumieniu nauki komputera i usługi Power BI. Aby bardziej pełną listę zobacz [Omówienie zintegrowanych narzędzi][].

**Narzędzia innych producentów:** Duża liczba dostawców narzędzia innej firmy mieć certyfikat integracji ich narzędzi z magazynu danych SQL. Aby uzyskać pełną listę zobacz [partnerów rozwiązanie magazynu danych SQL][].

## <a name="hybrid-data-sources-scenarios"></a>Hybrydowe scenariuszy źródeł danych

Program SQL Data Warehouse za pomocą PolyBase umożliwia użytkownikom bezprecedensowej przenoszenie danych między ich ekosystemu odblokowywanie możliwości konfiguracji hybrydowych zaawansowanych scenariuszy-relacyjnych i lokalnych źródeł danych.

Polybase pozwala korzystać z różnych źródeł danych za pomocą znanych poleceń T-SQL. Polybase pozwala zbadać-relacyjnych danych przechowywanych w magazynie obiektów Blob platformy Azure, tak jakby była zwykła tabeli. Za pomocą Polybase dane-relacyjne kwerendy lub importowanie danych-relacyjnych do magazynu danych SQL.

- PolyBase używa tabele zewnętrzne, aby uzyskiwać dostęp do danych-relacyjnych. Definicje tabeli są przechowywane w magazynie danych SQL i zapewnienia ich dostępności przy użyciu języka SQL i narzędzi, takich jak możesz chcesz uzyskać dostęp do danych relacyjnych normalny.

- Polybase jest o niesprecyzowanym w jego integracji. Udostępnia go te same funkcje i funkcji do wszystkich źródeł, które obsługuje. Dane odczytane przez Polybase mogą być w różnych formatach, takich jak pliki rozdzielane lub ORC.

- PolyBase można używać do uzyskiwania dostępu magazyn obiektów blob, która jest również używana jako miejsca do magazynowania dla klastrów HDInsight. To umożliwia dostęp do tych samych danych przy użyciu narzędzi relacyjnych i -relacyjnych.

## <a name="sla"></a>UMOWA DOTYCZĄCA POZIOMU USŁUG

Program SQL Data Warehouse oferuje produktu poziomu Umowa dotycząca poziomu usług (SLA) jako składnik pakietu Microsoft Online Services Umowa dotycząca poziomu usług. Aby uzyskać więcej informacji odwiedź stronę [Umowa dotycząca poziomu usług dla magazynu danych SQL][]. Umowa dotycząca poziomu usług informacji o wszystkich innych produktów mogą odwiedzić witrynę Azure [Świadczeniu] strony lub pobrać je na stronie [Licencjonowania zbiorowego][] . 

## <a name="next-steps"></a>Następne kroki

Dowiedz się, po zapoznaniu się nieco magazynu danych SQL, jak szybko [utworzyć magazynu danych SQL][] i [ładowanie przykładowych danych][]. Jeśli jesteś nowym użytkownikiem Azure, może się okazać [Azure słownik][] pomocne podczas wystąpienia terminologia nowy. Lub zapoznaj się niektóre z tych innych SQL danych magazynu zasobów.  

- [Przykłady sukcesów klientów]
- [Blogów]
- [Funkcja żądania]
- [Klipy wideo]
- [Blogów zespołu doradcze klienta]
- [Tworzenie bilet pomocy technicznej]
- [Forum w witrynie MSDN]
- [Forum przepełnienia stosu]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Tworzenie bilet pomocy technicznej]: ./sql-data-warehouse-get-started-create-support-ticket.md
[ładowanie przykładowych danych]: ./sql-data-warehouse-load-sample-databases.md
[Tworzenie magazynu danych SQL]: ./sql-data-warehouse-get-started-provision.md
[Dokumentacja dotycząca migracji]: ./sql-data-warehouse-overview-migrate.md
[Partnerzy rozwiązanie magazynu danych SQL]: ./sql-data-warehouse-partner-business-intelligence.md
[Omówienie zintegrowanych narzędzi]: ./sql-data-warehouse-overview-integrate.md
[Omówienie i przywracania kopii zapasowych]: ./sql-data-warehouse-restore-database-overview.md
[Azure — słownik]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Przykłady sukcesów klientów]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogów]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogów zespołu doradcze klienta]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funkcja żądania]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum w witrynie MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Forum przepełnienia stosu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Klipy wideo]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[Umowa dotycząca poziomu usług dla magazynu danych SQL]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Licencjonowanie zbiorowe]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Umów dotyczących poziomu usług]: https://azure.microsoft.com/en-us/support/legal/sla/
