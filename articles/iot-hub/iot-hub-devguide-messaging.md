<properties
 pageTitle="Przewodnik dewelopera — wiadomości | Microsoft Azure"
 description="Azure guide Deweloper Centrum IoT - urządzenia w chmurze i wiadomości w chmurze do urządzenia"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Wysyłanie i odbieranie wiadomości z Centrum IoT

## <a name="overview"></a>Omówienie

Centrum IoT udostępnia następujące pierwotnych wiadomości do komunikowania się z urządzeniem:

- [Urządzenia w chmurze] [ lnk-d2c] z urządzenia do aplikacji wewnętrzna baza danych.
- [Chmura do urządzenia] [ lnk-c2d] z aplikacji wewnętrzna baza danych (lub*Usługa* w *chmurze*).

Podstawowe właściwości Centrum IoT z obsługą wiadomości są niezawodności i ważności wiadomości. Te właściwości włączyć odporność do przerw w łączności na stronie urządzenia i załadować tych najwyższych wartościach w przypadku przetwarzania po stronie chmury. Centrum IoT zawiera *co najmniej raz* dostarczania gwarancje zarówno w urządzenia w chmurze, jak i w chmurze do urządzenia wiadomości.

Centrum IoT obsługuje wiele [protokołów skierowaną w urządzeniu] [ lnk-protocols] (na przykład MQTT, AMQP i HTTP). Do obsługi bezproblemowe współdziałanie przez protokoły, Centrum IoT definiuje [popularnym formatem wiadomości] [ lnk-message-format] obsługujący wszystkie protokoły skierowaną urządzenia.

Centrum IoT udostępnia punktu [końcowego zgodnego z programem zdarzenia Centrum] [ lnk-compatible-endpoint] aby umożliwić aplikacji wewnętrznej odczytywać wiadomości z urządzenia w chmurze odebrana przez Centrum.

### <a name="when-to-use"></a>Kiedy należy używać

Obsługa wiadomości jest podstawowe możliwości Centrum IoT. Używaj go, gdy chcesz wysyłać wiadomości z urządzenia w celu przechodzenia wstecz lub wysyłać wiadomości z sieci wewnętrznej do urządzenia.

Porównanie usług Centrum IoT i koncentratory zdarzenia, zobacz [Porównanie IoT Centrum i koncentratory zdarzenia][lnk-compare].

## <a name="device-to-cloud-messages"></a>Urządzenia w chmurze wiadomości

Możesz wysyłać wiadomości urządzenia w chmurze punkt końcowy skierowaną urządzenia (**/devices/ {identyfikator urządzenia}-wiadomości i zdarzeń**). Usługi wewnętrznej odbiera wiadomości urządzenia w chmurze za pośrednictwem punktu końcowego usługi skierowaną (**/messages/events**), zgodną z [Koncentratorów zdarzenia][lnk-event-hubs]. W związku z tym, można użyć standardowego [integracji koncentratory zdarzeń i SDK] [ lnk-compatible-endpoint] otrzymywać wiadomości urządzenia w chmurze.

Centrum IoT wykonuje urządzenia w chmurze wiadomości w sposób, który jest podobny do [Koncentratorów zdarzenia][lnk-event-hubs]. Centrum IoT urządzenia w chmurze wiadomości są bardziej jak koncentratory zdarzenia *zdarzeń* niż [Usługa Bus] [ lnk-servicebus] *wiadomości*.

Ta implementacja ma wpływ następujące czynności:

* Podobnie do zdarzenia koncentratory zdarzenia, urządzenia w chmurze wiadomości są trwałe i zatrzymane w Centrum IoT maksymalnie siedem dni (zobacz [Opcje konfiguracji urządzenia w chmurze][lnk-d2c-configuration]).
* Urządzenia w chmurze wiadomości są podzielona przez ustalony zbiór partycje na godzinę utworzenia (zobacz [Opcje konfiguracji urządzenia w chmurze][lnk-d2c-configuration]).
* Analogously do koncentratorów zdarzenia klientów czytanie wiadomości urządzenia w chmurze musi obsługiwać partycje i punkt kontrolny. Zobacz [Koncentratory zdarzenia — przez inne zdarzenia][lnk-event-hubs-consuming-events].
* Podobnie jak zdarzenia koncentratory zdarzenia wiadomości urządzenia w chmurze może mieć maksymalnie 256 KB i mogą być grupowane partiami zoptymalizować wysyła. Partii może być co najwyżej 256 KB i co najwyżej 500 wiadomości.

