<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w C i C# | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć używanie koncentratory zdarzenia Azure; Wysyłanie zdarzeń w C i odbieranie hem w C# za pomocą EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenie jest wysoce skalowalna spożyciu systemu, w którym można pobrania miliony imprez na sekundę, włączanie aplikacji do przetwarzania i analizowania dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Po zbierane do koncentratorów zdarzenia, możesz Przekształcanie i przechowywanie danych przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym lub klaster miejsca do magazynowania.

Aby uzyskać więcej informacji zobacz [Omówienie koncentratory zdarzenia][].

W tym samouczku dowiesz się, jak mogły zjeść tej ostatniej wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli w C i pobieranie ich równoległego korzystania z biblioteki C# [Hosta procesor zdarzenia][] .

Aby użyć tego samouczka, będą potrzebne następujące elementy:

+ Środowisku programowania C. Ten samouczek przyjmie stosem Zatoce na [Maszyn wirtualnych Linux Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md) z Ubuntu 14.04. Instrukcje dotyczące innych środowisk będzie dostępna w łącza zewnętrzne.

+ Microsoft Visual Studio Express dla systemu Windows

+ Konto Azure active. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  Uruchamianie **odbiorcy** projektu z poziomu programu Visual Studio, a następnie poczekaj, aż go, aby uruchomić odbiorców dla wszystkie partycje.

    ![][21]

2.  Uruchom program **nadawcy** i przeglądać zdarzenia są wyświetlane w oknie odbiorcy.

    ![][24]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy po skonstruowaniu aplikacji pracy, która umożliwia utworzenie Centrum zdarzeń i wysyła i odbiera dane na można przenosić do następujących scenariuszy:

- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .
- [Omówienie koncentratory zdarzenia][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host procesor zdarzenia]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
