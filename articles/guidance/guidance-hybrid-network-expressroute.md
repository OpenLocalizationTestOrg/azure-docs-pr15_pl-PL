<properties
   pageTitle="Implementacji architektury sieci hybrydowego z Azure ExpressRoute | Dokumentacja dotycząca architektura | Microsoft Azure"
   description="Jak wdrażać architektura bezpiecznej sieci witryny do witryny, wyświetlanego Azure wirtualnej sieci i sieci lokalnej połączone za pomocą Azure ExpressRoute."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Implementacji architektury sieci hybrydowego z Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące nawiązywanie połączenia z siecią lokalnego wirtualnych sieci Azure za pomocą ExpressRoute. ExpressRoute połączenia są wykonywane przy użyciu połączenia dedykowane prywatnego przez dostawcę łączności innych firm. Połączenie prywatne jest rozciągany sieci lokalnej Azure zapewniając dostęp do własnej infrastruktury IaaS Azure, publicznej punkty końcowe używane w PaaS i usług władz akredytacji bezpieczeństwa usługi Office 365. W tym dokumencie opisano przy użyciu ExpressRoute nawiązywania połączenia z pojedynczego Azure wirtualnej sieci (VNet) za pomocą tak zwany zaglądanie prywatne.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ten plan używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowego miejsce, w którym obciążenia są rozdzielone sieci lokalnej i Azure.

- Aplikacje dużych, krytyczne obciążenie pracą, które wymagają wysokiego stopnia skalowalność.

- Na dużą skalę urządzenia i przywracania kopii zapasowych danych, które musi być zapisany w trybie offline.

- Obsługa obciążenia Big Data.

- Używanie Azure witryn odzyskiwania.

> [AZURE.NOTE] [Omówienie kwestii technicznych ExpressRoute] [ expressroute-technical-overview] wprowadzenie do ExpressRoute.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnia ważne składniki w tej architektury:

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na stronie "Hybrydowych sieć — modelu encja-RELACJA".

![[0]][0]

- **Azure wirtualnych sieci (VNets).** Każdy VNet znajduje się w jednym regionie Azure i może zawierać wiele poziomów aplikacji. Warstwy aplikacji części przy użyciu podsieci w każdej VNet i/lub sieci grup zabezpieczeń (NSGs). 

- **Azure usług publicznych.** Są to usługi Azure, których może być wykorzystana aplikacji hybrydowych. Te usługi są również dostępne publicznie w Internecie, ale uzyskiwanie do nich dostępu za pośrednictwem obwód ExpressRoute zawiera krótki czas oczekiwania i przewidywalne wydajności, ponieważ ruch nie przechodzą przez Internet. Połączenia są wykonywane przy użyciu **publicznej zaglądanie**, adresy, które są własnością organizacji lub dostarczonym przez dostawcę usługi łączności. 

- **Usługi Office 365.** Są publicznie dostępne aplikacji usługi Office 365 i usług firmy Microsoft. Połączenia są wykonywane przy użyciu **programu Microsoft zaglądanie**, adresy, które są własnością organizacji lub dostarczonym przez dostawcę usługi łączności.

>[AZURE.NOTE] W przypadku nawiązywania połączenia bezpośrednio do programu Microsoft CRM Online za pośrednictwem zaglądanie firmy Microsoft.

- **Lokalne sieci firmowej.** Jest to sieć komputerów i urządzeń połączonych za pośrednictwem sieci lokalnej prywatne działa w obrębie organizacji.

- **Routery lokalne krawędzi.** Są to router nawiązać elektrycznego zarządzane przez dostawcę sieci lokalnej. W zależności od sposobu zainicjowano obsługę administracyjną połączenie należy podać publiczne adresy IP używane przez routery. 

- **Microsoft edge routery.** Są dwa routery w konfiguracji wysokiej dostępności aktywny aktywny. Te routery umożliwiają dostawcy łączności połączyć ich obwodów bezpośrednio do ich centrum danych. W zależności od sposobu zainicjowano obsługę administracyjną połączenie należy podać publiczne adresy IP używane przez routery.

- **Obwód ExpressRoute.** Jest to warstwy 2 lub obwód warstwy 3 podanego przez dostawcę usługi łączności sieci sprzężenia w lokalnej z Azure za pośrednictwem routerów krawędzi. Obwód używa infrastruktury sprzętu zarządzane przez dostawcę łączności.

