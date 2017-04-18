<properties 
   pageTitle="Strefy DNS i rekordy | Microsoft Azure" 
   description="Omówienie pomocy technicznej do obsługi strefy DNS i rekordy DNS firmy Microsoft Azure." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>Strefy DNS i rekordy

Ta strona wyjaśniono podstawowe pojęcia domen, strefy DNS i rekordy DNS i zestawy rekordów i jak są obsługiwane w systemie DNS Azure.

## <a name="domain-names"></a>Nazwy domen
 
Domain Name System jest hierarchię domen. Hierarchia zaczyna się od domeny "głównej", którego nazwa jest po prostu**.**.  Poniżej pochodzą domen najwyższego poziomu, takich jak "com", "netto", "schemat", "USA" lub "jp".  Poniżej tych są domen drugiego poziomu, takich jak "org.uk" lub "co.jp". Globalne rozpowszechniane są domen w hierarchii DNS, obsługiwanych przez serwery DNS na świecie.

Rejestratora nazw domen jest organizacją, która umożliwia uzyskanie nazwy domeny, na przykład "contoso.com".  Zakup nazwy domeny umożliwia w prawo, aby kontrolować hierarchii DNS przy użyciu tej nazwy, na przykład umożliwia bezpośrednie nazwę "www.contoso.com" w witrynie sieci web firmy. Rejestrator może obsługiwać domenę na własnych serwerów nazw w Twoim imieniu lub Alternatywnie możesz określić serwery nazw alternatywny.

Azure DNS oferuje infrastruktury serwera globalnie rozłożone, wysokiej dostępności nazwy, które można udostępniać swojej domeny. Udostępniając swoich domen w systemie DNS Azure, możesz zarządzać swoje rekordy DNS przy użyciu tych samych poświadczeń, API, narzędzia, rozliczenia i obsługi jako innych usług Azure.

Azure DNS nie obsługuje obecnie zakupu nazw domen. Jeśli chcesz kupić nazwę domeny, musisz użyć Rejestratora nazw domen innych firm. Rejestrator będzie zwykle opłatę small rocznej. Następnie domen może być obsługiwany w systemie DNS Azure do zarządzania rekordami DNS. Aby uzyskać szczegółowe informacje, zobacz [pełnomocnika domeny DNS Azure](dns-domain-delegation.md) .

## <a name="dns-zones"></a>Strefy DNS 

Strefy DNS służy do przechowywania rekordów DNS dla danej domeny. Aby rozpocząć, hostingu swojej domeny Azure DNS, należy utworzyć strefy DNS dla tej nazwy domeny. W tej strefie DNS zostanie utworzona każdego rekordu DNS dla swojej domeny. 

Na przykład domeny "contoso.com" może zawierać kilka rekordów DNS, na przykład "mail.contoso.com" (dla serwera poczty) i "www.contoso.com" (w przypadku witryny sieci web).

Podczas tworzenia strefy DNS w systemie DNS Azure, nazwa strefy musi być unikatowa w grupie zasobów. Tej samej nazwy strefy może nastąpić w różnych grupach zasobów lub innej subskrypcji Azure. W przypadku, gdy kilka stref o tej samej nazwie, każdego wystąpienia przypisano adresy serwerów inną nazwę. Można skonfigurować tylko jeden zestaw adresów z rejestratorem nazwy domeny.

>[AZURE.NOTE] Nie masz właścicielem nazwy domeny, aby utworzyć strefy DNS z tej nazwy domeny w usłudze DNS Azure. Należy jednak domena skonfigurować serwery nazw Azure DNS jako serwery prawidłową nazwę dla nazwy domeny z rejestratorem nazwy domeny.

Aby uzyskać więcej informacji zobacz [pełnomocnika domeny DNS Azure](dns-domain-delegation.md).

## <a name="dns-records"></a>Rekordy DNS

### <a name="record-types"></a>Typy rekordów

Każdy rekord DNS ma nazwę i typ. Rekordy są zorganizowane na różne typy zgodnie z danymi, które zawierają. Najpopularniejszym typem jest "" rekordu, który mapy nazwę na adres IP protokołu IPv4. Innym typem typowych jest rekord "MX", który mapy nazwę z serwerem poczty.

Azure DNS obsługuje wszystkie wspólnego typów rekordów DNS: A, PTR, AAAA, MX, NS, SOA, CNAME, SRV i TXT.

### <a name="record-names"></a>Rejestrowanie nazw

