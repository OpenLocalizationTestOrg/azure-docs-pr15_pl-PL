<properties
   pageTitle="Buforowanie wskazówki | Microsoft Azure"
   description="Wytyczne dotyczące pamięci podręcznej, aby zwiększyć wydajność i skalowalność."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Buforowanie wskazówki

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Buforowanie jest typowe metody, która ma na celu zwiększyć wydajność i skalowalność systemu. Jest to możliwe dzięki tymczasowo kopiowanie danych często używanych do szybkiego magazynu, który znajduje się Zamknij, aby aplikacji. Jeśli ten magazynowanie danych szybkie znajduje się bliżej aplikacji niż oryginalne źródło, następnie buforowanie może znacznie zwiększyć czas odpowiedzi w przypadku aplikacji klienckich przez szybciej obsługi danych.

Pamięci podręcznej są najbardziej efektywne gdy wystąpienie klienta wielokrotnie odczytuje tych samych danych, zwłaszcza jeśli wszystkie następujące warunki dotyczą oryginalnego magazynu danych:
- Pozostaje względnie statyczne.
- Jest powolne porównanie szybkości pamięci podręcznej.
- Podlega wysokiego poziomu konfliktu.
- Jest daleko opóźnień sieci mogą powodować dostęp do wolno.

## <a name="caching-in-distributed-applications"></a>Pamięci podręcznej w aplikacjach rozłożone

Aplikacje rozproszone zwykle zaimplementować jedną lub obie z następujących strategii, gdy buforowanie danych:

- Używanie prywatne pamięci podręcznej, w którym dane są przechowywanych lokalnie na komputerze, na którym działa wystąpienie aplikacji lub usługi.
- Za pomocą udostępnionego pamięci podręcznej, jak wspólne źródło, które mogą uzyskiwać dostęp do wielu procesów i/lub maszyn.

W obu przypadkach pamięci podręcznej mogą być wykonywane po stronie klienta i/lub po stronie serwera. Po stronie klienta jest wykonywane przez procesem, który umożliwia interfejsu użytkownika systemu, takie jak przeglądarki sieci web lub aplikacji klasycznej.
Buforowanie po stronie serwera jest wykonywane przez procesem, który umożliwia usług biznesowych, które są uruchomione zdalnie.

### <a name="private-caching"></a>Prywatne pamięci podręcznej

Najprostszy typ pamięci podręcznej jest magazyn w pamięci. Ma przechowywanych w obszarze adres pojedynczego procesu i dostęp bezpośredni przez kod, który jest uruchamiany w ten proces. Ten typ pamięci podręcznej jest bardzo szybki dostęp. Można również dołączyć bardzo skuteczny środek do przechowywania niewielkie ilości danych statycznych, ponieważ rozmiar pamięci podręcznej zazwyczaj jest ograniczona przez ilość pamięci dostępnej na komputer, na którym ten proces.

Jeśli potrzebujesz pamięci podręcznej więcej informacji niż jest fizycznie możliwe w pamięci, można napisać zapisujące dane do lokalnego systemu plików. Może być mniejsza dostępu niż dane, które są przechowywane w pamięci, ale nadal powinny być szybsze i bardziej niezawodne niż pobierania danych przez sieć.

Jeśli masz wiele wystąpień aplikacji korzystającej z tego modelu równoczesne działanie każdego wystąpienia aplikacji ma własny niezależnych pamięci podręcznej, przytrzymując własną kopię danych.

Traktować pamięci podręcznej jako migawkę oryginalne dane w pewnym momencie w przeszłości. Jeśli dane nie są statyczne, prawdopodobnie że wystąpień aplikacji różnych przechowywać różne wersje dane w ich pamięci podręcznej. Dlatego tę samą kwerendę wykonywaną przez te wystąpienia może zwracać inne wyniki, jak pokazano na rysunku 1.

![Za pomocą w pamięci podręcznej w różnych wystąpień aplikacji](media/best-practices-caching/Figure1.png)

_Rysunek 1: Używanie w pamięci podręcznej w różnych wystąpień aplikacji_

### <a name="shared-caching"></a>Udostępnione pamięci podręcznej

Używanie udostępnionej pamięci podręcznej może pomóc w zlikwidować wady, że dane mogą się różnić w poszczególnych pamięci podręcznej, która może wystąpić w przypadku pamięci w pamięci podręcznej. Buforowanie udostępnionych gwarantuje, że wystąpienia innej aplikacji, zobacz tego samego widoku zapisujące dane. Jest to możliwe dzięki lokalizowanie pamięci podręcznej w innej lokalizacji, zwykle hostowana w ramach usługi osobnych, jak pokazano na rysunku 2.

![Za pomocą udostępnionego pamięci podręcznej](media/best-practices-caching/Figure2.png)

_Rysunek 2: Za pomocą udostępnionego pamięci podręcznej_

Ważne zaletą ujęciu buforowania udostępnionego jest skalowalność, które znajdują się w nim. Wiele usług udostępnionych pamięci podręcznej są wykonywane przy użyciu klaster serwerów, a są używane oprogramowanie, które rozdziela dane w grupie w przejrzysty sposób. Wystąpienie aplikacji po prostu przesyła żądanie do usługi pamięci podręcznej.
Podstawowe infrastruktury jest odpowiedzialny za określanie lokalizację pamięci podręcznej danych w grupie. Można łatwo skalować pamięci podręcznej, dodając więcej serwerów.

Istnieją dwa główne wady ujęciu buforowania udostępnionego:
- Pamięć podręczna jest tempo dostęp do, ponieważ jest już przechowywanych lokalnie do każdego wystąpienia aplikacji.
- Wymóg implementacji usługi oddzielnych pamięci podręcznej może dodać złożoność rozwiązanie.

## <a name="considerations-for-using-caching"></a>Zagadnienia dotyczące korzystania z pamięci podręcznej

W poniższych sekcjach opisano bardziej szczegółowo zagadnienia dotyczące projektowania i używania pamięci podręcznej.

### <a name="decide-when-to-cache-data"></a>Określenie, kiedy należy pamięci podręcznej danych

Buforowanie znacznie poprawić wydajność, skalowalność i dostępność. Więcej danych, które masz i większą liczbą użytkowników, którzy potrzebują dostępu do tych danych, tym większy korzyści wynikających z pamięci podręcznej stają się. Jest tak, ponieważ buforowania zmniejsza opóźnienie i konfliktu skojarzonego z obsługi dużych ilości żądań w oryginalnego magazynu danych.

Na przykład bazy danych mogą obsługiwać ograniczoną liczbę połączeń jednocześnie. Pobieranie danych z udostępnionej pamięci podręcznej, jednak zamiast podstawowej bazy danych umożliwia aplikacji klienckiej uzyskać dostęp do tych danych, nawet wtedy, gdy liczba połączeń dostępne obecnie wyczerpaniu. Ponadto jeśli baza danych jest niedostępny, aplikacje klienckie może być możliwe kontynuowanie przy użyciu dane, które są przechowywane w pamięci podręcznej.

Należy rozważyć, czy buforowanie danych, który jest przeczytać często, ale zmodyfikowany rzadko (na przykład dane, które ma większą część operacji odczytu niż operacji zapisu). Jednak nie zaleca się używanie pamięci podręcznej jako autorytatywne sklepu ważnych informacji. Zamiast tego upewnij się, że wszystkie zmiany, które aplikacja nie możesz utracić zawsze są zapisywane w sklepie trwałych danych. Oznacza to, że jeśli pamięć podręczną jest niedostępny, aplikacja będą nadal działać przy użyciu magazynu danych, a nie utracić ważnych informacji.

### <a name="determine-how-to-cache-data-effectively"></a>Określanie sposobu skuteczne pamięci podręcznej danych

Klucz do efektywnego korzystania z pamięci podręcznej znajduje się w określaniu najbardziej odpowiednie dane pamięci podręcznej i buforowanie go we właściwym czasie. Dane można dodawać do pamięci podręcznej na żądanie po raz pierwszy, że są pobierane przez aplikację. Oznacza to, że wymagane przez aplikację do pobierania danych tylko raz z magazynu danych, a kolejne przez program access może być spełnione przy użyciu pamięci podręcznej.

Możesz też pamięci podręcznej może zostać w pełni lub częściowo wypełniona dane z wyprzedzeniem, zwykle podczas uruchamiania aplikacji (nazywanego obsługiwanie podejście). Jednak go może nie być wskazane wdrożenie obsługi dużych pamięci podręcznej, ponieważ tej metody można nałożyć przerwa, wysoki obciążenie oryginalnego magazynu danych podczas uruchamiania aplikacji.

Często analizę upodobania może ułatwić podejmowanie decyzji w pełni lub częściowo wstępnie wypełnić pamięci podręcznej, a także wybrać dane do pamięci podręcznej. Na przykład może być przydatne w pamięci podręcznej danych profilów użytkowników statyczną wartość początkową dla klientów, którzy za pomocą aplikacji regularnie (być może codziennie), ale nie dla klientów, którzy za pomocą aplikacji tylko raz w tygodniu.

Zazwyczaj buforowanie działa również z dane niezmienne lub aby rzadko zmiany. Jako przykład można wymienić informacje, takie jak produktu i cenach w aplikacji elektronicznego lub statyczne zasobów udostępnionych kosztów do utworzenia. Niektóre lub wszystkie z tych danych mogą być ładowane do pamięci podręcznej podczas uruchamiania aplikacji, aby zminimalizować zapotrzebowania na zasoby i w celu zwiększenia wydajności. Być może być tle okresowo aktualizuje odwołania są aktualne dane w pamięci podręcznej, aby upewnić się, że lub który Odświeża pamięć podręczną po odwołać danych zmian.

Buforowanie jest mniej przydatne dla danych dynamicznych, mimo że istnieje kilka wyjątków tego (zobacz sekcję pamięci podręcznej wysoce dynamiczne dane w dalszej części tego artykułu, aby uzyskać więcej informacji). Po zmianie danych regularnie, buforowanych informacji staje się przestarzałe bardzo szybko lub ogólnych synchronizacji pamięci podręcznej z oryginalnego magazynu danych, co pozwala zmniejszyć skuteczność pamięci podręcznej.

Należy zauważyć, że pamięci podręcznej musi zawierać pełne dane obiektu. Na przykład, jeśli element danych reprezentują wielowartościowego obiektu, takiego jak klienta przy użyciu nazwy, adresu i koncie, niektóre z tych elementów może być nadal statyczne (na przykład nazwa i adres), gdy inne osoby (na przykład saldo konta) mogą być bardziej dynamiczne. W następujących sytuacjach może być przydatne buforowanie statycznej części danych i pobierania (lub obliczyć) pozostałe informacje gdy jest wymagane.

