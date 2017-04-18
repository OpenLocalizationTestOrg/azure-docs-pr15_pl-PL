<properties
 pageTitle="Obsługa IoT Centrum MQTT | Microsoft Azure"
 description="Opis MQTT obsługi w poziomie Centrum IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT MQTT Centrum pomocy technicznej

Centrum IoT umożliwia urządzeń komunikować się z Centrum IoT punkty końcowe urządzenia, za pomocą [MQTT v3.1.1] [ lnk-mqtt-org] Protocol (protokół) na porcie 8883 lub v3.1.1 MQTT przez protokół WebSocket na porcie 443. Centrum IoT wymaga całą komunikację urządzenia, aby być chronione za pomocą protokołu TLS/SSL (w związku z tym Centrum IoT nie obsługuje-secure połączeń przez port 1883).

## <a name="connecting-to-iot-hub"></a>Nawiązywanie połączenia z Centrum IoT

Urządzenia umożliwia łączenie do koncentratora IoT za pomocą bibliotek w [Programie Microsoft Azure IoT SDK] protokołu MQTT[ lnk-device-sdks] lub przy użyciu MQTT protokół bezpośrednio.

## <a name="using-the-device-client-sdks"></a>Korzystanie z klienta urządzenia SDK

[Urządzenia klienta SDK] [ lnk-device-sdks] obsługujące protokół MQTT są dostępne dla języka Java, Node.js, C, C# i Python. Klient urządzenia SDK używać standardowego parametry połączenia Centrum IoT nawiązanie połączenia do koncentratora IoT. Do korzystania z protokołu MQTT, parametr Protokół klienta musi być równa **MQTT**. Domyślnie klient urządzenia, który SDK podłączyć do koncentratora IoT z **CleanSession** flagi jest ustawiona wartość **0** i **QoS 1** za pomocą wymiany wiadomości z poziomu Centrum IoT.

Gdy urządzenie jest podłączone do koncentratora IoT, klienta urządzenia SDK omówiono metody włączyć urządzenie, aby wysyłać i odbierać wiadomości z Centrum IoT.

Poniższa tabela zawiera łącza do przykładów kodu dla poszczególnych języków obsługiwanych i określić parametr w celu ustanowienia połączenia z Centrum IoT przy użyciu protokołu MQTT za pomocą.

| Język                   | Parametr Protocol (protokół)        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure-iot urządzenia — mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migracja aplikacji dla urządzeń z AMQP do MQTT
Jeśli korzystasz z [urządzenia klienta SDK][lnk-device-sdks], przenoszenie się z pomocą AMQP do MQTT wymaga zmiany parametru protokołu podczas inicjowania klienta, jak już wspomniano.

Jeśli ten sposób upewnij się, że sprawdź następujące elementy:

* AMQP zwraca błędy w przypadku wielu warunków, gdy MQTT połączenie zakończone. W wyniku do obsługi logiczny wyjątków może być wymagane zmiany.
* MQTT nie obsługuje operacji *odrzucenia* po odebraniu [wiadomości C2D][lnk-messaging]. Jeśli do wewnętrznej musi otrzymujesz odpowiedź od aplikacji urządzenia, warto rozważyć użycie [metody bezpośredni][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Bezpośrednio przy użyciu protokołu MQTT

Jeśli urządzenie nie za pomocą urządzenia klienta SDK, nadal można połączyć do punktów końcowych urządzeń publicznych przy użyciu protokołu MQTT. W pakiecie **POŁĄCZ** urządzenie należy używać następujących wartości:

- W polu **ClientId** za pomocą **identyfikator urządzenia**. 
- W polu **Nazwa użytkownika** za pomocą `{iothubhostname}/{device_id}`, gdzie {iothubhostname} jest pełny CName z poziomu Centrum IoT.

    Na przykład jeśli nazwa Twoim Centrum IoT jest **contoso.azure devices.net** i nazwa urządzenia jest **MyDevice01**, pełny pole **Username** powinien zawierać `contoso.azure-devices.net/MyDevice01`.

- W polu **hasło** za pomocą tokenu skojarzeń zabezpieczeń. Format tokenu skojarzeń zabezpieczeń jest taka sama jak protokoły HTTP i AMQP:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Aby uzyskać więcej informacji o sposobach generowania tokenów skojarzeń zabezpieczeń, zobacz sekcję urządzenia [za pomocą Centrum IoT]tokenów zabezpieczających[lnk-sas-tokens].
    
    Podczas testowania, można również użyć [Eksploratora urządzenia] [ lnk-device-explorer] narzędzie, aby szybko wygenerować tokenu skojarzeń zabezpieczeń, który można kopiować i wklejać do własnego kodu:
    
    1. Przejdź na kartę **Zarządzanie** w Eksploratorze urządzenia.
    2. Kliknij pozycję **Token skojarzeń zabezpieczeń** (prawy górny róg).
    3. Na **SASTokenForm**, wybierz urządzenie **identyfikator urządzenia** listy rozwijanej. Ustawianie z **pola TTL**.
    4. Kliknij pozycję **Generuj** , aby utworzyć token.
    
    Token skojarzeń zabezpieczeń, który jest generowany ma to strukturę:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Część token ten używany jako pole **hasła** , aby nawiązać połączenie za pomocą MQTT:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

Dla MQTT połączenia i odłączyć pakietów, Centrum IoT problemy zdarzenie w kanale **Monitorowania operacji** z dodatkowymi informacjami, które mogą ułatwić rozwiązywanie problemów z łącznością.

### <a name="sending-messages-to-iot-hub"></a>Wysyłanie wiadomości do Centrum IoT

Po wprowadzeniu połączenia, urządzenia można wysyłać wiadomości do Centrum IoT przy użyciu `devices/{device_id}/messages/events/` lub `devices/{device_id}/messages/events/{property_bag}` jako **Nazwę tematu**. `{property_bag}` Element umożliwia urządzeniu na wysyłanie wiadomości z dodatkowe właściwości w formacie zakodowany adres url. Na przykład:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] To `{property_bag}` element korzysta z tego samego kodu dla ciągów kwerendy w protokołu HTTP.

