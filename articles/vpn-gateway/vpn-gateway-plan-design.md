<properties 
   pageTitle="Brama VPN planowanie i projektowanie | Microsoft Azure"
   description="Więcej informacji na temat planowania Brama VPN i projektowania krzyżowe lokalnej, hybrydowych i VNet do VNet połączenia"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planowanie i projektowanie dla bramy sieci VPN

Planowanie i projektowanie lokalnej krzyżowe i VNet do VNet konfiguracji usługi może być proste i złożone, w zależności od potrzeb sieci. W tym artykule opisano podstawowe zagadnienia dotyczące planowania i projektowania.

## <a name="planning"></a>Planowanie


### <a name="compare"></a>Opcje połączeń między lokalne

Jeśli chcesz bezpieczne łączenie witryny lokalnej wirtualną sieć, masz trzy różne sposoby, aby to zrobić: witryny do witryny, punktu do witryny i ExpressRoute. Porównanie połączeń różnych lokalnej infrastrukturze, które są dostępne. Wybrana opcja może zależeć od różnych zagadnienia, takie jak:


- Jakiego rodzaju przepustowość wymaga rozwiązania?
- Czy chcesz komunikowanie się publicznie w Internecie za pośrednictwem bezpieczne połączenie sieci VPN lub przez połączenie prywatne?
- Czy masz publiczny adres IP, dostępny do użytku?
- Czy planujesz używać urządzenia VPN? Jeśli tak, czy zgodny?
- Czy jest to połączenie kilku komputerach, czy chcesz połączenia stałe dla witryny?
- Jakiego rodzaju Brama VPN jest wymagane na potrzeby rozwiązanie, które chcesz utworzyć?
- Które bramy SKU należy użyć?


