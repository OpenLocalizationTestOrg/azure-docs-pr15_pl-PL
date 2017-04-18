<properties
   pageTitle="Microsoft cloud services i zabezpieczeniami sieci | Microsoft Azure"
   description="Dowiedz się, niektóre z najważniejszych funkcji dostępnych w Azure ułatwia utworzenie bezpiecznych środowiskach sieci"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Zabezpieczeń usług i sieci chmury firmy Microsoft

Usług w chmurze firmy Microsoft dostarczanie usług informatycznych i infrastruktury, możliwości klasy korporacyjnej i wiele opcji łączności hybrydowego. Klientów można uzyskać do nich dostępu za pośrednictwem Internetu lub z Azure ExpressRoute, która udostępnia łączności sieci prywatnej. Platforma Microsoft Azure umożliwia bezproblemowe rozszerzona infrastruktury chmury i tworzenie wielowarstwowa architektury. Ponadto trzecich można włączyć udoskonalone funkcje oferując usług zabezpieczeń i wyposażenia wirtualną. Ten dokument zawiera omówienie bezpieczeństwa i architektury problemy, które klientów należy rozważyć podczas korzystania z usług w chmurze firmy Microsoft dostępna za pośrednictwem ExpressRoute. Obejmuje także, tworzenia zabezpieczyć usług w Azure wirtualnych sieci.

## <a name="fast-start"></a>Szybki start
Na poniższym wykresie logiczny można kierować do określonych przykład wiele technik zabezpieczeń dostępne z platformy Azure. Podręczna karta informacyjna można znaleźć w przykładzie najlepiej sprawy. Dokładniejszy objaśnienia kontynuuj czytanie za pośrednictwem papieru.
![Schemat blokowy opcje zabezpieczeń][0]