Można również użyć aplikacji klienckiej urządzenia `devices/{device_id}/messages/events/{property_bag}` jako **będzie nazwa tematu** do definiowania *będzie wiadomości* mają być przekazywane jako wiadomości telemetrycznego.

Centrum IoT nie obsługuje QoS 2 wiadomości. Jeżeli klient urządzenia opublikuje wiadomość z **QoS 2**, Centrum IoT zamyka połączenia sieciowego.
Centrum IoT nie zmieniają Przechowuj wiadomości. Jeśli urządzenie wysyła wiadomość flagą **PRZECHOWUJ** równa 1, dodaje koncentratora IoT **x i zachowują** właściwości aplikacji do wiadomości. W tym przypadku zamiast utrzymuje wiadomości Przechowuj, Centrum IoT przekazuje je do wewnętrznej bazy danych aplikacji.

### <a name="receiving-messages"></a>Odbieranie wiadomości

Do odbierania wiadomości z Centrum IoT, urządzenie należy subskrypcji, za pomocą `devices/{device_id}/messages/devicebound/#` jako **Filtr tematu**. Wielopoziomowe symbolu wieloznacznego **#** w filtrze tematu jest używany tylko do Zezwalaj urządzeniu na odbieranie dodatkowe właściwości w polu Nazwa tematu. Centrum IoT nie zezwala na użycie **#** lub **?** symbole wieloznaczne filtrowania tematów podrzędnych. Ponieważ Centrum IoT nie jest uniwersalny brokera wiadomości pub sub, ją obsługuje tylko nazwy udokumentowane tematów i filtry tematu.

Sprawdź notatkę, że urządzenie nie będziesz otrzymywać komunikaty z Centrum IoT, przed pomyślnie subskrybowanych do punktu końcowego określonego urządzenia reprezentowany przez `devices/{device_id}/messages/devicebound/#` filtr tematu. Po ustaleniu pomyślnego subskrypcji urządzenie rozpocznie się odbierania tylko wiadomości chmury do urządzenia, które zostały wysłane do niego po subskrypcji. Jeśli urządzenie łączy się z **CleanSession** flagi jest ustawiona wartość **0**, subskrypcji zostaną zachowywane między różnymi sesjami. W tym przypadku następnym razem ją realizuje połączenie z **CleanSession 0** urządzenie otrzyma zaległych wiadomości, które zostały wysłane do niego, gdy był on odłączony. Jeśli urządzenie jest używany flaga **CleanSession** ustawiona na **1** , ale go nie będziesz otrzymywać żadnych wiadomości z IoT Centrum do momentu jej subskrybuje punktu końcowego urządzenia.

Centrum IoT dostarcza wiadomości z **Nazwą tematu** `devices/{device_id}/messages/devicebound/`, lub `devices/{device_id}/messages/devicebound/{property_bag}` w przypadku dowolnej właściwości wiadomości. `{property_bag}`zawiera pary klucz wartość zakodowany adres url właściwości wiadomości. Tylko właściwości aplikacji oraz właściwości systemu można ustawić użytkownika (na przykład **komunikatu** lub **correlationId**) są uwzględniane w zbiorze właściwości. Nazwy właściwości systemu mają prefiks **$**, właściwości aplikacji za pomocą oryginalną nazwą właściwości nie prefiksu.

Gdy klient urządzenia subskrybuje tematu z **QoS 2**, Centrum IoT udziela maksymalna QoS poziomu 1 w pakiecie **SUBACK** . Po wykonaniu tej Centrum IoT będzie dostarczania wiadomości do urządzenia przy użyciu QoS 1.

### <a name="additional-considerations"></a>Uwagi dodatkowe

Jako wersję ostateczną uwagę, jeśli musisz dostosować zachowanie protokołu MQTT po stronie w chmurze należy przejrzeć [bramy protokołu Azure IoT] [ lnk-azure-protocol-gateway] umożliwiająca wdrażanie protokołu niestandardowego wysokiej wydajności bramy, która bezpośrednio z Centrum IoT. Brama protokołu Azure IoT umożliwia dostosowywanie protokołu urządzenia, aby zezwalały na wdrożeń MQTT brownfield lub inne protokoły niestandardowe. Ta metoda wymaga jednak uruchomieniu i działania bramy protokołu niestandardowego.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [notatki MQTT obsługuje] [ lnk-mqtt-devguide] w podręczniku Azure IoT Centrum deweloperów.

Aby dowiedzieć się więcej na temat protokołu MQTT, zapoznaj się z [dokumentacją MQTT][lnk-mqtt-docs].

Aby dowiedzieć się więcej na temat planowania wdrożenia IoT koncentratora, zobacz:

- [Azure certyfikowanych IoT urządzenia katalogu][lnk-devices]
- [Protokoły dodatkowe pomocy technicznej][lnk-protocols]
- [Porównanie z koncentratorów zdarzenia][lnk-compare]
- [Skalowanie, HA i DR][lnk-scaling]

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
