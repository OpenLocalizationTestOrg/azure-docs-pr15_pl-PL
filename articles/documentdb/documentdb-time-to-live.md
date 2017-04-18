<properties
   pageTitle="Wygaśnięcie danych w DocumentDB czasu na żywo | Microsoft Azure"
   description="TTL Microsoft Azure DocumentDB umożliwia dokumentów automatycznie usunięte z systemu po upływie pewnego czasu."
   services="documentdb"
   documentationCenter=""
   keywords="czas wygaśnięcia"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Dane w kolekcji DocumentDB automatycznie z czas wygaśnięcia wygasało

Aplikacje można utworzyć i zapisać dużych ilości danych. Niektóre z tych danych, na przykład komputera wygenerowane zdarzenia danych, dzienników i użytkownika sesji informacje są przydatne tylko przez ograniczony czas. Gdy dane będą nadmiar na potrzeby aplikacji jest bezpieczne usunąć te dane i zmniejszyć wymagania miejsca do magazynowania aplikacji.

"Czas wygaśnięcia" lub TTL Microsoft Azure DocumentDB umożliwia dokumentów automatycznie usunięte z bazy danych po upływie pewnego czasu. Domyślny czas wygaśnięcia można ustawić na poziomie zbioru i zastąpić na podstawie poszczególnych dokumentów. Po ustawieniu TTL jako domyślnego zbioru lub na poziomie dokumentu, DocumentDB automatycznie usunie dokumenty, które istnieje po upływie tego czasu w sekundach, ponieważ ich ostatniej modyfikacji.

Czas wygaśnięcia w DocumentDB używa przesunięcie przed, kiedy ostatnio zmodyfikował dokument. W tym celu używa pola _ts, która istnieje w każdym dokumencie. Pole _ts jest sygnatura czasowa typu unix epoch reprezentującą datę i godzinę. Pole _ts jest aktualizowane za każdym razem, dokument zostanie zmodyfikowany. 

## <a name="ttl-behavior"></a>Zachowanie TTL

Funkcja TTL jest kontrolowane przez właściwości TTL na dwóch poziomach - na poziomie zbioru i poziomie dokumentu. Wartości są ustawione w sekundach i są traktowane jako delta z _ts, który ostatnio zmodyfikował dokument na.

 1.  DefaultTTL kolekcji
  * Jeśli brakuje (lub ustaw wartość null), dokumenty nie są usuwane automatycznie.
  
  * W przypadku obecny i wartość "-1" = nieograniczony — dokumentów nie wygasały domyślnie
  
  * Jeśli obecnie i wartości jest niektóre liczby ("n") — wygaśnie dokumentów "n" sekundach od ostatniej modyfikacji

 2.  TTL dokumentów: 
  * Właściwość dotyczy tylko wtedy, gdy dla zbioru nadrzędnej DefaultTTL.
  
  * Zastępuje wartość DefaultTTL kolekcji nadrzędnej.

Po zainicjowaniu wygasła dokumentu (ttl + _ts > = bieżącego czasu serwera), dokument jest oznaczony jako "wygasłe". Nie będą mogli na tych dokumentów po upływie tego czasu i zostaną wykluczone z wyników kwerendy wykonywane. Dokumenty fizycznie są usuwane w systemie i są usuwane w tle używana w późniejszym czasie. To nie używać dowolnego [Żądania jednostki (RUs)](documentdb-request-units.md) z budżetu zbioru.

Logika powyżej może być prezentowany w poniższej tabeli:

|       | Ustawianie Brak nie DefaultTTL kolekcji | DefaultTTL = -1 w zbiorze | DefaultTTL = "n" w zbiorze|
| ------------- |:-------------|:-------------|:-------------|
| Brak TTL w dokumencie| Nic, aby zastąpić na poziomie dokumentu, ponieważ dokumentu i zbioru nie znają pojęcia TTL. | Nie ma żadnych dokumentów, w tym zbiorze wygaśnie. | Dokumenty w tym zbiorze wygaśnie po upływie interwału n. |
| TTL = -1 w dokumencie | Nic, aby zastąpić na poziomie dokumentu, ponieważ kolekcji nie definiuje Właściwość DefaultTTL, którą można zastąpić dokumentu. TTL nad dokumentem jest usunięcie interpretowany przez system. | Nie ma żadnych dokumentów, w tym zbiorze wygaśnie. | Dokument z pola TTL = 1 w tym zbiorze nigdy nie wygasa. Wszystkie inne dokumenty wygaśnie po interwału "n". |
|  TTL = n w dokumencie | Nic, aby zastąpić na poziomie dokumentu. TTL nad dokumentem w nie interpretowany przez system. | Dokument z pola TTL = n wygaśnie po n interwał, w sekundach. Inne dokumenty dziedziczą interwału -1, a nigdy nie wygasa. | Dokument z pola TTL = n wygaśnie po n interwał, w sekundach. Inne dokumenty będą dziedziczyć interwału "n" kolekcji. |


