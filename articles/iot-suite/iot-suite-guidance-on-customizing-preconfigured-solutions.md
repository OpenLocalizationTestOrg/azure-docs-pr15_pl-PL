<properties
    pageTitle="Dostosowywanie wstępnie rozwiązań | Microsoft Azure"
    description="Ten artykuł zawiera wskazówki na temat dostosowywania rozwiązań pakietu IoT Azure wstępnie."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Dostosowywanie wstępnie rozwiązania

Wstępnie skonfigurowane rozwiązania opisane z pakietem IoT Azure przedstawiają usługi w pakiecie współpracujących do przeprowadzania kompleksowe rozwiązania. Z tego punktu początkowego istnieje różnych miejsc, w których można rozszerzyć i dostosować rozwiązanie dla określonych scenariuszach. W poniższych sekcjach opisano typowe punkty dostosowywania.

## <a name="finding-the-source-code"></a>Znajdowanie kodu źródłowego

Kod źródłowy wstępnie rozwiązań jest dostępna w GitHub w następujących repozytoriach:

- Monitorowanie zdalne: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Konserwacja przewidywanych: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Kod źródłowy wstępnie rozwiązań znajduje się wykazać desenie i wskazówki dotyczące używanych do implementacji funkcji zakończenia do końca rozwiązania IoT za pomocą pakietu IoT Azure. Można znaleźć więcej informacji na temat Tworzenie i wdrażanie rozwiązań w repozytoria GitHub.

## <a name="changing-the-preconfigured-rules"></a>Zmienianie wstępnie skonfigurowanych reguł

