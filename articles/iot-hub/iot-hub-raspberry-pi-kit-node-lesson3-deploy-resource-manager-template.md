<properties
 pageTitle="Tworzenie aplikacji funkcji Azure i konta magazynu platformy Azure | Microsoft Azure"
 description="Aplikacja Azure funkcja wykrywa zdarzenia Centrum Azure IoT, przetwarza wiadomości przychodzących i zapisuje je z magazynem tabel platformy Azure."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 Tworzenie Azure funkcji aplikacji i konta magazynu platformy Azure

[Funkcje Azure](../../articles/azure-functions/functions-overview.md) to rozwiązanie łatwe uruchamiania fragmenty kodu, nazywane "funkcjami", w chmurze. Funkcja Azure aplikacji obsługuje wykonanie funkcje platformy Azure.

## <a name="311-what-will-you-do"></a>3.1.1 co będzie zrobić

Menedżer zasobów Azure szablon do tworzenia aplikacji funkcji Azure i konto Azure miejsca do magazynowania. Aplikacja Azure funkcja wykrywa zdarzenia Centrum Azure IoT, przetwarza wiadomości przychodzących i zapisuje je z magazynem tabel platformy Azure. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 będzie zdobyte informacje

- Jak wdrożyć Azure zasobów za pomocą [Menedżera zasobów Azure](../../articles/azure-resource-manager/resource-group-overview.md) .
- Jak używać aplikacji Azure funkcja przetwarzanie IoT Centrum wiadomości i zapisać je do tabeli w magazynie tabel platformy Azure.

## <a name="313-what-do-you-need"></a>3.1.3 co to są potrzebne

- Musisz ukończona pomyślnie poprzedniej lekcji: [Wprowadzenie do usługi malina Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) i [Tworzenie Twoim Centrum Azure IoT](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 Otwórz aplikację próbki

Otwórz przykładowy projekt w kodzie programu Visual Studio, uruchamiając następujące polecenia:

```bash
cd Lesson3
code .
```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- `app.js` Plików w `app` podfolderu jest plikiem źródłowym klucza. Ten plik źródłowy zawiera kod, aby wysłać wiadomość 20 godzin do Twoim Centrum IoT i miganie LED dla każdej wiadomości wysyłane.
- `arm-template.json` Jest plik szablonu Azure Menedżera zasobów, który zawiera aplikację funkcja Azure i konto Azure miejsca do magazynowania.
- `arm-template-param.json` Plik jest plikiem konfiguracji używane przez szablon Azure Menedżera zasobów.
- `ReceiveDeviceMessages` Podfolder zawiera kod Node.js funkcji Azure.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 skonfigurować szablony Azure Menedżera zasobów i tworzenie zasobów platformy Azure

Aktualizacja `arm-template-param.json` pliku programu Visual Studio kod.

![Azure parametrów szablonu Menedżera zasobów](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Zamień **[nazwa Centrum IoT]** **{nazwy Centrum}** określony w [lekcji 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
- Zamień **[ciąg prefiksu nowych zasobów]** dowolnego prefiksu odpowiednie. Prefiks sprawia, że nazwa zasobu jest globalnie unikatowe, aby uniknąć konfliktów. Nie należy używać kreski lub numer początkowy w prefiks.

> [AZURE.NOTE] Nie ma potrzeby `azure_storage_connection_string` w tej sekcji. Pozostaw je tak jak.

Po zaktualizowaniu `arm-template-param.json` plików, wdrażanie zasobów Azure, uruchamiając następujące polecenie:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Aby utworzyć te zasoby trwa około pięć minut. Podczas tworzenia zasobu jest w toku, można przenieść na do następnej sekcji.

## <a name="316-summary"></a>3.1.6 podsumowanie

Została utworzona aplikacji Azure funkcji przetwarzania IoT Centrum wiadomości, a konto Azure miejsca do magazynowania do przechowywania tych wiadomości. Na można przenosić do następnej sekcji w celu wdrażanie i uruchamianie próbki do wysyłania wiadomości urządzenia w chmurze w swojej Pi.

## <a name="next-steps"></a>Następne kroki

[3,2 uruchomić aplikację próbki do wysyłania wiadomości urządzenia w chmurze w swojej malina Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

