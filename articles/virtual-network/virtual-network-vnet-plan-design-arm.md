<properties
   pageTitle="Plan Azure wirtualnej sieci (VNet) i Podręcznik projektowania | Microsoft Azure"
   description="Dowiedz się, jak planowanie i projektowanie wirtualnych sieci platformy Azure zgodnie z własnymi potrzebami izolacji, łączności i lokalizacji."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Planowanie i projektowanie Azure wirtualnych sieci

Tworzenie VNet wypróbowanie to dość proste, ale prawdopodobnie oznacza to, wielu VNets zostanie wdrożony w czasie do obsługi wymagań produkcji danej organizacji. W przypadku niektórych planowania i projektowanie można wdrażać VNets i łączenie wydajniejsze potrzebne zasoby. Jeśli nie znasz VNets, jest zalecane, [Dowiedz się więcej o VNets](virtual-networks-overview.md) i [jak wdrożyć](virtual-networks-create-vnet-arm-pportal.md) jedną przed kontynuowaniem. 

## <a name="plan"></a>Planowanie

Niezwykle ważne praktyczne jest dokładnie zapoznać się z subskrypcji Azure, regiony i zasobów. Na liście zagadnienia poniżej można użyć jako punktu początkowego. Po zapoznaj się z tymi zagadnieniami dotyczącymi, można zdefiniować wymagania dla projektu sieci.

### <a name="considerations"></a>Zagadnienia dotyczące

Przed odbierania podczas planowania pytania poniżej, należy rozważyć następujące kwestie:

- Wszystkie obiekty, które można tworzyć w Azure składa się z jednego lub kilku zasobów. Maszyny wirtualnej (maszyn wirtualnych) jest zasobem karty sieciowej (NIC) używane przez maszyny jest zasobem, publiczny adres IP używane przez NIC jest zasobem, VNet jest połączony NIC jest zasobem.
- Możesz utworzyć zasoby w [regionie Azure](https://azure.microsoft.com/regions/#services) i subskrypcji. I zasobów może być połączony tylko z VNet, która istnieje w ustawieniach regionalnych i subskrypcji, które znajdują się w. 
- VNets można połączyć ze sobą za pomocą funkcji Azure [Brama VPN](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). Można także połączyć VNets różnych regionów i subskrypcje w ten sposób.
- VNets można nawiązać w sieci lokalnej, przy użyciu jednej z [opcji łączności](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) dostępnych w Azure. 
- Różne zasoby można grupować w [grupy zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups), co ułatwia zarządzanie zasobu jako jednostki. Grupa zasobów może zawierać zasoby z wielu regionów, ile zasobów należą do tej samej subskrypcji.

### <a name="define-requirements"></a>Definiowanie wymagań

Użyj pytania poniżej jako punktu startowego dla projektu sieci Azure.  

1. Jakie Azure lokalizacje będziesz używać hosta VNets?
2. Należy zapewnienie komunikacji między następujących lokalizacji Azure?
3. Należy zapewnienie komunikacji między VNet(s) usługi Azure i datacenter(s) sieci lokalnej?
4. Ile infrastruktury jako maszyny wirtualne usługi (IaaS), chmury usługi ról i, czy aplikacje sieci web potrzebnych dla tego rozwiązania?
5. Należy określić ruch na podstawie grup maszyny wirtualne (to znaczy frontonu sieci web serwerów i wewnętrznej bazy danych serwerów)?
6. Należy do sterowania ruch przy użyciu urządzenia wirtualnego?
7. Użytkownicy będą różnych zestawów uprawnień do różnych zasobów Azure?

### <a name="understand-vnet-and-subnet-properties"></a>Opis właściwości VNet i podsieci

VNet i podsieci zasoby pomocy Definiowanie granicę zabezpieczeń dla obciążenia działa Azure. VNet jest określony przez zbiór spacje adres, zdefiniowany CIDR bloków. 

>[AZURE.NOTE] Administratorzy sieci znają notacji CIDR. Jeśli nie znasz CIDR, [Dowiedz się więcej o](http://whatismyipaddress.com/cidr).

VNets zawiera następujące właściwości.

|Właściwość|Opis|Ograniczenia|
|---|---|---|
|**Nazwa**|Nazwa VNet|Ciąg znaków do 80. Może zawierać liter, cyfr, podkreślenie, kropek lub łączniki. Musi rozpocząć się od litery lub cyfry. Musi się kończyć literę, liczbę albo podkreślenia. Czy zawiera górnym lub małe litery.|  
|**Lokalizacja**|Lokalizacja Azure (nazywane też regionu).|Należy wybrać jedną z prawidłowych lokalizacji Azure.|
|**addressSpace**|Kolekcja prefiksów adresów, które składają się VNet w notacji CIDR.|Musi być tablicą bloków prawidłowych CIDR adres, łącznie z publicznej zakresy adresów IP.|
|**podsieci**|Kolekcja podsieci, które składają się VNet|Zobacz poniższą tabelę właściwości podsieci.||
|**dhcpOptions**|Obiekt, który zawiera jedną właściwość wymagane o nazwie **dnsServers**.||
|**dnsServers**|Tablica serwerów DNS używane przez VNet. Jeśli serwer nie zostanie określony, jest używana rozpoznawania nazw wewnętrznych Azure.|Musi być tablicą maksymalnie 10 serwerów DNS według adresów IP.| 

Podsieć jest zasób podrzędne VNet i ułatwia określ segmenty przestrzeni adresów w bloku CIDR, przy użyciu prefiksy adresów IP. NIC mogą być dodawane do podsieci lub połączony z pośrednictwem SMS, dostarczając łączności dla różnych obciążenia.

Podsieci zawiera następujące właściwości. 

|Właściwość|Opis|Ograniczenia|
|---|---|---|
|**Nazwa**|Nazwa podsieci|Ciąg znaków do 80. Może zawierać liter, cyfr, podkreślenie, kropek lub łączniki. Musi rozpocząć się od litery lub cyfry. Musi się kończyć literę, liczbę albo podkreślenia. Czy zawiera górnym lub małe litery.|
|**Lokalizacja**|Lokalizacja Azure (nazywane też regionu).|Należy wybrać jedną z prawidłowych lokalizacji Azure.|
|**addressPrefix**|Prefiks jeden adres, które składają się podsieci w notacji CIDR|Musi być pojedynczy blok CIDR należącym do jednego z VNet adres spacje.|
|**networkSecurityGroup**|NSG stosowane do danej podsieci|zobacz [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Tabelę routingu stosowane do danej podsieci|zobacz [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Kolekcja obiektów konfiguracji adresów IP używanych przez nic podłączony do podsieci|zobacz [Konfiguracja IP](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Rozpoznawanie nazw

Domyślnie korzysta z usługi VNet [Rozpoznawanie nazw dostarczonego przez Azure.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) rozpoznawanie nazw w VNet, a następnie na publicznego Internetu. Jednak jeśli Twojej VNets możesz połączyć się z centrami danych lokalnych, należy podać [serwera DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) do rozpoznawania nazw między sieci.  

### <a name="limits"></a>Ograniczenia

Przeglądanie sieci limity w artykule [ogranicza Azure](../azure-subscription-service-limits.md#networking-limits) , aby upewnić się, że projekt nie powodować konflikt z wymienionych limitów. Pewne ograniczenia można zwiększyć, otwierając bilet pomocy technicznej.

### <a name="role-based-access-control-rbac"></a>Kontrola dostępu oparta na rolach (RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) umożliwia kontrolowanie poziomu dostępu, które różnych użytkowników może być konieczne różnych zasobów w Azure. W ten sposób można oddzielnie różnic pracy przez zespół zgodnie z potrzebami. 

Jeśli chodzi o wirtualnych sieci, użytkownicy w roli **Współautora sieci** mają pełną kontrolę nad Menedżera zasobów Azure wirtualną sieć zasobów. Podobnie użytkowników w roli **Współautora klasyczny sieci** mają pełną kontrolę nad klasyczny wirtualną sieć zasobów.

>[AZURE.NOTE] Możesz również [tworzyć własne role](../active-directory/role-based-access-control-configure.md) oddzielanie potrzeb administracyjnych.

## <a name="design"></a>Projektowanie

Znając odpowiedzi na pytania w sekcji [Planowanie](#Plan) , przejrzyj przed definiowanie usługi VNets.

### <a name="number-of-subscriptions-and-vnets"></a>Liczba subskrypcji i VNets

Warto rozważyć utworzenie wielu VNets w następujących sytuacjach:

- **Maszyny wirtualne, które muszą znajdować się w różnych lokalizacjach Azure**. VNets platformy Azure są regionalne. Ta osoba nie może obejmować lokalizacji. Dlatego konieczne co najmniej jeden VNet dla każdej Azure lokalizacji, w których chcesz maszyny wirtualne hosta w.
- **Obciążenie pracą, które muszą być całkowicie odizolowane od siebie**. Możesz utworzyć osobne VNets, który nawet używanie samego obszarach adresów IP w celu wyodrębnienia różne obciążenia od siebie. 

Należy pamiętać, ograniczenia, które są widoczne powyżej są w rozbiciu na regiony na subskrypcję. Oznacza to, że można zwiększyć limit zasobów, które można zachować platformy Azure wiele subskrypcji. Sieć VPN witryny do witryny lub obwód ExpressRoute umożliwia łączenie VNets w różnych subskrypcjach.

### <a name="subscription-and-vnet-design-patterns"></a>Subskrypcja i VNet wzorców projektu

W poniższej tabeli przedstawiono niektóre typowe wzorce projektu dotyczące korzystania z subskrypcji i VNets.

|Scenariusz|Diagram|Zalety|Ikony|
|---|---|---|---|
|Pojedynczy subskrypcji, dwa VNets na aplikacji|![Pojedynczy subskrypcji](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Tylko jedną subskrypcję do zarządzania.|Maksymalna liczba VNets rozbiciu na regiony Azure. Po wykonaniu tej potrzebujesz więcej subskrypcji. Zapoznanie się z artykułem [ogranicza Azure](../azure-subscription-service-limits.md#networking-limits) , aby uzyskać szczegółowe informacje.|
|Jedną subskrypcję na aplikacji, dwa VNets na aplikacji|![Pojedynczy subskrypcji](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Używa tylko dwa VNets na subskrypcję.|Trudniej zarządzać istnienia zbyt wiele aplikacji.|
|Jedną subskrypcję jednostkowa firm dwóch VNets dla aplikacji.|![Pojedynczy subskrypcji](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Równowaga między liczbę subskrypcji i VNets.|Maksymalna liczba VNets na jednostki biznesowej (subskrypcji). Zapoznanie się z artykułem [ogranicza Azure](../azure-subscription-service-limits.md#networking-limits) , aby uzyskać szczegółowe informacje.|
|Jedną subskrypcję jednostkowa firm dwóch VNets dla każdej grupy aplikacji.|![Pojedynczy subskrypcji](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Równowaga między liczbę subskrypcji i VNets.|Aplikacje musisz samodzielnie przy użyciu podsieci i NSGs.|


### <a name="number-of-subnets"></a>Liczba podsieci

Należy rozważyć kilka podsieci w VNet w następujących sytuacjach:

- **Mało prywatnych adresów IP dla wszystkich nic w podsieci**. Jeśli Twój obszar adres podsieci nie zawiera za mało adresów IP dla liczby nic w podsieci, należy utworzyć wiele podsieci. Należy pamiętać, że Azure zastrzega sobie 5 prywatnych adresów IP z każdej podsieci, której nie można używać: imię i nazwisko adresy obszaru adresów (adres podsieci i multiemisja) i adresy 3 może być używany wewnętrznie (dla potrzeb DHCP i DNS). 
- **Zabezpieczenia**. Oddzielanie grup maszyny wirtualne od siebie dla obciążenia, które mają strukturę wielowarstwową za pomocą podsieci i stosowanie różnych [grup zabezpieczeń sieci (NSGs)](virtual-networks-nsg.md#subnets) dla tych podsieci.
- **Hybrydowe łączności**. Umożliwia bram VPN i obwodów ExpressRoute [nawiązania połączenia](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) z VNets między sobą, a także center(s) danych z lokalnego. Bramy sieci VPN i obwodów ExpressRoute wymagają podsieci we własnym zakresie ma zostać utworzony.
- **Wirtualna urządzenia**. Za pomocą wirtualnej urządzenia, na przykład zapora, akceleratorów WAN lub Brama VPN w VNet Azure. Po wykonaniu tej czynności należy [routingu](virtual-networks-udr-overview.md) na tych urządzeniach i wyodrębnienia je w ich podsieci.

### <a name="subnet-and-nsg-design-patterns"></a>Podsieć i NSG desenie projektu

W poniższej tabeli przedstawiono niektóre typowe wzorce projektu dotyczące korzystania z podsieci.

|Scenariusz|Diagram|Zalety|Ikony|
|---|---|---|---|
|Pojedyncza podsieć NSGs na warstwie aplikacji dla aplikacji|![Pojedyncza podsieć](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Tylko jedna podsieć do zarządzania.|Wiele NSGs niezbędne w celu wyodrębnienia każdej aplikacji.|
|Jedna podsieć dla aplikacji, NSGs w warstwie aplikacji|![Podsieć dla aplikacji](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Mniej NSGs do zarządzania.|Wiele podsieci do zarządzania.|
|Jedna podsieć w warstwie aplikacji, NSGs na aplikacji.|![Podsieć w warstwie](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Równowaga między numerem podsieci i NSGs.|Maksymalna liczba NSGs na subskrypcję. Zapoznanie się z artykułem [ogranicza Azure](../azure-subscription-service-limits.md#networking-limits) , aby uzyskać szczegółowe informacje.|
|Jedna podsieć na warstwie aplikacji dla aplikacji, NSGs na podsieć|![Podsieć w warstwie na aplikacji](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Niekiedy mniejszej liczby NSGs.|Wiele podsieci do zarządzania.|

## <a name="sample-design"></a>Przykładowe projektu

Aby przedstawić stosowania informacje w tym artykule, rozważ następującym scenariuszu.

Pracujesz dla firmy, która ma 2 centrach danych w Ameryce Północnej i centrami danych dwóch Europe. Zidentyfikował 6 aplikacji przeciwległych innego klienta, obsługiwane przez 2 różnych jednostek biznesowych, które chcesz przeprowadzić migrację Azure jako wdrożenia pilotażowego. Podstawowa architektura aplikacji są następujące:

- App1, App2 App3 i App4 są obsługiwane na serwerach Linux uruchamiania Ubuntu aplikacji sieci web. Każda aplikacja nawiązuje połączenie z serwerem innej aplikacji obsługującego RESTful usługi na serwerach Linux. RESTful usług nawiązywanie połączenia z bazą danych MySQL wewnętrznej.
- App5 i App6 są obsługiwane na serwerach systemu Windows z systemem Windows Server 2012 R2 aplikacji sieci web. Każda aplikacja łączy do wewnętrznej bazy danych programu SQL Server.
- Wszystkie aplikacje są obecnie obsługiwane w jednym z centrami danych firmy w Ameryce Północnej.
- Centra danych lokalnych za pomocą 10.0.0.0/8 przestrzeni adresów.

Musisz Projektowanie rozwiązania wirtualnej sieci, która spełnia następujące wymagania:

- Użycie zasobów innych jednostek biznesowych nie narusza każdej jednostki biznesowej.
- Należy zminimalizować ilość VNets i podsieci, aby ułatwić zarządzanie.
- Każdej jednostki biznesowej powinien mieć pojedynczy test rozwoju VNet używane dla wszystkich aplikacji.
- Każdą z nich jest hostowana w 2 centrach danych Azure na kontynent (Europy i Ameryki Północnej).
- Każdą z nich jest całkowicie odizolowane od siebie.
- Każdą z nich jest możliwy przez klientów w Internecie przy użyciu protokołu HTTP.
- Każdą z nich jest możliwy przez użytkowników połączony z centrami danych lokalnych przy użyciu zaszyfrowanego tunelem.
- Połączenie z centrami danych lokalnych należy używać istniejących urządzeniach sieci VPN.
- Grupa sieci firmy powinna mieć pełną kontrolę nad konfiguracji VNet.
- Deweloperów w każdej jednostki biznesowej tylko powinno być możliwe wdrażanie maszyny wirtualne do istniejącego podsieci.
- Wszystkie aplikacje będą podlegać migracji są Azure (dźwigu i zmiany).
- Bazy danych w każdym miejscu powinien replikować do innych lokalizacji Azure raz dziennie.
- Każdą z nich należy użyć serwerach frontonu 5 w sieci web, 2 serwery aplikacji (w razie potrzeby) i 2 serwery bazy danych.

### <a name="plan"></a>Planowanie

Należy uruchomić projekt planowania odpowiedzi na pytanie w sekcji [Definiowanie wymagań](#Define-requirements) , tak jak pokazano poniżej.

1. Jakie Azure lokalizacje będziesz używać hosta VNets?

    2 lokalizacje w Ameryce Północnej i 2 lokalizacje w Europie. Należy wybrać te oparte na fizycznej lokalizacji do istniejących centrach danych lokalnych. Dzięki temu połączenie z położeń fizyczna Azure ma lepsze opóźnienie.

2. Należy zapewnienie komunikacji między następujących lokalizacji Azure?

    Wartość Tak. Ponieważ baz danych muszą być replikowane do wszystkich lokalizacji.

3. Należy zapewnienie komunikacji między VNet(s) usługi Azure i sieci lokalnej center(s) danych?

    Wartość Tak. Ponieważ użytkowników połączonych z centrami danych lokalnego muszą mieć możliwość dostęp do aplikacji w zaszyfrowanej tunelem.
 
4. Ile IaaS maszyny wirtualne należy rozwiązania?

    200 IaaS maszyny wirtualne. App1 i App2 App3 wymagać 5 serwerów sieci web, każdy, 2 aplikacji na każdym serwerze i 2 serwerów baz danych każdego. To jest liczba 9 IaaS maszyny wirtualne w każdej aplikacji lub 36 IaaS maszyny wirtualne. App5 i App6 wymagają 5 serwerów sieci web, jak i 2 serwerów bazy danych. To jest liczba 7 IaaS maszyny wirtualne w każdej aplikacji lub 14 IaaS maszyny wirtualne. Dlatego konieczne 50 IaaS maszyny wirtualne dla wszystkich aplikacji w każdym regionie Azure. Ponieważ należy użyć 4 regionów, będzie 200 IaaS maszyny wirtualne.

    Konieczne będzie także podać serwerów DNS w każdym VNet lub z centrami danych lokalnych do rozpoznawania nazw między maszyny wirtualne usługi Azure IaaS i sieci lokalnej. 

5. Należy określić ruch na podstawie grup maszyny wirtualne (to znaczy frontonu sieci web serwerów i wewnętrznej bazy danych serwerów)?

    Wartość Tak. Każdą z nich należy całkowicie odizolowane od siebie, a każdej warstwy aplikacji należy również odizolowanych. 

6. Należy do sterowania ruch przy użyciu urządzenia wirtualnego?

    Wartość nie. Wirtualna urządzenia umożliwia zapewniają większą kontrolę nad ruch, tym bardziej szczegółowe rejestrowanie płaszczyzny danych. 

7. Użytkownicy będą różnych zestawów uprawnień do różnych zasobów Azure?

    Wartość Tak. Sieci zespołu musi pełną kontrolę na wirtualnej ustawienia sieciowe, gdy deweloperów tylko powinno być możliwe wdrażanie ich maszyny wirtualne do istniejącego podsieci. 

### <a name="design"></a>Projektowanie

Należy wykonać projektu, określając subskrypcje, VNets podsieci i NSGs. NSGs zostanie tutaj omówimy, ale należy więcej informacji o [NSGs](virtual-networks-nsg.md) przed zakończeniem projektu.

**Liczba subskrypcji i VNets**

Następujące wymagania dotyczą subskrypcji i VNets:

- Użycie zasobów innych jednostek biznesowych nie narusza każdej jednostki biznesowej.
- Należy zminimalizować ilość VNets i podsieci.
- Każdej jednostki biznesowej powinien mieć pojedynczy test rozwoju VNet używane dla wszystkich aplikacji.
- Każdą z nich jest hostowana w 2 centrach danych Azure na kontynent (Europy i Ameryki Północnej).

W zależności od tych wymagań, należy subskrypcji dla każdej jednostki firm. W ten sposób zużycie zasobów z jednostki biznesowej nie będą wliczane do limity dla innych jednostek biznesowych. A ponieważ chcesz ograniczyć liczbę VNets, należy rozważyć użycie deseniu **jedną subskrypcję jednostkowa firm dwóch VNets dla każdej grupy aplikacji** , jak pokazano poniżej.

![Pojedynczy subskrypcji](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Należy określić przestrzeni adresów dla każdego VNet. Ponieważ potrzebujesz wyśrodkowanie łączność między danych lokalnych Azure regionów, adres miejsca dla Azure VNets nie mogą powodować konfliktów z sieci lokalnej i adres zajmowanego przez każdego VNet należy nie mogą powodować konfliktów z innych istniejących VNets. Obszary adresów w poniższej tabeli można użyć w celu spełnienia tych wymagań.  

|**Subskrypcji**|**VNet**|**Azure region**|**Przestrzeń adresów**|
|---|---|---|---|
|BU1|ProdBU1US1|Zachód Stany Zjednoczone|172.16.0.0/16|
|BU1|ProdBU1US2|Wschodniej Stany Zjednoczone|172.17.0.0/16|
|BU1|ProdBU1EU1|Europa Północna|172.18.0.0/16|
|BU1|ProdBU1EU2|Europa Zachodnia|172.19.0.0/16|
|BU1|TestDevBU1|Zachód Stany Zjednoczone|172.20.0.0/16|
|BU2|TestDevBU2|Zachód Stany Zjednoczone|172.21.0.0/16|
|BU2|ProdBU2US1|Zachód Stany Zjednoczone|172.22.0.0/16|
|BU2|ProdBU2US2|Wschodniej Stany Zjednoczone|172.23.0.0/16|
|BU2|ProdBU2EU1|Europa Północna|172.24.0.0/16|
|BU2|ProdBU2EU2|Europa Zachodnia|172.25.0.0/16|

**Liczba podsieci i NSGs**

Następujące wymagania dotyczą podsieci i NSGs:

- Należy zminimalizować ilość VNets i podsieci.
- Każdą z nich jest całkowicie odizolowane od siebie.
- Każdą z nich jest możliwy przez klientów w Internecie przy użyciu protokołu HTTP.
- Każdą z nich jest możliwy przez użytkowników połączony z centrami danych lokalnych przy użyciu zaszyfrowanego tunelem.
- Połączenie z centrami danych lokalnych należy używać istniejących urządzeniach sieci VPN.
- Bazy danych w każdym miejscu powinien replikować do innych lokalizacji Azure raz dziennie.

Oparte na tych wymagań, można użyć jednej podsieci w warstwie aplikacji i za pomocą NSGs do filtrowania ruchu w każdej aplikacji. Dzięki temu wystarczy, że podsieci 3 w każdym VNet (fronton, warstwy aplikacji i warstwa danych) i jeden NSG każdej aplikacji na podsieć. W tym przypadku należy uwzględnić, przy użyciu modelu **jednej podsieci w warstwie aplikacji, NSGs na aplikacji** . Na poniższym rysunku przedstawiono stosowania modelu reprezentującą **ProdBU1US1** VNet.

![Jedna podsieć w warstwie, jeden NSG każdej aplikacji w warstwie](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Jednak trzeba również utworzyć dodatkowe podsieci dla połączenia VPN między VNets i z centrami danych lokalnych. I musisz określić przestrzeni adresów dla każdej podsieci. Na poniższym rysunku przedstawiono rozwiązanie próbki dla **ProdBU1US1** VNet. W tym scenariuszu czy replikować dla każdego VNet. Każdy kolor reprezentuje innej aplikacji.

![Przykładowe VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Kontrola dostępu**

Aby uzyskać dostęp do sterowania odnoszą się następujące wymagania:

- Grupa sieci firmy powinna mieć pełną kontrolę nad konfiguracji VNet.
- Deweloperów w każdej jednostki biznesowej tylko powinno być możliwe wdrażanie maszyny wirtualne do istniejącego podsieci.

W zależności od tych wymagań, można dodać użytkowników z sieci zespołu do wbudowanych rola **Współautora sieci** w każdej subskrypcji; i utworzyć niestandardową rolę dla deweloperów aplikacji w każdej subskrypcji nadanie im uprawnienia do dodawania maszyny wirtualne do istniejącego podsieci.

## <a name="next-steps"></a>Następne kroki

- [Rozmieszczanie wirtualnej sieci](virtual-networks-create-vnet-arm-template-click.md) oparty na scenariusza.
- Opis sposobu [równoważenia obciążenia](../load-balancer/load-balancer-overview.md) maszyny wirtualne IaaS i [zarządzanie nimi routing przez wielu regionów Azure](../traffic-manager/traffic-manager-overview.md).
- Dowiedz się więcej o [NSGs i jak planowanie i projektowanie](virtual-networks-nsg.md) rozwiązania NSG.
- Dowiedz się więcej o [krzyżowe pomieszczeń i opcje połączeń VNet](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
