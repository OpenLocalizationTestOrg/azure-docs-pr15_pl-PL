<properties
 pageTitle="Skonfiguruj urządzenie | Microsoft Azure"
 description="Konfigurowanie usługi malina Pi 3 pierwsze i zainstaluj system operacyjny Raspbian bezpłatne systemu operacyjnego, który jest zoptymalizowana pod kątem sprzętu malina Pi."
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

# <a name="11-configure-your-device"></a>1.1 Skonfiguruj urządzenie

## <a name="111-what-you-will-do"></a>1.1.1 co wykonasz

Konfigurowanie usługi Pi do pierwszego użycia czasu i instalowanie systemu operacyjnego Raspbian, bezpłatne systemu operacyjnego, który jest zoptymalizowana pod kątem sprzętu malina Pi. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2 co dowiesz się

W tej sekcji przedstawiono:

- Jak zainstalować Raspbian na swojej Pi
- Jak włączenia usługi Pi za pomocą kabla USB
- Sposób nawiązywania połączenia z siecią za pomocą kabla Ethernet lub sieci Wi-Fi do Pi
- Jak dodać LED do breadboard i łączenie się z Pi

## <a name="113-what-you-need"></a>1.1.3 co jest potrzebne

Do wykonania w tej sekcji, potrzebne następujące elementy w zestawie Starter malina Pi 3:

- Tablica malina Pi 3
- Karta MicroSD 16GB
- Power 2A 5 v dostarczyć sześć stopy micro kabla USB
- Breadboard
- Kable łącznika
- Opornik 560 Ohm
- Rozproszone 10mm LED
- Kabla Ethernet

![Elementy w zestawie Starter](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Należy również:

- Połączenie przewodowe lub bezprzewodowe dla swojego Pi nawiązać połączenie
- USB SD karty mini SD karta lub nagrać obraz systemu operacyjnego do karty MicroSD.
- Komputer z systemem Windows, Mac lub Linux. Komputer jest używany do instalowania Raspbian na karcie MicroSD.
- Połączenie internetowe, aby pobrać niezbędne narzędzia i oprogramowania

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 zainstalować Raspbian na karcie MicroSD

Przygotowywanie karty MicroSD zapisać obraz Raspbian.

1. Pobierz Raspbian.
  1. [Pobierz](https://www.raspberrypi.org/downloads/raspbian/) plik zip dla Joasia Raspbian o jeden piksel.
  2. Wyodrębnianie obrazu Raspbian do folderu na komputerze.
2. Zainstaluj Raspbian do karty MicroSD.
  1. [Pobierz](https://www.etcher.io) i zainstaluj narzędzie nagrywarka karty Etcher SD.
  2. Uruchom Etcher i wybierz obraz Raspbian wyodrębnieniu w kroku 1.
  3. Wybierz dysk karty MicroSD.
    Uwaga: Etcher został już zaznaczony poprawny dysk.
  4. Kliknij przycisk Flash do zainstalowania Raspbian do karty MicroSD.
  5. Usuwanie karty MicroSD z komputera po zakończeniu.
    Uwaga: To bezpiecznie usunąć kartę MicroSD bezpośrednio, ponieważ Etcher automatycznie wysuwa lub odinstalowuje karty MicroSD po zakończeniu.
  6. Włóż kartę MicroSD do swojego Pi.

![Włóż kartę SD](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 Włącz do Pi

Włącz usługi Pi za pomocą kabla USB micro i zasilania.

![Włącz](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Ważne jest, aby użyć zasilania w zestawie, co najmniej 2A, aby upewnić się, że usługi malina jest pobierany z za mało możliwości działa prawidłowo.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 połączenia z siecią Twojej malina Pi 3

Sieci przewodowej lub do sieci bezprzewodowej, można dołączyć do Pi. Upewnij się, że do tej samej sieci co ten komputer jest połączony z Pi. Na przykład można nawiązać z Pi ten sam przełącznik, że komputer jest połączony.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 nawiązywanie połączenia z sieci przewodowej

Nawiązywanie połączenia z Pi z sieci przewodowej za pomocą kabla Ethernet. Dwa LED na swojej Pi Włącz, jeśli nawiązaniu połączenia.

![Nawiązywanie połączenia za pomocą kabla Ethernet](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 połączyć się z siecią

Postępuj zgodnie z [instrukcjami](https://www.raspberrypi.org/learning/software-guide/wifi/) z Foundation Pi malina nawiązywania połączenia z sieci bezprzewodowej do Pi. Te instrukcje trzeba najpierw połączyć monitor i klawiatury do Pi.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 nawiązać do Pi LED

Aby wykonać to zadanie, użyj [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), kable łącznik LED i opornik. Łączenie ich z porty [ogólnego przeznaczenia wyjścia](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) do pi. 

![Breadboard, LED i rezystor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Podłącz krótszej gałęzi LED do **GND GPIO (Pin 6)**.
2. Podłącz dłużej gałęzi LED do jednego nogi opornik.
3. Połącz inne nogi opornik **GPIO**4 (Pin 7).

Należy zauważyć, że biegunowość LED jest ważne. To ustawienie biegunowości nazywaną aktywne niski.

![Szablon](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Gratulacje! Został pomyślnie skonfigurowany do Pi.

## <a name="118-summary"></a>1.1.8 podsumowanie

W tej sekcji znasz sposobu konfigurowania usługi Pi dzięki zainstalowaniu Raspbian, połączenia z siecią Twojej Pi i LED nawiązywanie połączenia z Pi. Zauważ, że LED jeszcze nie podświetlony. W następnej sekcji możesz zainstalować niezbędne narzędzia i oprogramowanie przygotowania klastrze przykładowej aplikacji do Pi.

![Sprzętowe jest gotowy](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Następne kroki

[1.2 Pobierz narzędzia](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
