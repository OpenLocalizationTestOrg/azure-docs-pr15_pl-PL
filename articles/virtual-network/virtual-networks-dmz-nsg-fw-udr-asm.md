<properties
   pageTitle="Przykład strefy Zdemilitaryzowanej — tworzenie strefy Zdemilitaryzowanej do ochrony sieci z zapory, UDR i NSG | Microsoft Azure"
   description="Tworzenie strefy Zdemilitaryzowanej za pomocą zapory, zdefiniowane przez użytkownika routingu (UDR) i grupy zabezpieczeń sieci (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Przykład 3 — Tworzenie strefy Zdemilitaryzowanej do ochrony sieci z zapory, UDR i NSG

[Wróć do strony zabezpieczeń obramowanie najważniejsze wskazówki][HOME]

W tym przykładzie utworzy strefy Zdemilitaryzowanej z zapory, czterech serwerów systemu windows, Routing zdefiniowane przez użytkownika, przekazywanie adresów IP i grupy zabezpieczeń sieci. Również prowadzi przez wszystkich odpowiednich polecenia służące do udostępnienia szczegółowego zrozumienia każdego kroku. Istnieje także sekcji scenariusz ruch o podanie szczegółowych krok po kroku, jak ruch przechodzą przez warstw obrony w strefy Zdemilitaryzowanej. Na koniec w odwołaniach do sekcji jest pełny kod i instrukcje tworzenia tego środowiska, aby przetestować i poeksperymentować z różnych scenariuszach. 

![Dwukierunkowa strefy Zdemilitaryzowanej z NVA, NSG i UDR][1]

## <a name="environment-setup"></a>Konfigurowanie środowiska
W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- Trzy usług w chmurze: "SecSvc001", "FrontEnd001" i "BackEnd001"
- Wirtualna sieć "CorpNetwork" z trzech podsieci: "SecNet", "FrontEnd" i "Wewnętrznej bazy danych"
- Urządzenia wirtualnej sieci, w tym przykładzie jest zapora, podłączony do podsieci SecNet
- Windows Server, reprezentująca serwer sieci web aplikacji ("IIS01")
- Serwery dwa okna, reprezentujących aplikacji ponownie zakończyć servers ("AppVM01", "AppVM02")
- Systemu Windows server, która reprezentuje serwera DNS ("DNS01")

W poniższej sekcji odwołania jest skrypt programu PowerShell, który zostanie utworzona większość środowiska opisany powyżej. Tworzenie maszyny wirtualne i wirtualnych sieci jednak wykonywane są przez przykładowy skrypt nie zostały opisane szczegółowo w tym dokumencie.

Aby utworzyć środowisko:

  1.    Zapisywanie pliku xml konfiguracji sieci zawarte w sekcji odwołania (zaktualizowany przy użyciu nazwy, lokalizację i adresów IP adresy zgodnie z danego scenariusza)
  2.    Aktualizowanie zmienne użytkownika skrypt, aby odpowiadał środowiska, które skrypt ma być uruchamiane na (subskrypcje, nazwy usług itp.)
  3.    Wykonywanie skryptów w programie PowerShell

**Uwaga**: region oznaczony w skrypt programu PowerShell musi odpowiadać oznaczony w pliku xml konfiguracji sieci.

Po pomyślnym uruchomieniem skryptu może podjąć następujące kroki skryptu po:

1.  Konfigurowanie reguły zapory, to jest objęte do sekcji poniżej: opis reguły zapory.
2.  Opcjonalnie w sekcji Materiały referencyjne są dwa skrypty konfigurowania do serwera sieci web i serwera aplikacji przy użyciu aplikacji sieci web prosty, aby umożliwić testowanie za pomocą tej konfiguracji strefy Zdemilitaryzowanej.

Gdy skrypt jest uruchamiany pomyślnie zapory reguł, należy wykonać, jest objęta zamieszczonymi w sekcji: reguły zapory.

## <a name="user-defined-routing-udr"></a>Kierowanie (UDR) zdefiniowane przez użytkownika
Domyślnie następujące przekierowuje system są definiowane następująco:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal jest zawsze prefix(es) zdefiniowane adres: VNet dla tej sieci określonych (ie zostanie on zmieniony z VNet na VNet w zależności od tego, jak każdy określonych VNet jest definiowana). Pozostała trasy system są statyczne i domyślne co powyżej.

Jak w przypadku priorytetu trasy są przetwarzane przy użyciu metody najdłuższej prefiks dopasowania (LPM), a więc specyficzny trasy w tabeli chcesz zastosować do danego docelowy adres.

W związku z tym w VNet do miejsca docelowego z powodu trasę 10.0.0.0/16 zostanie przekierowane ruchu (na przykład serwer DNS01 10.0.2.4) przeznaczonych do sieci lokalnej (10.0.0.0/16). Innymi słowy 10.0.2.4, trasę 10.0.0.0/16 jest specyficzny trasę, mimo że 10.0.0.0/8 i 0.0.0.0/0 również można zastosować, ale ponieważ są one mniejsze określonych nie wpływają na ruch. Dlatego ruchu do 10.0.2.4 mają następnego przeskoku lokalne VNet i po prostu rozsyłanie do miejsca docelowego.

Jeśli ruch jest przeznaczona do 10.1.1.1, na przykład, trasę 10.0.0.0/16 nie zastosować, ale 10.0.0.0/8 będzie najbardziej szczegółowych i ruch będzie inicjały ("czarny holed"), ponieważ następnego przeskoku ma wartość Null. 

Jeśli miejsce docelowe nie dotyczą prefiksy Null lub prefiksy VNETLocal, a następnie należy wykonać najmniej szczegółowej rozsyłanie 0.0.0.0/0 i kierowane z Internetem jako następnego przeskoku, a więc się Azure przez internet krawędzi.

Jeśli w tabeli routingu znajdują się dwie identyczne prefiksy, Oto kolejność preferencji na podstawie atrybutu "źródłowy" przekierowuje:

1.  "VirtualAppliance" = trasę zdefiniowany przez użytkownika ręcznie dodane do tabeli
2.  "VPNGateway" = trasę dynamiczne (BGP użycia z sieci hybrydowe), dodane przez dynamiczne protokół, te trasy może się zmienić w czasie jako protokół dynamicznej automatycznie odzwierciedla zmiany w sieci peered
3.  "Domyślne" = przekierowuje systemu, lokalne VNet i statyczne pozycje, jak pokazano w powyższej tabeli rozsyłania.

>[AZURE.NOTE] Teraz można użytkownika zdefiniowane routingu (UDR) z ExpressRoute i bram VPN wymusić lokalna krzyżowe ruchu wychodzącego i przychodzącego do sieci urządzenia wirtualnych (NVA).

#### <a name="creating-the-local-routes"></a>Tworzenie trasy lokalne

W tym przykładzie dwóch tabel routingu są potrzebne, jedną dla podsieci Frontend i wewnętrznej bazy danych. Każda tabela jest załadowana trasy statyczne odpowiednie dla danej podsieci. W celu w tym przykładzie każda tabela zawiera trzy przekierowuje:

1. Ruch podsieci lokalnej przy następnym przeskoków definiowane w celu zezwalanie na ruch podsieci lokalnej pominąć zapory
2. Ruch wirtualnej sieci z przeskoków następnego zdefiniowany zapory, zastępuje domyślna reguła, która umożliwia lokalne ruchu VNet rozsyłanie bezpośrednio
3. Cały ruch pozostała (0-0) z przeskoków następnego zdefiniowany zapory

