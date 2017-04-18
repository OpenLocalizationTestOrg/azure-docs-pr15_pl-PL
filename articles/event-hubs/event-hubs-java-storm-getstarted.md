<properties
    pageTitle="Wprowadzenie do koncentratorów zdarzenia w języku Java z Apache Burza | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć używanie koncentratory zdarzenia Azure; wysyła zdarzenia przy użyciu języka Java i pobieranie ich w klastrze Burza Apache."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="sethm"/>

# <a name="get-started-with-event-hubs"></a>Wprowadzenie do koncentratorów zdarzenia

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie

Koncentratory zdarzenie jest wysoce skalowalna spożyciu systemu, w którym można pobrania miliony imprez na sekundę, włączanie aplikacji do przetwarzania i analizowania dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Po zbierane do koncentratorów zdarzenia, możesz Przekształcanie i przechowywanie danych przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym lub klaster miejsca do magazynowania.

Aby uzyskać więcej informacji zobacz [Omówienie koncentratory zdarzenia][].

Ten samouczek opisano, jak zbieranie wiadomości do koncentratora wydarzenia przy użyciu aplikacji konsoli w języku Java i pobieranie ich z użyciem Burza Apache.

Aby użyć tego samouczka, będą potrzebne następujące elementy:

+ Środowisko projektowania Java skonfigurowany do uruchomienia [środowiska Maven](http://maven.apache.org/). Dla tego samouczka przyjęto założenie [Zaćmienie](https://www.eclipse.org/).

+ Konto Azure active. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  Uruchamianie klasy **LogTopology** z Zaćmienie, a następnie poczekaj, aż go, aby uruchomić odbiorców dla wszystkie partycje.

2.  Uruchom projektu **nadawcy** , naciśnij klawisz **Enter** w oknie konsoli i przeglądać zdarzenia są wyświetlane w oknie odbiorcy.

    ![][22]

> [AZURE.NOTE] W tym samouczku tylko do celów rozwoju za pomocą Burza w trybie lokalnym. Zobacz [Omówienie Burza HDInsight][] i oficjalnym dokumentacji [Burza Apache][] uzyskać więcej informacji wdrożeń Burza i desenie.

## <a name="next-steps"></a>Następne kroki

Następujące zasoby są dostępne do tworzenia aplikacji, integracji koncentratory zdarzeń i Burza.

- [Analizowanie danych czujnik z Burza i HDInsight] jest samouczek pełny scenariusz mogły zjeść tej ostatniej danych czujnika w klastrze Hadoop za pomocą koncentratorów zdarzenia, Burza i HBase.
- [Opracowanie streaming aplikacji przetwarzania danych za pomocą SCP.NET i C# na Burza i HDInsight][] jest samouczek dotyczący jak napisać procesy Burza przy użyciu języka C#.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md

[Burza Apache]: https://storm.incubator.apache.org
[Omówienie Burza HDInsight]: ../hdinsight/hdinsight-storm-overview.md
[Analizowanie danych czujnik z Burza i HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Można opracowywać streaming aplikacji przetwarzania danych za pomocą SCP.NET i C# na Burza i HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 