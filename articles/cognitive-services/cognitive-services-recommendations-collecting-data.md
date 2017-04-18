<properties
    pageTitle="Zbieranie danych na modelu szkolenie: komputer nauki zalecenia interfejsu API | Microsoft Azure"
    description="Azure maszynowego nauki zalecenia — zbieranie danych do przeszkolenie modelu"
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
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Zbieranie danych do przeszkolenie modelu #

Interfejs API zalecenia dowiaduje się na swoje transakcje ostatnich, aby dowiedzieć się, jakie elementy powinny być zalecane dla określonego użytkownika.

Po utworzeniu model konieczne będzie podanie dwuczęściowe informacji przed wykonaniem wszelkich szkoleń: plik wykazu i danych dotyczących użycia.

>   Jeśli jeszcze tego nie to już, zachęcamy do wykonania, [Przewodnik Szybki start](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Wykaz danych ##

### <a name="catalog-file-format"></a>Format pliku wykazu ###

Plik wykazu zawiera informacje o elementach ofercie do klienta.
Wykaz danych należy wykonać następujący format:

- Bez możliwości —`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Funkcje —`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Przykładowe wierszy w pliku katalogu

Bez funkcje:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Funkcje:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Szczegóły formatu

| Nazwa | Obowiązkowe | Typ |  Opis |
|:---|:---|:---|:---|
| Identyfikator elementu |Tak | [A-z], [a-z], [0-9], [_] & #40; Podkreślenie & #41; [-] & #40; łącznika & #41;<br> Maksymalna długość: 50 | Unikatowy identyfikator elementu. |
| Nazwa elementu | Tak | Dowolne znaki alfanumeryczne<br> Maksymalna długość: 255 | Nazwa elementu. |
| Element kategorii | Tak | Dowolne znaki alfanumeryczne <br> Maksymalna długość: 255 | Kategorii, do której ten element należy (np. gotowania książki teatralne...); może być pusty. |
| Opis | Nie, chyba że funkcje są prezentowanie (ale mogą być puste) | Dowolne znaki alfanumeryczne <br> Maksymalna długość: 4000 znaków | Opis tego elementu. |
| Lista funkcji | Brak | Dowolne znaki alfanumeryczne <br> Maksymalna długość: 4000; Maksymalna liczba funkcji: 20 | Rozdzielaną średnikami listę nazwy funkcji = wartości funkcji, która może służyć do poprawy rekomendacji modelu.|

#### <a name="uploading-a-catalog-file"></a>Przekazywanie pliku wykazu

Przyjrzyj się [interfejsu API odwołanie](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) do przekazywania pliku wykazu.  

Zauważ, że zawartość pliku wykazu mają być przekazywane w treści wezwania.

Po wysłaniu kilka plików wykazu do tego samego modelu kilka połączeń możemy wstawi tylko nowych elementów wykazu. Istniejące elementy pozostaną z wartości pierwotne. Nie można zaktualizować wykazu danych przy użyciu tej metody.

>   Uwaga: Maksymalny rozmiar pliku jest 200MB.
>   Maksymalna liczba elementów w wykazie obsługiwane wynosi 100 000 elementów.


## <a name="why-add-features-to-the-catalog"></a>Dlaczego warto dodać funkcje do katalogu?

Aparat zalecenia tworzy statystyczne model informujący o tym, jakie elementy są mogących polubiły lub zakupione. Jeśli masz nowy produkt, która nigdy nie ma już interakcji z go nie jest możliwe do tworzenia modelu o zdarzeniach współpracujących tylko. Załóżmy, że uruchamiania oferuje nową "dla dzieci violin" w sklepie, ponieważ nigdy nie zostały sprzedane tego violin przed nie można sprawdzić, jakie inne elementy do zaleca się z tym violin.

Który wspomniano, jeśli aparat zna informacji na temat tego violin (to znaczy jest musical instrument, może to być spowodowane wieku dzieci 7 – 10, nie jest violin drogich itp.), a następnie aparat można znaleźć na inne produkty z podobnych funkcji. Na przykład zostały sprzedane przez violin w przeszłości i zazwyczaj osób, które kupiły skrzypce nadają się kupić dyski CD muzyki i stojaki muzyki arkusza.  System można znaleźć te połączenia między funkcjami i rekomendacje w oparciu o funkcjach do nowych violin ma zastosowania nieco.

Funkcje są importowane jako część wykazu danych, a następnie ich pozycja (lub znaczenie funkcję w modelu) jest skojarzony po zakończeniu pozycja kompilacji. Pozycja funkcji można zmienić według struktury danych dotyczących użycia i typ elementów. Ale spójne zastosowania i elementów pozycję powinien mieć tylko małych zmianami. Pozycja funkcji jest liczba nieujemna. Cyfrę 0 oznacza, że ta funkcja nie zajęła (się dzieje, gdy wywołania tego interfejsu API przed zakończeniem pierwszego kompilacji pozycja). Daty, w którym został przypisany pozycję nosi nazwę aktualność wynik.


###<a name="features-are-categorical"></a>Funkcje są Categorical

Oznacza to, że należy utworzyć funkcje przypominających kategorii. Na przykład ceny = 9.34 nie jest funkcją kategorii. Z drugiej strony, funkcji, takich jak priceRange = Under5Dollars jest funkcją kategorii. Inny typowego błędu jest użycie nazwy elementu jako funkcji. Spowoduje to nazwa elementu, aby były unikatowe tak samo opisano kategorii. Upewnij się, że funkcje reprezentują kategorie elementów.


###<a name="how-manywhich-features-should-i-use"></a>Jak wielu /, które funkcje należy używać?


Ostatecznie zaleceń tworzyć obsługuje tworzenia modelu z funkcjami maksymalnie 20. Można także przypisywać funkcje więcej niż 20 do elementów w wykazie, ale spodziewasz się wykonaj kompilacji klasyfikację i wybierz tylko te funkcje należące wysoki. (Funkcja o pozycja 2.0 lub więcej jest dobrze funkcji). 


###<a name="when-are-features-actually-used"></a>Gdy funkcje używanych?

Funkcje są używane przez model, gdy jest za mało dane transakcji do zalecenia na informacjach transakcji tylko. Dlatego funkcje ma największy wpływ na "zimnej elementów" — kilka transakcji. Jeśli wszystkie elementy zawierają wystarczających informacji transakcji nie może być konieczne wzbogacanie modelu przy użyciu funkcji.


###<a name="using-product-features"></a>Korzystanie z funkcji produktu

Aby użyć funkcji w ramach usługi kompilacji, które należy:

1. Upewnij się, że katalogu zawiera funkcje po wysłaniu.

2. Wyzwalanie kompilacji klasyfikowania. Nie wywołuje analizy ważność i pozycję funkcji.

3. Wyzwalanie konstruowania zaleceń, ustawiając następujące Tworzenie parametrów: Ustaw useFeaturesInModel allowColdItemPlacement ma wartość PRAWDA, PRAWDA, a modelingFeatureList powinna być równa przecinkami listą funkcje, których chcesz użyć w celu poprawienia modelu. Aby uzyskać więcej informacji, zobacz [zalecenia dotyczące tworzenia parametrów typu](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Dane dotyczące użycia ##
Plik zastosowania zawiera informacje dotyczące sposobu używania tych elementów lub transakcje z Twojej firmy.

#### <a name="usage-format-details"></a>Informacje dotyczące sposobu użycia formatu
Plik zastosowania jest pliku CSV (wartości rozdzielane przecinkami), gdzie każdy wiersz w pliku zastosowania reprezentuje interakcji między użytkownikiem a elementu. Każdy wiersz jest formatowana w następujący sposób:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Nazwa  | Obowiązkowe | Typ | Opis
|-------|------------|------|---------------
|Identyfikator użytkownika|         Tak|[A-z], [a-z], [0-9], [_] & #40; Podkreślenie & #41; [-] & #40; łącznika & #41;<br> Maksymalna długość: 255 |Unikatowy identyfikator użytkownika.
|Identyfikator elementu|Tak|[A-z], [a-z], [0-9], [& #95;] & #40; Podkreślenie & #41; [-] & #40; łącznika & #41;<br> Maksymalna długość: 50|Unikatowy identyfikator elementu.
|Czas|Tak|Data w formacie: YYYY-MM-ddTHH (przykład 2013-06-20T10:00:00)|Czas danych.
|Zdarzenia|Brak | Jedną z następujących czynności:<br>• Kliknij przycisk<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Zakupu| Typ transakcji. |

#### <a name="sample-rows-in-a-usage-file"></a>Przykładowe wiersze w pliku zastosowania

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Przekazywanie pliku zastosowania

Przyjrzyj się [interfejsu API odwołanie](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) do przekazywania plików zastosowania.
Zwróć uwagę, musisz przekazać zawartość pliku zastosowania jako treści połączenia HTTP.

>  Uwaga:

>  Maksymalny rozmiar pliku: 200MB. Może przekazać kilka plików zastosowania.

>  Należy przekazać plik wykazu przed rozpoczęciem dodawania danych dotyczących użycia do modelu. Tylko elementy w pliku wykazu zostanie użyty w fazie szkolenie. Wszystkie pozostałe elementy będą ignorowane.

## <a name="how-much-data-do-you-need"></a>Ile danych są potrzebne?

Jakość modelu jest stopniu zależą od jakości i ilości danych.
System dowiaduje się użytkowników kupionego różnych elementów (nazywamy to Współtworzenie wystąpień). Kompilacjach zmianie wysokości progów również jest ważne elementy, które zostały nabyte w tym samym transakcje. 

Regułą ma większości elementów w 20 transakcje lub więcej, więc jeśli wcześniej używano 10 000 elementów w wykazie, zaleca czy masz co najmniej 20 razy tej liczbie transakcje i około 200 000. Ponownie jest to zasada. Konieczne będzie poeksperymentować z danymi.

Po utworzeniu modelu można wykonywać [offline oceny](cognitive-services-recommendations-buildtypes.md) Aby sprawdzić, jak modelu prawdopodobnie do wykonania.