Po utworzeniu tabeli routingu są one powiązane z ich podsieci. Dla podsieci Frontend tabeli routingu, raz utworzone i powiązane z podsieci powinna wyglądać następująco:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Na przykład następujące polecenia są używane do spisie rozsyłania, dodać trasę zdefiniowane przez użytkownika, a następnie powiązać tabelę routingu podsieci (Uwaga; wszystkie elementy poniżej rozpoczynający się od znak dolara (np.: $BESubnet) są zmienne zdefiniowane przez użytkownika z skrypt w sekcji odwołania tego dokumentu):

1.  Najpierw należy utworzyć podstawowej tabeli routingu. Ten fragment przedstawia tworzenie podsieci wewnętrznej bazy danych w tabeli. W obszarze skrypt dla podsieci Frontend również jest tworzona odpowiedniej tabeli.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Po utworzeniu tabeli routingu można dodawać trasy określonych zdefiniowanych przez użytkownika. W tym wstawkę cały ruch (0.0.0.0/0) będą kierowane do wirtualnego urządzenia (zmienna $VMIP [0] jest używany w celu przekazania w polu adres IP przypisany podczas wirtualnego urządzenia został utworzony we wcześniejszej części skrypt). W obszarze skrypt odpowiednia reguła również zostanie utworzona w tabeli serwera sieci Web.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Powyższy wpis rozsyłania zastąpi "0.0.0.0/0" trasy domyślnej, ale nadal istniejącą regułę 10.0.0.0/16 domyślnej umożliwiające ruch w VNet, aby skierować bezpośrednio do miejsca docelowego, a nie na urządzeniu wirtualnej sieci. Aby poprawne należy dodać to zachowanie reguły Obserwuj.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. W tym miejscu jest wybór należy. Z powyższych na dwa sposoby cały ruch będzie trasy do zapory dla oceny, nawet ruchu w ramach jednej podsieci. Może to konieczne, jednak aby umożliwić ruchu w celu rozsyłania lokalnie bez udziału z zapory innego podsieci, można dodać bardzo określonej reguły. Tą drogą stanów, które dowolnego adresu destine dla lokalnej podsieci można tylko rozsyłanie bezpośrednio (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Na koniec z tabeli routingu utworzona i wypełniona przekierowuje zdefiniowane przez użytkownika, tabeli musi być obecnie powiązane z podsieci. W obszarze skrypt fronton rozsyłania jest również powiązana tabela podsieci serwera sieci Web. Oto skrypt powiązanie podsieci wewnętrznej.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Przesyłanie dalej adresów IP
Funkcja companion do UDR, jest przekazywanie adresów IP. To ustawienie na urządzeniu wirtualnych, dzięki któremu mogą go odbierać dane nie zostały zaadresowane do urządzenia i przesyłanie dalej ruchu do miejsca docelowego ultimate.

Na przykład jeśli ruchu z AppVM01 żąda na serwerze DNS01 UDR chcesz rozesłać to zapory. Z adresów IP przekazywanie włączone ruch w docelowej DNS01 (10.0.2.4) zostaną zaakceptowane przez urządzenia (10.0.0.4) i następnie przekazywana do miejsca docelowego ultimate (10.0.2.4). Bez adresów IP przekazywanie włączone zapory ruch czy nie zostanie zaakceptowana przez urządzenia mimo że tabelę routingu jest Zapora jako następnego przeskoku. 

>[AZURE.IMPORTANT] Należy pamiętać włączyć przekazywanie adresów IP w połączeniu z routingu zdefiniowane przez użytkownika.

Aby skonfigurować przekazywanie adresów IP jest jednego polecenia i może odbywać się w czasie tworzenia maszyn wirtualnych. Dla przepływu w tym przykładzie wstawkę kodu jest pod koniec skrypt i zgrupowane z polecenia UDR:

1.  Połączenie wystąpienia maszyn wirtualnych, które w tym przypadku jest urządzenia wirtualnego zapory, a także włączyć przekazywanie adresów IP (Uwaga; dowolny element na czerwony rozpoczynający się od znak dolara (np.: $VMName[0]) jest zmienną zdefiniowane przez użytkownika ze skryptu zamieszczone w tej sekcji tego dokumentu. Zero w nawiasach kwadratowych [0] reprezentuje pierwszy maszyn wirtualnych w tablicy maszyny wirtualne dla przykładowy skrypt do pracy bez zmian, pierwszy maszyn wirtualnych (maszyn wirtualnych 0) musi być zapory):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Grupy zabezpieczeń sieci (NSG)
W tym przykładzie grupy NSG jest wbudowany i następnie załadować za pomocą jednej reguły. Ta grupa jest następnie powiązany tylko do serwera sieci Web i wewnętrznej bazy danych podsieci (nie SecNet). Deklaratywnie jest tworzone następującą regułę:

1.  Odmowa każdy ruch (wszystkie porty) z Internetu do całego VNet (wszystkie podsieci)

Mimo że NSGs są używane w tym przykładzie, jego głównym celem jest jako warstwę pomocniczej obrony przed konfiguracji ręcznej. Chcemy blokowania wszystkich przychodzącego ruchu z Internetu do jednego serwera sieci Web lub podsieci wewnętrznej bazy danych, ruch tylko należy przepływał przez podsieci SecNet zapory (i przy odpowiednio do serwera sieci Web lub wewnętrznej bazy danych podsieci). Ponadto z regułami UDR w miejscu, wszelkie dane, że do serwera sieci Web lub wewnętrznej bazy danych podsieci będzie przekierowywany się do zapory (dzięki UDR). Zapora może to widzieć jako asymetrycznym przepływu i czy upuszczać ruchu wychodzącego. A więc są trzy warstwy zabezpieczeń Ochrona podsieci serwera sieci Web i wewnętrznej bazy danych 1) otwórz punktów końcowych na FrontEnd001 i BackEnd001 usług w chmurze, (2) NSGs odmawianie ruchu z Internetu, 3) zapory upuszczanie asymetrycznym dane.

Jeden punkt interesujące dotyczące grupy zabezpieczeń sieci, w tym przykładzie jest, że zawiera ona tylko jedna reguła, jak pokazano poniżej, która jest odmówić do całego wirtualnej sieci, które zawierałoby podsieci zabezpieczeń ruchu internetowego. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Jednak ponieważ NSG tylko jest powiązany z serwera sieci Web i wewnętrznej bazy danych podsieci, reguła nie jest przetwarzana ruchu przychodzącego podsieci zabezpieczeń. W wyniku mimo że reguły NSG mówi nie ruch internetowy do dowolnego adresu na VNet, ponieważ NSG nigdy nie była związana podsieci zabezpieczeń ruch będzie przepływał do podsieci zabezpieczeń.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Reguły zapory
Na zaporę regułach przesyłania dalej muszą zostać utworzone. Ponieważ Zapora blokuje lub przekazywanie wszystkich przychodzący, wychodzący i wewnątrz VNet ruch jest wiele reguł zapory są wymagane. Ponadto cały ruch przychodzący będzie odwołań Usługa zabezpieczeń publiczny adres IP (na różnych portów), mają być przetwarzane przez zaporę. Najlepszym rozwiązaniem jest diagramu przepływów logiczne przed rozpoczęciem konfigurowania podsieci i reguły zapory, aby uniknąć przeanalizować później. Poniższa ilustracja jest widok logiczny reguły zapory w tym przykładzie:
 
![Widok logiczny reguły zapory][2]

