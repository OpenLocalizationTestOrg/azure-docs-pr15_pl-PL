<properties
    pageTitle="Co to jest Azure zdarzenia koncentratory? | Microsoft Azure"
    description="Omówienie i opis koncentratory zdarzenia Azure"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# <a name="what-is-azure-event-hubs"></a>Co to jest Azure zdarzenia koncentratory?

Azure koncentratory zdarzenia to usługa ingress wysoce skalowalna danych, która może mogły zjeść tej ostatniej miliony wydarzeń na sekundę, aby mogła procesu i analizowanie dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Koncentratory zdarzenie działa jako "drzwi wejściowe" dla zdarzenia planowana i gdy dane są zbierane do koncentratora zdarzenia, może zostać zamieniony i przechowywane za pomocą dowolnego dostawcy analizy w czasie rzeczywistym lub tworzeniu partii magazynowania karty. Koncentratory zdarzenia oddzielono produkcji strumień zdarzenia ze zużycia te zdarzenia, tak, aby odbiorcy zdarzenia dostęp do zdarzenia własne harmonogramem. Aby uzyskać więcej informacji i szczegółowe informacje techniczne zobacz [Omówienie koncentratory zdarzenia](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Możliwości koncentratory zdarzenia

Koncentratory zdarzenia to zdarzenie usługa, która zapewnia zdarzeń i przetwarzania telemetrycznego na ogromną skalę krótki czas oczekiwania i wysoka niezawodność przetwarzania. Ta usługa jest szczególnie przydatne dla:

- Aplikacja oprzyrządowania
- Środowisko użytkownika lub przetwarzanie przepływu pracy
- Scenariusze Internet czynności (IoT)

Niektóre inne możliwości klucza koncentratory zdarzenia zawierają zachowanie śledzenia w aplikacji dla urządzeń przenośnych, ruchu informacje z farmy serwerów sieci web, przechwytywania zdarzeń w gry w konsoli gry lub telemetrycznego zebrane z maszyny przemysłowe lub pojazdy połączonego.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać szczegółowe informacje dotyczące koncentratorów zdarzenia zobacz następujące tematy.

- [Omówienie koncentratory zdarzenia](event-hubs-overview.md)
- [Koncentratory zdarzenia programowania przewodnika](event-hubs-programming-guide.md)
- [Zdarzenia koncentratory dostępności i pomoc techniczna — często zadawane pytania](event-hubs-availability-and-support-faq.md)
- Rozpoczynanie pracy z [samouczka koncentratory zdarzenia][]
- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][]

[Samouczek koncentratory zdarzenia]: event-hubs-csharp-ephcs-getstarted.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
