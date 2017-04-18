<properties 
    pageTitle="Konfigurowanie zapory aplikacji sieci Web (WAF) w środowisku aplikacji usługi" 
    description="Dowiedz się, jak skonfigurować zaporę aplikacji sieci web przed środowiska usługi aplikacji." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurowanie zapory aplikacji sieci Web (WAF) w środowisku aplikacji usługi

## <a name="overview"></a>Omówienie ##
Web zapory aplikacji, takich jak [WAF Barracuda dla Azure](https://www.barracuda.com/programs/azure) jest dostępna w aplikacji sieci web, sprawdzając ruchu przychodzącego ruchu w sieci web do blokowania iniekcjom SQL, skryptów krzyżowych, przekazywania przed złośliwym oprogramowaniem i aplikacji DDoS i innymi atakami bezpieczny ułatwia [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) . Sprawdza również odpowiedzi od serwerów web wewnętrznej dla zapobiegania utracie danych (DLP). W połączeniu z izolacji i dodatkowe skalowania dostarczony przez środowiska usługi aplikacji, zapewnia to idealne rozwiązanie w środowisku aplikacjom hosta firm krytyczne sieci web wymagających wytrzymać złośliwych żądań i ruch dużą liczbą.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Konfiguracja ##
Dla tego dokumentu, który skonfiguruje możemy naszego środowiska usługi aplikacji za ładowania wielu strategie wystąpienia Barracuda WAF tak, aby tylko ruchu z WAF może osiągnąć środowisko usługi aplikacji i nie będą dostępne w strefy Zdemilitaryzowanej. Firma Microsoft będzie miał Menedżer ruchu Azure przed naszych wystąpienia Barracuda WAF do równoważenia obciążenia z wykorzystaniem Azure danych i regionów. Diagram wysokiego poziomu ustawień będzie wyglądać tego, co przedstawiono poniżej.

![Architektura][Architecture] 

> Uwaga: Z wprowadzeniem [ILB obsługę środowisko usługi aplikacji](app-service-environment-with-internal-load-balancer.md), można skonfigurować ASE niedostępne z strefy Zdemilitaryzowanej i udostępniane wyłącznie do sieci prywatnej. 

## <a name="configuring-your-app-service-environment"></a>Konfigurowanie środowiska aplikacji usługi ##
Aby skonfigurować środowisko usługi aplikacji, zapoznaj się z [naszą dokumentację](app-service-web-how-to-create-an-app-service-environment.md) na ten temat. Po umieszczeniu środowisku usługi aplikacji utworzony, możesz utworzyć [Aplikacji Web Apps](app-service-web-overview.md), [Aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md) i [Aplikacje Mobile](../app-service-mobile/app-service-mobile-value-prop.md) , w tym środowisku, w którym będą one wszystkie chronione za WAF możemy skonfigurować w następnej sekcji.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurowanie usługi WAF Cloud Barracuda ##
Barracuda zawiera [szczegółowe artykuł](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na temat wdrażania jej WAF na komputerze wirtualnych w Azure. Ale powodu ma nadmiarowości i wprowadza pojedynczego punktu awarii, chcesz wdrożyć co najmniej 2 maszyny wirtualne wystąpienie WAF do samej usługi w chmurze, po te instrukcje.

### <a name="adding-endpoints-to-cloud-service"></a>Dodawanie punktów końcowych do usługi w chmurze ###
Po umieszczeniu 2 lub więcej wystąpień WAF maszyn wirtualnych w usłudze w chmurze możesz dodać HTTP i HTTPS punkty końcowe, które są używane przez aplikację, jak pokazano na poniższej ilustracji przy użyciu [Azure portal](https://portal.azure.com/) .

![Konfigurowanie punktu końcowego][ConfigureEndpoint]

Użycie innych punktów końcowych, aplikacji upewnij się dodać te do tej listy, a także. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurowanie Barracuda WAF jej portalu zarządzania ###
Barracuda WAF używa TCP Port 8000 konfiguracji jego portalu zarządzania. Ponieważ mamy wielu wystąpień maszyny wirtualne WAF trzeba będzie powtórzyć opisane tu czynności dla każdego wystąpienia maszyn wirtualnych. 


> Uwaga: Po zakończeniu konfiguracji WAF, usuwanie punktu końcowego TCP/8000 usługi VMs WAF w celu zabezpieczenia usługi WAF.

Dodawanie punktu końcowego zarządzania, jak pokazano na poniższej ilustracji do skonfigurowania swojego WAF Barracuda.

![Dodawanie punktu końcowego zarządzania][AddManagementEndpoint]
 
Przejdź do punktu końcowego zarządzania w usłudze w chmurze za pomocą przeglądarki. Jeśli usługa w chmurze nosi nazwę test.cloudapp.net, czy dostęp do tego punktu końcowego, przechodząc do http://test.cloudapp.net:8000. Powinien zostać wyświetlony stronę logowania, takich jak poniżej że możesz logować się przy użyciu poświadczeń określonych w fazy instalacji WAF maszyn wirtualnych.

![Strona logowania zarządzania][ManagementLoginPage]

Raz możesz logowania powinna być widoczna pulpitu nawigacyjnego jak na poniższej ilustracji przedstawi podstawowych danych statystycznych dotyczących ochrony WAF.

![Pulpit nawigacyjny zarządzania][ManagementDashboard]

Kliknięcie na karcie usługi będzie umożliwiają skonfigurowanie usługi WAF dla usług, które chroni go. Aby uzyskać więcej informacji na temat konfigurowania usługi WAF Barracuda zwróć się [dokumentację](https://techlib.barracuda.com/waf/getstarted1). W przykładzie poniżej aplikacji sieci Web programu Azure obsługujące ruch na HTTP i HTTPS została skonfigurowana.

![Zarządzanie dodawać usług][ManagementAddServices]

> Uwaga: W zależności od sposobu skonfigurowania aplikacji i jakie funkcje są używane w środowisku usługi aplikacji, będzie konieczne przesyłania ruchu dla protokołu TCP porty 80 i 443, np. Jeśli masz konfiguracji adresów IP protokołu SSL dla aplikacji sieci Web. Wykaz portów sieci używanych w środowisku usługi aplikacji można znaleźć w [dokumentacji ruch przychodzący sterowania](app-service-app-service-environment-control-inbound-traffic.md) porty sieciowe sekcji.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurowanie Menedżera ruch platformy Microsoft Azure (OPCJONALNIE) ##
Jeśli aplikacja jest dostępna w wielu regionów, czy chcesz załadować, a następnie saldo ich za [Menedżer ruchu Azure](../traffic-manager/traffic-manager-overview.md). W tym celu możesz dodać punkt końcowy w [portal Azure klasyczny](https://manage.azure.com) przy użyciu nazwy usługi w chmurze dla usługi WAF w profilu Menedżer ruchu, jak pokazano na poniższej ilustracji. 

![Punkt końcowy Menedżer ruchu][TrafficManagerEndpoint]

Jeśli aplikacja wymaga uwierzytelniania, upewnij się, że masz jakiś zasób, który nie wymaga uwierzytelniania dla Menedżera ruch do ping dostępności aplikacji. Adres URL w sekcji Konfigurowanie można skonfigurować w [portal Azure klasyczny](https://manage.azure.com) , tak jak pokazano poniżej.

![Konfigurowanie Menedżer ruchu][ConfigureTrafficManager]

Aby przekazać polecenia ping Menedżer ruchu z Twojej WAF aplikacji, należy konfiguracji witryny sieci Web tłumaczenie na Twojej WAF Barracuda do przesyłania ruchu do aplikacji, jak pokazano w poniższym przykładzie.

![Tłumaczenie witryny sieci Web][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zabezpieczanie ruchu do środowiska usługi aplikacji przy użyciu grup zabezpieczeń sieci (NSG)##
Wykonaj [dokumentacji ruch przychodzący sterowania](app-service-app-service-environment-control-inbound-traffic.md) szczegóły Ograniczanie ruchu w środowisku usługi aplikacji od WAF tylko przy użyciu adresu VIP usługi w chmurze. Poniżej przedstawiono przykładowy polecenia programu Powershell wykonywania tego zadania dla TCP port 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Zamień SourceAddressPrefix wirtualny adres IP (VIP) z WAF usługi w chmurze.

> Uwaga: VIP usługi Cloud będą się zmieniały usuwanie i ponowne tworzenie usług w chmurze. Upewnij się zaktualizować adres IP w grupie zasobów sieci po wykonaniu tej czynności. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
