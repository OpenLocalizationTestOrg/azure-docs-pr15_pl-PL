<properties 
    pageTitle="Środowisko usługi aplikacji | Microsoft Azure" 
    description="Co to jest środowisku usługi Azure aplikacji? Wprowadzenie do środowiska usługi aplikacji." 
    keywords="Środowisko usługi Azure aplikacji, wirtualną sieć bezpiecznego sieci"
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

# <a name="app-service-environment-documentation"></a>Dokumentacja środowiska usługi aplikacji

Środowisko usługi aplikacji jest [Premium] [ PremiumTier] usługi Azure aplikacji usługa, która zapewnia w pełni odizolowanych i dedykowane środowisko bezpieczne uruchamiania aplikacji Azure aplikacji usługi w większej skali wysoki, łącznie z [Aplikacji Web Apps]w usłudze planu[WebApps], [Aplikacje Mobile][MobileApps]i [Aplikacje interfejsu API][APIApps].  

Środowiska usługi aplikacji doskonale nadają się do aplikacji obciążenia wymagające:

- Skala bardzo wysoki
- Izolacji i bezpieczny dostęp sieciowy

Klientów można utworzyć wielu środowiskach usługi aplikacji w jednym regionie Azure i wielu wielu regionów Azure.  Dzięki temu środowiska usługi aplikacji idealne rozwiązanie w przypadku poziomie skalowania warstwy aplikacji bez stanu w celu spełnienia wysoka obciążenia RPS.

Środowiska usługi aplikacji dotyczą wyłącznie aplikacje tylko jednego odbiorcy i zawsze wdrożonych w wirtualnej sieci.  Klienci mają precyzyjne sterowanie obu ruch sieciowy przychodzący i wychodzący aplikacji przy użyciu [grup zabezpieczeń sieci][NetworkSecurityGroups].  Aplikacje można również ustanowić szybkich bezpiecznego połączenia pośrednictwem wirtualnych sieci do lokalnych zasobów firmy.

Aplikacje często muszą uzyskać dostęp do zasobów firmy, takich jak wewnętrznej bazy danych i usług sieci web.  Aplikacje uruchomionych dla środowiska usługi aplikacji można uzyskać dostęp do zasobów dostępne za pośrednictwem [Witryny do witryny] [ SiteToSite] sieci VPN i [Azure ExpressRoute] [ ExpressRoute] połączenia.

* [Co to jest środowisku usługi aplikacji?](../app-service-web/app-service-app-service-environment-intro.md)
* [Tworzenie środowisku aplikacji usługi](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Tworzenie aplikacji w środowisku usługi aplikacji](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Tworzenie i używanie usługi równoważenia obciążenia wewnętrznych ze środowiskami aplikacji usługi](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurowanie środowiska aplikacji usługi](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Skalowanie aplikacji w środowisku usługi aplikacji](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Zabezpieczenia sieci i architektury](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Jak osoby

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Klipy wideo
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
