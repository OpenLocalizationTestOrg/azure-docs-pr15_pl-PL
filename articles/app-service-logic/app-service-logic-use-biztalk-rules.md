<properties
   pageTitle="Więcej informacji na temat i tworzenie aplikacji BizTalk reguły API w aplikacji logika | Microsoft Azure"
   description="W tym temacie opisano funkcje łącznik reguły BizTalk i instrukcje po jego zastosowania"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>Reguły BizTalk

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Reguły biznesowe obejmuje zasady i decyzji, które sterują procesami biznesowymi. Te zasady mogą być formalnego zdefiniowane w instrukcji procedury, zamówień lub umów lub nie istnieje jako wiedzy lub specjalizacji zawartymi w pracowników. Te zasady są dynamiczne i mogą ulec zmianie nad czasu ukończenia zmiany planów biznesowych, przepisów lub innych powodów.

Implementowania tych zasad w tradycyjnych językach programowania wymaga sporo czasu i można je zsynchronizować, a nie pozwala na zwykłym uczestniczyć w tworzenie i konserwacja zasad biznesowych. Reguły biznesowe BizTalk umożliwia szybkiego wykonania tych zasad i rozdzielenie pozostałą część proces biznesowy. Dzięki temu dokonywania wymagane zmiany w zasadach biznesowych bez wpływania pozostałą część proces biznesowy.

##<a name="why-rules"></a>Dlaczego reguły

Istnieją 3 najważniejszych powodów, dla których używać reguł biznesowych BizTalk w procesie biznesowym:

* Rozdzielenie reguł biznesowych z kodem aplikacji
- Zezwalaj na analitycy biznesowi mieć większą kontrolę nad Zarządzanie logiczny biznesowymi
+ Przejdź do produkcji szybciej zmiany warunków logicznych

##<a name="rules-concepts"></a>Pojęcia reguł

##<a name="vocabulary"></a>Słownictwo

_Słownictwo_ to zbiór definicji składający się z przyjazne nazwy obiektów komputerowego, używane w regule warunków i akcji. Definicje słownictwo ułatwiają reguł czytanie, opis i udostępnianie przez osoby w domenie określonego rodzaju.

Vocabularies są przeznaczone do Mostek odstępu między znaczeń właściwych firm i realizacji. Na przykład powiązanie danych dla stan zatwierdzenia może wskaż określone kolumny w niektórych wierszy w określonych bazy danych, przedstawione w postaci zapytania SQL. Zamiast tego rodzaju złożonych reprezentacja wstawiania w regule, zamiast tego może utworzyć definicję słownictwo, skojarzone z tym wiązanie danych z przyjazną nazwę "Statusu". Później możesz umieścić "Stan" w dowolnej liczby reguł, a aparat reguły można pobrać odpowiadające im dane z tabeli.

##<a name="rule"></a>Reguły

Reguły biznesowe zawierają deklaracyjnych decydujących prowadzenia procesów biznesowych. Reguła składa się z warunków i akcji. Warunek jest obliczana, a jeśli jej wartość PRAWDA, aparat reguł inicjuje jedną lub więcej akcji.
Reguły w ramach reguł biznesowych są definiowane przez w następującym formacie:

_IF_ _warunek_ _Następnie_ _Akcja_

Należy rozważyć poniższym przykładzie:

*Jeżeli kwota jest mniejsza niż lub równa argumentowi dostępnymi środkami*  
*NASTĘPNIE przeprowadzanie transakcji i drukowanie potwierdzenia*

Ta reguła określa, czy transakcji będą prowadzone przez zastosowanie reguł biznesowych w postaci w celu porównania dwóch wartości pieniężnych, w formie kwota transakcji i dostępnymi środkami.
Reguły biznesowej umożliwia tworzenie, modyfikowanie i wdrażanie reguł biznesowych. Możesz też poprzednich zadań można wykonywać programowy.

###<a name="conditions"></a>Warunki

Warunek jest PRAWDA i FAŁSZ (wartość logiczna) wyrażenie, które składa się z co najmniej jeden orzeczenia.
W naszym przykładzie predykatu mniej niż lub równe jest stosowana do kwota i dostępnymi środkami. Ten warunek zawsze oceni wartość PRAWDA lub FAŁSZ.
Orzeczenia mogą być połączone z operatorów logicznych AND, OR i nie chcesz, aby formularz wyrażenie logiczne, która jest potencjalnie bardzo duże, ale zawsze oceni wartość PRAWDA lub FAŁSZ.

