> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Wprowadzenie

Urządzenia twins są dokumenty JSON, których są przechowywane informacje o stanie urządzenia (metadane, konfiguracji i warunków). Centrum IoT będzie nadal występował podwójnego urządzenia, dla każdego urządzenia, które jest podłączone do koncentratora IoT.

Za pomocą urządzenia twins do:

* Przechowywanie metadane urządzenia z sieci wewnętrznej.
* Raport bieżące informacje o stanie takich jak możliwości i warunków (na przykład łączności metodę) z Twojej aplikacji urządzenie.
* Stan długotrwałych przepływy pracy (na przykład aktualizacje oprogramowania układowego i konfiguracji) Synchronizowanie aplikacji urządzenia i wewnętrznej.
* Kwerendy metadane urządzenia, konfiguracji lub stan.

> [AZURE.NOTE] Twins urządzenia są przeznaczone dla synchronizacji i wykonywanie zapytań za konfiguracji urządzeń i warunków. Za pomocą [urządzenia w chmurze wiadomości] [ lnk-d2c] sekwencji zdarzeń oznaczony znacznikiem czasowym (na przykład telemetrycznego strumieni danych opartych na czasie czujnika) i [metod chmurze do urządzenia] [ lnk-methods] dla kontrolkę interakcyjną urządzeń, takich jak włączenie wentylatora za pomocą aplikacji kontrolowane przez użytkownika.

Twins urządzenia są przechowywane w Centrum IoT i zawierają:

* *znaczniki*, urządzenia metadane dostępne tylko dla wewnętrznej;
* *odpowiednie właściwości*, JSON obiektów można modyfikować, wewnętrznej i według za pomocą aplikacji urządzenia; oraz
* Kończenie *zgłoszoną właściwości*, JSON obiektów można modyfikować za pomocą aplikacji urządzenia i czytelna przez tła. Znaczniki i właściwości nie mogą zawierać tablic, ale mogą być zagnieżdżane obiektów.

![][img-twin]

Ponadto wewnętrznej aplikacji można kwerendę twins urządzenia na podstawie powyższych danych.
Odwołują się do [Opis urządzenia twins] [ lnk-twins] uzyskać więcej informacji o twins urządzenia i [języka kwerend Centrum IoT] [ lnk-query] odwołania do wykonywania kwerend.

> [AZURE.NOTE] W tej chwili twins urządzenia są dostępne tylko z urządzeń łączących się Centrum IoT przy użyciu protokołu MQTT. Zajrzyj do [pomocy technicznej MQTT] [ lnk-devguide-mqtt] artykuł, aby uzyskać instrukcje dotyczące jak przekonwertować istniejącej aplikacji urządzenie za pomocą MQTT.

W tym samouczku pokazano sposób do:

- Tworzenie aplikacji wewnętrznej, który zapewnia *znaczniki* podwójnego urządzenia, a urządzenie symulowany raporty kanału łączności jako *zgłoszone właściwość* na podwójnego urządzenia.
- Kwerenda urządzeń z Twojej aplikacji wewnętrznej na znaczników i właściwości poprzednio utworzone przy użyciu filtrów.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md