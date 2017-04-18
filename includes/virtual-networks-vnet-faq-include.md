## <a name="virtual-network-basics"></a>Wirtualna sieć — informacje podstawowe

### <a name="what-is-an-azure-virtual-network-vnet"></a>Co to jest Azure wirtualnej sieci (VNet)?

Możesz za pomocą VNets Obsługa administracyjna i zarządzać wirtualnych sieci prywatnych (VPN) platformy Azure i, opcjonalnie łącze VNets z innych VNets platformy Azure lub z sieci lokalnej infrastruktury informatycznej do tworzenia rozwiązań hybrydowego lub między lokalnej. Każdy VNet utworzone ma własny blok CIDR i mogą być połączone z innych sieciach VNets i lokalnych, jak bloków CIDR nie kolidują. Masz również kontrolek ustawienia serwera DNS dla VNets i podziału VNet na podsieci.

Za pomocą VNets do:

- Tworzenie dedykowane tylko do chmury wirtualnej sieci prywatnej

    Czasami nie wymagają konfiguracji lokalnej krzyżowe dotyczących rozwiązania. Po utworzeniu VNet swoich usług i maszyny wirtualne w ramach usługi VNet można komunikować się bezpośrednio i bezpiecznie ze sobą w chmurze. Zachowuje ruch bezpiecznie w VNet, ale nadal można skonfigurować połączenia punkt końcowy dla maszyny wirtualne i usług, które wymagają komunikacji internetowej jako część rozwiązania.

- Bezpieczne rozszerzanie centrum danych

    Przy użyciu VNets można tworzyć tradycyjna sieci VPN (S2S) witryny do witryny bezpieczne przeskalować wydajność centrum danych. Za pomocą IPSEC VPN S2S zapewnić bezpieczne połączenie między Brama VPN firmy i Azure.

- Włączanie scenariuszy hybrydowego chmury

    VNets uzyskać możliwość obsługuje szeroką gamę scenariuszy hybrydowego chmury. Możesz bezpiecznie połączyć aplikacje oparte na chmurze dowolny typ systemu lokalnego, takich jak komputerów typu mainframe i komputerach z systemem Unix.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Jak ustalić, czy potrzeba wirtualnej sieci?

Odwiedź stronę [Omówienie wirtualnych sieci](../articles/virtual-network/virtual-networks-overview.md) , aby wyświetlić tabelę decyzji, która pomogą zdecydować najlepszym rozwiązaniem projektu sieci dla Ciebie.

### <a name="how-do-i-get-started"></a>Jak rozpocząć?

