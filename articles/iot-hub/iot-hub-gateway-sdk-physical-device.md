<properties
    pageTitle="Rzeczywiste urządzenie za pomocą SDK bramy IoT | Microsoft Azure"
    description="Azure Instruktaż IoT SDK bramy przy użyciu urządzenia SensorTag instrumentami Teksas wysyłanie danych do Centrum IoT przez bramę uruchomionych modułu obliczyć Edison firmy Intel"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT SDK bramy (beta) — wysyłanie wiadomości urządzenia w chmurze z urządzeniem rzeczywistą przy użyciu Linux

W tym instruktażu [Przykładowe niskiej energii Bluetooth] [ lnk-ble-samplecode] pokazano, jak używać [Azure IoT bramy SDK] [ lnk-sdk] do przodu telemetrycznego urządzenia w chmurze do koncentratora IoT z urządzenia fizycznego i sposobu kierowania poleceń z Centrum IoT do urządzenia fizycznego.

W tym instruktażu obejmuje:

* **Architektura**: ważne architektury informacji o przykładowych niskiej energii Bluetooth.

* **Tworzenie i uruchamianie**: kroki wymagane do tworzenie i uruchamianie próbki.

## <a name="architecture"></a>Architektura

Instruktaż pokazano, jak utworzyć i uruchomić bramę IoT firmy Intel Edison obliczyć modułu z systemem Linux. Brama jest tworzona przy użyciu zestawu SDK bramy IoT. W przykładzie zastosowano urządzenia SensorTag Teksas instrumentami Bluetooth min. energii (tabela) zbierać dane dotyczące temperatury.

Po uruchomieniu bramy go:

- Nawiązywanie połączeń z urządzenia SensorTag przy użyciu protokołu Bluetooth min. energii (tabela).
- Łączy do koncentratora IoT przy użyciu protokołu HTTP.
- Umożliwia przesłanie dalej telemetrycznego z urządzenia SensorTag do koncentratora IoT.
- Kieruje poleceń z Centrum IoT na urządzeniu SensorTag.

Brama zawiera następujące moduły:

- *Tabela modułu* , który interfejsy z urządzeniem tabela, aby otrzymywać dane dotyczące temperatury z poleceń urządzenia i Wyślij do urządzenia.
- *Tabela chmurze do modułu urządzenia* przetwarzający wiadomości JSON pochodzące z chmury w instrukcji tabela *Tabela modułu*.
- *Moduł rejestratora* , który rejestruje wszystkie komunikaty bramy.
- *Moduł mapowania tożsamości* tłumaczy adresy MAC urządzenia tabela i Centrum IoT Azure urządzenia tożsamości.
- *Moduł Centrum IoT* wydajnie przekazuje dane telemetrycznego do koncentratora IoT i odbiera poleceń urządzenia z Centrum IoT.
- *Tabela drukarki modułu* , który zinterpretowany telemetrycznego z urządzenia tabela i drukowanie sformatowane dane do konsoli umożliwiające rozwiązywanie problemów i debugowania.

### <a name="how-data-flows-through-the-gateway"></a>Sposób przepływu danych przez bramę

Na poniższym diagramie blok przedstawia proces blokowy telemetrycznego przekazywania danych:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Kroki, które elementu telemetrycznego przejście do Centrum IoT podróż z urządzenia tabela są:

1. Urządzenie Tabela generuje próbki temperatury i przesyła go na Bluetooth module tabela w bramy.
2. Moduł tabela otrzyma próbki i publikuje ją do brokera wraz z adres MAC urządzenia.
3. Moduł mapowania tożsamości wybiera ten komunikat i używa wewnętrznej tabeli do tłumaczenia adres MAC urządzenia do tożsamości IoT Centrum urządzenia (identyfikator urządzenia i klucza urządzenia). Następnie publikowania nową wiadomość, która zawiera dane dotyczące temperatury próbki, adres MAC urządzenia, identyfikator urządzenia i klucza urządzenia.
4. Moduł Centrum IoT otrzyma tej nowej wiadomości (generowane przez moduł mapowania tożsamości) i publikuje ją do koncentratora IoT.
5. Moduł rejestratora rejestruje wszystkich wiadomości z brokera pliku na dysku.

