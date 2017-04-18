<properties
 pageTitle="Uruchamianie aplikacji przykładowej wysyłania wiadomości urządzenia w chmurze | Microsoft Azure"
 description="Wdrażanie i uruchamianie aplikacji przykładowej usługi 3 Pi malina wysyła wiadomości do Centrum IoT i miga LED."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 uruchomić aplikację próbki do wysyłania wiadomości urządzenia w chmurze

## <a name="321-what-you-will-do"></a>3.2.1 co wykonasz

Wdrażanie i uruchamianie aplikacji próbki w swojej malina Pi 3 wysyłania wiadomości do Twojej Centrum IoT. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 co dowiesz się

- Jak korzystać z narzędzia gulp wdrażanie i uruchamianie aplikacji Node.js próbki w swojej Pi.

## <a name="323-what-you-need"></a>3.2.3 co jest potrzebne

- Musisz została pomyślnie ukończona poprzedniej sekcji w tej lekcji: [Tworzenie aplikacji funkcji Azure i konta magazyn Azure w celu przetwarzania i przechowywania IoT Centrum wiadomości](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 przejść do parametry połączenia IoT, jak Centrum i urządzenia

Parametry połączenia urządzenia umożliwia łączenie Pi z Twoim Centrum IoT. Centrum IoT ciąg połączenia jest używany do łączenia z Centrum IoT reprezentujący do Pi w Centrum IoT tożsamości urządzenia.

- Pobieranie parametrów połączenia Centrum IoT, uruchamiając następujące polecenie Azure polecenie:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`jest to nazwa określony w lekcji 2. Używanie `iot-sample` jako wartość `{resource group name}` nie zmiany wartości Lekcja 2.

- Pobieranie parametrów połączenia urządzenia, uruchamiając następujące polecenie:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`Przejście tę samą wartość co używane z poprzedniego polecenia. Za pomocą `iot-sample` jako wartość `{resource group name}` i używanie `myraspberrypi` jako wartość `{device id}` nie zmiany wartości Lekcja 2.

## <a name="325-configure-the-device-connection"></a>3.2.5 skonfiguruje połączenie urządzenia

1. Inicjowanie pliku konfiguracji, uruchamiając następujące polecenia:

    ```bash
    npm install
    gulp init
    ```

2. Otwórz plik konfiguracji urządzenia `config-raspberrypi.json` programu Visual Studio kodu, uruchamiając następujące polecenie:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Wprowadź następujące zamiany w `config-raspberrypi.json` pliku:

  - Zastąp **[adres IP lub nazwa hosta urządzenia]** adres IP urządzenia lub nazwa hosta masz od `device-discovery-cli` lub wartość odziedziczone skonfigurowane Lekcja 1.
  - Zamienianie **[parametry połączenia urządzenia IoT]** z `device connection string` możesz uzyskane w wyniku.
  - Zamienianie **[parametry połączenia Centrum IoT]** z `iot hub connection string` możesz uzyskane w wyniku.

Możesz zaktualizować `config-raspberrypi.json` pliku, aby może wdrożyć aplikację próbki ze swojego komputera.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 wdrażanie i uruchomić aplikację próbki

Wdrażanie i uruchamianie aplikacji próbki w swojej Pi, uruchamiając następujące polecenie:

```bash
gulp
```

> [AZURE.NOTE] Zadania gulp domyślne `install-tools`, `deploy`, i `run` kolejno zadania. W [lekcji 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)uruchomiono te zadania po kolei oddzielnie.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 Sprawdź działa aplikacja próbki

Powinien zostać wyświetlony LED, których jest połączony z Pi miganie co dwie sekundy. Za każdym razem, gdy LED miga, przykładowej aplikacji wysyła wiadomości do Twojej Centrum IoT i sprawdza, czy wiadomości zostały pomyślnie wysłane do Twojej koncentratora IoT. Ponadto dla każdej wiadomość odbierana przez Centrum IoT wiadomość jest drukowana w oknie konsoli. Przykładowa aplikacja kończy się automatycznie po wysłaniu wiadomości 20.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 podsumowanie

Został wdrożony i przeprowadzić nowej aplikacji przykładowych miganie do Pi do wysyłania wiadomości urządzenia w chmurze do usługi Centrum IoT. Możesz przenieść do następnej sekcji można monitorować wiadomości są zapisywane na koncie magazyn Azure.

## <a name="next-steps"></a>Następne kroki

[3.3 czytać wiadomości są zachowywane w magazynie Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)