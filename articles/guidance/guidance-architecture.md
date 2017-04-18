
<properties
   pageTitle="Wskazówki Azure | desenie i wskazówki dotyczące | Microsoft Azure"
   description="Architektura Azure odwołania"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Architektura Azure odwołania

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Ta zawartość jest aktywne rozwoju. Okazuje się przydatne dzisiaj, aby udostępniamy jego podgląd. Firma Microsoft wartość swoją opinię._

Nasze architektury odwołania są rozmieszczone według scenariuszu z wielu powiązanych architektury grupowane.
Każdej poszczególnych architektury oferuje najważniejsze wskazówki i porady czynności, a także wykonywalny składnik, który zawiera zalecenia.
Wiele architektury jest stopniowe; Tworzenie na bieżąco poprzedniego architektury, którzy mają mniej wymagania.

## <a name="designing-your-infrastructure-for-resiliency"></a>Projektowanie infrastruktury dla elastyczności

Tę serię zaczyna się od najważniejsze wskazówki dotyczące optymalną konfigurację maszyn wirtualnych i culminates we wdrożeniu wielu region z pracą awaryjną.

- [Uruchamianie maszyn wirtualnych systemu Windows Azure](guidance-compute-single-vm.md)
- [Uruchamianie maszyny Linux Azure](guidance-compute-single-vm-linux.md)
- [Uruchamianie wielu maszyn wirtualnych skalowalność i dostępność](guidance-compute-multi-vm.md)
- [Uruchamianie maszyn wirtualnych systemu Windows dla architektury N warstwowych](guidance-compute-n-tier-vm.md)
- [Uruchamianie maszyn wirtualnych Linux oraz dla architektury N warstwowych](guidance-compute-n-tier-vm-linux.md)
- [Uruchamianie maszyn wirtualnych systemu Windows w wielu regionach wysokiej dostępności](guidance-compute-multiple-datacenters.md)
- [Uruchamianie maszyn wirtualnych Linux w wielu regionach wysokiej dostępności](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Łączenie sieci lokalnej Azure

Tę serię rozpoczyna się od pokazujące sposoby nawiązywania połączenia istniejącej sieci Azure. Następnie rozwija dotyczyć wymagania dotyczące dostępności i zabezpieczeń.

- [Implementacji architektury sieci hybrydowego z Azure i lokalnych sieci VPN](guidance-hybrid-network-vpn.md)
- [Implementacji architektury sieci hybrydowego z Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Implementacji architektury sieci hybrydowych wysokiej dostępności](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Zabezpieczanie sieci hybrydowego

Tej serii obejmuje sprawdzone wskazówki na temat tworzenia strefy Zdemilitaryzowanej platformy Azure do bezpiecznego połączenia pochodzące z centrum danych lokalnych i w Internecie.

- [Implementacji strefy Zdemilitaryzowanej między Azure i lokalnych centrum danych](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Implementacji strefy Zdemilitaryzowanej między Azure i Internet](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Tożsamość usługi

Tę serię rozpoczyna się od pokazujące sposób używania usługi Azure Active Directory zapewniające uwierzytelnianie użytkownika w Azure. Następnie rozwija dotyczyć złożonych scenariuszy rozszerzanie infrastruktury DODAJE Azure i przy użyciu usług ADFS dla delegowania.

- [Implementacji usługi Azure Active Directory](./guidance-identity-aad.md)
- [Rozszerzanie usługi Active Directory (DODAJE) Azure](./guidance-identity-adds-extend-domain.md)
- [Tworzenie las zasobu usługi Active Directory katalogu (DODAJE) platformy Azure](./guidance-identity-adds-resource-forest.md)
- [Wdrażanie usług federacyjnych Active Directory (ADFS) w Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Projektując aplikacji web skalowalna przy użyciu Azure PaaS

Tej serii obejmuje zalecenia dotyczące tworzenia aplikacji sieci web skalowalna i wysokiej dostępności. 

- [Aplikacja sieci web podstawowe](guidance-web-apps-basic.md)
- [Zwiększanie skalowalność w aplikacji sieci web](guidance-web-apps-scalability.md)
- [Aplikacji sieci Web o wysokiej dostępności](guidance-web-apps-multi-region.md)
