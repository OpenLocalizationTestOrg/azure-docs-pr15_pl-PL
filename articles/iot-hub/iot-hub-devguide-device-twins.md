<properties
 pageTitle="Deweloper Przewodnik po — opis urządzenia twins | Microsoft Azure"
 description="Przewodnik Azure Deweloper Centrum IoT - Użyj urządzenia twins do synchronizowania danych stanu i konfiguracji między centrum IoT i urządzeniach"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Opis twins urządzenia — wyświetlanie podglądu

## <a name="overview"></a>Omówienie

*Twins urządzenia* są dokumenty JSON, których są przechowywane informacje o stanie urządzenia (metadane, konfiguracji i warunków). Centrum IoT będzie nadal występował podwójnego urządzenia, dla każdego urządzenia, które jest podłączone do koncentratora IoT. Będą opisane w tym artykule:

* Struktura podwójnego urządzenia: *znaczniki*, *odpowiednie* i *zgłoszone właściwości*, a
* działania, które aplikacje urządzenie i ponownie zostanie zakończone, można wykonywać na twins urządzenia.

> [AZURE.NOTE] W tej chwili twins urządzenia są dostępne tylko z urządzeń łączących się Centrum IoT przy użyciu protokołu MQTT. Zajrzyj do [pomocy technicznej MQTT] [ lnk-devguide-mqtt] artykuł, aby uzyskać instrukcje dotyczące jak przekonwertować istniejącej aplikacji urządzenia za pomocą MQTT.

### <a name="when-to-use"></a>Kiedy należy używać

Za pomocą urządzenia twins do:

* Zawierają dane meta dotyczące urządzenia w chmurze, np. wdrożenia lokalizacji Automat.
* Raport bieżące informacje o stanie takich jak możliwości i warunki z Twojej aplikacji urządzenia, np. urządzenie nawiązywania połączenia przez sieć komórkowa lub sieci Wi-Fi.
* Synchronizowanie stanu przepływów pracy długim między urządzenia aplikacji i z powrotem celu np. wewnętrznej, określając nową wersję oprogramowania układowego do zainstalowania i aplikacja urządzenia raportowania etapach procesu aktualizacji.
* Kwerendy metadane urządzenia, konfiguracji lub stan.

Za pomocą [urządzenia w chmurze wiadomości] [ lnk-d2c] sekwencji oznaczony znacznikiem czasowym zdarzenia, na przykład szeregu czasowego czujnik danych lub alarmów. Użyj [metody chmury do urządzenia] [ lnk-methods] dla kontrolkę interakcyjną urządzeń, takich jak włączenie wentylatora.

## <a name="device-twins"></a>Twins urządzenia

Twins urządzenia przechowywania informacji związanych z urządzenia które:

- Kończy się urządzenia i z powrotem umożliwia synchronizowanie warunków urządzenia i konfiguracji.
- Wewnętrzna aplikacji służy do kwerendy i docelowej długotrwałych operacji.

Do zarządzania cyklem życia podwójnego urządzenie jest połączone z odpowiedniego [urządzenia tożsamości][lnk-identity]. Twins niejawnie do tworzenia i usunięte podczas tworzenia lub usunięte w Centrum IoT nowej tożsamości urządzenia.

Podwójnego urządzenie jest dokument JSON, który zawiera:

* **Znaczniki**. Dokument JSON i odczytywana przez wewnętrzna. Znaczniki nie są widoczne dla aplikacji urządzenia.
* **Żądany właściwości**. Używana w połączeniu z właściwościami zgłoszoną do synchronizowania konfiguracji urządzenia lub warunku. Żądane właściwości można ustawić tylko przez aplikację ponownie zakończenia i mogą być odczytywane przez aplikację urządzenia. Aplikacja urządzenia również może otrzymywać powiadomienia w czasie rzeczywistym zmian na odpowiednie właściwości.
* **Właściwości zgłoszono**. Używana w połączeniu z odpowiednimi właściwościami do synchronizowania konfiguracji urządzenia lub warunku. Zgłoszoną właściwości można ustawić tylko za pomocą aplikacji urządzenia i może być odczytywana i kwerendy do końca aplikacji Wstecz.

