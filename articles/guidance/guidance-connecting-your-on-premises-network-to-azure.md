<properties
   pageTitle="Łączenie sieci lokalnej Azure | Microsoft Azure"
   description="W tym miejscu wyjaśniono i porównanie różnych metod nawiązywania połączenia usług chmury firmy Microsoft, takich jak Azure, Office 365 i Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Łączenie sieci lokalnej Azure

Firma Microsoft udostępnia kilka typów usług w chmurze. Podczas możesz połączyć z usługami publicznie w Internecie, można również nawiązać niektóre z tych usług za pomocą tunelem wirtualną sieć prywatną (VPN) przez Internet lub za pośrednictwem połączenia bezpośredniego do firmy Microsoft. Ten artykuł pomaga określić, którą opcję łączności najlepiej będzie zależnie od potrzeb, na podstawie typów usługami firmy Microsoft w chmurze, których można używać. Większość organizacji korzystać z wielu typów połączenia opisane poniżej.

## <a name="connecting-over-the-public-internet"></a>Nawiązywanie połączenia za pośrednictwem publicznego Internetu

Ten typ połączenia zapewnia dostęp do usług w chmurze firmy Microsoft bezpośrednio przez Internet, jak pokazano poniżej.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

To połączenie jest zwykle pierwszy typ używane do łączenia się z usługami w chmurze firmy Microsoft. W poniższej tabeli wymieniono wad i zalet tego typu połączenia.



| **Zalety**| **Zagadnienia dotyczące**|
|---------|---------|
|Wymaga żadnych zmian do sieci lokalnej, ile wszystkich urządzeń klienckich mają nieograniczony dostęp do wszystkich adresów IP i porty w Internecie.|Chociaż ruch często jest zaszyfrowany przy użyciu protokołu HTTPS, mogą zostać przechwycone w drodze ponieważ przez publicznego Internetu.|
|Łączyć się wszystkie usługi chmury firmy Microsoft na publicznego Internetu.|Nieoczekiwane opóźnienie ponieważ przechodzi połączenie internetowe jest aktywne.|
|Korzysta z istniejącego połączenia internetowego.||
|Nie wymaga zarządzania urządzeń łączności.||

To połączenie ma nie połączenia lub przepustowość koszty, ponieważ korzystasz z istniejącego połączenia internetowego. 

## <a name="connecting-with-a-point-to-site-connection"></a>Nawiązywanie połączenia za pomocą połączenia punkt do witryny

Ten typ połączenia zapewnia dostęp do niektórych usług firmy Microsoft cloud tunelem Tunneling protokół SSTP (Secure Socket) przez Internet, jak pokazano poniżej.

![P2S] Połączenia punkt witryny (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "")

Połączenie jest przekazane istniejące połączenie z Internetem, ale wymaga korzystania z bramą VPN Azure. W poniższej tabeli wymieniono wad i zalet tego typu połączenia.

| **Zalety**| **Zagadnienia dotyczące**|
|---------|---------|
|Wymaga żadnych zmian do sieci lokalnej, ile wszystkich urządzeń klienckich mają nieograniczony dostęp do wszystkich adresów IP i portów w Internecie.|Jeśli ruch jest zaszyfrowany przy użyciu protokołu IPSec, może zostać przechwycona podczas przesyłania ponieważ przez publicznego Internetu.|
|Korzysta z istniejącego połączenia internetowego.|Nieoczekiwane opóźnienie ponieważ przechodzi połączenie internetowe jest aktywne.|
|Przepustowość maksymalnie 200 Mb/s na bramy.|Wymaga tworzenie i zarządzanie oddzielnych połączeń między każde urządzenie w sieci lokalnej i każdej bramy, którą każde urządzenie należy nawiązać połączenie.|
|Można łączyć się z usługami Azure, połączone ze wirtualnych sieci Azure (VNet) takie jak maszyn wirtualnych Azure i usług w chmurze Azure.|Wymaga minimalnego prowadzonego administrowania Brama VPN Azure.|
||Nie można połączyć się z usługi Microsoft Office 365 lub Dynamics CRM Online.
||Nie można połączyć się z usługami Azure, których nie można połączyć z VNet.|

