<properties 
    pageTitle="Usługa Azure Bus | Microsoft Azure" 
    description="Wprowadzenie do korzystania z usługi Bus nawiązać Azure aplikacji do innych programów." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Usługa Azure Bus

Czy aplikacja lub usługa działa w chmurze lub lokalnie, często potrzebuje do współdziałania z innych aplikacji lub usług. Aby zapewnić ogólnie przydatny sposób, aby to zrobić, Microsoft Azure oferuje Bus usługi. W tym artykule trwa przyjrzeć się tej technologii opisujący, czym jest i dlaczego warto z niej korzystać.

## <a name="service-bus-fundamentals"></a>Podstawy Bus usługi

Różne style komunikacji wymagają różnych sytuacjach. Czasami co aplikacje wysyłać i odbierać wiadomości przy użyciu prostej kolejki jest najlepszym rozwiązaniem. W innych sytuacjach zwykłej kolejce mało; lepiej jest kolejki za pomocą publikowania i subskrybowania. W niektórych przypadkach wszystkie naprawdę potrzebne jest połączenie między aplikacjami; kolejek nie są wymagane. Bus usługi zawiera wszystkie trzy opcje Włączanie aplikacji interakcyjnie używać na różne sposoby.

Usługa Bus jest usługi w chmurze wielu dzierżawy, co oznacza, że usługa jest udostępniony przez wielu użytkowników. Każdego użytkownika, takie jak Twórca aplikacji tworzy *przestrzeń nazw*, a następnie określa mechanizmy komunikacji, których potrzebuje w tym obszarze nazw. Rysunek 1 pokazuje, jak wygląda.

![][1]
 
**Rysunek 1: Bus usługi udostępnia obsługę wielu dzierżawy nawiązywania połączenia aplikacji przez w chmurze.**

W obszarze nazw możesz użyć jednego lub więcej wystąpień cztery mechanizmy różnych komunikacji, z których każdy będzie łączy aplikacji w inny sposób. Dostępne są następujące:

- *Kolejki*, umożliwiające komunikacja dwukierunkowa z nich. Każda kolejka działa jako pośrednik (nazywane *broker*) przechowującego wysłanej wiadomości, dopóki nie zostały odebrane. Każda wiadomość jest odbierane przez jednego adresata.
- *Tematy*, które zawierają jedną dwukierunkowa komunikacja za pomocą *subskrypcji*— jeden temat może zawierać wiele subskrypcji. Jak kolejki tematu działa jako broker, ale każdej subskrypcji można opcjonalnie przy użyciu filtru odbierać tylko wiadomości spełniających określone kryteria.
- *Przekaźniki*, które zapewniają komunikacja dwukierunkowa. W przeciwieństwie do kolejek i tematy przekazywania nie przechowuj lotu wiadomości; nie jest broker. Zamiast tego po prostu przekazuje je do aplikacji docelowej.

Podczas tworzenia kolejki, tematu lub przekazywania, możesz nadać mu nazwę. Ta nazwa używana w połączeniu z niezależnie od o nazwie obszaru nazw, tworzy unikatowy identyfikator obiektu. Aplikacje można umożliwiają Bus usługi tej nazwy, a następnie za pomocą tej kolejki, tematu lub przekazywania na komunikowanie się ze sobą. 

Aby użyć dowolnego z tych obiektów w scenariuszu przekazywania, aplikacje systemu Windows mogą używać usługi Windows Communication Foundation (WCF). Dla kolejki i tematy aplikacje systemu Windows mogą używać zdefiniowane Bus usługi SMS interfejsów API. Aby ułatwić korzystanie z aplikacji — Windows tych obiektów, firma Microsoft udostępnia SDK dla języka Java, Node.js i innych języków. Można także przejść kolejki i tematów za pomocą interfejsów API umieść nad HTTP (s). 