###<a name="actions"></a>Akcje

Akcje są funkcjonalności konsekwencje sprawdzania warunku. Jeśli spełnienia warunku reguły są inicjowane żadnej akcji lub akcji.
W naszym przykładzie "prowadzenie transakcji" i "Drukowanie potwierdzenia" są akcje, które są wykonywane po i tylko wtedy, gdy warunek (w tym przypadku "Jeśli kwota jest mniejsza niż lub równa argumentowi dostępnymi środkami") ma wartość true.
Akcje są przedstawione w ramach reguł biznesowych przez operacji zestawu dokumentów XML.

##<a name="policy"></a>Zasady

Zasady jest logiczna Grupa reguł. Możesz utworzyć zasady, zapisać go, należy je przetestować i, po zakończeniu do wyników z niej korzystać w środowisku produkcyjnym.

###<a name="policy-composition"></a>Skład zasad

Można utworzyć zasady w portalu reguły. Zasady może zawierać dowolnie dużą zestaw reguł, ale zwykle tworzą zasady z reguł, które dotyczą domeny firmy w kontekście aplikacji, który będzie używany zasady.

###<a name="policy-testing"></a>Testowanie zasad
Przebieg badania z zasad można wykonywać skuteczne, przed użyciem w środowisku produkcyjnym. Portal reguł umożliwia podanie danych wejściowych do zasad, uruchom Zasady i wyświetlić jej wyniki. Dane wyjściowe obejmuje dzienniki, wykonanie reguły, ocena stanu i wyjść wyniku.

##<a name="sample-scenario---insurance-claims"></a>Przykładowy scenariusz – roszczeń ubezpieczeniowych
Załóżmy pobrać przykładowy scenariusz i szczegółową go jak możemy redagowanie dla tego samego warunków logicznych.

![Tekst alternatywny][1]

W scenariuszu really simple roszczeń ubezpieczeniowych wnioskodawca przesyła jego roszczeń ubezpieczeniowych (za pomocą dowolnego klienta, takich jak witryny sieci Web, telefon aplikacji itp). To żądanie roszczeń otrzymuje wysyłane do jednostki przetwarzania roszczeń jej działalności i na podstawie wyniku przetwarzania, roszczenia może być albo zatwierdzone odrzucone lub wysyłane razem w celu dalszego przetwarzania ręcznego.
Jednostki przetwarzania roszczeń w naszym scenariusz będzie jeden obejmujący reguł biznesowych dotyczących systemu. Sporządzanie przyjrzeć się bliżej tej jednostki, firma Microsoft może zobacz następujące artykuły:

![Tekst alternatywny][2]

Pozwól nam teraz za pomocą reguł biznesowych do wykonania tego reguł biznesowych.


##<a name="creation-of-rules-api-app"></a>Tworzenie reguły interfejsu Api aplikacji


1. Zaloguj się do portalu Azure
2. Wybierz nowy -> Marketplace, a następnie wyszukaj *BizTalk reguł*
3. Wybierz pozycję BizTalk na liście wyników. Zostanie wyświetlona karta BizTalk reguł
4. Kliknij przycisk *Utwórz* ![tekstu alternatywnego][3]
1. W nowej kartę, która zostanie otwarta wprowadź następujące informacje:  
    1. Nazwa — należy podać nazwę aplikacji interfejsu API reguł
    1. Plan usług aplikacji — zaznacz lub Utwórz nowy Plan aplikacji usługi
    1. Cennik Warstwa — wybierz mają tę aplikację do znajdują się w warstwie cennik
    1. Grupa zasobów — wybierz lub Utwórz grupę zasobów, gdzie powinien się znajdować w aplikacji
    2. Subskrypcja — Wybierz subskrypcję, mają być używane
    1. Lokalizacja — wybierz lokalizację geograficzną miejsce, w którym chcesz aplikacji do wdrożenia.
4.  Wybierz polecenie *Utwórz*. W ciągu kilku minut będzie można utworzyć aplikacji interfejsu API reguły BizTalk.

