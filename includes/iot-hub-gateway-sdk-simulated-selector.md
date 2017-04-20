> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Systemu Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

[Przekaż symulowanego urządzenia chmury próbce] w tym instruktażu przedstawiono sposób użycia [Zestawu SDK Azure IoT Gateway] [ lnk-sdk] do wysyłania telemetrycznego urządzenia do chmury Centrum IoT z urządzeń symulowane.

W tym instruktażu obejmuje:

1. **Architektura**: ważne architektury informacji na temat próbki symulowane przekazać chmury urządzenia.

2. **Tworzenie i uruchamianie**: kroki wymagane do tworzenia i uruchamiania próbki.

## <a name="architecture"></a>Architektura

Symulowane urządzenie chmury przesłać próbki pokazuje, jak utworzyć bramę, który wysyła telemetrycznego z symulowanego urządzeń do Centrum iot — za pomocą zestawu SDK. Symulowane urządzeń nie może połączyć się bezpośrednio do Centrum IoT, ponieważ:

- Urządzenia nie należy używać protokołem komunikacyjnym zrozumiane przez Centrum IoT.
- Urządzenia nie są dostatecznie inteligentny, aby pamiętać, tożsamość przypisana przez Centrum IoT.

Brama rozwiązuje te problemy dla symulowanych urządzeń w następujący sposób:

- Brama rozumie protokół używany przez urządzenia symulowanego odbiera telemetrycznego urządzenia do chmury od urządzeń i przekazuje te wiadomości do Centrum IoT przy użyciu protokołu zrozumiane przez koncentrator.
- Brama przechowuje Centrum IoT tożsamości w imieniu symulowanego urządzenia i działa jako serwer proxy symulowanego urządzenia wysyłania wiadomości do Centrum IoT.

Na poniższym diagramie przedstawiono główne składniki próbki, w tym moduły bramy:

![][1]


> [AZURE.NOTE] Moduły nie przechodzą wiadomości bezpośrednio ze sobą. Moduły publikowania komunikatów do wewnętrznego broker, który dostarcza wiadomości do innych modułów przy użyciu mechanizmu subskrypcji, jak pokazano na rysunku poniżej. Aby uzyskać więcej informacji, zobacz temat [wprowadzenie zestawu SDK bramy IoT][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protokół spożyciu modułu

Moduł ten jest punktem początkowym dla pobieranie danych z urządzenia, przy użyciu bramy i w chmurze. W tej próbce moduł wykonuje cztery zadania:

1.  Umożliwia tworzenie danych symulowanego temperatury.
    
    Uwaga: jeśli uzywasz rzeczywistych urządzeń modułu może odczytywać dane z tych urządzeń fizycznych.

2.  To umieszcza dane symulowanej temperatury w treści wiadomości.

3.  Dodaje właściwość o fałszywych adresów MAC do wiadomość, która zawiera dane symulowanej temperatury.

4.  Udostępnia wiadomości do następnego modułu łańcucha.

> [AZURE.NOTE] Moduł o nazwie **spożyciu protokołu X** na powyższym diagramie jest nazywany **symulowane urządzenie** w kodzie źródłowym.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT Hub identyfikator modułu

Ten moduł skanuje w poszukiwaniu wiadomości, które zawierają właściwość, która zawiera adres MAC, dodane przez moduł spożyciu protokołu symulacji. Jeśli moduł znajdzie takie właściwości, dodaje inną właściwość z klucza urządzenia Centrum IoT do wiadomości i następnie udostępnia wiadomości do następnego modułu łańcucha. Jest to, jak próbki kojarzy tożsamości urządzeń Centrum IoT z urządzeniami symulowane. Deweloper konfiguruje mapowanie między adresów MAC i tożsamości Centrum IoT ręcznie w ramach konfiguracji modułu. 

> [AZURE.NOTE]  Ten przykład używa adresu MAC jako unikatowy identyfikator urządzenia i jest całkowicie skorelowany z Centrum IoT tożsamości urządzenia. Można jednak napisać własny moduł, który używa inny identyfikator unikatowy. Na przykład może mieć urządzenia unikatowe numery seryjne lub dane telemetrycznego ma nazwę unikatową urządzenie osadzone w nim, które można użyć w celu określenia tożsamości urządzenia Centrum IoT.

### <a name="iot-hub-communication-module"></a>Centrum IoT modułu komunikacyjnego

Moduł ten przyjmuje wiadomości przy użyciu koncentratora IoT urządzenia tożsamości przypisany przez poprzedniego modułu i wysyła treść wiadomości do Centrum IoT przy użyciu protokołu HTTP. HTTP jest jednym z trzech protokołów zrozumiałych Centrum IoT.

Zamiast otwierania połączenia do Centrum IoT dla każdego urządzenia symulowane, moduł ten otwiera jednego połączenia HTTP z bramy do Centrum IoT i multipleksów połączenia ze wszystkich urządzeń symulowane przez to połączenie. Umożliwia to pojedyncza brama połączyć wiele więcej urządzeń, symulowane lub w inny sposób, niż byłoby to możliwe, jeśli otwarte połączenie unikatowy dla każdego urządzenia.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Symulowane próbki przekazać chmury urządzenia]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md