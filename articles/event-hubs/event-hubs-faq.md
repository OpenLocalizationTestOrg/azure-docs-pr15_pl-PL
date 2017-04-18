<properties 
    pageTitle="Koncentratory zdarzenia — często zadawane pytania (FAQ) | Microsoft Azure"
    description="Koncentratory zdarzenia — często zadawane pytania."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Koncentratory zdarzenia — często zadawane pytania

Koncentratory zdarzenia udostępnia pobrania na dużą skalę, utrzymywanie i przetwarzanie zdarzeń dane ze źródeł danych wysokiej przepustowości i/lub miliony urządzeń. Jeśli z kolejki Bus usługi i tematów, koncentratory zdarzenia umożliwia trwałych wdrożeniach polecenia i kontrolowanie scenariuszach [Internet czynności (IoT)](https://azure.microsoft.com/services/iot-hub/) .

W tym artykule omówiono cenach i zawiera odpowiedzi na niektóre często zadawane pytania dotyczące koncentratorów zdarzeń:

## <a name="pricing-information"></a>Informacje o cenach

Aby uzyskać pełne informacje o cenach koncentratory zdarzenia zobacz [szczegółowe informacje o cenach koncentratory zdarzenia](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Sposób obliczania zdarzeń ingress koncentratory zdarzenia

Każde zdarzenie wysyłane do koncentratora zdarzenia liczy jako fakturowanego wiadomości. *Zdarzenia ingress* jest definiowana jako jednostka danych, która jest mniejsza niż lub równa argumentowi 64 KB. Zdarzenie, która jest mniejsza niż lub równe 64 KB rozmiar jest uważany za jedno zdarzenie rozliczaniu. Jeśli zdarzenie jest większy niż 64 KB, liczba zdarzeń fakturowanego jest obliczana w rozmiarze zdarzenia w wielokrotnościach 64 KB. Na przykład zdarzenie 8 KB wysyłane do Centrum zdarzenie jest zafakturowane jako jedno zdarzenie, ale 96 KB wiadomości wysyłane do Centrum zdarzenie jest zafakturowane jako dwa zdarzenia.

Zdarzenia, które zużyte z Centrum zdarzenia jako także operacji zarządzania i kontrolować połączenia, takie jak punkty kontrolne, nie są liczone jako fakturowanego ingress zdarzenia, ale Naliczanie maksymalnie odliczenie jednostki przepustowość.

## <a name="what-are-event-hubs-throughput-units"></a>Co to są jednostki przepustowości koncentratorów zdarzenia?

Możesz wybrać jawnie koncentratory zdarzenia przepustowość jednostki, za pomocą portalu Azure lub szablony Menedżera zasobów koncentratory zdarzenia. Jednostki przepustowość dotyczą wszystkich koncentratorach zdarzenia w obszarze nazw koncentratory zdarzenia, a każdej jednostki przepustowość uprawnia nazw do następujące możliwości:

- W górę do 1 MB na sekundę ingress zdarzenia (zdarzenia wysyłane do koncentratora zdarzenia), ale nie więcej niż 1000 zdarzeń ingress, operacji zarządzania lub formantu interfejsu API połączeń na sekundę.

- Do 2 MB na sekundę zdarzenia wyjściowego (zdarzenia zużyte z Centrum zdarzeń).

- Maksymalnie 84 GB miejsca zdarzenia (umożliwiający okres przechowywania 24-godzinnego domyślne).

Jednostki przepustowości koncentratorów wydarzenia są wystawiona co godzinę, według maksymalna liczba jednostek wybranych ciągu danej godziny.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Jak są stosowane limity jednostki przepustowości koncentratorów zdarzenia

Jeśli przepustowość ingress całkowita lub prędkości zdarzenia sumy wejściowej między wszystkimi koncentratorami zdarzenia w obszarze nazw przekracza dodatki jednostki przepustowość agregujących, nadawcy będą ograniczenie i błędów, wskazująca, że przekroczono przydział ingress.

Jeśli przepustowość sumy wyjściowym lub stopa wyjściowego sumy wydarzenia między wszystkimi koncentratorami zdarzenia w obszarze nazw przekracza dodatki jednostki Agreguj przepustowość, odbiorców są ograniczenie i błędów, wskazująca, że przekroczono przydział wyjściowe. Ingress i wyjściowego przydziały są wymuszane oddzielnie, dlatego nadawcy może powodować zużycie zdarzenia zwolnić ani słuchawka zapobiec zdarzeń wysyłana do koncentratora zdarzenia.

Należy zauważyć, że wybór jednostki przepustowość niezależnie od liczby partycje koncentratory zdarzenia. Podczas każdego partition oferuje maksymalnej przepustowości 1 MB na drugim ingress (z maksymalnie 1000 zdarzeń sekundę) i 2 MB na drugim wyjściowym, jest bezpłatna stały się partycje. Opłata dotyczy jednostki zagregowaną przepustowość na wszystkich koncentratorach zdarzenia w obszarze nazw koncentratory zdarzenia. Za mało partycje do obsługi przewidywanego maksymalne obciążenie systemów, bez ponoszenia opłat jednostek przepustowość do momentu Załaduj zdarzeń w systemie faktycznie wymaga większe liczby przepustowość, a bez konieczności zmieniania struktury i architektury systemów co obciążenie w systemie zwiększa można tworzyć przy użyciu tego wzorca.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Czy istnieje limit liczby jednostek przepustowość, które można wybrać?

Istnieje domyślny przydział 20 jednostek przepustowość na przestrzeń nazw. Możesz przejąć normę większych jednostek przepustowość, zachowując bilet pomocy technicznej. Poza limit jednostek 20 przepustowość pakiety są dostępne w jednostkach przepustowość 20 i 100. Należy zauważyć, że za pomocą więcej niż 20 jednostek przepustowość usuwa możliwość zmiany liczby jednostek przepustowość bez zgłoszenia bilet pomocy technicznej.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Czy istnieje opłatę za zachowywania zdarzeń koncentratory zdarzeń dla więcej niż 24 godziny?

Zdarzenia koncentratory standardowy warstwa Zezwól przechowywania wiadomości okresy dłuższe niż 24 godziny, maksymalnie przez 30 dni. Jeśli rozmiar całkowitej liczby przechowywane zdarzenia przekracza odliczenie miejsca do magazynowania dla liczby jednostek wybranej przepustowości (84 GB na jednostkę przepustowość), rozmiar, którego rozmiar przekracza dodatek jest rozliczana według stawki opublikowanych obiektów Blob platformy Azure miejsca do magazynowania. Dodatek miejsca do magazynowania w każdej jednostki przepustowość obejmuje wszystkie koszty miejsca do magazynowania dla okresów przechowywania 24 godzin (ustawienie domyślne), nawet jeśli jednostka przepustowość jest używany w górę do odliczenie ingress maksymalna.

## <a name="what-is-the-maximum-retention-period"></a>Co to jest okres przechowywania maksymalną?

Warstwa koncentratory standardowy zdarzenie obsługuje obecnie okres przechowywania maksymalną 7 dni. Uwaga koncentratory zdarzenia nie są przeznaczone jako Magazyn trwały danych. Okresy przechowywania większe niż 24 godziny są przeznaczone do scenariusze, w których jest wygodny do ponownego odtworzenia strumienia zdarzenia, w tym samym systemów; na przykład do szkolenia lub weryfikacji nowy model nauki komputera na istniejące dane.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Jak jest magazynowania koncentratory zdarzenia obliczania i naliczany?

Całkowity rozmiar przechowywane zdarzenia, włącznie dowolnego wewnętrznych nagłówki zdarzeń lub struktur na dysku miejsca do magazynowania w wszystkich koncentratorach zdarzenia, jest mierzony w ciągu dnia. Maksymalny rozmiar miejsca do magazynowania jest obliczana na koniec dnia. Dzienny dodatek miejsca do magazynowania jest obliczana na podstawie minimalna liczba jednostek przepustowość, które zostały wybrane w ciągu dnia (każdej jednostki przepustowość zawiera dodatek 84 GB). Jeśli całkowity rozmiar przekracza obliczeniowe dzienny odliczenie miejsca do magazynowania, nadmiar magazynowania jest wystawiona przy użyciu stawki magazyn obiektów Blob platformy Azure (stawki **Lokalnie zbędne miejsca do magazynowania** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Do wysyłania i odbierania koncentratory zdarzeń i Bus usługi kolejek tematy mogą używać pojedynczego połączenia AMQP?

Tak, jak wszystkie koncentratory zdarzenia, kolejki i tematy są w tym samym nazw. Jako takie można zaimplementować dwukierunkowego, brokered łączności z wielu urządzeń z subsecond opóźnienia w sposób efektywny pod względem kosztów i wysoce skalowalna.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Opłat za połączenie brokered mają zastosowania do koncentratorów zdarzenia?

Dla nadawców opłata jak za połączenie tylko wtedy, gdy jest używany protokół AMQP. Istnieje nie opłat za połączenie do wysyłania zdarzeń przy użyciu protokołu HTTP, niezależnie od liczby wysyła systemy lub urządzenia. Jeśli planujesz używać AMQP (na przykład uzyskanie efektywniejsze streaming zdarzenie lub włączyć komunikacja dwukierunkowa w scenariuszach polecenia i kontrolowanie IoT), zobacz dołączoną stronę [informacje o cenach Bus usługi](https://azure.microsoft.com/pricing/details/service-bus/) , aby uzyskać informacje o co stanowi połączenie brokered i jak są taryfowe.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Jaka jest różnica między standardowej warstwy i zdarzeń koncentratory podstawowe?

Warstwa koncentratory standardowy zdarzenia zapewnia funkcje poza co to jest dostępne w zdarzenia koncentratory podstawowe i w niektórych systemach konkurencyjności. Funkcje te obejmują okres przechowywania więcej niż 24 godziny, a także możliwość wysyłania poleceń na większej liczbie urządzeń z opóźnienia subsecond, a także wysyłanie telemetrycznego na tych urządzeniach do zdarzenia koncentratory za pomocą jednego połączenia AMQP. Lista funkcji zobacz [szczegółowe informacje o cenach koncentratory zdarzenia](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Dostępność geograficznych

Koncentratory zdarzenie jest dostępny w następujących regionach:

|Geo|Obszary|
|---|---|
|Stany Zjednoczone|Centralnej Stany Zjednoczone, wschodniego USA, USA wschodniego 2, centralnej Południowej w Stanach Zjednoczonych, zachód Stany Zjednoczone|
|Europa|Europa Północna, Europa Zachodnia|
|Części Azji|Azji Wschodniej Azji Południowo-Wschodniej|
|Japonia|Japonia wschód, zachód Japonii|
|Brazylia|Brazylia Południowej|
|Australia|Australia Wschodzie, południowo Australia|

## <a name="support-and-sla"></a>Umowa dotycząca poziomu usług i pomocy technicznej

Pomoc techniczna dla koncentratorów wydarzenia są udostępniane na [forach społeczności](https://social.msdn.microsoft.com/forums/azure/home). Obsługa zarządzania rozliczeń i subskrypcji jest udostępniana bezpłatnie.

Aby dowiedzieć się więcej na temat naszych Umowa dotycząca poziomu usług, zobacz stronę [Umowy](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat koncentratory zdarzenia, zobacz następujące artykuły:

- [Omówienie koncentratory zdarzenia][].
- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].

[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