>[AZURE.NOTE] W zależności od urządzenia wirtualna sieć używane, różnią się porty zarządzania. W tym przykładzie, który odwołuje się do zapory NextGen Barracuda, który używa porty 22, 801 i 807. Zapoznaj się z dokumentacją producenta urządzenia, aby znaleźć odpowiednie porty używane do zarządzania urządzenia używanego.

### <a name="logical-rule-description"></a>Opis reguły logicznych.
Na powyższym diagramie logicznych podsieci zabezpieczeń nie jest widoczne, ponieważ Zapora jest tylko zasobów tej podsieci i ten schemat jest widoczna reguły zapory i sposobu ich logicznie zezwolić lub odmówić przepływów ruchu i nie tylko routingu ścieżkę. Ponadto zewnętrzne porty zaznaczonego dla ruchu RDP mają wyższą w granicach porty (8014 — 8026) i wybrano nieco wyrównać ostatnie dwa oktety lokalny adres IP łatwiejsze czytelność tekstu (np. adres serwera lokalnego 10.0.1.4 jest skojarzone z portu zewnętrznego 8014), jednak można używać żadnych wyższą-powodujące konflikt portów.

W tym przykładzie jest potrzebna 7 typy reguł, te typy reguł opisano w następujący sposób:

- Reguły zewnętrzne (w przypadku ruchu przychodzącego):
  1.    Reguła zarządzania: Ta reguła Przekieruj aplikacji pozwala na przekazywanie ruchu do porty zarządzania urządzenia wirtualnej sieci.
  2.    Reguły RDP (dla każdego serwera systemu windows): te cztery reguły (po jednej dla każdego serwera), aby umożliwić zarządzanie poszczególnych serwerach za pośrednictwem RDP. Może to być również połączone w jedną regułę w zależności od możliwości urządzenia wirtualna sieć używana.
  3.    Reguły ruchu aplikacji: Istnieją dwie reguły ruch aplikacji, pierwszego ruchu frontonu sieci web, a drugi dla ruchu wewnętrznej (np serwera sieci web do warstwy danych). Konfiguracja tych zasad zostaną zależą od architektury sieci (serwery rozmieszczenie) i ruchu przepływów (kierunku przepływu ruchu i portów, do których są używane).
      - Pierwszą regułę, aby umożliwić ruch rzeczywisty aplikacji nawiązania połączenia z serwerem aplikacji. Inne zasady pozwalają na zabezpieczenia, zarządzanie, itd., reguły aplikacji są co zezwolić użytkownikom zewnętrznym lub usługi uzyskać dostęp do aplikacji. W tym przykładzie jest serwer jednej sieci web na porcie 80, więc pojedynczy reguła aplikacji przekieruje ruchu przychodzącego do zewnętrznych adresów IP na sieci web serwerów wewnętrznych adres IP. Sesja przekierowanie ruchu czy translatora adresów Sieciowych uwagę do wewnętrznego serwera.
      - Drugą regułę ruchu aplikacji jest regułę wewnętrznej, aby umożliwić serwer sieci Web porozmawiać z serwera AppVM01 (ale nie AppVM02) za pomocą dowolnego portu.
- Wewnętrzne zasady (dla ruchu wewnątrz VNet)
  4.    Wychodzące do reguła internetowej: Ta reguła będzie zezwalanie na ruch z dowolnej sieci w celu przekazania do wybranych sieciach. Ta reguła jest zazwyczaj reguły domyślne już na zapory, ale wyłączony. Ta reguła powinna być włączona w tym przykładzie.
  5.    Reguła DNS: Ta reguła umożliwia tylko DNS (port 53) na przekazywanie ruchu do serwera DNS. Dla tego środowiska, który większości ruchu z serwera sieci Web do wewnętrznej bazy danych jest zablokowany ta reguła specjalnie umożliwia DNS z dowolnym podsieci lokalnej.
  6.    Reguła podsieci do sieci: Ta reguła jest umożliwienie dowolnego serwera podsieci wewnętrznej do połączenia z serwerem na podsieci fronton (ale nie odwrotnie).
- Reguła awaryjnych (dla ruchu, który nie spełnia powyższych):
  7.    Odmówić wszystkie reguły ruchu: To powinny mieć ostateczne reguły (pod względem priorytet) i jako takie Jeśli ruch będzie przepływał nie będzie pasować do żadnej z powyższych zasad, które zostaną usunięte przez tę regułę. Jest to reguła domyślne i zazwyczaj jest aktywna, nie są zazwyczaj potrzebne modyfikacje.

>[AZURE.TIP] Na drugiej reguły ruch aplikacji dowolnego portu jest dozwolone dla łatwe w tym przykładzie w scenariuszu rzeczywistą specyficzny port i zakresy adresów powinny być używane do zmniejszenie powierzchni atakiem tej reguły.

<br />

>[AZURE.IMPORTANT] Po utworzeniu wszystkich powyższych reguł, należy przejrzeć priorytet każdego reguły w celu zapewnienia ruch będzie dozwolony lub odmowa stosownie do potrzeb. W tym przykładzie reguły są w priorytetu. Jest łatwe do zablokowania wylogowywanie się z zapory z powodu nieprawidłowo numerowana reguły. Co najmniej upewnij się, że zarządzania samą zaporę jest zawsze bezwzględne najwyższe reguły Priority (priorytet).

### <a name="rule-prerequisites"></a>Wymagania wstępne dotyczące reguły
Jeden wymagania wstępne dotyczące maszyny wirtualnej uruchamianie zapory są publiczne punktów końcowych. Dla zapory przetwarzania ruch odpowiednie punkty końcowe publicznej musi być otwarty. Istnieją trzy typy ruchu w tym przykładzie; Ruch RDP 1) zarządzania ruchu do sterowania zapory i reguły zapory, 2) do kontrolowania serwerów systemu windows i ruch aplikacji 3). Są trzy kolumny typów ruchu w górnym połowa widok logiczny reguły zapory powyżej.

>[AZURE.IMPORTANT] W tym miejscu klucza takeway jest należy pamiętać, że **cały** ruch rozpocznie się za pośrednictwem zapory. Tak do pulpitu zdalnego na serwerze IIS01 mimo to w usłudze Cloud końcu przedniego i podsieci fronton dostępu do tego serwera możemy będzie konieczne RDP zapory na porcie 8014, a następnie pozwól zaporę, aby skierować żądanie RDP wewnętrznie do portu RDP IIS01. Przycisk "Połącz" Azure portal nie będą działać, ponieważ istnieje nie bezpośrednią ścieżkę RDP IIS01 (o ile portalu mogą widzieć). Oznacza to, że wszystkie połączenia z Internetem będzie usługa zabezpieczeń i portu, np. secscv001.cloudapp.net:xxxx (odwołanie powyżej diagramu mapowanie portu zewnętrzne i wewnętrzne adresu IP i portu).

Punkt końcowy można otworzyć w momencie tworzenia maszyn wirtualnych lub opublikować kompilacji w przykładowy skrypt i pokazano poniżej w poniższym przykładzie (Uwaga; dowolny element począwszy od znak dolara (np.: $VMName[$i]) jest zmienną zdefiniowane przez użytkownika ze skryptu zamieszczone w tej sekcji tego dokumentu. "$I" w nawiasach kwadratowych [$i], oznacza numer tablicy określonej maszyn wirtualnych w tablicy maszyny wirtualne):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Mimo że nie wyraźnie przedstawione tutaj z powodu stosowania zmiennych, ale punkty końcowe są **tylko** otwarte w usłudze w chmurze zabezpieczeń. To jest, aby upewnić się, że cały ruch przychodzący jest obsługiwane (kierowane, translatora adresów Sieciowych usunięte) przez zaporę.

