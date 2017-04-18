<properties 
    pageTitle="Omówienie architektury sieci środowisk aplikacji usługi" 
    description="Omówienie architektury ofApp topologia sieci środowiska usługi." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="network-architecture-overview-of-app-service-environments"></a>Omówienie architektury sieci środowisk aplikacji usługi

## <a name="introduction"></a>Wprowadzenie ##
Środowiska usługi aplikacji są zawsze tworzone w podsieci [wirtualnej sieci] [ virtualnetwork] -aplikacje uruchomione w środowisku usługi aplikacji można komunikować się z prywatnych punkty końcowe znajdujących się na tym samym topologii wirtualnej sieci.  Ponieważ klienci mogą zablokować części infrastruktury wirtualną sieć, jest zrozumieć typy przepływów komunikacji sieci, które występują ze środowiskiem usługi aplikacji.

## <a name="general-network-flow"></a>Przepływ sieci ogólne ##
 
Gdy środowisko aplikacji (ASE) używa publicznej wirtualny adres IP (VIP) dla aplikacji, cały ruch przychodzący dotrze tej publicznej VIP.  Dotyczy to HTTP i HTTPS ruch w aplikacji, a także inne ruch FTP, zdalnego debugowania funkcjonalność i operacji zarządzania Azure.  Aby uzyskać pełną listę określonych portów (wymaganych i opcjonalnych), że są dostępne na publicznej VIP zobacz artykuł na temat [kontrolowania ruchu przychodzącego] [ controllinginboundtraffic] w środowisku usługi aplikacji. 

Środowiska usługi aplikacji obsługują także uruchomione aplikacje, które są powiązane tylko z wirtualną sieć adresu wewnętrznego, nazywane także adres ILB (równoważenia obciążenia wewnętrznego).  Na ILB enabled ASE, HTTP i HTTPS ruch w aplikacji, a także zdalnego wywołania debugowania, otrzymują na adres ILB.  W przypadku typowych konfiguracji ILB ASE ruch FTP-FTPS również zostaną dostarczone na adres ILB.  Jednak operacji zarządzania Azure nadal będzie przepływał do portów 454 i 455 publicznej VIP: ILB włączone ASE.

Na poniższym diagramie pokazano omówiono różne przepływów sieci przychodzące i wychodzące w środowisku usługi aplikacji miejsce, w którym aplikacje są powiązane z publiczny adres IP wirtualnego:

![Ogólne przepływów sieci][GeneralNetworkFlows]

Środowisko usługi aplikacji można komunikować się z różnych punkty końcowe prywatne klienta.  Na przykład aplikacji działa w środowisku usługi aplikacji można nawiązać uruchomionych IaaS maszyn wirtualnych w tym samym topologii wirtualną sieć serwery bazy danych.

>[AZURE.IMPORTANT] Spojrzenie na diagramie sieciowym, "Inne obliczenia zasobów" wdrożonych w innej podsieci ze środowiska usługi aplikacji. Wdrażanie zasobów w tej samej podsieci z ASE blokuje połączenia z ASE do tych zasobów (z wyjątkiem określonych ASE wewnątrz kierowanie). Zamiast tego Wdroż innej podsieci (w tym samym VNET). Środowisko usługi aplikacji będzie mógł połączyć. Dodatkowa konfiguracja nie jest konieczne.

Środowiskach aplikacji usługi również komunikowanie się z bazy danych Sql i miejsca do magazynowania Azure zasobów niezbędnych do zarządzania i operacyjnym środowisku usługi aplikacji.  Niektóre zasoby Sql i miejsca do magazynowania, które środowisku usługi aplikacji komunikuje się z znajdują się w tym samym regionie jako środowiska usługi aplikacji, podczas gdy inne osoby znajdują się w zdalnych regionach Azure.  W wyniku ruchu wychodzącego łączność z Internetem jest zawsze wymagane na potrzeby środowisku aplikacji usługi poprawnego. 

Ponieważ środowisku usługi aplikacji zostanie wdrożony w podsieci, grupy zabezpieczeń sieci służą do sterowania ruchu przychodzącego do podsieci.  Aby uzyskać szczegółowe informacje o sposobie kontrolowania ruchu przychodzącego w środowisku usługi aplikacji, zobacz następujący [artykuł][controllinginboundtraffic].

Aby uzyskać szczegółowe informacje na temat umożliwić łączność z Internetem ruchu wychodzącego w środowisku usługi aplikacji, zobacz następujący artykuł informacji o pracy z [Rozsyłania Express][ExpressRoute].  Dotyczy to samo podejście opisane w artykule podczas pracy z połączeniem witryny do witryny i za pomocą wymuszone tunelowania.

## <a name="outbound-network-addresses"></a>Adresy sieciowe ruchu wychodzącego ##
Po wywołań wychodzących środowisku usługi aplikacji adres IP jest zawsze skojarzone z połączeń wychodzących.  Określony adres IP, który jest używany, zależy od tego, czy punkt końcowy wywoływana znajduje się w topologii wirtualną sieć lub poza topologii wirtualną sieć.

Jeśli punkt końcowy wywoływana znajduje się **poza** topologii wirtualnej sieci, adres ruchu wychodzącego (czyli ruchu wychodzącego adres NAT), który jest używany jest publicznej VIP środowiska usługi aplikacji.  Ten adres można znaleźć w interfejsie użytkownika portalu dla środowiska usługi aplikacji w karta właściwości.
 
