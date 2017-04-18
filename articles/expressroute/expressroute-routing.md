<properties
   pageTitle="Kierowanie wymagania dotyczące ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera szczegółowe wymagania dotyczące konfigurowania i zarządzania nimi routingu dla obwodów ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute wymagań dotyczących routingu  

Aby połączyć się z usługami w chmurze firmy Microsoft przy użyciu ExpressRoute, musisz skonfigurować i zarządzać nimi, routing. Niektórych dostawców łączności oferują konfigurowaniu i zarządzaniu nimi routingu jako zarządzanych usług. Skontaktuj się z dostawcy połączenia, jeśli oferują tej usługi. Jeśli nie, muszą spełniać następujące wymagania. 

Zapoznaj się z artykułem [obwody elektryczne i układy domen routingu](expressroute-circuit-peerings.md) Opis routingu sesje, które muszą zostać skonfigurowane celu ułatwienia łączności.

>[AZURE.NOTE] Microsoft nie obsługuje protokołów nadmiarowości router (na przykład HSRP, VRRP) w przypadku konfiguracji wysokiej dostępności. Firma Microsoft oparte na parze nadmiarowych sesji BGP na zaglądanie wysokiej dostępności.

## <a name="ip-addresses-used-for-peerings"></a>Adresy IP używane dla peerings

Należy zarezerwować kilka bloków adresów IP, aby skonfigurować routing między siecią i routery krawędzi (MSEEs) przedsiębiorstwa firmy Microsoft. W tej sekcji przedstawia wykaz wymagań oraz zasady dotyczące jak te adresy IP muszą być nabyte i używane.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Adresy IP używane dla Azure zaglądanie prywatnych

Aby skonfigurować peerings służy prywatnych adresów IP lub publicznych adresów IP. Zakres adresów używany do konfigurowania trasy nie nakładają się na z zakresów adresów służy do tworzenia wirtualnych sieci w Azure. 

 - Należy zarezerwować /29 podsieci lub podsieci dwóch /30 dla interfejsów routingu.
 - Podsieci używane do przesyłania może być prywatnych adresów IP lub publicznych adresów IP.
 - Podsieci nie może powodować konfliktu z zakresem zarezerwowana przez klienta do użytku w chmurze firmy Microsoft.
 - Jeśli /29 służy podsieci, będzie można podzielić na dwie /30 podsieci. 
     - Pierwszy /30 podsieci będzie używana dla łącze podstawowe, a drugi/30 podsieci będzie używana dla pomocniczej łącze.
     - Dla każdej /30 podsieci, należy użyć pierwszego adresu IP /30 podsieci na routera. Firma Microsoft będzie używać drugiego adresu IP /30 podsieci konfigurowania sesji BGP.
     - Musisz skonfigurować zarówno sesje BGP naszych [dostępność Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/) jest nieprawidłowy.  

#### <a name="example-for-private-peering"></a>Przykład zaglądanie prywatnych

Jeśli wybierzesz umożliwia konfigurowanie zaglądanie a.b.c.d/29, będzie można podzielić na dwa /30 podsieci. W poniższym przykładzie firma Microsoft będzie wyglądać na używania podsieci a.b.c.d/29. 

a.b.c.d/29 zostaną dzielenie a.b.c.d/30 i a.b.c.d+4/30 i przekazywane w dół do firmy Microsoft za pośrednictwem API obsługi administracyjnej. Dla podstawowego PE użyje a.b.c.d+1 jako VRF IP i Microsoft zajmie a.b.c.d+2 jako IP VRF dla podstawowego MSEE. Dla pomocniczej PE użyje a.b.c.d+5 jako VRF IP i Microsoft użyje a.b.c.d+6 jako VRF IP MSEE pomocniczą.

Należy rozważyć przypadek gdzie wybierz 192.168.100.128/29 skonfigurować zaglądanie prywatne. 192.168.100.128/29 zawiera adresy z 192.168.100.128 192.168.100.135, do których:

- 192.168.100.128/30 są przypisywane do link1, z dostawcą przy użyciu 192.168.100.129 i firmy Microsoft przy użyciu 192.168.100.130.
- 192.168.100.132/30 są przypisywane do Łącze2, z dostawcą przy użyciu 192.168.100.133 i firmy Microsoft przy użyciu 192.168.100.134.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>Adresy IP używane dla Azure publicznych i zaglądanie firmy Microsoft

