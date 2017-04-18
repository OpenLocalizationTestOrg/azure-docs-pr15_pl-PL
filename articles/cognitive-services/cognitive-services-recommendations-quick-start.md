<properties
    pageTitle="Przewodnik Szybki start: maszynowego uczenia zalecenia API | Microsoft Azure"
    description="Azure maszynowego uczenia zalecenia — Przewodnik Szybki Start"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Przewodnik Szybki start dla poznawcze interfejsu API zalecenia dotyczące usług

W tym dokumencie opisano sposób do wewnętrznego usłudze lub aplikacji za pomocą [Interfejsu API zalecenia](http://go.microsoft.com/fwlink/?LinkId=759710).
Można znaleźć więcej informacji na temat interfejsu API zalecenia i innych usług poznawcze [tutaj](http://go.microsoft.com/fwlink/?LinkId=759709). W niniejszym przewodniku można również znaleźć [Zalecenia API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) pod ręką.


<a name="Overview"></a>
## <a name="general-overview"></a>Ogólne omówienie

Ten dokument jest przewodnik krok po kroku. Naszym celem jest użytkownik mógłby wykonać kroki niezbędne do przeszkolenie modelu i wskaż zasoby, które umożliwia używanie modelu w środowisku produkcyjnym.

Tego ćwiczenia potrwa około 30 minut.

Aby korzystać z [Interfejsu API zalecenia](http://go.microsoft.com/fwlink/?LinkId=759710), należy wykonać następujące czynności:

1. Tworzenie modelu — model jest kontenerem danych użycia, wykazu danych i rekomendacji modelu.
1. Importowanie wykazu danych — wykaz zawiera metadane elementy.
1. Importowanie danych dotyczących użycia - danych dotyczących użycia można przekazywać sposoby 2 (lub oba):
  -  Przekazując plik, który zawiera dane użycia.
  -  Wysyłanie zdarzenia nabycia danych.
  Zazwyczaj przekazać plik zastosowania, aby móc tworzyć model rekomendacji początkowej (uruchamiania) i go używać, dopóki system zbiera za mało dane przy użyciu formatu nabycia danych.
1. Tworzenie modelu rekomendacji — jest to operacja asynchroniczna, w którym system rekomendacji pobiera wszystkie dane użycia i tworzy model rekomendacji. Operacja może potrwać kilka minut lub kilka godzin w zależności od rozmiaru danych i parametry konfiguracji kompilacji. Gdy powodujące kompilacji, zostanie wyświetlony identyfikatora kompilacji Go używać, aby sprawdzić, kiedy procesu tworzenia zostało zakończone przed rozpoczęciem korzystania zalecenia.
1. Korzystanie ze zalecenia — Uzyskaj zalecenia dotyczące określonego elementu lub listę elementów.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Zaczynamy!

Rozpoczęciem tworzenia modelu zalecenia. Następnie firma Microsoft będzie pomagają na temat korzystania z wyników generowane przez model zalecenia power w witrynie elektronicznego.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Zadanie 1 - podpisywania dla interfejsu API zalecenia ####

W tym zadaniu będzie utworzyć konto w usłudze interfejsu API zalecenia i tworzenie modelu zalecenia.

1. Przejdź do **Azure Portal** pod adresem [http://portal.azure.com/](http://portal.azure.com/) i zaloguj się przy użyciu konta Azure.

1.  Polecenie **+ Nowe**.

1. Wybierz opcję **analizy** .

1. Wybierz pozycję produktu **Poznawcze API usług** .
Ten produkt pozwoli rozpocząć subskrypcję dla usług poznawcze interfejsy API (nominalnej analizy tekstu, wzroku komputera, itp.). Dzisiaj firma Microsoft poświęcona interfejsu API zalecenia.

1. Strona główna poznawcze interfejs API usług wprowadź **nazwę konta** dla subskrypcji zalecenia. (Dla instace: "MyRecommendations"). Ta nazwa nie powinien mieć spacji w nim.

1. **Typ interfejsu API**wybierz **zalecenia**.

1. W **warstwie ceny**możesz wybrać planu. Możesz wybrać warstwie **wolny** dla 10 000 transakcji/miesiąca. To bezpłatne planu, zatem jest dobrym sposobem na rozpoczynanie próbuje systemu. Po przejściu do produkcji, zalecamy należy rozważyć, czy głośność żądania i odpowiednio zmienia typ planu. (Uwaga: możesz mieć tylko jedną subskrypcję bezpłatne warstwa naraz)

1. Wybierz **Grupę zasobów**lub utworzyć nową, jeśli nie masz już.

1. Możesz zmienić inne elementy w oknie dialogowym Tworzenie. Firma Microsoft wskazywał się, że dostawcy zasobów dzisiaj jest obsługiwana tylko z centrami danych Stanów Zjednoczonych.

1. Po zakończeniu z zaznaczeń, kliknij przycisk **Utwórz**.

1. Poczekaj kilka minut na zasobów do wdrożenia.
Po wdrożeniu, możesz przejść do sekcji **klawiszy** w karta **Ustawienia** , w którym zostaną udostępnione klucz podstawowy i pomocniczy korzystać z interfejsu API.  Skopiować klucza podstawowego, musisz go podczas tworzenia modelu pierwszego.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Zadanie 2, czy można przenosić danych? ####

Interfejs API zalecenia pokażemy z katalogu i transakcji w celu udostępniania zalecenia dobre produktu. Oznacza to, że należy go kanału danych dotyczących produktów (nazywamy to plik **wykazu** ) i zestaw transakcje duży, aby znaleźć interesujące desenie zużycie (nazywamy to **zastosowania**).

1. Zazwyczaj czy kwerendę transakcje bazy danych dla tych rodzajów informacji.
Przykładowe dane zostały zamieszczone na wypadek braku je pod ręką, na podstawie danych transakcji Microsoft Store.

 [Tutaj](http://aka.ms/RecoSampleData)możesz pobrać dane. Kopiowanie i rozpakowywanie MsStoreData.Zip do folderu na komputerze lokalnym.

 > **Uwaga:** Przykładowy kod, który będzie pobrać i uruchomić w 3 zadania zawiera dane przykładowe już osadzony w nim — dzięki to zadanie jest opcjonalne.  Który wspomniano, to zadanie będzie można pobrać bliższy zestawy danych i umożliwia zrozumienie danych wejściowych w lepiej interfejs API zalecenia.

1.  Teraz Przyjrzyjmy plik wykazu. Przejdź do lokalizacji, w którym zostały skopiowane dane.
 Otwórz plik wykazu w **Notatniku**.

 Można zauważyć, że plik wykazu jest bardzo proste. Ma następujący format`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32-64 E34 Online DwnLd pakietu Office <br>
 > AAA-04303, OfficeLangPack 2013 32-64 i Online DwnLd pakietu Office  <br>
 > C9F 00168, KRUSELL Kiruna Przerzucanie strona tytułowa dla Nokia Lumia 635 - wielbłądów, Akcesoria

 Firma Microsoft powinny wskazywać, że plik wykazu może być znacznie bardziej rozbudowane, na przykład możesz dodać metadanych o produktach (nazywamy te *Funkcje elementu*). Powinien zostać wyświetlony sekcji [format wykazu](http://go.microsoft.com/fwlink/?LinkID=760716) w odwołaniu interfejs API, aby uzyskać więcej informacji o formacie katalogu.

1. Załóżmy wykonaj te czynności z danych dotyczących użycia. Można zauważyć to Data zastosowania formatu `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015-08-04T11:02:52, 0003BFFDD4C2148C zakupu 297833400, 2015-08-04T11:02:50, 0003BFFDD4C2118D zakupu 297833300, 2015-08-04T11:02:40, 00030000D16C4237 zakupu 297833300, 2015-08-04T11:02:37, 0003BFFDD4C20B63 zakupu 297833400, 2015-08-04T11:02:12, 00037FFEC8567FB8 zakupu 297833400, 2015-08-04T11:02:04, zakupu

Zwróć uwagę, że pierwsze trzy elementy są wymagane. Typ zdarzenia jest opcjonalna. Możesz sprawdzić, czy [format zastosowania](http://go.microsoft.com/fwlink/?LinkID=760712) , aby uzyskać więcej informacji na ten temat.

 > **Ile danych są potrzebne?**
 <p>
To naprawdę zależy od rodzaju dane użycia. System dowiaduje się użytkowników kupionego różnych elementów. Dla niektórych wersjach takich jak zmianie wysokości progów jest ważne elementy, które zostały nabyte w tym samym transakcje. (Nazywamy tego *wystąpienia Współtworzenie*). Regułą ma większości elementów w 20 transakcje lub więcej, więc jeśli wcześniej używano 10 000 elementów w wykazie, zaleca czy masz co najmniej 20 razy tej liczbie transakcje i około 200 000.
Ponownie jest to zasada. Konieczne będzie poeksperymentować z danymi.
</p>

<a name="Ex1Task3"></a>
####Zadanie 3 — Tworzenie modelu zalecenia####

Teraz, gdy masz konto usługi i masz danych, tworzenie pierwszej modelu.

W tym zadaniu użyje przykładowej aplikacji do utworzenia pierwszego modelu.

1. Przede wszystkim należy pamiętać o [Zalecenia API Reference](http://go.microsoft.com/fwlink/?LinkId=759348).

1. Pobierz [Aplikację przykładową](http://go.microsoft.com/fwlink/?LinkID=759344) do folderu lokalnego.

1. Otwórz w programie Visual Studio rozwiązanie **RecommendationsSample.sln** znajduje się w folderze **C#** .

1. Otwórz plik **SampleApp.cs** . Uwaga Aby następujące kroki w pliku:
 + Tworzenie modelu
 + Dodawanie pliku wykazu
 + Dodawanie pliku zastosowania
 + Tworzenie modelu wyzwalacza
 + Uzyskiwanie zalecenie oparte na dwa elementy
<p></p>

1. Zamień wartości dla pola **AccountKey** klucz od 1 zadania.

1. Kroków rozwiązanie, pojawi się, jak zostanie utworzony w modelu.

1. Spróbuj wymienić pliki wykazu i zastosowania pobranego pozwala utworzyć nowy model Microsoft Store lub zalecenia książki. Należy zmienić z nazwą modelu i elementów, dla których możesz przejąć zalecenia.

1. Po utworzeniu modelu Zanotuj **Identyfikator modelu** jako będą potrzebne podczas żądania zalecenia w środowisku produkcyjnym.

>  Dowiedz się więcej na temat tworzenia typy i jak ma być obliczona jakości kompilacjach [tutaj](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Umieszczanie modelu produkcji! ###

Teraz, gdy wiesz, jak tworzenie modelu i stosowanie zalecenia, następnym krokiem jest umieszczanie go w produkcji w witrynie sieci Web aplikacji mobilnej lub integrowanie systemu CRM lub ERP.
Oczywiście każda z tych implementacji może się różnić. W związku z interfejsu API zalecenia wymagane są jako usługi sieci web, należy łatwo połączeń z dowolnego z tych różnych środowiskach.

**Ważne:**  Jeśli chcesz wyświetlić zalecenia z publicznej klienta (na przykład w witrynie handlu elektronicznego), należy utworzyć serwera proxy, aby zapewnić zalecenia. Jest to ważne, tak aby klucz interfejsu API nie jest dostępna do zewnętrznych obiektów (potencjalnie niezaufanych).

Oto kilka pomysłów lokalizacji, w której można korzystać zalecenia:

### <a name="product-page"></a>Strona produktu

**Zalecenia dotyczące elementu**
<p>Jeśli model został ćwiczenie danych sprzedaży, umożliwia klienta do *znalezienia produktów, które mogą być przydatne dla osób, które zostały kupione elementu źródła*.</p>
<p>Jeśli została ćwiczenie modelu danych, kliknij go, aby umożliwić klienta do *znalezienia produktów, które mogą kontrolowane przez osoby, które ostatnio elementu źródła*. Tego typu modelu może zwracać podobne elementy.</p>

**Często zakupiony razem zalecenia**
<p>A często zakupiony razem kompilacji może szkolenia, aby uzyskiwać zestawów elementów są może zostać zakupiony razem z tego elementu.</p>

### <a name="check-out-page"></a>Wyewidencjonowywanie strony

**Zalecenia dotyczące elementu**
<p>Zalecenia, które modelu można wykonać jako wprowadzania listę elementów. Aby możliwe było przekazywanie wszystkich elementów w koszyku jako dane wejściowe, aby uzyskać zalecenia.
W tym przypadku modelu zapewni zalecenia interesujące podane wszystkie elementy w koszyku.
</p>

### <a name="landing-page"></a>Strona główna

**Zalecenia dotyczące użytkownika**
<p>
Zalecenia, które modelu można wykonać jako wprowadź identyfikator użytkownika.  Użyje historii transakcji przez tego użytkownika o podanie spersonalizowane rekomendacje do określonego użytkownika.
</p>

Zapoznaj się z [Uzyskiwanie dokumentacji zalecenia elementu](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Co to jest dalej?
Gratulacje, jeśli go tak daleko! Aby dowiedzieć się, że możesz mogą odwiedzić witrynę pełne [Zalecenia API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) , jeśli masz pytania, nie się z nami umlapi@microsoft.com
