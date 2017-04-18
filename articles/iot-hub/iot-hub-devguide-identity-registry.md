<properties
 pageTitle="Przewodnik dewelopera — urządzenia tożsamości rejestru | Microsoft Azure"
 description="Azure IoT Centrum deweloperów guide - Opis rejestru tożsamości urządzenia i jak go używać do zarządzania urządzeniami"
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

# <a name="manage-device-identities-in-iot-hub"></a>Zarządzanie tożsamościami urządzenia w Centrum IoT

## <a name="overview"></a>Omówienie

Co IoT jest rejestru tożsamości urządzenia, które są przechowywane informacje o urządzeniach, które mogą być nawiązywanie połączenia z poziomu Centrum. Zanim urządzenia można połączyć do koncentratora, musi to być wpis dla danego urządzenia w rejestrze tożsamości urządzenia koncentratora. Urządzenie musi również uwierzytelnianie z Centrum na podstawie poświadczeń przechowywanych w rejestrze tożsamości urządzenia.

Na wysokim poziomie rejestru tożsamości urządzenia to zbiór pozostałych dostępem do zasobów tożsamości urządzenia. Po dodaniu wpisu w rejestrze koncentratora IoT tworzy zestaw zasobów na urządzenie w usłudze, takich jak kolejki, która zawiera lotu komunikatów w chmurze do urządzenia.

### <a name="when-to-use"></a>Kiedy należy używać

Jeśli jest potrzebna do zapewniania obsługi urządzeń, które nawiązywanie Centrum IoT i potrzebne do kontrolowania dostępu na urządzenie do punktów końcowych urządzenia skierowaną w Twoim Centrum za pomocą rejestru tożsamości urządzenia.

> [AZURE.NOTE] Rejestr tożsamości urządzenia nie zawiera wszystkie metadane specyficzne dla aplikacji.

## <a name="device-identity-registry-operations"></a>Operacje rejestru tożsamości urządzenia

Centrum IoT urządzenia tożsamości rejestru udostępnia następujące operacje:

* Tworzenie urządzenia tożsamości
* Aktualizowanie urządzenia tożsamości
* Pobieranie urządzenia tożsamości według identyfikatorów
* Usuwanie urządzenia tożsamości
* Lista do 1000 tożsamości
* Eksportowanie wszystkich tożsamości z magazynem obiektów blob platformy Azure
* Importowanie tożsamości z magazynem obiektów blob Azure