Ponadto na poziomie głównym podwójnego urządzenia zawiera właściwości tylko do odczytu z odpowiednich tożsamości, co w [rejestrze tożsamości urządzenia][lnk-identity].

![][img-twin]

Oto przykład dokumentu JSON podwójnego urządzenia:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

W oknie dialogowym obiektu głównego są właściwości systemu, a obiekty kontenera dla `tags` i oba `reported` i `desired properties`. `properties` Kontener zawiera niektóre elementy tylko do odczytu (`$metadata`, `$etag`, i `$version`) są opisaną odpowiednio w [metadanych podwójnego] [ lnk-twin-metadata] i [Optymistyczny współbieżności] [ lnk-concurrency] sekcji.

### <a name="reported-property-example"></a>Przykład zgłoszoną właściwości

W powyższym przykładzie zawiera podwójnego urządzenia `batteryLevel` właściwość raportowana za pomocą aplikacji urządzenia. Ta właściwość umożliwia kwerendy, a następnie działać na urządzeniach według ostatniego poziomu zgłoszoną baterii. Inny przykład woli możliwości urządzenia urządzenia aplikacji raportu lub opcje połączeń.

Uwaga właściwości jak zgłoszoną uprościć scenariusze, w którym wewnętrzna jest zainteresowania w ostatniej znanej wartości właściwości. Za pomocą [urządzenia w chmurze wiadomości] [ lnk-d2c] czy wewnętrzna mają być przetwarzane telemetrycznego urządzenia w postaci sekwencji oznaczony znacznikiem czasowym zdarzeniami, takimi jak szeregu czasowego.

### <a name="desired-configuration-example"></a>Przykład konfiguracja docelowa

W powyższym przykładzie `telemetryConfig` odpowiedniej i podać właściwości są używane przez aplikację ponownie zakończenia i urządzenia zsynchronizować konfigurację telemetrycznego w przypadku tego urządzenia. Na przykład:

1. Aplikacja wewnętrzna ustawia żądaną właściwość z wartością Konfiguracja docelowa. Poniżej przedstawiono część dokumentu z żądaną właściwość:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Aplikacja urządzenie jest powiadamiany o zmiany od razu, jeśli jest to połączenie lub na pierwszym ponownym. Aplikacja urządzenia następnie raportów zaktualizowaną konfigurację (lub warunku błędu przy użyciu `status` właściwości). Poniżej przedstawiono część zgłoszoną właściwości:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. Wewnętrzna aplikacji może pozostać śledzenie wyników operacji konfiguracji na wielu urządzeniach przez [Badanie] [ lnk-query] twins.

> [AZURE.NOTE] Powyższe wstawki znajdują się przykłady, zoptymalizowana pod kątem czytelność możliwy sposób do kodowania konfiguracji urządzenia, a jego stan. Centrum IoT nie nakłada określonych schemat odpowiedniej i podać właściwości twins urządzenia.

W większości przypadków twins są używane do synchronizacji długotrwałych operacji, takich jak aktualizacje oprogramowania układowego. Odwołują się do [odpowiedniej właściwości, aby skonfigurować urządzenia za pomocą] [ lnk-twin-properties] uzyskać więcej informacji na temat korzystania z właściwości do synchronizacji i śledzić czas wykonywania długotrwałych operacji na urządzeniach.

## <a name="back-end-operations"></a>Operacje wewnętrznej

Wewnętrzna działa na podwójnego za pomocą następujących czynności Atomowej, dostępne za pośrednictwem protokołu HTTP:

1. **Pobieranie podwójnego według identyfikatorów**. Operacja zwraca podać zawartość podwójnego dokument, w tym znaczników i potrzeby i właściwości systemu.
2. **Częściowo aktualizowanie podwójnego**. Operacja umożliwia wewnętrznej częściowo zaktualizować podwójnego znaczników i odpowiednie właściwości. Aktualizacja częściowa jest wyrażony jako dokument JSON dodaje lub aktualizuje dowolnej właściwości wymienionych. Ustawić właściwości `null` zostaną usunięte. Na przykład poniższy tworzy nową właściwość odpowiedniej z wartością `{"newProperty": "newValue"}`, zastępuje istniejącą wartość z `existingProperty` z `"otherNewValue"`i całkowicie usuwa `otherOldProperty`. Bez zmian wystąpić do innych istniejących odpowiednie właściwości lub znaczników:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Zamienianie odpowiednie właściwości**. Operacja umożliwia wewnętrznej całkowicie zastąpić wszystkich istniejących odpowiednie właściwości i zastąpić nowy dokument JSON dla `properties/desired`.
4. **Zamienianie znaczników**. Analogously Aby zamienić odpowiednie właściwości, tej operacji umożliwia wewnętrznej całkowicie zastąpić wszystkich znaczników i zastąpić nowy dokument JSON dla `tags`.

Wszystkie operacje powyżej obsługuje [Optymistyczny współbieżności] [ lnk-concurrency] i wymaga uprawnień **ServiceConnect** zgodnie z definicją w [zabezpieczeniach] [ lnk-security] artykuł.

Oprócz tych operacji wewnętrzna mogą wysyłać kwerendy twins przy użyciu przypominających SQL [kwerendy języka][lnk-query]oraz wykonywania operacji dotyczących dużych zestawów twins przy użyciu [zadań][lnk-jobs].

## <a name="device-operations"></a>Operacje urządzenia

Aplikacja urządzenie działa na podwójnego za pomocą Atomowej następujące operacje:

1. **Pobieranie podwójnego**. Operacja zwraca zawartość dokumentu podwójnego (dodanie tagów i potrzeby, zgłoszone i właściwości) dla aktualnie połączona urządzenia.
2. **Częściowo aktualizacji zgłoszone właściwości**. Operacja umożliwia aktualizacja częściowa zgłoszoną właściwości aktualnie połączona urządzenia. To używa tego samego formatu aktualizacji JSON jako wewnętrznej przeciwległych częściowej aktualizacji odpowiednie właściwości.
3. **Observe żądane właściwości**. Obecnie urządzeniami mogą być powiadamiane o aktualizacjach odpowiednie właściwości zaraz po ich wprowadzeniu. Urządzenie, otrzymuje wykonywane przez wewnętrzna tym samym formularzu aktualizacji (wymiana pełne lub częściowe).

Wszystkie operacje powyżej Wymagaj uprawnień **DeviceConnect** zgodnie z definicją w [zabezpieczeniach] [ lnk-security] artykuł.

[Urządzenie Azure IoT SDK] [ lnk-sdks] ułatwiają używanie wyżej czynności w wielu językach i platformach. Więcej informacji na temat szczegółów pierwotnych IoT Centrum synchronizacji odpowiednie właściwości można znaleźć w [przepływie ponowne urządzenia][lnk-reconnection].

> [AZURE.NOTE] Obecnie twins urządzenia są dostępne tylko z urządzeń łączących się Centrum IoT przy użyciu protokołu MQTT.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji na temat kontrolowanie dostępu do Twojej Centrum IoT.

## <a name="tags-and-properties-format"></a>Formatowanie znaczników i właściwości

Znaczniki, odpowiednie i zgłoszoną właściwości są JSON obiektów z następującym ograniczeniom:

* Klawisze w obiektach JSON są ciągów znaków UNICODE 128 znaków uwzględniana wielkość liter. Niedozwolone znaki wykluczyć znaki kontrolne UNICODE (segmenty C0 i C1) i `'.'`, `' '`, i `'$'`.
* Wszystkie wartości w obiekcie JSON mogą być następujące typy JSON: boolean, liczba, ciąg, obiekt. Tablice są niedozwolone.

## <a name="twin-size"></a>Rozmiar podwójnego

