<properties
    pageTitle="Zaloguj wyszukiwania analizy dziennika | Microsoft Azure"
    description="Wyszukiwanie dziennika umożliwiają łączenie i grupowania danych dowolnego komputera z wielu źródeł w środowisku."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Dziennik wyszukiwania analizy dziennika

W Centrum analiz dziennika jest funkcja wyszukiwania dziennika, dzięki czemu można łączyć i grupowania danych dowolnego komputera z wielu źródeł w środowisku. Rozwiązania są również obsługiwane przez dziennik wyszukiwania, aby wyświetlić możesz metryki obraca się wokół obszaru konkretnego problemu.

Na stronie wyszukiwania można utworzyć kwerendę, a następnie podczas wyszukiwania, można filtrować wyniki za pomocą kontrolek Faseta. Można także tworzyć zapytania zaawansowane przekształcenie, filtrowania i raportu na wyniki.

Typowe dziennika kwerend wyszukiwania są wyświetlane na większości stron rozwiązanie. W konsoli usługi OMS można kliknąć Kafelki lub przechodzenie do w do innych elementów, aby wyświetlić szczegóły dotyczące elementu za pomocą funkcji wyszukiwania dziennika.

W tym samouczku przedstawimy każde przykłady, aby omówić wszystkie podstawowe informacje w trakcie korzystania z funkcji wyszukiwania dziennika.

Firma Microsoft będzie rozpoczynać się proste, praktyczne przykłady, a następnie skonstruuj nad nimi, tak aby można uzyskać opis przypadków użycia praktyczne temat wyodrębnianie wniosków, które mają z danych za pomocą składni.

