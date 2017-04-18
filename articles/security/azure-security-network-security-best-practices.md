<properties
   pageTitle="Najważniejsze wskazówki dotyczące zabezpieczeń sieci Azure | Microsoft Azure"
   description="Ten artykuł zawiera zestaw najważniejsze wskazówki dotyczące używania zabezpieczeń sieci wbudowana możliwości Azure."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Najważniejsze wskazówki dotyczące zabezpieczeń sieci Azure

Microsoft Azure umożliwia połączenie maszyn wirtualnych i urządzeń do innych urządzeń sieciowych, umieszczając je w Azure wirtualnych sieci. Wirtualna sieć Azure jest konstrukcja wirtualną sieć, która umożliwia nawiązanie połączenia wirtualnej sieci karty do sieci wirtualnych, aby zezwolić na komunikację między urządzeniami sieci włączono opartych na protokole TCP. Azure maszyn wirtualnych połączenia wirtualnej sieci Azure będą mogli nawiązywanie połączenia z urządzenia w tej samej Azure wirtualnej sieci, różnych Azure wirtualnych sieci, w Internecie lub nawet w sieci lokalnej.

W tym artykule omówimy zbiór najważniejsze wskazówki dotyczące zabezpieczeń sieci Azure. Poniższe najważniejsze wskazówki pochodzą z naszych doświadczenie z siecią Azure i jej klientów, takich jak samodzielnie.

Dla każdego najlepiej będzie możemy wyjaśnić:

- Co to jest najbardziej skuteczne rozwiązanie
- Dlaczego użytkownik ma mieć możliwość tego najważniejsze wskazówki
- Jeśli nie można włączyć najlepiej, co może być skutkiem
- Możliwe rozwiązania alternatywne wobec najlepiej
- Jak możesz dowiedzieć się włączyć najlepiej

W tym artykule najważniejsze wskazówki dotyczące Azure sieci zabezpieczeń jest oparty na opinii zgody i możliwości platformy Azure i zestawy funkcji, jakie istnieją w czasie, który został zapisany w tym artykule. Opinie i technologie zmian w czasie i regularnie, aby uwzględnić zmiany zostaną zaktualizowane w tym artykule.

Azure sieci najważniejsze wskazówki dotyczące zabezpieczeń omawiane w tym artykule obejmują:

- Logicznie części podsieci
- Kontrolowanie zachowania routingu
- Włączanie wymuszonego Tunneling
- Używanie wirtualnych sieci urządzenia
- Wdrażanie DMZs do strefy zabezpieczeń
- Unikanie zagrożeń z Internetem dedykowane łącza WAN
- Optymalizowanie sprawności i wydajności
- Używanie równoważenia obciążenia globalny
- Wyłączanie dostępu RDP do Azure maszyn wirtualnych
- Włącz Centrum zabezpieczeń Azure
- Rozszerzona Azure centrum danych


## <a name="logically-segment-subnets"></a>Logicznie części podsieci

