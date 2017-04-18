<properties 
   pageTitle="Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP?"
   description="Dowiedz się, jak przy użyciu użytkownika zdefiniowane trasy (UDR) i przekazywanie adresów IP do przekazywania ruchu sieci wirtualnej urządzenia w Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP?
Po dodaniu wirtualnych maszyn wirtualnych sieci (VNet) platformy Azure można zauważyć, że maszyny wirtualne są możliwość komunikowania się między sobą w sieci, automatycznie. Nie należy określić bramy, nawet jeśli maszyny wirtualne znajdują się w różnych podsieci. To samo dotyczy komunikacji z maszyny wirtualne do publicznego Internetu, a nawet do programu lokalnego ma sieci, jeśli połączenie hybrydowego z Azure własne centrum danych.

Ten przepływ komunikacji jest możliwe, ponieważ Azure używa serii system trasy do zdefiniuj sposób przepływu ruchu IP. Trasy system sterowania przepływem komunikacji w następujących sytuacjach:

- Z poziomu tej samej podsieci.
- Z podsieci w obrębie VNet.
- Z maszyny wirtualne z Internetem.
- Z VNet do innego VNet przez bramę VPN.
- Z VNet do sieci lokalnej przez bramę VPN.

Na poniższym rysunku przedstawiono prosta konfiguracja z VNet, dwóch podsieci i kilka maszyny wirtualne, wraz z przekierowuje systemu, które zezwalają na ruch IP.

![System przekierowuje platformy Azure](./media/virtual-networks-udr-overview/Figure1.png)

Użyj przekierowuje system ułatwia ruch automatycznie dla wdrożenia, istnieją przypadki, w których chcesz kontrolować routing pakietów za pośrednictwem wirtualnych urządzenia. Można tak, tworząc przekierowuje zdefiniowane przez użytkownika, który określić następnego przeskoku pakietów ułożony do określonej podsieci, aby przejść do urządzenia wirtualnego zamiast i włączania przesyłania IP dla maszyn wirtualnych, działającego jako wirtualnego urządzenia.

Na poniższym rysunku przedstawiono przykład przekierowuje zdefiniowane przez użytkownika i przesyłanie dalej adresów IP, aby wymusić pakiety wysyłane do jednej podsieci z innej usługi za pośrednictwem wirtualnych urządzenia w trzecim podsieci.

