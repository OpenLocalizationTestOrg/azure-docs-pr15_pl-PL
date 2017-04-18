<properties
 pageTitle="Monitorowanie zdalnego wstępnie Instruktaż rozwiązanie | Microsoft Azure"
 description="Opis Azure IoT wstępnie rozwiązanie zdalnego monitorowania i jego architektury."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Zdalne monitorowanie wstępnie Instruktaż rozwiązanie

## <a name="introduction"></a>Wprowadzenie

Zdalnego pakietu IoT monitorowania [wstępnie rozwiązanie] [ lnk-preconfigured-solutions] implementacja zakończenia do końca monitoruje rozwiązanie w przypadku wielu komputerów z systemem w lokalizacjach zdalnych. Rozwiązanie łączy kluczowe usługi Azure zapewnienie ogólnego stosowania scenariusza biznesowego i można użyć jako punktu startowego dla własnego wdrożenia. Możesz [dostosować] [ lnk-customize] rozwiązanie zgodnie z potrzebami firmy.

W tym artykule opisano niektóre z najważniejszych elementów zdalnego monitorowania rozwiązanie ułatwia zrozumieć, jak to działa. Tej wiedzy ułatwia:

- Rozwiązywanie problemów w rozwiązaniu.
- Planowanie dostosowywania rozwiązanie do własnych określonych wymagań. 
- Projektowanie własnej rozwiązanie IoT, która korzysta z usługi Azure.

## <a name="logical-architecture"></a>Architektura logicznych.

Poniższy diagram przedstawia logiczne składniki wstępnie rozwiązania:

