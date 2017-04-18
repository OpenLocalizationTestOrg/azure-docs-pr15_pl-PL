<properties
    pageTitle="Tworzenie i używanie wewnętrznych równoważenia obciążenia ze środowiskiem usługi aplikacji | Microsoft Azure"
    description="Tworzenie i używanie ASE z ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Za pomocą usługi równoważenia obciążenia wewnętrznych ze środowiskiem aplikacji usługi #

Funkcja Environments(ASE) usługi aplikacji jest opcja serwisowa Premium usługi aplikacji Azure, która zapewnia możliwości rozszerzona konfiguracja, które nie jest dostępna w sygnatury wielu dzierżawy.  Funkcja ASE wdraża zasadniczo usługi Azure aplikacji w Network(VNet) wirtualnych usługi Azure.  Do uzyskania lepszego zrozumienia możliwości oferowane przez środowiska usługi aplikacji przeczytać, [Co to jest środowisku usługi aplikacji] [ WhatisASE] dokumentacji.  Jeśli nie znasz zalety działających w VNet przeczytaj [Azure wirtualna sieć często zadawane pytania dotyczące][virtualnetwork].  


## <a name="overview"></a>Omówienie ##


ASE mogą być rozmieszczone z punktu końcowego dostępne internet lub z adresu IP w swojej VNet.  Aby ustawić adres IP, pod adresem VNet, należy wdrożyć usługi ASE z wewnętrznych Balancer(ILB) ładowania.  Po skonfigurowaniu usługi ASE ILB zawierają:

- własnej domeny lub poddomeny.  Aby ułatwić, ten dokument założono poddomeny, ale można skonfigurować obie te metody.  
- certyfikat używany dla HTTPS
- Zarządzanie systemem DNS dla swojej poddomeny.  


W zamian możesz można wykonywanie następujących czynności:

- Host intranet aplikacji, takich jak aplikacje biznesowe bezpiecznie w chmurze, której można uzyskać dostęp za pośrednictwem witryny do witryny lub ExpressRoute VPN
- aplikacje hosta w chmurze, które nie są widoczne na liście publicznej serwery DNS
- Tworzenie internet samodzielnie wewnętrznej bazy danych aplikacji, które aplikacje frontonu bezpieczne można zintegrować z


#### <a name="disabled-functionality"></a>Funkcje wyłączone ####

Istnieje kilka rzeczy, które nie można wykonać podczas korzystania z ASE ILB.  Te czynności obejmują:

- przy użyciu IPSSL
- Przypisywanie adresów IP do określonej aplikacji
- Kupując i przy użyciu certyfikatu z aplikacją za pośrednictwem portalu.  Oczywiście nadal można uzyskać certyfikaty bezpośrednio z urzędu certyfikacji i używać go z aplikacji, po prostu nie za pośrednictwem portalu Azure.


## <a name="creating-an-ilb-ase"></a>Tworzenie ASE ILB ##

Tworzenie ASE ILB nie jest znacznie różni się od tworzenia ASE normalnie.  Do szczegółowego dyskusji dotyczących tworzenia ASE przeczytaj, [jak utworzyć środowisku usługi aplikacji][HowtoCreateASE].  Proces, aby utworzyć ASE ILB jest taki sam między tworzenia VNet podczas tworzenia ASE lub wybranie istniejącego VNet.  Aby utworzyć ASE ILB: 

1.  W portalu Azure wybierz **Nowy -> Web + Mobile -> środowisko usługi aplikacji**
2.  Wybierz subskrypcję
3.  Wybierz lub Utwórz nową grupę zasobów
4.  Wybierz lub Utwórz VNet
5.  Tworzenie podsieci, w przypadku wybrania opcji VNet
6.  Zaznacz **wirtualną siecią-> Konfiguracja VNet** i Ustaw typ VIP na wewnętrzne
7.  Podaj nazwę poddomeny (będzie używane w przypadku aplikacji utworzonych w tym ASE poddomeny)
8.  Wybierz przycisk Ok, a następnie utwórz


![][1]


W karta wirtualnej sieci jest opcja konfiguracji VNet.  Pozwala wybrane między VIP zewnętrznych lub wewnętrznych VIP.  Wartość domyślna to zewnętrzne.  Jeśli jest ustawiona na zewnętrzne do ASE użyje VIP dostępne internet.  Jeśli wybierzesz wewnętrznego, usługi ASE zostanie skonfigurowany z ILB dla adresu IP w ramach usługi VNet.  


