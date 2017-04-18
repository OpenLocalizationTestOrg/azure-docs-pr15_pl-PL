<properties 
    pageTitle="DocumentDB model hierarchii zasobów i pojęć | Microsoft Azure" 
    description="Informacje na temat DocumentDB w modelu hierarchicznych baz danych, zbiorów, funkcja zdefiniowana przez użytkownika (UDF), dokumentów, uprawnień do zarządzania zasobami i nie tylko."
    keywords="Hierarchiczne modelu, documentdb azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Model hierarchii zasobów DocumentDB i pojęć

Obiekty bazy danych, które zarządza DocumentDB są określane jako **zasoby**. Każdemu zasobowi jednoznacznie identyfikuje logiczne identyfikatora URI. Możliwa jest interakcja z zasobami przy użyciu standardowych zleceń HTTP, nagłówki żądanie/odpowiedź i kody stanu. 

Przez przeczytaniem tego artykułu, będziesz mieć możliwość odpowiedzieć na następujące pytania:

- Co to jest model zasobów firmy DocumentDB?
- Co to jest system zdefiniowany zasobów zamiast zasobów zdefiniowane przez użytkownika?
- Sposób rozwiązania zasobu?
- Jak pracować z kolekcji
- Jak pracować z procedur składowanych, wyzwalaczami i funkcje zdefiniowane przez użytkownika (UDF)?

## <a name="hierarchical-resource-model"></a>Model hierarchii zasobów
Jak na poniższym diagramie przedstawiono, hierarchicznych DocumentDB **modelu zasobów** składa się z zestawów zasobów w ramach konta bazy danych, każdy adresach za pomocą identyfikatora URI logicznej i stałe. Zestaw zasobów będą określane jako **kanału informacyjnego** w tym artykule. 

>[AZURE.NOTE] DocumentDB oferuje skuteczniejszej protokołu TCP, który jest również RESTful w jego komunikacji modelu dostępne za pośrednictwem programu [.NET klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).

![Model hierarchii zasobów DocumentDB][1]  
**Model hierarchii zasobów**   

Aby rozpocząć pracę z zasobami, musisz najpierw [utworzyć konto DocumentDB bazy danych](documentdb-create-account.md) , korzystając ze swojej subskrypcji Azure. Konto bazy danych może zawierać zestaw **baz danych**danych, z zawierające wielu **zbiorów**, z których każdy z kolei zawierają **przechowywanych procedur, wyzwalaczy, funkcji zdefiniowanych przez użytkownika, dokumenty** i powiązane z nim **Załączniki** (funkcji podglądu). Bazy danych jest również skojarzony **użytkowników**, każda z zestawu **uprawnień** dostępu do kolekcji, procedur składowanych, wyzwalaczy, funkcji zdefiniowanych przez użytkownika, dokumenty lub załączników. Baz danych, użytkowników, uprawnień i kolekcje są zdefiniowane przez system zasobów za pomocą znanych schematy, dokumenty i załączniki zawierają dowolnego, JSON zawartości zdefiniowane przez użytkownika.  