![Architektura logicznych.](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Symulowany urządzeń

W rozwiązaniu wstępnie symulowany urządzenie reprezentuje chłodzenia urządzenia (na przykład klimatyzacyjne budynku lub pomieszczenia jednostkę obsługi air). Po wdrożeniu wstępnie rozwiązanie również automatycznie dodawać czterech symulowany urządzeń, które są uruchamiane w [Azure WebJob][lnk-webjobs]. Symulowany urządzeń ułatwiają Eksplorowanie zachowanie rozwiązanie bez konieczności wdrażania dowolnego fizycznymi. Aby wdrożyć urządzenia rzeczywistego fizycznego, zobacz [Łączenie urządzenia do komputera zdalnego, monitorowanie wstępnie rozwiązanie] [ lnk-connect-rm] samouczka.

Każde urządzenie symulowany można wysłać następujące typy wiadomości do koncentratora IoT:

| Komunikat  | Opis |
|----------|-------------|
| Uruchamianie  | Po uruchomieniu urządzenia wysyła wiadomość **informacje o urządzeniach** zawierającą informacje o sobie w celu przechodzenia wstecz. Dane zawierają identyfikator urządzenia, metadane urządzeń, listę poleceń obsługuje urządzenia i bieżącej konfiguracji urządzenia. |
| Obecności | Urządzenie okresowo wysyła wiadomość **obecności** raportowania, czy urządzenia można ustawić obecności czujnika. |
| Telemetrycznego | Urządzenie okresowo wysyła wiadomość **telemetrycznego** raportów symulowany wartości temperatury i wilgotności zebrane z urządzenia symulowany czujników. |


Symulowany urządzeń wysłać następujące właściwości urządzenia w wiadomości **informacje o urządzeniach** :

| Właściwość               |  Cel |
|------------------------|--------- |
| Identyfikator urządzenia              | Identyfikator, która jest zawarte lub przydzielone, gdy urządzenie jest tworzona w rozwiązaniu. |
| Producenta           | Producenta urządzenia |
| Numer modelu           | Numer modelu urządzenia |
| Numer seryjny          | Numer seryjny urządzenia |
| Sprzętowego               | Bieżącą wersję oprogramowania układowego na tym urządzeniu |
| Platformy               | Architektura platformy urządzenia |
| Procesor              | Procesor uruchomione na urządzeniu |
| Zainstalowana pamięć RAM          | Ilość pamięci RAM zainstalowanej na tym urządzeniu |
| Centrum włączony stan      | Właściwość Stan Centrum IoT urządzenia |
| Godzina utworzenia           | Godzina utworzenia urządzenia w rozwiązaniu |
| Godziną aktualizacji           | Godzina ostatniej właściwości zostały zaktualizowane urządzenia |
| Szerokości               | Lokalizacja szerokości urządzenia |
| Długość geograficzna              | Długość geograficzna lokalizacja urządzenia |

Simulator nasion tych właściwości symulowany urządzenia z przykładowymi wartościami.  Za każdym razem simulator inicjuje urządzenie symulowany urządzenie wpisy wstępnie zdefiniowanych metadanych do koncentratora IoT. Należy pamiętać o tym, jak to zastępuje metadanych aktualizacje wprowadzone w portalu urządzenia.


Symulowany urządzenia mogą obsługiwać następujące polecenia wysyłane na pulpicie nawigacyjnym rozwiązanie za pośrednictwem Centrum IoT:

| Polecenie                | Opis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Wysyła _ping_ na urządzeniu, aby sprawdzić, czy jest aktywna   |
| StartTelemetry         | Pozwala uruchomić urządzenie wysyłające telemetrycznego                 |
| StopTelemetry          | Przestaje urządzenia wysyłania telemetrycznego             |
| ChangeSetPointTemp     | Zmienia wartość Ustaw punkt, wokół których jest generowany losowo danych |
| DiagnosticTelemetry    | Uaktywnia simulator urządzenia, aby wysłać wartość dodatkowe telemetrycznego (externalTemp) |
| ChangeDeviceState      | Zmienia właściwość rozszerzonym stanie urządzenia i wysyła wiadomości informacje urządzenia z urządzenia |

Potwierdzanie polecenie urządzenia w celu rozwiązania wstecz są dostarczane za pośrednictwem Centrum IoT.

## <a name="iot-hub"></a>Centrum IoT

[Centrum IoT] [ lnk-iothub] ingests dane wysyłane z urządzenia w chmurze i udostępnia go do zadań Azure analizy strumieniu (ASA). Centrum IoT również wysyła polecenia do urządzenia imieniu portalu urządzenia. Każde zadanie ASA strumienia używa osobnej grupie dla klientów indywidualnych Centrum IoT odczytać strumienia wiadomości na swoich urządzeniach.

## <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Zdalny rozwiązania monitorowania [Azure analizy strumieniu] [ lnk-asa] (ASA) wysyła wiadomości urządzenia odebrana przez Centrum IoT do innych składników wewnętrznej w celu przetworzenia lub miejsca do magazynowania. Różne zadania ASA niektóre zadania na podstawie zawartości wiadomości.

**Zadanie 1: informacje o urządzeniach** filtruje wiadomości informacji urządzenia ze strumienia wiadomości przychodzące i wysyła je do punktu końcowego Centrum zdarzenia. Urządzenie wysyła komunikaty informacji podczas uruchamiania i w odpowiedzi na polecenie **SendDeviceInfo** . To zadanie jest używane następujące definicji zapytania do identyfikowania wiadomości **informacje o urządzeniach** :

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

To zadanie wysyła dane wyjściowe do koncentratora zdarzenia potrzeby dalszego przetwarzania.

**Zadanie 2: reguły** oblicza przychodzących temperatury i wilgotności telemetrycznego wartości progi na urządzenie. Wartości progowe są ustawiane w edytorze reguł dostępnych na pulpicie nawigacyjnym rozwiązanie. Każdej pary urządzenia i wartości są przechowywane przez sygnatury obiektów blob, który analizy strumieniu brzmi w jako **Danych źródłowych**. Zadanie porównuje wartości niepustych przed określony próg urządzenia. Jeśli przekracza ">" warunku, zadanie Wyświetla zdarzenia **alarmu** , która wskazuje, że próg zostanie przekroczony, a także urządzenia, wartości i wartości sygnatury czasowej. To zadanie jest używane następujące definicji zapytania do identyfikowania telemetrycznego wiadomości, które powinno spowodować alarmu:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

To zadanie wysyła dane wyjściowe do koncentratora zdarzenia w celu dalszego przetwarzania i zapisuje szczegóły poszczególnych alertów magazynem obiektów blob w miejsce, w którym rozwiązanie pulpitu nawigacyjnego można przeczytać informacje alertu.

**Zadanie 3: telemetrycznego** działa na przychodzące strumienia telemetrycznego urządzenie na dwa sposoby. Pierwszy wysyła wszystkich wiadomości telemetrycznego z urządzenia magazynem obiektów blob trwałych dla długotrwałego przechowywania. Drugi oblicza wartości średniej, minimalną i maksymalną wilgotność w oknie przesuwania 5 minut i wysyła te dane do magazyn obiektów blob. Pulpit nawigacyjny rozwiązanie odczytuje dane telemetryczne z magazynem obiektów blob do wypełniania wykresów. To zadanie jest używane następujące definicji zapytania:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Koncentratory zdarzenia

Zadania ASA **reguł** i **informacje o urządzeniach** wyjściowy swoje dane z koncentratorów zdarzenia niezawodne przesyłanie dalej było **Procesor zdarzeń** działa w WebJob.

## <a name="azure-storage"></a>Azure miejsca do magazynowania

Rozwiązanie używa magazynem obiektów blob Azure, aby zachować wszystkie dane telemetrycznego nieprzetworzonych i podsumowane z urządzeń w rozwiązaniu. Pulpit nawigacyjny odczytuje dane telemetryczne z magazynem obiektów blob do wypełniania wykresów. Do wyświetlania alertów, pulpitu nawigacyjnego odczytuje dane z magazynem obiektów blob tego rekordów przekroczenia wartości telemetrycznego wartości progowe skonfigurowane. Rozwiązanie używa też magazyn obiektów blob do rejestrowania wartości progowe ustawione na pulpicie nawigacyjnym.

## <a name="webjobs"></a>WebJobs

Oprócz hostingu symulatorów urządzenia WebJobs w rozwiązaniu obsługiwać **Zdarzenia procesora** działa WebJob Azure ten uchwyty urządzenia informacji wiadomości i odpowiedzi polecenia. Używa:

- Urządzenie wiadomości informacji zaktualizować rejestru urządzenia (przechowywane w bazie danych DocumentDB) za pomocą bieżących informacji urządzenia.
- Polecenie wiadomości odpowiedzi do aktualizacji historii polecenia urządzenia (przechowywane w bazie danych DocumentDB).

## <a name="documentdb"></a>DocumentDB

Rozwiązanie używa DocumentDB bazy danych do przechowywania informacji na temat urządzeń podłączonych do rozwiązania. Te informacje obejmują metadane urządzeń i historię polecenia wysyłane do urządzenia z pulpitu nawigacyjnego.

## <a name="web-apps"></a>Aplikacje sieci Web

### <a name="remote-monitoring-dashboard"></a>Zdalny monitorowania pulpitu nawigacyjnego
Tę stronę w aplikacji sieci web są używane kontrolki języka javascript PowerBI (zobacz [Efekty wizualne PowerBI repo](https://www.github.com/Microsoft/PowerBI-visuals)) wizualizowanie danych telemetrycznych z urządzenia. Rozwiązanie korzystająca z zadania telemetrycznego ASA zapisane telemetrycznego blob miejsca do magazynowania.


### <a name="device-administration-portal"></a>Portalu administracyjnego urządzenia

Ta aplikacja sieci web umożliwia:

- Dostarczenie nowego urządzenia. Ta akcja określa unikatowy identyfikator i generuje klucz uwierzytelniania. Zapisuje informacje dotyczące urządzenia do Centrum IoT rejestru tożsamości i bazy danych DocumentDB określonego rozwiązania.
- Zarządzanie właściwościami urządzenia. Ta akcja zawiera właściwości istniejącego wyświetlania i aktualizowania nowe właściwości.
- Polecenia Wyślij do urządzenia.
- Wyświetlanie historii poleceń dla urządzenia.
- Włączanie i wyłączanie urządzenia.

## <a name="next-steps"></a>Następne kroki

Następujące wpisy blogu TechNet zawierają szczegółowe informacje o zdalnego monitorowania wstępnie rozwiązanie:

- [Monitorowanie zdalne Suite — w obszarze Ustawienia zaawansowane - IoT](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [Pakiet IoT - zdalnego monitorowania — dodawanie urządzeń symulowany i na żywo](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Aby kontynuować, wprowadzenie do pakietu IoT, czytając w następujących artykułach:

- [Podłącz urządzenie zdalnego rozwiązanie, wstępnie skonfigurowanego monitorowania][lnk-connect-rm]
- [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md