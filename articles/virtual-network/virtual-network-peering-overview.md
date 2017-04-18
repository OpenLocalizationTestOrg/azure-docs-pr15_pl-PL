
<properties
   pageTitle="Zaglądanie Azure wirtualną sieć | Microsoft Azure"
   description="Informacje na temat VNet zaglądanie platformy Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>Zaglądanie VNet

Zaglądanie VNet jest mechanizm, który łączy dwie sieci wirtualnych (VNets) w tym samym regionie za pośrednictwem sieci Azure szkieletowy. Po peered, dwa wirtualnych sieci pojawia się na celach łączności. Nadal zarządza nimi jako osobne zasoby, ale maszyn wirtualnych w tych wirtualnych sieci można komunikować się ze sobą bezpośrednio, stosując prywatnych adresów IP.

Ruch między maszyn wirtualnych w peered wirtualnych sieci odbywa się za pośrednictwem Azure infrastruktury, tak jak ruch jest przekierowywany między maszyny wirtualne w tej samej sieci wirtualnej. Oto niektóre z zalet korzystania z zaglądanie VNet:

- Małym opóźnieniem, wysokiej przepustowości połączenia między zasobów w różnych wirtualnych sieci.
- Możliwość używania zasobów, takich jak urządzenia sieci i bram VPN jako punkty przewozowe w peered VNet.
- Możliwość łączenia wirtualnej sieci, która korzysta z Menedżera zasobów Azure modelu z siecią wirtualną, która używa modelu Klasyczny wdrożenia i Włącz pełny łączność między zasobów w tych wirtualnych sieci.

Wymagania i najważniejszych aspektów zaglądanie VNet:

- Dwa wirtualnych sieci, które są peered powinny być w tym samym regionie Azure.
- Sieci wirtualne, które są peered powinien mieć siebie obszarach adresów IP.
- Zaglądanie VNet jest między dwoma wirtualnych sieci i braku pochodnych zależności przechodnie. Na przykład jeśli wirtualną sieć A jest peered z wirtualną sieć B, a jeśli wirtualną sieć B jest peered z wirtualną sieć C, nie tłumaczenie do wirtualnej sieci peered z wirtualnej sieci C.
- Zaglądanie można ustalić między wirtualnych sieci w dwóch różnych subskrypcjach jak długo użytkownikiem obu subskrypcji zezwala zaglądanie a subskrypcje są skojarzone z tej samej dzierżawy usługi Active Directory. 
- Zaglądanie między wirtualnej sieci w modelu Menedżera zasobów i model klasyczny wdrażania wymaga VNets należy w tej samej subskrypcji.
- Wirtualna sieć, która korzysta model wdrożenia Menedżera zasobów można peered z innego wirtualną sieć, która korzysta z tego modelu lub z siecią wirtualną korzystającego z modelu Klasyczny wdrożenia. Jednak wirtualnych sieci korzystające z modelu Klasyczny wdrożenia nie może być peered ze sobą.
- Chociaż komunikacji między maszyn wirtualnych w peered wirtualnych sieci nie ma żadnych ograniczeń przepustowości dodatkowe, zakończenie przepustowości na podstawie rozmiaru maszyn wirtualnych nadal dotyczy.


