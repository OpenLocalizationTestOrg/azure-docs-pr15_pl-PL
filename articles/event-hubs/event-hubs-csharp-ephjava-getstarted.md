<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w języku C# | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć używanie koncentratory zdarzenia Azure; Wysyłanie zdarzenia w języku C# i odbioru Java przy użyciu EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenia to usługa, która przetwarza dużych ilości danych zdarzenia (telemetrycznego) z połączenia urządzeń i aplikacji. Po do koncentratorów zdarzenie jest zbieranie danych, możesz przechowywanie danych przy użyciu klastrze miejsca do magazynowania lub przekształcić go za pośrednictwem dostawcy w czasie rzeczywistym analizy. Ta funkcja zbierania i przetwarzania dużych zdarzenie jest składnikiem klucza architektury nowoczesny aplikacji, łącznie z Internetem co (IoT).

Ten samouczek pokazano, jak za pomocą portalu klasyczny Azure Tworzenie Centrum zdarzenia. Go również pokazano, jak zbierać wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli napisana C# i jak je odzyskać równoległego korzystania z biblioteki Java zdarzenia procesor hosta.

Aby użyć tego samouczka, musisz następujące czynności:

+ [Program Microsoft Visual Studio](http://visualstudio.com)

+ Konto Azure active. <br/>Jeśli nie masz, możesz utworzyć bezpłatne konto na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  Uruchom projekt **odbiorcy** .

    ![][21]

2.  Uruchom projekt **nadawcy** .

    ![][22]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy po skonstruowaniu aplikacji pracy, która umożliwia utworzenie Centrum zdarzeń i wysyła i odbiera dane na można przenosić do następujących scenariuszy:

- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .
- [Omówienie koncentratory zdarzenia][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