Należy pamiętać, że mimo że usługa Bus samej zostanie uruchomiona w chmurze (oznacza to, że w Azure centrach danych firmy Microsoft), aplikacje, które używają go można uruchamiać dowolnego miejsca. Usługa Bus umożliwia łączenie aplikacje na Azure, na przykład lub aplikacje wewnątrz własnego centrum danych. Umożliwia także go nawiązać uruchamiania aplikacji na Azure lub innego platformy chmury z aplikacją lokalnego lub tabletów i telefonów. Użytkownik może nawet domowego, czujniki i innych urządzeń połączyć się z aplikacją centralnej lub jeden z pozostałych. Usługa Bus jest mechanizm przekazywania informacji w chmurze, która jest dostępna z dużym stopniu dowolnego miejsca. W jaki sposób możesz użyć zależy czynności należy wykonać aplikacji.

## <a name="queues"></a>Kolejki

Załóżmy, że użytkownik chce połączyć dwie aplikacje przy użyciu kolejki Bus usługi. Rysunek 2 przedstawia takiej sytuacji.

![][2]
 
**Rysunek 2: Usługi Bus kolejek zapewniają jednokierunkowe Kolejkowanie asynchroniczne.**

Ten proces jest proste: nadawcy wysyła wiadomość do kolejki Bus usług i adresatem wybiera wiadomości niektórych później. Kolejka może mieć tylko jednego odbiorcy, jak pokazano na ilustracji 2. Można także czytać wiele aplikacji z tej samej kolejki. W tej ostatniej sytuacji każda wiadomość jest odczytywane przez tylko jeden odbiorca. Wiele lanego usługi należy użyć zamiast tego tematu.

Każda wiadomość ma dwie części: zbiór właściwości każdej pary klucz wartość, a treść wiadomości binarne. Sposób ich używania zależy od tego, co aplikacja próbuje wykonać. Na przykład aplikacja wysłaniem wiadomości o ostatnie sprzedaży mogą zawierać właściwości *Sprzedawca = "Ava"* i *Kwota = 10000*. Treść wiadomości może być zawierają zeskanowany obraz podpisane umowy sprzedaży lub, jeśli nie istnieje, po prostu pozostaną puste.

Słuchawka można odczytać wiadomość z kolejki Bus usługi na dwa różne sposoby. Pierwszą opcję o nazwie *ReceiveAndDelete*usuwa wiadomość z kolejki i natychmiast usunąć ją. To jest proste, ale jeśli odbiorca ulega awarii, przed zakończeniem przetwarzania wiadomości, wiadomości zostaną utracone. Ponieważ został on usunięty z kolejki, nie innych odbiorcy do niego dostęp. 

Jest to druga opcja, *PeekLock*jest przeznaczona do pomocy z tym problemem. Przykład **ReceiveAndDelete**odczytu **PeekLock** usuwa wiadomość z kolejki. Jednak nie usuwa wiadomości. Zamiast tego go blokuje wiadomości, dzięki czemu niewidoczne do innych odbiorców, a następnie czeka na jeden z trzech zdarzeń:

- Jeśli odbiorca przetwarza komunikat pomyślnie, wywołuje **Wykonano**i kolejki usuwa wiadomość. 
- Jeśli odbiorca zdecyduje, że wiadomość nie może przetworzyć pomyślnie, połączeń **zrezygnować**. Kolejka następnie usuwa kłódka z wiadomości i umożliwia dostęp do innych odbiorców.
- Jeśli odbiorca wymaga oba te w terminie można konfigurować (domyślnie 60 sekund), kolejki założono, że adresat nie powiodło się. W tym przypadku działa, jak gdyby adresat miał o nazwie **zrezygnować**, udostępnianie wiadomości do innych odbiorców.

Zwróć uwagę, co może się zdarzyć, tutaj: tego samego komunikatu mogą się dostarczyć dwa razy, być może na dwóch różnych odbiorców. To należy przygotować aplikacji przy użyciu usługi Bus kolejek. Aby ułatwić wykrywania duplikatów, w każdej wiadomości ma unikatowy **Identyfikator komunikatu** właściwość domyślnie pozostaje taki sam, niezależnie od tego, jak często wiadomości jest odczytu z kolejki. 

