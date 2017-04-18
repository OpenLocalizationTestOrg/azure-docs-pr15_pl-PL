<properties
 pageTitle="Odczytywanie wiadomości są zachowywane w magazynie Azure | Microsoft Azure"
 description="Monitorowanie wiadomości urządzenia w chmurze, zgodnie z ich zapisaniu w Twoim magazynie tabel platformy Azure."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 czytać wiadomości są zachowywane w magazynie Azure

## <a name="331-what-will-you-do"></a>3.3.1 co będzie zrobić

Monitorowanie wiadomości urządzenia w chmurze, które są wysyłane z Twojej malina Pi 3 z Centrum IoT jako wiadomości są zapisywane w Twoim magazynie tabel platformy Azure. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 będzie zdobyte informacje

- Jak używać zadań czytanie wiadomości gulp do czytania wiadomości są zachowywane w Twoim magazynie tabel platformy Azure.

## <a name="333-what-do-you-need"></a>3.3.3 co to są potrzebne

- Możesz pomyślnie ukończona poprzedniej sekcji, [Uruchom aplikację przykładowe Azure miganie na swojej malina Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 odczytywanie wiadomości z Twojego konta miejsca do magazynowania

W poprzedniej sekcji uruchomieniu aplikacji przykładowej na do Pi. Wiadomości wysyłane aplikacji przykładowych na Twoim Centrum Azure IoT. Wiadomości wysyłane do Twojej Centrum IoT są przechowywane w magazynie tabel platformy Azure za pośrednictwem aplikacji Azure funkcji. Potrzebujesz parametry połączenia magazyn Azure do czytania wiadomości z Twoim magazynie tabel platformy Azure.

Aby odczytać wiadomości przechowywane w Twoim magazynie tabel platformy Azure, wykonaj następujące czynności:

1. Pobieranie parametrów połączenia, uruchamiając następujące polecenia:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Pobiera pierwszego polecenia `storage name` używaną w drugim poleceniu dotyczy uzyskiwania parametrów połączenia. `iot-sample`jest to wartość domyślna `{resource group name}` nie zmiany wartości Lekcja 2.

2. Otwórz plik konfiguracji `config-raspberrypi.json` pliku w kodzie programu Visual Studio, uruchamiając następujące polecenie:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Zamienianie `[Azure Storage connection string]` przy użyciu parametrów połączenia, masz w kroku 1.
4. Zapisywanie `config-raspberrypi.json` pliku.
5. Ponownego wysłania wiadomości i przeczytaj ich z Twoim magazynie tabel platformy Azure, uruchamiając następujące polecenie:

    ```bash
    gulp run --read-storage
    ```

    Logika dotycząca odczytu z magazynu tabel platformy Azure znajduje się w `azure-table.js` pliku.

    ![Uruchamianie — gulp miejsca do magazynowania odczytu](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 podsumowanie

Zostały pomyślnie połączony z Pi Twoim Centrum IoT w chmurze i używane miganie przykładowej aplikacji do wysyłania wiadomości urządzenia w chmurze. Funkcja Azure aplikacji jest także używany do przechowywania wiadomości przychodzących Centrum IoT z magazynem tabel platformy Azure. Na można przenosić do następnej lekcji na temat wysyłania wiadomości w chmurze do urządzenia z Twoim Centrum IoT do swojego Pi.

## <a name="next-steps"></a>Następne kroki

[Lekcja 4: Wysyłanie wiadomości w chmurze do urządzenia](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
