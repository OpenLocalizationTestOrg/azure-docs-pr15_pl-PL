<properties
   pageTitle="Publiczne i prywatne adresy IP (klasyczny) platformy Azure | Microsoft Azure"
   description="Dowiedz się więcej o publicznych i prywatnych adresów IP platformy Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>Adresy IP (klasyczny) platformy Azure
Adresy IP można przypisać Azure zasoby, aby komunikować się z innymi zasobami Azure, sieci lokalnej i w Internecie. Istnieją dwa typy adresów IP, możesz użyć w Azure: publiczne i prywatne.

Publiczne adresy IP są używane do komunikacji z Internetem, łącznie z usług Azure publicznej.

Prywatne adresy IP są używane komunikacji w ramach Azure wirtualnej sieci (VNet), usługi w chmurze i sieci lokalnej, korzystając z sieci VPN brama lub obwód ExpressRoute rozszerzenie sieci Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu wdrożenia Menedżera zasobów](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Publiczne adresy IP
Publiczne adresy IP Zezwalaj Azure zasobów w celu komunikowania się z Internetem i Azure usług publicznej, takich jak [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/), [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/) [baz danych programu SQL](../sql-database/sql-database-technical-overview.md)i [Azure miejsca do magazynowania](../storage/storage-introduction.md).

Publiczny adres IP jest skojarzony z następujących zasobów:

- Usług w chmurze
- IaaS wirtualnych maszyn
- Wystąpienia roli PaaS
- Bramy sieci VPN
- Bram aplikacji

### <a name="allocation-method"></a>Metoda alokacji
Jeśli publiczny adres IP musi być przydzielane do zasobu Azure, jest *dynamicznie* przydzielane z puli dostępne publiczny adres IP w lokalizacji, w której jest tworzona zasobu. Ten adres IP jest publikowany po zatrzymaniu zasobu. W przypadku, gdy z usługi w chmurze, polega zostały zatrzymane wszystkie wystąpienia roli, które można uniknąć przy użyciu *statycznego* (zastrzeżone) adres IP (zobacz [Usług w chmurze](#Cloud-services)).

>[AZURE.NOTE] Lista zakresów adresów IP, z których publiczne adresy IP są przydzielane Azure zasobów jest opublikowany w [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Rozpoznawanie nazwy hosta DNS
Podczas tworzenia usługi w chmurze lub maszyn wirtualnych IaaS, musisz podać nazwę DNS usługi cloud, który jest unikatowy przez wszystkie zasoby w Azure. Serwery DNS zarządzane Azure dla *nazwa_DNS*spowoduje to utworzenie mapowania. cloudapp.net publiczny adres IP zasobu. Na przykład podczas tworzenia usługi w chmurze z nazwą chmury usługi DNS **firmy contoso**, nazwę (FQDN) domeny w pełni kwalifikowane **contoso.cloudapp.net** rozwiąże publiczny adres IP (VIP): usługi w chmurze. Za pomocą tej nazwy FQDN do utworzenia rekordu CNAME domeny niestandardowej, wskazująca publiczny adres IP platformy Azure.

### <a name="cloud-services"></a>Usług w chmurze
Usługa w chmurze ma zawsze publiczny adres IP określane jako wirtualny adres IP (VIP). Możesz utworzyć punkty końcowe w usłudze w chmurze skojarzyć różne porty VIP do wewnętrznego portów maszyny wirtualne i wystąpień rola w usłudze w chmurze. 

Usługa w chmurze może zawierać wiele maszyny wirtualne IaaS lub wystąpienia roli PaaS, wszystkie dostępne za pośrednictwem samej VIP usługi cloud. Można także przypisać [wiele służącą do usługi w chmurze](../load-balancer/load-balancer-multivip.md), który umożliwia VIP wielu scenariuszy, takich jak środowiska wielu dzierżawy z witrynami sieci Web opartych na SSL.

Upewnij się, że publiczny adres IP usługi w chmurze pozostaje bez zmian, nawet wtedy, gdy wszystkie wystąpienia roli zostały zatrzymane przy użyciu *statycznego* publiczny adres IP, określane jako [Zastrzeżony adres IP](virtual-networks-reserved-public-ip.md). Można utworzyć zasób statyczną IP (zastrzeżone) w określonej lokalizacji i przypisać go do dowolnej usługi w chmurze w tym miejscu. Nie można określić rzeczywisty adres IP dla zastrzeżonego adresu IP, przydzielonego z puli adresy IP dostępne w lokalizacji, w której jest tworzona. Ten adres IP nie jest wydawane, dopóki nie zostaną jawnie usunięte.

W przypadku, gdy usługa w chmurze, statyczny (zastrzeżone) publiczne adresy IP są często używane w scenariusze:

- wymaga reguł zapory, które można zainstalować przez użytkowników końcowych.
- w zależności od zewnętrznych rozpoznawania nazw DNS i aktualizowanie rekordów wymaga dynamicznych adresów IP.
- Korzystanie z usługi zewnętrznej strony sieci web, które używają model zabezpieczeń IP podstawie.
- używa certyfikatów SSL połączone z adresem IP.

>[AZURE.NOTE] Po utworzeniu klasyczny maszyn wirtualnych kontenera *usługi w chmurze* jest tworzona przez Azure, w której znajduje się wirtualny adres IP (VIP). Po zakończeniu tworzenia portalu domyślnej RDP lub SSH *punkt końcowy* jest konfigurowany przez portalu, możesz łączyć się maszyn wirtualnych za pośrednictwem VIP usługi w chmurze. Można zarezerwować ten VIP usługi cloud, który zapewnia skuteczne zastrzeżonego adresu IP, aby nawiązać połączenie maszyn wirtualnych. Możesz otworzyć dodatkowe porty przez skonfigurowanie więcej punktów końcowych.

### <a name="iaas-vms-and-paas-role-instances"></a>Wystąpienia roli maszyny wirtualne IaaS i PaaS
Można przypisać publiczny adres IP bezpośrednio do IaaS [maszyn wirtualnych](../virtual-machines/virtual-machines-linux-about.md) lub wystąpienia roli PaaS w obrębie usługi w chmurze. To jest określane jako poziom wystąpienia publiczny adres IP ([ILPIP](virtual-networks-instance-level-public-ip.md)). Tylko może być dynamiczny ten publiczny adres IP.

>[AZURE.NOTE] To różni się od VIP usługi cloud, która jest kontenerem dla maszyny wirtualne IaaS lub PaaS wystąpienia roli, ponieważ usługi w chmurze mogą zawierać wiele IaaS maszyny wirtualne lub wystąpienia roli PaaS, wszystkie dostępne za pośrednictwem samej VIP usługi cloud.

### <a name="vpn-gateways"></a>Bramy sieci VPN
[Brama VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) może służyć do VNet Azure połączyć się z innymi Azure VNets lub sieci lokalnej. Brama VPN przypisano publicznej IP adres *dynamiczne*, która umożliwia komunikowanie się z siecią zdalną.

### <a name="application-gateways"></a>Bram aplikacji
Azure [bramy aplikacji](../application-gateway/application-gateway-introduction.md) może służyć do Layer7 równoważenia obciążenia do rozsyłania ruchu sieciowego według HTTP. Brama aplikacji przypisano publicznej IP adres *dynamiczne*, który służy jako VIP równoważenia obciążenia.

### <a name="at-a-glance"></a>Rzut oka
W poniższej tabeli pokazano każdego typu zasobów z metod alokacji możliwości (dynamic statyczny), a także możliwość przypisać wiele publicznych adresów IP.

|Zasób|Dynamiczne|Statyczne|Wiele adresów IP|
|---|---|---|---|
|Usługa w chmurze|Tak|Tak|Tak|
|Wystąpienia roli maszyn wirtualnych IaaS lub PaaS|Tak|Brak|Brak|
|Brama VPN|Tak|Brak|Brak|
|Brama aplikacji|Tak|Brak|Brak|

## <a name="private-ip-addresses"></a>Prywatne adresy IP
Prywatne adresy IP Zezwalaj Azure zasoby, aby komunikować się z innymi zasobami usługi w chmurze lub [wirtualną sieć](virtual-networks-overview.md)(VNet) lub sieci lokalnej (za pośrednictwem sieci VPN brama lub obwód ExpressRoute), bez użycia adresu IP dostępne Internet.

W modelu Azure wdrożenia klasyczny prywatny adres IP może przybierać następujące zasoby Azure:

- Wystąpienia roli maszyny wirtualne IaaS i PaaS
- Usługi równoważenia obciążenia wewnętrznych
- Brama aplikacji

### <a name="iaas-vms-and-paas-role-instances"></a>Wystąpienia roli maszyny wirtualne IaaS i PaaS
Wirtualnych maszyn utworzone za pomocą modelu Klasyczny wdrożenia są zawsze umieszczane w usłudze w chmurze, podobne do PaaS roli wystąpienia. Zachowanie prywatne adresy IP są podobne do tych zasobów.

Należy pamiętać, że usługi w chmurze mogą być rozmieszczone na dwa sposoby:

- Jako *autonomicznego* usługi w chmurze, której nie znajduje się w wirtualnej sieci.
- W ramach wirtualnej sieci.

#### <a name="allocation-method"></a>Metoda alokacji
W przypadku *autonomicznej* usługi w chmurze zasoby prywatny IP adres przydzielane *dynamicznie* przejść z prywatny zakres adresów IP centrum danych Azure. Może służyć tylko do komunikacji z innymi maszyny wirtualne w tym samym usługi w chmurze. Ten adres IP można zmienić po zatrzymaniu i pracę zasobu.

W przypadku usługi w chmurze wdrożony w wirtualnej sieci zasoby Uzyskaj prywatnych adresów IP przydzielane z zakresu adresów skojarzone adresy podsieci używane (określone w konfiguracji sieci). Ten prywatnych adresów IP może służyć do komunikacji między wszystkie maszyny wirtualne w VNet.

Ponadto w przypadku usług w chmurze w VNet, prywatny adres IP przydzielonego *dynamiczne* (za pomocą DHCP) domyślnie. Można go zmienić po zatrzymaniu i pracę zasobu. Aby upewnić się, że adres IP pozostaje bez zmian, należy ustawić metodę alokacji *statyczne*i podaj prawidłowy adres IP w zakresie odpowiedniego adresu.

Prywatne adresów IP są często używane do:

 - Maszyny wirtualne, które działają jak kontrolery domeny lub serwery DNS.
 - Maszyny wirtualne, które wymagają reguły zapory przy użyciu adresów IP.
 - Maszyny wirtualne z usługami dostęp do innych aplikacji za pomocą adresu IP.

#### <a name="internal-dns-hostname-resolution"></a>Rozpoznawanie nazwy hosta DNS wewnętrznych
Wszystkie maszyny wirtualne Azure i PaaS roli wystąpienia są skonfigurowane z [serwerów DNS zarządzane Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) domyślnie, chyba że wyraźnie skonfigurować niestandardowe serwerów DNS. Te serwery DNS zapewniają rozpoznawania nazw wewnętrznych maszyny wirtualne i wystąpień roli, które znajdują się w tej samej usługi VNet lub w chmurze.

Po utworzeniu maszyny mapowanie hostname jego prywatny adres IP jest dodawany do serwerów DNS zarządzane Azure. W przypadku Głosowa wielokrotne NIC nazwa hosta jest zamapowany prywatny adres IP podstawowego karty sieciowej. Jednak te informacje mapowania są ograniczone do zasobów w tym samym usługi w chmurze lub VNet.

W przypadku *autonomicznej* usługi w chmurze można spróbować rozwiązać nazwy hostów wszystkich wystąpień maszyny wirtualne/ról w tym samym usług w chmurze tylko. W przypadku usługi w chmurze w VNet można spróbować rozwiązać nazwy hostów wszystkich wystąpień maszyny wirtualne/ról w VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Urządzenia do równoważenia obciążenia wewnętrzny (ILB) i bram aplikacji
Prywatny adres IP można przypisać konfiguracji **fronton** [Azure wewnętrznych równoważenia obciążenia](../load-balancer/load-balancer-internal-overview.md) (ILB) lub [Azure aplikacji bramy](../application-gateway/application-gateway-introduction.md). Ten adres prywatny IP służy jako punkt końcowy wewnętrznych dostępne tylko dla zasobów w swojej sieci wirtualnych (VNet) i sieci zdalnych podłączony do VNet. W konfiguracji fronton można przypisać albo dynamiczne lub statyczne prywatny adres IP. Można też przypisać wiele prywatnych adresów IP, aby umożliwić scenariusze obejmujące wielu vip.

### <a name="at-a-glance"></a>Rzut oka
W poniższej tabeli pokazano każdego typu zasobów z metod alokacji możliwości (dynamic statyczny), a także możliwość przypisać wiele prywatnych adresów IP.

|Zasób|Dynamiczne|Statyczne|Wiele adresów IP|
|---|---|---|---|
|Maszyn wirtualnych (w *wersji autonomicznej* usługi w chmurze)|Tak|Tak|Tak|
|PaaS roli wystąpienia (w *wersji autonomicznej* usługi w chmurze)|Tak|Brak|Tak|
|Wystąpienia roli maszyn wirtualnych lub PaaS (w VNet)|Tak|Tak|Tak|
|Fronton równoważenia obciążenia wewnętrznych|Tak|Tak|Tak|
|Serwer bramy aplikacji sieci Web|Tak|Tak|Tak|

## <a name="limits"></a>Ograniczenia

W poniższej tabeli przedstawiono ograniczenia nałożone na IP adresowania platformy Azure na subskrypcję. Możesz [kontakt z pomocą techniczną](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby zwiększyć limity domyślne maksymalnie maksymalną, w zależności od potrzeb biznesowych.

||Domyślny limit|Limit maksymalny|
|---|---|---|
|Publiczne adresy IP (dynamiczny)|5|kontakt z pomocą techniczną|
|Zastrzeżone publiczne adresy IP|20|kontakt z pomocą techniczną|
|Publiczne VIP na wdrożenia (usługa w chmurze)|5|kontakt z pomocą techniczną|
|Prywatne VIP (ILB) na wdrożenia (usługa w chmurze)|1|1|

Upewnij się, że Przeczytaj pełny zestaw [limitów sieciach](azure-subscription-service-limits.md#networking-limits) platformy Azure.

## <a name="pricing"></a>Cennik

W większości przypadków publiczne adresy IP są bezpłatne. Istnieje nominalna opłata za pomocą dodatkowych i/lub statyczne publicznych adresów IP. Upewnij się, że wiesz, [ceny struktury publiczne adresy IP](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Różnice między Menedżer zasobów i wdrożeń klasyczny
Poniżej znajduje się porównanie funkcji adresów IP Menedżer zasobów i model klasyczny wdrożenia.

||Zasób|Klasyczny|Menedżer zasobów|
|---|---|---|---|
|**Publiczny adres IP**|MASZYN WIRTUALNYCH|Określane jako ILPIP (tylko dynamiczne)|Określane jako publiczny adres IP (dynamiczne lub statyczne)|
|||Przypisane do maszyn wirtualnych IaaS lub w przypadku wystąpienia roli PaaS|Skojarzona NIC maszyn wirtualnych|
||Internet przeciwległych równoważenia obciążenia|Określane jako VIP (dynamiczne) lub zastrzeżony adres IP (statyczny)|Określane jako publiczny adres IP (dynamiczne lub statyczne)|
|||Przypisane do usługi w chmurze|Skojarzony z konfiguracji fronton równoważenia obciążenia|
||||
|**Prywatny adres IP**|MASZYN WIRTUALNYCH|Określane jako DIP|Określane jako prywatny adres IP|
|||Przypisane do maszyn wirtualnych IaaS lub w przypadku wystąpienia roli PaaS|Przypisane do NIC maszyn wirtualnych|
||Usługi równoważenia obciążenia wewnętrzny (ILB)|Przypisane do ILB (dynamiczne lub statyczne)|Przypisane do konfiguracji fronton ILB (dynamiczne lub statyczne)|

## <a name="next-steps"></a>Następne kroki
- [Rozmieszczanie maszyn wirtualnych przy użyciu statycznego adresu IP prywatnych](virtual-networks-static-private-ip-classic-pportal.md) za pomocą portalu klasyczny.
