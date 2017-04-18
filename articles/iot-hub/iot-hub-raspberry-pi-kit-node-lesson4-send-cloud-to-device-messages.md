<properties
 pageTitle="Uruchom aplikację przykładowe otrzymywać wiadomości w chmurze do urządzenia | Microsoft Azure"
 description="Aplikacja przykładowa w 4 lekcji działa na swojej Pi i monitoruje wiadomości przychodzące od Twoim Centrum IoT. Nowe zadanie gulp wysyłać wiadomości do Twojej Pi z Twoim Centrum IoT do miganie LED."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 uruchomić aplikację próbki, aby otrzymywać wiadomości w chmurze do urządzenia

W tej sekcji należy wdrożyć przykładowej aplikacji na swojej malina Pi 3. Przykładowa aplikacja monitoruje wiadomości przychodzące od Twoim Centrum IoT. Można również uruchomić zadania gulp na komputerze do wysyłania wiadomości do Twojej Pi z Twoim Centrum IoT. Po odebraniu wiadomości, przykładowej aplikacji miga LED. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1 co wykonasz

- Łączenie aplikacji przykładowych z Twoim Centrum IoT.
- Wdrażanie i uruchamianie aplikacji przykładowej.
- Wysyłanie wiadomości z Twoim Centrum IoT do swojego Pi do miganie LED.

## <a name="412-what-you-will-learn"></a>4.1.2 co dowiesz się

- Jak można monitorować wiadomości przychodzące od Twoim Centrum IoT.
- Jak wysyłać wiadomości chmury do urządzenia z Twoim Centrum IoT do swojego Pi. 

## <a name="413-what-do-you-need"></a>4.1.3 co to są potrzebne

- 3 Pi malina, która jest skonfigurowana do użycia. Aby dowiedzieć się, jak skonfigurować usługi Pi, zobacz [Lekcja 1: rozpoczynanie pracy z urządzeniem malina Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Centrum IoT utworzonym w ramach subskrypcji Azure. Aby dowiedzieć się, jak tworzyć Centrum IoT usługi Azure, zobacz [Lekcja 2: Tworzenie Centrum IoT usługi Azure](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 łączenie aplikacji przykładowych z Twoim Centrum IoT

1. Upewnij się, znajdują się w folderze repo `iot-hub-node-raspberrypi-getting-started`. Otwórz aplikację próbki w kodzie programu Visual Studio, uruchamiając następujące polecenia:

    ```bash
    cd Lesson4
    code .
    ```

    Powiadomienie o `app.js` plików w `app` podfolderu. `app.js` Plik jest plikiem źródłowym klucza, zawierający kod monitorowanie wiadomości przychodzące od centrum IoT. `blinkLED` LED miga, funkcja.

    ![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Inicjowanie pliku konfiguracji z następujących poleceń:

    ```bash
    npm install
    gulp init
    ```

    Jeśli wykonaniu lekcji 3 na tym komputerze, wszystkich konfiguracji są dziedziczone, można przejść do kroku 4.1.5. Jeśli ukończone lekcji 3 na innym komputerze, należy zamienić symbole zastępcze w `config-raspberrypi.json` pliku. `config-raspberrypi.json` Plik znajduje się w podfolderze folderze głównym.

    ![Konfiguracji](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- Zastąp **[adres IP lub nazwa hosta urządzenia]** do Pi adres IP lub nazwa hosta, który zostanie wyświetlony, uruchamiając polecenie`devdisco list --eth`
- Zamienianie **[parametry połączenia urządzenia IoT]** przy użyciu parametrów połączenia urządzenia, który zostanie wyświetlony, uruchamiając polecenie `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- Zamienianie **[parametry połączenia Centrum IoT]** przy użyciu parametrów połączenia Centrum IoT, który zostanie wyświetlony, uruchamiając polecenie `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 wdrażanie i uruchomić aplikację próbki

Wdrażanie i uruchamianie aplikacji próbki w swojej Pi, uruchamiając następujące polecenia:
  
```
gulp
```

Polecenie gulp uruchamia pierwsze zadanie instalacji narzędzia. Następnie wdraża przykładowej aplikacji do swojego Pi. Na koniec uruchomi aplikację na swojej Pi i oddzielnego zadania na komputerze hosta, aby wysyłać 20 miganie wiadomości do Twojej Pi z Twoim Centrum IoT.

Po uruchomieniu aplikacji przykładowych uruchamiania słuchanie wiadomości z Twoim Centrum IoT. W międzyczasie zadania gulp wyśle odbiorcy kilka komunikatów "miganie" z Twoim Centrum IoT do Pi. Dla każdej odebranej wiadomości miganie przykładowej aplikacji wywołuje funkcję blinkLED do miganie LED.

Powinien zostać wyświetlony LED miganie co dwie sekundy jako zadania gulp wysyłane są 20 wiadomości z Twoim Centrum IoT do swojego Pi. Ostatnie jest komunikat "Przestań" zachęcający aplikacjom przestaną działać.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 podsumowanie

Zostały pomyślnie wysłane wiadomości z Twoim Centrum IoT do swojego Pi do miganie LED. Następnej sekcji jest opcjonalna sekcja, w którym pokazano, jak zmienić włączać i wyłączać zachowanie LED.

## <a name="next-steps"></a>Następne kroki

[Sekcja opcjonalna: Zmienianie włączać i wyłączać zachowanie LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)