W systemie DNS Azure określono rekordów przy użyciu nazw względnych. *W pełni kwalifikowaną* nazwę domeny (FQDN) zawiera nazwę strefy, dlatego nie ma *względną* nazwę. Na przykład względną nazwą rekord "www", "contoso.com" w strefie daje rekordu w pełni kwalifikowana nazwa "www.contoso.com".

Rekord *wierzchołka* jest rekordu DNS w głównym (lub *wierzchołka*) strefy DNS. Na przykład w strefie DNS "contoso.com" wierzchołka rekordu również znajduje się w pełni kwalifikowana nazwa "contoso.com" (to jest czasami nazywana *bez* domeny).  W Konwencji, nazwa względna '@' służy do tworzenia rekordów wierzchołka.

### <a name="record-sets"></a>Zestawy rekordów
Czasem trzeba utworzyć więcej niż jeden rekord DNS o podanej nazwie i typu. Załóżmy, że witryna sieci web "www.contoso.com" jest przechowywana na dwóch różnych adresów IP. Witryna sieci Web wymaga dwóch różnych rekordów, jedną dla każdego adresu IP. Oto przykład zestaw rekordów:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS zarządza rekordami DNS za pomocą *zestawów rekordu*. Zestaw rekordów (nazywane także zestaw rekordów *zasobów* ) jest kolekcja rekordów DNS w strefie, które mają taką samą nazwę i tego samego typu. Większość zestawów rekordów zawierających pojedynczy rekord, ale przykłady podobny do tego, w której zestaw rekordów zawiera więcej niż jeden rekord, nie są rzadko.

Na przykład załóżmy, że zostały już utworzone rekord "www", "contoso.com" w strefie, wskazująca IP adresu "134.170.185.46" (pierwszy rekord powyżej).  Aby utworzyć drugi rekord można będzie dodać tego rekordu do istniejącego zestawu rekordów, zamiast Tworzenie nowego zestawu rekordów.

Typy rekordów CNAME i SOA są wyjątki. Standardy DNS nie zezwalają wielu rekordów z tą samą nazwą dla tych typów, a więc te zestawy rekordów może zawierać tylko jeden rekord.

### <a name="time-to-live"></a>Czas trwania

Czas wygaśnięcia, czyli TTL Określa, jak długo każdy rekord jest buforowana przez klientów przed poszukiwanych ponownie. W powyższym przykładzie wartość TTL jest 3600 sekund lub 1 godzina.

W systemie DNS Azure wartość TTL jest określona dla zestawu rekordów, nie dla każdego rekordu, więc tę samą wartość jest używana dla wszystkich rekordów w ramach tego zestawu rekordów.  Możesz określić wartości TTL od 1 do 2,147,483,647 sekund.

### <a name="wildcard-records"></a>Symbol wieloznaczny rekordów