##<a name="vocabulary-creation"></a>Tworzenie słownika
Po utworzeniu aplikacji interfejsu API programu BizTalk reguł, następnym krokiem jest tworzenie vocabularies. Oczekiwania jest projektanta persona częściej używana do wykonania czynności tego ćwiczenia. Oto jak uzyskać dostęp do tej zadań:


1. Uruchamianie programu BizTalk zasady interfejsu API aplikacji z portalu, przechodząc do przeglądania -> interfejsu API aplikacje — ><Your Rules API App>. To rozpocznie się możesz pulpit nawigacyjny aplikacji interfejsu API reguły podobnie jak poniżej:

   ![Tekst alternatywny][4]

2. Wybierz "Słownictwo definicje". To samo wyświetlanych 3.w słownictwo tworzenia ekranu wybierz pozycję "Dodaj", aby rozpocząć dodawanie nowych definicji słownictwo.
Istnieją 2 typy definicji słownictwo obecnie obsługiwane — literałów i język XML.

##<a name="literal-definition"></a>Definicja literałów
1.  Po kliknięciu na "Dodaj", nowe karta "Dodawanie definicji" otwarty. Wprowadź następujące wartości
  1.    Nazwa — tylko znaki alfanumeryczne przewidywań bez żadnych znaków specjalnych. To musi być również unikatowa do istniejącej listy definicji słownictwo.
  2.    Opis — pole opcjonalne.
  3.    Typ definicji — istnieje obsługiwane typy 2. W tym przykładzie wybierz pozycję literałów
  4.    Typ danych — umożliwia użytkownikom wybierz typ danych definicji. Obecnie obsługiwane typy danych 4: i.  Ciąg — należy wprowadzić te wartości w podwójny cudzysłów ("przykład ciąg")  
    II. Boolean — to może być wartość PRAWDA lub FAŁSZ  
    III.    Numer — może być dowolna liczba dziesiętna  
    IV. Daty i godziny — oznacza, że rozdzielczości typu Data typu. Dane muszą być wprowadzane za pomocą tego formatu — mm/dd/rrrr hh: mm: AM\PM  
  5. Wprowadzanie — jest to miejsce, w którym należy wprowadzić wartość do definicji. Wprowadzona tutaj wartości muszą być zgodne z wybranym typem danych. Można wprowadzać pojedynczą wartość zestawu wartości oddzielone przecinkami lub zakresu wartości przy użyciu słów kluczowych *do*. Na przykład można wprowadzić wartość unikatowa 1; Zestaw 1, 2, 3; lub zakresie 1 do 5. Należy zauważyć, że zakres jest obsługiwana tylko dla liczby.
  6. Wybierz *przycisk OK*.

![Tekst alternatywny][5]
##<a name="xml-definition"></a>Definicja XML
Jeśli typ słownictwo zostanie wybrana jako XML, trzeba określać następujące wartości wejściowych  
  .    Schematu — klikanie na to zostanie otwarte nowe karta Zezwalanie użytkownikowi wybrać z listy schematy już przekazane lub zezwolić na przekazywanie nową.
b.    Wyrażenie XPATH — wprowadzania pobiera odblokowywane tylko po wybraniu schematu w poprzednim kroku. Klikając to będzie wyświetlana schematu, który został zaznaczony i umożliwia użytkownikowi wybierz węzeł, dla którego definicję słownictwo musi być utworzony.  
  c.    Silnia — tych danych wejściowych służy do identyfikowania obiektu danych, który chcesz rana do aparatu reguły w celu przetwarzania. To jest właściwość zaawansowaną i domyślnie jest ustawiona na elementem nadrzędnym wybranego wyrażenia XPATH. Silnia staje się szczególnie istotne w przypadku scenariusze łączenia i kolekcji.

![Tekst alternatywny][6]

### <a name="add-bulk"></a>Dodawanie zbiorcze
Obsługa tworzenia definicji słownictwo przechwycone powyższe czynności. Po utworzeniu definicji utworzony słownictwo pojawi się w formie listy. Istnieją wymagania, aby wygenerować wiele definicji z tego samego schematu zamiast powtarzających się powyższe czynności co jeden raz. Jest to miejsce, w którym funkcja zbiorcze Dodawanie staje się bardzo przydatne.
Wybieranie "Zbiorcze dodawanie" spowoduje przejście do nowej kart. Pierwszym krokiem jest wybierz schemat, dla którego mają zostać utworzone wiele definicji. Wybranie tej opcji spowoduje otwarcie nowego karta zezwolenia albo wybierz z listy schematy już przekazane lub zezwolenia na przekazywanie nową.
Teraz właściwości wyrażenia XPath otrzymuje odblokowana. Wybranie tej opcji zostanie otwarta przeglądarka schematu miejsce, w którym można wybrać wielu węzłów.
Nazwy wiele definicji utworzony jest domyślnie do nazwy wybrany węzeł. Te zawsze można modyfikować po utworzeniu.

