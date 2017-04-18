<properties 
    pageTitle="Odwołanie do analizy w aplikacji wniosków | Microsoft Azure" 
    description="Odwołanie do instrukcji w analizy, narzędzie zaawansowane wyszukiwanie wniosków aplikacji. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Odwołanie do analizy

[Analiza](app-insights-analytics.md) jest funkcją zaawansowane wyszukiwanie [Wniosków aplikacji](app-insights-overview.md). Te strony opisują języka kwerend analizy.

> [AZURE.NOTE] [Testowanie analizy w naszych danych symulowany](https://analytics.applicationinsights.io/demo) Jeśli aplikacji nie jest wysyłanie danych do aplikacji wniosków jeszcze.

## <a name="index"></a>Indeks


**Umożliwianie** [Umożliwianie](#let-clause)


**Kwerendy i operatorów** [Liczba](#count-operator)  |  [oceny](#evaluate-operator) | [rozszerzenie](#extend-operator) | [sprzężenia](#join-operator) | [limit](#limit-operator) | [mvexpand](#mvexpand-operator) | [analizy](#parse-operator) | [projektu](#project-operator) | [projektu — z dala od komputera](#project-away-operator) | [zakres](#range-operator) | [zmniejszyć](#reduce-operator) | [renderowania dyrektywy](#render-directive) | [Ogranicz klauzula](#restrict-clause) | [Sortowanie](#sort-operator) | [podsumowywanie](#summarize-operator) | [wykonać](#take-operator) | [górnej](#top-operator) | [zagnieżdżonych w górnej](#top-nested-operator) | [Unii](#union-operator) | [miejsce, w którym](#where-operator) | [miejsca w](#where-in-operator)

**Agregacji** [dowolny](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [Statystyka](#count) | [LICZ.Jeżeli](#countif) | [dcount](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [Percentyl](#percentile) | [percentylu](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [ODCH.standardowe](#stdev) | [Suma](#sum) | [Odchylenie](#variance)

**Wielkości skalarne** [Literałów wartości logicznych](#boolean-literals)  |  [Operatorów logicznych](#boolean-operators) | [poświaty](#casts) | [skalarne porównania](#scalar-comparisons) | [gettype](#gettype) | [mieszania](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Numery** [Operatory arytmetyczne](#arithmetic-operators)  |  [Stałe numeryczne](#numeric-literals) | [moduł.liczby](#abs) | [pojemnika](#bin) | [exp](#exp) | [powierzchnia](#floor) | [gamma](#gamma) | [dziennika](#log) | [LOS](#rand) | [pierwiastek](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Data i godzina** [Data i godzina wyrażenia](#date-and-time-expressions)  |  [Dat i godzin](#date-and-time-literals) | [temu](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [teraz](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Ciąg** [Identyfikatory GUID](#guids)  |  [Zastosowanym mieszaniem nazw literałów ciąg](#obfuscated-string-literals) | [Literałów ciąg](#string-literals) | [porównaniach ciągów znaków](#string-comparisons) | [countof](#countof) | [Wyodrębnianie](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [zamienić](#replace) | [Dzielenie](#split) | [strcat](#strcat) | [strlen](#strlen) | [podciąg](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Dynamiczne, obiektów i tablic** [Tablica i obiekt literałów](#array-and-object-literals)  |  [Funkcji dynamicznego obiekt](#dynamic-object-functions) | [dynamiczne obiektów w pozwalają klauzule](#dynamic-objects-in-let-clauses) | [wyrażeń ścieżka JSON](#json-path-expressions) | [nazwy](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [zakres](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Umożliwianie

### <a name="let-clause"></a>Umożliwianie klauzula

**Tabelaryczne umożliwić — nazewnictwa tabeli**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Wartość skalarna umożliwić — nazewnictwa wartości**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda umożliwić — nazw funkcji**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Klauzula pozwalają wiąże [nazwę](#names) tabelarycznego wynik, wartość skalarną lub funkcji. Klauzuli jest prefiks do kwerendy, a zakres powiązanie jest danej kwerendy. (Umożliwiają nie umożliwia jego nazwę czynności, które możesz użyć później podczas sesji.)

**W składni**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Typ:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Kwerenda nie poprzedzone klauzulę Zezwalaj.

**Przykłady**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Samosprzężenia:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Kwerendy i operatorów

Kwerendy na swojej telemetrycznego składa się z odwołaniem do strumień źródłowy, a po nim potok filtry. Na przykład:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Każdy filtr poprzedzone znaku kreski pionowej `|` jest wystąpieniem *operator*, z niektórych parametrów. Dane wejściowe operator są tabeli będącej wynikiem poprzedniego potoku. W większości przypadków parametry są [skalarne wyrażeń](#scalars) nad kolumnami danych wejściowych. W niektórych przypadkach parametry są nazw kolumn i wprowadzania danych, a w niektórych przypadkach parametr jest drugiej tabeli. Wynik kwerendy jest zawsze tabeli, nawet jeśli jego zawiera tylko jedną kolumnę i jeden wiersz.

Kwerendy mogą zawierać pojedynczy podziały wierszy, ale są zamykane przez pusty wiersz. Mogą zawierać komentarze między `//` i koniec wiersza.

Kwerendy może być poprzedzone co najmniej jeden [umożliwić klauzule](#let-clause), której Definiowanie wielkości skalarne, tabel lub funkcje, które mogą być używane w kwerendzie.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`jest używany w poniższych przykładach kwerendy do określenia powyższej tabeli potoku lub źródła.
> 

### <a name="count-operator"></a>operator liczba

`count` Zwraca liczbę rekordów (wierszy) w zestawie rekordów wprowadzania danych.

**W składni**

    T | count

**Argumenty**

* *T*: dane tabelaryczne, których rekordy mają zostać zliczone.

**Zwraca**

Ta funkcja zwraca tabelę zawierającą jeden rekord i kolumny typu `long`. Wartość komórki tylko jest liczby rekordów w *T*. 

**Przykład**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>Szacowanie operatora

`evaluate`jest mechanizm wewnętrzny, który umożliwia specjalistyczne algorytmy mają być dołączane do kwerend.

`evaluate`musi być ostatni podmiot w potoku kwerendy (z wyjątkiem możliwości `render`). Nie musi znajdować się w treści funkcji.

[Szacowanie autocluster](#evaluate-autocluster) | [oceny koszyk](#evaluate-basket) | [oceny diffpatterns](#evaluate-diffpatterns) | [oceny extractcolumns](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>Szacowanie autocluster

     T | evaluate autocluster()

AutoCluster znajduje typowe wzorce osobne atrybutów (wymiary) w danych i zmniejszy wyniki oryginalnej kwerendy (czy jest 100 lub 100 wierszy k) do niewielka liczba wzorców. AutoCluster opracowanym ułatwiające analizy błędów (wyjątki, awarii), ale potencjalnie może działać na dowolnym filtrowanego zestawu danych. 

**W składni**

    T | evaluate autocluster( arguments )

**Zwraca**

AutoCluster zwraca zestaw (zazwyczaj małych) wzorców, które części danych za pomocą wartości typowych udostępnionych przez wiele osobnych atrybutów. Każdy deseń jest reprezentowany przez wiersza w wynikach. 

Pierwszych dwóch kolumnach są liczba i procent wierszy z oryginalnym kwerendy, które są przechwytywane przez wzorzec. Pozostałe kolumny są z oryginalnym kwerendy i ich wartości jest określoną wartość z kolumny lub "*" oznacza wartościami zmiennych. 

Zauważ, że desenie nie są rozłączne: mogą być widoczne nakładające się i zwykle nie obejmuje wszystkie wiersze oryginalny. Niektóre wiersze nie może być w obszarze dowolny wzorzec.

**Porady**

* Używanie `where` i `project` w potoku wprowadzania danych w celu zmniejszenia dane do właśnie co Cię interesuje.
* Gdy znajdziesz interesującą wiersza, można przechodzić do jego, dodając konkretnych wartości, które usługi `where` filtru.

**Argumenty (wszystkie opcjonalne)**

* `output=all | values | minimal` 

    Format wyniki. Procent i liczba kolumn zawsze wyświetlana w wynikach. 

 * `all`-wszystkie kolumny z dane wejściowe są dane wyjściowe
 * `values`-spowoduje wyświetlenie kolumn tylko z "*" w wynikach
 * `minimal`-także spowoduje wyświetlenie kolumn, które są identyczne dla wszystkich wierszy w oryginalnej kwerendy. 


* `min_percent=`*podwójne* (domyślny: 1)

    Zakres minimalny procent wygenerowane wiersze.

    Przykład:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (domyślny: 25) 

    Liczba nasion określa liczbę punktów początkowych wyszukiwania lokalnego algorytmu. W niektórych przypadkach w zależności od struktury danych zwiększenie liczby nasion zwiększa liczbę (lub jakości) wyników za pomocą wyszukiwania zwiększenie miejsca na tempo zależnościami kwerendy. Num_seeds argument ma zmniejszenia wyniki w obu kierunków, zmniejszenie jego poniżej 5 osiągnie nieznaczne wydajności i zwiększanie powyżej 50 rzadko generują dodatkowych wzorców.

    Przykład:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < podwójne < 1*+ (domyślny: 0,5)

    Zapewnia niektórych kontrolę nad równowagi między rodzajowy (duży zakres) i znaczącą (wiele wartości udostępnionych). Zwiększanie size_weight zazwyczaj zmniejsza liczbę wzorców, a każdego wzorca zwykle obejmować większą wartość procentową. Zmniejszenie size_weight zazwyczaj tworzy bardziej szczegółowych desenie z bardziej udostępnionych wartości i mniejszy zakres wartości procentowej. W obszarze Ustawienia zaawansowane formuła jest ważona średnia geometryczna między znormalizowaną wynik ogólne i znaczącą wynik z size_weight i 1 size_weight jako wagi. 

    Przykład:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*nazwa_kolumny*

    Są uwzględnione każdego wiersza w danych wejściowych według określonej masy (domyślnie każdy wiersz ma grubości "1"), typowe zastosowania kolumny grubość ma mieć do pobierania konta lub bucketing agregacji danych, który już jest osadzony w każdym wierszu.

    Przykład:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Szacowanie koszyk

     T | evaluate basket()

Koszyk znajduje wszystkie desenie częste osobne atrybutów (wymiary) w danych i zwróci wszystkie desenie częste, które przekazywane progu częstość w pierwotnej kwerendy. Koszyk jest gwarantowana, aby znaleźć wszystkie desenie częste w serii danych, ale nie jest gwarantowana mają Wielomianowa wykonywalna. Podczas uruchamiania kwerendy jest liniowa w polu Liczba wierszy, ale w niektórych przypadkach może być wykładniczą liczby kolumn (wymiary). Koszyk jest oparty na algorytmu Apriori pierwotnie opracowanego dla wyszukiwania danych analizy koszyk. 

**Zwraca**

Wszystkie desenie znajdujących się w więcej niż określoną część (domyślnie 0,05) zdarzenia.

**Argumenty (wszystkie opcjonalne)**


* `threshold=`*0.015 < podwójne < 1* (domyślny: 0,05) 

    Ustawia minimalnego współczynnika wierszy, które mają być traktowane jako częste (desenie z mniejszy stosunek nie zostaną zwrócone).

    Przykład:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*nazwa_kolumny*

    Są uwzględnione każdego wiersza w danych wejściowych według określonej masy (domyślnie każdy wiersz ma grubości "1"), typowe zastosowania kolumny grubość ma mieć do pobierania konta lub bucketing agregacji danych, który już jest osadzony w każdym wierszu.

    Przykład: T | Szacowanie basket("weight_column=sample_Count")


* `max_dims=`*1 < int* (domyślny: 5)

    Ustawia maksymalna liczba nieskorelowane wymiarów na koszyk ograniczone przez domyślne, aby zmniejszyć wykonywania kwerend.


* `output=minimize` | `all` 

    Format wyniki. Procent i liczba kolumn zawsze wyświetlana w wynikach.

 * `minimize`-spowoduje wyświetlenie kolumn tylko z "*" w wynikach.
 * `all`-wszystkie kolumny z dane wejściowe są dane wyjściowe.




#### <a name="evaluate-diffpatterns"></a>Szacowanie diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns porównuje dwa zestawy danych tę samą strukturę i umożliwia znalezienie wyrazów desenie osobne atrybutów (wymiary), które charakteryzującą różnice między dwoma zestawami danych. Diffpatterns opracowanym ułatwiające analizy błędów (np. przez porównanie błędy nie błędów w określonym przedziale czasu), ale potencjalnie można znaleźć różnice między dowolne dwa zestawy danych tę samą strukturę. 

**W składni**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Zwraca**

Diffpatterns zwraca zestaw (zwykle mały) przez Przechwytywanie różnych części danych w dwóch zbiorach (to znaczy deseń przechwytywania duży procent wierszy w pierwszym zestawie danych i niskiej procent wierszy w zestawie drugiego). Każdy deseń jest reprezentowany przez wiersza w wynikach.

Pierwsze cztery są liczba i procent wierszy z oryginalnym kwerendy, które są przechwytywane, deseniu w każdym zestawie, piątej kolumnie różni się (w punktach wartości procentowej) między dwoma zestawami. Pozostałe kolumny są z oryginalnym kwerendy i ich wartości jest określoną wartość z kolumny lub * co oznacza wartościami zmiennych. 

Zauważ, że desenie nie są różne: mogą być widoczne nakładające się i zwykle nie obejmuje wszystkie wiersze oryginalny. Niektóre wiersze nie może być w obszarze dowolny wzorzec.

**Porady**

* Używanie miejsca i program project w potoku wprowadzania danych w celu zmniejszenia dane do właśnie co Cię interesuje.

* Gdy znajdziesz interesującą wiersza, można przechodzić do jego przez dodanie wartości określonych do filtrowania z miejsca.

**Argumenty**

* `split=`*Nazwa kolumny* (wymagany)

    Kolumna musi zawierać dokładnie dwie wartości. W razie potrzeby utwórz takiej kolumny:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*ciąg*

    Wyrażenie informuje algorytm są wyszukiwane tylko wzorców, które mają wyższą wartość procentową w zestawie danych docelowej, docelowej należy wybrać jedną z dwóch wartości Podziel kolumny.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < podwójne < 1* (domyślny: 0,05) 

    Ustawia minimalnego deseniu (wskaźnik) różnicę między dwoma zestawami.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Format wyniki. Procent i liczba kolumn zawsze wyświetlana w wynikach. 

 * `minimize`-spowoduje wyświetlenie kolumn tylko z "*" w wynikach
 * `all`-wszystkie kolumny z dane wejściowe są dane wyjściowe

* `weight_column=`*nazwa_kolumny*

    Są uwzględnione każdego wiersza w danych wejściowych według określonej masy (domyślnie każdy wiersz ma grubości "1"). Zazwyczaj kolumny wagi jest używane w celu uwzględnienia próbki konta lub bucketing agregacji danych, który już jest osadzony w każdym wierszu.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>Szacowanie extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns jest używana do wzbogacanie tabeli z wieloma kolumnami proste, które są dynamicznie wyodrębnionej poza (półprzezroczysty) strukturalnych kolumn na podstawie ich typu. Obecnie obsługuje tylko kolumny json obu dynamiczne i ciąg szeregowania jsons.


* `max_columns=`*int* (domyślny: 10) 

    Liczba nowych kolumn dodano jest dynamiczny i może być bardzo duży (faktycznie jest liczba unikatowych kluczy we wszystkich rekordach json), możemy należy ograniczyć go. Nowe kolumny są sortowane w kolejności malejącej według ich częstotliwość i maksymalnie max_columns zostaną dodane do tabeli.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*podwójne* (domyślny: 10.0) 

    Innym sposobem zawęża ignoruje kolumn, którego częstotliwość jest mniejsza niż min_percent nowe kolumny.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*wartość logiczna* (domyślny: PRAWDA) 

    Jeśli ma wartość PRAWDA nazwę kolumny złożone zostaną dodane jako prefiksu do nazw kolumn wyodrębnionej.


* `prefix_delimiter=`*ciąg* (domyślny: "_") 

    Jeśli add_prefix = true ten parametr określa ogranicznik, który będzie używany do łączenia nazwy nowych kolumn.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*wartość logiczna* (domyślny: FAŁSZ) 

    Jeśli ma wartość PRAWDA oryginalne kolumny (json) zostaną zachowane w tabeli wyników.


* `output=query | table` 

    Format wyniki. 

 * `table`-Wynik jest tę samą tabelę odebrana minus określonych kolumn wprowadzania oraz nowych kolumn, które zostały wyodrębnione z wprowadzania kolumn.
 * `query`-Wynik jest ciąg reprezentujący kwerendy, który spowodowałby uzyskać wynik w postaci tabeli. 




### <a name="extend-operator"></a>Rozszerzanie operatora

     T | extend duration = stopTime - startTime

Jeden lub więcej kolumn obliczeniowych można dołączyć do tabeli. 


**W składni**

    T | extend ColumnName = Expression [, ...]

**Argumenty**

* *T:* Tabela wejściowa.
* *ColumnName:* Nazwa kolumny do dodania. [Nazwy](#names) jest uwzględniana wielkość liter i mogą zawierać litery, znaki liczbowe lub "_". Używanie `['...']` lub `["..."]` do oferty słowa kluczowe lub nazw o innych znaków.
* *Wyrażenie:* Obliczanie na istniejących kolumn.

**Zwraca**

Kopia wprowadzania danych tabeli, w której dodatkowe określonych kolumn.

**Porady**

* Użyj [`project`](#project-operator) , jeśli chcesz także usuwanie lub zmienianie nazwy niektórych kolumn.
* Nie używaj `extend` po prostu uzyskanie krótszej wpisz nazwę w wyrażeniu długa. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Natywne kolumny w tabeli zostały zindeksowane; Nowa nazwa określa dodatkowej kolumny, która nie jest indeksowane, aby kwerenda prawdopodobnie może działać wolniej.

**Przykład**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>operator sprzężenia

    Table1 | join (Table2) on CommonColumn

Scala wiersze dwóch tabel przy pasujące wartości określonej kolumny.


**W składni**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumenty**

* *Tabela1* - "po lewej stronie" sprzężenia.
* *Tabela2* - "prawej" sprzężenia. Może być w wyrażeniu zagnieżdżonych kwerendy, która wyświetla tabeli.
* *CommonColumn* - kolumny, która ma taką samą nazwę w dwóch tabelach.
* *Rodzaju* - Określa, jak mają odpowiadać wierszy z dwóch tabel.

**Zwraca**

Tabela zawierająca:

* Kolumna, dla każdej kolumny w każdym z dwóch tabel, w tym zgodne kluczy. W przypadku kolizji nazwę kolumny po prawej stronie zostanie automatycznie zmieniona.
* Wiersz dla każdej dopasowanie między tabelami wprowadzania danych. Dopasowanie to wiersz zaznaczony z jednej tabeli, który ma taką samą wartość dla wszystkich `on` pola w formie wiersza w innej tabeli. 

* `Kind`nieokreślone

    Tylko jeden wiersz z lewej strony jest takie samo dla każdej wartości `on` klucz. Wynik zawiera wiersz dla każdego Dopasuj wiersz z wierszy z prawej strony.

* `Kind=inner`
 
     Wynik dla każdej kombinacji dopasowanych wierszy z lewej i prawej strony to wiersz.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Oprócz wewnętrzne dopasowania zawiera wiersza dla każdego wiersza w lewo (lub prawo), nawet jeśli został nie. W takim przypadku komórki niedopasowane dane wyjściowe zawierają wartości null.

* `kind=leftanti`

     Zwraca wszystkie rekordy z lewej strony, które nie mają wartości zgodne z prawej strony. Wynikowa tabela zawiera tylko kolumny z lewej strony. 
 
Jeśli istnieje kilka wierszy z tych samych wartości dla tych pól, zostanie wyświetlony wierszy dla wszystkich kombinacji.

**Porady**

Aby uzyskać optymalną wydajność:

* Używanie `where` i `project` w celu zmniejszenia liczby wierszy i kolumn w tabeli wprowadzania przed `join`. 
* Jeśli jedna tabela jest zawsze mniejsza niż drugi, można go używać jako po lewej stronie (gazociągami) sprzężenia.
* Kolumny dopasowania sprzężenia muszą mieć taką samą nazwę. Jeśli to konieczne zmienić nazwę kolumny w jednej z tabel, należy użyć operatora projektu.

**Przykład**

Uzyskiwanie rozszerzyć działań z dziennika, w których niektóre pozycje Oznacz rozpoczęcia i zakończenia działania. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>limit operator

     T | limit 5

Zwraca do określonej liczby wierszy z tabeli wprowadzania danych. Nie jest gwarantowana zwracanych rekordów. (Aby zwrócić określonych rekordów, użyj [`top`](#top-operator).)

**Alias**`take`

**W składni**

    T | limit NumberOfRows


**Porady**

`Take`jest proste i skuteczne sposobem Zobacz próbki wyników podczas pracy interakcyjnie. Należy pamiętać, że nie gwarantuje warzywa żadnych określonych wierszy lub przygotowywania ich w określonej kolejności.

Istnieje niejawne limit liczby wierszy zwracanych do klienta, nawet jeśli nie używasz `take`. Aby podnieść ten limit, należy użyć `notruncation` opcja żądania klienta.



### <a name="mvexpand-operator"></a>mvexpand operator

    T | mvexpand listColumn 

Rozwija listy z komórki (JSON) wpisany dynamiczne tak, aby każdy wpis ma osobnym wierszu. Wszystkie komórki w wierszu rozwiniętej są zduplikowane. 

(Zobacz też [`summarize makelist`](#summarize-operator) której działa przeciwne.)

**Przykład**

Załóżmy, że tabela wejściowa jest:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Witaj"|{"klucz": "wartość"}|
|2|"świecie"|[0,1, "k", "v"]|

    mvexpand D

Wynik jest następujący:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Witaj"|{"klucz": "wartość"}|
|2|"świecie"|0|
|2|"świecie"|1|
|2|"świata"|"k"|
|2|"świecie"|"v"|


**W składni**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumenty**

* *ColumnName:* W wyniku tablice w kolumnie nazwanych są rozwinięte w wielu wierszach. 
* *ArrayExpression:* Wyrażenie otrzymania tablicy. Jeśli ten formularz służy, dodaje się nową kolumnę i istniejący są zachowywane.
* *Nazwa:* Nazwa nowej kolumny.
* *Typename:* Rzutuje rozwiniętej wyrażenia określonego typu
* *RowLimit:* Maksymalna liczba wierszy wynikiem każdy wiersz oryginalnej. Wartość domyślna to 128.

**Zwraca**

Wiele wierszy dla każdej wartości w tablicy w kolumnie nazwanych lub w wyrażeniu tablicy.

Rozwinięty kolumny ma zawsze typ dynamiczne. Użyj rzutu, takich jak `todatetime()` lub `toint()` Aby obliczyć lub agregowanie wartości.

Obsługiwane są dwa tryby rozszerzenia zbiór właściwości:

* `bagexpansion=bag`: Zbiory właściwości są rozwinięte do zbiory wprowadzania pojedynczego właściwości. Jest to domyślne rozszerzenia.
* `bagexpansion=array`: Zbiory właściwości są rozwinięte do elementu dwa `[` *klucz*`,`*wartość* `]` tablicy struktury, jednolitego dostępu do kluczy i wartości (a także, na przykład z agregacji wartości odrębnych w nazwy właściwości). 

**Przykłady**


    exceptions | take 1 
  	| mvexpand details[0]

Dzieli rekord wyjątku na wiersze dla każdego elementu w polu Szczegóły.



### <a name="parse-operator"></a>operator analizy

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Pobiera wartości z ciągu. Za pomocą prostego lub zwykła wyrażenia.

**W składni**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumenty**

* `T`: Tabela wejściowa.
* `kind`: 
 * `simple`(ustawienie domyślne): `Match` ciągów są ciągami zwykły.
 * `relaxed`: Jeśli tekst nie analizować jako typ kolumny, kolumna jest ustawiona na wartość null i analizy będzie nadal występował 
 * `regex`: `Match` ciągi są wyrażeń regularnych.
* `Text`: Kolumny lub inne wyrażenie, którego wynikiem lub można konwertować ciągu.
* *Dopasowania:* Uwzględnij Następna część ciągu i odrzucić go.
* *Kolumny:* Następna część ciągu Przypisz do tej kolumny. Jeśli nie istnieje, zostanie utworzony kolumny.
* *Typ:* Analizowanie Następna część ciągu jako określonego typu, takich jak int, Data podwójny. 


**Zwraca**

Tabela wejściowa, rozszerzonego zgodnie z listy kolumn.

Elementy znajdujące się w `with` klauzula są dopasowywane źródłowego tekstu z kolei. Każdy element chews poza fragment tekstu źródła: 

* Literał ciągu lub wyrażenie przenosi pasujące kursor długością dopasowania.
* W analizy regex wyrażeń regularnych można użyć operatora zmniejszenie "?" możliwie jak przenieść Uwzględnij następujące.
* Nazwa kolumny z typem analizuje tekst jako określonego typu. Jeśli typ = pasujące do całego wzorca obniżone, unieważnia niepowodzenie analizy.
* Nazwa kolumny bez typu lub typu "string" kopiuje najmniejszej liczby znaków, aby uzyskać dostęp do następujących dopasowanie.
* " *" Pomija najmniejszej liczby znaków, aby uzyskać dostęp do następujących dopasowanie. Możesz użyć "*" na początku i końcu wzorca, lub po typu innego niż ciąg lub między pasujące ciągi.

Wszystkie elementy we wzorcu analizy musi odpowiadać poprawnie; w przeciwnym razie zostaną utworzone żadne wyniki. Wyjątkiem od tej reguły występuje wtedy, gdy rodzaju = obniżone, jeśli analizowania zmienną określonego kończy się niepowodzeniem, nadal pozostałą część analizy.

**Przykłady**

*Prosty:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Licznik | Prezentuj | Rok
---|---|---|---
1 | 2 | SOCKS | 63

*Złagodzone:*

Jeśli dane wejściowe zawiera poprawny dopasowanie dla każdej kolumny wpisany, obniżone analizy jest taki sam jako prosty analizy. Jednak jeśli jedna z wpisanym kolumn nie analizować poprawnie, obniżone analizy będzie nadal procesu rest deseniu, prostych analizy przestaje i kończy się niepowodzeniem wygenerować wyniki.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | Prezentuj | Rok
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

zasób | wycinek | blokady | Zwolnij | Poprzedni
---|---|---|---|---
Harmonogram | 16 | 2016-02-17 08:41:00 | 2016-02-17 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>operator projektu

    T | project cost=price*quantity, price

Zaznacz kolumny, które chcesz uwzględnić oraz zmienianie i usuwanie i wstawić nowe kolumny obliczanej. Kolejność kolumn w wyniku jest określona przez kolejność argumentów. Tylko do kolumn określonych w argumentach znajdują się w wyniku: innych w danych wejściowych są usuwane.  (Zobacz też `extend`.)


**W składni**

    T | project ColumnName [= Expression] [, ...]

**Argumenty**

* *T:* Tabela wejściowa.
* *ColumnName:* Nazwa kolumny są wyświetlane w danych wyjściowych. W przypadku nie ma *wyrażenia*, kolumna o tej nazwie musi wyświetlana w danych wejściowych. [Nazwy](#names) jest uwzględniana wielkość liter i mogą zawierać litery, znaki liczbowe lub "_". Używanie `['...']` lub `["..."]` do oferty słowa kluczowe lub nazw o innych znaków.
* *Wyrażenie:* Opcjonalnie wyrażenie skalarne, odwoływanie się do wprowadzania kolumn. 

    Jest dozwolone jest zwracana nowe kolumny obliczeniowej o takiej samej nazwie jak istniejącej kolumny w danych wejściowych.

**Zwraca**

Tabelę, która zawiera kolumny o nazwie jako argumenty, a jako wiele wierszy jako tabela wejściowa.

**Przykład**

W poniższym przykładzie pokazano kilka rodzajów operacje, które można wykonywać przy użyciu `project` operatora. Tabela wejściowa `T` zawiera trzy kolumny Typ `int`: `A`, `B`, i `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>operator projektu — z dala od komputera

    T | project-away column1, column2, ...

Wykluczanie określonych kolumn. Wynik zawiera wszystkie wprowadzania kolumny z wyjątkiem tych, których nazwy.

### <a name="range-operator"></a>operator zakresu

    range LastWeek from ago(7d) to now() step 1d

Umożliwia generowanie jednokolumnową tabelę wartości. Zwróć uwagę, że nie ma w potoku wprowadzania. 

|LastWeek|
|---|
|09:10:04.627 2015-12-05|
|09:10:04.627 2015-12-06|
|...|
|09:10:04.627 2015-12-12|



**W składni**

    range ColumnName from Start to Stop step Step

**Argumenty**

* *ColumnName:* Nazwa pojedynczej kolumny w tabeli wyników.
* *Start:* Najmniejsza wartość w danych wyjściowych.
* *Zatrzymać:* Największą wartość generowany w wyniku (lub powiązanych na największą wartość, jeśli *krok* kroki przez tę wartość).
* *Kroku:* Różnica między dwiema wartościami następujących po sobie. 

Argumenty muszą być wartości liczbowych, dat lub przedziału czasu. Nie mogą odwoływać kolumn tabeli. (Jeśli chcesz obliczyć zakres oparta na tabeli wprowadzania danych, za pomocą [zakres, *Funkcja*](#range), być może [mvexpand operator](#mvexpand-operator).) 

**Zwraca**

Tabela zawierająca jedną kolumnę o nazwie *ColumnName*, którego wartości znajdują się *rozpocząć*, *Uruchom* + *krok*..., łącznie z *zatrzymać*.

**Przykład**  

```AIQL
range Steps from 1 to 8 step 3
```

Tabela zawierająca jedną kolumnę o nazwie `Steps` o typie `long` i którego wartości są `1`, `4`, i `7`.

**Przykład**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Tabela północy w ciągu ostatnich siedmiu dni. Funkcja pojemnika (powierzchnia) zmniejsza zawsze na początek dnia.

**Przykład**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Przedstawia sposób, w jaki `range` operator służy do tworzenia małych, ad hoc, wymiarów tabeli, która następnie jest używany do wprowadzenia zera, w których dane źródłowe nie ma wartości.

### <a name="reduce-operator"></a>zmniejszanie operatora

    exceptions | reduce by outerMessage

Próbuje zgrupować podobne rekordy. Dla każdej grupy, operator Wyświetla `Pattern` uznaje, najlepiej opisuje tej grupy i `Count` rekordów w tej grupie.


![](./media/app-insights-analytics-reference/reduce.png)

**W składni**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumenty**

* *ColumnName:* Kolumna przyjrzenie się. Musi to być typu ciąg.
* *Próg:* Wartość z zakresu {od 0 do 1}. Domyślnie jest 0,001. Dla dużych nakładów próg powinna być niewielka. 

**Zwraca**

Dwie kolumny, `Pattern` i `Count`. W większości przypadków deseniem będzie pełną wartość z kolumny. W niektórych przypadkach może zidentyfikować typowe terminy i zastąpić zmiennej części z "*".

Na przykład wynik `reduce by city` mogą zawierać: 

|Wzór | Liczba |
|---|---|
| SAN * | 5182 |
| Saint * | 2846 |
| Moskwa | 3726 |
| \*-on-\* | 2730 |
| Paryż | 27163 |


### <a name="render-directive"></a>Renderowanie dyrektywy

    T | render [ table | timechart  | barchart | piechart ]

Renderowanie kieruje warstwy prezentacji sposobu pokazywania tabeli. Powinno być ostatnim elementem potoku. Jest wygodnym sposobem za pomocą kontrolek na ekranie, co pozwala zapisać kwerendę za pomocą metody określonej prezentacji.

### <a name="restrict-clause"></a>Ograniczanie klauzula 

Określa zestaw nazw tabel dostępnych dla operatorów, które należy wykonać. Na przykład:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>operator sortowania 

    T | sort by country asc, price desc

Sortowanie wierszy tabeli wprowadzania w kolejności według jednej lub więcej kolumn.

**Alias**`order`

**W składni**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumenty**

* *T:* Tabela wprowadzania do sortowania.
* *Kolumny:* Kolumna *T* , według której chcesz sortować. Typ wartości musi być numeryczne, daty, godziny lub ciąg.
* `asc`Sortowanie według w kolejności rosnącej, małych wysoki. Wartość domyślna to `desc`, malejąco wysoki, niski.

**Przykład**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Wszystkie wiersze w tabeli śledzenia, które mają określonego `ActivityId`, posortowane według ich sygnatura czasowa.

### <a name="summarize-operator"></a>Podsumowywanie operatora

Tworzy tabelę, która agreguje zawartość Tabela wejściowa.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Tabela, która jest wyświetlana liczba, czas trwania żądania średnia i zestawem miast w każdym kraju. Wynik każdego kraju, odrębnych jest wiersza. Kolumny wyjściowe umożliwia wyświetlenie liczby, Średni czas trwania, miasta i kraju. Wszystkie pozostałe kolumny wprowadzania są ignorowane.


    T | summarize count() by price_range=bin(price, 10.0)

Tabela zawierająca, ile elementów ma ceny w każdym przedziale [0,10.0], [10.0,20.0] i tak dalej. W tym przykładzie ma kolumnę dla liczby i jedną dla zakresu ceny. Wszystkie pozostałe kolumny wprowadzania są ignorowane.


**W składni**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumenty**

* *Kolumny:* Opcjonalnie nazwę kolumny wyników. Domyślnie nazwę pochodzące z wyrażenia. [Nazwy](#names) jest uwzględniana wielkość liter i mogą zawierać litery, znaki liczbowe lub "_". Używanie `['...']` lub `["..."]` do oferty słowa kluczowe lub nazw o innych znaków.
* *Agregacji:* Połączenie do agregacji, takie jak `count()` lub `avg()`, z nazwami kolumn jako argumenty. Zobacz [agregacji](#aggregations).
* *GroupExpression:* Wyrażenie nad kolumnami, która zapewnia zestaw unikatowych wartości. Zazwyczaj jest albo nazwę kolumny, która już zawiera ograniczonego zestawu wartości, lub `bin()` z kolumną liczbowe dat lub godzin, jako argumentu. 

Jeśli podasz wyrażenie liczbowe dat lub godzin bez użycia `bin()`, analizy automatycznie stosuje go interwał `1h` dla godzin, lub `1.0` dla liczb.

Jeśli nie podasz *GroupExpression,* całej tabeli znajduje się w wierszu pojedynczy wynik.



**Zwraca**

Wprowadzania wiersze są ułożone w grupach o tych samych wartości z `by` wyrażeń. Następnie funkcje agregacji określonej są obliczane dla każdej grupy, produkcji wiersz dla każdej grupy. Wynik zawiera `by` kolumny, a także co najmniej jedną kolumnę dla każdego obliczona agregacji. (Niektóre funkcje agregacji zwracają wiele kolumn).

Wynik ma dowolną liczbę wierszy są różne kombinacje `by` wartości. Jeśli chcesz podsumować zakresach wartości liczbowych, za pomocą `bin()` w celu zmniejszenia zakresy w osobnych wartości.

**Uwaga**

Mimo że dowolnego wyrażenia umożliwiają agregacji i wyrażenia grupowania, jest bardziej efektywne używanie nazw kolumn prosty lub stosowanie `bin()` do kolumny liczbowe.



### <a name="take-operator"></a>sporządzanie operatora

Alias [limitu](#limit-operator)


### <a name="top-operator"></a>operatora góry

    T | top 5 by Name desc nulls first

Zwraca pierwszą rekordy *N* posortowane według określonych kolumn.


**W składni**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumenty**

* *NumberOfRows:* Liczba wierszy *T* , aby zwrócić.
* *Sort_expression:* Wyrażenie, według której chcesz sortować wiersze. Zwykle jest tylko nazwę kolumny. Można określić więcej niż jeden sort_expression.
* `asc`lub `desc` (ustawienie domyślne) może pojawić się do formantu zaznaczenia czy rzeczywiście z "dołu" lub "góry" zakresu.
* `nulls first`lub `nulls last` kontrolek miejsce, w którym są wyświetlane wartości null. `First`to ustawienie domyślne dla `asc`, `last` to ustawienie domyślne dla `desc`.


**Porady**

`top 5 by name`odpowiada pozornie `sort by name | take 5`. Jednak działa szybciej i zawsze zwraca sortowania wyników, dlatego `take` sprawia, że ma takiej gwarancji.

### <a name="top-nested-operator"></a>operator zagnieżdżone góry

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Tworzy hierarchicznej wyniki, w przypadku przechodzenia z poprzedniego poziomu każdego poziomu. Okazuje się przydatne do odpowiedzi na pytania, które dźwięk, takich jak "co to są Najczęstsze żądania 5, a dla każdego z nich, co to są pakiety najwyższą wydajność 3 oraz dla każdego z nich, które są krajami 3 pierwszych żądania pochodzą z?"

**W składni**

   T | zagnieżdżone góry N z kolumny, według agregacji [,...]

**Argumenty**

* N:int — liczba wierszy do zwrotu lub przejście do następnego poziomu. W kwerendzie z trzema poziomami, gdzie N jest 5, 3 i 3 całkowitej liczby wierszy będą 45.
* Wykres KOLUMNOWY — kolumny Grupuj według agregacji. 
* AGREGACJI - [agregacji](#aggregations) , aby zastosować do każdej grupy wierszy. Wyniki tych agregacji określa górny grupy mają być wyświetlane.


### <a name="union-operator"></a>operator składania

     Table1 | union Table2, Table3

Przyjmuje dwie lub więcej tabel i zwraca wierszy z nich. 

**W składni**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumenty**

* *Tabela1*, *tabela2* ...
 *  Nazwa tabeli, takich jak `requests`, lub tabeli zdefiniowane w [klauzuli umożliwić](#let-clause); lub
 *  A kwerendy wyrażenie, takie jak`(requests | where success=="True")`
 *  Zestaw tabel określonych przy użyciu symboli wieloznacznych. Na przykład `e*` czy formularz sumę wszystkich tabel określonych w poprzednich klauzule pozwalają rozpoczął się o nazwie "e", razem z tabeli "wyjątków".
* `kind`: 
 * `inner`-Wynik ma podzbiór kolumn, które są wspólne dla wszystkich tabel wejściowych.
 * `outer`-Wynik ma wszystkich kolumn, które występują w innych danych wejściowych. Komórki, które nie zostały zdefiniowane przez wprowadzania wierszy są ustawione na wartość `null`.
* `withsource=`*ColumnName:* Jeśli określone, wynik będzie zawierać kolumnę o nazwie *ColumnName* , której wartość wskazuje, które tabeli źródłowej jest użytkownikiem każdego wiersza.

**Zwraca**

Tabela zawierająca tyle wierszy, ponieważ we wszystkich tabelach wprowadzania i tylu kolumn, ile jest nazw kolumn unikatowych w danych wejściowych.

**Przykład**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Union wszystkie tabele, których nazwy zaczynają się "tt".


**Przykład**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Liczba unikatowych użytkowników, oferowanych albo `exceptions` zdarzenia lub `traces` zdarzeń w ciągu ostatnich dnia. W wyniku kolumnę "elementu SourceTable" wskaże "Kwerenda" lub "Polecenie".

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Ta wersja efektywniejsze daje taki sam wynik. Przed utworzeniem Unii filtruje każdej tabeli.

### <a name="where-operator"></a>miejsce, w którym operator

     requests | where resultCode==200

Umożliwia filtrowanie tabeli do podzbioru wierszy spełniających predykatu.

**Alias**`filter`

**W składni**

    T | where Predicate

**Argumenty**

* *T:* Tabelaryczne dane wejściowe, których rekordy mają być filtrowane.
* *Predykatu:* A `boolean` [wyrażenie](#boolean) nad kolumnami *T*. Zostanie ona potraktowana dla każdego wiersza w *T*.

**Zwraca**

Wiersze w *T* , dla której *predykatu* jest `true`.

**Porady**

Aby uzyskać największą wydajność:

* **Użyj prostych porównań** między nazw kolumn i stałe. ("Stałe" oznacza stałą na tabeli — tak `now()` i `ago()` są prawidłowe, a więc wartości skalarne przypisuje się przy użyciu [ `let` klauzuli](#let-clause).)

    Na przykład woli `where Timestamp >= ago(1d)` do `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest postanowienia dotyczące pierwszego**: Jeśli masz wiele klauzul conjoined z `and`, najpierw umieść klauzule, które wymagają tylko jedną kolumnę. Dlatego `Timestamp > ago(1d) and OpId == EventId` jest większa niż na odwrót.


**Przykład**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Rekordy, które nie starszych niż 1 godzina i pochodzić z źródła o nazwie "Kuskus", a masz dwie kolumny o tej samej wartości. 

Zwróć uwagę, że firma Microsoft wprowadzane porównania dwóch kolumn, nie są używane indeks i wymusza skanowanie.


### <a name="where-in-operator"></a>gdzie w operatora

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**W składni**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumenty**

* `col`: Kolumny w tabeli.
* `expr1`...: Listy skalarne wyrażeń.

Używanie `in` jest używana do uwzględnienia tylko wiersze, w którym `col` jest równe jedno z wyrażeń `expr1...`.

Używanie `!in` aby uwzględnić tylko wiersze, w którym `col` nie jest równe jedno z wyrażeń `expr1...`.  


## <a name="aggregations"></a>Agregacji

Agregacje są funkcje używane do łączenia wartości w grupy utworzone w [podsumowywanie operacji](#summarize-operator). Na przykład w tej kwerendzie dcount() to funkcja agregacji:

    requests | summarize dcount(name) by success

### <a name="any"></a>dowolny 

    any(Expression)

Losowo wybiera jeden wiersz grupy i zwraca wartość określonego wyrażenia.

Jest to użyteczne, na przykład w przypadku niektórych kolumn ma dużej liczby wartości podobne (np kolumnę "tekst błędu") i mają zostać pobrane próbki danej kolumnie raz unikatowe wartości klucza złożonego grupy. 

**Przykład**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

W grupie, która minimalizuje maksymalizacja *ExprToMaximize*, a następnie zwraca wartość argumentu *ExprToReturn* umożliwia znalezienie wiersza (lub `*` zwraca cały wiersz).

**Porada**: nazwy kolumn przekazane przez automatycznie zostaną zmienione. Aby upewnić się, używasz prawo imiona i nazwiska, sprawdzanie wyników za pomocą `take 5` przed potoku wyników do innego operatora.

**Przykłady**

Dla każdej nazwy żądania Pokaż wystąpienia najdłuższej żądanie:

    requests | summarize argmax(duration, timestamp) by name

Pokaż wszystkie szczegóły najdłuższej żądanie, nie tylko sygnatura czasowa:

    requests | summarize argmax(duration, *) by name


Znajdź najniższej wartości poszczególnych metryki, razem z jego sygnatury czasowej i innych danych:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Średnia

    avg(Expression)

Oblicza średnią *wyrażenia* w grupie.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Zwraca minimalnego schematu, w którym dopuszcza wszystkie wartości *DynamicExpression*. 

Typ kolumny parametru musi być `dynamic` -torba tablica lub właściwości. 

**Przykład**

    exceptions | summarize buildschema(details)

Wynik:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Należy zauważyć, że `indexer` jest używany do oznaczania, gdzie należy używać indeksu liczbowego. Ten schemat niektórych prawidłowe ścieżki może być (zakładając, że indeksy te przykład znajdują się w zakresie):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Przykład**

Załóżmy, że wprowadzania kolumna zawiera trzy wartości dynamiczne:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Wynikowa schematu mogą być następujące:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Schemat zawiera nam który:

* Obiekt główny jest kontenerem z czterech właściwości o nazwie x, y i z t.
* Właściwość o nazwie "x", który może być typu "int" lub typu "ciąg".
* Właściwość o nazwie "y", który może albo typu "double" lub innego kontenera z właściwością o nazwie "w" typu "ciąg".
* ``indexer`` Słowa kluczowego wskazuje, że "z" i "t" tablic.
* Każdy element w tablicy "z" jest int lub ciąg.
* "t" jest tablica ciągów.
* Dla każdej właściwości jest niejawnie opcjonalne, a wszystkie Tablica może być puste.

##### <a name="schema-model"></a>Modelu schematu

Składnia schematu, zwracany jest następująca:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Są równoważne podzbiór adnotacje typ maszynowym, kodowane jako wartość dynamiczną. Maszynowym będzie w schemacie przykładzie:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Liczba

    count([ Predicate ])

Zwraca liczbę wierszy, dla których *predykatu* wynikiem `true`. Jeśli określono nie *predykatu* zwraca całkowitą liczbę rekordów w grupie. 

**Porada wydajności**: za pomocą `summarize count(filter)` zamiast`where filter | summarize count()`

> [AZURE.NOTE] Unikaj używania count(), aby znaleźć numer żądania, wyjątki lub innych zdarzeń, które wystąpiły. Podczas [pobierania](app-insights-sampling.md) jest operacja, liczba punktów danych w aplikacji wniosków będzie mniejsza niż liczba zdarzeń oryginalny. Użyj zamiast tego `summarize sum(itemCount)...`. Właściwość: itemCount odzwierciedla liczbę oryginalny zdarzenia, które są one reprezentowane przez każdego punktu danych podzielonego.

### <a name="countif"></a>LICZ.Jeżeli

    countif(Predicate)

Zwraca liczbę wierszy, dla których *predykatu* wynikiem `true`.

**Porada wydajności**: za pomocą `summarize countif(filter)` zamiast`where filter | summarize count()`

> [AZURE.NOTE] Nie korzystaj z funkcji LICZ znaleźć numer żądania, wyjątki lub innych zdarzeń, które wystąpiły. Podczas [pobierania](app-insights-sampling.md) jest operacja, liczbę punktów danych jest mniejsza niż liczba rzeczywista zdarzeń. Użyj zamiast tego `summarize sum(itemCount)...`. Właściwość: itemCount odzwierciedla liczbę oryginalny zdarzenia, które są one reprezentowane przez każdego punktu danych podzielonego.

### <a name="dcount"></a>DCount

    dcount( Expression [ ,  Accuracy ])

Zwraca szacowana liczba różnych wartości *wyrażenia* w tej grupie. (Aby wyświetlić listę unikatowych wartości, należy użyć [`makeset`](#makeset).)

*Dokładność*, jeśli określony, kontrolki równowagi między szybkości i dokładności.

 * `0`= najmniej dokładne i najszybszy obliczeń.
 * `1`Wartość domyślna, którą salda czasu dokładność i obliczeń; informacje o błędzie 0,8%.
 * `2`= najbardziej dokładne i najmniejszą obliczeń; informacji na temat błędów 0,4%.

**Przykład**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Zwraca szacowana liczba różnych wartości *wyrażenie* wiersze w grupie, dla których *predykatu* ma wartość PRAWDA. (Aby wyświetlić listę unikatowych wartości, należy użyć [`makeset`](#makeset).)

*Dokładność*, jeśli określony, kontrolki równowagi między szybkości i dokładności.

 * `0`= najmniej dokładne i najszybszy obliczeń.
 * `1`Wartość domyślna, którą salda czasu dokładność i obliczeń; informacje o błędzie 0,8%.
 * `2`= najbardziej dokładne i najmniejszą obliczeń; informacji na temat błędów 0,4%.

**Przykład**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Zwraca `dynamic` (JSON) tablica wszystkich wartości *wyrażenie* w grupie. 

* *MaxListSize* jest opcjonalna liczba całkowita limitu maksymalna liczba zwracanych elementów (wartość domyślna to *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Zwraca `dynamic` tablica (JSON) zestawu unikatowych wartości, które *wyrażenie* ma w grupie. (Porada: Aby zliczyć tylko unikatowych wartości, użyj [`dcount`](#dcount).)
  
*  *MaxSetSize* jest opcjonalna liczba całkowita limitu maksymalna liczba zwracanych elementów (wartość domyślna to *128*).

**Przykład**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Zobacz też [ `mvexpand` operator](#mvexpand-operator) funkcji przeciwne.


### <a name="max-min"></a>MAX, min

    max(Expr)

Oblicza wartość maksymalną liczbę *wyrażenie*.
    
    min(Expr)

Oblicza wartość minimalna *wyrażenie*.

**Porada**: to zapewnia minimum lub maksimum samodzielnie — na przykład najwyższe lub najniższe ceny. Jeśli jednak chcesz innych kolumn w wierszu — na przykład nazwa dostawcy z najniższe ceny - za pomocą [argmin lub argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>percentyl, percentylu, percentilew, percentilesw

    percentile(Expression, Percentile)

Zwraca szacowany dla *wyrażenia* określonego percentyla w grupie. Dokładność zależy od gęstości populacji w regionie wartość percentylu.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Jak `percentile()`, ale oblicza liczbę wartości percentylu (jest to szybszy pojedynczo obliczania każdego percentyl).

    percentilew(Expression, WeightExpression, Percentile)

Percentyl ważoną. Użyj tego ustawienia dla wstępnie zagregowane dane.  `WeightExpression`jest liczbą całkowitą, która wskazuje, ile wierszy oryginalnego są reprezentowane przez każdy wiersz zagregowane.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Jak `percentilew()`, ale oblicza liczbę wartości percentylu.

**Przykłady**


Wartość `duration` większej niż 95% próbki ustawiona i mniejszy niż 5% próbnej obliczania dla każdej nazwy żądanie:

    request 
  	| summarize percentile(duration, 95)
      by name

Pomijanie "przez..." do obliczenia dla całej tabeli.

Obliczanie jednocześnie kilka percentylu dla różnych żądania nazw:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Wyniki pokazują, że dla /Events/Index żądanie 5% żądania są odpowiedzi w mniej niż 2.44s, połowa je w 3.52s, i 5% jest mniejsza niż 6.85s.

Obliczanie wielu statystyki:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Ważoną percentylu

Użyj funkcji percentyl ważoną w przypadkach, gdy dane zostały wstępnie zagregowane. 

Załóżmy na przykład, aplikacji wykonuje tysiące operacji na sekundę, i chcesz sprawdzić swoje oczekiwania. Prosta rozwiązaniem jest wygenerować żądanie wniosków aplikacji lub niestandardowe wydarzenia dla każdej operacji. Utworzy wiele ruchu, mimo że adaptacyjne przy próbkowaniu zajmie efekt, aby zmniejszyć go. Chce zaimplementować jeszcze lepiej rozwiązanie: zapisze kodu w aplikacji agregacji danych przed wysłaniem go do aplikacji wnioski. Podsumowanie zagregowane będą wysyłane w regularnych interwałach, prawdopodobnie zmniejszania szybkość danych do kilku punktów na minutę.

Kod trwa strumienia pomiarów opóźnienie w milisekundach. Na przykład:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Oblicza wartości w następujących przedziałów:`{ 10, 20, 30, 40, 50, 100 }`

Okresowo wprowadza serię połączeń TrackEvent, jedną dla każdego kolorem niestandardowych rozmiarów w każdej konwersacji: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

Do analizy pojawi się jeden takiej grupy zdarzeń w następujący sposób:

`opCount` | `latency`| znaczenie
---|---|---
8 | 10 | = 8 operacje w Koszu 10 MS
6 | 20 | = 6 operacje w Koszu 20 MS
3 | 30 | = 3 operacje w Koszu 30 MS
1 | 40 | = operacje 1 w Koszu 40ms

Aby uzyskać szczegółowy obraz oryginalny dystrybucji opóźnienia zdarzenia, firma Microsoft korzysta z `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Wyniki są takie same, tak jakby były użyliśmy zwykły `percentiles` na oryginalnego zestawu wymiarów.

> [AZURE.NOTE] Ważoną percentylu nie są stosowane do [pobrane dane](app-insights-sampling.md), gdzie każdy wiersz próbki reprezentuje losowo oryginalny wierszy zamiast pojemnika. Funkcje percentyl zwykły są odpowiednie dla próbki danych.

#### <a name="estimation-error-in-percentiles"></a>Błąd oceny w percentylu

Agreguj percentylu zawiera wartość przybliżona przy użyciu [T szyfrowane](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Kilka ważnych uwag: 

* Granice wystąpi błąd oceny zależą od wartości percentylu wymagane. Dokładność zalecane jest na końcach [0..100] skalowanie percentylu 0 i 100 są dokładnie wartości minimalne i maksymalne rozkładu. Dokładność stopniowo zmniejsza kierunku środek skali. Jest najgorszego u mediana i jest ograniczona do 1%. 
* Granice błędu są obserwowane na pozycję nie na wartość. Załóżmy, że wartości percentylu (X, 50) zwrócił wartość Xm. Szacowanie gwarantuje, że co najmniej 49% i co najwyżej 51% wartości X są mniejsze niż Xm. Różnica między Xm i rzeczywista wartość mediany X jest nieograniczony teoretyczna.

### <a name="stdev"></a>ODCH.standardowe

     stdev(Expr)

Zwraca odchylenie standardowe *wyrażenia* w grupie.

### <a name="variance"></a>WARIANCJA

    variance(Expr)

Zwraca wariancję *wyrażenie* w grupie.

### <a name="sum"></a>Suma

    sum(Expr)

Zwraca sumę *wyrażenie* w grupie.                      


## <a name="scalars"></a>Wielkości skalarne

[poświaty](#casts) | [porównania](#scalar-comparisons)
<br/>
[GETTYPE](#gettype) | [mieszania](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Obsługiwane typy to:

| Typ      | Dodatkowe nazwy   | Równoważne typ .NET |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Poświaty

Czy można oddać z jednego typu do innego. Ogólnie Jeśli konwersja ma sens, to działa:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Sprawdź, czy ciąg można konwertować do określonego typu:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Porównanie skalarne

||
---|---
`<` |Mniej
`<=`|Mniejsze lub równe
`>` |Większa
`>=`|Większe lub równe
`<>`|Nie równa się
`!=`|Nie równa się 
`in`| Prawy operand jest tablicą (dynamicznymi) i lewy operand jest równe jednego z jej elementów.
`!in`| Prawy operand jest tablicą (dynamicznymi) i lewy operand nie jest równa jednej z jej elementów.




### <a name="gettype"></a>GETTYPE

**Zwraca**

Ciąg reprezentujący podstawowy typ miejsca do magazynowania w jednym argumentu. Jest to szczególnie przydatne, gdy masz wartości rodzaju `dynamic`: w tym przypadku `gettype()` powoduje pojawienie się, jak wartość jest kodowane.

**Przykłady**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>skrót

**W składni**

    hash(source [, mod])

**Argumenty**

* *źródła*: skalarna źródła skrótu jest obliczana na.
* *Mod*: modulo wartość ma być stosowana w wyniku mieszania.

**Zwraca**

Wartość xxhash (długa) danego wartość skalarna modulo danej wartości mod (Jeśli określony).

**Przykłady**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

`iff()` Funkcja ocenia pierwszego argumentu (predykacie) i zwraca wartość drugiego albo innych argumentów w zależności od tego, czy jest predykacie `true` lub `false`. Argumenty drugiego i trzeciego muszą być tego samego typu.

**W składni**

    iff(predicate, ifTrue, ifFalse)


**Argumenty**

* *predykat:* Wyrażenie, którego wynikiem jest `boolean` wartość.
* *ifTrue:* Wyrażenie, które elementy są obliczane i jej wartość zwrócone przez funkcję Jeżeli *predykat* daje w wyniku `true`.
* *ifFalse:* Wyrażenie, które elementy są obliczane i jej wartość zwrócone przez funkcję Jeżeli *predykat* daje w wyniku `false`.

**Zwraca**

Ta funkcja zwraca wartość *ifTrue* , jeśli wartość *predykat* `true`, lub wartość *ifFalse* w inny sposób.

**Przykład**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, isnotnull, notnull

    isnull(parsejson("")) == true

Pobiera jeden argument i informuje, czy ma wartość null.

**W składni**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Zwraca**

PRAWDA lub FAŁSZ w zależności od tego, czy znajduje się wartość null lub nie zawiera wartości null.


|x|IsNull(x)
|---|---
| "" | FAŁSZ
|znak "x" | FAŁSZ
|parsejson("")|wartość PRAWDA.
|parsejson("[]")|FAŁSZ
|parsejson("{}")|FAŁSZ

**Przykład**

    T | where isnotnull(PossiblyNull) | count

Należy zauważyć, że istnieją inne sposoby osiągnięcia w tym celu:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Oblicza kwerendy lub wyrażenie i zwraca wynik w postaci pojedynczą wartość. Ta funkcja jest przydatna do obliczeń etapowej; na przykład obliczania całkowita liczba zdarzeń, a następnie za pomocą którego planu bazowego.

**W składni**

    toscalar(query)
    toscalar(scalar)

**Zwraca**

Szacowane argument. Jeśli argument jest tabeli, zwraca pierwszej kolumny w pierwszym wierszu. (Zalecane jest rozmieszczanie, że argument zawiera tylko jedną kolumnę i wiersz).

**Przykład**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Literałów wartości logicznych

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Operatory logiczne

    and 
    or 

    

## <a name="numbers"></a>Numery

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Stałe numeryczne

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Operatory arytmetyczne

|| |
|---|-------------|
| + | Dodawanie         |
| - | Odejmowanie    |
| * | Mnożenie    |
| / | Dzielenie      |
| % | Modulo      |
||
|`<` |Mniej
|`<=`|Mniejsze lub równe
|`>` |Większa
|`>=`|Większe lub równe
|`<>`|Nie równa się
|`!=`|Nie równa się 


### <a name="abs"></a>Moduł.liczby

**W składni**

    abs(x)

**Argumenty**

* x - liczba całkowita, rzeczywistą lub przedziału czasu

**Zwraca**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>Kosz, floor

Zaokrągla liczbę wartości w dół do liczby całkowitej wielokrotnością rozmiaru danego pojemnika. Często używane w [`summarize by`](#summarize-operator) kwerendy. Jeśli masz rozproszone zestawu wartości one być pogrupowane w mniejszy zestaw określonych wartości.

Alias `floor`.

**W składni**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumenty**

* *wartość:* Liczby, daty lub przedziału czasu. 
* *roundTo:* "Rozmiar Kosza". Liczby, daty lub przedziału czasu, który dzieli *wartość*. 

**Zwraca**

Do najbliższej wielokrotności liczby *roundTo* poniżej *wartości*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Przykłady**

Wyrażenie | Wynik
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Następujące wyrażenie oblicza histogram czasy trwania, o rozmiarze kolorem 1 sekundę:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>EXP

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>powierzchnia

Alias [`bin()`](#bin).

### <a name="gamma"></a>gamma

Wartość [funkcji gamma](https://en.wikipedia.org/wiki/Gamma_function)

**W składni**

    gamma(x)

**Argumenty**

* *x:* Liczba rzeczywista

Dla dodatnie liczby całkowite `gamma(x) == (x-1)!` na przykład `gamma(5) == 4 * 3 * 2 * 1`.

Zobacz też [loggamma](#loggamma).


### <a name="log"></a>log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`powinny być liczbą rzeczywistą > 0. W przeciwnym razie zostanie zwrócona wartość null.

### <a name="loggamma"></a>loggamma


Logarytm naturalny wartość bezwzględna argumentu [funkcji gamma](#gamma).

**W składni**

    loggamma(x)

**Argumenty**

* *x:* Liczba rzeczywista


### <a name="rand"></a>LOS

Generator do liczb losowych.

* `rand()`-liczba rzeczywista od 0.0 do 1.0
* `rand(n)`-całkowitą z zakresu od 0 do n-1




### <a name="sqrt"></a>pierwiastek

Funkcja pierwiastek kwadratowy.  

**W składni**

    sqrt(x)

**Argumenty**

* *x:* Liczba rzeczywista > = 0.

**Zwraca**

* Liczba dodatnia tak, aby`sqrt(x) * sqrt(x) == x`
* `null`Jeśli argument ma wartość ujemną lub nie można przekonwertować na `real` wartość. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Data i godzina


[temu](#ago) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [teraz](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Data i godzina literałów

|||
---|---
**daty i godziny**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Godziny są zawsze UTC. Pominięcie daty zawiera czas dzisiaj.
`now()`|Bieżącą godzinę.
`now(`-*przedziału czasu*`)`|`now()-`*przedziału czasu*
`ago(`*przedziału czasu*`)`|`now()-`*przedziału czasu*
**przedziału czasu**|
`2d`|2 dni
`1.5h`|1,5 godziny 
`30m`|30 minut
`10s`|10 sekund
`0.1s`|druga 0,1
`100ms`| 100 milisekund
`10microsecond`|
`1tick`|100 ns
`time("15 seconds")`|
`time("2")`| 2 dni
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Data i godzina wyrażeń

Wyrażenie |Wynik
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Mniej
`<=`|Mniejsze lub równe
`>` |Większa
`>=`|Większe lub równe
`<>`|Nie równa się
`!=`|Nie równa się 




### <a name="ago"></a>temu

Odejmuje określonego przedziału czasu od bieżącego czasu UTC zegara. Jak `now()`, ta funkcja może być używany wiele razy w instrukcji i czasu zegara UTC, której dotyczy odwołanie jest taki sam dla wszystkich wystąpień.

**W składni**

    ago(a_timespan)

**Argumenty**

* *a_timespan*: interwał odejmowane od bieżącej godziny zegar UTC (`now()`).

**Zwraca**

    now() - a_timespan

**Przykład**

Wszystkie wiersze z sygnaturą czasową w ostatniej godziny:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Wyodrębnia określoną część daty jako liczba całkowita.

**W składni**

    datepart(part, datetime)

**Argumenty**

* `part:String`-{"Year", "Miesiąc", "Dzień", "Godz.", "Minuta", "Drugi", "Milisekund", "Mikrosekund", "Nanosekund"}
* `datetime`

**Zwraca**

Długa reprezentującą określonej części.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

Liczba porządkowa dzień w miesiącu.

**W składni**

    dayofmonth(a_date)

**Argumenty**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>DayOfWeek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Całkowita liczba dni od poprzedniego niedziela, jako `timespan`.

**W składni**

    dayofweek(a_date)

**Argumenty**

* `a_date`: A `datetime`.

**Zwraca**

`timespan` Od północy na początku poprzedniego niedziela, zaokrąglona w dół do liczby całkowitej liczby dni.

**Przykłady**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Liczba porządkowa liczba dni w roku.

**W składni**

    dayofyear(a_date)

**Argumenty**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday, endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Uzyskaj numer miesiąca (1 – 12) od daty i godziny.

**Przykład**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Pobieranie rok z daty i godziny.

**Przykład**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>teraz

    now()
    now(-2d)

Bieżącą godzinę zegar UTC opcjonalnie przesunięcie według określonego przedziału czasu. Ta funkcja może być używany wiele razy w instrukcji i czasu zegara, której dotyczy odwołanie jest taki sam dla wszystkich wystąpień.

**W składni**

    now([offset])

**Argumenty**

* *przesunięcie:* A `timespan`, dodać do bieżącej godziny zegar UTC. Domyślny: 0.

**Zwraca**

Bieżącą godzinę zegar UTC jako `datetime`.

    now() + offset

**Przykład**

Określa interwał od zdarzenia oznaczona predykacie:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday, startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Sprawdzanie, czy ciąg jest prawidłową datą:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Wynik całkowita reprezentuje numer tygodnia przez ISO 8601 standardowy. Pierwszy dzień tygodnia jest niedziela, a pierwszy tydzień roku jest tydzień, zawierającą pierwszy czwartek tego roku. (Ostatnie dni w roku w związku z tym mogą zawierać kilka dni tygodnia 1 następnym roku, lub pierwszych dni może zawierać niektóre tygodnia 52 lub 53 poprzedniego roku).


## <a name="string"></a>Ciąg

[countof](#countof) | [wyodrębnić](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [zamienić](#replace) | [Dzielenie](#split) | [strcat](#strcat) | [strlen](#strlen) | [podciąg](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Ciąg literałów

Reguły są takie same jak JavaScript.

Ciągi może być ujęte w pojedynczy lub podwójny znakami cudzysłowu. 

Ukośnik odwrotny (`\`) jest wykorzystywane w celu uniknięcia znaki, takie jak `\t` (karta), `\n` (nowy wiersz) i wystąpienia umieszczając znak cudzysłowu.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Ciąg zaciemnionego literałów

Ciąg zaciemnionego literałów są ciągami, których analizy będzie zasłaniały podczas wyprowadzania ciągu (na przykład do śledzenia). Proces zaciemniania zastępuje znaki wszystkich zaciemnionego uruchomienie (`*`) znaków.

Do utworzenia zaciemnionego literał ciągu znaków, dołączy `h` lub "H". Na przykład:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Porównaniach ciągów znaków

Operator|Opis|Wielkość liter|Przykład PRAWDA
---|---|---|---
`==`|Równa się |Tak| `"aBc" == "aBc"`
`<>` `!=`|Nie równa się|Tak| `"abc" <> "ABC"`
`=~`|Równa się |Brak| `"abc" =~ "ABC"`
`!~`|Nie równa się |Brak| `"aBc" !~ "xyz"`
`has`|Prawo-nadwozia1 po stronie (RHS) jest całego terminu w lewego-nadwozia1 po stronie (LHS)|Brak| `"North America" has "america"`
`!has`|RHS nie jest pełna terminu w LHS|Brak|`"North America" !has "amer"` 
`hasprefix`|RHS jest prefiksem terminu w LHS|Brak|`"North America" hasprefix "ame"`
`!hasprefix`|RHS nie jest prefiks płatności w LHS|Brak|`"North America" !hasprefix "mer"`
`hassuffix`|RHS jest sufiks terminu w LHS|Brak|`"North America" hassuffix "rth"`
`!hassuffix`|RHS nie jest sufiksem płatności w LHS|Brak|`"North America" !hassuffix "mer"`
`contains` | RHS występuje jako ciąg LHS|Brak| `"FabriKam" contains "BRik"`
`!contains`| RHS nie występuje w LHS|Brak| `"Fabrikam" !contains "xyz"`
`containscs` | RHS występuje jako ciąg LHS|Tak| `"FabriKam" contains "Kam"`
`!containscs`| RHS nie występuje w LHS|Tak| `"Fabrikam" !contains "Kam"`
`startswith`|RHS jest początkowej podciąg LHS.|Brak|`"Fabrikam" startswith "fab"`
`!startswith`|RHS nie jest początkowe podciąg LHS.|Brak|`"Fabrikam" !startswith "abr"`
`endswith`|RHS jest ciąg końcowych LHS.|Brak|`"Fabrikam" endswith "kam"`
`!endswith`|RHS nie jest ciąg końcowych LHS.|Brak|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS zawiera dopasowanie RHS|Tak| `"Fabrikam" matches regex "b.*k"`
`in`|Równa się jeden z elementów|Tak|`"abc" in ("123", "345", "abc")`
`!in`|Nie równa się jeden z elementów|Tak|`"bc" !in ("123", "345", "abc")`

Używanie `has` lub `in` Jeśli testowania obecność całkiem leksykalne terminu — oznacza to, że symbolu lub alfanumeryczny word ograniczone przez znaki niealfanumeryczne lub początek lub koniec pola. `has`wykonuje szybciej niż `contains`, `startswith` lub `endswith`. Pierwszy te kwerendy działa szybciej:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Zlicza wystąpień ciąg w ciągu. Pasujące ciągi zwykły może zachodzić na; nie odpowiada regex.

**W składni**

    countof(text, search [, kind])

**Argumenty**

* *tekst:* Ciąg.
* *wyszukiwania:* Zwykły ciągu lub wyrażenie regularne dopasuje w *tekście*.
* *rodzaju:* `"normal"|"regex"` Domyślne `normal`. 

**Zwraca**

Liczba pasujących do wyszukiwanego ciągu znaków może być w kontenerze. Pasujące ciągi zwykły może zachodzić na; nie odpowiada regex.

**Przykłady**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (nie 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>Wyodrębnianie

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Uzyskaj zgodnego z [wyrażeń regularnych](#regular-expressions) z ciągu tekstowego. Opcjonalnie następnie konwertuje wyodrębnionej podciąg wskazanej typu.

**W składni**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumenty**

* *regex:* [Wyrażenie](#regular-expressions).
* *captureGroup:* Wynik dodatni `int` stałą wskazująca grupę przechwytywania, aby wyodrębnić. 0 oznacza dopasowanie całego — 1 dla wartości spełniające pierwszej "(" nawias")" w wyrażeniu regularnym, 2 lub więcej dla kolejnych nawiasów.
* *tekst:* A `string` do wyszukiwania.
* *typeLiteral:* Literał wpisz opcjonalnie (np `typeof(long)`). Jeśli podano wyodrębnionej podciąg jest konwertowane na tego typu. 

**Zwraca**

Jeśli *regex* znajdzie dopasowanie *tekstu*: podciąg dopasowywane grupy do przechwytywania wskazanej *captureGroup*opcjonalnie konwertowane na *typeLiteral*.

Jeśli jest niezgodna lub konwersji typów kończy się niepowodzeniem: `null`. 

**Przykłady**

Przykładowy ciąg `Trace` są wyszukiwane definicję `Duration`. Uwzględnij jest konwertowana na `real`, a następnie iloczyn stałą czasu (`1s`), aby `Duration` jest wartością typu Liczba `timespan`. W tym przykładzie jest równe 123.45 sekund:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

W tym przykładzie jest równoważna `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>Funkcja IsEmpty, isnotempty, notempty

    isempty("") == true

Zwraca wartość true, jeśli argument jest pusty ciąg znaków lub wartość null.
Zobacz też [isnull](#isnull).


**W składni**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Zwraca**

Wskazuje, czy argument jest pusty ciąg lub isnull.

|x|IsEmpty(x)
|---|---
| "" | wartość PRAWDA.
|znak "x" | FAŁSZ
|parsejson("")|wartość PRAWDA.
|parsejson("[]")|FAŁSZ
|parsejson("{}")|FAŁSZ


**Przykład**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

Adres URL można podzielić jego części.

**W składni**

    parseurl(urlstring)

**Argumenty**

* *urlstring:* ADRES URL.

**Zwraca**

Obiekt zawierający części jako ciągi.

**Przykład**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>zamienianie

Zamień wszystkie dopasowania regex innego ciągu.

**W składni**

    replace(regex, rewrite, text)

**Argumenty**

* *regex:* [Wyrażenie](https://github.com/google/re2/wiki/Syntax) wyszukiwania *tekstu*. Może zawierać grup przechwytywania w "("nawiasy")". 
* *edycji:* Regex wymiana dla dowolnego dopasowanie *matchingRegex*. Używanie `\0` dotyczyć całego dopasowania `\1` dla pierwszej grupy przechwytywania `\2` i tak dalej dla kolejnych przechwytywania grup.
* *tekst:* Ciąg.

**Zwraca**

*tekst* po zamieniając wszystkie dopasowania *regex* ocen *edycji*. Dopasowuje nie pokrywają się.

**Przykład**

Tej instrukcji:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Występują następujące wyniki:

| x    | str | zastępuje|
|---|---|---|
| 1    | Liczba jest 1.000000  | Liczba była: 1.000000|
| 2    | Liczba jest 2.000000  | Liczba była: 2.000000|
| 3    | Liczba jest 3.000000  | Liczba była: 3.000000|
| 4    | Liczba jest 4.000000  | Liczba była: 4.000000|
| 5    | Liczba jest 5.000000  | Liczba była: 5.000000|
 



### <a name="split"></a>dzielenie

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Dzieli ciągu zgodnie z danym ogranicznika i zwraca tablicę ciągów z podciągów conatined. Opcjonalnie można ciąg określonych zwrócona, jeśli istnieje.

**W składni**

    split(source, delimiter [, requestedIndex])

**Argumenty**

* *źródła*: ciąg źródła, który będzie podzielonej według ogranicznika danej.
* *ogranicznika*: ogranicznik, który zostanie użyty w celu dzielenie ciąg źródła.
* *requestedIndex*: opcjonalne indeksu `int`. Jeśli podano tablicy zwracany ciąg będzie zawierać wymagane podciąg, jeśli istnieje. 

**Zwraca**

Tablica ciągów, zawierająca podciągi ciąg danym źródłowym są rozdzielone danego ogranicznika.

**Przykłady**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Łączy między 1 a 16 argumentów, które muszą być ciągami.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Długość ciągu.

### <a name="substring"></a>podciąg

    substring("abcdefg", 1, 2) == "bc"

Wyodrębnianie ciąg z ciągu danym źródłowym, rozpoczynając od danego indeksu. Opcjonalnie można określić długość wymagane podciąg.

**W składni**

    substring(source, startingIndex [, length])

**Argumenty**

* *źródło:* Ciąg źródła podciąg zostaną pobrane z.
* *startingIndex:* Od zera początkowej pozycji znaku podciągu wymagane.
* *długość:* Parametr opcjonalny, który może służyć do określ żądaną liczbę znaków w podciągu. 

**Zwraca**

Ciąg z ciągu. Podciąg zaczyna się od pozycji znaku startingIndex (od zera) i przechodzi do końca ciągu lub długość znaków, jeśli określony.

**Przykłady**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Konwertuje ciąg na małe litery.

### <a name="toupper"></a>toupper

    toupper("hello") == "HELLO"

Konwertuje ciąg na wielkie litery.



### <a name="guids"></a>Identyfikatory GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Dynamiczne, obiektów i tablic

[literałów](#dynamic-literals) | [rzutowania](#casting-dynamic-objects) | [operatory](#operators) | [umożliwić klauzul](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [zakres](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Oto wynik kwerendy z powodu wyjątku wniosków aplikacji. Wartość w `details` jest tablicą.

![](./media/app-insights-analytics-reference/310.png)

**Indeksowanie:** Indeks tablic i obiekty, tak jak JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Wykorzystaniem `arraylength` i innych funkcji analizy (nie ".length"!)

**Rzutowanie** W niektórych przypadkach należy do oddania element, który można wyodrębnić obiektu, ponieważ typ jej może być różna. Na przykład `summarize...to` wymaga określonego typu:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literałów** Aby utworzyć jawne tablicy lub grupa właściwości obiektu, należy zapisać go jako ciągu JSON i cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Aby umieścić od siebie właściwości obiektu w osobnych wierszach, użyj mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Aby znaleźć wszystkie ścieżki w obiekcie złożone:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** Aby znaleźć minimalne schematu, w którym udział wszystkich wartości wyrażenia w tabeli:

    exceptions | summarize buildschema(details)

Wynik:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Należy zauważyć, że `indexer` jest używany do oznaczania, gdzie należy używać indeksu liczbowego. Ten schemat niektórych prawidłowe ścieżki może być (zakładając, że indeksy te przykład znajdują się w zakresie):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Tablica i obiekt literałów

Aby utworzyć dynamiczne literałów, użyj `parsejson` (alias `todynamic`) z JSON ciąg argumentów:

* `parsejson('[43, 21, 65]')`-tablicy liczb
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-pojedynczą wartość typu dynamiczne, zawierającą liczbę
* `parsejson('"21"')`-pojedynczą wartość typu dynamicznego zawierającą ciąg

Uwaga, że w odróżnieniu od JavaScript, JSON wymaga stosowania podwójnych cudzysłowów (`"`) ciągi. Dlatego jest łatwiej oferty ciąg zakodowany JSON literałów przy użyciu pojedynczy cudzysłów (`'`).

W tym przykładzie tworzy wartość dynamiczną, a następnie używa jej pola:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Funkcje dynamiczne obiektu

|||
|---|---|
| *wartość* `in` *tablicy*| Wartość true, jeśli istnieje element *tablicy* który == *wartość*<br/>`where City in ('London', 'Paris', 'Rome')`
| *wartość* `!in` *tablicy*| Wartość true, jeśli istnieje żaden element *tablicy* który == *wartość*
|[`arraylength(`Tablica`)`](#arraylength)| Wartość null, jeśli nie jest tablicy.
|[`extractjson(`Ścieżka, obiekt`)`](#extractjson)|Używa ścieżki, aby przejść do obiektu.
|[`parsejson(`źródła`)`](#parsejson)| Ciąg JSON staje się obiektem dynamicznym.
|[`range(`od, krok`)`](#range)| Tablicę wartości
|[`mvexpand`listColumn](#mvexpand-operator) | Replikuje wiersz dla każdej wartości na liście w określonej komórki.
|[`summarize buildschema(`kolumny`)`](#buildschema) |Ustala schematu typu zawartości kolumny
|[`summarize makelist(`Kolumna`)`](#makelist)| Spłaszcza grup wierszy i umieszcza wartości w kolumnie w tablicy.
|[`summarize makeset(`kolumny`)`](#makeset) | Spłaszcza grup wierszy i umieszcza wartości kolumny w tablicy, bez duplikatów.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamiczne obiektów w pozwalają klauzul


[Umożliwianie klauzule](#let-clause) magazynu dynamicznego wartości jako ciągi, dlatego te dwie klauzule są równoważne i zarówno `parsejson` (lub `todynamic`) przed użyciem:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Liczba elementów w tablicy dynamicznej.

**W składni**

    arraylength(array)

**Argumenty**

* *tablicy:* A `dynamic` wartość.

**Zwraca**

Liczba elementów w *tablicy*lub `null` Jeśli *Tablica* nie jest tablicą.

**Przykłady**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Uzyskaj określony element tekstu JSON, przy użyciu wyrażenia ścieżki. Opcjonalnie można przekonwertować wyodrębnionej ciągu określonego typu.


**W składni**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Zwraca**

Ta funkcja wykonuje kwerendy JsonPath do źródła danych, który zawiera prawidłowe parametry JSON, opcjonalnie konwertowania tę wartość do innego typu, w zależności od trzeci argument.



**Przykład**

Notacji [kwadratowy]] i notacji kropki są równoważne:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Porady dotyczące wydajności**

* Stosowanie klauzul where przed użyciem`extractjson()`
* Warstwowy dopasowanie wyrażeń regularnych z [Wyodrębnianie](#extract) . Można uruchamiać znacznie szybsze i ma zastosowanie w przypadku JSON przy użyciu szablonu.
* Używanie `parsejson()` Jeśli chcesz wyodrębnić więcej niż jedną wartość z JSON.
* Należy rozważyć JSON analizowane w spożyciu przez deklarowanie typ kolumny dynamiczne.

### <a name="json-path-expressions"></a>Ścieżka JSON wyrażeń

|||
|---|---|
|`$`|Obiekt główny|
|`@`|Bieżący obiekt|
|`[0]`|Indeks dolny tablicy|
|`.`lub`[0]` | Podrzędne|

*(Firma Microsoft nie obecnie zaimplementować symboli wieloznacznych, rekursji, Unii lub wycinki.)*




### <a name="parsejson"></a>parsejson

Zinterpretowany `string` jako [wartość JSON](http://json.org/)) i zwraca wartość jako `dynamic`. Jest najwyższej do korzystania z `extractjson()` gdy chcesz wyodrębnić więcej niż jeden element obiektu złożonego JSON.

**W składni**

    parsejson(json)

**Argumenty**

* *json:* Dokument JSON.

**Zwraca**

Obiekt typu `dynamic` określony przez *json*.

**Przykład**

W poniższym przykładzie gdy `context_custom_metrics` jest `string` że wygląda tak: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

Poniższy fragment pobiera wartość, a następnie `duration` przedział w obiekcie, a następnie w że pobiera dwa gniazda `duration.value` i  `duration.min` (`118.0` i `110.0`odpowiednio).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>zakres

`range()` Funkcji (nie należy mylić z `range` operator) generuje tablicy dynamicznej przytrzymując serii równomiernie rozmieszczone wartości.

**W składni**

    range(start, stop, step)

**Argumenty**

* *start:* Wartość pierwszego elementu w tablicy wynikowej. 
* *zatrzymać:* Wartość ostatniego elementu w tablicy wynikowej lub co najmniej wartość jest większa niż ostatni element w tablicy wynikowej i w ramach liczbą całkowitą wielokrotnością liczby *krok* z menu *start*.
* *kroku:* Różnica między dwoma kolejnych elementów tablicy.

**Przykłady**

W poniższym przykładzie zwracany `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

W poniższym przykładzie zwracana tablica, przytrzymując wszystkie dni w roku 2015 r.:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Konwertuje ciąg na wartość dynamiczne.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Wylicza wszystkie wyrażenia ścieżki identyfikujące liśćmi w dynamiczny obiekt. 

**Zwraca**

Tablica ścieżka wyrażeń.

**Przykłady**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Uwaga "[0]" wskazuje na obecność tablicy, ale nie określono indeks używany przez określoną ścieżką.

### <a name="zip"></a>ZIP

    zip(list1, list2, ...)

Łączy zestaw list jedna lista krotki.

* `list1...`: Listy wartości

**Przykłady**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Nazwy

Nazwy mogą zawierać maksymalnie 1024 znaków. Jest uwzględniana wielkość liter i mogą zawierać litery, cyfry i podkreślenia (`_`). 

Oferty przy użyciu nazwy ["... '] or [" ... "] Aby dołączyć inne znaki lub nazwy za pomocą słowa kluczowego. Na przykład:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["ścieżka\\file\n\'x\'"] | Użyj \ escape znaków|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Nie anuluje - \ jest literałów|
|[@"\now& następnie\"] | |
|[miejsce, w którym] | Za pomocą słowa kluczowego języka jako nazwę|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


