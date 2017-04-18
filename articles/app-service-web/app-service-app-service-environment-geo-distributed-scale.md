<properties 
    pageTitle="Geo Distributed skali ze środowiskami aplikacji usługi" 
    description="Informacje o poziomie skalowanie aplikacji za pomocą geo dystrybucyjnej z Menedżer ruchu i w środowisku usługi aplikacji." 
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
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geo Distributed skali ze środowiskami aplikacji usługi

## <a name="overview"></a>Omówienie ##
Scenariusze aplikacji, które wymagają bardzo wysoki skali może przekroczyć zasobów wydajności dostępne do jednej strony dotyczącej wdrażania aplikacji.  Głosowanie aplikacji, sportowych i zdarzenia programu Rozrywka są wszystkie przykłady scenariuszy wymagających bardzo wysoki Skala. Wymagania wysoka skali może zostać spełnione przez poziomie Skalowanie zewnętrzne aplikacje, z wielu wdrożeniami aplikacji do obsługi wymagania ładowania najdalej odniesienia w jednym regionie i wielu regionów.

Środowiska aplikacji usługi są idealnym platformy poziomy skali się.  Raz aplikacji środowisku usługi został wybrany konfiguracji obsługujące stawki znane żądanie, deweloperzy można wdrażać dodatkowych środowisk usługi aplikacji w czasie "plików cookie krajarki" do osiągnięcia obciążenie odpowiedniej Szczyt.

Załóżmy na przykład, że aplikacja działa w konfiguracji środowiska usługi aplikacji przetestowano do obsługi żądań 20 K na sekundę (RPS).  Jeśli obciążenie odpowiedniej Szczyt jest 100 KB RPS, następnie środowiska usługi aplikacji pięć (5) można tworzyć i skonfigurowane tak, aby upewnić się, że aplikacja może obsługiwać maksymalne obciążenie przewidywane.

Ponieważ klienci zwykle dostęp do aplikacji przy użyciu domeny niestandardowej (lub vanity), deweloperzy muszą umożliwia rozpowszechnianie aplikacji żądania we wszystkich wystąpień środowisko usługi aplikacji.  Najlepszym sposobem w tym celu jest rozwiązać domeny niestandardowej za pomocą [profilu Menedżera ruch Azure][AzureTrafficManagerProfile].  Profil Menedżera ruch można skonfigurować, aby wskazywały we wszystkich poszczególnych środowisk usługi aplikacji.  Menedżer ruchu obsługiwać automatycznie dystrybucji klientów we wszystkich środowiskach usługi aplikacji opartych na ustawienia profilu Menedżer ruchu równoważenia obciążenia.  Ta metoda sprawdza się niezależnie od tego, czy wszystkie środowiska usługi aplikacji są rozmieszczone w jednym regionie Azure lub na całym świecie wdrożony w wielu regionów Azure.

Ponadto ponieważ klientom dostęp do aplikacji w domenie vanity, klientów znają liczby środowiska usługi aplikacji uruchamianie aplikacji.  W wyniku deweloperów można szybko i łatwo dodawania i usuwania, środowiskach usługi aplikacji opartych na obciążenie sieci obserwowane.

Diagram koncepcyjny poniżej przedstawia aplikację poziomie skalowania w trzech środowiskach usługi aplikacji w jednym regionie.

![Architektura koncepcyjny][ConceptualArchitecture] 

Pozostała część w tym temacie opisano kroki związane z konfigurowania rozłożone topologii aplikacji przykładowych przy użyciu wielu środowiskach usługi aplikacji.

## <a name="planning-the-topology"></a>Planowanie topologii ##
Przed zbudowania za aplikacji rozłożone, ułatwia kilka informacji fragmenty wyprzedzeniem.

