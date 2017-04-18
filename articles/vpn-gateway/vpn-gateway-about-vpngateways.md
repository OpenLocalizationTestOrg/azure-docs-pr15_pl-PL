<properties 
   pageTitle="Informacje o Brama VPN | Microsoft Azure"
   description="Informacje na temat połączenia VPN bramy Azure wirtualnych sieci."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Informacje o Brama VPN


Brama wirtualnej sieci jest używany do wysyłania ruchu sieciowego między Azure wirtualnych sieci i lokalizacji lokalnego oraz między wirtualnej sieci w obrębie Azure (VNet-VNet). Podczas konfigurowania bramy sieci VPN, należy utworzyć i skonfigurować bramę wirtualną sieć i połączenia wirtualnej sieci bramy.

W modelu wdrożenia Menedżera zasobów podczas tworzenia zasobu Brama wirtualnej sieci należy określić kilka ustawień. Jedno z ustawień wymagane jest "-GatewayType". Istnieją dwa typy Brama wirtualnej sieci: sieci Vpn i ExpressRoute. 

Gdy ruch sieciowy jest wysyłana na dedykowane połączenie prywatne, użyj typu bramy "ExpressRoute". Jest to także nazywane bramy ExpressRoute. Gdy ruch sieciowy są przesyłane zaszyfrowane przez połączenie publiczne, użyj typu bramy "Vpn". To jest określane jako brama VPN. Witryny do witryny, punktu do witryny i VNet do VNet połączenia wszystkich za pomocą bramy sieci VPN.

