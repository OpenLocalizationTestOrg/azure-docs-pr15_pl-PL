> [AZURE.SELECTOR]
- [C w systemie Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C w systemie Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C na mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Omówienie scenariusza

W tym scenariuszu stworzyć urządzenie, które wysyła następujące przyspieszając do zdalnego monitorowania [wstępnie skonfigurowane rozwiązanie][lnk-what-are-preconfig-solutions]:

- Temperatury zewnętrznej
- Temperatura wewnętrzna
- Wilgotność

Dla uproszczenia wartości próbki generuje kod na urządzeniu, ale zachęcamy do rozszerzyć próbki połączenie z urządzeniem rzeczywistych czujniki i wysyłając telemetrycznego prawdziwe.

Aby użyć tego samouczka, należy usłudze active Directory. Jeśli nie masz konta, można utworzyć darmowe konto próbne w zaledwie kilka minut. [Aby uzyskać szczegółowe informacje, zobacz][lnk-free-trial].

## <a name="before-you-start"></a>Przed rozpoczęciem

Przed pisania kodu dla danego urządzenia, należy obsługi administracyjnej zdalnego monitorowania roztworu wstępnie skonfigurowanych i obsługi administracyjnej nowe urządzenie niestandardowe w tym roztworze.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Zapewnij zdalnego monitorowania roztworu wstępnie skonfigurowane

Urządzenie, Utwórz w tym samouczku wysyła dane do wystąpienia [zdalnego monitorowania] [ lnk-remote-monitoring] wstępnie skonfigurowane rozwiązanie. Jeżeli nie zostało już przygotowana zdalne monitorowanie wstępnie skonfigurowane rozwiązanie na koncie Azure, wykonaj następujące czynności:

1. Na stronie <https://www.azureiotsuite.com/> kliknij **+** do utworzenia nowego rozwiązania.

2. Kliknij przycisk **Wybierz** w panelu **zdalne monitorowanie** , aby utworzyć nowe rozwiązanie.

3. Na stronie **Tworzenie zdalnego monitorowania rozwiązania** wprowadź wybraną **nazwę rozwiązania** , zaznacz **Region** ma być wdrożony do i wybierz Azure subskrypcję chcesz użyć. Kliknij przycisk **Utwórz rozwiązanie**.

4. Poczekaj, aż do zakończenia procesu zastrzegania.

> [AZURE.WARNING] Wstępnie skonfigurowane rozwiązania za pomocą usług Azure rozliczaniu. Należy koniecznie usunąć wstępnie skonfigurowane rozwiązanie z subskrypcji po zakończeniu z nim, aby uniknąć zbędnych opłat. Wstępnie skonfigurowane rozwiązanie można całkowicie usunąć z subskrypcji, odwiedzając stronę <https://www.azureiotsuite.com/> .

Po zakończeniu procesu zastrzegania dla zdalnego monitorowania rozwiązania, kliknij przycisk **Uruchom** , aby otworzyć Panel rozwiązanie w przeglądarce.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Zapewnij urządzenia w roztworze zdalnego monitorowania

> [AZURE.NOTE] Jeżeli urządzenie ma już zaopatrzona w rozwiązaniu, można pominąć ten krok. Należy znać poświadczenia urządzenia podczas tworzenia aplikacji klienta.

Dla urządzenia połączyć się wstępnie skonfigurowane rozwiązanie to musi zidentyfikować się Centrum IoT przy użyciu prawidłowych poświadczeń. Z roztworu pulpitu nawigacyjnego można pobrać poświadczeń urządzenia. Poświadczenia urządzenia dołączenie do aplikacji klienta, w dalszej części tego samouczka. 

Aby dodać nowe urządzenie do zdalnego monitorowania rozwiązania, należy wykonać następujące czynności w panelu rozwiązania:

1.  W lewym dolnym rogu pulpitu nawigacyjnego kliknij przycisk **Dodaj urządzenie**.

    ![][1]

2.  W panelu **Urządzenia niestandardowe** kliknij **Dodaj nowe**.

    ![][2]

3.  Wybierz opcję **Pozwól mi określić własne identyfikator urządzenia**, wprowadź identyfikator urządzenia, takie jak **mydevice**, kliknij przycisk **Sprawdź identyfikator** Sprawdź, czy że nazwa nie jest już w użyciu, a następnie kliknij przycisk **Utwórz** zainicjować obsługi urządzenia.

    ![][3]

5. Zanotuj poświadczenia urządzenia (identyfikator urządzenia, nazwa hosta koncentrator IoT i klucza urządzenia), aplikacja kliencka musi je połączyć do zdalnego monitorowania roztworu. Następnie kliknij przycisk **Gotowe**.

    ![][4]

6. Upewnij się, że urządzenia są wyświetlane w sekcji urządzenia. Stan urządzenia trwa **Oczekiwanie** , aż urządzenie nawiąże połączenie z rozwiązaniami do monitorowania zdalnego.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/