- **Domeny niestandardowe dla aplikacji:**  Co to jest nazwę domeny niestandardowej, który użytkownicy będą używać, aby uzyskać dostęp do tej aplikacji?  Przykładowe aplikacji niestandardowej nazwy domeny jest *www.scalableasedemo.com*
- **Domeny Menedżer ruchu:**  Nazwa domeny musi podczas tworzenia [profilu Menedżer ruchu Azure][AzureTrafficManagerProfile].  Ta nazwa zostanie połączona z sufiks *trafficmanager.net* zarejestrować wpis domeny, który jest zarządzane przez Menedżera ruch.  Dla aplikacji Przykładowa nazwa wybrany jest *skalowalna pokaz ase*.  W wyniku pełną nazwę domeny zarządzanego przez Menedżera ruch jest *skalowalna demo.trafficmanager.net ase*.
- **Strategię skalowania za aplikacji:**  Za aplikacji rozdzielane w wielu środowiskach usługi aplikacji w jednym regionie?  Wielu regionów?  Mieszanie i dopasowania z obu metod?  Decyzja powinna oparta na oczekiwań, z których pochodzą będzie ruchu klientów, a także jak można skalować pozostałą część aplikacji pomocniczych infrastruktura wewnętrznej.  Na przykład z aplikacją stateless 100% aplikacji może być znacznie skalowany przy użyciu kombinacji wielu środowiskach usługi aplikacji w rozbiciu na regiony Azure, pomnożone przez środowiska usługi aplikacji dla wielu regionów Azure.  Z 15 + publicznej Azure regionów dostępnych do wyboru klienci naprawdę można tworzyć za aplikacji wyraźny na całym świecie.  Dla aplikacji przykładowych używanych w tym artykule trzy środowiska usługi aplikacji zostały utworzone w jednym regionie Azure (południe centralna Niemetryczne).
- **Konwencji nazewnictwa dla środowiska usługi aplikacji:**  Każdego środowiska usługi aplikacji wymaga unikatową nazwę.  Oprócz jednej lub dwóch środowiskach usługi aplikacji jest pomocne konwencji nazewnictwa do identyfikowania każdego środowiska usługi aplikacji.  Przykładowe aplikacji użyto prostej konwencji nazewnictwa.  Nazwy tych trzech środowiskach usługi aplikacji są *fe1ase*, *fe2ase*i *fe3ase*.
- **Konwencja nazewnicza aplikacje:**  Ponieważ wielu wystąpień aplikacji zostanie wdrożony, nazwę jest wymagane dla każdego wystąpienia wdrożonej aplikacji.  Jedną mało znanego, ale bardzo wygodne funkcji środowiska usługi aplikacji jest to, że tej samej nazwy aplikacji może być używany w wielu środowiskach usługi aplikacji.  Ponieważ każdego środowiska usługi aplikacji ma sufiks domeny unikatowe, deweloperzy można ponownie użyć dokładnie samej nazwie aplikacji w środowisku każdego.  Na przykład deweloper może mieć aplikacje o nazwie w następujący sposób: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*itd.  Dla aplikacji przykładowych jednak każdego wystąpienia aplikacji dysponuje unikatową nazwę.  Nazwy wystąpień aplikacji używane są *webfrontend1*, *webfrontend2*i *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Ustawianie profilu Menedżer ruchu ##
Gdy wielu wystąpień aplikacji wdrożonych w wielu środowiskach usługi aplikacji, wystąpienia poszczególnych aplikacji można rejestrować przy użyciu Menedżera ruch.  Przykładowe aplikacji jako Menedżer ruchu profilu jest wymagany dla *skalowalna demo.trafficmanager.net ase* rozesłać klientów do dowolnej z następujących przypadkach wdrożonej aplikacji:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Wystąpienie aplikacji przykładowych wdrożone na pierwszym środowisko usługi aplikacji.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Wystąpienie aplikacji przykładowych wdrożone na drugim środowisko usługi aplikacji.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Wystąpienie aplikacji przykładowych wdrożony w trzecim środowisko usługi aplikacji.

Rejestrowanie wielu Azure aplikacji usługi punktów końcowych, wszystkie uruchomione w **tym samym** Azure region najprościej przy użyciu programu Powershell [obsługuje Menedżera ruch Menedżera zasobów Azure][ARMTrafficManager].  

Pierwszym krokiem jest utworzenie profilu Menedżer ruchu Azure.  Poniższy kod tworzenia profilu dla aplikacji przykładowe:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Zwróć uwagę, jak parametr *RelativeDnsName* ustawiono *skalowalna pokaz ase*.  To sposobu tworzenia i skojarzone z profilem Menedżer ruchu nazwy domeny *skalowalna demo.trafficmanager.net ase* .

Parametr *TrafficRoutingMethod* określa zasada, używanych Menedżer ruchu ustalenie sposobu rozciągnąć obciążenia klienta wszystkie dostępne punkty końcowe równoważenia obciążenia.  W tym przykładzie *ważone* metody została wybrana.  Spowoduje to żądania klienta są rozciągnąć wszystkich punktów końcowych zarejestrowanych aplikacji według wagi względne skojarzonych z każdym z punktów końcowych. 

