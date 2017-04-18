<properties
   pageTitle="Lokalizacje ExpressRoute | Microsoft Azure"
   description="Ten artykuł zawiera szczegółowe omówienie lokalizacje miejsce, w którym oferowanych usług i sposobie łączenia się Azure regionów."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>Partnerzy ExpressRoute i zaglądanie lokalizacji

Tabele w tym artykule zawiera informacje dotyczące ExpressRoute łączności dostawców zasięgu geograficznego ExpressRoute, usług w chmurze firmy Microsoft obsługiwany przez ExpressRoute i integratorów ExpressRoute (SIs).

## <a name="partners"></a>ExpressRoute łączności dostawców

ExpressRoute jest obsługiwany we wszystkich regionach Azure i lokalizacji. Następujące mapy zawiera listę regionów Azure i lokalizacje ExpressRoute. Lokalizacje ExpressRoute dotyczą miejsce, w którym Microsoft równorzędnymi użytkownikami z kilku dostawców usług.

![Mapa lokalizacji][0]

Będzie miał dostępu do usług Azure przez wszystkich regionów w regionie geopolitycznych, jeśli połączony z co najmniej jedna lokalizacja ExpressRoute w obszarze geopolitycznych. Poniższa tabela zawiera mapę Azure regionów do lokalizacji ExpressRoute w regionie geopolitycznych.

|**Region geopolitycznych**|**Regiony Azure**|**ExpressRoute lokalizacji**|
|---|---|---|
|**Ameryka Północna**|Wschodniej Stany Zjednoczone, zachód Stany Zjednoczone, wschodniego USA 2, centralnym USA, Południowej centralnym Stany Zjednoczone, Ameryka Północna centralnej w Stanach Zjednoczonych, wschód Kanadzie Central Kanada|Atlanta Chicago, Dallas, Wyszków, Los Angeles, Nowy Jork, Seattle, Dolinie Krzemowej, Washington kontrolera domeny, Montrealu +, Miasto Quebec +, Toronto|
|**Ameryka Południowa**|Brazylia Południowej|Wyspy Paulo|
|**Europa**|Zachód Wielkiej Brytanii północnej Europie, Zachodnia Europa, południe USA|Amsterdam, Dublin, Londyn, Newport(Wales) +, Paryż|
|**Kraje Azji**|Azji Wschodniej Azji Południowo-Wschodniej|Hongkong SAR, Singapur|
|**Japonia**|Japonia Zachodnia wschód Japonii|Osaka, Tokio|
|**Australia**|Australia kopiec, wschód Australia|Melbourne, Sydney|
|**Indie**|Indie Zachód, południe centralna, Indie Indie|Chennai, Mumbai|



Poniższa tabela zawiera informacje dotyczące regionów i geopolitycznych ograniczenia dla krajowych chmur.

|**Region geopolitycznych**|**Regiony Azure**|**ExpressRoute lokalizacji**|
|---|---|---|---|
|**Chmury Rządu Stanów Zjednoczonych**|Virginia Gov Gov Iowa, USA Stany Zjednoczone|Chicago, Dallas, Nowy Jork, Washington kontrolera domeny|
|**Chiny**|Chiny północ, Chinach wschód|Pekin, Szanghaj|
|**Niemcy**|Niemcy centralna, Niemcy wschód|Berlin, Frankfurt|


Łączność między różnymi regionami geopolitycznych nie jest obsługiwana w wersji standard ExpressRoute. Należy włączyć dodatek premium ExpressRoute do obsługi połączeń globalnej. Łączność z środowiska chmury krajowych nie jest obsługiwane. W razie wymagają można pracować z dostawcą łączności.


## <a name="connectivity-provider-locations"></a>Lokalizacje dostawcy łączności

