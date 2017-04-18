<properties 
   pageTitle="Połączenie hybrydowego z aplikacji poziomu 2 | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć wirtualnego urządzenia i UDR w celu utworzenia środowiska wielu aplikacji platformy Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Scenariusz wirtualnego urządzenia

Typowy scenariusz większe Azure klientów jest potrzeba umieszczenia aplikacji dwuwarstwowej udostępniana w Internecie podczas zezwolenia na dostęp do poziomu wstecz z centrum danych lokalnych. Ten dokument pomogą pośrednictwem za pomocą użytkownika zdefiniowane trasy (UDR), Brama VPN i wyposażenia wirtualna sieć wdrażania środowiska dwupoziomowy, która spełnia następujące wymagania:

- Aplikacja sieci Web musi być dostępne za pośrednictwem publicznego Internetu.
- Serwera sieci Web obsługującego aplikacji muszą mieć możliwość uzyskania dostępu do serwera aplikacji wewnętrznej bazy danych.
- Za pośrednictwem zapory urządzenia wirtualnego należy przejść do całego ruchu z Internetu do aplikacji sieci web. Wirtualna urządzenie zostanie użyty jedynie do ruchu internetowego.
- Cały ruch, przechodząc do serwera aplikacji należy przejść za pomocą urządzenia wirtualnego zapory. Wirtualna urządzenie będzie używane na dostęp do serwera zakończenia wewnętrznej bazy danych i odbierane z sieci lokalnej za pośrednictwem bramą VPN programu access.
- Administratorzy muszą mieć możliwość zarządzania urządzenia wirtualnego zapory ze swoich komputerów lokalnych, przy użyciu innej zapory wirtualnego urządzenia używane wyłącznie w celach zarządzania.

Jest to standardowy scenariusz strefy Zdemilitaryzowanej z strefy Zdemilitaryzowanej i chronionej sieci. Takie scenariusz może być skonstruowane w Azure przy użyciu NSGs, urządzenia wirtualnego zapory lub obu tych sposobów. W poniższej tabeli przedstawiono niektóre z ich wad i zalet między NSGs i wyposażenia wirtualnych zapory.

||Zalety|Ikony|
|---|---|---|
|NSG|Bezpłatne. <br/>Zintegrowane Azure RBAC. <br/>Reguły można tworzyć w szablonach ARM.|Złożoność może być różna w środowiskach większe. |
|Zapory|Pełna kontrola nad płaszczyzny danych. <br/>Centralne zarządzanie za pomocą konsoli zapory.|Koszt urządzenia zapory. <br/>Nie jest zintegrowany z Azure RBAC.|

Poniższe rozwiązania używa urządzeń wirtualnych zapory w celu zaimplementowania scenariusza sieci strefy Zdemilitaryzowanej chroniony.

## <a name="considerations"></a>Zagadnienia dotyczące

Można wdrażać środowiska wyjaśniono powyżej w Azure za pomocą różne funkcje dostępne już dziś, w następujący sposób.

- **Wirtualna sieć (VNet)**. VNet Azure działa w podobny sposób do sieci lokalnej i można części do jednej lub kilku podsieci o podanie izolacji ruchu i odstęp rozwiązania problemów.
- **Wirtualna urządzenia**. Kilku partnerów zapewniają wirtualnego urządzenia w Azure Marketplace, używanej do trzech zapory opisany powyżej. 
- **Trasy (UDR) zdefiniowane przez użytkownika**. Trasa tabel może zawierać UDRs używane przez sieć Azure przepływ pakietów w VNet. W poniższych tabelach rozsyłania mogą być stosowane do podsieci. Jedną z najnowszych funkcji w Azure jest możliwość stosowania tabeli trasy do GatewaySubnet, daje możliwość przekazywania wszystkich danych rozpoczynająca Azure VNet z połączeniem hybrydowych wirtualnego urządzenia.
- **Przekazywanie adresów IP**. Domyślnie aparat sieci Azure przesyłania pakietów do wirtualnej sieci interfejsu kart tylko wtedy, gdy docelowy adres IP pakietów pasuje do adresu IP karty Sieciowej. W związku z tym jeśli UDR Określa, że pakiet musi być wysyłane do danego urządzenia wirtualnego, aparat sieci Azure czy upuść pakietu. Aby upewnić się, że pakiet będą dostarczane do maszyn wirtualnych (w tym przypadku wirtualnego urządzenia), nie będącej rzeczywisty docelowy pakietu, musisz włączyć przekazywanie adresów IP dla wirtualnego urządzenia.
- **Grupy zabezpieczeń sieci (NSGs)**. W poniższym przykładzie nie korzystają z NSGs, ale NSGs stosowane do danej podsieci i/lub nic w rozwiązaniu można użyć w celu dalszego filtrowania ruchu i tych podsieci i nic.