Zalecamy przeprowadzać badania i zastosowania analizy wydajności do określenia, czy odpowiednia jest wstępnie lub na żądanie ładowania pamięci podręcznej lub obu tych sposobów. Decyzja powinny być oparte na zmienności i deseniu zastosowania danych. Analiza użycia i wydajności pamięci podręcznej jest szczególnie ważne w aplikacji, które dużym obciążeniem wystąpienia i muszą być bardzo skalowalna. Na przykład w scenariuszach wysoce skalowalna zrozumiałe może pamięci podręcznej w celu zmniejszenia obciążenia do przechowywania danych w czasie Alokacja maksymalna wartość początkową.

Buforowanie można także do obliczeń po uruchomieniu aplikacji. Jeśli operację przekształcenia danych lub wykonuje obliczenia skomplikowane, go zapisać wyniki operacji w pamięci podręcznej. Jeśli te same obliczenia jest wymagane później, aplikacja po prostu można pobrać wyniki z pamięci podręcznej.

Aplikacja można zmodyfikować dane, które są przechowywane w pamięci podręcznej. Jednak zaleca się Myślę o pamięci podręcznej przechowywania danych przejściowych, który można w każdej chwili zniknąć. Przydatne dane przechowywane w pamięci podręcznej. Upewnij się, zachować informacje w także oryginalnego magazynu danych. Oznacza to, że jeśli pamięć podręczną jest niedostępny, możesz zminimalizować ryzyko utraty danych.

### <a name="cache-highly-dynamic-data"></a>Wysoce dynamiczne dane pamięci podręcznej

Po umieszczeniu szybkie zmienianie informacji w magazynie danych trwałych go nałożyć ogólnych w systemie. Na przykład można rozważyć urządzeniu ciągłe raportów stanu lub innych wartości miary. Jeśli nie chcesz pamięci podręcznej danych na podstawie buforowanych informacji prawie zawsze będzie nieaktualne aplikacji, tą samą opłatą może być wartość true, gdy przechowywania i pobierania informacji z magazynu danych. W czasie, potrzebny w celu zapisania i uzyskiwanie zdalnego dostępu do danych może być zostały zmienione.

W przypadku takich jak należy rozważyć korzyści przechowywania informacji dynamiczne bezpośrednio w pamięci podręcznej zamiast w magazynie trwałych danych. Jeśli dane są niekrytyczne i nie wymaga inspekcji, następnie nie ma znaczenia jeśli rzadkie zmiany zostaną utracone.

### <a name="manage-data-expiration-in-a-cache"></a>Zarządzanie wygasania dane w pamięci podręcznej

W większości przypadków dane, które są przechowywane w pamięci podręcznej jest kopią danych, który jest przechowywany w oryginalnego magazynu danych. Dane w oryginalnego magazynu danych może się zmienić po jego została przechowywanych w pamięci podręcznej, powoduje pamięci podręcznej danych stają się przestarzałe. Wiele systemów buforowania umożliwiają konfigurowanie pamięci podręcznej, aby wygasało danych i zredukować okres, dla którego dane mogą być nieaktualne.

Po wygaśnięciu zapisujące dane, zostanie usunięty z pamięci podręcznej, a aplikacja musi pobrać dane z oryginalnego magazynu danych (go można umieścić informacje nowo pobrane ponownie w pamięci podręcznej). Po skonfigurowaniu pamięci podręcznej, możesz ustawić domyślną zasadę wygaśnięcia. W wielu usługach pamięci podręcznej możesz także określać okres wygasania dla poszczególnych obiektów po ich przechowywania programowo w pamięci podręcznej.
Niektóre pamięci podręcznej pozwalają określić okres wygasania jako wartość bezwzględna lub jako wartość przesuwania, która powoduje, że element ma zostać usunięty z pamięci podręcznej, jeśli nie jest dostępna w ramach określonego czasu. To ustawienie zastępuje dowolny zasad wygasania całej pamięci podręcznej, ale tylko dla określonych obiektów.

> [AZURE.NOTE] Należy rozważyć, czy termin wygaśnięcia pamięci podręcznej i obiekty, które zawiera dokładnie. Jeśli uzyskana za mała, obiekty wygaśnie zbyt szybko i zmniejszy zalet korzystania z pamięci podręcznej. Jeśli wprowadzisz okresu zbyt długo, można ryzyko danych przedawnieniu.

Istnieje także możliwość, że pamięci podręcznej może zapełnić danych może pozostać pobytu przez dłuższy czas. W tym przypadku żądań do dodawania nowych elementów w pamięci podręcznej może spowodować niektórych elementów wymusić usunięcie w procesie określanym jako eksmisji zwiększany. Usługi pamięci podręcznej zwykle wykluczenia danych w oparciu o najmniejszej niedawno używane (LRU), ale zwykle można zastąpić tych zasad i zapobieganie elementów zostanie usunięty. Jednak przyjąć tej metody wiąże się z ryzykiem przekraczają pamięci, które są dostępne w pamięci podręcznej. Aplikacja próbuje Dodawanie elementu do pamięci podręcznej zakończy się niepowodzeniem z powodu wyjątku.

Niektóre buforowania implementacji może zawierać zasady dodatkowe eksmisji zwiększany. Istnieje kilka typów zasad eksmisji zwiększany. W tym:
- Ostatnio używane zasady (w oczekiwanie, że dane nie będą wymagane ponownie).
- Zasady pierwszego w pierwszym out (najstarszych danych zostanie usunięty jako pierwsze).
- Zasady usuwania jawne, oparte na zdarzenie wyzwalane (na przykład danych modyfikowany).

### <a name="invalidate-data-in-a-client-side-cache"></a>Powodować dane w pamięci podręcznej po stronie klienta

Dane, które są przechowywane w pamięci podręcznej po stronie klienta, zazwyczaj jest uważany za poza nadzorem usługa, która zawiera dane do klienta. Usługa nie może bezpośrednio wymuszać klienta, aby dodać lub usunąć informacje z pamięci podręcznej po stronie klienta.

Oznacza to, że jest możliwe dla klienta, który używa niepoprawnie skonfigurowane pamięci podręcznej na używanie nieaktualne informacje. Na przykład jeśli zasady wygasania pamięci podręcznej nie są poprawnie wdrażane, klient może użyć nieaktualne informacje, które są buforowane lokalnie po zmianie informacji w źródle danych.

Jeśli tworzysz aplikację sieci web, która pełni danych za pośrednictwem połączenia HTTP na uzyskiwanie zdalnego dostępu do najnowszych informacji można wymusić niejawnie klienta sieci web (na przykład w przeglądarce lub serwera proxy sieci web). Można to zrobić, jeśli zasobu zostanie zaktualizowany poprzez zmianę identyfikator URI tego zasobu. Klienci sieci Web zazwyczaj używają URI zasobu jako klucz w pamięci podręcznej po stronie klienta, więc zmiana identyfikator URI klienta sieci web ignoruje dowolną wcześniej buforowanej wersji zasobu i zamiast tego pobiera nową wersję.

## <a name="managing-concurrency-in-a-cache"></a>Zarządzanie współbieżności w pamięci podręcznej

Często pamięci podręcznej mają zostać udostępniona przez wielu wystąpień aplikacji. Każdego wystąpienia aplikacji można czytać i zmodyfikować dane w pamięci podręcznej. W związku z tym tym samym problemów współbieżności, które mogą pojawić się przy użyciu dowolnego magazynu danych udostępnionych też zastosowane do pamięci podręcznej. W przypadku miejsce, w którym należy zmodyfikować dane, które są przechowywane w pamięci podręcznej aplikacji może być konieczne upewnij się, że aktualizacje wprowadzone przez jedno wystąpienie aplikacji należy zastępować zmiany wprowadzone przez innego wystąpienia.

W zależności od rodzaju danych i prawdopodobieństwa konfliktów należy przyjąć współbieżności na dwa sposoby:

- __Optymistyczny.__ Bezpośrednio przed zaktualizowaniem danych, aplikacja sprawdza, czy dane w pamięci podręcznej zostały zmienione, ponieważ został pobrany. Jeśli dane są nadal, mogą zostać wprowadzone zmiany. W przeciwnym razie aplikacja ma zdecydować, czy je zaktualizować. (W reguł biznesowych, którego tę decyzję będą specyficzne dla aplikacji). Tej metody nadaje się do sytuacji, gdy aktualizacje są rzadko lub miejsce, w którym jest mało prawdopodobne konfliktów.
- __Pesymistyczny.__ Po jego pobiera dane, aplikacja blokuje w pamięci podręcznej, aby uniemożliwić zmianę kolejnego wystąpienia. Ten proces zapewnia, że konfliktów nie mogą występować, ale można to także zablokować inne wystąpienia, które wymagają przetwarzania tych samych danych. Pesymistyczny współbieżności może mieć wpływ na skalowalność rozwiązanie i jest zalecane tylko w przypadku operacji krótkotrwałe. Takie rozwiązanie może być przydatne w sytuacjach, gdzie konfliktów są w większości przypadków, zwłaszcza w sytuacji, gdy aplikacja aktualizuje wiele elementów w pamięci podręcznej i należy zagwarantować, że te zmiany zostaną zastosowane spójne.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementowanie wysoką dostępność i skalowalność i poprawić wydajność

Unikanie używania pamięci podręcznej jako podstawowego repozytorium danych; to jest rola oryginalnego magazynu danych, z którego jest wypełniana pamięci podręcznej. Oryginalnego magazynu danych jest odpowiedzialny za zapewnienie utrzymywanie danych.

Uważaj, aby nie wprowadza krytyczne zależności o dostępności usług udostępnionych pamięci podręcznej do swojego rozwiązania. Aplikacja powinno być możliwe nadal działa, jeśli usługa, która zapewnia udostępnionej pamięci podręcznej jest niedostępny. Aplikacja powinna zawieszać lub nie kończą się niepowodzeniem podczas oczekiwania w usłudze pamięci podręcznej wznowić.