[Przykład 1: Tworzenie sieci obwód kształtu (nazywane także strefy Zdemilitaryzowanej, strefa zdemilitaryzowana i podsiecią) chronić aplikacje z grupami zabezpieczeń sieci (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Przykład 2: Tworzenie sieci obwodu chronić aplikacje z zapory i NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Przykład 3: Tworzenie sieci obwodu chronić sieci z zapory, zdefiniowane przez użytkownika rozsyłania (UDR) i NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Przykład 4: Dodawanie połączenia hybrydowego z witryny do witryny, wirtualne urządzenia wirtualną sieć prywatną (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Przykład 5: Dodaj połączenie hybrydowego z bramą witryny do witryny, Azure VPN.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Przykład 6: Dodaj połączenie hybrydowego z ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Przykłady dodawanie połączeń między wirtualnych sieci, wysokiej dostępności i łączenia usługi zostaną dodane do tego dokumentu w kilku kolejnych miesięcy.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Ochrona infrastruktury i zgodność z firmy Microsoft
Microsoft przyjęła pozycji Zarządzanie pomocniczych inicjatywy zgodności wymagane przez przedsiębiorstwa. Poniżej wymieniono niektóre certyfikaty zgodności dla Azure: ![znaczki Azure zgodności][1]

Aby uzyskać więcej informacji zobacz informacje o zgodności w [Centrum zaufania programu Microsoft](https://azure.microsoft.com/support/trust-center/compliance/).

Firma Microsoft udostępnia pełna sposobem ochrony infrastruktury chmury niezbędnymi do przeprowadzania globalnych usług informatycznych. Infrastruktury chmury firmy Microsoft zawiera sprzętu, oprogramowania, sieci i administracyjne i operatorów, oprócz fizycznie centrach danych.

![Funkcje zabezpieczeń Azure][2]

Ta metoda stanowi zabezpieczyć podstawę dla klientów wdrożyć ich usługi w chmurze firmy Microsoft. Następnym krokiem jest klienci do projektowania i utworzyć Architektura zabezpieczeń ochrony tych usług.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Architektura zabezpieczeń tradycyjny i sieci obwodowych
Mimo że Microsoft intensywnie inwestuje w ochronie infrastruktury chmury, klienci muszą także chronić ich usług w chmurze i grup zasobów. Wielowarstwowego podejście do zabezpieczeń zawiera najlepsze obrony. Strefy zabezpieczeń sieci obwód kształtu chroni zasoby sieci wewnętrznej z niezaufanych sieci. Sieć obwód kształtu odwołuje się do krawędzi lub części sieci, które znajdują się między Internetu i chronionych przedsiębiorstwa infrastrukturę informatyczną.

W sieciach typowe firmy infrastruktury core jest intensywnie wzmocniony na okręgów z kilka warstw zabezpieczeń urządzeń. Obramowanie każdej warstwy, składa się z urządzenia i punkty wymuszania zasad. Urządzenia mogą obejmować następujące czynności: zapory, zapobiegania Distributed odmowa usługi (DDoS) wykrywanie intruzów lub systemy ochrony (nazwy i adresy IP) i urządzeń sieci VPN. Wymuszania zasad można formę zasady zapory, listy kontroli dostępu (ACL) lub określonej routing. W pierwszym wierszu obrony w sieci, bezpośrednio akceptowania ruch przychodzący z Internetu, to kombinacja mechanizmy na ataki blok i szkodliwych ruch umożliwienie wiarygodnych żądania dalej do sieci. Kieruje ruch bezpośrednio do zasobów w sieci obwód kształtu. Ten zasób może następnie "skontaktować się" zasoby szczegółowego w sieci, tranzyt krawędzi następnego sprawdzania poprawności pierwszego. Zewnętrzne warstwy jest nazywany usługodawcy, ponieważ ta część sieci jest udostępniana w Internecie, zwykle za pomocą niektórych formy ochrony na obu stronach arkusza. Na poniższej ilustracji pokazano przykład sieci obwód kształtu jednej podsieci w sieci firmowej, z dwiema granicami zabezpieczeń.

![Sieć obwód kształtu w sieci firmowej][3]

Istnieje wiele architektury używanych do implementowania obwód kształtu sieci, od równoważenia obciążenia prosty przed farmy sieci web do wielu sieci obwód kształtu podsieci za pomocą różnej mechanizmy w każdej obramowanie blokowania ruchu i ochrona szczegółowego warstwy sieci firmowej. Jak usługodawcy jest wbudowany w zależności od określonych potrzeb organizacji i jej ogólną uszkodzenia ryzyka.

Jak klienci przenieść ich obciążenie pracą chmury publicznej, niezwykle ważne jest obsługę podobne funkcje architektury sieci obwód kształtu w Azure, aby spełnić wymagania dotyczące zabezpieczeń i zgodność. Ten dokument zawiera wskazówki dotyczące jak klienci mogą tworzyć środowiska sieciowego bezpiecznej platformy Azure. Omówiono w sieci obwód kształtu, ale także kompleksowe omówienie wielu aspektów zabezpieczeń sieci. Poniższe pytania o tej dyskusji:

- Jak można utworzyć sieć obwód kształtu w Azure
- Jakie są niektóre z funkcji Azure dostępnych do utworzenia usługodawcy?
- Jak być chronione obciążenia wewnętrznej
- Jak są kontrolowane obciążenie pracą w Azure komunikacja w Internecie?
- Jak sieci lokalnej mogą być chronione przed wdrażania Azure?
- Gdy funkcje zabezpieczeń natywnych Azure powinny być używane i urządzeń innych firm lub usług?

Na poniższym diagramie pokazano różne warstwy zabezpieczeń, który Azure udostępnia klientom. Te warstwy są zarówno macierzystego na platformie Azure się, jak i funkcje zdefiniowane przez klientów:

![Architektura zabezpieczeń Azure][4]

Ruch przychodzący z Internetu DDoS Azure ułatwia ochronę przed dużych atakami Azure. Następny warstwa jest zdefiniowane przez klientów publicznej punkty końcowe, które służą do określenia, jaki ruch można przechodzić do wirtualnej sieci usługi w chmurze. Natywne Azure wirtualna sieć izolacji zapewnia pełnej izolacji od innych sieci i że ruch przepływał tylko za pośrednictwem ścieżki użytkownika skonfigurowane i metod. Te ścieżki i metody są następnej warstwy, miejsce, w którym NSGs, UDR i wyposażenia wirtualnych sieci może służyć do tworzenia granic zabezpieczeń ochrony wdrażaniem aplikacji z chronionej sieci.

Następnej sekcji omówiono Azure wirtualnych sieci. Te wirtualnych sieci są tworzone przez klientów i są ich wdrożonej obciążenie pracą podłączonych do. Wirtualnych sieci są podstawą wszystkich funkcji zabezpieczeń sieci wymagane ustanowienie sieci obwód kształtu ochrony wdrażania klienta Azure.

## <a name="overview-of-azure-virtual-networks"></a>Omówienie Azure wirtualnych sieci
Przed ruchu internetowego można przejść do Azure wirtualnych sieci, dostępne są dwie warstwy zabezpieczeń związanych z platformy Azure:

1.  **Ochrona DDoS**: ochrona DDoS jest warstwy Azure sieci fizycznej chroniącego platformie Azure samej atakami dużych internetowych. Ataki umożliwiają wiele węzłów "robot" próba zasypać internetowej usługi. Azure ma niezawodne funkcje ochrony DDoS siatki na wszystkie przychodzące połączenie z Internetem. Tej warstwy ochrony DDoS ma nie konfigurować atrybuty użytkownika, a nie jest dostępny dla klientów. Chroni Azure jako platformy atakami na dużą skalę, ale nie chroni bezpośrednio poszczególnych klientów aplikacji. Dodatkowe warstwy odporność można skonfigurować przez klienta przed atakiem zlokalizowanym. Na przykład jeśli klienta A został zaatakowany z dużą skalę takiego ataku na publicznej punktu końcowego, Azure blokuje połączenia z tej usługi. Klient A może nie przez innego wirtualną sieć lub usługi punktu końcowego nie związane z atakiem przywrócenia usługi. Należy zauważyć, że chociaż klienta A mogą mieć wpływ na tego punktu końcowego, żadnych innych usług poza tego punktu końcowego będzie miało wpływu. Ponadto innych klientów i usług, zobaczy nie wpływu na tym atakiem.
2.  **Punkty końcowe usługi**: punkty końcowe Zezwalaj na chmurze usług lub zasobów grupy publicznej adresy Internet IP i portów. Punkt końcowy użyje tłumaczenia adresów sieciowych (NAT), aby skierować ruch do wewnętrznego adresu i portu w Azure wirtualnej sieci. To jest podstawowy ścieżki ruchu zewnętrznego do przekazania do wirtualną sieć. Usługa punkty końcowe są użytkownika można konfigurować w celu ustalenia jaki ruch przekazywana w i jak i gdzie go jest przetłumaczony w wirtualnej sieci.

Gdy ruch osiągnie wirtualną sieć, istnieje wiele funkcji, które są dostarczane do odtwarzania. Azure wirtualnych sieci są podstawą dla klientów dołączyć ich obciążenie pracą i miejsce, w którym ma zastosowanie podstawowe zabezpieczeń na poziomie sieci. Sieć prywatną (wirtualną sieć w trybie nakładania) jest platformy Azure dla klientów z następujące funkcje i właściwości:
 
- **Ruch izolacji**: wirtualnej sieci jest granicę izolacji ruch na platformie Azure. Wirtualna maszyn w jednej wirtualnej sieci nie możesz komunikować się bezpośrednio do maszyny wirtualne w innej sieci wirtualnej, nawet jeśli oba wirtualnych sieci są tworzone przez tego samego klienta. To jest właściwością krytyczne, która zapewnia maszyny wirtualne klienta i komunikacja pozostanie w wirtualnej sieci prywatnej.
- **Topologia multitier**: wirtualnych sieci użytkownicy zdefiniować topologii multitier przydzielając podsieci i wyznaczanie spacje osobny adres dla różnych elementów lub "warstw" ich obciążenie pracą. Te logiczne grupowanie i topologii umożliwiają klientom opracowanie zasad innego programu access na podstawie typów obciążenie pracą, a także kontrolowanie przepływu ruchu między poziomów.
- **Łączność między lokalnej**: klientów można ustalić łączność między lokalnej wirtualną sieć i wielu witrynach lokalnego lub innych wirtualnych sieci w Azure. Aby to zrobić, klienci mogli używać bram VPN Azure lub wyposażenia wirtualnej sieci innej firmy. Azure obsługuje VPN (S2S) witryny do witryny za pomocą standardowych protokołów IPsec/IKE i ExpressRoute prywatne łączności.
- **NSG** umożliwia tworzenie reguł (ACL) na żądanym poziomie szczegółowości: sieci interfejsów, pojedyncze maszyny wirtualne lub wirtualna podsieci. Kontrolowanie dostępu, zależnie od dostępnego lub odmawianie komunikacji między obciążenie pracą w sieci wirtualnej, z systemów sieci klienta za pośrednictwem połączenia między lokalnego lub bezpośredni komunikacji internetowej klientów.
- **UDR** i **Przekazywanie adresów IP** umożliwiają klientom Definiowanie ścieżek komunikacji między różnych poziomów w wirtualnej sieci. Klientów można wdrożyć zaporę, nazwy i adresy IP i innych urządzeń wirtualnych i ruch sieciowy jest rozsyłana za pośrednictwem tych urządzeń zabezpieczeń dla wymuszania zasad zabezpieczeń obramowanie, inspekcji i kontroli.
- **Urządzenia wirtualną sieć** w Azure Marketplace: zabezpieczeń urządzeń, takich jak zapory, urządzenia do równoważenia obciążenia i nazwy i adresy IP są dostępne w Azure Marketplace i Galeria obrazów maszyn wirtualnych. Aby ukończyć środowisku bezpiecznej sieci rozwiązania wdrażania tych urządzeń do ich wirtualnych sieci, a w szczególności na ich granice zabezpieczeń (w tym podsieci obwód kształtu).

Z tych funkcji i możliwości przykładem jak architektury sieci obwód kształtu może być skonstruowane w Azure jest następująca:

![Sieć obwód kształtu w Azure wirtualnej sieci][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Właściwości sieci obwód kształtu i wymagania
Usługodawcy jest zaprojektowane jako fronton sieci, bezpośrednio relacje komunikacji z Internetu. Pakiety przychodzące powinny przepływał przez urządzeń zabezpieczeń, takich jak zapory, nazwy i adresy IP, przed osiągnięciem serwery wewnętrznej. Pakiety Internet związanych z obciążeń pracą można również przepływał przez urządzenia zabezpieczeń u usługodawcy do wymuszania zasad, inspekcji i potrzeby inspekcji opuszczeniu sieci. Ponadto usługodawcy może obsługiwać bramy sieci VPN lokalnej krzyżowe między wirtualnych sieci klienta i sieci lokalnej.

### <a name="perimeter-network-characteristics"></a>Właściwości sieci obwód kształtu
Odwoływanie się do powyższej ilustracji, niektóre właściwości dobrej usługodawcy są następujące:

- Z Internetu:
    - Obwód kształtu Sieć podsieć się z Internetem, bezpośrednio komunikowania się z Internetem.
    - Publiczne adresy IP, służącą i/lub punktów końcowych usługi przekazywać ruch internetowy do frontonu sieci i urządzeń.
    - Ruch przychodzący z Internetu przechodzi zabezpieczeń urządzeń przed innymi zasobami sieci zewnętrznej.
    - Po włączeniu Zabezpieczenia wychodzących przechodzi ruch zabezpieczeń urządzeń, co stanowi ostatni krok przed przekazaniem do Internetu.
- Chronionej sieci:
    - Istnieje bezpośredniej ścieżki z Internetu infrastruktury core.
    - Kanały do infrastruktury core muszą przejść przy użyciu urządzeń zabezpieczeń, takich jak NSGs, zapory lub urządzenia VPN.
    - Inne urządzenia nie musi Mostek internetowe i infrastruktury core.
    - Urządzenia zabezpieczeń zarówno z Internetu i sieci chronionej przeciwległych granice usługodawcy (na przykład dwóch zapory ikony pokazano w powyższej ilustracji) faktycznie może być pojedynczy urządzenia wirtualne przy użyciu reguł zróżnicowane lub interfejsów dla każdej krawędzi. (Oznacza to, że jedno urządzenie, w logiczny sposób rozdzielone, obsługa ładowania dla obu ograniczenia usługodawcy).
- Inne typowe rozwiązania i ograniczenia:
    - Obciążenia nie należy przechowywać krytyczne informacje biznesowe.
    - Program Access i aktualizacje konfiguracji sieci obwód kształtu i wdrożenia są ograniczone do tylko autoryzowani administratorzy.

### <a name="perimeter-network-requirements"></a>Wymagania sieci obwód kształtu
Aby włączyć te właściwości, przestrzegaj następujących wytycznych na wymagania wirtualną sieć do zaimplementowania sieci pomyślnego obwód kształtu:

- **Architektury podsieci:** Określ wirtualną sieć tak, aby całą podsieć jest przeznaczona jako usługodawcy oddzielone od innych podsieci w tej samej sieci wirtualnej. Dzięki temu komunikacji między usługodawcy i innych przepływów poziomów podsieci wewnętrznego lub prywatnego przez zaporę lub urządzenia wirtualnego nazwy i adresy IP w granicach podsieci trasy zdefiniowane przez użytkownika.
- **NSG:** Obwód kształtu Sieć podsieć powinien być otwarty, aby zezwolić na komunikację z Internetem, ale nie oznacza że klientów należy pominięcie NSGs. Wykonaj typowe wskazówki dotyczące zabezpieczeń w celu zminimalizowania powierzchni sieci udostępniana w Internecie. Zablokowanie zakresy adresów zdalny może uzyskać dostęp do wdrożenia lub określonej aplikacji protokoły i porty, które są otwarte. Czasami może występować okoliczności, w których to nie zawsze jest możliwe. Na przykład jeśli klienci zewnętrznej witryny sieci Web w Azure, usługodawcy pozwolić przychodzących żądań sieci web z publicznych adresów IP, ale należy otwierać tylko porty aplikacji sieci web: TCP:80 i TCP:443.
- **Tabeli routingu:** Obwód kształtu Sieć podsieć powinny mieć możliwość komunikowania się z Internetem bezpośrednio, ale nie pozwolić bezpośredniej komunikacji do i z powrotem sieci end lub lokalnego bez pośrednictwa zapory lub zabezpieczeń urządzenia.
- **Konfiguracja urządzenia zabezpieczeń:** Aby skierować i Przeprowadź inspekcję pakietów między usługodawcy i pozostałe chronionej sieci, urządzenia zabezpieczeń, takich jak zapory, nazwy i adresy IP urządzeń może być wieloadresowego. Mogą zawierać osobnych nic dla sieci obwód kształtu i podsieci wewnętrznej. Nic u usługodawcy komunikować się bezpośrednio do i z Internetu, przy użyciu odpowiedniego NSGs i obwód kształtu tabeli routingu sieci. Nawiązywanie połączenia z podsieci wewnętrznej sieciowe więcej ograniczyła NSGs i tabele routingu odpowiednich podsieci wewnętrznej.
- **Funkcji zabezpieczeń urządzenia:** Urządzenia zabezpieczeń wdrożony u usługodawcy zazwyczaj należy wykonać następujące funkcje:
    - Zapora: Wymuszanie reguły zapory lub zasad kontroli dostępu dla przychodzących żądań.
    - Zagrożenie wykrywanie i zapobieganie: wykrywanie i łagodzenia atakami z Internetu.
    - Inspekcji i rejestrowania: utrzymywanie szczegółowych dzienników inspekcji i analizy.
    - Odwracanie serwera proxy: przekierowywanie przychodzących żądań do odpowiednich serwerów wewnętrznej. To dotyczy mapowania i zwykle tłumaczenia docelowych adresów na urządzeniach zewnętrzną, zapory na adresy serwerów wewnętrznej.
    - Przesyłanie dalej serwera proxy: dostarczanie translatora adresów Sieciowych i przeprowadzanie inspekcji dla komunikacji zainicjowane z poziomu wirtualnej sieci z Internetem.
    - Router: Przesyłania ruchu przychodzącego i podsieci krzyżowe wewnątrz wirtualnej sieci.
    - Urządzenia VPN: działającego jako bram VPN krzyżowe lokalnej dla lokalnego krzyżowe VPN łączność między sieci lokalnej klienta i Azure wirtualnych sieci.
    - Serwer sieci VPN: akceptowanie klientów sieci VPN nawiązywanie Azure wirtualnych sieci.

>[AZURE.TIP] Zachowaj następujące grupy dwóch oddzielnych: osób autoryzowanych dostęp do narzędzi zabezpieczeń sieci obwód kształtu i osób autoryzowanych jako administratorzy opracowywania, wdrażania i operacje aplikacji. Zachowanie osobnych tych grup umożliwia podział obowiązków i zapobiega jedna osoba pomijanie aplikacji zabezpieczeń i kontroli zabezpieczeń sieci.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Pytania do monit podczas tworzenia granice sieci
W tej sekcji chyba że wyraźnie wymienionych terminów "sieci" odwołuje się do Azure wirtualnych sieci prywatnych utworzone przez administratora subskrypcji. Termin nie odwołuje się do podstawowej sieci fizycznie w obrębie Azure.

Ponadto Azure wirtualnych sieci są często używane do rozszerzanie sieci lokalnej tradycyjny. Istnieje możliwość włączenia witryny do witryny lub ExpressRoute rozwiązań sieciowych hybrydowego z architektury sieci obwód kształtu. To jest ważne w budynku granice zabezpieczeń sieci.

Następujące trzy pytania są krytyczne odpowiedzieć, gdy tworzysz sieci z siecią obwód kształtu i wielu granice zabezpieczeń.

#### <a name="1-how-many-boundaries-are-needed"></a>1) liczbę granic są potrzebne?
Pierwszy punktów decyzyjnych jest zdecydować, ile granice zabezpieczeń są wymagane w danej sytuacji:

- Jedyną granicę: w sieci zewnętrznej obwód kształtu między wirtualną sieć i Internet.
- Dwiema granicami: jednego kształtu na stronie internetowej usługodawcy, a druga między podsieci obwód kształtu i podsieci wewnętrznej Azure wirtualnych sieci.
- Trzy ograniczenia: na stronie internetowej usługodawcy, po jednej między usługodawcy i wewnętrznej podsieci, a jeden między podsieci wewnętrznej i sieci lokalnej.
- Ograniczenia N: Numer zmiennej. W zależności od wymagań dotyczących zabezpieczeń jest naprawdę dowolną liczbę granic zabezpieczeń, które mogą być stosowane w danej sieci.

Liczbę i typ ograniczenia wymagane są zależne firmy ryzyka uszkodzenia i podanego scenariusza realizowane. Często jest wspólną decyzję wprowadzonymi przez wielu grup w organizacji, często tym ryzyka i zgodność zespołu, sieci i platformy zespołu i do zespołu opracowującego aplikacji. Osobom znajomości zabezpieczeń, dane związane i technologie używane powinien mieć powiedz w niniejszej decyzji zapewnienie stanowisko zabezpieczeń odpowiednich dla każdego implementacji.

>[AZURE.TIP] Za pomocą najmniejszą liczbę granic, które spełniają wymagania dotyczące zabezpieczeń dla danej sytuacji. Przy użyciu więcej granice trudniejsze operacje i rozwiązywanie problemów może być, a także zarządzania kłopotów związanych z zarządzaniem wieloma zasadami obramowanie w czasie. Jednak za mało ograniczenia zwiększać ryzyko. Niezwykle ważne jest znajdowanie saldo.

![Sieć hybrydowego z trzech granice zabezpieczeń][6]

Na powyższej ilustracji pokazano szczegółowego widoku sieci obramowanie trzy zabezpieczeń. Granice są między usługodawcy i Internet, Azure frontonu i wewnętrznej prywatne podsieci, a Azure podsieci wewnętrznej sieci firmowej lokalnego.

#### <a name="2-where-are-the-boundaries-located"></a>(2) gdzie znajdują się granice
Liczba granic są decyzję, gdzie można je zastosować po następnym podjęcie decyzji. Zazwyczaj są trzy opcje:
- Za pomocą internetowych pośredniczącym usługi (na przykład opartej na chmurze Web aplikacji zapora, który nie został omówiony w tym dokumencie)
- Za pomocą funkcji natywne i/lub urządzeń wirtualnych sieci platformy Azure
- Przy użyciu urządzeń fizycznych sieci lokalnej

Sieci czysto Azure dostępne są następujące opcje: natywne Azure funkcje (na przykład Azure urządzenia do równoważenia obciążenia) lub sieci wirtualnej urządzeniach z ekosystemie sformatowanego partnera Azure (na przykład zapory punkt wyboru).

Jeśli obramowanie jest potrzebne między Azure i sieci lokalnej, urządzenia zabezpieczeń mogą znajdować się po obu stronach połączenia (lub obie strony). Dlatego decyzję należy w lokalizacji, aby umieścić koła zębatego zabezpieczeń.

W poprzednim rysunku sieci Internet do obwód kształtu i ograniczenia przodu do tyłu zakończenia całkowicie są zawarte w Azure i musi być naturalne funkcje Azure lub sieci wirtualnej urządzenia. Na stronie Azure lub stronie lokalnego lub nawet w połączeniu z urządzenia po obu stronach, być może zabezpieczeń urządzeń na granicy między Azure (wewnętrznej podsieć) i sieci firmowej. Może to być znaczną wady i zalety jedną z tych opcji, które należy rozważyć poważnie.

Na przykład przy użyciu istniejącego sprzętu fizycznego zabezpieczenia na stronie sieci lokalnej przewaga że żadnych nowych narzędzi jest potrzebny. Wystarczy zmiany konfiguracji. Wadą, jest jednak, że cały ruch pochodzą ponownie Azure do sieci lokalnej było widoczne koła zębatego zabezpieczeń. Dlatego ruch Azure do Azure może ponoszenia istotne opóźnienie i wpływają na wydajność aplikacji i użytkownika wystąpić, jeśli zostało wymuszone powrót do sieci lokalnej wymuszania zasad zabezpieczeń.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) są granice implementacji?
Każdy granicę zabezpieczeń najprawdopodobniej będzie musiał wymagania różne możliwości (na przykład Identyfikatory i reguły zapory po stronie internetowej usługodawcy, ale tylko ACL między usługodawcy i podsieci wewnętrznej). Określanie, które urządzenia należy użyć, zależy od wymagań scenariusz i zabezpieczenia. W poniższej sekcji Przykłady 1, 2 i 3 omówiono niektóre opcje, które mogą zostać użyte. Przeglądanie funkcji Azure natywnych sieci i urządzeń w Azure udostępniła ekosystemie partnera pokazuje opcje tysięcy do rozwiązania praktycznie dowolnych scenariusza.

Podjęcie decyzji, czy inny implementacji klucza jest sposób nawiązywania połączenia z siecią w lokalnej z Azure. Należy używać Azure bramy wirtualnych lub urządzenia wirtualnych sieci? Te opcje zostały omówione bardziej szczegółowo w następnej sekcji (przykłady 4, 5 i 6).

Ponadto ruch między wirtualnej sieci w obrębie Azure mogą być potrzebne. Następujące scenariusze zostanie dodany w późniejszym terminie.

Znając odpowiedzi na pytania poprzedni sekcji [Szybkiego startu](#fast-start) może pomóc w określenia, które przykłady są najbardziej odpowiednie dla danego scenariusza.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Przykłady: Tworzenie granice zabezpieczeń z Azure wirtualnych sieci
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Przykład 1: Tworzenie sieci obwodu chronić aplikacji za pomocą NSGs
[Powrót do początku Fast](#fast-start) | [szczegółowy Tworzenie instrukcji w tym przykładzie][Example1]

![Sieci przychodzącego obwód kształtu z NSG][7]

#### <a name="environment-description"></a>Opis środowiska
W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- Dwa usług w chmurze: "FrontEnd001" i "BackEnd001"
- Sieci wirtualnej, "CorpNetwork" z dwóch podsieci: "FrontEnd" i "Wewnętrznej bazy danych"
- Grupy zabezpieczeń sieci zastosowanego do obu podsieci
- Windows server, który odpowiada serwer sieci web aplikacji ("IIS01")
- Dwa serwery systemu Windows, reprezentujące serwery wewnętrznej aplikacji ("AppVM01", "AppVM02")
- Systemu Windows server, która reprezentuje serwera DNS ("DNS01")

Skrypty i szablonu Menedżera zasobów Azure, zobacz [Tworzenie szczegółowe instrukcje][Example1].

#### <a name="nsg-description"></a>Opis NSG
W tym przykładzie grupę NSG jest wbudowany i następnie załadować z regułami sześć.

>[AZURE.TIP] Ogólnie mówiąc należy utworzyć określonych reguł "Zezwalaj" najpierw, a następnie bardziej ogólne reguły "Odmów". Priorytet danego decyduje o tym, jakie zasady są sprawdzane jako pierwsze. Gdy ruch znajduje się do określonej reguły, są obliczane w żadnych dalszych reguł. Reguły NSG można stosować w przychodzących i wychodzących kierunku (z punktu widzenia podsieci).

Deklaratywnie następujące reguły są tworzone dla ruchu przychodzącego:

1.  Ruch wewnętrzny DNS (port 53) jest dozwolone.
2.  Ruch RDP (port 3389) do maszyn wirtualnych z Internetu jest dozwolone.
3.  Ruch HTTP (port 80) z Internetu na serwerze sieci web (IIS01) jest dozwolone.
4.  Każdy ruch (wszystkie porty) z IIS01 do AppVM1 jest dozwolone.
5.  Odmowa każdy ruch (wszystkie porty) z Internetu do całej sieci wirtualnych (zarówno podsieci).
6.  Każdy ruch (wszystkie porty) z zewnętrzną podsieci do podsieci wewnętrznej zostało odrzucone.

Przy użyciu tych reguł związane z każdej podsieci, jeśli żądania HTTP przychodzące z Internetu na serwerze sieci web, obie reguły 3 (Zezwól) i 5 (odmowa) chcesz zastosować. Ale ponieważ reguły 3 ma wyższy priorytet, tylko chcesz zastosować i reguły 5 nie będzie przychodzących do odtwarzania. Dlatego żądania HTTP będzie mieć możliwość serwer sieci web. Jeśli ten sam ruch została próby nawiązania połączenia z serwerem DNS01, zasada 5 (odmowa) będzie należy zastosować i ruchu czy nie można przekazać do serwera. Reguły 6 (odmowa) blokuje frontonu podsieci z rozmówcy podsieci wewnętrznej (z wyjątkiem dozwolonego ruchu w regułach 1 i 4). Chroni sieci wewnętrznej, w przypadku aplikacji sieci web na końcu przedniego intensywność pogarsza atakującej. Tej czy mają ograniczony dostęp do sieci wewnętrznej "chroniony" (tylko dla zasobów wyświetlane na serwerze AppVM01).

Istnieje domyślne reguły wychodzącej, umożliwiająca ruchu w Internecie. W tym przykładzie mamy już zezwalania ruchu wychodzącego i nie modyfikowanie reguł ruchu wychodzącego. Do blokowania ruchu w obu kierunków, zdefiniowane przez użytkownika routing jest wymagane (Zobacz przykład 3).

#### <a name="conclusion"></a>Wnioski
Jest to stosunkowo niezwykle proste sposób izolowanie podsieci wewnętrznej z ruchu przychodzącego. Aby uzyskać więcej informacji, zobacz [Tworzenie szczegółowe instrukcje][Example1]. Między innymi następujące instrukcje:

- Sposoby tworzenia tej sieci obwód kształtu za pomocą skryptów programu PowerShell.
- Sposoby tworzenia tej sieci obwód kształtu z szablonem Azure Menedżera zasobów.
- Szczegółowe opisy każdego polecenia NSG.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch jest dozwolony lub odmowa w każdej warstwie.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Przykład 2: Tworzenie sieci obwodu chronić aplikacje z zapory i NSGs
[Powrót do początku Fast](#fast-start) | [szczegółowy Tworzenie instrukcji w tym przykładzie][Example2]

![Sieci przychodzącego obwód kształtu z NVA i NSG][8]

#### <a name="environment-description"></a>Opis środowiska
W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- Dwa usług w chmurze: "FrontEnd001" i "BackEnd001"
- Sieci wirtualnej, "CorpNetwork" z dwóch podsieci: "FrontEnd" i "Wewnętrznej bazy danych"
- Grupy zabezpieczeń sieci zastosowanego do obu podsieci
- Urządzenia wirtualnej sieci, w tym przypadku a zapora, podłączony do podsieci zewnętrznej
- Windows server, który odpowiada serwer sieci web aplikacji ("IIS01")
- Dwa serwery systemu Windows, reprezentujące serwery wewnętrznej aplikacji ("AppVM01", "AppVM02")
- Systemu Windows server, która reprezentuje serwera DNS ("DNS01")

Skrypty i szablonu Menedżera zasobów Azure, zobacz [Tworzenie szczegółowe instrukcje][Example2].

#### <a name="nsg-description"></a>Opis NSG
W tym przykładzie grupę NSG jest wbudowany i następnie załadować z regułami sześć.

>[AZURE.TIP] Ogólnie mówiąc należy określonych reguł "Zezwalaj" najpierw utworzyć, a następnie bardziej ogólne reguły "Odmów". Priorytet danego decyduje o tym, jakie reguły są sprawdzane jako pierwsze. Gdy ruch znajduje się do określonej reguły, są obliczane w żadnych dalszych reguł. Reguły NSG można stosować w przychodzących i wychodzących kierunku (z punktu widzenia podsieci).

Deklaratywnie następujące reguły są tworzone dla ruchu przychodzącego:

1.  Ruch wewnętrzny DNS (port 53) jest dozwolone.
2.  Ruch RDP (port 3389) do maszyn wirtualnych z Internetu jest dozwolone.
3.  Wszelkie ruchu internetowego (wszystkie porty) na urządzeniu wirtualna sieć (zapora) jest dozwolone.
4.  Każdy ruch (wszystkie porty) z IIS01 do AppVM1 jest dozwolone.
5.  Odmowa każdy ruch (wszystkie porty) z Internetu do całej sieci wirtualnych (zarówno podsieci).
6.  Każdy ruch (wszystkie porty) z zewnętrzną podsieci do podsieci wewnętrznej zostało odrzucone.

Przy użyciu tych reguł związane z każdej podsieci, jeśli żądania HTTP przychodzącego z Internetu zaporę, obie reguły 3 (Zezwalaj) i 5 (odmowa) chcesz zastosować. Ale ponieważ reguły 3 ma wyższy priorytet, tylko chcesz zastosować i reguły 5 nie będzie przychodzących do odtwarzania. Dlatego żądania HTTP będzie mieć możliwość zapory. Jeśli ten sam ruch próbował połączenia z serwerem IIS01, mimo że jest podsieci zewnętrzną, zasada 5 (odmowa) chcesz zastosować, i ruchu czy nie można przekazać do serwera. Reguły 6 (odmowa) blokuje zewnętrzną podsieci z rozmówcy podsieci wewnętrznej (z wyjątkiem dozwolonego ruchu w regułach 1 i 4). Chroni sieci wewnętrznej, w przypadku aplikacji sieci web na końcu przedniego intensywność pogarsza atakującej. Tej czy mają ograniczony dostęp do sieci wewnętrznej "chroniony" (tylko dla zasobów wyświetlane na serwerze AppVM01).

Istnieje domyślne reguły wychodzącej, która zezwala na ruch się z Internetem. W tym przykładzie mamy już zezwalania ruchu wychodzącego i nie modyfikowanie reguł ruchu wychodzącego. Do blokowania ruchu w obu kierunków, zdefiniowane przez użytkownika routing jest wymagane (Zobacz przykład 3).

#### <a name="firewall-rule-description"></a>Opis reguły zapory
Na zaporę powinny być tworzone regułach przesyłania dalej. Reguła jest potrzebna, ponieważ w tym przykładzie tylko kieruje ruch internetowy w powiązany z nią, a następnie na serwerze sieci web tylko jeden przekazywanie sieci NAT (NAT).

Reguły przesyłania dalej akceptuje dowolny ruchu przychodzącego źródłowy adres, który trafienia zapory próby osiągnięcia HTTP (port 80 i 443 dla HTTPS). Ma wysyłane poza interfejsu lokalnego zapory i Przekierowanie do serwera sieci web o adresie IP 10.0.1.5.

#### <a name="conclusion"></a>Wnioski
Jest to stosunkowo prosta sposób ochrony aplikacji za pomocą zapory i izolowanie podsieci wewnętrznej z ruchu przychodzącego. Aby uzyskać więcej informacji, zobacz [Tworzenie szczegółowe instrukcje][Example2]. Między innymi następujące instrukcje:

- Sposoby tworzenia tej sieci obwód kształtu za pomocą skryptów programu PowerShell.
- Jak utworzyć w tym przykładzie z szablonem Azure Menedżera zasobów.
- Szczegółowe opisy poszczególnych NSG reguły zapory i polecenia.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch jest dozwolony lub odmowa w każdej warstwie.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Przykład 3: Tworzenie sieci obwodu chronić sieci z zapory, UDR i NSG
[Powrót do początku Fast](#fast-start) | [szczegółowy Tworzenie instrukcji w tym przykładzie][Example3]

![Dwukierunkowa usługodawcy z NVA, NSG i UDR][9]

#### <a name="environment-description"></a>Opis środowiska
W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- Trzy usług w chmurze: "SecSvc001", "FrontEnd001" i "BackEnd001"
- Sieci wirtualnej, "CorpNetwork" z trzech podsieci: "SecNet", "FrontEnd" i "Wewnętrznej bazy danych"
- Urządzenia wirtualnej sieci, w tym przypadku a zapora, podłączony do podsieci SecNet
- Windows server, który odpowiada serwer sieci web aplikacji ("IIS01")
- Dwa serwery systemu Windows, reprezentujące serwery wewnętrznej aplikacji ("AppVM01", "AppVM02")
- Systemu Windows server, która reprezentuje serwera DNS ("DNS01")

Skrypty i szablonu Menedżera zasobów Azure, zobacz [Tworzenie szczegółowe instrukcje][Example3].

#### <a name="udr-description"></a>Opis UDR
Domyślnie następujące przekierowuje system są definiowane następująco:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal jest zawsze prefix(es) zdefiniowane adres wirtualnej sieci dla tej sieci określonych (czyli przybierze z wirtualną sieć wirtualną sieć, w zależności od tego, jak zdefiniowane każdego określonego wirtualną sieć). Pozostała trasy system są statyczne i domyślne jak wskazano w tabeli.

W tym przykładzie dwóch tabel routingu są tworzone, jedną dla zewnętrznej i wewnętrznej podsieci. Każda tabela jest załadowana trasy statyczne odpowiednie dla danej podsieci. W celu w tym przykładzie każda tabela zawiera trzy trasy, które skierowania całego ruchu (0.0.0.0/0) przez zaporę (następnego przeskoku = wirtualny urządzenia adres IP):

1. Ruch podsieci lokalnej przy następnym przeskoków definiowane w celu zezwalanie na ruch podsieci lokalnej pominąć zapory.
2. Ruch wirtualnej sieci z przeskoków następnego zdefiniowany zapory; zastępuje to domyślna reguła, która zezwala na lokalnej wirtualną ruch sieciowy skierować bezpośrednio.
3. Cały ruch pozostała (0-0) z przeskoków następnego zdefiniowany zapory.

>[AZURE.TIP] Nie ma wpisu podsieci lokalnej w UDR spowoduje przerwanie komunikacji podsieci lokalnej. 
> - W naszym przykładzie 10.0.1.0/24 wskazująca VNETLocal jest ważne, jak w przeciwnym razie pakietów opuszczania niepowodzenia serwer sieci Web (10.0.1.4) przeznaczone do innego lokalnego serwera (na przykład) 10.0.1.25 jako zostaną wysłane przez do NVA, który wyśle go do podsieci, a podsieci zostanie ponownie wyślij go do NVA i tak dalej.
> - Szanse Pętla routingu są zwykle lepsze w wielu nic urządzenia, które są podłączone bezpośrednio do każdego podsieć, którą komunikują się, które są często tradycyjny, lokalne, urządzenia. 

Po utworzeniu tabeli routingu są powiązane ich podsieci. Podsieć frontonu tabeli routingu, raz utworzone i powiązane z podsieci, będzie miała następującą postać:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] Teraz można stosować UDR do podsieci bramy, w którym jest połączony z obwodem ExpressRoute.
>
> Przykłady sposobów włączania sieci obwód kształtu za pomocą ExpressRoute lub witryny do witryny sieci przedstawiono w przykładach 3 i 4.


#### <a name="ip-forwarding-description"></a>Opis przekazywanie adresów IP
Przekazywanie adresów IP jest funkcją companion do UDR. To ustawienie na urządzeniu wirtualnych, dzięki któremu mogą go odbierać dane nie zostały zaadresowane do urządzenia, a następnie ustawisz przesyłanie ruchu do miejsca docelowego ultimate.

Na przykład jeśli ruchu z AppVM01 wykonuje żądanie na serwerze DNS01, UDR czy rozesłać to zapory. Z przekazywanie adresów IP włączone ruch w docelowej DNS01 (10.0.2.4) jest akceptowane przez urządzenia (10.0.0.4), a następnie przesłane dalej do miejsca docelowego ultimate (10.0.2.4). Bez adresów IP przekazywanie włączone zapory ruch czy nie zostanie zaakceptowana przez urządzenia, nawet jeśli tabela rozsyłania ma zaporę jako następnego przeskoku. Aby użyć wirtualnego urządzenia, jest niezwykle ważne Pamiętaj, aby włączyć przekazywanie adresów IP w połączeniu z UDR.

#### <a name="nsg-description"></a>Opis NSG
W tym przykładzie grupę NSG jest wbudowany i następnie załadować za pomocą jednej reguły. Ta grupa jest następnie powiązany tylko frontonu i wewnętrznej podsieci (nie SecNet). Deklaratywnie jest tworzone następującą regułę:

- Odmowa każdy ruch (wszystkie porty) z Internetu do całej sieci wirtualnych (wszystkie podsieci).

Mimo że NSGs są używane w tym przykładzie, głównym celem jest jako warstwę pomocniczą ochronę przed konfiguracji ręcznej. Celem jest, aby zablokować cały ruch przychodzący z Internetu do zewnętrznej lub wewnętrznej podsieci. Ruch tylko należy przepływał przez podsieci SecNet zapory (a następnie, w razie potrzeby do zewnętrznej lub wewnętrznej podsieci). Ponadto z regułami UDR w miejscu, wszelkie dane, że do zewnętrznej lub wewnętrznej podsieci będzie przekierowywany się do zapory (dzięki UDR). Zapora może to widzieć jako asymetrycznym przepływu i czy upuść ruchu wychodzącego. A więc są trzy warstwy zabezpieczeń Ochrona podsieci:
- Otwieranie punktów końcowych na FrontEnd001 i BackEnd001 usług w chmurze.
- NSGs odmawianie ruchu z Internetu.
- Ruch asymetrycznym upuszczanie zapory.

Jeden interesującej kwestii dotyczących NSG w tym przykładzie jest, czy zawiera ona tylko jeden reguły, która ma odmówić do całego wirtualnej sieci, w tym podsieci zabezpieczeń ruchu internetowego. Jednak ponieważ NSG tylko jest powiązany z zewnętrzną i wewnętrzną podsieci, reguła nie jest przetwarzana ruchu przychodzącego podsieci zabezpieczeń. W wyniku ruch będzie przepływał do podsieci zabezpieczeń.

#### <a name="firewall-rules"></a>Reguły zapory
Na zaporę powinny być tworzone regułach przesyłania dalej. Ponieważ Zapora jest blokowanie lub przekazywanie wszystkich przychodzący, wychodzący i ruch wewnątrz wirtualnej sieci, wiele reguł zapory są wymagane. Ponadto cały ruch przychodzący będzie odwołań Usługa zabezpieczeń publiczny adres IP (na różnych portów), mają być przetwarzane przez zaporę. Najlepszym rozwiązaniem jest diagramu logicznego przepływów przed rozpoczęciem konfigurowania podsieci i reguły zapory, aby uniknąć przeanalizować później. Poniższa ilustracja jest widok logiczny reguły zapory w tym przykładzie:
 
![Widok logiczny reguły zapory][10]

>[AZURE.NOTE] W zależności od urządzenia wirtualna sieć używane, różnią się porty zarządzania. W tym przykładzie jest używany zapory NextGen Barracuda, który używa porty 22, 801 i 807. Zapoznaj się z dokumentacją dostawcy urządzenia, aby znaleźć odpowiednie porty używane do zarządzania urządzenia.

#### <a name="firewall-rules-description"></a>Opis reguł zapory
Na powyższym diagramie logicznych podsieci zabezpieczeń nie jest wyświetlane. Jest tak, ponieważ Zapora jest tylko zasobów tej podsieci i ten schemat jest widoczna reguły zapory i sposobu ich logicznie zezwolić lub odmówić strumieni ruchu, nie tylko routingu ścieżkę. Ponadto zewnętrzne porty zaznaczonego dla ruchu RDP mają wyższą w granicach porty (8014 — 8026) i wybrano nieco wyrównać ostatnie dwa oktety lokalny adres IP łatwiejsze czytelność tekstu (na przykład adres serwera lokalnego 10.0.1.4 jest skojarzone z portu zewnętrznego 8014). Żadnych wyższą-powodujące konflikt portów, jednak mogą służyć.

W tym przykładzie jest potrzebna siedem typy reguł:

- Reguły zewnętrzne (w przypadku ruchu przychodzącego):
  1.    Reguła zarządzania: przekierowywanie tę aplikację reguła zezwala na przekazywanie ruchu do porty zarządzania urządzenia wirtualnej sieci.
  2.    Reguły RDP (dla każdego serwera systemu Windows): reguły te cztery, (po jednej dla każdego serwera) umożliwiają zarządzanie poszczególnych serwerach za pośrednictwem RDP. Może to również być połączone w jedną regułę, w zależności od możliwości urządzenia wirtualna sieć używana.
  3.    Reguły ruchu aplikacji: znajdują się dwa z tych reguł w pierwszym ruchu frontonu sieci web, a drugi dla ruchu wewnętrznej (na przykład serwer sieci web do warstwy danych). Konfiguracja tych reguł w zależności od architektury sieci (serwery rozmieszczenie) i ruchu przepływów (kierunku przepływu ruchu i portów, do których są używane).
      - Pierwszą regułę zezwala na ruch rzeczywistego zastosowania nawiązania połączenia z serwerem aplikacji. Zezwolić inne zasady dotyczące zabezpieczeń i zarządzania, zasady ruchu aplikacji są co zezwolić użytkownikom zewnętrznym lub usługi uzyskać dostęp do aplikacji. W tym przykładzie jest jednym serwerze sieci web na porcie 80. Dlatego pojedynczy reguła aplikacji przekierowuje ruchu przychodzącego do od zewnętrznych w celu web serwerów wewnętrznych adres IP. Sesja przekierowanie ruchu będzie można tłumaczyć za pośrednictwem translatora adresów Sieciowych do wewnętrznego serwera.
      - Druga reguła jest regułę wewnętrznej, aby zezwolić na serwerze sieci web porozmawiać z serwera AppVM01 (ale nie AppVM02) za pomocą dowolnego portu.
- Reguły wewnętrzne (dla ruchu w obrębie wirtualnych sieci)
  4.    Wychodzące do reguły internetowej: Ta reguła zezwala na ruch z dowolna sieć, aby przekazać do wybranych sieciach. Ta reguła jest zazwyczaj reguły domyślne już na zapory, ale wyłączony. Ta reguła powinna być włączona w tym przykładzie.
  5.    Reguła DNS: Ta reguła zezwala tylko na ruch DNS (port 53) w celu przekazania do serwera DNS. Dla tego środowiska większości ruchu z fronton wewnętrzna jest zablokowany. Ta reguła umożliwia specjalnie DNS z dowolnym podsieci lokalnej.
  6.    Podsieć podsieci reguły: Ta reguła jest umożliwienie dowolnego serwera podsieci wewnętrznej do połączenia z serwerem frontonu podsieci (ale nie odwrotnie).
- Reguła awaryjnych (dla ruchu, który nie spełnia dowolne z poprzedniego):
  7.    Wszystkie reguły ruchu odmówić: zawsze należy ostateczne reguły (pod względem priorytet) oraz sposób Jeśli przepływu danych nie będzie pasować do żadnej z poprzedniego zasady zostaną usunięte przez tę regułę. To jest to reguła domyślne i zazwyczaj aktywowana. Zazwyczaj potrzebne są żadnych zmian.

>[AZURE.TIP] Na drugiej reguły ruch aplikacji w celu uproszczenia w tym przykładzie dowolnego portu jest dozwolone. Scenariusz z rzeczywistą specyficzny port i zakresy adresów stosuje się w celu zmniejszenia powierzchni atakiem tej reguły.

Po utworzeniu wszystkich poprzednich reguł, należy przejrzeć priorytet każdego reguły w celu zapewnienia ruch będzie dozwolony lub odmowa stosownie do potrzeb. W tym przykładzie reguły są w priorytetu.

#### <a name="conclusion"></a>Wnioski
Jest to bardziej złożone, ale dokładniejszy sposób ochrony i izolowanie sieci niż poprzednich przykładach. (Przykład 2 chroni tylko aplikacji i przykład 1 po prostu izoluje podsieci). Ten projekt umożliwia monitorowanie ruchu w obu kierunków i chroni nie tylko serwer przychodzący aplikacji, ale wymusza zasad zabezpieczeń sieci na wszystkich serwerach w tej sieci. Ponadto w zależności od urządzenia używane, pełny ruch inspekcji i Sygnalizacja dostępności można osiągnąć. Aby uzyskać więcej informacji, zobacz [Tworzenie szczegółowe instrukcje][Example3]. Między innymi następujące instrukcje:

- Jak utworzyć tę przykładową sieć obwód kształtu za pomocą skryptów programu PowerShell.
- Jak utworzyć w tym przykładzie z szablonem Azure Menedżera zasobów.
- Szczegółowe opisy poszczególnych UDR NSG poleceń i reguły zapory.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch jest dozwolony lub odmowa w każdej warstwie.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Przykład 4: Dodawanie połączenia hybrydowego z witryny do witryny, wirtualne urządzenia wirtualną sieć prywatną (VPN)
[Powrót do początku szybkie](#fast-start) | Szczegółowe instrukcje kompilacji dostępne wkrótce

![Sieć obwód kształtu z NVA połączonych hybrydowych sieci][11]

#### <a name="environment-description"></a>Opis środowiska
Hybrydowe sieci przy użyciu sieci urządzenia wirtualnych (NVA) można dodawać do dowolnego typy sieci obwodowych opisany w przykładach 1, 2 lub 3.

Jak pokazano w powyższej ilustracji, połączenie VPN w Internecie (witryny do witryny) jest używane do łączenia sieci lokalnej Azure wirtualną siecią za pośrednictwem NVA.

>[AZURE.NOTE] Jeśli używasz ExpressRoute z publicznej zaglądanie Azure włączoną opcją powinny być tworzone trasę statyczną. Należy to rozsyłanie połączenie z adresem IP sieci VPN NVA firmy Internetu, a nie za pomocą ExpressRoute WAN. Translatora adresów Sieciowych wymaganych opcji zaglądanie publicznej Azure ExpressRoute rozbicie sesji VPN.

Gdy sieć VPN znajduje się w miejscu, NVA staje się Centrum dla wszystkich sieci i podsieci. Reguły przesyłania dalej zapory określają, które przepływu ruchu są dozwolone, są przekształcane za pośrednictwem translatora adresów Sieciowych, zostanie przekierowany lub są usuwane (nawet w przypadku przepływów ruch między sieci lokalnej i Azure).

Przepływ ruchu należy rozważyć umiarem, może być zoptymalizowany lub ograniczone przez tego wzorca projektu, w zależności od konkretnych przypadków użycia.

Przy użyciu środowiska wbudowany w przykładzie 3, a następnie dodanie witryny do witryny sieci VPN hybrydowych połączenie sieciowe, tworzy następujące projektu:

![Sieć obwód kształtu z NVA połączone za pomocą sieci VPN witryny do witryny][12]

Routera lokalnego lub innego urządzenia sieci, które jest zgodny z usługi NVA dla sieci VPN, będzie klienta VPN. To urządzenie fizycznie będzie odpowiedzialny za wprowadzenie i zachowywanie połączenie VPN z usługi NVA.

Logicznie do NVA, sieci wygląda cztery osobne "strefy zabezpieczeń" z regułami na NVA jest podstawowy Dyrektor ruchu między tymi strefami:

![Logiczna sieć z perspektywy NVA][13]

#### <a name="conclusion"></a>Wnioski
Dodawanie witryny do witryny hybrydowych sieci połączenie VPN Azure wirtualną sieć można rozszerzyć sieci lokalnej na Azure w bezpieczny sposób. Korzystając z połączenie VPN, ruchu są szyfrowane i kieruje przez Internet. NVA w tym przykładzie zawiera centralnej lokalizacji, aby wymusić i zarządzania zasadami zabezpieczeń. Aby uzyskać więcej informacji zobacz instrukcje szczegółowe Konstruuj (przyszłych). Między innymi następujące instrukcje:

- Jak utworzyć tę przykładową sieć obwód kształtu za pomocą skryptów programu PowerShell.
- Jak utworzyć w tym przykładzie z szablonem Azure Menedżera zasobów.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch będzie przepływał przez tego projektu.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Przykład 5: Dodawanie połączenia hybrydowego z bramą VPN witryny do witryny, Azure
[Powrót do początku szybkie](#fast-start) | Szczegółowe instrukcje kompilacji dostępne wkrótce

![Sieć obwód kształtu z sieci połączonych hybrydowych bramy][14]

#### <a name="environment-description"></a>Opis środowiska
Hybrydowe sieci przy użyciu bramy Azure VPN można dodać do dowolnego typu sieci obwód kształtu opisanego w przykładach 1 lub 2.

Jak pokazano na kolejnej ilustracji, połączenie VPN w Internecie (witryny do witryny) jest używane do łączenia sieci lokalnej Azure wirtualną siecią za pośrednictwem sieci VPN Azure bramy.

>[AZURE.NOTE] Jeśli używasz ExpressRoute z publicznej zaglądanie Azure włączoną opcją powinny być tworzone trasę statyczną. Należy to rozsyłanie połączenie z adresem IP sieci VPN NVA firmy Internetu, a nie za pomocą ExpressRoute WAN. Translatora adresów Sieciowych wymaganych opcji zaglądanie publicznej Azure ExpressRoute rozbicie sesji VPN.

Na poniższej ilustracji pokazano dwa krawędzie sieci w tej opcji. Na krawędzi pierwszego NVA i NSGs kontroli przepływu ruchu w obrębie Azure sieciach i między Azure i w Internecie. Druga krawędzi jest brama Azure VPN jest całkowicie oddzielnych sieci krawędź między wdrożeniem lokalnym a Azure.

Przepływ ruchu należy rozważyć umiarem, może być zoptymalizowany lub ograniczone przez tego wzorca projektu, w zależności od konkretnych przypadków użycia.

Przy użyciu środowiska wbudowany w przykładzie 1, a następnie dodanie witryny do witryny sieci VPN hybrydowych połączenie sieciowe, tworzy następujące projektu:

![Usługodawcy bramy połączone za pomocą połączenia ExpressRoute][15]

#### <a name="conclusion"></a>Wnioski
Dodawanie witryny do witryny hybrydowych sieci połączenie VPN Azure wirtualną sieć można rozszerzyć sieci lokalnej na Azure w bezpieczny sposób. Używanie natywnych brama Azure VPN, ruchu jest IPSec szyfrowane i kieruje za pośrednictwem Internetu. Ponadto za pomocą bramy sieci VPN Azure umożliwia dolnym opcję Koszt (nie dodatkowych licencjonowania kosztów, podobnie jak w przypadku innych firm NVAs). To jest najbardziej ekonomicznych w przykładzie 1, którego nie NVA jest używana. Aby uzyskać więcej informacji zobacz instrukcje szczegółowe Konstruuj (przyszłych). Między innymi następujące instrukcje:

- Jak utworzyć tę przykładową sieć obwód kształtu za pomocą skryptów programu PowerShell.
- Jak utworzyć w tym przykładzie z szablonem Azure Menedżera zasobów.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch będzie przepływał przez tego projektu.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Przykład 6: Dodawanie połączenia hybrydowego z ExpressRoute
[Powrót do początku szybkie](#fast-start) | Szczegółowe instrukcje kompilacji dostępne wkrótce

![Sieć obwód kształtu z sieci połączonych hybrydowych bramy][16]

#### <a name="environment-description"></a>Opis środowiska
Hybrydowe sieci przy użyciu połączenia prywatne peering ExpressRoute można dodać do dowolnego typu sieci obwód kształtu opisanego w przykładach 1 lub 2.

Jak pokazano na kolejnej ilustracji, ExpressRoute prywatne zaglądanie zapewnia bezpośrednie połączenie między sieci lokalnej i Azure wirtualnych sieci. Ruch transits sieci dostawcy usług i sieci Microsoft Azure, nigdy nie dotknąć w Internecie.

>[AZURE.NOTE] Istnieją pewne ograniczenia po użyciu UDR ExpressRoute, ze względu na złożoność routing dynamiczny używane w Azure bramy wirtualnych. Są to w następujący sposób:
>
>- UDR nie powinny być stosowane do podsieci bramy, na którym jest połączony ExpressRoute połączonych Azure wirtualnych bramy.
>- ExpressRoute połączone Azure wirtualne bramy nie może być urządzenia następny skok dla innych UDR powiązać podsieci.
>
>
<br />

>[AZURE.TIP] Używając ExpressRoute zachowuje firmową ruch sieciowy od Internetu ze względów bezpieczeństwa i znacznie zwiększenie wydajności. Pozwala także umów dotyczących poziomu usług od dostawcy ExpressRoute. Brama Azure można przekazać do 2 Gb/s z ExpressRoute, z witryny do witryny sieci VPN, przepustowość maksymalna Azure bramy jest 200 Mb/s.

Jak pokazano na poniższym diagramie, użyj tej opcji środowiska zawiera teraz dwie krawędzie sieci. NVA i NSG kontroli przepływu ruchu w obrębie Azure sieciach i między Azure i Internet, gdy bramy jest krawędź całkowicie oddzielnych sieci między lokalnego i Azure.

Przepływ ruchu należy rozważyć umiarem, może być zoptymalizowany lub ograniczone przez tego wzorca projektu, w zależności od konkretnych przypadków użycia.

Przy użyciu środowiska wbudowany w przykładzie 1, a następnie dodając połączenie sieciowe hybrydowych ExpressRoute tworzy następujące projektu:

![Sieć obwód kształtu z bramy połączone za pomocą połączenia ExpressRoute][17]

#### <a name="conclusion"></a>Wnioski
Dodanie połączenie sieciowe Peering ExpressRoute prywatnego można rozszerzyć sieci lokalnej o Azure bezpieczny, dolna opóźnienia wyższej wykonywania sposób. Ponadto za pomocą natywnego bramy Azure, jak w poniższym przykładzie zawiera dolnym opcję Koszt (Brak dodatkowych licencji, podobnie jak w przypadku innych firm NVAs). Aby uzyskać więcej informacji zobacz instrukcje szczegółowe Konstruuj (przyszłych). Między innymi następujące instrukcje:

- Jak utworzyć tę przykładową sieć obwód kształtu za pomocą skryptów programu PowerShell.
- Jak utworzyć w tym przykładzie z szablonem Azure Menedżera zasobów.
- Szczegółowe ruch przepływu scenariuszy, przedstawiający, jak ruch będzie przepływał przez tego projektu.

## <a name="references"></a>Odwołania
### <a name="helpful-websites-and-documentation"></a>Przydatne witryn i dokumentacji
- Azure programu Access przy użyciu Menedżera zasobów Azure:
- Uzyskiwanie dostępu do Azure przy użyciu programu PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Wirtualna dokumentacji sieciowej: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Dokumentacja grupy zabezpieczeń sieci: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Dokumentacja routingu zdefiniowane przez użytkownika: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure bram wirtualna: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Witryny do witryny sieci VPN: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- Dokumentacja ExpressRoute (należy koniecznie zapoznaj się z sekcji "Wprowadzenie" i "Jak do"): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Schemat blokowy opcje zabezpieczeń"
[1]: ./media/best-practices-network-security/compliancebadges.png "Identyfikatory Azure zgodności"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Funkcje zabezpieczeń Azure"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Strefy Zdemilitaryzowanej w sieci firmowej"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Architektura zabezpieczeń Azure"
[5]: ./media/best-practices-network-security/dmzazure.png "Strefy Zdemilitaryzowanej w Azure wirtualnej sieci"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Sieć hybrydowego z trzech granice zabezpieczeń"
[7]: ./media/best-practices-network-security/example1design.png "Ruch przychodzący strefy Zdemilitaryzowanej z NSG"
[8]: ./media/best-practices-network-security/example2design.png "Ruch przychodzący strefy Zdemilitaryzowanej z NVA i NSG"
[9]: ./media/best-practices-network-security/example3design.png "Dwukierunkowa strefy Zdemilitaryzowanej z NVA, NSG i UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Widok logiczny reguły zapory"
[11]: ./media/best-practices-network-security/example4designoptions.png "Strefy Zdemilitaryzowanej z NVA połączonych hybrydowych sieci"
[12]: ./media/best-practices-network-security/example4designs2s.png "Strefy Zdemilitaryzowanej z NVA połączone za pomocą sieci VPN witryny do witryny"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logiczna sieć z perspektywy NVA"
[14]: ./media/best-practices-network-security/example5designoptions.png "Strefy Zdemilitaryzowanej Azure bramy połączonych hybrydowych witryny do witryny sieci"
[15]: ./media/best-practices-network-security/example5designs2s.png "Strefy Zdemilitaryzowanej Azure bramy przy użyciu sieci VPN witryny do witryny"
[16]: ./media/best-practices-network-security/example6designoptions.png "Strefy Zdemilitaryzowanej Azure bramy połączonych hybrydowych ExpressRoute sieci"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "Strefy Zdemilitaryzowanej Azure bramy przy użyciu połączenia ExpressRoute"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