> [AZURE.SELECTOR]
[Lokalizacje przez dostawcę](expressroute-locations.md#connectivity-provider-locations)
[dostawców według lokalizacji](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Azure produkcji
| **Lokalizacja**  | **Dostawcy usług** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka sieci, AT & T NetBond British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud rozwiązań Internet - połączyć w chmurze, Interxion, Verizon komunikacji, pomarańczowy, Tata komunikacji, TeleCity grupa Telenor, poziomu 3 |
| **Atlanta** | Equinix |
| **Chennai** | Komunikacja tata |
| **Chicago** | AT & T NetBond, Comcast Equinix, poziomu 3 komunikacja, grupie Zayo |
| **Dallas** | AT & T NetBond, Cologix, Equinix, poziomu 3 komunikacja Megaport |
| **Witryna Dublin** | Colt, Telecity grupy |
| **Hongkong SAR** | Globalny telekomunikacyjnych British Telecom, Chinach, Equinix, Megaport, pomarańczowy, PCCW globalnej ograniczony, kliknij pozycję Komunikacja Tata Verizon |
| **(Londyn)** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, rozwiązania Internet - połączyć w chmurze, Vodafone Telenor, Verizon, komunikacji, MTN, NTT komunikacji, pomarańczowy, Tata komunikacji, Telecity grupa poziomu 3 Interxion, Jisc, |
| **Wyszków** | Poziom 3 komunikacji + Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo grupa |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Nowy Jork** | Equinix, Megaport, Zayo grupa |
| **Newport(Wales) +** | Następny Generowanie danych + |
| **Montrealu** | Cologix + |
| **Bombaj** | Komunikacja tata |
| **Osaka** | Komunikacja NTT Equinix Internet Initiative Japonii Inc. - IIJ, Softbank |
| **Paryż** | Interxion, Equinix + |
| **Wyspy Paulo** | Equinix, Telefonica |
| **Seattle** | Megaport Equinix, kliknij pozycję Komunikacja poziomu 3 |
| **Dolinie Krzemowej** | Sieci Aryaka AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, poziom 3 komunikacji, pomarańczowy, Tata komunikacji, Verizon, Zayo grupę |
| **Singapur** | Sieci Aryaka AT & T NetBond British Telecom, Equinix, InterCloud, Megaport, pomarańczowy, SingTel, Tata komunikacji, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, pomarańczowy, Telstra Corporation, Verizon |
| **Tokio** | Verizon Softbank, kliknij pozycję Komunikacja NTT Aryaka sieci, British Telecom, Colt, Equinix, Internet Initiative Japonii Inc. - IIJ, |
| **Toronto** | Cologix, Equinix, Zayo grupa |
| **Waszyngton kontrolera domeny** | Sieci Aryaka AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, poziom 3 komunikacji, Megaport, pomarańczowy, Tata komunikacji, Verizon, Zayo grupę |

 **+**oznacza wkrótce

### <a name="national-cloud-environments"></a>Środowiska chmury krajowe

#### <a name="us-government-cloud"></a>Chmury Rządu Stanów Zjednoczonych

| **Lokalizacja**  |**Dostawcy usług** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, poziomu 3 komunikacja Verizon |
| **Dallas** |  Equinix, Verizon + |
| **Nowy Jork** | Verizon Equinix poziomu 3 komunikacji +, |
| **Waszyngton kontrolera domeny** | AT & T NetBond, Equinix, poziomu 3 komunikacja Verizon |

#### <a name="china"></a>Chiny

| **Lokalizacja**  | **Dostawcy usług** |
|---------------|-----------------------|
| **Pekin** | Chiny telekomunikacyjnych |
| **Szanghaj** |  Chiny telekomunikacyjnych |
Aby uzyskać więcej informacji, zobacz [ExpressRoute w Chinach](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Niemcy

| **Lokalizacja**  | **Dostawcy usług** |
|---------------|-----------------------|
| **Berlin** | Colt +, e zadaszeniem |
| **Frankfurt** | Interxion Colt, Equinix, |

## <a name="nonpartners"></a>Łączność za pośrednictwem dostawcy usług nie na liście

Jeśli dostawca połączeń nie są widoczne w poprzedniej sekcji, możesz utworzyć połączenie.

- Skontaktuj się z dostawcą łączności, aby sprawdzić, czy są one podłączone do dowolnej wymiany w powyższej tabeli. Możesz sprawdzić poniższe łącza zebrać więcej informacji na temat usługi oferowane przez dostawców programu exchange. Wymiana Ethernet już połączenie kilku dostawców łączności.

    - [Chmura Equinix programu Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Masz dostawcy łączności rozszerzanie sieci w celu peering lokalizację.
    - Upewnij się, że dostawcy łączności rozszerza łączność w sposób wysokiej dostępności, tak aby były nie pojedynczych punktów awarii.
- Zamówień obwód ExpressRoute z serwerem exchange jako dostawcę łączności nawiązywania połączenia z firmy Microsoft.
    - Wykonaj kroki w sekcji [Tworzenie obwód ExpressRoute](expressroute-howto-circuit-classic.md) konfigurowania.

|**Lokalizacja**|**Exchange**|**Łączność dostawców**|
|-------------|------------|-------------------------|
| **Nowy Jork** | Equinix | Lightower |
| **Seattle** | Equinix | Komunikacja Alaska |
| **Dolinie Krzemowej** | Equinix | Komunikacja XO |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Waszyngton kontrolera domeny** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute integratorów

Włączanie prywatne łączności odpowiednio do potrzeb może być trudne, oparte na skali sieci. Możesz pracować z wymienionych integratorów wymienione w poniższej tabeli, aby pomóc w ułatwiającej rozpoczęcie korzystania z ExpressRoute.

|**Kontynent**|**Integratorów**|
|-------------|---------------------|
| **Kraje Azji** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet rozwiązań|
| **Z NAMI** | Avanade Inc., usługi profesjonalne Equinix, Perficient, zarządzanie projektu|

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o ExpressRoute zobacz [Często zadawane pytania dotyczące ExpressRoute](expressroute-faqs.md).
- Upewnij się, że są spełnione wszystkie wymagania wstępne. Zobacz [wymagania wstępne dotyczące ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa lokalizacji"
