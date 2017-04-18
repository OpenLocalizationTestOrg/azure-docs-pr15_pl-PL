<properties 
    pageTitle="Jak wdrażać nawigacji aspektowej w wyszukiwaniu Azure | Microsoft Azure | kategorie nawigacji wyszukiwania" 
    description="Dodawanie nawigacji Aspektowej aplikacjom Integracja z wyszukiwarki Azure, chmury hostowana usługa wyszukiwania na Microsoft Azure." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Jak wdrażać nawigacji aspektowej w wyszukiwaniu Azure

Nawigacji aspektowej jest mechanizmu filtrowania, który umożliwia nawigację samodzielnej nauki Przechodzenie do szczegółów w aplikacji wyszukiwania. Termin "nawigacji aspektowej" może być nieznanych, jest niemal zakładając, że użyto przed. Jak w poniższym przykładzie pokazano, nawigacji aspektowej ma więcej niż kategorie używane do filtrowania wyników.

## <a name="what-it-looks-like"></a>Jak wygląda

 ![][1]
  
Aspekty może pomóc w znalezieniu, czego szukasz, zapewniając, że nie można uzyskać wyników. Projektant aspekty umożliwiają uwidaczniane najbardziej przydatnych kryteria wyszukiwania do nawigowania między Boże usługi wyszukiwania. W przypadku aplikacji Sklep internetowy nawigacji aspektowej często jest tworzona przez marek, działy (buty dzieci), rozmiar, cena, popularności i oceny. 

Implementacji nawigacji aspektowej różni się w technologii wyszukiwania i mogą być bardzo złożone. W polu Wyszukaj Azure nawigacji aspektowej jest tworzona podczas czas kwerendy przy użyciu pól nadany wcześniej określone w schematu. W kwerendach, które tworzy aplikacji kwerendy, musisz wysłać *Parametry kwerendy Faseta* aby otrzymać wartości filtru Faseta dostępne w tym zestawie wyników dokumentu. Faktycznie przycięcia wynik dokumentu ustawić, muszą być stosowane w aplikacji `$filter` wyrażenia.

W odniesieniu do opracowywania aplikacji pisania kodu, który tworzy kwerendy stanowi część pracy. Wiele zachowania aplikacji, które mają z nawigacji aspektowej jest udostępniany przez usługę, w tym obsługa konfigurowania zakresów i uzyskiwanie liczniki dla Faseta wyników. Usługa zawiera również za pośrednictwem ustawień domyślnych, które ułatwiają uniknąć struktury nawigacji niewygodna. 

Ten artykuł zawiera następujące sekcje:

