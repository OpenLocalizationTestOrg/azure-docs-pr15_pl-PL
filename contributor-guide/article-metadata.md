

#<a name="metadata-for-azure-technical-articles"></a>Metadane Azure artykuły techniczne

Wszystkie artykuły techniczne Azure zawierać dwie sekcje metadanych - właściwości i sekcji znaczniki. W sekcji właściwości umożliwia niektóre witryny sieci Web automatyzacji i rzeczy funkcji optymalizacji aparatu wyszukiwania podczas sekcji znaczniki udostępnia wiele wewnętrznych raportowania zawartości. Sekcjami są wymagane.

- [W składni]
- [Użycie]
- [Atrybuty i wartości w sekcji właściwości]
- [Atrybuty i wartości dla sekcji znaczników]

##<a name="syntax"></a>W składni

W sekcji właściwości używa następującej składni:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

W sekcji znaczniki używa następującej składni:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Użycie

- Nazwa elementu i nazwy atrybutów jest uwzględniana wielkość liter.
- Na <properties> sekcja musi być pierwszy wiersz pliku.
- Pozostawić pusty wiersz po każdej sekcji metadanych i przed Twój tytuł strony, aby upewnić się, że tytuł strony jest poprawnie przekonwertować na format HTML podczas procesu publikowania.

## <a name="attributes-and-values-for-the-properties-section"></a>Atrybuty i wartości w sekcji właściwości

![](./media/article-metadata/checkmark-small.png)**Tytuł strony**: wymagane; Ważne Aby funkcji optymalizacji aparatu wyszukiwania. Tekst dla tego atrybutu jest wyświetlany, na karcie w przeglądarce i jako tytuł w wynikach wyszukiwania. Używanie 55 – 60 znaków, włącznie ze spacjami i łącznie z identyfikatorem witryny *| Microsoft Azure* (wpisany jako: miejsce miejsca potoku Microsoft Azure).  Tytuł strony powinny różnić się od H1.

![](./media/article-metadata/checkmark-small.png)**Opis**: wymagane; ważna w przypadku funkcji optymalizacji aparatu wyszukiwania (istotności) i funkcji dostępnych w witrynie. Opis powinien być co najmniej 125 znaków długi do 155 znaków maksymalną, włącznie ze spacjami. Opisz przeznaczenie zawartości, aby klienci będą wiedzieć, czy chcesz go wybrać z listy wyników wyszukiwania. Wartość jest:

- Ten tekst może być wyświetlany jako opis lub abstrakcyjne akapitu w wynikach wyszukiwania w Google.
- Ten tekst jest wyświetlany w [artykule wyniki indeksu](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**usługi**: wymagane dla artykułów, które zajmują się usługi. Ta wartość jest ofter nazywane "pracy usluga". Lista wszystkich stosownych usług, rozdzielając je przecinkami. Pierwszej usługi na liście będą sterują nawigacyjne łącza do stron nadrzędnych dla strony i nawigacji po lewej stronie, która jest wyświetlonej na stronie.

W artykułach określające zarówno wartości usług i documentationCenter wartość usług będzie sterują łącza do stron nadrzędnych. Dodatkowe wartości wyświetlane liście jako znaczniki w artykule opublikowanych. Wartości:

- usługi Active directory
- aktywne — katalogu b2c
- aktywne — katalogu — ds
- service\api aplikacji
- Interfejs API zarządzania
- Aplikacja usługi
- servic\mobile aplikacji
- service\web aplikacji
- service\logic aplikacji
- Brama aplikacji
- więcej informacji aplikacji
- automatyzacji
- Azure portal
- Kierownik Azure
- stos Azure
- wykonywanie kopii zapasowych
- partii
- Najważniejsze wskazówki
- BizTalk usług
- pamięć podręczna
- sieci CDN
- usług w chmurze
- wykaz danych
- factory danych
- dane lake — analizy
- dane lake magazynu
- Ćwiczenia devtest
- DNS
- documentdb
- expressroute
- Koncentratory zdarzenia
- Usługa hdinsight
- Centrum iot
- Klucz magazynu
- usługi równoważenia obciążenia
- Nauka komputera
- Marketplace
- usługi multimediów
- Telefon komórkowy zaangażowania
- Telefon komórkowy usług
- wieloskładnikowe uwierzytelnianie
- powiadomienie o koncentratory
- operacyjne wniosków
- operacje — Zarządzanie — pakiet
- powerapps
- Menedżer odzyskiwania
- redis pamięci podręcznej
- Funkcja RemoteApp
- Zarządzanie prawami
- Harmonogram
- Wyszukiwanie
- Centrum zabezpieczeń
- Usługa bus
- Usługa tkaninie
- Odzyskiwanie witryny
- Baza danych SQL
- magazynu w przypadku danych SQL
- raportowanie SQL
- miejsca do magazynowania
- przechowywanie
- storsimple
- analizy strumieniu
- Menedżer ruchu
- maszyn wirtualnych
- wirtualna sieć
- Visual studio — w trybie online
- Brama VPN
- witryny sieci Web

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: wymagane dla deweloperów na artykuły najlepsze polecane za pośrednictwem Centrum deweloperów. Określ Centrum deweloperów jednego lub języka, którego dotyczy tego artykułu. Wartość, która lista będzie sterują nawigacyjne łącza do stron nadrzędnych dla strony. W artykułach określające zarówno wartości usług i documentationCenter wartość usług będzie sterują łącza do stron nadrzędnych. Wartości:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **dopiskiem**
- **urządzeń przenośnych**: przestarzałe. Zamień określonej platformy urządzeń przenośnych.
- **IOS**: Verifing nowej wartości
- **android**: Sprawdzanie nowej wartości
- **Windows**: Sprawdzanie nowej wartości
- **xamarin**: Sprawdzanie nowej wartości

![](./media/article-metadata/checkmark-small.png)**autorzy**: wymagane tylko jedną wartość. Listy konto GitHub główny autor lub artykuł małych i średnich przedsiębiorstw. Ten atrybut dysków wiersz z nazwiskiem autora na opublikowany artykuł. Tylko jeden pomimo liczba mnoga nazwy atrybutu listy.

![](./media/article-metadata/checkmark-small.png)**Menedżer**: wymagane, jeśli jesteś współautora firmy Microsoft. Lista alias e-mail Menedżer publikowania zawartości obszaru technologii. Jeśli współautora społeczności, dołączyć atrybut, ale pozostawić go puste, możemy wypełnij go tak.

![](./media/article-metadata/checkmark-small.png)**Edytor**: nieużywany. Nie należy używać go do innych celów.

![](./media/article-metadata/checkmark-small.png)**znaczniki**: opcjonalne. Dołączanie tylko wtedy, gdy chcesz włączyć łącze w artykule łącza do stron nadrzędnych ze stroną artykuł indeks (http://azure.microsoft.com/documentation/articles/) do listy prefiltered artykułów, które są zgodne z jedną z zatwierdzony wartości. Te wartości są przeznaczone do umożliwiają grupowania zawartości w przypadku zawartości grupowania nie jest specyficzna dla usługi. Znaczniki można również dołączyć etykiety, który wskazuje stos technologii, których dotyczy ten artykuł. Ta wartość **nie** obsługuje znaczniki dowolnych lub hashtags; znaczniki musi być włączony w witrynie. Można podać wiele wartości znaczniki do wielu artykułów, rozdzielając je przecinkami. Zatwierdzone wartości są następujące:

  - Architektura
  - Kierownik Azure
  - Azure usługi zarządzania
  - rozliczenia
  - MySQL

![](./media/article-metadata/checkmark-small.png)**słowa kluczowe**: opcjonalne. Do użytku przez tylko champs funkcji optymalizacji aparatu wyszukiwania. Rozdziel terminy przecinkami. **Skontaktuj się z usługi champ funkcji optymalizacji aparatu wyszukiwania, przed zmienianie lub usuwanie zawartości w tym artykule zawierające tych terminów.** Ten atrybut rekordów słów kluczowych champ funkcji optymalizacji aparatu wyszukiwania zwraca się i są śledzone w celu poprawy pozycja wyszukiwania. Słowa kluczowe nie są renderowane w opublikowany kod HTML. Sprawdzanie poprawności nie wymaga tego atrybutu.

## <a name="attributes-and-values-for-the-tags-section"></a>Atrybuty i wartości dla sekcji znaczników

![](./media/article-metadata/checkmark-small.png)**MS.Service**: wymagane. Określa usługi Azure, narzędzie lub funkcją, której dotyczy tego artykułu. Jedną wartość na każdej stronie.

 Jeśli strona ma zastosowanie do wielu usług, wybierz usługę, do których najczęściej dotyczy bezpośrednio; na przykład artykułu, który używa aplikacji hostowanej w witrynach sieci web wykazać Bus usługi funkcji powinien mieć wartość **bus usługi** , a nie **witryny sieci web**. Jeśli strona ma zastosowanie do wielu usług jednakowo, wybierz pozycję **wiele**. Jeśli strona nie ma zastosowania do jakichkolwiek usług (będzie rzadko), wybierz pozycję **Brak**.

 - **usługi Active directory**
 - **aktywne — katalogu b2c**
 - **aktywne — katalogu — ds**
 - **Interfejs API zarządzania**
 - **Aplikacja usługi**: dotyczy tylko ogólne materiały koncepcyjne w aplikacji usługi
 - **Interfejs api usługi aplikacji**
 - **Aplikacja — usługa logiczny**
 - **Aplikacja usługi mobilny**
 - **Aplikacja usługi sieci web**
 - **więcej informacji aplikacji**
 - **Brama aplikacji**
 - **automatyzacji**
 - **Kierownik Azure**
 - **zabezpieczenia Azure**
 - **stos Azure**
 - **wykonywanie kopii zapasowych**
 - **partii**
 - **Najważniejsze wskazówki**
 - **BizTalk usług**
 - **rozliczenia**
 - **pamięć podręczna**
 - **sieci CDN**
 - **usług w chmurze**
 - **wykaz danych**
 - **dane lake magazynu**
 - **dane lake — analizy**
 - **Ćwiczenia devtest**
 - **expressroute**
 - **Usługa hdinsight**
 - **Internet rzeczy**
 - **Centrum iot**
 - **Klucz magazynu**
 - **Nauka komputera**
 - **Marketplace**: artykułów Azure marketplace
 - **usługi multimediów**
 - **Telefon komórkowy zaangażowania**
 - **Telefon komórkowy usług**
 - **wieloskładnikowe uwierzytelnianie**
 - **wiele**: strony dotyczy wielu usług jednakowo
 - **Brak**: stronie nie ma zastosowania do jakichkolwiek usług (rzadkich)
 - **powiadomienie o koncentratory**
 - **operacyjne wniosków**
 - **powerapps**
 - **Menedżer odzyskiwania**
 - **redis pamięci podręcznej**
 - **Funkcja RemoteApp**
 - **Zarządzanie prawami**
 - **Harmonogram**
 - **Centrum zabezpieczeń**
 - **Usługa bus**
 - **Usługa tkaninie**
 - **Odzyskiwanie witryny**: wcześniej usługi odzyskiwania
 - **Baza danych SQL**
 - **magazynu w przypadku danych SQL**
 - **raportowanie SQL**
 - **miejsca do magazynowania**
 - **Przechowywanie**: artykułów dotyczących usługi dostępne w sklepie Azure
 - **storsimple**
 - **Menedżer ruchu**
 - **maszyn wirtualnych**
 - **wirtualna sieć**
 - **Visual studio — w trybie online**
 - **Brama VPN**
 - **witryny sieci Web**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: wymagane. Określa język programowania, którego dotyczy ten artykuł. Pojedyncza wartość na każdej stronie.

 Jeśli strona ma zastosowanie do dwóch językach programowania jednakowo, wybierz pozycję **wiele**. Jeśli strona jest przede wszystkim koncepcyjny i jego zawartość jest stosowane w odniesieniu do wielu języków programowania, wybierz pozycję **wiele**. Jeśli strony nie jest przeznaczona dla deweloperów i stosowanie języka programowania nie ma znaczenia, wybierz pozycję **Brak**. Za pomocą **interfejsu api usługi rest** do identyfikowania interfejsu API usługi REST odwołanie tematów.

 - **cpp**
 - **DotNet**
 - **Java**
 - **języka JavaScript**
 - **wiele**: strony dotyczy wielu języków programowania równomiernie.
 - **Brak**: strona nie jest celem deweloperów i nie jest specyficzna dla każdej programowania wersji językowej.
 - **nodejs**
 - **Cel c**
 - **PHP**
 - **Python**
 - **interfejsu api usługi REST**
 - **dopiskiem**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: wymagane. Szczegóły wpisz temat. Większość nowych stron utworzone przez współautorów będzie artykuł lub odwołania.

 - **artykuł**: temat koncepcyjny, samouczek, funkcja Przewodnik lub inny artykuł-referencyjny

 - **Strona kampanii**: tylko Azure.com.  Strona, która zaprojektowane specjalnie jako strona początkowa kampanii zewnętrznych, a nie jest uwzględniane podstawowej witryny IA.  Nie należy używać dokumentacji artykułów lub zwykła dokumentu strony docelowe.  Przykłady: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/develop/Mobile/IOS/

 - **deweloperów-— strona główna Centrum**: tylko Azure.com.  Deweloperów środka strony głównej, np. / opracowywanie/netto /

 - **GET do artykuł**: przypisywanie do artykułów, które są polecane w sekcji wprowadzenie lub Przegląd nawigacji po lewej stronie usługi.

 - **główny artykuł**: samouczek "główny", przeznaczony do wprowadzeniem do usług lub funkcją, która pobiera odwiedzający pracę przy użyciu usługi szybko i dyski zapisy wolny wersji próbnej i aktywacji w witrynie MSDN. Przypisz tę wartość tylko do artykułów, które są polecane w górnej części strona początkowa dokumentacji tej usługi.

 - **Strona główna**: Strona główna dokumentacji najwyższego poziomu. Tylko mamy dwa: azure.microsoft.com/documentation/ i msdn.microsoft.com/library/azure/

 - **strony indeksu**: strony programowania języków, usług lub funkcji docelowe drugiego poziomu. Te są rozciągnąć Azure.com i biblioteki i są używane jako punktów wejścia bardziej szczegółowych informacji o zakresie. Przykłady: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **Strona infographic**: tylko Azure.com. Strona, która zawiera infographic można przeglądać lub plakatu, na przykład http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Odwołanie**: interfejs API odwołaniu (łącznie z interfejsu API usługi REST) lub na stronie odwołanie polecenia cmdlet programu PowerShell

 - **Usługa witrynie**: tylko Azure.com.  Stronę główną usługi dokumentu, np. /documentation/services/virtual-machines-

 - **sekcji — Strona główna witryny —**: tylko Azure.com. "Strona główna" dla określonego typu zawartości na azure.com przykłady: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **strony wideo**: tylko Azure.com.  Strona, która zawiera klipu wideo, na przykład http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: wymagane. Umożliwia określenie docelowej platformy, na przykład systemu Windows, Linux, Windows Phone, iOS, Android lub platformy specjalne pamięci podręcznej. Jedną wartość na każdej stronie. Ta wartość będzie **NA** większości tematy z wyjątkiem mobile i maszyn wirtualnych.

 - **pamięć podręczną w roli**
 - **wiele pamięci podręcznej**
 - **redis pamięci podręcznej**
 - **Usługa buforowania**
 - **udostępnione pamięci podręcznej**
 - **polecenia wiersza interfejsu**
 - **Ibiza**: zawartość, która korzysta z portalem Ibiza. Użyj tego tylko w przypadku funkcji są omawiane dostępne zarówno portalu Ibiza i portalu bieżącego.
 - **android telefon komórkowy**: Azure.com tylko teraz
 - **Telefon komórkowy html**: Azure.com tylko teraz
 - **Mobile ios**: Azure.com tylko teraz
 - **kindle telefon komórkowy**: Azure.com tylko teraz
 - **wielokrotności Mobile**
 - **Telefon komórkowy nokia-x**: Azure.com tylko teraz
 - **Telefon komórkowy phonegap**: Azure.com tylko teraz
 - **Telefon komórkowy sencha**: Azure.com tylko teraz
 - **Mobile windows**: Azure.com tylko teraz; Uniwersalny systemu Windows
 - **komórkowym systemu windows**
 - **magazynu w przypadku systemu windows Mobile**
 - **Telefon komórkowy xamarin**: Azure.com tylko teraz; Xamarin wszystkich platform
 - **Telefon komórkowy xamarin android**: Azure.com tylko teraz
 - **Telefon komórkowy — xamarin — ios**: Azure.com tylko teraz
 - **wiele**: strony dotyczy wielu platformach jednakowo
 - **Brak**: specyfikator platformy nie ma zastosowanie do tej strony
 - **programu PowerShell**
 - **linux maszyn wirtualnych**
 - **wiele maszyn wirtualnych**
 - **maszyn wirtualnych systemu windows**
 - **maszyn wirtualnych-systemu windows — sharepoint**
 - **sql server, maszyn wirtualnych systemu windows w-**
 - **w porównaniu z-— wprowadzenie**: identyfikuje grupę w PORÓWNANIU z wprowadzenie do strony. Tag dodany 14-12-1.
 - **w porównaniu z — co stało**: identyfikuje stronę w PORÓWNANIU z wprowadzenie pracę co się stało. Tag dodany 14-12-1.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: wymagane. Określa Azure obciążenie pracą, która dotyczy strony. Jedną wartość tylko na artykuł.

**Aktualizowanie 15-8-6** Wartość ms.workload mapowanego, xls, a nie wartość w pliku .md. Wartość ms.workload jest nadal wymagane na potrzeby weryfikacji, aż funkcję można aktualizować. Jest teraz planowany działające.  Użyj **"na"** jako wartość teraz.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: wymagane. Określa datę, dla której artykuł został ostatnio przeglądany istotności, dokładność, zrzutów ekranu i łącza pracy. Wprowadź datę w formacie mm/dd/rrrr. Data pojawia się też na opublikowany artykuł jako Data ostatniej aktualizacji.

![](./media/article-metadata/checkmark-small.png)**MS.AUTHOR**: wymagane. Określa autorów skojarzone z tym temacie. Za pomocą tej wartości wewnętrznych raportów (na przykład aktualność) skojarzyć prawo autorów z tego artykułu. Aby określić wiele wartości powinny oddziel je średnikami. Aliasy firmy Microsoft lub adresy e-mail wykonane są akceptowane. Długość może być dłuższa niż 200 znaków.


###<a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)


<!--Anchors-->
[W składni]: #syntax
[Użycie]: #usage
[Atrybuty i wartości w sekcji właściwości]: #attributes-and-values-for-the-properties-section
[Atrybuty i wartości dla sekcji znaczników]: #attributes-and-values-for-the-tags-section
