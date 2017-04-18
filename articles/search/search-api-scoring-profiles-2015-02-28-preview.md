<properties
    pageTitle="Wyników profile (Azure wyszukiwania pozostałych wersji interfejsu API 2015-02-28-wersja Preview) | Microsoft Azure | Podgląd wyszukiwania Azure interfejsu API"
    description="Wyszukiwanie Azure jest usługi wyszukiwania w chmurze hostowanej obsługującego odbiór sklasyfikowane wyniki według zdefiniowanych przez użytkownika profile wyników."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Profile wyników (Azure wyszukiwania pozostałych wersji interfejsu API 2015-02-28-wersja Preview)

> [AZURE.NOTE] Ten artykuł zawiera opis wyników profile w [2015-02-28-Podgląd](search-api-2015-02-28-preview.md). Obecnie nie ma żadnej różnicy między `2015-02-28` wersją wymienioną w [witrynie MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) i `2015-02-28-Preview` wersji opisanych tutaj, ale mimo to oferowane tego dokumentu w celu udostępniania dokumentów pokrycia przez całego interfejsu API.

## <a name="overview"></a>Omówienie

Wyników odwołuje się do obliczenia wynik wyszukiwania dla każdego elementu w wynikach wyszukiwania. Wynik jest wskaźnik istotności elementu w kontekście bieżącego operacji wyszukiwania. Wyższa wynik, trafniejsze elementu. W wynikach wyszukiwania elementy są pozycja uporządkowane od najwyższej do niska, oparte na wynik wyszukiwania obliczane dla każdego elementu.

Azure wyszukiwania używa domyślnego wyników do obliczenia początkowej wyniku, ale można dostosować obliczeń przez profil wyników. Profile wyników zapewniają większą kontrolę nad klasyfikację elementów w wynikach wyszukiwania. Na przykład można zwiększyć elementów według ich potencjalny przychód, promowanie nowe elementy lub być może zwiększyć elementy, które zostały w magazynie zbyt długo.

Profil wyników jest częścią definicji indeksu składający się z pól, funkcje i parametry.

