<properties
 pageTitle="Przewodnik dewelopera — zadania | Microsoft Azure"
 description="Azure IoT Centrum deweloperów przewodniku — Planowanie zadań do wykonania w wielu urządzeń podłączone do koncentratora usługi"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Planowanie zadań na wielu urządzeniach (wersja preview)

## <a name="overview"></a>Omówienie

Zgodnie z opisem w poprzednich artykułach, Centrum IoT Azure udostępnia wiele bloków konstrukcyjnych ([Właściwości podwójnego urządzenia i znaczniki] [ lnk-twin-devguide] i [metody chmury do urządzenia][lnk-dev-methods]).  Zazwyczaj IoT wewnętrznej aplikacje Włącz urządzenie administratorów i operatorów, aktualizować oraz korzystać z urządzeniami IoT zbiorczo i czasie według harmonogramu.  Zadania umieszczać wykonanie aktualizacje podwójnego urządzeń i metod C2D z zestawem urządzenia w momencie harmonogramu.  Na przykład operator należy użyć aplikacji wewnętrznej, która będzie inicjowania i śledzić zadanie, aby ponownie uruchomić zestaw urządzeń w budynku 43 i podłoga 3 w czasie, które nie będą one operacje budynku.

### <a name="when-to-use"></a>Kiedy należy używać

Należy rozważyć, czy za pomocą zadaniach przy: kopii rozwiązanie zakończyć potrzeb planowanie i śledzenie postępu dowolną z następujących czynności w zestawie urządzenia:

- Aktualizowanie właściwości podwójnego potrzeby urządzenia
- Aktualizowanie urządzenia podwójnego znaczników
- Wywoływanie metody C2D

## <a name="job-lifecycle"></a>Cykl życia zadania

Zadania są zainicjowane przez rozwiązanie wewnętrznej i obsługiwanych przez Centrum IoT.  Można zainicjować zadania za pomocą identyfikatora URI usługi skierowaną (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) i zapytanie dotyczące postępu na wykonywanie zadań za pomocą identyfikatora URI usługi skierowaną (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Po zainicjowaniu zadanie wyszukiwanie zadań umożliwi aplikacjom wewnętrznej Odśwież stan wykonywania zadań.

> [AZURE.NOTE] Po rozpoczęciu pracy nazwy i wartości właściwości może zawierać tylko US-ASCII drukowania alfanumeryczne, z wyjątkiem w następujący zestaw: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji o używaniu zadania.

## <a name="jobs-to-execute-direct-methods"></a>Zadania do wykonania metody bezpośredni

Oto informacje dotyczące żądania HTTP 1.1 wykonywania [Metoda bezpośrednia] [ lnk-dev-methods] zestawu urządzeń przy użyciu zadania:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Zadania, aby zaktualizować właściwości podwójnego urządzenia

Poniżej przedstawiono informacje dotyczące żądania HTTP 1.1 aktualizowania właściwości podwójnego urządzenia przy użyciu zadania:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Wyszukiwanie informacji o postępie zadań

Oto informacje dotyczące żądania HTTP 1.1 dla [kwerend dla zadań][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
ContinuationToken znajduje się na odpowiedź.  

## <a name="jobs-properties"></a>Właściwości zadań

Poniżej przedstawiono listę właściwości i ich opisy, których można używać podczas wykonywania kwerend dla zadania lub zadania wyników.

| Właściwość | Opis |
| -------------- | -----------------|
| **jobId** | Stosowanie przewidziane identyfikator zadania. |
| **czas rozpoczęcia** | Aplikacja udostępniana czas rozpoczęcia (ISO 8601) dla zadania. |
| **Zakończenia** | Centrum IoT udostępniana daty (ISO 8601) do ukończenia zadania. Prawidłowe tylko wtedy, gdy zadanie osiągnie stan "ukończony". | 
| **Typ** | Typy zadań: |
| | **scheduledUpdateTwin**: zadanie umożliwia aktualizowanie zestaw właściwości podwójnego potrzeby lub znaczników. |
| | **scheduledDeviceMethod**: zadanie umożliwia wywoływanie metody urządzenia dla zestawu podwójnego. |
| **Stan** | Bieżący stan zadania. Możliwe wartości stanu: |
| | **Oczekiwanie** : planowane i oczekiwanie do pobrania przez usługę zadania. |
| | **Zaplanowane** : według harmonogramu czas w przyszłości. |
| | **Uruchamianie** : aktywne zadania. |
| | **Anulowane** : zadanie zostało anulowane. |
| | **nie powiodło się** : nie można wykonać zadania. |
| | **ukończony** : zadanie zostało ukończone. |
| **deviceJobStatistics** | Statystyki dotyczące wykonywania tego zadania. |

W wersji preview obiekt deviceJobStatistics jest dostępna tylko wtedy, gdy ukończenia zadania.

| Właściwość | Opis |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Liczba urządzeń w zadaniu. |
| **deviceJobStatistics.failedCount** | Liczba urządzeń, gdy zadanie powiodło się. |
| **deviceJobStatistics.succeededCount** | Liczba urządzeń, gdy zadanie zakończyło się pomyślnie. |
| **deviceJobStatistics.runningCount** | Liczba urządzeń, które są obecnie uruchomione zadania. |
| **deviceJobStatistics.pendingCount** | Liczba urządzeń oczekujących w celu uruchomienia zadania. |


### <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] wyświetla różne języka SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT Centrum MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz wypróbować niektóre pojęcia opisane w tym artykule, może być w następujących samouczek koncentratora IoT:

- [Planowanie i emisji zadania][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