Centrum IoT wymusza ograniczenie rozmiaru 8KB na wartości `tags`, `properties/desired`, i `properties/reported`, z wyłączeniem elementy tylko do odczytu.
Rozmiar jest obliczana przez zliczania wszystkie znaki, z wyłączeniem UNICODE kontrolować znaków (segmenty C0 i C1) oraz miejsce `' '` po wyświetleniu poza stałej w postaci ciągu.
Centrum IoT odrzuci z powodu błędu wszystkie operacje, które chcesz zwiększyć rozmiar tych dokumentów powyżej limitu.

## <a name="twin-metadata"></a>Podwójnego metadanych

Centrum IoT zachowuje sygnatura czasowa ostatniej aktualizacji dla każdego obiektu JSON w odpowiedniej i podać właściwości. Sygnatury czasowe są w czasie UTC i zakodowany w formacie [ISO8601] `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Na przykład:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Informacje te są przechowywane na każdym poziomie (a nie tylko liście struktury JSON) w celu zachowania aktualizacje, które klucze obiektu.

## <a name="optimistic-concurrency"></a>Optymistyczny współbieżności

Znaczniki, odpowiednie i zgłoszoną właściwości wszystkich obsługuje optymistyczny współbieżności.
Znaczniki mają etag zgodnie [RFC7232], który reprezentacją JSON znacznika. Umożliwia to w operacji aktualizowania warunkowe z wewnętrzna zapewnienia spójności.

Właściwości odpowiedniej i zgłoszoną nie ma etags, ale `$version` wartości, która jest gwarantowana przyrostowe. Analogously do etag wersja może być używana przez stronę aktualizacji, takich jak (aplikacja urządzenia zgłoszoną właściwości) lub wewnętrzna odpowiednie właściwości wymuszenie spójności aktualizacje.

Wersje również są przydatne, gdy observing agenta (na przykład, aplikacja urządzenia, obserwowania odpowiednie właściwości) zawiera uzgodnić szczepy między wynik operacji pobierania i powiadomienie o aktualizacji. Sekcja [przepływu ponowne urządzenia] [ lnk-reconnection] znajdziesz więcej informacji.

## <a name="device-reconnection-flow"></a>Przepływ ponowne urządzenia

Centrum IoT powiadomień o aktualizacjach odpowiednie właściwości dla urządzeń Rozłączono nie zostaną zachowane. Wynika, że urządzenie, które łączy musi pobrać pełny odpowiednie właściwości dokumentu, oprócz subskrypcji powiadomień o aktualizacjach. Możliwość szczepy między powiadomień o aktualizacjach i pełne pobieranie, należy zapewnić przepływ następujące:

1. Aplikacja urządzenia łączy do koncentratora IoT.
2. Aplikacja urządzenia subskrybowane przez odpowiednie właściwości powiadomień o aktualizacjach.
3. Aplikacja urządzenia pobiera cały dokument żądane właściwości.

Aplikacja urządzenia można zignorować wszystkie powiadomienia z `$version` mniejszych lub równych niż wersja cały dokument pobrane. Jest to możliwe, ponieważ Centrum IoT gwarantuje wersje zawsze zwiększyć.

> [AZURE.NOTE] Tę logikę już jest zaimplementowana w [urządzeniu Azure IoT SDK][lnk-sdks]. Opis ten jest przydatna tylko wtedy, gdy aplikacja urządzenia nie można używać urządzenia Azure IoT SDK i musi program interfejsu MQTT bezpośrednio.

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] wyświetla różne języka SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT Centrum MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Teraz znasz już o twins urządzenia, może Cię zainteresować następujące tematy przewodnik dewelopera:

- [Wywoływanie metody bezpośrednio na urządzeniu][lnk-methods]
- [Planowanie zadań na wielu urządzeniach][lnk-jobs]

Jeśli chcesz wypróbować niektóre pojęcia opisane w tym artykule, może być zainteresować następujące samouczki Centrum IoT:

- [Jak używać podwójnego urządzenia][lnk-twin-tutorial]
- [Jak używać właściwości podwójnego][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png