<properties
   pageTitle="Implementacji architektury sieci bezpiecznego hybrydowych platformy Azure | Microsoft Azure"
   description="Jak zaimplementować architektury sieci bezpiecznego hybrydowych w Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
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

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Implementacji strefy Zdemilitaryzowanej między Azure i lokalnych centrum danych

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące realizacji sieci bezpiecznego hybrydowych obejmującego sieci lokalnej Azure. Ta architektura odwołanie zawiera strefy Zdemilitaryzowanej między sieci lokalnej i Azure wirtualnych sieci przy użyciu przekierowuje zdefiniowane przez użytkownika (UDRs). Strefy Zdemilitaryzowanej zawiera urządzenia wirtualna sieć wysokiej dostępności (NVAs), które zaimplementowania funkcji zabezpieczeń, takich jak zapory i inspekcję pakietów. Cały ruch wychodzący z VNet jest tunelowany życie z Internetem za pośrednictwem sieci lokalnej, mogą podlegać inspekcji. 

Ta architektura wymaga połączenia z centrum danych lokalnych zaimplementować za pomocą [bramy VPN][ra-vpn], lub [ExpressRoute] [ ra-expressroute] połączenia.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowych miejsce, w którym obciążenia Uruchom częściowo lokalnego i częściowo w Azure.

- Infrastruktura Azure kieruje ruch przychodzący lokalnego i z Internetu.

- Aplikacje wymagana kontrola ruchu wychodzącego. To jest często wymagane przepisami wielu systemów komercyjnego i może pomóc w zapobieganiu publicznej informacje prywatne.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnia ważne składniki w tej architektury:

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten schemat jest na stronie "Prywatne — strefy Zdemilitaryzowanej".

[! [0]][0]

- **Sieci lokalnej.** Jest to sieć komputerów i urządzeń połączonych za pomocą prywatnej sieci lokalnej zaimplementowana w organizacji.

- **Azure wirtualnej sieci (VNet).** VNet obsługuje aplikacji i inne zasoby uruchomione w chmurze.

- **Brama.** Brama zapewnia łączność między routerami w sieci lokalnej i VNet.

- **Wirtualna sieć urządzenia (NVA).** NVA to ogólny termin opisujący maszyny wykonywania zadań, takich jak ma zezwolenie lub odmowa dostępu jako zapora, optymalizowanie operacji WAN (w tym kompresji sieci), niestandardowe routingu lub inne funkcje sieci. 

- **Warstwa, warstwa firm i podsieci Warstwa danych.** Są to podsieci hostingu maszyny wirtualne i usług, które zaimplementować przykładem zastosowania poziomu 3 działa w chmurze. Zobacz [Uruchomione maszyny wirtualne systemu Windows dla architektury N warstwowych Azure] [ ra-n-tier] uzyskać więcej informacji.

- **Trasy zdefiniowane przez użytkownika (UDR).** [Trasy zdefiniowane przez użytkownika] [ udr-overview] zdefiniowanie przepływu ruchu IP w Azure VNets. 

> [AZURE.NOTE] W zależności od potrzeb połączenie VPN można skonfigurować przekierowuje obramowania Gateway Protocol (BGP) zamiast wykonania reguły przesyłania dalej, które skierowania ruchu za pomocą UDRs tyłu w obrębie sieci lokalnej.

- **Podsieć zarządzania.** Danej podsieci zawiera maszyny wirtualne, które Implementowanie zarządzania i możliwości monitorowania dla składniki działające w VNet. 

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołania, należy wykonać tych zaleceń, jeśli nie masz określonych wymóg, aby zalecenie nie mieści się na.

### <a name="rbac-recommendations"></a>Zalecenia RBAC

Utwórz kilka ról RBAC do zarządzania zasobami w aplikacji. Rozważ utworzenie DevOps [rolę niestandardową] [ rbac-custom-roles] z uprawnieniami do zarządzania infrastrukturą aplikacji. Rozważ utworzenie scentralizowane administrator IT [niestandardową rolę] [ rbac-custom-roles] zarządzanie zasobami sieci i oddzielnych zabezpieczeń administrator IT [niestandardową rolę] [ rbac-custom-roles] do zarządzania zasobami bezpiecznej sieci, takich jak NVAs. 

