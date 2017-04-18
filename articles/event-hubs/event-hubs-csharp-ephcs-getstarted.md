<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w języku C# | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę przy użyciu Azure zdarzenia koncentratory z C# i używanie EventProcessorHost."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenia to usługa, która przetwarza dużych ilości danych zdarzenia (telemetrycznego) z połączenia urządzeń i aplikacji. Po do koncentratorów zdarzenie jest zbieranie danych, możesz przechowywanie danych przy użyciu klastrze miejsca do magazynowania lub przekształcić go za pośrednictwem dostawcy w czasie rzeczywistym analizy. Ta funkcja zbierania i przetwarzania dużych zdarzenia jest składnikiem klucza architektury nowoczesny aplikacji, łącznie z Internetem co (IoT).

Ten samouczek pokazano, jak za pomocą portalu klasyczny Azure Tworzenie Centrum zdarzenia. Go też pokazano, jak zbierać wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli napisany w języku C# i jak je odzyskać przy użyciu równoległych biblioteka C# [Hosta procesor zdarzenia][] .

Aby użyć tego samouczka, musisz następujące czynności:

+ [Program Microsoft Visual Studio](http://visualstudio.com)

+ Konto Azure active. Jeśli nie masz, możesz utworzyć bezpłatne konto na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. Z w programie Visual Studio, otwórz projekt **słuchawka** utworzony wcześniej.

2. Kliknij prawym przyciskiem myszy rozwiązanie **odbiorcy** , a następnie kliknij przycisk **Dodaj**, a następnie kliknij **Istniejącego projektu**.
 
3. Znajdź istniejący plik Sender.csproj, a następnie kliknij dwukrotnie, aby dodać go do rozwiązania.
 
4. Ponownie kliknij prawym przyciskiem myszy rozwiązanie **odbiorcy** , a następnie kliknij przycisk **Właściwości**. Zostanie wyświetlona strona właściwości **odbiorcy** .

5. Kliknij pozycję **Projekt uruchamiania**, a następnie kliknij przycisk **wiele projektów uruchamiania** . Aby **rozpocząć**, należy zaznaczyć pole **akcji** dla projektów zarówno **odbiorcy** i **nadawcy** .

    ![][19]

6. Kliknij przycisk **współzależności projektów**. W oknie dialogowym **Projekty** kliknij **nadawcy**. W oknie dialogowym **zależny od** upewnij się, że jest zaznaczone pole wyboru **odbiorcy** .

    ![][20]

7. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości** .

1.  Naciśnij klawisz F5, aby uruchomić **odbiorcy** projektu z poziomu programu Visual Studio, a następnie poczekaj, aż go, aby uruchomić odbiorców dla wszystkie partycje.

    ![][21]

2.  Projekt **nadawcy** zostanie uruchomiony automatycznie. Naciśnij klawisz **Enter** w oknie konsoli i zobacz, zdarzenia są wyświetlane w oknie odbiorcy.

    ![][22]

Naciśnij **Klawisze Ctrl + C** w oknie **nadawcy** , aby zakończyć aplikację nadawcy, a następnie naciśnij klawisz **Enter** w oknie odbiorcy zamknąć tej aplikacji.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy po skonstruowaniu aplikacji pracy, która umożliwia utworzenie Centrum zdarzeń i wysyła i odbiera dane na można przenosić do następujących scenariuszy:

- Ukończono [przykładową aplikację, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .
- [Omówienie koncentratory zdarzenia][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host procesor zdarzenia]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[przykładową aplikację używa koncentratory zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
