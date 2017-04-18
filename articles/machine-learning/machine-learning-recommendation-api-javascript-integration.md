<properties 
    pageTitle="Maszynowego uczenia zalecenia: Integracja JavaScript | Microsoft Azure" 
    description="Dokumentacja zalecenia nauki komputera — integracja JavaScript - Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure maszynowego uczenia zalecenia - integracji języka JavaScript

>[AZURE.NOTE]Należy zacząć korzystać z usługi poznawcze interfejsu API zalecenia zamiast tej wersji. Usługa poznawcze zalecenia będzie zamieniania tej usługi, a opracowane zostaną wszystkie nowe funkcje. Ma nowych funkcji, takich jak tworzeniu partii pomocy technicznej lepiej interfejsu API Eksploratora, oczyszczania środowisko tworzenia konta i rozliczenia powierzchniowy, bardziej spójny interfejs API, itp.
> Dowiedz się więcej na temat [migrowania do nowej usługi poznawcze](http://aka.ms/recomigrate)


Ten dokument przedstawić jak zintegrować Twojej witryny za pomocą języka JavaScript. Kod JavaScript umożliwia do przesyłania danych oraz używanie zalecenia po utworzeniu modelu rekomendacji. Wszystkie operacje wykonywane za pośrednictwem JS są również dostępne w po stronie serwera.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. Omówienie ogólne
Integrowanie witryny z zalecenia ML Azure składają się na fazy 2:

1.  Wyślij zdarzeń do zalecenia Azure ML. Spowoduje to włączenie do tworzenia modelu rekomendacji.
2.  Korzystanie ze zalecenia. Po skonstruowaniu jest modelu można używać zalecenia. (Ten dokument nie wyjaśniono, jak tworzyć modelu, przeczytaj Przewodnik Szybki start, aby uzyskać więcej informacji na temat).


<ins>Faza I</ins>

W pierwszym etapie należy wstawić do stron html niewielką bibliotekę JavaScript umożliwiający stronę, aby wysłać zdarzenia występujące na stronie html do serwerów Azure ML zalecenia (za pośrednictwem Data Market):

![Drawing1][1]

<ins>Faza II</ins>

W drugim etapie, gdy chcesz pokazać zalecenia na stronie wybierz jedną z następujących opcji:

1. serwera (w fazie renderowania stron) połączeń Azure ML zalecenia dotyczące serwera (za pośrednictwem Data Market) Aby uzyskać zalecenia. Wyniki zawiera listę identyfikatorów elementów. Serwer musi wzbogacanie wyniki z elementami metadane (obrazy, opis) i wysłać utworzonej strony do przeglądarki.

![Drawing2][2]

2. innych opcji jest za pomocą małych plik JavaScript z fazy pierwszej uzyskać prosta lista zalecane elementy. Dane otrzymane w tym miejscu jest leaner niż w pierwszej opcji.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. wymagania wstępne dotyczące

1. Utwórz nowy model przy użyciu za pośrednictwem interfejsów API. Jak to zrobić, zobacz Przewodnik Szybki start.
2. Kodowanie z &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; z base64. (To posłuży do uwierzytelniania podstawowego do włączenia kodu JS nawiązać połączenie za pośrednictwem interfejsów API).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. przesyłania danych za pomocą języka JavaScript
Poniższe czynności ułatwiają wysyłanie zdarzeń:

1.  Uwzględnij biblioteki dostępne w kodzie. Możesz pobrać ją z nuget w następujący adres URL.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Dołączanie do biblioteki obsługę języka JavaScript zalecenia następujący adres URL: http://aka.ms/RecoJSLib1

3.  Zainicjowanie biblioteki zalecenia ML Azure z odpowiednimi parametrami.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Wyślij odpowiedniego zdarzenia. Zobacz szczegółowe sekcję na wszystkich typów zdarzeń (przykład kliknij zdarzenie)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Ograniczenia i obsługa przeglądarek
Jest to implementacji i jego oblicza się znajduje. Należy ją obsługuje wszystkie główne przeglądarki.

###<a name="32-type-of-events"></a>3,2. Typ zdarzenia
5 rodzaje zdarzenia, która obsługuje biblioteki: kliknij przycisk, kliknij pozycję rekomendacji, Dodaj do koszyka sklep usunąć z koszyka sklepu i zakupu. Istnieje dodatkowe zdarzenia, które służy do ustawiania kontekstu użytkownika o nazwie logowania.

####<a name="321-click-event"></a>3.2.1. Kliknij zdarzenie
To zdarzenie powinien być używany w dowolnym momencie, a użytkownik kliknie przycisk dla elementu. Zazwyczaj, gdy użytkownik kliknie elementu nowa strona zostanie otwarty w szczegóły elementu; na tej stronie można uruchomienie to zdarzenie.

Parametry:
- zdarzenia (ciąg, obowiązkowe) - "kliknij"
- element (ciąg, obowiązkowe) — Unikatowy identyfikator elementu
- Nazwa elementu (ciąg, opcjonalne) - nazwa elementu
- itemDescription (ciąg, opcjonalne) — opis dla elementu
- itemCategory (ciąg, opcjonalne) - kategorii elementu
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Lub dane opcjonalne:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Zalecenie kliknij zdarzenie
To zdarzenie powinien być używany w dowolnym momencie użytkownik kliknięciu elementu, który otrzymano od zaleceń ML Azure jako element zalecane. Zazwyczaj, gdy użytkownik kliknie elementu nowa strona zostanie otwarty w szczegóły elementu; na tej stronie można uruchomienie to zdarzenie.

Parametry:
- zdarzenia (ciąg, obowiązkowe) - "recommendationclick"
- element (ciąg, obowiązkowe) — Unikatowy identyfikator elementu
- Nazwa elementu (ciąg, opcjonalne) - nazwa elementu
- itemDescription (ciąg, opcjonalne) — opis dla elementu
- itemCategory (ciąg, opcjonalne) - kategorii elementu
- nasiona (ciąg tablica, argument opcjonalny) - nasion wygenerowanych kwerendy rekomendacji.
- recoList (ciąg tablica, argument opcjonalny) — wynik żądania rekomendacji, który wygenerował elementu, który został kliknięty.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Lub dane opcjonalne:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Dodawanie zdarzenia koszyka zakupów
To zdarzenie powinien być używany podczas użytkownika Dodawanie elementu do koszyka.
Parametry:
* zdarzenia (ciąg, obowiązkowe) - "addshopcart"
* element (ciąg, obowiązkowe) — Unikatowy identyfikator elementu
* Nazwa elementu (ciąg, opcjonalne) - nazwa elementu
* itemDescription (ciąg, opcjonalne) — opis dla elementu
* itemCategory (ciąg, opcjonalne) - kategorii elementu
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Usuwanie zdarzenia koszyka zakupów
Jeśli użytkownik usuwa element do koszyka, należy użyć tego zdarzenia.

Parametry:
* zdarzenia (ciąg, obowiązkowe) - "removeshopcart"
* element (ciąg, obowiązkowe) — Unikatowy identyfikator elementu
* Nazwa elementu (ciąg, opcjonalne) - nazwa elementu
* itemDescription (ciąg, opcjonalne) — opis dla elementu
* itemCategory (ciąg, opcjonalne) - kategorii elementu
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Zdarzenia zakupu
Jeśli użytkownik zakupionych jego koszyka, należy użyć tego zdarzenia.

Parametry:
* zdarzenia (ciąg) - "zakup"
* elementy (zakupionych []) - tablicy, przytrzymując wpisu dla każdego elementu zakupu.<br><br>
Format kupowane:
    * element (ciąg) — Unikatowy identyfikator elementu.
    * Liczba (int lub ciąg) — liczba elementów, które zostały zakupione.
    * Cena (ruchomości lub ciąg) — opcjonalnie pole — cenę produktu.

W poniższym przykładzie pokazano zakupu 3 elementów (33, 34, 35), 2 ze wszystkich pól (elementu, liczba, cena), a drugi (pozycja 34) bez ceny.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Zdarzenia logowania użytkownika
Azure biblioteki ML zalecenia dotyczące zdarzeń tworzy i używanie plików cookie w celu identyfikowania zdarzenia, które pochodzą z tej samej przeglądarki. W celu uzyskania lepszych wyników modelu zalecenia ML Azure umożliwia ustawianie Unikatowy identyfikator użytkownika, który zastępuje zastosowania plików cookie.

To zdarzenie powinien być używany po logowania użytkownika do witryny.

Parametry:
* zdarzenia (ciąg) - "userlogin"
* Użytkownik (ciąg) — Unikatowy identyfikator użytkownika.
        <script>
  Jeśli (typeof AzureMLRecommendationsEvent == "nieokreślone") {AzureMLRecommendationsEvent = []} AzureMLRecommendationsEvent.push ({zdarzenia: "userlogin" użytkownika: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. Używanie zalecenia za pomocą języka JavaScript
Kod, który używa zalecenie zostanie wywołana przez zdarzenie JavaScript przez klienta w sieci Web. Odpowiedź rekomendacji zawiera identyfikatory zalecane elementy, ich nazwiska oraz ich oceny. Warto użyć tej opcji tylko do wyświetlania listy zalecane elementy — Obsługa złożonych (na przykład dodając metadanych elementu) powinna odbywać się w integracji po stronie serwera.

###<a name="41-consume-recommendations"></a>4.1 Używanie zalecenia
Korzystać z zalecenia, które należy uwzględnić wymagane bibliotek JavaScript, na stronie i połączenie AzureMLRecommendationsStart. W sekcji 2.

Do korzystania z zalecenia dotyczące jednego lub kilku elementów, w których trzeba zadzwonić metodę o nazwie: AzureMLRecommendationsGetI2IRecommendation.

Parametry:
* elementów (Tablica ciągów) — jeden lub więcej elementów, aby uzyskać zalecenia dotyczące. Jeśli używają kompilacji zmianie wysokości progów następnie można ustawić tutaj tylko jeden element.
* numberOfResults (int) — liczba wymaganych wyników.
* includeMetadata (wartość logiczna, opcjonalne) — Jeśli ma wartość "true" oznacza, że pola metadanych, muszą być wypełnione w wyniku.
* Funkcja przetwarzania — funkcję, która będzie obsługiwać zalecenia zwracane. Dane są zwracane jako tablica:
    * Element — element Unikatowy identyfikator
    * Nazwa - Nazwa przedmiotu (jeśli istnieje w wykazie)
    * Ocena — ocena rekomendacji
    * metadane - ciąg reprezentujący metadanych elementu

Przykład: Poniższy kod żądania 8 zalecenia dotyczące elementu "64f6eb0d-947a-4c18-a16c-888da9e228ba" (bez określania includeMetadata - niejawnie z opisem że metadanych nie jest wymagane), następnie ZŁĄCZ.teksty wyniki do buforu.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