|Zasób   |Opis
|-----------|-----------
|Konto bazy danych   |Konto bazy danych jest skojarzona z zestawem baz danych i kwotą stałą magazyn obiektów blob załączników (funkcji podglądu). Możesz utworzyć jeden lub więcej kont bazy danych, korzystając ze swojej subskrypcji Azure. Aby uzyskać więcej informacji odwiedź stronę naszych [ceny strony](https://azure.microsoft.com/pricing/details/documentdb/).
|Bazy danych   |Baza danych jest kontener logiczny podzielona między zbiorami miejsca w dokumencie. Należy również kontenera użytkowników.
|Użytkownika   |Przestrzeń nazw logicznych zakres uprawnień. 
|Uprawnień |Token autoryzacji skojarzone z tym użytkownikiem, aby uzyskać dostęp do określonego zasobu.
|Kolekcja |Kolekcja jest kontenerem JSON dokumentów i skojarzone logiki kodu JavaScript w aplikacji. Kolekcja jest podmiot fakturowanego miejsce, w którym [Koszt](documentdb-performance-levels.md) zależy od poziomu wydajności związane z pobieraniem. Kolekcje może obejmować jeden lub więcej partycje serwerów i można skalować obsługę praktycznie nieograniczoną ilości miejsca do magazynowania lub przepustowość.
|Procedura składowana   |Logika aplikacji napisana JavaScript, który jest zarejestrowany zbioru i transakcyjnie wykonywane w aparat bazy danych.
|Wyzwalacza    |Logikę aplikacji napisana wykonywane przed lub po obu wstawkę kodu JavaScript Zamień lub operacji usuwania.
|UDF    |Logika aplikacji napisana JavaScript. Funkcji zdefiniowanych przez użytkownika umożliwiają modelu operator Kwerenda niestandardowa i tym samym rozszerzanie podstawową DocumentDB języka kwerend.
|Dokument   |Zdefiniowane przez użytkownika (dowolnego) zawartości JSON. Domyślnie Brak schematu należy zdefiniować ani wskaźników pomocniczej, trzeba być dostępne dla wszystkich dokumentów, które są dodawane do kolekcji.
|(Wersja preview) Załącznik   |Załącznik jest dokumentem specjalne zawierającej odwołania i skojarzone metadanych dla obiektów blob zewnętrznych i multimediów. Deweloper można mieć blob zarządzane przez DocumentDB i przechowywać go u usługodawcy blob zewnętrznych, takich jak OneDrive, Dropbox, itp. 


## <a name="system-vs-user-defined-resources"></a>System, a zasoby zdefiniowane przez użytkownika
Zasobów, takich jak konta bazy danych, baz danych, zbiorów, użytkowników, uprawnienia, procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika — mieć stały schematu i są nazywane zasoby systemowe. Natomiast zasobów, takich jak dokumenty i załączniki mają bez ograniczeń w schemacie i przykłady zasobów zdefiniowane przez użytkownika. W DocumentDB zarówno system i zdefiniowane przez użytkownika zasoby są przedstawione i zarządzać jako zgodne z standardowe JSON. Wszystkie zasoby systemowe lub użytkownika, ma poniższe wspólne właściwości.

> [AZURE.NOTE] Uwaga, że wszystkie generowany przez system właściwości w zasobie są prefiksem podkreślenia (_) w ich reprezentacji JSON.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Właściwość</strong></p></td>
            <td valign="top"><p><strong>Można ustawić użytkownika lub generowane przez system?</strong></p></td>
            <td valign="top"><p><strong>Cel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Generowane przez system</p></td>
            <td valign="top"><p>System wygenerowane unikatowe i hierarchicznych identyfikator zasobu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Generowane przez system</p></td>
            <td valign="top"><p>etag zasobu wymagane dla opis pliku pomocy</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Generowane przez system</p></td>
            <td valign="top"><p>Sygnatury czasowej zaktualizowane ostatniego zasobu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Generowane przez system</p></td>
            <td valign="top"><p>Unikatowe adresach URI zasobu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Identyfikator</p></td>
            <td valign="top"><p>Generowane przez system</p></td>
            <td valign="top"><p>Nazwa zasobu (z tym samym partition wartość klucza) zdefiniowane przez użytkownika. Jeśli użytkownik określi identyfikatora, identyfikator będą generowane przez system</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Przedstawienie przewodowy zasobów
DocumentDB nie przesądza wszystkie własnych rozszerzenia do kodowania standardowy lub specjalne JSON; działa on z standardowych zgodny z dokumentów JSON.  
 
### <a name="addressing-a-resource"></a>Adresowanie zasobu
Identyfikator URI adresach są wszystkie zasoby. Wartość właściwości **_self** zasobu reprezentuje względnego identyfikatora URI zasobu. Format identyfikatora URI składa się z-\<kanału\>-segmenty ścieżki {_rid}:  

|Wartość _self |Opis
|-------------------|-----------
|/DBS   |Kanał informacyjny baz danych przy użyciu konta bazy danych
|/DBS/ {dbName}  |Bazy danych za pomocą identyfikatora zgodne z wartością {dbName}
|/colls/ /DBS/ {dbName}   |Kanał informacyjny zbiorów w obszarze bazy danych
|/colls/ /DBS/ {dbName} {collName} |Kolekcji o identyfikatorze zgodne z wartością {collName}
|/colls/ /DBS/ {dbName} {collName}-dokumenty    |Kanał informacyjny dokumentów w kolekcji
|/docs/ /colls/ {collName} /DBS/ {dbName} {identyfikator}    |Dokument z identyfikatora zgodne z wartością {doc}
|/ Użytkownicy / /DBS/ {dbName}   |Kanał informacyjny użytkowników w bazie danych
|/ Użytkownicy / /DBS/ {dbName} {nazwa użytkownika}   |Użytkownik o identyfikatorze zgodne z wartością {user}
|/ Użytkownicy / /DBS/ {dbName} {nazwa użytkownika}-uprawnień   |Kanał informacyjny uprawnień przez użytkownika
|/permissions//użytkownicy / {nazwa użytkownika} /DBS/ {dbName} {permissionId}    |Uprawnień z identyfikatorem zgodne z wartością {uprawnień}
  
Każdemu zasobowi ma nazwę unikatowe zdefiniowane przez użytkownika, dostępne za pomocą właściwości identyfikator. Uwaga: w przypadku dokumentów, jeśli użytkownik określi identyfikator naszych obsługiwane SDK automatycznie generuje unikatowy identyfikator dokumentu. Identyfikator jest ciągiem zdefiniowane przez użytkownika w więcej niż 256 znaków, które są unikatowe w kontekście zasobu określonych nadrzędnej. 

Każdemu zasobowi też ma identyfikator zasobu hierarchicznych generowane przez system (nazywane również RID), który jest dostępny za pomocą właściwości _rid. RID kodowane całą hierarchię danego zasobu i jest wygodny reprezentacji wewnętrznej używany w celu wymuszenia więzów integralności w sposób rozłożone. RID jest unikatowy w ramach konta bazy danych i wewnętrznie jest używany przez DocumentDB dla efektywny routing bez konieczności wyszukiwania partition krzyżowego. Wartości _self i właściwości _rid są zarówno alternatywny i kanonicznego reprezentacji zasobu. 

Interfejsy API pozostałych DocumentDB obsługuje adresowania zasobów i routingu żądań identyfikator i właściwości _rid.

## <a name="database-accounts"></a>Konta bazy danych
Można dodawać konta bazy danych jeden lub więcej DocumentDB korzystając ze swojej subskrypcji Azure.

Możesz [tworzyć i zarządzać kontami bazy danych DocumentDB](documentdb-create-account.md) przez Portal Azure pod adresem [http://portal.azure.com/](https://portal.azure.com/). Tworzenie i zarządzanie nimi konta bazy danych wymaga dostępu administracyjnego i można wykonać tylko w ramach subskrypcji usługi Azure. 

### <a name="database-account-properties"></a>Właściwości konta bazy danych
Podczas inicjowania obsługi administracyjnej i zarządzania kontem bazy danych można skonfigurować i przeczytaj następujące właściwości:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Nazwa właściwości</strong></p></td>
            <td valign="top"><p><strong>Opis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Zasady zgodności</p></td>
            <td valign="top"><p>Ustaw dla tej właściwości, aby skonfigurować domyślny poziom zgodności dla wszystkich zbiorów na koncie bazy danych. Można zastąpić poziomu spójności dla na żądanie przy użyciu nagłówek [x-ms spójności poziom]. <p><p>Należy zauważyć, że ta właściwość dotyczy tylko <i>zasoby zdefiniowane przez użytkownika</i>. System wszystkie zasoby zdefiniowane są skonfigurowane do obsługi odczytuje/kwerendy z silnych spójności.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Klucze autoryzacji</p></td>
            <td valign="top"><p>Są to głównego i pomocniczego wzorca i tylko do odczytu klucze, które zapewniają dostęp administracyjny do wszystkich zasobów na koncie bazy danych.</p></td>
        </tr>
    </tbody>
</table>

Należy zauważyć, że oprócz inicjowania obsługi administracyjnej, konfigurowanie i zarządzanie kontem bazy danych z Azure Portal, możesz również programowy tworzyć i zarządzać DocumentDB konta bazy danych przy użyciu [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) , a także [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Bazy danych
DocumentDB bazy danych jest kontener logiczny jedną lub więcej zbiorów i użytkowników, jak pokazano na poniższym rysunku. Możesz utworzyć dowolną liczbę baz danych na koncie bazy danych DocumentDB limitom oferty.  

![Modelu bazy danych konta i zbiorów hierarchicznych][2]  
**Baza danych jest kontener logiczny użytkowników i kolekcji**

Baza danych może zawierać przechowywania dokumentów nieograniczoną podzielona według kolekcji, które tworzą domen transakcji dla dokumentów znajdujących się w nich. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Elastyczne skali DocumentDB bazy danych
Elastyczne domyślnie — od kilku GB petabytes SSD kopii przechowywania dokumentów i ustanawianie przepustowość jest DocumentDB bazy danych. 

W przeciwieństwie do bazy danych w tradycyjnych RDBMS bazy danych w DocumentDB nie jest ograniczone do jednego komputera. Z DocumentDB skali aplikacji potrzeb się rozrośnie, można utworzyć więcej zbiorów i baz danych. W rzeczywistości różne pierwszej aplikacji innych firm w programie Microsoft korzystają DocumentDB w skali dla klientów indywidualnych, tworząc bardzo duże DocumentDB każdego zawierające tysiące zbiory przy użyciu terabajtów miejsca w dokumencie. Możesz powiększanie lub zmniejszanie bazy danych przez dodanie lub usunięcie zbiorów do wymagań skali aplikacji. 

Możesz utworzyć dowolną liczbę kolekcji w bazie danych objęte oferty. Każdego zbioru ma SSD kopii miejsca do magazynowania i przepustowość zainicjowany dla Ciebie w zależności od poziomu zaznaczonego wydajności.

DocumentDB bazy danych jest również kontenera użytkowników. Użytkownik, w ruchu jest logiczny obszar nazw dla zestawu uprawnień zawiera szczegółowe autoryzacji i dostęp do zbiorów, dokumentów i załączniki.  
 
Podobnie jak w przypadku innych zasobów w modelu zasobów DocumentDB baz danych można utworzyć, zastąpione, usunięte, przeczytaj lub wyliczane łatwo przy użyciu [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub dowolną [SDK klienta](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB gwarantuje silnych spójność do czytania lub wykonywania zapytań na metadanych zasobu bazy danych. Automatyczne usuwanie bazy danych gwarantuje, że nie masz dostępu do kolekcji lub użytkowników, zawarte w nim.   

## <a name="collections"></a>Kolekcje
Kolekcja DocumentDB jest kontenera dla dokumentów JSON. Kolekcja jest również jednostka skali transakcji i kwerend. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastyczne SSD kopii przechowywania dokumentów
Kolekcja jest bardzo elastyczne — automatycznie powiększa się i zmniejsza się, jak można dodawać i usuwać dokumenty. Kolekcje logiczne zasobów i może obejmować fizycznie partycje lub serwerów. Liczba partycje w zbiorze zależy od DocumentDB na podstawie rozmiaru miejsca do magazynowania i przepustowość ustanawianie zbioru. Co partition w DocumentDB ma stałą przechowywania kopii SSD skojarzone z nim i są replikowane wysokiej dostępności. Zarządzanie partycją jest w pełni zarządzane przez Azure DocumentDB, a nie masz celu pisania kodu złożonych oraz zarządzanie usługi partycje. Kolekcje DocumentDB są **praktycznie nieograniczoną** pod względem miejsca do magazynowania i przepustowość. 

### <a name="automatic-indexing-of-collections"></a>Automatyczne indeksowanie zbiory
DocumentDB jest system PRAWDA wolny schematem bazy danych. Nie przyjęto założenie, lub Wymagaj dowolnego schematu dla dokumentów JSON. Po dodaniu dokumentów do kolekcji DocumentDB zostaną automatycznie zindeksowane je i są dostępne do kwerendy. Automatyczne indeksowanie dokumenty bez konieczności schematu lub indeksów pomocniczych jest klucza możliwości DocumentDB i jest włączona za pomocą technik konserwacji indeks zoptymalizowane zapisu, zwolnienia blokady i strukturę dziennika. DocumentDB obsługuje stałej liczby bardzo szybkie zapisywanie podczas nadal serwowania spójne kwerendy. Dokument programu i indeks przestrzeni dyskowej są używane do obliczania przestrzeni dyskowej, zużywanej przez każdego zbioru. Możesz sterować przechowywania i wydajność skutków skojarzone z indeksowania przez skonfigurowanie zasad indeksowania dla zbioru. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Konfigurowanie zasad indeksowania kolekcji
Zasady indeksowania każdego zbioru umożliwia nawiązywanie wydajności i miejsca do magazynowania korzystnych rozwiązań skojarzone z indeksowania. Dostępne w ramach indeksowania konfiguracji są następujące opcje:  

-   Wybierz, czy kolekcji automatycznie indeksy wszystkich dokumentów, lub nie. Domyślnie wszystkie dokumenty są automatycznie indeksowane. Możesz wyłączyć automatyczne indeksowanie i selektywne dodawanie tylko określonych dokumentów do indeksu. I odwrotnie selektywne można wykluczyć tylko określonych dokumentów. Można to osiągnąć przez ustawienie właściwości automatyczne być wartość PRAWDA lub FAŁSZ na indexingPolicy zbioru i użycie nagłówek [x-ms-indexingdirective] podczas wstawiania, zamienianie lub usuwanie dokumentu.  
-   Określ, czy chcesz uwzględnić lub wykluczyć określone ścieżki lub desenie w dokumentach z indeksu. Można uzyskać to ustawienie includedPaths i excludedPaths na indexingPolicy zbioru odpowiednio. Można również skonfigurować przechowywania i wydajność skutków zakres i mieszania kwerend dla określonej ścieżki wzorców. 
-   Wybieranie między synchroniczne (spójne) i aktualizacje asynchroniczne indeks (opóźnieniem). Domyślnie indeks jest aktualizowana synchronicznie na każdym Wstawianie, Zamień lub Usuń dokument do kolekcji. Dzięki temu kwerendy przestrzegać tego samego poziomu spójności, który odczytuje dokumentu. A DocumentDB jest zapisu zoptymalizowana pod obsługuje stałej ilości zapisu dokumentu oraz konserwacji synchroniczne indeks i obsługujących spójne kwerend, można skonfigurować określonych zbiorów lazily zaktualizować ich indeksu. Indeksowanie z opóźnieniem zwiększa wydajność zapisu dodatkowo i doskonale nadaje się do scenariusze spożyciu zbiorcze przede wszystkim czytania dużej zbiorów.

Zasady indeksowania mogą być zmieniane przez wykonywanie położenie w zbiorze. Można to osiągnąć przez [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx) [Azure Portal](https://portal.azure.com) lub [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Kwerenda kolekcji
Dokumenty w obrębie zbioru mogą mieć dowolnego schematów i bez ich ani schematu pomocniczy indeksy ustalonymi kwerendy można dokumentów w zbiorze. Kwerendy można zbioru przy użyciu [składni języka SQL DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)zawierająca sformatowany hierarchicznych, relacyjnych i przestrzenna operatory i rozszerzania za pośrednictwem oparte na JavaScript funkcji zdefiniowanych przez użytkownika. Gramatyki JSON umożliwia modelowanie JSON dokumentów jako drzew z etykietami jako węzły drzewa. Jest to wykorzystać zarówno za pomocą technik automatycznego indeksowania DocumentDB osoby, a także osoby DocumentDB SQL przy wybranej opcji. Język kwerend DocumentDB składa się z trzech głównych aspektach:   

1.  Niewielki zestaw operacji kwerendy, które mapowanie sposób naturalny drzewa, łącznie z hierarchii kwerend i prognozy. 
2.  Podzestaw relacyjnych operacji, w tym skład, filtr, planów, agregacji i samodzielnie sprzężenia. 
3.  Wyłącznie JavaScript podstawie funkcji zdefiniowanych przez użytkownika, które współpracują z (1) i (2).  

Pozwala podjąć próbę zrozumiałe funkcjonalności, wydajności i uproszczenia DocumentDB modelu kwerendy. Aparat bazy danych DocumentDB oryginalnie skompilowany i wykonany instrukcji SQL kwerendy. Kwerendy można zbioru przy użyciu [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub na dowolnej stronie [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). .NET SDK pochodzi z dostawcą LINQ.

> [AZURE.TIP] Możesz wypróbować DocumentDB i uruchom na naszych zestawu danych w [Playground zapytania](https://www.documentdb.com/sql/demo)SQL kwerendy.

### <a name="multi-document-transactions"></a>Transakcje wiele dokumentów
Transakcje bazy danych Podaj model programowania bezpiecznych i przewidywalne zajmujących się równoczesne zmiany danych. W RDBMS tradycyjny sposób tworzenia reguł biznesowych jest pisanie **procedury składowane** i/lub **wyzwalaczami** i wyślij go do serwera baz danych dla realizacji transakcji. W RDBMS programistom jest wymagane do postępować z dwoma różnymi językami programowania: 

- Programowania języka (JavaScript, Python, C#, Java, itp.) aplikacji (non transakcji)
- T-SQL, transakcji język programowania, który oryginalnie jest wykonywane przez bazę danych

Na podstawie głębokości zaangażowanie JavaScript i JSON bezpośrednio z poziomu aparatu bazy danych DocumentDB przewiduje intuicyjny model programowania wykonywanie kodu JavaScript podstawie logiki aplikacji bezpośrednio na zbiory procedur składowanych i wyzwalaczy. Dzięki temu obie z następujących czynności:

- Sterowanie skutecznego wprowadzenia w życie współbieżności, odzyskiwania automatycznego indeksowania wykresów obiektów JSON bezpośrednio w aparat bazy danych
- Wyrażanie w sposób naturalny przepływ sterowania, zmiennych zakresy, przydział i integracji obsługi pierwotnych z transakcje bazy danych bezpośrednio w odniesieniu do JavaScript języka programowania wyjątków

Logika JavaScript zarejestrowana na poziomie zbioru następnie może wydawać operacji bazy danych w dokumentach określonej kolekcji. DocumentDB niejawnie był zawijany JavaScript, na podstawie procedur przechowywanych i wyzwalaczy w otoczeniu kwasu transakcje izolacji migawki wszystkich dokumentów w zbiorze. W trakcie jego realizacji Jeśli kod JavaScript generuje wyjątek, następnie całej transakcji zostało przerwane. Wynikowa modelu programowania jest bardzo proste jeszcze zaawansowanych. Deweloperów języka JavaScript Uzyskaj "trwałe" model programowania jednocześnie nadal korzystając ich konstrukcji języku i pierwotnych biblioteki.   

Możliwość wykonywanie kodu JavaScript bezpośrednio z poziomu aparatu bazy danych, w tym samym miejscu adres jako puli buforów umożliwia performant i transakcji wykonywania operacji w bazie danych przed dokumenty zbioru. Ponadto DocumentDB aparat bazy danych umożliwia zobowiązanie głębokości do JSON i JavaScript eliminuje wszelkie impedancji niezgodności między systemów typ aplikacji i bazy danych.   

Po utworzeniu zbioru, można zarejestrować procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika zbioru przy użyciu [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub na dowolnej stronie [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). Po zarejestrowaniu możesz odwołać i wykonywanie ich. Należy rozważyć następujące procedury składowanej całkowicie napisana JavaScript, kod poniżej przyjmuje dwa argumenty (Nazwa książki i imię i nazwisko autora) i tworzy nowy dokument, kwerendy w dokumencie i następnie aktualizuje go — wszystko niejawnej transakcji kwasu. W dowolnym momencie podczas wykonywania Jeśli JavaScript wyjątku, cała transakcja przerwana.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Klienta można "wysłać" powyżej logiki JavaScript do bazy danych do realizacji transakcji za pośrednictwem protokołu HTTP wpisu. Aby uzyskać więcej informacji o korzystaniu z metod HTTP zobacz [RESTful interakcji z zasobami DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Zwróć uwagę, że ponieważ bazy danych rozumie oryginalnie JSON i JavaScript, jest nie niezgodności typów systemu, nie "Mapowania" lub"lub Magia generowanie kodu wymagane.   

Procedury przechowywane i wyzwalacze interakcja z kolekcji i dokumentów w zbiorze za pośrednictwem modelu przejrzyste obiekt, który udostępnia bieżącego kontekstu zbioru.  

Zbiorów w DocumentDB można utworzyć, usunięte, przeczytaj lub wyliczane łatwo za pomocą [Interfejsów API pozostałych DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub na dowolnej stronie [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB zawsze zapewnia silne spójność do czytania lub wykonywania zapytań na metadanych kolekcji. Automatyczne usuwanie zbioru gwarantuje, że możesz uzyskać dostępu do dokumentów, załączniki, procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika w nim zawarte.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Procedury składowane, wyzwalaczami i funkcje zdefiniowane przez użytkownika (UDF)
Zgodnie z opisem w poprzedniej sekcji, można napisać logiki aplikacji, aby uruchomić bezpośrednio z poziomu transakcji wewnątrz aparat bazy danych. Logikę aplikacji mogą być zapisywane w całości JavaScript i można zaprojektowany jako procedura składowana, wyzwalacz lub UDF. Kodu JavaScript w procedury składowanej lub wyzwalacza można wstawić, zamienianie i usuwanie, odczytu lub kwerendy dokumentów w zbiorze. Z drugiej strony JavaScript w UDF nie można wstawić, zamienianie lub usuwanie dokumentów. Funkcji zdefiniowanych przez użytkownika wyliczyć dokumenty zestawu wyników kwerendy i warzywa inny zestaw wyników. Dla wielu dzierżawy DocumentDB wymusza zarządzania zasobów ściśle rezerwacji podstawie. Każda procedura składowana, wyzwalacz lub UDF otrzymuje stały quantum zasobów systemu operacyjnego swojej pracy. Ponadto procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika nie można połączyć przed zewnętrznych bibliotek JavaScript i traktowane są jako zabronione jeśli przekraczają one budżety zasobów przydzielonych do nich. Można zarejestrować, unregister procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika z kolekcji przy użyciu interfejsów API pozostałych.  Rejestracji procedura składowana, wyzwalacz lub UDF jest wstępnie skompilowany i przechowywanych jako kod bajt, który pobiera wykonane później. Następną sekcję przedstawiono, jak umożliwia DocumentDB JavaScript SDK rejestrowanie, wykonywanie i wyrejestrowywanie procedura składowana, wyzwalacz i UDF. JavaScript SDK jest proste opakowanie za pośrednictwem [Interfejsów API pozostałych DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Rejestrowanie procedura składowana
Rejestracja procedura składowana tworzy nowy zasób procedura przechowywana w zbiorze za pośrednictwem protokołu HTTP wpisu.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Wykonywanie procedura składowana
Wykonanie procedury składowanej jest wykonywane przez wydawania HTTP POST przed istniejący zasób procedura składowana przekazując parametry do procedury w treści wezwania.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Wyrejestrowywanie procedura składowana
Wyrejestrowywanie procedura składowana należy po prostu wysyłając HTTP usuwanie przed istniejący zasób procedura składowana.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Rejestrowanie przed wyzwalacza
Rejestracja wyzwalacza należy utworzyć w przypadku nowego zasobu wyzwalacza w zbiorze za pośrednictwem protokołu HTTP wpisu. Możesz określić, czy wyzwalacz jest sprzed lub wyzwalacz wpis i typ operacji może być skojarzone z (np. Tworzenie, zamienianie, usuń lub wszystkie).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Wykonywanie wstępnej wyzwalacza
Wykonanie wyzwalacza odbywa się określając nazwę istniejącego wyzwalacza w momencie żądania WPIS/położenie/Usuwanie zasobu dokumentu za pomocą nagłówka żądania.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Wyrejestrowywanie przed wyzwalacza
Wyrejestrowywanie wyzwalacza po prostu odbywa się za pośrednictwem wydawania HTTP usuwanie przed istniejący zasób wyzwalacza.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Rejestrowanie UDF
Rejestracja UDF jest wykonywane przez tworzenie nowego zasobu UDF w zbiorze za pośrednictwem protokołu HTTP wpisu.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Wykonywanie UDF jako części kwerendy
UDF można określić jako części kwerendy SQL i jest używany jako sposób rozszerzenia podstawowych [języka SQL kwerendy DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Wyrejestrowywanie UDF 
Wyrejestrowywanie UDF jest po prostu wykonać wydawania HTTP usuwanie przed istniejący zasób UDF.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Mimo że wstawki powyżej pokazano rejestracji (POST), wyrejestrowania (położenie), odczytu i listy (Pobierz) i wykonywania (POST) przy użyciu [Zestawu SDK JavaScript DocumentDB](https://github.com/Azure/azure-documentdb-js), umożliwia także [Interfejsy API pozostałych](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub innego [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumenty
Można wstawiać, Zamień, usuwanie, przeczytaj, wyliczanie i kwerendy dowolnego JSON dokumentów w kolekcji. DocumentDB nie przesądza dowolnego schematu i nie wymaga indeksów pomocniczych w celu obsługi badania nad dokumentami w kolekcji.   

Jest usługą naprawdę otwartej bazy danych, DocumentDB nie magazynowa wszystkie typy danych specjalistyczne (np. Data godzina) lub określonych kodowań dokumentów JSON. Należy zauważyć, że DocumentDB nie wymaga specjalnych Konwencji JSON, należy skodyfikować relacje między różnymi dokumentów; Składnia SQL DocumentDB udostępnia wydajne hierarchii i relacyjne kwerendy operatorów do kwerendy i project dokumentów bez specjalnych adnotacji i potrzebę skodyfikować relacje między dokumentów przy użyciu różnych właściwości.  
 
Jako innymi zasobami dokumenty można tworzyć, zamieniono, usunięte, przeczytaj, wyliczenia i kwerendy, łatwo przy użyciu interfejsów API pozostałych lub dowolną [SDK klienta](https://msdn.microsoft.com/library/azure/dn781482.aspx). Usuwanie dokumentu natychmiast zwolnienie przydziału odpowiadające wszystkich zagnieżdżonych załączników. Poziom spójności odczytu dokumentów wykonuje zasad spójności na koncie bazy danych. Na podstawie na żądanie w zależności od wymagań spójności danych aplikacji można zastąpić tych zasad. Podczas wysyłania kwerend dokumentów, przeczytaj spójności wykonuje indeksowania trybie określonym w zbiorze. Dla "spójne" wynika to zasad spójności konta. 

## <a name="attachments-and-media"></a>Załączniki i multimediów
>[AZURE.NOTE] Załącznik i multimediów zasoby są funkcje preview.
 
DocumentDB pozwala na przechowywanie dwójkową obiektów blob i multimediów z DocumentDB lub do sklepu nośniku zdalnym. W tym obszarze można również reprezentować metadanych multimediów pod względem specjalne dokument o nazwie załącznika. Załącznik w DocumentDB jest specjalne dokumentu (JSON), który odwołuje się do multimediów blob przechowywanych w innym miejscu. Załącznik jest po prostu specjalne dokumentu, który rejestruje metadanych nośnika przechowywane w magazynie zdalnym multimediów (lokalizacja, autor itd.). 

Należy rozważyć, czy aplikacji społecznościowej odczytu, która używa DocumentDB do przechowywania adnotacje odręczne i metadanych, łącznie z komentarzami wyróżnia, zakładek, klasyfikacji "Lubię to" i dislikes itd., skojarzony książki elektronicznej danego użytkownika.   

-   Książki sam jest przechowywana zawartość w magazynie multimediów albo dostępne jako część DocumentDB konto bazy danych lub w sklepie nośniku zdalnym. 
-   Aplikacja może przechowywać metadanych każdego użytkownika jako odrębnych dokumentu — np. Janusza metadane Skoroszyt1 są przechowywane w dokumencie odwołuje się /colls/joe/docs/book1. 
-   Załączniki wskazująca na stronach zawartości książki danego użytkownika są przechowywane w obszarze odpowiedni dokument np /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 itp. 

Należy zauważyć, że przykłady wymienionych powyżej przy użyciu przyjaznego identyfikatorów do przedstawienia hierarchii zasobów. Zasoby są dostępne za pośrednictwem interfejsów API pozostałych za pośrednictwem zasobów unikatowych identyfikatorów. 

Dla danych multimediów, która zarządza DocumentDB właściwości _media załącznika będzie odwoływać się do multimediów przez jego identyfikatora URI. Zapewnia DocumentDB śmieci zbieranie multimedia, gdy wszystkie pozostałe odwołania są usuwane. DocumentDB automatycznie generuje załącznik podczas przekazywania nowych multimediów i wypełnia _media, aby wskazywały nowo dodany multimediów. Jeśli chcesz zapisać plik w magazynie obiektów blob zdalnego zarządzane przez Ciebie (np. OneDrive, Magazyn Azure skrzynki itp.), nadal można załączników odwołanie multimediów. W tym przypadku będzie samodzielne tworzenie załącznika i wypełnij jego właściwość _media.   

Podobnie jak w przypadku wszystkich innych zasobów załączniki można utworzyć, zastąpione, usunięte, przeczytaj lub wyliczane łatwo przy użyciu interfejsów API pozostałych lub dowolną klienta SDK. Podobnie jak w przypadku dokumentów, poziom spójności odczytu załączniki są uwzględniane zasad spójności na koncie bazy danych. Na podstawie na żądanie w zależności od wymagań spójności danych aplikacji można zastąpić tych zasad. Gdy wyszukiwanie załączników, przeczytaj spójności wykonuje indeksowania trybie określonym w zbiorze. Dla "spójne" wynika to zasad spójności konta. 
 
## <a name="users"></a>Użytkownicy
Użytkownik DocumentDB reprezentuje logiczną nazw do grupowania uprawnienia. Użytkownik DocumentDB może odpowiadać użytkownika w systemie zarządzania tożsamościami lub roli wstępnie zdefiniowanych aplikacji. DocumentDB użytkownika reprezentuje po prostu analizę do grupowania zestaw uprawnień w bazie danych.   

Stosowania wielu dzierżawy w aplikacji, użytkowników możesz tworzyć w DocumentDB, odnoszącą się do rzeczywistych użytkowników lub dzierżaw aplikacji. Następnie można utworzyć uprawnienia dla danego użytkownika, które odpowiadają kontrola dostępu w różnych zbiorach dokumentów, załączniki, itp.   

Jak aplikacje potrzebuje skalowania wzrost do użytkownika, mogą zastosować różne sposoby shard danych. Można modelować wszystkich użytkowników w następujący sposób:   

-   Każdy użytkownik mapy z bazą danych.
-   Każdy użytkownik mapy do kolekcji. 
-   Dokumenty odpowiadające wielu użytkowników, przejdź do zbioru dedykowane. 
-   Dokumenty odpowiadające wielu użytkowników, przejdź do zestawu zbiorów.   

Bez względu na to strategii sharding określone przez Ciebie możesz modelu rzeczywisty użytkowników jako użytkowników w bazie danych DocumentDB i skojarzyć lub dostosowywanie kolorów uprawnienia dla poszczególnych użytkowników.  

![Kolekcje użytkownika][3]  
**Strategie sharding i modelowania użytkowników**

Podobnie jak wszystkie inne zasoby użytkowników w DocumentDB można utworzyć, zastąpione, usunięte, przeczytaj lub wyliczane łatwo przy użyciu interfejsów API pozostałych lub dowolną klienta SDK. DocumentDB zawsze zapewnia silne spójność do czytania lub kwerend metadanych użytkownika zasobu. Warto wskazujące, to usunięcie użytkownika automatycznie zapewnia, że nie masz dostępu do jakichkolwiek uprawnień zawarte w nim. Mimo że DocumentDB ta druga operacja daje przydział uprawnień w ramach usuniętym użytkownikiem w tle, usuniętych uprawnienia znajduje się natychmiast ponownie do użycia.  

## <a name="permissions"></a>Uprawnienia
Z perspektywy kontroli dostępu do zasobów, takich jak konta bazy danych bazy danych, użytkownicy i uprawnienia są traktowane jako zasoby *administracyjne* ponieważ wymagają uprawnienia administracyjne. Z drugiej strony zasobów, w tym zbiory, dokumentów, załączniki, procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika są ograniczone w określonej bazie danych i traktowane jako *zasoby aplikacji*. Odpowiadający mu do dwóch typów zasobów i role, mające dostęp do nich (to znaczy administratorów i użytkowników), modelu autoryzacji definiuje dwa typy *klawiszy dostępu*: *klucza głównego* i *klucza zasobów*. Klucz główny jest częścią konto bazy danych i deweloper (lub administrator) kto jest inicjowania obsługi administracyjnej konta bazy danych. Ten klucz główny ma znaczenie administrator może służyć do zezwalają na dostęp do zasobów zarówno administracyjne i aplikacji. Natomiast klucz zasobu jest klucz szczegółowego programu access, który umożliwia dostęp do *określonego* zasobu aplikacji. W związku z tym przechwytuje relacji między użytkownik bazy danych oraz uprawnienia, które użytkownik ma dla określonego zasobu (np. zbioru, dokumentu, załącznik, procedura składowana, wyzwalacz lub UDF).   

Jedynym sposobem uzyskania klucza zasobów jest tworzenie zasobu uprawnień przez danego użytkownika. Pamiętaj, aby można było utworzyć lub pobrać uprawnień, klucz główny będą przedstawiane w nagłówku autoryzacji. Zasób uprawnień wiąże zasobu, dostępu i użytkownika. Po utworzeniu zasobu uprawnień, użytkownik musi tylko do prezentowania klucz zasobów w celu uzyskania dostępu do wybranego zasobu. W związku z tym kluczem zasobów można wyświetlić jako logicznej i niewielkie reprezentacja zasobów uprawnień.  

Podobnie jak w przypadku wszystkich innych zasobów uprawnienia w DocumentDB można utworzyć, zastąpione, usunięte, przeczytaj lub wyliczane łatwo przy użyciu interfejsów API pozostałych lub dowolną klienta SDK. DocumentDB zawsze zapewnia silne spójność do czytania lub kwerend metadanych uprawnień. 

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat pracy z zasobami za pomocą poleceń HTTP w [RESTful interakcji z zasobami DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