Istnieją jednak kilka ważnych różnice między IoT Centrum obsługi urządzeń w chmurze wiadomości i koncentratory zdarzeń:

* Zgodnie z opisem w oknie [Kontrolowanie dostępu do Centrum IoT] [ lnk-devguide-security] sekcji Centrum IoT zapewnia na urządzenie uwierzytelnianie i kontrola dostępu.
* Centrum IoT zapewnia miliony urządzeń jednocześnie (zobacz [przydziałów i ograniczania][lnk-quotas]), podczas gdy koncentratory zdarzenie jest ograniczone do 5000 połączeń AMQP na przestrzeń nazw.
* Centrum IoT nie zezwala na dowolnego partycje za pomocą **PartitionKey**. Urządzenia w chmurze wiadomości są podzielona według ich źródłowym **identyfikator urządzenia**.
* Skalowanie Centrum IoT nieznacznie różni się od skalowania koncentratory zdarzenia. Aby uzyskać więcej informacji, zobacz [Centrum IoT skalowania][lnk-guidance-scale].

> [AZURE.NOTE] Nie można zastąpić Centrum IoT dla zdarzenia koncentratorów we wszystkich scenariuszach. Na przykład w niektórych obliczeń przetwarzanie zdarzenia, może być konieczne na partycje zdarzeń w odniesieniu do różnych właściwość lub pole przed analizowanie strumienie danych. W tym scenariuszu można użyć koncentratora zdarzenia rozdzielenie dwie części strumienia przetwarzania procesu. Aby uzyskać więcej informacji, zobacz *partycje* w [Azure zdarzenia koncentratory omówienie][lnk-eventhub-partitions].

Aby uzyskać szczegółowe informacje dotyczące sposobu korzystania z urządzenia w chmurze wiadomości, zobacz [interfejsów API Centrum IoT i SDK][lnk-sdks].

> [AZURE.NOTE] Podczas wysyłania wiadomości urządzenia w chmurze przy użyciu protokołu HTTP, nazwy i wartości właściwości może zawierać tylko znaki alfanumeryczne ASCII, oraz ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Użytkownicy niebędący telemetrycznego ruchu

Często oprócz telemetrycznego punktów danych, urządzenia też wysyłać wiadomości i żądania, które wymagają wykonania i obsługi z warstwę logiki aplikacji biznesowej. Na przykład krytyczne alertów, które muszą wyzwalanie określonych akcji w wewnętrznej lub odpowiedź urządzenia do poleceń wysłanych z wewnętrzna.

Aby uzyskać więcej informacji o najlepszym sposobem procesu tego typu wiadomości, zobacz [Samouczek: sposobu przetwarzania wiadomości urządzenia w chmurze Centrum IoT] [ lnk-d2c-tutorial] samouczka.

### <a name="device-to-cloud-configuration-options"></a>Opcje konfiguracji urządzenia w chmurze

Centrum IoT udostępnia następujące właściwości umożliwiają kontrolowanie wiadomości urządzenia w chmurze.

* **Liczba partycją**. Ustawiana właściwość podczas tworzenia, aby określić liczbę partycje dla spożyciu zdarzenia urządzenia w chmurze.
* **Czas przechowywania**. Ta właściwość określa czas przechowywania wiadomości urządzenia w chmurze. Wartość domyślna to jeden dzień, ale można zwiększyć siedem dni.

Ponadto analogously do koncentratorów zdarzenia Centrum IoT umożliwia zarządzanie grupy dla klientów indywidualnych na urządzeniu na w chmurze odbieranie punktu końcowego.

