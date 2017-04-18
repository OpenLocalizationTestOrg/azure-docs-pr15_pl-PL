<properties
    pageTitle="Rozpoczynanie pracy z twins | Microsoft Azure"
    description="W tym samouczku pokazano sposób używania twins"
    services="iot-hub"
    documentationCenter="node"
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

# <a name="tutorial-get-started-with-device-twins-preview"></a>Samouczek: Wprowadzenie urządzenia twins (wersja preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na końcu tego samouczka konieczne będzie .NET i aplikacji konsoli Node.js:

* **AddTagsAndQuery.sln**, aplikacji dla programu .NET ma być uruchamiane wewnętrznej, co powoduje znaczniki lub kwerendy twins urządzenia.
* **TwinSimulatedDevice.js**, aplikacja Node.js, symuluje urządzenie, które łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej i raportów stanu łączności.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, w której można tworzyć zarówno urządzenia i wewnętrznej aplikacje.

Aby użyć tego samouczka są potrzebne następujące elementy:

+ Microsoft Visual Studio 2015 r.

+ Wersja node.js 0.10.x lub nowszym.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Tworzenie aplikacji usługi

W tej sekcji możesz utworzyć aplikację konsoli Node.js, który zapewnia metadane lokalizacji podwójnego skojarzone z **myDeviceId**. Go, a następnie kwerendy twins przechowywane w Centrum Wybieranie urządzenia znajdujące się w Stanach Zjednoczonych, a następnie te raportowania sieci komórkowej.

1. W programie Visual Studio Dodaj projekt Visual C# klasyczny komputerze z systemem Windows z bieżącym rozwiązaniem przy użyciu **Aplikacji konsoli** szablon projektu. Nazwa projektu **AddTagsAndQuery**.

    ![Nowy projekt Visual C# klasyczny komputerze z systemem Windows][img-createapp]

2. W oknie Eksplorator rozwiązań projektu **AddTagsAndQuery** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów Nuget**.

3. W oknie **Menedżer pakietów Nuget** upewnij się, że **wstępną Dołącz** jest zaznaczone, wyszukiwanie **microsoft.azure.devices**, wybierz pozycję **Zainstaluj** , aby zainstalować wersję *wstępną* **Microsoft.Azure.Devices** pakowanie i zaakceptuj warunki użytkowania. Ta procedura — pliki do pobrania, instaluje i dodaje odwołanie do [Microsoft Azure IoT usługi SDK] [ lnk-nuget-service-sdk] Nuget pakietu i jego zależności.

    ![Okno Menedżera pakietu Nuget][img-servicenuget]

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.Azure.Devices;

5. Dodawanie pola do klasy **programu** . Zamienianie wartości symbolu zastępczego przy użyciu parametrów połączenia koncentratora IoT, utworzoną w poprzedniej sekcji.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodaj następujące metody klasy **programu** :

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Klasa **RegistryManager** udostępnia wszystkie metody, które są wymagane do współdziałania z twins urządzenia z usługi. Poprzedni kod najpierw inicjuje obiektu **registryManager** , a następnie pobiera podwójnego dla **myDeviceId**, a na koniec aktualizuje jego znaczniki z informacjami o odpowiedniej lokalizacji.

    Po zaktualizowaniu, wykonywania dwóch zapytań: pierwszego wybiera tylko twins urządzenia urządzeń znajdujących się w instalacji **Redmond43** , a drugi usprawnia kwerendę, aby wybrać tylko urządzeń, które są również połączone za pośrednictwem sieci komórkowej.

    Należy zauważyć, że poprzedniego kodu, podczas tworzenia obiektu **kwerendy** określa maksymalną liczbę zwracanych dokumentów. Obiekt **kwerendy** zawiera właściwość logiczna **HasMoreResults** , który służy do wywołania metody **GetNextAsTwinAsync** wielokrotnie pobrać wszystkie wyniki. Metodę o nazwie **GetNextAsJson** jest dostępny dla wyników, które nie są twins urządzenia, na przykład wyniki kwerend agregacji.

7. Na koniec dodać kolejne wiersze do metody **główne** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Uruchomienia tej aplikacji, a powinna być widoczna jedno urządzenie w wynikach kwerendy pytaniem dla wszystkich urządzeń znajduje się w **Redmond43** i Brak kwerendy ograniczeniem wyniki do urządzenia, które korzystają z sieci komórkowej.

    ![Wyniki kwerendy w oknie][img-addtagapp]

W następnej sekcji, możesz utworzyć aplikację urządzenia, która zgłasza informacje dotyczące łączności i zmianę wyniku kwerendę w poprzedniej sekcji.

## <a name="create-the-device-app"></a>Tworzenie aplikacji urządzenia

W tej sekcji możesz utworzyć Node.js aplikacji konsoli, który łączy się z Centrum jako **myDeviceId**, a następnie aktualizuje jego podwójnego zgłoszone właściwości do zawierają informacje, że jest podłączone za pośrednictwem sieci komórkowej.

1. Utwórz nowy pusty folder o nazwie **reportconnectivity**. W folderze **reportconnectivity** Utwórz nowy plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **reportconnectivity** uruchom następujące polecenie, aby zainstalować **azure iot urządzenia**, a pakiet **azure-iot urządzenia — mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **ReportConnectivity.js** w folderze **reportconnectivity** .

4. Dodaj następujący kod do pliku **ReportConnectivity.js** i zastąpić symbol zastępczy **{Parametry połączenia urządzenia}** przy użyciu parametrów połączenia, skopiowany po utworzeniu **myDeviceId** urządzenia tożsamości:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Obiekt **klienta** udostępnia wszystkie metody, wymaganych do współdziałania z twins urządzenia z urządzenia. Kod, po jego inicjuje obiekt **klienta** pobiera podwójnego dla **myDeviceId** i aktualizuje właściwości zgłoszoną informacje dotyczące łączności.

5. Uruchom aplikację urządzenia

        node ReportConnectivity.js

    Powinien zostać wyświetlony komunikat `twin state reported`.

6. Teraz, gdy urządzenie podać informacje dotyczące jego łączności, powinien pojawić się w obu kwerendy. Uruchom aplikację.NET **AddTagsAndQuery** , aby ponownie uruchomić kwerendy. Ten czas **myDeviceId** będą widoczne w obu wyników kwerendy.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Następne kroki
W tym samouczku skonfigurowane nowego koncentratora IoT w portalu Azure, a następnie tworzone urządzenia tożsamości w rejestrze tożsamości koncentratora. Dodano metadane urządzenia jako znaczniki z aplikacją wewnętrznej i zapisano aplikacji symulacji w raporcie urządzenia łączności informacje w podwójnego urządzenia. Wiesz również jak wyszukiwać w tych informacji przy użyciu Centrum IoT języka kwerend SQL przypominających.

Skorzystaj z następujących zasobów, aby dowiedzieć się, jak:

- Wysyłanie telemetrycznego z urządzeń ze [Wprowadzenie do Centrum IoT] [ lnk-iothub-getstarted] samouczka
- Konfigurowanie urządzeń [za pomocą odpowiednie właściwości, aby skonfigurować urządzenia] za pomocą właściwości odpowiedniej osoby podwójnego[ lnk-twin-how-to-configure] samouczka
- Sterowanie urządzeniami interakcyjne (na przykład włączenie wentylatora za pomocą aplikacji kontrolowane przez użytkownika) z [Użyj metody bezpośredni] [ lnk-methods-tutorial] samouczka.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

