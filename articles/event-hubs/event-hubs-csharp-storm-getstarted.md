<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w języku C# z Apache Burza | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć używanie koncentratory zdarzenia Azure; Wysyłanie zdarzenia w C# i odbioru klaster Burza Apache."
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
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenie jest wysoce skalowalna spożyciu systemu, w którym można pobrania miliony imprez na sekundę, włączanie aplikacji do przetwarzania i analizowania dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Po zbierane do koncentratorów zdarzenia, możesz Przekształcanie i przechowywanie danych przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym lub klaster miejsca do magazynowania.

Aby uzyskać więcej informacji zobacz [Omówienie koncentratory zdarzenia].

W tym samouczku dowiesz się, jak mogły zjeść tej ostatniej wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli w języku C# i pobieranie ich z użyciem Burza Apache.

Aby można było użyć tego samouczka będą potrzebne następujące elementy:

+ [Program Microsoft Visual Studio](http://visualstudio.com)

+ Środowisko projektowania Java skonfigurowany do uruchomienia [środowiska Maven](http://maven.apache.org/). Dla tego samouczka przyjęto założenie [Zaćmienie](https://www.eclipse.org/)

+ Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć bezpłatne konto na kilka minut. Aby uzyskać szczegółowe informacje zobacz <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure bezpłatnej wersji próbnej</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  Uruchamianie klasy **LogTopology** z Zaćmienie, a następnie poczekaj, aż go, aby uruchomić odbiorców dla wszystkie partycje.

2.  Uruchom projektu **nadawcy** , naciśnij klawisz **Enter** w oknie konsoli i przeglądać zdarzenia są wyświetlane w oknie odbiorcy.

    ![][22]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy po skonstruowaniu aplikacji pracy, która umożliwia utworzenie Centrum zdarzeń i wysyła i odbiera dane na można przenosić do następujących scenariuszy:

- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 