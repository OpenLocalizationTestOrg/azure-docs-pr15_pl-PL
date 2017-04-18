<properties 
    pageTitle="Składnia SQL i języka SQL kwerendy DocumentDB | Microsoft Azure" 
    description="Informacje o składni języka SQL, pojęcia bazy danych i zapytania SQL dla DocumentDB, NoSQL bazy danych. SQL może być używany jako JSON języka kwerend w DocumentDB." 
    keywords="Składnia SQL, zapytania sql, zapytania sql, języka kwerend json, pojęcia bazy danych i zapytania sql, funkcje agregacji"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>Kwerenda SQL i składni języka SQL w DocumentDB
Microsoft Azure DocumentDB dokumentów podczas badania przy użyciu SQL (Structured Query Language) jako języka kwerend JSON. DocumentDB jest rzeczywiście wolny schematu. Ze względu na zaangażowanie do modelu danych JSON bezpośrednio z poziomu aparatu bazy danych umożliwia automatyczne indeksowanie JSON dokumenty bez konieczności schematów jawne lub tworzenie indeksów pomocniczych. 

Podczas projektowania języka kwerend dla DocumentDB było dwa cele pamiętać:

-   Zamiast inventing nowego języka kwerend JSON, trzeba obsługuje SQL. SQL jest jednym z języków kwerendy znanych i najbardziej popularne. DocumentDB SQL zapewnia model programowania formalne zaawansowanych kwerend nad dokumentami JSON.
-   Jako JSON dokumentu bazy danych obsługuje wykonywanie kodu JavaScript bezpośrednio w aparat bazy danych trzeba używać modelu programowania kodu JavaScript w jako podstawy dla naszych języka kwerend. DocumentDB SQL zostaje umieszczone w systemie typ i JavaScript, wyrażenia i wywołania funkcji. Ten w Włącz przewiduje model programowania naturalne relacyjnych prognozy, hierarchii nawigacji przez dokumentów JSON, samodzielnie sprzężenia przestrzenna kwerend i wywołania funkcje zdefiniowane przez użytkownika (UDF), całkowicie napisana JavaScript, między innymi funkcjami. 

Firma Microsoft podejrzenie, że te funkcje są klucz do zmniejszania tarcia między aplikacją i bazy danych i mają kluczowe znaczenie dla deweloperów produktywność.

