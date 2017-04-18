<properties
   pageTitle="Implementacji architektury sieci wysokiej dostępności hybrydowego | Microsoft Azure"
   description="Jak wdrażać architektura bezpiecznej sieci witryny do witryny, wyświetlanego Azure wirtualnej sieci i sieci lokalnej połączenia przy użyciu ExpressRoute pracy awaryjnej bramy sieci VPN."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Implementacji architektury sieci hybrydowych wysokiej dostępności

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące nawiązywanie połączenia z siecią lokalnego wirtualnych sieci Azure przy użyciu ExpressRoute, aby witryna wirtualną sieć prywatną (VPN) jako połączenie pracy awaryjnej. Ruch przepływa między sieci lokalnej i Azure wirtualnych sieci (VNet) za pośrednictwem połączenia ExpressRoute.  W przypadku utraty połączenia w układzie ExpressRoute ruch będą kierowane za pośrednictwem sieci VPN IPSec tunelem.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ten plan używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowego miejsce, w którym obciążenia są rozdzielone sieci lokalnej i Azure.

- Aplikacje dużych, krytyczne obciążenie pracą, które wymagają wysokiego stopnia skalowalność.

- Na dużą skalę urządzenia i przywracania kopii zapasowych danych, które musi być zapisany w trybie offline.

- Obsługa obciążenia Big Data.

- Używanie Azure witryn odzyskiwania.

Uwaga Jeśli obwód ExpressRoute jest niedostępna, trasę sieci VPN będzie tylko obsługi prywatne zaglądanie połączeń przez. Zaglądanie publicznych i Microsoft zaglądanie połączeń przejdzie przez Internet.

## <a name="architecture-diagram"></a>Diagram architektury

>[AZURE.NOTE] [Usługa Azure VPN bram] [ azure-vpn-gateway] wykonuje dwa typy bram wirtualnej sieci; Bram wirtualnej sieci VPN i bram wirtualnej sieci ExpressRoute. W tym dokumencie terminów, których *Brama VPN* odwołuje się do usługi Azure, podczas fraz *Brama wirtualnej sieci VPN* i *bramą wirtualnej sieci ExpressRoute* są używane do odpowiednio odwołują się do wdrożenia VPN i ExpressRoute bramy.

Poniższy diagram wyróżnia ważne składniki w tej architektury:

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się w "sieci hybrydowego - VPN encja-Relacja" strony.

![[0]][0]

- **Azure wirtualnych sieci (VNets).** Każdy VNet znajduje się w jednym regionie Azure i może zawierać wiele poziomów aplikacji. Warstwy aplikacji części przy użyciu podsieci w każdej VNet i/lub sieci grup zabezpieczeń (NSGs).

- **Lokalne sieci firmowej.** Jest to sieć komputerów i urządzeń połączonych za pośrednictwem sieci lokalnej prywatne działa w obrębie organizacji.

- **Urządzenia sieci VPN.** Jest to urządzenie lub usługa, która zapewnia zewnętrznych łączność z siecią lokalnego. Urządzenia sieci VPN może być urządzenie, lub może być oprogramowanie, takie jak Routing i zdalny dostęp Service () w systemie Windows Server 2012.

> [AZURE.NOTE] Aby uzyskać listę obsługiwanych urządzeń sieci VPN i uzyskać informacje o konfigurowaniu wybranego urządzenia VPN do łączenia się z bramą VPN Azure, zapoznaj się z instrukcjami dotyczącymi odpowiedniego urządzenia z [listy urządzeń VPN obsługiwanych przez Azure][vpn-appliance].

- **Brama wirtualnej sieci VPN.** Brama wirtualnej sieci VPN umożliwia VNet nawiązać połączenie VPN urządzenia w sieci lokalnej. Brama wirtualnej sieci VPN jest skonfigurowany do akceptowania wezwań z sieci lokalnej tylko przy użyciu urządzenia VPN. Aby uzyskać więcej informacji, zobacz [Łączenie sieci lokalnej wirtualną sieć Microsoft Azure][connect-to-an-Azure-vnet].

- **Brama wirtualnej sieci ExpressRoute.** Brama wirtualnej sieci ExpressRoute umożliwia VNet nawiązania połączenia z obwodem ExpressRoute na potrzeby łączności z sieci lokalnej.

- **Podsieć bramy.** Bram wirtualnej sieci odbywa się w tej samej podsieci.

