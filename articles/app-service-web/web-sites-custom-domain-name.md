<properties
    pageTitle="Mapowanie niestandardowej nazwy domeny na aplikację Azure"
    description="Dowiedz się, jak mapowanie aplikacji w usłudze Azure aplikacji niestandardowej nazwy domeny (vanity domeny)."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Mapowanie niestandardowej nazwy domeny na aplikację Azure

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

W tym artykule pokazano, jak ręcznie zamapować do aplikacji sieci web, wewnętrznej bazy danych aplikacji dla urządzeń przenośnych lub w [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md)interfejsu API aplikacji niestandardowej nazwy domeny. 

Aplikacji już zawiera unikatowy poddomeną azurewebsites.net. Na przykład jeśli nazwa aplikacji jest **firmy contoso**, jego nazwa domeny jest **contoso.azurewebsites.net**. Jednak można mapować domeny niestandardowej nazwy do aplikacji tak że jego adres URL, takich jak `www.contoso.com`, odzwierciedla wizerunku.

>[AZURE.NOTE] Uzyskaj pomoc od ekspertów Azure na [forach Azure](https://azure.microsoft.com/support/forums/). Aby nawet wyższy poziom pomocy technicznej przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) , a następnie kliknij przycisk **Uzyskiwanie pomocy technicznej**.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>Kup nową domenę niestandardową w Azure portal

Jeśli zakupiono nie jeszcze niestandardowej nazwy domeny, możesz kupić i zarządzać nim bezpośrednio w swojej aplikacji ustawienia w [Azure portal](https://portal.azure.com). Ta opcja ułatwia mapowanie domeny niestandardowej aplikacji, czy [Menedżer ruchu Azure](web-sites-traffic-manager-custom-domain-name.md) jest używany w aplikacji lub nie. 

Aby uzyskać instrukcje zobacz [kupowanie niestandardowej nazwy domeny dla usługi aplikacji](custom-dns-web-site-buydomains-web-app.md).

## <a name="map-a-custom-domain-you-purchased-externally"></a>Mapa domenę niestandardową, który został zakupiony zewnętrznie

Jeśli masz już zakupiony domenę niestandardową z [Azure DNS](https://azure.microsoft.com/services/dns/) lub innej firmy jako dostawcy, istnieją trzy główne kroki w celu zamapowania domeny niestandardowej aplikacji:

1. [Adres IP *(rekord tylko)* pobieranie aplikacji](#vip).
2. [Tworzenie rekordów DNS, które mapowanie domeny do aplikacji](#createdns). 
    - **Gdzie**: swojego własnego rejestratora narzędzia do zarządzania domeną (Azure DNS, GoDaddy, itp.).
    - **Dlaczego**: aby rejestratora domen zna jest odpowiedniej domeny niestandardowej w celu Azure aplikacji.
1. [Włączanie niestandardowej nazwy domeny dla usługi Azure aplikacji](#enable).
    - **Gdzie**: [Azure portal](https://portal.azure.com).
    - **Dlaczego**: Aby aplikacji zna reagować na żądania niestandardowej nazwy domeny.
3. [Propagowanie Sprawdź DNS](#verify).

### <a name="types-of-domains-you-can-map"></a>Typy domen, który można mapować

Azure aplikacji usługi umożliwia mapowanie następujące kategorie domen niestandardowych do aplikacji.

- **Domeny głównej** - nazwę domeny, którą zastrzeżone u rejestratora domen (reprezentowany przez `@` rekordu hosta, zwykle). Na przykład **contoso.com**.
- **Poddomena** - dowolnej domeny, która znajduje się w obszarze domeny głównej. Na przykład **www.contoso.com** (reprezentowany przez `www` rekord hosta).  Różne poddomen tej samej domeny głównej można mapować do innej aplikacji platformy Azure.
- **Symbol wieloznaczny domeny** - [wszelkie poddomeny, którego skrajnej lewej etykieta DNS `*` ](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (np. przechowywać rekordy `*` i `*.blogs`). Na przykład ** \*. contoso.com**.

### <a name="types-of-dns-records-you-can-use"></a>Typy rekordów DNS, których można używać

W zależności od Twoim potrzebom dwa różne rodzaje standardowy rekordy DNS umożliwia mapowanie domeny niestandardowej: 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - mapy niestandardowej nazwy domeny do aplikacji Azure wirtualnych adresów IP adresu bezpośrednio. 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - mapy niestandardowej nazwy domeny do nazwy domeny Azure Twojej aplikacji, * *&lt;*argumentu*>. azurewebsites.net**. 

Zaletą CNAME jest, istnieje przez zmiany adresów IP. Jeśli usuwanie i ponowne tworzenie aplikacji lub zmienianie wyższego poziomu cen wstecz na warstwie **udostępnione** wirtualny adres IP usługi aplikacji może się zmienić. Za pośrednictwem takiej zmiany rekord CNAME jest nadal ważna rekord A wymaga aktualizacji. 

Samouczek przedstawiono kroki dotyczące korzystania z rekord, a także stosowania rekordu CNAME.

>[AZURE.IMPORTANT] Nie należy tworzyć rekord CNAME dla swojej domeny głównej (to znaczy "rekordu głównego"). Aby uzyskać więcej informacji zobacz [Dlaczego nie rekord CNAME można użyć domeny głównej](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Aby zamapować domeny głównej usługi Azure aplikacji, należy użyć rekordu A.

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>Krok 1. *(Tylko rekord)* Uzyskać adres IP aplikacji
Do zamapować niestandardowej nazwy domeny przy użyciu rekordu A, potrzebujesz adresu IP aplikacji Azure. Jeśli zostanie zamapować, zamiast rekordu CNAME, Pomiń ten krok i przejść do następnej sekcji.

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).

2.  Kliknij **Aplikacji usług** w lewym menu.

4.  Kliknij aplikację, a następnie kliknij pozycję **domeny niestandardowe**.

6.  Zwróć uwagę na adres IP powyżej sekcji nazwy hostów.

    ![Mapy niestandardowej nazwy domeny z rekordem: adres IP pobieranie aplikacji Azure aplikacji usługi](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Nie zamykaj ta karta portalu. Będzie powrocie do niego po utworzeniu rekordy DNS.

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>Krok 2. Tworzenie rekordów DNS

Zaloguj się do rejestratora domen i narzędzie Dodawanie A rekord lub rekordu CNAME. Interfejs użytkownika każdej rejestratora jest nieco inne, skontaktuj się z dostawcą dokumentacji. Jednak poniżej przedstawiono ogólne wskazówki.

1.  Znajdź stronę służącą do zarządzania rekordami DNS. Poszukaj łącza lub obszarów witryny etykietą **Nazwy domeny**, **DNS**lub **Zarządzanie serwerem nazw**. Często znajdują się łącza, wyświetlanie informacji o koncie, a następnie szukasz łącza, takie jak **moich domen**.
2.  Poszukaj łącze, które pozwala na dodawanie i edytowanie rekordów DNS. Może to być **plik strefy** lub **Rekordów DNS** łącze lub łącze konfiguracji **Zaawansowane** .
3.  Utwórz rekord i zapisać wprowadzone zmiany.
    - [Instrukcje dotyczące rekordu A są tutaj](#a).
    - [Instrukcje dotyczące rekord CNAME są tutaj](#cname).

<a name="a"></a>
### <a name="create-an-a-record"></a>Utwórz rekord

Aby użyć rekordu A do mapowania aplikacji Azure adres IP, rzeczywiście potrzebujesz ma utworzyć rekord i rekord TXT. Rekord do rozpoznawania nazw DNS, samej, a rekord TXT Azure, aby zweryfikować, że jesteś właścicielem nazwy domeny niestandardowej. 

Konfigurowanie rekordu A następujący (@ zazwyczaj reprezentuje domeny głównej):
 
<table cellspacing="0" border="1">
  <tr>
    <th>Przykład nazwy FQDN</th>
    <th>Hosta</th>
    <th>Wartość</th>
  </tr>
  <tr>
    <td>contoso.com (główny)</td>
    <td>@</td>
    <td>Adres IP z <a href="#vip">krok 1</a></td>
  </tr>
  <tr>
    <td>www.contoso.com (pod)</td>
    <td>"www"</td>
    <td>Adres IP z <a href="#vip">krok 1</a></td>
  </tr>
  <tr>
    <td>*. contoso.com (symbol wieloznaczny)</td>
    <td>*</td>
    <td>Adres IP z <a href="#vip">krok 1</a></td>
  </tr>
</table>

Dodatkowe rekord TXT przejmuje Konwencji mapy z &lt; *poddomeny*>. &lt; *rootdomain*> do &lt; *argumentu*>. azurewebsites.net. Konfigurowanie rekordu TXT w następujący sposób:

<table cellspacing="0" border="1">
  <tr>
    <th>Przykład nazwy FQDN</th>
    <th>TXT hosta</th>
    <th>Wartość TXT</th>
  </tr>
  <tr>
    <td>contoso.com (główny)</td>
    <td>@</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (pod)</td>
    <td>"www"</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (symbol wieloznaczny)</td>
    <td>*</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
###Utwórz rekord CNAME

Jeśli używasz rekord CNAME do mapowania aplikacji Azure domyślną nazwę domeny, nie musisz dodatkowy rekord TXT jak rekord. 

>[AZURE.IMPORTANT] Nie należy tworzyć rekord CNAME dla swojej domeny głównej (to znaczy "rekordu głównego"). Aby uzyskać więcej informacji zobacz [Dlaczego nie rekord CNAME można użyć domeny głównej](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Aby zamapować domeny głównej usługi Azure aplikacji, należy użyć [rekord](#a) .

Konfigurowanie rekordu CNAME w następujący sposób (@ zazwyczaj reprezentuje domeny głównej):

<table cellspacing="0" border="1">
  <tr>
    <th>Przykład nazwy FQDN</th>
    <th>CNAME Host</th>
    <th>Wartość CNAME</th>
  </tr>
  <tr>
    <td>www.contoso.com (pod)</td>
    <td>"www"</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (symbol wieloznaczny)</td>
    <td>*</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
##Krok 3. Włączanie niestandardowej nazwy domeny dla aplikacji

Po powrocie do karta **Domen niestandardowych** w portalu Azure (zobacz [Krok 1](#vip)), musisz dodać w pełni kwalifikowaną nazwę domeny (FQDN) domeny niestandardowej do listy.

1.  Jeśli nie zostało to zrobione, zaloguj się do [portalu Azure](https://portal.azure.com).

2.  W portalu usługi Azure kliknij **Aplikacji usług** w lewym menu.

3.  Kliknij aplikację, a następnie kliknij pozycję **domeny niestandardowe** > **Dodaj nazwa hosta**.

4.  Dodawanie nazwy FQDN własnej domeny do listy (przykład **www.contoso.com**).

    ![Mapowanie niestandardowej nazwy domeny na aplikację Azure: dodawanie do listy nazw domen](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure spróbuje Sprawdź nazwę domeny, którego używasz, w tym miejscu. Upewnij się, że jest tą samą nazwą domeny, dla którego utworzono rekordu DNS w [kroku 2](#createdns). 

5.  Kliknij przycisk **Sprawdzanie poprawności**.

6.  Po kliknięciu Azure **sprawdzania poprawności** będzie rozpoczynanie weryfikacji domeny przepływu pracy. To sprawdza własności domeny, a także sukces dostępności i raport nazwa hosta lub szczegółowe błąd wskazowki porady na temat poprawienia błędu.    

7.  Po sprawdzeniu poprawności **hostname Dodaj** przycisk stanie się aktywny, i będzie mógł przypisywanie nazwa hosta. 

8.  Po zakończeniu Azure Konfigurowanie nowej niestandardowej nazwy domeny przejdź do niestandardowej nazwy domeny w przeglądarce. Przeglądarki należy otworzyć Azure aplikacji, co oznacza, że niestandardowej nazwy domeny jest poprawnie skonfigurowany.

> [AZURE.NOTE] Jeśli rekord DNS jest już używać (scenariusz ruch obsługi aktywnej domeny) i musisz preemptively powiązać aplikacji sieci web go do weryfikacji domeny, po prostu utworzyć rekordy TXT jako przykłady pokazano w poniższej tabeli. Dodatkowe rekord TXT przejmuje Konwencji mapy z &lt; *poddomeny*>. &lt; *rootdomain*> do &lt; *argumentu*>. azurewebsites.net. 
> <table cellspacing="0" border="1">
  <tr>
    <th>Przykład nazwy FQDN</th>
    <th>TXT hosta</th>
    <th>Wartość TXT</th>
  </tr>
  <tr>
    <td>contoso.com (główny)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (pod)</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
    <tr>
    <td>*. contoso.com (pod)</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>argumentu</i>>. azurewebsites.net</td>
  </tr>
</table>
Po utworzeniu rekordu DNS, wróć do Azure portal i Dodawanie niestandardowej nazwy domeny do aplikacji sieci web.
 

<a name="verify"></a>
##Sprawdź propagowanie DNS

Po wykonaniu kroków konfiguracji, może minąć trochę czasu, aby zmiany propagowane, w zależności od dostawcy DNS. Można sprawdzić, czy propagowanie DNS działa zgodnie z oczekiwaniami przy użyciu [http://digwebinterface.com/](http://digwebinterface.com/). Po przejściu do witryny, określanie nazwy hostów w polu tekstowym, a następnie kliknij przycisk **Drąż**. Sprawdź wyniki, aby potwierdzić, jeśli zawiera ostatnie zmiany zostały wprowadzone.  

![Mapowanie niestandardowej nazwy domeny na aplikację Azure: Sprawdź propagowanie DNS](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Propagowanie wpisy DNS może zająć do 48 godzin (czasami już). Jeśli wszystkie elementy zostały skonfigurowane poprawnie, musisz nadal poczekaj, aż propagowanie powiodła się.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak bezpiecznego niestandardowej nazwy domeny przy użyciu protokołu HTTPS, [kupując certyfikatu SSL w Azure](web-sites-purchase-ssl-web-site.md) lub [używanie certyfikatu SSL z innego miejsca](web-sites-configure-ssl-certificate.md).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

[Rozpoczynanie pracy z usługą DNS Azure](../dns/dns-getstarted-create-dnszone.md)  
[Tworzenie rekordów DNS dla aplikacji sieci web w domenie niestandardowej](../dns/dns-web-sites-custom-domain.md)  
[Pełnomocnik domeny DNS Azure](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