Należy użyć publicznych adresów IP, których jesteś właścicielem dotyczące konfigurowania sesji BGP. Microsoft muszą mieć możliwość weryfikowanie własności adresów IP za pośrednictwem routingu rejestrów Internet i rejestry routingu Internet. 

- Należy użyć unikatowego/29 podsieci lub /30 dwóch podsieci, aby skonfigurować BGP zaglądanie dla każdego zaglądanie na ExpressRoute elektrycznego (Jeśli masz więcej niż jedno). 
- Jeśli /29 służy podsieci, będzie można podzielić na dwie /30 podsieci. 
    - Pierwszy /30 podsieci będzie używana dla łącze podstawowe, a drugi/30 podsieci będzie używana dla pomocniczej łącze.
    - Dla każdej /30 podsieci, należy użyć pierwszego adresu IP /30 podsieci na routera. Firma Microsoft będzie używać drugiego adresu IP /30 podsieci konfigurowania sesji BGP.
    - Musisz skonfigurować zarówno sesje BGP naszych [dostępność Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/) jest nieprawidłowy.

## <a name="public-ip-address-requirement"></a>Wymaganie adres IP publicznej 

### <a name="private-peering"></a>Zaglądanie prywatnych 

Możesz używać publiczny czy prywatny adresy IP protokołu IPv4 dla zaglądanie prywatne. Firma Microsoft udostępnia izolacji zakończenia do końca ruchu, nakładające się adresów z innymi klientami nie jest możliwe w przypadku zaglądanie prywatne. Te adresy nie są ogłaszane na Internet. 

### <a name="public-peering"></a>Zaglądanie publicznej

Azure publicznej ścieżkę peering umożliwia łączenie się z usługami wszystkie obsługiwane platformy Azure na ich publicznych adresów IP. Należą do usług wymienionych w sekcji [Często zadawane pytania dotyczące ExpessRoute](expressroute-faqs.md) i usług obsługiwanych przez niezależnych dostawców oprogramowania firmy Microsoft Azure. Nawiązywanie połączenia usługi Microsoft Azure na publicznej zaglądanie jest zawsze inicjowane z sieci do sieci firmy Microsoft. Należy użyć publiczne adresy IP dla ruchu przeznaczonego dla sieci firmy Microsoft.

### <a name="microsoft-peering"></a>Zaglądanie firmy Microsoft

Ścieżka peering firmy Microsoft umożliwia nawiązywanie połączenia z usługami firmy Microsoft w chmurze, które nie są obsługiwane przez Azure publicznej ścieżkę peering. Na liście usług obejmuje usługi Office 365, takie jak usługi Exchange Online, usłudze SharePoint Online, w programie Skype dla firm i CRM Online. Firma Microsoft obsługuje dwukierunkową łączność na zaglądanie firmy Microsoft. Mogą wejść do sieci Microsoft ruchu przeznaczonego dla usług w chmurze firmy Microsoft, należy użyć prawidłowych publicznych adresów IP protokołu IPv4.

Upewnij się, liczba swój adres IP i jak są zarejestrowane dla Ciebie w jednym kancelarii wymienione poniżej.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Publiczne adresy IP ogłaszane do firmy Microsoft przez ExpressRoute musi nie były ogłaszane w Internecie. Może to spowodować uszkodzenie łączności z innych usług firmy Microsoft. Jednak publiczne adresy IP używane przez serwery w sieci, które komunikować się z punktów końcowych usługi Office 365 w programie Microsoft mogą być objęte przez ExpressRoute. 

## <a name="dynamic-route-exchange"></a>Dynamiczne rozsyłania programu exchange

Kierowanie exchange będą przez protokół eBGP. Sesje EBGP są ustanowione między MSEEs i routery. Uwierzytelnianie sesji BGP nie jest wymagane. W razie potrzeby można skonfigurować mieszanie MD5. Zobacz [Konfigurowanie routingu](expressroute-howto-routing-classic.md) i [obwód inicjowania obsługi administracyjnej, przepływy pracy i Stany obwodów](expressroute-workflows.md) informacje dotyczące konfigurowania BGP sesji.

## <a name="autonomous-system-numbers"></a>Autonomiczne numerów systemowych

Microsoft użyje 12076 jako Azure publiczne, prywatne Azure i zaglądanie firmy Microsoft. Firma Microsoft jest zarezerwowana nich z 65515 do 65520 do użytku wewnętrznego. Obsługiwane są zarówno 16 i 32 bit jako liczby.

