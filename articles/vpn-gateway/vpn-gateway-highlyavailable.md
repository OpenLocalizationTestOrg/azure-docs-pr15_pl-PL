<properties
   pageTitle="Omówienie wysokiej dostępności konfiguracji bramy sieci VPN Azure | Microsoft Azure"
   description="W tym artykule omówiono wysoce dostępnych opcji konfiguracji przy użyciu bram VPN Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Wysoce dostępne lokalnej infrastrukturze i łącznością VNet do VNet

W tym artykule omówiono wysoce dostępnych opcji konfiguracji sieci lokalnej krzyżowe i VNet do VNet łączność przy użyciu bram Azure VPN.

## <a name = "activestandby"></a>Informacje o nadmiarowości brama Azure VPN

Co brama Azure VPN składa się z dwóch wystąpień w konfiguracji wstrzymania aktywne. Dla każdego planowanej konserwacji lub zakłóceń niezaplanowane, która aktywne wystąpienia wstrzymania wystąpienie przejąć (funkcją przełączania awaryjnego) automatycznie i wznowienie połączenia VNet do VNet lub S2S VPN. Przełączanie na spowoduje, że krótkie przerwy. Dla planowanej konserwacji łączność należy je przywrócić w ciągu 10 lub 15 sekund. Niezaplanowane problemów odzyskiwania połączenia będą dłużej, 1 i pół minut w przypadku najgorszego około 1 minuty. Dla połączeń klienta P2S VPN do bramy połączenia P2S zostanie odłączona, a użytkownicy będą musieli ponownie nawiązać połączenie z komputerów klienckich.