- **Połączenie VPN.** Połączenie ma właściwości, które określają typu połączenia (IPSec) i klawisz udostępnione urządzenia VPN lokalnego do szyfrowania ruchu.

- **Obwód ExpressRoute.** Jest to warstwy 2 lub obwód warstwy 3 podanego przez dostawcę usługi łączności sieci sprzężenia w lokalnej z Azure za pośrednictwem routerów krawędzi. Obwód używa infrastruktury sprzętu zarządzane przez dostawcę łączności.

- **Aplikacja N warstwowych chmury.** Jest to aplikacja, obsługiwany w Azure. Może zawierać wiele poziomów, z wiele podsieci połączonych za pomocą urządzenia do równoważenia obciążenia Azure. Ruch w każdej podsieci mogą podlegać reguły zdefiniowane przy użyciu [Grup zabezpieczeń sieci Azure][azure-network-security-group](NSGs). Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zabezpieczeń platformy Microsoft Azure][getting-started-with-azure-security].

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołania można śledzić tych zaleceń, chyba że masz określonych wymóg, aby zalecenie nie mieści się.

### <a name="vnet-and-gatewaysubnet"></a>VNet i GatewaySubnet

Tworzenie bramy wirtualnej sieci ExpressRoute i Brama wirtualnej sieci VPN, w tym samym VNet. Oznacza to, że powinny one udostępnianie tej samej podsieci o nazwie **GatewaySubnet**

Jeśli VNet już zawiera podsieci o nazwie **GatewaySubnet**, należy zagwarantować, że ma /27 lub większą przestrzeń adresową. Jeśli istniejącą podsieć jest za mały, je usunąć w następujący sposób i utworzyć nową, jak pokazano na następny punktor:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Jeśli VNet nie zawiera podsieci o nazwie **GatewaySubnet**, następnie utwórz nową w następujący sposób:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>Sieć VPN i ExpressRoute bram

Sprawdź, czy Twoja organizacja spełnia [wymagania wstępne ExpressRoute] [ expressroute-prereq] do łączenia się z Azure.

Jeśli masz już Brama wirtualnej sieci VPN w swojej VNet Azure, usuń go, tak jak pokazano poniżej.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Postępuj zgodnie z instrukcjami [implementacji architektury sieci hybrydowego z Azure ExpressRoute] [ implementing-expressroute] nawiązanie połączenia z ExpressRoute.

Postępuj zgodnie z instrukcjami [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ implementing-vpn] ustanowić połączenie VPN wirtualną sieć bramy.

Po ustaleniu połączenia wirtualnej sieci bramy, przetestuj środowiska następujący:

1. Upewnij się, że można nawiązać połączenie z sieci lokalnej usługi VNet Azure.

2. Skontaktuj się z dostawcą, aby zatrzymać łączności ExpressRoute testowania.

3. Upewnij się, że możesz nadal łączyć z sieci lokalnej do swojego VNet Azure za pomocą bramy połączenia wirtualnej sieci VPN.

4. Skontaktuj się z dostawcą do reestabilish ExpressRoute łączności.

## <a name="considerations"></a>Zagadnienia dotyczące

Aby uzyskać informacje o ExpressRoute, zobacz [implementacji architektury sieci hybrydowego z Azure ExpressRoute] [ guidance-expressroute] wskazówki.

Aby uzyskać informacje VPN witryny do witryny, zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ guidance-vpn] wskazówki.

Aby uzyskać informacje ogólne zabezpieczeń Azure, zobacz [usług w chmurze firmy Microsoft i zabezpieczeń sieci][best-practices-security].

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Jeśli masz istniejącej infrastruktury lokalnego już skonfigurowane z urządzenia odpowiedniej sieci, można wdrożyć architektura odwołania, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Czekaj na łącze, aby otworzyć w portalu Azure, a następnie wykonaj następujące czynności: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-hybrid-vpn-er-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Czekaj na łącze, aby otworzyć w portalu usługi Azure, a następnie wprowadź, a następnie wykonaj następujące czynności: 
  - Wybierz pozycję **Użyj istniejącego** w sekcji **Grupa zasobów** , a następnie wprowadź `ra-hybrid-vpn-er-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki i postanowienia, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architektura architektury sieci hybrydowych wysokiej dostępności za pomocą bramy ExpressRoute i sieci VPN"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
