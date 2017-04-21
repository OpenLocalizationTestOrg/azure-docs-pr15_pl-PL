#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Konfigurowanie niestandardowej nazwy domeny dla witryny sieci Web Azure

Podczas tworzenia witryny sieci Web Azure udostępnia przyjazny poddomeny w domenie azurewebsites.net, więc usługi Użytkownicy mają dostęp do witryny sieci Web przy użyciu adresu URL, takie jak http://&lt;w witrynie Moja witryna >. azurewebsites.net. Jednak jeśli skonfigurujesz witrynami sieci Web usługi udostępnione lub trybie standardowym, można mapować witryny sieci Web do własnej nazwy domeny.

Opcjonalnie możesz Azure ruch Menedżer służy do równoważenia obciążenia przychodzących ruchu do witryny sieci Web. Aby uzyskać więcej informacji o tym, jak działa Menedżer ruchu z witrynami sieci Web, zobacz [Kontrolowanie ruch witryn sieci Web Azure przy użyciu Menedżera ruch Azure][trafficmanager].

> [AZURE.NOTE] Procedury w tym zadaniu mają zastosowanie do witryny sieci Web Azure; dla usług w chmurze zobacz <a href="/develop/net/common-tasks/custom-dns/">Konfigurowanie nazwy domeny niestandardowej w Azure</a>.

> [AZURE.NOTE] Kroki opisane w tym zadaniu trzeba skonfigurować witrynami sieci Web usługi udostępnione lub trybie standardowym mogą ulec zmianie, ile konta dla subskrypcji. Aby uzyskać więcej informacji, zobacz <a href="/pricing/details/web-sites/">Szczegóły ceny witryn sieci Web</a> .

W tym artykule:

-   [Opis rekordów CNAME i A](#understanding-records)
-   [Konfigurowanie witryn sieci web w trybie udostępnionych lub standardowy](#bkmk_configsharedmode)
-   [Dodawanie witryn sieci web do Menedżera ruchu](#trafficmanager)
-   [Dodawanie CNAME dla domeny niestandardowej](#bkmk_configurecname)
-   [Dodawanie rekordu A dla domeny niestandardowej](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Opis rekordów CNAME i A</h2>

CNAME (lub rekordów aliasów), a rekordy obu umożliwiają kojarzenie nazwy domeny z witryny sieci Web, jednak każdy działają inaczej.

###<a name="cname-or-alias-record"></a>Rekord CNAME lub Alias

Rekord CNAME mapy *określonej* domeny, na przykład **contoso.com** lub **www.contoso.com**z nazwą domeny kanonicznych. W tym przypadku nazwa kanoniczna domeny jest ** &lt;MojaApl >. azurewebsites.net** nazwy domeny Azure witryny sieci Web lub ** &lt;MojaApl >. trafficmgr.com** nazwy domeny profilu Menedżer ruchu. Po utworzeniu CNAME tworzy alias dla ** &lt;MojaApl >. azurewebsites.net** lub ** &lt;MojaApl >. trafficmgr.com** nazwy domeny. Wpis CNAME rozwiąże adres IP swojego ** &lt;MojaApl >. azurewebsites.net** lub ** &lt;MojaApl >. trafficmgr.com** nazwy domeny na automatycznie, dzięki czemu zmiana adresu IP witryny sieci Web nie musi podejmować żadnych działań.

> [AZURE.NOTE] Niektóre rejestratorów domen Zezwalaj tylko do zamapować poddomen, używając rekordu CNAME, na przykład www.contoso.com, a nie głównej nazwy, na przykład contoso.com. Aby uzyskać więcej informacji dotyczących rekordów CNAME zapoznaj się z dokumentacją, dostarczony przez rejestratora, <a href="http://en.wikipedia.org/wiki/CNAME_record">Zapis Wikipedia rekord CNAME</a>lub dokument <a href="http://tools.ietf.org/html/rfc1035">IETF domen — Implementacja i specyfikacja</a> .

###<a name="a-record"></a>Rekord

Rekord A map domenę, na przykład **contoso.com** lub **www.contoso.com**, *lub domena symboli* takich jak ** \*. contoso.com**, adres IP. W przypadku witryny sieci Web Azure wirtualnych adresów IP usługi lub określony adres IP adres istnieje zakupionych dla witryny sieci Web. Dlatego głównych korzyści, jakie rekord A nad rekord CNAME ma mieć jeden wpis, która korzysta z symboli wieloznacznych, takich jak * **. contoso.com**, które chcesz obsługiwać żądań wiele domen podrzędnych takich jak * *mail.contoso.com**; * *login.contoso.com**, lub * *www.contso.com**.

> [AZURE.NOTE] Ponieważ rekordu A jest mapowane statyczny adres IP, nie można automatycznie rozpoznać zmiany adresu IP witryny sieci Web. Adres IP do użytku z rekordami znajduje się po skonfigurowaniu ustawień nazwy domeny niestandardowej dla witryny sieci Web; Jednak tę wartość mogą zmieniać usuwanie i ponowne tworzenie witryny sieci Web lub zmień tryb witryny sieci Web, aby z powrotem do zwolnienia.

> [AZURE.NOTE] Nie można użyć rekordów przy użyciu Menedżera ruchu równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Kontrolowanie ruch witryn sieci Web Azure przy użyciu Menedżera ruch Azure][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Konfigurowanie witrynach sieci Web w trybie udostępnionych lub standardowy</h2>

Ustawianie niestandardowej nazwy domeny w witrynie sieci Web jest dostępna tylko dla udostępnione i standardowy trybów Azure witryn sieci Web. Przed przełączeniem witryny sieci Web z trybu bezpłatną witrynę sieci Web do udostępniania lub Tryb standardowy witryny sieci Web, musisz najpierw usunąć wydatków naciśnij klawisze Caps Lock w miejscu dla subskrypcji witryny sieci Web. Aby uzyskać więcej informacji na udostępnione i standardowe ceny tryb szczegółowe [Ceny][PricingDetails].

1. W przeglądarce, otwórz [Portalu zarządzania][portal].
2. Na karcie **witryny sieci Web** kliknij nazwę witryny.

    ![][standardmode1]

3. Kliknij kartę **Skala** .

    ![][standardmode2]


4. W sekcji **Ogólne** Ustaw tryb witryny sieci Web, klikając pozycję **UDOSTĘPNIONE**.

    ![][standardmode3]

    > [AZURE.NOTE] Jeśli będzie używany Menedżer ruchu z tej witryny sieci Web, wybierz tryb standardowy należy użyć zamiast udostępnione.

5. Kliknij przycisk **Zapisz**.
6. Po wyświetleniu monitu o wzrost kosztu tryb udostępniania (lub trybie standardowym, jeśli wybierzesz standardowe), kliknij przycisk **Tak** , jeśli użytkownik zgadza się.

    <!--![][standardmode4]-->

    **Uwaga**<br />
   Jeśli zostanie wyświetlony komunikat o błędzie "Konfigurowanie skala dla witryny sieci Web"Nazwa witryny sieci Web"nie powiodło się", możesz użyć przycisku Szczegóły, aby uzyskać więcej informacji.

<a name="trafficmanager"></a><h2>(Opcjonalnie) Dodawanie do witryny sieci Web do Menedżera ruchu</h2>

Jeśli chcesz użyć swojej witryny sieci Web przy użyciu Menedżera ruch, wykonaj następujące czynności.

1. Jeśli nie masz już profil Menedżera ruchu, korzystając z informacji w sekcji [Tworzenie profilu Menedżer ruchu za pomocą szybkiego tworzenia] [ createprofile] go utworzyć. Uwaga **. trafficmgr.com** nazwy domeny skojarzonej z profilu Menedżer ruchu. Będzie on używany w późniejszym etapie.

2. Skorzystaj z informacji w oknie [Dodawanie lub usuwanie punkty końcowe] [ addendpoint] Dodawanie witryny sieci Web jako punkt końcowy w swoim profilu Menedżer ruchu.

    > [AZURE.NOTE] Jeśli witryny sieci Web nie jest wyświetlana podczas dodawania punktu końcowego, upewnij się, że nie jest skonfigurowana do trybie standardowym. Praca z menedżerem ruchu, należy użyć standardowego trybu dla witryny sieci Web.

3. Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

4. Teraz znaleźć miejsce, w którym można wybrać lub wprowadzić rekordy CNAME. Być może trzeba wybrać typ rekordu, z listy w dół lub przejdź do strony ustawień zaawansowanych. Należy szukać wyrazy **CNAME** **Alias**lub **poddomeny**.

5. Musi także udostępnić domeny lub poddomeny alias CNAME. Na przykład **"www"** , jeśli chcesz utworzyć aliasu dla **www.customdomain.com**.

5. Również musisz podać nazwę hosta, która jest nazwą domeny kanonicznych dla tego aliasu CNAME. Jest to **. trafficmgr.com** nazwę witryny sieci Web.

Na przykład następujący rekord CNAME przekazuje cały ruch z **www.contoso.com** , aby **contoso.trafficmgr.com**, nazwę domeny w witrynie sieci Web:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias-hosta Nazwa poddomeny</strong></td>
<td><strong>Kanonicznych domeny</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

PRAWDA hosta (contoso.azurewebsite.net), nie będzie widoczna w odwiedzającego **www.contoso.com** , więc procesu przesyłania dalej jest niewidoczne dla użytkownika końcowego.

> [AZURE.NOTE] Jeśli korzystasz z Menedżer ruchu z witryny sieci Web, nie musisz wykonaj kroki opisane w poniższych sekcjach "**Dodaj CNAME dla domeny niestandardowej**" i "**Dodaj rekord A dla domeny niestandardowej**". Rekord CNAME utworzonym w poprzednich krokach będzie skierować ruch przychodzący do Menedżera ruch, który kieruje ruch do witryny sieci Web endpoint(s).

<a name="bkmk_configurecname"></a><h2>Dodawanie CNAME dla domeny niestandardowej</h2>

Do utworzenia rekordu CNAME, użytkownik musi Dodaj nowy wpis w tabeli DNS dla domeny niestandardowej za pomocą narzędzi dostarczony przez rejestratora. Każdy rejestrator zawiera podobne, ale nieco inną metodę określania rekordu CNAME, ale pojęcia są takie same.

1. Użyj jednej z następujących metod znajdowanie **. azurewebsite.net** nazwy domeny przypisane do witryny sieci Web.

    * Zaloguj się do [Portalu zarządzania Azure][portal]wybierz swoją witrynę internetową, wybierz **pulpit nawigacyjny**, a następnie znajdź pozycję **Adres URL witryny** w sekcji **Szybkie rzut** .

    * Instalowanie i konfigurowanie [Azure programu Powershell](/manage/install-and-configure-windows-powershell/), a następnie użyj następujące polecenie:

            get-azurewebsite yoursitename | select hostnames

    * Instalowanie i konfigurowanie [Azure interfejs wiersza polecenia](/manage/install-and-configure-cli/), a następnie użyj następującego polecenia:

            azure site domain list yoursitename

    Zapisz to **. azurewebsite.net** nazwę, ponieważ będzie używana w następującej procedurze.

3. Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

4. Teraz znaleźć miejsce, w którym można wybrać lub wprowadzić rekordy CNAME. Być może trzeba wybrać typ rekordu, z listy w dół lub przejdź do strony ustawień zaawansowanych. Należy szukać wyrazy **CNAME** **Alias**lub **poddomeny**.

5. Musi także udostępnić domeny lub poddomeny alias CNAME. Na przykład **"www"** , jeśli chcesz utworzyć aliasu dla **www.customdomain.com**. Jeśli chcesz utworzyć aliasu dla domeny głównej, może być dostępna jako "**@**" symbol w narzędziach DNS rejestratora.

5. Również musisz podać nazwę hosta, która jest nazwą domeny kanonicznych dla tego aliasu CNAME. Jest to **. azurewebsite.net** nazwę witryny sieci Web.

Na przykład następujący rekord CNAME przekazuje cały ruch z **www.contoso.com** , aby **contoso.azurewebsite.net**, nazwę domeny w witrynie sieci Web:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias-hosta Nazwa poddomeny</strong></td>
<td><strong>Kanonicznych domeny</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>contoso.azurewebsite.NET</td>
</tr>
</table>

PRAWDA hosta (contoso.azurewebsite.net), nie będzie widoczna w odwiedzającego **www.contoso.com** , więc procesu przesyłania dalej jest niewidoczne dla użytkownika końcowego.

> [AZURE.NOTE] Powyższym przykładzie dotyczy tylko ruchu w poddomeny __ciągu "www"__ . Ponieważ nie można używać symboli wieloznacznych rekordów CNAME, należy utworzyć jeden CNAME dla każdej domeny i poddomeny. Jeśli chcesz przekierowanie ruchu z poddomen, takich jak *. contoso.com, pod Twoim adresem azurewebsite.net możesz skonfigurować wpis __Przekierowania adresu URL__ lub __Adres URL do przodu__ w ustawień DNS, lub Utwórz rekord.

> [AZURE.NOTE] Może upłynąć trochę czasu dla swojego CNAME propagowanie w systemie DNS. Nie można skonfigurować CNAME dla witryny sieci Web, dopóki jest propagowana CNAME. Za pomocą usług, takich jak <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> CNAME jest dostępna.

###<a name="add-the-domain-name-to-your-website"></a>Dodaj nazwę domeny do witryny sieci Web

Po propagowane ma rekord CNAME dla nazwy domeny, musisz skojarzyć go z witryny sieci Web. Możesz dodać nazwę domeny niestandardowej określonego rekordu CNAME do witryny sieci Web przy użyciu albo Azure interfejsu wiersza polecenia (polecenie Azure) lub za pomocą portalu zarządzania Azure.

**Aby dodać nazwę domeny przy użyciu narzędzia wiersza polecenia**

Zainstalować i skonfigurować [Azure interfejs wiersza polecenia](/manage/install-and-configure-cli/), a następnie użyj następujące polecenie:

    azure site domain add customdomain yoursitename

Na przykład poniższa doda nazwę domeny niestandardowej **www.contoso.com** **contoso.azurewebsite.net** witryny sieci Web:

    azure site domain add www.contoso.com contoso

Aby potwierdzić, że nazwa domeny niestandardowej został dodany do witryny sieci Web przy użyciu następującego polecenia:

    azure site domain list yoursitename

Lista zwracane przez polecenie powinien zawierać nazwę domeny niestandardowej, a także domyślny **. azurewebsite.net** wpisu.

**Aby dodać nazwę domeny za pomocą portalu zarządzania Azure**

1. W przeglądarce, otwórz [Portal Azure zarządzania][portal].

2. Na karcie **witryny sieci Web** kliknij nazwę witryny, wybierz **pulpit nawigacyjny**, a następnie u dołu strony wybierz pozycję **Zarządzanie domenami** .

    ![][setcname2]

6. W polu tekstowym **Nazw DOMEN** wpisz nazwę domeny, który został skonfigurowany.

    ![][setcname3]

6. Kliknij znacznik wyboru, aby zaakceptować nazwę domeny.

Po zakończeniu konfiguracji niestandardowej nazwy domeny zostaną zapisane w sekcji **nazw domen** na stronie **Konfigurowanie** witryny sieci Web.

<a name="bkmk_configurearecord"></a><h2>Dodawanie rekordu dla domeny niestandardowej</h2>

Aby utworzyć rekord, możesz znaleźć adres IP witryny sieci Web. Następnie dodaj wpis w tabeli DNS dla domeny niestandardowej za pomocą narzędzi dostarczony przez rejestratora. Każdy rejestrator zawiera podobne, ale nieco inną metodę określania rekordu A, ale pojęcia są takie same. Oprócz tworzenia rekordu A, musisz też utworzyć rekord CNAME Azure używaną do weryfikacji rekord.

1. W przeglądarce, otwórz [Portal Azure zarządzania][portal].

2. Na karcie **witryny sieci Web** kliknij nazwę witryny, wybierz **pulpit nawigacyjny**, a następnie wybierz pozycję **Zarządzanie domenami** od dołu ekranu.

    ![][setcname2]

5. W oknie dialogowym **Zarządzanie domen niestandardowych** Znajdź **Adres IP do użycia podczas konfigurowania rekordów**. Skopiuj adres IP. Będzie on używany podczas tworzenia rekordu A.

5. W oknie dialogowym **Zarządzanie domen niestandardowych** Uwaga awverify nazwy domeny na końcu tekstu znajdującego się w górnej części okna dialogowego. Należy **awverify.mysite.azurewebsites.net** , gdzie **w witrynie Moja witryna** jest nazwą witryny sieci Web. Kopiowanie, tak jak nazwę domeny używaną podczas tworzenia weryfikacji rekordu CNAME.

6. Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

6. Znajdź miejsce, w którym można wybrać lub wprowadzić rekordy A i CNAME. Być może trzeba wybrać typ rekordu, z listy w dół lub przejdź do strony ustawień zaawansowanych.

7. Wykonaj poniższe czynności, aby utworzyć rekord:

    1. Wybierz lub wprowadź domena lub poddomena, który będzie używany rekordu A. Na przykład zaznacz **ciągu "www"** , jeśli chcesz utworzyć aliasu dla **www.customdomain.com**. Jeśli chcesz utworzyć wpis symboli wieloznacznych dla wszystkich poddomen, wprowadź "__*__". Obejmuje to wszystkie domeny podrzędne, takie jak **mail.customdomain.com**, **login.customdomain.com**oraz **www.customdomain.com**.

        Jeśli chcesz utworzyć rekord A dla domeny głównej, może być dostępna jako "**@**" symbol w narzędziach DNS rejestratora.

    2. Wprowadź adres IP usługi w chmurze w polu dostarczonych. Skojarzenie wpis domeny używane w rekordzie o adresie IP wdrożenia usługi cloud.

        Na przykład po rekordu przekazuje cały ruch z **contoso.com** do **137.135.70.239**, adres IP naszych wdrożonej aplikacji:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Nazwa hosta/poddomeny</strong></td>
        <td><strong>Adres IP</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        W tym przykładzie przedstawiono tworzenia rekordu A dla domeny głównej. Jeśli chcesz utworzyć wpis symbol wieloznaczny dotyczyć wszystkich poddomen, należy wpisać "__*__" jako poddomena.

7. Następnie utwórz rekord CNAME alias **awverify**i domenę kanonicznych **awverify.mysite.azurewebsites.net** uzyskany wcześniej.

    > [AZURE.NOTE] Gdy aliasu awverify może działać w przypadku niektórych rejestratorów, inne osoby mogą wymagać alias pełną nazwę domeny awverify.www.customdomainname.com lub awverify.customdomainname.com.

    Na przykład poniższa utworzy Azure umożliwia sprawdzenie konfiguracji rekordu A rekord CNAME.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias-hosta Nazwa poddomeny</strong></td>
    <td><strong>Kanonicznych domeny</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Może upłynąć trochę czasu, awverify CNAME propagowanie w systemie DNS. Nie można skonfigurować nazwę domeny niestandardowej zdefiniowanych przez rekord A dla witryny sieci Web, aż awverify CNAME jest propagowana. Za pomocą usług, takich jak <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> CNAME jest dostępna.

###<a name="add-the-domain-name-to-your-website"></a>Dodaj nazwę domeny do witryny sieci Web

Po jest propagowana **awverify** rekordu CNAME dla nazwy domeny, możesz skojarzyć domeny niestandardowej zdefiniowanych przez rekord z witryną sieci Web. Możesz dodać nazwę domeny niestandardowej zdefiniowanych przez rekord do witryny sieci Web przy użyciu polecenie Azure lub za pomocą portalu zarządzania Azure.

**Aby dodać nazwę domeny przy użyciu interfejsu wiersza polecenia Azure (polecenie Azure)**

Instalowanie i konfigurowanie [Polecenie Azure](/manage/install-and-configure-cli/), a następnie użyj następujące polecenie:

    azure site domain add customdomain yoursitename

Na przykład poniższa spowoduje dodanie niestandardowej nazwy domeny **contoso.com** **contoso.azurewebsite.net** witryny sieci Web:

    azure site domain add contoso.com contoso

Aby potwierdzić, że nazwa domeny niestandardowej został dodany do witryny sieci Web przy użyciu następującego polecenia:

    azure site domain list yoursitename

Lista zwracane przez polecenie powinien zawierać nazwę domeny niestandardowej, a także domyślny **. azurewebsite.net** wpisu.

**Aby dodać nazwę domeny za pomocą portalu zarządzania Azure**

1. W przeglądarce, otwórz [Portal Azure zarządzania][portal].

2. Na karcie **witryny sieci Web** kliknij nazwę witryny, wybierz **pulpit nawigacyjny**, a następnie u dołu strony wybierz pozycję **Zarządzanie domenami** .

    ![][setcname2]

6. W polu tekstowym **Nazw DOMEN** wpisz nazwę domeny, który został skonfigurowany.

    ![][setcname3]

6. Kliknij znacznik wyboru, aby zaakceptować nazwę domeny.

Po zakończeniu konfiguracji niestandardowej nazwy domeny zostaną zapisane w sekcji **nazw domen** na stronie **Konfigurowanie** witryny sieci Web.

> [AZURE.NOTE] Po dodaniu niestandardowej nazwy domeny zdefiniowanych przez rekord do witryny sieci Web, możesz usunąć rekord CNAME awverify, korzystając z narzędzi dostępnych przez rejestratora. Jednak jeśli chcesz dodać A innego rekordu w przyszłości, musisz ponownie utworzyć awverify rekordu przed możesz skojarzyć nową nazwę domeny zdefiniowanych przez nowy rekord z witryny sieci Web.

## <a name="next-steps"></a>Następne kroki

-   [Jak zarządzać witryn sieci web](/manage/services/web-sites/how-to-manage-websites/)

-   [Konfigurowanie certyfikatu SSL dla witryny sieci Web](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