Można modyfikować wszystkie te właściwości, albo programowo za pośrednictwem [dostawcy zasobów Centrum IoT interfejsy API pozostałych][lnk-resource-provider-apis], lub za pomocą [Azure portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Fałszowaniu Anti właściwości

Aby uniknąć urządzenia fałszowaniu w wiadomościach urządzenia w chmurze, Centrum IoT sygnatury wszystkich wiadomości z następujących właściwości:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Dwa pierwsze zawierają **identyfikator urządzenia** i **generationId** źródłowym urządzenia, jak [Właściwości tożsamości urządzenia][lnk-device-properties].

Właściwość **ConnectionAuthMethod** zawiera obiekt JSON seryjnych o następujących właściwościach:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Wiadomości w chmurze do urządzenia

Wysyłanie wiadomości chmury do urządzenia za pośrednictwem punktu końcowego usługi skierowaną (**/messages/devicebound**). Urządzenie odbiera je za pośrednictwem punktu końcowego specyficzne dla urządzenia (**/devices/ {identyfikator urządzenia}-wiadomości-devicebound**).

Każda wiadomość w chmurze do urządzenia jest przeznaczona dla pojedynczego urządzenia przez ustawienie właściwości **do** **/devices/ {identyfikator urządzenia}-wiadomości-devicebound**.

>[AZURE.IMPORTANT] Każda kolejka urządzenia zawiera co najwyżej 50 wiadomości chmury do urządzenia. Próbujesz wysłać więcej wiadomości na tym samym urządzeniu wynikiem jest błąd.

> [AZURE.NOTE] Podczas wysyłania wiadomości w chmurze do urządzenia, nazwy i wartości właściwości może zawierać tylko znaki alfanumeryczne ASCII, oraz ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Cykl życia wiadomości

Aby mieć pewność co najmniej raz dostarczenia wiadomości, Centrum IoT będzie nadal występował chmury do urządzenia wiadomości w kolejkach na urządzenie. Urządzenia jawnie musi potwierdzić *ukończenia* Centrum IoT usunąć je z niej. Gwarantuje to ochrony przed łączności i awariami urządzenia.

Na poniższym diagramie przedstawiono na wykresie stan cyklu życia komunikatu chmury do urządzenia.

![Cykl życia wiadomości w chmurze do urządzenia][img-lifecycle]

Usługa wysyła wiadomość, jest traktowany jako *został umieszczony w kolejce*. Gdy urządzenie chce *otrzymywać* wiadomości, Centrum IoT *blokady* wiadomości (ustawia stan do **niewidoczne**) zezwalania innych wątków na tym samym urządzeniu, aby rozpocząć otrzymywać kolejnych wiadomości e-mail. Po zakończeniu przetwarzania wiadomości wątku urządzenia powiadamia Centrum IoT, *wykonując* wiadomości.

Urządzenie można także:

- *Odrzuć* wiadomości, która powoduje, że Centrum IoT ją ustawić stan **Deadlettered** . Uwaga: nawiązywanie połączenia z MQTT urządzenia nie ma możliwości odrzucenia C2D wiadomości.
- *Opuszczenie* wiadomości, który sprawia, że Centrum IoT, aby umieścić wiadomości w kolejce ze stanem ustaw **został umieszczony w kolejce**.

Wątek może nie działać przetwarzania wiadomości bez powiadomienia Centrum IoT. W tym przypadku wiadomości automatycznie przejść ze stanem **niewidoczne** do stanu **został umieszczony w kolejce** po *widoczność (lub Zablokuj) limit czasu*. Wartość domyślna tego limitu to minutę.

Wiadomości można przechodzenie między **został umieszczony w kolejce** i **niewidoczne** stany dla co najwyżej liczba określona dla właściwości **Liczba dostarczaniem maksymalny** na IoT Centrum. Po tej liczbie przejścia Centrum IoT ustawia stan wiadomości do **Deadlettered**. Podobnie, Centrum IoT ustawia stan wiadomości do **Deadlettered** po jego czas wygaśnięcia (zobacz [czas wygaśnięcia][lnk-ttl]).

Samouczek dotyczący wiadomości w chmurze do urządzenia, zobacz [Samouczek: sposobu wysyłania wiadomości w chmurze do urządzenia z Centrum IoT][lnk-c2d-tutorial]. Do tematów odwołanie w różnych interfejsów API i SDK udostępniają funkcje chmury do urządzenia, zobacz [interfejsy API Centrum IoT i SDK][lnk-sdks].

> [AZURE.NOTE] Zazwyczaj wiadomości w chmurze do urządzenia wykonaj przy każdym utratę wiadomości nie wpłynie logiki aplikacji. Na przykład treść wiadomości zostały pomyślnie zachowywane w magazynu lokalnego lub operacji została wykonana. Wiadomość może być obciążone przejściowych informacji, których utratę nie będzie mieć wpływ na funkcjonalność aplikacji. Czasami długim zadań, można wykonać wiadomości chmury do urządzenia po utrzymuje opis zadania w magazynu lokalnego. Następnie możesz powiadomić wewnętrznej aplikacji z jedną lub więcej wiadomości urządzenia w chmurze na różnych etapach postęp zadania.

### <a name="message-expiration-time-to-live"></a>Wygaśnięcia wiadomości (czas wygaśnięcia)

Każda wiadomość chmury do urządzenia ma czasu wygaśnięcia. Ten czas jest ustawiony przez usługę (WE właściwości **ExpiryTimeUtc** ) lub Centrum IoT przy użyciu domyślny *czas wygaśnięcia* określony jako właściwość IoT Centrum. Zobacz [Opcje konfiguracji chmury do urządzenia][lnk-c2d-configuration].

> [AZURE.NOTE] Typowa skorzystać z ważności wiadomości i uniknąć wysyłania wiadomości do urządzenia Rozłączono jest ustalenie krótki czas wygaśnięcia wartości. Tej metody uzyskuje taki sam wynik jak utrzymywanie stan połączenia urządzenia będąc zwiększyć jego wydajność. Jeśli zażądać potwierdzenia wiadomości, Centrum IoT o tym, które urządzenia będą mogli otrzymywać wiadomości i urządzeń nie są w trybie online lub nie powiodła się.

### <a name="message-feedback"></a>Opinie wiadomości

Podczas wysyłania wiadomości chmury do urządzenia usługę można zażądać potwierdzeń dostarczenia wiadomości informacji dotyczących stan końcowy tej wiadomości.

- Po ustawieniu właściwości **Ack** na **dodatnie**Centrum IoT generuje wiadomość opinii, jeśli i tylko wtedy, gdy wiadomość chmury do urządzenia osiągnięto stan **ukończone** .
- Po ustawieniu właściwości **Ack** **minus**Centrum IoT generuje komunikat opinii tylko wtedy, gdy, wiadomości chmury do urządzenia osiągnie stan **Deadlettered** .
- Po ustawieniu właściwości **Ack** do **pełnego**Centrum IoT generuje wiadomość opinii w każdym przypadku.

> [AZURE.NOTE] Jeśli **Ack** jest **Pełny**, a nie zostanie wyświetlony komunikat opinii, oznacza to, że wiadomość opinii wygasła. Usługa nie wiesz, co się stało z oryginalną wiadomość. W praktyce usługi powinny zapewnić, że może przetworzyć opinii przed jej wygaśnięciem. Czas wygaśnięcia maksymalna dwa dni, które umożliwia dużo czasu na wykonanie usługi działa ponownie wystąpi błąd.

Zgodnie z opisem w [punkty końcowe][lnk-endpoints], Centrum IoT zapewnia opinii za pośrednictwem punktu końcowego usługi skierowaną (**/messages/servicebound/feedback**) w postaci wiadomości. Znaczenie do odbierania opinii są takie same jak w przypadku wiadomości w chmurze do urządzenia i że masz samego [cyklu wiadomości][lnk-lifecycle]. Jeśli to możliwe, opinii wiadomości jest przetwarzany wsadowo w jednej wiadomości, przy zachowaniu następującego formatu:

| Właściwość | Opis |
| -------- | ----------- |
| EnqueuedTime | Sygnatura czasowa wskazująca, kiedy wiadomość została utworzona. |
| Nazwa użytkownika | `{iot hub name}` |
| Typ zawartości | `application/vnd.microsoft.iothub.feedback.json` |

Treść jest seryjny JSON tablicę rekordów, każda z następujących właściwości:

| Właściwość | Opis |
| -------- | ----------- |
| EnqueuedTimeUtc | Sygnatura czasowa wskazująca, kiedy się stało z wynikami wiadomości. Na przykład urządzenie wykonane lub wiadomość wygasła. |
| OriginalMessageId | **Komunikatu** wiadomości chmury do urządzenia, do którego odnosi się te informacje opinii. |
| StatusCode | Wymagane liczba całkowita. Używane w wiadomości opinii generowane przez Centrum IoT. <br/> 0 = sukcesu <br/> 1 = wygasł <br/> 2 = dostarczaniem maksymalny liczby <br/> 3 = message odrzucone |
| Opis | Ciąg wartości dla **StatusCode**. |
| Identyfikator urządzenia | **Identyfikator urządzenia** urządzenia wiadomości chmury do urządzenia, do którego odnosi się ten element opinii. |
| DeviceGenerationId | **DeviceGenerationId** urządzenia wiadomości chmury do urządzenia, do którego odnosi się ten element opinii. |


>[AZURE.IMPORTANT] Usługi, musisz określić **komunikatu** dla wiadomości chmury do urządzenia można było jej opinii być zgodne z oryginalną wiadomość.

W poniższym przykładzie pokazano w treści wiadomości opinii.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Opcje konfiguracji chmury do urządzenia

Każdy Centrum IoT udostępnia następujące opcje konfiguracji wiadomości w chmurze do urządzenia.

| Właściwość | Opis | Zakres i domyślne |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Domyślny czas TTL dla wiadomości w chmurze do urządzenia. | Interwał ISO_8601 maksymalnie 2D (minimalne 1 minuta). Wartość domyślna: 1 godzina. |
| maxDeliveryCount | Liczba dostarczaniem maksymalny dla kolejek chmury do urządzenia na urządzenie. | 1 do 100. Domyślne: 10. |
| feedback.ttlAsIso8601 | Zachowywanie opinii powiązanych z usługi wiadomości. | Interwał ISO_8601 maksymalnie 2D (minimalne 1 minuta). Wartość domyślna: 1 godzina. |
| feedback.maxDeliveryCount | Liczba dostarczaniem maksymalny dla kolejki opinii. | 1 do 100. Domyślne: 100. |

Aby uzyskać więcej informacji, zobacz [Tworzenie IoT koncentratory][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Czytanie wiadomości urządzenia w chmurze

Centrum IoT udostępnia punkt końcowy dla usług wewnętrznej do czytania wiadomości urządzenia w chmurze odebrana przez Twoim Centrum. Punkt końcowy to wydarzenie Centrum zgodnego z programem, pozwalającego na wszelkie mechanizmy za pomocą usługi koncentratory zdarzenie obsługuje czytania wiadomości.

Kiedy używać [SDK Bus usługi Azure dla środowiska .NET] [ lnk-servicebus-sdk] lub [Zdarzenie koncentratory - hosta procesor zdarzenia][lnk-eventprocessorhost], za pomocą dowolne ciągi połączenia Centrum IoT z odpowiednimi uprawnieniami. Następnie użyć nazwy zdarzenia Centrum **wiadomości i zdarzeń** .

Kiedy używać SDK (lub integracji produktu) znają z Centrum IoT, należy pobrać zgodnego z programem zdarzenia Centrum punktu końcowego i nazwę zgodnego z programem zdarzenia Centrum od ustawień Centrum IoT w [Azure portal][lnk-management-portal]:

1. Karta Centrum IoT kliknij **wiadomości**.
2. W sekcji **Ustawienia urządzenia w chmurze** , możesz znaleźć następujące wartości: **punkt końcowy zgodnego z programem zdarzenia Centrum**, **nazwę zgodnego Centrum zdarzeń**i **partycje**.

    ![Ustawienia urządzenia w chmurze][img-eventhubcompatible]

> [AZURE.NOTE] Jeśli zestawu SDK wymaga wartość **Nazwa hosta** lub **Namespace** , usunąć z schemat punkt **końcowy zgodnego Centrum zdarzenia**. Jeśli na przykład punkt końcowy zgodnego Centrum zdarzenie jest **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **Nazwa hosta** będzie **iothub SN myiothub-1234.servicebus.windows.net**i **Namespace** będzie **iothub SN myiothub-1234**.

Następnie można użyć dowolnej zasad zabezpieczeń udostępniania, które z uprawnieniami **ServiceConnect** nawiązywania połączenia z określoną Centrum zdarzenia.

Jeśli potrzebne do utworzenia Centrum zdarzeń parametry połączenia przy użyciu poprzednich informacji, użyj następującego wzorca:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Oto lista SDK i integracji, których można używać z punktów końcowych zgodnego Centrum zdarzenia, które udostępnia Centrum IoT:

* [Klient koncentratory zdarzenia języka Java](https://github.com/hdinsight/eventhubs-client)
* [Spout Burza Apache](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). [Spout źródła](https://github.com/apache/storm/tree/master/external/storm-eventhubs) można wyświetlać na GitHub.
* [Integracja Apache Spark](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji na temat wymianę wiadomości z Centrum IoT.

## <a name="message-format"></a>Format wiadomości

Centrum IoT wiadomości obejmują:

* Ustawianie właściwości *systemu*. Właściwości, które Centrum IoT zinterpretowany lub ustawia. Wartość jest wstępnie.
* Zestaw *Właściwości aplikacji*. Słownik właściwości ciągu, definiujące aplikacji i programu access, bez konieczności deserializacji treści wiadomości. Centrum IoT nigdy nie modyfikuje tych właściwości.
* Nieprzezroczysty binarne treści.

Aby uzyskać więcej informacji na temat sposobu wiadomość jest kodowane w różnych protokołów zobacz [interfejsy API Centrum IoT i SDK][lnk-sdks].

W poniższej tabeli wymieniono zestaw właściwości systemu w Centrum IoT wiadomości.

| Właściwość | Opis |
| -------- | ----------- |
| Identyfikator komunikatu | Identyfikator użytkownika można ustawić wiadomości, używany dla wzorców żądanie odpowiedź. Format: Uwzględniania wielkości liter ciągu (maksymalnie 128 znaków) znaków alfanumerycznych ASCII 7-bitową + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Numer sekwencji | Liczba (unikatowych na urządzeniu kolejkę) przypisane przez Centrum IoT do każdej wiadomości w chmurze do urządzenia. |
| Aby | Miejsce docelowe określonego w [Chmurze do urządzenia] [ lnk-c2d] wiadomości. |
| ExpiryTimeUtc | Data i godzina ważności wiadomości. |
| EnqueuedTime | Data i godzina otrzymania wiadomości przez Centrum IoT. |
| CorrelationId | Właściwość ciąg wiadomość odpowiedź, która zawiera zwykle komunikatu żądania, wzorców żądanie odpowiedź. |
| Nazwa użytkownika | Identyfikator służy do określania pochodzenia wiadomości. Gdy wiadomości są generowane przez Centrum IoT, jest ustawiona na `{iot hub name}`. |
| ACK | Generator wiadomości opinii. Ta właściwość jest używana w chmurze do urządzenia wiadomości do żądania Centrum IoT wygenerować wiadomości opinii w związku ze wiadomości przez urządzenie. Możliwe wartości: **Brak** (ustawienie domyślne): wiadomość nie został wygenerowany, **dodatnia**: wyświetlony komunikat opinii, jeśli wiadomość została ukończona, **ujemne**: komunikat opinii wygasł (lub dostarczenia maksymalna liczba osiągnięto) bez przez urządzenia, lub **pełne**: ujemne i dodatnie. Aby uzyskać więcej informacji, zobacz [opinie wiadomości][lnk-feedback]. |
| ConnectionDeviceId | Identyfikator określonych przez Centrum IoT wiadomości urządzenia w chmurze. Zawiera **identyfikator urządzenia** urządzenia, która przysłała wiadomość. |
| ConnectionDeviceGenerationId | Identyfikator określonych przez Centrum IoT wiadomości urządzenia w chmurze. Zawiera **generationId** (zgodnie z [Właściwości tożsamości urządzenia][lnk-device-properties]) urządzenia, która przysłała wiadomość. |
| ConnectionAuthMethod | Metody uwierzytelniania określonych przez Centrum IoT wiadomości urządzenia w chmurze. Ta właściwość zawiera informacje dotyczące metody uwierzytelniania używane do uwierzytelnienia urządzenia wysłania wiadomości. Aby uzyskać więcej informacji, zobacz [urządzenia do chmury fałszowaniu anti][lnk-antispoofing].|

## <a name="communication-protocols"></a>Protokoły komunikacji

Centrum IoT zapewnia urządzenia, aby użyć [MQTT][lnk-mqtt], MQTT nad WebSockets, [AMQP][lnk-amqp], AMQP WebSockets i HTTP protokoły komunikacji stronie urządzenia. Poniższa tabela zawiera wysokiego poziomu zalecenia dotyczące wyboru Protocol (protokół):

| Protocol (protokół) | Kiedy należy wybrać protokołu |
| -------- | ------------------------------------ |
| MQTT <br> MQTT na WebSocket     | Użyj na wszystkich urządzeniach, które nie wymagają łączenie wielu urządzeń (każda z własną poświadczenia na urządzenie) za pomocą tego samego połączenia TLS. |
| AMQP <br> AMQP na WebSocket    | Użyj na pola i chmura bram skorzystać z połączenia Multipleksowanie na urządzeniach. |
| HTTP    | Zastosowanie w przypadku urządzeń, które nie obsługują innych protokołów. |

Po wybraniu z protokołu komunikacji urządzenia po stronie, rozważ następujące wskazówki:

* **Wzór chmury do urządzenia**. HTTP nie ma skuteczny sposób wykonania wypychanych serwera. Jako takie korzystając z protokołu HTTP, urządzenia ankieta IoT Centrum wiadomości w chmurze do urządzenia. Ta metoda jest nieefektywne urządzenia i Centrum IoT. W obszarze bieżący wytycznych HTTP każde urządzenie należy skorzystać dla wiadomości, co 25 minut lub więcej. Z drugiej strony MQTT AMQP pomocy technicznej i wypychanych serwera po odebraniu wiadomości w chmurze do urządzenia. Umożliwiają natychmiastowe umieszcza wiadomości z Centrum IoT na urządzeniu. Jeśli opóźnienie dostarczenia ma znaczenie, MQTT lub AMQP są protokoły używane. W przypadku urządzeń rzadko połączonego HTTP działa także.
* **Pole bramy**. Podczas korzystania z MQTT i HTTP, nie można połączyć wielu urządzeń (każda z własną poświadczenia na urządzenie) za pomocą tego samego połączenia TLS. W związku z tym dla [pola bramy scenariuszy][lnk-azure-gateway-guidance], te protokoły są utratę jakości, ponieważ wymagają jedną połączenia TLS między bramy pola i Centrum IoT dla każdego urządzenia połączenie do bramy pola.
* **Urządzenia zasobów**. Biblioteki MQTT i HTTP mają mniejsze rozmiary niż bibliotek AMQP. Jako takie Jeśli urządzenie jest ograniczona zasobów (na przykład mniejszą niż 1 MB pamięci RAM), te protokoły może być tylko implementacji protokołu dostępne.
* **Przechodzenie sieci**. Standardowy protokół AMQP korzysta z portu 5671, gdy wykrywa MQTT na porcie 8883, co może powodować problemy w sieci, które są zamknięte do protokołów innych niż HTTP. MQTT nad WebSockets, AMQP WebSockets i HTTP są dostępne do użycia w tym scenariuszu.
* **Rozmiar ładunku**. MQTT i AMQP są binarne protokołów, które powodują bardziej zwarty ładunki niż HTTP.

> [AZURE.NOTE] Przy użyciu protokołu HTTP, każde urządzenie należy skorzystać w chmurze do urządzenia wiadomościach, co 25 minut lub więcej. Jednak podczas opracowywania, jest możliwość ankieta częściej niż 25 minut.

## <a name="port-numbers"></a>Numery portów

Urządzenia można komunikować się z Centrum IoT platformy Azure za pomocą różnych protokołów. Zazwyczaj wybór protokół wynika specyficznych wymagań rozwiązanie. W poniższej tabeli wymieniono porty wyjściowe, które muszą być otwarte dla urządzenia, aby można było korzystać z określonym protokołem:

| Protocol (protokół) | Porty |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT na WebSockets | 443    |
| AMQP     | 5671    |
| AMQP na WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (MDM) | 5684 |

Po utworzeniu koncentratora IoT w regionie Azure koncentratora przechowuje tego samego adresu IP ważności koncentratora. Jednak aby zachować jakości usług, jeśli Microsoft przenosi Centrum IoT jednostce inną skalę go przypisywana jest adres IP.

## <a name="notes-on-mqtt-support"></a>Uwagi dotyczące obsługi MQTT

Centrum IoT wykonuje protokołu v3.1.1 MQTT następujące ograniczenia i szczególne zachowanie:

  * **QoS 2 nie jest obsługiwane**. Gdy klient urządzenia opublikuje wiadomość z **QoS 2**, Centrum IoT zamyka połączenia sieciowego. Gdy klient urządzenia subskrybuje tematu z **QoS 2**, Centrum IoT udziela maksymalna QoS poziomu 1 w pakiecie **SUBACK** .
  * **Przechowuj wiadomości nie są zachowywane**. Jeśli klienta urządzenia opublikuje wiadomość flagą PRZECHOWUJ równa 1, dodaje Centrum IoT **x i zachowują** właściwości aplikacji do wiadomości. W tym przypadku Centrum IoT nie zmieniają Przechowuj wiadomości, ale zamiast tego przekazuje je do aplikacji wewnętrznej.

Aby uzyskać więcej informacji, zobacz [IoT MQTT Centrum pomocy technicznej][lnk-devguide-mqtt].

Jako wersję ostateczną uwagę, należy przejrzeć [bramy protokołu Azure IoT] [ lnk-azure-protocol-gateway] umożliwiająca wdrażanie protokołu niestandardowego wysokiej wydajności bramy, która bezpośrednio z Centrum IoT. Brama protokołu Azure IoT umożliwia dostosowywanie protokołu urządzenia, aby zezwalały na wdrożeń MQTT brownfield lub inne protokoły niestandardowe. Ta metoda wymaga jednak uruchomieniu i działania bramy protokołu niestandardowego.

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] listy język różne SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT koncentratora MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu wysyłać i odbierać wiadomości przy użyciu Centrum IoT, może Cię w następujących tematach przewodnik dewelopera:

- [Przekazywanie plików z urządzenia][lnk-devguide-upload]
- [Zarządzanie tożsamościami urządzenia w Centrum IoT][lnk-devguide-identities]
- [Kontrolowanie dostępu do Centrum IoT][lnk-devguide-security]
- [Synchronizowanie stanu i konfiguracji za pomocą urządzenia twins][lnk-devguide-device-twins]
- [Wywoływanie metody bezpośrednio na urządzeniu][lnk-devguide-directmethods]
- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Jeśli chcesz wypróbować niektóre pojęcia opisane w tym artykule, może być zainteresować następujące samouczki Centrum IoT:

- [Wprowadzenie do Centrum IoT Azure][lnk-getstarted-tutorial]
- [Jak można wysyłać chmury do urządzenia z Centrum IoT][lnk-c2d-tutorial]
- [Sposób przetwarzania Centrum IoT urządzenia w chmurze wiadomości][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
