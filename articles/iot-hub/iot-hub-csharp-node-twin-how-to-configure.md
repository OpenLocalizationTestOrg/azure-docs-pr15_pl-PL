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
* **SetDesiredConfigurationAndQuery**, aplikacji konsoli programu .NET ma być uruchamiane wewnętrznej, który określa konfiguracja docelowa na urządzeniu i kwerend proces aktualizacji konfiguracji.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, w której można tworzyć zarówno urządzenia i wewnętrznej aplikacje.

Aby użyć tego samouczka są potrzebne następujące elementy:

+ Microsoft Visual Studio 2015 r.

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

2. W swojej wiersza polecenia w folderze **simulatedeviceconfiguration** uruchom następujące polecenie, aby zainstalować **azure iot urządzenia**, a pakiet **azure-iot urządzenia — mqtt** :

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

    Należy zauważyć, że dla uproszczenia, poprzedniego kodu używa domyślnie ustalonej dla początkowej konfiguracji. Rzeczywistą aplikacji prawdopodobnie chcesz załadować tej konfiguracji z magazynu lokalnego.
    
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

W tej sekcji możesz utworzyć aplikację konsoli .NET, która aktualizuje *odpowiednie właściwości* na podwójnego skojarzone z **myDeviceId** z obiektem konfiguracji telemetrycznego. Następnie kwerendy twins urządzenia przechowywane w Centrum i jest wyświetlana różnica między konfiguracji odpowiednie i zgłoszoną urządzenia.

1. W programie Visual Studio Dodaj projekt Visual C# klasyczny komputerze z systemem Windows z bieżącym rozwiązaniem przy użyciu szablonu projektów **Console Application** . Nazwa projektu **SetDesiredConfigurationAndQuery**.

    ![Nowy projekt Visual C# klasyczny komputerze z systemem Windows][img-createapp]

2. W oknie Eksplorator rozwiązań projektu **SetDesiredConfigurationAndQuery** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów Nuget**.

3. W oknie **Menedżer pakietów Nuget** upewnij się, że **wstępną Dołącz** jest zaznaczone, wyszukiwanie **microsoft.azure.devices**, wybierz pozycję **Zainstaluj** , aby zainstalować wersję *wstępną* **Microsoft.Azure.Devices** pakowanie i zaakceptuj warunki użytkowania. Ta procedura — pliki do pobrania, instaluje i dodaje odwołanie do [Microsoft Azure IoT usługi SDK] [ lnk-nuget-service-sdk] Nuget pakietu i jego zależności.

    ![Okno Menedżera pakietu Nuget][img-servicenuget]

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Dodawanie pola do klasy **programu** . Zamień wartości symbolu zastępczego parametry połączenia dla Centrum IoT, utworzoną w poprzedniej sekcji.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodaj następujące metody klasy **programu** :

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    Obiekt **rejestru** udostępnia wszystkie metody, które są wymagane do współdziałania z twins urządzenia z usługi. Poprzedniego kodu, po jej inicjuje obiekt **rejestru** , pobiera podwójnego dla **myDeviceId**i aktualizuje jego odpowiednie właściwości z obiektem konfiguracji telemetrycznego.
    Po wykonaniu tej co 10 sekund go kwerendy twins przechowywane w Centrum i na wydruku konfiguracji odpowiednie i zgłoszoną telemetrycznego. Zapoznaj się z [Centrum IoT języka kwerend] [ lnk-query] Aby dowiedzieć się, jak do generowania raportów sformatowanego na wszystkich swoich urządzeniach.

    > [AZURE.IMPORTANT] Ta aplikacja kwerendy Centrum IoT co 10 sekund w celach opisowy. Generowanie raportów widocznych dla użytkownika na wielu urządzeniach, a nie wykrywa zmian za pomocą kwerendy. Jeśli rozwiązanie wymaga w czasie rzeczywistym powiadomień o zdarzeniach urządzenia za pomocą [urządzenia w chmurze wiadomości][lnk-d2c].

7. Na koniec dodać kolejne wiersze do metody **główne** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. **SimulateDeviceConfiguration.js** jest uruchomiony, przy częstotliwość pięć minut zamiast 24 godziny wysyłania uruchamianie aplikacji .NET z programu Visual Studio przy użyciu **F5** , a powinna być widoczna konfigurację zgłoszoną zmienić odniesienie **sukcesu** na **Oczekiwanie** **efektywnego** ponownie przy użyciu nowego aktywne.

    > [AZURE.IMPORTANT] Istnieje opóźnienie maksymalnie minutę między operacji raport urządzenia i wyników kwerendy. To jest umożliwienie infrastruktury kwerendy do pracy w skali bardzo wysoki. Pobieranie spójne widoki pojedynczy podwójnego użyj metody **getDeviceTwin** klasy **rejestru** .

## <a name="next-steps"></a>Następne kroki

W tym samouczku ustawianie konfiguracja docelowa jako *żądane właściwości* z wewnętrzna, a napisane aplikacji dla urządzenia do wykrywania tej zmiany i zasymulowania raportowania stanu jako *Właściwości zgłoszoną* do podwójnego procesu aktualizacji wielu krok.

Skorzystaj z następujących zasobów, aby dowiedzieć się, jak:

- Wysyłanie telemetrycznego z urządzeń ze [Wprowadzenie do Centrum IoT] [ lnk-iothub-getstarted] samouczka
- Planowanie lub wykonywania operacji na dużych zestawów urządzeń, zobacz [Używanie zadań planowanie i emitowanie operacje urządzenia] [ lnk-schedule-jobs] samouczka.
- Sterowanie urządzeniami interakcyjne (na przykład włączenie wentylatora za pomocą aplikacji kontrolowane przez użytkownika), [Użyj metody bezpośredni] [ lnk-methods-tutorial] samouczka.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

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