- [Sposoby tworzenia go](#howtobuildit)
- [Tworzenie warstwy prezentacji](#presentationlayer)
- [Tworzenie indeksu](#buildindex)
- [Sprawdzanie jakości danych](#checkdata)
- [Konstruowanie kwerendy](#buildquery)
- [Porady dotyczące sterowania nawigacji aspektowej](#tips)
- [Na podstawie wartości w zakresie nawigacji aspektowej](#rangefacets)
- [Oparte na GeoPoints nawigacji aspektowej](#geofacets)
- [Przetestuj](#tryitout)

##<a name="why-use-it"></a>Dlaczego warto używać go
Najskuteczniejszą aplikacji wyszukiwania ma wiele modeli interakcji oprócz pola wyszukiwania. Nawigacji aspektowej jest punktem wejścia alternatywny do wyszukiwania, oferowanie wygodnym sposobem ręcznie wpisywać wyrażeniach złożonych wyszukiwania.

##<a name="know-the-fundamentals"></a>Znasz podstawy

Jeśli jesteś nowym użytkownikiem usługi wyszukiwania rozwoju, najlepszym sposobem traktować nawigacji aspektowej jest, że jest wyświetlany możliwości wyszukiwania samodzielnej nauki. Jest to typ możliwości wyszukiwania rozwijania, w zależności od wstępnie zdefiniowanych filtrów, używane do szybko zawęzić dół wyników wyszukiwania za pomocą akcji punkt i kliknij. 

**Model interakcji**

Obsługę wyszukiwania dla nawigacji aspektowej jest iteracyjne, więc warto, zacznij od zrozumienia za pomocą sekwencji zapytania, które ujawniać w odpowiedzi na działanie użytkownika.

Punkt początkowy jest na stronie aplikacji, która zapewnia nawigacji aspektowej, zazwyczaj umieszczane na zewnętrznej. Nawigacji aspektowej jest zazwyczaj strukturze drzewa z polami wyboru dla każdej wartości lub które można kliknąć tekst. 

1.  Kwerendy wysyłane do wyszukiwania Azure określa strukturę nawigacji aspektowej przez jeden lub więcej parametrów kwerendy Faseta. Na przykład kwerenda może zawierać `facet=Rating`, być może z `:values` lub `:sort` opcję, aby uściślić prezentacji.
2.  Warstwy prezentacji są renderowane za pomocą stronę wyszukiwania, która zawiera nawigacji aspektowej przy użyciu aspekty określonej na żądanie.
3.  Mieć struktury nawigacji aspektowej, zawierającej klasyfikacji, użytkownik kliknie "4" Aby wskazać, że powinny być wyświetlane tylko produkty z przedziałem 4 lub nowszym. 
4.  W odpowiedzi aplikacja wysyła kwerendę zawierającą`$filter=Rating ge 4` 
5.  Warstwy prezentacji aktualizuje stronę, przedstawiający zestaw wyników ograniczona, zawierającą tylko te elementy, które spełniają nowe kryteria (w tym przypadku produktów ocenianie 4 i nowsze).

Aspekt jest parametru zapytania, ale nie należy mylić z wejście kwerendy. Nigdy nie jest używany jako kryterium w kwerendzie. Zamiast tego traktować Faseta parametry kwerendy jako danych wejściowych do struktury nawigacji, który wróci w odpowiedzi. Dla każdego parametru zapytania Faseta podane wyszukiwania Azure oceni, ile dokumenty znajdują się w części wyników dla każdej wartości Faseta.

Powiadomienie o `$filter` w kroku 4. Jest to ważnych aspektów nawigacji aspektowej. Mimo że aspekty i filtry są niezależne w interfejsie API, będą potrzebne do przeprowadzania obsługi, które mają. 

**Desenie projektu**

Kod aplikacji deseniu jest umożliwia powrót struktury nawigacji aspektowej wraz z wyników Faseta oraz wyrażenie $filter, która obsługuje to zdarzenie kliknij pozycję Faseta parametry kwerendy. Wyobrazić `$filter` zwracane wyrażenie jako kodu źródłowego rzeczywisty skracania wyników wyszukiwania do warstwy prezentacji. Mieć aspekt kolorów, klikając pozycję kolor czerwony jest wdrażane za pośrednictwem `$filter` wyrażenie wybierające tylko tych elementów, które mają kolor czerwony. 

**Podstawy kwerendy wyszukiwania Azure**

W polu Wyszukaj Azure żądania jest określona przez jeden lub więcej parametrów kwerendy (zobacz [Wyszukiwania dokumentów](http://msdn.microsoft.com/library/azure/dn798927.aspx) opis każdego z nich). Parametry kwerendy są wymagane, ale są konieczne co najmniej jeden w kolejności kwerendy jest nieprawidłowy.

Dokładność, zazwyczaj traktować jako możliwość odfiltrowywania trafień nie ma znaczenia, uzyskuje się przez co najmniej jedną z następujących wyrażeń:

- **Wyszukiwanie =**<br/>
Wartość tego parametru stanowi wyrażenie wyszukiwania. Może to być pojedynczy blok tekstu lub złożonego wyrażenia wyszukiwania zawierająca wiele warunków i operatory. Na serwerze wyrażenie wyszukiwania jest używana do przeszukiwania pełnego tekstu, kwerenda pola wyszukiwania w indeksie dla pasujących terminów zwraca wyników w kolejności pozycja. Jeśli ustawisz `search` na wartość pustą, kwerendy wykonanie jest przez cały indeks (oznacza to, że `search=*`). W tym przypadku innych elementów kwerendy, takie jak `$filter` lub punktowanie profilu, będzie podstawowego czynników wpływających na dokumenty, które są zwracane `($filter`) i w jakiej kolejności (`scoringProfile` lub `$orderb`y).

- **$filter =**<br/>
Filtr jest zaawansowanych mechanizm ograniczanie rozmiaru wyników wyszukiwania na podstawie wartości atrybutów określonego dokumentu. A `$filter` jest obliczana jako pierwsza, a po nim logiki faceting, generowany przez dostępne wartości i odpowiadające im liczniki dla każdej wartości

Wyrażenia złożone wyszukiwania zmniejszy wydajność kwerendy. Jeśli to możliwe, korzystać z wyrażenia filtru dobrze zbudowane, aby zwiększyć dokładność i zwiększyć wydajność kwerendy.

Aby lepiej zrozumieć, jak filtr dodaje większą precyzją, porównanie złożonego wyrażenia wyszukiwania zawierającej wyrażenia filtru:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Mimo że oba zapytania są prawidłowe, druga jest przełożonego, jeśli szukasz innych niż motele parkowania w Seattle. Pierwszej kwerendy zależy od tych określonych wyrazów wymienionych lub nie wymienione w ciągu pól, takich jak nazwa, opis i inne pola zawierającego dane można wyszukiwać. Drugiego zapytania wyszukuje dokładne dopasowanie na dane strukturalne i może być o wiele bardziej precyzyjne.

W aplikacjach, które zawierają nawigacji aspektowej ma upewnij się, że każda akcja użytkownika w strukturze nawigacji aspektowej towarzyszy Zawężanie wyników wyszukiwania, stosując wyrażenia filtru.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Sposoby tworzenia go

Nawigacji aspektowej w wyszukiwaniu Azure jest zaimplementowana w kodzie aplikacji tworzy wezwanie, ale zależy od wstępnie zdefiniowanych elementów schematu.

Wstępnie zdefiniowane w wyszukiwaniu indeks jest `Facetable [true|false]` atrybut indeksu, ustawić dla wybranych pól, aby włączyć lub wyłączyć ich stosowania w strukturze nawigacji aspektowej. Bez `"Facetable" = true`, nie można używać pola w nawigacji Faseta.

W momencie kwerendy kodzie aplikacji tworzy żądanie zawiera `facet=[string]`, zawierającego pola Faseta przez parametr wezwanie. Kwerenda może mieć wiele aspekty, takie jak `&facet=color&facet=category&facet=rating`, każdy z nich oddzielone handlowe "i" (&) znaków.

Kod aplikacji należy również tworzyć `$filter` wyrażenia do obsługi zdarzeń kliknij pozycję w nawigacji aspektowej. A `$filter` zmniejsza wyników wyszukiwania przy użyciu wartości Faseta jako kryteriów filtru.

Wyszukiwanie Azure zwraca wyników wyszukiwania w wyszukiwane terminy, wprowadzone przez użytkownika, oraz aktualizacje struktury nawigacji aspektowej. W polu Wyszukaj Azure nawigacji aspektowej jest konstrukcji jednopoziomowa, z wartościami aspekt i zlicza, ile wyników znajdują się dla każdego z nich.

Warstwy prezentacji w kodzie miejsce pracy użytkownika. Należy go listy części składowe nawigacji aspektowej, takie jak etykiety, wartości, pola wyboru i liczby. Interfejsu API usługi REST wyszukiwania Azure jest niezależne od platformy, należy więc niezależnie od języka i chcesz platformy. Najważniejsze ma zawierać elementy interfejsu użytkownika, które obsługują odświeżanie przyrostowe, ze stanem zaktualizowanego interfejsu użytkownika, jak każdy dodatkowe Faseta jest zaznaczone. 

W poniższych sekcjach zostaną wykonane dokładniejsze przedstawienie sposobu tworzenia każdej strony, zaczynając od warstwy prezentacji.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Tworzenie warstwy prezentacji

Praca z warstwy prezentacji może ułatwić odkrywanie wymagania, które mogą być pominięte w przeciwnym razie i opis możliwości są niezbędne do wyników wyszukiwania.

W odniesieniu do nawigacji aspektowej strony sieci web lub aplikacji wyświetla strukturę nawigacji aspektowej, wykrywa danych wprowadzonych przez użytkownika na stronie i wstawia zmienionych elementów. 

Dla aplikacji sieci web AJAX jest często używane w warstwie prezentacji, ponieważ umożliwia odświeżenie zmiany przyrostowe. Można także użyć programu ASP.NET MVC lub innych platform wizualizacja, którego można nawiązać połączenie z usługą Azure wyszukiwania za pomocą protokołu HTTP. Przykładowa aplikacja zawarte w tym artykule — **Wykazu AdventureWorks** — dzieje wniosek MVC programu ASP.NET

Następującym przykładzie pobrane z pliku **index.cshtml** przykładowej aplikacji tworzy dynamiczne strukturę HTML do wyświetlania nawigacji aspektowej na stronie wyników wyszukiwania. W próbce nawigacji aspektowej jest wbudowany w strony wyników wyszukiwania i pojawia się po użytkownika zostały przesłane wyszukiwany termin.

Powiadomienie, że każdy aspekt ma etykietę (kolorów, kategorie ceny), powiązania z polem aspektowej (kolor, NazwaKategorii, listPrice) i w `.count` parametr, używane do zwracania liczba odnalezionych dla wyniku Faseta elementów.

  ![][2]
 

> [AZURE.TIP] Podczas projektowania strony wyników wyszukiwania, należy pamiętać o dodaniu mechanizm wyczyszczenie aspekty. Użycie pola wyboru, użytkownicy mogą łatwo intuit czyszczenie filtrów. Dla innych układów może być konieczne deseniu łącza do stron nadrzędnych lub innej metody creative. Na przykład w wykazie AdventureWorks przykładowej aplikacji Kliknięcie tytułu, wykazu AdventureWorks, aby zresetować strony wyszukiwania.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Tworzenie indeksu

Faceting jest włączona na podstawie według pola w indeksie, za pomocą tego atrybutu indeksu: `"Facetable": true`.  
Wszystkie typy pól, które niekiedy może być używane w nawigacji aspektowej są `Facetable` domyślnie. Takie typy pól `Edm.String`, `Edm.DateTimeOffset`, a dla wszystkich typów pól liczbowych (zasadniczo dla wszystkich typów pól są facetable z wyjątkiem `Edm.GeographyPoint`, nie można używać w nawigacji aspektowej). 

Podczas tworzenia indeksu, najlepszym rozwiązaniem dla nawigacji aspektowej jest jawnie wyłączy faceting dla pól, które nie mogą być używane jako Faseta.  W szczególności pól ciąg dla pojedynczych wartości, takich jak nazwa produktu lub identyfikator powinna być równa `"Facetable": false` aby zapobiec przypadkowym (i nieskuteczne) użycia nawigacji aspektowej.

Schemat AdventureWorks wykazu aplikacji przykładowych (przycięte z niektórych atrybutów, aby zmniejszyć ogólny rozmiar) jest następujący:

 ![][3]
 
Należy zauważyć, że `Facetable` jest wyłączona dla pól Parametry, które nie powinny być używane jako aspekty, takie jak identyfikator lub nazwę. Włączanie faceting poza miejsce, w którym jej nie potrzebujesz pomaga zachować mały rozmiar indeksu, a zazwyczaj zwiększa wydajność.

> [AZURE.TIP] Zgodnie z zaleceniami dotyczącymi obejmuje pełny zestaw atrybutów indeksu dla każdego pola. Mimo że `Facetable` jest domyślnie włączona w przypadku niemal wszystkich pól celowo ustawienie każdego atrybutu może pomóc w Pomyśl za pośrednictwem skutki każda decyzja schematu. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Sprawdzanie jakości danych 

Podczas tworzenia aplikacji oparte na danych, przygotowywanie danych często jest jedną z większym części zadania. Wyszukiwanie aplikacji jest nie wyjątku. Jakości danych ma bezpośredni wpływ na czy struktury nawigacji aspektowej zostaje zgodnie z oczekiwaniami ustalenie, a także jego skuteczność ułatwiają tworzenia filtrów zmniejszających wynik.

W polu Wyszukaj Azure Boże wyszukiwania został utworzony z dokumentów, które wypełniają indeksu. Dokumenty zawierają wartości, które są używane do obliczenia aspekty. Jeśli chcesz aspekt marki lub cena każdego dokumentu powinny zawierać wartości *Nazwamarki* i *ProductPrice* są prawidłowe, spójne i wydajność jako opcję filtru.

Poniżej przedstawiono kilka przypomnienia co przewijać dla:

- Dla każdego pola, który chcesz Faseta, zastanów się czy zawiera wartości, które są odpowiednie jako filtry w wyszukiwaniu samodzielnej nauki. Wartości powinny być krótkie, opisu i wystarczająco charakterystyczne do oferowania zrozumiałe między opcjami konkurencyjnych.
- Błędy pisowni lub prawie pasujące wartości. Jeśli aspekt na kolor, a wartości pól dołączyć pomarańczowy oraz Ornage (błąd pisowni), Faseta, oparte na polu kolor chcesz wznowić oba.
- Mieszane wielkość liter tekstu można też zrobić spustoszenie w nawigacji aspektowej z pomarańczowy i pomarańczowy wyświetlane jako dwa różne wartości. 
- Jedno- i liczba mnoga wersji tej samej wartości może spowodować Faseta oddzielnych dla każdej z nich.

Jak można wyobrazić sobie starannością podczas przygotowywania danych jest aspektem skutecznych nawigacji aspektowej.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Konstruowanie kwerendy

Kod, którego pisanie dla konstruowania kwerend należy określić wszystkich części prawidłową kwerendą, tym wyszukiwanych wyrażeń, aspekty, filtry, wyników profile — nic używane do sformułowania żądanie. W tej sekcji omówimy, gdzie aspekty pasują do kwerendy i używania filtrów z aspektami do przeprowadzania zestaw wyników ograniczona.

Przykład często to dobre miejsce do rozpoczęcia. Poniższy przykład pobrane z pliku **CatalogSearch.cs** tworzy żądanie, które tworzy nawigacji Faseta na podstawie koloru, kategoria i cena. 

Zwróć uwagę, że aspekty są integralną w tej aplikacji próbki. Funkcję wyszukiwania w katalogu AdventureWorks zaprojektowano wokół nawigacji aspektowej i filtry. Jest to widoczne w widocznym położenie nawigacji aspektowej na stronie. Przykładowa aplikacja zawiera identyfikatora URI parametrów aspekty (kolor, kategorii, ceny) jako właściwości metody Search (jako wykonane w przykładowej aplikacji).

  ![][4]
 
Parametry kwerendy Faseta jest ustawiona na pole, a następnie w zależności od typu danych, może być dodatkowo parametryczne przez rozdzielany przecinkami listę, która zawiera `count:<integer>`, `sort:<>`, `intervals:<integer>`, i `values:<list>`. Lista wartości jest obsługiwane dla danych liczbowych, podczas konfigurowania zakresów. Wyświetlać [Dokumenty wyszukiwania (API wyszukiwania Azure)](http://msdn.microsoft.com/library/azure/dn798927.aspx) , aby uzyskać informacje dotyczące sposobu użycia.

Wraz z aspekty żądanie opracowany przez aplikację należy również tworzyć filtrów w celu zawężenia zestawu dokumentów candidate na podstawie wyboru wartość aspekt. W sklepie roweru nawigacji aspektowej zapewnia wskazówek na pytania, takie jak "jakie kolory, producenci i typy rowery są dostępne", podczas filtrowania odpowiedzi pytania, takie jak "które dokładnie rowery są oznaczone kolorem czerwonym, rowery górskie, w tym ceny zakres".

Gdy użytkownik kliknie "Czerwony", aby wskazać tylko czerwone produkty mają być wyświetlane, następnej kwerendy wysyła aplikacja będzie zawierać `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Najważniejsze wskazówki dotyczące nawigacji aspektowej

Poniżej przedstawiono kilka wskazówek.

- **Dokładność**<br/>
Użyj filtrów. Jeśli użytkownik korzysta tylko wyszukiwanych wyrażeń samodzielnie wynikające może spowodować, że dokument mają zostać zwrócone, który nie ma wartość precyzyjnie Faseta w jednym z pól. 

- **Pola docelowe**<br/>
W grafice rozwijania szczegółów zazwyczaj chcesz zawierać tylko dokumenty, które mają wartość Faseta w określonym polu (aspektowej), nie dowolne miejsce w wielu wszystkie pola wyszukiwania. Dodawanie filtru wzmacnia pola docelowego, przekierowując usługi w celu wyszukiwania tylko w polu aspektowej dla pasującą wartość.

- **Indeks wydajności**<br/>
Jeśli aplikacja używa wyłącznie nawigacji aspektowej (to znaczy nie wyszukiwania pole), możesz oznaczyć jako `searchable=false`, `facetable=true` da bardziej zwarty indeksu. Ponadto indeksowania występuje tylko na wartości całego Faseta, nie dzielenie wyrazów lub indeksowania części wartości kilku słów.

- **Wydajność**<br/>
Filtry zawęzić zestaw dokumentów candidate wyszukiwania i wyłączyć je z klasyfikowania. Jeśli masz duże zestawu dokumentów przy użyciu rozwijania Faseta bardzo selektywne w dół często pozwala znacznie lepszą wydajność.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Porady dotyczące sterowania nawigacji aspektowej

Poniżej znajduje się arkusz Porada wskazówek dotyczących problemy specyficzne dla.

**Dodawanie etykiet dla każdego pola w nawigacji Faseta**

Etykiety są zazwyczaj definiowane w HTML lub formularza (**index.cshtml** w przykładowej aplikacji). Ma nie interfejsu API Azure wyszukiwania aspekt nawigacji etykiet lub innego rodzaju metadanych.

**Definiowanie pola, które mogą być używane jako Faseta**

Odwoływanie się, że schemat indeksem określa pola, które są dostępne do użycia jako Faseta. Przy założeniu, że pole jest facetable, kwerenda umożliwia określenie pól do aspekt przez. Pola, według których są faceting zawiera wartości, które są wyświetlane poniżej etykiety. 

Wartości, które są wyświetlane w obszarze każdej etykiecie są pobierane z indeksu. Na przykład jeśli pole Faseta jest *Kolor*, dostępne dodatkowe funkcje filtrowania wartości będą wartości dla tego pola (czerwony, czarny i tak dalej).

W przypadku liczbowych i Data/Godzina wartości tylko wartości można jawnie ustawione w polu Faseta (na przykład `facet=Rating,values:1|2|3|4|5`). Lista wartości jest dozwolone dla tych typów pól w celu uproszczenia odstęp Faseta wyników na ciągłe zakresy (albo na podstawie wartości liczbowe lub okresy zakresy). 

**Przycinanie Faseta wyników**

Faseta wyniki są znalezionych w wynikach wyszukiwania, które są zgodne z terminem Faseta dokumentów. W poniższym przykładzie w wynikach wyszukiwania, *przetwarzania danych w chmurze*, 254 elementy również mają *wewnętrznych Specyfikacja* jako typu zawartości. Elementy nie są zawsze wzajemnie wykluczających się. Jeśli element spełnia kryteria obu filtrów, jest liczony w każdej z nich. Jest to możliwe, kiedy faceting na `Collection(Edm.String)` pól, które są często używane do wykonania, znakowanie dokumentów.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Ogólnie Jeśli stwierdzisz, że wyniki Faseta trwale są zbyt duże, zalecamy, możesz dodać więcej filtrów, zgodnie z opisem w sekcjach wcześniejszych, aby nadać więcej opcji zawęzić wyszukiwanie użytkowników aplikacji.

**Ogranicz liczbę elementów w nawigacji Faseta**

Dla każdego pola aspektowej w drzewie nawigacji jest domyślny limit 10 wartości. To ustawienie domyślne sens struktury nawigacji, ponieważ zachowuje listy wartości do odpowiednich rozmiarach. Można zastąpić domyślną przypisując wartość do zliczenia.

- `&facet=city,count:5`Określa, że tylko pierwszych 5 miasta znaleziony w górnym rogu sklasyfikowane wyniki są zwracane jako wynik Faseta. Podane wyszukiwany termin "lotnisk" i 32 dopasowania, jeśli określa kwerendę `&facet=city,count:5`, w wynikach Faseta zostaną uwzględnione tylko pięć pierwszych unikatowych miasta o wszystkich dokumentów w wynikach wyszukiwania.

Powiadomienie o rozróżnianie Faseta wyników i wyników wyszukiwania. Wyniki wyszukiwania są wszystkie dokumenty, które są zgodne z kwerendy. Aspekt wyniki są zgodne wyniki dla każdej wartości aspekt. W tym przykładzie wyniki wyszukiwania będą obejmowały nazw miast, które nie są na liście klasyfikacji Faseta (5 w naszym przykładzie). Wyniki, które są odfiltrowywane za pośrednictwem nawigacji aspektowej stają się widoczne dla użytkownika, gdy dana osoba czyści aspekty lub wybiera inne aspekty oprócz miasto. 

> [AZURE.NOTE] Omawiane `count` Jeśli istnieje więcej niż jeden typ może być trudne. Następujące tabela zawiera krótkie podsumowanie użycia terminów w interfejsu API wyszukiwania Azure, przykładowy kod i dokumentacji. 

- `@colorFacet.count`<br/>
W kodzie prezentacji parametru liczba powinna być widoczna na Faseta, umożliwia wyświetlanie liczbę wyników aspekt. W wynikach Faseta liczba wskazuje liczbę dokumentów, które odpowiadają na Faseta terminów lub zakresu.

- `&facet=City,count:12`<br/>
W kwerendzie Faseta można ustawić wartość liczby.  Wartość domyślna to 10, ale można ustawić ją wyżej lub niżej. Ustawianie `count:12` pobiera pasuje do góry 12 w wynikach Faseta przez liczbę dokumentów.

- "`@odata.count`"<br/>
W odpowiedzi na kwerendę ta wartość wskazuje liczbę pasujących elementów w wynikach wyszukiwania. Na ogół jest większy niż suma wszystkie wyniki Faseta łączone ze względu na obecność elementów, które są zgodne z wyszukiwany termin, ale mają odpowiedników wartość Faseta.


**Poziomy nawigacji aspektowej** 

Jak wspomniano, istnieje bezpośredni obsługę zagnieżdżania aspekty w hierarchii. Okno nawigacji aspektowej obsługuje tylko jeden poziom filtrów. Jednak istnieją obejścia. Czy kodowanie Faseta hierarchicznej struktury `Collection(Edm.String)` wpisem jeden punkt na hierarchii. Zastosowanie tego obejścia wykracza poza zakres tego artykułu, ale można zapoznać zbiorów w [OData według przykładu](http://msdn.microsoft.com/library/ff478141.aspx). 

**Sprawdzanie poprawności pola**

W przypadku tworzenia listy aspekty dynamicznie oparte na danych wprowadzonych przez użytkownika niezaufanych, należy albo sprawdzić, czy nazwy pól aspektowej są prawidłowe lub escape imiona i nazwiska podczas tworzenia adresów URL za pomocą `Uri.EscapeDataString()` .NET lub jej równowartość w swojej platformy wybór.

**Liczby w Faseta wyników**

Dodając filtr do aspektowej kwerendy, warto zachowanie poufności Faseta (na przykład `facet=Rating&$filter=Rating ge 4`). Technicznego, Faseta = ocena nie jest potrzebna, ale zachowanie go zwraca liczby wartości Faseta oceny 4 lub nowszy. Na przykład jeśli użytkownik kliknie "4" i kwerenda zawiera filtr dla większe lub równe "4", liczone są zwracane dla każdego klasyfikacji, który jest 4 i w górę.  

**Sharding wpływ na Faseta liczników**

W niektórych przypadkach może się okazać, że statystyką Faseta, które nie pasują zestawów wyników (zobacz [nawigacji Aspektowej w wyszukiwaniu Azure (wpis na forum)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Zlicza Faseta może być niedokładne z powodu architektura sharding. Każdy indeks wyszukiwania zawiera wiele odłamki i każdy z nich raporty aspekty góry N przez liczbę dokumentów, które następnie są łączone w jeden wynik. Jeśli niektóre odłamki masz wiele pasujących wartości, gdy inne osoby mają mniejsze, może się okazać że niektóre wartości Faseta brakuje lub w obszarze obliczana w wynikach.

Mimo że to zachowanie może się zmienić w dowolnym momencie to zachowanie w przypadku wystąpienia dzisiaj, możesz rozpocząć pracę wokół niego, sztucznie pompowania statystykę:<number> do bardzo dużej liczby wymuszenie pełny raportów z każdego shard. Jeśli wartość argumentu liczba: jest większa niż lub równa liczbie wartości unikatowe w polu, masz gwarancję dokładne wyniki. Jednak podczas zlicza dokumentu są naprawdę wysokie, jest zmniejszenie wydajności, więc używać z tę opcję rozwagą.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Nawigacja Faseta na podstawie wartości zakresu

Faceting zakresach jest wymagane typowych aplikacji wyszukiwania. Obsługiwane są zakresy danych liczbowych i wartości daty/godziny. Więcej informacji o poszczególnych podejście w [Dokumentach wyszukiwania (Azure wyszukiwania API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure wyszukiwania ułatwia budowy zakres, zapewniając dwie metody dla przetwarzania zakresu. W przypadku obu metod wyszukiwania Azure tworzy odpowiednie zakresy podane wartości wejściowych podany. Na przykład jeśli określony zakres wartości 10 | 20 | 30, zostanie automatycznie utworzony zakresów 0 -10, od 10 do 20, 20 – 30. Przykładowa aplikacja usuwa wszystkie interwałów, które są puste. 

**Metoda 1: Należy użyć parametru interwału**<br/>
Aby ustawić aspekty cena w przyrostach 10 zł, należy określić:`&facet=price,interval:10`


**Metoda 2: Używanie listy wartości**<br/>
Aby dane liczbowe można używać listy wartości.  Należy rozważyć zakres aspekt listPrice wyświetlany w następujący sposób:

  ![][5]

Zakres jest określony w pliku **CatalogSearch.cs** przy użyciu listy wartości:

    facet=listPrice,values:10|25|100|500|1000|2500

Poszczególne zakresy opracowano przy użyciu 0 jako punktu startowego wartości na liście jako punktu końcowego i następnie przycięte poprzedniego zakresu utworzyć osobne odstępach. Azure wyszukiwania będą w tym jako część nawigacji aspektowej. Nie masz pisanie kodu dla każdego okresu struktury.

### <a name="build-a-filter-for-facet-ranges"></a>Tworzenie filtru dla zakresów Faseta ###

Aby filtrować dokumenty na podstawie zakresu wybrane przez użytkownika, można użyć `"ge"` i `"lt"` filtrowanie operatorów w wyrażeniu dwuczęściowe definiujące punkty końcowe zakresu. Na przykład, jeśli użytkownik wybierze zakres 10-25, filtr będzie `$filter=listPrice ge 10 and listPrice lt 25`.

W przykładowej aplikacji aby ustawić punkty końcowe wyrażenie filtru korzysta z **priceFrom** i **priceTo** parametrów. **Utwórz filtr** w **CatalogSearch.cs** zawiera wyrażenie filtru, które udostępnia dokumenty w zakresie.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Nawigacja filtrowane według GeoPoints

Wspólne Aby wyświetlić filtruje pomagające w wybranym magazynu, restauracji lub miejsce docelowe oparte na jego odległość do Twojej bieżącej lokalizacji. Gdy filtr tego typu może wyglądać nawigacji aspektowej, jest faktycznie tylko filtru. Firma Microsoft tu wspomnieć dla osób, które kto specjalnie odnaleźć implementacji wskazówki dotyczące tego problemu określonego projektu.

Dostępne są dwie funkcje geograficzne w wyszukiwaniu Azure, **geo.distance** i **geo.intersects**.

- Funkcja **geo.distance** zwraca odległość w km między dwoma punktami, jednego pola, a z których jedna jest stałą przesyłane jako część filtru. 

- Funkcja **geo.intersects** zwraca wartość PRAWDA, jeśli dany punkt znajduje się w danym wielokąta, gdzie punktu jest polem i Wielokąt jest określony jako stałą listy współrzędnych w ramach filtru.

Przykłady filtru można znaleźć w [składni wyrażeń OData (wyszukiwanie Azure)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Przetestuj

Azure wyszukiwania Adventure Works pokaz w witrynie Codeplex zawiera przykłady zawarte w tym artykule. Podczas pracy z wynikami wyszukiwania, obejrzyj adresu URL zmian w budowie kwerendy. Ta aplikacja się dzieje, aby dołączyć aspekty identyfikator URI podczas wybierania każdej z nich.

1.  Konfigurowanie aplikacji przykładowych do przy użyciu adresu URL usługi i klucz interfejsu api. 

    Zwróć uwagę, schemat, która jest zdefiniowana w pliku Plik Program.cs projektu CatalogIndexer. Określa pola facetable koloru, listPrice, rozmiar, grubość, NazwaKategorii i modelName.  Tylko niektóre z tych (kolor, listPrice, NazwaKategorii) są faktyczną zaimplementowana w nawigacji aspektowej.

3.  Uruchom aplikację. 

    Na początku jest widoczny tylko w polu wyszukiwania. Kliknij przycisk Wyszukaj od razu w celu uzyskania wyników wszystkich lub wpisz wyszukiwany termin.

    ![][7]
 
4.  Wprowadź wyszukiwany termin, na przykład roweru i kliknij przycisk **Wyszukaj**. Kwerenda wykonywana szybko.
 
    Struktura nawigacji aspektowej jest również zwracana z wyników wyszukiwania.  W adresie URL aspekty kolorów, kategorie i ceny jest wartością null. Na stronie wyników wyszukiwania struktury nawigacji aspektowej zawiera liczbę wynik każdego Faseta.

     ![][8]
 
5.  Kliknij przycisk kolor, kategoria i cena zakres. Aspekty jest wartością null na początkowej wyszukiwania, ale jak przyjmować wartości, wyniki wyszukiwania są obcinane elementów, które są zgodne z nie jest już. Należy zauważyć, że identyfikator URI przejmuje zmiany w kwerendzie.

    ![][9]
 
6.  Aby wyczyścić aspektowej kwerendę tak, aby spróbować zachowania innej kwerendy, kliknij pozycję **Wykaz AdventureWorks** w górnej części strony.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Następny krok

Aby przetestować swoją wiedzę, możesz dodać pole Faseta *modelName*. Indeks jest skonfigurowana dla tego Faseta, więc żadne zmiany do indeksu są wymagane. Ale należy zmodyfikować HTML, aby uwzględnić nowe Faseta modeli i Dodaj pole Faseta Konstruktora kwerend.

Aby uzyskać więcej wniosków na zasady projektowania dla nawigacji aspektowej zaleca się poniższe łącza:

- [Projektowanie na potrzeby wyszukiwania Aspektowej](http://www.uie.com/articles/faceted_search/)
- [Wzorców projektu: Nawigacji Aspektowej](http://alistapart.com/article/design-patterns-faceted-navigation)

Możesz też obejrzeć [Dive głębokości wyszukiwania Azure](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). Na 45:25 istnieje pokaz dotyczące implementowania aspekty.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 