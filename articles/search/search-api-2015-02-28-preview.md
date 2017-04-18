<properties
   pageTitle="Azure usługi REST wersja API 2015-02-28-Podgląd wyszukiwania | Microsoft Azure | Podgląd wyszukiwania Azure interfejsu API"
   description="Azure usługi REST wersja API 2015-02-28-Podgląd wyszukiwania zawiera badawczych funkcje, takie jak programy do analizowania języku naturalnym i moreLikeThis wyszukiwania."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Interfejsu API usługi REST usługi Azure wyszukiwania: Wersja 2015-02-28-Preview

Ten artykuł dotyczy dokumentacji dotyczących `api-version=2015-02-28-Preview`. W tym podglądzie rozszerza bieżącej wersji ogólnodostępne, [wersja api = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), zapewniając następujące funkcje badawczych:

- `moreLikeThis`kwerendy parametr w [Dokumentach wyszukiwania](#SearchDocs) interfejsu API. Inne dokumenty, które dotyczą innego określonego dokumentu jest wyszukiwany.

Kilka dodatkowych części `2015-02-28-Preview` interfejsu API usługi REST opisano oddzielnie. W tym:

- [Profile wyników](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indeksatory](search-api-indexers-2015-02-28-preview.md)

Azure Usługa wyszukiwania jest dostępna w różnych wersjach. Zajrzyj do [Przechowywania wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) Aby uzyskać szczegółowe informacje.

## <a name="apis-in-this-document"></a>Interfejsy API w tym dokumencie

Interfejs API usługi Azure wyszukiwania obsługuje dwa składni adresu URL dla operacji interfejsu API: prosty i OData (zobacz [obsługę OData (API wyszukiwania Azure)](http://msdn.microsoft.com/library/azure/dn798932.aspx) , aby uzyskać szczegółowe informacje). Na poniższej liście przedstawiono składnię prosty.

[Tworzenie indeksu](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Aktualizowanie indeksu](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Uzyskiwanie indeksu](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Wyświetlanie listy indeksów](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Uzyskiwanie statystyki indeksu](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testowanie analizatora](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Usuwanie indeksu](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Dodawanie, usuwanie i aktualizowanie danych w indeksu](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Przeszukaj dokumenty](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Wyszukiwanie dokumentu](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Liczba dokumentów](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Sugestie](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Operacje indeksu

Można tworzyć i zarządzać indeksy w usłudze Azure wyszukiwania za pomocą prostego żądania HTTP (WPIS: GET, położenie, Usuń) przed danym indeksie zasobu. Aby utworzyć indeks, najpierw OPUBLIKOWAĆ dokument JSON opisujący schematu indeksu. Schemat definiuje pola indeksu, ich typów danych i opisami ich zastosowania (na przykład w pełnotekstowe przeszukiwanie, filtry, sortowania lub faceting). Definiuje również profile wyników, suggesters i inne atrybuty, aby skonfigurować zachowanie indeksu.

W poniższym przykładzie zestawiono zapoznać się z ilustracją schematu na potrzeby wyszukiwania na hotel informacji przy użyciu pola Opis zdefiniowane w dwóch językach. Zwróć uwagę, jak atrybuty kontrolowanie sposobu używania pola. Na przykład `hotelId` jest używany jako klucz dokumentu (`"key": true`) i nie jest uwzględniana pełnotekstowe przeszukiwanie (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Po utworzeniu indeksu zostanie przekazany dokumenty, które wypełniają indeks. Zobacz [Dodawanie lub aktualizowanie dokumentów](#AddOrUpdateDocuments) w tym następnego kroku.

Wideo wprowadzenie do indeksowanie wyszukiwania Azure zobacz [kanału 9 chmury obejmuje odcinka na Azure wyszukiwania](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Tworzenie indeksu

Indeks jest podstawowy sposób organizowanie i wyszukiwanie dokumentów w wyszukiwaniu Azure, podobnie jak tabeli są rozmieszczone rekordów w bazie danych. Każdy indeks ma kolekcję dokumentów, że wszystkie zgodny ze schematem indeks (nazwy pól, typy danych i właściwości), ale indeksy także określić dodatkowe konstrukcji (suggesters, profile wyników i opcje CORS) definiujące innych zachowań wyszukiwania.

Możesz utworzyć nowy indeks w ramach usługi Azure wyszukiwania przy użyciu HTTP wpisu lub Wyślij wezwanie. Treści wezwania jest schematem JSON, zawierający informacje indeks i konfiguracji.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Można również za pomocą funkcji i określ nazwę indeksu identyfikatora URI. Jeśli indeks nie istnieje, zostanie on utworzony.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Tworzenie indeksu określa strukturę dokumenty przechowywane i używana w operacji wyszukiwania. Wypełnianie indeks jest operacji. W tym kroku można użyć [indeksowania](https://msdn.microsoft.com/library/azure/mt183328.aspx) (dostępne dla obsługiwanych źródeł danych) lub operacji [Dodawanie, aktualizowanie lub usuwanie dokumentów](https://msdn.microsoft.com/library/azure/dn798930.aspx) . Indeks odwrócona jest generowany podczas są publikowane dokumenty.

**Uwaga**: Maksymalna liczba indeksów dozwolone zależy od ceny warstwy. Bezpłatna usługa umożliwia indeksy maksymalnie 3. Standardowa usługa umożliwia 50 indeksy dla danej usługi wyszukiwania. Aby uzyskać szczegółowe informacje, zobacz [limity i ograniczeń](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądania obsługi. Żądanie **Create Index** może być skonstruowane za pomocą metody POST lub położenie. Gdy używasz WPIS, musisz podać nazwę indeksu w treści wezwania wraz z definicja schematu indeksu. POŁOŻENIE Nazwa indeksu ma część adresu URL. Jeśli indeks nie istnieje, zostanie utworzony. Jeśli już istnieje, jest aktualizowana do nowej definicji.

Nazwa indeksu musi być małe litery, zaczynać literą lub numer, nie ukośników lub kropki i zawierać mniej niż 128 znaków. Po uruchomieniu nazwa indeksu literą lub numer pozostałą część nazwy mogą zawierać dowolną literę, liczbę i kreski, ile kresek nie są kolejne.

`api-version` Jest wymagane. Aby uzyskać listę dostępnych wersji, zobacz [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `Content-Type`: Wymagane. Można ustawić`application/json`
- `api-key`: Wymagane. `api-key` Jest używane do
- Uwierzytelnianie żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Create Index** musi zawierać `api-key` ustawione na wartość klucza administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwy usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

<a name="RequestData"></a>
**Żądanie składni treści**

Treść żądania zawiera definicja schematu, który zawiera listę pól w dokumentach, które zostaną wprowadzone do tego indeksu, typów danych, atrybuty, a także opcjonalna lista wyników profilów, które są używane do wyniku pasujących dokumentów w czasie kwerendy.

Należy zauważyć, że WPIS żądanie, musisz określić nazwę indeksu w treści wezwania.

Może istnieć tylko jednego pola klucza w indeksie. Musi być polem ciągu. W tym polu reprezentuje unikatowy identyfikator dla każdego dokumentu przechowywane w indeksie.

Główne składniki indeksu są następujące:

- `name`
- `fields`które będzie być wprowadzone do tego indeksu, w tym nazwę, typ danych i właściwości, które definiują dozwolone operacje na tym polu.
- `suggesters`używane do funkcji Autouzupełnianie lub dynamiczne kwerend.
- `scoringProfiles`używane do klasyfikowania wynik wyszukiwania niestandardowego. Aby uzyskać szczegółowe informacje, zobacz [Dodaj profile wyników](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` używane do określania, jak dokumenty/kwerendy są podzielone na tokeny można indeksować i wyszukiwania. Aby uzyskać szczegółowe informacje, zobacz [Analiza w wyszukiwaniu Azure](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`Umożliwia zastąpienie domyślnego zachowania wyników.
- `corsOptions`Aby umożliwić kwerend krzyżowych origin dla indeksu.

Składnia struktury ładunku żądanie wygląda następująco: Przykładowe żądanie znajduje się bardziej na w tym temacie.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Atrybuty indeksu**

Następujące atrybuty można ustawić podczas tworzenia indeksu. Aby uzyskać szczegółowe informacje dotyczące tworzenia wyników i wyników zobacz [Dodawanie profile wyników](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`— Umożliwia ustawienie Nazwa pola.

`type`— Umożliwia ustawienie typu danych dla pola. Aby uzyskać listę obsługiwanych typów, zobacz [Obsługiwane typy danych](#DataTypes) .

`searchable`-Oznacza pole jako pełnotekstowym możliwe wyszukiwania. Oznacza to, że ma zostać wykonana analizy, takich jak dzielenie wyrazów podczas indeksowania. Jeśli ustawisz `searchable` pola do wartości, takie jak "day słońce", wewnętrznie go będzie można podzielić na poszczególnych tokeny "słońce" i "dzień". Dzięki temu pełnotekstowym wyszukuje tych terminów. Pól typu `Edm.String` lub `Collection(Edm.String)` są `searchable` domyślnie. Pola innych typów nie może być `searchable`.

  - **Uwaga**: `searchable` pola używają dodatkowe miejsce w indeksie, ponieważ wyszukiwania Azure będzie przechowywana dodatkową wersję plikach wartość pola wyszukiwania pełnotekstowym. Jeśli chcesz oszczędzać miejsce w indeksie, a nie potrzebujesz pola mają zostać uwzględnione w wyszukiwaniu, ustaw `searchable` do `false`.

`filterable`— Pozwala pola odwoływać się do `$filter` kwerend. `filterable`różni się od `searchable` w sposób obsługi ciągów. Pól typu `Edm.String` lub `Collection(Edm.String)` , które są `filterable` nie ulegają dzielenia wyrazów, aby porównania są dokładnie pasuje do tylko. Na przykład, jeśli użytkownik ustawia pole na `f` "słońce dzień" `$filter=f eq 'sunny'` spowoduje znalezienie żadne dopasowania, ale `$filter=f eq 'sunny day'` będzie. Wszystkie pola są `filterable` domyślnie.

`sortable`-, Domyślnie system wyniki sortowane według wyników, ale w wielu środowiska użytkownicy będą sortowanie według pól w dokumentach. Pól typu `Collection(Edm.String)` nie może być `sortable`. Wszystkie pozostałe pola są `sortable` domyślnie.

`facetable`— Zazwyczaj używany w prezentacji wyników wyszukiwania, która zawiera liczbę kliknięć według kategorii (na przykład, wyszukiwanie aparaty cyfrowe i trafień Zobacz przez marki przez megapikseli, cena, itp.). Ta opcja jest niedostępna dla pól typu `Edm.GeographyPoint`. Wszystkie pozostałe pola są `facetable` domyślnie.

  - **Uwaga**: pól typu `Edm.String` , które są `filterable`, `sortable`, lub `facetable` może mieć maksymalnie 32 KB długości. Jest tak, ponieważ takie pola są traktowane jako pojedynczy wyszukiwany termin, a maksymalna długość okresu w wyszukiwaniu Azure wynosi 32KB. Jeśli chcesz przechowywać tekstu więcej niż to, w polu jeden ciąg znaków, należy ustawić `filterable`, `sortable`, i `facetable` do `false` w swojej definicji indeksu.

  - **Uwaga**: pola czy żadna z powyższych atrybuty ustawić `true` (`searchable`, `filterable`, `sortable`, lub`facetable`) pole jest skuteczne wyłączane z indeksu odwrócona. Ta opcja jest przydatna w przypadku pól, które nie są używane w kwerendach, ale są potrzebne w wynikach wyszukiwania. Wyłączenie takie pola z indeksem zwiększa wydajność.

`key`-Oznaczane jako zawierające unikatowych identyfikatorów dokumentów w indeksie. Wybrani dokładnie jedno pole `key` pola i musi być typu `Edm.String`. Pola klucza może służyć do wyszukiwania dokumentów bezpośrednio za pośrednictwem [Interfejsu API odnośnika](#LookupAPI).

`retrievable`-Określa, czy pole może zostać zwrócony w wynikach wyszukiwania.  Jest to przydatne, gdy chcesz używać pola (na przykład marża) jako filtr, sortowania lub wyników mechanizmu, ale nie mają pola, które mają być widoczne dla użytkownika końcowego. Ten atrybut musi być `true` dla `key` pola.

`analyzer`-Ustawia nazwę analizatora, aby użyć pola w czasie wyszukiwania i indeksowania czasu. Dozwolone zestawów wartości zobacz [programy do analizowania](https://msdn.microsoft.com/library/mt605304.aspx). Tej opcji można używać tylko z `searchable` pola i nie można ustawić razem z albo `searchAnalyzer` lub `indexAnalyzer`.  Po analizatora jest zaznaczona, nie można zmienić w polu.

`searchAnalyzer`-Ustawia nazwę analizatora używane podczas wyszukiwania dla pola. Dozwolone zestawów wartości zobacz [programy do analizowania](https://msdn.microsoft.com/library/mt605304.aspx). Tej opcji można używać tylko z `searchable` pola. Umieszcza się wraz z `indexAnalyzer` i nie można ustawić wraz z `analyzer` opcji. Ten analizatora mogą być aktualizowane na istniejące pole.

`indexAnalyzer`-Ustawia nazwę analizatora używane w czasie indeksowania dla pola. Dozwolone zestawów wartości zobacz [programy do analizowania](https://msdn.microsoft.com/library/mt605304.aspx). Tej opcji można używać tylko z `searchable` pola. Umieszcza się wraz z `searchAnalyzer` i nie można ustawić wraz z `analyzer` opcji. Po analizatora jest zaznaczona, nie można zmienić w polu.

`suggesters`— Umożliwia ustawienie Tryb wyszukiwania i pola, które są źródłem zawartości sugestie. Aby uzyskać szczegółowe informacje, zobacz [Suggesters](#Suggesters) .

`scoringProfiles`-Określa wyników zachowań niestandardowych umożliwiające wpłynąć na elementy, które pojawiają się wyżej w wynikach wyszukiwania. Profile wyników składają się z funkcji i grubości pola. Aby uzyskać więcej informacji dotyczących atrybutów używany w profilu wyników, zobacz [Dodawanie wyników profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Obsługa języków**

Pola, w których podlegają analizy że większość często obejmuje dzielenia wyrazów, normalizacji tekstu i filtrować terminy. Domyślnie pola wyszukiwania w wyszukiwaniu Azure są analizowane za pomocą [analizatora Apache Lucene standardowy](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) , który dzieli tekstu na elementy zgodnie z regułami["segmentacji tekst Unicode"](http://unicode.org/reports/tr29/) . Ponadto analizatora standardowy konwertuje wszystkie znaki do postaci małe litery. Zarówno indeksowanych dokumentów i wyszukiwane terminy przejść przez analizę podczas indeksowania i przetwarzania kwerend.

Wyszukiwanie Azure obsługuje wiele języków. Każdego języka, wymaga analizatora niestandardowych tekst, którego konta dla właściwości danego języka. Wyszukiwanie Azure udostępnia dwa typy programy do analizowania:

- programy do 35 analizowania przez Lucene.
- programy do 50 analizowania przez własnych języku naturalnym Microsoft przetwarzania technologii używane w pakiecie Office i usługi Bing.

Niektórzy deweloperzy wygodniej jest bardziej przyjaznej, prosta, Otwórz źródło rozwiązania Lucene. Programy do analizowania Lucene są szybciej, ale narzędzia analizy serwera Microsoft mają zaawansowane funkcje, takie jak Lematyzacja, program word decompounding (w języków, takich jak niemiecki, duński, holenderski, szwedzki, norweski, estoński, Zakończ, węgierskim, słowacki) i jednostki rozpoznawania (adresy URL, wiadomości e-mail, daty, numerów). Jeśli to możliwe należy uruchomić porównania programy firmy Microsoft i Lucene do analizowania zdecydować, który jest lepszym rozwiązaniem.

***Jak porównanie***

Analizatora Lucene dla języka angielskiego rozszerza analizatora standardowy. Go usuwa Zaimki dzierżawcze (końcowe firmy) z wyrazów, dotyczy wynikające zgodnie [wynikające Porter algorytmu](http://tartarus.org/~martin/PorterStemmer/)i usuwa, języka angielskiego [zatrzymać wyrazów](http://en.wikipedia.org/wiki/Stop_words).

W porównaniu z analizatora Microsoft wykonuje Lematyzacja zamiast wynikające. Oznacza to, że może obsługiwać formy wyrazu odmienione i o nieregularnym kształcie znacznie lepiej jakich wyników w wynikach wyszukiwania trafniejsze (czujki Moduł 7 prezentacji [MVA wyszukiwania Azure](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) uzyskać więcej szczegółowych informacji).

Indeksowanie przy użyciu narzędzia analizy serwera Microsoft jest średnio dwa do trzech razy mniejsza odpowiedniki Lucene w zależności od języka. Wydajność wyszukiwania nie znacznie narusza średni rozmiar kwerend.

***Konfiguracja***

Dla każdego pola w definicji indeksu można ustawić `analyzer` właściwość do analizatora nazwę, która określa, które języka i dostawcy. Tym samym analizatora zostaną zastosowane podczas indeksowania i wyszukiwania dla tego pola.
Na przykład może zawierać oddzielne pola angielski, francuski i hiszpański opisy hotel, znajdują się obok siebie w tym samym indeksem. Umożliwia określenie pola specyficzne dla języka do wyszukiwania przed w kwerendach [Parametry kwerendy "searchFields"](#SearchQueryParameters) . Możesz przejrzeć przykłady kwerend, które zawierają `analyzer` właściwości w [Dokumentach wyszukiwania](#SearchDocs). 

***Lista analizatora***

Poniżej przedstawiono listę obsługiwanych języków razem z nazw analizatora Lucene i firmy Microsoft.

<table style="font-size:12">
    <tr>
        <th>Język</th>
        <th>Nazwa analizatora firmy Microsoft</th>
        <th>Nazwa analizatora Lucene</th>
    </tr>
    <tr>
        <td>Arabski</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>Armeński</td>
        <td></td>
        <td>HY.lucene</td>
    </tr>
    <tr>
        <td>Bengalski</td>
        <td>BN.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskijski</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>Bułgarski</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>Kataloński</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>Chiński uproszczony</td>
        <td>zh Hans.microsoft</td>
        <td>zh Hans.lucene</td>     
    </tr>
    <tr>
        <td>Chiński tradycyjny</td>
        <td>zh Hant.microsoft</td>
        <td>zh Hant.lucene</td>     
    <tr>
    <tr>
        <td>Chorwacki</td>
        <td>hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Czeski</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>Duński</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>Holenderski</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>Angielski</td>        
        <td>EN.Microsoft</td>
        <td>EN.lucene</td>      
    </tr>
    <tr>
        <td>Estoński</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Fiński</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>Francuski</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>Galicyjski</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>Niemiecki</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>Grecki</td>
        <td>el.Microsoft</td>
        <td>el.lucene</td>      
    </tr>
    <tr>
        <td>Gudżarati</td>
        <td>gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebrajski</td>
        <td>HE.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>Węgierski</td>      
        <td>HU.Microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Islandzki</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonezyjski (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>Irlandzki</td>
        <td></td>
        <td>GA.lucene</td>
    </tr>
    <tr>
        <td>Włoski</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>Japoński</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>Ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreański</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Łotewski</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>Litewski</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajski (łaciński)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>MR.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norweski</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>Perski</td>
        <td></td>
        <td>fa.lucene</td>      
    </tr>
    <tr>
        <td>Polski</td>
        <td>pl.Microsoft</td>
        <td>pl.lucene</td>      
    </tr>
    <tr>
        <td>Portugalski (Brazylia)</td>
        <td>PT Br.microsoft</td>
        <td>PT Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugalski (Portugalia)</td>
        <td>PT Pt.microsoft</td>        
        <td>PT Pt.lucene</td>
    </tr>
    <tr>
        <td>Pendżabski</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumuński</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Rosyjski</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>Serbski (Cyrylica)</td>
        <td>SR cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbski (łaciński)</td>
        <td>SR latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Słowacki</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Słoweński</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hiszpański</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Szwedzki</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>Tamilski</td>
        <td>Ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Tajski</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turecki</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>Ukraiński</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Wietnamski</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Ponadto wyszukiwania Azure udostępnia konfiguracji analizatora niezależne od języka</td>
    <tr>
        <td>Składanie standardowy ASCII</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Segmentacji tekst Unicode (standardowy mechanizmu dzielenia na tokeny)</li>
            <li>Filtr składanych ASCII - konwertuje znaki Unicode, które nie należą do zestawu znaków ASCII najpierw 127 do odpowiedniki ASCII. Jest to przydatne do usunięcia znaków diakrytycznych.</li>
        </ul>
        </td>
    </tr>
</table>

Wszystkie programy do analizowania nazwami odnoszący z <i>lucene</i> są obsługiwane przez [programy do analizowania języka Apache Lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Więcej informacji na temat ASCII składania filtru można znaleźć [w tym miejscu](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` definiuje pola, które w indeksie są używane do obsługi funkcji Autouzupełnianie w wynikach wyszukiwania. Zazwyczaj ciągów wyszukiwania częściowego są wysyłane do [Interfejsu API sugestii](#Suggestions) podczas wyszukiwania użytkownik, a API zwraca zestaw sugerowane fraz. Suggester, podany w indeksie określa pola, które służą do tworzenia dynamiczne wyszukiwanych terminów. Aby uzyskać szczegółowe informacje o konfiguracji, zobacz [Suggesters](#Suggesters) .

**Profile wyników**

A `scoringProfile` definiuje wyników zachowań niestandardowych umożliwiające wpłynąć na elementy, które pojawiają się wyżej w wynikach wyszukiwania. Profile wyników składają się z funkcji i grubości pola. Aby używać ich, należy określić profil, który według nazwy w ciągu kwerendy.

Domyślnie wyników profilu działa w tle, aby obliczyć wynik wyszukiwania dla wszystkich elementów w zestawie wyników. Możesz użyć wewnętrznych, bez nazwy profilu wyników. Możesz też ustawić `defaultScoringProfile` do korzystania z profilu niestandardowego jako domyślny, wywoływane zawsze, gdy nie określono niestandardowego profilu w ciągu kwerendy.

Zobacz [Profile wyników Dodaj do indeksu wyszukiwania (Azure wyszukiwania usługi interfejsu API usługi REST)](search-api-scoring-profiles-2015-02-28-preview.md) , aby uzyskać szczegółowe informacje.

**Opcje CORS**

Javascript po stronie klienta nie można wywołać dowolnego interfejsy API domyślnie, ponieważ przeglądarce uniemożliwia wszystkich żądań origin krzyżowe. Włącz CORS (współużytkowanie zasobów między pochodzenia), ustawiając `corsOptions` atrybut, aby umożliwić kwerendy krzyżowe origin do indeksu. Uwaga tylko kwerendy pomocy technicznej interfejsy API CORS ze względów bezpieczeństwa. Dla CORS można ustawić następujące opcje:

- `allowedOrigins`(wymagany): jest to lista źródeł, które będą miały przyznany dostęp do indeksu. Oznacza to, że kodu Javascript obsługiwanych z tych miejsc pochodzenia będzie mógł kwerendy indeksu (przy założeniu, że zawiera poprawny klucz interfejsu API). Każdy pochodzi zwykle formularza `protocol://fully-qualified-domain-name:port` jednak często pominięcia portu. Zobacz [Ten artykuł](http://go.microsoft.com/fwlink/?LinkId=330822) , aby uzyskać więcej informacji.
 - Jeśli chcesz umożliwić dostęp do wszystkich miejsc pochodzenia, obejmują `*` jako jeden element w `allowedOrigins` tablicy. Należy zauważyć, że **nie jest to zalecane wskazówki dotyczące usług wyszukiwania produkcji.** Jednak może być przydatne w przypadku rozwoju lub debugowania.
- `maxAgeInSeconds`(opcjonalnie): przeglądarki pomocą tej wartości do określenia czasu trwania (w sekundach) odpowiedzi wstępnej CORS pamięci podręcznej. Musi to być nieujemna liczba całkowita. Większe ta wartość jest, będzie lepszą wydajność, ale dłużej będzie CORS zmiany zasad zostały wprowadzone. Jeśli nie zostanie ustawiona, zostanie użyty domyślny czas trwania 5 minut.

<a name="CreateUpdateIndexExample"></a>
**Przykład treści wezwania**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Odpowiedź**

Żądania pomyślnego: "utworzone 201".

Domyślnie treść odpowiedzi będzie zawierać JSON definiujące indeks, który został utworzony. Jeśli `Prefer` nagłówek jest ustawiona na `return=minimal`, treść odpowiedzi będzie pusta i będzie kodu stanu sukces "204 bez zawartości" zamiast "utworzone 201". Jest to możliwe, niezależnie od tego, czy położenie lub wpisu został użyty do utworzenia indeksu.

**Uwagi**

Obecnie są obsługiwane w ograniczonym aktualizacji schematu indeksu. Wszystkie aktualizacje schematu, które wymagają ponownego indeksowania, takie jak zmiana typy pól nie są obecnie obsługiwane. Mimo że istniejące pola nie można zmienić ani usunąć, nowe pola można dodawać do istniejącego indeksu w dowolnym momencie. Dodanie nowego pola wszystkie istniejące dokumenty w indeksie automatycznie uzyskuje wartość null dla tego pola. Brak odstępów między dodatkowego miejsca do magazynowania zostanie zużyte, aż nowe dokumenty są dodawane do indeksu.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Funkcja sugestie dotyczące wyszukiwania Azure jest możliwości kwerendy dynamiczne lub autouzupełniania, podając listę potencjalnych wyszukiwanych terminów w odpowiedzi na ciąg częściowej wartości wejściowych wprowadzone w polu wyszukiwania. Zauważą sugestii dotyczących kwerend podczas korzystania z wyszukiwarki komercyjnego sieci web: wpisywanie ".NET" w usłudze Bing tworzy listę postanowienia dotyczące ".NET 4,5", ".NET Framework 3,5" itd. Podczas korzystania z usługi wyszukiwania interfejsu API usługi REST, wykonania sugestie niestandardową aplikację wyszukiwania Azure należy spełnić następujące wymagania:

- Włącz sugestie, dodając budowie **suggester** w indeksie, podając nazwę, tryb wyszukiwania i Lista pól, dla których dynamiczne jest wywoływana. Na przykład po określeniu "Nazwa_miasta" jako pole źródła, wpisując ciąg wyszukiwania częściowego "Morzu" spowoduje "Radom", "Bocznej" i "Seatac" (wszystkich trzech są nazw miast rzeczywista) dostępny w górę jako sugestii dotyczących kwerend dla użytkownika.

- Wywołaj sugestie, dzwoniąc [Sugestie dotyczące interfejsu API](#Suggestions) w kodzie aplikacji. Zazwyczaj ciągów wyszukiwania częściowego są wysyłane do usługi podczas wyszukiwania użytkownik, a ten interfejs API zwraca zestaw sugerowane fraz.

W tym artykule wyjaśniono, jak skonfigurować **suggester**. Należy również sprawdzić [Interfejsu API sugestii](#Suggestions) szczegółowe informacje na temat sposobu używania suggester.

**Użycie**

`Suggesters`są tworzone w indeksie i pracy najlepiej, gdy są używane do sugerowania określonych dokumentów zamiast luźno terminy lub frazy. Najważniejsze pola candidate są tytuły, nazwy i innych stosunkowo zwroty identyfikujące elementu. Mniej efektywne powtarzające się pola, takie jak kategorie i znaczniki, lub są bardzo długa pól, takich jak pola opisy lub komentarze.

W ramach definicji indeksu, możesz dodać pojedynczy suggester do `suggesters` zbioru. Właściwości, które definiują suggester są następujące:

- `name`: Nazwa suggester. Użyj nazwy suggester podczas wywoływania `suggest` interfejsu API.
- `searchMode`: Strategia wykorzystane do wyszukania candidate fraz. Tryb tylko obecnie obsługiwane jest `analyzingInfixMatching`, który wykonuje elastyczne pasujących fraz na początku lub w środku zdania.
- `sourceFields`: Wykaz jedno lub więcej pól, które są źródła zawartości sugestii dotyczących. Tylko pola typu `Edm.String` i `Collection(Edm.String)` mogą być źródłem sugestii dotyczących. Można używać tylko pola, które nie mają analizatora języków niestandardowych, ustaw.

**Przykład suggester**

Suggester jest częścią indeksu. Może istnieć tylko jeden suggester `suggesters` zbioru w bieżącej wersji, razem z pakietem kolekcji pól i `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Jeśli użyto publicznej wersja zapoznawcza wyszukiwania Azure `suggesters` zastępuje starsze właściwość logiczna (`"suggestions": false`) obsługiwany którego sugestie prefiksu tylko krótkie ciągi znaków (3-25 znaków). Jej duplikat `suggesters`, obsługuje infix pasujących znajdujące pasujących terminów na początku lub na środku zawartość pola z lepszych uszkodzenia dla błędów w ciągach wyszukiwania. Począwszy od wersji ogólnodostępne, jest teraz tylko stosowania sugestie interfejsu API. Starszy `suggestions` właściwość, która została wprowadzona w `api-version=2014-07-31-Preview` będzie nadal działać w tej wersji, ale nie działa w `2015-02-28` lub nowszy Azure wyszukiwania.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Aktualizowanie indeksu

Możesz zaktualizować istniejący indeks w ciągu wyszukiwania Azure za pomocą żądanie HTTP umieszczenie. Aktualizacje mogą obejmować dodaje nowe pola do istniejącego schematu, zmodyfikowanie opcji CORS i modyfikowanie profilów wyników. Aby uzyskać więcej informacji, zobacz [Dodawanie wyników profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Określ nazwę indeksu, aby zaktualizować na identyfikator URI żądania:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Ważne:** Obsługa aktualizacji schematu indeks jest ograniczone do działania, które nie wymagają odbudowanie indeksu wyszukiwania. Wszystkie aktualizacje schematu, które wymagają ponownego indeksowania, takie jak zmiana typy pól nie są obecnie obsługiwane. Mimo że istniejące pola nie można zmienić ani usunąć można dodawać nowe pola w dowolnym momencie. To samo dotyczy `suggesters`. Można dodać nowe pola do suggester w czasie są dodawane polach, ale nie można usunąć pola z `suggesters` i istniejące pola nie można dodać do `suggesters`.

Podczas dodawania nowego pola do indeksu, wszystkie istniejące dokumenty w indeksie automatycznie uzyskuje wartość null dla tego pola. Brak odstępów między dodatkowego miejsca do magazynowania zostanie zużyte, aż nowe dokumenty są dodawane do indeksu.

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądania obsługi. Żądania **Aktualizacji indeksu** jest tworzona przy użyciu protokołu HTTP umieszczenie. POŁOŻENIE Nazwa indeksu ma część adresu URL. Jeśli indeks nie istnieje, zostanie utworzony. Jeśli indeks już istnieje, jest aktualizowana do nowej definicji.

Nazwa indeksu musi być małe litery, zaczynać literą lub numer, nie ukośników lub kropki i zawierać mniej niż 128 znaków. Po uruchomieniu nazwa indeksu literą lub numer pozostałą część nazwy mogą zawierać dowolną literę, liczbę i kreski, ile kresek nie są kolejne.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `Content-Type`: Wymagane. Można ustawić`application/json`
- `api-key`: Wymagane. `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądania **Aktualizacji indeksu** może zawierać `api-key` ustawione na wartość klucza administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Żądanie składni treści**

Podczas aktualizowania istniejący indeks, treści musi zawierać oryginalnej definicji schematu oraz nowe pola, które chcesz dodać, a także zmienione profile wyników, suggesters CORS opcji i, jeśli istnieją. W przypadku modyfikowania nie profilów wyników i opcji CORS, musi zawierać oryginalne od utworzenia indeksu. Ogólnie składnię najważniejsze aktualizacje można pobrać definicji indeksu z GET, zmodyfikuj ją, a następnie je zaktualizować z położenie.

Składnia schematu umożliwia tworzenie indeksu jest przedstawiony poniżej dla wygody. Aby uzyskać więcej informacji, zobacz [Tworzenie indeksu](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Odpowiedź**

Żądania pomyślnego: "204 bez zawartości".

Domyślnie treść odpowiedzi będzie pusta. Jednak jeśli `Prefer` nagłówek jest ustawiona na `return=representation`, treść odpowiedzi będzie zawierać JSON zaktualizowania definicji indeksu. W tym przypadku będzie sukcesu kodu stanu "200 OK".

**Aktualizowanie definicji indeksu z niestandardowego narzędzia analizy serwera**

Po zdefiniowano analizatora, mechanizmu dzielenia na tokeny, token lub znak filtru, nie można modyfikować. Nowe można dodać do istniejącego indeksu tylko wtedy, gdy `allowIndexDowntime` flaga jest ustawiona na Prawda w żądania aktualizacji indeksu: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Należy zauważyć, że operacja umieści indeksu w trybie offline żądania kwerendy kończy się niepowodzeniem i co najmniej kilku sekund powoduje usługi indeksowania. Wydajność i zapisu dostępność indeksu może być utrata kilka minut po zaktualizowaniu indeksu lub dłużej bardzo duże indeksy.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Indeksy listy

Operacja **Indeksy listy** obecnie zwraca listę indeksów w usłudze Azure wyszukiwania.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądania obsługi. Żądanie **Indeksy listy** może być skonstruowane za pomocą metody GET.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: Wymagane. `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Indeksy listy** muszą zawierać `api-key` Ustaw klucz administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Oto przykład treść odpowiedzi:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Należy zauważyć, że można filtrować odpowiedź w dół, aby tylko właściwości, które mogą Cię zainteresować. Na przykład tylko listę nazw, należy użyć OData `$select` kwerendy opcję:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

W tym przypadku odpowiedzi z powyższego przykładu będzie wyglądać następująco:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Jest to metoda przydatne, aby zapisać przepustowości, jeśli masz wiele indeksów w tej usługi wyszukiwania.

<a name="GetIndex"></a>
## <a name="get-index"></a>Uzyskiwanie indeksu

Operacji **Indeksowania uzyskiwanie** otrzymuje definicja indeksu wyszukiwania Azure.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądania **Uzyskania indeksu** może być skonstruowane za pomocą metody GET.

[Nazwa indeksu] w URI żądania określa indeks zwraca z kolekcji indeksy.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądania **Uzyskania indeksu** może zawierać `api-key` Ustaw klucz administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Zobacz przykład JSON, [Tworzenie i aktualizowanie indeksu](#CreateUpdateIndexExample) na przykład ładunku odpowiedź.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Usuwanie indeksu

Operacji **Usunięcia indeksowania** usuwa indeksu i skojarzone dokumenty z usługi Azure wyszukiwania. Nazwa indeksu można uzyskać na pulpicie nawigacyjnym usługi w portalu Azure lub z interfejsu API. Aby uzyskać szczegółowe informacje, zobacz [Indeksy listy](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądanie **Usuwanie indeksu** może być skonstruowane za pomocą metody DELETE.

[Nazwa indeksu] w URI żądania określa indeks usuwanie z kolekcji indeksy.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: Wymagane. `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla adresu URL usługi. Żądanie **Usuwanie indeksu** może zawierać `api-key` ustawione na wartość klucza administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 204 Brak zawartości jest zwracana na pomyślne odpowiedź.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Uzyskiwanie statystyki indeksu

Operacja **Uzyskiwanie statystyki indeks** zwraca z wyszukiwania Azure liczbę dokumentów do bieżącego indeksu oraz użycie miejsca do magazynowania.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statystyki dotyczące miejsca do magazynowania i liczba rozmiar dokumentu są zbierane co kilka minut, nie w czasie rzeczywistym. W związku z tym statystyki zwrócony przez ten interfejs API może nie odzwierciedlać, zmiany spowodowane ostatniej operacji indeksowania.

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądań. Żądanie **Uzyskiwanie statystyki indeksu** może być skonstruowane za pomocą metody GET.

[Nazwa indeksu] w URI żądania wskazuje usługę, aby wyznaczyć statystyki indeksu dla określonego indeksu.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.


**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Uzyskiwanie statystyki indeksu** może zawierać `api-key` Ustaw klucz administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Treść odpowiedzi znajduje się w następującym formacie:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Testowanie analizatora

**Analizowanie interfejsu API** pokazano, jak analizatora dzieli tekstu na tokeny.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądań. Żądanie **Analizowanie interfejs API** może być skonstruowane za pomocą metody POST.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.


**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Analizowanie interfejsu API** musi zawierać `api-key` Ustaw klucz administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę indeksu i nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

lub

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

`analyzer_name`, `tokenizer_name`, `token_filter_name` i `char_filter_name` , muszą być poprawne nazwy programy do analizowania wstępnie zdefiniowany lub niestandardowy, tokenizers, token filtrów oraz filtry znak indeksu. Aby dowiedzieć się więcej o procesie leksykalne analiza zobacz [Analiza w wyszukiwaniu Azure](https://aka.ms/azsanalysis).

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Treść odpowiedzi znajduje się w następującym formacie:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analizowanie przykład interfejsu API**

**Żądanie**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Odpowiedź**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Operacje dokumentu

W polu Wyszukaj Azure indeksu są przechowywane w chmurze i wypełnione przy użyciu dokumentów JSON, które można przekazać do usługi. Wszystkie dokumenty, które można przekazać obejmują Boże danych wyszukiwania. Dokumenty będą zawierać pola, niektóre z nich są plikach do wyszukiwanych terminów jak są one przekazane. `/docs` Część adresu URL w interfejsie API wyszukiwania Azure reprezentuje zbiór dokumentów w indeksie. Wszystkie operacje wykonywane na kolekcji, takich jak przekazywanie, wstawianie, usuwanie i badania sporządzanie dokumentów umieścić w kontekście indeks, dlatego adresy URL dla tych operacji zawsze zaczynają się od `/indexes/[index name]/docs` nazwy danego indeksu.

Kod aplikacji albo wygenerować JSON dokumentów do przekazania do wyszukiwania Azure lub [indeksatora](https://msdn.microsoft.com/library/dn946891.aspx) umożliwia ładowanie dokumentów, jeśli źródło danych jest baza danych SQL Azure lub DocumentDB. Zazwyczaj indeksy zostaną wypełnione z jednego zestawu danych dostarczone.

Należy zaplanować na konieczności jednego dokumentu dla każdej pozycji, którą chcesz przeszukać. Aplikację dzierżawa filmu może być jeden dokument na film, sklepu aplikacji mogą mieć jeden dokument na SKU, aplikacja kursów online może mieć jeden dokument na kursie, przedsiębiorstwo badań może mieć jednego dokumentu dla każdej akademicki papieru w ich repozytorium i tak dalej.

Dokumenty składa się z co najmniej jednego pola. Pola mogą zawierać tekst, który jest plikach przez funkcję wyszukiwania Azure do wyszukiwane terminy, a także innych niż plikach lub wartości nietekstowe, które mogą być używane w profilów wyników lub filtry. Nazwy, typy danych i funkcje wyszukiwania dla każdego pola obsługiwane są uzależnione od schematu indeksu. Jednego z pól schematu indeksu muszą być oznaczone jako identyfikator i każdy dokument musi mieć wartość w polu Identyfikator, który identyfikuje tego dokumentu w indeksie. Wszystkie pozostałe pola dokumentu są opcjonalne i będzie domyślnie ma wartość null, jeśli określony. Zauważ, że wartości null nie zajmują miejsca w indeksie wyszukiwania.

Przed można przekazać dokumenty, musisz zostały już utworzone indeksu w usłudze. Zobacz [Create Index](#CreateIndex) szczegółowe informacje na temat tego najpierw.

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Dodawanie, aktualizowanie lub usuwanie dokumentów

Możesz przekazać, korespondencji seryjnej, korespondencji seryjnej lub przekazywania lub Usuń dokumenty z określonego indeksu przy użyciu protokołu HTTP wpisu. W przypadku dużej liczby aktualizacji zaleca się tworzeniu partii dokumentów maksymalnie (1000 dokumenty na partię) lub około 16 MB na partię.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądania obsługi. Możesz przekazać, korespondencji seryjnej, korespondencji seryjnej lub przekazywania lub Usuń dokumenty z określonego indeksu przy użyciu protokołu HTTP wpisu.

Identyfikator URI żądania zawiera [Nazwa indeksu], określając indeks możesz publikować dokumenty. Tylko możesz publikować dokumenty, aby jeden indeks naraz.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `Content-Type`: Wymagane. Można ustawić`application/json`
- `api-key`: Wymagane. `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Dodawanie dokumentów** może zawierać `api-key` ustawione na wartość klucza administratora (zamiast klawisza kwerendy).

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Treść żądania zawiera co najmniej jeden dokumentów do indeksowania. Dokumenty są oznaczane Unikatowy klucz. Każdy dokument jest skojarzony z akcją: przekazywania, korespondencji seryjnej, mergeOrUpload lub Usuń. Przekaż żądania muszą zawierać dane dokumentu jako zbiór par klucz wartość.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Klucze dokument może zawierać tylko litery, cyfry, kreski ("-"), podkreślenia ("_") i znaku równości ("="). Aby uzyskać więcej informacji zobacz artykuł [reguły nazw](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Akcje dokumentu**

- `upload`: Akcja przekazywania jest podobna do "upsert" miejsce, w którym dokument zostanie wstawiony jeżeli jest nowe i zaktualizowane zastępowana Jeśli istnieje. Należy zauważyć, że wszystkie pola są zastępowane w przypadku aktualizacji.
- `merge`: Istniejący dokument korespondencji seryjnej aktualizuje określone pola. Jeśli dokument nie istnieje, korespondencji seryjnej nie powiedzie się. Wszystkie pola wybrane w podczas tworzenia korespondencji seryjnej zastąpią istniejące pole w dokumencie. Ta opcja uwzględnia pól typu `Collection(Edm.String)`. Na przykład, jeśli dokument zawiera pole "znaczników" z wartością `["budget"]` i wykonać podczas tworzenia korespondencji seryjnej z wartością `["economy", "pool"]` "tagów", będzie końcowej pola "znaczniki" `["economy", "pool"]`. Tak, to **nie** można `["budget", "economy", "pool"]`.
- `mergeOrUpload`: zachowuje się jak `merge` Jeśli dokumentu zawierającego dany klucz już istnieje w indeksie. Jeśli dokument nie istnieje w zachowuje się jak `upload` z nowym dokumentem.
- `delete`: Usuwanie usunie określonego dokumentu z indeksu. Uwaga wszystkie pola można określić w `delete` operacji niż pole klucza będą ignorowane. Aby usunąć pojedyncze pole z dokumentu, należy użyć `merge` zamiast tego i po prostu na wartość pola jawnie `null`.

**Odpowiedź**

Kod stanu 200 (OK) jest zwracana dla pomyślną odpowiedź, co oznacza, że wszystkie elementy zostały pomyślnie zindeksowane. To jest wskazywana przez `status` właściwość jest ustawiona na PRAWDA dla wszystkich elementów, a także jako `statusCode` ustawiona do 201 (w przypadku przekazany dokumenty) lub 200 (w przypadku dokumentów scalonych lub usunięte):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Po co najmniej jeden element nie został pomyślnie zindeksowane, zwracana jest kodu stanu 207 (wiele stanów). Elementy, które nie są indeksowane mają `status` pola jest ustawiona na wartość false. `errorMessage` i `statusCode` właściwości wskaże powód indeksowania komunikat o błędzie:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

W poniższej tabeli opisano różne kody stanu poszczególnych dokumentów, które mogą zostać zwrócone w odpowiedzi. Uwaga Niektóre występować problemy ze żądanie, podczas innych osób wskazują tymczasowe błędów. Te ostatnie którą należy ponowienia z opóźnieniem.

<table style="font-size:12">
    <tr>
        <th>Kod stanu</th>
        <th>Znaczenie</th>
        <th>Powtarzający operację</th>
        <th>Notatki</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument został pomyślnie zmodyfikowane lub usunięte.</td>
        <td>n/d!</td>
        <td>Operacje usuwania są <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Oznacza to, że nawet wtedy, gdy klucz dokumentu nie istnieje w indeksie, próby operacji usuwania z tego klawisza spowoduje kod stanu 200.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument został utworzony pomyślnie.</td>
        <td>n/d!</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Wystąpił błąd w dokumencie, który uniemożliwia operacji indeksowania.</td>
        <td>Brak</td>
        <td>Komunikat o błędzie w odpowiedź wskazują, na czym polega problem z dokumentem.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Nie można scalić dokument, ponieważ dany klucz nie istnieje w indeksie.</td>
        <td>Brak</td>
        <td>Ten błąd nie występuje przekazywania, ponieważ są one tworzyć nowe dokumenty, a nie pojawia się dla usuwa ponieważ są one <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Konflikt wersji wykryto podczas próby indeksowanie dokumentu.</td>
        <td>Tak</td>
        <td>Dzieje się tak, gdy próbujesz więcej niż jeden raz jednocześnie indeksu tego samego dokumentu.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Indeks jest tymczasowo niedostępny, ponieważ został zaktualizowany z ustawioną flagą "allowIndexDowntime", "true".</td>
        <td>Tak</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Usługi wyszukiwania jest tymczasowo niedostępny, prawdopodobnie ze względu na dużym obciążeniu.</td>
        <td>Tak</td>
        <td>Kod powinien czekać przed ponawianie próby w tym przypadku lub ryzyka, przedłużenie niedostępności usługi.</td>
    </tr>
</table> 

**Uwaga**: Jeśli kod klienta napotka często 207 odpowiedzi, jedna z możliwych przyczyn jest system obciążeniu. Można to potwierdzić, zaznaczając pole wyboru `statusCode` właściwość 503. Jeśli jest to możliwe, zalecamy ***ograniczania żądania indeksowania***. W przeciwnym razie jeśli indeksowania ruch nie subside systemu można uruchomić odrzucenie wszystkich żądania z błędami 503.

Kod stanu 429 wskazuje, że przekroczono przydział programu na Liczba dokumentów na indeks. Możesz utworzyć nowy indeks lub uaktualnić limitów wyższa wydajność.

**Przykład:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Przeszukaj dokumenty

Operacji **wyszukiwania** jest wydawany jako żądania GET lub POST i określa parametry przedstawiające kryteriów wyboru pasujących dokumentów.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kiedy używać wpisu zamiast GET**

Korzystając z HTTP GET połączenie **wyszukiwania** interfejsu API, należy pamiętać, że długość adresu URL żądania nie może przekraczać 8 KB. Jest to zazwyczaj wystarczająco dla większości aplikacji. Jednak niektóre aplikacje określenia bardzo duże kwerendy lub wyrażenia filtru OData. W przypadku tych aplikacji przy użyciu protokołu HTTP WPIS jest lepszym rozwiązaniem, ponieważ umożliwia większe filtrach i kwerendach niż GET. WPIS liczba warunków lub klauzul w kwerendzie ma współczynnik ograniczanie nie rozmiar nieprzetworzonych kwerendy, ponieważ limit rozmiaru żądania wpisu jest około 16 MB.

> [AZURE.NOTE] Mimo że limit rozmiaru żądania WPIS jest bardzo duża, kwerend wyszukiwania i wyrażenia filtru nie może być dowolnie złożonych. Aby uzyskać więcej informacji na temat ograniczeń złożoność kwerendy i filtrowanie wyszukiwania, zobacz [Lucene składnię](https://msdn.microsoft.com/library/mt589323.aspx) i [składni wyrażeń OData](https://msdn.microsoft.com/library/dn798921.aspx) .

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądania **wyszukiwania** mogą być wykonane przy użyciu metody GET lub POST.

Identyfikator URI żądania określa indeks do kwerendy dla wszystkich dokumentów, które są zgodne z parametrów. Parametry są określane w ciągu kwerendy w przypadku żądania GET, a w wezwaniu żądania treści w przypadku wpisu.

Zgodnie z zaleceniami dotyczącymi podczas tworzenia żądania GET Pamiętaj, aby [kodowanie adresu URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry kwerendy określonych podczas połączenia interfejsu API usługi REST bezpośrednio. W przypadku operacji **wyszukiwania** obejmuje:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

Kodowanie adresów URL jest zalecane tylko w powyższych parametrów kwerendy. Jeśli możesz przypadkowo kodowanie adresu URL ciągu kwerendy całego (wszystko po?), spowoduje zablokowanie żądania.

Ponadto kodowanie adresów URL jest konieczny tylko podczas nawiązywania połączeń z interfejsu API usługi REST bezpośrednio przy użyciu GET. Nie kodowanie adresów URL jest niezbędne podczas nawiązywania połączeń z **wyszukiwania** przy użyciu wpisu lub podczas korzystania z [biblioteki klienta .NET](https://msdn.microsoft.com/library/dn951165.aspx), która obsługuje kodowanie adresów URL dla Ciebie.

<a name="SearchQueryParameters"></a>
**Parametry kwerendy**

**Wyszukiwanie** akceptuje kilka parametrów, które zapewniają kryteriów kwerendy, a także określić sposób wyszukiwania. Pozwala udostępnić tych parametrów w adresie URL ciągu kwerendy, podczas połączenia **wyszukiwania** alerty, a także jako właściwości JSON w treści żądania podczas nawiązywania połączeń z **wyszukiwania** za pomocą wpisu. Składnia dla niektórych parametrów jest nieco różnić między GET i POST. Różnice te podano odpowiednio poniżej:

`search=[string]`(opcjonalnie) — tekst do wyszukania. Wszystkie `searchable` pola przeszukiwane są domyślnie, chyba że `searchFields` zostało określone. Podczas wyszukiwania `searchable` pola, sam tekst wyszukiwania jest plikach, więc wiele terminów mogą być oddzielone światło (na przykład: `search=hello world`). Aby uwzględnić wszystkie terminów, należy użyć `*` (może to być przydatne w przypadku kwerend filtru logiczna). Pominięcie ten parametr działa tak samo jak opcja `*`. Zobacz [Prosty składnią kwerendy](https://msdn.microsoft.com/library/dn798920.aspx) dla szczegóły dotyczące składni wyszukiwania.

  - **Uwaga**: wyniki może być czasem zaskakująco podczas badania nad `searchable` pola. Mechanizmu dzielenia na tokeny zawiera logiki do obsługi spraw wspólne dla tekstu w języku angielskim jak apostrofy, przecinki w liczby, itp. Na przykład `search=123,456` będą zgodne pojedynczy termin 123,456 zamiast poszczególnych warunków 123 i 456, ponieważ przecinki są używane jako separatory tysięcy dla dużej liczby w języku angielskim. Z tego powodu zalecamy, za pomocą pustego miejsca zamiast znaki interpunkcyjne, oddzielając terminów w `search` parametru.

`searchMode=any|all`(opcjonalnie, domyślnie `any`) — czy dowolne lub wszystkie kryteria wyszukiwania musi odpowiadać w celu zliczania dokumentu jako dopasowanie.

`searchFields=[string]`(opcjonalnie) — na liście nazwy pól rozdzielane przecinkami, aby wyszukać określony tekst. Pola docelowe musi być oznaczony jako `searchable`.

`queryType=simple|full`(opcjonalnie, domyślnie `simple`) — Jeśli ustawiono "proste" wyszukiwany tekst jest interpretowana przy użyciu języka prostej kwerendy, która pozwala na symbole, takie jak +, * i "". Kwerendy są obliczane przez wszystkie pola wyszukiwania (lub pola wskazanej w `searchFields`) w każdym dokumencie domyślnie. Jeśli ustawiono typ kwerendy `full` wyszukiwany tekst jest interpretowany użycia języka kwerend Lucene, dzięki czemu ważonej i specyficzne dla pola wyszukiwania. Zobacz [Prosty składnię](https://msdn.microsoft.com/library/dn798920.aspx) i [Lucene składnią kwerendy](https://msdn.microsoft.com/library/mt589323.aspx) dla szczegóły na składni wyszukiwania. 
 
> [AZURE.NOTE] Zakres wyszukiwania w Lucene języka kwerend nie jest obsługiwane na $filter która oferuje podobną funkcję.

`moreLikeThis=[key]`(opcjonalnie) **Ważne:** Ta funkcja jest dostępna tylko w `2015-02-28-Preview`. Ta opcja nie można używać w kwerendzie zawierającej dany parametr wyszukiwania tekstu `search=[string]`. `moreLikeThis` Parametru znajduje dokumentów, które są podobne do dokumentu określony przez klucz dokumentu. Gdy żądania wyszukiwania jest realizowana z `moreLikeThis`, Lista wyszukiwanych terminów jest generowany w oparciu o częstotliwości i rzadkość terminów w dokumencie źródłowym. Te zasady są następnie używane do wysłania żądania. Domyślnie zawartości wszystkich `searchable` są traktowane jako pola, chyba że `searchFields` służy do ograniczania pola, które są przeszukiwane.  

`$skip=#`(opcjonalnie) — liczba wyników wyszukiwania, aby pominąć; Nie może być większa niż 100 000. Jeśli trzeba skanowanie dokumentów w kolejności, ale nie można używać `$skip` ze względu na to ograniczenie, warto rozważyć użycie `$orderby` na kluczu całkowicie zamówiono i `$filter` z zakresu zamiast tego kwerendy.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `skip` zamiast `$skip`.

`$top=#`(opcjonalnie) — liczba wyników wyszukiwania do pobierania. To mogą zostać użyte w połączeniu z `$skip` do wykonania po stronie klienta stronicowanie wyników wyszukiwania.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `top` zamiast `$top`.

`$count=true|false`(opcjonalnie, domyślnie `false`) — określa, czy do pobierania całkowita liczba wyników. Jest to liczba wszystkich dokumentów, które są zgodne z `search` i `$filter` parametry, ignorując `$top` i `$skip`. Ustawienie wartości `true` mogą mieć wpływ na wydajność. Należy zauważyć, że liczba zwracanych jest przybliżenie.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `count` zamiast `$count`.

`$orderby=[string]`(opcjonalnie) — listy przecinkami wyrażeń, aby sortować wyniki według. Każdy wyrażenie może być nazwa pola lub numer telefonu, aby `geo.distance()` funkcji. Każdy wyrażenie może nastąpić `asc` umieszczone w kolejności rosnącej, a `desc` oznaczającą malejąco. Wartość domyślna to rosnąco. TIES będzie podziałem wyniki dopasowania dokumentów. Jeśli nie `$orderby` określono domyślnego porządku sortowania jest malejąco według wyników dopasowanie dokumentu. Istnieje ograniczenie 32 klauzul dla `$orderby`.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `orderby` zamiast `$orderby`.

`$select=[string]`(opcjonalnie) — lista rozdzielanych przecinkami pól do pobierania. Jeśli nie zostanie podany, wszystkie pola oznaczone jako w uwzględnianej podczas pobierania wyników w schemacie są uwzględniane. Możesz również jawnie zażądać wszystkie pola, ustawiając ten parametr na `*`.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `select` zamiast `$select`.

`facet=[string]`(zero lub więcej) — pole do Faseta przez. Opcjonalnie ciąg może zawierać parametry, aby dostosować faceting wyrażoną w postaci przecinkami `name:value` pary. Prawidłowe parametry są następujące:

- `count`(maksymalna liczba warunków Faseta; domyślny wynosi 10). Jest brak maksimum, ale wyższe wartości spowodować odpowiednich zmniejszenie wydajności, zwłaszcza jeśli pole aspektowej zawiera informacje o dużej liczby unikatowych terminów.
  - Na przykład: `facet=category,count:5` uzyskuje kategorie pięciu najlepszych wyników Faseta.  
  - **Uwaga**: Jeśli `count` parametr jest mniejsza niż liczba unikatowych terminów, wyniki mogą nie odpowiadać. To jest ze względu na sposób faceting kwerendy są rozmieszczane odłamki. Zwiększanie `count` zazwyczaj zwiększa dokładność liczby terminów, ale kosztem wydajności.
- `sort`(jeden z `count` sortowanie *Malejąco* według liczby, `-count` sortowanie *Rosnąco* według liczby, `value` sortowanie *Rosnąco* według wartości, lub `-value` sortowanie *Malejąco* według wartości)
  - Na przykład: `facet=category,count:3,sort:count` uzyskuje kategorii najwyższego trzy Faseta wyników w kolejności malejącej według liczbę dokumentów przy użyciu nazwy miasta. Na przykład górny trzech kategorii są budżet, Motel i luksusowe i budżetu jest liczba 5, Motel ma 6 i luksusowych ma 4, pakiety zostaną w kolejności Motel, budżet, luksusowych.
  - Na przykład: `facet=rating,sort:-value` tworzy przedziałów dla wszystkich możliwych ocen, w kolejności malejącej według wartości. Na przykład jeśli klasyfikacje są od 1 do 5, pakiety będzie zamówić 5, 4, 3, 2, 1, niezależnie od liczby dokumentów zgodne Każda ocena.
- `values`(rozdzielany znakami potoku liczbową lub `Edm.DateTimeOffset` wartości określających dynamiczne zestawu wartości wpisu Faseta)
  - Na przykład: `facet=baseRate,values:10|20` tworzy trzy pakiety: jedną dla stóp bazowych 0 do, ale nie włączając maksymalnie 10, jedną dla 10 z wyłączeniem 20 i jedną dla 20 lub nowszym.
  - Na przykład: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` tworzy dwa pakiety: jedną dla hotele renovated przed lutego 2010 i jedną dla hotele renovated luty 1st, 2010 lub nowszego.
- `interval`(liczba całkowita interwał większy niż 0 dla liczb, lub `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` dla wartości czasu daty)
  - Na przykład: `facet=baseRate,interval:100` tworzy przedziałów oparte na zakresach stóp bazowych o rozmiarze 100. Na przykład jeśli stawki podstawowe są wszystkie między 60 $ a $600, będzie przedziałów 0-100, 100-200, 200-300 300-400, 400-500 i 500-600.
  - Na przykład: `facet=lastRenovationDate,interval:year` tworzy jednym kolorem dla każdego roku, kiedy zostały renovated hotele.
- `timeoffset`([+-] hh: mm, [+-] hh: mm, lub [+-] hh) `timeoffset` jest opcjonalna. Można połączyć tylko z `interval` opcji i tylko wtedy, gdy zastosowany do pola typu `Edm.DateTimeOffset`. Wartość określa przesunięcie czasu UTC koncie ustalając granic godziny.
  - Na przykład: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` używa krawędzi dzień rozpoczynający u UTC 01:00:00 (północ w strefie czasowej docelowej)
- **Uwaga**: `count` i `sort` można połączyć w tym samym Specyfikacja Faseta, ale nie można połączyć z `interval` lub `values`, i `interval` i `values` nie można łączyć ze sobą.
- **Uwaga**: aspekty interwał na Data i godzina są obliczane według czasu UTC, jeśli `timeoffset` nie zostało określone. Na przykład: dla `facet=lastRenovationDate,interval:day`, dnia obramowanie zaczyna się od 00:00:00 UTC. 

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `facets` zamiast `facet`. Ponadto zostało ono określone jako tablica JSON ciągów, gdzie każdy ciąg jest wyrażenie osobnych Faseta.

`$filter=[string]`(opcjonalnie) — strukturalne wyrażenia w standardowej składni OData. Zobacz [Składni wyrażeń OData](#ODataExpressionSyntax) szczegółowe informacje na temat podzestaw gramatyki wyrażenie OData obsługuje Azure wyszukiwania.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `filter` zamiast `$filter`.

`highlight=[string]`(opcjonalnie) — wyróżnienie zestaw nazw pól przecinkami na potrzeby trafień. Tylko `searchable` pól może być używany do wyróżnienia trafień.

`highlightPreTag=[string]`(opcjonalnie, domyślnie `<em>`) — ciąg znacznika, który dołącza trafienie wyróżnienia. Należy ustawić z `highlightPostTag`.

> [AZURE.NOTE] Podczas nawiązywania połączeń z **wyszukiwania** przy użyciu GET, zastrzeżone znaków w adresie URL musi być procent zakodowany (na przykład % 23 zamiast #).

`highlightPostTag=[string]`(opcjonalnie, domyślnie `</em>`)-znacznik ciąg, który dołącza trafienie wyróżnienia. Należy ustawić z `highlightPreTag`.

> [AZURE.NOTE] Podczas nawiązywania połączeń z **wyszukiwania** przy użyciu GET, zastrzeżone znaków w adresie URL musi być procent zakodowany (na przykład % 23 zamiast #).

`scoringProfile=[string]`(opcjonalnie) — nazwa wyników profil, który ma być obliczona odpowiadać wyniki dla pasujących dokumentów w celu sortowania wyników.

`scoringParameter=[string]`(zero lub więcej) - wskazuje wartości dla każdego parametru zdefiniowanego w funkcji wyników (na przykład `referencePointParameter`) przy użyciu formatu `name-value1,value2,...`.

- Na przykład jeśli wyników profilu definiuje funkcję z parametrem o nazwie "mylocation" opcja ciągu kwerendy będzie `&scoringParameter=mylocation--122.2,44.8`. Pierwszy łącznika oddziela nazwę z listy wartości podczas drugiego łącznika jest częścią pierwszą wartość (długości geograficznej w tym przykładzie).
- Wyników parametrów takich jak tagu zwiększenia, która może zawierać przecinki, można escape takich wartości, na liście przy użyciu apostrofy. Jeśli same wartości zawierają apostrofy, możesz je escape podwajając.
  - Na przykład jeśli masz znacznika zwiększenia parametr o nazwie "mytag" i chcesz zwiększyć w tagu wartości "Witaj, O'Brien" i "Kit", kwerenda będzie opcję Parametry `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Zauważ, że oferty są tylko wymagane dla wartości, które zawierają przecinki.

> [AZURE.NOTE] Podczas połączenia **wyszukiwania** przy użyciu wpisu, ten parametr jest o nazwie `scoringParameters` zamiast `scoringParameter`. Także określić go jako tablicę JSON ciągów, gdzie każdy ciąg jest osobny `name-values` pary.

`minimumCoverage`(opcjonalnie, domyślnie 100)-liczbę z zakresu od 0 do 100 wskazujący procent indeks, który musi być objęta kwerendy wyszukiwania, aby zapytanie, które ma zostać zgłoszone jako sukcesu. Domyślnie cały indeks musi być dostępna lub `Search` zwróci kod stanu HTTP 503. Jeśli ustawisz `minimumCoverage` i `Search` zakończyło się powodzeniem, jego zwróci HTTP 200 i uwzględnić `@search.coverage` wartości w odpowiedzi wskazujący procent indeks, który został uwzględniony w kwerendzie.

> [AZURE.NOTE] Ustawienie ten parametr na wartość niższej niż 100 może być przydatne do zapewnienia dostępności wyszukiwania, nawet w przypadku usługi przy użyciu tylko jednej replice. Jednak nie wszystkie pasujących dokumentów zapewniające występować w wynikach wyszukiwania. Jeśli odwołanie wyszukiwania jest w aplikacji dostępności, a następnie najlepiej pozostawić `minimumCoverage` przy domyślnej wartości 100.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

Uwaga: dla tej operacji `api-version` jest określony jako parametru zapytania w adresie URL, niezależnie od tego, czy połączenie **wyszukiwania** z GET lub POST.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla adresu URL usługi. Żądanie **wyszukiwania** można określić klucza administrator albo klucz kwerendy dla `api-key`.

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Dla GET: Brak.

Dla wpisu:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Kontynuacji odpowiedzi wyszukiwania częściowego**

Czasami Azure wyszukiwania nie może zwrócić wszystkie wymagane wyniki pojedyncza odpowiedź wyszukiwania. Może się to zdarzyć różnych powodów, na przykład gdy kwerenda zażąda zbyt wiele dokumentów, nie określając `$top` lub podanie wartości `$top` jest zbyt duża. W takich przypadkach wyszukiwania Azure będzie zawierać `@odata.nextLink` adnotacji w treści odpowiedzi, a także `@search.nextPageParameters` Jeśli został on żądanie POST. Za pomocą wartości tych adnotacji do sformułowania kolejnego żądania wyszukiwania, aby przejść do następnej części odpowiedzi wyszukiwania. Jest to ***kontynuacji*** pierwotnego żądania wyszukiwania i adnotacje zazwyczaj są nazywane ***tokeny kontynuacji***. Zobacz [przykład poniżej](#SearchResponse) szczegółowe informacje o składni tych adnotacji i miejsce, w którym są wyświetlane w treści odpowiedzi. 

Powody, dla których Dlaczego Azure wyszukiwania może zwrócić tokeny kontynuacji są specyficzne dla implementacji i mogą ulec zmianie. Efektywne klientów powinna być zawsze gotowy do obsługi przypadkach, gdy są zwracane dokumenty mniej niż oczekiwane i tokenu kontynuacji jest dołączany, aby kontynuować pobieranie dokumentów. Należy również zauważyć, że tę samą metodę HTTP należy użyć jako oryginalne wezwanie aby kontynuować. Na przykład jeśli wysłano żądania GET żądań kontynuacji wysyłania należy również użyć GET (i podobnie dla wpisu).

<a name="SearchResponse"></a>
**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Przykłady:**

Dodatkowe przykłady można znaleźć na stronie [Składni wyrażeń OData Azure wyszukiwania](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Indeks wyszukiwania posortowane w kolejności malejącej według daty.


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = * & $orderby = lastRenovationDate desc & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "*", "orderby": "lastRenovationDate desc"}

2)  W polu wyszukiwania aspektowej w indeksie i pobieranie aspekty dla kategorii, klasyfikacji, znaczniki, a także elementy z baseRate w określonych zakresów:


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = test & aspekt = kategorii & aspekt = Klasyfikacja & aspekt = znaczników i aspekt = baseRate, wartości: 80 | 150 | 220 i wersja api = 2015-02-28-Preview

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-podglądu {"Wyszukaj": "test", "aspekty": ["kategoria", "ocena", "znaczniki", "baseRate, wartości: 80 | 150 | 220"]}

3)  Przy użyciu filtru, zawężenia poprzednich wyników kwerendy aspektowej, gdy użytkownik kliknie ocena 3 i kategorii "Motel":


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = test & Faseta = znaczników i Faseta = baseRate, wartości: 80 | 150 | 220 & $filter = Klasyfikacja eq 3 i kategorii eq "Motel" & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "test", "aspekty": ["znaczniki", "baseRate, wartości: 80 | 150 | 220"], "filtr": "ocena eq 3 i category eq"Motel""}

4) W wyszukiwaniu aspektowej należy ustawić maksymalnie na warunkach unikatowe zwrócone w kwerendzie. Wartość domyślna to 10, ale można zwiększyć lub zmniejszyć, za pomocą tej wartości `count` parametru `facet` atrybut:


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = test & aspekt = miasta, Zliczanie: 5 & wersja api = 2015-02-28-Preview

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "test", "aspekty": ["Miasto, Zliczanie: 5"]}

5)  Wyszukiwanie indeksu w określonych pól; Na przykład pole specyficzne dla języka:


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = hôtel & searchFields = description_fr & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "hôtel", "searchFields": "description_fr"}

6) Indeks wyszukiwania w wielu polach. Na przykład można przechowywać i kwerendzie pola wyszukiwania w wielu językach, w tym samym indeksem.  Opisy w języku angielskim i francuskim współistnienie w tym samym dokumencie, można z powrotem dowolne lub wszystkie w wynikach kwerendy:


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = hotel & searchFields = opis, description_fr & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "hotel", "searchFields": "opis, description_fr"}

Notatki można tylko kwerendę jeden indeks naraz. Nie należy tworzyć wiele indeksów dla każdego języka, chyba że planujesz kwerendy pojedynczo.

7)  Stronicowanie — uzyskiwanie 1st strony elementów (rozmiar strony jest 10):


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = * & $skip = 0 & $top = 10 & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "*", "Pomiń": 0 "górny": 10}

8)  Stronicowanie — uzyskiwanie 2 strony elementów (rozmiar strony jest 10):


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = * & $skip = 10 & $top = 10 & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "*", "Pomiń": 10 "górny": 10}

9)  Pobieranie określonego zestawu pól:


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = * & $select = hotelName, opis i wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "*", "Zaznacz": "hotelName, opis"}

10)  Pobieranie dokumentów pasujących wyrażenia określonego filtru:


    Uzyskiwanie /indexes/hotels/docs? $filter = (baseRate stronę 60 i baseRate lt 300) lub eq hotelName "Utrzymywania ozdobnych" & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"filtr": "(baseRate stronę 60 i baseRate lt 300) lub eq hotelName"Ozdobnych pobytu""}

11) Wyszukiwanie fragmentów indeks i zwrotu z trafień wyróżnienia


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania coś = & Wyróżnianie = opis & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "element", "Wyróżnianie": "opis"}

12) W indeksie i zwracają dokumenty posortowane od lokalizacji bliżej je w kierunku od odwołania


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania coś = & $orderby=geo.distance (lokalizacji, geography'POINT(-122.12315 47.88121) ") & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "element", "orderby": "geo.distance (lokalizacji, geography'POINT(-122.12315 47.88121)") "}

13) W indeksie przy założeniu jest wyników profilu o nazwie "geo" przy użyciu dwóch funkcji wyników odległości, definiując jeden parametr o nazwie "currentLocation", a drugi zdefiniować parametr o nazwie "lastLocation"


    GET /indexes/hotels/docs? wyszukiwania coś = & scoringProfile = geo & scoringParameter = currentLocation — 122.123,44.77233 & scoringParameter = lastLocation — 121.499,44.2113 & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "element", "scoringProfile": "geo", "scoringParameters": ["currentLocation — 122.123,44.77233", "lastLocation — 121.499,44.2113"]}

14) Znajdowanie dokumentów w indeksu przy użyciu [prostej kwerendy składni](https://msdn.microsoft.com/library/dn798920.aspx). Ta kwerenda zwraca hotele miejsce, w którym można wyszukiwać pola zawierają warunki "wygody" i "położenie", ale nie "motel":


    Uzyskiwanie /indexes/hotels/docs? wyszukiwania = wygody + lokalizacji-motel & searchMode = all & wersja api = 2015-02-28-Podgląd

    WPIS /indexes/hotels/docs/search? wersja api = 2015-02-28-Podgląd {"Wyszukaj": "wygody + lokalizacji-motel", "searchMode": "wszystkie"}

Zwróć uwagę na zastosowanie `searchMode=all` powyżej. W tym ten parametr zastępuje domyślne z `searchMode=any`, zapewnienie który `-motel` oznacza "AND NOT" zamiast "Lub nie". Bez `searchMode=all`, zostanie wyświetlony "Lub NOT" który rozszerza, a nie ogranicza liczbę wyników wyszukiwania i może to być counter-intuitive niektórych użytkowników.

15) Znajdowanie dokumentów w indeksie składnią [kwerendy lucene](https://msdn.microsoft.com/library/mt589323.aspx). Ta kwerenda zwraca hotele, których pole kategorii zawiera termin "budżet", a wszystkie pola wyszukiwania zawierające frazę "ostatnio renovated". Dokumenty zawierające frazę "ostatnio renovated" są wyżej w wyniku wartość zwiększenie wydajności terminów (3)

    Uzyskiwanie /indexes/hotels/docs?search = kategorii: budżet i \"ostatnio renovated\"^ 3 i searchMode = all & wersja api = 2015-02-28-podglądu & querytype = pełny

    WPIS /indexes/hotels/docs/search?api-version = 2015-02-28-Podgląd {"Wyszukaj": "kategoria: budżet i \"ostatnio renovated\"^ 3", "queryType": "Pełna", "searchMode": "wszystkie"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Wyszukiwanie dokumentu

Operacja **Wyszukiwania dokumentu** pobiera dokumentu z wyszukiwania Azure. Jest to przydatne, gdy użytkownik kliknie wynik wyszukiwania określonych i chcesz wyszukać szczegółowe informacje na temat tego dokumentu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądanie **Dokumentu wyszukiwania** można tworzona w następujący sposób.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternatywnie możesz użyć tradycyjnych składni OData do klucza wyszukiwania:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Identyfikator URI żądania zawiera [Nazwa indeksu] i [klucz], określając dokumentów do pobierania z indeks. Wystarczy tylko jeden dokument w danej chwili. Pobieranie wielu dokumentów w pojedyncze żądanie za pomocą **wyszukiwania** .

**Parametry kwerendy**

`$select=[string]`(opcjonalnie) — lista rozdzielanych przecinkami pól do pobierania. Jeśli nieokreślona lub ustawić `*`, wszystkie pola oznaczone jako w uwzględnianej podczas pobierania wyników w schemacie znajdują się w rzut.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

Uwaga: dla tej operacji `api-version` jest określony jako parametru zapytania.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla adresu URL usługi. Żądanie **Dokumentu wyszukiwania** można określić klucza administrator albo klucz kwerendy dla `api-key`.

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Przykład**

Wyszukiwanie dokumentu, który ma klucz "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Wyszukiwanie dokumentu, który ma klucz "3" przy użyciu składni OData:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Liczba dokumentów

Operacja **Liczba dokumentów** pobiera licznik dokumentów do indeksu wyszukiwania. `$count` Składni jest częścią protokołu OData.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądanie **Liczba dokumentów** może być skonstruowane za pomocą metody GET.

[Nazwa indeksu] w wezwaniu identyfikatora URI wskazuje usługę, aby zwrócona liczba wszystkich elementów w kolekcji dokumentów określonego indeksu.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych.

- `Accept`: Ta wartość musi być równa `text/plain`.
- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla adresu URL usługi. Żądanie **Liczba dokumentów** można określić klucza administrator albo klucz kwerendy dla `api-key`.

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Brak.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Treść odpowiedzi zawiera wartość count jako liczbę całkowitą sformatowane jako zwykły tekst.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Sugestie

Operacja **Sugestie** pobiera sugestii na podstawie danych wprowadzonych wyszukiwania częściowego. Zazwyczaj jest używane w pola wyszukiwania w celu zapewniają dynamiczne sugestie, jak użytkownicy wprowadzania wyszukiwanych terminów.

Żądania sugestii celem sugerowania dokumentów docelowej, więc sugerowane tekst może się powtarzać, jeśli w wielu dokumentach candidate są zgodne z tym samym wyszukiwania wprowadzania. Możesz użyć `$select` do pobierania innych pól dokumentu (w tym klucz dokumentu) tak, aby ustalić, który dokument jest źródłem dla każdej sugestii.

Operacja **Sugestie** jest wydawany jako żądania GET lub POST.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kiedy używać wpisu zamiast GET**

Korzystając z HTTP GET połączenie **Sugestie dotyczące** interfejsu API, należy pamiętać, że długość adresu URL żądania nie może przekraczać 8 KB. Jest to zazwyczaj wystarczająco dla większości aplikacji. Jednak niektóre aplikacje warzywa bardzo duże kwerendy, w szczególności OData wyrażenia filtru. W przypadku tych aplikacji przy użyciu protokołu HTTP WPIS jest lepszym rozwiązaniem, ponieważ umożliwia filtry większych niż GET. WPIS liczba klauzul w filtrze ma współczynnik ograniczanie nie rozmiar ciąg nieprzetworzonych filtru, ponieważ limit rozmiaru żądania wpisu jest około 16 MB.

> [AZURE.NOTE] Mimo że limit rozmiaru żądania WPIS jest bardzo duża, wyrażenia filtru nie może być dowolnie złożonych. Aby uzyskać więcej informacji na temat ograniczeń złożoność filtru, zobacz [składni wyrażeń OData](https://msdn.microsoft.com/library/dn798921.aspx) .

**Żądanie**

Protokół HTTPS jest wymagane dla żądania obsługi. Żądanie **Sugestie** może być skonstruowane za pomocą metod GET lub POST.

Identyfikator URI żądania Nazwa indeksu do kwerendy. Parametry, takie jak częściowo wprowadzania wyszukiwany termin, są określane w ciągu kwerendy w przypadku żądania GET, a w wezwaniu żądania treści w przypadku wpisu.

Zgodnie z zaleceniami dotyczącymi podczas tworzenia żądania GET Pamiętaj, aby [kodowanie adresu URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry kwerendy określonych podczas połączenia interfejsu API usługi REST bezpośrednio. **Sugestie dotyczące** operacji obejmuje:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

Kodowanie adresów URL jest zalecane tylko w powyższych parametrów kwerendy. Jeśli możesz przypadkowo kodowanie adresu URL ciągu kwerendy całego (wszystko po?), spowoduje zablokowanie żądania.

Ponadto kodowanie adresów URL jest konieczny tylko podczas nawiązywania połączeń z interfejsu API usługi REST bezpośrednio przy użyciu GET. Nie kodowanie adresów URL jest niezbędne podczas nawiązywania połączeń z **sugestii** , używając wpisu lub podczas korzystania z [biblioteki klienta .NET](https://msdn.microsoft.com/library/dn951165.aspx), która obsługuje kodowanie adresów URL dla Ciebie.

**Parametry kwerendy**

**Sugestie dotyczące** akceptuje kilka parametrów, które zapewniają kryteriów kwerendy, a także określić sposób wyszukiwania. Pozwala udostępnić tych parametrów w adresie URL podczas połączenia **Sugestie** alerty, a także jako właściwości JSON w treści żądania podczas nawiązywania połączeń z **sugestii** za pośrednictwem WPIS w ciągu kwerendy. Składnia dla niektórych parametrów jest nieco różnić między GET i POST. Różnice te podano odpowiednio poniżej:

`search=[string]`— Wyszukiwanie tekstu za pomocą proponowanie kwerendy. Musi zawierać co najmniej 1 znak i nie więcej niż 100 znaków.

`highlightPreTag=[string]`(opcjonalnie) — ciąg znakowanie który dołącza do wyszukiwania trafień. Należy ustawić z `highlightPostTag`.

> [AZURE.NOTE] Podczas nawiązywania połączeń z **sugestii** , używając GET, zastrzeżone znaków w adresie URL muszą być procent kodowane (na przykład % 23 zamiast #).

`highlightPostTag=[string]`(opcjonalnie) — ciąg oznaczać który dołączy do wyszukiwania trafień. Należy ustawić z `highlightPreTag`.

> [AZURE.NOTE] Podczas nawiązywania połączeń z **sugestii** , używając GET, zastrzeżone znaków w adresie URL muszą być procent kodowane (na przykład % 23 zamiast #).

`suggesterName=[string]`-nazwę suggester zgodnie z `suggesters` zbioru, który jest częścią definicji indeksu. A `suggester` określa pola, które są zeskanowany dla zapytania sugerowane terminy. Aby uzyskać szczegółowe informacje, zobacz [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(opcjonalny, ustawienie = false)-ustawiona na PRAWDA ten interfejs API spowoduje znalezienie sugestie nawet wtedy, jeśli istnieje zastępująca lub Brak znaku w polu wyszukiwania tekst. Gdy to zapewnia lepsze możliwości w niektórych scenariuszach znajduje się na wydajność, koszt rozmyty sugestie wyszukiwania wolniej i używanie większej ilości zasobów.

`searchFields=[string]`(opcjonalnie) — na liście nazwy pól rozdzielane przecinkami do wyszukiwania określonej wyszukiwany tekst. Pola docelowe musi być włączony sugestii dotyczących.

`$top=#`(opcjonalny, ustawienie = 5) — liczba sugestii do pobrania. Musi być liczbą z przedziału od 1 do 100.

> [AZURE.NOTE] Podczas połączenia **Sugestie** , używając wpisu, ten parametr jest o nazwie `top` zamiast `$top`.

`$filter=[string]`(opcjonalnie) — wyrażenie, które filtruje dokumenty rozważyć sugestie.

> [AZURE.NOTE] Podczas połączenia **Sugestie** , używając wpisu, ten parametr jest o nazwie `filter` zamiast `$filter`.

`$orderby=[string]`(opcjonalnie) — listy przecinkami wyrażeń, aby sortować wyniki według. Każdy wyrażenie może być nazwa pola lub numer telefonu, aby `geo.distance()` funkcji. Każdy wyrażenie może nastąpić `asc` umieszczone w kolejności rosnącej, a `desc` oznaczającą malejąco. Wartość domyślna to rosnąco. Istnieje ograniczenie 32 klauzul dla `$orderby`.

> [AZURE.NOTE] Podczas połączenia **Sugestie** , używając wpisu, ten parametr jest o nazwie `orderby` zamiast `$orderby`.

`$select=[string]`(opcjonalnie) — lista rozdzielanych przecinkami pól do pobierania. Jeśli nie zostanie podany, zwracany jest tylko klucz i sugestii dotyczących tekstem dokumentu. Wszystkie pola mogą jawnie zażądać, ustawiając ten parametr na `*`.

> [AZURE.NOTE] Podczas połączenia **Sugestie** , używając wpisu, ten parametr jest o nazwie `select` zamiast `$select`.

`minimumCoverage`(opcjonalnie, domyślnie 80)-liczbę z zakresu od 0 do 100 wskazujący procent indeks, który musi być objęta kwerendy sugestie, aby zapytanie, które ma zostać zgłoszone jako sukcesu. Domyślnie, musi być dostępny co najmniej 80% indeks lub `Suggest` zwróci kod stanu HTTP 503. Jeśli ustawisz `minimumCoverage` i `Suggest` zakończyło się powodzeniem, jego zwróci HTTP 200 i uwzględnić `@search.coverage` wartości w odpowiedzi wskazujący procent indeks, który został uwzględniony w kwerendzie.

> [AZURE.NOTE] Ustawienie ten parametr na wartość niższej niż 100 może być przydatne do zapewnienia dostępności wyszukiwania, nawet w przypadku usługi przy użyciu tylko jednej replice. Jednak nie wszystkie pasujące sugestie zapewniające występować w wynikach. Jeśli odwołanie jest do aplikacji dostępności, a następnie najlepiej nie chcesz obniżyć poziom `minimumCoverage` poniżej wartość domyślną 80.

`api-version=[string]`(wymagany). Wersja jest `api-version=2015-02-28-Preview`. [Przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) podano szczegółowe informacje oraz alternatywne wersje.

Uwaga: dla tej operacji `api-version` jest określony jako parametru zapytania w adresie URL, niezależnie od tego, czy połączenie **sugestii** z GET lub POST.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych

- `api-key`: `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla adresu URL usługi. Żądanie **Sugestie** można określić albo administrator kwerendy klawisz lub jako `api-key`.

Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwa usługi i `api-key` z pulpitu nawigacyjnego usług w portalu Azure. Zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

**Treść żądania**

Dla GET: Brak.

Dla wpisu:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Jeśli opcja rzut jest używana do pobierania pól są uwzględniane w każdy element tablicy:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Przykład**

Pobieranie sugestii 5, gdzie wprowadzania wyszukiwania częściowego jest "luksów"

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