Klient zarządzania będzie musi być zainstalowany na komputerze PC do zarządzania zapory i tworzenia konfiguracji potrzebne. Zobacz dostawcy dokumentacji z zapory (lub innych NVA) do zarządzania urządzeniem. Do końca w tej sekcji i następnej sekcji Tworzenie reguły zapory, opisując konfiguracja zapory, za pośrednictwem klienta zarządzania dostawców (to znaczy nie Azure portal lub programu PowerShell).

Instrukcje dotyczące pobierania klienta i nawiązywanie połączeń z Barracuda, w tym przykładzie użyto można znaleźć tutaj: [Barracuda NG administratora](https://techlib.barracuda.com/NG61/NGAdmin)

Po zalogowaniu na zapory, ale przed utworzeniem reguł zapory, istnieją dwie klasy obiektu wstępne mogących powodować, tworząc reguły łatwiejsze. Obiekty usługi i sieci.

W tym przykładzie trzy obiekty nazwany sieci powinny być zdefiniowane (jeden podsieci serwera sieci Web i podsieci wewnętrznej bazy danych, a także obiektu typu sieć dotyczące adresu IP serwera DNS). Aby utworzyć nazwany sieci; rozpoczynając od pulpitu nawigacyjnego klienta Barracuda NG administratora, przejdź na kartę Konfiguracja, w sekcji Konfiguracja operacyjne kliknij zestaw reguł, kliknij przycisk "Sieci" w menu zapory obiektów, a następnie kliknij przycisk Nowy w menu Edycja sieci. Obiekt sieci teraz można utworzyć, dodając nazwę i prefiks:

![Tworzenie obiektu w sieci serwera sieci Web][3]
 
Spowoduje to utworzenie nazwanych sieć podsieci serwera sieci Web, czy można utworzyć podobny obiektu dla danej podsieci wewnętrznej bazy danych. Teraz podsieci można łatwiej odwoływać się według nazwy w regułach zapory.

Obiektu serwera DNS:

![Tworzenie obiektu serwera DNS][4]

Pojedynczy odwołanie adres IP zostaną wykorzystane w regule DNS w dalszej części dokumentu.

Druga wstępne obiekty są obiektami usług. Te będzie reprezentował porty połączenia RDP dla każdego serwera. Ponieważ istniejącego obiektu usługi RDP jest powiązana domyślny port RDP, 3389, nowych usług można tworzyć zezwalanie na ruch z portów zewnętrznych (8014 8026). Nowe porty może również dodany do istniejącej usługi RDP, ale w celu ułatwienia pokaz, można utworzyć regułę dla każdego serwera. Aby utworzyć nową regułę RDP dla serwera; rozpoczynając od pulpitu nawigacyjnego klienta Barracuda NG administratora, przejdź na kartę Konfiguracja, w sekcji Konfiguracja operacyjne kliknij zestaw reguł, a następnie kliknij pozycję "Usługi" w menu obiektów zapory, przewiń w dół na liście usług i wybierz usługę "RDP". Kliknij prawym przyciskiem myszy i wybierz polecenie Kopiuj, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję Wklej. Teraz jest obiektem usługi RDP Copy1, które można edytować. Kliknij prawym przyciskiem myszy RDP Copy1 i wybierz pozycję Edytuj usługi Edytuj obiekt okna protokołu pop długości, jak pokazano poniżej:

![Kopiowanie reguły RDP domyślnej][5]

Następnie można edytować wartości reprezentować usługę RDP dla określonego serwera. Dla AppVM01 powyżej reguły RDP domyślnej należy zmodyfikować, aby odzwierciedlała Nowa nazwa usługi, opis i zewnętrznych Port RDP używany na diagramie rysunek 8 (Uwaga: porty są zmieniane z domyślnego RDP 3389 na zewnętrznym port używany dla tego serwera określonego, w przypadku AppVM01 portu zewnętrznego jest 8025) poniżej przedstawiono zmienione usługi :

![Reguła AppVM01][6]
 
Należy powtórzyć ten proces utworzyć usług RDP przeznaczone dla pozostałych serwerów; AppVM02, DNS01 i IIS01. Tworzenie tych usług spowoduje utworzenie reguły prostszy i wyraźniejsze w następnej sekcji.

>[AZURE.NOTE] RDP obsługę zapory nie jest potrzebna dwóch powodów; 1) pierwszego zapory maszyn wirtualnych jest obraz systemem Linux, więc SSH może być używany na porcie 22 do zarządzania maszyn wirtualnych zamiast RDP i (2) port 22, a dwa porty zarządzania są dozwolone w pierwszą regułę zarządzania opisane poniżej, aby umożliwić łączności zarządzania.

### <a name="firewall-rules-creation"></a>Tworzenie reguły zapory
Istnieją trzy typy reguł zapory, w tym przykładzie użyto, wszystkie mają różne ikony:

Reguły przekierowywania aplikacji: ![Przekierowywanie ikony aplikacji][7]

Reguły NAT miejsce docelowe: ![Ikona translatora adresów Sieciowych miejsce docelowe][8]

Reguła przebiegu: ![Ikona przekazywania][9]

Więcej informacji na temat tych reguł można znaleźć w witrynie sieci web Barracuda.

Aby utworzyć następujących reguł (lub Sprawdź istniejące zasady domyślne), rozpoczynając od pulpitu nawigacyjnego administratora NG Barracuda klienta, przejdź na kartę Konfiguracja w konfiguracji operacyjnych sekcji kliknij pozycję zestaw reguł. Siatki o nazwie "Główne reguł" zostaną wyświetlone istniejących reguł aktywne i nieaktywne na tym Zapora. W prawym górnym rogu siatka jest małe, zielony "+" przycisku, kliknij tutaj, aby utworzyć nową regułę (Uwaga: zapory może być "zablokowana" w przypadku zmian, jeśli będzie widoczny przycisk oznaczony "Zablokować" i nie można tworzyć lub edytować reguły, kliknij ten przycisk, aby "odblokować" zestaw reguł i Zezwalaj na edytowanie). Jeśli chcesz edytować istniejącą regułę, zaznacz tę regułę, kliknij prawym przyciskiem myszy i wybierz pozycję Edytuj regułę.

Gdy reguły są tworzone i/lub zmodyfikowane, musi być przypisany do zapory i następnie aktywowane, jeśli nie można to zrobić zmiany reguły nie zostały wprowadzone. Proces wypychanych i aktywacji został opisany poniżej nazwy reguły szczegóły.

Szczegółowe informacje na temat reguł wymagany do wykonania w tym przykładzie się następująco:

- **Reguła zarządzania**: przekierowywanie tę aplikację reguła zezwala na przekazywanie ruchu do porty zarządzania urządzenia wirtualnej sieci, w tym przykładzie zapory NextGen Barracuda. Porty zarządzania są 801 807 i opcjonalnie 22. Porty zewnętrzne i wewnętrzne są takie same (to znaczy nie tłumaczenie port). Reguły, ustawienia zarządzania dostęp, jest to reguła domyślne i domyślnie (w wersji zapory NextGen Barracuda 6.1).

    ![Reguła zarządzania][10]

>[AZURE.TIP] Przestrzeń adresów źródła w tej regule jest, jeśli są znane zakresy adresów IP zarządzania, zmniejszenie zakresu będzie również zmniejszyć powierzchnię atakiem porty zarządzania.

