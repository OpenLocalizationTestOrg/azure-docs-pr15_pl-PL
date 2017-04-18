<properties
   pageTitle="Podłącz urządzenie za pomocą Node.js | Microsoft Azure"
   description="Opisano, jak podłączyć urządzenie pakietu IoT Azure wstępnie zdalnego monitorowania rozwiązanie przy użyciu aplikacji napisana Node.js."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Podłącz urządzenie do zdalnego monitorowania rozwiązanie wstępnie (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Tworzenie i uruchamianie node.js rozwiązanie próbki

1. Aby klonowanie repozytorium GitHub *Microsoft Azure IoT SDK* i zainstalować w środowisko pulpitu *urządzeń Microsoft Azure IoT SDK dla Node.js* , wykonaj [przygotowania środowiska programowania] [ lnk-github-prepare] instrukcje.

2. Z lokalną kopię [azure-iot SDK] [ lnk-github-repo] repozytorium, pliki kopii następujące dwie z folderu próbki węzeł urządzenia do folderu na urządzeniu:

  - Packages.JSON
  - remote_monitoring.js

3. Otwórz plik remote_monitoring.js i odszukaj zmienną następujące czynności:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Zastąp ciąg połączenia urządzenia **[parametry połączenia urządzenia Centrum IoT]** . Można znaleźć wartości do koncentratora IoT hostname, identyfikator urządzenia i klucza urządzenia zdalnego monitorowania pulpicie nawigacyjnym rozwiązanie. Parametry połączenia urządzenia ma następujący format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Jeśli nazwa hosta usługi Centrum IoT jest **contoso** i swój identyfikator urządzenia jest **mydevice**, ciąg połączenia będzie wyglądał:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Zapisz plik. Uruchom następujące polecenia w wierszu polecenia w folderze, który zawiera te pliki, aby zainstalować pakiety to konieczne, a następnie uruchomić przykładowej aplikacji:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md