![Tekst alternatywny][7]

##<a name="policy-creation"></a>Tworzenie zasad
Po utworzeniu wymagane vocabularies projektanta oczekuje się, że analityka biznesowego będzie jeden tworzenia zasad biznesowych przez Azure Portal.  
    1. na aplikację reguły utworzone jest obiektyw zasad, kliknięcie którego użytkownik przejdzie do strony tworzenia zasad.  
    2. w tym stronie zostanie wyświetlona na liście zasad stosowanych ten określonej aplikacji reguły. Analityk można dodawać nowych zasad, po prostu wpisując nazwę zasady, a dwukrotne kliknięcie karty. Wiele zasad może znajdować się w jednej aplikacji interfejsu API reguły.  
    3. Wybieranie tworzone zasady potrwa użytkownikowi na stronie Szczegóły zasad jedno miejsce, w którym Zobacz reguły, które są w zasadzie.  
    ![Tekst alternatywny][8]
 4.  Wybierz pozycję "Dodaj", aby dodać nową regułę. Spowoduje to przejście do nowych kart.

##<a name="rule-creation"></a>Tworzenie reguł
Reguła jest zbiór instrukcji warunków i akcji. Jeśli warunek ma wartość PRAWDA, są wykonywane czynności. W karta Utwórz regułę nadaj nazwę reguły unikatowe (dla tej zasady) i opis (opcjonalnie).
Pole warunku (jeśli jest) może służyć do tworzenia złożonych instrukcji warunkowych. Słowa kluczowe obsługiwane są poniższe zależności:  
1.  I — operator warunkowy  
2.  Lub — operator warunkowy  
3.  czy\_nie\_istnieje  
4.  istnieje  
5.  FAŁSZ  
6.  jest\_równa\_do  
7.  jest\_większa\_niż  
8.  jest\_większa\_niż\_równa\_do  
9.  jest\_w  
10. jest\_mniej\_niż  
11. jest\_mniej\_niż\_równa\_do  
12. jest\_nie\_w  
13. jest\_nie\_równa\_do  
14. mod  
15. wartość PRAWDA.  

Pole Action(THEN) może zawierać wiele instrukcji, jedną dla każdego wiersza, aby utworzyć akcje, które mają być wykonywane. Słowa kluczowe obsługiwane są poniższe zależności:  
1.  równa się  
2.  FAŁSZ  
3.  wartość PRAWDA.  
4.  zatrzymanie  
5.  mod  
6.  wartości null  
7.  Aktualizowanie  

Pola warunkiem i akcją dostarczają Intellisense, aby ułatwić szybkie tworzenie reguły. Może to być wyzwalane przez naciśnięcie klawiszy ctrl + spacja lub po prostu rozpoczęciu wpisywania tekstu. Słowa kluczowe pasujących znaki będą automatycznie filtrowane w dół i wyświetlane. W oknie technologii intellisense są wyświetlane wszystkie definicji słownictwo i słów kluczowych.
![Tekst alternatywny][9]

##<a name="explicit-forward-chaining"></a>Jawne łączenia do przodu
Obsługa reguł BizTalk jawne do przodu łączenia, jeśli użytkownicy mają być ponownie Oceń reguły w odpowiedzi na niektóre akcje, mogą wyzwalać to za pomocą niektórych słów kluczowych. Słowa kluczowe, obsługiwane są następujące:  
   1.   aktualizowanie <vocabulary definition> — słowa kluczowego ponownie sprawdza wszystkie reguły korzystające z definicją określoną słownictwo w jego stan.  
   2.   Zatrzymania — słowa kluczowego przestaje wszystkie wykonania reguły