- **RDP reguł**: reguły te NAT miejsce docelowe, aby umożliwić zarządzanie poszczególnych serwerach za pośrednictwem RDP.
Istnieją cztery krytyczne, pola potrzebne do utworzenia tej reguły:
  1.    Źródła — aby umożliwić RDP z dowolnego miejsca, odwołanie "Dowolny" jest używany w polu źródło.
  2.    Usługa — za pomocą odpowiedniego obiektu usługi utworzony wcześniej, w tym przypadku "AppVM01 RDP", zewnętrzne porty przekierowywania do serwerów lokalny adres IP i port 3386 (domyślny port RDP). Ta reguła określone jest RDP dostępu do AppVM01.
  3.    Miejsce docelowe-powinna być *lokalne portów zapory*, "DCHP 1 lokalny adres IP" lub eth0, jeśli przy użyciu statycznego adresy IP. Liczby porządkowe (eth0, eth1 itp.) mogą być inne, jeśli urządzenia sieciowe ma wiele interfejsów lokalny. Jest to numer portu zapory wysyła z (może być taka sama jak odbierania port), rzeczywisty routingu miejsce docelowe znajduje się w polu listy docelowej.
  4.    Przekierowywanie — w tej sekcji wskazuje wirtualnego urządzenia, gdzie ostatecznie przekierowywać ruch. Najłatwiejszym przekierowania jest umieszczenie adresu IP i portu (opcjonalnie) w polu listy docelowej. Jeśli port nie jest używany z numerem portu docelowego na żądanie przychodzące będą używane (ie nie tłumaczenie), jeśli port jest oznaczony port będzie również translatora adresów Sieciowych wraz z IP zajmie.

    ![Reguła RDP][11]

    Łącznie cztery reguły RDP muszą zostać utworzone: 

  	|   Nazwa reguły    | Serwer  |   Usługa   |  Lista docelowa  |
  	|----------------|---------|-------------|---------------|
  	| RDP do IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP do DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP do AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP do AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Zawęzić zakres pól źródłowych i usług w dół zmniejszy powierzchni atakiem. Najbardziej ograniczony zakres, który pozwoli funkcji powinien być używany.

- **Reguły ruchu aplikacji**: istnieją dwie reguły ruch aplikacji: pierwszego ruchu frontonu sieci web, a drugi dla ruchu wewnętrznej (np serwera sieci web do warstwy danych). Te reguły zostaną zależą od architektury sieci (serwery rozmieszczenie) i ruchu przepływów (kierunku przepływu ruchu i portów, do których są używane).

    Najpierw omówiony jest reguła fronton dla ruchu w sieci web:

    ![Reguła firmy Microsoft w sieci Web][12]
 
    Ta reguła docelowego translatora adresów Sieciowych zezwala na ruch rzeczywistego zastosowania nawiązania połączenia z serwerem aplikacji. Inne zasady pozwalają na zabezpieczenia, zarządzanie, itd., reguły aplikacji są co zezwolić użytkownikom zewnętrznym lub usługi uzyskać dostęp do aplikacji. W tym przykładzie jest serwer jednej sieci web na porcie 80, więc jednej aplikacji reguła przekieruje ruchu przychodzącego do zewnętrznych adresów IP na sieci web serwerów wewnętrznych adres IP.

    **Uwaga**:, że nie ma żadnego portu, w polu listy docelowej przypisany, więc port wejściowy 80 (lub 443 dla wybranej usługi) zostanie użyte w przekierowywanie serwer sieci web. Jeśli serwer sieci web nasłuchują do innego portu, na przykład portu 8080, pole listy docelowej można zaktualizować do 10.0.1.4:8080 umożliwia także przekierowywanie portu.

    Następny reguły ruchu aplikacji jest regułę wewnętrznej, aby umożliwić serwer sieci Web porozmawiać z serwera AppVM01 (ale nie AppVM02) za pomocą dowolnej usługi:

    ![Reguła AppVM01][13]

    Ta reguła przebieg umożliwia dowolny serwer usług IIS podsieci serwera sieci Web do osiągnięcia AppVM01 (adres IP 10.0.2.5) na dowolnym porcie przy użyciu protokołu dostęp do danych potrzeb przez aplikację sieci web.

    W tym zrzut ekranu "\<jawne dest\>" jest używany w polu docelowym do oznaczają 10.0.2.5 jako miejsca docelowego. Może to być albo jawne pokazany lub a o nazwie obiektu sieci (jak zostało to wykonane w wymagania wstępne dotyczące serwera DNS). Jest to administratora zapory, które będą używane metody. Aby dodać 10.0.2.5 jako Explict Desitnation, kliknij dwukrotnie w pierwszym pustym wierszu w obszarze \<jawne dest\> i wprowadź adres w oknie, które pojawia się.

    Z tej reguły przekazać nie translatora adresów Sieciowych jest potrzebna, ponieważ jest to ruchu wewnętrznym, więc można ustawić metody połączenia "Brak SNAT".

    **Uwaga**: sieć źródła w tej regule jest dowolny zasób podsieci serwera sieci Web, jeśli będzie tylko jedną lub znane określoną liczbę serwery sieci web, obiekt sieci można utworzyć zasobu dokładniej do adresów IP dokładnie zamiast całego podsieci serwera sieci Web.

>[AZURE.TIP] Ta reguła jest używana usługa "Dowolne" Aby ułatwić konfigurowanie i używanie aplikacji przykładowej, dzięki temu będzie również ICMPv4 (ping) w jednej reguły. Jednak nie jest zalecane praktyki. Porty i protokoły ("usługi") należy zwęzić, aby możliwe minimum, które umożliwia operacji aplikacji zmniejszyć powierzchnię atakiem w tej granicy.

<br />

>[AZURE.TIP] Mimo że ta reguła zawiera odwołanie jawne dest używany, spójne podejście powinien być używany w konfiguracji zapory. Zalecane jest, można użyć nazwanego obiektu sieci w całym w czytelności łatwiejsze i obsługą. Jawne dest tutaj służy tylko do pokazywania metody odniesienia i ogólnie nie zaleca się (szczególnie w przypadku złożonych konfiguracji).

- **Wychodzące reguły Internet**: przekazać tę regułę, aby umożliwić ruch z dowolnej sieci źródła w celu przekazania do wybranych sieciach miejsca docelowego. Ta reguła jest to domyślne reguła zazwyczaj już zapory Barracuda NextGen, ale jest wyłączony. Kliknięcie prawym przyciskiem myszy tę regułę można uzyskać dostęp do polecenia aktywować regułę. Reguła pokazano w przykładzie został zmodyfikowany dodać dwa podsieci lokalnych, które zostały utworzone jako odniesienia w sekcji wstępne tego dokumentu do atrybutu źródła tej reguły.

    ![Reguła ruchu wychodzącego][14]

- **Reguła DNS**: przekazać tę regułę zezwala tylko na ruch DNS (port 53) w celu przekazania do serwera DNS. Dla tego środowiska, który większości ruchu z serwera sieci Web do wewnętrznej bazy danych jest zablokowany ta reguła umożliwia specjalnie DNS.

    ![Reguła DNS][15]

    **Uwaga**: W tym ekranu jest dołączany zrzut metody połączenia. Ponieważ ta reguła jest wewnętrzny IP do wewnętrznego ruchu adres IP, NATing nie jest wymagane, to metody połączenia jest równa "Brak SNAT" dla tej reguły przebiegu.

