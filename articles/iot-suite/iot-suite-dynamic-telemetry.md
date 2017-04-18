<properties
    pageTitle="Używanie dynamicznego telemetrycznego | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby dowiedzieć się, jak używać telemetrycznego dynamiczne z zdalnego monitorowania rozwiązanie wstępnie."
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
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynamiczne telemetrycznego za pomocą zdalnego rozwiązanie wstępnie monitorowania

## <a name="introduction"></a>Wprowadzenie

Dynamiczne telemetrycznego umożliwia wizualizowanie dowolnego telemetrycznego wysyłane do zdalnego monitorowania rozwiązanie wstępnie skonfigurowane. Symulowany urządzeń, które wdrożyć rozwiązanie wstępnie Wyślij telemetrycznego temperatury i wilgotności, które można wyświetlać wizualizację na pulpicie nawigacyjnym. Jeśli dostosowywanie istniejących urządzeniach symulowany, tworzenie nowych urządzeniach symulowany lub łączenie fizycznymi wstępnie rozwiązanie, możesz wysłać innych wartości telemetrycznego, takich jak temperatury zewnętrznych, obrotów na MINUTĘ lub prędkość wiatru. Następnie można wyświetlać wizualizację ten dodatkowy telemetrycznego na pulpicie nawigacyjnym.

Samouczku prosty Node.js symulowany urządzenie, które można łatwo modyfikować wypróbowanie telemetrycznego dynamiczne.

Aby użyć tego samouczka, musisz:

- Aktywną subskrypcję Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatną wersję próbną Azure][lnk_free_trial].
- [Node.js] [ lnk-node] wersji 0.12.x lub nowszej.

Można użyć tego samouczka we wszystkich systemach operacyjnych, takich jak Windows i Linux oraz, w którym zainstalowano Node.js.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Konfigurowanie urządzenia symulowany Node.js

1. Zdalny monitorowania pulpitu nawigacyjnego kliknij przycisk **+ Dodaj urządzenie** , a następnie dodaj niestandardowe urządzenie. Zanotuj Centrum IoT hostname, identyfikator urządzenia i klucza urządzenia. Potrzebne w dalszej części tego samouczka podczas przygotowywania z aplikacją kliencką urządzenia remote_monitoring.js.

2. Upewnij się, ta wersja Node.js 0.12.x lub nowszym jest zainstalowany na komputerze rozwoju. Uruchamianie `node --version` w wierszu polecenia lub w powłoce, aby sprawdzić wersję. Aby dowiedzieć się, jak zainstalować Node.js w systemie Linux za pomocą Menedżera pakietów, zobacz [Instalowanie Node.js za pomocą Menedżera pakietów][node-linux].

3. Po zainstalowaniu Node.js klonowanie najnowszą wersję pakietu [azure-iot SDK] [ lnk-github-repo] repozytorium na komputerze rozwoju. Zawsze używaj **wzorca** gałąź najnowszą wersję bibliotek i próbki.

4. Z lokalną kopię [azure-iot SDK] [ lnk-github-repo] repozytorium, Kopiuj następujące dwa pliki z folderu próbki węzeł urządzenia do Opróżnij folder na tym komputerze rozwoju:

  - Packages.JSON
  - remote_monitoring.js

5. Otwórz plik remote_monitoring.js i odszukaj następujące definicje zmiennych:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Zastąp ciąg połączenia urządzenia **[parametry połączenia urządzenia Centrum IoT]** . Użyj wartości z Centrum IoT hostname, identyfikator urządzenia i klucza urządzenia, które zostały wprowadzone w kroku 1 Zapisz. Parametry połączenia urządzenia ma następujący format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Jeśli nazwa hosta usługi Centrum IoT jest **contoso** i swój identyfikator urządzenia jest **mydevice**, ciąg połączenia wygląda następująco:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Zapisz plik. Uruchom następujące polecenia w powłoki lub folder, który zawiera te pliki, aby zainstalować pakiety to konieczne, a następnie uruchomić aplikacji przykładowej wiersza polecenia:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Obserwowanie dynamiczne telemetrycznego w działaniu

Pulpit nawigacyjny zawiera telemetrycznego temperatury i wilgotności z istniejących symulowany urządzeń:

![Domyślny pulpit nawigacyjny][image1]

Jeśli wybierzesz Node.js symulowany urządzenie, którego uruchomiono w poprzedniej sekcji, pojawi się temperatury, wilgotności i temperatury zewnętrznych telemetrycznego:

![Dodawanie zewnętrznego temperatury do pulpitu nawigacyjnego][image2]

Zdalny rozwiązanie monitorowania automatycznie wykrywa typ telemetrycznego dodatkowe temperatury zewnętrznych i dodanie go do wykresu na pulpicie nawigacyjnym.

## <a name="add-a-telemetry-type"></a>Dodawanie typu danych telemetrycznych

Następnym krokiem jest, aby zamienić telemetrycznego wygenerowane przez urządzenie symulowany Node.js przy użyciu nowego zestawu wartości:

