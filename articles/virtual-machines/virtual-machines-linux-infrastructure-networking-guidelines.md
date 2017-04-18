<properties
    pageTitle="Wskazówki dotyczące infrastruktury sieci | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie wdrażania wirtualnej sieci w usługach Azure infrastruktury."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Wskazówki dotyczące infrastruktury sieci

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono opis wymaganych czynności planowania łączności i wirtualnej sieci w Azure między istniejącymi środowiskami lokalna.


## <a name="implementation-guidelines-for-virtual-networks"></a>Zasady implementacji wirtualnych sieci

Decyzje:

- Jakiego rodzaju wirtualną sieć są potrzebne do obsługi infrastruktury lub z pracą IT (tylko do chmury lub między lokalne)?
- Wirtualnych sieci lokalnej krzyżowe ile miejsca adres należy do obsługi podsieci i maszyny wirtualne teraz i w przyszłości rozsądne rozszerzenia?
- Zamierzasz utworzyć scentralizowane wirtualnych sieci lub poszczególnych wirtualnych sieci dla każdej grupy zasobów?

Zadania:

- Definiowanie przestrzeni adresów wirtualnych sieci ma zostać utworzony.
- Definiowanie zestawu podsieci i przestrzeni adresów dla każdej z nich.
- Wirtualnych sieci lokalnej krzyżowe definiują zestaw przestrzeni adresów sieci lokalnej dla lokalizacji lokalnego, potrzebne do osiągnięcia maszyny wirtualne w wirtualnej sieci.
- Praca z lokalnej sieci zespołu w celu zapewnienia odpowiedniej routingu skonfigurowano podczas tworzenia krzyżowe lokalnej wirtualnych sieci.
- Tworzenie wirtualnych sieci przy użyciu Konwencja nazewnictwa.


## <a name="virtual-networks"></a>Wirtualnych sieci

Wirtualnych sieci są niezbędne do obsługi komunikacji między wirtualnych maszyn. Można zdefiniować podsieci, niestandardowe adres IP, ustawień DNS, filtrowanie zabezpieczeń i równoważenia obciążenia, podobnie jak w przypadku sieci fizycznych. Przy użyciu [Bramy sieci VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) lub [rozsyłania Express elektrycznego](../expressroute/expressroute-introduction.md), Azure wirtualnych sieci można nawiązać sieci lokalnej. Więcej informacji o [wirtualnych sieci i ich składników](../virtual-network/virtual-networks-overview.md).

Przy użyciu grup zasobów, masz elastyczność, w jaki sposób projekt składniki wirtualnej sieci. Maszyny wirtualne można nawiązać wirtualnych sieci poza własne grupy zasobów. Wspólne podejście do projektowania jest tworzenie grup scentralizowane zasobów, które zawierają infrastruktury sieci core można zarządzać przez zespół typowych. Maszyny wirtualne i ich zastosowań wdrożony oddzielnych grup zasobów. Ta metoda umożliwia dostęp właściciele aplikacji do grupy zasobów, która zawiera ich maszyny wirtualne bez otwierania dostępu do konfiguracji większej wirtualnych zasobów sieci.

## <a name="site-connectivity"></a>Łączność witryny

### <a name="cloud-only-virtual-networks"></a>Tylko do chmury wirtualnych sieci
Jeśli użytkowników lokalnych i komputerach nie wymagają trwających łączności z programem maszyny wirtualne w Azure wirtualnej sieci, projektu wirtualnej sieci to bezpośrednio do przodu:

![Diagram podstawowe wirtualną sieć tylko do chmury](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Ta metoda jest zazwyczaj dla obciążeń pracą z Internetu, takich jak serwer sieci web internetowych. Możesz zarządzać te maszyny wirtualne za pomocą SSH lub połączeń sieci VPN punktu do witryny.

Ponieważ nie łączą się sieci lokalnej, tylko do Azure wirtualnych sieci za pomocą dowolnej części obszaru prywatnych adresów IP. Przestrzeń adresów może być tego samego obszaru prywatne, który znajduje się w Użyj lokalnego.


### <a name="cross-premises-virtual-networks"></a>Wirtualnych sieci lokalnej krzyżowe
Jeśli użytkowników lokalnych i komputerach wymagają trwających łączności z programem maszyny wirtualne w Azure wirtualnej sieci, należy utworzyć wirtualnej sieci lokalnej krzyżowe. Nawiązywanie połączenia wirtualnej sieci z sieci lokalnej za pomocą ExpressRoute lub połączenie VPN witryny do witryny.

![Diagram wirtualnej sieci lokalnej krzyżowe](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

W tej konfiguracji Azure wirtualnej sieci jest zasadniczo opartej na chmurze rozszerzeniem sieci lokalnej.

Ponieważ łączą się do sieci lokalnej krzyżowe lokalnej że wirtualnych sieci, należy użyć część adresu ilość miejsca zajmowaną przez organizację, który jest unikatowy. W ten sam sposób lokalizacjami firmowej są przypisywane Określona podsieć IP Azure staje się innej lokalizacji jako rozszerzenie się z siecią.

Aby umożliwić pakietów na komunikację z wirtualnej sieci lokalnej krzyżowe do sieci lokalnej, należy skonfigurować zestaw odpowiednich lokalnego prefiksów adresów w ramach definicji sieci lokalnej wirtualną sieć. W zależności od przestrzeni adresów wirtualnej sieci i zestaw lokalizacji odpowiedniego lokalnego może istnieć wiele prefiksów adresów w sieci lokalnej.

Możesz przekonwertować tylko do chmury wirtualnej sieci wirtualnej sieci lokalnej krzyżowe, ale prawdopodobnie wymaga ponownego IP usługi Azure zasoby i przestrzeni adresów wirtualnych sieci. Dlatego też rozważyć, jeśli wirtualną sieć musi być połączony z siecią w lokalnym po przypisaniu podsieć adresów IP.

## <a name="subnets"></a>Podsieci
Podsieci umożliwiają organizowanie zasoby, które są powiązane, albo logicznie (na przykład jedną podsieć dla maszyny wirtualne skojarzony z tej samej aplikacji), lub fizycznie (na przykład jedną podsieć według grup zasobów). Można również zastosować technik izolacji podsieci, aby zwiększyć bezpieczeństwo.

Wirtualnych sieci lokalnej krzyżowe należy zaprojektować podsieci z tych samych konwencji, których używasz dla zasobów lokalnych. **Azure zawsze używa pierwsze trzy adresy IP przestrzeni adresów dla każdej podsieci**. Aby określić liczbę potrzebnych dla podsieci adresów, zacznij od liczenie maszyny wirtualne, które są potrzebne teraz. Szacowanie dla przyszłego zwiększenia ilości, a następnie skorzystaj z poniższej tabeli, aby określić rozmiar podsieci.

Liczba potrzebne maszyny wirtualne | Liczbę bitów hosta potrzebne | Rozmiar podsieci
--- | --- | ---
1 – 3 | 3 | 29
4-11     | 4 | -28
12-27 | 5 | -27
28-59 | 6 | -26
60 — 123 | 7 | / 25

> [AZURE.NOTE] Dla podsieci normalny lokalnego maksymalna liczba adresów hosta podsieć z bitów hosta n jest 2<sup>n</sup> -2. Podsieć Azure maksymalna liczba adresów hosta podsieć z bitów hosta n jest 2<sup>n</sup> -5 (2 i 3 dla adresów używanych w każdej podsieci Azure).

Jeśli wybierzesz rozmiar podsieci, który jest za mały, masz do ponownego IP i ponownie wdróż maszyny wirtualne w podsieci.


## <a name="network-security-groups"></a>Grupy zabezpieczeń sieci
Możesz zastosować zasad filtrowania ruchu, która przepływa za pośrednictwem wirtualnych sieci przy użyciu grup zabezpieczeń sieci. Można tworzyć szczegółowego zasad filtrowania zapewnienie środowisku sieci wirtualnej sterowanie ruch przychodzący i wychodzący ruch, źródłowa i docelowa zakresów adresów IP, dozwolone portów itp. Grupy zabezpieczeń sieci mogą być stosowane do podsieci w wirtualnej sieci lub bezpośrednio w interfejsie danego wirtualnej sieci. Zalecane jest mają pewien poziom grupy zabezpieczeń sieci filtrowania ruchu w sieci wirtualnej. Więcej informacji o [Grupach zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Dodatkowe składniki sieci
Podobnie jak w przypadku fizycznej infrastruktury sieci lokalnej, Azure wirtualnej sieci może zawierać więcej niż tylko podsieci i adresy IP. Podczas projektowania infrastruktury aplikacji może być niektóre z tych dodatkowych składników:

- [Bram VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - łączenia Azure wirtualnej sieci innych Azure wirtualnych sieci lub łączenia sieci lokalnej przez połączenie VPN witryny do witryny. Implementowanie rozsyłania Express połączenia dla dedykowanego, bezpiecznego połączenia. Możesz także podać bezpośredni dostęp użytkownicy z połączenia VPN punktu do witryny.
- [Usługa równoważenia obciążenia](../load-balancer/load-balancer-overview.md) - zawiera równoważenia obciążenia ruchu ruchu wewnętrznych i zewnętrznych stosownie do potrzeb.
- [Brama aplikacji](../application-gateway/application-gateway-introduction.md) — Usługa równoważenia obciążenia HTTP równoważenia obciążenia w warstwie aplikacji, dostarczając kilka dodatkowych korzyści wynikających dla aplikacji sieci web zamiast wdrażania Azure.
- [Menedżer ruchu](../traffic-manager/traffic-manager-overview.md) — dystrybucji ruchu systemem DNS do kierowania użytkowników końcowych do najbliższego punkt końcowy dostępnych aplikacji, umożliwiając do obsługi aplikacji poza Azure centrach danych w różnych regionach.


## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 