Dowiedz się więcej o usłudze [Brama VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) , jego [ceny](https://azure.microsoft.com/pricing/details/vpn-gateway)i [ceny](https://azure.microsoft.com/pricing/details/data-transfers)transfer danych ruchu wychodzącego.

## <a name="connecting-with-a-site-to-site-connection"></a>Nawiązywanie połączenia z połączeniem witryny do witryny

Ten typ połączenia zapewnia dostęp do niektórych usług firmy Microsoft cloud za pośrednictwem tunelem IPSec przez Internet, jak pokazano poniżej.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Połączenia witryny do witryny")

Połączenie jest wprowadzane w istniejącym połączeniem internetowym, ale wymaga korzystania z bramą VPN Azure z skojarzone ceny i transfer danych ruchu wychodzącego ceny. W poniższej tabeli wymieniono wad i zalet tego typu połączenia.

| **Zalety**| **Zagadnienia dotyczące**|
|---------|---------|
|Wszystkich urządzeń w sieci lokalnej komunikować się z usługami Azure połączony z VNet, dlatego nie trzeba konfigurować poszczególne połączenia dla każdego urządzenia.|Jeśli ruch jest zaszyfrowany przy użyciu protokołu IPSec, mogą zostać przechwycone w drodze ponieważ przez publicznego Internetu.|
|Korzysta z istniejącego połączenia internetowego.|Nieoczekiwane opóźnienie ponieważ przechodzi połączenie internetowe jest aktywne.|
|Służy do łączenia się z usługi Azure, które mogą być połączone z VNet, takich jak maszyn wirtualnych i usług w chmurze.|Należy skonfigurować i zarządzanie sprawdzanej VPN urządzenia * lokalnego.|
|Przepustowość maksymalnie 200 Mb/s na bramy.|Wymaga minimalnego codziennego zarządzania Brama VPN Azure.|
|Można wymusić ruchu wychodzącego zainicjowane w chmurze maszyn wirtualnych sieci lokalnej inspekcji i rejestrowanie przy użyciu zdefiniowanych przez użytkownika przekierowuje lub obramowania Gateway Protocol (BGP) **.|Nie można połączyć się z usługi Microsoft Office 365 lub Dynamics CRM Online.|
||Nie można połączyć się z usługami Azure, których nie można połączyć z VNet.|
||Jeśli korzystasz z usług, które inicjowania połączeń z powrotem do lokalnego urządzenia i wymaga zasad zabezpieczeń, może być konieczne zapory między sieci lokalnej i Azure.|

- * Wyświetlanie listy [sprawdzania poprawności urządzeń sieci VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Dowiedz się więcej o używaniu [przekierowuje zdefiniowane przez użytkownika](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) lub [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) wymusić routing z Azure VNets z urządzeniem lokalnym.

## <a name="connecting-with-a-dedicated-private-connection"></a>Nawiązywanie połączenia z dedykowanego połączenia prywatnego

Tego typu połączenia zapewnia dostęp do wszystkich usług w chmurze firmy Microsoft przez połączenie prywatne dedykowane, do firmy Microsoft, które nie przechodzą przez Internet, jak pokazano poniżej.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute połączenia")

Połączenie wymaga korzystania z usługi ExpressRoute i połączenia z dostawcą łączności. W poniższej tabeli wymieniono wad i zalet tego typu połączenia.

| **Zalety**| **Zagadnienia dotyczące**|
|---------|---------|
|Ruch nie przechwycone podczas przesyłania publicznie w Internecie ponieważ jest używane połączenie dedykowane przez dostawcę usług.|Wymaga zarządzania routera lokalnego.|
|Przepustowość do 10 Gb/s na obwód ExpressRoute i przepustowość do 2 Gb/s do każdej bramy.|Wymaga połączenia dedykowane do dostawców usług łączności.|
|Opóźnienie przewidywalne ponieważ jest połączenie dedykowane do firmy Microsoft, które nie przechodzą przez Internet.|Może być wymagana minimalnego stałego administrowania jeden lub więcej bram sieci VPN Azure (Jeśli połączenie z obwodem VNets).|
|Nie wymaga szyfrowanego komunikacji, ale w razie potrzeby można szyfrować ruchu.| Jeśli korzystasz z usługami w chmurze, które inicjowania połączeń z powrotem do lokalnego urządzenia, może być konieczne zapory między sieci lokalnej i Azure.|
|Bezpośrednio połączyć się ze wszystkich usług chmury firmy Microsoft, z wyjątkiem kilku *.|Wymaga translacji adresów sieciowych (NAT) adresów IP lokalnego wprowadzanie centrach danych firmy Microsoft dla usług, których nie można połączyć z VNet.* *|
|Można wymusić zainicjowane w chmurze maszyn wirtualnych za pośrednictwem sieci lokalnej kontroli i rejestrowanie przy użyciu BGP ruchu wychodzącego.|

- * Przeglądanie [listy usług](../expressroute/expressroute-faqs.md#supported-services) , które nie mogą być używane z ExpressRoute. Twoja subskrypcja Azure musi zostać zatwierdzony nawiązywania połączenia z usługą Office 365.  Zobacz artykuł [Azure ExpressRoute dla usługi Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) , aby uzyskać szczegółowe informacje.
- ** Dowiedz się więcej o ExpressRoute [translatora adresów Sieciowych](../expressroute/expressroute-nat.md) wymagania.

Dowiedz się więcej o [ExpressRoute](../expressroute/expressroute-introduction.md), jego skojarzony [ceny](https://azure.microsoft.com/pricing/details/expressroute)i [dostawców łączności](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Uwagi dodatkowe

- Niektóre z powyższych opcji mają różne maksymalną, które można obsługę połączeń VNet, połączenia bramy i innych kryteriów. Zalecane jest, przejrzyj Azure [ogranicza sieci](../azure-subscription-service-limits.md#networking-limits) do zrozumienia dowolnej z nich miało wpływ typów połączenia, którego chcesz używać. 
- Jeśli planujesz połączyć bramy połączenie VPN witryny do witryny z tym samym VNet jako bramy ExpressRoute, należy zapoznać się z ograniczeniami ważne najpierw. Zobacz artykuł [połączenia współistnienia witryny do witryny i konfigurowanie ExpressRoute](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) , aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki

Poniższe zasoby wyjaśniono, jak wdrażać typów połączeń omówione w tym artykule.

-   [Implementowanie połączenia punkt do witryny](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Implementowanie połączenia witryny do witryny](guidance-hybrid-network-vpn.md)
-   [Implementowanie dedykowane połączenie prywatne](guidance-hybrid-network-expressroute.md)
-   [Implementowanie dedykowane połączenia prywatnego za pomocą połączenia witryny do witryny wysokiej dostępności](guidance-hybrid-network-expressroute-vpn-failover.md)