Rola DevOps powinna zawierać uprawnienia, aby wdrożyć składniki aplikacji, a także monitor i uruchom ponownie maszyny wirtualne. Scentralizowane roli administratora obejmuje uprawnienia do monitorowania zasobów. Żadna z tych ról powinny mieć dostęp do zasobów NVA, jak to powinna być ograniczona do zabezpieczeń rolę administratora IT.

### <a name="resource-group-recommendations"></a>Zalecenia dotyczące grupy zasobów

Azure zasobów, takich jak maszyny wirtualne, VNets i urządzenia do równoważenia obciążenia można łatwo zarządzać grupując je do grup zasobów. Następnie można przypisać role powyżej do każdej grupy zasobów, aby ograniczyć dostęp.

Zaleca się utworzenie następujących czynności:

- Grupa zasobów, zawierający podsieci (z wyłączeniem maszyny wirtualne), NSGs i zasobów bram nawiązywania połączenia z siecią lokalnego. Przypisz scentralizowane roli administratora do tej grupy zasobów.

- Grupa zasobów, zawierającą maszyny wirtualne NVAs (w tym równoważenia obciążenia), pole skoku i inne maszyny wirtualne zarządzania i UDR podsieci bramy, który wymusza cały ruch przez NVAs. Przypisywanie zabezpieczeń rolę administratora IT do tej grupy zasobów.

- Oddzielnych grup zasobów dla każdej warstwy aplikacji zawierające równoważenia obciążenia i maszyny wirtualne. Należy zauważyć, że ta grupa zasobów nie powinny zawierać podsieci dla każdej warstwy. Przypisywanie roli DevOps do tej grupy zasobów.

### <a name="virtual-network-gateway-recommendations"></a>Zalecenia dotyczące Brama wirtualnej sieci

Ruch lokalnego przekazuje do VNet przez bramę wirtualnej sieci. Zalecamy, aby [Brama Azure VPN] [ guidance-vpn-gateway] lub [bramy Azure ExpressRoute][guidance-expressroute]. 

### <a name="nva-recommendations"></a>Zalecenia dotyczące NVA

NVAs dostarczają różne usługi zarządzania i monitorowanie ruchu w sieci. Azure Marketplace oferuje kilka NVAs dostawców, w tym:

- [Zapora aplikacji sieci Web barracuda] [ barracuda-waf] [Barracuda NextGen zapory] i serwera[barracuda-nf]

- [Spójne sieci VNS3 zapory i routera/sieci VPN][vns3]

- [FortiGate Fortinet-maszyn wirtualnych][fortinet]

- [Zapora aplikacji sieci Web SecureSphere][securesphere]

- [Zapora aplikacji sieci Web DenyAll][denyall]

- [Sprawdzanie vSEC punkt][checkpoint]

Jeśli żaden z tych NVAs innych firm nie spełnia wymagań, możesz utworzyć niestandardowe NVA, przy użyciu maszyny wirtualne. Przykład tworzenia niestandardowych NVAs Zobacz strefy Zdemilitaryzowanej w architekturze tego odwołania, który wykonuje następujące funkcje:

- Ruch jest przekierowywany przy użyciu [przekazywanie adresów IP] [ ip-forwarding] na nic NVA.

- Ruch jest dozwolony przekazywanie NVA tylko wtedy, gdy jest to potrzebne w tym celu. Każdy maszyn wirtualnych NVA w architekturze odwołanie jest proste router Linux z ruchu przychodzącego przychodzące do sieci interfejsu *eth0*i ruchu wychodzącego reguł dopasowania zdefiniowanych przez skrypty wysyłane za pośrednictwem interfejsu sieci *eth1*. 

- Ruch kierowane do podsieci zarządzania nie przechodzą przez NVAs i NVAs można skonfigurować tylko z podsieci zarządzania. Jeśli ruch do podsieci zarządzania jest wymagany do przekazywane za pośrednictwem NVAs, jest trasy do podsieci zarządzania, aby rozwiązać problem NVAs, jeśli powinien zakończyć się niepowodzeniem.  

