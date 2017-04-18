<properties
   pageTitle="Omówienie zabezpieczeń sieci Azure | Microsoft Azure"
   description=" Ten artykuł ułatwia zrozumienie Microsoft Azure występują do oferowania w zakresie zabezpieczeń sieci. Firma Microsoft udostępnia podstawowe informacje i informacje o podstawowych pojęć związanych z zabezpieczeniami sieci i wymagania na Azure występują do oferowania w każdym z tych obszarów. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Omówienie zabezpieczeń sieci Azure

Microsoft Azure zawiera niezawodne infrastrukturę sieci do obsługi aplikacji i usług łączności wymagania usługi. Łączność sieciowa jest możliwe między zasobów znajdujących się w Azure, między lokalnego i Azure hostowana zasobów i oraz z Internetem i Azure.

Celem tego artykułu jest aby ułatwić zrozumienie Microsoft Azure występują do oferowania w zakresie zabezpieczeń sieci. W tym miejscu firma Microsoft udostępnia podstawowe wyjaśnienia podstawowych pojęć związanych z zabezpieczeniami sieci i wymagania. Również otrzymasz informacji na Azure występują do oferowania w każdym z tych obszarów. Istnieje wiele łącza do innej zawartości, umożliwiające uzyskanie szczegółowego opis w obszarach, w których interesują.

Ten artykuł Omówienie zabezpieczeń sieci Azure będzie skoncentrowanie się na następujących czynności:

- Azure sieci
- Kontrola dostępu do sieci
- Bezpiecznego połączenia zdalnego dostępu i lokalnej krzyżowe
- Dostępność
- Rejestrowanie
- Rozpoznawanie nazw
- Architektura strefy Zdemilitaryzowanej
- Centrum zabezpieczeń Azure

## <a name="azure-networking"></a>Azure sieci

Maszyn wirtualnych potrzebujesz połączenia sieciowego. Aby obsługiwać ten wymóg, Azure wymaga maszyn wirtualnych jest połączenie z siecią wirtualną Azure. Wirtualna sieć Azure jest logiczne konstrukcji, utworzona na tkaninie sieci fizycznej Azure. Każdy logiczne Azure wirtualnej sieci jest ze wszystkich innych Azure wirtualnej sieci. Pomaga to upewnić się, że ruch sieciowy wdrożeń nie jest dostępny dla innych klientów Microsoft Azure.

Dowiedz się więcej:

- [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Kontrola dostępu do sieci
Kontrola dostępu do sieci jest czynnością ograniczenia łączności do i z określonych urządzeń lub podsieci w wirtualnej sieci Azure. Celem kontroli dostępu do sieci jest upewnij się, że maszyn wirtualnych i usługi są dostępne tylko dla użytkowników i urządzeń, do których mają dostęp. Uzyskiwanie dostępu do formantów są oparte na zezwolenie lub odmowa decyzje dotyczące połączeń do i z z maszyn wirtualnych lub usługi.

Azure obsługuje kilka typów kontroli dostępu do sieci. W tym:

- Kontrola warstwy sieci
- Kierowanie sterowania i wymuszone tunneling
- Wirtualna sieć zabezpieczeń urządzeń

### <a name="network-layer-control"></a>Kontrola warstwy sieci
Wszelkie zabezpieczone wdrożenie wymaga niektórych miary kontroli dostępu do sieci. Celem kontroli dostępu do sieci jest upewnij się, czy maszyn wirtualnych i usług sieciowych, które działają w tych maszyn wirtualnych można komunikować się tylko z innych urządzeń sieciowych są potrzebne do komunikowania się z, a wszystkie próby połączenia są blokowane.

W razie potrzeby kontroli dostępu na poziomie sieci podstawowe (na podstawie adresu IP i protokoły TCP i UDP), można używać grup zabezpieczeń sieci. Grupa zabezpieczeń sieci (NSG) jest Zapora filtrowania podstawowe pakietów i pozwala na sterowanie dostępem według [krotki 5](https://www.techopedia.com/definition/28190/5-tuple). NSGs nie zawierają inspekcji warstwy aplikacji lub uwierzytelniony kontroli dostępu.

Dowiedz się więcej:

- [Grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Kontrolka rozsyłania i wymuszonego tunelowania
Możliwość sterowania routingu zachowaniem sieci wirtualnej Azure jest krytyczne zabezpieczeniami i dostępem kontrola dostępu do sieci. Jeśli routing jest niepoprawnie skonfigurowane, aplikacji i usług obsługiwane na tym komputerze wirtualnych może połączyć się urządzeń nie chcesz, aby nawiązać połączenie, łącznie z urządzeniami i jest przez intruzów potencjalne.

Azure sieci obsługuje możliwość dostosowania działanie routingu ruchu w sieci dla usługi Azure wirtualnych sieci. Dzięki temu będzie można zmienić domyślne pozycje tabeli routingu w wirtualnych sieci Azure. Kontrolka działanie routingu ułatwia upewnij się, że cały ruch z pewnych urządzeń lub grupy urządzeń wprowadza lub pozostawia wirtualnej sieci Azure do określonej lokalizacji.

Na przykład może być urządzenie zabezpieczające wirtualnej sieci wirtualnej sieci Azure. Chcesz upewnić się, że cały ruch do i z siecią wirtualną Azure przechodzi przez tego urządzenia wirtualnego zabezpieczeń. Możesz to zrobić przez skonfigurowanie [Marszrut zdefiniowane przez użytkownika](../virtual-network/virtual-networks-udr-overview.md) w Azure.

[Wymuszone tunelowania](https://www.petri.com/azure-forced-tunneling) jest mechanizm, których można użyć, aby upewnić się, że usługi nie są dozwolone do nawiązywania połączeń z urządzeniami w Internecie. Należy zauważyć, że to różni się od akceptowania połączeń przychodzących, a następnie może uzyskać do nich. Potrzebujesz serwerach frontonu sieci web na odpowiedź na żądanie z hostów internetowych, a więc Internet-powierzając jej ich konserwację ruch jest dozwolony ruch przychodzący na tych serwerach sieci web i na serwerach sieci web mogą odpowiedzieć.

Co nie chcesz umożliwić to serwer frontonu sieci web do zainicjowania żądania ruchu wychodzącego. Wniosek może stanowić ryzyko związane z zabezpieczeniami, ponieważ te połączenia mogą być używane do pobierania złośliwego oprogramowania. Nawet wtedy, gdy chcesz tych serwerach frontonu, aby zainicjować żądań wychodzących z Internetem, można wymusić przejście przez serwery proxy do lokalnej sieci web można korzystać z adresu URL, filtrowania i rejestrowania.

Czy chcesz zamiast tego temu za pomocą tunelowania wymuszonego. Po włączeniu wymuszonego tunneling, wszystkie połączenia z Internetem wymuszeniem za pośrednictwem Centrum lokalnego. Możesz skonfigurować wymuszonego tunneling wykorzystując przekierowuje zdefiniowane przez użytkownika.

Dowiedz się więcej:

- [Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Wirtualna sieć zabezpieczeń urządzeń
Gdy grup zabezpieczeń sieci, przekierowuje zdefiniowane przez użytkownika i wymuszonego tunneling zapewniają poziom bezpieczeństwa w sieci i transportu warstw [modelu OSI](https://en.wikipedia.org/wiki/OSI_model), może być godzin, gdy chcesz włączyć zabezpieczenia na poziomie wyższymi niż sieci.

Na przykład wymagań dotyczących zabezpieczeń mogą zawierać:

- Uwierzytelniania i autoryzacji przed zezwolenia na dostęp do aplikacji
- Wykrywanie intruzów i intruzów odpowiedzi
- Aplikacja warstwy inspekcji dla protokołów wysokiego poziomu
- Filtrowanie adresów URL
- Oprogramowanie antywirusowe z poziomu sieci i ochrony przed złośliwym oprogramowaniem
- Ochrona przed Robot sieci Web
- Kontrola dostępu do aplikacji
- Dodatkowe ochrony DDoS (nad DDoS, ochrona pod warunkiem tkaninie Azure, sam)

Masz dostęp do tych funkcji zabezpieczeń rozszerzonego sieci przy użyciu rozwiązania partnerów Azure. Można znaleźć najbardziej aktualne partnera Azure rozwiązań zabezpieczeń sieci strony [Azure Marketplace](https://azure.microsoft.com/marketplace/) , wyszukując frazę "zabezpieczenia" i "zabezpieczeń sieci".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Bezpieczny dostęp zdalny i łączność między lokalne
Ustawienia konfiguracji i zarządzanie usługi Azure zasobów należy zdalnie. Ponadto możesz zechcieć wdrażanie rozwiązań [hybrydowych IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) , zawierających składniki lokalnej i w chmurze publicznej Azure. Następujące scenariusze wymaga bezpiecznego dostępu zdalnego.

Azure sieci obsługuje następujących scenariuszy bezpieczny dostęp zdalny:

- Łączenie pojedynczych stanowiska pracy z Azure wirtualnej sieci
- Łączenie sieci lokalnej wirtualną sieć Azure z sieci VPN
- Łączenie sieci lokalnej wirtualną sieć Azure za pomocą dedykowanego łącza WAN
- Połączyć Azure wirtualnych sieci ze sobą

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Łączenie pojedynczych stanowiska pracy z Azure wirtualnej sieci
Czasami może wystąpić sytuacja, gdy chcesz włączyć poszczególne operacje lub programista pracownikom zarządzać maszyn wirtualnych i usług w Azure. Na przykład potrzebny jest dostęp do maszyny wirtualnej w wirtualnej sieci Azure i zasady dotyczące zabezpieczeń nie zezwala na RDP lub SSH zdalny dostęp do poszczególnych maszyn wirtualnych. W tym przypadku umożliwia połączenie VPN punktu do witryny.

Połączenie VPN punktu do witryny protokół [SSTP sieci VPN](https://technet.microsoft.com/library/cc731352.aspx) umożliwiają konfigurowanie prywatne i bezpieczne połączenia między użytkownikiem a wirtualną sieć Azure. Po nawiązaniu połączenia VPN użytkownik będzie mógł RDP lub SSH łącze sieci VPN do maszyn wirtualnych w wirtualnej sieci Azure (zakładając, że użytkownik może przeprowadzać uwierzytelnianie i jest autoryzowany).

Dowiedz się więcej:

- [Konfigurowanie połączenia punkt do witryny z wirtualnej sieci przy użyciu programu PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Łączenie sieci lokalnej Azure wirtualnej sieci przy użyciu sieci VPN
Można nawiązać całej sieci firmowej lub części, wirtualną sieć Azure. To jest wspólny dla hybrydowych IT scenariusze miejsce, w którym przedsiębiorstw [rozszerzyć ich centrum danych lokalnych do Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). W większości przypadków firmy będzie przechowywana części usługi Azure i części w wersji lokalnej, takich jak Jeżeli rozwiązanie zawiera serwerach frontonu sieci web w Azure i wewnętrznej bazy danych lokalnych. Ten rodzaj połączenia "lokalnej infrastrukturze" również wprowadzić zarządzania Azure znajduje się zasoby bardziej bezpiecznego i Włącz scenariusze, takie jak rozszerzanie kontrolery domeny usługi Active Directory do Azure.

Jednym ze sposobów w tym celu jest za pomocą sieci [VPN witryny do witryny](https://www.techopedia.com/definition/30747/site-to-site-vpn). Różnica między VPN witryny do witryny i VPN punktu do witryny to, że VPN punktu do witryny łączy jedno urządzenie Azure wirtualną sieć podczas VPN witryny do witryny łączy całej sieci (na przykład sieci lokalnej) Azure wirtualną sieć. Witryny do witryny sieci VPN z siecią wirtualną Azure tryb wysokim poziomie zabezpieczeń IPsec tunelem VPN Protocol (protokół).

Dowiedz się więcej:

- [Tworzenie VNet Menedżera zasobów z połączenie VPN witryny do witryny za pomocą Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planowanie i projektowanie dla bramy sieci VPN](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Łączenie sieci lokalnej Azure wirtualną sieć za pomocą dedykowanego łącza WAN
Połączenia VPN punktu do witryny i witryny do witryny obowiązują umożliwiających łączność między lokalnej. Jednak niektóre organizacje lepiej, aby miał on następujące wady:

- Połączenia VPN przenieść dane przez Internet — to udostępnia następujące połączeń potencjalne problemy związane z przenoszeniem danych przez sieć publiczną. Ponadto nie można zagwarantować niezawodność i dostępność dla połączenia z Internetem.
- Połączenie VPN Azure wirtualnych sieci można uznać, że ograniczenia przepustowości dla niektórych aplikacji i celów, ich maksymalny limit u wokół 200Mbps.

Organizacji, które potrzebują najwyższego poziomu zabezpieczeń i dostępność dla ich połączeń między lokalnej zwykle nawiązywanie połączenia z zdalnego witryn za pomocą dedykowanego łącza WAN. Azure udostępnia możliwość używania dedykowane łącza WAN, które umożliwiają łączenie sieci lokalnej wirtualną sieć Azure. Ta opcja jest włączona przez Azure ExpressRoute.

Dowiedz się więcej:

- [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Połączyć Azure wirtualnych sieci ze sobą
Jest możliwe przy użyciu wielu wirtualnych sieci Azure dla wdrożeń. Istnieje wiele powodów, dlaczego można to zrobić. Przyczyny mogą być w celu uproszczenia zarządzania; inny może być ze względów bezpieczeństwa. Niezależnie od tego, czy motywacji lub uzasadnienie umieszczanie zasobów w innych sieciach wirtualnych Azure może być powinny zasoby na każdej z sieci na łączenie się ze sobą.

Jedną z opcji będzie dla usług w jeden Azure wirtualnej sieci łączenie się z usługami w innej sieci wirtualnej Azure przez "pętli ponownie" przez Internet. Połączenie czy Uruchom w jednym Azure wirtualnej sieci, przejdź za pośrednictwem Internetu, a następnie wróć do miejsca docelowego Azure wirtualną sieć. Ta opcja udostępnia połączenie wbudowane w dowolnej komunikacji internetowych problemów z zabezpieczeniami.

Lepszym rozwiązaniem może być tworzenie Azure wirtualna sieć do Azure wirtualnych sieci witryny do witryny VPN. Ten Azure wirtualna sieć do Azure wirtualnych sieci witryny do witryny VPN używa tego samego protokołu [Tryb tunelem IPsec](https://technet.microsoft.com/library/cc786385.aspx) jako połączenie VPN witryny do witryny lokalnej krzyżowe wymienionych powyżej.

Korzyścią ze stosowania Azure wirtualna sieć do Azure wirtualnych sieci witryny do witryny VPN jest, czy połączenie VPN utworzono na tkaninie Azure sieci; nie połączyć w Internecie. Zapewnia dodatkową warstwę zabezpieczeń w porównaniu z witryny do witryny sieci VPN, łączących się za pośrednictwem Internetu.

Dowiedz się więcej:

- [Konfigurowanie połączenia VNet do VNet za pomocą Menedżera zasobów Azure i programu PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Dostępność
Dostępność to kluczowych dowolnego programu zabezpieczeń. Jeśli użytkowników i systemy nie ma dostępu muszą uzyskiwać dostęp przez sieć, usługę można uznać zostało naruszone. Azure występują technologii sieciowych, które obsługują następujące mechanizmy wysokiej dostępności:

- Równoważenia obciążenia opartego na HTTP
- Równoważenia obciążenia poziomu sieciowego
- Globalne równoważenia obciążenia

Równoważenia obciążenia jest mechanizm przeznaczona do równe rozłożenie powiązania między poszczególnymi wielu urządzeń. Cele równoważenia obciążenia są:

- Zwiększa dostępność — jeśli ładowanie połączeń Saldo na wielu urządzeniach, co najmniej jednym z urządzeń może stać się niedostępne i usług uruchomionych na pozostałych urządzeń online nadal udostępniać zawartość z usługi
- Zwiększanie wydajności — podczas ładowania połączeń Saldo na wielu urządzeniach, jednego urządzenia nie ma podjęcie przewaga procesor. Zamiast tego wymagania przetwarzania i ilości pamięci serwowania zawartość jest rozmieszczony wielu urządzeń.

### <a name="http-based-load-balancing"></a>Równoważenia obciążenia opartego na HTTP
Organizacji, w których uruchomienie usług sieci web często najszybciej mają równoważenia obciążenia oparte na HTTP przed tych usług sieci web, aby ułatwić zapewnienie odpowiedniego poziomu wydajność oraz dostępność. W odróżnieniu od urządzenia do równoważenia obciążenia sieci tradycyjny decyzji oparte na HTTP równoważenia obciążenia urządzenia do równoważenia obciążenia są oparte na właściwości protokołu HTTP, a nie na protokoły warstwy sieci i transportu.

Aby zapewnić oparte na HTTP równoważenia obciążenia dla usług sieci web, Azure dostarcza bramie aplikacji Azure. Obsługa Azure bramy aplikacji:

- Oparte na HTTP równoważenia obciążenia — decyzji równoważenia obciążenia są wykonywane na podstawie cechy specjalne protokołu HTTP
- Koligacja sesji plików cookie — służy do sprawdzenia, czy połączeń ustanowionych z jednym z serwerów za tym równoważenia obciążenia pozostaje bez zmian między klientem a serwerem tej funkcji. To uzyskanie stabilność transakcji.
- Odciążanie SSL — po klienta połączenie jest nawiązywane z równoważenia obciążenia, że sesji między klientem a równoważenia obciążenia jest zaszyfrowany przy użyciu protokołu HTTPS (SSL /) Protocol (protokół). Aby zwiększyć wydajność, możesz jednak opcja połączenia między równoważenia obciążenia i serwerem sieci web za pomocą równoważenia obciążenia protokołu HTTP (bez szyfrowania). To jest nazywany "SSL offload" ponieważ serwerów sieci web za usługi równoważenia obciążenia nie daje związane z szyfrowania obciążenie procesora i w związku z tym powinno być możliwe żądania obsługi szybciej.
- Adresy URL rozsyłanie zawartości — ta funkcja umożliwia równoważenia obciążenia przy podejmowaniu decyzji umożliwiające przekazywanie połączeń na podstawie adresu URL docelowej. Zapewnia większą elastyczność niż rozwiązań składających ładowanie równoważenia decyzji na podstawie adresów IP.

Dowiedz się więcej:

- [Omówienie aplikacji bramy](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Równoważenia obciążenia poziomu sieciowego
W odróżnieniu od równoważenia obciążenia oparte na HTTP równoważenia obciążenia poziomu sieciowego sprawia, że decyzje równoważenia obciążenia na podstawie IP adres i port (port TCP lub UDP).
Aby uzyskać korzyści wynikających z poziomu równoważenia obciążenia sieciowego w Azure za pomocą usługi równoważenia obciążenia Azure. Niektóre główne cechy równoważenia obciążenia Azure obejmują:

- Równoważenia obciążenia poziomu sieciowego według numerów portu i adresem IP
- Obsługa protokołu warstwy aplikacji
- Ładowanie sald do Azure maszyn wirtualnych i chmury usługi roli wystąpienia
- Można używać zarówno w Internecie (równoważenia obciążenia zewnętrznych), jak i w innych niż Internet przeciwległych maszyn wirtualnych i aplikacji (równoważenia obciążenia wewnętrznego)
- Punkt końcowy monitorowania, która jest używana do określenia usług za usługi równoważenia obciążenia mają stają się niedostępne

Dowiedz się więcej:

- [Internet przeciwległych równoważenia obciążenia między wiele maszyn wirtualnych lub usług](../load-balancer/load-balancer-internet-overview.md)
- [Omówienie równoważenia obciążenia wewnętrznych](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globalne równoważenia obciążenia
Niektóre organizacje będzie możliwe najwyższego poziomu dostępności. Jeden sposób, aby osiągnąć ten cel jest obsługi aplikacji w globalnie rozłożone centrach danych. Kiedy aplikacja znajduje się w centrach danych znajdujących się na całym świecie, jest możliwe dla całego regionu geopolitycznych stają się niedostępne i nadal masz aplikacji w górę i żywa.

Oprócz zalety dostępność Ci się hostingu aplikacji w globalnie rozłożone centrach danych możesz również uzyskać wydajności. Następujące korzyści wydajności można uzyskać przy użyciu mechanizmu, który kieruje żądania usługi z centrum danych, który znajduje się najbliżej urządzenia, na którym jest zgłaszającego żądanie.

Równoważenia obciążenia globalnej może dostarczyć obie te korzyści. Platformy Azure można uzyskać zalety globalnej równoważenia obciążenia za pomocą Menedżera ruch Azure.

Dowiedz się więcej:

- [Co to jest Menedżer ruchu?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Rejestrowanie
Rejestrowanie na poziomie sieci jest funkcją klucza w dowolnym scenariuszu zabezpieczeń sieci. Platformy Azure można rejestrować informacji uzyskanych dla grup zabezpieczeń sieci uzyskanie sieci poziom rejestrowania informacji. Z rejestrowaniem NSG spowoduje wyświetlenie informacji:

- Dzienniki inspekcji — Dzienniki te służą do wyświetlania wszystkie operacje przesłane do subskrypcji Azure. Dzienniki te są domyślnie włączone i mogą być używane w portalu Azure.
- Dzienniki zdarzeń — te dzienniki zawierają informacje o jakie zasady NSG zostały zastosowane.
- Licznik dzienniki — Dzienniki te pozwalają wiedzieć, ile razy każda reguła NSG została zastosowana do odmówić lub zezwalanie na ruch.

Za pomocą [Usługi Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), narzędziu wizualizacji danych zaawansowanych do wyświetlania i analizowania dzienników.

Dowiedz się więcej:

- [Dziennik analizy grup zabezpieczeń sieci (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Rozpoznawanie nazw
Rozpoznawanie nazw jest funkcją krytyczne dla wszystkich usług, które hosta platformy Azure. Z perspektywy zabezpieczeń złamania funkcji rozpoznawania nazw może prowadzić do Intruz przekierowuje żądania z witryny do witryny. Rozpoznawanie nazw bezpiecznego jest wymagane dla usług chmury hostowana.

Istnieją dwa rodzaje rozpoznawanie nazw, które należy adres:

- Rozpoznawanie nazw wewnętrznych — rozpoznawanie nazw wewnętrznych jest używane przez usługi na wirtualnych sieci Azure i/lub sieci lokalnej. Nazwy używane do rozpoznawania nazw wewnętrznych nie są dostępne w Internecie. Zapewnia optymalne zabezpieczenia ważne jest schematem rozdzielczość wewnętrzna nazwa nie jest dostępne dla użytkowników zewnętrznych.
- Rozpoznawanie nazw zewnętrznych — zewnętrznych funkcji rozpoznawania nazw jest używany przez osoby i urządzenia poza lokalnego i usługi Azure wirtualnych sieci. Są to nazwy, które są widoczne w Internecie i są używane do bezpośredniego połączenia z usługi opartej na chmurze.

Rozpoznawanie nazw wewnętrznych masz dwie opcje:

- Serwer Azure wirtualna sieć DNS — podczas tworzenia nowego Azure wirtualnej sieci, serwer DNS jest tworzony. Ten serwer DNS może rozpoznawania nazw komputerów znajdujących się w tym Azure wirtualnej sieci. Ten serwer DNS nie jest konfigurowany i jest zarządzane przez Menedżera tkaninie Azure, więc co rozwiązanie rozdzielczość bezpiecznego nazwę.
- Przesuń do serwera DNS — istnieje możliwość wprowadzenia serwera DNS wybranej przez użytkownika w Twojej sieci wirtualnej Azure. Ten serwer DNS może być usługi Active Directory zintegrowanego serwera DNS lub dedykowanego rozwiązania serwera DNS dostarczony przez partnera Azure korzystających z usługi Azure Marketplace.

Dowiedz się więcej:

- [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)
- [Zarządzanie serwery DNS używane przez sieć wirtualną (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Dla zewnętrznych rozpoznawania nazw DNS masz dwie opcje:

- Udostępniać własne zewnętrznych DNS lokalnego serwera
- Udostępniać własnego zewnętrznego serwera DNS u dostawcy usługi

Wiele dużych organizacjach będzie udostępniać swoje własne DNS servers lokalnego. Ponieważ specjalizacji sieci i globalnej informacji o obecności w tym celu ich to zrobić.

W większości przypadków lepiej jest do obsługi rozpoznawanie nazw DNS u dostawcy usługi. Ci dostawcy usług mają specjalizacji sieci i globalnej obecności zapewnienie bardzo wysoki dostępność dla sieci usługi rozpoznawania nazw. Dostępność jest istotne dla usługi DNS, ponieważ jeśli Twojej usługi rozpoznawania nazw nie, nikt nie będzie dostęp do Internetu przeciwległych usług.

Azure umożliwia wysokiej dostępności i performant zewnętrznych DNS rozwiązanie w postaci Azure DNS. To rozwiązanie rozpoznawanie nazw zewnętrznych wykorzystuje infrastruktury Azure DNS na całym świecie. Umożliwia udostępniać swojej domeny Azure za pomocą tych samych poświadczeń, interfejsów API, narzędzi i rozliczeń jako innych usług Azure. W ramach Azure również dziedziczy kontrolek silne zabezpieczenia wbudowane platformy.

Dowiedz się więcej:

- [Omówienie Azure DNS](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>Architektura strefy Zdemilitaryzowanej
Wiele organizacji przedsiębiorstwa umożliwia DMZs segmentu ich sieci, aby utworzyć strefę buforu między Internetu i ich usług. Strefy Zdemilitaryzowanej część sieci jest traktowany jako strefy zabezpieczeń min. i elementy zawartości nie wysokiej wartości są umieszczane w tej części sieci. Pojawi się zwykle urządzenia ochrony sieci, mające na części strefy Zdemilitaryzowanej i innej sieci interfejsem połączenie z siecią, zawierającą maszyn wirtualnych i usług, które Zaakceptuj połączenia przychodzące z Internetu.

Istnieje kilka zmian strefy Zdemilitaryzowanej projektu i decyzji o wdrożeniu strefy Zdemilitaryzowanej i następnie typu strefy Zdemilitaryzowanej, jeśli zdecydujesz się korzystać, jest oparta na określonych wymagań dotyczących zabezpieczeń sieci.

Dowiedz się więcej:

- [Zabezpieczenia usług chmury firmy Microsoft i sieci](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Centrum zabezpieczeń Azure
Centrum zabezpieczeń ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia i zapewnia zwiększone wgląd i kontrolować, zabezpieczenia Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

Centrum zabezpieczeń Azure pomaga zoptymalizować i monitorowanie zabezpieczeń sieci przez:

- Dostarczanie zalecenia dotyczące zabezpieczeń sieci
- Monitorowanie stanu konfiguracji zabezpieczeń sieci
- Alerty o sieciowe zagrożeń zarówno na poziomie punktu końcowego i sieci

Dowiedz się więcej:

- [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-intro.md)
