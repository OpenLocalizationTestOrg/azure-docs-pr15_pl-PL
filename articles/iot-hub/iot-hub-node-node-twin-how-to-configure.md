<properties
    pageTitle="Używanie właściwości podwójnego | Microsoft Azure"
    description="W tym samouczku pokazano sposób używania właściwości podwójnego"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Samouczek: Konfigurowanie urządzenia (wersja preview) przy użyciu odpowiedniej właściwości

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na końcu tego samouczka masz dwie aplikacji konsoli Node.js:

* **SimulateDeviceConfiguration.js**, aplikację symulacji, która oczekuje aktualizacji Konfiguracja docelowa i raporty o stanie procesu aktualizacji symulowany konfiguracji.
* **SetDesiredConfigurationAndQuery.js**, aplikacja Node.js ma być uruchamiane wewnętrznej, który ustawia odpowiednią konfigurację na urządzeniu i kwerendy proces aktualizacji konfiguracji.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, w której można tworzyć zarówno urządzenia i wewnętrznej aplikacje.

Aby użyć tego samouczka są potrzebne następujące elementy:

+ Wersja node.js 0.10.x lub nowszym.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

Jeśli zostały wykonane [wprowadzenie urządzenia twins] [ lnk-twin-tutorial] samouczek, masz już koncentratora Zarządzanie włączone urządzenia i urządzenia tożsamości o nazwie **myDeviceId**; i można przejść do [tworzenia aplikacji symulacji] [ lnk-how-to-configure-createapp] sekcji.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która łączy się z Centrum jako **myDeviceId**, czeka aktualizacji Konfiguracja docelowa, a następnie przedstawia aktualizacji w procesie aktualizacji symulowany konfiguracji.

1. Utwórz nowy pusty folder o nazwie **simulatedeviceconfiguration**. W folderze **simulatedeviceconfiguration** Utwórz nowy plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **simulatedeviceconfiguration** uruchom następujące polecenie, aby zainstalować **azure iot urządzenia**, a **urządzenie mqtt, azure iot w —** pakiet:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **SimulateDeviceConfiguration.js** w folderze **simulatedeviceconfiguration** .

4. Dodaj następujący kod do pliku **SimulateDeviceConfiguration.js** i zastąpić symbol zastępczy **{Parametry połączenia urządzenia}** przy użyciu parametrów połączenia, skopiowany po utworzeniu **myDeviceId** urządzenia tożsamości:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Obiekt **klienta** udostępnia wszystkie metody, które są wymagane do współdziałania z twins urządzenia z urządzenia. Poprzedniego kodu, po jej inicjuje obiekt **klienta** pobiera podwójnego dla **myDeviceId**i dołącza obsługi aktualizacji na odpowiednie właściwości. Program obsługi sprawdza, że nie jest żądania zmiany rzeczywista konfiguracja przez porównanie configIds, a następnie wywołuje metodę, rozpoczynająca się zmiany konfiguracji.

    Należy zauważyć, że dla uproszczenia, poprzedniego kodu używa domyślnie ustalonej dla początkowej konfiguracji. Liczba rzeczywista aplikacji prawdopodobnie będzie Załaduj tej konfiguracji z magazynu lokalnego.
    
    > [AZURE.IMPORTANT] Zdarzenia zmiany żądaną właściwość zawsze emisję raz na połączenia urządzenia, pamiętaj sprawdzić, czy jest rzeczywistą zmianę we właściwościach odpowiedniej przed wykonaniem żadnych działań.

5. Dodawanie poniższych metod przed `client.open()` wywołania:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Metoda **initConfigChange** aktualizuje zgłoszoną właściwości obiektu podwójnego lokalne z żądania aktualizacji konfiguracji ustawienie stanu na **Oczekiwanie na**, a następnie zaktualizuje podwójnego urządzenia w usłudze. Po pomyślnym zaktualizowaniu podwójnego, jego symuluje długotrwałych procesu kończącego w realizacji **completeConfigChange**. Ta metoda aktualizuje właściwości zgłoszoną podwójnego lokalnych Ustawianie stanu **powodzenia** i usuwanie obiektu **pendingConfig** . Następnie aktualizuje podwójnego w usłudze.

    Należy który, aby zapisać przepustowość, informacjom właściwości są aktualizowane, określając tylko właściwości do zmodyfikowania (nazwanych **poprawki** w kodzie powyżej), zamiast zamieniania cały dokument.

    > [AZURE.NOTE] Ten samouczek nie symuluje dowolne zachowanie aktualizacji równoczesne konfiguracji. Niektóre procesy aktualizacji konfiguracji może być możliwe w zależności od zmian konfiguracji docelowej podczas aktualizacji jest uruchomiony, inne osoby mogły je z kolejki, a inne osoby mogą odrzucić warunek błędu. Upewnij się, że należy rozważyć, czy zachowanie procesu określonej konfiguracji, a następnie dodaj odpowiednie logiczny przed rozpoczęciem zmian konfiguracji.