## <a name="configuring-ttl"></a>Konfigurowanie TTL

Domyślnie czas wygaśnięcia jest wyłączona, domyślnie wszystkie zbiory DocumentDB i na wszystkie dokumenty.

## <a name="enabling-ttl"></a>Włączanie TTL

Aby włączyć TTL w kolekcji lub dokumentów w zbiorze, należy ustawić właściwość DefaultTTL zbioru -1 lub liczbę dodatnią zera. Ustawienie DefaultTTL na-1 oznacza, że domyślne wszystkich dokumentów w zbiorze będzie zawsze aktywne, ale DocumentDB usługi należy monitorować kolekcji dla dokumentów, które mają zastąpić to ustawienie domyślne.

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurowanie domyślnego TTL w zbiorze

Jest możliwe skonfigurować domyślny czas wygaśnięcia na poziomie zbioru. 

Aby ustawić wartość TTL w zbiorze, musisz podać liczbę dodatnią zera wskazujący okres, w sekundach, aby wygasało wszystkich dokumentów w zbiorze po ostatniej modyfikacji sygnatura czasowa dokumentu (_ts).

Lub -1, co oznacza, że wszystkie dokumenty, w którym został wstawiony do zbioru zostanie czas nieokreślony live domyślnie można ustawić domyślny.

## <a name="setting-ttl-on-a-document"></a>Ustawienie TTL w dokumencie

Oprócz ustawienia domyślne TTL w zbiorze można ustawić określone TTL na poziomie dokumentu. Spowoduje to zastępują domyślne kolekcji.

Aby ustawić wartość TTL nad dokumentem, musisz podać liczbę dodatnią zera wskazujący okres, w sekundach, aby wygasały dokumentu po ostatniej modyfikacji sygnatura czasowa dokumentu (_ts).

Aby ustawić ten przesunięcie wygaśnięcia, ustawianie pola TTL w dokumencie.

Jeśli dokument zawiera nie ma pola TTL, zostaną zastosowane domyślne kolekcji.

Po wyłączeniu TTL na poziomie zbioru pola TTL w dokumencie będą ignorowane aż TTL włączono ponownie kolekcji.


## <a name="extending-ttl-on-an-existing-document"></a>Rozszerzanie TTL nad istniejącym dokumentem

Możesz zresetować TTL nad dokumentem, wykonując dowolną operacji zapisu dokumentu. Spowoduje to będzie równa _ts bieżącą godzinę, a odliczanie do wygaśnięcia dokumentu, zgodnie z ustawieniem ttl, rozpocznie się ponownie.

Jeśli chcesz zmienić ustawienia ttl dokumentu, możesz zaktualizować pole jak inne pola można ustawić.


## <a name="removing-ttl-from-a-document"></a>Usuwanie TTL z dokumentu

Jeśli ustawiono pola TTL nad dokumentem i nie jest już potrzebne ten dokument, aby wygasało, następnie można pobrać dokument, usuń pola TTL i zastąpić dokument na serwerze.

Po usunięciu pola TTL z dokumentu zostanie zastosowany domyślny kolekcji.

Aby zatrzymać dokumentu z wygasa i nie dziedziczą z kolekcji należy ustawić wartości z pola TTL-1.


## <a name="disabling-ttl"></a>Wyłączanie TTL

Aby wyłączyć TTL całkowicie w zbiorze i zatrzymać proces tła z szukasz wygasłe dokumenty Właściwość DefaultTTL w zbiorze powinny zostać usunięte.