Każda wirtualna sieć może zawierać tylko jeden Brama wirtualnej sieci według typu bramy. Na przykład możesz mieć jeden wirtualną sieć bramy, która używa - GatewayType ExpressRoute oraz takie, które używa - GatewayType Vpn. W tym artykule omówiono przede wszystkim Brama VPN. Aby uzyskać więcej informacji o ExpressRoute zobacz [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Cennik

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>SKU bramy

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Aby uzyskać więcej informacji na temat bramy wersji produktu zobacz [SKU bramy](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Szacowane Agreguj przepustowość poszczególnych wersji

W poniższej tabeli przedstawiono typy bramy i Szacowana przepustowość agregacji. Ta tabela odnosi się do Menedżera zasobów i modeli wdrażania klasyczny.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Konfigurowanie bramy sieci VPN

Po skonfigurowaniu bramą VPN instrukcje, których można użyć zależą od modelu wdrożenia, który umożliwia tworzenie wirtualnych sieci. Na przykład jeśli utworzysz usługi VNet przy użyciu modelu Klasyczny wdrożenia umożliwia wskazówki i instrukcje dotyczące modelu Klasyczny wdrożenia tworzenie i konfigurowanie ustawień bramy sieci VPN. Aby uzyskać więcej informacji o modelach wdrażania zobacz [Opis Menedżera zasobów i modeli wdrażania klasyczny](../resource-manager-deployment-model.md).

Połączenie VPN brama zależy od wielu zasobów, które zostały skonfigurowane dla określonych ustawień. Większość zasobów można skonfigurować oddzielnie, mimo że musi być skonfigurowany w określonej kolejności w niektórych przypadkach. Możesz rozpocząć tworzenie i konfigurowanie zasobów za pomocą jednego narzędzia konfiguracji, takie jak Azure portal. Następnie można później zdecydować przełączyć się do innego narzędzia, na przykład programu PowerShell, aby skonfigurować dodatkowe zasoby lub zmodyfikować istniejące zasobów, gdy jest to właściwe. Obecnie nie można skonfigurować dla wszystkich zasobów i ustawienia zasobu w portalu Azure. Instrukcje w artykułach dla każdego połączenia topologii określać, gdy jest wymagana narzędzia określonej konfiguracji. Aby uzyskać informacje dotyczące poszczególnych zasobów i ustawień bramy sieci VPN zobacz [Ustawienia o Brama VPN](vpn-gateway-about-vpn-gateway-settings.md).

W poniższych sekcjach zawarto tabel zawierających listę:

- model dostępne wdrażania
- Narzędzia dostępne konfiguracji
- łącza, które prowadzą bezpośrednio do artykułu, jeśli jest dostępna

Za pomocą diagramów i opisy przy wyborze topologii połączenia zgodnie z wymaganiami. Diagramy Pokaż topologii głównym według planu bazowego, ale można tworzyć bardziej złożone konfiguracji za pomocą diagramów wskazówką.

## <a name="site-to-site-and-multi-site"></a>Witryny do witryny i wiele witryn

### <a name="site-to-site"></a>Witryny do witryny

Połączenie VPN witryny do witryny (S2S) bramy przekracza połączenie VPN IPsec/IKE (IKEv1 lub IKEv2) tunelem. Tego typu połączenia wymaga urządzenia VPN znajduje się w lokalnej mające publiczny adres IP do niego przypisana i nie znajduje się za translatora adresów sieciowych. S2S połączenia może być używany do krzyżowe lokalnego i hybrydowego konfiguracji.   

![Połączenie S2S] (./media/vpn-gateway-about-vpngateways/demos2s.png "witryny do witryny")


### <a name="multi-site"></a>Wiele witryn

Można utworzyć i skonfigurować połączenie VPN brama między usługi VNet i wielu sieci lokalnej. Podczas pracy z wielu połączeń, należy użyć typu RouteBased VPN (dynamiczne bramy dla VNets klasyczny). Ponieważ VNet może zawierać tylko jednej bramy sieci VPN, wszystkie połączenia przez bramę udostępnianie przepustowości. Jest to często połączenia "wielu witryny".
 

![Połączenia z serwisem wielu elementów] (./media/vpn-gateway-about-vpngateways/demomulti.png "wiele witryn")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Modele wdrożenia i metody witryny do witryny i wiele witryn

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet do VNet

Wirtualna sieć nawiązywanie połączenia z innym wirtualnej sieci (VNet-VNet) jest podobna do VNet nawiązywanie połączeń z lokalizacji witryny lokalnej. Oba typy łączności za pomocą bramy sieci VPN o podanie kanału bezpiecznego przy użyciu IPsec i IKE. Możesz nawet połączyć VNet do VNet komunikowanie się z połączenia z serwisem wielu konfiguracji. Pozwala ustanowić topologii sieci, łączące między lokalnej łączność z łącznością między wirtualnej sieci.

Może być VNets, z którymi się połączyć:

- w takich samych lub różnych regionów
- w tym samym lub innym subskrypcji 
- w tym samym modelach rozmieszczania


![VNet VNet połączenia] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet do vnet")

#### <a name="connections-between-deployment-models"></a>Połączenia między modelami wdrażania

Azure obecnie występują dwa modele wdrażania: klasyczny i Menedżera zasobów. Jeśli używany był Azure przez dłuższy czas, prawdopodobnie masz maszyny wirtualne Azure i ról wystąpienie uruchomione w klasycznym VNet. Do nowszego maszyny wirtualne i wystąpienia roli może działać w VNet utworzone w Menedżerze zasobów. Możesz utworzyć połączenie między VNets, aby umożliwić zasobów w jednym VNet można komunikować się bezpośrednio z zasobami w innym.

#### <a name="vnet-peering"></a>Zaglądanie VNet

Można użyć VNet zaglądanie do utworzenia połączenia, jak wirtualna sieć spełnia określonych wymagań. Zaglądanie VNet nie używa Brama wirtualnej sieci. Aby uzyskać więcej informacji zobacz [zaglądanie VNet](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Modeli wdrażania i metody VNet do VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punkt do witryny

Połączenie VPN punktu do witryny (P2S) bramy umożliwia tworzenie bezpiecznego połączenia z siecią wirtualną z pojedynczym komputerem klienckim. P2S przekracza połączenie VPN SSTP (Secure Socket Tunneling Protocol). Połączenia P2S nie wymagają urządzenia VPN lub adres IP publicznej do pracy. Możesz ustanowić połączenie VPN, uruchamiając go z komputera klienckiego. Tego rozwiązania jest przydatne, gdy chcesz się połączyć z VNet z lokalizacji zdalnej, takich jak z domu lub konferencji, lub gdy masz tylko kilku klientów, którzy potrzebują nawiązywania połączenia z VNet. P2S połączenia może służyć w połączeniu z S2S połączenia za pomocą tej samej bramie sieci VPN, pod warunkiem, że wszystkie wymagania dotyczące konfiguracji dla obu połączeń są zgodne.


Połączenia punkt witryny ![] (./media/vpn-gateway-about-vpngateways/demop2s.png "punkt do witryny")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Modeli wdrażania i metody punktu do witryny

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

W związku z ExpressRoute Brama wirtualnej sieci skonfigurowano typu bramy "ExpressRoute" zamiast "Vpn". Aby uzyskać więcej informacji o ExpressRoute zobacz [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Połączenia witryny do witryny i ExpressRoute współistnienia

ExpressRoute jest bezpośredni, dedykowane połączenie z sieci WAN (nie publicznie w Internecie) do programu Microsoft Services, w tym Azure. Witryny do witryny sieci VPN ruch podróży szyfrowane publicznie w Internecie. Możliwość skonfigurowania połączenia VPN witryny do witryny i ExpressRoute dla tej samej sieci wirtualnej ma wiele zalet.

Konfigurowanie sieci VPN witryny do witryny jako ścieżkę bezpiecznego pracy awaryjnej dla ExpressRoute lub nawiązywanie połączenia z witryn, które nie są częścią sieci, ale które są połączone za pośrednictwem ExpressRoute za pomocą sieci VPN witryny do witryny. Powiadomienie to wymaga dwóch bram wirtualną sieć dla tej samej sieci wirtualnej go przy użyciu - GatewayType Vpn, a drugi — za pomocą - GatewayType ExpressRoute.


![Połączenie Coexist] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Modele wdrożenia i metody S2S i ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Następne kroki

Planowanie konfiguracji bramy sieci VPN. Zobacz [VPN brama planowanie i projektowanie](vpn-gateway-plan-design.md) i [Łączenie sieci lokalnej Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
