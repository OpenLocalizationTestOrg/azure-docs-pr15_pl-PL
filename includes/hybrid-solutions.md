#<a name="azure-service-bus"></a>Usługa Azure Bus

Czy aplikacja lub usługa działa w chmurze lub lokalnie, często potrzebuje do współdziałania z innych aplikacji lub usług. Aby zapewnić ogólnie przydatny sposób, aby to zrobić, Azure oferuje Bus usługi. W tym artykule trwa przyjrzeć się tej technologii opisujący, czym jest i dlaczego warto z niej korzystać.

## <a name="service-bus-fundamentals"></a>Podstawy Bus usługi
Różne style komunikacji wymagają różnych sytuacjach. Czasami co aplikacje wysyłać i odbierać wiadomości przy użyciu prostej kolejki jest najlepszym rozwiązaniem. W innych sytuacjach zwykłej kolejce mało; lepiej jest kolejki za pomocą publikowania i subskrybowania. I w niektórych przypadkach wszystkie naprawdę potrzebne jest połączenie między aplikacje & #151; kolejek nie są wymagane. Usługa Bus zawiera wszystkie trzy opcje co aplikacji jest interakcja kilka różnych sposobów.

Usługa Bus jest usługi w chmurze wielu dzierżawy, co oznacza, że usługa jest udostępniony przez wielu użytkowników. Każdego użytkownika, takie jak Twórca aplikacji tworzy *przestrzeń nazw*, a następnie określa mechanizmy komunikacji, których potrzebuje w tym obszarze nazw. [Rysunek 1](#Fig1) pokazuje, jak wygląda.

<a name="Fig1"></a>![Diagram przedstawiający Bus usługi Azure][svc-bus]
 
**Rysunek 1: Bus Usługa udostępnia usługę wiele dzierżawy nawiązywania połączenia aplikacji do chmury.**

W obszarze nazw możesz użyć jednego lub więcej wystąpień cztery mechanizmy różnych komunikacji, z których każdy będzie łączy aplikacji w inny sposób. Dostępne są następujące:

- *Kolejki*, umożliwiające komunikacja dwukierunkowa z nich. Każda kolejka działa jako pośrednik (nazywane *broker*) przechowującego wysłanej wiadomości, dopóki nie zostały odebrane. Każda wiadomość jest odbierane przez jednego adresata.
- *Tematy*, które zawierają jedną dwukierunkowa komunikacja za pomocą *subskrypcji*— jeden temat może zawierać wiele subskrypcji. Jak kolejki tematu działa jako broker, ale każdej subskrypcji można opcjonalnie przy użyciu filtru odbierać tylko wiadomości spełniających określone kryteria.
- *Przekaźniki*, które zapewniają komunikacja dwukierunkowa. W przeciwieństwie do kolejek i tematy przekazywania nie zawierają lotu wiadomości it firmy nie broker. Należy po prostu przekazuje je do aplikacji docelowej.
- *Koncentratory zdarzeń*, które dostarczają zdarzeń i telemetrycznego wchodzącego w chmurze na ogromną skalę małe opóźnienia i wysoka niezawodność.

Podczas tworzenia kolejki, temat, przekazywania lub Centrum zdarzenia, możesz nadać mu nazwę. Ta nazwa używana w połączeniu z niezależnie od o nazwie obszaru nazw, tworzy unikatowy identyfikator obiektu. Aplikacje można umożliwiają Bus usług tę nazwę, a następnie komunikowanie się ze sobą za pomocą tej kolejki, temat, przekazywania lub Centrum zdarzenia. 

Aby użyć dowolnego z tych obiektów, aplikacje systemu Windows mogą używać usługi Windows Communication Foundation (WCF). Kolejki, tematy i zdarzeń koncentratory Windows aplikacje mogą używać zdefiniowane Bus usługi SMS interfejsów API. Aby ułatwić korzystanie z aplikacji — Windows tych obiektów, firma Microsoft udostępnia SDK dla języka Java, Node.js i innych języków. Można także przejść, kolejki, tematów i koncentratory zdarzeń za pomocą interfejsów API pozostałych za pomocą protokołu HTTP. 

Należy pamiętać, że mimo że usługa Bus samej zostanie uruchomiona w chmurze (oznacza to, że w Azure centrach danych firmy Microsoft), aplikacje, które używają go można uruchamiać dowolnego miejsca. Usługa Bus umożliwia łączenie aplikacje na Azure, na przykład lub aplikacje wewnątrz własne centrum danych. Umożliwia także go nawiązać uruchamiania aplikacji na Azure lub innego platformy chmury z aplikacją lokalnego lub tabletów i telefonów. Użytkownik może nawet domowego, czujniki i innych urządzeń połączyć się z aplikacją centralnej lub jeden z pozostałych. Usługa Bus jest mechanizm ogólnego komunikacji w chmurze, która jest dostępna z dużym stopniu dowolnego miejsca. W jaki sposób możesz użyć zależy czynności należy wykonać aplikacji.


## <a name="queues"></a>Kolejki

Załóżmy, że użytkownik chce połączyć dwie aplikacje przy użyciu kolejki Bus usługi. [Rysunek 2](#Fig2) przedstawia takiej sytuacji.

<a name="Fig2"></a>![Diagram kolejek Bus usług][queues]
 
**Rysunek 2: Usługi Bus kolejek zapewniają jednokierunkowe Kolejkowanie asynchroniczne.**

Ten proces jest proste: nadawcy wysyła wiadomość do kolejki Bus usługi, a adresatem przejmuje wiadomości niektórych później. Kolejki może zawierać tylko jednego odbiorcy, jak przedstawiono w [Rysunek 2](#Fig2) lub wielu aplikacje mogą odczytywać z tej samej kolejki. W tej ostatniej sytuacji każda wiadomość jest odczytywane przez tylko jeden odbiorca-wielokrotnego lanego usługi należy używać tematu w zamian.

Każda wiadomość ma dwie części: zbiór właściwości każdej pary klucz wartość, a treść wiadomości binarne. Sposób ich używania zależy od tego, co aplikacja próbuje wykonać. Na przykład aplikacja wysłaniem wiadomości o ostatnie sprzedaży mogą zawierać właściwości *Sprzedawca = "Ava"* i *Kwota = 10000*. Treść wiadomości może być zawierają zeskanowany obraz podpisane umowy sprzedaży lub, jeśli nie istnieje, po prostu pozostaną puste.

Słuchawka można odczytać wiadomość z kolejki Bus usługi na dwa różne sposoby. Pierwszą opcję o nazwie *ReceiveAndDelete*usuwa wiadomość z kolejki i natychmiast usunąć ją. To jest proste, ale jeśli odbiorca ulega awarii, przed zakończeniem przetwarzania wiadomości, wiadomości zostaną utracone. Ponieważ został on usunięty z kolejki, nie innych odbiorcy do niego dostęp. 

Jest to druga opcja, *PeekLock*jest przeznaczona do pomocy z tym problemem. Jak ReceiveAndDelete przeczytaj PeekLock usuwa wiadomość z kolejki. Jednak nie usuwa wiadomości. Zamiast tego blokuje wiadomości, dzięki czemu niewidoczne do innych odbiorców, następnie czeka na jeden z trzech zdarzeń:

- Jeśli odbiorca przetwarza komunikat pomyślnie, wywołuje *Wykonano*i kolejki usuwa wiadomość. 
- Jeśli odbiorca zdecyduje, że wiadomość nie może przetworzyć pomyślnie, połączeń *zrezygnować*. Kolejka następnie usuwa kłódka z wiadomości i umożliwia dostęp do innych odbiorców.
- Jeśli odbiorca wymaga oba te w terminie można konfigurować (domyślnie 60 sekund), kolejki założono, że adresat nie powiodło się. W tym przypadku działa, jak gdyby adresat miał o nazwie Abandon udostępnianie wiadomości do innych odbiorców.

Zwróć uwagę, co może się zdarzyć, tutaj: tego samego komunikatu mogą się dostarczyć dwa razy, być może na dwóch różnych odbiorców. To należy przygotować aplikacji przy użyciu usługi Bus kolejek. Aby ułatwić wykrywania duplikatów, w każdej wiadomości ma unikatowy identyfikator komunikatu właściwość domyślnie pozostaje taki sam, niezależnie od tego, jak często wiadomości jest odczytu z kolejki. 

Kolejki są przydatne w sytuacjach jest kilka. Poinformuj ich aplikacji komunikacji, nawet gdy oba nie jest uruchomiony w tym samym czasie, coś, co jest szczególnie przydatne z partii i aplikacji dla urządzeń przenośnych. Kolejka o wielu odbiorców także równoważenia obciążenia automatycznych, ponieważ wysłane wiadomości są rozciągnąć tych odbiorców.


## <a name="topics"></a>Tematy

Przydatne są kolejki nie są zawsze właściwe rozwiązanie. Czasami tematy Bus usługi ma lepsze wyniki. [Rysunek 3](#Fig3) przedstawia ten ogólny obraz.

<a name="Fig3"></a>![Diagram tematy Bus usługi i subskrypcje][topics-subs]
 
**Rysunek 3: W zależności od subskrypcji aplikacja określa filtr, może odbierać niektóre lub wszystkie wiadomości wysłane do tematu Bus usługi.**

Temat jest podobna umożliwiają kolejki na różne sposoby. Nadawców przesyłania wiadomości do tematu w taki sam sposób, lecz wysyłać wiadomości do kolejki i tych wiadomości wyglądają tak samo jak w przypadku kolejek. Duży różnica polega na tym, że tematy pozwalają każdego aplikację utworzyć własny subskrypcji, definiując *Filtr*. Subskrybentów następnie zostanie wyświetlony tylko wiadomości, które są zgodne z tego filtru. Na przykład [Rysunek 3](#Fig3) przedstawia nadawcy i tematu z trzech subskrybentów, każda z własną filtru:

- Subskrybenta 1 otrzymuje tylko wiadomości, które zawierają właściwość *Sprzedawca = "Ava"*.
- Subskrybenta 2 odbiera wiadomości, które zawierają właściwość *Sprzedawca = "Dopiskiem"* i/lub zawiera właściwości *Kwota* , której wartość jest większa niż 100 000. Być może dopiskiem jest Menedżer sprzedaży, a więc chce wyświetlić własne sprzedaży i wszystkie ogólnej sales niezależnie od tego, który sprawia, że.
- Subskrybentów 3 został ustawiony filtru na *wartość PRAWDA*, co oznacza, że otrzyma wszystkich wiadomości. Na przykład ta aplikacja może być odpowiedzialnych za utrzymanie dziennik inspekcji i w związku z tym należy wyświetlić wszystkie wiadomości.

Podobnie jak w przypadku kolejki, subskrybentów tematu może czytać wiadomości przy użyciu ReceiveAndDelete lub PeekLock. W przeciwieństwie do kolejki jednak pojedynczej wiadomości wysyłane do tematu może zostać odebrana przez wielu subskrybentów. Tej metody, nazywanego dodatkiem *Opublikuj oraz subskrybowanie*jest przydatne, gdy wiele aplikacji może być zainteresowanych w tej wiadomości. Definiując filtr prawym każdego subskrybenta można nacisnąć, do części strumienia wiadomości, którą należy go wyświetlić.


## <a name="relays"></a>Przekaźniki

Zarówno kolejek i tematy stanowią asynchroniczne komunikacji jednokierunkowej za pośrednictwem brokera. Ruch będzie przepływał tylko w jednym kierunku, a istnieje bezpośrednie połączenie między nadawcy i odbiorcy. Ale co zrobić, jeśli nie chcesz to? Załóżmy, że aplikacje muszą zarówno wysyłać i odbierać wiadomości, lub prawdopodobnie ma bezpośrednie łącze między nimi i nie trzeba brokera do przechowywania wiadomości. Adres scenariuszy, takich jak Bus usługi zawiera przekaźniki, jak pokazano na [ilustracji 4](#Fig4) .

<a name="Fig4"></a>![Diagram przedstawiający przekazywania Bus usługi][relay]
 
**Rysunek 4: Usługi Bus przekazywania zawiera obraz, dwukierunkowe komunikacji między aplikacjami.**

To jest widocznych pytanie, aby poprosić o przekaźniki: do czego służy jedną? Nawet jeśli kolejki nie jest konieczne, dlaczego wprowadzić aplikacji komunikowanie się za pośrednictwem usługi w chmurze, a nie tylko korzystać bezpośrednio? Odpowiedź to, że rozmawiać bezpośrednio może być trudniejsze niż się wydaje.

Załóżmy, że chcesz połączyć dwie aplikacje lokalnego, zarówno z wewnątrz firmy centrach danych. Każdy z nich znajduje się za zaporą, a każdego centrum danych prawdopodobnie używa translacji adresów sieciowych (NAT). Zapora blokuje przychodzące dane na wszystkich, ale kilka portów i translatora adresów Sieciowych oznacza, że w komputerze, na którym pracuje każdej aplikacji nie ma stały adres IP, który może osiągnąć bezpośrednio z poza centrum danych. Bez niektórych dodatkową pomoc dotyczącą nawiązywanie połączenia te aplikacje za pośrednictwem publicznego Internetu jest problemy.

Przekazywania Bus usługi zawiera Pomoc. Na komunikowanie się bi techniką przez przekazywania, każdą z nich ustala wychodzące połączenie TCP z Bus usługi, a następnie zachowany otwarte. Wszystkie komunikacji między obiema aplikacjami są przenoszone przez te połączenia. Ponieważ każdego połączenia zostało ustanowione z wewnątrz centrum danych, zapory umożliwi ruchu przychodzącego dla każdej aplikacji bez otwierania nowe porty. Tej metody staje się również rozwiązać problem translatora adresów Sieciowych, ponieważ każda aplikacja ma spójne punkt końcowy w chmurze w całej komunikacji. Wymiany danych do przekazywania, aplikacje można uniknąć problemów, które w przeciwnym razie spowodowałby komunikacji trudne. 

Aby użyć przekaźniki Bus usługi, aplikacje oparte na systemie Windows Communication Foundation (WCF). Usługa Bus zawiera powiązań WCF, ułatwiające proste dla aplikacji systemu Windows interakcyjnie używać za pośrednictwem przekaźniki. Aplikacje, które już korzystać WCF może zazwyczaj tylko określ jedną z tych powiązań, a następnie do siebie za pośrednictwem przekaźnika. W przeciwieństwie do kolejek i tematów jednak przekaźniki z aplikacji — Windows, podczas możliwości korzystania z wysiłku programowania; Brak standardowy bibliotek są dostarczane.

W przeciwieństwie do kolejek i tematy aplikacji nie jest jawnie tworzona przekaźniki. Zamiast tego gdy aplikacja, z której chce otrzymywać wiadomości nawiąże połączenie TCP z Bus usługi, przekazywania zostanie utworzona automatycznie. Po przerwaniu połączenia przekazywania zostanie usunięty. Aby umożliwić aplikacji znaleźć przekazywania utworzone przez nasłuch, Bus Usługa udostępnia rejestru, która umożliwia aplikacji zlokalizować określonego przekazywania według nazwy.

Przekaźniki są właściwe rozwiązanie, gdy potrzebne bezpośredniej komunikacji między aplikacjami. Na przykład można rozważyć linii lotniczych rezerwacji systemu w centrum danych lokalnego, które muszą być dostępne z kioski ewidencjonowania i urządzeń przenośnych oraz innych komputerów. Aplikacje we wszystkich tych systemów może zależeć przekaźniki Bus usługa w chmurze, aby komunikować się, w miejsce, w którym może działać.

## <a name="event-hubs"></a>Koncentratory zdarzenia

Koncentratory zdarzenie jest wysoce skalowalna spożyciu systemu, w którym można przetwarzać miliony wydarzeń na sekundę, włączanie aplikacji w celu przetwarzania i analizowanie dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Za pomocą Centrum zdarzeń można na przykład zbierać dane dotyczące wydajności aparatu na żywo z floty samochodów. Po zbierane do koncentratorów zdarzenia, możesz Przekształcanie i przechowywanie danych przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym lub klaster miejsca do magazynowania. Aby uzyskać więcej informacji na temat koncentratory zdarzeń zobacz [Omówienie koncentratory zdarzenia][].

## <a name="summary"></a>Podsumowanie

Łączenie aplikacji zawsze było część tworzenie rozwiązań ukończone i zakres scenariuszy, które wymagają aplikacji i usług w celu komunikowania się między sobą jest ustawiona na zwiększenie nowe aplikacje i urządzenia są połączone z Internetem. Udostępniając rozwiązań opartych na chmurze realizacji tego za pośrednictwem kolejek, tematy przekaźniki i koncentratory zdarzenia, Bus usługi ma na celu dzięki to podstawowa funkcja jest łatwiejsza do wykonania i szerokiego wglądu.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Omówienie koncentratory zdarzenia]: https://msdn.microsoft.com/library/azure/dn836025.aspx
