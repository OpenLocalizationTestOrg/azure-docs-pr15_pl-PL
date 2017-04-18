<properties
   pageTitle="Stanowe diagnostyki zaufania usługi | Microsoft Azure"
   description="Funkcje diagnostyczne dla usług zaufanego Stateful"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Funkcje diagnostyczne dla usług zaufanego Stateful
Klasy Stateful zaufanego StatefulServiceBase usług emituje zdarzeń [źródła zdarzeń](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) , które mogą być używane do debugowania usługi, Przekaż spostrzeżeń jak działa środowisko uruchomieniowe i pomoc w rozwiązywaniu problemów.

## <a name="eventsource-events"></a>Zdarzenia źródła zdarzeń
Nazwa źródła zdarzeń dla klasy Stateful zaufanego usług StatefulServiceBase jest "Ruch usługi Microsoft ServiceFabric". Wydarzenia z tego źródła zdarzenia pojawiają się w oknie [Zdarzenia diagnostyki](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) , gdy usługa jest [debugowania w programie Visual Studio](service-fabric-debugging-your-application.md).

Przykłady narzędzi i technologii, które pomagają w zbierania i/lub wyświetlanie zdarzeń źródła zdarzeń są [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Diagnostyka pakietu Microsoft Azure](../cloud-services/cloud-services-dotnet-diagnostics.md)i [Biblioteki TraceEvent Microsoft](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Zdarzenia

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Opis zdarzenia|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Przedstawienie informacji|Podczas zostanie uruchomiona usługa RunAsync zadania|
|StatefulRunAsyncCancellation|2|Przedstawienie informacji|Podczas zadanie RunAsync usługi czy zostało anulowane.|
|StatefulRunAsyncCompletion|3|Przedstawienie informacji|Podczas usługi RunAsync zadanie zostało ukończone|
|StatefulRunAsyncSlowCancellation|4|Ostrzeżenie|Podczas zadanie RunAsync usługi trwa zbyt długo, aby ukończyć anulowania|
|StatefulRunAsyncFailure|5|Błąd|Podczas zadanie RunAsync usługi zgłasza wyjątek|

## <a name="interpret-events"></a>Interpretowanie zdarzenia

Zdarzenia StatefulRunAsyncInvocation, StatefulRunAsyncCompletion i StatefulRunAsyncCancellation są przydatne Writer usługi, aby poznać zasady cyklu świadczenia usługi —, a także chronometraż po ukończeniu, zostało anulowane lub pracę usługi. Może to być przydatne podczas debugowania problemy z usługą lub opis świadczenia usługi.

Moduły zapisujące usługi powinna zwrócić szczególną uwagę na zdarzenia StatefulRunAsyncSlowCancellation i StatefulRunAsyncFailure, ponieważ wskazują problemy z usługą.

StatefulRunAsyncFailure jest wysyłane, gdy zadanie RunAsync() usługi zgłasza wyjątek. Zazwyczaj wyjątek wskazuje komunikat o błędzie lub błąd w usłudze. Ponadto wyjątek powoduje usługi kończy się niepowodzeniem, więc jest ona przenoszona do innego węzła. Może to być operacji drogich i może opóźnić przychodzących żądań, gdy usługa jest przenoszony. Autorzy usługi należy ustalić przyczynę wyjątku i, jeśli to możliwe, zmniejszeniu go.

StatefulRunAsyncSlowCancellation jest wysyłane, gdy żądanie anulowania zadania RunAsync trwa dłużej niż cztery sekundy. Usługa trwa zbyt długo, aby ukończyć anulowania, wpływa na możliwość szybkiego ponownego uruchomienia w innym węźle usługi. Może to wpłynąć dostępności usługi.
