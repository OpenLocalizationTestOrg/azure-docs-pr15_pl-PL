<properties 
    pageTitle="Jak kontrolować, ruch przychodzący w środowisku aplikacji usługi" 
    description="Dowiedz się, jak skonfigurować reguły zabezpieczeń sieciowych do sterowania ruchu przychodzącego w środowisku usługi aplikacji." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Jak kontrolować, ruch przychodzący w środowisku aplikacji usługi

## <a name="overview"></a>Omówienie ##
Środowisko usługi aplikacji można utworzyć w **obu** wirtualną sieć Azure Menedżera zasobów, **lub** klasycznym wdrożeniu modelu [wirtualnej sieci][virtualnetwork].  Nowe wirtualnej sieci i Nowa podsieć można zdefiniować w czasie utworzone środowisku usługi aplikacji.  Możesz też środowisku usługi aplikacji można utworzyć w istniejącym wirtualnej sieci i podsieci istniejącego.  Z ostatnich zmian w czerwcu 2016 ASEs teraz mogą być rozmieszczone w wirtualnych sieci korzystające z zakresów publiczny adres lub obszar adresów RFC1918 (to znaczy adresy prywatne).  Aby uzyskać więcej informacji na temat tworzenia w środowisku usługi aplikacji zobacz [jak tworzenie środowisku usługi aplikacji][HowToCreateAnAppServiceEnvironment].

Środowisko usługi aplikacji zawsze muszą zostać utworzone w podsieci, ponieważ podsieci zapewnia granicy w sieci, które mogą być używane do blokowania ruchu przychodzącego za nadrzędny urządzenia i usługi tak, aby ruch HTTP i HTTPS jest dopuszczalne tylko z określonej nadrzędny adresów IP.

Ruch przychodzący i wychodzący ruch sieciowy podsieci sterują [grupy zabezpieczeń sieci][NetworkSecurityGroups]. Sterowanie ruchu przychodzącego wymaga Tworzenie zasad zabezpieczeń sieci w grupie zabezpieczeń sieci, a następnie przypisując zabezpieczeń sieci grupowanie podsieci zawierającej środowisko usługi aplikacji.

Gdy grupy zabezpieczeń sieci jest przypisany do podsieci, ruch przychodzący do aplikacji w środowisku usługi aplikacji jest dozwolonych/zablokowanych według Zezwalaj i odmówić reguł zdefiniowanych w grupie zabezpieczeń sieci.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Porty sieciowe wykorzystywane w środowisku usługi aplikacji ##
Przed blokowania przychodzącego ruchu sieciowego z grupy zabezpieczeń sieci, jest ważne zestaw portów wymaganych i opcjonalnych sieci używane przez środowisku usługi aplikacji.  Przypadkowe zamknięcia poza ruch do niektórych portów może spowodować utratę funkcjonalności w środowisku usługi aplikacji.

Poniżej przedstawiono listę porty używane przez środowisku usługi aplikacji. Wszystkie porty są **TCP**, chyba że wyraźnie inaczej:

- 454: **wymagane portu** używane przez Azure infrastruktury do zarządzania i konserwacji aplikacji środowiska usługi za pośrednictwem protokołu SSL.  Nie należy blokować ruch do tego portu.  Ten port jest zawsze związane z publicznej VIP części ASE.
- 455: **wymagane portu** używane przez Azure infrastruktury do zarządzania i konserwacji aplikacji środowiska usługi za pośrednictwem protokołu SSL.  Nie należy blokować ruch do tego portu.  Ten port jest zawsze związane z publicznej VIP części ASE.
- 80: domyślny port dla ruchu przychodzącego protokołu HTTP do aplikacji działa w aplikacji usługi plany w środowisku usługi aplikacji.  Na ASE z włączoną obsługą ILB ten port jest związany z adresu ILB ASE.
- 443: domyślny port dla ruchu przychodzącego protokołu SSL do aplikacji działa w aplikacji usługi plany w środowisku usługi aplikacji.  Na ASE z włączoną obsługą ILB ten port jest związany z adresu ILB ASE.
- 21: Kanał sterowania FTP.  Ten port można zablokować bezpieczne, jeśli nie jest używany FTP.  Na ASE z włączoną obsługą ILB ten port może być powiązany adres ILB dla ASE.
- 990: kontrola kanał FTPS.  Ten port można zablokować bezpieczne, jeśli nie jest używany FTPS.  Na ASE z włączoną obsługą ILB ten port może być powiązany adres ILB dla ASE.
- 10001 10020: kanałów danych FTP.  Podobnie jak w przypadku kanału kontroli porty te mogą zostać bezpiecznie zablokowane Jeśli FTP nie jest używany.  Na ASE z włączoną obsługą ILB ten port może być powiązany adres ILB ASE.
- 4016: na potrzeby zdalnego debugowania w programie Visual Studio 2012.  Ten port można zablokować bezpieczne, jeśli nie jest używana funkcja.  Na ASE z włączoną obsługą ILB ten port jest związany z adresu ILB ASE.
- 4018: na potrzeby zdalnego debugowania w programie Visual Studio 2013.  Ten port można zablokować bezpieczne, jeśli nie jest używana funkcja.  Na ASE z włączoną obsługą ILB ten port jest związany z adresu ILB ASE.
- 4020: na potrzeby zdalnego debugowania za pomocą programu Visual Studio 2015 r.  Ten port można zablokować bezpieczne, jeśli nie jest używana funkcja.  Na ASE z włączoną obsługą ILB ten port jest związany z adresu ILB ASE.

