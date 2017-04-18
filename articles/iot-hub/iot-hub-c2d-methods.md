<properties
 pageTitle="Bezpośrednie metodami | Microsoft Azure"
 description="W tym samouczku pokazano sposób bezpośredni metodami"
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
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Samouczek: Metodami bezpośrednie

## <a name="introduction"></a>Wprowadzenie

Azure Centrum IoT jest w pełni zarządzane usługa, która pozwala zaufanego i bezpieczna komunikacja dwukierunkowa między miliony IoT urządzeń i aplikacji wewnętrzna baza danych. Samouczki poprzedniego ([Wprowadzenie do Centrum IoT] i [Wysyłanie wiadomości chmury do urządzenia z Centrum IoT]) przedstawiono podstawowe funkcje wiadomości z Centrum IoT urządzenia w chmurze i w chmurze do urządzenia. Centrum IoT daje także możliwość wywołania metody nietrwałe na urządzeniach z chmury. Metody reprezentują żądanie odpowiedź interakcji z urządzeniem podobne do połączenia HTTP, w tym powiodła się lub się nie powieść natychmiast (po zdefiniowane przez użytkownika przekroczenie limitu czasu) umożliwić użytkownikowi o stanie połączenia. [Wywoływanie metody bezpośrednio na urządzeniu] [ lnk-devguide-methods] metod bardziej szczegółowo opisano i zawiera wskazówki o tym, kiedy metodami biurowa lub wiadomości w chmurze do urządzenia.

W tym samouczku pokazano sposób do:

- Azure portal umożliwia tworzenie Centrum IoT i tworzenie urządzenia tożsamości w Twoim Centrum IoT.
- Tworzenie symulowany urządzenie ma bezpośrednich metodę, która może być wywoływana przez chmury.
- Tworzenie aplikacji konsoli wywołującej metodę bezpośrednio na tym urządzeniu symulowany za pośrednictwem Centrum usługi IoT.

> [AZURE.NOTE] W tej chwili bezpośrednich metod są dostępne tylko z urządzeń łączących się Centrum IoT przy użyciu protokołu MQTT. Zajrzyj do [pomocy technicznej MQTT] [ lnk-devguide-mqtt] artykuł, aby uzyskać instrukcje dotyczące jak przekonwertować istniejącej aplikacji urządzenia za pomocą MQTT.

Na końcu tego samouczka masz dwie aplikacji konsoli Node.js:

* **CallMethodOnDevice.js**, połączeń metodą na urządzeniu symulowany i wyświetla odpowiedzi.
* **SimulatedDevice.js**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej i odpowiada metody o nazwie w chmurze.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, których można użyć do utworzenia obu uruchamiania aplikacji na urządzeniach i sieci wewnętrznej rozwiązanie.

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Wersja node.js 0.10.x lub nowszym.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która odpowiada metodę o nazwie przez w chmurze.

1. Utwórz nowy pusty folder o nazwie **simulateddevice**. W folderze **simulateddevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby zainstalować pakiet SDK urządzenia **azure iot urządzeń** i pakiet **azure-iot urządzenia — mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **SimulatedDevice.js** w folderze **simulateddevice** .

4. Dodaj następujący `require` instrukcje na początku pliku **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Dodawanie zmiennej **connectionString** i za jej pomocą tworzyć klienta urządzenia. **{Parametry połączenia urządzenia}** należy zastąpić wygenerowania w sekcji *Tworzenie urządzenia tożsamości* parametry połączenia:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Dodaj następującą funkcję do wykonania metody na urządzeniu:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Otwieranie połączenia do Centrum IoT i Rozpocznij zainicjować odbiornika metody:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Zapisz i zamknij plik **SimulatedDevice.js** .

> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład ponów próbę połączenia), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Wywołania metody na urządzeniu

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która wywołuje metodę symulowany urządzenia, a następnie wyświetli odpowiedź.

1. Utwórz nowy pusty folder o nazwie **callmethodondevice**. W folderze **callmethodondevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **callmethodondevice** uruchom następujące polecenie, aby zainstalować pakiet **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz plik **CallMethodOnDevice.js** w folderze **callmethodondevice** .

4. Dodaj następujący `require` instrukcje na początku pliku **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Dodaj następujące deklaracji zmiennych i Zamień wartości symbolu zastępczego przy użyciu parametrów połączenia z koncentratora IoT.

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Tworzenie klienta, aby otworzyć połączenie z Twoim Centrum IoT.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Dodaj następującą funkcję wywołania metody urządzenia i drukować odpowiedź urządzenia do konsoli:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Zapisz i zamknij plik **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze **simulateddevice** uruchom następujące polecenie, aby uruchomić wykrywanie metody połączenia z Twoim Centrum IoT:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. W wierszu polecenia — w folderze **callmethodondevice** uruchom następujące polecenie, aby rozpocząć monitorowanie Twoim Centrum IoT:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Zostanie wyświetlony urządzenia zareagowania metody przez drukowanie wiadomość i aplikacji, w której o nazwie wyświetlania metody odpowiedź z urządzenia:

    ![][9]
    
## <a name="next-steps"></a>Następne kroki

W tym samouczku skonfigurowane nowego koncentratora IoT w portalu Azure, a następnie tworzone urządzenia tożsamości w rejestrze tożsamości koncentratora. Ta tożsamość urządzenia umożliwia włączyć aplikację symulacji reakcji na metody wywoływane w chmurze. Utworzono aplikację wywołuje metody na tym urządzeniu i wyświetla odpowiedź z urządzenia. 

Aby kontynuować, wprowadzenie do Centrum IoT i Eksplorowanie innych sytuacjach IoT, zobacz:

- [Wprowadzenie do Centrum IoT]
- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Aby dowiedzieć się, jak zwiększyć usługi IoT metody rozwiązania i planowanie połączeń na wielu urządzeniach, zobacz [Planowanie i emisji zadania] [ lnk-tutorial-jobs] samouczka.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Wysyłanie wiadomości chmury do urządzenia z Centrum IoT]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Wprowadzenie do Centrum IoT]: iot-hub-node-node-getstarted.md