Na poniższym diagramie blok przedstawiono potoku przepływu danych polecenia urządzenia:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Moduł Centrum IoT okresowo sprawdza Centrum IoT dla nowych wiadomości polecenia.
2. Gdy moduł Centrum IoT otrzymuje nową wiadomość polecenia, opublikuje go do brokera.
3. Moduł mapowania tożsamości wybiera polecenie wiadomości i używa wewnętrznej tabeli do tłumaczenia Centrum IoT identyfikator urządzenia do urządzenia adres MAC. Następnie publikowania nowej wiadomości, która zawiera adres MAC urządzenia docelowego w planie właściwości wiadomości.
4. Tabela chmurze do modułu urządzenia wybiera ten komunikat i przekształca je w pisane z wielkiej litery instrukcji Tabela moduł tabela. Następnie publikowania nowej wiadomości.
5. Moduł Tabela wybiera ten komunikat i wykonuje instrukcję we/wy przez komunikowania się z urządzeniem tabela.
6. Moduł rejestratora rejestruje wszystkich wiadomości z brokera do pliku.

## <a name="prepare-your-hardware"></a>Przygotowywanie sprzętu

Ten samouczek założono, że używasz [SensorTag instrumentami Teksas](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) podłączeniu do tablicy Edison firmy Intel.

### <a name="set-up-the-edison-board"></a>Konfigurowanie tablicy Edison

Przed rozpoczęciem należy upewnić się, że możesz połączyć urządzenia Edison do sieci bezprzewodowej. Aby skonfigurować urządzenia Edison, należy nawiązać komputera-hosta. Intel zawiera wprowadzenie przewodniki dla następujących systemów operacyjnych:

- [Wprowadzenie do tablicy rozwoju Edison firmy Intel w systemie Windows 64-bitowa][lnk-setup-win64].
- [Wprowadzenie do tablicy rozwoju Edison firmy Intel w systemie Windows w wersji 32-bitowej][lnk-setup-win32].
- [Wprowadzenie do tablicy rozwoju Edison firmy Intel w systemie Mac OS X][lnk-setup-osx].
- [Wprowadzenie do tablicy Edison Intel® na Linux][lnk-setup-linux].

Konfigurowanie urządzenia Edison i zapoznać się z nim, należy wykonać wszystkie kroki opisane w poniższych "wprowadzenie" artykuły oprócz ostatniego kroku "Wybierz pozycję IDE", jest konieczne samouczka bieżącego. Na końcu procesu konfigurowania Edison masz:

- Świeci usługi Edison najnowszą oprogramowania układowego.
- Ustanowić połączenie szeregowe od hosta do Edison.
- Uruchom skrypt **configure_edison** , aby ustawić hasło i włączyć sieci Wi-Fi na Twojej Edison.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Włączanie łączności z urządzeniem SensorTag z płyty Edison

Przed uruchomieniem próbki, należy sprawdzić, czy płyty Edison łączy na urządzeniu SensorTag.

Najpierw należy sprawdzić, czy usługi Edison łączy na urządzeniu SensorTag.

1. Wyłączanie blokowania bluetooth na Edison i sprawdź, czy numer wersji jest **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Wykonanie polecenia **bluetoothctl** . Powłoki bluetooth interakcyjne są teraz. 

3. Wpisz polecenie **Włącz** do potęgi kontroler bluetooth w górę. Powinien zostać wyświetlony dane wyjściowe podobne do:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Nadal w powłoce interakcyjnych bluetooth wpisz polecenie **Skanuj w** wykonać skanowanie w poszukiwaniu urządzenia bluetooth. Powinien zostać wyświetlony dane wyjściowe podobne do:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Urządzenie SensorTag ułatwienia odnajdowania naciskając przycisk małych (zielona LED należy flash). Edison należy wykrywanie urządzenia SensorTag:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    W tym przykładzie widać, że adres MAC urządzenia SensorTag jest **A0:E6:F8:B5:F6:00**.