- **Dostawcy łączności.** Są to firmy, które zapewniają połączenie, albo za pomocą warstwy 2 lub 3 warstwy łączność między centrum danych i Azure centrum danych.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołania można śledzić tych zaleceń, chyba że masz określonych wymóg, aby zalecenie nie mieści się.

### <a name="connectivity-providers"></a>Łączność dostawców

Wybierz odpowiedni dostawca łączności ExpressRoute dla Twojej lokalizacji. Aby uzyskać listę dostawców łączności dostępne na miejscu, użyj następującego polecenia programu PowerShell Azure:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute łączności dostawców połączyć z centrum danych do firmy Microsoft w następujący sposób:

- **Współtworzenie znajdujące się na exchange chmury.** Jeśli znajdujesz Współtworzenie w miejscu z serwerem exchange w chmurze, można uporządkować połączeń między wirtualnych w chmurze firmy Microsoft za pośrednictwem dostawcy Współtworzenie lokalizacji Ethernet exchange. Współtworzenie lokalizacji dostawców mogą oferować warstwy 2 między połączenia lub zarządzanych Layer 3 między połączenia między infrastruktury w funkcji współtworzenia lokalizacji i w chmurze firmy Microsoft.

- **Typu punkt-punkt połączenia Ethernet.** Umożliwia nawiązanie połączenia lokalnego centrach danych i biurami w chmurze firmy Microsoft przy użyciu łączy Ethernet typu punkt-punkt. Dostawców Ethernet typu punkt-punkt mogą oferować połączenia Layer 2 lub zarządzanych Layer 3 połączenia między witryny i w chmurze firmy Microsoft.

- **Dowolna z każdym sieci (IPVPN).** Usługi WAN można zintegrować z chmury firmy Microsoft. IPVPN dostawców (zazwyczaj MPLS VPN) oferuje dowolna z każdym łączność między oddziałów i centrach danych. Chmury firmy Microsoft mogą połączona do swojego WAN, aby wyglądał tak samo jak inne oddziału. WAN dostawców oferuje zwykle zarządzanych łączności Layer 3.

Aby uzyskać więcej informacji na temat dostawców łączności, zobacz [ExpressRoute obwody elektryczne i układy domen routingu][connectivity-providers].

### <a name="expressroute-circuit"></a>Obwód ExpressRoute

Upewnij się, że organizacji spełnia [wymagania wstępne ExpressRoute] [ expressroute-prereqs] do łączenia się z Azure.

Jeśli jeszcze tego nie zrobiono, Dodaj podsieć o nazwie `GatewaySubnet` do VNet usługi Azure i Tworzenie bramy wirtualnej sieci ExpressRoute przy użyciu usługi Azure VPN bramy. Aby uzyskać więcej informacji o tym procesie, zobacz [ExpressRoute przepływów pracy na potrzeby obsługi administracyjnej elektrycznego i Stany obwodów][ExpressRoute-provisioning].

Utworzenie obwód ExpressRoute w następujący sposób:

1. Uruchom następujące polecenia programu PowerShell:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Wysyłanie `ServiceKey` dla nowego obwodu usługodawcy.

3. Poczekaj, aż dostawcy do zapewniania obsługi obwodu. Można sprawdzić stan obsługi administracyjnej obwodu przy użyciu następującego polecenia programu PowerShell:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

`Provisioning state` w `Service Provider` sekcji wynik zmieni się z `NotProvisioned` do `Provisioned` kiedy obwodu jest gotowy.

>[AZURE.NOTE]Jeśli używasz połączenia layer 3 dostawcę należy skonfigurować i zarządzanie nimi routing Podaj informacje niezbędne w celu umożliwienia dostawcę w celu zaimplementowania odpowiednie trasy.

Jeśli korzystasz z połączenia warstwy 2, wykonaj następujące czynności:

1. Rezerwowanie dwa/30 podsieci składający się z prawidłowych publicznych adresów IP dla każdego typu zaglądanie chcesz zaimplementować. Te/30 podsieci będzie używana do dostarczania adresów IP dla routerów dla układu. W przypadku wdrażania prywatnych, publicznych i zaglądanie firmy Microsoft, musisz podsieci 6 /30 z prawidłowych publicznych adresów IP.     