Aby zapewnić sobie ogólny obraz tego, jak wygląda wyników profilu, w poniższym przykładzie pokazano prosty profil o nazwie "geo". Tego typu zwiększa elementy, które mają wyszukiwany termin w `hotelName` pola. Ponadto użyto `distance` funkcja używanie opcji elementy, które należą do dziesięciu km od bieżącej lokalizacji. Jeśli ktoś wyszukuje terminów "numer identyfikacyjny" i "numer identyfikacyjny" się dzieje, jako część nazwy hotel, dokumenty, które obejmują hotele z "numer identyfikacyjny" pojawi się wyżej w wynikach wyszukiwania.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Aby użyć tego profilu wyników, kwerenda jest określona w sposób określ profil w ciągu kwerendy. W tej kwerendzie Zwróć uwagę, parametry kwerendy `scoringProfile=geo` w wezwaniu.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Ta kwerenda wyszukuje termin "numer identyfikacyjny" i przekazuje w bieżącej lokalizacji. Uwaga Ta kwerenda takich jak zawiera inne parametry `scoringParameter`. Parametry kwerendy są opisane w [Dokumentach wyszukiwania (Azure wyszukiwania API)](search-api-2015-02-28-preview.md#SearchDocs).

Kliknij pozycję [przykład](#example) , aby zapoznać się z bardziej szczegółowy przykład wyników profilu.

## <a name="what-is-default-scoring"></a>Co to jest wyników domyślne?

Wyników oblicza wynik wyszukiwania dla każdego elementu w zestawie wyników numerowana pozycja. Każdy element w zestawie wyników wyszukiwania jest przypisana wynik wyszukiwania, a następnie sklasyfikowane jako najwyższej do najniższej. Elementy z wyższymi wyniki są zwracane do aplikacji. Domyślnie są zwracane górny 50, ale możesz użyć `$top` parametr zwraca liczbę większych lub mniejszych elementów (maksymalnie 1000 pojedyncza odpowiedź).

Domyślnie wynik wyszukiwania jest obliczana w zależności od właściwości statystyczne danych i kwerendy. Funkcja wyszukiwania Azure znajduje dokumentów zawierających wyszukiwanych terminów w ciągu kwerendy (niektórych lub wszystkich, w zależności od `searchMode`), favoring w dokumentach zawierających wiele wystąpienia wyszukiwanego terminu. Wynik wyszukiwania rośnie nawet większy, jeśli termin jest rzadkich różnych Boże danych, ale typowych w dokumencie. Podstawa dla tej metody obliczeniowej istotności nosi nazwę TF IDF lub (terminów dokumentu odwrotność częstotliwość częstotliwość).

Zakładając, że jest niestandardowym sortowaniem, są następnie klasyfikowane wyniki według wyników wyszukiwania przed są zwracane do aplikacji wywołującej. Jeśli `$top` nie zostanie określony, 50 elementów o najwyższych wyszukiwania są zwracane wynik.

Wartości wynik wyszukiwania można powtórzyć w zestawie wyników. Na przykład może być 10 elementów z wynikiem 1.2 20 z wynikiem 1.0 oraz 20 elementami z wynikiem liczby 0,5. Jeśli wiele trafień sam wynik wyszukiwania, kolejnością te same elementy uzyskał nie zdefiniowano, a nie jest stabilny. Uruchom ponownie kwerendę, a może zostać wyświetlony elementy położenie shift. Mieć dwa elementy identyczne wynik, jest gwarantowana, która znajduje się na początku.

## <a name="when-to-use-custom-scoring"></a>Kiedy należy używać niestandardowych wyników

Domyślne zachowanie klasyfikowania nie Przejdź skrajnej za mało w spotkaniu celów biznesowych, należy utworzyć jeden lub więcej profilów wyników. Na przykład można zdecydować, czy istotności wyszukiwania ma preferować pełni nowo dodany elementów. Analogicznie może być polem zawierającym marża zysku lub innego pola, wskazująca, potencjalny przychód. Zwiększenia trafień, które przynoszą korzyści dla Twojej firmy może być istotny czynnik w podejmowaniu decyzji o użyciu profile wyników.

Oparte na trafności kolejnością również jest zaimplementowana za pośrednictwem wyników profile. Należy rozważyć, czy strony wyników wyszukiwania, używanych w przeszłości, które umożliwiają sortowanie według cena, daty, ocena lub istotności. W polu Wyszukaj Azure wyników profile sterują opcja "istotności". Definicji istotności jest kontrolowane przez Ciebie uzależniona od celów biznesowych i typ wyników wyszukiwania, którego mają zostać dostarczone.

<a name="example"></a>
## <a name="example"></a>Przykład

Jak wspomniano, niestandardowe wyników jest zaimplementowana za pośrednictwem wyników profile zdefiniowane w schemacie indeksu.

W tym przykładzie przedstawiono schemat indeksu o dwa profile wyników (`boostGenre`, `newAndHighlyRated`). Wszelkie zapytanie ten indeks, który zawiera albo profil parametru zapytania użyje profilu wynik zestaw wyników.

[Wypróbuj w tym przykładzie](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Przepływ pracy

Aby zastosować niestandardowy zachowanie wyników, Dodawanie profilu wyników w schemacie definiujące indeks. Możesz mieć profili wyników w indeksie, ale w czasie w dowolnej danej kwerendy można określić tylko jeden profil.

Rozpoczynanie [szablonu](#bkmk_template) opisane w tym temacie.

Podaj nazwę. Profile wyników są opcjonalne, ale jeśli dodasz jedną nazwę jest wymagane. Pamiętaj obserwować konwencje nazewnictwa dla pól (rozpoczyna się od litery pozwala uniknąć znaków specjalnych i słowa zastrzeżone). Aby uzyskać więcej informacji, zobacz temat [Nazewnictwa reguł](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Treści profilu wyników jest tworzona z pól ważonej i funkcje.

### <a name="weights"></a>Grubości ###

`weights` Właściwość profilu wyników określa par wartości z pola Nazwa, których przypisanie Względna waga do pola. W tym [przykładzie](#example)albumTitle, gatunek i artistName pola są zwiększane 1,5, 5 i 2, odpowiednio. Dlaczego jest gatunek zwiększane tak znacznie większą niż pozostałe? Jeśli wyszukiwanie odbywa się nad danymi, która jest nieco jednorodnych (tak jak w przypadku z "gatunek" w `musicstoreindex`), może być konieczne większe odchylenie w względne grubości. Na przykład w `musicstoreindex`, "skale" pojawia się jako obu określonego rodzaju i opisy gatunek identyczne ma inną pisownię. Gatunek, aby być większe niż opis gatunek, należy pole gatunek muszą dużo wyższa Względna waga.

### <a name="functions"></a>Funkcje ###

Funkcje są używane podczas dodatkowe obliczenia są wymagane do konkretnych kontekstów. Typy funkcji prawidłową `freshness`, `magnitude`, `distance` i `tag`. Każda funkcja ma parametry, które są unikatowe.

  - `freshness`Jeśli chcesz zwiększyć, należy użyć jak nowych i starych elementu jest. Tej funkcji można używać tylko z polami daty i godziny (`Edm.DataTimeOffset`). Uwaga `boostingDuration` atrybut jest używany tylko z funkcją aktualność.
  - `magnitude`powinien być używany, gdy chcesz zwiększyć według jak wysoki lub niski jest wartością liczbową. Scenariusze połączeń dla tej funkcji zawierać zwiększenia marża zysku, najwyższa cena, najniższa cena lub liczba pliki do pobrania. Możesz zmienić zakres najwyższej do niskiej, jeśli chcesz, aby arcus deseń (na przykład w celu elementy zwiększenie wydajności niższej cenie więcej niż wyższej cenie elementów). Nadana zakres cen z $100 $1, należy ustawić `boostingRangeStart` 100 i `boostingRangeEnd` na 1, aby zwiększyć niższej cenie elementów. Tej funkcji można używać tylko z polami Podwójna precyzja i liczba całkowita.
  - `distance`powinien być używany do chcesz zwiększyć odległość lub lokalizacji geograficznej. Tej funkcji można używać tylko z `Edm.GeographyPoint` pola.
  - `tag`powinien być używany do chcesz zwiększyć przez tagi wspólne między dokumentami i kwerend wyszukiwania. Tej funkcji można używać tylko z `Edm.String` i `Collection(Edm.String)` pola.
  
#### <a name="rules-for-using-functions"></a>Zasady używania funkcji ####

  - Typ funkcji (aktualność, wielkość, odległość, znacznik) musi być małe litery.
  - Funkcje nie mogą zawierać wartości null lub jest pusty. W szczególności jeśli zawiera nazwa_pola, musisz Ustaw coś.
  - Funkcje dotyczą tylko podatne pola. Aby uzyskać więcej informacji o polach podatne, zobacz [Tworzenie indeksu](search-api-2015-02-28-preview.md#CreateIndex) .
  - Funkcje dotyczą tylko pola zdefiniowane w kolekcji pól indeksu.

Po zdefiniowaniu indeksu, należy utworzyć indeks, przekazując schematu indeksu, a po nim dokumentów. Aby uzyskać instrukcje dotyczące tych operacji, zobacz [Tworzenie indeksu](search-api-2015-02-28-preview.md#CreateIndex) i [Dodaj lub aktualizuj dokumenty](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Po skonstruowaniu indeks powinien mieć profil wyników funkcjonalności współdziała z danych wyszukiwania.

<a name="bkmk_template"></a>
## <a name="template"></a>Szablon
W tej sekcji przedstawiono składnię i szablon wyników profile. Zapoznaj się z [Odwołanie atrybut indeksu](#bkmk_indexref) w następnej sekcji opisy atrybutów.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Wyników właściwości profilu

**Uwaga** Funkcja wyników dotyczą tylko pola, które są podatne.

| Właściwość | Opis |
|----------|-------------|
| `name`   | Wymagane. Jest to nazwa profilu wyników. Wynika z tej samej konwencji nazewnictwa pola. Musi rozpoczynać się od litery, nie może zawierać kropek, dwukropkami lub @ symbole, a nie może rozpoczynać się od frazy "azureSearch" (z uwzględnieniem wielkości liter). |
| `text` | Zawiera właściwość grubości. |
| `weights` | Opcjonalnie. Para wartości z pola Nazwa określająca nazwę pola i Względna waga. Względna waga musi być dodatniej liczby całkowitej lub liczba zmiennoprzecinkowa. Możesz określić nazwę pola bez odpowiedniej wagi. Grubości są używane do wskazywania znaczenie jedno pole względem innego. |
| `functions` | Opcjonalnie. Należy zauważyć, że funkcja wyników można stosować tylko do pól, które są podatne. |
| `type` | Wymagane dla wyników funkcje. Wskazuje typ funkcji należy użyć. Prawidłowe wartości to `magnitude`, `freshness`, `distance` i `tag`. Może zawierać więcej niż jedną funkcję w poszczególnych wyników profilu. Nazwa funkcji musi być małe litery. |
| `boost` | Wymagane dla wyników funkcje. Liczba dodatnia, używany jako współczynnika nieprzetworzonych wynik. Nie może być równa 1. |
| `fieldName` | Wymagane dla wyników funkcje. Funkcja wyników dotyczą tylko pola, które są częścią kolekcji pól indeksu i są podatne. Ponadto każdego typu funkcji wprowadzono dodatkowe ograniczenia (aktualność jest używany z polami daty/godziny, wielkość z liczby całkowitej lub podwójne pól, odległość z pola Lokalizacja i znacznika z polami zbioru ciąg lub ciąg). Można określić tylko jedno pole w każdej definicji funkcji. Na przykład aby użyć wielkość dwa razy w tym samym profilu, czy potrzebne dwa wielkości definicje, jedną dla każdego pola. |
| `interpolation` | Wymagane dla wyników funkcje. Określa nachylenie którego wynik zwiększenia podwyżki od początku zakresu na końcu zakresu. Prawidłowe wartości to `linear` (ustawienie domyślne), `constant`, `quadratic`, i `logarithmic`. Aby uzyskać szczegółowe informacje, zobacz [Ustawianie interpolations](#bkmk_interpolation) . |
| `magnitude` | Wielkość wyników funkcja służy do zmiany klasyfikacji na podstawie zakresu wartości dla pola liczbowego. Niektóre z najczęściej używanych przykłady użycia tych są:<ul><li>Oceny w formie gwiazdek: zmiany wyników na podstawie wartości w polu "Gwiazda klasyfikacji". Gdy dwa elementy są odpowiednie, elementu z wyższymi ocena pojawi się najpierw.</li><li>Margines: Gdy dotyczą dwóch dokumentów sprzedawcy detalicznego może chcesz zwiększyć dokumenty, które mają wyższą marginesy najpierw.</li><li>Kliknij pozycję liczników: za pomocą akcji do produktów lub strony, kliknij pozycję aplikacje, które śledzenie, można użyć wielkość do elementów zwiększenie wydajności, które zwykle uzyskać najbardziej ruch.</li><li>Pobieranie liczników: dla aplikacji śledzenia materiały do pobrania, umożliwia funkcja wielkość zwiększenie elementy, które mają najbardziej pliki do pobrania.</li></ul> |
| `magnitude:boostingRangeStart` | Ustawia wartość początkowa zakresu, w którym jest uzyskał wielkość. Wartość musi być liczba całkowita lub liczba zmiennoprzecinkowa. Gwiazdek od 1 do 4 będzie 1. Marginesy ponad 50% będzie 50. |
| `magnitude:boostingRangeEnd` | Ustawia wartość końcowa zakresu, w którym jest uzyskał wielkość. Wartość musi być liczba całkowita lub liczba zmiennoprzecinkowa. Gwiazdek od 1 do 4 będzie 4. |
| `magnitude:constantBoostBeyondRange` | Prawidłowe wartości to PRAWDA lub FAŁSZ (ustawienie domyślne). Po ustawieniu wartość true, pełny zwiększenie wydajności nadal będą stosowane do dokumentów, które mają wartości dla pola docelowego, która jest większa niż górną granicę zakresu. Jeśli ma wartość FAŁSZ, zwiększenie wydajności tej funkcji nie będą stosowane do dokumentów o wartości dla pola docelowego, która znajduje się poza zakresem. |
| `freshness` | Aktualność wyników funkcji służy do zmiany klasyfikacji wyników dla elementów na podstawie wartości w polach DateTimeOffset. Na przykład elementu z najnowszą datą można wyżej niż starszych elementów. (Należy pamiętać, że także możliwość pozycja elementów takich jak zdarzenia kalendarza z datami przyszłych tak, aby elementy bliżej bieżącej można wyżej niż elementy dodatkowo w przyszłości.) W bieżącej wersji usługi zostanie usunięty jeden koniec zakresu do bieżącej godziny. Drugi koniec raz w przeszłości na podstawie `boostingDuration`. Aby zwiększyć zakres niekiedy w przyszłości należy użyć wartości ujemne `boostingDuration`. Zwiększenia zmienia się z maksymalnie oraz minimalny zakres jest określona przez interpolację zastosowane do wyników profilu (patrz rysunek poniżej). Aby odwrócić współczynnik zwiększający zastosowane, wybierz pozycję współczynnik zwiększenie wydajności mniejsza niż 1. |
| `freshness:boostingDuration` | Ustawia okres wygasania, po którym rozpoczyna się zwiększanie będą dostępne dla określonego dokumentu. Zobacz [zestaw boostingDuration] [#bkmk_boostdur] w następnej sekcji składnię i przykłady. |
| `distance` | Zamknij wynik dokumenty oparte na temat odległość, o jaką funkcja wyników jest używana do wpływ lub o ile są one względem lokalizacji geograficznej odwołania. Lokalizacja odwołanie znajduje się w ramach kwerendy w parametrze (za pomocą `scoringParameter` kwerendy parametrycznej) jako długa, lat argument. |
| `distance:referencePointParameter` | Parametr w celu przekazania w kwerendach, aby użyć jako lokalizacji odwołania. scoringParameter jest parametru zapytania. Zobacz [Wyszukiwania dokumentów](search-api-2015-02-28-preview.md#SearchDocs) opisy parametry kwerendy. |
| `distance:boostingDistance` | Liczba określająca odległość w km z lokalizacji odwołania, gdzie się kończy zakres zwiększający. |
| `tag` | Znacznik wyników funkcji jest używany do wpływa na wynik dokumenty na podstawie znaczników w dokumentach i kwerend wyszukiwania. Dokumenty, które są wspólne kwerendy wyszukiwania będą zwiększane. Tagi kwerendy wyszukiwania ma postać wyników parametru w każdego żądania wyszukiwania (za pomocą `scoringParameter` kwerendy parametrycznej). |
| `tag:tagsParameter` | Parametr w celu przekazania w kwerendach, aby określić tagi dla poszczególnych żądań. `scoringParameter`jest parametru zapytania. Zobacz [Wyszukiwania dokumentów](search-api-2015-02-28-preview.md#SearchDocs) opisy parametry kwerendy. |
| `functionAggregation` | Opcjonalnie. Dotyczy tylko wtedy, gdy określony są funkcje. Prawidłowe wartości to: `sum` (ustawienie domyślne), `average`, `minimum`, `maximum`, i `firstMatching`. Wynik wyszukiwania jest pojedyncza wartość, która jest obliczana na podstawie wielu zmiennych, łącznie z wielu funkcji. Ten atrybuty wskazuje, jak zwiększenie wszystkich funkcji są łączone w jednym agregacji zwiększenie wydajności, następnie zastosowanego do wyniku dokument podstawowy. Wynik podstawowy jest oparty na wartość tf idf obliczana na podstawie dokumentu i kwerendy wyszukiwania. |
| `defaultScoringProfile` | Podczas wykonywania żądania wyszukiwania, jeśli jest określony żaden profil wyników, to domyślne wyników jest używanych (tf-idf tylko). Domyślna nazwa profilu wyników można ustawić tutaj powoduje Azure wyszukiwania używać tego profilu, gdy nie określonego profilu znajduje się w żądania wyszukiwania. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Ustawianie interpolations

Interpolations umożliwiają definiowanie nachylenie, dla którego wynik zwiększenia podwyżki od początku zakresu na końcu zakresu. Mogą być używane następujące interpolations:

  - `Linear`: Dla elementów, które są w zakresie min i max zwiększenie wydajności, stosowany do elementu, zostaną wykonane w kwocie stale malejących. Liniowy jest interpolacji domyślne punktowania profilu.
  - `Constant`: Dla elementów, które znajdują się w początkowej i końcowej zakresu stałej zwiększenie wydajności zostanie zastosowany do klasyfikacji wyników.
  - `Quadratic`: W porównaniu z interpolacji liniowej ma stale malejących zwiększenie wydajności kwadratowa zmniejszy się pierwotnie w tempie mniejszym, a następnie jak zbliża się zakres końcowy, zmniejszenie interwałem dużo wyższe. Ta opcja interpolacji nie jest dozwolona w tagu wyników funkcje.
  - `Logarithmic`: W porównaniu z interpolacji liniowej ma stale malejących zwiększenie wydajności Skala logarytmiczna zmniejszy się pierwotnie w tempie szybszym i następnie jak zbliża się zakres końcowy, zmniejszenie odstępach dużo mniejszy. Ta opcja interpolacji nie jest dozwolona w tagu wyników funkcje.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Ustawianie boostingDuration

`boostingDuration`jest atrybutem funkcji aktualność. Służy do Ustawianie wygaśnięcia okresu, po którym rozpoczyna się zwiększanie będą dostępne dla określonego dokumentu. Na przykład aby zwiększyć linii produktu lub marki 10 dni w okresie promocyjnym, należy określić 10-dniowego okresu jako "P10D" dla tych dokumentów. Lub aby zwiększyć Określ nadchodzących wydarzeniach w następnym tygodniu "-P7D".

`boostingDuration`musi być sformatowany jako wartość "dayTimeDuration" XSD (ograniczony podzbiór wartość czasu trwania ISO 8601). Deseń to: `[-]P[nD][T[nH][nM][nS]]`.

Poniższa tabela zawiera kilka przykładów.

| Czas trwania | boostingDuration |
|----------|------------------|
| 1 dzień | "P1D" |
| 2 dni i 12 godzin | "P2DT12H" |
| 15 minut | "PT15M" |
| 30 dni, 5 godzin, 10 minut, a 6.334 sekund | "P30DT5H10M6.334S" |

Aby uzyskać więcej przykładów, zobacz [schematu XML: typy danych (witryna sieci web adresem W3.org)](http://www.w3.org/TR/xmlschema11-2/).

**Zobacz też**
[interfejsu API usługi REST usługi Azure wyszukiwania](http://msdn.microsoft.com/library/azure/dn798935.aspx) w witrynie MSDN <br/>
[Tworzenie indeksu (wyszukiwanie Azure interfejsu API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) w witrynie MSDN<br/>
[Dodaj profil wyników do indeksu wyszukiwania](http://msdn.microsoft.com/library/azure/dn798928.aspx) w witrynie MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