- **Reguła podsieci do sieci**: przekazać ta reguła jest domyślna reguła, która została aktywowana i zmodyfikowane w celu umożliwienia dowolnego serwera podsieci wewnętrznej do połączenia z serwerem podsieci frontonu. Ta reguła jest ruchu wszystkich wewnętrznym, więc SNAT nie można ustawić metody połączenia.

    ![Reguła VNet wewnątrz][16]

    **Uwaga**: dwukierunkowego pole wyboru nie jest zaznaczone (ani nie jest zaznaczone pole wyboru w większości reguł), to ma znaczenie dla tej reguły, ponieważ ułatwia to reguły "w jedną stronę", połączenie może zostać zainicjowana z podsieci wewnętrznej frontonu sieci, ale nie odwrotnie. Zaznaczenie tego pola wyboru została ta reguła będzie włączyć ruch dwukierunkowa, którego z naszych diagram logiczny nie jest wymagana.

- **Odmówić wszystkie reguły ruchu**: zawsze należy ostateczne reguły (pod względem priorytet) oraz sposób jeśli ruch będzie przepływał nie będzie pasować do żadnej z poprzedniego zasady zostaną usunięte przez tę regułę. Jest to reguła domyślne i zazwyczaj jest aktywna, nie są zazwyczaj potrzebne modyfikacje. 

    ![Zapora odmówić reguły][17]

>[AZURE.IMPORTANT] Po utworzeniu wszystkich powyższych reguł, należy przejrzeć priorytet każdego reguły w celu zapewnienia ruch będzie dozwolony lub odmowa stosownie do potrzeb. W tym przykładzie zasady są w kolejności, w jakiej zostały należy w siatce główne reguły w kliencie zarządzania Barracuda przekierowywania.

## <a name="rule-activation"></a>Aktywacja reguły
Z zestaw reguł modyfikować tak, aby specyfikacji wentylacyjna — diagram zestaw reguł należy przekazać do zapory i następnie aktywowane.

![Aktywacja reguły zapory][18]
 
W prawym górnym rogu klienta zarządzania są klastrze przycisków. Kliknij przycisk "Wyślij zmian", aby wysłać zmienione reguły zapory, a następnie kliknij przycisk "Aktywuj".
 
W aktywacji zestaw reguł zapory tej kompilacji środowiska przykład zostało wykonane.

## <a name="traffic-scenarios"></a>Scenariusze ruchu
>[AZURE.IMPORTANT] Kluczowe takeway jest należy pamiętać, że **cały** ruch rozpocznie się za pośrednictwem zapory. Tak do pulpitu zdalnego na serwerze IIS01 mimo to w usłudze Cloud końcu przedniego i podsieci fronton dostępu do tego serwera możemy będzie konieczne RDP zapory na porcie 8014, a następnie pozwól zaporę, aby skierować żądanie RDP wewnętrznie do portu RDP IIS01. Przycisk "Połącz" Azure portal nie będą działać, ponieważ istnieje nie bezpośrednią ścieżkę RDP IIS01 (o ile portalu mogą widzieć). Oznacza to, że wszystkie połączenia z Internetem będzie usługa zabezpieczeń i portu, np. secscv001.cloudapp.net:xxxx.

W przypadku następujących scenariuszy następujących reguł zapory powinny być stosowane:

1.  Zarządzanie zaporą
2.  RDP do IIS01
3.  RDP do DNS01
4.  RDP do AppVM01
5.  RDP do AppVM02
6.  Aplikacja ruchu w sieci Web
7.  Ruch aplikacji do AppVM01
8.  Wychodzące do Internetu
9.  Frontend do DNS01
10. Ruch wewnątrz podsieci (wstecz end, aby tylko fronton)
11. Odmów wszystkich

Zestaw reguł Zapora najprawdopodobniej będzie wiele reguł oprócz tych, zasady dotyczące dowolnego danego zapory również zostanie przydzielony numery priorytetu innego niż wymienione tutaj. Tej listy i skojarzone liczby są o podanie istotności między tylko te reguły 11 i priorytet względny między nimi. Innymi słowy; rzeczywisty zapory "RDP do IIS01" może być reguły liczby 5, ale dopóki jest mniejsza od reguły "Zarządzanie zaporą" i powyżej reguły "RDP DNS01" chcesz wyrównać z tej listy. Listy również pomoże w poniżej scenariusze umożliwiające ich omówień; przykład "FW reguły 9 (DNS)". Ich omówień, cztery reguły RDP będzie można wspólnie skrót, "reguły RDP" kiedy scenariusz ruch nie ma wpływu na RDP.

Również odwoływanie grup zabezpieczeń sieci to w miejscu na ruch przychodzący z Internetu podsieci serwera sieci Web i wewnętrznej bazy danych.

#### <a name="allowed-internet-to-web-server"></a>(Dozwolone) Internet do serwera sieci Web
1.  Strony internetowe użytkownika żądania HTTP z SecSvc001.CloudApp.Net (Internet przeciwległych usługi w chmurze)
2.  Chmury usługi przebiegów ruch przez otwarte punktu końcowego na porcie 80 interfejsu zapory z 10.0.0.4:80
3.  Nie NSG przypisane do zabezpieczeń podsieci tak reguły NSG system zezwalanie na ruch do Zapora systemu Windows
4.  Ruch trafienia wewnętrzny adres IP zapory (10.0.1.4)
5.  Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    6 reguły Zapory (aplikacja: sieci Web) zastosować ruch jest dozwolony, zapory NAT go do 10.0.1.4 (IIS01)
6.  Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie dotyczy (ruch została translatora adresów Sieciowych czy przez zaporę, a więc adres źródłowy jest teraz zaporę, która znajduje się w podsieci zabezpieczeń i widoczne dla podsieci Frontend NSG być ruchu "lokalne", a więc jest dozwolona), przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
7.  IIS01 oczekuje na ruchu w sieci web, odbiera żądania i uruchamia, przetwarzania żądania
8.  IIS01 próby inicjuje sesji FTP, aby AppVM01 podsieci wewnętrznej bazy danych
9.  Trasa UDR podsieci Frontend sprawia, że zapory następnego przeskoku
10. Nie reguł ruchu wychodzącego podsieci Frontend ruch jest dozwolony
11. Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    6 reguły Zapory (aplikacja: sieci Web) nie zastosować, przejście do następnego reguły
  4.    7 reguły Zapory (aplikacja: wewnętrznej bazy danych) zastosować ruch jest dozwolony, zapora przekazuje dane do 10.0.2.5 (AppVM01)
12. Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
13.  AppVM01 odbiera żądanie inicjuje sesję i odpowiedzi
14. Trasa UDR podsieci wewnętrznej bazy danych powoduje, że zapory następnego przeskoku
15. Ponieważ reguły nie NSG ruchu wychodzącego podsieci wewnętrznej bazy danych odpowiedź jest dozwolona
16. Ponieważ to zwraca ruch na wskazanych sesji zapora przekazuje odpowiedź do serwera sieci web (IIS01)
17. Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
18. Serwer usług IIS odbiera odpowiedź, wykonuje transakcja z AppVM01, a następnie uzupełnia tworzenia odpowiedzi HTTP, ta odpowiedź HTTP jest wysyłana do żądającego
19. Ponieważ reguły nie NSG ruchu wychodzącego podsieci Frontend odpowiedzi jest dozwolona
20. Odpowiedź HTTP trafienia zapory i, ponieważ jest odpowiedź wskazanych sesji translatora adresów Sieciowych zostanie odebrane przez zaporę
21. Zapora przekierowuje następnie odpowiedź do użytkownika Internet
22. Ponieważ nie ruchu wychodzącego reguły NSG lub przeskoków UDR podsieci serwera sieci Web, którą może odpowiedzi i użytkownik Internetu, otrzymuje żądanej strony sieci web.