- Maszyny wirtualne dla NVA są uwzględniane w dostępność ustawianie za usługi równoważenia obciążenia. UDR w podsieci bramy kieruje żądania NVA do równoważenia obciążenia.

Inny rekomendacji brać pod uwagę łączy wiele NVAs w serii z każdego NVA wykonywania zadania specjalistyczne zabezpieczeń. Dzięki temu każdej funkcji zabezpieczeń do zarządzania na zasadzie na NVA. Na przykład NVA implementacji Zapora może znajdować się w serii, z uruchomionymi usługami tożsamości NVA. Zależnościami w celu ułatwienia zarządzania jest dodanie przeskoków dodatkowe sieci może zwiększyć opóźnienie, dlatego upewnij się, że to nie ma wpływu na wydajność usługi aplikacji.

### <a name="nsg-recommendations"></a>Zalecenia NSG

Brama VPN udostępnia publiczny adres IP dla połączenia z siecią lokalnego. Zaleca się utworzenie grupy zabezpieczeń sieci (NSG) dla reguł ruchu przychodzącego NVA podsieci wykonawczego do zablokowania całego ruchu nie pochodzą z sieci lokalnej.

Zalecane jest również implementacji NSGs dla każdej podsieci zapewnienie drugiego poziomu ochrony przed ruchu przychodzącego pominięcie NVA niepoprawnie skonfigurowane lub wyłączone. Na przykład podsieci warstwa sieci web w architekturze odwołania wykonuje NSG za pomocą reguły, aby zignorować wszystkie żądania inne niż te otrzymane od sieci lokalnej (192.168.0.0/16) lub VNet i inną regułę, która ignoruje wszystkie wnioski nie na porcie 80. 

### <a name="internet-access-recommendations"></a>Zalecenia dotyczące dostępu do Internetu

[Wymuszanie tunelem] [ azure-forced-tunneling] cały ruch wychodzący internet za pośrednictwem sieci lokalnej przy użyciu witryny do witryny sieci VPN tunelem i przekazać je do Internetu, przy użyciu sieci tranlation adres (NAT). To będzie zarówno zapobiec przypadkowym wyciekom poufne informacje przechowywane w warstwie usługi danych i umożliwiają kontroli i inspekcji całego ruchu wychodzącego.

> [AZURE.NOTE] Całkowite nie blokuj ruchu internetowego z warstwy sieci web, firm i aplikacji. Jeśli te warstwy za pomocą usług Azure PaaS, które polegają na publicznych adresów IP dla rejestrowanie diagnostyczne maszyn wirtualnych, Pobierz rozszerzeń maszyn wirtualnych i inne funkcje. Diagnostyka Azure wymaga również, że składniki można odczytywać i zapisywać konto Azure magazynowania zależne od internetowej.

