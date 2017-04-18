<properties
   pageTitle="Omówienie Azure wirtualnej sieci (VNet)"
   description="Więcej informacji na temat wirtualnych sieci (VNets) platformy Azure."
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

# <a name="virtual-network-overview"></a>Omówienie wirtualnych sieci

Azure wirtualnej sieci (VNet) jest reprezentacją sieci w chmurze.  Jest logiczne izolacji chmura Azure dedykowane do swojej subskrypcji. W pełni można kontrolować bloki adresów IP, ustawień DNS, zasady zabezpieczeń i trasy do tabel w obrębie tej sieci. Możesz także dodatkowo segmentu z VNet na podsieci i uruchamianie Azure IaaS wirtualnych maszyn i/lub [usług w chmurze (PaaS roli wystąpień)](../cloud-services/cloud-services-choose-me.md). Ponadto można nawiązać sieci wirtualnej sieci lokalnej przy użyciu jednej z [opcji łączności](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) dostępnych w Azure. W zasadzie można rozwinąć sieci Azure, z pełną kontrolę nad bloki adresów IP przy użyciu korzyści skali przedsiębiorstwa, który udostępnia Azure.

Aby lepiej zrozumieć VNets, zapoznaj się poniżej, rysunek przedstawia sieci lokalnej uproszczone.

![Sieci lokalnej](./media/virtual-networks-overview/figure01.png)

Na ilustracji powyżej przedstawiono sieć lokalnego połączenia z Internetem za pośrednictwem routera. Można też wyświetlić zaporę między router i strefy Zdemilitaryzowanej hostingu serwer DNS i farmy serwerów sieci web. Farmy serwerów sieci web jest równoważenia przy użyciu równoważenia obciążenia sprzętu, jest udostępniana w Internecie, która powoduje zużycie zasobów z wewnętrznej podsieci obciążenia. Podsieci wewnętrznej jest oddzielony od strefy Zdemilitaryzowanej przez inną zaporę i serwery Active Directory kontrolera domeny hosts, serwerów baz danych i serwerów aplikacji.

Tej samej sieci może być obsługiwany w Azure, jak pokazano na poniższej ilustracji.

![Azure wirtualnej sieci](./media/virtual-networks-overview/figure02.png)

Zwróć uwagę, jak Azure infrastruktury przejmuje roli routera, zezwolenia na dostęp ze swojego VNet do publicznego Internetu bez konieczności dowolnej konfiguracji. Grupy zabezpieczeń sieci (NSGs) zastosowane do wszystkich poszczególnych podsieci można zastąpić zapory. I urządzenia do równoważenia obciążenia fizycznie są zastępowane przez internet wewnętrznych i przeciwległych urządzenia do równoważenia obciążenia platformy Azure.