#### <a name="allowed-internet-rdp-to-backend"></a>(Dozwolone) Internet RDP do wewnętrznej bazy danych
1.  Administrator serwera w Internecie żądania sesja RDP AppVM01 za pośrednictwem SecSvc001.CloudApp.Net:8025, gdzie 8025 jest użytkownika przypisany numer portu reguły zapory "RDP AppVM01"
2.  Usługa w chmurze przekazuje ruch przez punkt końcowy otwarte na porcie 8025 do zapory interfejs 10.0.0.4:8025
3.  Nie NSG przypisane do zabezpieczeń podsieci tak reguły NSG system zezwalanie na ruch do Zapora systemu Windows
4.  Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    FW reguła 2 (RDP IIS) nie zastosować, przejście do następnego reguły
  3.    FW zasady 3 (RDP DNS01) nie zastosować, przejście do następnego reguły
  4.    Stosowanie FW reguły 4 (RDP AppVM01), ruch jest dozwolony, zapory NAT go do 10.0.2.5:3386 (port RDP na AppVM01)
5.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie dotyczy (ruch została translatora adresów Sieciowych czy przez zaporę, a więc adres źródłowy jest teraz zaporę, która znajduje się w podsieci zabezpieczeń i widoczne dla podsieci wewnętrznej bazy danych NSG być ruchu "lokalne", a więc jest dozwolona), przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
6.  AppVM01 oczekuje na ruch RDP i odpowiada
7.  Nie reguły ruchu wychodzącego NSG zasady domyślne i zwrotu ruch jest dozwolony
8.  UDR kieruje ruch wychodzący do zapory jako następnego przeskoku
9.  Ponieważ to zwraca ruch na wskazanych sesji zapora przekazuje odpowiedź do użytkownika internet
10. Sesja RDP jest włączona
11. AppVM01 monituje o podanie hasła nazwa użytkownika

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Dozwolone) Wyszukiwanie serwera DNS sieci Web dla serwera DNS
1.  Web Server, IIS01, wymagań w www.data.gov strumieniowego źródła danych, ale musi rozpoznać adresu.
2.  Konfiguracja sieciowa dla list VNet DNS01 (10.0.2.4 podsieci wewnętrznej bazy danych) jako podstawowy serwer DNS, IIS01 przesyła żądanie DNS DNS01
3.  UDR kieruje ruch wychodzący do zapory jako następnego przeskoku
4.  Nie reguły ruchu wychodzącego NSG są powiązane z podsieci serwera sieci Web, ruch jest dozwolony
5.  Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    FW zasad 6 i 7 (reguły aplikacji) nie zastosować, przejście do następnego reguły
  4.    8 reguły Zapory (Internet w celu) nie zastosować, przejście do następnego reguły
  5.    Stosowanie 9 reguły Zapory (DNS), ruch jest dozwolony, zapora przekazuje dane do 10.0.2.4 (DNS01)
6.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
7.  Serwer DNS odbiera żądania
8.  Serwer DNS nie ma adres przechowywanych w pamięci podręcznej i z pytaniem główny serwer DNS w Internecie
9.  UDR kieruje ruch wychodzący do zapory jako następnego przeskoku
10. Nie reguł ruchu wychodzącego NSG podsieci wewnętrznej bazy danych ruch jest dozwolony
11. Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    FW zasad 6 i 7 (reguły aplikacji) nie zastosować, przejście do następnego reguły
  4.     Stosowanie FW zasady 8 (Internet w celu), ruch jest dozwolony, sesji jest SNAT się główny serwer DNS w Internecie
12. Serwer Internet DNS odpowiada, ponieważ w tej sesji zostało zainicjowane przez zaporę, odpowiedź zostanie odebrane przez zaporę
13. Jak to ustanowienia sesji zapory przekazuje odpowiedź na serwerze początkowa DNS01
14. Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
15. Serwer DNS odbiera i umieszcza odpowiedź w pamięci podręcznej i odpowiada żądania początkowego powrót do IIS01
16. Trasa UDR podsieci wewnętrznej bazy danych powoduje, że zapory następnego przeskoku
17. Istnieją żadne reguły wychodzącej NSG podsieci wewnętrznej bazy danych, ruch jest dozwolony
18. Jest to sesję wskazanych w zaporze, odpowiedzi jest przekazywany przez zaporę na serwerze usług IIS
19. Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    Istnieje reguła nie NSG, która dotyczy przychodzące ruchu z podsieci wewnętrznej bazy danych do podsieci serwera sieci Web, więc żaden z NSG zasady, stosowanie
  2.    Reguła systemowa domyślne zezwolenia na ruch między podsieci umożliwi ruch, więc ruch jest dozwolony
20. IIS01 odbiera odpowiedź z DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Dozwolone) Serwera wewnętrznej bazy danych do serwera sieci Web serwera
1.  Administrator zalogowany do AppVM02 za pośrednictwem RDP żąda pliku bezpośrednio z serwera IIS01 za pomocą Eksploratora plików systemu windows
2.  Trasa UDR podsieci wewnętrznej bazy danych powoduje, że zapory następnego przeskoku
3.  Ponieważ reguły nie NSG ruchu wychodzącego podsieci wewnętrznej bazy danych odpowiedź jest dozwolona
4.  Przetwarzanie reguły zapory rozpoczynają się od:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    FW zasad 6 i 7 (reguły aplikacji) nie zastosować, przejście do następnego reguły
  4.    8 reguły Zapory (Internet w celu) nie zastosować, przejście do następnego reguły
  5.    Reguły Zapory 9 DNS (Domain Name System) nie zastosować, przejście do następnego reguły
  6.    Stosowanie FW reguły 10 (wewnątrz podsieć), ruch jest dozwolony, zapora przekazuje ruch do 10.0.1.4 (IIS01)
5.  Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
6.  Przy założeniu pisane z wielkiej litery uwierzytelniania i autoryzacji, IIS01 zaakceptuje żądanie i odpowiada
7.  Trasa UDR podsieci Frontend sprawia, że zapory następnego przeskoku
8.  Ponieważ reguły nie NSG ruchu wychodzącego podsieci Frontend odpowiedzi jest dozwolona
9.  Jak jest to istniejącej sesji w zaporze odpowiedź jest dozwolona i zapory zwraca odpowiedź do AppVM02
10. Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (blok Internet) nie zastosować, przejście do następnego reguły
  2.    Domyślne reguły NSG zezwalanie na ruch podsieci do sieci, ruch jest dozwolony, Zatrzymaj przetwarzanie reguł NSG
11. AppVM02 odbiera odpowiedź

#### <a name="denied-internet-direct-to-web-server"></a>(Odmowa) Internet bezpośrednio do serwera sieci Web
1.  Użytkownik Internetu próbuje uzyskać dostęp do serwera sieci web, IIS01, za pośrednictwem usługi FrontEnd001.CloudApp.Net
2.  Ponieważ nie punkty końcowe są otwarty ruch HTTP, to nie będzie przebiegać przez usługi w chmurze, a nie połączenia z serwerem
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, NSG (Internet bloku) podsieci serwera sieci Web umożliwia zablokowanie ruch
4.  Na końcu trasę UDR podsieci serwera sieci Web będzie wysyłać dowolnego ruchu wychodzącego z IIS01 do zapory jako następnego przeskoku i zapory będzie to zobaczyć jako asymetrycznym ruchu i upuść ruchu wychodzącego odpowiedzi, więc są co najmniej trzy warstwy niezależnych obrony między internetowe i IIS01 za pośrednictwem jego usługi w chmurze zapobieganie autoryzacji niewłaściwy dostęp.