2. Konfigurowanie routingu dla obwodu ExpressRoute. Musisz uruchomić polecenia poniżej dla każdego typu zaglądanie chcesz skonfigurować (prywatnych, publicznych i Microsoft).

    >[AZURE.NOTE]Zobacz [Tworzenie i modyfikowanie routing obwód ExpressRoute] [ configure-expressroute-routing] Aby uzyskać szczegółowe informacje. Aby dodać sieci zaglądanie kierowania ruchu sieciowego, należy użyć następujące polecenia programu PowerShell:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Rezerwowanie kolejnej puli prawidłowych publicznych adresów IP do użycia w przypadku translatora adresów Sieciowych dla publicznej i zaglądanie firmy Microsoft. Zalecane jest różnych pula dla każdego zaglądanie. Określ puli do dostawcy łączności, więc można skonfigurować anonsy BGP dla tych zakresów.

[Łącze do prywatnych VNet(s) w chmurze do elektrycznego ExpressRoute][link-vnet-to-expressroute]. Należy użyć następujących poleceń programu PowerShell:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Zwróć uwagę następujące punkty:

- Wymiany informacji dotyczących routingu między siecią a Azure ExpressRoute używa obramowania Gateway Protocol (BGP).

- Można połączyć wiele VNets znajduje się w różnych regionów, aby ten sam obwód ExpressRoute, dopóki wszystkie VNets i obwód ExpressRoute znajdują się w tym samym regionie geopolitycznych.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Obwody ExpressRoute wprowadź ścieżkę szerokopasmowych między sieciami. Zazwyczaj wyższa przepustowości większy koszt. Mimo że niektórych dostawców umożliwiają zmienianie przepustowość, upewnij się, że wybierz początkowy przepustowość przewyższają potrzeb oraz zapewnia miejsce dla wzrostu. W przypadku, gdy chcesz zwiększyć przepustowość w przyszłości, pozostają z dwoma opcjami.

Zwiększanie przepustowości. Pamiętaj, że nie wszystkie dostawców pozwalają na tym dynamiczne. A należy unikać konieczności takim jak największym stopniu. Ale w razie potrzeby po sprawdzeniu u dostawcy, uruchom poniższe polecenia.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Można zwiększyć przepustowość bez utraty łączności. Obniżanie wersji przepustowość spowoduje zakłócenia w łączności. Musisz usunąć obwodu i utwórz go ponownie z nowej konfiguracji.