Po wprowadzeniu znanych z technik wyszukiwania, możesz przejrzeć [analizy dziennika logowania odwołanie wyszukiwania](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Korzystanie z filtrów podstawowych

Jest przede wszystkim, aby dowiedzieć się, że pierwsza część wyszukiwania kwerendy przed zastosowaniem "|" znaku kreski pionowej pionowej, jest zawsze *filtru*. Możesz myśleć go jako klauzuli WHERE TSQL — określają, *jakie* podzestawu danych, aby uwzględniał wylogowywanie się z magazynu danych usługi OMS. Wyszukiwanie w magazynie danych jest głównie temat określania cech dane, które mają zostać wyodrębnione, dlatego naturalne, że kwerendy zaczyna się z klauzuli WHERE.

Filtry najbardziej podstawowe, których można używać są *słowa kluczowe*, takie jak "błąd" lub "Przekroczono limit czasu" lub nazwę komputera. Następujące typy prostych kwerend na ogół wraca różnych kształtów danych w ten sam zestaw wyników. Jest to spowodowane analizy dziennika ma różnych *typów* danych w systemie.


### <a name="to-conduct-a-simple-search"></a>Aby przeprowadzić wyszukiwanie proste
1. W portalu usługi OMS kliknij przycisk **Wyszukaj dziennika**.  
    ![Wyszukiwanie kafelków](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. W polu kwerendy wpisz `error` , a następnie kliknij przycisk **Wyszukaj**.  
    ![Błąd wyszukiwania](./media/log-analytics-log-searches/oms-search-error.png)  
    Na przykład kwerenda `error` na poniższej ilustracji zwracane 100 000 rekordów **zdarzenia** (zbierane przez zarządzanie dziennikiem), 18 rekordów **ConfigurationAlert** (generowane przez ocena konfiguracji) i 12 rekordów **ConfigurationChange** (przechwycone przez śledzenie zmian).   
    ![wyniki wyszukiwania](./media/log-analytics-log-searches/oms-search-results01.png)  

Te filtry nie są naprawdę klas typy obiektów. *Typ* jest tylko znacznik, lub właściwości lub ciąg/Nazwa kategorię, podłączonego do elementu danych. Niektóre dokumenty w systemie są oznakowane jako **Typ: ConfigurationAlert** , a niektóre są oznakowane jako **Typ: wydajności**lub **Typu: zdarzenia**i tak dalej. Każdy wynik wyszukiwania, dokumentu, rekordu lub wpisu Wyświetla nieprzetworzonych właściwości i ich wartości dla każdego z tych fragmentami danych i umożliwia nazw pól określić w filtrze, kiedy mają zostać pobrane tylko te rekordy, których pole znajdującym się wartość nie.

*Typ* jest wyjątkowo tylko pola, które mają wszystkie rekordy, nie różni się od innych polach. To zostało ustalone na podstawie wartości w polu Typ. Rekord ma innego formularza lub kształt. Niektórych **Typ = wydajności**, lub **Typ = zdarzenia** jest również składni, którą trzeba Dowiedz się, jak wyszukiwać dane dotyczące wydajności lub zdarzeń.

Po nazwę pola, a przed wartością można użyć dwukropek (:) lub znak równości (=). **Typ: zdarzenia** i **Typ = zdarzenia** są równoważne w znaczeniu, możesz wybrać styl wolisz.

Tak, jeśli typ = wydajności rekordy mają pole o nazwie "CounterName", a następnie można napisać podobne do kwerendy `Type=Perf CounterName="% Processor Time"`.

Zapewni to tylko dane dotyczące wydajności nazwę licznika wydajności w przypadku "% czasu procesora".

### <a name="to-search-for-processor-time-performance-data"></a>Aby wyszukać dane dotyczące wydajności czasu procesora
- W polu wyszukiwania kwerendy wpisz`Type=Perf CounterName="% Processor Time"`

Można również być bardziej szczegółowe i używać **InstanceName = _ "Ogółem"** w kwerendzie, która jest licznik wydajności systemu Windows. Możesz również wybrać Faseta albo inną **wartość pola:**. Filtr zostaną automatycznie dodane do filtru na pasku kwerendy. Może to widzieć na poniższej ilustracji. Pokazuje miejsca kliknij, aby dodać **nazwa_wystąpienia: "_Suma"** w kwerendzie bez wpisywania wszystko.

![Faseta wyszukiwania](./media/log-analytics-log-searches/oms-search-facet.png)

Teraz staje się zapytania`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

W tym przykładzie nie musisz określić **Typ = wydajności** uzyskać taki wynik. Ponieważ pola CounterName i nazwa_wystąpienia istnieć tylko dla rekordów typu = wydajności, kwerenda jest określone, aby zwracała te same wyniki co dłużej, poprzedniego:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Jest to spowodowane wszystkich filtrów w kwerendzie są obliczane jako *i* ze sobą. Efektywne, więcej pól, możesz dodać do kryteriów, otrzymasz mniej, więcej określonych i dostosowanego wyników.

Na przykład kwerenda `Type=Event EventLog="Windows PowerShell"` jest identyczny z `Type=Event AND EventLog="Windows PowerShell"`. Zwraca wszystkie zdarzenia, które zostały zarejestrowane i zbierane z dziennika zdarzeń systemu Windows PowerShell. Po dodaniu filtru wielokrotnie wybierając wielokrotnie samej Faseta, problem jest czysto kosmetycznych — może folderu mało istotne paska wyszukiwania, ale nadal program zwraca te same wyniki ponieważ niejawne operatora AND jest zawsze dostępne.

Niejawne operatora AND wycofać łatwo przy użyciu operatora NOT jawnie. Na przykład:

`Type:Event NOT(EventLog:"Windows PowerShell")`lub jego odpowiednika `Type=Event EventLog!="Windows PowerShell"` zwrot wszystkie zdarzenia w innych dzienników, które nie są dziennika programu Windows PowerShell.

Lub można użyć innych operatorów logicznych, takich jak "Lub". Poniższa kwerenda zwraca rekordy, dla których dziennik zdarzeń jest System lub dowolnej aplikacji.

```
EventLog=Application OR EventLog=System
```

Za pomocą powyższa kwerenda, otrzymasz wpisy dla obu dzienników w ten sam zestaw wyników.

Jednak jeśli usuniesz lub pozostawiając niejawne i w miejscu, następnie poniższe zapytanie nie zwrócą żadnych wyników, ponieważ nie istnieje wpis dziennika zdarzeń, który należy do obu dzienników. Każdy wpis dziennika zdarzeń został zapisany jest tylko jeden z dwóch dzienników.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Korzystanie z filtrów dodatkowe

Poniższe zapytanie zwraca pozycje 2 dzienniki zdarzeń dla wszystkich komputerów, które zostały wysłane dane.

```
EventLog=Application OR EventLog=System
```

![wyniki wyszukiwania](./media/log-analytics-log-searches/oms-search-results03.png)

Wybranie jednego z pól lub filtry zostaną zawęzić kwerendę do określonego komputera, z wyłączeniem innych pól tekstowych. Wynikowa kwerenda będzie wyglądać w następujący sposób.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Co oznacza w poniższym przykładzie, ze względu na niejawne i

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Każda kwerenda jest obliczane w następującej kolejności jawne. Uwaga nawias.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Podobnie jak pole dziennika zdarzeń można pobrać tylko dane dotyczące zestawu określonych komputerach przez dodanie lub. Na przykład:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Podobnie to poniższe zapytanie zwraca **czas Procesora (%)** dla dwóch komputerów tylko.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Operatory logiczne
Daty i godziny i pola liczbowe, można wyszukać wartości *większe niż* *mniejsze niż*, i *mniejsze niż lub równa*. Można używać prostych operatorów, takich jak >, <>; =, < =,! = na pasku wyszukiwania zapytania.


Określonego dziennika zdarzeń można wyszukiwać w określonym okresie. Na przykład przy użyciu następującego wyrażenia mnemoniczny wyraża się ostatnich 24 godzin.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Aby wyszukać przy użyciu operatorów logicznych
- W polu wyszukiwania kwerendy wpisz`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![Wyszukiwanie z wartość logiczna](./media/log-analytics-log-searches/oms-search-boolean.png)

Mimo że można kontrolować przedział czasu graficznie i większość czasu można to zrobić, ma zalet łącznie z filtrem czas bezpośrednio do kwerendy. Na przykład to rozwiązanie idealne przy użyciu pulpitów nawigacyjnych, gdzie można zastąpić czasu dla każdego fragmentu, bez względu na *globalnej* selektor czasu na stronie pulpitu nawigacyjnego. Aby uzyskać więcej informacji zobacz [Sprawach czasu na pulpicie nawigacyjnym](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Gdy filtrowanie według czasu, pamiętaj o uzyskiwanie wyników dla *przecięcia* dwóch okresów: określony w portalu usługi OMS (S1) i określony w kwerendzie (S2).

![Przecięcie](./media/log-analytics-log-searches/oms-search-intersection.png)

Oznacza to, jeśli okresy nie przecinają się, na przykład w portalu usługi OMS miejsce, w którym możesz wybrać **Ten tydzień** w kwerendzie służącego do określania **Ostatni tydzień**, nie ma żadnych przecięcia i nie będziesz otrzymywać żadnych wyników.

Operatory porównania tego pola TimeGenerated również są przydatne w innych sytuacjach. Na przykład z pola liczbowe.

Podane na przykład, że alerty ocena konfiguracji mają następujące wartości ważności:

- 0 = informacji
- 1 = ostrzeżenia
- 2 = krytyczne

Możesz wyszukiwać zarówno ostrzeżenie i krytyczne alerty i również wykluczyć informacyjna z nich przy użyciu następującej kwerendy:

```
Type=ConfigurationAlert  Severity>=1
```


Umożliwia także zakres kwerendy. Oznacza to, umożliwiają początek i koniec zakres wartości w kolejności. Na przykład jeśli chcesz zdarzenia w dzienniku zdarzeń programu Operations Manager, gdzie identyfikator zdarzenia jest większe niż lub równe 2100, ale nie jest większa niż 2199, następnie poniższa kwerenda zwróci je.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Składnia zakresu, należy użyć jest separator pola: wartość dwukropek (:), a *nie* znak równości (=). Ująć w nawiasy kwadratowe dolnymi i górną granicę zakresu i oddzielić dwie kropki (.).

## <a name="manipulate-search-results"></a>Opracowanie wyników wyszukiwania

Jeśli szukasz danych należy uściślić kwerendę wyszukiwania i że masz dobry poziom kontroli nad wyniki. Po pobraniu wyników, można użyć polecenia umożliwiające przekształcanie ich.

Polecenia w dzienniku analizy wyszukiwania *musi* wykonaj po znaku znaku kreski pionowej (|). Filtr musi być zawsze pierwsza część ciągu kwerendy. Definiuje zestaw danych, nad którym pracujesz z, a następnie "przewody" tych wyników do polecenia. Następnie można potoku dodać dodatkowe polecenia. Przypomina luźno proces programu Windows PowerShell.

Na ogół język wyszukiwania analizy dziennika próbuje wykonaj stylu programu PowerShell i wskazówki dokonanie podobne do informatyków i zmniejszenia krzywą uczenia się.

Polecenia mają nazwy zleceń, dzięki czemu można łatwo sprawdzić, co zrobić.  

### <a name="sort"></a>Sortowanie

Polecenie Sortuj umożliwia określenie porządku sortowania według jednej lub wielu pól. Nawet jeśli nie używasz, domyślnie, wymuszane jest czas, w kolejności malejącej. Najnowsze wyniki są zawsze w górnej części wyników wyszukiwania. Oznacza to, że po uruchomieniu wyszukiwania z `Type=Event EventID=1234` co naprawdę wykonaniu dla Ciebie jest:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Jest tak, ponieważ jest typ obsługi, które znasz w dziennikach. Na przykład w Podglądzie zdarzeń systemu Windows.

Zmienianie sposobu rozmieszczenia zwracane wyniki są umożliwia sortowanie. W poniższych przykładach pokazano, jak to działa.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Prosta przykładach pokazano, działania polecenia — zmieniają one kształt wyników, które zwracane filtr.

### <a name="limit-and-top"></a>Limit i od góry
Inne mniej znane polecenie jest ograniczona. Limit wynosi czasownikowe przypominających programu PowerShell. Limit wynosi funkcjonalny identyczne z poleceniem GÓRNEGO. Następujące kwerendy zwracają ten sam efekt.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Aby wyszukać góry
- W polu wyszukiwania kwerendy wpisz`Type=Event EventID=600 | Top 1`   
    ![Wyszukiwanie góry](./media/log-analytics-log-searches/oms-search-top.png)

Na powyższym obrazie są tysiąc 358 rekordów zawierających identyfikator zdarzenia = 600. Pola, aspekty i filtry po lewej stronie zawsze Pokaż informacji na temat wyniki zwrócone *przez część filtru* kwerendy, w której znajduje się przed znakiem potoku. Okienko **wyników** zwraca tylko najnowsze wynik 1, ponieważ polecenie przykładowe kształcie i przekształcania wyniki.

### <a name="select"></a>Wybierz pozycję

Wybierz polecenie zachowuje się jak Select-Object w programie PowerShell. Zwraca filtrowanych wynikach, których nie ma wszystkich swoich oryginalnych właściwości. Wybiera tylko wybrane właściwości.

#### <a name="to-run-a-search-using-the-select-command"></a>Aby uruchomić wyszukiwanie przy użyciu polecenia select

1. W polu Wyszukaj wpisz `Type=Event` , a następnie kliknij przycisk **Wyszukaj**.
2. Kliknij przycisk **+ Pokaż więcej** w jednym z wyników, aby wyświetlić wszystkie właściwości, które mają wyniki.
3. Wybierz niektóre z tych jawnie i kwerenda zmieni się na `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Wybierz pozycję wyszukiwania](./media/log-analytics-log-searches/oms-search-select.png)

Jest to polecenie szczególnie przydatne, gdy chcesz kontrolować wynik wyszukiwania i wybierz pozycję tylko części danych, które naprawdę znaczenia dla podróży często nie jest pełna rekordu. Jest to przydatny również wtedy, gdy rekordy różnych typów mają *pewne* wspólne właściwości, ale nie *Wszystkie* ich właściwości są wspólne. Generować dane wyjściowe więcej w sposób naturalny wygląda tabeli lub pracę oraz, jeśli eksportowane do pliku CSV, a następnie sprasowany w programie Excel.



## <a name="use-the-measure-command"></a>Za pomocą polecenia miary

MIARA jest jednym z najbardziej wszechstronna poleceń w wyszukiwaniach z użyciem analizy dziennika. Umożliwia stosowanie statystycznych *funkcji* do danych i agregacji wyników pogrupowane według danego pola. Istnieje wiele funkcji statystycznych, obsługiwanych przez miary.

### <a name="measure-count"></a>Zmierz count()

Pierwsza funkcja statystyczne do pracy z, a spośród najłatwiejszym zapoznać się z jest funkcja *count()* .

Wyniki z dowolnej kwerendy wyszukiwania, takich jak `Type=Event`, Pokaż filtry skrót aspekty po lewej stronie wyników wyszukiwania. Filtry pokazywania wartości za pomocą danego pola wyników w wyszukiwania.

![Liczba miary wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Na przykład na ilustracji powyżej pojawi się pole **komputera** i jego wskazuje, że w zdarzeń niemal 739 tysiąc w wynikach 68 wartości unikatowych i różnią się w polu **komputera** w tych rekordach. Kafelek tylko Wyświetla 5 pierwszych, które są najczęściej używane wartości 5, które są zapisywane w polach **komputera** ), posortowane według liczbę dokumentów, które zawierają tej wartości określonej w tym polu. Na ilustracji widać, że — wśród nich niemal 369 tysiąc — tysiąc 90 pochodzić z komputera OpsInsights04.contoso.com, tysiąc 83 z komputera DB03.contoso.com i tak dalej.


Co zrobić, jeśli chcesz wyświetlić wszystkie wartości, ponieważ fragmentu jest wyświetlana, tylko tylko 5 pierwszych?

To polecenie miary możliwości przez funkcję count(). Ta funkcja nie używa żadnych parametrów. Określ tylko pola, według których mają być grupowane według — pole **komputera** w tym przypadku:

`Type=Event | Measure count() by Computer`

![Liczba miary wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Jednak **komputer** jest po prostu pole używane *w* każdy fragment dane — nie relacyjne bazy danych są związane i nie ma żadnego osobnych **komputera** obiektu dowolnego miejsca. Tylko wartości *w* serii danych można opisać które jednostki wygenerowane je i wiele innych cech i cechy danych — w związku z tym terminów *Faseta*. Jednak także tylko można grupować za pomocą innych pól. Ponieważ oryginalne wyniki niemal 739 tysiąc zdarzeń, które są przekazywane w potoku do polecenia miary mają także pole o nazwie **identyfikator zdarzenia**, możesz zastosować sam sposób, aby Grupuj według tego pola i liczba wydarzeń według identyfikator zdarzenia:

```
Type=Event | Measure count() by EventID
```

Jeśli nie interesują Cię liczba rzeczywista rekordów, zawierające określoną wartość, ale zamiast tego Jeśli chcesz tylko listy same wartości, możesz dodać polecenie *Wybierz* na końcu tego i po prostu wybierz pierwszą kolumnę:

```
Type=Event | Measure count() by EventID | Select EventID
```

Następnie można uzyskać bardziej skomplikowanych i wstępnie Sortuj wyniki w kwerendzie, lub możesz kliknąć pozycję kolumny w siatce zbyt.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Aby wyszukać Statystyka miary

- W polu wyszukiwania kwerendy wpisz`Type=Event | Measure count() by EventID`
- Dołączanie `| Select EventID` na końcu kwerendy.
- Na koniec dołączanie `| Sort EventID asc` na końcu kwerendy.


Istnieje kilka ważnych uwag na Zwróć uwagę i wyróżnienie:

Najpierw zostanie wyświetlony wynik nie są oryginalne wyniki nieprzetworzonych już. Zamiast tego są one wyniki zagregowane — przede wszystkim grupy wyników. Nie jest to problem, ale należy się zapoznać, jest interakcja bardzo inny kształt danych, który różni się od oryginalnego nieprzetworzonych kształt, który jest tworzony w czasie rzeczywistym w wyniku funkcji agregacji i statystycznych.

Drugi **Zliczanie miary** obecnie zwraca tylko wyniki odrębnych 100 pierwszych. Ten limit nie dotyczą inne funkcje statystyczne. Tak zwykle musisz najpierw zastosować filtr bardziej precyzyjne, aby wyszukać określone elementy przed zastosowaniem count() miary.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Przy użyciu polecenia miary należy użyć funkcji min i max

Istnieją różne scenariusze, w którym **Max() miary** i **Min() miary** są przydatne. Jednak ponieważ każda funkcja jest przeciwne od siebie, możemy przedstawić Max() i możesz poeksperymentować z Min() własne.

Jeśli kwerenda zdarzeń zabezpieczeń, mają właściwość **poziom** , który może być różna. Na przykład:

```
Type=SecurityEvent
```

![Rozpocznij zliczanie miary wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Jeśli chcesz wyświetlić najwyższe wartości dla wszystkich zabezpieczenia zdarzeń podane komputera z programem, Grupuj według pola, możesz użyć

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Wyszukiwanie miary max komputera](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Będzie ono wyświetlania dla komputerów, które zawierają **poziom** rekordów, większość z nich że co najmniej poziom 8, wiele sprzedał poziomu 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maksymalny czas miary wyszukiwania wygenerowane komputera](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Ta funkcja działa również z liczbami, ale współpracuje ona także z pól daty i godziny. Warto wyszukać ostatniego lub ostatnio sygnaturę czasową dowolne dane indeksowane na każdym komputerze. Na przykład: kiedy ostatnio zdarzeń zabezpieczeń zgłoszono dla każdego komputera?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Funkcja avg za pomocą polecenia miary

Funkcji statystycznych Avg() używane miarą pozwala na obliczanie średniej wartości dla niektórych pól i grupowanie wyników za pomocą pola tym samym lub innym. To jest przydatne w wielu przypadkach, na przykład dane dotyczące wydajności.

Zaczniemy od dane dotyczące wydajności. Należy zauważyć, że usługi OMS obecnie zbiera liczniki wydajności na komputerach w systemach Windows i Linux.

Aby wyszukać *Wszystkie* dane dotyczące wydajności, najbardziej podstawowa kwerenda jest:

```
Type=Perf
```

![Rozpocznij avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg01.png)

Jest najpierw można zauważyć, że analizy dziennika zawiera trzy perspektyw: listy, którą przedstawiono przedstawia rzeczywisty rekordów za wykresów; Tabela, w której przedstawiono tabelaryczny dane licznika wydajności; i miar, która jest wyświetlana, wykresy liczników wydajności.

Na powyższym obrazie istnieją dwa zestawy pola oznaczone wskazujące następujące czynności:

- Pierwszy zestaw identyfikuje nazwę licznika wydajności systemu Windows, nazwy obiektu i nazwę wystąpienia w filtrze kwerendy. Są to pola, które prawdopodobnie będą najczęściej jako aspekty i filtry
- **Równowartości** jest wartością rzeczywistą licznika. W tym przykładzie wartość jest *75*.
- **TimeGenerated** jest 12:51 w formacie 24-godzinnym.

Oto widok metryki na wykresie.

![Rozpocznij avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg02.png)

Po odczytu o kształcie rekordu wydajności i konieczności przeczytaj o innych technik wyszukiwania miara Avg() umożliwia agregowanie danych liczbowych tego typu.

Oto prosty przykład:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Wyszukiwanie avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

W tym przykładzie możesz wybrać działanie całkowity czas Procesora licznik i średnia przez komputer. Jeśli chcesz zawęzić wyniki do ostatniej 6 godzin, możesz za pomocą formant filtru czasu lub określ w kwerendzie w następujący sposób:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Aby wyszukać przy użyciu funkcji średnia z poleceniem miary
- W polu wyszukiwania kwerendy wpisz `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Możesz agregacji i grupowania danych *na* komputerach. Załóżmy na przykład, że masz zestawu hostów w jakąś farmy, gdzie każdy węzeł jest równa do dowolnego innego tylko te same wpisaniu pracy i należy około równoważenia obciążenia. Można uzyskać ich liczniki przekierowane w jednym z następujących czynności kwerend oraz średnich dla całej farmy. Możesz rozpocząć, wybierając pozycję komputerach z następującym przykładzie:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Teraz, gdy masz komputerów tylko chcesz zaznaczyć dwie kluczowe wskaźniki wydajności (KPI): % użycie Procesora i % wolnego miejsca na dysku. Tak staje się tę część kwerendy:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Teraz możesz dodać komputerów i liczniki z następującym przykładzie:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Ponieważ masz bardzo określonego zaznaczenia, polecenie **miary Avg()** można przywrócić średnia nie przez komputer, ale farmie, po prostu grupowanie na podstawie CounterName. Na przykład:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Umożliwia przydatne Widok kompaktowy kilka kluczowych wskaźników wydajności środowiska usługi.

![Grupowanie avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg04.png)


Łatwe za pomocą kwerendy wyszukiwania na pulpicie nawigacyjnym. Na przykład możesz zapisać kwerendę wyszukiwania i Tworzenie pulpitu nawigacyjnego z niego o nazwie *Kluczowych wskaźników wydajności sieci Web farmy*. Aby dowiedzieć się więcej o korzystaniu z pulpitów nawigacyjnych, zobacz [Tworzenie niestandardowego pulpitu nawigacyjnego do analizy dziennika](log-analytics-dashboards.md).

![pulpit nawigacyjny avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Używanie funkcji Suma z poleceniem miary

Funkcja suma jest podobne do innych funkcji polecenia miary. Można zobaczyć przykład o korzystaniu z funkcji Suma [W3C usług IIS dzienniki](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx)wyszukiwanie w Microsoft Azure operacyjne wnioski.

Za pomocą Max() i Min() z liczby, daty i godziny i ciągi tekstowe. Ciągi tekstowe są sortowane alfabetycznie i uzyskiwanie pierwsze i ostatnie.

Nie można jednak używać Sum(), z wyjątkiem pola liczbowe. Dotyczy to również Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Funkcja PERCENTYL za pomocą polecenia miary

Percentyl, funkcja jest podobna do Avg() i Sum(), w tym można używać tylko go dla pól liczbowych. Możesz użyć dowolnego percentyl między 1 do 99 na pola liczbowego. Można również użyć poleceń zarówno **Percentyl** , jak i **pct** . Oto kilka przykładów:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Gdzie za pomocą polecenia

W przypadku, gdy polecenie działa jak filtr, ale mogą być stosowane w potoku dodatkowo filtrować wyniki zagregowane, oferowanych przez polecenie miary — zamiast wyników nieprzetworzonych którego są filtrowane na początku kwerendy.

Na przykład:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Możesz dodać drugim "|" znaków i gdzie polecenie tylko w trybie komputerach, których średnia Procesora jest ponad 80% z następującym przykładzie:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Jeśli znasz program Microsoft System Center - programu Operations Manager można traktować polecenia where w zakresie zarządzania dodatkiem Service pack. Przykład gdyby reguły pierwsza część kwerenda będzie źródła danych i gdzie polecenie ma wykrywania warunku.

Umożliwia kwerendy jako kafelka w **Moim pulpitu nawigacyjnego**jako monitor typu, sprawdzanie, kiedy procesorów komputera są wykorzystywane nadmiernie. Aby dowiedzieć się więcej na temat pulpitów nawigacyjnych, zobacz [Tworzenie niestandardowego pulpitu nawigacyjnego do analizy dziennika](log-analytics-dashboards.md). Można także tworzyć i używać pulpitów nawigacyjnych za pomocą aplikacji dla urządzeń przenośnych. Aby uzyskać więcej informacji zobacz [Aplikacji usługi OMS Mobile ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). W dolnej dwóch fragmentów poniższy obraz, możesz zobaczyć monitor wyświetlania listy, a jako liczba. Przede wszystkim zawsze ma wartość zero i z listy, aby być puste. W przeciwnym razie wskazuje warunek alertów. W razie potrzeby można użyć go do podglądu na komputery, które są w obszarze ciśnienia.

![pulpit nawigacyjny urządzeń przenośnych](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Użyj operatora in

Operator *w* wraz z *NOT IN* umożliwia używanie subsearches, które wyszukiwania, które obejmują Przeszukaj jako argument. Znajdują się w nawiasami klamrowymi {} w innej wyszukiwania *podstawowego* lub *zewnętrznego* . Wynik subsearch, często listy odrębnych wyników, następnie jest używany jako argument w jego podstawowego wyszukiwania.

Za pomocą subsearches zgodnie z podzbiorów danych, że nie da się opisać bezpośrednio w wyrażeniu wyszukiwania, ale mogą być generowane z wyników wyszukiwania. Na przykład jeśli Interesujesz się przy użyciu jednego wyszukiwania, aby znaleźć wszystkie zdarzenia z *komputerów brakujących aktualizacji zabezpieczeń*, należy zaprojektować subsearch, identyfikujący najpierw tego *komputerach brakujących aktualizacji zabezpieczeń* , zanim znajdzie wydarzeń należących do tych hostów.

Tak express *komputerach obecnie brakujących aktualizacji zabezpieczeń wymagane* z następującej kwerendy:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Po utworzeniu listy umożliwia wyszukiwanie jako wewnętrzne wyszukiwanie kanału listę komputerów w zewnętrznym wyszukiwania (podstawowe), który będzie wyglądać na zdarzenia dla komputerów. W tym celu załączenie wewnętrzne wyszukiwania w nawiasach klamrowych i podawania jej wyniki jako możliwe wartości-pole filtru w polu Wyszukaj zewnętrzne korzystanie z operatora IN. Kwerenda będzie wyglądać:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Również powiadomienie filtru czasu używane w wyszukiwaniu wewnętrzne, ponieważ ocenę aktualizacji systemu pobiera migawkę wszystkich komputerów co 24 godziny. Można tworzyć zapytania wewnętrzne najprostsze i bardziej precyzyjne przez wyszukiwanie tylko na dzień. Zewnętrzne wyszukiwania użyje wyboru strefy czasowej w interfejsie użytkownika odzyskiwania zdarzeń z ostatnich 7 dni. Aby uzyskać więcej informacji o czasie operatorów, zobacz [operatorów logicznych](#boolean-operators) .

Ponieważ możesz naprawdę zastosować tylko wyniki wyszukiwania wewnętrzne jako filtr wartości dla nich zewnętrzne, nadal możesz zastosować polecenia w polu Wyszukaj w zewnętrznym. Nadal można na przykład grupować powyższe zdarzenia z innego polecenia miary:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Zazwyczaj ma wewnętrzne zapytanie szybko wykonać, ponieważ analizy dziennika zawiera strony usługi przekroczenia limitu czasu, a także zwraca niewielkiej ilości wyników. Jeśli wewnętrzne kwerenda zwraca więcej wyników, na liście wyników obcinana, potencjalnie może powodować zewnętrzne wyszukiwania zwraca niepoprawne wyniki.

Inna reguła to, że wewnętrzne wyszukiwania musi obecnie dostarczania wyników *zagregowane* . Innymi słowy musi ona zawierać polecenia *miary* ; Nie można obecnie ranna nieprzetworzonych wyników w zewnętrznym wyszukiwania.

Ponadto może istnieć tylko jeden operatora IN i musi to być ten filtr w kwerendzie. Nie może być wielu operatorów w lub czy to zasadniczo uniemożliwia uruchamianie wielu subsearches: istotne jest że tylko jedna wyszukiwanie sub/wewnętrzna jest możliwe każdego zewnętrzne wyszukiwania.

Nawet w przypadku te limity w umożliwia wielu rodzajów skorelowany wyszukiwania i pozwala na zdefiniowanie podobnej do grup, takich jak komputery, użytkownicy lub pliki — niezależnie od pola danych zawierają. Poniżej przedstawiono więcej przykładów:

**Wszystkie aktualizacje brakuje komputerach wyłączonym ustawienia aktualizacji automatycznych**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Wszystkie zdarzenia błędu na komputerach z programem SQL Server (= miejsce, w którym zostało uruchomione ocena SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Wszystkie zdarzenia zabezpieczeń z komputerów, które są kontrolerami domeny (= miejsce, w którym zostało uruchomione ocena AD)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Które innych kont jest zalogowany na tym samym komputerach, gdzie konta BACONLAND\jochan zalogował się?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Za pomocą polecenia różnią się

Jak wynika z nazwy, to polecenie zawiera listę unikatowych wartości dla pola. Jest zaskakująco proste, ale bardzo przydatny. Taki sam może osiągnąć przy użyciu polecenia count() miary także, jak pokazano poniżej.

```
Type=Event | Measure count() by Computer
```

![Przykład polecenia RÓŻNIĄ się wyszukiwania](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Jeśli jednak wszystkie Cię interesuje jest po prostu listę unikatowych wartości, a nie liczbę dokumentów zawierających wartości, a następnie DISTINCT umożliwiają przejrzystość i czytelność dane wyjściowe i krótszej składni, jak pokazano poniżej.

```
Type=Event | Distinct Computer
```
![Przykład polecenia RÓŻNIĄ się wyszukiwania](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Funkcja countdistinct przy użyciu polecenia miary
Funkcja countdistinct zlicza liczba różnych wartości w każdej grupie. Na przykład może być używane zliczania unikatowych komputerów raportowania dla każdego typu:

```
* | measure countdistinct(Computer) by Type
```

![Usługi OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Za pomocą polecenia interwału miary
Z u zbieranie danych dotyczących wydajności w czasie rzeczywistym, można zbierać i wizualizowanie dowolnego licznika do analizy dziennika. Wprowadzenie kwerendy, którą **Typ: wydajności** zwróci tysięcy metryczne wykresów na podstawie liczby liczniki i serwery w środowisku analizy dziennika. Z agregacją metryki na żądanie może przeglądać ogólnego metryki w środowisku wysokiego poziomu i głębokości dive do bardziej szczegółowego danych potrzebnych do.

Załóżmy, że chcesz wiedzieć, co to jest średnia Procesora na swoich komputerach. Spojrzenie na Średnia Procesora na każdym komputerze może nie być przydatne, ponieważ może uzyskać wygładzania wyników. Aby ustalić do szczegółowe informacje, można zagregować wynik w mniejszym czasu fragmentów okna i Znajdź w szeregu czasowego różnych wymiarów. Na przykład można wykonywać godzinowe średnią użycie Procesora na swoich komputerach w następujący sposób:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Miara średnia interwału](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Domyślnie wyniki te będą wyświetlane na wykresie liniowym interakcyjnych wielu serii.  Ten wykres obsługuje serii przełączanie (z ponowne skalowanie oś y), powiększanie i umieszczenie wskaźnika myszy.  Opcja wyświetlania tabeli jest nadal dostępne w celu wyświetlania nieprzetworzonych danych, jeśli to konieczne.

Można również grupować za pomocą innych pól. W tym przykładzie wyświetlane wszystkie liczniki % dla jednego komputera określonych i mają zostać wie, co jest percentylu co godzina 70 każdej licznika:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Jedyne pamiętać, to, że te zapytania nie są ograniczone do liczników wydajności. Można je zastosować do dowolnego metryki. W tym przykładzie wyświetlane dzienniki programu IIS W3C. Chcę dowiedzieć się, co to jest maksymalny czas potrzebny na 5 minut dla każdego żądania przetwarzania:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Używanie wielu agregacji w jednej kwerendy
Możesz określić wiele klauzul agregacji w poleceniu miary.  Każdy z nich może być aliasu niezależnie.  Jeśli nie jest podany alias wyniku Nazwa pola będzie funkcji agregującej, który był używany (to znaczy "avg(CounterValue)" dla avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![Usługi OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Oto kolejny przykład:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe informacje na temat wyszukiwania dziennika Zobacz:

- Aby rozszerzyć wyszukiwanie dziennika za pomocą [pól niestandardowych do analizy dziennika](log-analytics-custom-fields.md) .
- Przeglądanie [analizy dziennika logowania odwołania wyszukiwania](log-analytics-search-reference.md) Aby wyświetlić wszystkie pola wyszukiwania i aspektów, które są dostępne w dzienniku analizy.