#### <a name="denied-internet-to-backend-server"></a>(Odmowa) Internet do serwera wewnętrznej bazy danych
1.  Użytkownik Internetu próbuje uzyskać dostęp do plików na AppVM01 za pośrednictwem usługi BackEnd001.CloudApp.Net
2.  Ponieważ nie punkty końcowe są otwarte do udziału plików, to nie może przekazać usług w chmurze, a nie połączenia z serwerem
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, NSG (Internet bloku) będą zablokowanie takiego ruchu
4.  Na końcu trasę UDR chcesz wysyłać dowolnego ruchu wychodzącego z AppVM01 do zapory jako następnego przeskoku i zapory może to widzieć jako asymetrycznym ruchu i upuść ruchu wychodzącego odpowiedzi, a więc są co najmniej trzy warstwy niezależnych obrony między internetowe i AppVM01 za pośrednictwem jego usługi w chmurze autoryzacji nieodpowiedni dostęp.

#### <a name="denied-frontend-server-to-backend-server"></a>(Odmowa) Serwer Frontend serwera wewnętrznej bazy danych
1.  Przyjmuje IIS01 zostało naruszone i jest uruchomiony niebezpieczny kod próby skanowania serwery podsieci wewnętrznej bazy danych.
2.  Trasa UDR podsieci serwera sieci Web będzie wysyłać dowolnego ruchu wychodzącego z IIS01 do zapory jako następnego przeskoku. Nie jest to może się zmienić w złamany maszyn wirtualnych.
3.  Zapory będzie przetwarzał ruchu, jeśli żądanie zostało AppVM01 lub do serwera DNS w celu wyszukiwania DNS, które mogą potencjalnie dozwolony ruch przez zaporę (z powodu 7 reguły Zapory i 9). Inny ruch czy zablokowane przez FW zasady 11 (Odmów wszystkim).
4.  Jeśli zaawansowane wykrywania zagrożenie włączono zapory (które nie jest wymienione w tym dokumencie, zapoznaj się z dokumentacją dostawcy dla swojego urządzenia określonej sieci zaawansowane funkcje zagrożenia), jeśli ruch zawierałoby znane podpisów lub desenie, które Flaga reguły zaawansowane zagrożenie można zapobiegać nawet ruch, niedozwolonego przez reguły podstawowe przekazywanie omówiony w tym dokumencie.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Odmowa) Wyszukiwanie Internet DNS na serwerze DNS
1.  Użytkownik Internetu spróbuje wyszukać wewnętrznych rekordu DNS na DNS01 za pośrednictwem usługi BackEnd001.CloudApp.Net 
2.  Ponieważ nie punkty końcowe są otwarty ruch DNS, to nie będzie przebiegać przez usługi w chmurze, a nie połączenia z serwerem
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, reguły NSG (Internet bloku) podsieci serwera sieci Web umożliwia zablokowanie ruch
4.  Na końcu trasę UDR podsieci wewnętrznej bazy danych będzie wysyłać dowolnego ruchu wychodzącego z DNS01 do zapory jako następnego przeskoku i zapory będzie to zobaczyć jako asymetrycznym ruchu i upuść ruchu wychodzącego odpowiedzi, więc są co najmniej trzy warstwy niezależnych obrony między internetowe i DNS01 za pośrednictwem jego usługi w chmurze zapobieganie autoryzacji niewłaściwy dostęp.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Odmowa) Internet SQL dostępu przez zaporę
1.  Internet użytkownik zażąda danych SQL z SecSvc001.CloudApp.Net (Internet przeciwległych usługi w chmurze)
2.  Ponieważ nie punkty końcowe są otwarty SQL, to nie będzie przebiegać usług w chmurze i nie osiągnięcia zapory
3.  Jeśli punkty końcowe SQL były otwarte jakiegoś powodu, zapory chcesz rozpocząć przetwarzanie reguły:
  1.    FW reguły 1 (FW zarządzania) nie zastosować, przejście do następnego reguły
  2.    Reguły Zapory 2-5 (RDP reguły) nie zastosować, przejście do następnego reguły
  3.    FW reguły 6 i 7 (reguły aplikacji) nie zastosować, przejście do następnego reguły
  4.    8 reguły Zapory (Internet w celu) nie zastosować, przejście do następnego reguły
  5.    Reguły Zapory 9 DNS (Domain Name System) nie zastosować, przejście do następnego reguły
  6.    10 reguły Zapory (wewnątrz podsieć) nie zastosować, przejście do następnego reguły
  7.    Zastosuj FW zasady 11 (Odmów wszystkim), jest reguła blokowane, Zatrzymaj przetwarzanie


## <a name="references"></a>Odwołania
### <a name="main-script-and-network-config"></a>Skrypt głównym i konfiguracji sieci
Pełna skrypt należy zapisać w pliku skryptu programu PowerShell. Zapisywanie konfiguracji sieci w pliku o nazwie "NetworkConf2.xml".
Zmodyfikuj zmienne zdefiniowane przez użytkownika, stosownie do potrzeb. Uruchom skrypt, a następnie postępuj zgodnie z instrukcjami instalacji reguły zapory powyżej.

#### <a name="full-script"></a>Pełna skryptu
Ten skrypt będzie, na podstawie zmiennych zdefiniowanych przez użytkownika:

1.  Nawiązywanie połączenia z subskrypcji usługi Azure
2.  Utwórz nowe konto miejsca do magazynowania
3.  Tworzenie nowego VNet i trzech podsieci, zgodnie z definicją w pliku konfiguracji sieci
4.  Tworzenie pięciu maszyn wirtualnych; Zapora 1 i 4 windows server maszyny wirtualne
5.  Konfigurowanie, łącznie z UDR:
  1.    Tworzenie nowych tabel rozsyłania
  2.    Dodawanie trasy do tabel
  3.    Powiązać podsieci odpowiednie tabele
6.  Włącz przesyłanie dalej IP na NVA
7.  Konfigurowanie NSG w tym:
  1.    Tworzenie NSG
  2.    Dodawanie reguły
  3.    Wiązanie NSG do odpowiedniej podsieci

Ten skrypt programu PowerShell powinny być uruchamiane lokalnie na, że połączenie internetowe komputera lub serwera.

>[AZURE.IMPORTANT] Po uruchomieniu tego skryptu mogą istnieć ostrzeżenia i inne komunikaty informacyjne pop w programie PowerShell. Tylko komunikaty o błędach w kolorze czerwonym są przyczyny dotyczą.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Plik konfiguracyjny sieci
Zapisz ten plik xml z lokalizacją zaktualizowane i dodać łącze do tego pliku do zmiennej $NetworkConfigFile w skrypt powyżej.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Przykładowe skrypty aplikacji
Jeśli chcesz zainstalować przykładową aplikację dla tej i inne przykłady strefy Zdemilitaryzowanej jedną dostarczono na następujące łącze: [Przykładowy skrypt aplikacji][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Dwukierunkowa strefy Zdemilitaryzowanej z NVA, NSG i UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Widok logiczny reguły zapory"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Tworzenie obiektu w sieci serwera sieci Web"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Tworzenie obiektu serwera DNS"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopiowanie reguły RDP domyślnej"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "Reguła AppVM01"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona Przekieruj aplikacji"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona translatora adresów Sieciowych miejsce docelowe"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona przebiegu"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Reguła zarządzania"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Reguła RDP"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Reguła firmy Microsoft w sieci Web"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Reguła AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Reguła ruchu wychodzącego"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Reguła DNS"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Reguła VNet wewnątrz"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Zapora odmówić reguły"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktywacja reguły zapory"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