6. Uruchom aplikację urządzenia:

        node SimulateDeviceConfiguration.js

    Powinien zostać wyświetlony komunikat `retrieved device twin`. Zachowaj uruchamianie aplikacji.

## <a name="create-the-service-app"></a>Tworzenie aplikacji usługi

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która aktualizuje *odpowiednie właściwości* na podwójnego skojarzone z **myDeviceId** z obiektem konfiguracji telemetrycznego. Następnie kwerendy twins urządzenia przechowywane w Centrum i jest wyświetlana różnica między konfiguracji odpowiednie i zgłoszoną urządzenia.

1. Utwórz nowy pusty folder o nazwie **setdesiredandqueryapp**. W folderze **setdesiredandqueryapp** Utwórz nowy plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **setdesiredandqueryapp** uruchom następujące polecenie, aby zainstalować pakiet **azure iothub** :

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **SetDesiredAndQuery.js** w folderze **addtagsandqueryapp** .

4. Dodaj następujący kod do pliku **SetDesiredAndQuery.js** i zastąpić symbol zastępczy **{Parametry połączenia usługi}** przy użyciu parametrów połączenia, skopiowany po utworzeniu Twoim Centrum:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Obiekt **rejestru** udostępnia wszystkie metody, które są wymagane do współdziałania z twins urządzenia z usługi. Poprzedniego kodu, po jej inicjuje obiekt **rejestru** , pobiera podwójnego dla **myDeviceId**i aktualizuje jego odpowiednie właściwości z obiektem konfiguracji telemetrycznego. Po wykonaniu tej wywołuje zdarzenia funkcja **queryTwins** 10 sekund.

    > [AZURE.IMPORTANT] Ta aplikacja kwerendy Centrum IoT co 10 sekund w celach opisowy. Generowanie raportów widocznych dla użytkownika na wielu urządzeniach, a nie wykrywa zmian za pomocą kwerendy. Jeśli rozwiązanie wymaga w czasie rzeczywistym powiadomień o zdarzeniach urządzenia za pomocą [urządzenia w chmurze wiadomości][lnk-d2c].

5. Dodaj następujące prawa kod przed `registry.getDeviceTwin()` wywołania w celu zaimplementowania funkcji **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Poprzednich kwerend kod twins przechowywane w Centrum i na wydruku konfiguracji odpowiednie i zgłoszoną telemetrycznego. Zapoznaj się z [Centrum IoT języka kwerend] [ lnk-query] Aby dowiedzieć się, jak do generowania raportów sformatowanego na wszystkich swoich urządzeniach.


8. Uruchomiony **SimulateDeviceConfiguration.js** uruchamianie aplikacji:

        node SetDesiredAndQuery.js 5m

    Powinien zostać wyświetlony opisane częstotliwość pięć minut zamiast 24 godziny wysyłania zmiana konfiguracji odniesienie **sukcesu** na **Oczekiwanie** **efektywnego** ponownie przy użyciu nowego aktywne.

    > [AZURE.IMPORTANT] Istnieje opóźnienie maksymalnie minutę między operacji raport urządzenia i wyników kwerendy. To jest umożliwienie infrastruktury kwerendy do pracy w skali bardzo wysoki. Pobieranie spójne widoki pojedynczy podwójnego użyj metody **getDeviceTwin** klasy **rejestru** .

## <a name="next-steps"></a>Następne kroki

W tym samouczku ustawianie konfiguracja docelowa jako *odpowiednie właściwości* z aplikacji wewnętrznej, a napisane aplikacji dla symulacji wykrywanie przez zmienianie lub zasymulowania raportowania stanu jako *zgłoszone właściwości* do podwójnego procesu aktualizacji wielu kroku.

Skorzystaj z następujących zasobów, aby dowiedzieć się, jak:

- Wysyłanie telemetrycznego z urządzeń ze [Wprowadzenie do Centrum IoT] [ lnk-iothub-getstarted] samouczka
- Planowanie lub wykonywania operacji na dużych zestawów urządzeń, zobacz [Używanie zadań planowanie i emitowanie operacje urządzenia] [ lnk-schedule-jobs] samouczka.
- Sterowanie urządzeniami interakcyjne (na przykład włączenie wentylatora za pomocą aplikacji kontrolowane przez użytkownika), [Użyj metody bezpośredni] [ lnk-methods-tutorial] samouczka.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