W związku z tym aplikacja muszą mieć możliwość wykrywanie dostępność usługi pamięci podręcznej i powrotu do oryginalnego magazynu danych jeśli pamięć podręczną jest niedostępny. [Rozmieszczenie — wzór](http://msdn.microsoft.com/library/dn589784.aspx) jest przydatne w przypadku obsługi w tym scenariuszu. Usługa, która zapewnia pamięci podręcznej można odzyskać, a po stał się dostępny, pamięci podręcznej można zapełnienia podczas odczytu danych formularza oryginalnego magazynu danych, wykonując strategii, takie jak [Deseń odłogowania pamięci podręcznej](http://msdn.microsoft.com/library/dn589799.aspx).

Jednak mogą występować wpływ skalowalność w systemie Jeśli aplikacja przechodzi do oryginalnego magazynu danych podczas pamięci podręcznej jest tymczasowo niedostępny.
Gdy odzyskane magazynu danych oryginalnego magazynu danych może swamped żądaniami dane, uzyskując limity czasu i nie powiodło się połączenia.

Należy rozważyć, czy implementowania lokalnych, prywatne pamięci podręcznej w każdym wystąpieniu aplikacji, razem z udostępnionej pamięci podręcznej, które dostęp do wszystkich wystąpień aplikacji. Gdy program pobiera elementu, może on sprawdzić, najpierw w lokalnej pamięci podręcznej, a następnie w udostępnionego pamięci podręcznej, a na koniec w oryginalne dane są przechowywane. Lokalnej pamięci podręcznej mogą zostać wypełnione przy użyciu danych w udostępnionym cache lub w bazie danych Jeśli udostępniony pamięci podręcznej jest niedostępny.

Ta metoda wymaga Konfiguracja zachować ostrożność, aby zapobiec zbyt przedawnieniu w odniesieniu do udostępnionego pamięci podręcznej lokalnej pamięci podręcznej. Jednak lokalnej pamięci podręcznej działa jako bufor Jeśli udostępniony pamięci podręcznej jest nieosiągalny. Rysunek 3 przedstawia tej struktury.

![Lokalne prywatne pamięci podręcznej za pomocą udostępnionego pamięci podręcznej](media/best-practices-caching/Caching3.png)
_Rysunek 3: lokalnego, prywatne pamięci podręcznej za pomocą udostępnionego pamięci podręcznej_

Do obsługi dużych pamięci podręcznej, które zawierają dane względnie długo, niektóre usługi pamięci podręcznej jest dostępna opcja dostępności korzystający automatyczne przejście, jeśli pamięć podręczną staje się niedostępna. Ta metoda polega zazwyczaj replikacji pamięci podręcznej danych, który jest przechowywany na serwerze podstawowego pamięci podręcznej na serwerze pomocnicza pamięć podręczna i przełączanie na serwerze pomocniczym, jeśli podstawowy serwer nie lub łączności zostaną utracone.

Aby zmniejszyć czas oczekiwania, który jest skojarzony z zapisywania do wielu miejsc docelowych, replikacji na serwerze pomocniczym może wystąpić asynchroniczne, gdy dane są zapisywane w pamięci podręcznej na serwerze podstawowym. Tej metody prowadzi do możliwości, że niektóre buforowane informacje zostaną utracone w przypadku awarii, ale część danych powinien być mały w porównaniu z ogólny rozmiar pamięci podręcznej.

Jeśli udostępniony pamięci podręcznej jest duży, może być korzystne partycje zapisujące dane w węzłach, aby zmniejszyć szanse konfliktu i poprawić skalowalność. Wiele udostępnionej pamięci podręcznej obsługuje możliwość dynamicznego dodawania (i usuwania) węzły i wyrównać dane przez partycje. Ta metoda może być obejmować klaster, w którym są prezentowane kolekcję węzłów do aplikacji klienckich jako płynną, pojedynczy pamięci podręcznej. Wewnętrznie jednak dane są rozdzielane między węzły po strategii dystrybucji wstępnie zdefiniowanych, który równomiernie Załaduj. [Wytyczne podziału danych](http://msdn.microsoft.com/library/dn589795.aspx) w witrynie sieci Web firmy Microsoft zawiera więcej informacji na temat możliwych strategii podziału.

Klaster można zwiększyć dostępność pamięci podręcznej. Jeśli węzeł nie powiedzie się, pozostała część pamięci podręcznej jest nadal dostępne.
Klaster często jest używana w połączeniu z replikacją i pracy awaryjnej. Każdy węzeł mogą być replikowane i replice mogą być szybko przełączyć do trybu online Jeśli węzeł nie powiedzie się.

Wiele więcej i operacji zapisu może obejmować wartości danych lub obiektów. Jednak czasami może być konieczne do przechowywania lub szybko pobrać dużych ilości danych.
Na przykład obsługiwanie pamięci podręcznej może obejmować zapisywania w pamięci podręcznej setki lub tysiące elementów. Aplikacja może wystąpić konieczność pobierania dużej liczby powiązanych elementów z pamięci podręcznej jako część samego żądania.

Wiele dużych pamięci podręcznej zapewniają operacje wsadowe do tych celów. To umożliwia aplikacjom klienta grupowanie dużej liczby elementów do pojedynczego żądania i zmniejsza obciążenie, które jest skojarzony z wykonywania dużej liczby wezwań na małe.

## <a name="caching-and-eventual-consistency"></a>Spójności buforowania i ewentualnego

Deseń odłogowania pamięci podręcznej pracy wystąpienia aplikacji, które wypełnia pamięci podręcznej musi mieć dostęp do najbardziej aktualne i spójne wersji danych. W układzie wykonuje ewentualne spójności (na przykład magazyn danych zreplikowanej) to może nie być wielkość liter.

Jedno wystąpienie aplikacji można zmodyfikować element danych i powodować buforowanej wersji tego elementu. Inne wystąpienie aplikacji może próbować przeczytaj ten element z pamięci podręcznej, co powoduje, że — brak trafienia pamięci podręcznej, więc odczytuje dane z magazynu danych i dodaje ją do pamięci podręcznej. Jednak jeśli magazynu danych nie zostały w pełni zsynchronizowane z innymi replikami, aplikację może odczytać i wypełnienia pamięci podręcznej ze starej wartości.

Aby uzyskać więcej informacji na temat obsługi spójności danych zobacz stronę [Elementarz spójności danych](http://msdn.microsoft.com/library/dn589800.aspx) w witrynie sieci Web firmy Microsoft.

### <a name="protect-cached-data"></a>Ochrona danych pamięci podręcznej

Niezależnie od usługi pamięci podręcznej, należy rozważyć ochrony danych, który jest przechowywany w pamięci podręcznej przed nieautoryzowanym dostępem. Istnieją dwa główne zagadnienia:

- Prywatność danych w pamięci podręcznej
- Prywatność dane w postaci przepływał między pamięci podręcznej i aplikacji, która korzysta z pamięci podręcznej

Aby chronić dane w pamięci podręcznej, usługi pamięci podręcznej może zastosować mechanizm uwierzytelniania, która wymaga, aby określić następujące ustawienia aplikacji:
- Które tożsamości dostęp do danych w pamięci podręcznej.
- Jakie operacje (odczytu i zapisu), które mogą wykonywać tych tożsamości.

Aby zmniejszyć którą jest skojarzony z odczytywanie i zapisywanie danych, po tożsamości przyznano zapisu i/lub do odczytu w pamięci podręcznej, że tożsamości można używać dowolnych danych w pamięci podręcznej.

Jeśli chcesz ograniczyć dostęp do podzbiór zapisujące dane, możesz wykonać jedną z następujących czynności:

- Można podzielić pamięci podręcznej partycje (przy użyciu różnych pamięci podręcznej serwerów) i tylko udzielić dostępu do tożsamości partycje, które powinni używać.
- Szyfrowanie danych w każdy podzbiór przy użyciu różnych kluczy i udostępnia kluczy szyfrowania tylko dla tożsamości, które mają dostęp do każdego podzbiór. Aplikacja klienta może być nadal będzie mógł pobrać wszystkie dane w pamięci podręcznej, ale tylko będą mogli odszyfrowywania danych, dla której ma klawiszy.

Należy również chronić dane jako przepływał i pamięci podręcznej. Aby to zrobić, zależą od funkcji zabezpieczeń zapewnianych przez infrastruktury sieciowej, który umożliwia nawiązywanie połączenia z pamięci podręcznej aplikacji klienckich. Jeśli pamięć podręczną jest wdrażane za pomocą serwera na miejscu w obrębie tej samej organizacji obsługującego aplikacje klienckie, następnie izolacji samej sieci może nie wymagać wykonania dodatkowych czynności. Jeśli pamięć podręczną znajduje się zdalnie i wymaga połączenia TCP i HTTP przez sieć publiczną (na przykład Internet), warto rozważyć implementacji protokołu SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Zagadnienia dotyczące stosowania pamięci podręcznej z Microsoft Azure

Azure udostępnia pamięci podręcznej Redis Azure. To jest implementacja Otwórz źródło Redis pamięci podręcznej, które działa jako usługa w Azure centrum danych. Czy aplikacja jest zaimplementowana jako usługi w chmurze, witryny sieci Web, lub wewnątrz Azure maszyn wirtualnych zapewnia buforowania usługa, która może uzyskać do nich dostęp z dowolnej aplikacji Azure. Pamięć podręczną można udostępniać w aplikacjach klienckich, z kluczem odpowiedni dostęp.

Azure Redis pamięci podręcznej jest rozwiązaniem buforowania wysokiej wydajności, która zapewnia dostępność, skalowalność i zabezpieczeń. Zazwyczaj działa jako usługa rozciągnąć co najmniej jednym komputerze dedykowane. Pozwala podjąć próbę do przechowywania te informacje, które można w pamięci w celu zapewnienia szybkiego dostępu. Ta architektura ma na celu krótki czas oczekiwania i wysokiej wydajności, zmniejszając do wykonywania operacji We/Wy działa wolno.

 Azure Redis pamięci podręcznej jest zgodny z wielu różnych interfejsów API, które są używane w aplikacjach klienckich. Jeśli masz aplikacje korzystające z funkcji już Azure Redis pamięci podręcznej uruchomiony w lokalnej pamięci podręcznej Redis Azure udostępnia ścieżkę szybkiej migracji do pamięci podręcznej w chmurze.

> [AZURE.NOTE] Azure udostępnia usługi zarządzanych pamięci podręcznej. Ta usługa jest oparty na aparat pamięci podręcznej tkaninie usługi Azure. Umożliwia tworzenie rozproszonej pamięci podręcznej, który może być współużytkowany przez luźno powiązanych aplikacji. Pamięci podręcznej znajduje się na serwerach wysokiej wydajności w Azure centrum danych.
Jednak ta opcja nie jest zalecane i jest dostępna wyłącznie do obsługi istniejących aplikacji, które są wbudowane używać tej funkcji. Dla wszystkich nowych wdrożeniach zamiast tego użyj Azure Redis w pamięci podręcznej.
>
> Ponadto Azure obsługuje pamięci podręcznej w roli. Ta funkcja umożliwia tworzenie pamięci podręcznej, które są specyficzne dla usługi w chmurze.
Pamięci podręcznej jest obsługiwana przez wystąpienia w sieci web lub pracownika roli i można uzyskać dostęp tylko przez role, które działają w ramach tej samej jednostki wdrożenia usługi cloud. (Jednostka wdrożenia jest zestaw wystąpień roli, wdrożonych jako usługi w chmurze do określonego regionu). Wykres kolumnowy grupowany pamięci podręcznej, a wszystkie wystąpienia roli w tej samej jednostce wdrożenia, który obsługuje pamięci podręcznej są uwzględniane w tym samym klastrze pamięci podręcznej. Jednak ta opcja nie jest zalecane i jest dostępna wyłącznie do obsługi istniejących aplikacji, które są wbudowane używać tej funkcji. Dla wszystkich nowych wdrożeniach zamiast tego użyj Azure Redis w pamięci podręcznej.
>
> Zarówno usługi zarządzanych pamięci podręcznej Azure i pamięci podręcznej w roli Azure są obecnie slated emerytury na 16 listopada 2016.
Zaleca się migrowanie Azure Redis w pamięci podręcznej w przygotowaniu tej emerytury. Aby uzyskać więcej informacji, odwiedź stronę   [Co to jest oferująca Azure Redis w pamięci podręcznej i jaki rozmiar należy używać?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) w witrynie sieci Web firmy Microsoft.


### <a name="features-of-redis"></a>Funkcje Redis

 Redis jest większa niż serwer prosty pamięci podręcznej. Rozłożone bazy danych w pamięci usługa udostępnia zestaw poleceń rozległa, która obsługuje wiele typowych scenariuszy. Są one opisane w dalszej części tego dokumentu w sekcji przy użyciu Redis pamięci podręcznej. W tej sekcji opisano niektóre z najważniejszych funkcji, które znajdują się Redis.

### <a name="redis-as-an-in-memory-database"></a>Redis jako bazy danych w pamięci

Redis obsługuje zarówno operacje odczytu i zapisu. W Redis zapisy mogą być chronione przed awarii przy zapisywaniu okresowo w pliku migawki lokalne lub w pliku dziennika tylko do dołączającej planu taryfowego. Nie jest w przypadku wielu pamięci podręcznej (które należy uwzględnić przejściowy magazynów).

 Zapisuje wszystkie są asynchroniczne, a nie blokują klientów z odczytywanie i zapisywanie danych. Gdy Redis uruchomienia uruchomiony, odczytuje dane z pliku dziennika lub migawki i wykorzystuje go do utworzenia w pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Redis pozostawanie](http://redis.io/topics/persistence) w witrynie sieci Web Redis.

> [AZURE.NOTE] Redis nie daje gwarancji wszystkich zapisów zostaną zapisane w przypadku awarii losowych, że w najgorszym mogą utracić zaledwie kilku sekund warte danych. Pamiętaj, że pamięci podręcznej nie ma pełnić rolę źródła danych autorytatywnych, a spoczywa aplikacji przy użyciu pamięci podręcznej zapewnienie pomyślnie zapisany krytycznych danych do magazynu odpowiednie dane. Aby uzyskać więcej informacji zobacz [deseniu odłogowania pamięci podręcznej](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis typy danych

Redis jest magazynem wartość klucza, w którym wartości może zawierać typów prostych lub struktur złożonych danych, takich jak mieszania, list i zestawów. Obsługuje zestaw Atomowej operacji dotyczących tych typów danych. Klucze mogą być stałe lub oznakowane z ograniczony czas wygaśnięcia, w którym klawisza i jego wartość są automatycznie usuwane z pamięci podręcznej. Aby uzyskać więcej informacji na temat kluczy Redis i wartości odwiedź stronę [Wprowadzenie do Redis typów danych i poboru](http://redis.io/topics/data-types-intro) w witrynie sieci Web Redis.

#### <a name="redis-replication-and-clustering"></a>Replikacja redis i klaster

Redis obsługuje replikacji nadrzędny podrzędny do zapewnienia dostępności i zachować przepustowość. Pisanie, że operacje, aby węźle głównym Redis są replikowane do co najmniej jeden węzły podrzędne. Operacje odczytu można obsługiwane przez wzorcu lub na dowolnej stronie podwładnych.

W przypadku partycją sieci podwładnych nadal może służyć dane, a następnie przezroczysty ponownie zsynchronizować ze wzorcem podczas ponownego ustanowienia połączenia. Aby uzyskać szczegółowe informacje odwiedź stronę [replikacji](http://redis.io/topics/replication) w witrynie sieci Web Redis.

Redis także klaster, umożliwia przezroczyste dzielenia danych na odłamki na serwerach i rozdzielić obciążenie. Ta funkcja zwiększa skalowalność, ponieważ można dodawać nowych serwerów Redis i zwiększanie danych ponownie podzielony na partycje jako rozmiar pamięci podręcznej.

Ponadto każdego serwera w klastrze mogą być replikowane przy użyciu replikacji nadrzędny podrzędny. Dzięki temu dostępność w każdym węźle w klastrze. Aby uzyskać więcej informacji na temat klastrów i sharding odwiedź stronę [Redis klaster samouczka strony](http://redis.io/topics/cluster-tutorial) w witrynie sieci Web Redis.

### <a name="redis-memory-use"></a>Redis użycie pamięci

Redis pamięci podręcznej ma ograniczone rozmiar, która zależy od zasobów dostępnych na hoście. Po skonfigurowaniu serwera Redis, można określić maksymalnej ilości pamięci, których można używać. Można także skonfigurować klucz w pamięci podręcznej Redis mieć czas wygaśnięcia, po upływie którego są automatycznie usuwane z pamięci podręcznej. Ta funkcja może zapobiec w pamięci podręcznej z wypełniania stare lub przestarzałe danych.

Jak wypełnia pamięci, Redis automatycznie można wykluczyć kluczy i ich wartości, wykonując liczba zasad. Wartość domyślna to LRU (najmniej ostatnio używane), ale możesz również wybrać inne zasady, takich jak losowo eksmitowania klawiszy lub wyłączenie eksmisji zwiększany całkowicie (w, wielkość liter prób, aby dodać elementy do błędów pamięci podręcznej, jeśli jest pełny). Strony [Przy użyciu Redis jako pamięci podręcznej LRU](http://redis.io/topics/lru-cache) znajdziesz więcej informacji.

### <a name="redis-transactions-and-batches"></a>Redis transakcji i partie

Redis umożliwia aplikacji klienckiej przesłać serii operacji, które odczytywanie i zapisywanie danych w pamięci podręcznej jako Atomowej. Wszystkie polecenia w transakcji pewno mogą być uruchamiane po kolei, a nie polecenia wydawane przez innych równoczesne klientów będzie interwoven między nimi.

Jednak nie są spełnione transakcje zgodnie z relacyjnej bazy danych chcesz wykonać. Przetwarzanie transakcji składa się z dwóch etapów — pierwszym jest gdy polecenia znajduje się w kolejce, a druga jest uruchomienie polecenia. Polecenie etapie Kolejkowanie polecenia, które składają się transakcji są przesyłane przez klienta. Jeśli jakiegoś błąd występuje na tym etapie (na przykład błąd składni lub nieprawidłową liczbę parametrów) Redis następnie odmawia przetwarzania całej transakcji i usuwa go.

Podczas wykonywania fazy Redis wykonanie każdego kolejce polecenia w kolejności. Jeśli polecenie nie powiedzie się podczas tej fazy, Redis będzie nadal występował z następnego polecenia kolejce i wycofana skutków dowolnych poleceń, które zostały już uruchomione. Ten formularz uproszczone transakcji pomaga zachować wydajności i uniknąć problemów z wydajnością wynikające z konfliktu.

Redis implementowania formularza optymistyczny blokowania ułatwiających zachowanie spójności. Aby uzyskać szczegółowe informacje dotyczące transakcji i blokowanie z Redis odwiedź [stronę transakcje](http://redis.io/topics/transactions) w witrynie sieci Web Redis.

Redis obsługuje również nie transakcji tworzeniu partii żądania. Protokół Redis, który klienci używać do wysyłania poleceń na serwerze Redis umożliwia klientowi wysyłanie serii operacji jako część samego żądania. Może to pomóc w celu zmniejszenia fragmentacji pakietów w sieci. Podczas przetwarzania, odbywa się każdego polecenia. Jeśli dowolny z tych poleceń jest nieprawidłowo, one będą odrzucane (które nie są widoczne transakcji), ale pozostałe polecenia zostanie wykonana. Również jest gwarantowana o kolejności przetwarzania poleceń w grupie.

### <a name="redis-security"></a>Redis zabezpieczeń

Redis jest ograniczony wyłącznie do zapewnienia szybkiego dostępu do danych i przeznaczony do uruchamiania w zaufanej środowisko, które są dostępne tylko dla klientów zaufanych. Redis obsługuje model zabezpieczeń ograniczone na podstawie uwierzytelniania hasła. (Jest to możliwe całkowicie usunąć uwierzytelniania mimo że nie jest to zalecane.)

Wszystkich uwierzytelnionych klientów udostępniać tego samego hasła globalnej i mieć dostęp do tych samych zasobów. Jeśli potrzebujesz bardziej zaawansowane zabezpieczeń logowania, należy zaimplementować własne warstwy zabezpieczeń przed Redis serwera, a wszystkie żądania klientów powinien przechodzić tej dodatkowej warstwy. Redis należy nie bezpośrednio udostępniane klientom niezaufanych lub nieuwierzytelnionych.

Można ograniczyć dostęp do poleceń, wyłączając je lub zmienić ich nazwy (i dostarcz jedynie klientów uprzywilejowanych pod inną nazwą).

Redis nie obsługuje bezpośrednio jakiejkolwiek szyfrowania danych, aby wszystkie kodowanie musi być wykonywane przez aplikacje klienckie. Ponadto Redis nie umożliwia formę zabezpieczenia transportu. Jeśli potrzebujesz ochrony danych przepływał przez sieć, zalecamy implementacji serwera proxy SSL.

Aby uzyskać więcej informacji odwiedź stronę [Redis zabezpieczeń](http://redis.io/topics/security) w witrynie sieci Web Redis.

> [AZURE.NOTE] Azure Redis pamięci podręcznej udostępnia własnej warstwy zabezpieczeń, w którym łączą się klienci. Podstawowe serwery Redis nie są dostępne do publicznej sieci.

### <a name="using-the-azure-redis-cache"></a>Za pomocą Azure Redis pamięci podręcznej

Pamięć podręczna Redis Azure zapewnia dostęp do Redis serwery mają zainstalowany na serwerach hostowana u Azure centrum danych; działa jako elewacji, która zapewnia zabezpieczeń i kontroli dostępu. Za pomocą portalu zarządzania Azure umożliwia obsługę pamięci podręcznej. Portalu zawiera wiele wstępnie zdefiniowanych konfiguracji, począwszy od 53GB pamięci podręcznej działającego jako dedykowane usługa, która obsługuje komunikacji SSL (w przypadku privacy) i replikacji nadrzędny podrzędny z SLA 99,9% dostępności, w dół do 250 MB pamięci podręcznej bez replikacji (nie gwarancje dostępności) udostępnionego sprzęt z systemem.

Za pomocą portalu zarządzania Azure, można również skonfigurować zasady eksmisji zwiększany pamięci podręcznej i kontrolowanie dostępu do pamięci podręcznej, dodając użytkowników do ról dostarczone; Czytnik właściciela, Współpracownik. Role te zdefiniować operacje, które mogą wykonywać członkowie. Na przykład członkowie roli właściciela mają pełną kontrolę nad pamięci podręcznej (w tym zabezpieczeń) i jego zawartość, członkowie roli współautora można odczytywanie i zapisywanie informacji w pamięci podręcznej i członkowie roli czytnika tylko można pobierać dane z pamięci podręcznej.

Większość zadań administracyjnych są wykonywane za pośrednictwem portalu zarządzania Azure i dlatego, że są wiele administracyjnych poleceń dostępnych w standardowej wersji Redis nie jest dostępny, łącznie z możliwością modyfikować programistycznie, konfiguracji zamknięcia serwera Redis skonfiguruj dodatkowe slaves lub wymusić Zapisz dane na dysku.

Portal Azure zarządzania zawiera wygodny graficzne umożliwiający monitorowanie wydajności pamięci podręcznej. Na przykład możesz wyświetlić liczbę połączeń odniesienia, liczba żądań wykonanej głośności Odczyt i zapis, i liczba pamięci podręcznej trafienia i Chybienia w pamięci podręcznej. Za pomocą tych informacji, można określić skuteczność pamięci podręcznej i w razie potrzeby przełączyć się do różnych konfiguracji lub zmiana zasad eksmisji zwiększany. Ponadto można tworzyć alerty, które powodują wysyłanie wiadomości e-mail do administratora Jeśli co najmniej jeden metryki krytyczne wykraczają poza zakresem oczekiwanych. Na przykład jeśli liczba trafień pamięci podręcznej jest większa niż określona wartość w ostatniej godziny, administrator może otrzymywać alerty pamięci podręcznej jest za mały lub danych może być zostanie usunięty zbyt szybko.

Można również monitorować Procesora, pamięci i obciążenie sieci pamięci podręcznej.

Dodatkowe informacje i przykłady przedstawiający, jak utworzyć i skonfigurować pamięci podręcznej Azure Redis odwiedź stronę [laptop wokół Azure Redis w pamięci podręcznej](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) w blogu Azure.

## <a name="caching-session-state-and-html-output"></a>Buforowania stan sesji i wyjściowe HTML

Jeśli tworzony ASP.NET aplikacje, aby uruchomić przy użyciu role Azure sieci web, można zapisać informacje o stanie sesji i wyjściowy kod HTML w pamięci podręcznej Redis Azure internetowe. Dostawca stan sesji Azure Redis w pamięci podręcznej umożliwia udostępnianie informacji o sesji między wystąpieniami różnych aplikacji sieci web ASP.NET i jest bardzo przydatne w sytuacjach farmy sieci web miejsce, w którym klient / serwer koligacji nie jest dostępny i pamięci podręcznej sesji danych w pamięci nie jest właściwe.

Za pomocą dostawcy stan sesji z pamięci podręcznej Azure Redis zapewnia kilka korzyści, w tym:

- Można udostępniać stan sesji między dużej liczby wystąpień aplikacji sieci web ASP.NET, dostarczając zwiększona skalowalność
- Obsługuje on kontrolowaną, jednocześnie dostęp do tych samych danych stan sesji dla wielu czytników i Edytor jednego, i
- Służy kompresji zapisywania pamięci i poprawić wydajność sieci.

Aby uzyskać więcej informacji odwiedź stronę [Dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-session-state-provider.md) w witrynie sieci Web firmy Microsoft.

> [AZURE.NOTE] Nie należy używać dostawcy stan sesji Azure Redis w pamięci podręcznej dla aplikacji ASP.NET, uruchamianych poza Azure środowiska. Opóźnienie uzyskiwania dostępu do pamięci podręcznej z poza Azure wyeliminować zalety wydajności pamięci podręcznej danych.

Podobnie dostawcy pamięci podręcznej danych wyjściowych Azure Redis w pamięci podręcznej umożliwia zapisanie odpowiedzi HTTP generowany przez aplikację sieci web programu ASP.NET. Za pomocą dostawcy pamięci podręcznej danych wyjściowych z pamięci podręcznej Azure Redis poprawić czas odpowiedzi aplikacji renderowanych złożonych wyników HTML; wystąpień aplikacji generowania podobne odpowiedzi można wprowadzić za pomocą fragmentów udostępnionego wyjścia w pamięci podręcznej zamiast generowania nowo wyjściowy kod HTML.  Aby uzyskać więcej informacji odwiedź stronę [ASP.NET dane wyjściowe pamięci podręcznej dostawcy Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-output-cache-provider.md) w witrynie sieci Web firmy Microsoft.

### <a name="azure-redis-cache"></a>Azure Redis pamięci podręcznej

Azure Redis pamięci podręcznej zapewnia dostęp do serwerów Redis, które są obsługiwane w Azure centrum danych. Działa jako elewacji, która zapewnia zabezpieczeń i kontroli dostępu. Za pomocą portalu Azure umożliwia obsługę pamięci podręcznej.

Portalu zawiera wiele wstępnie zdefiniowanych konfiguracji. Te z zakresu od 53 GB pamięci podręcznej działającego jako dedykowane usługi obsługującej komunikacji SSL (w przypadku privacy) i replikacji nadrzędny podrzędny z SLA 99,9% dostępności, w dół do 25 0 MB pamięci podręcznej bez replikacji (nie gwarancje dostępności) udostępnionego sprzęt z systemem.

Za pomocą portalu Azure, można również skonfigurować zasady eksmisji zwiększany pamięci podręcznej i kontrolowanie dostępu do pamięci podręcznej, dodając użytkowników do ról opisane.  Role te definiujące czynności, które mogą wykonywać członkowie zawiera właściciela, współautorów i czytnika. Na przykład członkowie roli właściciela mają pełną kontrolę nad pamięci podręcznej (w tym zabezpieczeń) i jego zawartość, członkowie roli współautora można odczytywanie i zapisywanie informacji w pamięci podręcznej i członkowie roli czytnika tylko można pobierać dane z pamięci podręcznej.

Większość zadań administracyjnych są wykonywane za pośrednictwem portalu Azure. Z tego powodu wiele poleceń administracyjnych, które są dostępne w wersji standardowej Redis są niedostępne, łącznie z możliwością programowy zmodyfikować konfigurację, zamknięcia serwera Redis, skonfigurować dodatkowe podwładnych lub wymusić zapisywanie danych na dysku.

Azure portal zawiera wygodny graficzne umożliwiający monitorowanie wydajności pamięci podręcznej. Na przykład możesz wyświetlić liczbę połączeń odbywa się liczba żądań wykonywana, wielkość Odczyt i zapis oraz liczby trafień w pamięci podręcznej i Chybienia w pamięci podręcznej. Za pomocą tych informacji, można określić skuteczność pamięci podręcznej i w razie potrzeby przełącz się do innej konfiguracji lub zmiana zasad eksmisji zwiększany.

Ponadto można tworzyć alerty, które powodują wysyłanie wiadomości e-mail do administratora Jeśli co najmniej jeden metryki krytyczne wykraczają poza zakresem oczekiwanych. Można na przykład alert administratora, jeśli liczba trafień pamięci podręcznej jest większa niż określona wartość w ostatniej godziny, ponieważ oznacza pamięci podręcznej jest za mały lub danych może być zostanie usunięty zbyt szybko.

Można również monitorować Procesora, pamięci i obciążenie sieci pamięci podręcznej.

Dodatkowe informacje i przykłady przedstawiający, jak utworzyć i skonfigurować pamięci podręcznej Azure Redis odwiedź stronę [laptop wokół Azure Redis w pamięci podręcznej](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) w blogu Azure.

## <a name="caching-session-state-and-html-output"></a>Buforowania stan sesji i wyjściowe HTML

Jeśli tworzysz aplikacji sieci web ASP.NET, aby uruchomić przy użyciu role Azure sieci web, możesz zapisać sesji podać informacje i wyjściowy kod HTML w pamięci podręcznej Redis Azure. Dostawca stan sesji Azure Redis w pamięci podręcznej umożliwia udostępnianie informacji o sesji między wystąpieniami różnych aplikacji sieci web ASP.NET i jest bardzo przydatne w sytuacjach farmy sieci web miejsce, w którym klient / serwer koligacji nie jest dostępny i pamięci podręcznej sesji danych w pamięci nie jest właściwe.

Za pomocą dostawcy stan sesji z pamięci podręcznej Azure Redis zapewnia kilka korzyści, w tym:

- Stan sesji udostępniania z dużą liczbą wystąpień aplikacji sieci web ASP.NET.
- Dostarczanie zwiększona skalowalność.
- Obsługi kontrolowaną, jednocześnie dostęp do tych samych danych stan sesji dla wielu czytników i Edytor jednego.
- Aby zapisać pamięci i poprawić wydajność sieci przy użyciu kompresji.

Aby uzyskać więcej informacji odwiedź stronę [dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-session-state-provider.md) w witrynie sieci Web firmy Microsoft.

> [AZURE.NOTE] Nie należy używać dostawcy stan sesji Azure Redis w pamięci podręcznej z aplikacjami ASP.NET, uruchamianych poza Azure środowiska. Opóźnienie uzyskiwania dostępu do pamięci podręcznej z poza Azure wyeliminować zalety wydajności pamięci podręcznej danych.

Podobnie dostawcy pamięci podręcznej danych wyjściowych Azure Redis w pamięci podręcznej umożliwia zapisanie odpowiedzi HTTP generowany przez aplikację sieci web programu ASP.NET. Za pomocą dostawcy pamięci podręcznej danych wyjściowych z pamięci podręcznej Azure Redis poprawić czas odpowiedzi aplikacji renderowanych złożonych wyników HTML. Można wprowadzić wystąpień aplikacji, generujących podobne odpowiedzi stosowania fragmentów udostępnionego wyjścia w pamięci podręcznej zamiast generowania nowo wyjściowy kod HTML. Aby uzyskać więcej informacji odwiedź stronę programu [ASP.NET dane wyjściowe pamięci podręcznej dostawcy Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-output-cache-provider.md) w witrynie sieci Web firmy Microsoft.

## <a name="building-a-custom-redis-cache"></a>Tworzenie niestandardowego Redis pamięci podręcznej

Azure Redis pamięci podręcznej działa jako elewacji źródłowych serwery Redis. Obecnie obsługuje zbiór stałe konfiguracje ale nie przewiduje Redis klaster. Jeśli wymaga konfiguracji zaawansowanej, które nie są objęte Azure Redis pamięci podręcznej (na przykład większa niż 53 GB pamięci podręcznej) można tworzyć i udostępniać serwery Redis przy użyciu Azure maszyn wirtualnych.

Jest to potencjalnie złożonym procesem, ponieważ może być konieczne tworzenie kilku maszyny wirtualne ma pełnić rolę wzorca i podrzędnymi węzły, jeśli chcesz realizowania replikacji. Ponadto jeśli chcesz utworzyć klaster, następnie należy wiele wzorców i serwery podrzędne. Topologii minimalnego replikacji grupowany zawierającego wysoki stopień dostępności i skalowalność składa się z co najmniej sześciu maszyny wirtualne zorganizowane jako trzy pary serwerów nadrzędny podrzędny (klastrze musi zawierać co najmniej trzy węzły wzorca).

Każdej pary nadrzędny podrzędny powinien znajdować się blisko siebie, aby zminimalizować opóźnienie. Jednak każdy zestaw pary może być uruchomiony w różnych Azure centrach danych znajduje się w różnych regionów, jeśli chcesz zlokalizować buforowane Zamknij aplikacje, które najprawdopodobniej będzie używać tej funkcji. Strona [Uruchamiania Redis na maszyny Linux CentOS platformy Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) w witrynie sieci Web firmy Microsoft opisano tym przykładzie pokazano, jak utworzyć i skonfigurować węzeł Redis działającego jako maszyn wirtualnych Azure.

[AZURE.NOTE] Pamiętaj, że w przypadku zastosowania własnego Redis pamięci podręcznej w ten sposób, możesz są odpowiedzialne za monitorowanie, zarządzanie i zabezpieczanie usługi.

## <a name="partitioning-a-redis-cache"></a>Podział Redis pamięci podręcznej

Podział pamięci podręcznej polega na dzielenie pamięci podręcznej na wielu komputerach. Ta struktura udostępnia wiele zalet przy użyciu serwera pojedynczy pamięci podręcznej, w tym:

- Tworzenie pamięci podręcznej, który jest znacznie większy niż mogą być przechowywane na serwerze.
- Rozpowszechnianie danych na serwerach, zwiększanie dostępności. Jeśli jeden serwer kończy się niepowodzeniem lub staje się niedostępna, dane, które zawiera, jest niedostępna, ale dane na pozostałe serwery nadal są dostępne. W pamięci podręcznej to nie ważnych ponieważ buforowane jest przejściowych kopię danych, który jest przechowywany w bazie danych. Zamiast tego zapisujące dane na serwerze, która staje się niedostępna mogą buforowane na innym serwerze.
- Rozłożenie obciążenia serwerów, zwiększając wydajność i skalowalność.
- Zamknij Geolocating danych dla użytkowników łączących się z nią, co powoduje zmniejszenie opóźnienie.

W pamięci podręcznej najczęściej rodzaj podziału jest sharding. W tym strategii każdego partition (lub shard) jest Redis pamięci podręcznej w osobnym. Danych są kierowane do określonego partition za pomocą reguł sharding, którego można używać różnych metod do rozpowszechniania danych. [Wzór Sharding](http://msdn.microsoft.com/library/dn589797.aspx) zawiera więcej informacji na temat wykonywania sharding.

Aby zastosować podziału w pamięci podręcznej Redis, można wykonać jedną z następujących metod:

- _Routing kwerend po stronie serwera._ Tej techniki aplikacją kliencką przesyła żądanie do wszystkich serwerów Redis, które składają się pamięci podręcznej (prawdopodobnie serwer najbliższej). Każdy serwer Redis przechowuje metadane opisujące partycją go zawiera, a także informacje o tym, które znajdują się partycje na innych serwerach. Serwer Redis sprawdza, czy żądania klienta. Jeśli może zostać rozwiązany lokalnie, wykona żądanej operacji. W przeciwnym razie przekazuje żądanie do odpowiedniego serwera. Ten model jest wykonywane przez Redis klaster i opisano bardziej szczegółowo [Redis samouczek klaster](http://redis.io/topics/cluster-tutorial) na stronie Redis witryny sieci Web. Klaster redis jest niewidoczny dla aplikacji klienckich i dodatkowe serwery Redis można dodać do klaster (i danych ponownie podzielona) bez konieczności ponownego konfigurowania klientów.

- _Po stronie klienta podziału._ W tym modelu z aplikacją kliencką zawiera logikę (ewentualnie w formularzu biblioteki), która kieruje żądania do odpowiedniego serwera Redis. Tej metody można używać z Azure Redis w pamięci podręcznej. Tworzenie wielu Azure Redis pamięci podręcznej (po jednej dla każdego partycją danych) i wdrożenie logika po stronie klienta, który przekierowuje żądania poprawne pamięci podręcznej. Jeśli schemat podziału zmieni (Jeśli dodatkowe bufory Redis Azure są tworzone, na przykład), aplikacje klienckie może być konieczne jest skonfigurowana.

- _Serwer proxy pomocy podziału._ W tym schemacie klienta aplikacji Wysyłanie wezwania do usługi pośredniczącego serwera proxy, który rozumie, jak partycją danych i następnie przesyła żądanie do odpowiednich Redis serwera. Tej metody można także z pamięci podręcznej Redis Azure; Usługa serwera proxy, może być wykonany jako usługa w chmurze Azure. Ta metoda wymaga dodatkowy poziom złożoność w celu wdrożenia usługi i żądania może potrwać dłużej przeprowadzić programowi podziału po stronie klienta.

Strony [partycjonowanie: sposób podziału danych między wielu wystąpień Redis](http://redis.io/topics/partitioning) na Redis znajdują się dodatkowe informacje o implementacji podziału z Redis witryny sieci Web.

### <a name="implement-redis-cache-client-applications"></a>Wdrożenie aplikacji klienckich Redis pamięci podręcznej

Redis obsługuje aplikacje klienckie napisanym w wielu językach programowania. Jeśli tworzysz nowe aplikacje przy użyciu programu .NET Framework, to zalecane jest korzystanie z biblioteki klienta StackExchange.Redis. Ta biblioteka zawiera .NET Framework modelu obiektów, które abstracts szczegóły dotyczące nawiązywania połączenia z serwerem Redis, wysyłanie poleceń i otrzymujesz odpowiedzi. Jest ona dostępna w programie Visual Studio jako pakiet NuGet. Tej samej biblioteki umożliwia nawiązanie połączenia z pamięci podręcznej Azure Redis lub niestandardowe pamięci podręcznej Redis hostowana w maszyny.

Aby połączyć się z serwerem Redis Użyj statycznego `Connect` metody `ConnectionMultiplexer` zajęć. Połączenie, które tworzy ta metoda jest przeznaczony do użytku w czasie z aplikacją kliencką i tego samego połączenia mogą być używane przez wiele wątków jednocześnie. Ponownie nawiązać połączenie i nie Odłącz za każdym razem, wykonywanie operacji Redis, ponieważ może to zmniejszyć wydajność.

Możesz określić parametry połączenia, takie jak adres hosta Redis oraz hasło. Jeśli korzystasz z pamięci podręcznej Azure Redis hasło jest albo podstawowym lub pomocniczym klucz, który jest generowany Azure Redis w pamięci podręcznej za pomocą portalu zarządzania Azure.

Po utworzeniu połączenia z serwerem Redis, można uzyskać uchwyt Redis bazie danych, która pełni funkcję pamięci podręcznej. Połączenie Redis `GetDatabase` metody, w tym celu. Można pobierać elementy z pamięci podręcznej i przechowywania danych w pamięci podręcznej za pomocą `StringGet` i `StringSet` metod. Te metody inne niż oczekiwane klucz jako parametr i zwracać elementu w pamięci podręcznej, które ma pasującą wartość (`StringGet`) lub Dodawanie elementu do pamięci podręcznej przy użyciu tego klucza (`StringSet`).

W zależności od lokalizacji na serwerze Redis wiele operacji może spowodować opóźnienie niektórych żądanie są przesyłane do serwera i odpowiedź są zwracane do klienta. Biblioteka StackExchange informacje asynchroniczne wersji wiele metod, które udostępnia go pomocnych w aplikacjach klienckich pozostaną odpowiada. Te metody obsługuje [Opartym na zadaniu asynchroniczne deseniu](http://msdn.microsoft.com/library/hh873175.aspx) w architekturze .NET Framework.

Następujący fragment kodu zawiera metodę o nazwie `RetrieveItem`. Implementacja deseń odłogowania pamięci podręcznej, oparty na Redis i biblioteki StackExchange go ilustruje. Metoda pobiera wartość klucza ciąg i próbuje pobrać odpowiedni element z pamięci podręcznej Redis, dzwoniąc `StringGetAsync` metody (wersja asynchroniczne `StringGet`).

Jeśli element nie zostanie znaleziony, są pobierane z podstawowych danych źródłowych przy użyciu `GetItemFromDataSourceAsync` metody (jest to metoda lokalny i nie jest częścią biblioteki StackExchange). Następnie jest dodawana do pamięci podręcznej przy użyciu `StringSetAsync` metody, może je pobrać szybciej następnym razem.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

`StringGet` i `StringSet` metody nie są ograniczone do pobierania i przechowywania wartości ciągu. Mogą przyjmować dowolnego elementu, który jest seryjny jako tablicę bajtów. Jeśli chcesz zapisać obiekt .NET można szeregować go jako strumień bajtów i używać `StringSet` sposobem zapisywanie w pamięci podręcznej.

Podobnie, można odczytać obiektu z pamięci podręcznej, przy użyciu `StringGet` metody i deserializacji go jako obiekt .NET. Poniższy kod przedstawia zestaw metod rozszerzenia interfejsu IDatabase ( `GetDatabase` metoda połączenia Redis zwraca `IDatabase` obiekt) oraz niektóre przykładowy kod, która korzysta z tych metod do odczytu i zapisu `BlogPost` obiektu w pamięci podręcznej:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Poniższy kod ilustruje metodę o nazwie `RetrieveBlogPost` korzystającego z tych metod rozszerzenia do odczytu i zapisu można `BlogPost` obiektu w pamięci podręcznej, wykonując deseniu odłogowania pamięci podręcznej:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis obsługuje polecenie Przetwarzanie potokowe Jeśli aplikacją kliencką wysyła wielu żądania asynchroniczne. Redis można Multipleksowanie żądania przy użyciu tego samego połączenia zamiast odbierania i odpowiada na polecenia w sekwencji ściśle.

Ta metoda pozwala zmniejszyć opóźnienie dokonując efektywniejsze korzystanie z sieci. Poniższy fragment kodu przedstawiono przykład pobierającego dane klientów dwóch jednocześnie. Kod przesyła dwa żądania, a następnie przetwarza niektóre inne (niewidoczne) przed oczekujące na wyniki. `Wait` Metody obiektu pamięci podręcznej jest podobne do .NET Framework `Task.Wait` metody:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Strona [dokumentacji Azure Redis w pamięci podręcznej](https://azure.microsoft.com/documentation/services/cache/) w witrynie sieci Web firmy Microsoft zawiera więcej informacji na temat tworzenia aplikacje klienckie używające pamięci podręcznej Redis Azure. Dodatkowe informacje są dostępne na [podstawowe zastosowania strony](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) w witrynie sieci Web StackExchange.Redis.

Na stronie [potoki i multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) na tym samym witryny sieci Web jest więcej informacji na temat operacji asynchronicznych i przetwarzanie potokowe z Redis i biblioteki StackExchange.  Następnej sekcji w tym artykule, przy użyciu Redis pamięci podręcznej, znajdują się przykłady niektórych bardziej zaawansowanych technik, które można stosować do danych, które są przechowywane w pamięci podręcznej Redis.

## <a name="using-redis-caching"></a>Za pomocą Redis pamięci podręcznej

Najłatwiejszym korzystanie z Redis w pamięci podręcznej dotyczy jest par klucz wartość, których wartość jest ciągiem Niezinterpretowana o dowolnej długości, która może zawierać dowolne dane binarne. (Jest to zasadniczo tablicę bajtów, które mogą być traktowane jako ciąg). W tym scenariuszu zostało przedstawione w aplikacjach klienckich Implementowanie Redis w pamięci podręcznej sekcję w tym artykule.

Zauważ, że klawiszy również zawierać Niezinterpretowana danych, aby móc używać wszelkie informacje binarne jako klucz. Im dłuższy jest klucz, jednak więcej miejsca spowoduje przejście do przechowywania i dłużej trwa wykonywanie operacji wyszukiwania. Dla użyteczności i łatwości konserwacji starannie zaprojektować usługi keyspace, a następnie użyj klawiszy zrozumiałej (ale nie pełne).

Na przykład użyj klawiszy strukturalne, takie jak "klienta: 100" w celu przedstawienia klucz dla klienta przy użyciu Identyfikatora 100 zamiast po prostu "100". Ten program pozwala łatwo odróżnić wartości, które przechowywanie różnych typów danych. Na przykład również można klucz "zamówienia: 100" reprezentować klucz 100 identyfikator zamówienia.

Oprócz jednowymiarowych ciągi binarne wartości w pary klucz wartość Redis można też nacisnąć bardziej uporządkowanego informacji, takich jak list, ustawia (sortowane i nieposortowane) i mieszania. Redis udostępnia zestaw pełna polecenia, które można modyfikować tych typów, a wiele z tych poleceń są dostępne dla aplikacji .NET Framework za pośrednictwem biblioteki klienta, takich jak StackExchange. Na stronie [Wprowadzenie do Redis typów danych i poboru](http://redis.io/topics/data-types-intro) w witrynie sieci Web Redis jest bardziej szczegółowe omówienie tych typów i polecenia, których można użyć manipulować nimi.

W tej sekcji przedstawiono kilka typowych przypadków użycia dla tych typów danych i poleceń.

### <a name="perform-atomic-and-batch-operations"></a>Prowadzenie Atomowej i operacje wsadowe

Redis obsługuje serii Atomowej operacji get i ustaw wartości ciągu. Te operacje usuwanie zagrożenia możliwe wyścigowe, które mogą wystąpić podczas korzystania z osobnych `GET` i `SET` polecenia. Operacji, które są dostępne należą:

- `INCR`, `INCRBY`, `DECR`, i `DECRBY`, które spełniają Atomowej przyrostu i Zmniejsz operacji na danych liczbowych liczby całkowite. Biblioteka StackExchange informacje przeciążone wersji `IDatabase.StringIncrementAsync` i `IDatabase.StringDecrementAsync` sposobów wykonywania tych operacji i zwracana wartość wyniku, który jest przechowywany w pamięci podręcznej. Poniższy fragment kodu ilustruje sposób użycia tych metod:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, który pobiera wartość, która jest skojarzony z kluczem i zmieni go na nową wartość. Biblioteka StackExchange sprawia, że tej operacji są dostępne za pośrednictwem `IDatabase.StringGetSetAsync` metody. Wstawkę kodu poniżej przedstawiono przykład tej metody. Kod zwraca wartość bieżącą, który jest skojarzony z kluczem "danych: licznik" z poprzedniego przykładu. Następnie resetuje wartość tego klucza do zera, wszystkie jako część tej samej operacji:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`i `MSET`, które zwracana lub zmienianie zestawu wartości ciągu jako pojedyncza. `IDatabase.StringGetAsync` i `IDatabase.StringSetAsync` metod są nadmiernie obsługuje tę funkcję, jak pokazano w poniższym przykładzie:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Można także połączyć wiele operacji do jednej transakcji Redis, zgodnie z opisem w sekcji transakcje i partie Redis wcześniej w tym artykule. Biblioteka StackExchange zapewnia obsługę transakcji za pośrednictwem `ITransaction` interfejsu.

Możesz utworzyć `ITransaction` obiektu przy użyciu `IDatabase.CreateTransaction` metody. Wywołania polecenia do transakcji przy użyciu metod dostarczonych wraz z `ITransaction` obiektu.

`ITransaction` Interfejsu zapewnia dostęp do zestawu metod jest podobne do tych dostępna dla firmy `IDatabase` interfejs, z wyjątkiem, że wszystkie metody są asynchroniczne. Oznacza to, że są one tylko wykonywane, kiedy `ITransaction.Execute` metody. Wartość, który został zwrócony przez `ITransaction.Execute` metoda wskazuje, czy transakcja została utworzona pomyślnie (PRAWDA) lub jeśli nie powiodło się (false).

Poniższy fragment kodu przedstawia przykładową tego liczniki dwóch etapami i zmniejsza w ramach tej samej transakcji:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Należy pamiętać, że transakcje Redis w przeciwieństwie do transakcji w relacyjnych baz danych. `Execute` Metody po prostu kolejek wszystkie polecenia, które składają się transakcji, która ma być uruchamiane, i w przypadku dowolnej z nich utworzonym transakcji został zatrzymany. Jeśli wszystkie polecenia zostały umieszczone w kolejce pomyślnie, każdego polecenia uruchamiane asynchroniczne.

Jeśli polecenie nie powiedzie się, pozostałe nadal przetwarzania. Jeśli potrzebujesz zweryfikować, że polecenia zakończyło się pomyślnie, możesz pobrać wyniki polecenia przy użyciu właściwości **wynik** odpowiednie zadanie, tak jak w powyższym przykładzie. Właściwość **Result** do czytania Blokuj połączeń wątku zakończenia zadania.

Aby uzyskać więcej informacji zobacz stronę [transakcji w Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) w witrynie sieci Web StackExchange.Redis.

Po wykonaniu operacji wsadowe można używać `IBatch` interfejsu biblioteki StackExchange. Ten interfejs zapewnia dostęp do zestaw metod podobne do tych dostępna dla firmy `IDatabase` interfejs, z wyjątkiem, że wszystkie metody są asynchroniczne.

Możesz utworzyć `IBatch` obiektu za pomocą `IDatabase.CreateBatch` metody, a następnie uruchomić partię przy użyciu `IBatch.Execute` metody, jak pokazano w poniższym przykładzie. Kod po prostu ustawia wartość ciągu, etapami i zmniejsza tym samym liczniki używane w poprzednim przykładzie, a wyniki są wyświetlane:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Należy zrozumieć, że w odróżnieniu od transakcji, jeśli polecenie w partii kończy się niepowodzeniem, ponieważ jest nieprawidłowo, inne polecenia może nadal działać. `IBatch.Execute` Metoda nie zwraca wskazanie powodzenia lub niepowodzenia.

### <a name="perform-fire-and-forget-cache-operations"></a>Prowadzenie fire i zapomnij operacje pamięci podręcznej

Redis fire obsługuje i zapomnij operacji przy użyciu polecenia flag. W tej sytuacji klient po prostu inicjuje operację, ale występują żadne odsetki w wyniku i nie czeka polecenia do wykonania. W poniższym przykładzie przedstawiono sposób wykonuje polecenie INCR jako ognia i zapomnij operacji:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Określanie klawiszy automatycznie wygasania

Po umieszczeniu elementu w pamięci podręcznej Redis można określić limit czasu, po upływie którego elementu zostaną automatycznie usunięte z pamięci podręcznej. Można również sprawdzać, ile dłużej klucz występują przed jej wygaśnięciem przy użyciu `TTL` polecenia. To polecenie jest dostępne dla aplikacji StackExchange przy użyciu `IDatabase.KeyTimeToLive` metody.

Poniższy fragment kodu przedstawia sposób Ustawianie czasu wygaśnięcia 20 sekund na kluczu i kwerend pozostały czas użytkowania klucza:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Czas wygaśnięcia można także ustawić określoną datę i godzinę, przy użyciu polecenia wygaśnięcia, które są dostępne w bibliotece StackExchange jako `KeyExpireAsync` metody:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Porada:_ Możesz ręcznie usunąć element z pamięci podręcznej, przy użyciu polecenia DEL, które jest dostępne za pośrednictwem biblioteki StackExchange jako `IDatabase.KeyDeleteAsync` metody.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Elementy buforowane powiązania między za pomocą znaczników

Zestaw Redis to zbiór wiele elementów, które współużytkują jednego klucza. Można utworzyć zestaw przy użyciu polecenia SADD. Elementy w zestawie można pobrać za pomocą polecenia SMEMBERS. Biblioteka StackExchange zawiera polecenia SADD za `IDatabase.SetAddAsync` polecenie metody i SMEMBERS z `IDatabase.SetMembersAsync` metody.

Można także połączyć istniejących zestawów, aby utworzyć nowe zestawy przy użyciu SDIFF (Ustawianie różnica), SPIEKIEM (Ustawianie przecięcia) i polecenia SUNION (Ustawianie Unii). Biblioteka StackExchange łączy te operacje w `IDatabase.SetCombineAsync` metody. Pierwszy parametr tej metody określa operację zestawu do wykonania.

Następujące fragmenty kodu pokazano, jak zestawy może być przydatne do szybkiego przechowywania i pobierania kolekcje elementów pokrewnych. Ten kod zawiera `BlogPost` typu opisaną w sekcji aplikacje klienckie pamięci podręcznej Redis Implementowanie wcześniej w tym artykule.

A `BlogPost` obiekt zawiera cztery pola — Identyfikatora, tytuł, wynik klasyfikację i zbiór znaczniki. Pierwszy fragment kodu, poniżej zawiera dane przykładowe służy do wypełniania C# lista `BlogPost` obiektów:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Dla każdej z nich można przechowywać znaczniki `BlogPost` obiektu znajduje się w pamięci podręcznej Redis i skojarzyć każdy zestaw identyfikator `BlogPost`. Dzięki temu aplikację szybko odnaleźć wszystkie znaczniki, które należą do określonego wpisu w blogu. Aby włączyć wyszukiwanie w przeciwnym kierunku i znaleźć wszystkich wpisów w blogu, które współużytkują konkretnego znacznika, można utworzyć inny zestaw, który zawiera wpisów blogu odwoływanie się do Identyfikatora znacznika w kluczu:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Te struktury umożliwiają bardzo efektywnie wykonywać wiele typowych kwerend. Można na przykład znaleźć i wyświetlić wszystkie znaczniki dla blogu 1 w następujący sposób:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Można znaleźć wszystkie znaczniki, które są wspólne dla blogu opublikować wpis w blogu i 1 2 za pomocą operacji zestawu przecięcia, w następujący sposób:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

I można znaleźć wszystkich wpisów w blogu, zawierające konkretnego znacznika:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Znajdowanie ostatnio używanych elementów

Typowe zadania wymagane z wielu aplikacji jest znajdowanie najczęściej ostatnio używanych elementów. Na przykład w witrynie blogu może być do wyświetlania informacji na temat najbardziej ostatnio odczytu wpisów w blogu.

Tej funkcji można zaimplementować przy użyciu listy Redis. Lista Redis zawiera wiele elementów, które współużytkują ten sam klucz. Listy działa jako kolejka dwóch grotach. Elementy można przekazać do obu koniec listy za pomocą poleceń (prawo naciśnięcie) RPUSH i LPUSH (po lewej stronie naciśnięcie). Elementy można pobrać z obu koniec listy za pomocą poleceń LPOP i RPOP. Można także zwracać zestaw elementów za pomocą poleceń LRANGE i ROZMIEŚĆ.

Wstawki kodu poniżej pokazano, jak można wykonać następujące operacje za pomocą biblioteki StackExchange. Ten kod zawiera `BlogPost` typ z poprzednich przykładów. Wpis w blogu jest czytania przez użytkownika, `IDatabase.ListLeftPushAsync` metody umieszcza tytuł wpisu w blogu na listę, który jest skojarzony z klucza "blog:recent_posts" w pamięci podręcznej Redis.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Jak więcej wpisów w blogu będą mogli odczytywać, ich tytuły są usuwane na tej samej liście. Lista jest uporządkowana według kolejności, w jakiej zostały dodane tytuły. Są najbardziej ostatnio odczytu wpisów w blogu na lewym końcu listy. (Jeśli samej wpis w blogu jest więcej niż jeden raz do odczytu, będzie miał wiele wpisów na liście.)

Tytuły najbardziej ostatnio Przeczytaj wpisy można wyświetlić za pomocą `IDatabase.ListRange` metody. Ta metoda przyjmuje klucz, który zawiera listę, punktu początkowego i punktu końcowego. Poniższy kod pobiera tytuły 10 wpisów w blogu (elementy od 0 do 9) na końcu lewej listy:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Należy zauważyć, że `ListRangeAsync` metody nie powoduje usunięcia elementów z listy. Aby to zrobić, możesz użyć `IDatabase.ListLeftPopAsync` i `IDatabase.ListRightPopAsync` metod.

Aby zapobiec rosnących czas nieokreślony na liście, można okresowo wybrakowane elementów poprzez przycinanie na liście. Wstawkę kodu poniżej pokazano, jak usunąć wszystkie, ale pięciu lewej elementów z listy:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Implementowanie tablicy znak wiodący

Domyślnie elementy w zestawie nie są przechowywane w określonej kolejności. Uporządkowany zestaw można utworzyć przy użyciu polecenia ZADD ( `IDatabase.SortedSetAdd` metody w bibliotece StackExchange). Elementy są uporządkowane według przy użyciu wartości liczbowej o nazwie wynik, który jest dostępny jako parametr do polecenia.

Poniższy fragment kodu dodaje tytuł wpisu w blogu do listy uporządkowanej. W tym przykładzie każdego wpisu w blogu zawiera pole wynik, które zawiera klasyfikację wpis w blogu.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Możesz pobierać tytuły wpis w blogu i wyniki rosnąco wynik przy użyciu `IDatabase.SortedSetRangeByRankWithScores` metody:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] Udostępnia biblioteki StackExchange `IDatabase.SortedSetRangeByRankAsync` metodę, która zwraca dane w kolejności wynik, ale nie zwraca wyników.

Można również pobierać elementy w porządku malejącym wyników i ograniczyć liczbę elementów, które są zwracane, dostarczając dodatkowe parametry `IDatabase.SortedSetRangeByRankWithScoresAsync` metody. Następny przykład Wyświetla tytuły i wyniki górny 10 sklasyfikowane wpisów w blogu:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

W następnym przykładzie użyto `IDatabase.SortedSetRangeByScoreWithScoresAsync` metodę, która umożliwia ograniczanie elementy, które są zwracane do tych, które się mieścić w wyniku danego zakresu:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Wiadomości za pomocą kanałów

Oprócz działający jako pamięci podręcznej danych, serwer Redis zapewnia, wiadomości wysyłane przez mechanizm wysokiej wydajności programu publisher i subskrybenta. Aplikacje klienckie można zasubskrybować kanału i innych aplikacji lub usług można publikować wiadomości do kanału. Subskrybowanie aplikacje następnie będą otrzymywać te wiadomości i może przetworzyć je.

Redis zawiera polecenie SUBSKRYBUJ aplikacje klienckie za pomocą subskrybować kanały. To polecenie zakłada nazwę jednego lub kilku kanałów, na których ta aplikacja akceptuje wiadomości. Biblioteka StackExchange zawiera `ISubscription` interfejs, co umożliwia aplikacja .NET Framework do subskrybowania i publikowanie kanałów.

Możesz utworzyć `ISubscription` obiektu za pomocą `GetSubscriber` metody połączenia z serwerem Redis. Następnie nasłuchują komunikatów kanałem przy użyciu `SubscribeAsync` metody tego obiektu. W poniższym przykładzie pokazano sposób subskrybować kanał o nazwie "wiadomości: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Pierwszy parametr `Subscribe` metoda jest nazwę kanału. Ta nazwa wykonuje tych samych konwencji, które są używane przez klawiszy w pamięci podręcznej. Nazwa może zawierać dowolne dane binarne, jednak zaleca się używać ciągów stosunkowo krótkim, znaczących aby zapewnić dobrej wydajności i łatwość.

Należy zauważyć, że nazw, używany przez kanały różni się od używana przez klawiszy. Oznacza to, że możesz mieć kanałów i klawiszy, które mają taką samą nazwę, chociaż może być trudne do obsługi kodzie aplikacji.

Drugi parametr jest pełnomocnika akcji. Ten pełnomocnika asynchroniczne uruchamiane zawsze, gdy przychodzi nowa wiadomość jest wyświetlana w kanale. W tym przykładzie po prostu wyświetli komunikat na konsoli (wiadomość będzie zawierać tytuł wpisu w blogu).

Aby opublikować do kanału, aplikacja użyć polecenia Redis publikowanie. Udostępnia biblioteki StackExchange `IServer.PublishAsync` metody do wykonania tej operacji. Następny wstawkę kodu pokazano, jak publikować wiadomości do kanału "wiadomości: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Istnieje kilka punktów, które należy zrozumieć o mechanizmu publikowania/subskrypcji:

- Wiele subskrybentów subskrybować tego samego kanału, a wszystkie otrzymają wiadomości, które są publikowane w tym kanale.
- Subskrybenci odbierać wiadomości, które zostały opublikowane po przyjęły. Kanały nie są buforowane, a po opublikowaniu wiadomości infrastruktury Redis umieszcza wiadomości do każdego subskrybentów, a następnie usunie ją.
- Domyślnie wiadomości są odbierane przez subskrybentów w kolejności, w jakiej są wysyłane. W układzie wysoce aktywnej z dużą liczbą wiadomości i wielu subskrybentów i wydawców sekwencyjne dostarczenie wiadomości może zmniejszyć wydajność systemu. Każda wiadomość jest niezależna od kolejności jest ważne, aby umożliwić równoczesne przetwarzanie przez system Redis, co może pomóc w celu zwiększenia czas reakcji. Można to osiągnąć w kliencie StackExchange, ustawiając PreserveAsyncOrder połączenia używanego przez abonenta ma wartość FAŁSZ:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Wskazówki i desenie pokrewne

Podczas implementowania pamięci podręcznej w aplikacji następującego wzorca może być także dotyczącym rozwiązania:

- [Pamięć podręczną odłogowania deseniu](http://msdn.microsoft.com/library/dn589799.aspx): tego wzorca opisano, jak załadować dane na żądanie w pamięci podręcznej z magazynu danych. Ten wzorzec pomaga zachować spójność danych, który jest przechowywany w pamięci podręcznej i danych w oryginalnego magazynu danych.
- [Wzór Sharding](http://msdn.microsoft.com/library/dn589797.aspx) zawiera informacje dotyczące wdrażania poziomy podziału w celu zwiększenia skalowalność podczas przechowywania i uzyskiwanie dostępu do dużych ilości danych.

## <a name="more-information"></a>Więcej informacji

- Strona [klasy MemoryCache](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) w witrynie sieci Web firmy Microsoft
- Strona [dokumentacji Azure Redis w pamięci podręcznej](https://azure.microsoft.com/documentation/services/cache/) w witrynie sieci Web firmy Microsoft
- Strony [Azure Redis pamięci podręcznej często zadawane pytania](redis-cache/cache-faq.md) w witrynie sieci Web firmy Microsoft
- Strony [model konfiguracji](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) w witrynie sieci Web firmy Microsoft
- Strony [Opartym na zadaniu asynchroniczne deseniu](http://msdn.microsoft.com/library/hh873175.aspx) w witrynie sieci Web firmy Microsoft
- Strony [potoki i multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) na repo StackExchange.Redis GitHub
- Strona [Redis pozostawanie](http://redis.io/topics/persistence) w witrynie sieci Web Redis
- [Replikacja strony](http://redis.io/topics/replication) w witrynie sieci Web Redis
- [Redis klaster samouczek](http://redis.io/topics/cluster-tutorial) strony w witrynie sieci Web Redis
- [Partycjonowanie: sposób podziału danych między wielu wystąpień Redis](http://redis.io/topics/partitioning) strony w witrynie sieci Web Redis
- [Za pomocą Redis jako pamięci podręcznej LRU](http://redis.io/topics/lru-cache) strony w witrynie sieci Web Redis
- [Transakcje](http://redis.io/topics/transactions) strony w witrynie sieci Web Redis
- [Redis zabezpieczeń](http://redis.io/topics/security) strony w witrynie sieci Web Redis
- Strona [laptop wokół Azure Redis w pamięci podręcznej](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) w blogu Azure
- [Uruchamianie Redis na maszyny Linux CentOS platformy Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) stron w witrynie sieci Web firmy Microsoft
- Strony [dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-session-state-provider.md) w witrynie sieci Web firmy Microsoft
- Strony [programu ASP.NET dane wyjściowe pamięci podręcznej dostawcy Azure Redis w pamięci podręcznej](redis-cache/cache-aspnet-output-cache-provider.md) w witrynie sieci Web firmy Microsoft
- [Wprowadzenie do Redis typów danych i poboru](http://redis.io/topics/data-types-intro) strony w witrynie sieci Web Redis
- [Podstawowe zastosowania](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) strony w witrynie sieci Web StackExchange.Redis
- Strony [transakcji w Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) na StackExchange.Redis repo
- [Przewodnik podziału danych](http://msdn.microsoft.com/library/dn589795.aspx) w witrynie sieci Web firmy Microsoft
