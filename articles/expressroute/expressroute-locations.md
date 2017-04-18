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
|**Ameryka Północna**|Wschodniej Stany Zjednoczone, zachód Stany Zjednoczone, USA wschodniego 2, centralnej Stany Zjednoczone, Południowej centralnej Stany Zjednoczone, Ameryka Północna centralnej w Stanach Zjednoczonych, Kanada Środkowa Kanadzie wschód|Atlanta Chicago, Dallas, Wyszków, Los Angeles, Nowy Jork, Seattle, Dolinie Krzemowej, Washington kontrolera domeny, Montrealu +, Miasto Quebec +, Toronto|
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

| **Dostawca usług**  |**Microsoft Azure** | **Usługa Office 365 i CRM w trybie Online** | **Lokalizacje** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Obsługiwane | Obsługiwane | Melbourne, Sydney |
| **[Aryaka sieci]( http://www.aryaka.com/)** | Obsługiwane | Obsługiwane | Amsterdam, Dolinie Krzemowej, Singapur, Tokio, Washington kontrolera domeny |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Obsługiwane | Obsługiwane | Amsterdam, Chicago, Dallas, Londynie Dolinie Krzemowej, Singapur, Sydney, Washington kontrolera domeny |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Obsługiwane | Obsługiwane | Amsterdam, Hongkong SAR, Londyn, Dolinie Krzemowej, Singapur, Sydney, Tokio, Washington kontrolera domeny |
|**CenturyLink** | Wkrótce | Wkrótce| Dolinie Krzemowej |
|**Chiny Telecom globalny** | Obsługiwane | Brak obsługi | Hongkong SAR |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Obsługiwane | Wkrótce | Toronto Dallas Montrealu + |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Obsługiwane | Obsługiwane | Amsterdam, Tokio Dublin, Londyn, |
| **Comcast** | Obsługiwane | Obsługiwane | Warszawa, Dolinie Krzemowej, Washington kontrolera domeny |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Obsługiwane | Obsługiwane | Los Angeles | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Obsługiwane | Obsługiwane | Amsterdam, Atlanta, Chicago, Dallas, Hongkong SAR, Londynie, Los Angeles, Melbourne, Nowy Jork, Osaka, Paryż +, Wyspy Paulo, Seattle, Dolinie Krzemowej, Singapur, Sydney, Tokio, Toronto, Washington kontrolera domeny |
| **euNetworks** |  Obsługiwane | Obsługiwane | Amsterdam |
| **GÉANT** | Obsługiwane | Obsługiwane | Amsterdam |
| **[Internet Initiative Japonii Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Obsługiwane | Obsługiwane | Osaka, Tokio |
| **[InterCloud]( https://www.intercloud.com/)** | Obsługiwane | Obsługiwane | Kontrolera domeny Waszyngton Amsterdam, Londyn, Singapur |
| **Połączenie internetowe rozwiązań — chmury** | Obsługiwane | Obsługiwane | Amsterdam, Londyn |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Obsługiwane | Obsługiwane | Amsterdam, Londyn, Paryż |
| **Jisc** | Obsługiwane | Obsługiwane | (Londyn) | 
| **[Komunikacja poziomu 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Obsługiwane | Obsługiwane | Amsterdam, Chicago, Dallas, Warszawie +, Londyn, Seattle, Dolinie Krzemowej, Washington kontrolera domeny |
| **Megaport** | Obsługiwane | Obsługiwane | Dallas, Hongkong SAR nazwę miasta Wyszków, Los Angeles Melbourne, Nowy Jork Seattle, Singapur Sydney, Washington kontrolera domeny |
| **MTN** | Obsługiwane | Obsługiwane | (Londyn) |
| **Następny Generowanie danych** | Wkrótce | Wkrótce | Newport(Wales) + |
| **NEXTDC** | Obsługiwane | Obsługiwane | Melbourne, Sydney |
| **NTT komunikacji** | Obsługiwane | Obsługiwane | Londyn, Los Angeles Osaka, Tokio |
| **[Pomarańczowy]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Obsługiwane | Obsługiwane | Londyn Amsterdam, Hongkong SAR, Dolinie Krzemowej, Singapur Sydney, Washington kontrolera domeny |
| **Ograniczone PCCW globalny** | Obsługiwane | Obsługiwane | Hongkong SAR |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Obsługiwane | Obsługiwane | Singapur |
| **Softbank** | Obsługiwane | Obsługiwane | Osaka, Tokio | 
| **[Komunikacja tata](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Obsługiwane | Obsługiwane | Amsterdam, Chennai, Hong Kong, Londyn, Bombaj, kontrolera domeny Waszyngton Dolinie Krzemowej, Singapur |
| **[Grupa TeleCity]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Obsługiwane | Obsługiwane | Amsterdam, Dublin, Londyn |
| **Telefonica** | Obsługiwane | Obsługiwane | Wyspy Paulo |
| **Telenor** | Obsługiwane | Obsługiwane | Amsterdam, Londyn |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Obsługiwane | Obsługiwane | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Obsługiwane | Obsługiwane | Amsterdam, Hongkong SAR, Londyn, Dolinie Krzemowej, Singapur, Sydney, Tokio, Washington kontrolera domeny |
| **Vodafone** | Obsługiwane | Brak obsługi | (Londyn) | 
| **[Grupa Zayo]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Obsługiwane | Obsługiwane | Warszawa, Dolinie Krzemowej Los Angeles, Nowy Jork, Toronto, Washington kontrolera domeny |

 **+**oznacza wkrótce

### <a name="national-cloud-environments"></a>Środowiska chmury krajowe

#### <a name="us-government-cloud"></a>Chmury Rządu Stanów Zjednoczonych

| **Dostawca usług**  |**Microsoft Azure** | **Usługi Office 365** | **Lokalizacje** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Obsługiwane | Obsługiwane | Chicago, Washington kontrolera domeny |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Obsługiwane | Obsługiwane | Chicago, Dallas, Nowy Jork, Washington kontrolera domeny |
| **[Komunikacja poziomu 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Obsługiwane | Obsługiwane | Chicago, Nowy Jork +, Washington kontrolera domeny |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Obsługiwane | Obsługiwane | Chicago, Dallas + Nowy Jork, Washington kontrolera domeny |

#### <a name="china"></a>Chiny

| **Dostawca usług**  |**Microsoft Azure** | **Usługi Office 365** | **Lokalizacje** |
|-----------------------|--------------------|----------------|---------------|
| **Chiny telekomunikacyjnych** | Obsługiwane | Brak obsługi | Pekin, Szanghaj|
Aby uzyskać więcej informacji, zobacz [ExpressRoute w Chinach](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Niemcy

| **Dostawca usług**  |**Microsoft Azure** | **Usługi Office 365** | **Lokalizacje** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Obsługiwane | Brak obsługi | Berlin + Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Obsługiwane | Brak obsługi | Frankfurt|
| **e zadaszeniem** | Obsługiwane | Brak obsługi | Berlin|
| **Interxion** | Obsługiwane | Brak obsługi | Frankfurt|

## <a name="nonpartners"></a>Łączność za pośrednictwem dostawcy usług nie na liście

Jeśli dostawca połączeń nie są widoczne w poprzedniej sekcji, możesz utworzyć połączenie.

- Skontaktuj się z dostawcą łączności, aby sprawdzić, czy są one podłączone do dowolnej wymiany w powyższej tabeli. Możesz sprawdzić poniższe łącza zebrać więcej informacji na temat usługi oferowane przez dostawców programu exchange. Wymiana Ethernet już połączenie kilku dostawców łączności.

    - [Chmura Equinix programu Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Masz dostawcy łączności rozszerzanie sieci w celu peering lokalizację.
    - Upewnij się, że dostawcy łączności rozszerza łączność w sposób wysokiej dostępności, tak aby były nie pojedynczych punktów awarii.
- Zamówień obwód ExpressRoute z serwerem exchange jako dostawcę łączności nawiązywania połączenia z firmy Microsoft.
    - Wykonaj kroki w sekcji [Tworzenie obwód ExpressRoute](expressroute-howto-circuit-classic.md) konfigurowania.

|**Dostawca połączeń**|**Exchange**|**Lokalizacje**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapur|
|**Komunikacja Alaska**|Equinix|Seattle|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|Nowy Jork, Washington kontrolera domeny|
|**[Komunikacja XO](http://www.xo.com/)**|Equinix|Dolinie Krzemowej|


## <a name="expressroute-system-integrators"></a>ExpressRoute integratorów

Włączanie prywatne łączności odpowiednio do potrzeb może być trudne, oparte na skali sieci. Możesz pracować z wymienionych integratorów wymienione w poniższej tabeli, aby pomóc w ułatwiającej rozpoczęcie korzystania z ExpressRoute.

|**Systemu polega**|**Kontynent**|
|---|---|
|**[Avanade Inc.](http://www.avanade.com/)**| Kraje Azji, Europe, USA |
|**[Rozwiązania DotNet](http://www.dotnetsolutions.co.uk/)**| Europa |
|**[Usługi profesjonalne Equinix](http://www.equinix.com/services/consulting/)**|Z NAMI|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Kraje Azji |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Z NAMI |
|**[Zarządzanie projektu](http://www.projectleadership.net/azure)** | Z NAMI |

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o ExpressRoute zobacz [Często zadawane pytania dotyczące ExpressRoute](expressroute-faqs.md).
- Upewnij się, że są spełnione wszystkie wymagania wstępne. Zobacz [wymagania wstępne dotyczące ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa lokalizacji"
