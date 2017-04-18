<properties 
    pageTitle="Bezpieczne połączenie z zasobami wewnętrznej bazy danych w środowisku usługi aplikacji" 
    description="Informacje na temat bezpiecznie łączyć się z zasobami wewnętrznej bazy danych w środowisku usługi aplikacji." 
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

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Bezpieczne połączenie z zasobami wewnętrznej bazy danych w środowisku usługi aplikacji #

## <a name="overview"></a>Omówienie ##
Ponieważ środowisku usługi aplikacji zawsze jest tworzony w **obu** wirtualną sieć Azure Menedżera zasobów, **lub** klasycznym wdrożeniu modelu [wirtualną sieć][virtualnetwork], połączenia wychodzące z środowisku usługi aplikacji do innych zasobów wewnętrznej bazy danych można przepływ wyłącznie przez sieć wirtualną.  Z ostatnich zmian w czerwcu 2016 ASEs mogą być rozmieszczone w wirtualnych sieci korzystające z zakresów publiczny adres lub obszar adresów RFC1918 (to znaczy adresy prywatne).  

Na przykład może być programu SQL Server w klastrze maszyn wirtualnych z portem 1433 zablokowana.  Punkt końcowy może być ACLd tylko umożliwienie dostępu z innymi zasobami w tej samej sieci wirtualnej.  

Inny przykład poufne punktów końcowych może działać w lokalnym i mieć połączenie z Azure za pomocą dowolnej z [Witryny do witryny] [ SiteToSite] lub [Azure ExpressRoute] [ ExpressRoute] połączenia.  W wyniku tylko zasoby w wirtualnych sieci połączony z witryny do witryny lub tunelami ExpressRoute będą mieli dostęp do lokalnego punktów końcowych.

Dla wszystkich tych scenariuszy aplikacje uruchomione w środowisku usługi aplikacji będą mogli zapewnić bezpieczne połączenie do różnych serwerów i zasobów.  Ruchu wychodzącego z aplikacji działa w środowisku usługi aplikacji do prywatnych punktów końcowych w tej samej sieci wirtualnych (lub podłączony do tej samej sieci wirtualnych) będzie tylko przepływ przez wirtualną sieć.  Ruch wychodzący do prywatnych punkty końcowe nie będzie przepływał publicznie w Internecie.

Jedno zastrzeżenie: dotyczy ruchu wychodzącego w środowisku usługi aplikacji do punktów końcowych w wirtualnej sieci.  Środowiska usługi aplikacji nie może się połączyć punkty końcowe maszyn wirtualnych znajduje się w tej **samej** podsieci co środowisko usługi aplikacji.  Zwykle nie należy to problem jak środowiska usługi aplikacji są rozmieszczane do podsieci zastrzeżone wyłącznie do użytku przez środowisko usługi aplikacji.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Połączenia wychodzące i wymagania systemu DNS ##
Dla środowisku aplikacji usługi poprawnego wymaga ruchu wychodzącego dostęp do różnych punktów końcowych. Pełną listę zewnętrznych punktów końcowych, używane przez ASE znajduje się w sekcji "Wymagana łączność sieciowa" artykułu [Konfiguracja sieciowa dla ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Środowiska usługi aplikacji wymaga prawidłowych infrastruktury DNS skonfigurowane dla wirtualnej sieci.  Zmiana dowolnego powodu konfigurację DNS po utworzeniu środowisku usługi aplikacji, deweloperzy można wymusić środowisku usługi aplikacji do pobrania nowa konfiguracja DNS.  Powodujące stopniowe środowiska Uruchom ponownie komputer za pomocą ikony "Uruchom ponownie" znajduje się w górnej części karta zarządzania środowisko usługi aplikacji w portalu spowoduje, że środowisko do pobrania nowa konfiguracja DNS.

Zalecane jest również, że wszelkie niestandardowe serwery DNS na vnet można zainstalować wyprzedzeniem przed utworzeniem środowisku usługi aplikacji.  Konfiguracja DNS wirtualną sieć zostanie zmieniona, podczas tworzenia środowiska usługi aplikacji, który spowoduje awarię proces tworzenia środowiska usługi aplikacji.  W podobny szyjnej Jeśli istnieje niestandardowe serwera DNS po drugiej stronie bramy VPN i serwera DNS jest nieosiągalny lub niedostępne, proces tworzenia środowiska usługi aplikacji również powiedzie się.

## <a name="connecting-to-a-sql-server"></a>Nawiązywanie połączenia z programem SQL Server
Typowe konfiguracji programu SQL Server ma punktu końcowego nasłuchują na porcie 1433:

![Punkt końcowy programu SQL Server][SqlServerEndpoint]

Istnieją dwie metody ograniczania ruchu do tego punktu końcowego:


- [Sieci listy kontroli dostępu] [ NetworkAccessControlLists] (sieciowy ACL)

- [Grupy zabezpieczeń sieci][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Ograniczanie dostępu za pomocą list ACL sieci

Port 1433 mogą być chronione za pomocą listy kontroli dostępu w sieci.  Przykład poniżej klienta whitelists adresów, pochodzących z umieszczona w wirtualnej sieci i blokuje dostęp do innych klientów.

![Przykład listy kontroli dostępu do sieci][NetworkAccessControlListExample]

Wszystkie aplikacje uruchomione w środowisku usługi aplikacji w tej samej sieci wirtualnej co program SQL Server będzie mógł połączyć się z wystąpieniem programu SQL Server przy użyciu adresu IP **VNet wewnętrznych** maszyny wirtualnej programu SQL Server.  

Parametry połączenia przykładzie poniżej odwołuje się do programu SQL Server przy użyciu adresu IP prywatne.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Chociaż maszyny wirtualnej jest także publicznej końcowy, ze względu na sieć ACL będą odrzucane próby nawiązania połączenia przy użyciu publicznego adresu IP. 

## <a name="restricting-access-with-a-network-security-group"></a>Ograniczaniu dostępu za pomocą sieci grupy zabezpieczeń
Alternatywne podejście do zabezpieczania dostępu dotyczy grupy zabezpieczeń sieci.  Grupy zabezpieczeń sieci można stosować do poszczególnych maszyn wirtualnych albo w podsieci zawierającej maszyn wirtualnych.

Najpierw należy można utworzyć grupę zabezpieczeń sieci:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Ograniczanie dostępu do tylko VNet ruch wewnętrzny jest bardzo proste z grupy zabezpieczeń sieci.  Domyślne reguły w grupie zabezpieczeń sieci tylko Zezwalaj na dostęp z innych klientów sieci w tej samej sieci wirtualnej.

W wyniku blokowanie dostępu do programu SQL Server jest tak proste, jak stosowanie grupy zabezpieczeń sieci z jego domyślne reguły do obu maszyn wirtualnych z programu SQL Server lub podsieci zawierającej maszyn wirtualnych.

Poniższe przykładowe dotyczy grupy zabezpieczeń sieci zawierającej podsieci:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Wynik to zestaw reguł zabezpieczeń, które blokują dostęp zewnętrzny, a jednocześnie VNet dostęp do:

![Domyślne reguły zabezpieczeń sieciowych][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Wprowadzenie
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji][IntroToAppServiceEnvironment]

Aby wokół sterowanie ruchu przychodzącego w środowisku usługi aplikacji zobacz [kontrolowanie ruchu przychodzącego w środowisku usługi aplikacji][ControlInboundASE]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
