<properties
 pageTitle="Opcjonalna sekcja — Zmienianie włączać i wyłączać zachowanie LED | Microsoft Azure"
 description="Dostosowywanie wiadomości, aby zmienić LED Włączanie i wyłączanie funkcji zachowanie."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 sekcja opcjonalna: Zmienianie włączać i wyłączać zachowanie LED

## <a name="421-what-you-will-do"></a>4.2.1 co wykonasz

Dostosowywanie wiadomości, aby zmienić LED Włączanie i wyłączanie funkcji zachowanie. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 co dowiesz się

Dodatkowe funkcje Node.js umożliwia zmianę LED Włączanie i wyłączanie funkcji zachowanie.

## <a name="423-what-you-need"></a>4.2.3 co jest potrzebne

Musisz została pomyślnie ukończona [4.1 uruchomić aplikację próbki na swojej Pi malina do odbierania chmurą wiadomości urządzenia](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Dodawanie funkcji Node.js

1. Otwórz aplikację próbki w kodzie programu Visual Studio, uruchamiając następujące polecenia:

    ```bash
    cd Lesson4
    code .
    ```

2. Otwórz `app.js` pliku, a następnie dodaj następujące funkcje na końcu:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![Aktualizowanie app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Dodaj następujące warunki przed domyślnego jedną w bloku wielkość liter przełącznika `receiveMessageCallback` funkcji:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Przykładowa aplikacja odpowiedzieć więcej instrukcji przy użyciu wiadomości został skonfigurowany. Instrukcja "na" powoduje włączenie LED i instrukcji "off" wyłącza LED.

4. Otwórz plik gulpfile.js, a następnie dodaj nową funkcję przed funkcja `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Aktualizowanie gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. W `sendMessage` funkcja, Zamień linię `var message = buildMessage(sentMessageCount);` z nowego wiersza w następujących wstawki kodu:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Zapisz wszystkie zmiany.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 wdrażanie i uruchomić aplikację próbki

Wdrażanie i uruchamianie aplikacji próbki w swojej Pi, uruchamiając następujące polecenie:

```bash
gulp
```

Powinien zostać wyświetlony LED włączone przez dwie sekundy, a następnie wyłączone dla innego dwie sekundy. Ostatnią wiadomość "Przestań" przestaje uruchomienie aplikacji próbki.

![Włączanie i wyłączanie funkcji](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Gratulacje! Pomyślnie dostosowane wiadomości, które są wysyłane do Pi z Twoim Centrum IoT.

### <a name="427-summary"></a>4.2.7 podsumowanie

Ta sekcja opcjonalna demos jak dostosować wiadomości, dzięki czemu przykładowej aplikacji można kontrolować włączać i wyłączać zachowanie LED w inny sposób.