1. Zatrzymywanie urządzenia symulowany Node.js, wpisując w wierszu polecenia lub powłoki **Klawisze Ctrl + C** .

2. W pliku remote_monitoring.js można zobaczyć wartości danych podstawowych dla istniejących temperatury, wilgotności i telemetrycznego temperatury zewnętrznych. Dodawanie danych podstawowych wartość **obrotów na minutę** w następujący sposób:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Urządzenie symulowany Node.js funkcja **generateRandomIncrement** w pliku remote_monitoring.js Dodawanie losowe przyrost do wartości danych podstawowych. Generowanie liczb losowych wartość **obrotów na minutę** przez dodanie wiersza kodu po istniejących randomizations w następujący sposób:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Dodaj nową wartość obrotów na minutę do ładunku JSON, które urządzenie wysyła do koncentratora IoT:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Uruchom Node.js urządzenia symulowane przy użyciu następującego polecenia:

    ```
    node remote_monitoring.js
    ```

6. Obserwowanie nowy typ telemetrycznego obrotów na MINUTĘ, której są wyświetlane na wykresie na pulpicie nawigacyjnym:

![Dodawanie obrotów na MINUTĘ do pulpitu nawigacyjnego][image3]

> [AZURE.NOTE] Może być konieczne wyłączenie, a następnie Włącz urządzenie Node.js na stronie **urządzeń** na pulpicie nawigacyjnym, aby od razu zobaczyć zmiany.

## <a name="customize-the-dashboard-display"></a>Dostosowywanie sposobu wyświetlania pulpitu nawigacyjnego

**Informacje o urządzeniach** wiadomości może zawierać metadanych dotyczących telemetrycznego, które urządzenie można wysłać do Centrum IoT. Te metadane można określić typy telemetrycznego, które urządzenie wysyła. Zmień wartość **deviceMetaData** w pliku remote_monitoring.js, aby uwzględnić definicję **telemetrycznego** zgodnie z definicją **poleceń** . Następujący fragment kodu zawiera definicję **poleceń** (Pamiętaj dodać `,` po definicji **polecenia** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Zdalny rozwiązanie monitorowania używa bez uwzględniania wielkości liter dopasowania do porównywania definicja metadanych z danymi w strumieniu telemetrycznego.

Dodawanie definicji **telemetrycznego** , jak pokazano w powyższej wstawkę kodu nie zmienia zachowanie pulpitu nawigacyjnego. Jednak metadane można także dołączyć atrybutami **DisplayName** , aby dostosować wyświetlanie na pulpicie nawigacyjnym. Aktualizacja definicja metadanych **telemetrycznego** , jak pokazano w poniższej wstawki kodu:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Następujące zrzucie ekranu pokazano, jak ta zmiana modyfikuje legendy wykresu na pulpicie nawigacyjnym:

![Dostosowywanie legendy wykresu][image4]

> [AZURE.NOTE] Może być konieczne wyłączenie, a następnie Włącz urządzenie Node.js na stronie **urządzeń** na pulpicie nawigacyjnym, aby od razu zobaczyć zmiany.

## <a name="filter-the-telemetry-types"></a>Filtrowanie typów danych telemetrycznych

Domyślnie na wykresie na pulpicie nawigacyjnym pokazano każdej serii danych w strumieniu telemetrycznego. **Informacje o urządzeniach** metadanych umożliwia zapobiec wyświetlaniu telemetrycznego określonych typów na wykresie. 

Aby wyświetlić tylko telemetrycznego temperatury i wilgotności wykres, należy pominąć **ExternalTemperature** z metadanych **telemetrycznego** **Informacje o urządzeniach** w następujący sposób:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Na zewnątrz temperatury** nie jest już wyświetlany na wykresie:

![Filtrowanie danych telemetrycznych na pulpicie nawigacyjnym][image5]

Tę zmianę dotyczy tylko wykres. Wartości danych **ExternalTemperature** nadal są przechowywane i udostępniane dla jakiegokolwiek przetwarzania wewnętrznej bazy danych.

> [AZURE.NOTE] Może być konieczne wyłączenie, a następnie Włącz urządzenie Node.js na stronie **urządzeń** na pulpicie nawigacyjnym, aby od razu zobaczyć zmiany.

## <a name="handle-errors"></a>Obsługi błędów

Strumień danych wyświetlanych na wykresie jego **Typ** metadanych **Informacje o urządzeniu** musi odpowiadać typ danych wartości telemetrycznego. Na przykład jeśli metadane Określa, że **Typ** danych wilgotność jest **int** **podwójne** znajduje się w strumieniu telemetrycznego następnie telemetrycznego wilgotności nie są wyświetlane na wykresie. Jednak wartości **wilgotność** nadal są przechowywane i udostępniane dla jakiegokolwiek przetwarzania wewnętrznej bazy danych.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy użytkownik zobaczył, jak używać telemetrycznego dynamiczne, możesz dowiedzieć się więcej o sposobach rozwiązania wstępnie użytkowania informacje o urządzeniach: [urządzenie informacji metadanych w zdalnego monitorowania wstępnie rozwiązanie][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