6. Wyłącz skanowanie wpisując polecenie **Skanuj wyłączyć** .
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Łączenie się z urządzeniem SensorTag przy użyciu adresu MAC wprowadzając **Łączenie \<adres MAC >**. Zwróć uwagę, że jest skrócona Poniższe przykładowe dane wyjściowe:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Uwaga: Można wyświetlić listę cech GATT urządzenia ponownie przy użyciu polecenia **atrybutów listy** .

8. Można teraz odłączyć od urządzenia za pomocą polecenia **odłączyć** , a następnie Zamknij z powłoki bluetooth za pomocą polecenia **Zamknij** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Teraz możesz uruchomić przykład tabela bramy na urządzeniu Edison.

## <a name="run-the-ble-gateway-sample"></a>Uruchamianie przykładowych Tabela bramy

Aby uruchomić przykładowe Tabela na Twojej Edison, musisz wykonać trzy zadania:

- Konfigurowanie dwóch urządzeń próbki w Twoim Centrum IoT.
- Tworzenie zestawu SDK bramy IoT na urządzeniu Edison.
- Konfigurowanie i uruchamianie Przykładowa tabela na urządzeniu Edison.

Podczas pisania SDK bramy IoT obsługuje tylko bram, które moduły tabela w systemie Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurowanie dwóch urządzeń próbki w Twoim Centrum IoT

- [Tworzenie Centrum IoT] [ lnk-create-hub] w ramach subskrypcji Azure należy nazwę Twoim Centrum do przeprowadzenia tego instruktażu. Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- Dodaj jedno urządzenie o nazwie **SensorTag_01** na Twoim Centrum IoT i zanotuj jego klucza identyfikator i urządzenia. Możesz użyć [Eksploratora urządzenia lub Eksploratora iothub] [ lnk-explorer-tools] narzędzia, aby dodać to urządzenie do Centrum IoT utworzony w poprzednim kroku i pobrać jego klucza. To urządzenie będzie mapować na urządzeniu SensorTag podczas konfigurowania bramy.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Tworzenie zestawu SDK bramy IoT na urządzeniu Edison

Wersja **cyfra** na Edsion nie obsługuje submodules. Aby pobrać pełny źródło dla zestawu SDK bramy IoT do Edison masz dwie opcje:

- Opcja #1: Klonowanie [Azure IoT bramy SDK] [ lnk-sdk] repozytorium w swojej Edison i ręcznie klonowanie repozytorium dla każdego submodule.
- Opcja #2: Klonowanie [Azure IoT bramy SDK] [ lnk-sdk] repozytorium na komputerze, gdzie **cyfra** obsługuje submodules, a następnie skopiuj pełną repozytorium z submodules na Twojej Edison.

Jeśli wybierzesz opcję #2, należy użyć następujących poleceń **cyfra** na klonowanie SDK bramy IoT i wszystkie jego submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Następnie należy zip całego lokalnego repozytorium do pojedynczej archiwum, przed skopiowaniem do Edison. Za pomocą narzędzi, takich jak **pscp** jest dołączany do **Kit** , aby skopiować plik archiwum do Edison. Na przykład:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Gdy masz pełną kopię repozytorium IoT SDK bramy na Twojej Edison, można tworzyć przy użyciu następującego polecenia z folderu, który zawiera zestawu SDK:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Konfigurowanie i uruchamianie Przykładowa tabela na urządzeniu Edison

Aby zainicjować i uruchomić próbki, musisz skonfigurować każdym module biorący udział w bramy. Ta konfiguracja znajduje się w pliku JSON i musisz skonfigurować wszystkie pięć moduły uczestniczących. Istnieje przykładowy plik JSON opisane w repozytorium o nazwie **gateway_sample.json** , którego można użyć jako punktu wyjścia do tworzenia pliku konfiguracji. Ten plik jest w folderze **próbki ble_gateway_hl-src** lokalną kopię repozytorium IoT SDK bramy.

W poniższych sekcjach opisano, jak edytować ten plik konfiguracyjny dla próbki tabela i przyjęto założenie, że repozytorium IoT bramy SDK znajduje się w **/home/root/azure-iot-gateway-sdk-** folderu na urządzeniu Edison. Jeśli repozytorium jest w innym miejscu, w związku z tym należy dostosować ścieżki:

#### <a name="logger-configuration"></a>Konfiguracja rejestratora

Przy założeniu repozytorium bramy znajduje się w folderze **/home/root/azure-iot-gateway-sdk-**, moduł rejestratora należy skonfigurować następująco:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Konfiguracja modułu tabela

Konfiguracji próbki dla urządzenia Tabela przyjęto urządzenia SensorTag instrumentami Teksas. Standardowe urządzenie tabela może działać zgodnie z GATT peryferyjnych powinna działać, ale będzie aktualizowany identyfikatorów charakterystycznych GATT i danych (Aby uzyskać instrukcje zapisu). Dodaj adres MAC SensorTag urządzenia: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>Centrum IoT modułu

Dodaj nazwę Twoim Centrum IoT. Wartość sufiksu jest zazwyczaj **azure devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfiguracja modułu mapowania tożsamości

Dodaj adres MAC urządzenia SensorTag i identyfikator urządzenia i klucza urządzenia **SensorTag_01** dodanych w Twoim Centrum IoT:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfiguracja modułu Tabela drukarki

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Konfiguracja routingu

Następujące zapewnia następujące czynności:
- Moduł **rejestratora** odbiera i logowania wszystkich wiadomości.
- Moduł **SensorTag** wysyła wiadomości do **mapowania** i moduły **Tabela drukarki** .
- Moduł **mapowania** wysyła wiadomości do modułu **IoTHub** były wysyłane w górę do Twoim Centrum IoT.
- Moduł **IoTHub** wysyła wiadomości do modułu **mapowania** .
- Moduł **mapowania** wysyła wiadomości do modułu **SensorTag** .

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Aby uruchomić przykład można uruchomić **ble_gateway_hl** binarne przekazywania ścieżkę do pliku konfiguracji JSON. Jeśli użyto pliku **gateway_sample.json** , polecenie do wykonania wygląda następująco:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Może być konieczne mały przycisk na tym urządzeniu SensorTag, aby była odnajdowania przed uruchomieniem próbki.

Po uruchomieniu próbki, możesz użyć [Eksploratora urządzenia lub Eksploratora iothub] [ lnk-explorer-tools] narzędzie, aby monitorować wiadomości bramy przesyłania dalej za pomocą urządzenia SensorTag.

## <a name="send-cloud-to-device-messages"></a>Wysyłanie wiadomości w chmurze do urządzenia

Moduł Tabela obsługuje również wysyłanie instrukcje z poziomu Centrum IoT Azure na urządzeniu. [Azure IoT Centrum urządzenia Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) lub [Eksploratora Centrum IoT](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) służy do wysyłania wiadomości JSON, które moduł bramy Tabela przekazuje do urządzenia tabela. Na przykład jeśli korzystasz z urządzenia SensorTag instrumentami Teksas następnie możesz wysłać następujące komunikaty JSON na urządzeniu z Centrum IoT.

- Resetowanie wszystkich LED i brzęczyka (wyłączyć tę funkcję)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Konfigurowanie we/wy jako "zdalnego"

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Włączanie LED czerwony

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Włączanie zielona LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Włączanie brzęczyka

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Zachowanie domyślne dla urządzenia przy użyciu protokołu HTTP, aby nawiązać połączenie Centrum IoT jest sprawdzenie co 25 minut polecenie Nowy. W związku z tym jeśli wysyłasz kilka oddzielnych poleceń należy czekać 25 minut dla urządzenia, aby otrzymywać kolejnych poleceń.

> [AZURE.NOTE] Brama również sprawdza, czy nowe polecenia przy każdym uruchomieniu, aby wymusić przetwarzania polecenia przez zatrzymywanie i uruchamianie bramy.

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz uzyskać bardziej zaawansowane opis zestawu SDK bramy IoT i poeksperymentować z przykładami kodu, odwiedź następujące samouczki Deweloper i zasoby:

- [Azure bramy IoT SDK][lnk-sdk]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