##<a name="enabledisable-rules"></a>Reguły Enable\Disable
Każdą regułę z zasady może być włączona lub wyłączona. Domyślnie wszystkie reguły są włączone. Wyłączone reguły nie można wykonać podczas wykonywania zasad. Reguły Enable\Disable można zrobić z karta reguły bezpośrednio — polecenia są dostępne na pasku poleceń w górnej części karta, lub z zasad, z menu kontekstowego (kliknij prawym przyciskiem myszy na reguły) jest dostępna opcja enable\disable.

##<a name="rule-priority"></a>Priorytetu reguły
Wszystkie reguły zasady są wykonywane w kolejności. Priorytet wykonanie zależy od kolejności, w którym występują w zasadach. Ten priorytet może zostać zmieniony, po prostu przeciągając i upuszczając reguły.

##<a name="test-policy"></a>Testowanie zasad
Istnieje możliwość przetestowania zasad programu karta testowanie zasad przy użyciu polecenia "Testowanie zasad". W tym karta zostanie wyświetlona lista definicji słownictwo, które są używane w zasadach wymagają danych wejściowych użytkownika. Użytkownicy mogą ręcznie dodawać wartości dla tych danych wejściowych dla ich Scenariusz testów. Ewentualnie, użytkownicy mogą także wybrać do zaimportowania testowanie XMLs dla wartości wejściowych. Gdy wszystkie dane wejściowe są, test można uruchamiać i wyjściowe Każda definicja słownictwo będą wyświetlane w kolumnie wyników w celu ich łatwego porównania. Aby wyświetlić dzienniki przyjazny analityka biznesowego, kliknij przycisk "Wyświetl dzienniki", aby wyświetlić dzienniki wykonanie. Aby zapisać dzienniki, opcja "Zapisz dane wyjściowe" jest dostępna do przechowywania testowanie wszystkich powiązanych danych na potrzeby analizy niezależnie.

## <a name="using-rules-in-logic-apps"></a>Za pomocą reguł w logiczny aplikacji
Gdy zasady został utworzony i przetestowane, teraz jest gotowy do zużycie. Możesz utworzyć nowej aplikacji logiczny, wybierając logiki aplikacji z lewej części strony głównej portalu. Po utworzeniu aplikacji logika uruchamianie go a następnie wybierz *Wyzwalacze i akcje*. Następnie możesz wybrać szablon *Tworzenie od podstaw* . Postępuj zgodnie z instrukcjami dodać aplikacji interfejsu API reguł BizTalk do aplikacji logicznych. Po zakończeniu to nastąpi opcję, aby wybrać której aplikacji interfejsu API reguły (Akcja) do miejsca docelowego. Akcje zawierają wykaz zasady, które mają być wykonywane. Wybierz konkretne, po upływie którego danych wejściowych wymagane zasady muszą być podane. Użytkownicy mogą używać danych wyjściowych z aplikacji interfejsu API reguły w dół dla dalszych podejmowania decyzji.

## <a name="using-rules-via-apis"></a>Za pomocą reguł za pośrednictwem interfejsów API
Aplikacja interfejsu API reguły można wywołać również za pomocą bogatego zestawu interfejsów API. Użytkownicy w ten sposób nie są ograniczone do tylko przy użyciu aplikacji logiki i za pomocą reguł w dowolnej aplikacji, nawiązywanie połączeń pozostałych. Porównaj API pozostałe dostępne można wyświetlić, klikając pozycję obiektywu "Interfejsu API definicji" na pulpicie nawigacyjnym reguł.

![Tekst alternatywny][10]

Oto przykład jedną wykorzystania przez ten interfejs API w C#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Należy zauważyć, że powyżej aplikacji interfejsu API reguł zawiera jej ustawienia zabezpieczeń, ustaw "Publicznej Anon". Tego pola można ustawić z poziomu aplikacji interfejsu API przy użyciu — wszystkie ustawienia -> Ustawienia aplikacji -> poziom dostępu.

![Tekst alternatywny][11]

## <a name="editing-vocabulary-and-policy"></a>Edytowanie słownictwo i zasad
Jedną z najważniejszych zalet stosowania reguł biznesowych jest zmiany reguł biznesowych może być przyczyną znacznie szybsze produkcji. Wszelkie zmiany wprowadzone do słownika i zasady natychmiast jest stosowana w produkcji. Użytkownik chce po prostu przejdź do definicji odpowiednich słownictwo lub zasad i nie wprowadzaj zmian ona obowiązywać.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
