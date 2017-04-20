
# <a name="azure-and-internet-of-things"></a>Azure i Internet rzeczy

Witamy w Microsoft Azure i Internet przedmiotów (). W tym artykule wprowadza architektury rozwiązania IoT, która opisuje typowe cechy roztworu IoT, który zostanie wdrożony przy użyciu usług Azure. Rozwiązania IoT wymagają bezpiecznego komunikację dwukierunkową między urządzeniami, ewentualnie numerowania w milionach i typu back-end rozwiązanie. Na przykład typu back-end rozwiązanie może umożliwia automatyczne analiz predykcyjnych uzyskiwanie szczegółowych informacji z strumienia zdarzenia urządzenia do chmury.

Centrum IoT Azure jest blok konstrukcyjny klucza podczas implementowania tej architektury rozwiązania IoT przy użyciu usług Azure. Pakiet iot zawiera kompletne, end-to-end, implementacje tej architektury w określonych scenariuszach IoT. Na przykład: 

- *Zdalne monitorowanie* rozwiązanie umożliwia monitorowanie stanu urządzeń takich jak automaty. 
- Rozwiązanie *konserwacji predykcyjnej* pomaga przewidywać potrzeby konserwacji urządzeń, takich jak pompy w zdalnym pompowni i uniknąć nieplanowanych przestojów.

## <a name="iot-solution-architecture"></a>Architektura rozwiązania IoT

Na poniższym diagramie przedstawiono typowy architektury rozwiązania IoT. Diagram nie zawiera nazw żadnych szczególnych usług Azure, ale w tym artykule opisano kluczowe elementy rodzajowe architektury rozwiązania IoT. W takiej architekturze urządzeń IoT zbierania danych przesyłane są do bramy chmury. Bramy chmury udostępnia dane do przetwarzania przez inne usługi zaplecza, z którym dane dostarczane są do innych aplikacji biznesowych lub podmiotom ludzi za pośrednictwem pulpitu nawigacyjnego lub inne urządzenie prezentacji.

![Architektura rozwiązania IoT][img-solution-architecture]

> [AZURE.NOTE] Szczegółowe omówienie architektury IoT, zobacz [Microsoft Azure IoT odniesienia architektury][lnk-refarch].

### <a name="device-connectivity"></a>Urządzenia łączności

W tej architektury rozwiązania IoT urządzeń Wyślij telemetrycznego, takich jak dane pomiarowe ze stacji pomp, do końcowego chmura dla przechowywania i przetwarzania. W scenariuszu konserwacji predykcyjnej back-end mogą służyć strumienia danych z czujników do określenia, kiedy konkretne pompy wymaga konserwacji. Urządzenia można również otrzymywać i reagować na polecenia urządzenia cloud czytając wiadomości od końcowego chmury. Na przykład w scenariuszu konserwacji predykcyjnej back-end rozwiązanie może wysyłać polecenia do innych pomp w stacji pomp, aby rozpocząć skierowanie przepływów tuż przed konserwacji ma się rozpocząć upewnić się, że inżynier utrzymania można rozpocząć po jej odebraniu.

Jednym z największych wyzwań dla projektów IoT jest sposób niezawodną i bezpieczną podłączania urządzeń do rozwiązania back-end. IoT urządzenia mają różne charakterystyki, w porównaniu do innych klientów, takich jak przeglądarek i aplikacji dla urządzeń przenośnych. Urządzenia IoT:

- Są często osadzonych systemów nie operatorem ludzi.
- Można wdrożyć w lokalizacjach zdalnych, gdzie dostęp fizyczny jest kosztowne.
- Może być tylko osiągalny przez roztwór back-end. Istnieje inny sposób interakcji z urządzeniem.
- Może być bardzo ograniczona moc i przetwarzania zasobów.
- Może mieć łączność sieciowa sporadyczne, powolna lub droga.
- Może być konieczne używanie protokołów własnościowych, niestandardowa lub branżowe aplikacji.
- Można tworzyć, używając duży zestaw popularnych platform sprzętowych i programowych.

Oprócz powyższych wymagań dowolne rozwiązanie IoT musi również dostarczyć skali, bezpieczeństwo i niezawodność. Wynikowy zestaw wymagania łączności jest czasochłonne zaimplementować przy użyciu tradycyjnych technologii takich jak kontenery sieci web i obsługą wiadomości brokerów. Centrum IoT Azure i zestawy SDK urządzenia IoT ułatwiają wdrażanie rozwiązań, które spełniają te wymagania.

Urządzenia mogą komunikować się bezpośrednio z punktu końcowego bramy chmury, lub jeśli urządzenie nie korzysta z protokołów komunikacyjnych, które obsługuje bramy chmury, można połączyć za pomocą bramy pośrednich. Na przykład [brama protokołu Azure IoT] [ lnk-protocol-gateway] można wykonać translacji protokołu, jeśli urządzenia nie można używać żadnych protokołów, które obsługuje Centrum IoT.

### <a name="data-processing-and-analytics"></a>Przetwarzania danych i analizy

W chmurze IoT zakończyć powrotem rozwiązanie jest, gdzie występuje większość przetwarzania danych, takich jak filtrowanie i agregowania telemetrycznego i przesyłać go do innych usług. Zakończ rozwiązania IoT wstecz:

- Odbiera telemetrycznego w skali od urządzeń i określa sposób przetwarzania i przechowywania danych. 
- Mogą włączyć do wysyłania poleceń z chmury do konkretnego urządzenia.
- Zapewnia funkcji rejestracji urządzenia, które zapewniają świadczenia urządzeń oraz do kontroli, które urządzenia mogą połączyć się do infrastruktury.
- Umożliwia śledzenie stanu urządzeń i monitorowanie ich działania.

W scenariuszu konserwacji predykcyjnej back-end rozwiązanie przechowuje dane historyczne telemetrycznego. Back-end mogą używać tych danych identyfikować wzorce, które wskazują, że są należne na konkretne pompy.

Rozwiązania IoT mogą zawierać sprzężeń zwrotnych automatyczne. Na przykład moduł analiz w back-end można zidentyfikować z telemetrycznego, że temperatura konkretnego urządzenia jest wyższa od normalnego poziomu działalności. Rozwiązania mogą następnie wysyłać polecenia do urządzenia, instrukcją do podjęcia działań naprawczych.

### <a name="presentation-and-business-connectivity"></a>Prezentacja i business łączność

Do prezentacji i biznesowych połączeń warstw pozwala użytkownikom końcowym do interakcji z rozwiązania IoT i urządzeń. Umożliwia użytkownikom wyświetlanie i analizowanie danych zebranych z ich urządzeniami. Widoki te mogą przyjąć formę pulpitów nawigacyjnych i raportów analizy Biznesowej, które mogą być wyświetlane zarówno dane historyczne lub w pobliżu danych w czasie rzeczywistym. Na przykład operatora można sprawdzić stan określonego pompowni i Zobacz wszystkie alerty wygenerowane przez system. Warstwa ta umożliwia także integrację IoT rozwiązanie back-end z istniejących aplikacji biznesowych związać w przedsiębiorstwie procesów biznesowych lub przepływów pracy. Na przykład rozwiązanie konserwacji predykcyjnej można zintegrować z systemu planowania, w którym książek inżyniera do odwiedzenia stacji pomp, gdy roztwór identyfikuje pompy potrzebie konserwacji.

![Pulpit nawigacyjny rozwiązania IoT][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