>[AZURE.NOTE] Istnieją dwa tryby wdrażania platformy Azure: klasyczny (nazywane także zarządzanie usługą) i Azure Menedżera zasobów (rąk). Klasyczny VNets mogą zostać dodane do grupy koligacji lub utworzone jako VNet regionalne. Jeśli masz VNet w grupie koligacji zaleca się migrowanie [go do regionalnych VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Zalety wirtualnej sieci

- **Izolacji**. VNets są całkowicie odizolowane od siebie. Umożliwia tworzenie rozłączne sieci rozwijanie, testowanie i produkcji korzystające z tym samym bloki adresów CIDR.

- **Dostęp do publicznego Internetu**. Wszystkie maszyny wirtualne IaaS i PaaS wystąpień ról w VNet dostęp do publicznego Internetu domyślnie. Dostęp można kontrolować, za pomocą sieci grup zabezpieczeń (NSGs).

- **Dostęp do maszyny wirtualne w VNet**. W tej samej sieci wirtualnej można uruchomione wystąpienia roli PaaS i maszyny wirtualne IaaS i mogli oni połączyć ze sobą za pomocą prywatnych adresów IP, nawet jeśli znajdują się w różnych podsieci bez konieczności Konfigurowanie bramy lub użyj publicznych adresów IP.

- **Rozpoznawanie nazw**. Azure udostępnia rozpoznawanie nazw wewnętrznych maszyny wirtualne IaaS i PaaS roli wystąpień wdrożony w swojej VNet. Można także wdrożyć serwery DNS i skonfigurować VNet ich stosowania.

- **Zabezpieczenia**. Ruch wprowadzania i kończenie pracy maszyn wirtualnych i wystąpień roli PaaS w VNet można sterować przy użyciu grup zabezpieczeń sieci.

- **Łączność**. VNets można łączyć ze sobą za pomocą bram sieci lub zaglądanie VNet. VNets mogą być połączone z centrami danych lokalnych za pośrednictwem sieci VPN witryny do witryny lub Azure ExpressRoute. Aby dowiedzieć się więcej na temat połączenia VPN witryny do witryny, odwiedź stronę [o VPN bramy](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Aby dowiedzieć się więcej na temat ExpressRoute, odwiedź stronę [ExpressRoute omówienie kwestii technicznych](../expressroute/expressroute-introduction.md). Aby dowiedzieć się więcej o VNet zaglądanie, odwiedź stronę [zaglądanie VNet](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Upewnij się, że tworzenie VNet przed wdrożeniem maszyny wirtualne IaaS ani PaaS roli wystąpienia do środowiska usługi Azure. Oparty na architekturze ARM maszyny wirtualne wymagają VNet i jeśli nie określisz istniejących VNet Azure tworzy domyślny VNet, który może być CIDR adres bloku mogą powodować konfliktów z sieci lokalnej. Uniemożliwienie nawiązywanie połączenia z VNet do sieci lokalnej.

## <a name="subnets"></a>Podsieci

Podsieć jest zakres adresów IP w VNet, VNet można podzielić na wiele podsieci dla organizacji i zabezpieczeń. Maszyny wirtualne i wystąpień roli PaaS wdrożony podsieci (w tym samym lub innym) w VNet można komunikować się ze sobą bez żadna dodatkowa konfiguracja. Można również skonfigurować trasę tabel i NSGs do podsieci.

## <a name="ip-addresses"></a>Adresy IP


Istnieją dwa typy adresów IP przypisanych do zasobów w Azure: *publiczne* i *prywatne*. Publiczne adresy IP Zezwalaj Azure zasobów w celu komunikowania się z Internetem i innych Azure usługi publiczne, takie jak [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/), [Koncentratory zdarzenia Azure](https://azure.microsoft.com/documentation/services/event-hubs/). Adresy IP prywatnego pozwala komunikacji między zasobów w sieci wirtualnej, wraz z tymi połączenia przez sieć VPN, bez użycia adresy internetowe routingu IP.

Aby dowiedzieć się więcej na temat adresów IP w Azure, odwiedź stronę [adresów IP w wirtualnej sieci](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Urządzenia do równoważenia obciążenia Azure

Maszyn wirtualnych i usług w chmurze w wirtualnej sieci może nastąpić Internetem przy użyciu urządzenia do równoważenia obciążenia Azure. Aplikacje biznesowe, które są wewnętrznej stronie można tylko równoważenia przy użyciu usługi równoważenia obciążenia wewnętrznych obciążenia.

- **Usługi równoważenia obciążenia zewnętrznych**. Za pomocą usługi równoważenia obciążenia zewnętrznych aby zapewnić wysoką dostępność maszyny wirtualne IaaS i PaaS wystąpienia roli uzyskać do nich dostęp z publicznego Internetu.

- **Usługa równoważenia obciążenia wewnętrzne**. Za pomocą usługi równoważenia obciążenia wewnętrznych zapewnienie wysokiej dostępności maszyny wirtualne IaaS i wystąpień roli PaaS uzyskać do nich dostęp z innych usług w Twojej VNet.

Aby dowiedzieć się więcej na temat równoważenia obciążenia w Azure, odwiedź stronę [Podgląd równoważenia obciążenia](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Grupa zabezpieczeń sieci (NSG)

Możesz utworzyć NSGs kontrolować ruch przychodzący i wychodzący dostęp do interfejsów sieciowych (NIC) maszyny wirtualne i podsieci. Każdy NSG zawiera jeden lub więcej reguł, określając, czy ruch zostanie zatwierdzony lub odmowa oparte na źródłowy adres IP, port źródłowy, docelowy adres IP i port docelowy. Aby dowiedzieć się więcej na temat NSGs, odwiedź stronę [Co to jest grupa zabezpieczeń sieci](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Wirtualna urządzenia

Wirtualna urządzenie jest po prostu kolejną maszyn wirtualnych w swojej VNet uruchamiającego funkcję urządzenia oprogramowanie podstawie, takich jak zapory, optymalizacja WAN lub wykrywanie intruzów. Możesz utworzyć trasę w Azure, aby skierować ruch do VNet za pośrednictwem wirtualnych urządzenia, aby używać funkcji.

Na przykład NSGs może służyć do zabezpieczenia na Twojej VNet. NSGs zapewnia jednak warstwy 4 listy kontroli dostępu (ACL) do pakietów przychodzących i wychodzących. Jeśli chcesz używać modelu zabezpieczeń 7 warstwy, należy użyć urządzenia zapory.

Wirtualna urządzenia zależy od tego, [drogi i przesyłanie dalej IP zdefiniowane przez użytkownika](virtual-networks-udr-overview.md).

## <a name="limits"></a>Ograniczenia
Ograniczenia liczby wirtualnych sieci dozwolone w subskrypcji, aby uzyskać więcej informacji znajdują się [ograniczenia sieciowe Azure](../azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Cennik
Jest bez żadnych dodatkowych kosztów dla platformy Azure za pomocą wirtualnych sieci. Wystąpienia obliczeń uruchomione w Vnet jest rozliczana według stawki standardowej zgodnie z opisem w [Ceny maszyn wirtualnych Azure](https://azure.microsoft.com/pricing/details/virtual-machines/). [Bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) i [publicznych adresów IP] (https://azure.microsoft.com/pricing/details/ip-addresses/) używane w VNet będzie również opłaconych stawki standardowej.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie VNet](virtual-networks-create-vnet-arm-pportal.md) i podsieci.
- [Tworzenie maszyn wirtualnych w VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Informacje na temat [NSGs](virtual-networks-nsg.md).
- Informacje na temat [drogi i przesyłanie dalej IP zdefiniowane przez użytkownika](virtual-networks-udr-overview.md).
