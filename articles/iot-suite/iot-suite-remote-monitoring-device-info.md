<properties
 pageTitle="Metadane informacji urządzeń zdalnego rozwiązania monitorowania | Microsoft Azure"
 description="Opis Azure IoT wstępnie rozwiązanie zdalnego monitorowania i jego architektury."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metadane informacji urządzeń w zdalne monitorowanie wstępnie rozwiązanie

Zdalne monitorowanie wstępnie rozwiązanie pakietu IoT Azure zaprezentowano podejście do zarządzania metadane urządzeń. W tym artykule omówiono podejście, które ma to rozwiązanie pozwala zapoznać się z:

- Jakie urządzenia metadane przechowuje rozwiązanie.
- Jak rozwiązanie zarządza metadane urządzeń.

## <a name="context"></a>Kontekst

Zdalny monitorowania rozwiązanie wstępnie używa [Azure IoT Centrum] [ lnk-iot-hub] umożliwiający urządzeniach wysyłanie danych do chmury. Centrum IoT zawiera [urządzenia tożsamości rejestru] [ lnk-identity-registry] kontrolowania dostępu do Centrum IoT. Centrum IoT rejestru tożsamości urządzenia różni się od zdalnego monitorowania określonego rozwiązania *rejestru urządzenia* przechowującego metadane informacji urządzeń. Zdalny rozwiązanie monitorowania używa [DocumentDB] [ lnk-docdb] bazy danych w celu zaimplementowania jego rejestru urządzenia do przechowywania metadanych informacji o urządzeniu. [Microsoft Azure IoT odwołanie architektura] [ lnk-ref-arch] opisuje rolę rejestru urządzenia typowe rozwiązania IoT.

> [AZURE.NOTE] Zdalny monitorowania rozwiązanie wstępnie przechowywana rejestru tożsamości urządzenia synchronizacji z rejestru urządzenia. Umożliwia zarówno jednoznacznie identyfikować każdego urządzenia podłączonego do koncentratora usługi IoT ten sam identyfikator urządzenia.

[Podgląd zarządzania urządzenia Centrum IoT] [ lnk-dm-preview] zapewnia funkcje Centrum IoT, które są podobne do funkcji zarządzania informacji urządzenia opisane w tym artykule. Zdalny rozwiązanie monitorowania używa obecnie tylko ogólnie dostępne funkcje (GA) w Centrum IoT.

## <a name="device-information-metadata"></a>Metadane informacji urządzeń

Urządzenie informacji metadanych JSON dokument w bazie danych DocumentDB rejestru urządzenia ma następującą strukturę:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: urządzeniu zapisuje te właściwości, a urządzenie jest urząd dla tych danych. Inne właściwości urządzenia przykład zawierać producenta, numer modelu i liczbę kolejną. 
- **Identyfikator urządzenia**: identyfikator urządzenia unikatowe. Ta wartość jest taki sam w rejestrze Centrum IoT urządzenia tożsamości.
- **HubEnabledState**: stan urządzenia w Centrum IoT. Ta wartość jest ustawiany **wartość null** , aż urządzenie najpierw łączy. W portalu rozwiązanie wartość **null** jest reprezentowane jako urządzenie jest "zarejestrowane, ale nie ma".
- **CreatedTime**: czas utworzenia urządzenia.
- **DeviceState**: stan zgłoszone przez urządzenie.
- **UpdatedTime**: czas ostatniej aktualizacji urządzenia za pośrednictwem portalu rozwiązanie.
- **SystemProperties**: portal rozwiązanie zapisuje właściwości systemu i urządzenia nie ma żadnych informacji o tych właściwości. Właściwość system przykład jest **ICCID** , jeśli rozwiązanie jest autoryzowany z i połączenia z usługą zarządzania urządzeniami z obsługą SIM.
- **Polecenia**: listę poleceń urządzenie obsługuje. Urządzenie zawiera te informacje do rozwiązania.
- **CommandHistory**: lista poleceń wysyłane przez zdalnego rozwiązanie monitorowania urządzenia i stanu tych poleceń.
- **IsSimulatedDevice**: flagę, który identyfikuje urządzenie jako urządzenie symulowany.
- **identyfikator**: Unikatowy identyfikator DocumentDB dla tego dokumentu urządzenia.

