<properties
    pageTitle="Azure Centrum IoT Node.js wprowadzenie | Microsoft Azure"
    description="Samouczek wprowadzający Azure koncentratora IoT za pomocą Node.js wprowadzenie. Za pomocą Centrum IoT Azure i Node.js SDK IoT Microsoft Azure do wdrożenia rozwiązanie programu Internet rzeczy."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Wprowadzenie do Centrum IoT Azure dla Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na końcu tego samouczka masz trzy aplikacji konsoli Node.js:

* **CreateDeviceIdentity.js**, co spowoduje utworzenie urządzenia tożsamości i skojarzony klucz nawiązywania połączenia między urządzeniem symulowany.
* **ReadDeviceToCloudMessages.js**, wyświetlającego telemetrycznego wysyłane przez symulowany urządzenia.
* **SimulatedDevice.js**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej i wysyła wiadomości telemetrycznego co drugi przy użyciu protokołu AMQP.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, których można użyć do utworzenia obu uruchamiania aplikacji na urządzeniach i sieci wewnętrznej rozwiązanie.

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Wersja node.js 0.10.x lub nowszym.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Teraz utworzono Twoim Centrum IoT. Masz hostname Centrum IoT i Centrum IoT parametry połączenia, które należy wykonać podczas przerabiania tego samouczka.

## <a name="create-a-device-identity"></a>Tworzenie urządzenia tożsamości

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która umożliwia utworzenie urządzenia tożsamości w rejestrze tożsamości w Twoim Centrum IoT. Urządzenie nie może nawiązać połączenia Centrum IoT, o ile nie zawiera on wpis w rejestrze tożsamości urządzenia. Zapoznaj się z sekcją **Rejestru tożsamości urządzenia** [IoT Centrum deweloperów przewodnik] [ lnk-devguide-identity] uzyskać więcej informacji. Podczas uruchamiania tej aplikacji konsoli generuje unikatowy identyfikator i klucz, który urządzenia służy do identyfikacji podczas wysyłania wiadomości urządzenia w chmurze do koncentratora IoT.

1. Utwórz nowy pusty folder o nazwie **createdeviceidentity**. W folderze **createdeviceidentity** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **createdeviceidentity** uruchom następujące polecenie, aby zainstalować pakiet SDK usługi **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. Przy użyciu edytora tekstu, Utwórz plik **CreateDeviceIdentity.js** w folderze **createdeviceidentity** .

4. Dodaj następujący `require` instrukcji na początku pliku **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Dodaj następujący kod do pliku **CreateDeviceIdentity.js** i Zamień wartość symbol zastępczy parametry połączenia dla Centrum IoT, który został utworzony w poprzedniej sekcji: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Dodaj następujący kod, aby utworzyć definicję urządzenia w rejestrze tożsamości urządzenia w Twoim Centrum IoT. Kod ten tworzy urządzenia, jeśli identyfikator urządzenia nie istnieje w rejestrze, w przeciwnym razie zwraca klucz istniejące urządzenie:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Zapisz i zamknij plik **CreateDeviceIdentity.js** .

8. Aby uruchomić aplikację **createdeviceidentity** , wykonaj następujące polecenie w wierszu polecenia w folderze createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Zanotuj **identyfikator urządzenia** i **klucza urządzenia**. Potrzebujesz tych wartości później podczas tworzenia aplikacji, która łączy do koncentratora IoT jako urządzenie.

> [AZURE.NOTE] Centrum IoT rejestru tożsamości są przechowywane tożsamości urządzenia umożliwiające bezpieczny dostęp do Centrum. Przechowuje identyfikatory urządzeń i klawisze służące do pracy jako poświadczeń zabezpieczeń i flagi włączone/wyłączone, którą można wyłączyć dostęp dla każdego urządzenia. Jeśli aplikacja wymaga do przechowywania inne metadane specyficzne dla urządzenia, używaj sklepu specyficzne dla aplikacji. Można znaleźć w [Podręczniku dewelopera Centrum IoT] [ lnk-devguide-identity] uzyskać więcej informacji.

## <a name="receive-device-to-cloud-messages"></a>Odbieranie wiadomości urządzenia w chmurze

W tej sekcji możesz utworzyć aplikację konsoli Node.js, który odczytuje urządzenia w chmurze wiadomości z Centrum IoT. Centrum IoT udostępnia [Koncentratory zdarzenia][lnk-event-hubs-overview]-zgodne punktu końcowego, aby możliwe było czytać wiadomości urządzenia w chmurze. Aby zachować proste czynności, ten samouczek tworzy czytnika podstawowe, który nie jest odpowiedni dla wdrożenia wysokiej wydajności. [Proces urządzenia w chmurze wiadomości] [ lnk-process-d2c-tutorial] samouczku pokazano, sposób przetwarzania wiadomości urządzenia w chmurze w skali. [Wprowadzenie do koncentratorów zdarzenia] [ lnk-eventhubs-tutorial] samouczek zawiera dalsze informacje dotyczące celu przetwarzania wiadomości z koncentratorów zdarzeń i ma zastosowanie do punktów końcowych zgodnego Centrum IoT Centrum zdarzenia.

