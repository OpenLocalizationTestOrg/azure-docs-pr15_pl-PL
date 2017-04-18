<properties
 pageTitle="Uzyskuj dostęp do narzędzi Azure (macOS 10.10) | Microsoft Azure"
 description="Zainstaluj na macOS Python i interfejs wiersza polecenia Azure (polecenie Azure)."
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

# <a name="21-get-azure-tools-macos-1010"></a>2.1 uzyskać dostęp do narzędzi Azure (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 co wykonasz

Instalowanie Azure interfejs wiersza polecenia (polecenie Azure). Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 co dowiesz się

- Jak zainstalować polecenie Azure.
- Jak dodawać podgrupę IoT polecenie Azure.

## <a name="213-what-you-need"></a>2.1.3 co jest potrzebne

- Mac połączenie z Internetem
- Aktywną subskrypcję Azure. Jeśli nie masz konta, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut.

## <a name="214-install-python"></a>2.1.4 Instalowanie Python

Mimo że macOS zawiera Python 2.7 okno, zaleca się zainstalować Python za pośrednictwem Homebrew. Zobacz [Instalowanie Python na macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Zainstaluj Python i pip, uruchamiając następujące polecenie:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 instalowanie polecenie Azure

Polecenie Azure udostępnia wiele platform środowiska wiersza polecenia Azure, co pozwala pracować bezpośrednio w wierszu polecenia zainicjować obsługę dla zasobów i zarządzanie nimi. 

Aby zainstalować najnowszą polecenie Azure, wykonaj następujące czynności:

1. Uruchom następujące polecenia w oknie końcowych. Może upłynąć pięć minut, aby zainstalować polecenie Azure.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Sprawdź instalację, uruchamiając następujące polecenie:

    ```bash
    az iot -h
    ```
  
Powinien zostać wyświetlony następujący wynik Jeśli Instalacja powiodła się.

![w-m iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 podsumowanie

Po zainstalowaniu polecenie Azure. Przejdź do następnej sekcji, aby utworzyć tożsamość użytkownika Centrum IoT Azure i urządzenia, za pomocą interfejsu wiersza polecenia Azure.

## <a name="next-steps"></a>Następne kroki

[2.2 utworzyć Twoim Centrum IoT i zarejestrować usługi malina Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