Poniższa tabela może pomóc zdecydować, najlepszym rozwiązaniem łączności rozwiązania.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Wymagania dotyczące bramy według typu VPN i SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Aby uzyskać więcej informacji o bramy wersji produktu zobacz [Ustawienia bramy sieci VPN](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Agreguj przepustowość według typu SKU i sieci VPN

W poniższej tabeli przedstawiono typy bramy i Szacowana przepustowość agregacji. Szacowany Agreguj przepustowość może być decydującymi czynnik w projekcie.
Ceny różnią się SKU bramy. Aby uzyskać informacje o cenach zobacz [Ceny bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/). Ta tabela odnosi się do Menedżera zasobów i modeli wdrażania klasyczny.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Obsługiwane konfiguracje według typu SKU i sieci VPN

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Przepływ pracy

Poniższa lista przedstawia typowe przepływu pracy dla łączności chmurze:

1.  Projektowanie i planowanie topologii łączności listy obszary adresów dla wszystkich sieci, z którymi chcesz się połączyć.
2.  Tworzenie Azure wirtualnej sieci. 
3.  Tworzenie bramy sieci VPN wirtualnej sieci.
4.  Tworzenie i konfigurowanie połączeń sieci lokalnej lub w innych sieciach wirtualnych (w razie potrzeby).
5.  Tworzenie i konfigurowanie połączenia punkt do witryny dla bramy sieci Azure VPN (w razie potrzeby).
 

## <a name="design"></a>Projektowanie

### <a name="topologies"></a>Topologii połączenia

Zacznij od przeglądająca diagramów w artykule [O Brama VPN](vpn-gateway-about-vpngateways.md) . Ten artykuł zawiera podstawowe diagramów modeli wdrażania dla każdego topologii (Menedżer zasobów lub klasyczna), a które wdrożenia narzędzi można używać do wdrażania konfiguracji.   

### <a name="designbasics"></a>Podstawowe informacje o projekcie

W poniższych sekcjach omówiono podstawy bramy sieci VPN. Rozważ też [ograniczenia usług sieci](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Informacje o podsieci

Podczas tworzenia połączenia, należy rozważyć zakresy podsieci. Nie możesz mieć nakładające się zakresy adresów podsieci. Nakładające się podsieć jest jednym wirtualnej sieci lub lokalizacji lokalnego zawiera tę samą przestrzeń adresową, zawierający w innej lokalizacji. Oznacza to, czy potrzebujesz usługi inżynierów sieci dla sieci lokalnej lokalnego do dotycząca szczególnych się zakresu do użycia dla adresów IP usługi Azure adresowania miejsca i podsieci. Potrzebujesz przestrzeń adresów, która nie jest używany w sieci lokalnej lokalnego. 

Unikanie nakładające się podsieci ważne jest również podczas pracy z połączeniami VNet do VNet. Jeśli zachodzi z podsieci, a adres IP istnieje zarówno na wysyłanie i docelowego VNets, nie VNet do VNet połączenia. Azure nie kierowania danych do innych VNet, ponieważ adres docelowy jest częścią wysyłająca VNet. 

Bramy sieci VPN wymagają określonej podsieci o nazwie podsieci bramy. Wszystkie podsieci bramy musi mieć nazwę GatewaySubnet działał poprawnie. Pamiętaj nie nazwę danej podsieci bramy pod inną nazwą, a nie wdrażanie maszyny wirtualne albo jakiś inny podsieci bramy. Zobacz [podsieci bramy](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Informacje o sieci lokalnej bram

Brama sieci lokalnej zazwyczaj odwołuje się do swojej lokalizacji lokalnego. W modelu Klasyczny wdrożenia bramy sieci lokalnej nosi nazwę lokalnej witryny sieci. Podczas konfigurowania bramy sieci lokalnej, nadaj mu nazwę, określić publiczny adres IP urządzenia VPN lokalnego i Określ prefiksy adresów, które znajdują się w lokalizacji lokalnej. Azure przegląda prefiksów adres docelowy dla ruchu sieciowego, sprawdza konfiguracji, która została wybrana bramy sieci lokalnej i przesyła pakiety odpowiednio. W razie potrzeby można modyfikować tych prefiksów adresów. Aby uzyskać więcej informacji zobacz [bram sieci lokalnej](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>Informacje o typach bramy

Wybieranie typu poprawna bramy dla danej topologii jest krytyczne. Jeśli wybierzesz niewłaściwego typu, Centrum nie będą działać prawidłowo. Typ bramy Określa, jak bramy samej łączy i to ustawienie konfiguracji modelu wdrożenia Menedżera zasobów.

Dostępne są następujące typy bramy:

- Sieci VPN
- ExpressRoute

#### <a name="connectiontype"></a>Informacje o typach połączeń

Każdy konfiguracja wymaga typu określonego połączenia. Dostępne są następujące typy połączeń:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Informacje o typach sieci VPN

Każdy konfiguracja wymaga określonego typu VPN. Jeśli polega na łączeniu dwóch konfiguracji, takich jak tworzenie połączenia witryny do witryny i połączenia punkt do witryny z tym samym VNet, należy użyć typu VPN, spełniającego wymagania obu połączeń.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

W poniższej tabeli przedstawiono typ VPN podczas mapowania konfiguracji każdego połączenia. Upewnij się, że typ VPN do bramy odpowiada konfiguracji, która ma zostać utworzona. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>Urządzenia VPN połączeń witryny do witryny

Aby skonfigurować połączenie witryny do witryny, bez względu na to model wdrożenia potrzebne następujące elementy:

- Urządzenie VPN jest zgodny z bram Azure VPN
- Adres IP protokołu IPv4 publicznej jest poza NAT

Musisz możliwości Konfigurowanie urządzenia VPN lub innej osobie, który można skonfigurować urządzenie dla Ciebie. Aby uzyskać więcej informacji o urządzeniach sieci VPN zobacz [temat VPN urządzenia](vpn-gateway-about-vpn-devices.md). Artykuł urządzenia VPN zawiera informacje dotyczące zatwierdzonych urządzeń, wymagania dotyczące urządzeń, które nie zostały zatwierdzone i łączy do dokumentów Konfiguracja urządzenia, jeśli to możliwe.

### <a name="forcedtunnel"></a>Należy rozważyć, czy routing wymuszonego tunelem

W przypadku większości konfiguracji można skonfigurować tunneling wymuszonego. Wymuszone tunneling pozwala Przekieruj lub "wymuszanie" cały ruch Internet związanych z powrotem do lokalizacji lokalnego za pośrednictwem tunelem VPN witryny do witryny, kontroli i inspekcji. Jest to wymagane krytycznych dla większości enterprise IT zasady. 

Bez wymuszonego tunneling ruch Internet związanych z pośrednictwem usługi SMS platformy Azure będzie zawsze przechodzenia z infrastruktury sieciowej Azure bezpośrednio się z Internetem, bez opcji pozwala sprawdzać lub kontrolować dane. Dostęp do Internetu nieautoryzowanego potencjalnie może prowadzić do ujawnienie informacji lub innych typów naruszenia bezpieczeństwa.

**Wymuszone tunelowania diagramu**

![Wymuszone Tunneling połączenia] (./media/vpn-gateway-plan-design/forced-tunnel.png "wymuszone tunneling")

W obu modeli wdrażania i za pomocą różnych narzędzi można skonfigurować wymuszonego połączenie tunelowania. Zobacz poniższej tabeli, aby uzyskać więcej informacji. Firma Microsoft aktualizować w tej tabeli jako nowe artykuły, nowe modele wdrożenia i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tabeli.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Następne kroki

Zobacz artykuły [VPN brama często zadawanych PYTAŃ](vpn-gateway-vpn-faq.md) i [O bramy sieci VPN](vpn-gateway-about-vpngateways.md) , aby uzyskać więcej informacji pomóc użytkownikowi w projekcie.

Aby uzyskać więcej informacji na temat określonych ustawień zobacz [Temat ustawienia bramy sieci VPN](vpn-gateway-about-vpn-gateway-settings.md).




