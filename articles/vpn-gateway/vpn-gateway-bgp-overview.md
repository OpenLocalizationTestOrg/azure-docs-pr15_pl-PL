<properties
   pageTitle="Omówienie BGP z bram Azure VPN | Microsoft Azure"
   description="W tym artykule omówiono BGP z Azure VPN bramy."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Omówienie BGP z bram Azure VPN

W tym artykule omówiono Obsługa BGP (protokół bram granicznych) w Azure VPN bramy.

## <a name="about-bgp"></a>Informacje o BGP

BGP to standardowy protokół routingu często używanych w Internecie do wymiany informacji routingu i osiągalności między dwie lub więcej sieci. Użycia w kontekście Azure wirtualnych sieci BGP umożliwia Azure bram sieci VPN oraz lokalnego urządzenia VPN, o nazwie BGP współpracownicy lub sąsiadów wymiany "kieruje" informujące obu bramy na dostępności i osiągalności tych prefiksów obsłużone bram lub routery, których dotyczą. Można także włączyć przewozowe routingu spośród wielu sieci na propagowanie przekierowuje, których brama BGP uczy się od jednego końcówki BGP innych partnerom BGP BGP.
 
### <a name="why-use-bgp"></a>Dlaczego warto używać BGP?

BGP jest opcjonalną funkcją używanym w programie Azure sieci VPN, które są oparte na rozsyłania bramy. Możesz również upewnij się, że urządzenia VPN lokalnego obsługują BGP, aby włączyć funkcję. Możesz nadal korzystać bram Azure VPN i lokalnych urządzeń sieci VPN bez BGP. Odpowiada używania trasy statyczne (bez BGP) *a* za pomocą dynamiczne routingu z BGP między sieci i Azure.

Istnieje kilka korzyści i nowe funkcje z BGP:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Obsługuje aktualizacje automatyczne i elastyczne prefiksu

Z BGP wystarczy zadeklarować minimalne prefiks do określonych równorzędnych BGP przez sieć VPN S2S IPsec tunelem. Może być tak mały, jak prefiks hosta (/ 32) adres IP BGP równorzędnych urządzenia VPN lokalnego. Można kontrolować, które lokalnego prefiksów sieci, które chcesz ogłaszanie Azure umożliwia wirtualnej sieci Azure na dostęp.
    
Można również ogłaszanie większe prefiksy, które mogą zawierać niektóre VNet prefiksów adresów, takich jak duże prywatne obszar adresów IP (na przykład 10.0.0.0/8). Pamiętaj, że prefiksów nie mogą być identyczne z jednym z prefiksów VNet. Te trasy identyczne z prefiksy VNet będą odrzucane.

>[AZURE.IMPORTANT] Obecnie reklam trasy domyślnej (0.0.0.0/0) do bram Azure VPN zostaną zablokowane. Dalsze aktualizacja będzie dostępna po włączeniu tej funkcji.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Obsługuje wiele tuneli między VNet i lokalnej witryny z automatyczne przejście według BGP

Można utworzyć wiele połączeń między VNet usługi Azure i urządzeniach VPN lokalnego w tej samej lokalizacji. Ta funkcja zapewnia różne tunele (ścieżki) w obu sieciach w konfiguracji aktywny aktywny. Po odłączeniu jedną tunele odpowiednich przekierowuje zostaną wycofane za pośrednictwem BGP i dane zostaną zostaną automatycznie przesunięte do pozostałych tuneli.
    
Na poniższym diagramie przedstawiono prosty przykład wysokiej dostępności Instalatora:
    
![Wiele ścieżek aktywne](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Obsługi przewozowe routingu między sieci lokalnej i wielu VNets Azure

BGP umożliwia wielu bram dowiedzieć się i propagowanie prefiksy z innej sieci, czy są one bezpośrednio lub pośrednio połączone. Może to umożliwić przewozowe routingu z bram Azure VPN między witryn lokalnego lub sieci wielu Azure wirtualną.
    
Na poniższym diagramie przedstawiono przykład topologii wielu przeskoków z wielu ścieżek, które można ruchu w obu sieciach lokalnego za pośrednictwem sieci VPN Azure bram w Microsoft Networks tranzytowego:

![Czas przesyłania wielu przeskoków](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP często zadawane pytania


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Następne kroki

Zobacz [wprowadzenie BGP na bram Azure VPN](./vpn-gateway-bgp-resource-manager-ps.md) kroki konfigurowania BGP sieci lokalnej krzyżowe i VNet do VNet połączenia.