## <a name="outbound-connectivity-and-dns-requirements"></a>Połączenia wychodzące i wymagania systemu DNS ##
Dla środowisku aplikacji usługi poprawnego wymaga ruchu wychodzącego dostęp do różnych punktów końcowych. Pełną listę zewnętrznych punktów końcowych, używane przez ASE znajduje się w sekcji "Wymagana łączność sieciowa" artykułu [Konfiguracja sieciowa dla ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Środowiska usługi aplikacji wymaga prawidłowych infrastruktury DNS skonfigurowane dla wirtualnej sieci.  Zmiana dowolnego powodu konfigurację DNS po utworzeniu środowisku usługi aplikacji, deweloperzy można wymusić środowisku usługi aplikacji do pobrania nowa konfiguracja DNS.  Powodujące stopniowe środowiska Uruchom ponownie komputer za pomocą ikony "Uruchom ponownie" znajduje się w górnej części karta zarządzania środowisko usługi aplikacji w [Azure portal] [ NewPortal] spowoduje, że środowisko do pobrania nowa konfiguracja DNS.

Zalecane jest również, że wszelkie niestandardowe serwery DNS na vnet można zainstalować wyprzedzeniem przed utworzeniem środowisku usługi aplikacji.  Konfiguracja DNS wirtualną sieć zostanie zmieniona, podczas tworzenia środowiska usługi aplikacji, który spowoduje awarię proces tworzenia środowiska usługi aplikacji.  W podobny szyjnej Jeśli istnieje niestandardowe serwera DNS po drugiej stronie bramy sieci VPN i serwera DNS jest nieosiągalny lub niedostępne, proces tworzenia środowiska usługi aplikacji również nie powiedzie się.

## <a name="creating-a-network-security-group"></a>Tworzenie grupy zabezpieczeń sieci ##
Aby uzyskać szczegółowe informacje na sposób działania grup zabezpieczeń sieci zobacz następujące [informacje][NetworkSecurityGroups].  Szczegóły poniżej dotykowe na wyróżnienie grup zabezpieczeń sieci, z fokusem na konfigurowanie i stosowanie grupy zabezpieczeń sieci podsieci, która zawiera środowisku usługi aplikacji.

**Uwaga:** Grupy zabezpieczeń sieci można skonfigurować graficznie za pomocą [Azure Portal](https://portal.azure.com) lub przy użyciu programu PowerShell Azure.

Grupy zabezpieczeń sieci najpierw są tworzone jako osoba prowadząca autonomicznego skojarzonego z subskrypcją. Ponieważ grupy zabezpieczeń sieci są tworzone w regionie Azure, upewnij się, utworzenia grupy zabezpieczeń sieci w tym samym regionie jako środowisko usługi aplikacji.

Poniżej przedstawiono, tworzenie grupy zabezpieczeń sieci:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Po utworzeniu grupy zabezpieczeń sieci jedną lub więcej reguł zabezpieczeń sieci zostaną dodane do niego.  Ponieważ zestaw reguł może się zmienić w czasie zalecane jest Rozmieść schemacie numerowania używanej do priorytety reguły w celu ułatwienia wstawić dodatkowe reguły w czasie.

W poniższym przykładzie pokazano reguły jawnie dają dostęp do porty zarządzania wymagane przez Azure infrastruktury zarządzania i utrzymują środowisko usługi aplikacji.  Zauważ, że cały ruch zarządzania przepływał przez SSL i jest zabezpieczone certyfikatów klientów, nawet jeśli są otwarte porty są niedostępne przez osobę inną niż infrastruktura zarządzania Azure.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Po zablokowaniu dostępu do portu 80 i 443 "ukryć" środowisku usługi aplikacji za urządzenia nadrzędnego lub usługi, należy znać nadrzędny adres IP.  Na przykład, jeśli korzystasz z zapory aplikacji sieci web (WAF), WAF ma własny adres IP (lub adresy), których używa podczas buforowania ruchu za środowisko usługi aplikacji.  Należy użyć tego adresu IP w parametrze *SourceAddressPrefix* reguły zabezpieczeń sieci.

W poniższym przykładzie ruch przychodzący z określonego adresu IP nadrzędny jawnie jest dozwolone.  Adres *1.2.3.4* jest używany jako symbol zastępczy dla adresu IP nadrzędny WAF.  Zmień wartość na zgodny z adresem używane przez urządzenia nadrzędnego lub usługę.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
W razie potrzeby obsługę FTP następujących reguł może służyć jako szablon do udzielania dostępu do danych i port sterowania FTP porty kanału.  Ponieważ FTP jest stanowe Protocol (protokół), nie można skierować ruch FTP przy użyciu protokołu HTTP/HTTPS tradycyjnych urządzenia zapory lub serwera proxy.  W tym przypadku należy ustawić *SourceAddressPrefix* na inną wartość — na przykład zakres adresów IP deweloper lub wdrożenia komputerów, na które FTP klientach jest uruchomiony. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Uwaga:** zakresie portów kanału danych może się zmienić w okresie podglądu.)

Jeśli zastosowano zdalnego debugowania za pomocą programu Visual Studio następujących reguł pokazano, jak udzielić dostępu.  Istnieje osobne regułę dla każdej obsługiwanej wersji programu Visual Studio, ponieważ w każdej wersji używa innego portu zdalne debugowanie.  Podobnie jak w przypadku dostępu FTP zdalny ruch debugowania może nie odzwierciedlać prawidłowo za pośrednictwem tradycyjnych WAF lub urządzenia serwera proxy.  *SourceAddressPrefix* Ustaw zamiast tego zakres adresów IP Deweloper komputerów z systemem Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Przypisywanie grupy zabezpieczeń sieci do podsieci ##
Grupy zabezpieczeń sieci ma domyślne reguły zabezpieczeń, który uniemożliwia dostęp do całego ruchu zewnętrznych.  Wynik z łączenie zasad zabezpieczeń sieci opisanych powyżej i reguły zabezpieczeń domyślne blokowanie ruchu przychodzącego jest tylko ruch z adresu źródłowego, które zakresy skojarzony z akcją *zezwalania* będą mogli przesyłać dane do aplikacji w środowisku usługi aplikacji.

Po grupy zabezpieczeń sieci zostanie wypełniona reguły zabezpieczeń, musi być przypisane do podsieci zawierającej środowisko usługi aplikacji.  Polecenie przydziału odwołuje się zarówno nazwę wirtualnej sieci, w którym znajduje się środowisko usługi aplikacji, a także nazwę podsieci, w której utworzono środowisko usługi aplikacji.  

W poniższym przykładzie pokazano grupę zabezpieczeń sieci jest przypisana do podsieci i wirtualną sieć:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Po pomyślnym przypisania grupy zabezpieczeń sieci (przydziału jest długotrwałych operacji i może potrwać kilka minut), tylko ruchu przychodzącego *Zezwalaj* reguł dopasowania zostanie pomyślnie osiągnięcia aplikacji w środowisku usługi aplikacji.

Dla kompletności w poniższym przykładzie pokazano, jak usunąć i dlatego dis-skojarzyć grupy zabezpieczeń sieci z podsieci:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Uwagi dotyczące jawne IP protokołu SSL ##
Jeśli aplikację skonfigurowano jawne SSL IP adres (dotyczy ASEs mające tylko publicznej VIP), zamiast adres IP domyślnej aplikacji środowiska usługi HTTP i HTTPS ruch jest przenoszony do danej podsieci na inny zestaw porty inne niż porty 80 i 443.

Poszczególne pary porty używane przez każdego adresu IP SSL można znaleźć w interfejsie użytkownika portalu z karta interfejsu użytkownika szczegóły środowisko usługi aplikacji.  Wybierz pozycję "wszystkie ustawienia"--> "Adresy IP".  Karta "Adresy IP" Pokazuje tabelę wszystkich adresów IP SSL skonfigurowanego specjalnie dla środowiska usługi aplikacji, wraz z pary port specjalny, który jest używany aby skierować ruch HTTP i HTTPS skojarzone z każdego adresu IP SSL.  Jest to pary portu, który musi być używana dla parametrów DestinationPortRange, podczas konfigurowania reguły w grupie zabezpieczeń sieci.

Po aplikacji w ASE jest skonfigurowany do używania protokołu SSL IP, klienci zewnętrzni, którzy nie będą widoczne i nie trzeba się przejmować mapowania pary port specjalny.  Ruch do aplikacji będzie przepływał zwykle na adres IP SSL skonfigurowany.  Tłumaczenie do sparowania port specjalny automatycznie dzieje się wewnętrznie podczas ostatnim etapie routingu ruchu do podsieci zawierającej ASE. 

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji][IntroToAppServiceEnvironment]

Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby korzystać z aplikacji bezpiecznego połączenia do zasobu wewnętrznej bazy danych w środowisku usługi aplikacji zobacz [bezpieczne połączenie z zasobami wewnętrznej bazy danych w środowisku usługi aplikacji][SecurelyConnecttoBackend]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