Zdalny rozwiązanie monitorowania zawiera trzy [Analizy strumieniu Azure](https://azure.microsoft.com/services/stream-analytics/) zadań do wykonania urządzenia informacji, telemetrycznego i reguły logiki wyświetlane rozwiązania.

Zadania analizy strumieniu trzy i ich składni opisano szczegółowo w [zdalne monitorowanie Instruktaż wstępnie rozwiązanie](iot-suite-remote-monitoring-sample-walkthrough.md). 

Możesz edytować te zadania bezpośrednio do zmiany logikę lub Dodawanie logiki określonych do rozwiązania. Zadania analizy strumieniu można znaleźć w następujący sposób:
 
1. Przejdź do [portalu Azure](https://portal.azure.com).
2. Przejdź do grupy zasobów z taką samą nazwę jak rozwiązania IoT. 
3. Zaznacz zadanie analizy strumieniu Azure, który chcesz zmodyfikować. 
4. Zatrzymaj zadanie, wybierając pozycję **Zatrzymaj**w zestawie poleceń. 
5. Edytowanie danych wejściowych, kwerendy i wyjściowe.

    Należy zmienić kwerendę dla zadania **reguł** użyć prostej modyfikacji **"<"** zamiast **">"**. Portal rozwiązań nadal jest wyświetlana **">"** gdy edytowanie reguły, ale można zauważyć zachowanie jest przerzucony ze względu na zmiany w zadaniu źródłowych.

6. Rozpoczynanie zadania

> [AZURE.NOTE] Zdalny monitorowania pulpitu nawigacyjnego zależy od określonych danych, więc zmiany zadania mogą powodować niepowodzenie pulpitu nawigacyjnego.

## <a name="adding-your-own-rules"></a>Dodawanie własnych reguł

Oprócz zmiany wstępnie zadań analizy strumieniu Azure, Azure portal służy do dodawania nowych zadań lub dodać nowe kwerendy do istniejących zadań.

## <a name="customizing-devices"></a>Dostosowywanie urządzenia

Jedną z najbardziej typowych czynności rozszerzenia współpracuje z urządzenia specyficzne dla danego scenariusza. Istnieje kilka metod pracy z urządzenia. Te metody obejmują zmiany na urządzeniu symulowany zgodne rozwiązania lub przy użyciu [SDK urządzenia IoT][] nawiązywania połączenia między urządzeniem fizycznie rozwiązanie.

Przewodnik krok po kroku do dodawania urządzenia do zdalnego monitorowania rozwiązanie wstępnie uzyskać [Iot pakietu nawiązywanie połączenia urządzeń](iot-suite-connecting-devices.md) i [Zdalnego monitorowania przykładowe SDK C](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , która jest przeznaczona do pracy z zdalnego monitorowania rozwiązanie wstępnie.

### <a name="creating-your-own-simulated-device"></a>Tworzenie własnej symulowany urządzenia

Zawarte w zdalnego kod źródłowy monitorowania rozwiązanie (wymienionych powyżej), jest .NET simulator. Ten simulator jest jednym obsługi administracyjnej jako część rozwiązania i może się zmienić wysłanie metadanych, telemetrycznego lub odpowiadanie na różne polecenia.

Wstępnie skonfigurowane simulator zdalnego monitorowania rozwiązania wstępnie to urządzenie zapewnia, które emituje telemetrycznego temperatury i wilgotności, można modyfikować simulator w programie project [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) , gdy została forked repozytorium GitHub.

### <a name="available-locations-for-simulated-devices"></a>Dostępne lokalizacje symulowany urządzeń

Domyślny zestaw lokalizacji znajduje się w Seattle/Redmond, Washington, USA. Możesz zmienić te lokalizacje w [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Tworzenie i używanie urządzenia (fizycznej)

[Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) zapewniają bibliotek łączenie wielu typów urządzenia (językach i systemy operacyjne) do IoT rozwiązań.

## <a name="modifying-dashboard-limits"></a>Modyfikowanie ograniczenia pulpitu nawigacyjnego

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Liczba urządzenia wyświetlane na liście rozwijanej pulpitu nawigacyjnego

Wartość domyślna to 200. Możesz zmienić ten numer w [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Numer PIN, aby wyświetlić w kontrolce mapy Bing

Wartość domyślna to 200. Możesz zmienić ten numer w [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Okres telemetrycznego wykresu

Wartość domyślna to 10 minut. Można to zmienić w [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Ręczne konfigurowanie ról aplikacji

Poniższa procedura zawiera opis sposobu dodawania ról aplikacji **Administrator** , a **tylko do odczytu** na wstępnie rozwiązanie. Należy zauważyć, że wstępnie rozwiązania obsługi administracyjnej z witryny azureiotsuite.com już zawierać role **administratora** i **tylko do odczytu** .

Członkowie roli **tylko do odczytu** można wyświetlić pulpit nawigacyjny i lista urządzeń, ale nie możesz dodać urządzeń, zmienić atrybuty urządzeń lub wysyłać polecenia.  Członkowie roli **administratora** mają pełny dostęp do wszystkich funkcji w rozwiązaniu.

1. Przejdź do [portalu klasyczny Azure][lnk-classic-portal].

2. Wybierz pozycję **usługi Active Directory**.

3. Kliknij nazwę dzierżawy AAD użytego podczas zainicjowano obsługę administracyjną rozwiązania.

4. Kliknij pozycję **aplikacje**.

5. Kliknij nazwę aplikacji, która jest zgodna z nazwą rozwiązanie wstępnie skonfigurowane. Jeśli nie widzisz aplikacji na liście, wybierz pozycję **aplikacje właścicielem mojej firmy** w **Pokazywanie** listy rozwijanej i kliknij znacznik wyboru.

6.  U dołu strony kliknij pozycję **Zarządzaj pojawiają** , a następnie **Pobierz pojawiają**.

7. Spowoduje to pobranie pliku .json na komputerze lokalnym.  Otworzyć tego pliku do edycji w edytorze tekstów wybranych przez użytkownika.

8. W trzecim wierszu pliku .json znajdziesz:

  ```
  "appRoles" : [],
  ```
  Zastąp to następujące czynności:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Zapisz plik zaktualizowanych .json (możesz zastąpić istniejący plik).

10.  W portalu zarządzania Azure w dolnej części strony zaznacz **Zarządzanie pojawiają** , a następnie **Przekaż pojawiają** przekazać plik .json, który został zapisany w poprzednim kroku.

11. Role **administracyjne** i **tylko do odczytu** zostało dodane do aplikacji.

12. Aby przypisać jedną z następujących ról użytkowników w katalogu, zobacz [uprawnienia w witrynie azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Opinie

Czy masz dostosowanie chcesz wyświetlić wymienionych w tym dokumencie? Dodaj sugestie dotyczące funkcji do [Głosu użytkownika](https://feedback.azure.com/forums/321918-azure-iot)lub komentarz do artykułu poniżej. 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat opcji dostosowywania wstępnie rozwiązań, zobacz:

- [Łączenie aplikacji logiki do rozwiązania Azure IoT pakietu zdalnego monitorowania wstępnie][lnk-logicapp]
- [Dynamiczne telemetrycznego za pomocą zdalnego rozwiązanie wstępnie monitorowania][lnk-dynamic]
- [Metadane informacji urządzeń w zdalne monitorowanie wstępnie rozwiązanie][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Urządzenie IoT SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
