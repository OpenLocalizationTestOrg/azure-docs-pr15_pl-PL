<properties 
   pageTitle="Wirtualna sieć VPN brama — często zadawane pytania | Microsoft Azure"
   description="Brama VPN — często zadawane pytania. Często zadawane pytania dotyczące połączeń między lokalnej Microsoft Azure wirtualnej sieci, połączenia konfiguracji hybrydowych i bramy sieci VPN"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>Brama VPN — często zadawane pytania

## <a name="connecting-to-virtual-networks"></a>Nawiązywanie połączenia z wirtualnych sieci

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Czy można połączyć wirtualnych sieci w różnych regionach Azure?
Wartość Tak. W rzeczywistości nie ma żadnych ograniczeń region. Jeden wirtualną sieć można nawiązać innego wirtualną sieć w tym samym lub w innym regionie Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Czy można połączyć wirtualnych sieci w różnych subskrypcjach?
Wartość Tak.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Czy mogę nawiązać wiele witryn z jednym wirtualnej sieci?

Umożliwia nawiązanie połączenia do wielu witryn przy użyciu programu Windows PowerShell oraz interfejsy API pozostałych Azure. Zobacz sekcję [łączności VNet do VNet i wiele witryn](#multi-site-and-vnet-to-vnet-connectivity) — często zadawane pytania.
## <a name="what-are-my-cross-premises-connection-options"></a>Jakie są opcje połączenia między lokalnej?

Obsługiwane są następujące połączenia między lokalnej:

- [Aby witryna](vpn-gateway-site-to-site-create.md) — połączenie VPN przez IPsec (IKE w wersji 1 i 2 IKE). Tego typu połączenia wymaga urządzenia VPN lub RRAS.

- Punkt witryny [](vpn-gateway-point-to-site-create.md) — połączenie VPN nad SSTP (Secure Socket Tunneling Protocol). To połączenie nie wymaga urządzenia VPN.

- [VNet — VNet](virtual-networks-configure-vnet-to-vnet-connection.md) — tego typu połączenia jest taka sama, jak konfiguracji witryny do witryny. VNet do VNet przekracza połączenie VPN IPsec (IKE w wersji 1 i 2 IKE). Nie wymaga urządzenia VPN.

- [Wiele witryn](vpn-gateway-multi-site.md) — jest to wariant konfiguracji witryny do witryny, która zezwala na łączenie wielu witrynach lokalnej wirtualną siecią.

- [ExpressRoute](../expressroute/expressroute-introduction.md) — ExpressRoute to połączenie bezpośrednie Azure z sieci WAN, nie publicznie w Internecie. Zobacz [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md) i [ExpressRoute — często zadawane pytania](../expressroute/expressroute-faqs.md) , aby uzyskać więcej informacji.

Aby uzyskać więcej informacji o połączeniach zobacz [Temat Brama VPN](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Jaka jest różnica między połączenia witryny do witryny i punkt do witryny?

Połączenia **Witryny do witryny** umożliwiają połączenie między wszystkich komputerów znajdujących się w swojej siedzibie maszyn wirtualnych lub wystąpienia ról w obrębie sieci wirtualnej, w zależności od tego, jak chcesz skonfigurować routing. To świetne rozwiązanie dla połączenia zawsze dostępne lokalnej infrastrukturze, a nadaje się dobrze w przypadku konfiguracji hybrydowych. Tego typu połączenia zależy od urządzenia sieci VPN IPsec (sprzętu lub urządzenia wygładzone), które muszą być wdrożone na krawędzi sieci. Aby utworzyć tego typu połączenia, musisz mieć wymagany sprzęt VPN i zewnętrznie przeciwległych adres IP protokołu IPv4.

Punkt witryny **** połączenia umożliwiają łączenie z jednego komputera z dowolnego miejsca do dowolnego elementu znajdującego się w sieci wirtualnej. Używa klienta VPN w polu systemu Windows. W ramach konfiguracji punktu do witryny należy zainstalować certyfikat i package konfiguracji klienta VPN, co zawiera ustawienia zezwalające na komputerze, aby połączyć się z dowolnym maszyn wirtualnych lub wystąpienia ról w wirtualnej sieci. Jest doskonałe, gdy chcesz się połączyć wirtualnej sieci, ale nie są umieszczane w lokalnej. Jest on również dobrym rozwiązaniem, gdy nie mają dostępu do sieci VPN sprzętu albo zewnętrznie przeciwległych adres IP protokołu IPv4, są one wymagane przez połączenie witryny do witryny. 

Wirtualna sieć w celu wykorzystania zarówno witryny do witryny i punkt do witryny jednocześnie, można skonfigurować pod warunkiem, że tworzenie połączenie witryny do witryny przy użyciu typu VPN oparte na trasy do bramy. Typy sieci VPN rozsyłania są nazywane dynamiczne bram w modelu Klasyczny wdrożenia.

### <a name="what-is-expressroute"></a>Co to jest ExpressRoute?

ExpressRoute pozwala na tworzenie prywatnych połączeń między centrach danych firmy Microsoft i infrastruktury w swojej siedzibie lub w środowisku Współtworzenie lokalizacji. Z ExpressRoute możesz nawiązywać połączenia programu Microsoft cloud usług, takich jak Microsoft Azure i usługi Office 365 na urządzenie ExpressRoute partnerów współpracujących lokalizacji lub bezpośrednio nawiązywanie połączenia z istniejącej sieci WAN (na przykład dostarczony przez dostawcę usług sieci VPN MPLS).

ExpressRoute połączenia oferują większe bezpieczeństwo, więcej niezawodności większej przepustowości i opóźnienia niższej niż typowy połączenia przez Internet. W niektórych przypadkach do przenoszenia danych między sieci lokalnej i Azure za pomocą połączenia ExpressRoute również może przynieść korzyści kosztów. Jeśli już zostały utworzone połączenie między lokalne z sieci lokalnej Azure, możesz przeprowadzić migrację połączenie ExpressRoute przy zachowaniu wirtualnej sieci niezmieniony.

Zobacz [ExpressRoute — często zadawane pytania](../expressroute/expressroute-faqs.md) , aby uzyskać więcej informacji.

## <a name="site-to-site-connections-and-vpn-devices"></a>Połączenia witryny do witryny i urządzeń sieci VPN

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Co należy wziąć pod uwagę podczas wybierania urządzenia VPN?

Sprawdzeniu poprawności zestawu standardowych urządzeń VPN witryny do witryny we współpracy z dostawcami urządzenia. Listę znanych zgodnych urządzeń sieci VPN, ich odpowiednie instrukcje dotyczące konfigurowania lub próbki i specyfikacji urządzenia można znaleźć [w tym miejscu](vpn-gateway-about-vpn-devices.md). Wszystkich urządzeń w rodziny urządzenie na liście zgodny z znane należy pracować z wirtualnej sieci. Aby pomóc, skonfiguruj urządzenie VPN, zajrzyj do przykładowych konfiguracji urządzenia lub łącze odpowiadające rodziny odpowiedniego urządzenia.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Co należy zrobić, mając urządzenia VPN, którego nie ma na liście znane zgodne urządzenie?

Jeśli nie widzisz urządzenia na liście znane zgodnego urządzenia VPN i ma zostać użyty do połączenie VPN, należy sprawdzić, czy spełnia obsługiwane IPsec/IKE konfiguracji opcji i parametrów wymienionych [poniżej](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Urządzenia spełniające minimalne wymagania powinno działać z sieci VPN bramy. Aby uzyskać dodatkowe instrukcje pomocy technicznej i konfiguracji, skontaktuj się z producentem urządzenia.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Dlaczego moje tunelem VPN na podstawie zasad przejść w dół po ruch jest bezczynny?

Jest to oczekiwane działanie w przypadku na podstawie zasad (nazywane także routing statyczny) bram sieci VPN. Gdy ruchu w tunelem jest bezczynny przez ponad 5 minut, będzie można usunięte tunelem. Po uruchomieniu ruch ułożony w dowolnym kierunku tunelem będzie ustanowić natychmiast.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Aby nawiązać połączenie Azure można używać oprogramowania sieci VPN?

Witryny do witryny lokalnej krzyżowe konfiguracji obsługujemy routingu systemu Windows Server 2012 i serwery dostępu zdalnego (RRAS).

Innych rozwiązań sieci VPN oprogramowania należy Praca z naszych bramy, jak są one zgodne z branży standardowy IPsec implementacji. Skontaktuj się z dostawcą oprogramowania, aby uzyskać instrukcje dotyczące konfiguracji i obsługa techniczna.

## <a name="point-to-site-connections"></a>Połączenia punkt do witryny

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Jakie systemy operacyjne można używać z punktu do witryny?

Obsługiwane są poniższe systemy operacyjne:

- Windows 7 (32-bitowej i 64-bitowa)

- Windows Server 2008 R2 (tylko wersja 64-bitowa)

- Windows 8 (32-bitowej i 64-bitowa)

- System Windows 8.1 (32-bitowej i 64-bitowa)

- System Windows Server 2012 (tylko wersja 64-bitowa)

- Windows Server 2012 R2 (tylko wersja 64-bitowa)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Czy można używać dowolnego klienta VPN oprogramowania dla punktu do witryny, która obsługuje SSTP?

Wartość nie. Pomoc techniczna jest ograniczona tylko do wersji systemu operacyjnego Windows wymienionych powyżej.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Ile punktów końcowych klienta VPN może mieć w mojej konfiguracji punktu do witryny?

Firma Microsoft obsługuje maksymalnie 128 klienci sieci VPN, aby można było połączyć się z siecią wirtualną w tym samym czasie.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Czy mogę używać własnej wewnętrznego głównego infrastruktury kluczy publicznych urzędu certyfikacji łączności punktu do witryny?

Wartość Tak. Wcześniej można użyć tylko certyfikatów głównych podpisem. Nadal możesz przekazać 20 certyfikatów głównych.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Czy mogę przechodzenie przez serwery proxy i zapory za pomocą funkcji punktu do witryny?

Wartość Tak. Aby tunelem za pośrednictwem zapory korzystamy SSTP (Secure Socket Tunneling Protocol). Ten tunelem będzie wyświetlany jako połączenie HTTPs.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Jeśli ponownie uruchomić komputer kliencki skonfigurowane dla punktu do witryny, sieci VPN automatycznie nawiąże?

Domyślnie komputer kliencki będzie nie przywrócić połączenie VPN automatycznie.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Czy punkt do witryny pomocy technicznej automatyczne-ponownie nawiązać połączenie i DDNS na klientach sieci VPN?

Ponownie Połącz automatycznie i DDNS obecnie nie są obsługiwane w sieci VPN punktu do witryny.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Witryny do witryny może mieć i występować konfiguracji punktu do witryn dla tej samej sieci wirtualnej?

Wartość Tak. Oba te rozwiązania będzie działać, jeśli masz typ RouteBased VPN Centrum. Model klasyczny wdrażania należy dynamiczne bramy. Czy firma Microsoft nie witrynie pomocy technicznej punkt do-statycznego routingu bram VPN lub bramy przy użyciu - VpnType PolicyBased.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Czy można skonfigurować klienta punktu do witryny do nawiązywanie połączenia z wieloma wirtualnych sieci, w tym samym czasie?

Tak, jest możliwe. Wirtualnych sieci nie mogą mieć nakładające się prefiksy adresów IP, a spacje adres punktu do witryny nie musi zachodzi między wirtualnych sieci.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Ile przepustowości można oczekiwać przy użyciu połączeń witryny do witryny lub punkt do witryny?

Trudno Obsługa dokładnie przepustowość tuneli VPN. IPsec i SSTP są protokoły sieci VPN dużej szyfrowania. Również przepustowość jest ograniczona opóźnienie i przepustowość między sieci lokalnej i w Internecie.

## <a name="gateways"></a>Bramy

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Co to jest bramy na podstawie zasad (kierowanie statyczne)?

Na podstawie zasad bram zaimplementować na podstawie zasad sieci VPN. Na podstawie zasad sieci VPN szyfrowanie i bezpośredniego pakiety przez tunele IPsec na podstawie kombinacji prefiksów adresów między sieci lokalnej i Azure VNet. Zasady (lub selektor ruch) jest zazwyczaj definiowana jako listy dostępu w konfiguracji sieci VPN.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Co to jest oparte na rozsyłania bramy (kierowanie dynamiczne)?

Oparte na rozsyłania bram wdrożenie sieci VPN oparte na trasę. Sieci VPN oparte na rozsyłania za pomocą "przekierowuje" w IP przekazywania lub tabeli routingu pakietów bezpośrednio do ich odpowiednie interfejsy tunelem. Następnie interfejsów tunelem szyfrowanie lub odszyfrowywanie pakietów i tunele. Selektor zasad lub ruch sieci VPN rozsyłania podstawie są skonfigurowane jako dowolna z każdym (lub symbole wieloznaczne).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Gdzie mogę uzyskać Mój adres IP bramy sieci VPN, zanim go utworzyć?

Wartość nie. Musisz utworzyć najpierw uzyskanie uzyskać adres IP. Jeśli usunięcie i ponowne utworzenie bramy sieci VPN zmiany adresu IP.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Sposób uzyskiwanie uwierzytelniania tunelem mojej sieci VPN?

Azure VPN używane uwierzytelnianie PSK (klucz wstępny). Firma Microsoft generowania klucza wstępnego (PSK) tworzymy tunelem VPN. Możesz zmienić PSK generowane automatycznie do własnego za pomocą polecenia cmdlet programu ustaw wstępny klucz PowerShell lub interfejsu API usługi REST.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Czy za pomocą interfejsu API ustawić klucz wstępny można skonfigurować Moje na podstawie zasad (routing statyczny) bramy sieci VPN?

Tak, ustaw wstępny klucz i interfejsu API programu PowerShell polecenia cmdlet może służyć do skonfigurowania Azure na podstawie zasad (statycznym) sieci VPN i rozsyłania (dynamicznego) routingu sieci VPN oparte na.

### <a name="can-i-use-other-authentication-options"></a>Czy można używać metod uwierzytelniania?

Firma Microsoft są ograniczone do używania kluczy wstępnych (PSK) do uwierzytelniania.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Co to jest "podsieci bramy" i dlaczego jest ona potrzebna?

Mamy uruchamiające łączność między lokalnej usługi bramy. 

Musisz utworzyć podsieć bramy dla swojego VNet skonfigurować bramę VPN. Wszystkie podsieci bramy musi mieć nazwę GatewaySubnet działał poprawnie. Nie nazwę danej podsieci bramy czegoś innego. I nie wdrażanie maszyny wirtualne lub innych elementów do podsieci bramy.

Minimalny rozmiar podsieci bramy zależy od całkowicie konfiguracji, która ma zostać utworzona. Mimo że istnieje możliwość utworzenia jak /29 w przypadku niektórych konfiguracji podsieci bramy, zaleca się utworzenie podsieci bramy /28 lub większa (/ 28 /27, /26 itp.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Do mojego podsieci bramy można wdrożyć maszyn wirtualnych lub wystąpień roli?

Wartość nie.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Jak określić, które przechodzi ruch przez bramę VPN?

Jeśli korzystasz z portalem klasyczny Azure, Dodaj każdy zakres, który ma być wysyłane przez bramę wirtualnej sieci na stronie sieci w obszarze sieci lokalnej.

### <a name="can-i-configure-forced-tunneling"></a>Można skonfigurować wymuszone Tunneling?

Wartość Tak. Zobacz [Konfigurowanie wymuszone tunelowania](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Można skonfigurować własnej serwer sieci VPN platformy Azure i go używać, aby nawiązać połączenie sieci lokalnej?

Tak, należy wdrożyć własne bram sieci VPN lub serwery platformy Azure z usługi Azure Marketplace lub tworzenie Routery VPN. Należy skonfigurować trasy zdefiniowane przez użytkownika w wirtualnej sieci upewnij się, że ruch jest przekierowywany prawidłowo między sieci lokalnej i z podsieci wirtualną sieć.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Dlaczego niektóre porty są otwierane w mojej sieci VPN brama?

Są one wymagane na potrzeby komunikacji Azure infrastruktury. (Zablokowane) przez Azure certyfikaty są chronione. Bez certyfikatów pisane z wielkiej litery podmiotów zewnętrznych, w tym klientów tych bram nie będzie mógł spowodować wpływu na tych punktów końcowych.

Brama VPN jest istotnie wieloadresowego urządzenia z jednego naciśnięcie NIC w sieci prywatnej klienta i jeden NIC przeciwległych publicznej sieci. Jednostki Azure infrastruktury nie naciśnij do klienta sieci prywatnych dla zachowania zgodności, więc potrzebują korzystanie z publicznej punkty końcowe komunikacji infrastruktury. Publiczne punkty końcowe są okresowo zeskanowane przez Azure inspekcji.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Więcej informacji na temat typów bramy, wymagania i przepustowość

Aby uzyskać więcej informacji zobacz [Temat ustawienia bramy sieci VPN](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Wiele witryn i łącznością VNet do VNet

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Jakiego typu bram mogą obsługiwać wiele witryn i łączności VNet do VNet?

Tylko podstawie rozsyłania VPN (kierowanie dynamiczne).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Z typem sieci VPN RouteBased do VNet innego typu PolicyBased VPN można połączyć VNet?

Wirtualnych sieci, nie należy używać oparte na rozsyłania (routing dynamiczny) sieci VPN.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Ruch VNet do VNet są bezpieczne?

Tak, jest chroniony przez szyfrowanie IPsec i IKE.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Czy VNet do VNet ruch podróży nad szkieletowego Azure?

Wartość Tak.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Ile lokalnych witryn i wirtualnych sieci mogą jednej wirtualnej sieci nawiązywać połączenie?

Maksymalna liczba. 10 łączny bram Basic i standardowy Routing dynamiczny; 30 dla bramy wysokiej wydajności sieci VPN.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Czy mogę sieci VPN punktu witryny korzystanie z sieci wirtualnych z wielu tuneli VPN?

Tak, można używać z bram VPN łączenia z wielu witryn lokalnego i innych wirtualnych sieci VPN punktu do witryny (P2S).

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Wiele tunele można konfigurować między wirtualnej sieci i mojej lokalnej witryny przy użyciu sieci VPN wiele witryn?

Nie, zbędne tunele między Azure wirtualnej sieci i lokalnej witryny nie są obsługiwane.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Mogą być widoczne nakładające się adres spacje między połączonego wirtualnych sieci i lokalne witryny lokalnej?

Wartość nie. Nakładające się spacje adres spowoduje przekazanie pliku konfiguracji sieci lub "Tworzenie wirtualnych sieci" kończy się niepowodzeniem.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Jak uzyskać większej przepustowości z więcej VPN witryny do witryny niż pojedynczy wirtualnej sieci?

Nie, wszystkie tuneli VPN, w tym sieci VPN punktu do witryny, udostępnianie tej samej bramie Azure VPN i przepustowości.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Czy można używać w ruchu tranzytowym między witrynami lokalnego lub do innego wirtualną sieć brama Azure VPN?

**Model klasyczny wdrażania**<br>
Ruch przewozowe za pośrednictwem sieci VPN Azure bramy jest możliwe przy użyciu modelu Klasyczny wdrożenia, ale zależy od spacji adres statycznie zdefiniowanych w pliku konfiguracji sieci. BGP nie jest jeszcze obsługiwane z Azure wirtualnych sieci i bram sieci VPN przy użyciu modelu Klasyczny wdrożenia. Bez BGP ręczne definiowanie spacje adres przewozowe jest bardzo błędu podatne i nie jest zalecane.<br>
**Model wdrażania Menedżera zasobów**<br>
Jeśli jest używany model wdrożenia Menedżera zasobów, zobacz sekcję [BGP](#bgp) , aby uzyskać więcej informacji.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Azure generuje samego klucza wstępnego IPsec/IKE Moje połączenia VPN dla tej samej sieci wirtualnej?

Nie, Azure domyślnie generuje różnych kluczy wstępnych dla różnych połączeń VPN. Za pomocą polecenia cmdlet programu PowerShell lub ustawianie VPN brama klucz interfejsu API usługi REST można jednak ustaw wartość klucza, które wolisz. Klucz musi być alfanumerycznej ciągiem o długości od 1 do 128 znaków.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Azure opłatę za ruch między wirtualnych sieci?

Ruchu między różnymi sieciami wirtualnych Azure Azure opłat tylko w przypadku ruch przechodzenie z jednego regionu Azure do drugiego. Stawka opłaty znajduje się na stronie Azure [Ceny bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) .


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Czy mogę nawiązać wirtualnej sieci z sieci VPN IPsec Moje elektrycznego ExpressRoute?

Tak, to jest obsługiwane. Aby uzyskać więcej informacji zobacz [Konfigurowanie ExpressRoute i połączenia VPN witryny do witryny, które występować](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Łączność między lokalnego i maszyny wirtualne

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Jeśli Moje maszyn wirtualnych znajduje się w wirtualnej sieci i mam połączenia między lokalnej, jak należy się połączyć do maszyn wirtualnych?

Dostępnych jest kilka opcji. Jeśli masz RDP włączony i utworzeniu punktu końcowego, umożliwia nawiązanie połączenia z maszyn wirtualnych przy użyciu VIP. W tym przypadku należy określić VIP oraz portu, który chcesz nawiązać połączenie. Musisz skonfigurować port na komputerze wirtualnych ruchu. Czy zazwyczaj przejdź do portalu klasyczny Azure i zapisać ustawień połączenia RDP na komputerze. Ustawienia zawierają niezbędnych informacji o połączeniu.

Jeśli masz wirtualną sieć z łącznością między lokalnej skonfigurowane umożliwia nawiązanie połączenia na komputerze wirtualnych za pomocą DIP wewnętrzny lub prywatny adres IP. Na komputerze wirtualnych przez DIP wewnętrznych można nawiązywania połączenia z innego komputera wirtualnej znajdującego się w tej samej sieci wirtualnej. Nie można RDP na komputerze wirtualnych przy użyciu DIP, jeśli łączysz się z lokalizacji poza siecią wirtualną. Na przykład jeśli masz punktu do witryny wirtualnej sieci skonfigurowany i nie możesz nawiązać połączenie ze swojego komputera, można połączyć maszyn wirtualnych przez DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Jeśli Moje maszyn wirtualnych znajduje się w wirtualnej sieci z łącznością między lokalnej, czy cały ruch z moim maszyn wirtualnych przejść przez to połączenie?

Wartość nie. Ruch miejsce docelowe przejdzie IP znajdującej się w zakresy adresów IP sieci lokalnej wirtualną sieć, określone przez bramę wirtualnej sieci. Ruch ma miejsce docelowe, które w wirtualnej sieci granicach IP znajdujących się w wirtualnej sieci. Inny ruch jest wysyłane za pośrednictwem usługi równoważenia obciążenia do publicznej sieci lub jeśli jest używane tunelowanie wymuszonego, wysyłane przez bramę Azure VPN. Jeśli rozwiązywania problemów, to należy się upewnić, że wszystkie zakresy wymienionych w Twojej sieci lokalnej, który ma zostać wysłana pocztą bramy. Upewnij się, że zakresy adresów sieci lokalnej nie pokrywają się z jednym z zakresów adresów w wirtualnej sieci. Ponadto chcesz zweryfikować, że właściwy adres IP serwera DNS, którego używasz jest rozpoznawania nazwy.


## <a name="virtual-network-faq"></a>Wirtualna sieć — często zadawane pytania

Można wyświetlić informacje dodatkowe wirtualną sieć w [Wirtualnej sieci — często zadawane pytania](../virtual-network/virtual-networks-faq.md).
 