Utworzone profilem każdego wystąpienia aplikacji zostanie dodane do profilu jako natywnych Azure punktu końcowego.  Kod poniżej pobiera odwołanie do każdej aplikacji sieci web frontonu, a następnie dodaje każdej aplikacji jako punkt końcowy Menedżer ruchu przez parametr *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Zwróć uwagę, jak jest jedno wywołanie *AzureTrafficManagerEndpointConfig Dodaj* dla każdego wystąpienia poszczególnych aplikacji.  Parametr *TargetResourceId* w kolejnych poleceń programu Powershell odwołuje się do jednego z trzy wystąpienia wdrożonej aplikacji.  Profil Menedżera ruch będzie rozmieszczony Załaduj wszystkie trzy punkty końcowe w profilu.

W parametrze *Grubość* wszystkie trzy punkty końcowe używać tej samej wartości (10).  Powoduje ruch Menedżer rozmieszczania zleceń klientów we wszystkich wystąpieniach aplikacji trzy równomierne. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Wskazującą aplikacji niestandardowej domeny w domenie menedżer ruchu ##
Ostatnim krokiem konieczne jest, aby wskazywały domeny niestandardowej aplikacji w domenie menedżer ruchu.  Przykładowe aplikacji oznacza to, wskazującego *www.scalableasedemo.com* *skalowalna demo.trafficmanager.net ase*.  W tym kroku należy wykonać u rejestratora domen, która zarządza domeny niestandardowej.  

Za pomocą narzędzi do zarządzania rejestratorem Twojej domeny, CNAME rekordów musi zostać utworzone, którego punkty domeny niestandardowej w domenie menedżer ruchu.  Na poniższym obrazie przedstawiono przykład tej konfiguracji CNAME wygląda tak:

![CNAME dla domeny niestandardowej][CNAMEforCustomDomain] 

Mimo że nie omówione w tym temacie, należy pamiętać, że każde wystąpienie poszczególnych aplikacji musi mieć domeny niestandardowej, także zarejestrowany go.  W przeciwnym razie jeśli żądanie ułatwia wystąpienie aplikacji, a aplikacja ma niestandardowej domeny zarejestrowane z aplikacją, żądania zakończy się niepowodzeniem.  

W tym przykładzie domeny niestandardowej jest *www.scalableasedemo.com*i każdego wystąpienia aplikacji ma domeny niestandardowej skojarzone z nim.

![Domeny niestandardowej][CustomDomain] 

Dla recap z rejestrowania domeny niestandardowej z aplikacjami usług aplikacji Azure, zobacz następujący artykuł o [rejestrowaniu domen niestandardowych][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Sprawdzanie topologii Distributed ##
W wyniku konfiguracji Menedżer ruchu i systemie DNS jest, że żądania *www.scalableasedemo.com* będzie przepływał przez następującej:

1. Przeglądarki lub urządzenia spowoduje, że wyszukiwanie DNS dla *www.scalableasedemo.com*
2. Wpis CNAME u rejestratora domen powoduje, że wyszukiwanie DNS, aby nastąpi przekierowanie do Menedżera ruch Azure.
3. Wyszukiwanie DNS przewidziano *skalowalna demo.trafficmanager.net ase* względem jednej serwerów DNS Menedżer ruchu Azure.
4. W zależności od zasad (parametr *TrafficRoutingMethod* wcześniej używany podczas tworzenia profilu Menedżer ruchu) do równoważenia obciążenia, Menedżer ruchu wybierz jedną z skonfigurowaną punkty końcowe i wrócić Kwalifikowaną tego punktu końcowego do przeglądarki lub urządzenia.
5.  Ponieważ FQDN punktu końcowego jest adres Url wystąpienia aplikacji uruchomione w środowisku usługi aplikacji, przeglądarki lub urządzenia zostanie wyświetlone pytanie, serwer DNS Azure firmy Microsoft, aby rozpoznawać nazwy FQDN adres IP. 
6. Przeglądarki lub urządzenia wyśle żądanie HTTP/S połączenie z adresem IP.  
7. Żądanie zostanie obciążony jednego wystąpienia aplikacji uruchomione na jednej ze środowiska usługi aplikacji.

Na poniższym obrazie konsoli przedstawia wyszukiwanie DNS dla domeny niestandardowej aplikacji przykładowych rozwiązania wystąpieniem aplikacji uruchomione na jednym z trzech środowiskach usługi aplikacji przykładowych (w tym przypadku drugi trzy środowiska usługi aplikacji):

![Wyszukiwanie DNS][DNSLookup] 

## <a name="additional-links-and-information"></a>Dodatkowe łącza i informacje ##
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Dokumentacja dotycząca programu Powershell [obsługuje Menedżera ruch Menedżera zasobów Azure][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
