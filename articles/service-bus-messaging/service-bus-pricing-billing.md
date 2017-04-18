<properties 
    pageTitle="Usługa Bus ceny i rozliczenia | Microsoft Azure"
    description="Omówienie usługi Bus ceny struktury."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Usługa Bus ceny i rozliczenia

Usługa Bus jest oferowane w Basic, Standard i [Premium](service-bus-premium-messaging.md) warstwy. Możesz wybrać warstwa usług dla każdego obszaru nazw usługi Bus usług, które są tworzone i wybranie tej opcji Warstwa zastosowanie przez wszystkie obiekty utworzone w tym obszarze nazw.

>[AZURE.NOTE] Aby uzyskać szczegółowe informacje dotyczące bieżącego ceny Bus usługi zobacz [Bus usługi Azure ceny strony](https://azure.microsoft.com/pricing/details/service-bus/)i [Bus usługi — często zadawane pytania](service-bus-faq.md#service-bus-pricing).

Bus usługi jest używane następujące dwa metr kolejek i tematy i subskrypcje:

1. **Operacje wiadomości**: zdefiniowany interfejsu API przed kolejki lub tematu subskrypcji usługi punktów końcowych. Ten licznik zastąpi wiadomości wysyłane lub odbierane jako jednostkę podstawową fakturowanego wykorzystania kolejek i tematy i subskrypcji.

2. **Brokered połączeń**: definiowana jako Alokacja maksymalna liczba połączeń trwałych Otwórz przed kolejek, tematy lub subskrypcje w okresie próbki danej godziny. Ten licznik ma zastosowanie tylko w standardowej warstwie, w którym można otworzyć dodatkowe połączenia (wcześniej połączenia zostały ograniczone do 100 na kolejki tematu i subskrypcji) za opłatą połączenia nominalna.

**Standardowa** warstwa wprowadza stopniowe cennik dla operacji wykonanych przy użyciu kolejki i tematy i subskrypcje, uzyskując oparte na głośność rabatów maksymalnie 80% najwyższym poziomie zastosowania. Istnieje także opłatę podstawowej standardowe poziomu 10 zł za miesiąc, który umożliwia wykonywanie operacji do 12,5 miliona miesięcznie bez dodatkowych opłat.

Poziom **Premium** zapewnia izolacji zasobu w warstwie Procesora i pamięci obciążenie pracą każdego klienta jest uruchamiany oddzielnie. Ten kontener zasobu jest określana mianem *wiadomości jednostek*. Przestrzeń nazw poszczególnych premium przydzielonego co najmniej jedną jednostkę wiadomości. Możesz kupić 1, 2 lub 4 jednostek wiadomości dla każdego obszaru nazw usługi Bus Premium. Pojedynczy obciążenie pracą lub podmiotów może obejmować wiele jednostek wiadomości i liczba jednostek wiadomości można zmienić w będzie, mimo że rozliczeń znajduje się w 24-godzinnego lub codziennie stawka kosztów. Wynik to przewidywalne i powtarzalnych wydajności dotyczących rozwiązania oparte na Bus usługi. Nie tylko jest to wydajności, bardziej przewidywalne i dostępne, ale jest również szybciej. Wiadomości Premium Bus usługi Azure opiera się na aparat magazynu wprowadzone w koncentratory zdarzenia Azure. Dzięki wiadomościom Premium, szybciej niż standardowy warstwa jest najwyższą wydajność.

Zauważ, że standardowe opłaty podstawowej jest naliczany tylko raz na miesiąc na subskrypcję Azure. Oznacza to, że po utworzeniu jeden Standard lub Premium warstwa Bus usługi nazw będzie mógł tworzyć możliwie jak najwięcej dodatkowe Standard lub Premium warstwa przestrzenie nazw zgodnie z oczekiwaniami w tej samej subskrypcji Azure bez ponoszenia dodatkowych opłat podstawowej.

Wszystkie istniejące usługi Bus przestrzenie nazw utworzone przed 1 listopada 2014 r zostały automatycznie umieszczane w warstwie standardowy. Dzięki temu będzie mieć dostęp do wszystkich funkcji dostępnych z Bus usługi. Następnie [portal Azure klasyczny][] umożliwia konwertowanie na warstwie podstawowe, w razie potrzeby.

W poniższej tabeli podsumowano różnice funkcjonalne między poziomów Basic i standardowe-Premium.

|Funkcja|Podstawowe|Standardowe i Premium|
|---|---|---|
|Kolejki|Tak|Tak|
|Zaplanowane wiadomości|Tak|Tak|
|Tematy i subskrypcji|Brak|Tak|
|Przekaźniki|Brak|Tak|
|Transakcje|Brak|Tak|
|Duplikowanie do|Brak|Tak|
|Sesje|Brak|Tak|
|Duże wiadomości|Brak|Tak|
|ForwardTo|Brak|Tak|
|SendVia|Brak|Tak|
|Brokered połączenia (w pakiecie)|100 na Bus usługi nazw|1000 na Azure subskrypcji|
|Brokered połączenia (nadmiar dozwolone)|Brak|Tak (rozliczana)|

## <a name="messaging-operations"></a>Operacje wiadomości

W ramach nowego modelu cennik zmienia się rozliczeniami dla kolejki i tematy i subskrypcji. Tych obiektów jesteś w trakcie przejmowania z rozliczeń wiadomości rozliczenia dla operacji. "Operacja" odwołuje się do dowolnego połączenia interfejsu API stosunku kolejki lub tematu subskrypcji punktu końcowego usługi. Obejmuje to operacje stan zarządzania, wysyłania/odbierania i sesji.

|Typ operacji|Opis|
|---|---|
|Zarządzanie|Tworzenie, Odczyt, aktualizacja, usuwanie (OBSŁUGIWAŁ) przed kolejki lub tematy i subskrypcji.|
|Wiadomości|Wysyłanie i odbieranie wiadomości z kolejki lub tematy i subskrypcji.|
|Stan sesji|Wprowadzenie lub ustawienie stanu sesji w kolejce lub tematu i subskrypcji.|

Następujące ceny były efektywnej Rozpoczynanie 1 listopada 2014 r.:

|Podstawowe|Koszt|
|---|---|
|Operacje|0,05 $ na milionów operacji|

|Standardowe|Koszt|
|---|---|
|Podstawa opłat|10 zł/miesiąca|
|Najpierw 12.5 milionów operacji/miesiąca|Dostępny|
|12.5-100 milionów operacji/miesiąca|0,80 $ na milionów operacji|
|100 milionów - 2500 milionów operacji/miesiąca|0,50 $ na milionów operacji|
|Ponad 2500 milionów operacji/miesiąca|0,20 zł na milionów operacji|

|Premium|Koszt|
|---|---|
|Dzienny|$11.13 stałej stopy na jednostkę wiadomości|

## <a name="brokered-connections"></a>Brokered połączenia

*Połączenia Brokered* zezwalały upodobania klienta, które wymagają dużej liczby "trwale połączenia" nadawcy i odbiorcy przed kolejek, tematy lub subskrypcji. Trwale połączonego nadawcy i odbiorcy są te, które nawiązać połączenie za pomocą AMQP lub HTTP z wartością zero odbieranie przekroczenia limitu czasu (na przykład HTTP długie sondowanie). HTTP nadawcy i odbiorcy natychmiastowej limitu czasu nie generują brokered połączenia.

Tematy i subskrypcji i kolejek wcześniej limit 100 równoczesne połączenia na adres URL. Bieżący schemat rozliczeń usuwa limit na adres URL dla kolejki i tematy i subskrypcje i wykonuje przydziałami i pomiaru połączeniach brokered na poziomie subskrypcji Azure i Bus usługi nazw.

Podstawowe warstwa zawiera i jest ograniczone do 100 brokered połączeń na Bus usługi nazw. Połączenia powyżej tylu będą odrzucane w warstwie podstawowe. Standardowy warstwa usuwa limit na nazw i zlicza zastosowania agregacji brokered połączenia przez Azure subskrypcji. W warstwie standardowy 1000 połączeń brokered na subskrypcję Azure będą mogli bez dodatkowych opłat (poza podstawowej opłaty). Przy użyciu w sumie 1000 połączeń brokered więcej niż warstwa standardowe usługi Bus nazw Azure subskrypcji zostanie naliczony stopniowe zgodnie z harmonogramem, jak pokazano w poniższej tabeli.

|Brokered połączenia (standardowa warstwa)|Koszt|
|---|---|
|Pierwszy 1000/miesiąc|Dołączone do podstawowej opłat|
|1000-100 000 miesięczny|0,03 USD na połączenia/miesiąc|
|500 000-100 000 miesięcy|0 $, 025N na połączenia/miesiąc|
|Do 500 000/miesiąca|0.015 $ na połączenia/miesiąc|

>[AZURE.NOTE] 1000 połączeń brokered dołączonych do wiadomości warstwie standardowy (przy użyciu podstawowych opłat za) i można je udostępnić we wszystkich kolejkach, tematy i subskrypcje w obrębie skojarzone Azure subskrypcji.

<br />

>[AZURE.NOTE] Rozliczenia jest oparty na Alokacja maksymalna liczba równoległych połączeń i jest wybrane proporcjonalnie, co godzinę na podstawie 744 godzin na miesiąc.

|Poziom Premium
|---|
|Brokered połączenia nie jest naliczany w warstwie Premium.|

Aby uzyskać więcej informacji o połączeniach brokered zobacz sekcję [często zadawane pytania](#faq) w dalszej części tego tematu.

## <a name="relay"></a>Funkcja przekazywania

Przekaźniki są dostępne tylko w standardowej warstwa nazw. W przeciwnym razie ceny i połączenia przydziałów dla przekaźniki pozostaną niezmienione. Oznacza to, że przekaźniki będą nadal nakładanych na liczby wiadomości (nie operacji) i przekazywania godzin.

|Przekazywania ceny|Koszt|
|---|---|
|Godziny przekazywania|0,10 $ dla każdej godziny 100 przekazywania|
|Wiadomości|0,01 $ dla każdej 10 000 wiadomości|

## <a name="faq"></a>FAQ

### <a name="how-is-the-relay-hours-meter-calculated"></a>Sposób obliczania miernik godzin przekazywania?

[Ten](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)temat.

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Co to są brokered połączenia i jak uzyskiwanie naliczona opłata za ich?

Połączenie brokered jest definiowana jako jedną z następujących czynności:

1. AMQP połączenie z klienta do kolejki Bus usługi lub tematu i subskrypcji.

2. Połączenie HTTP, aby otrzymywać wiadomości z tematem Bus usługi lub kolejki, która zawiera wartość limitu czasu Odbierz większa od zera.

Usługa Bus opłaty za Alokacja maksymalna liczba równoczesne połączeń brokered przekracza ilość uwzględniane (1000 w warstwie standardowe). Wartości są mierzone co godzinę, wybrane proporcjonalnie dzieląc przez 744 godzin w ciągu miesiąca, a następnie dodaje się nad miesięczny okres rozliczeniowy. Uwzględniane ilości (1000 połączeń brokered miesięcznie) jest stosowana na koniec okresu rozliczeniowego względem sumy proporcjonalna wartości co godzinę.

Na przykład:

1. Każde z 10 000 urządzeń łączy się za pomocą jednego połączenia AMQP i odbiera poleceń z tematu Bus usługi. Urządzenia Wyślij telemetrycznego zdarzeń do koncentratora zdarzenia. Jeśli wszystkie urządzenia połączyć 12 godzin dziennie, następujące opłat za połączenie stosowanie (oprócz innych usług Bus tematu opłat): 10 000 połączeń *12 godzin* 31 dni / 744 = 5000 brokered połączenia. Po miesięczny dodatek 1000 połączeń brokered czy naliczany 4000 brokered połączeń w wysokości 0,03 USD na jedną brokered połączenia, czyli łącznie 120 zł.

2. 10 000 urządzeń odbierania wiadomości z kolejki Bus usługi za pośrednictwem protokołu HTTP, określając limit czasu zera. Jeśli połączyć wszystkich urządzeń na 12 godzin codziennie, zobaczą następujące opłat za połączenie (oprócz innych opłat usługi Bus): 10 000 HTTP odbieranie połączeń *12 godzin dziennie* 31 dni i godzin 744 = 5000 brokered połączenia.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Opłat za połączenie brokered dotyczą kolejek i tematy i subskrypcji?

Wartość Tak. Istnieje nie opłat za połączenie do wysyłania zdarzeń przy użyciu protokołu HTTP, niezależnie od liczby wysyła systemy lub urządzenia. Odbieranie zdarzeń z przy użyciu większa od zera, nazywane "długi sondowanie," przekroczenie limitu czasu HTTP generuje opłat za połączenie brokered. Połączenia AMQP wygenerować opłat za połączenie brokered niezależnie od tego, czy połączenia są używane do wysyłania i odbierania. Zauważ, że 100 brokered połączenia są dozwolone bezpłatnie w podstawowych nazw. To także maksymalna liczba brokered połączeń dozwolonych dla Azure subskrypcji. Najpierw 1000 brokered połączeń przez wszystkie standardowe przestrzenie nazw w subskrypcji usługi Azure są uwzględniane bez dodatkowych opłat (poza podstawowej opłaty). Ponieważ te dodatki są wystarczająco obejmujące wiele scenariuszy wysyłania wiadomości do usługi, opłat za połączenie brokered zazwyczaj tylko stają się odpowiednie, jeśli zamierzasz użyć AMQP lub HTTP ankieta początkowym z dużą liczbą klientów. na przykład, aby osiągnąć efektywniejsze streaming wydarzenie lub włączyć komunikacja dwukierunkowa z wielu urządzeń lub wystąpień aplikacji.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o cenach Bus usługi zobacz [Bus usługi Azure ceny strony](https://azure.microsoft.com/pricing/details/service-bus/).

- Zobacz [Usługa Bus — często zadawane pytania](service-bus-faq.md#service-bus-pricing) dotyczące niektóre typowe często zadawane pytania bus usługi ceny i rozliczenia.

[Portal Azure klasyczny]: http://manage.windowsazure.com