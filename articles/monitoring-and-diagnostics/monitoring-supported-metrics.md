<properties
    pageTitle="Azure metryki Monitor - obsługiwane metryki na typ zasobu | Microsoft Azure"
    description="Lista metryki dostępne dla każdego typu zasobów przy użyciu monitora Azure."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Obsługiwane metryki Monitor Azure

Monitorowanie Azure udostępnia kilka sposobów interakcję miar, w tym wykresy je w portalu, uzyskiwanie do nich dostępu za pośrednictwem interfejsu API usługi REST i badania ich za pomocą programu PowerShell lub interfejsu wiersza polecenia. Oto pełną listę wszystkich metryki obecnie dostępne z potoku metryczne Azure Monitor.

> [AZURE.NOTE] Inne wskaźniki mogą być dostępne w portalu lub przy użyciu starszych interfejsów API. Ta lista obejmuje tylko publicznej Podgląd metryki dostępne za pomocą publicznej preview skonsolidowane potoku metryczne Azure Monitor.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|CoreCount|Liczba Core|Liczba|Suma|Całkowita liczba rdzeni na koncie partii|
|TotalNodeCount|Liczba węzeł|Liczba|Suma|Całkowita liczba węzłów na koncie partii|
|CreatingNodeCount|Tworzenie węzeł Statystyka|Liczba|Suma|Liczba węzłów tworzone|
|StartingNodeCount|Licznik węzeł początkowej|Liczba|Suma|Liczba węzłów uruchamianie|
|WaitingForStartTaskNodeCount|Oczekiwanie rozpoczęcia zadania węzeł Statystyka|Liczba|Suma|Liczba węzłów Trwa oczekiwanie na Rozpocznij zadanie do wykonania|
|StartTaskFailedNodeCount|Rozpoczęcie zadania nie powiodło się liczba węzeł|Liczba|Suma|Liczba węzłów miejsce, w którym Rozpocznij zadanie zostało zakończone niepowodzeniem|
|IdleNodeCount|Bezczynne Statystyka węzeł|Liczba|Suma|Liczba węzłów bezczynne|
|OfflineNodeCount|Liczba węzeł trybu offline|Liczba|Suma|Liczba węzłów trybu offline|
|RebootingNodeCount|Ponowne uruchamianie węzeł Statystyka|Liczba|Suma|Liczba ponowne uruchomienie węzły|
|ReimagingNodeCount|Liczba reimaging węzeł|Liczba|Suma|Liczba węzłów reimaging|
|RunningNodeCount|Liczba węzeł|Liczba|Suma|Liczba uruchomione węzły|
|LeavingPoolNodeCount|Opuszczanie Statystyka węzeł puli|Liczba|Suma|Liczba węzłów opuszczania puli|
|UnusableNodeCount|Nie można używać Statystyka węzeł|Liczba|Suma|Liczba węzłów nie będzie można używać|
|TaskStartEvent|Zdarzenia rozpoczęcia zadania|Liczba|Suma|Całkowita liczba zadań, które zostały uruchomione|
|TaskCompleteEvent|Zdarzenia wykonania zadania|Liczba|Suma|Całkowita liczba zadań, które zostały wykonane|
|TaskFailEvent|Zdarzenia błędów zadań|Liczba|Suma|Całkowita liczba zadań, które zostały wykonane w stanie awarii|
|PoolCreateEvent|Tworzenie puli zdarzeń|Liczba|Suma|Całkowita liczba pul, które zostały utworzone|
|PoolResizeStartEvent|Pula zmiany rozmiaru rozpoczęcia zdarzenia|Liczba|Suma|Całkowita liczba zmienia rozmiar puli, które zostały uruchomione|
|PoolResizeCompleteEvent|Zdarzenia Complete zmiany rozmiaru puli|Liczba|Suma|Całkowita liczba zmienia rozmiar puli, które zostały wykonane|
|PoolDeleteStartEvent|Pula Usuń rozpoczęcia zdarzenia|Liczba|Suma|Całkowita liczba usuwa puli, które zostały uruchomione|
|PoolDeleteCompleteEvent|Zdarzenia Complete Usuń puli|Liczba|Suma|Całkowita liczba usuwa puli, które zostały wykonane|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|connectedclients|Połączonych klientów|Liczba|Maksymalna||
|totalcommandsprocessed|Operacje razem|Liczba|Suma||
|Trafienia w pamięci podręcznej|Trafienia w pamięci podręcznej|Liczba|Suma||
|cachemisses|Chybienia w pamięci podręcznej|Liczba|Suma||
|getcommands|Otrzymuje|Liczba|Suma||
|setcommands|Zestawy|Liczba|Suma||
|evictedkeys|Usuniętego klawiszy|Liczba|Suma||
|totalkeys|Wszystkie klucze|Liczba|Maksymalna||
|expiredkeys|Wygasłe klawiszy|Liczba|Suma||
|usedmemory|Używane pamięci|Bajtów|Maksymalna||
|usedmemoryRss|Pamięci używanych danych RSS|Bajtów|Maksymalna||
|serverLoad|Obciążenie serwera|Procent|Maksymalna||
|cacheWrite|Wpisywanie pamięci podręcznej|BytesPerSecond|Maksymalna||
|cacheRead|Odczyt pamięci podręcznej|BytesPerSecond|Maksymalna||
|percentProcessorTime|PROCESOR|Procent|Maksymalna||
|connectedclients0|Połączonych klientów (Shard 0)|Liczba|Maksymalna||
|totalcommandsprocessed0|Operacje razem (Shard 0)|Liczba|Suma||
|cachehits0|Trafień w pamięci podręcznej (Shard 0)|Liczba|Suma||
|cachemisses0|Chybienia w pamięci podręcznej (Shard 0)|Liczba|Suma||
|getcommands0|Otrzymuje (Shard 0)|Liczba|Suma||
|setcommands0|Zestawy (Shard 0)|Liczba|Suma||
|evictedkeys0|Klucze usuniętego (Shard 0)|Liczba|Suma||
|totalkeys0|Wszystkie klucze (węzeł 0)|Liczba|Maksymalna||
|expiredkeys0|Wygasłe klawiszy (Shard 0)|Liczba|Suma||
|usedmemory0|Używane pamięci (Shard 0)|Bajtów|Maksymalna||
|usedmemoryRss0|Używane pamięci RSS (Shard 0)|Bajtów|Maksymalna||
|serverLoad0|Obciążenie serwera (Shard 0)|Procent|Maksymalna||
|cacheWrite0|Pamięć podręczna zapisu (Shard 0)|BytesPerSecond|Maksymalna||
|cacheRead0|Odczyt pamięci podręcznej (Shard 0)|BytesPerSecond|Maksymalna||
|percentProcessorTime0|Procesor (Shard 0)|Procent|Maksymalna||
|connectedclients1|Połączonych klientów (Shard 1)|Liczba|Maksymalna||
|totalcommandsprocessed1|Operacje razem (Shard 1)|Liczba|Suma||
|cachehits1|Trafienia w pamięci podręcznej (Shard 1)|Liczba|Suma||
|cachemisses1|Chybienia w pamięci podręcznej (Shard 1)|Liczba|Suma||
|getcommands1|Otrzymuje (Shard 1)|Liczba|Suma||
|setcommands1|Zestawy (Shard 1)|Liczba|Suma||
|evictedkeys1|Klucze usuniętego (Shard 1)|Liczba|Suma||
|totalkeys1|Wszystkie klucze (węzeł 1)|Liczba|Maksymalna||
|expiredkeys1|Wygasłe klawiszy (Shard 1)|Liczba|Suma||
|usedmemory1|Używane pamięci (Shard 1)|Bajtów|Maksymalna||
|usedmemoryRss1|Używane pamięci RSS (Shard 1)|Bajtów|Maksymalna||
|serverLoad1|Obciążenie serwera (Shard 1)|Procent|Maksymalna||
|cacheWrite1|Pamięć podręczna zapisu (Shard 1)|BytesPerSecond|Maksymalna||
|cacheRead1|Odczyt pamięci podręcznej (Shard 1)|BytesPerSecond|Maksymalna||
|percentProcessorTime1|Procesor (Shard 1)|Procent|Maksymalna||
|connectedclients2|Połączonych klientów (Shard 2)|Liczba|Maksymalna||
|totalcommandsprocessed2|Operacje razem (Shard 2)|Liczba|Suma||
|cachehits2|Trafienia w pamięci podręcznej (Shard 2)|Liczba|Suma||
|cachemisses2|Chybienia w pamięci podręcznej (Shard 2)|Liczba|Suma||
|getcommands2|Otrzymuje (Shard 2)|Liczba|Suma||
|setcommands2|Zestawy (Shard 2)|Liczba|Suma||
|evictedkeys2|Klucze usuniętego (Shard 2)|Liczba|Suma||
|totalkeys2|Wszystkie klucze (węzeł 2)|Liczba|Maksymalna||
|expiredkeys2|Wygasłe klawiszy (Shard 2)|Liczba|Suma||
|usedmemory2|Używane pamięci (Shard 2)|Bajtów|Maksymalna||
|usedmemoryRss2|Używane pamięci RSS (Shard 2)|Bajtów|Maksymalna||
|serverLoad2|Obciążenie serwera (Shard 2)|Procent|Maksymalna||
|cacheWrite2|Pamięć podręczna zapisu (Shard 2)|BytesPerSecond|Maksymalna||
|cacheRead2|Odczyt pamięci podręcznej (Shard 2)|BytesPerSecond|Maksymalna||
|percentProcessorTime2|Procesor (Shard 2)|Procent|Maksymalna||
|connectedclients3|Połączonych klientów (Shard 3)|Liczba|Maksymalna||
|totalcommandsprocessed3|Operacje razem (Shard 3)|Liczba|Suma||
|cachehits3|Trafienia w pamięci podręcznej (Shard 3)|Liczba|Suma||
|cachemisses3|Chybienia w pamięci podręcznej (Shard 3)|Liczba|Suma||
|getcommands3|Otrzymuje (Shard 3)|Liczba|Suma||
|setcommands3|Zestawy (Shard 3)|Liczba|Suma||
|evictedkeys3|Klucze usuniętego (Shard 3)|Liczba|Suma||
|totalkeys3|Wszystkie klucze (węzeł 3)|Liczba|Maksymalna||
|expiredkeys3|Wygasłe klawiszy (Shard 3)|Liczba|Suma||
|usedmemory3|Używane pamięci (Shard 3)|Bajtów|Maksymalna||
|usedmemoryRss3|Używane pamięci RSS (Shard 3)|Bajtów|Maksymalna||
|serverLoad3|Obciążenie serwera (Shard 3)|Procent|Maksymalna||
|cacheWrite3|Pamięć podręczna zapisu (Shard 3)|BytesPerSecond|Maksymalna||
|cacheRead3|Odczyt pamięci podręcznej (Shard 3)|BytesPerSecond|Maksymalna||
|percentProcessorTime3|Procesor (Shard 3)|Procent|Maksymalna||
|connectedclients4|Połączonych klientów (Shard 4)|Liczba|Maksymalna||
|totalcommandsprocessed4|Operacje razem (Shard 4)|Liczba|Suma||
|cachehits4|Trafienia w pamięci podręcznej (Shard 4)|Liczba|Suma||
|cachemisses4|Chybienia w pamięci podręcznej (Shard 4)|Liczba|Suma||
|getcommands4|Otrzymuje (Shard 4)|Liczba|Suma||
|setcommands4|Zestawy (Shard 4)|Liczba|Suma||
|evictedkeys4|Klucze usuniętego (Shard 4)|Liczba|Suma||
|totalkeys4|Wszystkie klucze (węzeł 4)|Liczba|Maksymalna||
|expiredkeys4|Wygasłe klawiszy (Shard 4)|Liczba|Suma||
|usedmemory4|Używane pamięci (Shard 4)|Bajtów|Maksymalna||
|usedmemoryRss4|Używane pamięci RSS (Shard 4)|Bajtów|Maksymalna||
|serverLoad4|Obciążenie serwera (Shard 4)|Procent|Maksymalna||
|cacheWrite4|Pamięć podręczna zapisu (Shard 4)|BytesPerSecond|Maksymalna||
|cacheRead4|Odczyt pamięci podręcznej (Shard 4)|BytesPerSecond|Maksymalna||
|percentProcessorTime4|Procesor (Shard 4)|Procent|Maksymalna||
|connectedclients5|Połączonych klientów (Shard 5)|Liczba|Maksymalna||
|totalcommandsprocessed5|Operacje razem (Shard 5)|Liczba|Suma||
|cachehits5|Trafienia w pamięci podręcznej (Shard 5)|Liczba|Suma||
|cachemisses5|Chybienia w pamięci podręcznej (Shard 5)|Liczba|Suma||
|getcommands5|Otrzymuje (Shard 5)|Liczba|Suma||
|setcommands5|Zestawy (Shard 5)|Liczba|Suma||
|evictedkeys5|Klucze usuniętego (Shard 5)|Liczba|Suma||
|totalkeys5|Wszystkie klucze (węzeł 5)|Liczba|Maksymalna||
|expiredkeys5|Wygasłe klawiszy (Shard 5)|Liczba|Suma||
|usedmemory5|Używane pamięci (Shard 5)|Bajtów|Maksymalna||
|usedmemoryRss5|Używane pamięci RSS (Shard 5)|Bajtów|Maksymalna||
|serverLoad5|Obciążenie serwera (Shard 5)|Procent|Maksymalna||
|cacheWrite5|Pamięć podręczna zapisu (Shard 5)|BytesPerSecond|Maksymalna||
|cacheRead5|Odczyt pamięci podręcznej (Shard 5)|BytesPerSecond|Maksymalna||
|percentProcessorTime5|Procesor (Shard 5)|Procent|Maksymalna||
|connectedclients6|Połączonych klientów (Shard 6)|Liczba|Maksymalna||
|totalcommandsprocessed6|Operacje razem (Shard 6)|Liczba|Suma||
|cachehits6|Trafienia w pamięci podręcznej (Shard 6)|Liczba|Suma||
|cachemisses6|Chybienia w pamięci podręcznej (Shard 6)|Liczba|Suma||
|getcommands6|Otrzymuje (Shard 6)|Liczba|Suma||
|setcommands6|Zestawy (Shard 6)|Liczba|Suma||
|evictedkeys6|Klucze usuniętego (Shard 6)|Liczba|Suma||
|totalkeys6|Wszystkie klucze (węzeł 6)|Liczba|Maksymalna||
|expiredkeys6|Wygasłe klawiszy (Shard 6)|Liczba|Suma||
|usedmemory6|Używane pamięci (Shard 6)|Bajtów|Maksymalna||
|usedmemoryRss6|Używane pamięci RSS (Shard 6)|Bajtów|Maksymalna||
|serverLoad6|Obciążenie serwera (Shard 6)|Procent|Maksymalna||
|cacheWrite6|Pamięć podręczna zapisu (Shard 6)|BytesPerSecond|Maksymalna||
|cacheRead6|Odczyt pamięci podręcznej (Shard 6)|BytesPerSecond|Maksymalna||
|percentProcessorTime6|Procesor (Shard 6)|Procent|Maksymalna||
|connectedclients7|Połączonych klientów (Shard 7)|Liczba|Maksymalna||
|totalcommandsprocessed7|Operacje razem (Shard 7)|Liczba|Suma||
|cachehits7|Trafienia w pamięci podręcznej (Shard 7)|Liczba|Suma||
|cachemisses7|Chybienia w pamięci podręcznej (Shard 7)|Liczba|Suma||
|getcommands7|Otrzymuje (Shard 7)|Liczba|Suma||
|setcommands7|Zestawy (Shard 7)|Liczba|Suma||
|evictedkeys7|Klucze usuniętego (Shard 7)|Liczba|Suma||
|totalkeys7|Wszystkie klucze (węzeł 7)|Liczba|Maksymalna||
|expiredkeys7|Wygasłe klawiszy (Shard 7)|Liczba|Suma||
|usedmemory7|Używane pamięci (Shard 7)|Bajtów|Maksymalna||
|usedmemoryRss7|Używane pamięci RSS (Shard 7)|Bajtów|Maksymalna||
|serverLoad7|Obciążenie serwera (Shard 7)|Procent|Maksymalna||
|cacheWrite7|Pamięć podręczna zapisu (Shard 7)|BytesPerSecond|Maksymalna||
|cacheRead7|Odczyt pamięci podręcznej (Shard 7)|BytesPerSecond|Maksymalna||
|percentProcessorTime7|Procesor (Shard 7)|Procent|Maksymalna||
|connectedclients8|Połączonych klientów (Shard 8)|Liczba|Maksymalna||
|totalcommandsprocessed8|Operacje razem (Shard 8)|Liczba|Suma||
|cachehits8|Trafienia w pamięci podręcznej (Shard 8)|Liczba|Suma||
|cachemisses8|Chybienia w pamięci podręcznej (Shard 8)|Liczba|Suma||
|getcommands8|Otrzymuje (Shard 8)|Liczba|Suma||
|setcommands8|Zestawy (Shard 8)|Liczba|Suma||
|evictedkeys8|Klucze usuniętego (Shard 8)|Liczba|Suma||
|totalkeys8|Wszystkie klucze (węzeł 8)|Liczba|Maksymalna||
|expiredkeys8|Wygasłe klawiszy (Shard 8)|Liczba|Suma||
|usedmemory8|Używane pamięci (Shard 8)|Bajtów|Maksymalna||
|usedmemoryRss8|Używane pamięci RSS (Shard 8)|Bajtów|Maksymalna||
|serverLoad8|Obciążenie serwera (Shard 8)|Procent|Maksymalna||
|cacheWrite8|Pamięć podręczna zapisu (Shard 8)|BytesPerSecond|Maksymalna||
|cacheRead8|Odczyt pamięci podręcznej (Shard 8)|BytesPerSecond|Maksymalna||
|percentProcessorTime8|Procesor (Shard 8)|Procent|Maksymalna||
|connectedclients9|Połączonych klientów (Shard 9)|Liczba|Maksymalna||
|totalcommandsprocessed9|Operacje razem (Shard 9)|Liczba|Suma||
|cachehits9|Trafienia w pamięci podręcznej (Shard 9)|Liczba|Suma||
|cachemisses9|Chybienia w pamięci podręcznej (Shard 9)|Liczba|Suma||
|getcommands9|Otrzymuje (Shard 9)|Liczba|Suma||
|setcommands9|Zestawy (Shard 9)|Liczba|Suma||
|evictedkeys9|Klucze usuniętego (Shard 9)|Liczba|Suma||
|totalkeys9|Wszystkie klucze (węzeł 9)|Liczba|Maksymalna||
|expiredkeys9|Wygasłe klawiszy (Shard 9)|Liczba|Suma||
|usedmemory9|Używane pamięci (Shard 9)|Bajtów|Maksymalna||
|usedmemoryRss9|Używane pamięci RSS (Shard 9)|Bajtów|Maksymalna||
|serverLoad9|Obciążenie serwera (Shard 9)|Procent|Maksymalna||
|cacheWrite9|Pamięć podręczna zapisu (Shard 9)|BytesPerSecond|Maksymalna||
|cacheRead9|Odczyt pamięci podręcznej (Shard 9)|BytesPerSecond|Maksymalna||
|percentProcessorTime9|Procesor (Shard 9)|Procent|Maksymalna||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|NumberOfCalls|Całkowita liczba interfejsu API|Liczba|Suma|Całkowita liczba interfejsu API.|
|NumberOfFailedCalls|Całkowita liczba interfejsu API nie powiodło się|Liczba|Suma|Całkowita liczba interfejsu API nie powiodło się.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|Wartość procentową Procesora|Wartość procentową Procesora|Procent|Średnia|Wartość procentową jednostek przydzielonych obliczeń, które są obecnie używane przez maszyny wirtualne|
|Sieci w|Sieci w|Bajtów|Suma|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|
|Sieci się|Sieci się|Bajtów|Suma|Liczbę bajtów na wszystkich interfejsów sieciowych przez maszyny wirtualne (ruchu wychodzącego)|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajtów|Suma|Całkowita liczba bajtów odczyt dysku podczas okresu monitorowania|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajtów|Suma|Całkowita liczba bajtów zapisanych na dysku podczas okresu monitorowania|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Odczyt dysku operacji i/o na SEKUNDĘ|
|Operacje zapisu dysku na sekundę|Operacje zapisu dysku na sekundę|CountPerSecond|Średnia|Zapisu na dysku operacji i/o na SEKUNDĘ|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|Wartość procentową Procesora|Wartość procentową Procesora|Procent|Średnia|Wartość procentową jednostek przydzielonych obliczeń, które są obecnie używane przez maszyny wirtualne|
|Sieci w|Sieci w|Bajtów|Suma|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|
|Sieci się|Sieci się|Bajtów|Suma|Liczbę bajtów na wszystkich interfejsów sieciowych przez maszyny wirtualne (ruchu wychodzącego)|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajtów|Suma|Całkowita liczba bajtów odczyt dysku podczas okresu monitorowania|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajtów|Suma|Całkowita liczba bajtów zapisanych na dysku podczas okresu monitorowania|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Odczyt dysku operacji i/o na SEKUNDĘ|
|Operacje zapisu dysku na sekundę|Operacje zapisu dysku na sekundę|CountPerSecond|Średnia|Zapisu na dysku operacji i/o na SEKUNDĘ|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|Wartość procentową Procesora|Wartość procentową Procesora|Procent|Średnia|Wartość procentową jednostek przydzielonych obliczeń, które są obecnie używane przez maszyny wirtualne|
|Sieci w|Sieci w|Bajtów|Suma|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|
|Sieci się|Sieci się|Bajtów|Suma|Liczbę bajtów na wszystkich interfejsów sieciowych przez maszyny wirtualne (ruchu wychodzącego)|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajtów|Suma|Całkowita liczba bajtów odczyt dysku podczas okresu monitorowania|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajtów|Suma|Całkowita liczba bajtów zapisanych na dysku podczas okresu monitorowania|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Odczyt dysku operacji i/o na SEKUNDĘ|
|Operacje zapisu dysku na sekundę|Operacje zapisu dysku na sekundę|CountPerSecond|Średnia|Zapisu na dysku operacji i/o na SEKUNDĘ|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrycznego próby wysłania wiadomości|Liczba|Suma|Próba numer urządzenia do chmury telemetrycznego wiadomości były wysyłane do Twojej Centrum IoT|
|d2c.telemetry.ingress.SUCCESS|Telemetrycznego wiadomości wysłanych|Liczba|Suma|Numer urządzenia do chmury telemetrycznego wiadomości pomyślnie wysłane do użytkownika Centrum IoT|
|c2d.Commands.egress.COMPLETE.SUCCESS|Polecenia wykonane|Liczba|Suma|Liczba chmury do poleceń urządzenia pomyślnie wykonano przez urządzenie|
|c2d.Commands.egress.Abandon.SUCCESS|Polecenia porzucone.|Liczba|Suma|Liczba chmury do poleceń urządzenia porzucone przez urządzenie|
|c2d.Commands.egress.Reject.SUCCESS|Polecenia odrzucone|Liczba|Suma|Liczba chmury do poleceń urządzenia odrzucona przez urządzenie|
|devices.totalDevices|Suma urządzeń|Liczba|Suma|Liczba urządzeń zarejestrowany w Twoim Centrum IoT|
|devices.connectedDevices.allProtocol|Podłączone urządzenia|Liczba|Suma|Liczba urządzeń podłączonych do Twoim Centrum IoT|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|INREQS|Przychodzących żądań|Liczba|Suma|Zdarzenia Centrum przychodzących wiadomości przepustowości dla nazw|
|SUCCREQ|Żądania pomyślnie|Liczba|Suma|Całkowita liczba żądań pomyślnie dla nazw|
|FAILREQ|Żądania nie powiodło się|Liczba|Suma|Całkowita liczba żądań nie powiodło się dla nazw|
|SVRBSY|Błędy zajęty serwera|Liczba|Suma|Błędy zajęty serwera nazw|
|INTERR|Błędy wewnętrznego serwera|Liczba|Suma|Błędy sumy wewnętrznego serwera nazw|
|MISCERR|Inne błędy|Liczba|Suma|Całkowita liczba żądań nie powiodło się dla nazw|
|INMSGS|Wiadomości przychodzących|Liczba|Suma|Całkowita liczba wiadomości przychodzących dla nazw|
|OUTMSGS|Wiadomości wychodzących|Liczba|Suma|Suma wychodzących wiadomości dla nazw|
|EHINMBS|Przychodzące bajty na sekundę|BytesPerSecond|Suma|Zdarzenia Centrum przychodzących wiadomości przepustowości dla nazw|
|EHOUTMBS|Wychodzące bajty na sekundę|BytesPerSecond|Suma|Suma wychodzących wiadomości dla nazw|
|EHABL|Archiwizowanie wiadomości zaległości|Liczba|Suma|Komunikaty archiwum Centrum w zaległości dla nazw|
|EHAMSGS|Archiwizowanie wiadomości|Liczba|Suma|Centrum zdarzeń zarchiwizowane wiadomości w obszarze nazw|
|EHAMBS|Archiwizowanie wiadomości przepustowość|BytesPerSecond|Suma|Zdarzenia Centrum wiadomości zarchiwizowane przepustowość w obszarze nazw|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|RunsStarted|Zostanie uruchomiony pracę|Liczba|Suma|Liczba przepływ pracy jest uruchamiany wprowadzenie.|
|RunsCompleted|Zostanie uruchomiony wykonane|Liczba|Suma|Liczba przepływ pracy jest uruchamiany złożonym.|
|RunsSucceeded|Zostanie uruchomiony zakończyła się pomyślnie|Liczba|Suma|Liczba przepływ pracy jest uruchamiany pomyślnie.|
|RunsFailed|Zostanie uruchomiony nie powiodła się|Liczba|Suma|Liczba przepływ pracy jest uruchamiany nie powiodło się.|
|RunsCancelled|Zostanie uruchomiony anulowana|Liczba|Suma|Liczba przepływ pracy jest uruchamiany anulowane.|
|RunLatency|Uruchamianie opóźnienie|Sekundy|Średnia|Opóźnienie ukończony przepływ pracy jest uruchamiany.|
|RunSuccessLatency|Uruchamianie opóźnienie sukcesu|Sekundy|Średnia|Czas oczekiwania pomyślnie przepływ pracy jest uruchamiany.|
|RunThrottledEvents|Uruchamianie ograniczonej zdarzenia|Liczba|Suma|Liczba akcji lub wyzwalacza ograniczenie zdarzeń.|
|RunFailurePercentage|Uruchamianie procent błąd|Procent|Suma|Procent przepływ pracy jest uruchamiany nie powiodło się.|
|ActionsStarted|Akcje pracę |Liczba|Suma|Liczba akcje przepływu pracy uruchomienia.|
|ActionsCompleted|Akcje wykonane |Liczba|Suma|Liczba akcje przepływu pracy ukończony.|
|ActionsSucceeded|Akcje zakończyła się pomyślnie |Liczba|Suma|Liczba akcje przepływu pracy zakończyła się pomyślnie.|
|ActionsFailed|Akcje nie powiodła się|Liczba|Suma|Liczba akcje przepływu pracy nie powiodło się.|
|ActionsSkipped|Akcje pominięte |Liczba|Suma|Liczba akcje przepływu pracy są pomijane.|
|ActionLatency|Opóźnienie akcji |Sekundy|Średnia|Opóźnienie akcje przepływu pracy ukończony.|
|ActionSuccessLatency|Opóźnienie sukcesu akcji |Sekundy|Średnia|Opóźnienie akcje pomyślnie przepływu pracy.|
|ActionThrottledEvents|Akcja ograniczenie zdarzenia|Liczba|Suma|Liczba akcji ograniczenie zdarzeń.|
|TriggersStarted|Wyzwalacze pracę |Liczba|Suma|Liczba przepływów pracy wyzwalaczy uruchomienia.|
|TriggersCompleted|Wyzwalacze wykonane |Liczba|Suma|Liczba wyzwalaczy przepływ pracy ukończony.|
|TriggersSucceeded|Wyzwalacze zakończyła się pomyślnie |Liczba|Suma|Liczba wyzwalaczy przepływu pracy zakończyła się pomyślnie.|
|TriggersFailed|Wyzwalacze nie powiodła się |Liczba|Suma|Liczba wyzwalaczy przepływu pracy nie powiodło się.|
|TriggersSkipped|Wyzwalacze pominięte|Liczba|Suma|Liczba wyzwalaczy przepływu pracy są pomijane.|
|TriggersFired|Wyzwalacze uruchamiany |Liczba|Suma|Liczba wyzwalaczy przepływ pracy jest uruchamiany.|
|TriggerLatency|Opóźnienie wyzwalacza |Sekundy|Średnia|Czas oczekiwania wyzwalaczy przepływ pracy ukończony.|
|TriggerFireLatency|Opóźnienie Fire wyzwalacza |Sekundy|Średnia|Czas oczekiwania uruchamiany wyzwalaczy przepływu pracy.|
|TriggerSuccessLatency|Opóźnienie sukcesu wyzwalacza |Sekundy|Średnia|Czas oczekiwania powiodło się wyzwalaczy przepływu pracy.|
|TriggerThrottledEvents|Ograniczenie wyzwalacza zdarzeń|Liczba|Suma|Liczba przepływów pracy wyzwalacza ograniczenie zdarzeń.|
|BillableActionExecutions|Wykonania akcji fakturowanego|Liczba|Suma|Liczba wprowadzenie wystawiona wykonania akcji przepływu pracy.|
|BillableTriggerExecutions|Wykonania fakturowanego wyzwalacza|Liczba|Suma|Liczba wprowadzenie wystawiona wykonania wyzwalacza przepływu pracy.|
|TotalBillableExecutions|Suma wykonania fakturowanego|Liczba|Suma|Liczba wprowadzenie wystawiona wykonania przepływu pracy.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|Przepustowość|Przepustowość|BytesPerSecond|Średnia||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|SearchLatency|Opóźnienie wyszukiwania|Sekundy|Średnia|Opóźnienie średnia wyszukiwania usługi wyszukiwania|
|SearchQueriesPerSecond|Kwerendy wyszukiwania na sekundę|CountPerSecond|Średnia|Kwerendy wyszukiwania na sekundę usługi wyszukiwania|
|ThrottledSearchQueriesPercentage|Procent kwerend wyszukiwania ograniczonej|Procent|Średnia|Procent kwerend wyszukiwania, które zostały ograniczenie usługi wyszukiwania|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|CPUXNS|Użycie Procesora na przestrzeń nazw|Procent|Maksymalna|Usługa bus premium nazw Procesora zastosowania metryki|
|WSXNS|Użycie rozmiar pamięci na przestrzeń nazw|Procent|Maksymalna|Usługa bus premium nazw pamięci zastosowania metryki|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|cpu_percent|Procent Procesora|Procent|Średnia|Procesor procent|
|physical_data_read_percent|Procent Jo danych|Procent|Średnia|Procent Jo danych|
|log_write_percent|Procent Jo dziennika|Procent|Średnia|Procent Jo dziennika|
|dtu_consumption_percent|Procent DTU|Procent|Średnia|Procent DTU|
|miejsca do magazynowania|Całkowity rozmiar bazy danych|Bajtów|Maksymalna|Całkowity rozmiar bazy danych|
|connection_successful|Pomyślnego połączenia|Liczba|Suma|Pomyślnego połączenia|
|connection_failed|Połączenia nie powiodło się|Liczba|Suma|Połączenia nie powiodło się|
|blocked_by_firewall|Zablokowane przez zaporę|Liczba|Suma|Zablokowane przez zaporę|
|Zakleszczenie|Zakleszczenia|Liczba|Suma|Zakleszczenia|
|storage_percent|Procent rozmiaru bazy danych|Procent|Maksymalna|Procent rozmiaru bazy danych|
|xtp_storage_percent|Percent(Preview) miejsca do magazynowania OLTP w pamięci|Procent|Średnia|Percent(Preview) miejsca do magazynowania OLTP w pamięci|
|workers_percent|Procent pracowników|Procent|Średnia|Procent pracowników|
|sessions_percent|Sesje procent|Procent|Średnia|Sesje procent|
|dtu_limit|DTU limit|Liczba|Średnia|DTU limit|
|dtu_used|DTU używane|Liczba|Średnia|DTU używane|
|service_level_objective|Cel poziomu usług bazy danych|Liczba|Suma|Cel poziomu usług bazy danych|
|dwu_limit|dwu limit|Liczba|Maksymalna|dwu limit|
|dwu_consumption_percent|Procent DWU|Procent|Średnia|Procent DWU|
|dwu_used|DWU używane|Liczba|Średnia|DWU używane|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|cpu_percent|Procesor procent|Procent|Średnia|Procesor procent|
|physical_data_read_percent|Procent Jo danych|Procent|Średnia|Procent Jo danych|
|log_write_percent|Procent Jo dziennika|Procent|Średnia|Procent Jo dziennika|
|dtu_consumption_percent|Procent DTU|Procent|Średnia|Procent DTU|
|storage_percent|Procent miejsca do magazynowania|Procent|Średnia|Procent miejsca do magazynowania|
|workers_percent|Procent pracowników|Procent|Średnia|Procent pracowników|
|sessions_percent|Sesje procent|Procent|Średnia|Sesje procent|
|eDTU_limit|eDTU limit|Liczba|Średnia|eDTU limit|
|storage_limit|Limit miejsca do magazynowania|Bajtów|Średnia|Limit miejsca do magazynowania|
|eDTU_used|eDTU używane|Liczba|Średnia|eDTU używane|
|storage_used|Używane miejsca do magazynowania|Bajtów|Średnia|Używane miejsca do magazynowania|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|ResourceUtilization|SU % wykorzystania|Procent|Maksymalna|SU % wykorzystania|
|InputEvents|Zdarzenia wprowadzania danych|Liczba|Suma|Zdarzenia wprowadzania danych|
|InputEventBytes|Zdarzenie wprowadzania bajtów|Bajtów|Suma|Zdarzenie wprowadzania bajtów|
|LateInputEvents|Opóźnione zdarzeń wprowadzania danych|Liczba|Suma|Opóźnione zdarzeń wprowadzania danych|
|OutputEvents|Dane wyjściowe zdarzenia|Liczba|Suma|Dane wyjściowe zdarzenia|
|ConversionErrors|Błędów konwersji danych|Liczba|Suma|Błędów konwersji danych|
|Błędy|Błędy wykonania|Liczba|Suma|Błędy wykonania|
|DroppedOrAdjustedEvents|Kolejność zdarzeń|Liczba|Suma|Kolejność zdarzeń|
|AMLCalloutRequests|Funkcja żądania|Liczba|Suma|Funkcja żądania|
|AMLCalloutFailedRequests|Żądania funkcji nie powiodło się|Liczba|Suma|Żądania funkcji nie powiodło się|
|AMLCalloutInputEvents|Funkcja zdarzenia|Liczba|Suma|Funkcja zdarzenia|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|CpuPercentage|Procesor procent|Procent|Średnia|Procesor procent|
|MemoryPercentage|Procent ilości pamięci|Procent|Średnia|Procent ilości pamięci|
|DiskQueueLength|Długość kolejki dysku|Liczba|Suma|Długość kolejki dysku|
|HttpQueueLength|Długość kolejki http|Liczba|Suma|Długość kolejki http|
|Właściwość BytesReceived|Dane w|Bajtów|Suma|Dane w|
|Żądania|Danych|Bajtów|Suma|Danych|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|CpuTime|Czas Procesora|Sekundy|Suma|Czas Procesora|
|Żądania|Żądania|Liczba|Suma|Żądania|
|Właściwość BytesReceived|Dane w|Bajtów|Suma|Dane w|
|Żądania|Danych|Bajtów|Suma|Danych|
|Http2xx|Http 2xx|Liczba|Suma|Http 2xx|
|Http3xx|Http 3xx|Liczba|Suma|Http 3xx|
|Http401|HTTP 401|Liczba|Suma|HTTP 401|
|Http403|HTTP 403|Liczba|Suma|HTTP 403|
|Http404|HTTP 404|Liczba|Suma|HTTP 404|
|Http406|HTTP 406|Liczba|Suma|HTTP 406|
|Http4xx|Http 4xx|Liczba|Suma|Http 4xx|
|Http5xx|Błędy serwera HTTP|Liczba|Suma|Błędy serwera HTTP|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajtów|Suma|Zestaw roboczy pamięci|
|AverageMemoryWorkingSet|Średnia pamięć zestaw roboczy|Bajtów|Średnia|Średnia pamięć zestaw roboczy|
|AverageResponseTime|Średni czas reakcji|Sekundy|Średnia|Średni czas reakcji|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metryka|Nazwa wyświetlana metryczne|Jednostki|Typ agregacji|Opis|
|---|---|---|---|---|
|CpuTime|Czas Procesora|Sekundy|Suma|Czas Procesora|
|Żądania|Żądania|Liczba|Suma|Żądania|
|Właściwość BytesReceived|Dane w|Bajtów|Suma|Dane w|
|Żądania|Danych|Bajtów|Suma|Danych|
|Http2xx|Http 2xx|Liczba|Suma|Http 2xx|
|Http3xx|Http 3xx|Liczba|Suma|Http 3xx|
|Http401|HTTP 401|Liczba|Suma|HTTP 401|
|Http403|HTTP 403|Liczba|Suma|HTTP 403|
|Http404|HTTP 404|Liczba|Suma|HTTP 404|
|Http406|HTTP 406|Liczba|Suma|HTTP 406|
|Http4xx|Http 4xx|Liczba|Suma|Http 4xx|
|Http5xx|Błędy serwera HTTP|Liczba|Suma|Błędy serwera HTTP|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajtów|Suma|Zestaw roboczy pamięci|
|AverageMemoryWorkingSet|Średnia pamięć zestaw roboczy|Bajtów|Średnia|Średnia pamięć zestaw roboczy|
|AverageResponseTime|Średni czas reakcji|Sekundy|Średnia|Średni czas reakcji|

## <a name="next-steps"></a>Następne kroki

- [Przeczytaj o metryki w monitorze Azure](./monitoring-overview.md#monitoring-sources)
- [Tworzenie alertów na metryki](./insights-receive-alert-notifications.md)
- [Eksportowanie metryk przestrzeni dyskowej, Centrum zdarzeń i analizy dziennika](./monitoring-overview-of-diagnostic-logs.md)
