<properties
    pageTitle="Azure Monitor autoscaling typowych metryki. | Microsoft Azure"
    description="Dowiedz się, jakie metryki są często używane do autoscaling z usługami w chmurze, maszyn wirtualnych i aplikacji sieci Web."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure metryki typowych autoscaling monitora

Azure Monitor autoscaling pozwala przeskalować liczbę uruchomione wystąpienia, w górę lub w dół, na podstawie danych telemetrycznych (metryki). W tym dokumencie opisano typowe miar, które warto używać. W Portal Azure dla usług w chmurze i farmy serwerów można wybrać metryki zasobów, aby przeskalować. Jednak możesz również wybrać dowolny metryki z innego zasobu przeskalować przez.

Poniżej przedstawiono szczegółowe informacje o tym, jak znaleźć i listy metryki, który chcesz skalować przez. Stosuje również skalowania zestawy skali maszyn wirtualnych.

## <a name="compute-metrics"></a>Obliczanie metryki
Domyślnie maszyn wirtualnych Azure w wersji 2 pochodzi z rozszerzeniem diagnostyki skonfigurowane i mają następujące metryki włączone.

- [Metryki gościa dla maszyn wirtualnych systemu Windows w wersji 2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Metryki gościa dla maszyn wirtualnych systemu Linux w wersji 2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Możesz użyć `Get MetricDefinitions` interfejsu API-PoSH-polecenie, aby wyświetlić dostępne dla zasobu VMSS metryki. 

Jeśli korzystasz z zestawów skali maszyn wirtualnych i nie widać określonej metryki na liście, jest prawdopodobnie *wyłączone* w numer wewnętrzny diagnostyki.

Jeśli nie jest określonej metryki pobrane lub transferowane częstotliwością ma, można zaktualizować konfiguracji diagnostyki.

Jeśli w obu przypadkach powyżej ma wartość true, następnie przejrzyj [Użyj programu PowerShell umożliwiające diagnostyki Azure w maszyny wirtualnej z systemem Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) o programu PowerShell do konfigurowania i aktualizowania rozszerzenia diagnostyki maszyn wirtualnych Azure umożliwiające metrykę. Ten artykuł zawiera także przykładowy plik konfiguracyjny diagnostyki.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Obliczanie metryki dla maszyn wirtualnych systemu Windows w wersji 2 jako gość systemu operacyjnego

Po utworzeniu nowego maszyn wirtualnych (wersja 2) w Azure diagnostyki jest włączona przy użyciu rozszerzenia diagnostyki.

Istnieje możliwość wygenerowania listę miar przy użyciu następującego polecenia w programie PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Możesz utworzyć alert dla następujących metryki.

|Nazwa metryki|   Jednostki|
|---|---|
|\Processor(_Total)\% czas procesora    |Procent|
|\Processor(_Total)\% uprzywilejowane czasu   |Procent|
|\Processor(_Total)\% czas użytkownika |Procent|
|Częstotliwość \Processor informacje (_Total) \Processor |Liczba|
|\System\Processes| Liczba|
|Liczba \Thread \Process (_Total)| Liczba|
|Liczba \Handle \Process (_Total)  |Liczba|
|\Memory\% Zadeklarowane bajty w użyciu   |Procent|
|\Memory\Available bajtów|   Bajtów|
|\Memory\Committed bajtów    |Bajtów|
|\Memory\Commit limit|  Bajtów|
|Bajty stronicowanej \Memory\Pool|  Bajtów|
|\Memory\Pool niestronicowanej|   Bajtów|
|\PhysicalDisk(_Total)\% czasu na dysku| Procent|
|\PhysicalDisk(_Total)\% czas odczytu dysku|    Procent|
|\PhysicalDisk(_Total)\% czas zapisu na dysku|   Procent|
|Transfer \Disk \PhysicalDisk (_Total) na sekundę   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk/s   |CountPerSecond|
|Zapisy \Disk \PhysicalDisk (_Total) na sekundę  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk bajtów/s   |BytesPerSecond|
|Bajty odczytane/s \Disk \PhysicalDisk (_Total)| BytesPerSecond|
|\PhysicalDisk (_Total) \Disk zapisu bajtów na sekundę |BytesPerSecond|
|\Avg \PhysicalDisk (_Total) Długość kolejki dysku|  Liczba|
|\Avg \PhysicalDisk (_Total) Długość kolejki odczytu dysku| Liczba|
|\Avg \PhysicalDisk (_Total) Długość kolejki zapisu dysku |Liczba|
|\LogicalDisk(_Total)\% miejsca| Procent|
|Megabajtów \Free \LogicalDisk (_Total)|   Liczba|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Obliczanie metryki dla maszyn wirtualnych systemu Linux w wersji 2 jako gość systemu operacyjnego

Po utworzeniu nowego maszyn wirtualnych (wersja 2) w Azure diagnostyki jest domyślnie włączona przy użyciu narzędzia diagnostyczne rozszerzenia.

Istnieje możliwość wygenerowania listę miar przy użyciu następującego polecenia w programie PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Możesz utworzyć alert dla następujących metryki.

|Nazwa metryki                            |Jednostki|
|---|---|
|\Memory\AvailableMemory                |Bajtów|
|\Memory\PercentAvailableMemory         |Procent|
|\Memory\UsedMemory                     |Bajtów|
|\Memory\PercentUsedMemory              |Procent|
|\Memory\PercentUsedByCache             |Procent|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bajtów|
|\Memory\PercentAvailableSwap           |Procent|
|\Memory\UsedSwap                       |Bajtów|
|\Memory\PercentUsedSwap                |Procent|
|\Processor\PercentIdleTime             |Procent|
|\Processor\PercentUserTime             |Procent|
|\Processor\PercentNiceTime             |Procent|
|\Processor\PercentPrivilegedTime       |Procent|
|\Processor\PercentInterruptTime        |Procent|
|\Processor\PercentDPCTime              |Procent|
|\Processor\PercentProcessorTime        |Procent|
|\Processor\PercentIOWaitTime           |Procent|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekundy|
|\PhysicalDisk\AverageWriteTime         |Sekundy|
|\PhysicalDisk\AverageTransferTime      |Sekundy|
|\PhysicalDisk\AverageDiskQueueLength   |Liczba|
|\NetworkInterface\BytesTransmitted     |Bajtów|
|\NetworkInterface\BytesReceived        |Bajtów|
|\NetworkInterface\PacketsTransmitted   |Liczba|
|\NetworkInterface\PacketsReceived      |Liczba|
|\NetworkInterface\BytesTotal           |Bajtów|
|\NetworkInterface\TotalRxErrors        |Liczba|
|\NetworkInterface\TotalTxErrors        |Liczba|
|\NetworkInterface\TotalCollisions      |Liczba|




## <a name="commonly-used-web-server-farm-metrics"></a>Często używane metryki sieci Web (farmy serwerów)

Można również wykonać autoscale na podstawie typowych metryk serwer sieci web, takich jak długość kolejki Http. Nazwa metryki jego jest **HttpQueueLength**.  Sekcji poniżej wymieniono dostępnego serwera metryki farmy (aplikacje sieci Web).

### <a name="web-apps-metrics"></a>Metryki aplikacji sieci Web

Istnieje możliwość wygenerowania listę metryki aplikacji sieci Web przy użyciu następującego polecenia w programie PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Możesz alert na lub przeskalować przez te metryki.

|Nazwa metryki        |Jednostki|
|---                |---|
|CpuPercentage      |Procent|
|MemoryPercentage   |Procent|
|DiskQueueLength    |Liczba|
|HttpQueueLength    |Liczba|
|Właściwość BytesReceived      |Bajtów|
|Żądania          |Bajtów|


## <a name="commonly-used-storage-metrics"></a>Często używane metryki magazynowania
Można skalować długość kolejki miejsca do magazynowania, czyli liczba wiadomości w kolejce miejsca do magazynowania. Długość kolejki miejsca do magazynowania jest specjalne jednostki metryczne i progu zastosowane będzie liczba wiadomości dla każdego wystąpienia. Oznacza to, jeśli istnieją dwa wystąpienia, a jeśli próg jest ustawiona na 100, będzie skalowanie w przypadku całkowita liczba wiadomości w kolejce 200. Na przykład 100 wiadomości dla każdego wystąpienia.

Możesz skonfigurować to Portal Azure w karta **Ustawienia** . Dla zestawów skali maszyn wirtualnych możesz zaktualizować ustawienia Autoscale w szablonie ARM *metricName* *ApproximateMessageCount* oraz przekazać identyfikator kolejki miejsca do magazynowania jako *metricResourceUri*.

Na przykład z kontem miejsca do magazynowania klasyczny metricTrigger ustawienie autoscale będzie zawierać:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Konta miejsca do magazynowania (innych niż klasyczny) będzie zawierać metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Często używane metryki Bus usługi

Można skalować długość kolejki Bus usługi, czyli liczba wiadomości w kolejce Bus usługi. Długość kolejki Bus usługi jest specjalne jednostki metryczne i progu określonego zastosowane będzie liczba wiadomości dla każdego wystąpienia. Oznacza to, jeśli istnieją dwa wystąpienia, a jeśli próg jest ustawiona na 100, będzie skalowanie w przypadku całkowita liczba wiadomości w kolejce 200. Na przykład 100 wiadomości dla każdego wystąpienia.

Dla zestawów skali maszyn wirtualnych możesz zaktualizować ustawienia Autoscale w szablonie ARM *metricName* *ApproximateMessageCount* oraz przekazać identyfikator kolejki miejsca do magazynowania jako *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] W przypadku Bus usługi koncepcja grupa zasobów nie istnieje, ale Menedżer zasobów Azure tworzy grupę domyślną zasobu w rozbiciu na regiony. Grupa zasobów jest zazwyczaj w formacie "Default - ServiceBus-[region]". Na przykład "Domyślny-ServiceBus-EastUS", "Domyślny-ServiceBus-WestUS", "Domyślny-ServiceBus-AustraliaEast" itp.
