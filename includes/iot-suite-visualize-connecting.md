## <a name="view-device-telemetry-in-the-dashboard"></a>Widok urządzenia telemetrycznego na pulpicie nawigacyjnym

Pulpit nawigacyjny w roztworze monitorowania zdalnego umożliwia wyświetlanie przyspieszając urządzeń wysłane do Centrum IoT.

1. W przeglądarce powrócić do zdalnego monitorowania pulpitu nawigacyjnego rozwiązanie, kliknij opcję **urządzenia** w panelu po lewej stronie, aby przejść do **listy urządzeń**.

2. Na **liście urządzeń**powinny być widoczne czy stan urządzenia jest obecnie **uruchomiony**.

    ![][18]

3. Kliknij **pulpit nawigacyjny** powrót do pulpitu nawigacyjnego, wybierz urządzenie w **urządzeniu do widoku** listy rozwijanej do wyświetlania jego telemetrycznego. Przyspieszając z przykładowej aplikacji jest 50 jednostek dla temperatury wewnętrznej, 55 jednostek dla temperatury zewnętrznej i 50 jednostek w odniesieniu do wilgotności. Należy zauważyć, że domyślnie pulpit nawigacyjny wyświetla tylko wartości temperatury i wilgotności.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Wysyłać polecenia do urządzenia

Pulpit nawigacyjny w rozwiązanie zdalnego monitoringu pozwala na wysyłanie poleceń do urządzenia za pośrednictwem Centrum IoT. Na przykład w rozwiązanie zdalnego monitoringu można wysyłać polecenia ustawić temperaturę wewnątrz urządzenia.

1. Na zdalnego monitorowania pulpitu nawigacyjnego rozwiązanie kliknij **urządzenia** w panelu po lewej stronie, aby przejść do **listy urządzeń**.

2. Kliknij **Identyfikator urządzenia** dla urządzenia z **listy urządzenia**.

3. W panelu **Szczegóły urządzenia** kliknij przycisk **polecenia**.

    ![][13]

4. W **polecenia** menu Zaznacz **SetTemperature**, a następnie wprowadź nową wartość temperatury **temperatury** . Kliknij przycisk **Wyślij polecenia** do wysyłania polecenia do urządzenia.

    ![][14]

    > [AZURE.NOTE] Historia poleceń początkowo pokazuje stan polecenia jako **Oczekujące**. Gdy urządzenie uznaje polecenia, stan zmienia się na **Sukces**.

5. Na pulpicie nawigacyjnym Sprawdź, czy urządzenie jest teraz wysyłanie 75 jako nową wartość temperatury.

## <a name="next-steps"></a>Następne kroki

Artykuł [Dostosowywanie wstępnie skonfigurowane rozwiązania] [ lnk-customize] w tym artykule opisano kilka metod, można rozszerzyć ten przykład. Możliwe rozszerzenia obejmują przy użyciu rzeczywistych czujniki i wykonywania dodatkowych poleceń.

Dowiedz się więcej o [uprawnienia azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