> [AZURE.NOTE] Informacje o urządzeniach również mogą zawierać metadanych w celu opisania telemetrycznego, które urządzenie wysyła do koncentratora IoT. Zdalny rozwiązanie monitorowania używa metadanych telemetrycznego dostosować sposób pulpitu nawigacyjnego wyświetlania [dynamiczne telemetrycznego][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Cykl życia

Po utworzeniu urządzenia w portalu rozwiązanie, rozwiązanie tworzy wpis w rejestrze jego urządzenia, jak pokazano wcześniej. Duża część informacji jest początkowo stubbed i **HubEnabledState** jest ustawiona na **wartość null**. W tym momencie rozwiązanie również tworzy wpis urządzenia w rejestrze tożsamości urządzenia, który umożliwia generowanie kluczy urządzenia używanego do uwierzytelniania IoT Centrum.

Po podłączeniu najpierw urządzenia rozwiązanie, wysyła komunikat informacyjny urządzenia. Ten komunikat informacji o urządzeniu zawiera właściwości urządzenia, takich jak producenta urządzenia, numer modelu i liczbę kolejną. Komunikat informacyjny urządzenia również zawiera listę poleceń, które urządzenie obsługuje w tym informacje o parametry polecenia. Gdy rozwiązanie otrzymuje tę wiadomość, aktualizuje metadane urządzeń informacji w rejestrze urządzenia.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Umożliwia wyświetlanie i edytowanie informacji o urządzeniach w portalu rozwiązanie

Lista urządzeń w portalu rozwiązanie zawiera następujące właściwości urządzenia jako kolumny: **Stan**, **identyfikator urządzenia**, **producenta**, **Numer modelu**, **numer seryjny**, **sprzętowego**, **platformy**, **Procesor**i **Zainstalowany pamięci RAM**. Właściwości urządzenia, **szerokości** i **długości geograficznej** sterują lokalizacji w aplikacji mapy Bing na pulpicie nawigacyjnym. 

![Lista urządzeń][img-device-list]

Kliknięcie przycisku **Edytuj** w okienku **Szczegóły urządzenia** w portalu rozwiązanie, możesz edytować te właściwości. Edycja tych właściwości aktualizacji rekordu dla urządzenia w bazie danych DocumentDB. Jednak jeśli urządzenie wysyła wiadomość informacje zaktualizowane urządzenia, zostaną zastąpione wszelkie zmiany wprowadzone w portalu rozwiązanie. Nie można edytować **identyfikator urządzenia**, **Nazwa hosta**, **HubEnabledState**, **CreatedTime**, **DeviceState**i właściwości **UpdatedTime** w portalu rozwiązanie, ponieważ tylko urządzenie ma urząd na tych właściwości.

![Edytuj urządzenia][img-device-edit]

Portal rozwiązanie umożliwia usuwanie urządzenia z rozwiązania. Po usunięciu urządzenia rozwiązanie usuwa metadane informacji urządzeń z rejestru urządzenia rozwiązanie i usuwa wpisu urządzenia w rejestrze Centrum IoT urządzenia tożsamości. Zanim będzie można usunąć urządzenie, musisz wyłączyć.

![Usuwanie urządzenia][img-device-remove]

## <a name="device-information-message-processing"></a>Przetwarzanie wiadomości informacji o urządzeniu

Urządzenie informacji wiadomości za pomocą urządzenia różni się od telemetrycznego wiadomości. Urządzenie informacji wiadomości zawierają informacje, takie jak właściwości urządzenia, polecenia, które można odpowiedzieć urządzenia i żadnej historii polecenia. Centrum IoT, sam ma żadnych informacji o metadanych zawarte w wiadomości informacji urządzenia i przetwarza wiadomości w taki sam sposób przetwarza wszystkie wiadomości urządzenia w chmurze. Zdalny rozwiązania monitorowania [Azure analizy strumieniu] [ lnk-stream-analytics] zadania (ASA) odczytuje wiadomości z Centrum IoT. Filtry dla wiadomości, które zawierają zadania analizy strumieniu **DeviceInfo** **"Typ obiektu": "DeviceInfo"** i przekazuje je do wystąpieniu hosta **EventProcessorHost** w zadaniu sieci web. Logika w przypadku **EventProcessorHost** używa identyfikator urządzenia znaleźć rekord DocumentDB dla określonego urządzenia i zaktualizować rekord. Rekord rejestru urządzenia teraz zawiera informacje, takie jak właściwości urządzenia, poleceń i historii poleceń.

> [AZURE.NOTE] Komunikat informacyjny urządzenie jest standardowy komunikat urządzenia w chmurze. Rozwiązanie rozróżnia wiadomości informacji urządzenia i telemetrycznego wiadomości za pomocą kwerend ASA.

## <a name="example-device-information-records"></a>Przykład urządzenia informacji rekordów

Zdalnego monitorowania rozwiązanie wstępnie używa dwóch typów rekordów informacji urządzenia: rekordów dla urządzeń symulowany wdrażane rozwiązanie i rekordy dla urządzeń niestandardowych, możesz połączyć się rozwiązanie.

### <a name="simulated-device"></a>Symulacji

W poniższym przykładzie pokazano rekordu informacji urządzenia JSON symulowany urządzenia. Ten rekord ma wartość dla **UpdatedTime**, która wskazuje, że urządzenie została wysłana wiadomość **DeviceInfo** do koncentratora IoT. Rekord zawiera kilka typowych właściwości urządzenia, definiuje sześciu polecenia symulowany urządzenia obsługuje i ma flaga **IsSimulatedDevice** ustawiona na **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Niestandardowe urządzenie

Poniższy przykład pokazuje rekord informacji urządzenia JSON niestandardowe urządzenia i ma ustawioną flagą **IsSimulatedDevice** **0**. Możesz zobaczyć czy to urządzenie niestandardowe obsługuje dwa polecenia i portalu rozwiązanie została wysłana polecenia **SetTemperature** na urządzeniu:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Poniżej przedstawiono wiadomości JSON **DeviceInfo** urządzenia wysyłane do aktualizacji metadanych informacji na urządzeniu:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Następne kroki

Po zakończeniu nauki, jak dostosować wstępnie rozwiązań można eksplorować niektóre z innych funkcji i możliwości rozwiązań pakietu IoT wstępnie:

- [Omówienie rozwiązania przewidywanych konserwacji wstępnie][lnk-predictive-overview]
- [Często zadawane pytania dotyczące pakietu IoT][lnk-faq]
- [Zabezpieczenia IoT od podstaw][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
