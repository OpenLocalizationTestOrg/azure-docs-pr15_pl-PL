<properties
 pageTitle="Przewodnik dewelopera — słownik | Microsoft Azure"
 description="Glosariusz terminów typowych odnoszących się do Centrum IoT"
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

# <a name="glossary-of-iot-hub-terms"></a>Glosariusz terminów Centrum IoT

W tym artykule opisano niektóre typowe warunki skojarzone z Centrum IoT.

## <a name="advanced-message-queueing-protocol-amqp"></a>Protokół Kolejkowanie wiadomości Zaawansowane (AMQP)

[AMQP](https://www.amqp.org/) jest jednym z wiadomości protokołów koncentratora IoT obsługuje komunikowania się z urządzeniami. Aby uzyskać więcej informacji na temat obsługiwanych przez Centrum IoT protokoły zobacz [Wysyłanie i odbieranie wiadomości z Centrum IoT](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Chmura do urządzenia (C2D)

Zazwyczaj używany w odniesieniu do wiadomości wysyłanych z Centrum IoT do połączonego urządzenia. Często te wiadomości są polecenia, które poinstruuj urządzenie, aby wykonać akcję. Aby uzyskać więcej informacji zobacz [Wysyłanie i odbieranie wiadomości z Centrum IoT](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Warunek

Odwołuje się do informacje o stanie urządzenia, takie jak metoda łączności obecnie w użyciu, zgłoszone przez aplikacji dla urządzeń. Urządzenia również zgłosić ich możliwości. Kwerendy można warunek i możliwości przy użyciu podwójnego urządzenia.

## <a name="data-point-message"></a>Wiadomość punktu danych

Wiadomość punktu danych jest wiadomość chmury do urządzenia, która zawiera dane telemetryczne na przykład prędkość wiatru lub temperatury.

## <a name="desired-properties"></a>Odpowiednie właściwości

W kontekście twins urządzenia odpowiednie właściwości są używane w połączeniu z *zgłoszone właściwości* do synchronizowania konfiguracji urządzenia lub warunku. Żądane właściwości można ustawić tylko przez aplikację ponownie zakończenia i obserwuje się za pomocą aplikacji urządzenia. 

## <a name="device-to-cloud-d2c"></a>Urządzenia w chmurze (D2C)

Zazwyczaj używany w odniesieniu do wiadomości wysyłanych z urządzeniami do Centrum IoT. Te wiadomości mogą być [punkt danych](#data-point-message) lub [interakcyjnych](#interactive-message) wiadomości. Aby uzyskać więcej informacji zobacz [Wysyłanie i odbieranie wiadomości z Centrum IoT](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Urządzenie tożsamości rejestru

[Urządzenia tożsamości rejestru](iot-hub-devguide-identity-registry.md) jest wbudowany część koncentratora IoT, który przechowuje informacje dotyczące poszczególnych urządzeń może łączyć się koncentratora.

## <a name="device"></a>Urządzenie

W kontekście IoT urządzenie jest zazwyczaj drobnych, autonomicznej urządzeniem może zbierać dane lub innego urządzenia. Na przykład urządzenie może być urządzenie monitorowania środowiska lub kontroler systemów pojenia i wentylacji w cieplarnianych.

## <a name="device-twin"></a>Podwójnego urządzenia

[Podwójnego urządzenie](iot-hub-devguide-device-twins.md) jest kopią w Centrum IoT warunek i konfiguracji ustawień urządzenia fizycznego. Podwójnego urządzenia służy do zarządzania konfiguracją urządzenia fizycznego.

## <a name="direct-method"></a>Metoda bezpośrednia

[Metoda bezpośrednia](iot-hub-devguide-direct-methods.md) jest umożliwia wykonywanie na urządzeniu przez interfejs API na Twoim Centrum IoT umożliwiające wyzwalanie metody.

## <a name="event-hub-compatible-endpoint"></a>Punkt końcowy zgodny z Centrum zdarzenia

Aby odczytać urządzenia w chmurze wiadomości wysłane do użytkownika Centrum IoT, można połączyć z punktu końcowego na Twoim Centrum i odczytać tych wiadomości za pomocą dowolnej metody zgodnego z programem zdarzenia Centrum. Metody zgodny z Centrum zdarzeń użyć zdarzenia SDK koncentratory i analizy strumieniu Azure.

## <a name="field-gateway"></a>Pole bramy

Brama pól umożliwia łączności dla urządzeń, które nie można połączyć się bezpośrednio do koncentratora IoT i jest zazwyczaj używany lokalnie z urządzenia. Aby uzyskać więcej informacji, zobacz [Co to jest Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Zadanie

Do wewnętrznej rozwiązanie umożliwia zadania planowanie i śledzenie działań zestawu urządzeń zarejestrowanych w Twoim Centrum IoT. Działania obejmują aktualizowania właściwości podwójnego potrzeby urządzenia, aktualizowanie znaczniki podwójnego urządzenia i wywoływanie metody bezpośredniego.

## <a name="protocol-gateway"></a>Brama Protocol (protokół)

Brama Protocol (protokół) jest zazwyczaj używany w chmurze, a także protokół tłumaczeń urządzenia podłączone do koncentratora IoT. Aby uzyskać więcej informacji, zobacz [Co to jest Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interakcyjne wiadomości

Interakcyjne komunikat jest chmury do urządzenia wyzwalające natychmiastowego działania w wewnętrznej aplikacji. Na przykład urządzenie może wysłać alarmu o błędzie, które powinny być automatycznie rejestrowane w systemie CRM.

## <a name="iot-hub"></a>Centrum IoT

Centrum IoT jest w pełni zarządzane usługa Azure, umożliwiająca dwukierunkowy i niezawodności komunikacji między miliony IoT urządzeń i wewnętrznej rozwiązanie. Aby uzyskać więcej informacji, zobacz [Co to jest Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>Pakiet IoT

Azure pakietu IoT pakiety razem wielu usługi Azure przy użyciu wstępnie rozwiązań. Te rozwiązania wstępnie pozwalają szybko Rozpocznij pracę z końca do końca implementacji typowe scenariusze IoT. Aby uzyskać więcej informacji, zobacz [Co to jest pakiet IoT Azure?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Zadanie

[Zadania](iot-hub-devguide-jobs.md) w Centrum IoT umożliwia wykonywanie operacji, takich jak sprzętowego uaktualnienia na wielu urządzeniach podłączone do koncentratora usługi.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) jest jednym z wiadomości protokołów Centrum IoT obsługuje komunikowania się z urządzeniami. Aby uzyskać więcej informacji na temat obsługiwanych przez Centrum IoT protokoły zobacz [Wysyłanie i odbieranie wiadomości z Centrum IoT](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Właściwości zgłoszoną

W kontekście twins urządzenia zgłoszoną właściwości są używane w połączeniu z *odpowiednimi właściwościami* do synchronizowania konfiguracji urządzenia lub warunku. Zgłoszoną właściwości można ustawić tylko za pomocą aplikacji urządzenia i może być odczytywana i kwerendy do końca aplikacji Wstecz.

## <a name="tags"></a>Znaczniki

W kontekście devcie twins znaczniki są urządzenia metadane przechowywane i pobrane przez aplikację wewnętrznej w postaci dokumentu JSON. Znaczniki nie są widoczne dla aplikacji na urządzeniu.

## <a name="system-properties"></a>Właściwości systemu

W kontekście twins urządzenia właściwości systemu są tylko do odczytu i zawiera informacje dotyczące użycie urządzenia, takie jak ostatni stan połączenia i czasu aktywności.