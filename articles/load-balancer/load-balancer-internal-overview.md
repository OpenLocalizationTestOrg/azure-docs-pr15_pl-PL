
<properties
   pageTitle="Usługi równoważenia obciążenia wewnętrznych omówienie | Microsoft Azure"
   description="Omówienie równoważenia obciążenia wewnętrznych i jego funkcji. Jak działa równoważenia obciążenia dla Azure i możliwych scenariuszy, aby skonfigurować wewnętrzny punkty końcowe"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internal-load-balancer-overview"></a>Omówienie równoważenia obciążenia wewnętrznych

W odróżnieniu od następnie Internet przeciwległych równoważenia obciążenia równoważenia obciążenia wewnętrzny (ILB) kieruje ruch tylko do zasobów w usłudze w chmurze lub przy użyciu sieci VPN, aby uzyskać dostęp do infrastruktury Azure. Infrastruktury ogranicza dostęp do równoważenia obciążenia wirtualne adresy IP (służącą) usługi w chmurze lub wirtualnej sieci, tak aby były będą nigdy nie można bezpośrednio poddana punktu końcowego Internet. Dzięki temu wiersza wewnętrznego aplikacji branżowych (LOB), aby uruchomić platformy Azure i można uzyskać dostęp z w chmurze lub zasobów lokalnych.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Dlaczego może być konieczne równoważenia obciążenia wewnętrznych

Azure wewnętrznych ładowanie równoważenia (ILB) zawiera równoważenia między maszyn wirtualnych, które znajdują się w obrębie usługi w chmurze lub wirtualną sieć w zakresie regionalne. Uzyskać informacji na temat używanie i konfigurację wirtualnych sieci w zakresie regionalne zobacz [Regionalne wirtualnych sieci](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) w blogu Azure. Istniejące wirtualnych sieci skonfigurowane dla grupy koligacja nie można użyć ILB.

ILB udostępnia następujące typy równoważenia obciążenia:

- W ramach usługi w chmurze, w środowisku maszyn wirtualnych systemu do zestawu maszyn wirtualnych, które znajdują się w tym samym usługi w chmurze (patrz rysunek 1).
- W wirtualnej sieci z maszyn wirtualnych w wirtualnej sieci do zestawu maszyn wirtualnych, które znajdują się w tym samym usługi w chmurze do katalogu sieci, (patrz rysunek 2).
- Krzyżowe lokalnej wirtualną sieć z komputerów lokalnych do zestawu maszyn wirtualnych, które znajdują się w tym samym usługi w chmurze do katalogu sieci (patrz rysunek 3).
- Aplikacje z Internetu, wielu, w których są z Internetu poziomów wewnętrznej, ale wymagają równoważenia obciążenia dla ruchu z poziomu z Internetu.
- Równoważenia obciążenia dla aplikacji FIRMOWYCH obsługiwany w Azure bez konieczności sprzętu równoważenia obciążenia dodatkowe lub oprogramowania. W tym lokalne serwery w zestawie komputerów ruch jest ładowania strategie.

## <a name="internet-facing-multi-tier-applications"></a>Dostępnych aplikacji wielu przez Internet


Warstwa ma punkty końcowe przeciwległych Internet dla klientów internetowych i jest częścią zestawu równoważenia obciążenia. Usługi równoważenia obciążenia rozdziela przychodzący ruch z klientami sieci web dla port TCP 443 (HTTPS) na serwerach sieci web.

Serwer bazy danych jest punkt końcowy ILB serwerów sieci web za pomocą do przechowywania. Obciążenie bazy danych usługi równoważenia punktu końcowego, w którym ruch jest równoważenia na serwerach bazy danych w zestawie ILB obciążenia.

Poniższa ilustracja przedstawia Internet przeciwległych wielu aplikacji w tym samym usługi w chmurze.

Rysunek 1 - dostępnych aplikacji wielu przez Internet

![Wewnętrzna usługą równoważenia obciążenia pojedynczy chmury](./media/load-balancer-internal-overview/IC736321.png)

Inny możliwe do użycia w przypadku aplikacji wielu jest po wdrożeniu ILB usługi w chmurze innego niż przez inne usługi dla ILB.

Usług w chmurze za pomocą tej samej sieci wirtualnej uzyskuje dostęp do punktu końcowego ILB.

Rysunek 2 pokazuje frontonu sieci web, serwerów są w usłudze w chmurze różnych z wewnętrznej bazy danych i przy użyciu punkt końcowy ILB w tej samej sieci wirtualnej.

![Ładowanie wewnętrznych równoważenia między usługami w chmurze](./media/load-balancer-internal-overview/IC744147.png)

Rysunek 2 - serwerach frontonu w usłudze w chmurze różnych

## <a name="intranet-line-of-business-applications"></a>Intranet linią aplikacji biznesowych

Ruch z klientów w sieci lokalnej Uzyskaj równoważenia obciążenia przez zbiór serwerów LOB przy użyciu połączenia VPN Azure siecią.

Komputer kliencki uzyskuje dostęp do adresu IP z usługi Azure VPN przy użyciu punktu do witryny sieci VPN. Umożliwia użycie aplikacji LOB hostowanej za punktem końcowym ILB.

![Równoważenia obciążenia wewnętrznych przy użyciu punktu do witryny sieci VPN](./media/load-balancer-internal-overview/IC744148.png)

Rysunek 3 - aplikacji LOB znajdującej się za punktem końcowym kg

Inny scenariusz LOB ma sieci VPN witryny do wirtualnej sieci, w którym skonfigurowano punkt końcowy ILB. Dzięki temu lokalnego na ruch sieciowy do punktu końcowego ILB.

![Równoważenia obciążenia wewnętrznych przy użyciu sieci VPN witryny](./media/load-balancer-internal-overview/IC744150.png)

Rysunek 4 - ruchu w sieci lokalnej kierowane do punktu końcowego ILB


## <a name="next-steps"></a>Następne kroki

[Azure Menedżera zasobów pomocy technicznej dla usługi równoważenia obciążenia Azure](load-balancer-arm.md)

[Przed rozpoczęciem konfigurowania dostępnych równoważenia obciążenia przez Internet](load-balancer-get-started-internet-arm-ps.md)

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)