Jeśli korzystasz z wersji Standard dla ExpressRoute i konieczności uaktualnienia do Premium lub zmienić korzystasz z ceny plan (taryfowej lub nieograniczoną), uruchomienie poleceń poniżej. `Sku.Tier` Właściwość może być `Standard` lub `Premium`; `Sku.Name` właściwość może być `MeteredData` lub `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Upewnij się, `Sku.Name` dopasowania właściwości `Sku.Tier` i `Sku.Family`. Jeśli zmienisz rodzinie i warstwy, ale nie nazwy, połączenie jest wyłączona.

Można też uaktualnić SKU bez przerywania, ale nie można przełączać planowi nieograniczoną cennik taryfowe. Obniżanie wersji SKU, do wykorzystania przepustowości musi pozostawać domyślny limit wersji standard.

> [AZURE.NOTE] ExpressRoute udostępnia dwa plany cennik klientom, na podstawie pomiaru lub nieograniczoną danych. Zobacz [ceny ExpressRoute] [ expressroute-pricing] Aby uzyskać szczegółowe informacje. Opłaty zależą od przepustowości elektrycznego. Przepustowość prawdopodobnie zależą od dostawcy dostawcy. Używanie `Get-AzureRmExpressRouteServiceProvider` polecenia cmdlet, aby wyświetlić dostawców dostępne w danym regionie i przepustowości, które oferują.

Pojedynczy obwód ExpressRoute mogą obsługiwać wiele peerings i VNet łącza. Zobacz [limity ExpressRoute] [ expressroute-limits] uzyskać więcej informacji.

W przypadku dodatkowej opłaty Dodatek Premium ExpressRoute udostępnia:

- Do 10 000 przekierowuje na elektrycznego. Bez dodatku Premium ExpressRoute limit wynosi 4000 przekierowuje na elektrycznego.

- Połączenie globalne włączanie obwód ExpressRoute zlokalizowanych w dowolnym miejscu na świecie dostępu do zasobów w dowolnej region, a nie tylko regiony, w tym samym kontynent.

- Zwiększenie liczby łączy VNet na obwód od 10 do większych limit, w zależności od przepustowości obwodu.

Obwody ExpressRoute pozwalają seria tymczasową sieć maksymalnie dwa razy przepustowości limit zamówionych dla bez dodatkowych opłat. W tym celu należy za pomocą łączy zbędne. Jednak nie wszystkie dostawców łączności obsługują tę funkcję; Upewnij się, że dostawcy łączności włącza tę funkcję przed w zależności od go.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Można skonfigurować wysoką dostępność dla połączenia Azure na różne sposoby, w zależności od typu dostawcy, którego używasz oraz liczby ExpressRoute obwody elektryczne i układy połączenia wirtualnej sieci bramy, które chcesz skonfigurować. Poniższe punkty podsumowanie opcje dostępności:

- ExpressRoute nie obsługuje protokoły nadmiarowości router takich jak HSRP i VRRP implementowania wysokiej dostępności. Zamiast tego używa zbędne para sesji BGP na zaglądanie. W celu ułatwienia wysokiej dostępności połączenia z siecią, Microsoft Azure przepisy możesz z dwoma portami zbędne routerami (część Microsoft edge) w konfiguracji aktywny aktywny.

- Jeśli używasz połączenia warstwy 2 wdrożyć nadmiarowych routerów w sieci lokalnej w konfiguracji aktywny aktywny. Połącz obiegu podstawowego jeden router i pomocniczą elektrycznego do drugiego. Zapewni to połączenie wysokiej dostępności na obu końcach połączenia. Jest to konieczne, jeśli jest wymagane ExpressRoute SLA. Zobacz [Umowa dotycząca poziomu usług dla Azure ExpressRoute] [ sla-for-expressroute] Aby uzyskać szczegółowe informacje.

Poniższej diagramu w konfiguracji z routerami zbędne lokalnego połączony z głównego i pomocniczego obwodów. Każdy obwód obsługuje ruch zaglądanie publiczne i prywatne zaglądanie (każdego zaglądanie jest oznaczony para /30 adres spacje, zgodnie z opisem w poprzedniej sekcji).

![[1]][1]

Jeśli korzystasz z połączenia layer 3, sprawdź, że zawiera zbędne BGP sesje, które obsługują dostępność dla Ciebie.

Wirtualnych sieci można połączyć wiele obwodów ExpressRoute i każdego obwody mogą być dostarczane przez dostawców usług różnych. Ta strategia zapewnia dodatkowe wysokiej dostępności i awarii możliwości odzyskiwania.

Konfigurowanie sieci VPN witryny do witryny jako ścieżkę pracy awaryjnej dla ExpressRoute. Dotyczy tylko zaglądanie prywatne. Dla usługi Azure i usługi Office 365 Internet jest ścieżka tylko pracy awaryjnej.

Sesje BGP są domyślnie używane wartość limitu czasu bezczynności 60 sekund. Po sesji Przekroczono limit 3 razy, trasę jest oznaczony jako niedostępne, a cały ruch jest przekierowywana do pozostałych routera. Ten limit czasu drugiego 180 może być zbyt długi kluczowych aplikacji. W takim przypadku można zmienić ustawienia limitu czasu BGP routera lokalnego mniejszą wartość.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Możesz użyć [Narzędzi łączności Azure (AzureCT)] [ azurect] monitorowanie łączność między Azure i lokalnych centrum danych usługi.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Opcje zabezpieczeń można skonfigurować dla połączenia Azure na różne sposoby, w zależności od potrzeb zgodności i zagadnienia dotyczące zabezpieczeń usługi. Poniższe punkty podsumowanie opcji zabezpieczeń programu.

ExpressRoute działa w warstwie 3. Zagrożenia w warstwie aplikacji można zapobiec za pomocą urządzenia zabezpieczeń sieci, który ogranicza ruch wiarygodnych zasobów. Ponadto połączeń ExpressRoute przy użyciu publicznej zaglądanie może zostać zainicjowana tylko z lokalnego. Dzięki temu usługi rozpoczęcie uzyskiwania dostępu do i utraty danych lokalnych z publicznego Internetu.

Aby zapewnić maksymalne zabezpieczenia, Dodaj sieci zabezpieczeń urządzeń między sieci lokalnej i routery krawędzi dostawcy. Dzięki temu można uniemożliwić przychodu nieautoryzowanego ruchu VNet:

![[2]][2]

Do celów inspekcji lub zgodności, może być konieczne uniemożliwić bezpośredni dostęp składniki działające w VNet z Internetem i implementowanie [wymuszone tunneling][forced-tuneling]. W tej sytuacji ruch internetowy powinien być przekierowany wstecz przez serwer proxy z lokalnego miejsce, w którym mogą podlegać inspekcji. Blokowanie ruchu nieautoryzowanym ułożony się i filtrowania potencjalnie złośliwym ruchu przychodzącego można skonfigurować serwer proxy.

![[3]][3]

Domyślnie maszyny wirtualne Azure uwidaczniają punkty końcowe używane umożliwiające zalogowanie się na potrzeby zarządzania - RDP i zdalne programu Powershell dla systemu Windows maszyny wirtualne i SSH dla systemem Linux maszyny wirtualne po wdrożeniu za pośrednictwem portalu Azure. Maksymalne zwiększenie bezpieczeństwa, nie Włącz publiczny adres IP dla usługi maszyny wirtualne i upewnij się, że te maszyny wirtualne nie są publicznie dostępne za pomocą NSGs. Maszyny wirtualne ma być dostępna jedynie przy użyciu wewnętrzny adres IP. Te adresy mogą być dostępne za pośrednictwem sieci ExpressRoute, włączanie lokalnego personelu DevOps przeprowadzić wszystkie niezbędne konfiguracji i konserwacji.

Jeśli punkty końcowe zarządzania musi aktywować dla maszyny wirtualne do sieci zewnętrznej, użyj NSGs i/lub uzyskać dostęp do listy kontroli, aby ograniczyć widoczność tych portów do listy sprawdzonej adresów IP lub sieci.

## <a name="troubleshooting-considerations"></a>Zagadnienia dotyczące rozwiązywania problemów

Jeśli wcześniej działa elektrycznego ExpressRoute teraz nie może połączyć, w przypadku braku dowolnej konfiguracji zmiany lokalnego lub sieci prywatnej VNet może być konieczne skontaktuj się z dostawcą łączności i pracować z nimi, aby rozwiązać ten problem. Za pomocą następujących poleceń programu PowerShell Azure do sprawdzania niektórych ograniczone i pomagają w określeniu, gdzie mogą znajdować się problemów:

- Sprawdź, czy zainicjowano obwodu:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Wynik tego polecenia pokazano kilka właściwości usługi scalonego, łącznie z `ProvisioningState`, `CircuitProvisioningState`, i `ServiceProviderProvisioningState` tak jak pokazano poniżej.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Jeśli `ProvisioningState` nie jest ustawiona na `Succeeded` po próbujesz utworzyć nowy obwód usunąć obwodu przy użyciu polecenia poniżej i spróbuj ponownie go utworzyć.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Jeśli dostawca była już obsługi administracyjnej obwód i `ProvisioningState` jest ustawiona na `Failed`, lub `CircuitProvisioningState` nie jest `Enabled`, skontaktuj się z dostawcą w celu uzyskania pomocy.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Jeśli masz istniejącej infrastruktury lokalnego już skonfigurowane z urządzenia odpowiedniej sieci, można wdrożyć architektura odwołania, wykonując następujące czynności:

Jeśli masz już skonfigurowane z urządzenia VPN istniejącej infrastruktury lokalnego, można wdrożyć architektura odwołania, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Czekaj na łącze, aby otworzyć w portalu Azure, a następnie wykonaj następujące czynności: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-hybrid-er-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Czekaj na łącze, aby otworzyć w portalu Azure, a następnie wykonaj następujące czynności: 
  - Wybierz pozycję **Użyj istniejącego** w sekcji **Grupa zasobów** , a następnie wprowadź `ra-hybrid-er-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

6. Poczekaj, aż wdrożenia do wykonania.

## <a name="next-steps"></a>Następne kroki

- Zobacz [implementacji architektury sieci hybrydowych wysokiej dostępności] [ highly-available-network-architecture] uzyskać informacji na temat zwiększenie dostępności sieci hybrydowych na podstawie ExpressRoute awarii połączenie VPN.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybrydowe architektury sieci przy użyciu Azure ExpressRoute"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Używanie nadmiarowych routerów z ExpressRoute obwodów głównego i pomocniczego"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Dodawanie zabezpieczeń urządzeń do sieci lokalnej"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Za pomocą wymuszone tunneling zostać objęte inspekcją ruchu powiązanych z Internetu"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Znajdowanie klucza ServiceKey obwodu ExpressRoute"