Usunięcie tej właściwości jest inne niż ustawienie -1. Ustawienie do nowych dokumentów-1 oznacza dodawane do kolekcji będzie zawsze live, ale można ją zastąpić na określonych dokumentów w zbiorze.

Usunięcie tej właściwości całkowicie z kolekcji oznacza, że nie ma żadnych dokumentów wygaśnie, nawet w przypadku dokumentów, które zostały jawnie zastąpiona poprzedniego domyślne.


## <a name="faq"></a>FAQ

**Co będzie TTL kosztować mnie?**

Istnieje bez żadnych dodatkowych kosztów ustawienie TTL w dokumencie.

**Jak długo trwa usuwanie Mój dokument, gdy wartość TTL jest uruchomiony?**

Dokumenty są oznaczone jako niedostępny po zainicjowaniu wygasła dokumentu (ttl + _ts > = bieżącego czasu serwera). Nie będą mogli na tych dokumentów po upływie tego czasu i zostaną wykluczone z wyników kwerendy wykonywane. Dokumenty fizycznie są usuwane przez system w tle. Nie zajmie dowolnego RUs z kolekcji budżetu.

**Jeśli trwa upływie pewnego czasu, aby usunąć dokumenty, zostaną one są wliczane do Moje przydziału (i rachunku) do momentu ich usunięcie?**

Nie, po wygaśnięciu dokumentu nie będą nie naliczane do przechowywania dokumentów i rozmiaru dokumentów nie będą wliczane do przydziału miejsca do magazynowania dla zbioru.

**Będzie TTL w dokumencie miały wpływu na opłaty RU**

Nie, nie będzie żadnego wpływu na opłaty RU dla operacji wykonywanych na dowolny dokument w DocumentDB.

**Spowoduje usunięcie dokumentów wpływ na przepustowość, którą można mieć obsługi administracyjnej w mojej zbiorze?**

Nie, obsługiwać żądania przed kolekcji, otrzymają priorytet nad przeprowadzenie procesu tło do usunięcia dokumentów. Dodawanie pola TTL do każdego dokumentu nie wpłynie to.

**Po wygaśnięciu dokumentu, jak długo go pozostanie w mojej zbioru dopóki zostanie usunięty?**

Zaraz po wygaśnięciu dokumentu nie będą dostępne. Dokładną datę i godzinę dokument jest przechowywany w zbiorze przed faktycznie usuwana jest niedeterministyczny i będą oparte na po zakończeniu procesu tła można usunąć dokument.

**Czy wygasłe dokumenty zostaną usunięte we wszystkich węzłach, czy jest to "po pewnym czasie consistent?"**

Dokument będzie dostępna w tym samym czasie we wszystkich węzłach i we wszystkich regionach.

**Czy istnieje kosztem RU monitorowania TTL zadania w tle?**

Nie, jest bezpłatna RU to.

**Jak często są zaznaczone wygaśnięcia TTL**

Sprawdzanie wygaśnięcia TTL nie się dzieje w tle. Podczas odpowiadania na żądanie usługi wewnętrznej bazy danych wykonaj tekście kontroli i wykluczyć wszystkie dokumenty, które wygasły. Usunięcie fizycznego dokumentu polega na tylko asynchroniczne uruchamianego w tle. Częstotliwość ten proces jest określona przez RUs dostępne w zbiorze.

**Czy funkcja TTL są stosowane tylko do całych dokumentów lub wartości nieruchomości pojedynczy dokument może wygaśnie?**

TTL ma zastosowanie do całego dokumentu. Jeśli chcesz wygasało tylko część dokumentu, następnie zalecane jest że wyodrębniania część dokumentu głównego w do innego dokumentu "połączone", a następnie użyj TTL wyodrębnionej dokumentu.

**Czy funkcja TTL ma wymagań indeksowania?**

Wartość Tak. Kolekcja musi być [indeksowania zasad Ustawianie](documentdb-indexing-policies.md) opóźnieniem lub zgodne. Ustawiana DefaultTTL zbioru z indeksowania zestawu Brak spowoduje błąd, podobnie jak próby wyłączyć indeksowanie zbioru, który ma DefaultTTL już ustawione.


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat Azure DocumentDB, można znaleźć na stronie [*dokumentacji*](https://azure.microsoft.com/documentation/services/documentdb/) usługi.




