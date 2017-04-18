<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w języku Java | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć używanie koncentratory zdarzenia Azure; wysyła zdarzenia przy użyciu języka Java i pobieranie ich w C# za pomocą EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenie jest wysoce skalowalna spożyciu systemu, w którym można pobrania miliony imprez na sekundę, włączanie aplikacji do przetwarzania i analizowania dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Po zbierane do koncentratorów zdarzenia, możesz Przekształcanie i przechowywanie danych przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym lub klaster miejsca do magazynowania.

Aby uzyskać więcej informacji zobacz [Omówienie koncentratory zdarzenia][].

W tym samouczku pokazano, jak mogły zjeść tej ostatniej wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli w języku Java i pobieranie ich równoległego korzystania z biblioteki C# [Hosta procesor zdarzenia][] .

Aby można było użyć tego samouczka, będą potrzebne następujące elementy:

+ Środowisko projektowania Java. Ten samouczek przyjmie [Zaćmienie](https://www.eclipse.org/).

+ [Program Microsoft Visual Studio](http://visualstudio.com)

+ Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć bezpłatne konto na kilka minut. Aby uzyskać szczegółowe informacje zobacz <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure bezpłatnej wersji próbnej</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  Uruchamianie programu project **odbiorcy** z programu Visual Studio, a następnie poczekaj, aż go, aby uruchomić odbiorców dla wszystkie partycje.

    ![][21]

2.  Uruchom projekt **nadawcy** .

    ![][22]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy po skonstruowaniu aplikacji pracy, która umożliwia utworzenie Centrum zdarzeń i wysyła i odbiera dane na można przenosić do następujących scenariuszy:

- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .

Aby uzyskać więcej informacji zobacz [Centrum deweloperów języka Java](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-java-ephcs-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host procesor zdarzenia]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 