Można znaleźć w [dokumentacji wirtualną sieć](https://azure.microsoft.com/documentation/services/virtual-network/) rozpocząć pracę. Ta strona zawiera łącza do typowych czynności konfiguracyjne, a także informacje, która pomoże zrozumieć czynności, które należy wziąć pod uwagę podczas projektowania wirtualnej sieci.

### <a name="what-services-can-i-use-with-vnets"></a>Jakie usługi można używać z VNets?

VNets można używać z szereg różnych usługi Azure, takie jak usług w chmurze (PaaS), maszyn wirtualnych i aplikacji sieci Web. Istnieją jednak kilka usług, które nie są obsługiwane w VNet. Sprawdź, czy określonej usługi, których chcesz użyć i sprawdź, czy jest zgodne.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Czy mogę używać VNets bez połączenia między lokalnej?

Wartość Tak. Za pomocą VNet bez użycia witryny do witryny łączności. Jest to szczególnie przydatne, jeśli chcesz uruchomić kontrolery domeny i farmy programu SharePoint w Azure.

## <a name="virtual-network-configuration"></a>Konfigurowanie sieci wirtualnej

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Z jakich narzędzi służy do tworzenia VNet?

Następujące narzędzia pozwala utworzyć lub skonfigurować wirtualnej sieci:

- Azure Portal (dla classic i VNets Menedżera zasobów).

- Plik konfiguracyjny sieci (netcfg - tylko klasyczny VNets). Zobacz [Konfigurowanie sieci wirtualnej za pomocą pliku konfiguracji sieci](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (dla classic i VNets Menedżera zasobów).

- Polecenie Azure (dla classic i VNets Menedżera zasobów).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Jakie zakresy adresów można korzystać w moim VNets?

Za pomocą publicznej zakresy adresów IP i wszelkie zdefiniowane w [RFC 1918](http://tools.ietf.org/html/rfc1918)zakres adresów IP.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Czy mogę używać publicznych adresów IP w moim VNets?

Wartość Tak. Aby uzyskać więcej informacji na temat publicznej zakresy adresów IP Zobacz [obszar adresów IP publicznej w wirtualnej sieci (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Pamiętaj o swojej publicznej adresy IP nie będzie dostępna bezpośrednio z Internetu.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Czy istnieje limit liczby podsieci w wirtualnej sieci?

Nie istnieje limit liczby podsieci używanych w VNet. Wszystkie podsieci muszą być w pełni zawarte w przestrzeni adresów wirtualnych sieci i nie powinny się nakładać ze sobą.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Czy istnieją jakiekolwiek ograniczenia dotyczące używania adresów IP w tych podsieci?

Azure zastrzega sobie niektóre adresy IP w każdej podsieci. Imię i nazwisko adresów IP podsieci są zarezerwowane dla zgodność Protocol (protokół), oraz 3 więcej adresów na potrzeby usług Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Jak małe i jak duże może być VNets i podsieci?

Najmniejsza podsieci, które obsługujemy jest /29 i największych jest /8 (za pomocą definicji podsieci CIDR).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Zaimportować Moje VLAN Azure za pomocą VNets?

Wartość nie. VNets są nakładki Layer 3. Azure nie obsługuje dowolny znaczeń właściwych Layer-2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Na moim VNets i podsieci można określić niestandardowe zasady routingu?

Wartość Tak. Umożliwia użytkownikowi zdefiniowane routingu (UDR). Aby uzyskać więcej informacji o UDR odwiedź stronę [drogi zdefiniowane przez użytkownika i przekazywanie adresów IP](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets obsługują multiemisji lub emisji?

Wartość nie. Firma Microsoft nie obsługują multiemisji lub emisji.

### <a name="what-protocols-can-i-use-within-vnets"></a>Jakie protokoły można używać w VNets?

Możesz użyć standardowych protokołów opartego na protokole w VNets. Jednak pakiety encapsulated multiemisji, emisji, IP w IP i pakiety ogólnego Routing Encapsulation (GRE) są blokowane w VNets. Standardowe protokoły, które działają obejmują:

- PORT TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Czy można ping Moje routery domyślne w VNet?

Wartość nie.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Czy można używać do diagnozowania łączności tracert?

Wartość nie.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Czy mogę dodawać podsieci, po utworzeniu VNet?

Wartość Tak. Podsieci można dodawać do VNets w dowolnym momencie jak adres podsieci nie jest częścią innej podsieci VNet.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Rozmiar mojej podsieci można modyfikować po jego utworzeniu?

Można dodawać, usuwać, rozwiń lub Zmniejsz podsieci, jeśli nie ma żadnych maszyny wirtualne lub usług wdrożony w nim przy użyciu poleceń cmdlet programu lub pliku NETCFG. Można również dodawanie, usuwanie, rozwiń lub Zmniejsz wszystkie prefiksy, jak podsieci, które zawierają maszyny wirtualne lub usług nie dotyczy zmiany.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Podsieci można zmodyfikować po ich utworzeniu?

Wartość Tak. Można dodawać, usuwania i modyfikowania bloków CIDR używane przez VNet.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Mogę nawiązywać połączenie internetowe jest aktywne, jeśli korzystam z moimi usługami w VNet?

Wartość Tak. Wszystkie usługi wdrożony w VNet można łączenie się z Internetem. Co usługa w chmurze wdrożony w Azure ma publicznie adresach VIP do niego przypisana. Należy zdefiniować wprowadzania punkty końcowe dla ról PaaS i dla maszyn wirtualnych włączania tych usług zaakceptować połączenia z Internetem.

### <a name="do-vnets-support-ipv6"></a>VNets obsługują protokół IPv6?

Wartość nie. W tej chwili nie można używać protokołu IPv6 VNets.

### <a name="can-a-vnet-span-regions"></a>VNet może obejmować regionów?

Wartość nie. VNet jest ograniczona do jednego regionu.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Czy mogę nawiązać VNet innego VNet platformy Azure?

Wartość Tak. Możesz utworzyć VNet VNet komunikacji za pomocą interfejsów API pozostałych lub środowiska Windows PowerShell. W przypadku nawiązywania połączenia VNets za pośrednictwem zaglądanie VNet. Dowiedz się więcej o zaglądanie [tutaj.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Rozpoznawanie nazw (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Jakie są opcje DNS dla VNets?

Umożliwia Tabela decyzji na stronie [Rozpoznawanie nazw maszyny wirtualne i wystąpień roli](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) poprowadzą Cię dostępne opcje DNS.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Dla VNet można określić serwery DNS?

Wartość Tak. W obszarze Ustawienia VNet można określić adresy IP serwera DNS. To będą stosowane serwery DNS domyślne dla wszystkich maszyny wirtualne w VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Ile serwerów DNS można określić?

Można określić do 12 serwerów DNS.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Moje serwery DNS można modyfikować po utworzeniu mają sieci?

Wartość Tak. Na liście serwera DNS dla swojej VNet można zmienić w dowolnym momencie. Jeśli zmienisz lista serwerów DNS, będzie konieczne ponowne uruchomienie każdej maszyny wirtualne w swojej VNet w kolejności ich do pobrania nowy serwer DNS.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Co to jest DNS dostarczonego przez Azure i działa z VNets?

DNS dostarczonego przez Azure to usługa DNS dzierżawy wielu oferowanych przez firmę Microsoft. Azure rejestruje wszystkie maszyny wirtualne i wystąpienia ról w tej usłudze. Ta usługa udostępnia rozpoznawanie nazw przez hostname maszyny wirtualne i wystąpień roli znajdujących się w tym samym usługi w chmurze i FQDN maszyny wirtualne i wystąpień ról w tym samym VNet.

> [AZURE.NOTE] Istnieje ograniczenie w tym czasie pierwszego usług w chmurze 100 w wirtualnej sieci rozpoznawania nazw między dzierżawy za pomocą dostarczonego przez Azure DNS. Jeśli korzystasz z serwera DNS, to ograniczenie nie ma zastosowania.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Można można zastąpić ustawienia DNS na na maszyny / usługi podstawa

Wartość Tak. Serwery DNS można ustawić na podstawie usługi na chmurze, aby zastąpić domyślne ustawienia sieci. Jednak zaleca się, że używasz możliwie DNS sieci.

### <a name="can-i-bring-my-own-dns-suffix"></a>Czy można wyświetlić własnej sufiks DNS?

Wartość nie. Nie można określić niestandardowe sufiks DNS dla usługi VNets.

## <a name="vnets-and-vms"></a>VNets i maszyny wirtualne

### <a name="can-i-deploy-vms-to-a-vnet"></a>Aby VNet można wdrożyć pośrednictwem SMS?

Wartość Tak.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Aby VNet można wdrożyć pośrednictwem Linux SMS?

Wartość Tak. Można wdrażać dowolne distro Linux obsługiwanych przez Azure.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Jaka jest różnica między VIP publicznej i wewnętrzny adres IP?

- Wewnętrzny adres IP jest adres IP przypisywany do każdego maszyn wirtualnych w VNet przez DHCP. Nie jest publicznej witrynie. Jeśli utworzono VNet wewnętrzny adres IP przypisano z zakresu określonego w ustawieniach podsieci usługi VNet. Jeśli nie masz VNet, nadal można przypisywać wewnętrzny adres IP. Wewnętrzny adres IP pozostaną z maszyn wirtualnych dla jego istnienia, chyba że który maszyn wirtualnych jest alokację.

- VIP publicznej jest publiczny adres IP, przypisany do chmury równoważenia usługi lub załaduj. Nie przydzielono się bezpośrednio do swojej maszyn wirtualnych karty sieciowej. VIP pozostanie przy usługi w chmurze, który jest przydzielone do wszystkich maszyny wirtualne w tej usłudze w chmurze są alokację lub usunięte. W tym momencie jest publikowany.

### <a name="what-ip-address-will-my-vm-receive"></a>Jaki adres IP otrzymają Moje maszyn wirtualnych?

- **Adres IP wewnętrznego-** Po zainstalowaniu maszyny do VNet maszyn wirtualnych otrzymuje wewnętrzny adres IP z puli adresów IP przez użytkownika. Maszyny wirtualne komunikowanie się w VNets za pomocą adresów IP. Azure przypisuje dynamiczne wewnętrzny adres IP, możesz jednak zażądać statyczny adres dla swojego maszyn wirtualnych. Aby dowiedzieć się więcej na temat statycznych adresów IP, odwiedź stronę [jak ustawić statyczny adres IP wewnętrznego](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Usługi maszyn wirtualnych również jest skojarzony z VIP, mimo że VIP nigdy nie przydzielono maszyn wirtualnych bezpośrednio. VIP jest publiczny adres IP, które mogą być przypisane do usługi w chmurze. Można opcjonalnie rezerwowanie VIP dla usługi w chmurze.

- **ILPIP-** Można również skonfigurować poziom wystąpienia publiczny adres IP (ILPIP). ILPIPs są bezpośrednio związane z maszyn wirtualnych, zamiast usługi w chmurze. Aby dowiedzieć się więcej na temat ILPIPs, odwiedź stronę [Poziomu wystąpienia publicznej omówienie adresów IP](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Czy można zarezerwować wewnętrzny adres IP, dla maszyn wirtualnych, który będzie można utworzyć w późniejszym czasie?

Wartość nie. Nie można zarezerwować wewnętrzny adres IP. Jeśli wewnętrzny adres IP jest dostępna zostanie przypisany do maszyn wirtualnych lub roli wystąpienie przez serwerze. Tego maszyn wirtualnych może być lub może nie być, w którym chcesz wewnętrzny adres IP ma być przypisany do. Można jednak zmienić wewnętrzny adres IP utworzone maszyn wirtualnych do dowolnego dostępne wewnętrzny adres IP.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Wykonaj wewnętrzny zmiana adresów IP dla maszyny wirtualne w VNet?

Wartość Tak. Z maszyn wirtualnych dla użytkowania pozostaną adresów IP, chyba że jest alokację maszyn wirtualnych. Gdy maszyny jest alokację, wewnętrzny adres IP zwolnieniu, chyba że zdefiniowanych wewnętrznych statycznego adresu IP dla usługi maszyn wirtualnych. Jeśli maszyn wirtualnych jest po prostu zatrzymać (a nie umieszczać w stanie **zatrzymania (Deallocated)**) z adresem IP pozostanie przydzielony do maszyn wirtualnych.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Czy mogę ręcznie przypisać adresy IP do nic w pośrednictwem SMS?

Wartość nie. Nie można zmienić dowolne właściwości interfejsu maszyny wirtualne. Wszelkie zmiany mogą prowadzić do potencjalnie utracie łączności do maszyn wirtualnych.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Co się stanie z moim adresów IP wyłączenie I maszyny?

Brak. Adresy IP (VIP publicznej i wewnętrzny adres IP) pozostanie z usługi w chmurze lub maszyn wirtualnych.

> [AZURE.NOTE] Jeśli chcesz po prostu zamknij maszyn wirtualnych, nie używaj portalu zarządzania to zrobić. Przycisk Zamknij będzie obecnie cofnąć maszyny wirtualnej.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Czy można przenosić maszyny wirtualne z jednej podsieci do innej podsieci w VNet bez ponownego wdrażania?

Wartość Tak. Więcej informacji można znaleźć [w tym miejscu](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Dla mojej maszyn wirtualnych można skonfigurować statyczny adres MAC?

Wartość nie. Adres MAC nie można skonfigurować statycznie.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Adres MAC jest taka sama dla mojej maszyn wirtualnych po jego utworzeniu?

Tak, adres MAC jest taka sama dla maszyny mimo że maszyn wirtualnych został zatrzymany (deallocated) i dalszymi.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Można łączyć się z Internetem z maszyn wirtualnych w VNet?

Wartość Tak. Wszystkie usługi wdrożony w VNet można łączenie się z Internetem. Ponadto każdej usługi w chmurze wdrożony w Azure ma publicznie adresach VIP do niego przypisana. Należy zdefiniować wprowadzania punkty końcowe dla ról PaaS i dla maszyny wirtualne włączania tych usług zaakceptować połączenia z Internetem.

## <a name="vnets-and-services"></a>VNets i usług

### <a name="what-services-can-i-use-with-vnets"></a>Jakie usługi można używać z VNets?

Można używać tylko usługi obliczeń w VNets. Do uruchamiania usługi są ograniczone do usług w chmurze (ról w sieci web i pracowników) i maszyny wirtualne.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Czy mogę korzystać z wirtualną sieć Web Apps?

Wartość Tak. Można wdrażać aplikacje sieci Web wewnątrz VNet przy użyciu ASE (Środowisko usługi aplikacji). Dodawanie do tej, aplikacje sieci Web można bezpiecznie łączyć i uzyskać dostęp do swojego VNet Azure zasobów, jeśli masz skonfigurowane dla swojego VNet punktu do witryny. Aby uzyskać więcej informacji zobacz:


- [Tworzenie aplikacji sieci Web w środowisku usługi aplikacji](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Integracja wirtualną sieć aplikacji sieci Web](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Przy użyciu integracji VNet i hybrydowego z aplikacjami sieci Web](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integracja aplikacji sieci web z wirtualną sieć Azure](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Czy można zainstalować usług w chmurze z sieci web i pracownik ról (PaaS) w VNet?

Wartość Tak. Można wdrażać usługi PaaS w VNets.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Jak wdrożyć role PaaS na VNet?

Można to zrobić, określając nazwę VNet i mapowań /subnet ról w sekcji Konfiguracja sieci konfiguracji usługi. Nie ma potrzeby aktualizacji usługi plików binarnych.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Czy mogę przenieść Moje usług i VNets?

Wartość nie. Nie można przenosić usług i VNets. Należy usunąć i ponownie wdrożyć usługę, aby przenieść ją do innego VNet.

## <a name="vnets-and-security"></a>VNets i zabezpieczeń

### <a name="what-is-the-security-model-for-vnets"></a>Co to jest model zabezpieczeń systemu VNets?

VNets są całkowicie odizolowane od siebie i inne usługi obsługiwane w infrastrukturze Azure. VNet jest obramowanie zaufania.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Na moim VNets można zdefiniować list ACL lub NSGs?

Wartość nie. Nie można skojarzyć list ACL lub NSGs do VNets. Jednak ACL można zdefiniować na wprowadzania punkty końcowe dla maszyny wirtualne wdrożonych VNets i NSGs można skojarzyć z podsieci lub nic.

### <a name="is-there-a-vnet-security-whitepaper"></a>Czy istnieje dokument zabezpieczeń VNet?

Wartość Tak. Możesz pobrać ją [tutaj](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>Interfejsy API, schematy i narzędzia

### <a name="can-i-manage-vnets-from-code"></a>W kodzie można zarządzać VNets?

Wartość Tak. Interfejsy API pozostałych umożliwia zarządzanie VNets i lokalnej krzyżowe łączności. Więcej informacji można znaleźć [w tym miejscu](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Czy istnieje narzędzia obsługę VNets?

Wartość Tak. Za pomocą narzędzi programu PowerShell i wiersza polecenia dla różnych platform. Więcej informacji można znaleźć [w tym miejscu](http://go.microsoft.com/fwlink/?LinkId=317721).