![Łączności IPv6](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- 2 grup zasobów, nie jest wyświetlana na diagramie. 
    - **ONPREMRG**. Zawiera wszystkie zasoby niezbędne w celu zasymulowania sieci lokalnej.
    - **AZURERG**. Zawiera wszystkie zasoby niezbędne do Azure wirtualnego środowiska sieciowego. 
- VNet o nazwie **onpremvnet** umożliwia symulowanie centrum danych lokalnych części w sposób opisany poniżej.
    - **onpremsn1**. Podsieci zawierającej maszyny wirtualnej (maszyn wirtualnych) działa Ubuntu symulowanie lokalnego serwera.
    - **onpremsn2**. Podsieci zawierającej maszyny uruchomiony Ubuntu symulowanie komputera lokalnego używane przez administratora.
- Ma jednego urządzenia wirtualnego zapory o nazwie **OPFW** na **onpremvnet** używany do obsługi tunelem do **azurevnet**.
- VNet o nazwie **azurevnet** części wymienione poniżej.
    - **azsn1**. Podsieć zapory zewnętrznej używane wyłącznie do zapory zewnętrznej. Cały ruch internetowy będzie okazać się do danej podsieci. Danej podsieci zawiera tylko NIC połączone z zapory zewnętrznej.
    - **azsn2**. Podsieć fronton hostingu maszyny działającego jako serwer sieci web, która będzie dostępna z Internetu.
    - **azsn3**. Podsieć wewnętrznej bazy danych hostingu maszyny uruchamiania serwera aplikacji wewnętrznej bazy danych, który będzie dostępna przez serwer frontonu sieci web.
    - **azsn4**. Podsieć zarządzania wyłącznie umożliwiające uzyskanie dostępu do funkcji zarządzania do wszystkich urządzeń wirtualnych zapory. Danej podsieci zawiera tylko NIC dla każdego urządzenia wirtualnego zapory używany w rozwiązaniu.
    - **GatewaySubnet**. Podsieć połączenia Azure hybrydowych wymagane na potrzeby ExpressRoute i Brama VPN zapewniające łączność między Azure VNets i innych sieci. 
- Istnieją 3 zapory urządzenia wirtualnej sieci **azurevnet** . 
    - **AZF1**. Zapory zewnętrznej poddana publicznego Internetu za pomocą publicznej zasobu adresu IP w Azure. Należy upewnić się, że masz szablonu z witryny Marketplace lub bezpośrednio z dostawcą urządzenia, że przepisy urządzenia wirtualnego 3-NIC.
    - **AZF2**. Umożliwia wybranie ruch między **azsn2** i **azsn3**wewnętrznych zapory. Jest to także urządzenia wirtualnego 3-NIC.
    - **AZF3**. Zarządzanie Zapora dostępne dla administratorów w centrum danych lokalnych i połączony z podsieci zarządzania służące do zarządzania wszystkie urządzenia zapory. Znajdowanie szablonów urządzenia wirtualnego 2-NIC na rynku lub zażądać bezpośrednio z dostawcą urządzenia.

## <a name="user-defined-routing-udr"></a>Kierowanie (UDR) zdefiniowane przez użytkownika

Każda podsieć platformy Azure można połączyć z tabeli UDR używane do określania, jak ruch zainicjowana w tym rozesłaniu podsieci. Jeśli UDRs nie zdefiniowano, Azure używa trasy domyślne, aby umożliwić przepływ ruchu z jednej podsieci do innej. Aby lepiej zrozumieć UDRs, odwiedź stronę [Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP](./virtual-networks-udr-overview.md#ip-forwarding).

Aby upewnić się, że komunikacji to zrobić za pomocą urządzenia prawidłowa zapora, oparte na ostatnie wymaganie powyżej, należy utworzyć w poniższej tabeli rozsyłania zawierające UDRs w **azurevnet**.

### <a name="azgwudr"></a>azgwudr

W tym scenariuszu tylko ruch wynikających z lokalnego Azure będzie używana do zarządzania zapory przez nawiązanie połączenia z **AZF3**, a ruch należy przejść przez zaporę wewnętrznych, **AZF2**. Dlatego tylko jedna droga, należy **GatewaySubnet** , tak jak pokazano poniżej.

|Miejsce docelowe|Następny skok|Wyjaśnienie|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Umożliwia ruchu lokalnego osiągnięcia zapory zarządzania **AZF3**|

### <a name="azsn2udr"></a>azsn2udr

|Miejsce docelowe|Następny skok|Wyjaśnienie|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Zezwala na ruch do obsługi serwera aplikacji za pomocą **AZF2** podsieci wewnętrznej bazy danych|
|0.0.0.0/0|10.0.2.10|Zezwala na cały ruch przesłana za pośrednictwem **AZF1**|

### <a name="azsn3udr"></a>azsn3udr

|Miejsce docelowe|Następny skok|Wyjaśnienie|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Umożliwia ruchu **azsn2** przepływ z serwera aplikacji do serwer_sieci_Web za pośrednictwem **AZF2**|

Ponadto potrzebny do tworzenia tabel routingu dla podsieci w **onpremvnet** symulowanie centrum danych lokalnych.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Miejsce docelowe|Następny skok|Wyjaśnienie|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Umożliwia ruchu **onpremsn2** za pośrednictwem **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Miejsce docelowe|Następny skok|Wyjaśnienie|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Umożliwia ruchu kopii podsieci platformy Azure za pośrednictwem **OPFW**|
|192.168.1.0/24|192.168.2.4|Umożliwia ruchu **onpremsn1** za pośrednictwem **OPFW**|

## <a name="ip-forwarding"></a>Przesyłanie dalej adresów IP 

UDR i przekazywanie adresów IP są funkcje, których można używać w połączeniu, aby umożliwić wirtualnych wyposażenia, można używać do sterowania ruch w VNet Azure.  Wirtualna urządzenia ma więcej niż maszyny uruchamiana aplikacja używana do obsługi ruchu sieciowego w określony sposób, na przykład zapora lub urządzenie NAT.

Wirtualna urządzenie maszyn wirtualnych muszą mieć możliwość odbierania ruchu przychodzącego, nie zaadresowany do siebie. Aby umożliwić maszyn wirtualnych do odbierania ruchu skierowana w inne miejsce, należy włączyć przekazywanie adresów IP dla maszyn wirtualnych. To jest Azure ustawienie nie ustawienia w systemie operacyjnym gościa. Urządzenia wirtualnego nadal musi obsługiwać różnego rodzaju aplikacji do obsługi ruchu przychodzącego i odpowiednio go rozesłać.

Aby dowiedzieć się więcej na temat przekazywania IP, odwiedź stronę [Co to są trasy zdefiniowane przez użytkownika i przekazywanie adresów IP](./virtual-networks-udr-overview.md#ip-forwarding).

Na przykład załóżmy, że w Azure vnet są następujące ustawienia:

- Podsieć **onpremsn1** zawiera maszyny o nazwie **onpremvm1**.
- Podsieć **onpremsn2** zawiera maszyny o nazwie **onpremvm2**.
- Wirtualna urządzenia o nazwie **OPFW** jest połączony z **onpremsn1** i **onpremsn2**.
- Trasa zdefiniowane przez użytkownika, połączone z **onpremsn1** Określa, że cały ruch do **onpremsn2** musi być wysyłane do **OPFW**.

W tym momencie Jeśli **onpremvm1** próbuje nawiązać połączenie z **onpremvm2**, zostanie użyty UDR i ruch będą wysyłane do **OPFW** jako następnego przeskoku. Należy pamiętać, że nie są zmieniane miejsca docelowego pakietu rzeczywiste, nadal wskazanego **onpremvm2** jest miejscem docelowym. 

Bez przekazywanie adresów IP dla **OPFW**Azure wirtualnej sieci logiczny powoduje usunięcie pakiety, ponieważ tylko umożliwia pakiety były wysyłane do maszyny, jeśli adres IP maszyn wirtualnych jest miejsce docelowe pakietu.

Z przekazywanie adresów IP logiczny Azure wirtualną sieć będzie przesyłać pakiety OPFW, bez zmieniania jej oryginalny adres docelowy. **OPFW** musi obsługiwać pakietów i określanie, co zrobić z nich.

Scenariusz powyżej do pracy, należy włączyć przekazywanie adresów IP na sieciowe **OPFW**, **AZF1**, **AZF2**i **AZF3** , które są używane do routingu (wszystkie nic z wyjątkiem pól połączone z podsieci zarządzania). 

## <a name="firewall-rules"></a>Reguły zapory

Zgodnie z powyższym opisem IP przekazywanie tylko gwarantuje, że pakiety są wysyłane do wirtualnego urządzeń. Urządzenia nadal wymaga zdecydować, co zrobić z tych pakietów. Scenariusz powyżej należy utworzyć następujące reguły w urządzeń:

### <a name="opfw"></a>OPFW

OPFW reprezentuje urządzenie lokalnego zawierające następujące reguły:

- **Trasa**: cały ruch do 10.0.0.0/16 (**azurevnet**) muszą być wysyłane za pośrednictwem tunelem **ONPREMAZURE**.
- **Zasady**: Zezwalaj na cały ruch dwukierunkowa między **port2** i **ONPREMAZURE**.
 
### <a name="azf1"></a>AZF1

AZF1 reprezentuje Azure urządzenia wirtualny zawierający następujące reguły:

- **Zasady**: Zezwalaj na cały ruch dwukierunkowa między **port1** oraz **port2**.

### <a name="azf2"></a>AZF2

AZF2 reprezentuje Azure urządzenia wirtualny zawierający następujące reguły:

- **Trasa**: cały ruch do 10.0.0.0/16 (**onpremvnet**) muszą być wysyłane na adres IP Azure bramy (to znaczy 10.0.0.1) za pośrednictwem **port1**.
- **Zasady**: Zezwalaj na cały ruch dwukierunkowa między **port1** oraz **port2**.

## <a name="network-security-groups-nsgs"></a>Grupy zabezpieczeń sieci (NSGs)

W tym scenariuszu NSGs są nie jest używany. Jednak można zastosować NSGs do każdej podsieci, aby ograniczyć ruch przychodzący i wychodzący. Na przykład można zastosować następujące reguły NSG do zewnętrznych podsieci FW.

**Przychodzące**

- Zezwól cały ruch TCP z Internetu na porcie 80 na dowolnym maszyn wirtualnych w podsieci.
- Odmów wszystkich innych rodzajów ruchu z Internetu.

**Wychodzące**
- Zezwalaj tylko na pakiety z Internetem.

## <a name="high-level-steps"></a>Wysoki poziom czynności

Aby wdrożyć w tym scenariuszu, wykonaj poniższe czynności wysokiego poziomu.

1.  Zaloguj się do subskrypcji usługi Azure.
2.  Jeśli chcesz wdrożyć VNet symulowanie sieci lokalnej Zapewnij zasoby, które są częścią **ONPREMRG**.
3.  Zapewnij zasoby, które są częścią **AZURERG**.
4.  Zapewnienie tunelem z **onpremvnet** do **azurevnet**.
5.  Gdy zainicjowano obsługę administracyjną wszystkie zasoby, zaloguj się do **onpremvm2** i ping 10.0.3.101, aby przetestować łączność między **onpremsn2** i **azsn3**.
