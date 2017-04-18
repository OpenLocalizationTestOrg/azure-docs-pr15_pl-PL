<properties
 pageTitle="Rozwiązywanie problemów z | Microsoft Azure"
 description="Rozwiązywanie problemów z strony możliwości Node.js Pi malina"
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

# <a name="troubleshooting"></a>Rozwiązywanie problemów

## <a name="hardware-issues"></a>Problemy ze sprzętem

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Aplikacja zostanie uruchomiona dobrze, ale nie jest miganie LED

Ten problem dotyczy zawsze łączności elektrycznego sprzętu. Wykonaj następujące czynności, aby identyfikowanie problemów związanych z.

1. Sprawdzanie, jeśli wybierzesz poprawny **GPIO** do tablicy. Dwa porty w tej lekcji powinny być **GND GPIO (Pin 6)** i **04 GPIO (Pin 7)**.
2. Sprawdzanie poprawności polarności usługi LED. Nogi dłużej powinny wskazywać **dodatnia**, anody numeru pin.
3. Użyj **3, 3V Pin** i **numer Pin GND** na swojej malinowe Pi 3. Do Pi traktowane jako Power kontrolera domeny. Sprawdź, czy LED działa prawidłowo.

![Specyfikacja LED](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Innych problemów ze sprzętem

Informacji na temat rozwiązywania typowych problemów na malina Pi 3 można znaleźć w [oficjalnym strony rozwiązywania problemów](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problemy dotyczące pakietu node.js

### <a name="no-response-during-gulp-tasks"></a>Brak odpowiedzi podczas gulp zadań

Jeśli występują problemy z uruchamianiem gulp zadań, można dodać `--verbose` opcja debugowania. Spróbuj zakończenie bieżącego zadania gulp z `Ctrl + C` , a następnie uruchom następujące polecenie w oknie konsoli, aby wyświetlić komunikaty debugowania. Może zostać wyświetlony szczegółowe komunikaty o błędach drukowanych w danych wyjściowych konsoli. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problemy z odnajdowaniem urządzeń

W celu rozwiązywania typowych problemów z `devdisco` polecenia, zaznacz [plik readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Inne problemy z NPM

Spróbuj zaktualizować pakietu NPM przy użyciu następującego polecenia:

```bash
npm install -g npm
```

Jeśli problem nadal występuje, komentarz na końcu tego artykułu, lub Utwórz problem Github w naszym [Repozytorium próbki](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Zdalne debugowanie

### <a name="run-the-sample-application-in-debug-mode"></a>Uruchom aplikację próbki w trybie debugowania

```bash
gulp run --debug
```

Gdy aparat debugowania będzie gotowa, powinna być widoczna ```Debugger listening on port 5858``` w wyniku konsoli.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfigurowanie kod w PORÓWNANIU z, aby połączyć się z urządzeniem zdalnym

Otwórz panel **Debugowanie** po lewej stronie.

Kliknij zielony przycisk **Rozpocznij debugowanie** (F5). Kod w PORÓWNANIU z otwiera plik **launch.json** , który chcesz zaktualizować.

Aktualizowanie plików **launch.json** o następującej treści. Zamienianie `[device hostname or IP address]` za pomocą urządzenia rzeczywisty adres IP lub nazwa hosta.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Konfiguracja zdalnego debugowania](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Dołącz do zdalnego aplikacji

Kliknij zielony przycisk **Rozpocznij debugowanie** (F5), aby rozpocząć debugowanie. 

W tym artykule [kodu JavaScript w PORÓWNANIU z kodu](https://code.visualstudio.com/docs/languages/javascript#_debugging) , aby dowiedzieć się więcej na temat debugowania.

![Zdalne debugowanie interakcyjnych](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Problemy z usługą Azure interfejsu wiersza polecenia

Polecenie Azure jest kompilacja do podglądu. Może dotyczyć [Przewodnik instalacji Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) poszukiwanie rozwiązań.

Jeśli wystąpią wszystkie błędy przy użyciu narzędzia plik [problem](https://github.com/Azure/azure-cli/issues) w sekcji **problemy** GitHub repo.

W celu rozwiązywania typowych problemów zaznacz [plik readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problemy dotyczące instalacji Python

### <a name="legacy-installation-issues-macos"></a>Problemy z instalacją starsze (macOS)

Podczas instalowania **pip**, po starsze zainstalowanych z uprawnieniami **su** pakietów jest generowany błąd uprawnień. Taka sytuacja występuje, ponieważ poprzednia instalacja Python przy użyciu brew (macOS) nie jest całkowicie odinstalować. Niektóre pakiety **pip** poprzedniej instalacji zostały utworzone przez główny, co powoduje błąd uprawnień. Rozwiązaniem jest usunięcie tych pakietów zainstalowanych przez główny. Aby wykonać to zadanie, wykonaj następujące czynności:

1. Przejdź do /usr/local/lib/python2.7/site-packages
2. Tworzenie listy pakietów przez główny:`ls -l | grep root`
3. Odinstaluj pakietów z kroku2:`sudo rm -rf {package name}`
4. Ponownie zainstaluj Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Centrum problemów

Jeśli zostały pomyślnie obsługi administracyjnej koncentratora Azure IoT za pomocą `azure-cli`, a potrzebne narzędzie do zarządzania urządzeniami nawiązywanie połączenia z Twoim Centrum IoT, spróbuj użyć następujących narzędzi:

### <a name="device-explorer"></a>Eksplorator urządzenia

[Eksplorator urządzenie](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) działa na komputerze lokalnym systemu Windows i łączy się z Centrum IoT platformy Azure. Komunikuje następujące [Centrum IoT punkty końcowe](iot-hub-devguide.md):

- *Zarządzanie tożsamościami urządzenia* do obsługi administracyjnej i zarządzanie urządzeniami zarejestrowana w Twoim Centrum IoT.
- *Odbierz urządzenia w chmurze* pozwalają monitorować wiadomości wysyłanych z urządzenia do usługi Centrum IoT.
- *Wysyłanie chmury do urządzenia* , aby możliwe było wysyłać wiadomości z urządzeniami z Twoim Centrum IoT.

Konfigurowanie usługi `IoT hub connection string` w to narzędzie, aby używać wszystkich funkcji.

### <a name="iot-hub-explorer"></a>Centrum IoT Eksploratora

[Centrum IoT Eksploratora](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) jest wiele platform przykładowe polecenie Narzędzie do zarządzania klientami urządzenia. Narzędzie umożliwia zarządzanie urządzeniami w rejestrze tożsamości, monitorowanie urządzenia w chmurze wiadomości i wysyłanie poleceń w chmurze do urządzenia.

Aby zainstalować najnowszą wersję (wersji wstępnej) w narzędziu iothub Eksploratora, uruchom następujące polecenie w środowisku wiersza polecenia:

```
npm install -g iothub-explorer@latest
```

Aby uzyskać dodatkową pomoc dotyczącą wszystkie polecenia Eksploratora iothub i ich parametry umożliwiają następujące polecenie:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Zarządzanie zasobami za pomocą Azure portal

W tych lekcji pełną obsługę interfejsu wiersza polecenia są dostarczane do tworzenia i zarządzania wszystkich Azure zasobów. Można także korzystać z [Azure portal](../azure-portal-overview.md) świadczenia pomocy, zarządzania nimi oraz debugowanie Azure zasobów.

## <a name="azure-storage-issues"></a>Problemy z usługą Azure miejsca do magazynowania

[Eksplorator magazynu Microsoft Azure (wersja Preview)](http://storageexplorer.com) to aplikacja autonomicznego firmy Microsoft, który umożliwia pracę z danymi Azure miejsca do magazynowania w systemie Windows, macOS i Linux. To narzędzie umożliwia nawiązywanie połączenia z tabeli i wyświetlić zawarte w niej dane. Za pomocą tego narzędzia do rozwiązywania problemów z magazyn Azure z.