Po wybraniu wewnętrznego, możliwość dodawania większej liczby adresów IP do swojego ASE jest usuwana i zamiast tego należy podać poddomeny ASE.  W ASE z zewnętrznych VIP ASE zostanie użyta nazwa w poddomeny w przypadku aplikacji utworzonych w tym ASE.  
Jeśli usługi ASE był nazywany ***contosotest*** i aplikacji, w tym był nazywany ASE ***Test*** następnie poddomeny będzie mieć format ***contosotest.p.azurewebsites.net*** i adres URL, aplikacja będzie ***mytest.contosotest.p.azurewebsites.net***.  
Po ustawieniu typu VIP do wewnętrznego nazwy ASE nie jest używane w poddomeny potrzeby ASE.  Poddomena jest określ jawnie.  Jeśli do poddomeny był ***contoso.corp.net*** i wprowadzone aplikacji, w tym ASE o nazwie ***timereporting*** adres URL dla tej aplikacji może być ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>Aplikacje w ASE ILB ##

Tworzenie aplikacji w ASE ILB jest taka sama, jak zwykle tworzenia aplikacji w ASE.  

1. W portalu Azure wybierz **Nowy -> Web + Web -> telefon komórkowy** lub **Telefon komórkowy** lub **Aplikacji interfejsu API**
2. Wprowadź nazwę aplikacji
2. Wybierz subskrypcję
3. Wybierz lub Utwórz grupę zasobów
4. Wybierz lub Utwórz Plan(ASP) usługi aplikacji.  Jeśli następnie tworzenia nowego ASP Wybierz swojego ASE jako lokalizacji i wybierz roboczych ma strona ASP ma być utworzony w.  Po utworzeniu ASP możesz wybrać usługi ASE, a lokalizacji pulę pracownika.  Po określeniu nazwy aplikacji pojawi się, że poddomeny pod swoją nazwą aplikacji zastępuje poddomeny dla swojego ASE.   
5. Wybierz pozycję Utwórz.  Jeśli chcesz, aby aplikację, aby były widoczne na pulpicie nawigacyjnym, należy zaznacz pole wyboru **Przypnij do pulpitu nawigacyjnego** .  

![][2]


W obszarze Nazwa aplikacji nazwy poddomeny aktualizowany tak, aby odzwierciedlała poddomeny usługi ASE.  


## <a name="post-ilb-ase-creation-validation"></a>Sprawdzanie poprawności tworzenia ILB ASE wpisu ##

ASE ILB jest nieco inna niż non - ILB ASE.  Jak już zauważyć potrzebne do zarządzania własne DNS i musisz także podać swój własny certyfikat dla połączeń HTTPS.  


Po utworzeniu usługi ASE można zauważyć, czy poddomeny wyświetlane poddomeny określonej i w menu **Ustawienia** o nazwie **Certyfikat ILB**znajduje się nowy element.  ASE zostanie utworzony przy użyciu certyfikatu z podpisem własnym, który ułatwia testowanie HTTPS.  Portalu informujący, że należy podać swój własny certyfikat dla HTTPS, ale jest to na dysku, trzeba mieć certyfikat, który pasuje do poddomeny.  

![][3]


Jeśli są po prostu testujesz czynności i nie wiesz, jak utworzyć certyfikat, można za pomocą aplikacji konsoli MMC usług IIS Tworzenie certyfikatu z podpisem samodzielnie.  Po jego utworzeniu można wyeksportować jako plik PFX, a następnie przekazanie go w Interfejsie użytkownika ILB certyfikatu. Podczas uzyskiwania dostępu do witryny zabezpieczone przy użyciu certyfikatu z podpisem własnym, przeglądarki pozwala ostrzeżenie witryny, którą chcesz uzyskać dostęp nie jest bezpieczny z powodu braku możliwości certyfikatu.  Jeśli chcesz uniknąć tego ostrzeżenia konieczne jest prawidłowo podpisany certyfikat, który pasuje do poddomeny i ma łańcuch zaufania, który jest rozpoznawany przez przeglądarki.

![][6]

Jeśli chcesz, spróbuj przepływ z własnych certyfikatów i przetestować HTTP i HTTPS dostęp do Twojej ASE:

1.  Przejdź do pozycji ASE interfejsu użytkownika, po utworzeniu ASE **ASE -> Ustawienia -> Certyfikaty ILB**
2.  Ustaw ILB certyfikatu, wybierając plik pfx certyfikatu i hasło.  W tym kroku zajmie trochę czasu przetwarzania a wyświetlany jest komunikat, że operacji skalowania jest w toku.
3.  Uzyskać adres ILB dla swojego ASE (**ASE -> Właściwości -> wirtualny adres IP**)
4.  Tworzenie aplikacji sieci web w ASE po utworzeniu 
5.  Tworzenie maszyn wirtualnych, jeśli nie masz w tym VNET (nie w tej samej podsieci jako podział ASE lub elementy)
6.  Ustawianie DNS dla swojej poddomeny.  Możesz za pomocą symboli wieloznacznych do poddomeny w systemie DNS, lub jeśli chcesz wykonać kilka prostych testów edytować plik hosts na Twojej maszyn wirtualnych, aby ustawić adres VIP IP nazwa aplikacji sieci web.  Gdyby ASE swojej nazwy poddomeny. ilbase.com i zapewnić mytestapp aplikacji sieci web może być na mytestapp.ilbase.com następnie ustawić w pliku hosts.  (W systemie Windows plik hosts znajduje się na C:\Windows\System32\drivers\etc\)
7.  Za pomocą przeglądarki na tym maszyn wirtualnych i przejdź do pozycji http://mytestapp.ilbase.com (lub inną nazwę aplikacji sieci web jest z Twojej poddomeny)
8.  Za pomocą przeglądarki na tym maszyn wirtualnych i przejdź do https://mytestapp.ilbase.com należy zaakceptować braku zabezpieczeń, jeśli przy użyciu certyfikatu z podpisem własnym.  


Adres IP swojego ILB są widoczne w ustawieniach jako wirtualny adres IP

![][4]


## <a name="using-an-ilb-ase"></a>Za pomocą ASE ILB ##

#### <a name="network-security-groups"></a>Grupy zabezpieczeń sieci ####

ASE ILB umożliwia izolacji sieci dla aplikacji, jak aplikacje nie są dostępne lub nawet znane przez internet.  Jest to doskonały do obsługi witryn intranetowych, takich jak linią aplikacji biznesowych.  Należy ograniczyć dostęp nawet dodatkowo możesz nadal korzystać Groups(NSGs) zabezpieczeń sieci do kontrolowania dostępu na poziomie sieci. 


Jeśli chcesz dodatkowo ograniczyć dostęp za pomocą NSGs należy upewnij się, że nie podziału komunikacji, których potrzebuje ASE do działania.  Mimo że protokołu HTTP/HTTPS jest przypisany dostęp tylko za pośrednictwem ILB używane przez ASE ASE nadal zależy od zasobów poza VNet.  Aby sprawdzić, jakie dostęp do sieci jest nadal potrzebny poszukaj informacji w dokumencie na [Kontrolowanie ruch przychodzący w środowisku usługi aplikacji] [ ControlInbound] i dokumentów na [Stronie Szczegóły konfiguracji sieci w środowiskach usługi aplikacji z ExpressRoute][ExpressRoute].  


Aby skonfigurować usługi NSGs musisz znać adres IP, który jest używany przez Azure do zarządzania programu ASE.  Ten adres IP jest również adres IP ruchu wychodzącego z Twojej ASE, jeśli jest to żądania internetowe.  Aby znaleźć ten adres IP adres przejdź do pozycji **Ustawienia -> właściwości** i Znajdź **Adres IP ruchu wychodzącego**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Zarządzanie ILB ASE ogólne ####

Zarządzanie ASE ILB głównie jest taka sama, jak zwykle Zarządzanie ASE.  Musisz rozbudowy programu pul pracownika do obsługi więcej wystąpień ASP i rozbudowy serwerach frontonu do obsługi zwiększenia ilości ruch protokołu HTTP/HTTPS.  Aby uzyskać ogólne informacje na temat zarządzania konfiguracją ASE czytanie dokumentu na [Konfigurowanie środowiska usługi aplikacji][ASEConfig].  


Elementy dodatkowe zarządzania to Zarządzanie certyfikatem i zarządzania systemem DNS.  Musisz uzyskać i przekazywanie certyfikatu na potrzeby HTTPS po utworzeniu ILB ASE i zastąpić ją przed jej wygaśnięciem.  Ponieważ Azure właścicielem domeny podstawowej udostępniamy odpowiednie certyfikaty dotyczące ASEs z zewnętrznych VIP.  Ponieważ poddomeny używane przez ASE ILB może być cokolwiek, należy podać swój własny certyfikat dla HTTPS. 


#### <a name="dns-configuration"></a>Konfiguracji systemu DNS ####

Podczas korzystania z zewnętrznego VIP DNS zarządza Azure.  Dowolnej aplikacji utworzony w swojej ASE zostaną automatycznie dodane do DNS Azure, czyli publicznej DNS.  W ASE ILB trzeba zarządzać własną DNS.  Dla danego poddomeny, takiej jak contoso.corp.net potrzebne do utworzenia DNS A rekordami, które wskazują na Twój adres ILB:

    * 
    publikowanie *.SCM ftp 


## <a name="getting-started"></a>Wprowadzenie
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji][WhatisASE]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