Azure DNS obsługuje [rekordy symboli wieloznacznych](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Rekordy symboli są zwracane w odpowiedzi na dowolną kwerendę o nazwie pasujące (o ile nie znajduje się bliżej dopasowanie z wartością symboli zestawu rekordów). Symbol wieloznaczny zestawy rekordów są obsługiwane dla wszystkich typów rekordów, z wyjątkiem NS i SOA.  

Aby utworzyć zestaw rekordów symboli wieloznacznych, należy użyć nazwy zestawu rekordów "\*". Możesz też umożliwia także nazwisko z "\*"jako jego lewej etykieta, na przykład"\*foo".

### <a name="cname-records"></a>Rekordy CNAME

Zestawy rekordów CNAME nie współistnienie inne zestawy rekordów o takiej samej nazwie. Na przykład, nie można utworzyć rekord CNAME ustaw o nazwie względne "www", a rekord A nazwą względne "www" w tym samym czasie.

Ponieważ wierzchołka strefy (nazwa = ‘@’) zawsze zawiera zestawy, które zostały utworzone podczas tworzenia strefy rekordów NS i SOA, nie można utworzyć rekord CNAME na wierzchołka strefy.

Te ograniczenia mogą pojawić się ze standardami DNS i nie są ograniczenia dotyczące Azure DNS.

### <a name="ns-records"></a>Rekordów serwera nazw

Zestaw rekordów serwera nazw jest tworzony automatycznie w wierzchołka każdej strefie (nazwa = ‘@’), i usuwane automatycznie po usunięciu strefy (nie można go usunąć oddzielnie).  Można modyfikować ustawienia TTL tego zestawu rekordów, ale nie można zmodyfikować rekordy, które są wstępnie skonfigurowane, aby odwołać się do serwerów nazw Azure DNS przypisane do strefy.

Możesz tworzyć i usuwać inne rekordy serwera nazw w strefie nie u wierzchołka strefy.  Pozwala na konfigurowanie stref podrzędnych (zobacz [Delegowanie domeny podrzędne w systemie DNS Azure](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>Rekordy SOA

Zestaw rekordów SOA jest tworzony automatycznie w wierzchołka każdej strefie (nazwa = ‘@’), i usuwane automatycznie po usunięciu strefy.  Rekordy SOA nie można utworzyć ani usuwać oddzielnie. 

Możesz zmienić wszystkie właściwości rekordu SOA z wyjątkiem właściwość "host", która jest wstępnie skonfigurowana, aby odwołać się do nazwy serwera nazwy podstawowej dostarczony przez Azure DNS.

### <a name="spf-records"></a>Rekordy SPF

Rekordy zasad Framework (SPF) nadawcy służą do określania, serwery poczty e-mail, które mają uprawnienia do wysyłania wiadomości e-mail w imieniu nazwę danej domeny.  Poprawnej konfiguracji rekordy SPF ważne jest, aby uniemożliwić adresatom oznaczania wiadomości e-mail jako "śmieci".

Specyfikacje DNS RFC wprowadzona nowy typ rekordu "SPF" do obsługi tego scenariusza. Aby obsługiwać starsze serwery nazw, również dozwolone wykorzystanie typem rekordu TXT, aby określić rekordy SPF.  Ten niejednoznaczności doprowadziły do zamieszania, który został rozwiązany przy [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  To zaznaczone rekordy SPF powinny być tworzone tylko przy użyciu typu rekordu TXT, a przestarzałe typu rekordu SPF. 

**Rekordy SPF są obsługiwane przez system DNS Azure i ma zostać utworzona za pomocą tego typu rekordu TXT.** Typ przestarzałego rekordu SPF nie jest obsługiwane. Podczas [importowania DNS strefy pliku](dns-import-export.md)wszystkie rekordy SPF przy użyciu tego typu rekordu SPF są konwertowane na tego typu rekordu TXT. 

### <a name="srv-records"></a>Rekordy SRV

[Rekordy SRV](https://en.wikipedia.org/wiki/SRV_record) są używane przez różne usługi do określenia lokalizacji serwerów. Podczas określania rekord SRV w usłudze Azure DNS:

- *Usługa* i *Protokół* należy określić jako część nazwy zestawu rekordów, prefiksem podkreślenia.  Na przykład "\_sip. \_tcp.name ".  Dla rekordu u wierzchołka strefy, jest niepotrzebna, aby określić '@' w nazwie rekordu, po prostu użyć usługa i protokół, np. "\_sip. \_tcp ".
- *Priorytet*, *Grubość*, *port*i *docelowej* są określane jako parametrów każdego rekordu w zestawie rekordów. 

## <a name="tags-and-metadata"></a>Znaczniki i metadanych

### <a name="tags"></a>Znaczniki
Znaczniki są Lista par wartości z pola Nazwa i są używane przez Menedżera zasobów Azure do etykiety zasobów.  Azure Menedżer zasobów używa znaczniki, aby umożliwić filtrowane widoki rachunku Azure i umożliwia również ustawić zasady, na którym tagi są wymagane. Aby uzyskać więcej informacji na temat znaczników zobacz [Używanie znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md).

Azure DNS obsługuje przy użyciu tagów Menedżera zasobów Azure zasobów strefy DNS.  Nie obsługuje znaczniki na zestawach rekordów DNS.

### <a name="metadata"></a>Metadane

Alternatywnie do znaczników zestawu rekordów Azure DNS obsługuje adnotacje zestawy rekordów za pomocą "metadanych".  Podobnie jak znaczniki, metadane umożliwia kojarzenie pary nazwa wartość z każdego zestawu rekordów.  Może to być przydatne, na przykład do rekordu przeznaczenia każdego zestawu rekordów.  W odróżnieniu od znaczniki metadanych nie może służyć do zapewniania widoku filtrowanego rachunku Azure i nie można określić w zasad Azure Menedżera zasobów.

## <a name="limits"></a>Ograniczenia

Podczas przy użyciu systemu DNS Azure, mają zastosowanie następujące ograniczenie domyślne:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Następne kroki

- Aby rozpocząć korzystanie z usługi Azure DNS, Dowiedz się, jak [utworzyć strefy DNS](dns-getstarted-create-dnszone-portal.md) i [Tworzenie rekordów DNS](dns-getstarted-create-recordset-portal.md).
- Aby przeprowadzić migrację istniejącej strefy DNS, Dowiedz się, jak [Importowanie i plik strefy DNS](dns-import-export.md).