Zaleca się wprowadzenie obserwując poniższym klipie wideo, w przypadku gdy Aravind Ramachandran przedstawiono możliwości podczas badania i DocumentDB, i, odwiedź witrynę serwisie [Playground kwerendy](http://www.documentdb.com/sql/demo), gdzie możesz wypróbować DocumentDB i uruchom na naszych zestawu danych SQL kwerendy.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Następnie wróć do niniejszego artykułu, gdzie Zaczniemy od samouczek zapytania SQL, który przeprowadzi Cię przez kilka prostych dokumentów JSON i poleceń SQL.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Wprowadzenie do poleceń SQL w DocumentDB
Aby wyświetlić DocumentDB SQL w miejscu pracy, Przejdźmy zaczyna kilka prostych dokumentów JSON i szczegółową kilka prostych kwerend dla go. Należy rozważyć, czy te dwa dokumenty JSON o dwie grupy. Należy zauważyć, że z DocumentDB, firma Microsoft nie jawnie tworzenie schematów ani wskaźników pomocniczą. Po prostu należy wstawić dokumentów JSON do kolekcji DocumentDB, a następnie kwerendy. W tym miejscu jest proste JSON informacji o dokumencie z rodziny Andersen, elementów nadrzędnych, dzieci (i ich domowych), adres i rejestracji. Dokument zawiera ciągi, liczby, wartości logiczne, tablicach i zagnieżdżonych właściwości. 

**Dokument**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Oto drugiego dokumentu z jednego różnica — `givenName` i `familyName` są używane zamiast `firstName` i `lastName`.

**Dokument**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Teraz spróbuj kilka kwerend dla tych danych, aby poznać niektóre z najważniejszych aspektów DocumentDB SQL. Na przykład poniższa kwerenda zwróci dokumenty jeśli odpowiada pole Identyfikator `AndersenFamily`. Ponieważ jest to `SELECT *`, wyniki kwerendy jest cały dokument JSON:

**Kwerendy**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Teraz warto rozważyć wielkości liter, w którym trzeba sformatować dane wyjściowe JSON w inny kształt. Ta kwerenda projektów nowy obiekt JSON z dwóch zaznaczonych pól Nazwa i Miasto, gdy ten adres, Miasto ma taką samą nazwę jak stan. W tym przypadku zastępuje "NY, NY".

**Kwerendy**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Wyniki**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Następny kwerenda zwraca wszystkie imiona dzieci w obrębie rodziny odpowiada o identyfikatorze `WakefieldFamily` uporządkowanych według miasta zamieszkania.

**Kwerendy**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Wyniki**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Chcemy skierować uwagę pod kilkoma względami warto zauważyć języka kwerend DocumentDB przykładów, które udostępniamy zobaczył pory:  
 
-   Ponieważ DocumentDB SQL pracuje nad wartości JSON, dotyczy drzewa kształcie podmioty zamiast wierszy i kolumn. Dlatego języka umożliwia dotyczą węzłów drzewa na dowolnym poziomie dowolnego, takich jak `Node1.Node2.Node3…..Nodem`, tak jak relacyjne SQL odwołujące się do dwóch części odwołanie `<table>.<column>`.   
-   Język structured query language współdziała z schematu mniej danych. W związku z tym system typów należy powiązać dynamiczne. Tym samym wyrażeniu wygeneruje różnych typów na różne dokumenty. Wynik kwerendy jest prawidłową wartością JSON, ale nie jest gwarantowana być stała schematu.  
-   DocumentDB jedynie dokumentów ściśle JSON. Oznacza to, że system typów i wyrażenia są ograniczone do zająć się tylko z typami JSON. Zobacz [Specyfikacja JSON](http://www.json.org/) , aby uzyskać więcej informacji.  
-   Kolekcja DocumentDB jest wolne schematu kontenera JSON dokumentów. Relacje w jednostki danych w ramach i między dokumentów w zbiorze są niejawnie przechwytywane według zawartości, a nie według klucza podstawowego i obcego relacji klucza. To jest ważnym aspektem warte wskazujące świetle sprzężenia wewnątrz dokumentu, omówiono w dalszej części tego artykułu.

## <a name="documentdb-indexing"></a>Indeksowanie DocumentDB

Przed możemy przejść do składnia DocumentDB SQL, warto poznawanie indeksowania projektu w DocumentDB. 

Przeznaczenie indeksy bazy danych ma służyć kwerend w ich różnych formularzy i kształty z zużycie zasobów minimalne (na przykład Procesora i wejścia i wyjścia) zapewniając dobre przepustowości i małego opóźnienia. Często wybór indeksu prawa do wykonywania kwerend bazy danych wymaga dużo planowania i eksperyment. Ta metoda stanowi wyzwaniem dla baz danych bez schematu miejsce, w którym dane nie są zgodne z ściśle schematu i szybko rozwoju. 

W związku z tym gdy możemy zaprojektowane DocumentDB indeksowania podsystemu, możemy ustawić następujące cele:

-   Indeksowanie dokumenty bez konieczności schematu: podsystemu indeksowania nie wymaga wszelkie informacje o schemacie ani nie przyjmuje żadnych założeń dotyczących schematu dokumentów. 

-   Obsługa wydajną i zaawansowanych kwerend hierarchiczny i relacyjne: indeks obsługuje języka kwerend DocumentDB wydajność, tym obsługę hierarchiczny i relacyjne prognozy.

-   Obsługa spójne kwerendy in face of stałej liczby zapisów: dla zapisu Wysoka przepustowość obciążeń pracą z kwerendami spójne, indeks jest aktualizowana stopniowo wydajność i online obliczu stałej liczby zapisu. Aktualizacja indeksu spójne jest ważnych do obsługi kwerend na poziomie spójności, w którym użytkownik skonfigurowany usługi zarządzania dokumentami.

-   Obsługa wielu dzierżawy: podanej modelu podstawie rezerwacji zasobów zarządzania między dzierżawami, indeks aktualizacje są wykonywane w ramach budżetu zasoby systemowe (Procesora, pamięci i operacji wejścia i wyjścia na sekundę) wg replice. 

-   Wydajność miejsca do magazynowania: skuteczność kosztów ogólnych na ilość miejsca do magazynowania indeksu jest ograniczona i przewidywalne. Jest to kluczowe znaczenie, ponieważ DocumentDB zezwala projektanta wprowadzić koszt podstawie kompromisów między indeks obciążenie względem wydajności kwerend.  

Zapoznaj się z [próbki DocumentDB](https://github.com/Azure/azure-documentdb-net) w witrynie MSDN dla próbki przedstawiający, jak skonfigurować zasady indeksowania dla zbioru. Załóżmy najwyższy do szczegółów składnia DocumentDB SQL.


## <a name="basics-of-a-documentdb-sql-query"></a>Podstawowe informacje o zapytania DocumentDB SQL
Co zapytanie składa się z klauzuli SELECT i opcjonalnie FROM i klauzul WHERE na standardów ANSI SQL. Zazwyczaj dla każdej kwerendy źródła w klauzuli FROM jest wyliczyć. Następnie w źródle do pobrania podzbioru JSON dokumentów jest stosowany filtr w klauzuli WHERE. Na koniec klauzula SELECT jest używany do projektu wymagane JSON wartości na liście.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>Klauzula FROM
`FROM <from_specification>` Klauzula jest opcjonalny, jeśli źródło jest filtrowana lub planowanego w dalszej części kwerendy. Celem w tej klauzuli jest określenie źródła danych, na którym musi działać kwerendy. Często całego zbioru jest źródłem, ale zamiast tego można określić podzbiór kolekcji. 

Zapytania, takich jak `SELECT * FROM Families` wskazuje, że cały zbiór rodziny jest źródło, w którym wyliczyć. Specjalny identyfikator katalogu głównego może służyć reprezentować zbioru zamiast nazwę kolekcji. Poniższa lista zawiera reguły, które są wymuszane dla kwerendy:

- Kolekcja może być aliasu, takich jak `SELECT f.id FROM Families AS f` lub po prostu `SELECT f.id FROM Families f`. W tym miejscu `f` odpowiada `Families`. `AS`to opcjonalne słowo kluczowe, aby alias identyfikator.

-   Uwaga to jeden raz, ponieważ oryginalne źródło nie może być powiązane. Na przykład `SELECT Families.id FROM Families f` ma niepoprawną, ponieważ nie można już rozpoznać identyfikator "Rodzin".

-   Wszystkie właściwości, które muszą odwoływać się musi być w pełni kwalifikowana. W przypadku braku schematu ścisłe przestrzeganie to jest wymuszone w celu uniknięcia wszelkich niejednoznacznych powiązań. Dlatego `SELECT id FROM Families f` ma niepoprawną od właściwość `id` nie jest powiązany.
    
### <a name="sub-documents"></a>Dokumentów podrzędnych
Źródła być ograniczona w taki sposób, aby mniejszy zestaw. Na przykład do wyliczania podrzędny drzewa w każdym dokumencie, podrzędny głównego może stanie się źródła, jak pokazano w poniższym przykładzie.

**Kwerendy**

    SELECT * 
    FROM Families.children

**Wyniki**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Chociaż powyższym przykładzie tablicy są używane jako źródło, obiekt może również jako źródło, w którym jest tego, co przedstawiono w poniższym przykładzie. Każda prawidłowych JSON wartość (nie zdefiniowano) można znaleźć w źródle uznaje się do uwzględnienia w wyniku kwerendy. Jeśli nie masz kilka rodziny `address.state` wartości, będą one wykluczone w wyniku kwerendy.

**Kwerendy**

    SELECT * 
    FROM Families.address.state

**Wyniki**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>Klauzula WHERE
Klauzula WHERE (**`WHERE <filter_condition>`**) jest opcjonalne. Określa że warunki dokumentów JSON dostarczony przez źródła musi spełniać w celu zapewnienia częścią wyniku. Dowolny dokument JSON muszą być określone warunki na wartość "true" ma zostać uwzględniony w wyniku. Klauzula WHERE jest używana przez warstwy indeksu w celu ustalenia najmniejszą bezwzględne podzestaw dokumentów źródłowych, które mogą być częścią wyniku. 

Poniższe zapytanie żądania dokumentów, które zawierają właściwość name, których wartość jest `AndersenFamily`. Każdy inny dokument, który nie ma właściwość name lub miejsce, w którym wartość jest niezgodna `AndersenFamily` jest wyłączona. 

**Kwerendy**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Poprzednim przykładzie pokazano równości prostej kwerendy. DocumentDB SQL obsługuje również wiele skalarne wyrażeń. Najczęściej używane są binarnych i jednoargumentowy wyrażeń. Właściwość odwołania z obiektu JSON źródłowego są również prawidłowych wyrażeń. 

Następujące operatory binarne są obecnie obsługiwane i mogą zostać użyte w kwerendach, jak pokazano w poniższych przykładach:  
<table>
<tr>
<td>Średnia</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitowe</td>    
<td>|, &, ^, <<, >>, >>> (prawy shift wypełnienia zero) </td>
</tr>
<tr>
<td>Logicznych.</td>
<td>I, LUB NIE</td>
</tr>
<tr>
<td>Porównanie</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Ciąg</td> 
<td>|| (złączanie)</td>
</tr>
</table>  

Spójrzmy na niektórych kwerend przy użyciu operatorów binarnych.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Operatory jednoargumentowy +,-, ~ nie są również obsługiwane i może być używany wewnątrz kwerendy, jak pokazano w poniższym przykładzie:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Oprócz binarnych i jednoargumentowy operatorów odwołań do właściwości są również dozwolone. Na przykład `SELECT * FROM Families f WHERE f.isRegistered` zwraca JSON dokumentu zawierającego właściwość `isRegistered` miejsce, w którym wartość właściwości jest równa JSON `true` wartość. Inne wartości (FAŁSZ, wartość null, nieokreślone, `<number>`, `<string>`, `<object>`, `<array>`, itp.) prowadzi do dokumentu źródłowego są wykluczone z wyników. 

### <a name="equality-and-comparison-operators"></a>Operatory równości i porównania
W poniższej tabeli pokazano wyniki porównania równości w języku SQL DocumentDB między dowolne dwa typy JSON.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Otwórz</strong>
         </td>
         <td valign="top">
            <strong>Nieokreślone</strong>
         </td>
         <td valign="top">
            <strong>Wartości null</strong>
         </td>
         <td valign="top">
            <strong>Wartość logiczna</strong>
         </td>
         <td valign="top">
            <strong>Liczba</strong>
         </td>
         <td valign="top">
            <strong>Ciąg</strong>
         </td>
         <td valign="top">
            <strong>Obiekt</strong>
         </td>
         <td valign="top">
            <strong>Tablica</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nieokreślone<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Wartości null<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Wartość logiczna<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Liczba<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Ciąg<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Obiekt<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nieokreślone </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tablica<strong>
         </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
Nieokreślone </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
      </tr>
   </tbody>
</table>

Dla innych operatory porównania, takich jak >, > =,! =, < i < = następujących reguł zastosować:   

-   Porównanie różnych typów wyników w nieokreślone.
-   Porównanie dwa obiekty lub dwie tablice wyników w nieokreślone.   

Jeśli wynik wyrażenie skalarne, które w filtrze jest zdefiniowane, odpowiedni dokument nie będzie uwzględniony w wyniku, ponieważ nieokreślone logicznie nie równe "prawda".

### <a name="between-keyword"></a>MIĘDZY słów kluczowych
Za pomocą słowa kluczowego BETWEEN aby wyrazić zapytań zakresy wartości, podobnie jak w języku SQL ANSI. MIĘDZY można przed ciągów lub liczb.

Na przykład ta kwerenda zwraca wszystkie dokumenty rodziny, w których ocen pierwszy element podrzędny wynosi 1-5 (obie włącznie). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

W odróżnieniu od w ANSI SQL umożliwia także klauzuli BETWEEN w klauzuli FROM, jak w poniższym przykładzie.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Dla krótszy czas wykonania kwerendy Pamiętaj, aby utworzyć zasady indeksowania składającego się zakres indeks przed dowolnego liczbowe właściwości i ścieżki, które są filtrowane w klauzuli BETWEEN. 

Główna różnica pomiędzy przy użyciu BETWEEN w DocumentDB i języka SQL ANSI jest że można wyrazić kwerend zakres dla właściwości mieszanych typów — na przykład może być "klasy" jest liczbą (5) w przypadku niektórych dokumentów i ciągi w innym ("grade4"). W tych przypadkach takich jak w JavaScript, porównania dwóch różnych typów wyników w "nieokreślone", a dokument zostanie pominięta.

### <a name="logical-and-or-and-not-operators"></a>Logiczne (AND, OR i NOT) operatorów
Operatory logiczne działają na wartości logiczne. Logiczne tabel prawdy dla tych operatorów przedstawiono w poniższych tabelach.

LUB|PRAWDA|FAŁSZ|Nieokreślone
---|---|---|---
PRAWDA|PRAWDA|PRAWDA|PRAWDA
FAŁSZ|PRAWDA|FAŁSZ|Nieokreślone
Nieokreślone|PRAWDA|Nieokreślone|Nieokreślone

ORAZ|PRAWDA|FAŁSZ|Nieokreślone
---|---|---|---
PRAWDA|PRAWDA|FAŁSZ|Nieokreślone
FAŁSZ|FAŁSZ|FAŁSZ|FAŁSZ
Nieokreślone|Nieokreślone|FAŁSZ|Nieokreślone

NIE|  |
---|---
PRAWDA|FAŁSZ
FAŁSZ|PRAWDA
Nieokreślone|Nieokreślone

### <a name="in-keyword"></a>W słów kluczowych
Słowo kluczowe w można sprawdzić, czy określona wartość odpowiada wartości na liście. Na przykład ta kwerenda zwraca wszystkie dokumenty rodziny miejsce, w którym jest "WakefieldFamily" lub "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

W tym przykładzie zwraca wszystkie dokumenty stanu w przypadku dowolnej z określonymi wartościami.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Trzyelementowy (?) i operatory łączonej (?)
Operatory Trzyelementowy i Coalesce umożliwia tworzenie wyrażeń warunkowych podobne do popularnych języków programowania, takich jak C# i JavaScript. 

Operator trójargumentowy (?) mogą być bardzo przydatne podczas tworzenia nowych właściwości JSON w czasie rzeczywistym. Na przykład możesz teraz napisać kwerendy do klasyfikowania poziomy klasy do ludzi formularza czytelne jak dla początkujących i pośrednie i zaawansowane tak jak pokazano poniżej.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Można zagnieżdżać wywołań operator like w kwerendzie poniżej.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Jako z innych operatorów zapytania, jeśli odwołania właściwości w wyrażenie warunkowe nie są widoczne w każdym dokumencie lub jeśli typy porównywane są różne, następnie te dokumenty będą wykluczone w wynikach kwerendy.

Operator łączonej (?) może służyć do efektywne sprawdzić obecność właściwości (alias jest definiowana) w dokumencie. To jest przydatny, gdy przetwarzanie kwerend dotyczących półstrukturalnych lub dane mieszanych typów. Na przykład ta kwerenda zwraca "nazwisko", jeśli istnieją lub "nazwisko" Jeśli nie Prezentuj.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Właściwość cytowane dostępu
Może też uzyskiwać dostęp za pomocą operatora cudzysłowach właściwości właściwości `[]`. Na przykład `SELECT c.grade` i `SELECT c["grade"]` są równoważne. Tej składni jest przydatny, gdy trzeba escape właściwość, która zawiera spacje, znaki specjalne, lub się dzieje, aby udostępnić taką samą nazwę jak słowo kluczowe SQL lub słowo zastrzeżone.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>Klauzula SELECT
Klauzula SELECT (**`SELECT <select_list>`**) jest wymagana i umożliwia określenie wartości, jakie będą pobierane z kwerendy, podobnie jak w języku SQL ANSI. Podzbioru, który jest zostały przefiltrowane u góry dokumentu źródłowego są przekazywane na faza przewidywania, gdzie określonymi wartościami JSON są pobierane i nowy obiekt JSON jest tworzona, dla każdego danych wejściowych przekazywanych na niego. 

W poniższym przykładzie pokazano typowy kwerendy WYBIERAJĄCEJ. 

**Kwerendy**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Właściwości zagnieżdżonych
W poniższym przykładzie, możemy projekcję dwie właściwości zagnieżdżonych `f.address.state` i `f.address.city`.

**Kwerendy**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Rzut obsługuje także wyrażeń JSON, jak pokazano w poniższym przykładzie.

**Kwerendy**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Przyjrzyjmy się rolę `$1` tutaj. `SELECT` Klauzula musi utworzyć obiekt JSON i od klucza nie jest dostępne, firma Microsoft korzysta z argumentów niejawne zmiennych o nazwach rozpoczynających się `$1`. Na przykład ta kwerenda zwraca dwóch zmiennych argumentów niejawne etykietą `$1` i `$2`.

**Kwerendy**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Wygładzanie
Teraz przejdźmy rozszerzanie powyższym przykładzie z wygładzanie jawne wartości. Jest używany dla aliasów słowo kluczowe. Zauważ, że jest opcjonalne, jak pokazano podczas projekcję druga wartość jako `NameInfo`. 

W przypadku, gdy kwerenda ma dwie właściwości o takiej samej nazwie, wygładzanie należy zmienić nazwy co najmniej jedną z właściwości tak, że są one sobie w wyniku przewidywanego.

**Kwerendy**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Wyrażenie skalarne
Oprócz odwołania właściwość klauzula SELECT obsługuje skalarne wyrażeniami stałe, arytmetyczne wyrażeń, wyrażeń logicznych itp. Na przykład Oto prostej kwerendy "Witaj świecie".

**Kwerendy**

    SELECT "Hello World"

**Wyniki**

    [{
      "$1": "Hello World"
    }]


Oto przykład bardziej złożone, która używa wyrażenie skalarne.

**Kwerendy**

    SELECT ((2 + 11 % 7)-2)/3   

**Wyniki**

    [{
      "$1": 1.33333
    }]


W poniższym przykładzie wynik wyrażenie skalarne, które jest wartością logiczną.

**Kwerendy**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Wyniki**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Tworzenie tablicy i obiektu
Inną funkcją kluczowe SQL DocumentDB jest tworzenie tablicy i obiektów. W poprzednim przykładzie Pamiętaj, że firma Microsoft utworzone nowy obiekt JSON. Podobnie jedną można także konstruować tablicach jak przedstawiono w poniższych przykładach.

**Kwerendy**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Wyniki**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>WARTOŚĆ słowa kluczowego
Słowo kluczowe **wartość** umożliwia zwraca wartość JSON. Na przykład kwerendy, jak pokazano poniżej zwraca wartość skalarna `"Hello World"` zamiast `{$1: "Hello World"}`.

**Kwerendy**

    SELECT VALUE "Hello World"

**Wyniki**

    [
      "Hello World"
    ]


Poniższa kwerenda zwraca wartość JSON bez `"address"` etykiety w wynikach.

**Kwerendy**

    SELECT VALUE f.address
    FROM Families f 

**Wyniki**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Poniższy przykład rozszerza tę opcję, aby wyświetlić jak zwrócić wartości pierwotne JSON (poziom liścia drzewa JSON). 

**Kwerendy**

    SELECT VALUE f.address.state
    FROM Families f 

**Wyniki**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
Specjalne operator (*) jest obsługiwany do programu project dokumentu jako-jest. Jeśli używany, musi to być tylko pola przewidywane. Podczas zapytania, takich jak `SELECT * FROM Families f` jest prawidłowa, `SELECT VALUE * FROM Families f ` i `SELECT *, f.id FROM Families f ` są nieprawidłowe.

**Kwerendy**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Wyniki**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>Operatora góry
Najważniejsze słowa kluczowego można ograniczyć liczbę wartości z kwerendy. Jeśli góry jest używana w połączeniu z klauzula ORDER BY, zestaw wyników jest ograniczona do N pierwszą liczbę zamówionych wartości; w przeciwnym razie zwraca numer pierwszej N wyników w kolejności nieokreślone. Zgodnie z zaleceniami dotyczącymi w instrukcji SELECT zawsze za pomocą klauzula ORDER BY klauzuli TOP. To jest jedynym sposobem właściwie wskazują, które wiersze dotyczy góry. 


**Kwerendy**

    SELECT TOP 1 * 
    FROM Families f 

**Wyniki**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

GÓRY można używać z wartościami stałymi (jak pokazano powyżej) lub za pomocą kwerend parametrycznych wartości zmiennej. Aby uzyskać więcej informacji zobacz kwerend parametrycznych poniżej.

## <a name="order-by-clause"></a>Klauzula ORDER BY
Podobnie jak w ANSI SQL, mogą zawierać opcjonalny klauzula Order By podczas wykonywania kwerend. Klauzula może zawierać opcjonalny argument ROS/MAL, aby określić kolejność, w której można pobrać wyniki. Bardziej szczegółowy widok według kolejności można znaleźć w [Kolejności DocumentDB przez instruktażu](documentdb-orderby.md).

Na przykład Oto kwerendę, która pobiera rodziny uporządkowanych według nazwy miasta pobytu.

**Kwerendy**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Wyniki**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

I w tym czasie kwerendę, która pobiera rodziny uporządkowanych według daty utworzenia, która jest przechowywana jako liczba przedstawiająca epoch, znaczy, czas od 1 stycznia 1970 w sekundach.

**Kwerendy**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Wyniki**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Pojęcia zaawansowane bazy danych i zapytania SQL
### <a name="iteration"></a>Iteracji
Nowe konstrukcji zostało dodane za pomocą słów kluczowych **w** w języku SQL DocumentDB do obsługi, które na tablicach JSON. Źródła od zapewnia obsługę iteracji. Zacznijmy poniższym przykładzie:

**Kwerendy**

    SELECT * 
    FROM Families.children

**Wyniki**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Teraz Przyjrzyjmy się innej kwerendzie, która wykonuje iterację przez dzieci w kolekcji. Uwaga różnicy w tablicy wyjściowej. W tym przykładzie dzieli `children` i spłaszcza wyniki do pojedynczej tablicy.  

**Kwerendy**

    SELECT * 
    FROM c IN Families.children

**Wyniki**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

To jeszcze bardziej można filtrować według każdej pojedynczych pozycji tablicy, jak pokazano w poniższym przykładzie.

**Kwerendy**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Wyniki**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Sprzężenia
Relacyjne bazy danych ważne jest potrzebna, aby dołączyć do tabel. Jest logiczna następstwem do projektowania znormalizowaną schematy. W przeciwieństwie do tego DocumentDB dotyczy modelu danych nieznormalizowany wolny schematu dokumentów. To jest logiczny odpowiednik a "samosprzężenia".

Składni, która obsługuje język to sprzężenie sprzężenia < from_source2 > < from_source1 >... Dołączanie < from_sourceN >. Ogólnie mówiąc to zwraca zestaw **N**- krotki (krotki z wartościami **N** ). Każdej krotki zawiera wartości wytwarzane przez Iterowanie wszystkie aliasy zbioru przez ich odpowiednich zestawów. Innymi słowy to pełny iloczyn wektorowy zestawów uczestniczenie w sprzężenia.

W poniższych przykładach pokazano, jak działa klauzuli JOIN. W poniższym przykładzie wynik jest pusta, ponieważ iloczyn wektorowy każdego dokumentu ze źródła i pustego zestawu jest pusta.

**Kwerendy**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Wyniki**  

    [{
    }]


W poniższym przykładzie jest to między głównym dokumentu i `children` główny podrzędnych. Jest iloczyn wektorowy między dwoma obiektami JSON. Fakt, że dzieci jest tablicy nie jest w sprzężenia skutecznych, ponieważ są możemy zajmujących się pojedynczego głównego, która jest tablicą elementów podrzędnych. W związku z tym wynik zawiera tylko dwa wyników, ponieważ dokładnie tylko jeden dokument Zwraca iloczyn wektorowy każdego dokumentu z tablicy.

**Kwerendy**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Wyniki**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


W poniższym przykładzie pokazano bardziej konwencjonalny sprzężenia:

**Kwerendy**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Wyniki**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Najpierw pamiętać jest `from_source` **Dołączanie** klauzula jest iterację. Tak przepływ w tym przypadku jest w następujący sposób:  

-   Rozwijanie każdy element podrzędny **c** w tablicy.
-   Stosowanie iloczyn wektorowy z katalogu głównego dokumentu **f** każdy element podrzędny **c** , która została spłaszczoną w pierwszym kroku.
-   Na koniec projektu tylko właściwość name obiektu **f** głównego. 

Pierwszy dokument (`AndersenFamily`) zawiera tylko jeden element podrzędny, więc zestaw wyników zawiera tylko jeden obiekt odpowiadający mu w tym dokumencie. Drugim dokumencie (`WakefieldFamily`) zawiera dwa elementy podrzędne. Tak iloczyn wektorowy tworzy oddzielny obiekt dla każdego dziecka, powodując dwóch obiektów, po jednym dla każdego dziecka odpowiadający mu w tym dokumencie. Uwaga pola głównego w oba te dokumenty będą takie same, zgodnie z oczekiwaniami w iloczyn wektorowy.

Narzędzie rzeczywistą sprzężenia ma krotki formularza z iloczyn wektorowy do kształtu, który jest trudne do projektu. Ponadto jak należy sprawdzić, w poniższym przykładzie, można filtrować na kombinacja krotki że umożliwia użytkownik wybrał warunku spełniać krotki ogólnej.

**Kwerendy**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Wyniki**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



W tym przykładzie jest naturalne rozszerzenie powyższym przykładzie i wykonuje podwójne sprzężenia. Tak można wyświetlić iloczyn wektorowy jako poniższy kod pseudo.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`zawiera jeden element podrzędny, kto ma jedną pet. Tak, iloczyn wektorowy zwraca jeden wiersz (1*1*1) z tej rodziny. WakefieldFamily jednak zawiera dwa elementy podrzędne, ale tylko jeden element podrzędny "Jesse" ma domowych. Jesse ma jednak domowych 2. W związku z tym iloczyn wektorowy daje wierszy*1*2 = 2 1 z tej rodziny.

W następnym przykładzie na jest dodatkowy filtr `pet`. Z wyłączeniem wszystkich krotki gdzie nazwa pet nie jest "Tle". Zwróć uwagę, że możemy utworzyć krotki z tablic, filtr w dowolnej części krotki i program project dowolną kombinację elementów. 

**Kwerendy**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Wyniki**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>Integracja języka JavaScript
DocumentDB zapewnia model programowania wykonywania logiki aplikacji JavaScript podstawie bezpośrednio na zbiory procedur składowanych i wyzwalaczy. Dzięki temu dla obu:

-   Możliwość wykonywania operacji OBSŁUGIWAŁ transakcji wysokiej wydajności i kwerend dla dokumentów w zbiorze na podstawie głębokości Włączanie obsługi języka JavaScript bezpośrednio z poziomu aparatu bazy danych. 
-   Modelowanie naturalne przepływ sterowania, zmiennych zakresy, przydział i integracji wyjątku obsługi pierwotnych z transakcje bazy danych. Więcej informacji na temat DocumentDB obsługi języka JavaScript integracji można znaleźć w dokumentacji programowania po stronie serwera JavaScript.

###<a name="user-defined-functions-udfs"></a>Funkcje (UDF) zdefiniowane przez użytkownika
Oraz typy zdefiniowanych w tym artykule DocumentDB SQL zapewnia obsługę dla użytkownika zdefiniowane funkcje (UDF). W szczególności skalarne funkcji zdefiniowanych przez użytkownika są obsługiwane, gdzie programiści można przekazywać zero lub wiele argumentów i zwracać wyniku jeden argument ponownie. Każdy z tych argumentów są sprawdzane pod kątem są wartości JSON prawne.  

Składnia DocumentDB SQL jest używane do obsługi logiczny niestandardową aplikację za pomocą funkcji zdefiniowane przez użytkownika. Funkcji zdefiniowanych przez użytkownika mogą być rejestrowane w DocumentDB, a następnie odwoływać się jako część zapytania SQL. W rzeczywistości funkcji zdefiniowanych przez użytkownika exquisitely są przeznaczone do wywołania przez kwerendy. Jako swoje następstwo ta opcja funkcji zdefiniowanych przez użytkownika nie mają dostępu do obiektu kontekstu, które mają innych typów JavaScript (procedur składowanych i wyzwalaczy). Ponieważ wykonywanie kwerendy jako tylko do odczytu, można uruchamiać podstawowa albo pomocnicza replik. W związku z tym funkcji zdefiniowanych przez użytkownika są przeznaczone do uruchamiania na pomocniczym repliki w przeciwieństwie do innych typów JavaScript.

Poniżej przedstawiono przykładowy sposób UDF można zarejestrować w DocumentDB bazy danych, a w szczególności w obszarze kolekcji dokumentów.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
W poprzednim przykładzie jest tworzona UDF, którego nazwa ma `REGEX_MATCH`. Przyjmuje dwie wartości ciągu JSON `input` i `pattern` i sprawdza, czy pierwszy dopasowania wzorca określonego w drugim przy użyciu funkcji string.match() i JavaScript.


Teraz możemy użyć tego UDF w kwerendzie w rzut. Funkcji zdefiniowanych przez użytkownika musi być kwalifikowany z prefiksem uwzględniania wielkości liter "udf." gdy wywołana z poziomu kwerendy. 

>[AZURE.NOTE] Przed 2015-3-17 DocumentDB obsługiwane UDF połączeń bez "udf." Prefiks, takich jak ZAZNACZANIE REGEX_MATCH(). Została zastąpiona tego połączeń wzorca.  

**Kwerendy**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Wyniki**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF również można używać wewnątrz filtru, jak pokazano w poniższym przykładzie, również poprzedzone "udf." Prefiks:

**Kwerendy**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Wyniki**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


W zasadzie funkcji zdefiniowanych przez użytkownika są prawidłowe skalarne wyrażeń i mogą być używane w zarówno planów i filtry. 

Aby rozwinąć na możliwości funkcji zdefiniowanych przez użytkownika, Oto inny przykład z wyrażenie warunkowe:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Poniżej przedstawiono przykładowy, który wykonuje UDF.

**Kwerendy**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Wyniki**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Jak zaprezentowania poprzednich przykładach, funkcji zdefiniowanych przez użytkownika Integracja z możliwości języka JavaScript z SQL DocumentDB sformatowanego interfejs programowalnej zrobić złożonych logiczny procedurach, warunkowe przy użyciu wbudowanych funkcji obsługi języka JavaScript.

DocumentDB SQL przekazuje argumenty funkcji zdefiniowanych przez użytkownika dla każdego dokumentu w źródle u bieżącego etapu (klauzuli WHERE lub klauzula SELECT) przetwarzania UDF. Wynikiem jest włączona bezproblemowo ogólnego potoku wykonania. Jeśli właściwości określonych przez UDF parametry nie są dostępne w wartości JSON, parametr jest traktowana jako zdefiniowane i w związku z tym wywołania UDF jest całkowite usunięcie pominięty. Podobnie w przypadku nieokreślone wynik UDF go nie znajduje się w wyniku. 

Podsumowując funkcji zdefiniowanych przez użytkownika są doskonałych narzędzi do złożonych warunków logicznych w ramach kwerendy.

### <a name="operator-evaluation"></a>Operator oceny
DocumentDB, naturalne jest bazie JSON rysuje równoleżników z operatorów JavaScript i ich znaczenie oceny. Podczas DocumentDB próbuje zachować JavaScript do znaczeń właściwych pod względem obsługi JSON, oceny operacji odchylenie w niektórych przypadkach.

W języku SQL DocumentDB, w odróżnieniu od tradycyjnych SQL typów wartości często nie są znane do momentu wartości są faktyczną pobierane z bazy danych. Aby efektywnie wykonywać kwerendy, większość operatorów ma wymagania dotyczące typu ściśle. 

DocumentDB SQL nie działa niejawnej konwersji, w przeciwieństwie do języka JavaScript. Na przykład zapytania, takich jak `SELECT * FROM Person p WHERE p.Age = 21` pasuje do dokumentów, które zawierają właściwość wieku, której wartość jest 21. Każdy inny dokument, których właściwość wieku odpowiada warianty niekiedy nieskończonej ciąg "21" lub inne, takie jak "021", "21.0", "0021", "00021", nie zostanie dopasowana itp. W przeciwieństwie to JavaScript umiejscowienia niejawnie rzutować liczby wartości ciągu (na podstawie operatora, ex: ==). Ten wybór jest istotne dla efektywnego indeksu pasujących w języku SQL DocumentDB. 

## <a name="parameterized-sql-queries"></a>Kwerend parametrycznych SQL
DocumentDB obsługuje kwerendy z parametrami wyrażona ze znanym @ notacji. Parametryczne SQL zawiera rozbudowany obsługi i ucieczce użytkownika do wprowadzania, zapobieganie przypadkowe narażenie danych za pomocą uruchomienie SQL. 

Na przykład można napisać kwerendę, która przyjmuje nazwisko i adres stan jako parametry, a następnie wykonywania dla różnych wartości nazwisko i adres Stan oparte na danych wprowadzonych przez użytkownika.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Ten można następnie wysyłane żądanie DocumentDB jako zapytanie parametryczne JSON, tak jak pokazano poniżej.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Argument do góry można ustawić za pomocą kwerend parametrycznych, tak jak pokazano poniżej.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Wartości parametrów może mieć dowolne prawidłowe JSON (ciągi, liczby, wartości logiczne, null, a nawet tablicami lub zagnieżdżone JSON). Również ponieważ DocumentDB jest mniej schematu, parametry nie zostały zweryfikowane przed dowolnego typu.

##<a name="built-in-functions"></a>Funkcje wbudowane
DocumentDB obsługuje także wiele wbudowanych funkcji typowych operacji, które mogą być używane wewnątrz kwerend, takich jak funkcje zdefiniowane przez użytkownika (UDF).

<table>
<tr>
<td>Funkcje matematyczne</td> 
<td>Moduł.liczby, CEILING, EXP, FLOOR, dziennika, LOG10, POWER, ROUND, logowania, pierwiastek, KWADRATOWY, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, KOT, stopnie, PI RADIANÓW, SIN i TAN</td>
</tr>
<tr>
<td>Wpisz funkcji sprawdzania</td>    
<td>IS_ARRAY, IS_BOOL IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED i IS_PRIMITIVE</td>
</tr>
<tr>
<td>Funkcje tekstowe</td>   
<td>"Concat", zawiera, ENDSWITH, INDEX_OF, lewej, długość, DOLNY, funkcje LTRIM, ZAMIEŃ, SKOPIOWANYMI, odwrotna kolejność, prawy, RTRIM, STARTSWITH, PODCIĄG i GÓRNYM</td>
</tr>
<tr>
<td>Funkcji tablicowych</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH i ARRAY_SLICE</td>
</tr>
<tr>
<td>Funkcje przestrzenna</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID i ST_ISVALIDDETAILED</td>
</tr>
</table>  

Jeśli obecnie korzystasz z funkcji zdefiniowanych przez użytkownika (UDF), dla którego wbudowanej funkcji jest teraz dostępna, należy używać odpowiednich funkcji wbudowanych, ponieważ ma być szybsze uruchamianie i bardziej efektywne. 

### <a name="mathematical-functions"></a>Funkcje matematyczne
Funkcje matematyczne wykonywanie obliczeń, zazwyczaj oparte na wartości wejściowych, które są podane jako argumenty i zwraca wartość liczbową. Oto Tabela obsługiwanych wbudowanych funkcji matematycznych.

<table>
<tr>
<td><strong>Użycie</strong></td>
<td><strong>Opis</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Zwraca wartość bezwzględną (dodatnie) określonego wyrażenia liczbowego.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">CEILING (num_expr)</a></td> 
<td>Zwraca najmniejszą wartość liczby całkowitej większe niż lub równe określonej wyrażenie liczbowe.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">POWIERZCHNIA (num_expr)</a></td> 
<td>Zwraca największą liczbę całkowitą mniejsza niż lub równa określonej wyrażenie liczbowe.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Zwraca wykładnik potęgi określonej wyrażenia liczbowego.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">Dziennik (num_expr [, base])</a></td> 
<td>Zwraca logarytm naturalny określonej wyrażenie liczbowe lub przy użyciu określonej podstawy logarytmu</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>Zwraca wartość logarytmiczna base 10 określonego wyrażenia liczbowego.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">ZAOKRĄGLANIE (num_expr)</a></td> 
<td>Zwraca wartość liczbową, zaokrąglona do najbliższej całkowitej.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>Zwraca wartość liczbową zaokrąglane do najbliższej całkowitej.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">Pierwiastek (num_expr)</a></td>   
<td>Zwraca wartość pierwiastka kwadratowego z określonego wyrażenia liczbowego.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">KWADRATOWY (num_expr)</a></td>   
<td>Zwraca kwadrat określone wyrażenie liczbowe.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (num_expr, num_expr)</a></td>   
<td>Zwraca określona wartość potęgi określonej wyrażenie liczbowe.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">ZNAK (num_expr)</a></td>   
<td>Zwraca wartość logowania (-1, 0, 1) określone wyrażenie liczbowe.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>Zwraca kąt, wyrażony w radianach, której cosinus jest określony wyrażenie liczbowe; Skrót cosinus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>Zwraca kąt, wyrażony w radianach, którego sinus to określone wyrażenie liczbowe. Jest to także sinus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>Zwraca kąt, wyrażony w radianach, którego tangens jest określony wyrażenie liczbowe. Jest to także tangens.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Zwraca kąt, wyrażony w radianach, między dodatnia osi x i promienisty względem punktu początkowego punktu (y, x), gdzie x i y są wartościami dwóch wyrażeń określonych ruchomości.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Zwraca cosinus trygonometryczne określony kąt wyrażony w radianach, określonego wyrażenia.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Zwraca trygonometryczne cotangens kąta określonego w radianach w określonej wyrażenie liczbowe.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">STOPNIE (num_expr)</a></td> 
<td>Zwraca odpowiedni kąt wyrażony w stopniach dla kąta określonego w radianach.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Zwraca wartość stałej PI.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIANY (num_expr)</a></td> 
<td>Zwraca radiany, gdy wyrażenie liczbowe, wyrażony w stopniach, zostanie wprowadzony.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Zwraca sinus trygonometryczne określony kąt wyrażony w radianach, określonego wyrażenia.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Zwraca tangens wprowadzania wyrażenia określonego wyrażenia.</td>
</tr>

</table> 

Na przykład można teraz uruchamianie kwerend podobnej do następującej:

**Kwerendy**

    SELECT VALUE ABS(-4)

**Wyniki**

    [4]

Główna różnica między funkcjami DocumentDB w porównaniu z języka SQL ANSI to, że są one przeznaczone do pracy z danych bez schematu i mieszanymi schematu. Na przykład jeśli użytkownik ma otwarty dokument, gdzie brakuje właściwość rozmiar lub ma wartość nienumeryczną, takich jak informacja "Brak danych", a następnie dokument jest pominięty, zamiast powrócić komunikat o błędzie.

### <a name="type-checking-functions"></a>Wpisz funkcji sprawdzania
Funkcje sprawdzania typów umożliwiają sprawdzanie typu wyrażenia w ciągu zapytania SQL. Funkcje sprawdzania typów może służyć do ustalania typu właściwości w dokumentach w czasie rzeczywistym, gdy jest zmienną lub nieznany. Poniżej przedstawiono tabelę obsługiwane kontrola funkcje wbudowane typów.

<table>
<tr>
  <td><strong>Użycie</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli typ wartości jest tablicą.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli wartość jest wartością logiczną.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli typ wartości ma wartość null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli typ wartości jest liczbą.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli typ wartości jest obiektem JSON.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli wartość jest ciąg.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli właściwość została przypisana wartość.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (wyrażenie)</a></td>
  <td>Zwraca wartość logiczną wskazującą, jeśli typ wartości jest ciąg, liczba, wartość logiczna lub wartość null.</td>
</tr>

</table>

Za pomocą tych funkcji, można teraz uruchamianie kwerend podobnej do następującej:

**Kwerendy**

    SELECT VALUE IS_NUMBER(-4)

**Wyniki**

    [true]

### <a name="string-functions"></a>Funkcje tekstowe
Następujące funkcje skalarne wykonywanie operacji na wejściowej wartości ciągu i zwrócić ciąg, wartość liczbową lub logicznych. Oto Tabela ciąg wbudowanych funkcji:

Użycie|Opis
---|---
[DŁUGOŚĆ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Zwraca liczbę znaków wyrażenia określonego ciągu znaków
["Concat" (str_expr str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Zwraca ciąg, który jest wynikiem łączenia dwóch lub więcej wartości ciągów.
[SUBSTRING (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Zwraca część wyrażenie ciągu.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Zwraca wartość logiczną wskazującą, czy wyrażenie tekstowe pierwszej kończy się na sekundę
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Zwraca wartość logiczną wskazującą, czy wyrażenie tekstowe pierwszej kończy się na sekundę
[ZAWIERA (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Zwraca wartość logiczną wskazującą, czy wyrażenie tekstowe pierwszej zawiera drugie.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Zwraca pozycję początkową pierwszego wystąpienia drugi ciąg wyrażenie w pierwszym wyrażeniem określonego ciągu lub -1, jeśli nie można odnaleźć tego ciągu.
[LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Zwraca część po lewej stronie ciągiem o określoną liczbę znaków.
[RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Zwraca prawa część ciągu z określoną liczbę znaków.
[Funkcje LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Zwraca wyrażenie tekstowe po usuwa spacje wiodące.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Zwraca wyrażenie tekstowe po obcinanie wszystkich spacji końcowych.
[DOLNY (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Zwraca wyrażeniem po przekonwertowaniu danych wielkie litery znaku na małe litery.
[GÓRNY (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Zwraca wyrażenie tekstowe po przekonwertowaniu danych znak małe litery na wielkie litery.
[ZAMIEŃ (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Zamienia wszystkie wystąpienia określonej wartości ciągu na inną wartość ciągu.
[SKOPIOWANYMI (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Powtarza wartość ciągu określoną liczbę razy.
[Odwrotna kolejność (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Zwraca odwrotnej kolejności wartość ciągu.

Za pomocą tych funkcji, można teraz uruchamiać kwerend, takich jak następujące czynności. Na przykład można powrócić nazwiska na wielkie litery w następujący sposób:

**Kwerendy**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Wyniki**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Lub ciągów podobnie jak w tym przykładzie:

**Kwerendy**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Wyniki**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Funkcje tekstowe można również w klauzuli WHERE można filtrować wyniki, tak jak w poniższym przykładzie:

**Kwerendy**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Wyniki**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Funkcji tablicowych
Następujące funkcje skalarne wykonywanie operacji na tablicy wartości wejściowej, zwracana liczbowe, wartość logiczną lub tablicy. Oto tabeli tablicy wbudowanych funkcji:

Użycie|Opis
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Zwraca liczbę elementów wyrażenia określonej tablicy.
[ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Zwraca tablicę, która jest wynikiem łączenia dwóch lub więcej wartości tablicy.
[ARRAY_CONTAINS (arr_expr, wyrażenie)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Zwraca wartość logiczną wskazującą, czy tablica zawiera określonej wartości.
[ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Zwraca część wyrażenia tablicy.

Funkcje Tablica może służyć do manipulowania tablice w formacie JSON. Na przykład Oto kwerendę, która zwraca wszystkich dokumentów, w przypadku, gdy elementów nadrzędnych jest "Krzysztof Wakefield". 

**Kwerendy**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Wyniki**

    [{
      "id": "WakefieldFamily"
    }]

Oto kolejny przykład użycia ARRAY_LENGTH uzyskać liczby dzieci na rodzinę.

**Kwerendy**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Wyniki**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Funkcje przestrzenna

DocumentDB obsługuje następujące funkcje wbudowane Otwórz geograficzne Consortium (OGC) do wykonywania kwerend geograficznych. Aby uzyskać więcej informacji na temat geograficzne obsługi DocumentDB, zobacz [Praca z danymi geograficzne w Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Użycie</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Zwraca odległości między dwoma wyrażeniami punktu GeoJSON.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Zwraca wartość wskazującą, czy punktu GeoJSON określonej w pierwszym argumencie jest w Wielokąt GeoJSON w drugim argumencie Wyrażenie logiczne.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Zwraca wartość logiczną wskazującą, czy określone wyrażenie punkt lub Wielokąt GeoJSON jest prawidłowy.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Zwraca wartość JSON zawierających wartość logiczną wartość, jeśli określony punkt lub Wielokąt wyrażenie GeoJSON jest prawidłowe i nieprawidłowe, ponadto przyczyny jako wartość ciągu.</td>
</tr>
</table>

Funkcje przestrzenna może służyć do wykonywania querries odległość przestrzenna danych. Na przykład Oto kwerendę, która zwraca wszystkie dokumenty rodziny, które znajdują się w 30 km we wskazanej lokalizacji przy użyciu wbudowanych funkcji ST_DISTANCE. 

**Kwerendy**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Wyniki**

    [{
      "id": "WakefieldFamily"
    }]

Jeśli dołączysz przestrzenna indeksowania w indeksowania zasad, następnie "odległość kwerendy" będzie udostępniania efektywne za pośrednictwem indeksu. Aby uzyskać więcej informacji na temat indeksowania przestrzenna zobacz poniższą sekcję. Jeśli nie masz przestrzenna indeksu dla określonej ścieżki, można wykonywać przestrzenna zapytania, określając `x-ms-documentdb-query-enable-scan` żądanie nagłówka z wartość "prawda". W .NET można to zrobić przez przekazanie argument opcjonalny **FeedOptions** do kwerendy z [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ustawioną na wartość PRAWDA. 

ST_WITHIN może służyć do sprawdzania, jeśli punkt znajduje się w obrębie wielokąta. Często wielokąty są używane do przedstawiania ograniczenia, takie jak kody pocztowe, stan granic lub naturalne kształty. Ponownie Jeśli przestrzenna indeksowania w indeksowania zasad, następnie "w" kwerend będzie udostępniania efektywne za pośrednictwem indeks. 

Argumenty Wielokąt ST_WITHIN może zawierać tylko pojedynczy dzwonek, to znaczy wielokątami nie może zawierać dziury w nich. Sprawdzanie [DocumentDB limity](documentdb-limits.md) maksymalna liczba punktów dozwolone w wielokąta dla kwerendy ST_WITHIN.

**Kwerendy**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Wyniki**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Podobnie jak Niedopasowane typy działa w kwerendzie DocumentDB, jeśli wartość lokalizacji określona w albo argument jest nieprawidłowo lub nieprawidłowe, a następnie będzie oceniać **Nieokreślone** i szacowane dokumentu, aby pominięte z wyników kwerendy. Jeśli kwerenda zwraca żadnych wyników, uruchom ST_ISVALIDDETAILED do Debuguj Dlaczego typ spatail jest nieprawidłowy.     

ST_ISVALID i ST_ISVALIDDETAILED można sprawdzić, czy przestrzenna obiekt jest prawidłowy. Na przykład poniższa kwerenda sprawdza ważność punkt z nowym wartość szerokości zakresu (-132.8). ST_ISVALID zwraca wartość logiczną i ST_ISVALIDDETAILED zwraca wartość logiczna i ciąg zawierający powód, dlaczego jest traktowany jako nieprawidłowe.

**Kwerendy**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Wyniki**

    [{
      "$1": false
    }]

Aby sprawdzić poprawność wielokątów można także te funkcje. Na przykład, w tym miejscu firma Microsoft korzysta z ST_ISVALIDDETAILED do sprawdzania poprawności wielokąt, który nie jest zamknięty. 

**Kwerendy**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Wyniki**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Który otacza przestrzenna funkcje i składnia SQL DocumentDB. Teraz Przyjrzyjmy jak LINQ kwerenda działa i sposobu interakcji z składnią możemy zobaczył pory.

## <a name="linq-to-documentdb-sql"></a>LINQ DocumentDB SQL
LINQ to model programowania .NET, który wyraża obliczeń jako kwerendy na strumienie obiektów. DocumentDB zapewnia klientem biblioteki stronie interfejsem za pomocą LINQ przez ułatwianie konwersji między obiektami JSON i .NET i mapowanie podzbiór LINQ kwerendy do DocumentDB kwerend. 

Na poniższym obrazie przedstawiono architekturę obsługi zapytań LINQ za pomocą DocumentDB.  Korzystanie z klienta DocumentDB, deweloperzy mogą tworzyć obiekt **IQueryable** bezpośrednie kwerendy dostawcę kwerendy DocumentDB przetwarza zapytania LINQ w kwerendzie DocumentDB. Kwerenda jest następnie przekazywane do serwera DocumentDB w celu pobrania zestawu wyników w formacie JSON. Zwracane wyniki są rozszeregować na strumień obiekty .NET po stronie klienta.

![Architektura obsługi zapytań LINQ za pomocą DocumentDB - składni języka SQL, języka kwerend JSON, pojęcia bazy danych i zapytania SQL][1]
 


### <a name="net-and-json-mapping"></a>Mapowanie JSON i .NET
Mapowanie między obiekty .NET i JSON dokumentów jest naturalne — każdego pola członka danych są mapowane do obiektu JSON, gdzie nazwa pola są mapowane na "klucz" część obiektu, a część "wartość" jest lokalizacji mapowane na wartość część obiektu. Należy rozważyć poniższym przykładzie. Utworzony obiekt rodziny są mapowane do dokumentu JSON, tak jak pokazano poniżej. I odwrotnie, dokument JSON są mapowane z powrotem na obiekt programu .NET.

**C# zajęć**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ do tłumaczenia SQL
Dostawca zapytania DocumentDB wykonuje najlepsze mapowanie nakładu z LINQ zapytania do zapytania DocumentDB SQL. Następujący opis przyjęto założenie, że czytnik zawiera podstawowe znajomości LINQ.

Najpierw systemu typ obsługujemy wszystkich JSON typów pierwotnych — typów liczbowych, wartość logiczna ciągu i wartość null. Obsługiwane są następujące typy JSON. Obsługiwane są poniższe wyrażenia skalarną.

-   Wartości stałych — te zawiera stałej wartości pierwotnych typów danych w czasie, które jest obliczane kwerendy.

-   Wyrażenia indeksu tablicy i właściwości — te wyrażenia odwołać się do właściwości obiektu lub elementu tablicy.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Wyrażenia arytmetyczne - należą typowe wyrażenia arytmetyczne na wartości liczbowe i logiczne. Aby uzyskać pełną listę odwołują się do specyfikacji SQL.

        2 * family.children[0].grade;
        x + y;

-   Porównanie wyrażeniem - należą do porównywania wartości ciągu na niektórych wartość stała ciągu.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Obiekt tablica wyrażenie tworzenia — te wyrażenia zwrotu obiekt typu złożonego wartość lub anonimowych lub tablicę tych obiektów. Te wartości można zagnieżdżać.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Lista obsługiwanych LINQ operatorów
Poniżej przedstawiono listę obsługiwanych LINQ operatorów przez dostawcę LINQ dostępny w pakiecie DocumentDB .NET SDK.

-   **Wybierz pozycję**: prognozy Przetłumacz na SQL SELECT tym budowy obiektu
-   **Gdzie**: filtry Przetłumacz na SQL WHERE i obsługa techniczna tłumaczenia między & &, || i! Operatory SQL
-   **SelectMany**: umożliwia unwinding tablic do klauzuli SQL JOIN. Służy do łańcucha zagnieżdżone wyrażeń, aby odfiltrować elementów tablicy
-   **OrderBy i OrderByDescending**: przekłada się na ORDER BY rosnąco i malejąco:
-   **CompareTo**: przekłada się na zakres porównania. Często używanych ciągów, ponieważ nie jest porównywalna z .NET
-   **Sporządzanie**: przekłada się na wierzchu SQL ograniczania wyników kwerendy
-   **Funkcje matematyczne**: obsługuje tłumaczenie z. Asin moduł.liczby, Acos, w sieci, Atan ZAOKR.w.górę.matematyczne(liczba;[istotność];[tryb]) Cos Exp, powierzchnia, dziennika, Log10, Pow, ZAOKR, logowania, Sin, pierwiastek, Tan, Truncate odpowiednikiem funkcji wbudowanych SQL.
-   **Funkcje tekstowe**: obsługuje tłumaczenie z. EndsWith "concat", zawiera w sieci, IndexOf, liczba, ToLower, TrimStart, Zamień, odwrotna kolejność, TrimEnd, StartsWith, podciąg, ToUpper do odpowiednich funkcji wbudowanych SQL.
-   **Funkcji tablicowych**: obsługuje tłumaczenie z. "Concat", zawiera i liczba, aby równoważne funkcje wbudowane SQL w sieci.
-   **Funkcje rozszerzeń geograficzne**: obsługuje tłumaczenia z metod element zastępczy odległość w ramach IsValid i IsValidDetailed do odpowiedniej funkcji wbudowanych SQL.
-   **Funkcja rozszerzenia funkcja zdefiniowana przez użytkownika**: Obsługa tłumaczenia z metody element zastępczy UserDefinedFunctionProvider.Invoke do użytkownika odpowiadające im określonych funkcji.
-   **Różne**: obsługuje tłumaczenia łączonej i operatorów warunkowego. Można przetłumaczyć zawiera ciąg zawiera, ARRAY_CONTAINS lub w SQL w zależności od kontekstu.

### <a name="sql-query-operators"></a>Operatory kwerendy SQL
Oto kilka przykładów pokazujących, jak niektóre standardowe operatory zapytań LINQ przekształcić w dół do kwerendy DocumentDB.

#### <a name="select-operator"></a>Wybierz Operator
Składnia jest `input.Select(x => f(x))`, gdzie `f` jest wyrażenie skalarne.

**Wyrażenie lambda LINQ**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**Wyrażenie lambda LINQ**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**Wyrażenie lambda LINQ**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
Składnia jest `input.SelectMany(x => f(x))`, gdzie `f` jest wyrażenie skalarne, które zwraca typ kolekcji.

**Wyrażenie lambda LINQ**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Miejsce, w którym operator
Składnia jest `input.Where(x => f(x))`, gdzie `f` to wyrażenie skalarne, która zwraca wartość logiczną.

**Wyrażenie lambda LINQ**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**Wyrażenie lambda LINQ**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Złożone zapytania SQL
Operatory powyżej mogą się składać do tworzenia bardziej zaawansowanych kwerend. Ponieważ DocumentDB obsługuje zagnieżdżonych zbiorów, skład można być łączone lub zagnieżdżone.

#### <a name="concatenation"></a>Łączenie 

Składnia jest `input(.|.SelectMany())(.Select()|.Where())*`. Połączone kwerendy można uruchomić z opcjonalną `SelectMany` kwerendy obserwowane przez wielokrotność `Select` lub `Where` operatorów.


**Wyrażenie lambda LINQ**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**Wyrażenie lambda LINQ**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**Wyrażenie lambda LINQ**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**Wyrażenie lambda LINQ**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Zagnieżdżanie

Składnia jest `input.SelectMany(x=>x.Q())` w przypadku pytań `Select`, `SelectMany`, lub `Where` operatora.

W kwerendzie zagnieżdżonych wewnętrzne kwerendy zostanie zastosowany do każdy element zbioru zewnętrzne. Język, który jest ważne funkcji wewnętrzne kwerenda może dotyczyć pola elementów w kolekcji zewnętrzne, takie jak samosprzężenia.

**Wyrażenie lambda LINQ**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**Wyrażenie lambda LINQ**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**Wyrażenie lambda LINQ**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>Wykonywanie kwerend SQL
DocumentDB udostępnia zasoby za pośrednictwem interfejsu API usługi REST, który może być wywoływana przez dowolny język umożliwiający żądania HTTP/HTTPS. Ponadto DocumentDB oferuje programowania bibliotek kilku popularnych języków, takich jak .NET, Node.js, JavaScript i Python. Interfejsu API usługi REST i różnych bibliotek obsługują kwerend za pomocą języka SQL. Zestaw SDK programu .NET obsługuje LINQ kwerendy oprócz SQL.

W poniższych przykładach pokazano, jak utworzyć kwerendę i prześlij go przed DocumentDB konta bazy danych.

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST
DocumentDB oferuje Otwórz RESTful model programowania za pomocą protokołu HTTP. Czy obsługi administracyjnej konta bazy danych za pomocą subskrypcji usługi Azure. Model zasobów DocumentDB składa się z zestawów zasobów na koncie bazy danych, z których każdy jest adresowania, przy użyciu identyfikatora URI logicznej i stabilny. Zestaw zasobów jest określane jako źródło danych w tym dokumencie. Konto bazy danych zawiera zestaw baz danych, każdy zawiera wiele kolekcji, z których w Włącz każdy zawierać dokumentów, funkcji zdefiniowanych przez użytkownika i innych typów zasobów.

Model podstawowej interakcji z tych zasobów jest za pośrednictwem zleceń HTTP GET, położenie, OPUBLIKUJ i Usuń z ich interpretacji standardowy. Czasownikowe WPIS służy do tworzenia nowego zasobu, wykonywania procedury składowanej lub wydawania kwerendy DocumentDB. Kwerendy są zawsze tylko operacje odczytu bez strony skutków.

W poniższych przykładach pokazano WPIS kwerendy DocumentDB na zbiór zawierający dwa przykładowe dokumenty że firma Microsoft pory przeglądania. Kwerenda zawiera prosty filtr we właściwościach nazwy JSON. Zwróć uwagę na zastosowanie `x-ms-documentdb-isquery` i typ zawartości: `application/query+json` nagłówków do oznacza, że operacja jest kwerendy.


**Żądanie**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Wyniki**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Drugi przykład przedstawia bardziej złożonej kwerendy zwracającej wiele wyników z sprzężenia.

**Żądanie**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Wyniki**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Jeśli wyniki kwerendy nie mieści się na jednej stronie wyników, a następnie interfejsu API usługi REST zwraca token kontynuacji za pośrednictwem `x-ms-continuation-token` nagłówka odpowiedzi. Klienci mogą podzielony na strony wyników, łącznie z nagłówkiem w kolejnych wyniki. Liczba wyników na stronie można sterować także za pośrednictwem `x-ms-max-item-count` numer nagłówka.

Aby zarządzać zasadami spójności danych dla kwerendy, należy użyć `x-ms-consistency-level` nagłówka, takie jak wszystkie żądania interfejsu API usługi REST. Spójności sesji jest wymagane do również echo r `x-ms-session-token` nagłówka plików Cookie w wezwaniu kwerendy. Należy zauważyć, że zasady indeksowania kwerendy zbierania również mogą mieć wpływ spójności wyników kwerendy. Z ustawienia zasad indeksowania domyślnie dla zbiorów indeks jest zawsze bieżącego wraz z zawartością dokumentu i wyniki kwerendy będą zgodne spójności wybrany dla danych. Jeśli zasady indeksowania jest złagodzone do opóźnieniem, kwerendy może zwracać starych wyników. Więcej informacji można znaleźć w [Poziomy spójności DocumentDB] [consistency-levels].

Jeśli skonfigurowane indeksowania zasady dotyczące zbierania nie obsługuje określonej kwerendy, serwer DocumentDB zwraca 400 "nieprawidłowe żądanie". To jest zwracana dla kwerend zakres dla ścieżek skonfigurowane dla wyszukiwania mieszania (równości) i ścieżki wyraźnie wykluczone z indeksowania. `x-ms-documentdb-query-enable-scan` Nagłówka można określić, aby umożliwić zapytania, aby wykonać skanowanie, gdy indeksu nie jest dostępna.

### <a name="c-net-sdk"></a>ZESTAW SDK C# (.NET)
Zestaw SDK programu .NET obsługuje LINQ i SQL kwerendy. W poniższym przykładzie pokazano sposób wykonywania kwerend prosty filtr wprowadzone wcześniej w tym dokumencie.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


W tym przykładzie porównano dwie właściwości równości w każdym dokumencie i używa anonimowe prognozy. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Następnej pokazano sprzężenia, wyrażony za pomocą LINQ SelectMany.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Klient programu .NET automatycznie iterację wszystkich stron wyników kwerendy w blokach foreach, jak pokazano powyżej. Opcje kwerendy wprowadzone w sekcji interfejsu API usługi REST są również dostępne w użyciu .NET SDK `FeedOptions` i `FeedResponse` klas w metodzie CreateDocumentQuery. Liczba stron można sterować przy użyciu `MaxItemCount` ustawienie. 

Można również jawnie kontrolować stronicowanie, tworząc `IDocumentQueryable` za pomocą `IQueryable` obiekt, a następnie według czytania` ResponseContinuationToken` wartości i przekazywania ich ponownie jako `RequestContinuationToken` w `FeedOptions`. `EnableScanInQuery`można ustawić tak, aby włączyć skanowanie, gdy kwerenda nie są obsługiwane przez skonfigurowane zasady indeksowania. W przypadku zbiorów podzielone na partycje, można użyć `PartitionKey` w celu uruchomienia kwerendy pojedynczego partition (chociaż DocumentDB można automatycznie wyodrębniania to tekst kwerendy), i `EnableCrossPartitionQuery` uruchamianie kwerend, które mogą muszą być uruchamiane na wielu partycje. 

Zapoznaj się z [próbki DocumentDB .NET](https://github.com/Azure/azure-documentdb-net) dla próbki więcej zawierające kwerendy. 

### <a name="javascript-server-side-api"></a>JavaScript API po stronie serwera 
DocumentDB zapewnia model programowania wykonywania logiki aplikacji JavaScript podstawie bezpośrednio na zbiory, przy użyciu procedur składowanych i wyzwalaczy. Logika JavaScript zarejestrowana na poziomie zbioru następnie może wydawać operacji w bazie danych dla operacji w dokumentach danego zbioru. Te operacje są owinięty otaczającego kwasu transakcje.

Poniższym przykładzie pokazano, jak za pomocą queryDocuments na serwerze interfejsu API języka JavaScript wykonują kwerendy z wewnątrz procedur przechowywanych i wyzwalaczy.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Funkcje agregacji

Wbudowana obsługa funkcji agregujących jest w działa, ale w razie potrzeby funkcji Licznik lub Suma w tym samym czasie można uzyskać ten sam wynik, przy użyciu różnych metod.  

Na ścieżce odczytu:

- Funkcji agregujących można wykonywać pobierania danych i wykonując zestawienia lokalnie. Ma go zalecane jest użycie rzut tani zapytania, takich jak `SELECT VALUE 1` zamiast cały dokument, takich jak `SELECT * FROM c`. Pomaga to maksymalizować liczbę dokumentów przetwarzane na każdej stronie wyników, w celu uniknięcia dodatkowych przesyłania danych do usługi w razie potrzeby.
- Za pomocą procedury składowanej aby zminimalizować opóźnienia sieci w powtarzających się niepotrzebnej. Przykładowe procedura przechowywana, która oblicza liczbę elementów dla danego filtru kwerendy, zobacz [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Procedura składowana można zezwolić użytkownikom na łączenie reguł biznesowych sformatowanego oraz wykonywanie agregacji w skuteczny sposób.

Na ścieżce zapisu:

- Inny wspólnego wzorca jest wstępnie agregacji wyników w ścieżce "zapisu". Jest to szczególnie atrakcyjnego, gdy liczba żądań "więcej" jest wyższymi niż "Pisanie" żądania. Raz wstępnie zagregowane, wyniki są dostępne z jednego miejsca żądania odczytu.  Najlepszym sposobem wstępnie agregacji w DocumentDB jest Ustawianie wyzwalacza, który jest wywoływana z każdego "Pisanie" i aktualizowanie metadanych dokumentu, który zawiera najnowsze wyniki kwerendy, która jest podejmowana materialized. Na przykład znajdziesz w próbce [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) aktualizacje minSize, maxSize i totalSize dokument metadanych dla zbioru. Próbki może zostać wydłużony zaktualizować licznik, Suma itp.

##<a name="references"></a>Odwołania
1.  [Wprowadzenie do Azure DocumentDB][introduction]
2.  [Specyfikacja DocumentDB SQL](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [Przykłady DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
4.  [Poziomy spójności DocumentDB][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  Specyfikacja języka JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Kwerendy technik oceny dużych baz danych [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Przetwarzania w systemach równoległe relacyjnej bazy danych, naciśnij społeczeństwa IEEE komputera, 1994 kwerend
11. Tan lu, Ooi, przetwarzania w systemach równoległe relacyjnej bazy danych, naciśnij społeczeństwa IEEE komputera, 1994 kwerend.
12. Christopher Olston Reed Benjaminowi, Utkarsh Srivastava, Kumar Ravi, Tomkins osoby o imieniu Andrzej: łaciński świnka: nie tak obcego języka przetwarzanie danych SIGMOD 2008.
13.     G. Graefe. Ramy kaskady optymalizacji zapytania. Eng. IEEE danych Bull., 18(3): 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