Kolejki są przydatne w sytuacjach jest kilka. Umożliwiają aplikacjom komunikować się nawet po obu nie jest uruchomiony w tym samym czasie, coś, co jest szczególnie przydatne z partii i aplikacji dla urządzeń przenośnych. Kolejka o wielu odbiorców także równoważenia obciążenia automatycznych, ponieważ wysłane wiadomości są rozciągnąć tych odbiorców.

## <a name="topics"></a>Tematy

Przydatne są kolejki nie są zawsze właściwe rozwiązanie. Czasami tematy Bus usługi ma lepsze wyniki. Rysunek 3 przedstawia ten ogólny obraz.

![][3]
 
**Rysunek 3: W zależności od subskrypcji aplikacja określa filtr, może odbierać niektóre lub wszystkie wiadomości wysłane na temat usługi Bus.**

*Temat* jest podobna umożliwiają kolejki na różne sposoby. Nadawców przesyłanie wiadomości na temat w taki sam sposób, lecz wysyłać wiadomości do kolejki i tych wiadomości wyglądają tak samo jak w przypadku kolejek. Duży różnica polega na tym, że tematy umożliwiają każdej aplikacji odbierającej utworzyć własny *subskrypcji* , definiując *Filtr*. Subskrybentów następnie zostanie wyświetlony tylko wiadomości, które są zgodne z tego filtru. Na przykład rysunek 3 przedstawia nadawcy i tematu z trzech subskrybentów, każda z własną filtru:

- Subskrybenta 1 otrzymuje tylko wiadomości, które zawierają właściwość *Sprzedawca = "Ava"*.
- Subskrybentów 2 odbiera wiadomości, które zawierają właściwość *Sprzedawca = "Dopiskiem"* i/lub zawiera właściwości *Kwota* , której wartość jest większa niż 100 000. Być może dopiskiem jest kierownik, więc chce wyświetlić własne sprzedaży i wszystkie ogólnej sales niezależnie od tego, który sprawia, że.
- Subskrybentów 3 został ustawiony filtru na *wartość PRAWDA*, co oznacza, że otrzyma wszystkich wiadomości. Na przykład ta aplikacja może być odpowiedzialnych za utrzymanie dziennik inspekcji i w związku z tym należy wyświetlić wszystkie wiadomości.

Podobnie jak w przypadku kolejki, subskrybentów tematu może czytać wiadomości przy użyciu **ReceiveAndDelete** lub **PeekLock**. W przeciwieństwie do kolejki jednak pojedynczej wiadomości wysyłane do tematu może zostać odebrana przez wiele subskrypcji. Tej metody, często nazywanych *Opublikuj oraz subskrybowanie* (lub *pub-sub*), jest przydatne, gdy wiele aplikacji interesuje, w tym samym wiadomości. Definiując filtr prawym każdego subskrybenta można nacisnąć, do części strumienia wiadomości, którą należy go wyświetlić.

## <a name="relays"></a>Przekaźniki

Zarówno kolejek i tematy stanowią asynchroniczne komunikacji jednokierunkowej za pośrednictwem brokera. Ruch będzie przepływał tylko w jednym kierunku, a istnieje bezpośrednie połączenie między nadawcy i odbiorcy. Ale co zrobić, jeśli nie chcesz to? Załóżmy, że aplikacje muszą zarówno wysyłać i odbierać wiadomości, lub prawdopodobnie ma bezpośrednie łącze między nimi i nie trzeba brokera do przechowywania wiadomości. Adres scenariuszy, takich jak Bus Usługa udostępnia *przekazuje*, jak pokazano na ilustracji 4.

![][4]
 
**Rysunek 4: Usługi Bus przekazywania zawiera obraz, dwukierunkowe komunikacji między aplikacjami.**

To jest widocznych pytanie, aby poprosić o przekaźniki: do czego służy jedną? Nawet jeśli kolejki nie jest konieczne, dlaczego wprowadzić aplikacji komunikowanie się za pośrednictwem usługi w chmurze, a nie tylko korzystać bezpośrednio? Odpowiedź to, że rozmawiać bezpośrednio może być trudniejsze niż się wydaje.