> [AZURE.NOTE] Punkt końcowy zgodnego Centrum zdarzenia zawsze czytania wiadomości urządzenia w chmurze za pośrednictwem protokołu AMQP.

1. Utwórz nowy pusty folder o nazwie **readdevicetocloudmessages**. W folderze **readdevicetocloudmessages** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby zainstalować pakiet **azure zdarzenia — koncentratory** :

    ```
    npm install azure-event-hubs --save
    ```

3. Przy użyciu edytora tekstu, Utwórz plik **ReadDeviceToCloudMessages.js** w folderze **readdevicetocloudmessages** .

4. Dodaj następujący `require` instrukcje na początku pliku **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Dodaj następujące deklaracji zmiennych i Zamień wartości symbolu zastępczego przy użyciu parametrów połączenia z koncentratora IoT.

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Dodaj następujące dwie funkcje, które wydruk do konsoli:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Dodaj następujący kod do tworzenia **EventHubClient**, Otwórz połączenie z Twoim Centrum IoT i Tworzenie odbiorcy dla każdego partycją. Ta aplikacja używa filtru podczas tworzenia adresatem tak, aby odbiorcy odczytuje tylko wiadomości wysłane do Centrum IoT po uruchamiania odbiorcy. Ten filtr jest przydatne w środowisku testowym, aby wyświetlić tylko bieżący zestaw wiadomości. W środowisku produkcyjnym kodzie upewnij się, że przetwarza wszystkie wiadomości - zobacz [sposobu przetwarzania wiadomości urządzenia w chmurze Centrum IoT] [ lnk-process-d2c-tutorial] samouczka, aby uzyskać więcej informacji:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Zapisz i zamknij plik **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która symuluje z urządzenia, które wysyła wiadomości urządzenia w chmurze do koncentratora IoT.

1. Utwórz nowy pusty folder o nazwie **simulateddevice**. W folderze **simulateddevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby zainstalować pakiet SDK urządzenia **azure iot urządzeń** i pakiet **azure-iot urządzenia — amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **SimulatedDevice.js** w folderze **simulateddevice** .

4. Dodaj następujący `require` instrukcje na początku pliku **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Dodawanie zmiennej **connectionString** i za jej pomocą tworzyć klienta urządzenia. Zamienianie **{youriothostname}** o nazwie Centrum IoT utworzonej sekcji *Tworzenie Centrum IoT* . **{Yourdevicekey}** należy zastąpić wartość klucza urządzenia wygenerowania w sekcji *Tworzenie tożsamości urządzenia* :

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Dodaj następującą funkcję, aby wyświetlić dane wyjściowe aplikacji:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Tworzenie zwrotnego i wysyłanie nowej wiadomości do Twojej Centrum IoT co sekundę za pomocą funkcji **setInterval** :

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Otwórz połączenie z Twoim Centrum IoT i rozpocząć wysyłanie wiadomości:

    ```
    client.open(connectCallback);
    ```

9. Zapisz i zamknij plik **SimulatedDevice.js** .

> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].


## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby rozpocząć monitorowanie Twoim Centrum IoT:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Aplikacja klienta usługi Centrum IoT node.js monitorowanie wiadomości urządzenia w chmurze][7]

2. W wierszu polecenia — w folderze **simulateddevice** uruchom następujące polecenie, aby rozpocząć wysyłanie dane telemetryczne na Twoim Centrum IoT:

    ```
    node SimulatedDevice.js
    ```

    ![Aplikacji klienckiej node.js Centrum IoT urządzenia do wysyłania wiadomości urządzenia w chmurze][8]

3. Fragment **zastosowania** w [Azure portal] [ lnk-portal] wyświetlana liczba wiadomości wysłane do niego:

    ![Azure portalu zastosowania kafelków pokazujący liczbę wiadomości wysłane do Centrum IoT][43]

## <a name="next-steps"></a>Następne kroki

W tym samouczku skonfigurowane nowego koncentratora IoT w portalu Azure, a następnie tworzone urządzenia tożsamości w rejestrze tożsamości koncentratora. Ta tożsamość urządzenia umożliwia włączyć aplikację symulacji wysyłania wiadomości urządzenia w chmurze do koncentratora. Utworzono aplikację, która zawiera wiadomości odebranych przez Centrum. 

Aby kontynuować, wprowadzenie do Centrum IoT i Eksplorowanie innych sytuacjach IoT, zobacz:

- [Łączenie urządzenia][lnk-connect-device]
- [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
- [Wprowadzenie do programu IoT SDK bramy][lnk-gateway-SDK]

Aby dowiedzieć się, jak rozszerzyć rozwiązania IoT i przetwarzania wiadomości urządzenia w chmurze w skali, zobacz [wiadomości urządzenia w chmurze proces] [ lnk-process-d2c-tutorial] samouczka.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
