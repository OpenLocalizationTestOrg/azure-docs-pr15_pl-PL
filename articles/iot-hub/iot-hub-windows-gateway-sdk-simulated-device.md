<properties
    pageTitle="Symulacja urządzenia z SDK bramy IoT | Microsoft Azure"
    description="Ilustrowanie wysyłanie telemetrycznego z urządzeniem symulowane przy użyciu zestawu SDK bramy IoT Azure za pomocą systemu Windows Azure Instruktaż IoT SDK bramy."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT SDK bramy (beta) — wysyłanie wiadomości urządzenia w chmurze z urządzeniem symulowane przy użyciu systemu Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Tworzenie i uruchamianie próbki

Przed rozpoczęciem należy:

- [Konfigurowanie środowiska programowania] [ lnk-setupdevbox] do pracy z zestawu SDK w systemie Windows.
- [Tworzenie Centrum IoT] [ lnk-create-hub] w ramach subskrypcji Azure należy nazwę Twoim Centrum do przeprowadzenia tego instruktażu. Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- Dodawanie dwóch urządzeń do Twoim Centrum IoT i zanotuj ich identyfikatory i kluczy urządzenia. Możesz użyć [Eksploratora urządzenia lub Eksploratora iothub] [ lnk-explorer-tools] narzędzie, aby dodać urządzenia do Centrum IoT utworzony w poprzednim kroku i pobieranie klucze.

Aby utworzyć próbki:

1. Otwórz wiersz polecenia **wiersza polecenia Deweloper VS2015** .
2. Przejdź do folderu głównego w kopii lokalnej repozytorium **azure-iot brama sdk** .
3. Uruchamianie **narzędzia\\build.cmd** skrypt. Ten skrypt tworzy plik rozwiązania Visual Studio, tworzy rozwiązanie i uruchamiania testów. Rozwiązania programu Visual Studio można znaleźć w folderze **Tworzenie** lokalnej kopii repozytorium **azure-iot brama sdk** .

Aby uruchomić przykład:

Otwórz plik w edytorze tekstów, **próbki\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** w Twojej kopii lokalnej repozytorium **azure-iot brama sdk** . Ten plik konfiguruje modułów w bramy przykładowe:

- Moduł **IoTHub** nawiązuje połączenie z Twoim Centrum IoT. Skonfiguruj go wysyłanie danych do swojego Centrum IoT. W szczególności wartość **IoTHubName** nazwę Twoim Centrum IoT i ustaw wartość **IoTHubSuffix** **azure devices.net**. Ustaw wartość **transportu** do jednego z: "HTTP", "AMQP" lub "MQTT". Należy zauważyć, że obecnie tylko "HTTP" udostępni jedno połączenie TCP dla wszystkich wiadomości urządzenia. Jeśli ustawiono wartość "AMQP" lub "MQTT" bramy zachowają oddzielnego połączenia TCP do koncentratora IoT dla każdego urządzenia.
- Moduł **mapowania** mapy adresów MAC urządzenia symulowany identyfikatorom urządzenia IoT Centrum. Upewnij się, że wartości **identyfikator urządzenia** zgodne identyfikatory dwóch urządzeń dodanych do Twoim Centrum IoT, oraz że wartości **deviceKey** zawierają kluczy urządzenia dwa.
- Moduły **BLE1** i **BLE2** są symulowany urządzenia. Należy pamiętać o tym, jak ich adresy MAC odpowiadają układowi w module **mapowania** .
- Moduł **rejestratora** rejestruje aktywności bramy w pliku.
- Wartości **ścieżka modułu** , jak pokazano poniżej przyjęto założenie, że sklonowany repozytorium IoT bramy SDK w katalogu głównym dysku **C:** . Jeśli pobrane do innej lokalizacji, należy odpowiednio dostosować wartości **ścieżka modułu** .
- Tablica **łączy** w dolnej części pliku JSON łączy modułów **BLE1** i **BLE2** modułu **mapowania** i modułu **mapowania** w **IoTHub** module. Zapewnia również, że wszystkie wiadomości są rejestrowane w module **rejestratora** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Zapisz zmiany wprowadzone w pliku konfiguracji.

Aby uruchomić przykład:

1. W wierszu polecenia przejdź do folderu głównego lokalną kopię repozytorium **azure-iot brama sdk** .
2. Uruchom następujące polecenie:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Możesz użyć [Eksploratora urządzenia lub Eksploratora iothub] [ lnk-explorer-tools] narzędzie, aby monitorować wiadomości, które Centrum IoT otrzymuje z bramy.


## <a name="next-steps"></a>Następne kroki

Jeśli chcesz uzyskać bardziej zaawansowane opis zestawu SDK bramy IoT i poeksperymentować z przykładami kodu, odwiedź następujące samouczki Deweloper i zasoby:

- [Wysyłanie wiadomości urządzenia w chmurze z rzeczywistą urządzenia z IoT SDK bramy][lnk-physical-device]
- [Azure bramy IoT SDK][lnk-gateway-sdk]

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 