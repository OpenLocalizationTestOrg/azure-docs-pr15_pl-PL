<properties 
    pageTitle="Wprowadzenie do środowiska aplikacji usługi" 
    description="Informacje na temat funkcji Środowisko usługi aplikacji, która zawiera bezpieczne sprzężone VNet, dedykowane skali uruchamiania wszystkie aplikacje." 
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

# <a name="introduction-to-app-service-environment"></a>Wprowadzenie do środowiska aplikacji usługi

## <a name="overview"></a>Omówienie ##
Środowisko usługi aplikacji jest [Premium] [ PremiumTier] usługi Azure aplikacji usługa, która zapewnia w pełni odizolowanych i dedykowane środowisko bezpieczne uruchamiania aplikacji Azure aplikacji usługi w większej skali wysoki, łącznie z [Aplikacji Web Apps]w usłudze planu[WebApps], [Aplikacje Mobile][MobileApps]i [Aplikacje interfejsu API][APIApps].  

Środowiska usługi aplikacji doskonale nadają się do aplikacji obciążenia wymagające:

- Skala bardzo wysoki
- Izolacji i bezpieczny dostęp sieciowy

Klientów można utworzyć wiele środowiska usługi aplikacji w jednym regionie Azure i wielu wielu regionów Azure.  Dzięki temu środowiska usługi aplikacji idealne rozwiązanie w przypadku poziomie skalowania warstwy aplikacji pozbawionego stan celu spełnienia wysoka obciążenia RPS.

Środowiskach aplikacji usługi dotyczą wyłącznie aplikacje tylko jednego odbiorcy i zawsze wdrożonych w wirtualnej sieci.  Klienci mają precyzyjne sterowanie obu ruch sieciowy przychodzący i wychodzący aplikacji i aplikacji można ustanawiać szybkich bezpiecznego połączenia za pośrednictwem sieci wirtualne lokalnych zasobów firmy.

Wszystkie artykuły i w jaki sposób — do użytkownika o środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Zawiera omówienie jak środowiska usługi aplikacji włączać wysoka skala i zabezpieczać dostęp do sieci, zobacz [Głębokości Dive AzureCon] [ AzureConDeepDive] w środowiskach usługi aplikacji!

Aby poszerzony na poziomie skalowania przy użyciu wielu środowiskach usługi aplikacji zobacz artykuł o tym, jak skonfigurować [aplikację geo distributed za][GeodistributedAppFootprint].

Aby zobaczyć, jak Architektura zabezpieczeń wyświetlane w Dive głębokości AzureCon został skonfigurowany, zobacz artykuł w implementacji w [warstwie Architektura zabezpieczeń](app-service-app-service-environment-layered-security.md) ze środowiskami usługi aplikacji.

Aplikacje uruchomionych dla środowiska usługi aplikacji może mieć dostępu bramkowane przez nadrzędny urządzeniach, takich jak zapory aplikacji sieci web (WAF).  W tym scenariuszu opisano, jak artykuł na temat [konfigurowania WAF dla środowiska usługi aplikacji](app-service-app-service-environment-web-application-firewall.md) . 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Zasoby dedykowane obliczeń ##
Wszystkie zasoby obliczeń w środowisku usługi aplikacji są zarezerwowane wyłącznie dla pojedynczej subskrypcji, a środowiskiem usługi aplikacji można skonfigurować do obliczeń zasobów do pięćdziesiąt (50) w trybie wyłączności do jednej aplikacji.

Środowisko usługi aplikacji składa się z puli zasobów obliczeń zewnętrzną, a także pul zasobów jednego do trzech pracownik obliczeń. 

Pula zewnętrzną zawiera zasoby obliczeń odpowiedzialnego za rozwiązanie SSL jako równoważenia obciążenia oraz automatyczne żądań aplikacji w środowisku usługi aplikacji. 

