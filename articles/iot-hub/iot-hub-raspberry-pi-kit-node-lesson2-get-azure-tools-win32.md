<properties
 pageTitle="Uzyskuj dostęp do narzędzi Azure (Windows 7 +) | Microsoft Azure"
 description="Zainstaluj Python i interfejs wiersza polecenia Azure (polecenie Azure) w systemie Windows 7 i nowszych wersjach."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 uzyskać dostęp do narzędzi Azure (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 co wykonasz

Zainstaluj Python i Azure wiersza polecenia interfejsu (polecenie Azure). Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 co dowiesz się

- Jak zainstalować Python.
- Jak zainstalować polecenie Azure.

## <a name="213-what-you-need"></a>2.1.3 co jest potrzebne

- Dla systemu Windows z połączeniem internetowym
- Aktywną subskrypcję Azure. Jeśli nie masz konta, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut.

## <a name="214-install-python"></a>2.1.4 Instalowanie Python

Zainstaluj Python na komputerze z systemem Windows. Możesz zainstalować Python 2.7, 3.4 lub 3.5. Ten samouczek jest oparty na Python 2.7. Jeśli masz już zainstalowaną Python, przejdź do sekcji 2.1.5.

[Przywracanie Python dla systemu Windows](https://www.python.org/downloads/)

Należy dodać ścieżkę foldery, w którym python.exe i pip.exe są zainstalowane w systemie `PATH` zmiennej środowiska. Domyślnie python.exe jest zainstalowany w `C:\Python27` i pip.exe jest instalowany w `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 instalowanie polecenie Azure

Polecenie Azure udostępnia wiele platform środowiska wiersza polecenia Azure, co pozwala pracować bezpośrednio w wierszu polecenia zainicjować obsługę dla zasobów i zarządzanie nimi.

Aby zainstalować polecenie Azure, wykonaj następujące czynności:

1. Otwórz okno wiersza polecenia jako administrator.
2. Uruchom następujące polecenia:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Sprawdź instalację, uruchamiając następujące polecenie:

    ```bash
    az iot -h
    ```

Jeśli instalacja powiedzie się, zobacz następujący wynik.

![w-m iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 podsumowanie

Po zainstalowaniu polecenie Azure. Przejdź do następnej sekcji, aby utworzyć tożsamość użytkownika Centrum IoT Azure i urządzenia, za pomocą interfejsu wiersza polecenia Azure.

## <a name="next-steps"></a>Następne kroki

[2.2 utworzyć Twoim Centrum IoT i zarejestrować usługi malina Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
