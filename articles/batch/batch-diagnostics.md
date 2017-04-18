<properties
   pageTitle="Rejestrowanie diagnostyczne Azure wsadowe | Microsoft Azure"
   description="Nagrywanie i analizowania zdarzeń dziennika diagnostycznych dla partii Azure konta zasobów, takich jak pule i zadania."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Rejestrowanie diagnostyczne Azure partii

Podobnie jak w przypadku wielu usług Azure, usługę partii emituje zdarzeń dziennika dla niektórych zasobów w czasie trwania zasobu. Możesz włączyć partii Azure dzienniki diagnostyczne w celu rejestrowanie zdarzeń dla zasobów, takich jak pule i zadań i następnie za pomocą dzienników diagnostycznych oceny i monitorowania. Tworzenie wydarzenia takie jak puli, Usuń puli, Rozpoczęcie zadania wykonania zadania i inne osoby są wyświetlane w partii dzienniki diagnostyczne.

>[AZURE.NOTE] W tym artykule omówiono rejestrowania zdarzeń dla samych zasobach konta partię, nie zadania i zadania, dane wyjściowe. Aby uzyskać szczegółowe informacje na temat przechowywania danych wyjściowych zadania i zadań zobacz [dane wyjściowe i zadań utrzymują partii Azure](batch-task-output.md).

## <a name="prerequisites"></a>Wymagania wstępne

* [Konto Azure partii](batch-account-create-portal.md)

* [Konto Azure miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account)

  Aby dzienniki diagnostyczne partii, musi utworzyć konto magazyn Azure miejsce, w którym Azure będą przechowywane pliki dziennika. Określanie tego konta magazynu podczas [włączyć rejestrowanie diagnostyczne](#enable-diagnostic-logging) dla Twojego konta partię. Konto miejsca do magazynowania, zadanej po włączeniu zbieranie dzienników nie jest taki sam, jak konto połączone magazynowania określonych w artykułach [pakietów aplikacji](batch-application-packages.md) i [utrzymywanie wynik zadania](batch-task-output.md) .

  >[AZURE.WARNING] Jest **naliczany** danych przechowywanych w konta magazynu platformy Azure. Ta opcja uwzględnia dzienniki diagnostyczne omawiane w tym artykule. Zapamiętaj podczas projektowania [zasad przechowywania dziennika](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Włącz rejestrowanie diagnostyczne

Domyślnie dla Twojego konta partii nie włączono rejestrowanie diagnostyczne. Należy najpierw włączyć rejestrowanie diagnostyczne dla każdego konta partii, które chcesz monitorować:

[Jak włączyć zbierania dzienników diagnostycznych](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Zaleca się, że możesz odczytane pełny artykuł [Omówienie Azure dzienniki diagnostyczne](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) , aby poznać nie tylko sposób, aby włączyć rejestrowanie, ale kategorie dziennika obsługiwana przez różne usługi Azure. Na przykład partii Azure obsługuje obecnie jedną kategorię dziennika: **Dzienniki usługi**.

## <a name="service-logs"></a>Dzienniki usługi

Dzienniki usługi partii Azure zawierają zdarzeń wysyłane przez usługę Azure partii czasie trwania zasobu partii puli lub zadania. Każde zdarzenie dostarczanych przez partii są przechowywane w określone konto miejsca do magazynowania w formacie JSON. Na przykład jest treści przykładowe **puli tworzenie zdarzenia**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Każda jednostka zdarzenia znajduje się w pliku .json w określone konto Azure miejsca do magazynowania. Jeśli chcesz uzyskać bezpośredni dostęp dzienniki, możesz przejrzeć [schematu dzienniki diagnostyczne w oknie konta miejsca do magazynowania](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Usługa dziennika zdarzeń

Usługa wsadowe emituje obecnie następujących zdarzeń dziennika usługi. Ta lista może nie być pełne, ponieważ dodatkowe zdarzenia dodano od czasu ostatniej aktualizacji w tym artykule.

| **Usługa dziennika zdarzeń** |
| ------------------ |
| [Tworzenie puli][pool_create] |
| [Rozpocznij usuwanie puli][pool_delete_start] |
| [Usuwanie puli wykonania][pool_delete_complete] |
| [Rozpocznij zmiany rozmiaru puli][pool_resize_start] |
| [Zmienianie rozmiaru puli wykonania][pool_resize_complete] |
| [Rozpoczęcie zadania][task_start] |
| [Zadanie wykonane][task_complete] |
| [Niepowodzenie zadania][task_fail] |

## <a name="next-steps"></a>Następne kroki

Oprócz przechowywania zdarzeń dziennika diagnostyczne w konto Azure miejsca do magazynowania, można również strumienia zdarzeń dziennika usługi partii do [Centrum zdarzeń Azure](../event-hubs/event-hubs-what-is-event-hubs.md)i wysłać je do [Analizy dziennika Azure](../log-analytics/log-analytics-overview.md).

* [Przesyłanie strumieniowe Azure dzienniki diagnostyczne w celu koncentratory zdarzenia](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Przesyłaj strumieniowo wsadowe zdarzeń diagnostycznych z usługą ingress wysoce skalowalna danych koncentratory zdarzenia. Koncentratory zdarzenia można mogły zjeść tej ostatniej miliony wydarzeń na sekundę, które można przekształcić i przechowywanie przy użyciu dowolnego dostawcy analizy w czasie rzeczywistym.

* [Analizowanie Azure dzienniki diagnostyczne za pomocą analizy dziennika](../log-analytics/log-analytics-azure-storage-json.md)

  Wyślij dzienniki diagnostyczne do analizy dziennika miejsce, w którym można analizować je w portalu usługi operacje zarządzania pakietu (OMS) lub je wyeksportować do analizy w usłudze Power BI lub Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