![Adres IP wychodzące][OutboundIPAddress]

Ten adres można również oznaczyć dla ASEs mające tylko publicznej VIP, tworzenie aplikacji w środowisku usługi aplikacji, a następnie wykonywanie *nslookup* na adres tej aplikacji. Adres IP wynikowy jest zarówno publicznej VIP, a także środowisko usługi aplikacji ruchu wychodzącego translatora adresów Sieciowych adres.

Jeśli punkt końcowy wywoływana znajduje się **wewnątrz** topologii wirtualnej sieci, adres wychodzących połączeń aplikacji będzie wewnętrzny adres IP zasobu poszczególnych obliczeń uruchamianie aplikacji.  Jednak nie jest trwała mapowanie wirtualnej sieci adresów IP do aplikacji.  Aplikacje można poruszanie się w różnych obliczeń zasobów i puli dostępne obliczeń, które zasobów w środowisku usługi aplikacji można zmienić ze względu na skalowanie operacji.

Jednak ponieważ środowisku usługi aplikacji jest zawsze umieszczane w podsieci, są się zagwarantować, że wewnętrzny adres IP zasobu obliczeń uruchamiania aplikacji zawsze będzie znajdowała się w zakresie CIDR podsieci.  W wyniku po szerokiego ACL lub grupy zabezpieczeń, sieci są używane do bezpieczny dostęp do innych punktów końcowych w wirtualnej sieci, podsieci zakresu zawierającego środowisko usługi aplikacji musi mieć przyznany dostęp.

Na poniższym diagramie przedstawiono te pojęcia szczegółowo:

![Adresy sieciowe ruchu wychodzącego][OutboundNetworkAddresses]

Na powyższym rysunku:

- Ponieważ publicznej VIP środowiska usługi aplikacji jest 192.23.1.2, który jest adres IP ruchu wychodzącego używane podczas nawiązywania połączenia do punktów końcowych "Internet".
- Zakres CIDR zawierające podsieci dla środowiska usługi aplikacji jest 10.0.1.0/26.  Inne punkty końcowe w tym samym infrastruktury wirtualną sieć zostanie wyświetlona połączeń z aplikacji jako pochodzące od gdzieś w tym zakresie adresów.

## <a name="calls-between-app-service-environments"></a>Połączenia między środowiskami aplikacji usługi ##
Bardziej złożone scenariusz może wystąpić, jeśli wdrażanie wielu środowiskach usługi aplikacji w tej samej sieci wirtualnych i nawiązywanie połączeń wychodzących z jednego środowiska usługi aplikacji do innego środowiska usługi aplikacji.  Następujące typy współzależności środowisko usługi aplikacji połączeń również są traktowane jako połączenia "Internet".

Na poniższym diagramie przedstawiono przykład warstw architektury z aplikacjami na jednego środowiska usługi aplikacji (np. aplikacje sieci web "Wierzch drzwi") wywoływanie aplikacji na drugim środowisko usługi aplikacji (np. wewnętrznej wewnętrznej interfejsu API aplikacje nie mają być dostępne w Internecie). 

![Połączenia między środowiskami aplikacji usługi][CallsBetweenAppServiceEnvironments] 

W powyższym przykładzie środowisko usługi aplikacji "ASE jeden" ma adres IP ruchu wychodzącego 192.23.1.2.  Jeśli aplikacji uruchomionych dla tego środowisko usługi aplikacji powoduje, że połączenia wychodzące do aplikacji uruchomionych dla drugiego środowisko usługi aplikacji ("ASE druga") znajduje się w tej samej sieci wirtualnej połączenia wychodzące będzie traktowana jako połączenia "Internet".  W wyniku ruch sieciowy przychodzące do drugiego środowisko usługi aplikacji zostanie wyświetlona jako pochodzące od 192.23.1.2 (to znaczy nie podsieci zakresu adresów: pierwszego środowisko usługi aplikacji).

Mimo że połączeń między różnych środowiskach usługi aplikacji są traktowane jako "Internet" połączeń, gdy obu środowisk usługi aplikacji znajdują się w tym samym regionie Azure ruch sieciowy pozostanie w sieci Azure regionalne i nie będzie przepływał fizycznie publicznie w Internecie.  W wyniku umożliwia grupy zabezpieczeń sieci podsieci drugiego środowisko usługi aplikacji Zezwalaj tylko przychodzący z pierwszej środowiska usługi aplikacji (którego ruchu wychodzącego adres IP jest 192.23.1.2), więc zapewnianie bezpiecznego komunikacji między środowiskami usługi aplikacji.

## <a name="additional-links-and-information"></a>Dodatkowe łącza i informacje ##
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Szczegółowych informacji na temat przychodzące porty używane przez środowiska usługi aplikacji i używanie grup zabezpieczeń sieci do kontrolowania ruchu przychodzącego jest dostępna [w tym miejscu][controllinginboundtraffic].

Szczegółowe informacje na temat korzystania z użytkownika zdefiniowane trasy do udzielania ruchu wychodzącego dostęp do Internetu do środowisk usługi aplikacji jest dostępna w tym [artykule][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