Załóżmy, że chcesz połączyć dwie aplikacje lokalnego, zarówno z wewnątrz firmy centrach danych. Każdy z nich znajduje się za zaporą i każdego centrum danych prawdopodobnie używa translacji adresów sieciowych (NAT). Zapora blokuje przychodzące dane na wszystkich, ale kilka portów i translatora adresów Sieciowych oznacza, że w komputerze, na którym pracuje każdej aplikacji nie ma stały adres IP, który może osiągnąć bezpośrednio z poza centrum danych. Bez niektórych dodatkową pomoc dotyczącą nawiązywanie połączenia te aplikacje za pośrednictwem publicznego Internetu jest problemy.

Może pomóc w przekazywania Bus usługi. Na komunikowanie się bi techniką przez przekazywania, każdą z nich ustala wychodzące połączenie TCP z Bus usługi, a następnie zachowany otwarte. Wszystkie komunikacji między obiema aplikacjami przesyłany te połączenia. Ponieważ każdego połączenia zostało ustanowione z wewnątrz centrum danych, zapora zezwala na ruch przychodzący dla każdej aplikacji bez otwierania nowe porty. Tej metody staje się również rozwiązać problem translatora adresów Sieciowych, ponieważ każda aplikacja ma spójne punkt końcowy w chmurze w całej komunikacji. Wymiany danych do przekazywania, aplikacje można uniknąć problemów, które w przeciwnym razie spowodowałby komunikacji trudne. 

Aby użyć przekaźniki Bus usług, aplikacje oparte na systemie Windows Communication Foundation (WCF). Usługa Bus zawiera powiązań WCF, ułatwiające proste dla aplikacji systemu Windows interakcyjnie używać za pośrednictwem przekaźniki. Aplikacje, które już korzystać WCF może zazwyczaj tylko określ jedną z tych powiązań, a następnie ze sobą za pośrednictwem przekaźnika. W przeciwieństwie do kolejek i tematów jednak przekaźniki z aplikacji — Windows, podczas możliwości korzystania z wysiłku programowania; Brak bibliotek standardowe są dostarczane.

W przeciwieństwie do kolejek i tematy aplikacji nie jest jawnie tworzona przekaźniki. Zamiast tego gdy aplikacja, z której chce otrzymywać wiadomości nawiąże połączenie TCP z usługi Bus, przekazywania zostanie utworzona automatycznie. Po przerwaniu połączenia przekazywania zostanie usunięty. Aby włączyć aplikację znaleźć przekazywania utworzone przez nasłuch, Bus Usługa udostępnia rejestru, która umożliwia aplikacji zlokalizować określonego przekazywania według nazwy.

Przekaźniki są właściwe rozwiązanie, gdy potrzebne bezpośredniej komunikacji między aplikacjami. Na przykład można rozważyć linii lotniczych rezerwacji systemu w centrum danych lokalnego, które muszą być dostępne z kioski ewidencjonowania i urządzeń przenośnych oraz innych komputerów. Aplikacje we wszystkich tych systemów może zależeć przekaźniki Bus usługa w chmurze, aby komunikować się, w miejsce, w którym może działać.

## <a name="summary"></a>Podsumowanie

Łączenie aplikacji zawsze było część tworzenie rozwiązań ukończone i zakres scenariuszy, które wymagają aplikacji i usług w celu komunikowania się między sobą jest ustawiona na zwiększenie nowe aplikacje i urządzenia są połączone z Internetem. Udostępniając rozwiązań opartych na chmurze realizacji tego za pośrednictwem kolejkach, tematy i przekaźniki, usługa Bus ma na celu dzięki to podstawowa funkcja jest łatwiejsza do wykonania i szerokiego wglądu.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy Bus usługa Azure, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- Jak używać [usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)
- Jak używać [usługi Bus tematów](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Jak używać [usługi Bus przekazywania](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Przykłady Bus usługi](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
