<properties
   pageTitle="Publiczne i prywatne w Menedżerze zasobów Azure adresów IP | Microsoft Azure"
   description="Dowiedz się więcej o publiczne i prywatne w Menedżerze zasobów Azure adresów IP"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>Adresy IP platformy Azure
Adresy IP można przypisać Azure zasoby, aby komunikować się z innymi zasobami Azure, sieci lokalnej i w Internecie. Istnieją dwa typy adresów IP, których można używać w Azure:

- **Publiczne adresy IP**: na potrzeby komunikacji z Internetem, łącznie z usług Azure publicznej
- **Prywatne adresy IP**: na potrzeby komunikacji w ramach Azure wirtualnej sieci (VNet), a do lokalnej sieci użycie bramy sieci VPN lub obwód ExpressRoute rozszerzenie sieci Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](virtual-network-ip-addresses-overview-classic.md).

Jeśli znasz model klasyczny wdrażania, sprawdź [różnice w adresowanie między klasyczny IP i Menedżera zasobów](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Publiczne adresy IP
Publiczne adresy IP Zezwalaj Azure zasobów w celu komunikowania się z Internetem i Azure usług publicznej, takich jak [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/), [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/) [baz danych programu SQL](../sql-database/sql-database-technical-overview.md)i [Azure miejsca do magazynowania](../storage/storage-introduction.md).

W Menedżerze zasobów Azure publiczny adres [IP](resource-groups-networking.md#public-ip-address) jest zasobu, która ma swoje właściwości. Publicznej zasobu adresu IP można skojarzyć z dowolną z następujących zasobów:

- Maszyn wirtualnych (maszyn wirtualnych)
- Urządzenia do równoważenia obciążenia z Internetu
- Bramy sieci VPN
- Bram aplikacji

### <a name="allocation-method"></a>Metoda alokacji
Istnieją dwie metody, w których adres IP przydzielonego do *publicznej* zasobu IP - *dynamiczne* lub *statyczne*. Metoda alokacji domyślny jest *dynamiczne*, gdzie adres IP **nie** jest przydzielona w momencie jego utworzenia. Zamiast tego publiczny adres IP przydzielonego po uruchomieniu (lub utworzyć) skojarzone zasobów (na przykład równoważenia obciążenia lub maszyn wirtualnych). Adres IP jest publikowany zatrzymać (lub usunięcie) zasobu. Ta opcja powoduje adresu IP w celu zmianie zatrzymać i uruchomić zasobu.

Aby upewnić się, że adres IP skojarzony zasób pozostaje taki sam, można ustawić metodę alokacji jawnie *statyczne*. W tym przypadku przypisany adres IP jest od razu. Wydaniu tylko wtedy, gdy Usuń zasób lub zmienić jego metoda alokacji *dynamiczny*.

>[AZURE.NOTE] Nawet wtedy, gdy ustawiono metodę alokacji *statyczne*, nie możesz określić adres IP rzeczywista przydzielonych do *zasobu IP publicznej*. Zamiast tego otrzymuje przydzielane z puli dostępnych adresów IP w lokalizacji Azure, jest tworzony w dla tego zasobu.

Publicznych adresów IP są często używane w następujących sytuacjach:

- Użytkownicy końcowi wymagana jest aktualizacja reguły zapory na komunikowanie się z zasobami Azure.
- Rozpoznawanie nazw DNS, gdzie zmiany w polu adres IP wymaga zaktualizowanie rekordów.
- Zasoby Azure komunikować się z innych aplikacji lub usługi, które używają model zabezpieczeń oparty na adres IP.
- Możesz użyć certyfikatów SSL połączone z adresem IP.

>[AZURE.NOTE] Lista zakresów adresów IP, z których publicznych adresów IP (dynamiczne statyczny) są przydzielane Azure zasobów jest opublikowany w [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Rozpoznawanie nazwy hosta DNS
Możesz określić etykietę nazwy DNS domeny dla publicznej zasobu adresów IP, który tworzy mapowanie *domainnamelabel*. *Lokalizacja*. cloudapp.azure.com publiczny adres IP na serwerach DNS zarządzane Azure. Na przykład po utworzeniu publicznej zasobu IP z **contoso** jako *domainnamelabel* w **Stanach Zjednoczonych zachód** Azure *lokalizacji*, nazwy (FQDN) domeny w pełni kwalifikowane **contoso.westus.cloudapp.azure.com** będzie rozpoznawać publiczny adres IP zasobu. Za pomocą tej nazwy FQDN do utworzenia rekordu CNAME domeny niestandardowej, wskazująca publiczny adres IP platformy Azure.

>[AZURE.IMPORTANT] Poszczególne etykiety nazw domen utworzone musi być unikatowa w lokalizacji Azure.  

### <a name="virtual-machines"></a>Maszyn wirtualnych
Publiczny adres IP można skojarzyć z [systemu Windows](../virtual-machines/virtual-machines-windows-about.md) i [Linux oraz](../virtual-machines/virtual-machines-linux-about.md) maszyn wirtualnych, przypisując do jego **sieciowej**. W przypadku wielu sieciowej maszyn wirtualnych można przypisać go do tylko *podstawowy* interfejs sieci. Możesz przypisać dynamiczne lub statyczne publiczny adres IP maszyny.

### <a name="internet-facing-load-balancers"></a>Urządzenia do równoważenia obciążenia z Internetu
Publiczny adres IP można skojarzyć z [Usługi równoważenia obciążenia Azure](../load-balancer/load-balancer-overview.md), przypisując do konfiguracji **serwera sieci Web** usługi równoważenia obciążenia. Ten publiczny adres IP służy jako równoważenia obciążenia wirtualny adres IP (VIP). Dynamiczne lub statyczne publiczny adres IP można przypisać do równoważenia obciążenia zewnętrzną. Możesz też przypisać wiele publicznych adresów IP usługi równoważenia obciążenia zewnętrzną, która umożliwia [VIP wielu](../load-balancer/load-balancer-multivip.md) scenariuszy, takich jak środowisku wielu dzierżawy z witrynami sieci Web opartych na SSL.

### <a name="vpn-gateways"></a>Bramy sieci VPN
[Brama VPN Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md) jest używany do łączenia Azure wirtualnej sieci (VNet), inne VNets Azure lub sieci lokalnej. Chcesz przypisać publiczny adres IP jego **konfiguracji IP** w celu mogła nawiązać połączenia z siecią zdalną. *Dynamiczne* publiczny adres IP można obecnie przypisać tylko do bramy sieci VPN.

### <a name="application-gateways"></a>Bram aplikacji
Publiczny adres IP można skojarzyć z Azure [Bramy aplikacji](../application-gateway/application-gateway-introduction.md), przypisując do konfiguracji **serwera sieci Web** bramy. Ten publiczny adres IP służy jako VIP usługi równoważenia obciążenia. Obecnie możesz można przypisać tylko *dynamiczne* publiczny adres IP serwera sieci Web konfiguracji bramy aplikacji.

### <a name="at-a-glance"></a>W skrócie
Poniższa tabela zawiera określone właściwości, za pomocą którego można skojarzyć z zasobów najwyższego poziomu i metod możliwej alokacji (dynamiczne lub statyczne), które mogą być używane publiczny adres IP.

|Zasób najwyższego poziomu|Adres IP skojarzenia|Dynamiczne|Statyczne|
|---|---|---|---|
|Maszyn wirtualnych|Interfejs sieciowy|Tak|Tak|
|Usługa równoważenia obciążenia|Konfiguracja fronton|Tak|Tak|
|Brama VPN|Konfiguracja IP bramy|Tak|Brak|
|Brama aplikacji|Konfiguracja fronton|Tak|Brak|

## <a name="private-ip-addresses"></a>Prywatne adresy IP
Prywatne adresy IP Zezwalaj Azure zasoby do komunikowania się z innymi zasobami w [wirtualnej sieci](virtual-networks-overview.md) lub sieci lokalnej za pośrednictwem sieci VPN brama lub obwód ExpressRoute bez użycia adresu IP dostępne Internet.

W modelu wdrożenia Azure Menedżera zasobów prywatny adres IP jest skojarzony z następującymi typami Azure zasoby:

- Maszyny wirtualne
- Urządzenia do równoważenia obciążenia wewnętrzny (ILBs)
- Bram aplikacji

### <a name="allocation-method"></a>Metoda alokacji
Przydzielono prywatny adres IP z zakresu adresów podsieci, do której dołączono zasobu. Zakres adresów podsieć jest częścią VNet zakres adresów.

Istnieją dwie metody, w których jest przydzielony prywatny adres IP: *dynamiczne* lub *statyczne*. Domyślną metodę alokacji jest *dynamiczne*, której adres IP jest automatycznie przydzielany z podsieci zasobu (za pomocą DHCP). Ten adres IP można zmienić po zatrzymać i uruchomić tego zasobu.

Można ustawić metodę alokacji *statyczne* upewnij się, że adres IP nie zmienia się. W tym przypadku także należy podać prawidłowy adres IP, który jest częścią podsieci zasobu.

Prywatne adresów IP są często używane do:

- Maszyny wirtualne, które działają jak kontrolery domeny lub serwery DNS.
- Zasoby, które wymagają reguły zapory przy użyciu adresów IP.
- Zasoby dostęp do innych aplikacji i zasobów za pomocą adresu IP.

### <a name="virtual-machines"></a>Maszyn wirtualnych
Prywatny adres IP jest przypisany **sieciowej** [systemu Windows](../virtual-machines/virtual-machines-windows-about.md) i [Linux oraz](../virtual-machines/virtual-machines-linux-about.md) maszyn wirtualnych. W przypadku wielu sieciowej maszyn wirtualnych każdy interfejs otrzymuje prywatny adres IP przydzielone. Metoda alokacji można określić jako dynamiczne lub statyczne dla karty sieciowej.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Wewnętrzna rozpoznawania nazwy hosta DNS (w przypadku maszyny wirtualne)
Wszystkie Azure maszyny wirtualne są skonfigurowane z [serwerów DNS zarządzane Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) domyślnie, chyba że wyraźnie skonfigurować niestandardowe serwerów DNS. Te serwery DNS zapewniają rozpoznawanie nazw wewnętrznych dla maszyny wirtualne, które znajdują się w tym samym VNet.

Po utworzeniu maszyny mapowanie hostname jego prywatny adres IP jest dodawany do serwerów DNS zarządzane Azure. W przypadku wielu sieciowej maszyn wirtualnych nazwa hosta jest zamapowany na adres IP prywatnego interfejsu sieci podstawowej.

Maszyny wirtualne skonfigurowano serwery DNS zarządzane Azure będą mogli rozpoznawać nazwy hostów o wszystkich maszyny wirtualne w ich VNet ich prywatnych adresów IP.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Urządzenia do równoważenia obciążenia wewnętrzny (ILB) i bram aplikacji
Prywatny adres IP można przypisać konfiguracji **fronton** [Azure wewnętrznych równoważenia obciążenia](../load-balancer/load-balancer-internal-overview.md) (ILB) lub [Azure aplikacji bramy](../application-gateway/application-gateway-introduction.md). Ten adres prywatny IP służy jako punkt końcowy wewnętrznych dostępne tylko dla zasobów w swojej sieci wirtualnych (VNet) i sieci zdalnych podłączony do VNet. W konfiguracji fronton można przypisać albo dynamiczne lub statyczne prywatny adres IP.

### <a name="at-a-glance"></a>W skrócie
Poniższa tabela zawiera określone właściwości, za pomocą którego można skojarzyć z zasobów najwyższego poziomu i metod możliwej alokacji (dynamiczne lub statyczne), które mogą być używane prywatny adres IP.

|Zasób najwyższego poziomu|Skojarzenia adres IP|Dynamiczne|Statyczne|
|---|---|---|---|
|Maszyn wirtualnych|Interfejs sieciowy|Tak|Tak|
|Usługa równoważenia obciążenia|Konfiguracja fronton|Tak|Tak|
|Brama aplikacji|Konfiguracja fronton|Tak|Tak|

## <a name="limits"></a>Ograniczenia

Ograniczenia nałożone na adresy IP są oznaczone w pełny zestaw [limitów sieciach](azure-subscription-service-limits.md#networking-limits) platformy Azure. Limity te są według regionów i subskrypcji. Możesz [kontakt z pomocą techniczną](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby zwiększyć limity domyślne maksymalnie maksymalną, w zależności od potrzeb biznesowych.

## <a name="pricing"></a>Cennik

Publiczne adresy IP, może być nominalna opłat. Aby dowiedzieć się więcej na temat ceny adresu IP w Azure, sprawdź stronę [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) .

## <a name="next-steps"></a>Następne kroki
- [Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu portalu Azure](virtual-network-deploy-static-pip-arm-portal.md)
- [Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu szablonu](virtual-network-deploy-static-pip-arm-template.md)
- [Wdrażanie maszyn wirtualnych przy użyciu statycznego adresu IP prywatnych za pomocą portalu Azure](virtual-networks-static-private-ip-arm-pportal.md)