Nie ma żadnych wymagań wokół symetrii transfer danych. Ścieżki do przodu i zwrotu może przechodzenia pary różnych routera. Identyczne przekierowuje musi ogłaszane z obu stron przez wielu pary elektrycznego należących możesz. Metryki trasy nie muszą być identyczne.

## <a name="route-aggregation-and-prefix-limits"></a>Kierowanie agregacji prefiks u góry i u dołu

Obsługujemy prefiksy do 4000 ogłaszane z nami za pośrednictwem Azure zaglądanie prywatne. Czy można zwiększyć do 10 000 prefiksy po włączeniu dodatku premium ExpressRoute. Firma Microsoft zaakceptować maksymalnie 200 prefiksy BGP sesji dla Azure publicznej oraz zaglądanie firmy Microsoft. 

Sesja BGP zostanie usunięty, jeśli liczba prefiksy przekracza limit. Firma Microsoft akceptuje trasy domyślne tylko prywatne łącze peering. Dostawca należy odfiltrować trasy domyślnej i prywatnych adresów IP (RFC 1918) z platformy Azure publicznej i ścieżek zaglądanie firmy Microsoft. 

## <a name="transit-routing-and-cross-region-routing"></a>Czas przesyłania i routingu między regionu

ExpressRoute nie można skonfigurować jako routery przewozowe. Konieczne będzie zależne od dostawcy łączności dla usług routingu przewozowe.

## <a name="advertising-default-routes"></a>Trasy domyślne reklamami

Trasy domyślne są dozwolone tylko na sesje Azure zaglądanie prywatne. W takim przypadku firma Microsoft może kierować wszystkie ze skojarzonymi wirtualnych sieci z siecią. Reklamę trasy domyślne do prywatnych zaglądanie spowoduje ścieżki internetowe z platformy Azure blokowaniu. Konieczne jest zastosowanie Twojej firmy krawędzi, aby skierować ruch od i do internetowej dla usługi obsługiwane platformy Azure. 

 Aby włączyć łączność z innych usług Azure i usługi infrastruktury, musi upewnij się, że jest jedną z następujących elementów w miejscu:

 - Azure zaglądanie publicznej włączono ruch trasy do publicznych punktów końcowych
 - Możesz używać zdefiniowanych przez użytkownika routingu umożliwia połączenie z Internetem dla każdej podsieci wymagające łączność z Internetem.
 
>[AZURE.NOTE] Reklamę trasy domyślne podziały systemu Windows i innych maszyn wirtualnych Aktywacja licencji. Postępuj zgodnie z instrukcjami [poniżej](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) do obejścia tego problemu.

## <a name="support-for-bgp-communities-preview"></a>Obsługa BGP społeczności (wersja Preview)


Ta sekcja zawiera omówienie używania społeczności BGP w ExpressRoute. Microsoft będzie ogłaszanie trasy w publicznych i Microsoft zaglądanie ścieżek trasy ze społeczności odpowiednie wartości. Uzasadnienie to i szczegółowych informacji na temat wartości społeczności opisane poniżej. Microsoft, nie będą jednak przestrzegać wartości społeczności oznakowane marszrut ogłaszane do firmy Microsoft.

Jeśli łączysz się do firmy Microsoft przez ExpressRoute w jednym miejscu peering w regionie geopolitycznych, uzyskuje dostęp do wszystkich usług w chmurze firmy Microsoft przez wszystkich regionów granicach geopolitycznych. 

Na przykład jeśli połączenie do firmy Microsoft w Amsterdam za pośrednictwem ExpressRoute, będzie miał dostęp do wszystkich usług chmury firmy Microsoft, obsługiwany w Europie północnej i Europa Zachodnia. 

Zapoznaj się z stronie [partnerów ExpressRoute i lokalizacji zaglądanie](expressroute-locations.md) szczegółową listą regionów geopolitycznych, skojarzone Azure regionów i odpowiadające im ExpressRoute zaglądanie lokalizacji.

Możesz kupić więcej niż jeden obwód ExpressRoute w rozbiciu na regiony geopolitycznych. Masz wiele połączeń umożliwia znaczące korzyści na wysokiej dostępności z powodu geo nadmiarowości. W przypadku której znajduje się wiele obwodów ExpressRoute otrzymasz ten sam zestaw prefiksy ogłaszane firmy Microsoft na publicznej zaglądanie i Microsoft zaglądanie ścieżek. Oznacza to, że będą mieć wiele ścieżek z sieci do firmy Microsoft. Może to powodować potencjalnie częściowej optymalne decyzje dotyczące routingu w sieci. W wyniku mogą występować łączności częściowej optymalnego środowiska do różnych usług. 

