<properties
    pageTitle="Wysyłanie wiadomości chmury do urządzenia z Centrum IoT | Microsoft Azure"
    description="Postępuj zgodnie z tego samouczka, aby dowiedzieć się, jak wysyłać wiadomości chmury do urządzenia Centrum IoT Azure za pomocą języka Java."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Samouczek: Sposobu wysyłania wiadomości chmury do urządzenia z Centrum IoT i Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Wprowadzenie

Azure Centrum IoT jest w pełni zarządzanych usług, który ułatwia włączanie i niezawodności komunikacji dwukierunkowego między miliony IoT urządzeń i zakończyć aplikację ponownie. [Wprowadzenie do Centrum IoT] samouczek pokazano, jak tworzyć Centrum IoT, obsługi administracyjnej urządzenia tożsamości w nim i kod symulowany urządzenie, które wysyła wiadomości urządzenia w chmurze.

Ten samouczek opiera się na [Wprowadzenie do Centrum IoT]. Jak przedstawiono do:

- Z Twojej aplikacji chmury wewnętrznej wysyłanie wiadomości chmury do urządzenia do jednego urządzenia za pośrednictwem Centrum IoT.
- Odbieranie wiadomości chmury do urządzenia na urządzeniu.
- Z chmury usługi aplikacji zakończenia, należy zażądać potwierdzenia dostarczenia (*Opinia*) dla wiadomości wysyłane do urządzenia z Centrum IoT.

Więcej informacji na temat wiadomości w chmurze do urządzenia można znaleźć w [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

Na końcu tego samouczka możesz uruchomić dwóch aplikacji konsoli Node.js:

* **SimulatedDevice**, zmodyfikowaną wersję aplikacji utworzony w [Wprowadzenie do Centrum IoT], który łączy się z Centrum IoT i odbiera wiadomości w chmurze do urządzenia.
* **SendCloudToDeviceMessage**, które wysyła wiadomość chmury do urządzenia do symulowany urządzenia za pomocą Centrum IoT, a następnie otrzymuje jego potwierdzenia dostarczenia.

> [AZURE.NOTE] Centrum IoT obsługuje SDK dla wielu platformy urządzeń i języki (w tym C, Java i Javascript) SDK urządzenia Azure IoT. Aby uzyskać instrukcje krok po kroku dotyczące nawiązywania połączenia między urządzeniem kod tego samouczka i ogólnie do koncentratora IoT Azure, zobacz [Centrum deweloperów IoT Azure].

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Wersja node.js 0.10.x lub nowszym.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Odbieranie wiadomości na tym urządzeniu symulowany

W tej sekcji można zmodyfikować aplikację symulacji utworzone w [Wprowadzenie do Centrum IoT] otrzymywać wiadomości w chmurze do urządzenia z poziomu Centrum IoT.

1. Przy użyciu edytora tekstu, otwórz plik SimulatedDevice.js.

2. Modyfikowanie funkcja **connectCallback** do obsługi wiadomości wysłanych z Centrum IoT. W tym przykładzie urządzenie zawsze wywołuje **wykonania** funkcji powiadomienia Centrum IoT o tym, że zostały przetworzone wiadomości. Nową wersję funkcji **connectCallback** wygląda następująco:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Jeśli używasz protokołu HTTP zamiast MQTT lub AMQP jako transportu wystąpienia **DeviceClient** sprawdza wiadomości z Centrum IoT rzadko (mniej niż co 25 minut). Aby uzyskać więcej informacji o różnicach między MQTT, AMQP i HTTP pomocy technicznej i ograniczania Centrum IoT, zobacz [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Wysyłanie wiadomości chmury do urządzenia

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która wysyła wiadomości w chmurze do urządzenia do aplikacji symulacji. Potrzebujesz identyfikator urządzenia dodanych w samouczku [Wprowadzenie do Centrum IoT] urządzenia. Należy również parametry połączenia z koncentratora IoT, które można uzyskać w [portalu Azure].

1. Utwórz pusty folder o nazwie **sendcloudtodevicemessage**. W folderze **sendcloudtodevicemessage** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia. Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```

2. W swojej wiersza polecenia w folderze **sendcloudtodevicemessage** uruchom następujące polecenie, aby zainstalować pakiet **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **SendCloudToDeviceMessage.js** w folderze **sendcloudtodevicemessage** .

4. Dodaj następujący `require` instrukcje na początku pliku **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Dodaj następujący kod do pliku **SendCloudToDeviceMessage.js** . Zamień symbol zastępczy wartość ciągu połączenia parametry połączenia dla Centrum IoT, utworzonego w samouczku [Wprowadzenie do Centrum IoT] . Zamienić symbol zastępczy urządzenia docelowej identyfikator urządzenia dodanych w samouczku [Wprowadzenie do Centrum IoT] urządzenia:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Dodaj następującą funkcję, aby wydrukować wyniki operacji do konsoli:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Dodaj następującą funkcję do wydrukowania dostarczenia wiadomości opinii do konsoli:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Dodaj następujący kod, aby wysłać wiadomość do urządzenia i obsługę wiadomości opinii, podczas urządzenie uznaje wiadomości chmury do urządzenia:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Zapisz i zamknij plik **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W dolnym wierszu polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby wysłać telemetrycznego do Centrum IoT i słuchanie wiadomości w chmurze do urządzenia:

    ```
    node SimulatedDevice.js 
    ```

    ![Uruchom aplikację symulacji][img-simulated-device]

2. W wierszu polecenia — w folderze **sendcloudtodevicemessage** uruchom następujące polecenie, aby wysłać wiadomość chmury do urządzenia i poczekaj opinii potwierdzenia:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Uruchom aplikację, aby wysłać polecenie c2d][img-send-command]

    > [AZURE.NOTE] Sake i uproszczenia tego samouczka nie zaimplementować wszelkie zasady ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak wysyłać i odbierać wiadomości w chmurze do urządzenia. 

Aby wyświetlić przykłady pełną rozwiązań zakończenia do końca korzystające z Centrum IoT, zobacz [Pakiet IoT Azure].

Aby dowiedzieć się więcej na temat tworzenia rozwiązań z Centrum IoT, zobacz [Przewodnik dewelopera Centrum IoT].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Wprowadzenie do Centrum IoT]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Centrum IoT przewodnik dewelopera]: iot-hub-devguide.md
[Centrum deweloperów IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Pakiet Azure IoT]: https://azure.microsoft.com/documentation/suites/iot-suite/