![Podstawowe zaglądanie VNet](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Łączność
Po dwóch wirtualnych sieci są peered, maszyny wirtualnej (web pracownik roli) w wirtualnej sieci mogą bezpośrednio łączyć się z pozostałych maszyn wirtualnych w peered wirtualnej sieci. Te dwie sieci jest pełny poziomie IP łączności.

Czas oczekiwania sieci na podróż między dwoma maszyn wirtualnych w peered wirtualnych sieci jest taka sama jak podróż w sieci lokalnej wirtualną. Przepustowość sieci jest oparty na przepustowość, która jest dozwolona dla maszyny wirtualnej proporcjonalne do jego rozmiar. Nie jest wszelkie dodatkowe ograniczenia przepustowości.

Ruch między maszyn wirtualnych w peered wirtualnych sieci jest skierowana bezpośrednio za pośrednictwem Azure infrastruktury wewnętrznej i nie bramy.

Maszyn wirtualnych w wirtualnej sieci dostęp do wewnętrznej równoważenia obciążenia (ILB) punkty końcowe w peered wirtualnej sieci. Grupy zabezpieczeń sieci (NSGs) mogą być stosowane w obu wirtualną sieć, aby zablokować dostęp do innych wirtualnych sieci lub podsieci, w razie potrzeby.

Gdy użytkownicy skonfigurować zaglądanie, ich otwieranie lub zamykanie reguły NSG między wirtualnych sieci. Jeśli użytkownik wybierze otworzyć pełnego łączność między peered wirtualnych sieci (które jest domyślna opcja), mogli używać NSGs na określonej podsieci lub maszyn wirtualnych o blokowaniu lub odmówić dostępu określonych.

Dostarczonego przez Azure wewnętrznych rozpoznawania nazw DNS dla maszyn wirtualnych nie działa peered wirtualnych sieci. Maszyn wirtualnych mieć wewnętrznych nazw DNS rozpoznawana tylko w sieci lokalnej wirtualną. Użytkownicy mogą jednak skonfigurować maszyn wirtualnych, które działają w peered wirtualnych sieci jako serwerów DNS sieci wirtualnej.

## <a name="service-chaining"></a>Usługa łączenia
Użytkownicy mogą konfigurować zdefiniowane przez użytkownika rozsyłania tabele wskazujące maszyn wirtualnych w peered wirtualnych sieci jako "następny skok" w polu adres IP, jak pokazano na diagramie w dalszej części tego artykułu. To umożliwia użytkownikom osiągnięcia usługi łączenia, za pomocą których można kierować ruchu z jednej sieci wirtualnych do wirtualnego urządzenia, działający w peered wirtualną sieć za pomocą tabel rozsyłania zdefiniowane przez użytkownika.

Użytkownicy również skuteczne można tworzyć środowiskach typu gwiazda — miejsce, w którym Centrum może obsługiwać elementów infrastruktury, takich jak urządzenia wirtualnej sieci. Wszystkie gwiazdy wirtualnych sieci mogą następnie równorzędne z go, a także podzbiór ruchu do urządzenia, które działają w Centrum wirtualnej sieci. Krótko mówiąc, zaglądanie VNet umożliwia adres IP następnego przeskoku na "zdefiniowane przez użytkownika tabeli routingu" jako adres IP maszyny wirtualnej w peered wirtualnej sieci.

## <a name="gateways-and-on-premises-connectivity"></a>Łączność bram i lokalnych
Każdy wirtualnej sieci, niezależnie od tego, czy jest peered z innego wirtualną sieć, nadal można mieć własny bramy i używać go nawiązywania połączenia z lokalnym. Użytkowników można także skonfigurować [VNet do VNet połączenia](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) za pomocą bramy, mimo że są peered wirtualnych sieci.

Gdy obie opcje wzajemne połączenia wirtualnej sieci zostały skonfigurowane, komunikacji między wirtualnych sieci przepływał przez peering konfigurację (oznacza to, że za pośrednictwem Azure szkielet).

Gdy wirtualnych sieci są peered, użytkowników można również skonfigurować bramy w peered wirtualną sieć jako punktu przewozowe do lokalnego. W tym przypadku wirtualnych sieci, która korzysta z bramą zdalną nie może mieć własny bramy. Jeden wirtualnej sieci może zawierać tylko jeden bramy. Albo można bramy lokalnej lub zdalnej bramy (w peered sieci wirtualnej), jak pokazano na poniższej ilustracji.

Przewozowe bramy nie jest obsługiwana w peering relacji między wirtualnych sieci przy użyciu modelu Menedżera zasobów i osoby przy użyciu modelu Klasyczny wdrożenia. Oba wirtualnych sieci w relacji peering konieczne praca przy użyciu modelu wdrożenia Menedżera zasobów dla przewozowe bramy.

Gdy sieci wirtualne, które współużytkują połączenie Azure ExpressRoute są peered, przez peering relacji moduł ruch między nimi (oznacza to, że za pośrednictwem sieci szkieletowego Azure). Użytkownicy nadal mogą korzystać lokalne bram w każdym wirtualną sieć nawiązywania połączenia z obwodu lokalnego. Można też kliknąć można za pomocą udostępnionego bramy i konfigurowanie przewozowe połączenia lokalnego.

![Przewozowe zaglądanie VNet](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Inicjowanie obsługi administracyjnej
Zaglądanie VNet jest uprzywilejowanych operacji. Jest to funkcja osobnych w obszarze nazw VirtualNetworks. Użytkownik może być podawana praw Aby autoryzować zaglądanie. Użytkownik, który ma dostęp do odczytu i zapisu do wirtualnej sieci automatycznie dziedziczy tych praw.

Użytkownik, który jest albo administratorem lub użytkownikiem peering możliwości zainicjować peering operacji na inną VNet. Jeśli istnieje pasująca żądanie zaglądanie po drugiej stronie, a inne wymagania są spełnione, zostanie utworzona zaglądanie.

Zapoznaj się z artykuły w sekcji "Następne kroki", aby dowiedzieć się więcej na temat ustanawiania VNet zaglądanie między dwoma wirtualnych sieci.

## <a name="limits"></a>Ograniczenia
Istnieją ograniczenia liczby peerings niedozwolonego pojedynczej wirtualnej sieci. Zapoznaj się z [Azure limity sieci](../azure-subscription-service-limits.md#networking-limits) , aby uzyskać więcej informacji.

## <a name="pricing"></a>Cennik
Zaglądanie VNet będą dostępne bezpłatnie w okresie przeglądu. Po zwolnieniu będzie opłatę nominalna na ruch ingress i wyjściowym, który używa zaglądanie. Aby uzyskać więcej informacji zapoznaj się z [ceny strony](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Następne kroki
- [Konfigurowanie zaglądanie między wirtualnych sieci](virtual-networks-create-vnetpeering-arm-portal.md).
- Informacje na temat [NSGs](virtual-networks-nsg.md).
- Informacje na temat [trasy zdefiniowane przez użytkownika i przesyłanie dalej adresów IP](virtual-networks-udr-overview.md).