[Azure wirtualnych sieci](https://azure.microsoft.com/documentation/services/virtual-network/) są podobne do sieci LAN w sieci lokalnej. Ideą Azure wirtualnej sieci jest utworzenie pojedynczej IP adres oparte na miejsce sieci prywatnej w której można umieścić wszystkie [maszyn wirtualnych Azure](https://azure.microsoft.com/services/virtual-machines/). Prywatne obszarach adresów IP dostępne są w klasy (10.0.0.0/8), klasy B (172.16.0.0/12) i klasy C zakresy (192.168.0.0/16).

Podobnie jak co możesz zrobić lokalne, warto przeprowadzić segmentu większą przestrzeń adresową na podsieci. Zasady podsieci [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) podstawie umożliwia tworzenie usługi podsieci.

Routing między podsieci zostanie przeprowadzona automatycznie i nie trzeba ręcznie skonfigurować tabele routingu. Jednak to domyślne ustawienie to Brak bez kontroli dostępu do sieci między podsieci, tworzonych w wirtualnej sieci Azure. Aby można było utworzyć sieć uzyskiwanie dostępu do formantów między podsieci, musisz przeniesiony element między podsieci.

Jedną z możliwości, które można wykonać to zadanie jest [Grupa zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) (NSG). Prosty pakietów inspekcji korzystające krotki 5 (adres IP źródła, port źródłowy, docelowy adres IP, port docelowy i protokół warstwy 4) są NSGs sposobem tworzenia Zezwalaj odmówić reguły dla ruchu sieciowego. Można nadać lub odmówić ruchu do i pojedynczy adres IP, do i z wielu adresów IP lub nawet do i z całego podsieci.

Za pomocą NSGs kontroli dostępu do sieci między podsieci umożliwia umieszczenie zasoby, które należą do samej strefy zabezpieczeń lub rola w ich podsieci. Na przykład można traktować prostych aplikacji poziomu 3, z poziomu sieci web, w warstwie logiki aplikacji i Warstwa bazy danych. Umieszczenie maszyn wirtualnych, które należą do każdej z tych poziomów na własne podsieci. Następnie możesz używać NSGs do sterowania ruch między podsieci:

- Maszyn wirtualnych poziomu sieci Web można tylko inicjowania połączeń na komputerach logiki aplikacji i mogą akceptować tylko połączenia z Internetem
- Maszyn wirtualnych logiki aplikacji tylko może inicjować połączenia z poziomu bazy danych, a jedynie połączenia z poziomu sieci web
- Maszyn wirtualnych Warstwa bazy danych nie można zainicjować połączenia przy użyciu innych poza własne podsieci i tylko można akceptować połączenia z poziomu logiki aplikacji

Aby dowiedzieć się więcej na temat grup zabezpieczeń sieci i jak można używać ich do logicznie segmentów sieci wirtualnej Azure, przeczytaj artykuł [Co to jest grupa zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Kontrolowanie zachowania routingu

Po umieszczeniu maszyny wirtualnej w wirtualnej sieci Azure można zauważyć maszyny wirtualnej można nawiązać innych maszyn wirtualnych w tej samej Azure wirtualnej sieci, nawet w przypadku pozostałych maszyn wirtualnych różnych podsieci. Dlaczego jest to możliwe przyczyny jest tam zbiór trasy systemu, które są domyślnie włączone umożliwiające ten typ komunikacji. Te trasy domyślne Zezwalaj maszyn wirtualnych w tej samej Azure wirtualnej sieci do inicjowania połączeń ze sobą i z Internetem (dla komunikacji wychodzącej tylko w Internecie).

Trasy system domyślne są przydatne w przypadku wielu wdrożeń, wiążą się godzin, gdy chcesz dostosować konfigurację routingu dla wdrożeń. Dostosowania pozwoli skonfigurować adres następnego przeskoku do osiągnięcia określonych miejsc docelowych.

Zaleca się konfigurowanie przekierowuje zdefiniowane przez użytkownika, przy umieszczaniu wirtualną sieć urządzenia zabezpieczeń, które będą porozmawiamy o później dobrym rozwiązaniem.

> [AZURE.NOTE] Trasy zdefiniowane przez użytkownika nie są wymagane i trasy system domyślne będą działać w większości przypadków.

Więcej informacji o przekierowuje zdefiniowane przez użytkownika i jak je skonfigurować, czytając artykuł [Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP](../virtual-network/virtual-networks-udr-overview.md)można znaleźć.

## <a name="enable-forced-tunneling"></a>Włączanie wymuszonego Tunneling

Aby lepiej zrozumieć wymuszonego tunneling, jest przydatne dowiedzieć się, jakie "dzielonych tunneling" jest.
Najbardziej typowy przykład tunelowania podziału pojawia się przy użyciu połączenia VPN. Załóżmy, nawiązują połączenie VPN z pokoju hotel z siecią firmową. To połączenie pozwala na łączenie się z zasobami w sieci firmowej i całą komunikację do zasobów w sieci firmowej przechodzenie przez tunelem VPN.

Co się dzieje, gdy chcesz połączyć się z zasobami w Internecie? Po włączeniu dzielenie tunelowania tych połączeń przejdź bezpośrednio do Internetu, a nie za pośrednictwem tunelem VPN. Niektóre ekspertów zabezpieczeń należy rozważyć, czy to zagrożenie i dlatego zaleca się, że dzielenie tunelowania wyłączone i wszystkie połączenia, te przeznaczone do Internetu i te przeznaczone dla zasobów firmy obsłużone tunelem VPN. Dzięki temu jest następnie wymuszenie połączenia z Internetem za pośrednictwem sieci firmowej urządzeń zabezpieczeń, które nie być w przypadku, jeśli klienta VPN połączenie z Internetem poza tunelem VPN.

Teraz przejdźmy tej doprowadzić do maszyn wirtualnych w wirtualnej sieci Azure. Trasy domyślne dla wirtualnej sieci Azure Zezwalaj maszyn wirtualnych zainicjować ruchu w Internecie. To zbyt może oznaczać ryzyko związane z zabezpieczeniami, te połączenia wychodzące można zwiększyć powierzchnię atakiem maszyny wirtualnej i można użyć przez intruzów.
Z tego powodu zalecamy włączanie wymuszonego tunneling w środowisku maszyn wirtualnych systemu, gdy masz połączenie lokalne krzyżowe między wirtualnej sieci Azure i sieci lokalnej. Będzie porozmawiamy o krzyżowe łączności lokalnej w dalszej części tego Azure sieci najlepsze dokumentu rozwiązania.

Jeśli nie masz połączenie lokalne krzyżowego, upewnij się, korzystać z grup zabezpieczeń sieci (zostało to opisane wcześniej) lub Azure wirtualnej sieci zabezpieczeń urządzeń (opisanych dalej), aby zapobiec połączenia wychodzące do Internetu z maszyn wirtualnych Azure.

Aby dowiedzieć się więcej o wymuszone tunelowania i jak włączyć, przeczytaj artykuł [Konfigurowanie wymuszone tunelowania przy użyciu programu PowerShell i Menedżera zasobów Azure](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Za pomocą urządzenia wirtualnej sieci

Gdy grup zabezpieczeń sieci i routingu zdefiniowane przez użytkownika umożliwiają pewien stopień bezpieczeństwa sieci w sieci i transportu warstw [modelu OSI](https://en.wikipedia.org/wiki/OSI_model), mają wystąpić sytuacje, w którym będzie ma lub konieczne włączenie zabezpieczeń do wysokiego poziomu stosu. W takich sytuacjach zalecamy wdrożeniem wirtualną sieć zabezpieczeń urządzeń oferowanych przez partnerów Azure.

Sieć Azure zabezpieczeń urządzeń zapewnia znacznie zwiększenie poziomu zabezpieczeń na dostarczanych przez sieć poziomu kontrolki. Funkcje zabezpieczeń sieci dostarczony przez wirtualną sieć zabezpieczeń urządzeń należą:

- Zapory
- Wykrywanie intruzów-intruzów zapobiegania
- Luka w zabezpieczeniach zarządzania
- Formant aplikacji
- Wykrywania anomalii opartego na sieci
- Filtrowanie sieci Web
- Oprogramowanie antywirusowe
- Ochrona Botnet

Jeśli potrzebny wyższy poziom zabezpieczeń sieci, nie można uzyskać przy użyciu sieci dostępu na poziomie kontrolek, następnie zalecamy badanie i wdrażanie Azure wirtualną sieć zabezpieczeń urządzenia.

Aby dowiedzieć się, jakiego urządzenia zabezpieczeń Azure wirtualną sieć są dostępne i ich możliwości, odwiedź stronę [Azure Marketplace](https://azure.microsoft.com/marketplace/) i wyszukaj "zabezpieczenia" i "zabezpieczeń sieci".

##<a name="deploy-dmzs-for-security-zoning"></a>Wdrażanie DMZs do strefy zabezpieczeń
Strefy Zdemilitaryzowanej lub "obwód kształtu Sieć" jest segment sieci fizycznego lub logicznego, który zapewnia dodatkowe warstwy zabezpieczeń między aktywów i Internet. Zamiarem strefy Zdemilitaryzowanej jest umieszczenie urządzenia kontroli dostępu do sieci specjalistyczne na krawędzi sieci strefy Zdemilitaryzowanej, tak, aby tylko odpowiednie ruch jest dozwolony w przeszłości urządzenie ochrony sieci i w wirtualnej sieci Azure.

DMZs są przydatne, ponieważ możesz skoncentrować zarządzanie kontrolą dostępu do sieci monitorowania, rejestrowania i raportowania na urządzeniach przy krawędzi Azure wirtualnej sieci. W tym miejscu będzie zwykle umożliwia zapobiegania DDoS, systemów zapobiegania wykrywanie i intruzów intruzów (nazwy i adresy IP), reguły zapory i zasady, filtrowanie sieci web, sieci ochrony przed złośliwym oprogramowaniem i inne. Urządzenia ochrony sieci znajdują się między Internetu i sieci wirtualnej Azure i interfejs w obu sieciach.

To podstawowa projekt strefy Zdemilitaryzowanej wiążą się wiele różnych projektów strefy Zdemilitaryzowanej, takich jak odwrotnych, umiejscowieni trzy, wielu umiejscowieni i innych użytkowników.

W przypadku wszystkich wdrożeń wysokim poziomie zabezpieczeń zaleca się rozważyć wdrożenie strefy Zdemilitaryzowanej w celu zwiększenia poziomu zabezpieczeń sieci Azure zasobów.

Aby dowiedzieć się więcej na temat DMZs i jak je wdrożyć platformy Azure, przeczytaj artykuł [usługami w chmurze firmy Microsoft i zabezpieczeń sieci](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Unikanie zagrożeń z Internetem dedykowane łącza WAN
Wiele organizacji wybrano trasę hybrydowych IT. Hybrydowy IT niektóre aktywów informacji firmy znajdują się w Azure, a inne osoby pozostaną w lokalnej. W wielu przypadkach niektóre składniki usługi zostanie uruchomiony w Azure, a inne składniki pozostaną w lokalnej.

Hybrydowy scenariusz IT jest zazwyczaj różnego rodzaju łączność między lokalnej. To krzyżowe lokalnej łączności umożliwia firmie do łączenia ich sieci lokalnej Azure wirtualnych sieci. Dostępne są dwa rozwiązania w zakresie łączności między lokalnej:

- VPN witryny do witryny
- ExpressRoute

[Witryny do witryny sieci VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) reprezentuje wirtualnego połączenia prywatne między sieci lokalnej i Azure wirtualnych sieci. To połączenie ma obowiązywać w Internecie i umożliwia informacji "tunelem" wewnątrz szyfrowanego połączenia między sieci i Azure. Witryny — połączenie VPN jest technologią bezpieczne, dojrzałe przedsiębiorstw małych wdrożeniu dziesięcioleci. Szyfrowanie tunelem odbywa się trybie [tunelem IPsec](https://technet.microsoft.com/library/cc786385.aspx).

Witryny do witryny sieci VPN jest zaufane, wiarygodnych i ustanowienia technologii, ruchu w tunelem przechodzenie przez Internet. Ponadto przepustowości stosunkowo jest ograniczona do maksymalnie o 200Mbps.

Jeśli jest wymagane szczególnych poziom zabezpieczeń lub wykonania dla połączeń między lokalnej, zaleca się użycie Azure ExpressRoute dla łączność między lokalnej. ExpressRoute jest dedykowane WAN łącza między swojej lokalizacji lokalnego lub dostawcy hostingu programu Exchange. Ponieważ jest to połączenie telco, dane nie podróży przez Internet i dlatego nie są wyświetlane na potencjalne ryzyko związane z komunikacja w Internecie.

Aby dowiedzieć się więcej o sposobie działania Azure ExpressRoute i jak wdrożyć, przeczytaj artykuł [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optymalizowanie sprawności i wydajności
Poufności, integralności i dostępności (CIA) obejmują Triada dzisiejszą największe znaczenie modelu zabezpieczeń. O szyfrowania jest poufność i prywatność, integralności jest jak zagwarantować, że dane nie są zmieniane przez osoby nieautoryzowane i dostępność jest jak zagwarantować, że użytkownicy uwierzytelnieni będą mieli dostęp do informacji, które mają uprawnień dostępu do. Błąd w jednym z tych obszarów reprezentuje potencjalne złamania zabezpieczeń.

Dostępność można traktować jako o sprawności i wydajności. Jeśli usługa nie działa, informacje nie są dostępne. Jeśli jest to niskiej sposób dane, nie będzie można używać wydajność, firma Microsoft należy rozważyć dane mają być niedostępne. W związku z tym z perspektywy zabezpieczeń, należy wykonać, niezależnie od możemy upewnij się, że masz naszych usług optymalnego sprawności i wydajności.
Popularne i skuteczne metody, można zwiększyć dostępność i wydajność jest za pomocą równoważenia obciążenia. Równoważenia obciążenia jest to metoda dystrybucji ruchu sieciowego na serwerach, które są częścią usługi. Na przykład jeśli masz serwerach frontonu sieci web jako część tej usługi można równoważenia obciążenia dystrybuowanie ruchu między wieloma serwerach frontonu sieci web.

Rozpowszechnianie ruch zwiększa dostępność, ponieważ jeżeli jednego z serwerów sieci web jest niedostępna, równoważenia obciążenia będzie przestać wysyłać ruch do tego serwera i przekierowanie ruchu do serwerów, które są nadal w trybie online. Równoważenia obciążenia pozwala wydajności, ponieważ procesora, sieci i pamięci obciążenie obsługiwać żądania obejmowała wszystkie Załaduj strategie serwerów.

Zaleca się, że możesz zastosować równoważenia obciążenia przy każdym możesz i odpowiednio do usług. Czy w poniższych sekcjach będzie można rozwiązać.
Na poziomie Azure wirtualną sieć Azure zawiera z trzech podstawowych załadowaniu równoważenia opcje:

- Równoważenia obciążenia opartego na HTTP
- Zewnętrzne równoważenia obciążenia
- Wewnętrzna równoważenia obciążenia

## <a name="http-based-load-balancing"></a>Równoważenia obciążenia opartego na HTTP
Równoważenia obciążenia oparte na HTTP opiera decyzji dotyczących którego serwera, aby wysłać połączeń przy użyciu właściwości protokołu HTTP. Azure ma równoważenia obciążenia HTTP, wybiegającą według nazwy aplikacji bramy.

Zalecamy, możesz nam Azure Application Gateway po:

- Aplikacje, które wymagają żądania z tej samej sesji użytkownika/klienta do osiągnięcia tej samej maszyny wirtualnej wewnętrznej. Przykłady to samo zakupy, aplikacje koszyka i serwery poczty w sieci web.
- Aplikacje, które mają być wolny farmy serwerów sieci web z góry opłaty za SSL korzystając z funkcji [odciążania SSL](https://f5.com/glossary/ssl-offloading) aplikacji bramy.
- Aplikacjami, takimi jak sieci dostarczania zawartości, które wymagają że wielu żądania HTTP na tym samym długotrwałe połączenia TCP przesyłane i ładowanie strategie do różnych serwerów wewnętrznej.

Aby dowiedzieć się więcej na temat sposobu działania bramy aplikacji Azure i jak go używać wdrożeń, przeczytaj artykuł [Omówienie bramy aplikacji](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Zewnętrzne równoważenia obciążenia
Równoważenia obciążenia zewnętrznych odbywa się w przypadku połączeń przychodzących z Internetu ładowania rozłożony na serwerach znajduje się w wirtualnej sieci Azure. Usługi równoważenia obciążenia zewnętrznych Azure umożliwiają tej funkcji i zaleca się jej używać podczas sesji lepkich nie wymagają lub SSL offload.

W odróżnieniu od równoważenia obciążenia oparte na HTTP zewnętrznej usługi równoważenia obciążenia używa informacji w sieci i transportu warstw model sieci OSI do podejmowanie decyzji na serwerze, jakie załadować połączenia saldo.

Firma Microsoft zaleca używanie zewnętrznych równoważenia obciążenia zawsze, gdy masz [Aplikacje bezstanowe](http://whatis.techtarget.com/definition/stateless-app) akceptowanie przychodzących żądań z Internetu.

Aby dowiedzieć się więcej o sposobie działania równoważenia obciążenia zewnętrznych Azure i w jaki sposób można ją, wdrożyć, zapoznaj się z przeczytaj artykuł [wprowadzenie Get tworzenia Internet przeciwległych równoważenia obciążenia w Menedżerze zasobów przy użyciu programu PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Wewnętrzna równoważenia obciążenia
Równoważenia obciążenia wewnętrzny jest podobne do równoważenia obciążenia zewnętrznych i używa ten sam mechanizm załadować saldo połączenia się z serwerami spodem. Jedyna różnica polega na tym, że równoważenia obciążenia w tym przypadku akceptowania połączeń z maszyn wirtualnych, które nie są w Internecie. W większości przypadków połączeń, które są akceptowane do równoważenia obciążenia inicjowanych przez urządzeń w wirtualnej sieci Azure.

Firma Microsoft zaleca używanie wewnętrznych równoważenia obciążenia dla scenariuszy, które będą korzystać z tej funkcji, na przykład gdy chcesz załadować saldo połączeń z serwerami SQL lub wewnętrznej sieci web.

Aby dowiedzieć się więcej o sposobie działania wewnętrznego równoważenia obciążenia Azure i jak można ją wdrożyć, przeczytaj artykuł [wprowadzenie Get tworzenie wewnętrznego równoważenia obciążenia przy użyciu programu PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Używanie równoważenia obciążenia globalny
Powoduje, że komputerowego chmury publicznej możliwe do wdrożenia globalnie rozkładana aplikacje, które zawierają składniki znajduje się w centrach danych na całym świecie. Jest to możliwe w programie Microsoft Azure z powodu obecności globalnego centrum danych firmy Azure. W odróżnieniu od technologii wspomniano wcześniej równoważenia obciążenia równoważenia obciążenia globalnego umożliwia udostępnienie usług nawet wtedy, gdy cały centrach danych może stać się niedostępne.

Możesz uzyskać ten rodzaj globalnej równoważenia obciążenia w Azure wykorzystując [Menedżer ruchu Azure](https://azure.microsoft.com/documentation/services/traffic-manager/). Menedżer ruchu powoduje, że jest można załadować saldo połączenia usług na podstawie lokalizacji użytkownika.

Na przykład jeśli użytkownik jest żądania usługi z Unii Europejskiej, połączenie jest przekierowanie do usługi znajduje się w centrum danych Unii Europejskiej. To część ruch Menedżera globalnej Załaduj równoważenia pomaga zwiększyć wydajność, ponieważ nawiązywanie połączenia z najbliższym centrum danych jest większa niż nawiązywanie połączeń z centrami danych, które są daleko.

Na stronie Dostępność równoważenia obciążenia globalnej sprawdza, czy usługi jest dostępny, nawet jeśli całego centrum danych powinno zostać udostępnione.

Na przykład jeśli Azure centrum danych powinien stają się niedostępne z powodu środowiska powodów, dla których lub z powodu awarii (na przykład błędy sieciowe), połączenia z usługą może być przekierowane do najbliższego centrum danych w trybie online. Ten równoważenia obciążenia globalnej jest wykonywana korzystając z zasad DNS, które można tworzyć w Menedżerze ruch.

Firma Microsoft zaleca używanie Menedżera ruch dla każdego chmury rozwiązania przez siebie ma zakres dystrybucji u wielu regionów i wymaga najwyższego poziomu możliwe czas pracy.

Aby dowiedzieć się więcej na temat Menedżer ruchu Azure i jak wdrożyć go, przeczytaj artykuł [Co to jest Menedżer ruchu](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Wyłączanie dostępu RDP-SSH do Azure maszyn wirtualnych
Istnieje możliwość osiągnięcia maszyn wirtualnych Azure za pomocą [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) i protokoły [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH). Protokoły te umożliwiają zarządzanie maszyn wirtualnych z lokalizacji zdalnych i są standardowe w przypadku komputerów centrum danych.

Potencjalny problem zabezpieczenia w przypadku używania tych protokołów w Internecie to, że ataki różnymi sposobami [atakami](https://en.wikipedia.org/wiki/Brute-force_attack) można używać do uzyskiwania dostępu do maszyn wirtualnych Azure. Po dostęp nią one dla naruszenia innych maszyn wirtualnych sieci Azure za pomocą komputera wirtualnych jako punktu uruchamianie lub nawet ataki urządzeń sieciowych poza Azure.

Z tego powodu zalecamy wyłączenie bezpośredni dostęp RDP i SSH do maszyn wirtualnych Azure z Internetu. Po wyłączeniu bezpośredni dostęp RDP i SSH z Internetu, masz inne opcje, które umożliwiają dostęp do tych maszyn wirtualnych dla zarządzania zdalnego:

- VPN punktu do witryny
- VPN witryny do witryny
- ExpressRoute

[Punkt do witryny sieci VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) jest inna nazwa połączenia klient/serwer sieci VPN dostępu zdalnego. Sieć VPN punktu do witryny umożliwia jednemu użytkownikowi połączyć się z siecią wirtualną Azure przez Internet. Po nawiązaniu połączenia punkt do witryny, użytkownik będzie mógł nawiązywanie połączenia z dowolnym maszyn wirtualnych znajdujących się w wirtualnej sieci Azure, którymi użytkownik połączony za pośrednictwem sieci VPN punktu do witryny za pomocą RDP lub SSH. Przy założeniu, że użytkownik jest autoryzowany do osiągnięcia tych maszyn wirtualnych.

Punkt do witryny sieci VPN jest bezpieczniejsze niż bezpośrednie połączenia RDP lub SSH, ponieważ użytkownik ma do uwierzytelnienia dwa razy przed połączeniem maszyn wirtualnych. Najpierw należy uwierzytelniania (i uprawnione) nawiązać połączenie VPN punktu do witryny; drugi, użytkownik musi uwierzytelniania (i uprawnione) ustanawiania sesji RDP lub SSH.

[Witryny do witryny sieci VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) łączy całej sieci z inną siecią przez Internet. Sieć VPN witryny — umożliwia łączenie sieci lokalnej wirtualną sieć Azure. Jeśli wdrażania witryny do witryny sieci VPN, użytkownicy w Twojej sieci lokalnej będzie mógł połączyć się maszyn wirtualnych sieci wirtualnej Azure przy użyciu protokołu RDP lub SSH przy użyciu połączenia VPN witryny do witryny i nie wymaga dostęp bezpośredni RDP lub SSH przez Internet.

Za pomocą dedykowanego łącza WAN umożliwiają korzystanie z funkcji podobne do sieci VPN witryny do witryny. Główne różnice ma wartość 1. dedykowane łącza WAN nie przechodzą przez Internet i 2. dedykowane łącza WAN są zwykle więcej stabilny i performant. Azure umożliwia rozwiązanie dedykowane łącza WAN w postaci [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Włącz Centrum zabezpieczeń Azure
Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia i zapewnia zwiększone wgląd i kontrolować, zabezpieczenia Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

Centrum zabezpieczeń Azure ułatwia optymalizowanie i monitorowanie zabezpieczeń sieci przez:

- Dostarczanie zalecenia dotyczące zabezpieczeń sieci
- Monitorowanie stanu konfiguracji zabezpieczeń sieci
- Alerty o sieciowe zagrożeń zarówno na poziomie punktu końcowego i sieci

Zdecydowanie zaleca się włączenie Azure Centrum zabezpieczeń dla wszystkich Azure wdrożeń.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure i jak je włączyć dla wdrożeń, przeczytaj artykuł [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Bezpieczne rozszerzona centrum danych Azure
Wiele enterprise IT organizacji szukasz aby rozwinąć w chmurze zamiast rosnących centrach lokalnego. Ten rozszerzeń reprezentuje rozszerzenie istniejącej infrastruktury informatycznej w chmurze publicznej. Korzystając z lokalnej krzyżowe opcje połączeń jest możliwe traktowania wirtualnej sieci Azure jako tylko innej podsieci na infrastrukturę sieci lokalnej.

Istnieje jednak wiele planowanie i projektowanie problemy, które należy uwzględnić najpierw. Jest to szczególnie ważne w zakresie zabezpieczeń sieci. Jedną z najlepsze sposoby zrozumieć, jak podejście do tego projektu jest, aby zobaczyć przykład.

Firma Microsoft utworzyła [Diagram architektury odwołanie rozszerzenia centrum danych](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) i pomocniczych zabezpieczenia, aby lepiej zrozumieć, jak będzie wyglądać rozszerzeniem centrum danych. Dzięki temu przykładem implementacji odwołanie umożliwia planowanie i projektowanie rozszerzeniem bezpiecznego przedsiębiorstwa centrum danych w chmurze. Zalecamy zapoznanie się ten dokument, aby uzyskać ogólny obraz tego, kluczowe składniki bezpieczne rozwiązanie.

Aby dowiedzieć się więcej na temat bezpieczne rozszerzona centrum danych Azure, zapoznaj się z wideo [I i centrum danych Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