![Aktywne gotowości](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Łączność wysokiej dostępności lokalnej infrastrukturze

Aby zapewnić lepszą dostępność połączenia krzyżowego lokalnej, dostępnych jest kilka opcji:

- Wielu urządzeń VPN lokalnego
- Brama Azure VPN aktywny aktywny
- Kombinacja

### <a name = "activeactiveonprem"></a>Wielu urządzeń VPN lokalnego

Umożliwia wielu urządzeń sieci VPN z sieci lokalnej nawiązać połączenie bramy sieci Azure VPN, jak pokazano na poniższym rysunku:

![Wiele lokalnego VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Ta konfiguracja zawiera wiele aktywnych tuneli z tej samej bramie Azure VPN z urządzeniami lokalnego w tej samej lokalizacji. Istnieje kilka wymagania i ograniczenia:

1. Należy utworzyć wiele połączeń S2S VPN na swoich urządzeniach VPN Azure. Po podłączeniu wielu urządzeń sieci VPN z tej samej sieci lokalnej Azure potrzebne do utworzenia jednej bramy sieci lokalnej dla każdego urządzenia VPN i jedno połączenie z bramy sieci Azure VPN w taki sposób, aby brama sieci lokalnej.

2. Bram sieci lokalnej odpowiadające urządzenia VPN musi mieć unikatowy publicznych adresów IP we właściwości "GatewayIpAddress".

3. BGP jest wymagany dla tej konfiguracji. Każdej bramy sieci lokalnej reprezentujące urządzenie VPN musi mieć unikatowy BGP równorzędnych adres IP określona dla właściwości "BgpPeerIpAddress".

4. Pole właściwości AddressPrefix w każdej bramy sieci lokalnej nie musi zachodzić. Należy określić "BgpPeerIpAddress" w /32 CIDR format w polu AddressPrefix, na przykład 10.200.200.254/32.

5. Należy używać BGP do ogłaszanie samej prefiksy tej samej sieci prefiksy do bramy sieci Azure VPN lokalnego i dane będą przekazywane przez te tunel jednocześnie.

6. Każdego połączenia są wliczane maksymalna liczba tuneli dla bramy sieci Azure VPN, 10 dla Basic i standardowej wersji produktu, a 30 SKU o odwróconej. 

W tej konfiguracji bramy Azure VPN jest nadal w trybie gotowości aktywne, w tym samym awaryjna i krótkie przerwy będzie wykonane jak opisano [powyżej](#activestandby). Ale to ta konfiguracja chroni przed awariami systemów bądź zakłócenia w sieci lokalnej i urządzeń sieci VPN.
 
### <a name="active-active-azure-vpn-gateway"></a>Brama Azure VPN aktywny aktywny

Teraz można utworzyć bramy Azure VPN w konfiguracji aktywny aktywny, miejsce, w którym oba wystąpienia maszyny wirtualne bramy ustalają tuneli S2S VPN do lokalnego urządzenia VPN, jak pokazano na poniższej ilustracji:

![Aktywny aktywny](./media/vpn-gateway-highlyavailable/active-active.png)

W tej konfiguracji każdego wystąpienia Azure bramy będą mieć unikatowy publiczny adres IP, i każdy określi tunelem VPN S2S IPsec/IKE urządzenia VPN lokalnego określonych w swojej sieci lokalnej bramy i połączenia. Zauważ, że oba tuneli VPN są faktyczną część tego samego połączenia. Konieczne będzie nadal Skonfiguruj urządzenie VPN lokalnego na do zaakceptowania lub ustanawiać dwóch tunele S2S VPN do tych dwóch Azure VPN bramy publicznych adresów IP.

Ponieważ wystąpień bram Azure znajdują się w konfiguracji aktywny aktywny, ruchu z usługi Azure wirtualną sieć do swojej sieci lokalnej będą kierowane do obu tunele jednocześnie, nawet wtedy, gdy urządzenia VPN lokalnego może preferować w pełni tunelem jeden na drugi. Uwaga Jeśli samego przepływu TCP i UDP będzie zawsze przechodzenie przez tego samego tunelem lub ścieżkę, chyba że konserwacji zdarzenie w jednym z wystąpień.

Sytuacji planowanej konserwacji lub niezaplanowane wydarzenia wystąpieniu jednej bramy, zostanie rozłączone tunelem IPsec z tego wystąpienia urządzenia VPN lokalnego. Odpowiednie trasy na urządzeniach VPN zostaje usunięty lub wycofane automatycznie tak, aby dane zostaną przełączono do innych aktywne tunelem IPsec. Na stronie Azure Przełącz na zostanie przeprowadzona automatycznie z dotyczy wystąpienia do aktywnego wystąpienia.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Nadmiarowości narożnej: aktywny aktywny VPN bram sieciach zarówno Azure i lokalnych

Najbezpieczniejszym rozwiązaniem jest łączenie bram aktywny aktywny w sieci i Azure, jak pokazano na poniższej ilustracji.

![Zrzut ekranu przedstawiający dwie nadmiarowości](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

W tym miejscu tworzenie i konfigurowanie bramy Azure VPN w konfiguracji aktywny aktywny i Tworzenie bramy sieci lokalnej i dwa połączenia z dwóch lokalnego urządzenia VPN zgodnie z powyższym opisem. Wynik to z łącznością Pełna sieć 4 tuneli IPsec między Azure wirtualnej sieci i sieci lokalnej.

Wszystkich bram i tunele są aktywne ze strony Azure, aby dane zostaną rozłożone między wszystkie tunele 4 jednocześnie, mimo że każdego przepływu TCP i UDP zostanie ponownie wykonaj tunelem samego lub ścieżkę, ze strony Azure. Mimo że się dane, może wyświetlić nieco większa przepustowość nad tuneli IPsec, podstawowym celem tej konfiguracji jest wysokiej dostępności. I z powodu statystyczne charakter rozmieszczanie, trudno Podaj miar na różnych warunków ruchu aplikacji wpłynie na przepustowość agregacji.

Ta topologia wymaga dwie bramy sieci lokalnej oraz dwa połączenia do obsługi parze VPN lokalnego i BGP jest wymagane do zezwalania na połączenia dwóch do tej samej sieci lokalnej. Te wymagania są takie same jak [powyżej](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Wysoce dostępne VNet do VNet połączenia Azure VPN bram

Tej samej konfiguracji aktywny aktywny można także zastosować do połączeń Azure VNet-VNet. Tworzenie bramy sieci VPN aktywny aktywny dla obu wirtualnych sieci i łączenia ich do formularza samej łączności Pełna sieć 4 tuneli między dwoma VNets, jak pokazano na poniższej ilustracji:

![VNet do VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Dzięki temu są zawsze para tuneli między dwoma wirtualnych sieci wszystkie zdarzenia planowanej konserwacji, dostarczając jeszcze lepiej dostępności. Mimo że sam topologii dla między lokalnym łączności wymaga dwóch połączeń topologii VNet do VNet, jak pokazano powyżej muszą tylko jedno połączenie dla każdej bramy. Ponadto BGP jest opcjonalny, jeśli wymagane jest przewozowe routing przez połączenia VNet do VNet.


## <a name="next-steps"></a>Następne kroki

Zobacz [Konfigurowanie bramy sieci VPN aktywny-aktywny dla lokalnego krzyżowe i VNet do VNet połączenia](vpn-gateway-activeactive-rm-powershell.md) kroki konfigurowania aktywny aktywny lokalnej infrastrukturze i VNet do VNet połączenia.
