<properties
 pageTitle="Operacje IoT centrum monitorowania"
 description="Przegląd operacji Azure IoT centrum monitorowania, umożliwiając monitorować stan operacji na Twoim Centrum IoT w czasie rzeczywistym"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Wprowadzenie do operacji monitorowania

Operacje IoT centrum monitorowania umożliwia monitorowanie stanu operacji na Twoim Centrum IoT w czasie rzeczywistym. Centrum IoT śledzi zdarzenia w kilku kategoriach operacji i możesz wybrać opcję do wysyłania zdarzeń z jednej lub wielu kategorii do punktu końcowego usługi IoT centrum przetwarzania. Można monitorować dane pod kątem błędów lub skonfigurować przetwarzanie bardziej złożone na podstawie wzorców danych.

Centrum IoT monitoruje pięć kategorii zdarzeń:

- Operacje tożsamości urządzenia
- Telemetrycznego urządzenia
- Polecenia w chmurze do urządzenia
- Połączenia
- Wysyłanie plików

## <a name="how-to-enable-operations-monitoring"></a>Jak włączyć operacje monitorowania

1. Tworzenie Centrum IoT. Może znaleźć instrukcje na temat tworzenia Centrum IoT w [Wprowadzenie] [ lnk-get-started] przewodnika.

2. Otwórz karta z Twoim Centrum IoT. W tym miejscu kliknij **monitorowania operacji**.

    ![][1]

3. Wybierz kategorie monitorowania, które chcesz monitorować, a następnie kliknij przycisk **Zapisz**. Zdarzenia są dostępne podczas czytania od punktu końcowego zgodnego z programem zdarzenia Centrum wyszczególnione w **ustawieniach monitorowania**. Punkt końcowy Centrum IoT nosi nazwę `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Kategorie zdarzeń i sposobach ich używania

Każdy operacje monitorowania ścieżki kategorii innego typu interakcji z Centrum IoT i każdej kategorii monitorowania ma schemat definiujące struktury zdarzeń z danej kategorii.

### <a name="device-identity-operations"></a>Operacje tożsamości urządzenia

Kategoria operacje tożsamości urządzenia śledzi błędy występujące podczas próby tworzenie, aktualizowanie i usuwanie wpisu w Twoim Centrum IoT tożsamości rejestru. Śledzenie tej kategorii są przydatne w przypadku scenariuszy inicjowania obsługi administracyjnej.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Telemetrycznego urządzenia

Kategoria telemetrycznego urządzenia śledzi błędy, które występują w Centrum IoT i dotyczą proces telemetrycznego. Ta kategoria zawiera błędy występujące podczas wysyłania zdarzeń telemetrycznego (na przykład ograniczania) oraz odbiór zdarzeń telemetrycznego (na przykład nieautoryzowanego czytnika). Należy zauważyć, że ta kategoria nie może przechwycić błędy spowodowane przez kodu uruchomionego na urządzeniu.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Polecenia w chmurze do urządzenia

Kategoria polecenia chmury do urządzenia śledzi błędy, które występują w Centrum IoT i są związane z potoku polecenia urządzenia. Ta kategoria zawiera błędy występujące podczas wysyłania poleceń (na przykład nieautoryzowanego nadawcy), otrzymuje poleceń (na przykład dostarczania liczby) i odbierania polecenie opinii, (takie jak opinii wygasł). Ta kategoria nie może przechwytywać błędy z urządzenia obsługującego nieprawidłowo polecenia, jeśli polecenie został pomyślnie dostarczony.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Połączenia

Kategoria połączenia śledzi błędy występujące podczas urządzeń Podłączanie lub odłączanie z Centrum IoT. Śledzenie tej kategorii jest przydatne do identyfikowania próby nieautoryzowanego połączenia i śledzenie, gdy połączenie zostanie przerwane dla urządzeń w zakresie łączności niskiej.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Wysyłanie plików

Kategoria przekazywania pliku śledzi błędy, które występują w Centrum IoT i dotyczą funkcjonalność przekazywania plików. Ta kategoria zawiera błędy występujące przy użyciu identyfikatora URI skojarzeń zabezpieczeń (na przykład po jej wygaśnięciu przed urządzeniu powiadamia koncentratora przekazywanie zostało ukończone), przekazywania zakończonego niepowodzeniem zgłoszone przez urządzenie, a gdy plik nie znajduje się w magazynie podczas tworzenia wiadomości powiadomień Centrum IoT. Należy zauważyć, że ta kategoria nie może przechwycić błędy występujące bezpośrednio podczas urządzenie przekazywania pliku do magazynu.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Następne kroki

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md