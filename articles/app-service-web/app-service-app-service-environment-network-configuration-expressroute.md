<properties 
    pageTitle="Praca z rozsyłania Express szczegóły konfiguracji sieci" 
    description="Szczegóły konfiguracji sieciowej do uruchamiania aplikacji usługi środowiskach w wirtualnych sieci połączone obwodem ExpressRoute." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Szczegóły konfiguracji sieci w środowiskach aplikacji usługi z ExpressRoute 

## <a name="overview"></a>Omówienie ##
Klienci mogą się łączyć [Azure ExpressRoute] [ ExpressRoute] obwód infrastruktury wirtualnej sieci, a więc rozszerzenia sieci lokalnej Azure.  Środowisku usługi aplikacji można utworzyć w podsieci tej [wirtualnej sieci] [ virtualnetwork] infrastruktury.  Aplikacje uruchomione w środowisku usługi aplikacji można następnie ustanowić bezpiecznego połączenia z zasobami wewnętrznej dostępne tylko za pomocą połączenia ExpressRoute.  

Środowisko usługi aplikacji można utworzyć w **obu** wirtualną sieć Azure Menedżera zasobów, **lub** klasycznym wdrożeniu modelu wirtualnej sieci.  Z ostatnich zmian w czerwcu 2016 ASEs teraz również mogą być rozmieszczone w wirtualnych sieci korzystające z zakresów publiczny adres lub obszar adresów RFC1918 (to znaczy adresy prywatne). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Łączność sieciowa wymagane ##
Istnieją wymagania łączności sieci dla środowiska usługi aplikacji, która nie będzie spełniony początkowo w wirtualnej sieci połączony z ExpressRoute.  Środowiskach aplikacji usługi wymagają wszystkie z następujących czynności, aby działać prawidłowo:


-  Łączność sieciowa ruchu wychodzącego do magazynowania Azure punkty końcowe na całym świecie obu porty 80 i 443.  Ta opcja uwzględnia punkty końcowe znajduje się w tym samym regionie, jak środowisko usługi aplikacji, a także punkty końcowe miejsca do magazynowania znajduje się w **innych** regionach Azure.  Azure miejsca do magazynowania punkty końcowe rozwiązania w obszarze następujące domeny DNS: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* i *file.core.windows.net*.  
-  Łączność sieciowa ruchu wychodzącego z usługą Azure pliki na porcie 445.
-  Łączność sieciowa ruchu wychodzącego do punktów końcowych bazy danych Sql znajdującego się w tym samym regionie jako środowisko usługi aplikacji.  Punkty końcowe bazy danych SQL rozwiązania w obszarze następujące domeny: *database.windows.net*.  W tym celu otwierania dostęp do portów 1433 11000 11999 i 14000 14999.  Aby uzyskać więcej informacji, zobacz [Ten artykuł na użycia portu wersji 12 bazy danych Sql](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Łączność sieciowa ruchu wychodzącego do punktów końcowych płaszczyzny zarządzania Azure (zarówno ASM i rąk punkty końcowe).  Ta opcja uwzględnia ruchu wychodzącego łączności zarówno *management.core.windows.net* , jak i *management.azure.com*. 
-  Łączność sieciowa ruchu wychodzącego *ocsp.msocsp.com*, *mscrl.microsoft.com* i *crl.microsoft.com*.  Jest to niezbędne do obsługi funkcji SSL.
-  Konfiguracja DNS wirtualną sieć musi umożliwiać wszystkie punkty końcowe i domeny wymienionych w punktach wcześniejszych.  Jeśli te punkty końcowe nie powiedzie się, próby utworzenia środowisko usługi aplikacji zakończy się niepowodzeniem i istniejących środowisk usługi aplikacji zostaną oznaczone jako nieprawidłowe.
-  Dostęp wychodzący na porcie 53 jest wymagane na potrzeby komunikacji z serwerów DNS.
-  Jeśli istnieje niestandardowe serwera DNS po drugiej stronie bramy sieci VPN, musi być dostępne z podsieci zawierającej środowisko usługi aplikacji serwera DNS. 
-  Ścieżka ruchu wychodzącego sieciowa nie można przechodzić przez wewnętrznych firmowych serwerów proxy ani może być życie tunelowane do lokalnego.  Spowoduje to zmianę skutecznych adresów NAT wychodzącego ruchu sieciowego ze środowiska usługi aplikacji.  Zmiana adresu translatora adresów Sieciowych wychodzącego ruchu sieciowego w środowisku usługi aplikacji spowoduje błędów połączenia do wielu punkty końcowe wymienionych powyżej.  Powoduje niepomyślne tworzenia środowiska usługi aplikacji, a także wcześniej prawidłowy środowiska usługi aplikacji oznaczony jako nieprawidłowe.  
-  Ruch przychodzący dostęp do wymagane porty dla środowiska usługi aplikacji należy zezwolić zgodnie z opisem w tym [artykule][requiredports].

Wymagania DNS może być spełnione w celu zapewnienia prawidłowych infrastruktura DNS jest skonfigurowane i obsługiwane dla wirtualnej sieci.  Zmiana dowolnego powodu konfigurację DNS po utworzeniu środowisku usługi aplikacji, deweloperzy można wymusić środowisku usługi aplikacji do pobrania nowa konfiguracja DNS.  Powodujące stopniowe środowiska Uruchom ponownie komputer za pomocą ikony "Uruchom ponownie" znajduje się w górnej części karta zarządzania środowisko usługi aplikacji w [Azure portal] [ NewPortal] spowoduje, że środowisko do pobrania nowa konfiguracja DNS.

Może być spełnione wymagania dostępu do sieci przychodzącego przez skonfigurowanie [grupy zabezpieczeń sieci] [ NetworkSecurityGroups] podsieci środowisko usługi aplikacji, aby umożliwić wymagane uprawnienia dostępu, zgodnie z opisem w tym [artykule][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Włączanie połączenia sieciowego ruchu wychodzącego dla środowiska aplikacji usługi##
Domyślnie nowo utworzony elektrycznego ExpressRoute anonsuje domyślną trasę, która umożliwia ruchu wychodzącego łączność z Internetem.  W tej konfiguracji środowisku usługi aplikacji będzie można nawiązać połączenia z innymi Azure punktów końcowych.

Jednak typowych konfiguracji klienta jest definiowanie własnych trasy domyślnej (0.0.0.0/0), który wymusza wychodzący ruch internetowy, natomiast przepływ lokalnego.  Ten ruch niezmiennie podziały środowiska usługi aplikacji, ponieważ ruchu wychodzącego jest zablokowane obsługiwanych lokalnie lub translatora adresów Sieciowych czy nierozpoznanym zbiór adresy, które nie są już współpracują z różnych Azure punkty końcowe.

Rozwiązanie jest określenie jednej (lub więcej) przekierowuje zdefiniowane przez użytkownika (UDRs) na podsieci, która zawiera środowisko usługi aplikacji.  UDR Określa trasy specyficzne dla podsieci, które będą uznane zamiast trasy domyślnej.

Jeśli to możliwe zaleca się następującej konfiguracji:

- Konfiguracja ExpressRoute anonsuje 0.0.0.0/0 i domyślnie życie tuneli wszystkich ruchu wychodzącego lokalnego.
- UDR zastosowane do podsieci zawierającej środowisko usługi aplikacji określa 0.0.0.0/0 z typem następnego przeskoku internetowego (na przykład jest dostępne w dół, w tym artykule).

Połączony tej procedury powoduje, że poziomie podsieci UDR ma pierwszeństwo ExpressRoute wymuszone tunelowanie zapewnić ruchu wychodzącego dostęp do Internetu w ten sposób ze środowiska usługi aplikacji.

> [AZURE.IMPORTANT] Przekierowuje zdefiniowane w UDR, **musi** spełniać określone wystarczająco pierwszeństwo dowolnego przekierowuje ogłaszane w konfiguracji ExpressRoute.  W poniższym przykładzie użyta 0.0.0.0/0 szeroki zakres adresów i jako takie mogą potencjalnie być przypadkowo zastąpione reklam rozsyłania za pomocą bardziej szczegółowych zakresy adresów.
>
>Środowiska usługi aplikacji nie są obsługiwane w przypadku konfiguracji ExpressRoute tego **krzyżowe ogłaszanie ścieżki z publicznej peering ścieżkę dostępu do prywatnych ścieżkę peering**.  Konfiguracjach ExpressRoute publicznej zaglądanie skonfigurowane, otrzymają reklam rozsyłania od firmy Microsoft dla dużego zestawu zakresy adresów IP Azure firmy Microsoft.  Jeśli te zakresy adresów są ogłaszane krzyżowe na ścieżce peering prywatne, w wyniku jest wszelkich pakietów wychodzących z podsieci środowisko usługi aplikacji konieczności tunelowane życie w infrastrukturze sieciowej lokalnego klienta.  Ten przepływ sieć nie jest obecnie obsługiwane ze środowiskami usługi aplikacji.  Możliwym rozwiązaniem tego problemu jest zatrzymanie ścieżki reklamami krzyżowe z publicznej peering ścieżka prywatne ścieżkę peering.

Informacje na przekierowuje zdefiniowane przez użytkownika jest dostępna w to [Omówienie][UDROverview].  

Szczegółowe informacje na temat tworzenia i konfigurowania przekierowuje zdefiniowane przez użytkownika jest dostępna w tym [Przewodniku][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Przykład konfiguracji UDR dla środowiska aplikacji usługi ##

**Wymagania wstępne**

1. Instalowanie programu Powershell Azure ze [strony pobierania Azure] [ AzureDownloads] (z czerwca 2015 lub nowszy).  W obszarze "Wiersza polecenia narzędzia" znajduje się link "Zainstaluj" w obszarze "Windows Powershell", który spowoduje zainstalowanie najnowszej poleceń cmdlet programu Powershell.

2. Zaleca się, że unikatowej podsieci jest tworzona wyłącznie do użytku w środowisku usługi aplikacji.  Dzięki temu, że UDRs stosowane do danej podsieci tylko otwarty ruchu wychodzącego dla środowiska usługi aplikacji.
3. **Ważne**: nie wdrażać środowisko usługi aplikacji poczekaj, aż **po** następujące czynności konfiguracyjne są wykonywane.  Dzięki temu łączność sieciowa wychodzące są dostępne przed podjęciem próby wdrażanie środowisku usługi aplikacji.

**Krok 1: Tworzenie tabeli rozsyłania nazwanych**

Poniższy fragment tworzy tabelę rozsyłania o nazwie "DirectInternetRouteTable" w regionie Zachód nam Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Krok 2: Utworzyć jeden lub więcej trasy w tabeli routingu**

Należy dodać jeden lub więcej trasy do tabeli routingu w celu umożliwienia ruchu wychodzącego dostęp do Internetu.  

Zalecane podejście do konfigurowania wychodzącej dostęp do Internetu jest określenie trasę dla 0.0.0.0/0, jak pokazano poniżej.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Należy pamiętać tego 0.0.0.0/0 jest zakresem adresu szeroki i jako takie zostanie zastąpiona bardziej szczegółowych zakresy adresów ogłaszane przez ExpressRoute.  Ponownie powtórzyć wcześniejszego zalecenia, UDR trasa 0.0.0.0/0 powinien być używany w połączeniu z konfiguracją ExressRoute tylko oferującej także 0.0.0.0/0. 

Alternatywnie możesz pobrać pełna i zaktualizowanych listę zakresów CIDR używany przez Azure.  Plik Xml zawierający wszystkie zakresy adresów Azure IP jest dostępny w [Centrum pobierania Microsoft][DownloadCenterAddressRanges].  

Mimo że Zauważ, że te zakresy zmieniać w czasie, co wymaga okresowych ręczne aktualizacje trasy zdefiniowane przez użytkownika do synchronizowania.  Ponadto ponieważ domyślne górną granicę 100 trasy w jednym UDR, będzie konieczne "Podsumowanie" zakresy adresów Azure IP, aby dopasować limicie 100 rozsyłania pamiętając, że UDR zdefiniowane trasy powinny być bardziej szczegółowe niż trasy ogłaszane przez usługi ExpressRoute.  


**Krok 3: Kojarzenie tabeli rozsyłania podsieci zawierającej środowisko usługi aplikacji**

Ostatnim krokiem konfiguracji jest skojarzyć tabeli trasy do podsieci, miejsce, w którym zostanie wdrożony środowisko usługi aplikacji.  Następujące polecenie kojarzy "DirectInternetRouteTable" do "ASESubnet" po pewnym czasie zawierający środowisku usługi aplikacji.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Krok 4: Końcowych czynności**

Po tabeli routingu jest powiązany z podsieci, zaleca się najpierw przetestować i Potwierdź zamierzonego efektu.  Na przykład wdrażanie maszyny wirtualnej do danej podsieci i upewnić się, że:


- Ruch wychodzący do punktów końcowych Azure i innych niż Azure wspomniano wcześniej **w tym artykule jest ułożony w dół obwód ExpressRoute** .  Jest bardzo ważne zweryfikować to zachowanie, ponieważ w przypadku ruchu wychodzącego z podsieci nadal wymuszone tunelowane lokalnego, środowisko usługi aplikacji tworzenie zawsze nie powiedzie się. 
- Wyszukiwanie DNS dla punkty końcowe wspomniano wcześniej, wszystkie rozwiązuje poprawnie. 

Po potwierdzeniu są powyższe czynności, należy usunąć maszyny wirtualnej, ponieważ podsieci musi być "puste" w czasie utworzone środowisko usługi aplikacji.
 
Kontynuuj tworzenie środowisku usługi aplikacji!

## <a name="getting-started"></a>Wprowadzenie
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji][IntroToAppServiceEnvironment]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
