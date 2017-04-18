<properties
   pageTitle="Podłącz urządzenie za pomocą C na mbed | Microsoft Azure"
   description="Opisano, jak podłączyć urządzenie pakietu IoT Azure wstępnie zdalnego monitorowania rozwiązanie przy użyciu aplikacji napisana C uruchomione na urządzeniu z systemem mbed."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Podłącz urządzenie do zdalnego monitorowania rozwiązanie wstępnie (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Tworzenie i uruchamianie rozwiązanie przykładowe C

Poniższe instrukcje przedstawiają uzyskać procedurę nawiązywania połączenia [z obsługą mbed FRDM K64F Freescale] [ lnk-mbed-home] urządzenia do zdalnego monitorowania rozwiązania.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Podłącz urządzenie mbed na komputerze sieci i pulpitu

1. Podłącz urządzenie mbed z siecią za pomocą kabla Ethernet. Ten krok jest konieczny, ponieważ przykładowej aplikacji wymaga dostęp do Internetu.

2. Zobacz [Wprowadzenie do mbed] [ lnk-mbed-getstarted] do nawiązywania połączenia między urządzeniem mbed Komputerem stacjonarnym.

3. Jeśli Komputerem stacjonarnym systemu Windows, zobacz [Konfiguracja komputera] [ lnk-mbed-pcconnect] konfigurowania portu szeregowego dostępu do urządzenia mbed.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Tworzenie projektu mbed i zaimportować przykładowy kod

1. W przeglądarce sieci web przejdź do mbed.org [Witryna dewelopera](https://developer.mbed.org/). Jeśli nie masz jeszcze, pojawi się opcja, aby utworzyć konto (jest to bezpłatne). W przeciwnym razie Zaloguj się przy użyciu poświadczeń konta. Następnie kliknij pozycję **kompilatora** w prawym górnym rogu strony. Ta akcja powoduje interfejsem *obszaru roboczego* .

2. Upewnij się, że platformy sprzętowej, którego używasz, jest wyświetlany w prawym górnym rogu okna, lub kliknij ikonę w prawym rogu, aby zaznaczyć platformy sprzętowej.

3. Kliknij przycisk **Importuj** w menu głównym. Następnie kliknij pozycję **kliknij tutaj** , aby zaimportować z adresu URL łącza obok mbed logo kuli ziemskiej.

    ![][6]

4. W oknie podręcznym Wprowadź łącze dla próbki https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ kod, a następnie kliknij przycisk **Importuj**.

    ![][7]

5. W oknie kompilatora mbed widać, że importowania tego projektu importuje także różnych bibliotek. Niektóre są dostarczane i obsługiwane przez zespół Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a inne są dostępne w katalogu bibliotek mbed bibliotek innych firm.

    ![][8]

6. Otwórz plik remote_monitoring\remote_monitoring.c i zlokalizuj poniższy kod w pliku:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Zastąp [identyfikator urządzenia] i [klucza urządzenia] z danymi urządzenia umożliwiające przykładowego programu nawiązywania połączenia z Twoim Centrum IoT. Użyj Hostname Centrum IoT, aby zamienić [nazwa IoTHub] i [IoTHub sufiks, to znaczy azure devices.net] symboli zastępczych. Na przykład jeśli nazwa hosta usługi IoT Centrum jest **contoso.azure devices.net**, **contoso** jest **hubName** i wszystko po **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Szczegółową kodu

Jeśli interesuje Cię działania programu, w tej sekcji opisano niektóre kluczowe części przykładowy kod. Jeśli chcesz uruchomić kod, przejdź do [tworzenia i uruchamiania programu](#buildandrun).

#### <a name="defining-the-model"></a>Definiowanie modelu

W przykładzie użyto [serializatora] [ lnk-serializer] biblioteki, aby zdefiniować modelu, który określa urządzenie można wysłać do Centrum IoT i odbieranych z Centrum IoT wiadomości. W tym przykładzie nazw **Contoso** definiuje modelu **termostat** , który określa dane telemetrycznego **temperatury**, **ExternalTemperature**i **wilgotności** wraz z metadanych, takie jak identyfikator urządzenia, właściwości urządzenia i polecenia, które odpowiada urządzenia:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Związanych z definicją modelu są definicje polecenia **SetTemperature** i **SetHumidity** , które urządzenie odpowiada:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Łączenie modelu do biblioteki

Funkcje **sendMessage** i **IoTHubMessage** są standardowy kod do wysyłania telemetrycznego z urządzenia i nawiązywanie połączeń z wiadomości z Centrum IoT obsługi polecenia.

#### <a name="the-remotemonitoringrun-function"></a>Funkcja remote_monitoring_run

Funkcja **głównego** programu wywołuje funkcję **remote_monitoring_run** podczas uruchamiania aplikacji do wykonania zachowanie urządzenia jako centrum IoT klienta urządzenia. Ta funkcja **remote_monitoring_run** głównie składa się z par zagnieżdżone funkcje:

- **platformy\_inicjowania** i **platformy\_deinit** wykonywania operacji inicjowanie i zamknięcie specyficzne dla platformy.
- **Serializator\_inicjowania** i **serializatora\_deinit** inicjowania i usuwania zainicjować biblioteki serializatora.
- **IoTHubClient\_tworzenie** i **IoTHubClient\_Destroy** tworzenie uchwyt klienta **iotHubClientHandle**, przy użyciu poświadczeń urządzenia do łączenia się z Twoim Centrum IoT.

W sekcji głównej funkcji **remote_monitoring_run** program wykonuje następujące operacje za pomocą uchwytu **iotHubClientHandle** :

- Tworzy wystąpienie modelu termostat Contoso i konfiguruje zwrotne wiadomości dla dwóch poleceń.
- Wysyła informacji o urządzeniu, łącznie z poleceniami, który obsługuje on do Twojej koncentratora IoT za pośrednictwem biblioteki serializatora. Centrum otrzymuje tę wiadomość, zmienia stan urządzenia na pulpicie nawigacyjnym z **Oczekujące** **uruchomiony**.
- Zostanie uruchomiony pętli **podczas** wysyła temperatury, zewnętrznych temperatury i wilgotności wartości do koncentratora IoT co sekundę.

Odwołanie Oto przykładowa wiadomość **DeviceInfo** wysyłane do Centrum IoT podczas uruchamiania:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Odwołanie Oto przykładowa wiadomość **telemetrycznego** wysyłane do Centrum IoT:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Odwołanie Oto przykładowa otrzymania z Centrum IoT **polecenia** :

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Tworzenie i uruchamianie programu

1. Kliknij polecenie **skompilować** do utworzenia programu. Można bezpiecznie zignorować ostrzeżenia, ale jeśli kompilacja generuje błędy, należy naprawić je przed kontynuowaniem.

2. Jeśli kompilacja zakończyło się pomyślnie, kompilatora mbed witryny sieci Web generuje plik bin z nazwą projektu i pobiera go na komputerze lokalnym. Skopiuj plik bin na urządzeniu. Zapisywanie pliku bin na urządzeniu powoduje, że urządzenie, aby ponownie uruchomić i uruchomić program zawartych w pliku bin. Można ręcznie uruchomić ponownie program w dowolnym momencie, naciskając przycisk Resetuj na tym urządzeniu mbed.

3. Nawiązywanie połączenia urządzeń za pomocą aplikacji klienckiej SSH, takich jak Kit. Można określić portu szeregowego używanego urządzenia, zaznaczając pole wyboru Menedżer zadań systemu Windows.

    ![][11]

4. W Kit kliknij **szeregowego** typu połączenia. Urządzenie zazwyczaj łączy się transmisji 9600, dlatego należy wprowadzić 9600 w polu **szybkość** . Następnie kliknij przycisk **Otwórz**.

5. Wykonywanie uruchamiania programu. Może być konieczne Resetowanie tablicy (naciśnij klawisze CTRL + podziału lub naciśnij przycisk Resetuj tablicy) Jeśli program nie rozpocznie się automatycznie po połączeniu.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