![System przekierowuje platformy Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Trasy zdefiniowane przez użytkownika są stosowane tylko do ruchu wychodzącego podsieć. Nie można utworzyć trasy, aby określić, jak ruch wejścia podsieci z Internetu, na przykład. Ponadto urządzenia, którą chcesz przesłać dalej ruch nie może być w tej samej podsieci, którego pochodzą dane. Zawsze z kopią osobną podsieć dla urządzeń. 

## <a name="route-resource"></a>Trasa zasobów
Pakiety są wysyłane przez sieć TCP/IP oparta na tabeli rozsyłania zdefiniowanej w każdym węźle w sieci fizycznej. Tabela rozsyłania jest zbiór poszczególnych przekierowuje umożliwia decydowanie, gdzie dalej na podstawie adresu IP miejsca docelowego. Trasa składa się z następujących czynności:

|Właściwość|Opis|Ograniczenia|Zagadnienia dotyczące|
|---|---|---|---|
| Prefiks adresu | CIDR docelowej, do którego ma zastosowanie trasę, takich jak 10.1.0.0/16.|Musi być prawidłowe zakresy CIDR przedstawiających adresy na publicznego Internetu, Azure wirtualnej sieci lub lokalnego centrum danych.|Upewnij się, że **Prefiks adresu** nie zawiera adres w polu **adres następnego przeskoku**, inaczej pakiety zostanie wprowadzony w pętli, przechodząc ze źródła do następnego przeskoku bez kiedykolwiek osiągnięcia miejsca docelowego. |
| Następny typ przeskoków | Typ Azure przeskoku pakiet powinny trafiać do. | Należy wybrać jedną z następujących wartości: <br/> **Wirtualna sieć**. Reprezentuje wirtualnej sieci lokalnej. Na przykład jeśli masz dwie podsieci, 10.1.0.0/16 i 10.2.0.0/16 w tej samej sieci wirtualnej trasę dla każdej podsieci w tabeli rozsyłania ma wartość następnego skoku *Wirtualnej*sieci. <br/> **Brama wirtualnej sieci**. Reprezentuje Brama VPN Azure S2S. <br/> **Internet**. Reprezentuje domyślne bramy internetowej dostarczony przez infrastruktury Azure. <br/> **Wirtualna urządzenia**. Reprezentuje wirtualnego urządzenia, dodanych do usługi Azure wirtualnej sieci. <br/> **Brak**. Reprezentuje czarnej. Pakiety przesyłane dalej do czarnej nie zostaną w ogóle przesłane dalej.| Warto rozważyć użycie typu **Brak** przestanie pakietów liczby do określonego miejsca docelowego. | 
| Adres następnego przeskoku | Adres następnego przeskoku zawiera adres IP, które pakiety powinna być przesyłana dalej. Następnego przeskoku wartości są dozwolone tylko w przekierowuje następny typ przeskoków w przypadku *Wirtualnych urządzenia*.| Musi być adresem IP, który jest dostępny w wirtualnej sieci, w którym zastosowano trasę zdefiniowane przez użytkownika. | Jeśli adres IP reprezentuje maszyny, upewnij się, że można włączyć [przekazywanie adresów IP](#IP-forwarding) w Azure maszyn wirtualnych. |

W programie PowerShell Azure niektóre z wartości "NextHopType" mają różne nazwy:
- Wirtualna sieć jest VnetLocal
- Wirtualna sieć brama jest VirtualNetworkGateway
- Wirtualna urządzenie jest VirtualAppliance
- Internet jest Internet
- Brak jest Brak

### <a name="system-routes"></a>Trasy systemu
Co podsieć utworzone w wirtualnej sieci jest automatycznie skojarzone z tabelą rozsyłania, który zawiera następujące reguły rozsyłania systemu:

- **Lokalna reguła Vnet**: Ta reguła jest tworzona automatycznie dla każdej podsieci wirtualnej sieci. Określa, czy istnieje bezpośrednie łącze między maszyny wirtualne w VNet i czy jest nie pośredniej następnego przeskoku.
- **Reguła lokalnego**: Ta reguła ma zastosowanie do całego ruchu skierowanego do zakresu adresów lokalnych i używa Brama VPN jako miejsca docelowego dla następnego przeskoku.
- **Reguła internetowej**: ten uchwyty reguły cały ruch przeznaczone do publicznego Internetu (adres prefiks 0.0.0.0/0) i używa bramy internetowej infrastruktury jako następnego przeskoku dla całego ruchu przeznaczone do Internetu.

### <a name="user-defined-routes"></a>Trasy zdefiniowane przez użytkownika
W przypadku większości środowisk wystarczy przekierowuje systemu zdefiniowanych przez Azure. Jednak może być konieczne tworzenie tabeli rozsyłania i dodać jeden lub więcej trasy w szczególnych przypadkach, takich jak:

- Wymuszanie tunelowania w Internecie za pośrednictwem sieci lokalnej.
- Używanie urządzeń wirtualnych w środowisku usługi Azure.

W powyższych scenariuszach, musisz utworzyć tabelę rozsyłania i Dodaj zdefiniowane przez użytkownika trasy do niego. Masz wiele tabel rozsyłania i tę samą tabelę rozsyłania można skojarzyć z jednej lub kilku podsieci. I każdej podsieci może być skojarzony tylko z tabelą trasa. Wszystkie maszyny wirtualne i usług w chmurze w użyciu podsieci tabelę routingu skojarzone z tej podsieci.

Podsieci są oparte na system trasy do momentu tabeli routingu jest przypisana do podsieci. Skojarzenie istnieje, jest wykonywane oparte na Najdłuższy prefiks dopasowania (LPM) między zarówno przekierowuje zdefiniowane przez użytkownika, jak i przekierowuje system routingu. Jeśli istnieje więcej niż jedna trasa z tym samym dopasowanie LPM trasę zaznaczona jest oparte na jego pochodzenie w następującej kolejności:

1. Trasa zdefiniowane użytkownika
1. Trasa BGP (jeśli jest używana ExpressRoute)
1. System rozsyłania

Aby dowiedzieć się, jak utworzyć trasy zdefiniowane przez użytkownika, zobacz, [jak utworzyć drogi i Włącz IP przekazywanie platformy Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Trasy zdefiniowane przez użytkownika są stosowane tylko do usługi Azure maszyny wirtualne i chmury. Na przykład jeśli chcesz dodać urządzenia wirtualnego zapory między sieci lokalnej i Azure, musisz utworzyć trasę zdefiniowane przez użytkownika w tabelach Azure rozsyłania przesyłania dalej cały ruch, przechodząc do przestrzeni adresów w lokalnej wirtualną urządzenia. Można również dodać trasę zdefiniowane przez użytkownika (UDR), aby GatewaySubnet do przekazywania wszystkich danych z lokalnego Azure za pośrednictwem wirtualnych urządzenia. To jest dodanie ostatnie.

### <a name="bgp-routes"></a>Trasy BGP
Jeśli masz połączenie ExpressRoute sieci lokalnej i Azure, możesz włączyć BGP propagowanie ścieżki z sieci lokalnej Azure. Te trasy BGP są używane w taki sam sposób jak drogi systemu i przekierowuje zdefiniowane przez użytkownika w każdej podsieci Azure. Aby uzyskać więcej informacji, zobacz [Wprowadzenie ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Można skonfigurować środowisko Azure umożliwia wymuszanie tunneling za pośrednictwem sieci lokalnej, tworząc użytkownika trasę zdefiniowane dla 0.0.0.0/0 podsieci, używającej Brama VPN jako następnego przeskoku. Jednak to tylko wtedy, gdy korzystasz z bramą VPN nie ExpressRoute. ExpressRoute wymuszonego tunneling skonfigurowano za pośrednictwem BGP.

## <a name="ip-forwarding"></a>Przesyłanie dalej adresów IP
Jak opisano powyżej, jest jednym z głównych powodów do tworzenia zdefiniowanych trasę użytkownika jest do przesyłania ruchu do wirtualnego urządzenia. Wirtualna urządzenia ma więcej niż maszyny uruchamiana aplikacja używana do obsługi ruchu sieciowego w określony sposób, na przykład zapora lub urządzenie NAT.

Wirtualna urządzenie maszyn wirtualnych muszą mieć możliwość odbierania ruchu przychodzącego, nie zaadresowany do siebie. Aby umożliwić maszyn wirtualnych do odbierania ruchu skierowana w inne miejsce, należy włączyć przekazywanie adresów IP dla maszyn wirtualnych. To jest Azure ustawienie nie ustawienia w systemie operacyjnym gościa.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [utworzyć trasy w modelu wdrożenia Menedżera zasobów](virtual-network-create-udr-arm-template.md) i kojarzenie ich do podsieci. 
- Dowiedz się, jak [utworzyć trasy w modelu Klasyczny wdrożenia](virtual-network-create-udr-classic-ps.md) i kojarzenie ich do podsieci.
