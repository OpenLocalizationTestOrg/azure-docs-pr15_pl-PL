<properties
 pageTitle="Wprowadzenie do Pi malinowe 3 | Microsoft Azure"
 description="Rozpoczynanie pracy z malina Pi 3, utworzenie Centrum IoT usługi Azure i podłącz do Pi do koncentratora IoT"
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

# <a name="get-started-with-raspberry-pi-3"></a>Wprowadzenie do Pi malinowe 3

W tym samouczku zacznij od nauki podstawy pracy z malina Pi 3 tej uruchomionego Raspbian. Możesz następnie Dowiedz się, jak w łatwy sposób połączyć urządzenia w chmurze z [Centrum IoT Azure](iot-hub-what-is-iot-hub.md). Windows 10 IoT Core próbkach można znaleźć w [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Lekcja 1: Konfigurowanie urządzenia

![Diagram E2E Lekcja 1](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

W tej lekcji Skonfiguruj urządzenie malina Pi 3 z systemem operacyjnym, konfigurowanie środowiska programowania i wdrażania aplikacji do Pi.

### <a name="configure-your-device"></a>Konfigurowanie urządzenia

Konfigurowanie usługi malina Pi 3 do użytku po raz pierwszy i zainstaluj system operacyjny Raspbian bezpłatne systemu operacyjnego, który jest zoptymalizowana pod kątem sprzętu malina Pi.

*Szacowany czas trwania: 30 minut* 

[Przejdź do "Skonfiguruj urządzenie"](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Pobierz narzędzia
Pobierz narzędzia i oprogramowanie do tworzenia i wdrażania pierwszej aplikacji dla malina Pi 3.

*Szacowany czas trwania: 20 minut* 

[Przejdź do pozycji "Pobierz narzędzia"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Tworzenie i wdrażanie aplikacji miganie

Klonowanie przykładowej aplikacji Node.js z Github i gulp do wdrożenia tę aplikację do Twojej tablicy malina Pi 3. Ta aplikacja przykładowa miga LED podłączony do tablicy co dwie sekundy.

*Szacowany czas trwania: 5 minut* 

[Przejdź do pozycji "Tworzenie i wdrażanie aplikacji miganie"](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekcja 2: Tworzenie Twoim Centrum IoT

![Diagram E2E lekcji 2](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

W tej lekcji utworzyć bezpłatne konto Azure, obsługi administracyjnej usługi Centrum Azure IoT i utworzyć pierwszy urządzenia w Centrum Azure IoT.

Wykonywanie Lekcja 1, przed rozpoczęciem tej lekcji.

### <a name="get-the-azure-tools"></a>Pobierz narzędzia Azure

Instalowanie Azure interfejs wiersza polecenia (polecenie Azure).

*Szacowany czas trwania: 10 minut* 

[Przejdź do pozycji pobieranie Azure narzędzia.](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Tworzenie Twoim Centrum IoT i zarejestrować usługi malina Pi 3

Tworzenie grupy zasobów, obsługi administracyjnej usługi pierwszego Centrum Azure IoT i dodawanie pierwszego urządzenia do Centrum IoT Azure za pomocą interfejsu wiersza polecenia Azure. 

*Szacowany czas trwania: 10 minut* 

[Przejdź do "Tworzenie Twoim Centrum IoT i zarejestrować usługi malina Pi 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Lekcja 3: Wysyłanie wiadomości urządzenia w chmurze

![Diagram E2E lekcji 3](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

W tej lekcji wiadomości wysyłanych z usługi Pi na Twoim Centrum IoT. Można także tworzyć aplikacji Azure funkcja przejmuje wiadomości przychodzące od Twoim Centrum IoT i zapisuje je z magazynem tabel platformy Azure.

Przed rozpoczęciem tej lekcji wykonaj lekcji 1 i 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Tworzenie aplikacji funkcji Azure i konta magazynu platformy Azure

Menedżer zasobów Azure szablon do tworzenia aplikacji funkcji Azure i konto Azure miejsca do magazynowania.

*Szacowany czas trwania: 10 minut* 

[Przejdź do "Tworzenie Azure funkcji aplikacji i Magazyn Azure konta"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Uruchamianie aplikacji przykładowej wysyłania wiadomości urządzenia w chmurze

Wdrażanie i uruchamianie aplikacji przykładowej do wysyłania wiadomości do Centrum IoT urządzenia malina Pi 3.

*Szacowany czas trwania: 10 minut* 

[Przejdź do "Uruchom przykładowej aplikacji do wysyłania wiadomości urządzenia w chmurze"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Odczytywanie wiadomości są zachowywane w magazynie Azure
Monitorowanie wiadomości urządzenia w chmurze, zgodnie z ich zapisaniu w Twoim magazynie Azure.

*Szacowany czas trwania: 5 minut* 

[Przejdź do "Czytanie wiadomości zachowywane w magazynie Azure"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Lekcja 4: Wysyłanie wiadomości w chmurze do urządzenia

![Diagram E2E lekcji 4](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

W tej lekcji demos jak wysyłać wiadomości z Twoim Centrum Azure IoT do swojego malina Pi 3. Wiadomości sterowanie włączać i wyłączanie zachowanie LED, których jest połączony z Pi. Przykładowa aplikacja jest gotowa umożliwiające wykonania tego zadania.

Wykonywanie lekcji 1, 2 i 3 przed rozpoczęciem tej lekcji.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Uruchom aplikację przykładowe otrzymywać wiadomości w chmurze do urządzenia

Aplikacja przykładowa w 4 lekcji działa na swojej Pi i monitoruje wiadomości przychodzące od Twoim Centrum IoT. Nowe zadanie gulp wysyłać wiadomości do Twojej Pi z Twoim Centrum IoT do miganie LED.

*Szacowany czas trwania: 10 minut* 

[Przejdź do "Uruchom przykładowej aplikacji otrzymywać wiadomości w chmurze do urządzenia"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Sekcja opcjonalna: Zmienianie włączać i wyłączać zachowanie LED

Dostosowywanie wiadomości, aby zmienić LED Włączanie i wyłączanie funkcji zachowanie.

*Szacowany czas trwania: 10 minut* 

[Przejdź do pozycji "sekcja opcjonalna: Zmienianie włączać i wyłączać zachowanie LED"](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli są spełnione wszystkie troubles podczas lekcji, może zwrócić rozwiązań na tej stronie.

[Przejdź do sekcji "Rozwiązywanie problemów"](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