Dodatkowo zaleca się sprawdzenie wychodzący ruch internetowy jest tunelowany życie poprawnie. Jeśli używasz połączenie VPN z [Usługa routing i zdalny dostęp] [ routing-and-remote-access-service] na serwerze lokalnego za pomocą narzędzi, takich jak [WireShark] [ wireshark] lub [Microsoft wiadomości Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Zarządzanie podsieci zalecenia

Podsieć zarządzania zawiera pole szybkiego dostępu, które wykonuje funkcje monitorowania i zarządzanie. Zaimplementować następujące zalecenia dla pola szybkiego dostępu:
- Nie należy tworzyć publiczny adres IP w polu skok. 
- Tworzenie jednej trasy dostęp do pola skoku przez bramę przychodzące NSG w podsieci zarządzania odpowiedzieć tylko na żądania z dozwolonych trasę.
- Wykonywanie wszystkich zadań zarządzania bezpiecznego ograniczyć do pola szybkiego dostępu.

### <a name="nva-recommendations"></a>Zalecenia dotyczące NVA

Dołączanie warstwy 7 NVA przerwanie połączenia aplikacji na poziomie NVA i zachować koligację z poziomów wewnętrznej bazy danych. Gwarantuje to symetrycznej łączności zwraca odpowiedź ruchu z poziomów wewnętrznej bazy danych za pośrednictwem NVA.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Architektura odwołanie zawiera usługi równoważenia obciążenia kierowanie ruchu w sieci lokalnej do puli urządzeń NVA. Opisane wcześniej, urządzenia NVA są maszyny wirtualne wykonywanie reguł rozsyłania ruchu w sieci i są rozmieszczane do [Ustawianie dostępności][availability-set]. Ten projekt umożliwia monitorowanie przepustowość NVAs w czasie i Dodaj urządzenia NVA w odpowiedzi na zwiększenie ładowania.

Standardowy brama SKU VPN obsługuje stałej przepustowość do 100 MB/s. Duży SKU wydajności udostępnia maksymalnie 200 MB/s. Dla wyższej przepustowości Rozważ uaktualnienie do bramy ExpressRoute. ExpressRoute zapewnia przepustowość do 10 GB mniejsze opóźnienia niż połączenie VPN.

> [AZURE.NOTE] Artykuły [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ guidance-vpn-gateway] i [implementowania hybrydowych architektury sieci przy użyciu Azure ExpressRoute] [ guidance-expressroute] opisują zagadnień skalowalność Azure bramy.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Architektura odwołanie zawiera usługi równoważenia obciążenia rozpowszechniania żądania ze źródeł lokalnych do puli urządzeń NVA Azure. Urządzenia NVA są maszyny wirtualne wykonywanie reguł rozsyłania ruchu w sieci i są rozmieszczane do [Ustawianie dostępności][availability-set]. Usługi równoważenia obciążenia regularnie kwerendy sondy kondycji na każdym NVA i usunie wszelkie nie odpowiada NVAs z puli. 

Jeśli korzystasz z platformy Azure ExpressRoute zapewniające łączność między sieci VNet i lokalnych, [należy skonfigurować bramę VPN, aby zapewnić pracy awaryjnej] [ guidance-vpn-failover] Jeśli połączenie ExpressRoute staje się niedostępna.

Aby uzyskać szczegółowe informacje na utrzymanie dostępność dla połączeń sieci VPN i ExpressRoute, zobacz artykuły [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ guidance-vpn-gateway] i [implementowania hybrydowych architektury sieci przy użyciu Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Wszystkich aplikacji i monitorowanie zasobów powinny być wykonywane przez pole skok w podsieci zarządzania. W zależności od potrzeb aplikacji może być konieczne dodawać kolejne zasoby, monitorowania w podsieci zarządzania, ale ponownie dowolnego z tych dodatkowych zasobów dostępne za pośrednictwem okna szybkiego dostępu.

Jeśli połączenie bramy w Twojej sieci lokalnej Azure nie działa, można nadal osiągnąć pole skoku wdrażając PIP, dodanie go do pola skoku i zdalnych w z Internetu.

Podsieci każdego poziomu w architekturze odwołanie jest chroniony NSG reguł, a może być konieczne utworzyć regułę, aby otworzyć port 3389 dostępu RDP na maszyny wirtualne systemu Windows lub 22 dostępu SSH na maszyny wirtualne Linux. Inne narzędzia do zarządzania i monitorowania może wymagać reguły, aby otworzyć dodatkowe porty.

Jeśli korzystasz z ExpressRoute, zapewniając łączność między centrum danych lokalnych i Azure, za pomocą [Narzędzi łączności Azure (AzureCT)] [ azurect] do monitorowania i rozwiązywania problemów z połączeniem.

> [AZURE.NOTE] Można znaleźć dodatkowe informacje w szczególności mające na celu monitorowania i zarządzanie połączeniami VPN i ExpressRoute w artykułach [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ guidance-vpn-gateway] i [implementowania hybrydowych architektury sieci przy użyciu Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Ta architektura odwołanie wykonuje wiele poziomów zabezpieczeń: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Kierowanie wszystkie żądania użytkownika lokalnego za pośrednictwem NVA

UDR w podsieci bramy blokuje wszystkie żądania użytkownika inne niż te otrzymane od lokalnego. Przejścia UDR dozwolone żądania NVAs w podsieci prywatnej strefy Zdemilitaryzowanej, a te żądania są przekazywane do aplikacji jeśli mogą one zgodnie z zasadami NVA. Inne drogi można dodawać do UDR, ale upewnij się, że przypadkowo nie pomijać NVAs lub blokowanie ruchu administracyjne przeznaczonych do zarządzania podsieci.

Usługi równoważenia obciążenia przed NVAs pełnić rolę urządzenia zabezpieczeń pomijając ruch portów, które nie są otwarte w reguły równoważenia obciążenia. Urządzenia do równoważenia obciążenia w architekturze odwołanie tylko nasłuchują żądania HTTP na porcie 80 i żądań protokół HTTPS na porcie 443. Należy udokumentować wszelkie dodatkowe reguły dodane do urządzenia do równoważenia obciążenia i mieć pewność, że nie ma żadnych problemów zabezpieczeń należy monitorować dane.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Za pomocą NSGs blok/pass ruch między warstwy aplikacji

Poszczególnych poziomów sieci web, firm i danych ograniczyć ruch między nimi przy użyciu NSGs. Oznacza to, że warstwa firm używa NSG do zablokowania całego ruchu, że nie pochodzi z poziomu sieci web oraz warstwy danych użyto NSG, aby zablokować cały ruch, że nie pochodzi z poziomu firm. Jeśli masz wymaganie rozwiń węzeł reguły NSG umożliwienie szerszego dostępu do tych poziomów, należy rozważyć te wymagania na wypadek zabezpieczeń. Każdy nowy ścieżki ruchu przychodzącego przedstawia szansy sprzedaży dla uszkodzenia danych przypadkowe lub purposeful wyciek lub aplikacji.

### <a name="devops-access"></a>DevOps dostępu

Ogranicz operacje DevOps w każdej warstwie przy użyciu [RBAC] [ rbac] do zarządzania uprawnieniami. Gdy udzielanie uprawnień, użyj [zasada przyznawania jak najmniejszych uprawnień][security-principle-of-least-privilege]. Zaloguj się wszystkie operacje administracyjne i wykonać regularne kontrole, aby upewnić się, że zmiany konfiguracji zostały zaplanowane.

> [AZURE.NOTE] W przypadku szerszej informacji, przykłady i scenariusze dotyczące Zarządzanie zabezpieczeniami sieci za pomocą Azure, zobacz [usługami w chmurze firmy Microsoft i zabezpieczeń sieci][cloud-services-network-security]. Aby uzyskać szczegółowe informacje o ochronie zasobów w chmurze, zobacz [Wprowadzenie do zabezpieczeń platformy Microsoft Azure][getting-started-with-azure-security]. W przypadku dodatkowe szczegóły dotyczące adresowania bezpieczeństwo przez połączenie bramy Azure, zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ guidance-vpn-gateway] i [implementowania hybrydowych architektury sieci przy użyciu Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Wdrożenie dla architektury odwołanie, które wykonuje następujące zalecenia jest dostępna na Github. Ta architektura odwołanie zawiera wirtualnej sieci (VNet), grupa zabezpieczeń sieci (NSG), równoważenia obciążenia i dwóch wirtualnych maszyn.

Architektura informacyjna mogą być rozmieszczone albo z systemu Windows i Linux oraz maszyny wirtualne postępując zgodnie z instrukcjami poniżej: 

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Po otwarciu łącze w portalu Azure, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-private-dmz-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Pliki parametru zawierają stałe administratora nazwę użytkownika i hasło dla wszystkich maszyny wirtualne i zdecydowanie zalecane jest, aby natychmiast zmienić oba. Dla każdego Głosowa w ramach wdrożenia zaznacz go w portalu Azure, a następnie kliknij na **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** do innej witryny.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak wdrażać [strefy Zdemilitaryzowanej między Azure i w Internecie](./guidance-iaas-ra-secure-vnet-dmz.md).
- Dowiedz się, jak wdrażać [architektury sieci hybrydowych wysokiej dostępności](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Zabezpieczanie architektury sieci hybrydowego"