Te operacje można użyć optymistyczny współbieżności jako określonych w [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Jedynym sposobem na wszystkich tożsamości w rejestrze tożsamości Centrum pobierania jest użycie [Eksportowanie] [ lnk-export] funkcji.

Rejestr koncentratora IoT tożsamości urządzenia:

- Nie zawiera wszystkie metadane aplikacji.
- Możliwy takich jak słownik, przy użyciu **identyfikator urządzenia** jako klucz.
- Nie obsługuje wyrazisty kwerend.

Rozwiązanie IoT zwykle zawiera osobne magazynu określonego rozwiązania, który zawiera metadane specyficzne dla aplikacji. Na przykład magazynu określonego rozwiązania w rozwiązaniu inteligentne konstrukcyjnych czy rekord pokoju, w którym zostanie wdrożony czujnik temperatury.

> [AZURE.IMPORTANT] Tylko za pomocą rejestru tożsamości urządzenia do zarządzania urządzeniami i inicjowania obsługi administracyjnej operacji. Operacje wysokiej wydajności w czasie wykonywania nie powinien zależeć operacji w rejestrze tożsamości urządzenia. Na przykład sprawdzanie stanu połączenia urządzenia przed wysłaniem polecenie nie jest obsługiwane deseniu. Upewnij się sprawdzić [ograniczania stawki] [ lnk-quotas] dla rejestru tożsamości urządzenia i [weryfikowania urządzenia] [ lnk-guidance-heartbeat] deseniem.

## <a name="disable-devices"></a>Wyłączanie urządzeń

Możesz wyłączyć urządzeń aktualizując właściwości **Stan** tożsamości w rejestrze. Zazwyczaj można użyć tej właściwości w dwa scenariusze:

- Podczas inicjowania obsługi administracyjnej procesu aranżacji. Aby uzyskać więcej informacji, zobacz [Udostępnianie urządzeń][lnk-guidance-provisioning].
- Jeśli dowolnego powodu rozważ urządzenie jest złamane lub stał się nieautoryzowane.

## <a name="import-and-export-device-identities"></a>Importowanie i eksportowanie tożsamości urządzenia

Możesz wyeksportować tożsamości urządzenia zbiorczo z koncentratora IoT tożsamości rejestru, przy użyciu operacji asynchronicznych [IoT Centrum zasobów dostawcy końcowy][lnk-endpoints]. Eksportowanie są długotrwałe zadań, które umożliwia zapisywanie danych tożsamości urządzenia odczytać z rejestru tożsamości kontenera obiektów blob dostarczonym przez klienta.

Możesz zaimportować tożsamości urządzenia zbiorczo do koncentratora IoT tożsamości rejestru, przy użyciu operacji asynchronicznych [IoT Centrum zasobów dostawcy końcowy][lnk-endpoints]. Importowanie plików są długotrwałe zadań, które zapisywanie danych tożsamości urządzenia do tożsamości rejestru urządzenia przy użyciu danych w kontenerze obiektów blob dostarczonym przez klienta.

- Uzyskać szczegółowe informacje na temat importowania i eksportowania interfejsów API, zobacz [Centrum IoT zasobów dostawca interfejsów API pozostałych][lnk-resource-provider-apis].
- Aby dowiedzieć się więcej o uruchamianiu importowanie i eksportowanie zadania, zobacz [Zarządzanie zbiorcze tożsamości urządzenia Centrum IoT][lnk-bulk-identity].

## <a name="device-provisioning"></a>Urządzenie inicjowania obsługi administracyjnej.

Rozwiązanie danego IoT są przechowywane dane urządzenia zależy od konkretnych wymagań rozwiązaniem. Ale co najmniej, muszą być przechowywane rozwiązanie tożsamości urządzenia i kluczy uwierzytelniania. Azure Centrum IoT zawiera rejestru tożsamości, które mogą zawierać wartości dla każdego urządzenia, takich jak identyfikatory, klucze uwierzytelniania i kody stanu. Rozwiązanie umożliwia przechowywanie danych wszelkie dodatkowe urządzenie innych Azure usług, takich jak magazyn tabel platformy Azure, magazyn obiektów blob platformy Azure lub Azure DocumentDB.

*Udostępnianie* jest proces dodawania dane początkowe urządzenia do sklepów w rozwiązaniu. Aby włączyć nowe urządzenie nawiązać połączenie z Centrum, możesz dodać nowy identyfikator urządzenia i klucze w rejestrze tożsamości IoT Centrum. W ramach procesu obsługi administracyjnej może być konieczne zainicjowania danych specyficznych dla urządzenia w innych rozwiązań.

## <a name="device-heartbeat"></a>Weryfikowania urządzenia

Rejestr tożsamości Centrum IoT zawiera pole o nazwie **connectionState**. Pole **connectionState** należy używać tylko podczas opracowywania i debugowanie IoT rozwiązań należy nie kwerend pola w czasie wykonywania (na przykład, aby sprawdzić, jeśli urządzenie jest podłączone w celu określenia, czy wysyłać wiadomość SMS lub wiadomości chmury do urządzenia).

Jeśli wymaga tego rozwiązania IoT sprawdzić, czy urządzenie jest podłączone (w czasie wykonywania, lub z dokładnością więcej niż właściwość **connectionState** ), rozwiązania wykonała *weryfikowania deseniem*.

W strukturze weryfikowania urządzenie wysyła wiadomości urządzenia w chmurze co najmniej raz każdego stała ilość czasu (na przykład co najmniej raz na godzinę). Oznacza to, że nawet jeśli urządzenia nie ma żadnych danych do wysłania, nadal wysyła pustego urządzenia w chmurze wiadomości (zazwyczaj właściwość identyfikujący go jako weryfikowania). Na stronie usługi rozwiązanie przechowuje mapy z ostatniego weryfikowania otrzymanych dla każdego urządzenia i przyjęto założenie, że występuje problem z urządzeniem, jeśli nie odbiera wiadomości weryfikowania w oczekiwany czas.

Bardziej złożone implementacji może zawierać informacje z [monitorowania operacji] [ lnk-devguide-opmon] do identyfikowania urządzenia, które są próby połączenie lub komunikacja, ale awarii. Podczas implementowania deseniu weryfikowania upewnij się, że sprawdzanie [IoT Centrum przydziałów i limity][lnk-quotas].

> [AZURE.NOTE] Jeśli rozwiązanie IoT musi stan połączenia urządzenia wyłącznie w celu ustalenia, czy do wysyłania wiadomości w chmurze do urządzenia, a wiadomości nie są emisji do dużych zestawów urządzeń, znacznie upraszcza deseń brać pod uwagę jest użycie krótki czas wygaśnięcia. Uzyskuje ten sam wynik, jak utrzymywanie urządzenia rejestru stan połączenia przy użyciu deseniu weryfikowania będąc znacznie zwiększyć jego wydajność. Istnieje także możliwość, przez żądanie potwierdzenia wiadomości, zgłaszane przez Centrum IoT jakimi urządzeniami może odbierać wiadomości i które nie są w trybie online lub są nie powiodło się.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołania dostarczać więcej informacji na temat rejestru tożsamości devcie.

## <a name="device-identity-properties"></a>Właściwości tożsamości urządzenia

Urządzenie tożsamości są przedstawiane jako JSON dokumentów za pomocą następujących właściwości.

| Właściwość | Opcje | Opis |
| -------- | ------- | ----------- |
| Identyfikator urządzenia | wymagane tylko do odczytu na aktualizacje | Uwzględniania wielkości liter ciąg znaków alfanumerycznych ASCII 7-bitową (maksymalnie 128 znaków) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | wymagane tylko do odczytu | Ciąg wygenerowane przez Centrum, uwzględniania wielkości liter więcej niż 128 znaków. Umożliwia rozróżnianie urządzenia z tym samym **identyfikator urządzenia**zostały usunięte i ponownie utworzony. |
| etag | wymagane tylko do odczytu | Ciąg reprezentujący słabych etag dla tożsamości urządzenia, zgodnie z [RFC7232][lnk-rfc7232].|
| uwierzytelnianie | opcjonalne | Obiekt złożone zawierające materiały informacji i zabezpieczeń uwierzytelniania. |
| auth.symkey | opcjonalne | Złożony obiekt zawierający podstawowy i pomocniczy klucz, przechowywane w formacie base64. |
| Stan | Wymagane | Wskaźnik programu access. Może być **włączone** lub **wyłączone**. Jeśli **włączone**urządzenie może nawiązać połączenie. Jeśli **wyłączone**, to urządzenie nie może uzyskać dostępu dowolnego urządzenia skierowaną punktu końcowego. |
| statusReason | opcjonalne | 128 znaków długości ciągu przechowującej Przyczyna stanu tożsamości urządzenia. Wszystkie znaki UTF-8 są dozwolone. |
| statusUpdateTime | tylko do odczytu | Czasowy wskaźnik pokazujący, datę i godzinę ostatniej aktualizacji stanu. |
| connectionState | tylko do odczytu | Pole wskazujące stan połączenia: **Połączono** lub **Rozłączono**. W tym polu odpowiada Widok Centrum IoT stanu połączenia urządzenia. **Ważne**: to pole powinny być używane tylko do celów rozwoju i debugowania. Stan połączenia jest aktualizowana tylko dla urządzeń za pomocą MQTT lub AMQP. Ponadto jest oparty na poziom protokołu pakietów (pakiety MQTT lub polecenia ping AMQP) i nie może mieć Maksymalne opóźnienie tylko 5 minut. Z tego powodu mogą występować wyniki fałszywie dodatnie, takie jak urządzenia zgłoszone jako połączenie, ale które są faktyczną rozłączane. |
| connectionStateUpdatedTime | tylko do odczytu | Zaktualizowano czasowy wskaźnika, przedstawiający datę i godzinę ostatniej stan połączenia. |
| lastActivityTime  | tylko do odczytu | Wskaźnik czasowy zawierające datę i godzinę ostatniej urządzenia połączone, odebrane lub wysłane wiadomości. |

> [AZURE.NOTE] Stan połączenia tylko może oznaczać Widok Centrum IoT stanu połączenia. Aktualizacje dotyczące tego stanu, w zależności od konfiguracji i parametry sieci, może być opóźniona.

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] listy język różne SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT koncentratora MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Teraz masz wiesz, jak korzystać z Centrum IoT urządzenia tożsamości rejestru, może Cię w następujących tematach przewodnik dewelopera:

- [Kontrolowanie dostępu do Centrum IoT][lnk-devguide-security]
- [Synchronizowanie stanu i konfiguracji za pomocą urządzenia twins][lnk-devguide-device-twins]
- [Wywoływanie metody bezpośrednio na urządzeniu][lnk-devguide-directmethods]
- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Jeśli chcesz wypróbować pewne pojęcia opisane w tym artykule, może być w następujących samouczek koncentratora IoT:

- [Wprowadzenie do Centrum IoT Azure][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md