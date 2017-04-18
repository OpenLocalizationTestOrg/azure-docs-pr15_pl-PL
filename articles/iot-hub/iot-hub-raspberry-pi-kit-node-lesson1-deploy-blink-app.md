<properties
 pageTitle="Tworzenie i wdrażanie aplikacji miganie | Microsoft Azure"
 description="Klonowanie przykładowej aplikacji Node.js z Github i gulp do wdrożenia tę aplikację do Twojej tablicy malina Pi 3. Ta aplikacja przykładowa miga LED podłączony do tablicy co dwie sekundy."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 Tworzenie i wdrażanie aplikacji miganie

## <a name="131-what-you-will-do"></a>1.3.1 co wykonasz

Klonowanie przykładowej aplikacji Node.js z Github i wdrażanie aplikacji przykładowych do swojego malina Pi 3 za pomocą narzędzia gulp. Przykładowa aplikacja miga LED podłączony do tablicy co dwie sekundy. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2 co dowiesz się

- Jak używać `device-discover-cli` narzędzie, aby pobrać sieci informacji na temat usługi Pi.
- Jak wdrożyć i przeprowadzić przykładowej aplikacji do Pi.
- Jak wdrożyć i aplikacje zdalnie uruchomionych do Pi.

## <a name="133-what-you-need"></a>1.3.3 co jest potrzebne

Możesz pomyślnie ukończona sekcji Obserwuj Lekcja 1:

- [Konfigurowanie urządzenia](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Pobierz narzędzia](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 uzyskać nazwy adresów i IP usługi Pi

Otwórz wiersz polecenia w systemie Windows lub końcowych okna macOS lub Ubuntu, a następnie uruchom następujące polecenie:

```bash
devdisco list --eth
```

Powinien zostać wyświetlony wynik, który jest podobny do następującego:

![Odnajdowanie urządzeń](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Zwróć uwagę na `IP address` i `hostname` do pi. Potrzebujesz w dalszej części tej sekcji.

> [AZURE.NOTE] Upewnij się, że do tej samej sieci co ten komputer jest połączony z Pi. Na przykład jeśli komputer jest podłączony do sieci bezprzewodowej, podczas Twojej Pi jest podłączony do sieci przewodowej, nie widać adresu IP w wyniku devdisco.

## <a name="135-clone-the-sample-application"></a>1.3.5 klonowanie przykładowej aplikacji

Aby otworzyć przykładowy kod, wykonaj następujące czynności:

1. Klonowanie repozytorium próbki z Github, uruchamiając następujące polecenie:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Otwórz aplikację próbki w kodzie programu Visual Studio, uruchamiając następujące polecenia:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

`app.js` Plików w `app` podfolderu jest plikiem źródłowym klucza, zawierający kod do kontrolki LED.

### <a name="136-install-application-dependencies"></a>1.3.6 instalowanie zależności aplikacji

Zainstaluj bibliotek i innych modułach, potrzebnych dla przykładowej aplikacji, uruchamiając następujące polecenie:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 skonfiguruje połączenie urządzenia

Aby skonfigurować połączenie urządzenia, wykonaj następujące kroki:

1. Generowanie pliku konfiguracji urządzeń, uruchamiając następujące polecenie:

    ```bash
    gulp init
    ```

    Plik konfiguracyjny `config-raspberrypi.json` zawiera poświadczeń służy do logowania do Pi. Aby uniknąć systemu poświadczeń użytkownika, plik konfiguracyjny jest generowany w podfolderze `.iot-hub-getting-started` głównym folderu na komputerze.

2. Otwórz plik konfiguracji urządzenia w kodzie programu Visual Studio, uruchamiając następujące polecenie:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Zamień symbol zastępczy `[device hostname or IP address]` adres IP lub nazwa hosta, który jest wyświetlany w sekcji 1.3.4.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Gratulacje! Utworzono pomyślnie pierwszego przykładowej aplikacji usługi liczby pi.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 wdrażanie i uruchomić aplikację próbki

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 zainstalować Node.js i NPM na swojej Pi

Zainstaluj Node.js i NPM w swojej Pi, uruchamiając następujące polecenie:

```bash
gulp install-tools
```

Może upłynąć dziesięć minut po raz pierwszy wykonać to zadanie.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 wdrażanie i uruchom aplikację próbki

Wdrażanie i uruchamianie aplikacji próbki w, uruchamiając następujące polecenie:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 Sprawdź działa aplikacji

Powinien zostać wyświetlony LED na swojej Pi miganie co dwie sekundy.  Jeśli nie widzisz miganie LED, zobacz [Podręcznik rozwiązywania problemów](iot-hub-raspberry-pi-kit-node-troubleshooting.md) dla rozwiązania typowych problemów.
![Miganie LED](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Używanie `Ctrl + C` aby zakończyć tę aplikację.

## <a name="139-summary"></a>1.3.9 podsumowanie

Zostały zainstalowane narzędzia potrzebne do pracy z Twojej Pi i wdrożyć przykładowej aplikacji do swojego Pi do miganie LED. Możesz teraz można przejść do następnej lekcji do tworzenia, wdrażania i uruchomić innej aplikacji próbki, która umożliwia nawiązywanie połączeń z Pi Centrum IoT Azure, aby wysyłać i odbierać wiadomości.

## <a name="next-steps"></a>Następne kroki

Możesz przystąpić do lekcji 2, który zaczyna się zaczynać [uzyskać dostęp do narzędzi Azure](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