Microsoft będzie oznaczać prefiksy ogłaszane za pośrednictwem publicznego zaglądanie i Microsoft zaglądanie z odpowiednie wartości społeczności BGP określającym obszar prefiksów znajdują się w. Czy w przypadku korzystania z wartości społeczności, aby właściwych decyzji dotyczących routingu do oferowania [optymalny rozkład klientom](expressroute-optimize-routing.md).

| **Region geopolitycznych** | **Region Microsoft Azure** | **Wartość społeczności BGP** |
|---|---|---|
| **Ameryka Północna** |    |  |
|    | Wschodniej Stany Zjednoczone | 12076:51004 |
|    | Wschodniej USA 2 | 12076:51005 |
|    | Zachód Stany Zjednoczone | 12076:51006 |
|    | Stany Zjednoczone zachód 2 | 12076:51026 |
|    | Stany Zjednoczone centralnej zachód | 12076:51027 |
|    | Ameryka Północna centralnej w Stanach Zjednoczonych | 12076:51007 |
|    | Południowej centralnej Stany Zjednoczone | 12076:51008 |
|    | Centralny Stany Zjednoczone | 12076:51009 |
|    | Kanada Środkowa | 12076:51020 |
|    | Wschód Kanada | 12076:51021 |
| **Ameryka Południowa** |  |  |
|    | Brazylia Południowej | 12076:51014 |
| **Europa** |    |  |
|    | Europa Północna | 12076:51003 |
|    | Europa Zachodnia | 12076:51002 |
| **Części Azji** |    |   |
|    | Wschodnioazjatyckie | 12076:51010 |
|    | Kraje Azji Południowo | 12076:51011 |
| **Japonia** |     |   |
|    | Japonia wschód | 12076:51012 |
|    | Japonia zachód | 12076:51013 |
| **Australia** |    |   | 
|    | Wschód Australia | 12076:51015 |
|    | Australia kopiec | 12076:51016 |
| **Indie** |    |   |
|    | Południe Indie | 12076:51019 |
|    | Indie zachód | 12076:51018 |
|    | Centralny Indie | 12076:51017 |

Wszystkie trasy ogłaszane od firmy Microsoft będzie oznakowanie z wartością odpowiednie społeczności. 

>[AZURE.IMPORTANT] Prefiks globalny oznakowano wartość odpowiednie społeczności i będzie ogłaszane tylko wtedy, gdy dodatek premium ExpressRoute jest włączona.


Oprócz powyższych Microsoft również będzie oznaczać prefiksy opartego na usłudze należą do. Dotyczy to tylko zaglądanie firmy Microsoft. W poniższej tabeli przedstawiono mapowanie usługi BGP społeczności wartości.

| **Usługa** | **Wartość społeczności BGP** |
|---|---|
| **Exchange** | 12076:5010 |
| **Programu SharePoint** | 12076:5020 |
| **Program Skype dla firm** | 12076:5030 |
| **CRM w trybie Online** | 12076:5040 |
| **Inne usługi Office 365** | 12076:5100 |

>[AZURE.NOTE] Microsoft nie przestrzegać wartości społeczności BGP, które można ustawić przekierowuje ogłaszane do firmy Microsoft.

## <a name="next-steps"></a>Następne kroki

- Konfigurowanie połączenia ExpressRoute.

    - [Tworzenie obwód ExpressRoute dla modelu Klasyczny wdrożenia](expressroute-howto-circuit-classic.md) lub [Tworzenie i modyfikowanie obwód ExpressRoute za pomocą Menedżera zasobów Azure](expressroute-howto-circuit-arm.md)
    - [Konfigurowanie routingu modelu Klasyczny wdrożenia](expressroute-howto-routing-classic.md) lub [Konfigurowanie routingu modelu wdrożenia Menedżera zasobów](expressroute-howto-routing-arm.md)
    - [Łącze klasyczny VNet obwodem ExpressRoute](expressroute-howto-linkvnet-classic.md) lub [łącze VNet Menedżera zasobów, aby obwód ExpressRoute](expressroute-howto-linkvnet-arm.md)