Każdy roboczych zawiera obliczeń zasoby przydzielone do [Planów usługi aplikacji][AppServicePlan], które z kolei zawierają jeden lub więcej aplikacji Azure aplikacji usługi.  Ponieważ w środowisku usługi aplikacji może być maksymalnie trzy pul innego pracownika, możesz mieć możliwość wybierz pozycję Zasoby różnych obliczeń dla każdej puli pracownika.  

Na przykład umożliwia tworzenie jednej roboczych z zasobami mniej zaawansowanych obliczeń dla planów usługi aplikacji przeznaczonych do test lub opracowywania aplikacji.  Druga (lub nawet trzecia) roboczych można użyć bardziej zaawansowanych zasobów obliczeń przeznaczonych do uruchamiania aplikacji produkcji plany usługi aplikacji.

Aby uzyskać więcej informacji na liczbę obliczeń zasobów dostępnych dla pul frontonu i Pracownik, zobacz, [jak konfigurowanie środowisku usługi aplikacji][HowToConfigureanAppServiceEnvironment].  

Szczegółowe informacje na temat dostępnych obliczyć rozmiarów zasobów obsługiwanych w środowisku usługi aplikacji, skontaktuj się [Cennik usługi aplikacji] [ AppServicePricing] strony i przejrzyj dostępne opcje dla środowiska usługi aplikacji Premium cennik warstwa.

## <a name="virtual-network-support"></a>Obsługa wirtualnej sieci ##
Środowisko usługi aplikacji można utworzyć w **obu** wirtualną sieć Azure Menedżera zasobów, **lub** klasycznym wdrożeniu modelu wirtualną sieć ([więcej informacji o wirtualnych sieci][MoreInfoOnVirtualNetworks]).  Ponieważ środowisku usługi aplikacji zawsze istnieje w wirtualnej sieci i bardziej precyzyjne w podsieci wirtualnej sieci, mogą korzystać z funkcji zabezpieczeń wirtualnych sieci do sterowania obu przychodzących i wychodzących komunikacji.  

Środowisko usługi aplikacji może być albo dostępnych z publiczny adres IP, lub wewnętrznego przeciwległych z adresem Azure równoważenia obciążenia wewnętrzny (ILB) przez Internet.

Można używać [grup zabezpieczeń sieci] [ NetworkSecurityGroups] Aby ograniczyć komunikację w sieci przychodzącego do podsieci, w którym znajduje się środowisku usługi aplikacji.  Pozwala na uruchamianie aplikacji nadrzędny urządzeń i usług, takich jak zapory aplikacji sieci web i dostawców władz akredytacji bezpieczeństwa sieci.

Aplikacje także często muszą uzyskać dostęp do zasobów firmy, takich jak wewnętrznej bazy danych i usług sieci web.  Wspólne podejście jest udostępnienie te punkty końcowe tylko do wewnętrznego ruchu sieciowego ułożony w Azure wirtualnej sieci.  Po środowisku usługi aplikacji jest dołączony do tej samej sieci wirtualnej jako wewnętrzny usługi, aplikacje działa w środowisku można uzyskiwać do nich dostęp, w tym punkty końcowe dostępne za pośrednictwem [Witryny do witryny] [ SiteToSite] i [Azure ExpressRoute] [ ExpressRoute] połączenia.

Aby uzyskać więcej informacji na temat działania środowiska usługi aplikacji z wirtualnych sieci i sieci lokalnej można znaleźć w następujących artykułach na [Architektury sieci][NetworkArchitectureOverview], [Sterowanie ruch przychodzący][ControllingInboundTraffic]i [Bezpiecznie nawiązywanie połączenia z pomocą][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Jak do tworzenia aplikacji środowisko usługi][HowToCreateAnAppServiceEnvironment]

Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

Aby uzyskać omówienie architektury sieci środowisko usługi aplikacji, zobacz [Omówienie architektury sieci] [ NetworkArchitectureOverview] artykuł.

Aby uzyskać szczegółowe informacje o środowisku usługi aplikacji za pomocą ExpressRoute, zobacz następujący artykuł na [Express i kierowanie środowiska usługi aplikacji][NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
