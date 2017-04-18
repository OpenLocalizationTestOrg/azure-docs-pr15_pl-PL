<properties 
    pageTitle="Architektura zabezpieczeń ułożone warstwowo ze środowiskami aplikacji usługi" 
    description="Wdrażanie Architektura zabezpieczeń ułożone warstwowo ze środowiskami usługi aplikacji." 
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
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementacji architekturę warstwy zabezpieczeń ze środowiskami aplikacji usługi

## <a name="overview"></a>Omówienie ##
 
Ponieważ środowiska usługi aplikacji udostępniać środowisku odizolowanych środowisko uruchomieniowe wdrożony w wirtualnej sieci, deweloperzy mogą tworzyć Architektura zabezpieczeń ułożone warstwowo dostarczając różne poziomy dostępu do sieci dla każdego poziomu fizycznie aplikacji.

Typowe woli jest Ukryj końców wstecz interfejsu API z ogólne dostęp do Internetu, a Zezwalaj tylko interfejsy API zwany przez aplikacje sieci web nadrzędnego.  [Sieci grup zabezpieczeń (NSGs)] [ NetworkSecurityGroups] umożliwia podsieci zawierającej środowiska usługi aplikacji ograniczenia publicznego dostępu do aplikacji interfejsu API.

Na poniższym diagramie pokazano przykład Architektura aplikacji WebAPI podstawie wdrożony w środowisku usługi aplikacji.  Trzy osobne wystąpienia aplikacji, wdrożony w trzech oddzielnych środowiska usługi aplikacji nawiązywać połączenia wewnętrznej do samej aplikacji WebAPI.

![Architektura koncepcyjny][ConceptualArchitecture] 

Zielony znak plus wskazują, że grupy zabezpieczeń sieci podsieci zawierającej "apiase" umożliwia połączeń przychodzących z aplikacji web nadrzędny jako dobrze połączeń z samej siebie.  Jednak tej samej grupy zabezpieczeń sieci jawnie powoduje zablokowanie dostępu do ogólne ruchu przychodzącego z Internetu. 

Pozostała część w tym temacie opisano kroki potrzebne do skonfigurowania grupy zabezpieczeń sieci podsieci zawierającej "apiase".

## <a name="determining-the-network-behavior"></a>Określanie zachowania sieci ##
Aby dowiedzieć się, jakie zasady zabezpieczeń sieci, musisz określić, których klienci sieci będą mogli do osiągnięcia środowisko usługi aplikacji zawierającej aplikację interfejsu API i klientów zostaną zablokowane.

Od [grupy zabezpieczeń sieci (NSGs)] [ NetworkSecurityGroups] są stosowane do podsieci i środowiska usługi aplikacji są rozmieszczane na podsieci, zasady zawarte w NSG dotyczą **wszystkich** aplikacji uruchomionych w środowisku usługi aplikacji.  Używanie architektury próbki w tym artykule, po grupy zabezpieczeń sieci jest stosowana do podsieci zawierającej "apiase", wszystkie aplikacje uruchomionych "apiase" środowisko usługi aplikacji będą one chronione przez ten sam zestaw reguł zabezpieczeń. 

- **Określić adres IP ruchu wychodzącego nadrzędny osób dzwoniących:**  Co to jest adres IP lub adresy nadrzędny osób dzwoniących?  Te adresy należy jawnie mogli uzyskiwać dostęp w NSG.  Ponieważ połączeń między środowiskami usługi aplikacji są traktowane jako połączenia "Internet", oznacza to, że adres IP ruchu wychodzącego przypisane do każdej z trzech nadrzędny środowiska usługi aplikacji musi mieć udzielony dostęp w NSG dla podsieci "apiase".   Aby uzyskać więcej informacji dotyczących planowania ruchu wychodzącego adresu IP w przypadku aplikacji działa w środowisku usługi aplikacji zobacz [Architektury sieci] [ NetworkArchitecture] artykuł Omówienie.
- **Aplikację interfejsu API wewnętrznej będą się połączenie?**  Czasami pomijany i delikatne punkt jest scenariusz, gdzie aplikacja wewnętrznej musi się połączeń.  Jeśli aplikacji interfejsu API sieci wewnętrznej w środowisku usługi aplikacji musi się połączenie, jest to również traktowane jako połączenia "Internet".  W architekturze próbki w tym celu zezwolenia na dostęp z ruchu wychodzącego adresu IP "apiase" środowisko usługi aplikacji także.

## <a name="setting-up-the-network-security-group"></a>Aby skonfigurować grupy zabezpieczeń sieci ##
Gdy zbiór wychodzące adresy IP są znane, następnym krokiem jest utworzyć grupę zabezpieczeń sieci.  Grupy zabezpieczeń sieci mogą być tworzone dla obu Menedżera zasobów opartych na wirtualnych sieci, a także klasyczny wirtualnych sieci.  W przykładach poniżej pokazano, tworzenie i konfigurowanie NSG w klasycznym wirtualnej sieci przy użyciu programu Powershell.

Architektury przykładowe środowiska znajdują się w południe centralnej nam, więc pustego NSG jest tworzona w danym regionie:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Najpierw jawne zezwolić reguła jest dodawana do infrastruktury zarządzania Azure opisanymi w artykule na [ruch przychodzący] [ InboundTraffic] dla środowiska usługi aplikacji.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Następnie dwie reguły są dodawane do zezwolić HTTP i HTTPS połączeń z pierwszej środowiska usługi aplikacji nadrzędny ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Przepłukać i powtórz dla drugiego i trzeciego nadrzędny aplikacji usługi środowisk ("fe2ase" i "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ponadto należy udzielić dostępu do ruchu wychodzącego adresu IP interfejsu API wewnętrznej środowisko usługi aplikacji, dzięki czemu można zadzwonić powrót do tego samego.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Żadne inne reguły zabezpieczeń sieci trzeba można skonfigurować, ponieważ każdy NSG ma zestaw domyślnych reguł, które blokują dostęp przychodzący z Internetu domyślnie.

Poniżej przedstawiono pełną listę reguł w grupie zabezpieczeń sieci.  Zwróć uwagę, jak ostatni reguły, która zostanie wyróżniona, blokuje ruchu przychodzącego dostęp z wszystkich osób dzwoniących inne niż te, które zostały jawnie przyznane programu access.

![Konfiguracja NSG][NSGConfiguration] 

Ostatnim krokiem jest stosowanie NSG do podsieci, która zawiera "apiase" środowisko usługi aplikacji.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Z NSG stosowane do danej podsieci Aby zadzwonić do środowiska "apiase" są dozwolone tylko tych trzech nadrzędny aplikacji usługi środowiskach i środowisko usługi aplikacji zawierający interfejs API wewnętrznej.


## <a name="additional-links-and-information"></a>Dodatkowe łącza i informacje ##
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Informacje o [grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md). 

Opis [wychodzące adresy IP] [ NetworkArchitecture] i w środowisku usługi aplikacji.

[Portów] [ InboundTraffic] używane przez środowiska usługi aplikacji.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
