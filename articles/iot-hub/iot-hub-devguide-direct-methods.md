<properties
 pageTitle="Przewodnik dewelopera — bezpośrednie metody | Microsoft Azure"
 description="Przewodnik Azure Deweloper Centrum IoT - użyj metody bezpośredni wywołania kodu na urządzeniach"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Wywoływanie metody bezpośrednio na urządzeniu (wersja preview)

## <a name="overview"></a>Omówienie

Centrum IoT daje możliwość wywołania metody na urządzeniach z chmury. Metody reprezentują interakcji żądanie odpowiedź, za pomocą urządzenia, które są podobne do połączenia HTTP, w tym powiodła się lub się nie powieść natychmiast (po zdefiniowane przez użytkownika przekroczenie limitu czasu) umożliwić użytkownikowi o stanie połączenia. Jest to przydatne w sytuacjach, gdzie kursu natychmiastowego działania różni się w zależności od tego, czy urządzenie udało się odpowiedzieć na wiadomość, takich jak wysyła telefoniczne wznowienia SMS na urządzeniu, jeśli urządzenie jest niedostępny (SMS jest bardziej drogich niż wywołania metody).

Metody można traktować jako zdalnego wywołania procedury bezpośrednio na urządzeniu. Może być wywoływana tylko metody, które zostały wykonane na urządzeniu z chmury. Chmura próbuje wywoływanie metody na urządzeniu, której nie ma tej metody zdefiniowane, wywołanie metody zakończy się niepowodzeniem.

Każda z metod urządzenie jest przeznaczony dla jednego urządzenia. [Zadania] [ lnk-devguide-jobs] umożliwia wywoływanie metody na wielu urządzeniach i kolejki wywołania metody Rozłączono urządzeń.

Każda osoba mająca uprawnienia **Usługa połączyć** w Centrum IoT może wywołać metody na urządzeniu.

### <a name="when-to-use"></a>Kiedy należy używać

Metody urządzenia są podobne do [wiadomości w chmurze do urządzenia] [ lnk-devguide-messages] w tym oba włączyć chmury wewnętrznej do przekazania informacji do urządzenia, ale różnią się podstawowe metody. Koncepcji metod są synchroniczne i nie trwały, gdy asynchroniczne z 48 godzin ważności wiadomości w chmurze do urządzenia.

Metody wykonaj deseń żądanie odpowiedź i nie są trwałe. Brak ważności zawiera dwa natychmiastowe korzyści po są steruje urządzeń:

- **Natychmiastowe opinii na temat wykonywania metody** oznacza, że nie jest konieczne zarządzanie korelacji między żądaniem i odpowiedzi.
- **Wyższej przepustowości** oznacza, że operacje mogą być wykonywane szybciej, ponieważ Centrum IoT nie udostępnia wszelkie wytrzymałości. Centrum IoT zapewnia kolejnych połączeń metoda na jednostkę niż wiadomości w chmurze do urządzenia.

Wiadomości w chmurze do urządzenia niekoniecznie poleceń na urządzeniu, ale zamiast reprezentują usługi w chmurze przechodzące jakimś informacji na urządzeniu, aby wznowić pracę w jego wolnym i których urządzenie może lub mogą nie odpowiadać. Chmura do urządzenia wiadomości mają przekroczenia limitu czasu dłużej (maksymalnie 48 godzin) podczas metod wygaśnie szybciej.

Użyj metody urządzenia dla wywoływanie natychmiastowej poleceń na urządzeniu i zadania według harmonogramu wywołania polecenia na urządzeniu.

## <a name="method-lifecycle"></a>Metoda cyklu

Metody są wykonywane na tym urządzeniu i może wymagać zero lub więcej wartości wejściowych w ładunku metody poprawnie wystąpienia. Wywoływanie bezpośrednie metody za pomocą identyfikatora URI usługi skierowaną (`{iot hub}/twins/{device id}/methods/`). Urządzenie, otrzymuje bezpośrednich metod za pośrednictwem tematu MQTT specyficzne dla urządzenia (`$iothub/methods/POST/{method name}/`). Firma Microsoft może obsługiwać metod na dodatkowych protokołów sieciowych urządzenia po stronie w przyszłości.

> [AZURE.NOTE] Po rozpoczęciu procesu metoda bezpośrednia na urządzeniu, nazwy i wartości właściwości może zawierać tylko US-ASCII drukowania alfanumeryczne, z wyjątkiem w następujący zestaw: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Metody są synchroniczne i albo pomyślnie lub niepowodzenie limit czasu, po (domyślny: 30 sekund, można ustawić w górę do wartości 3600 sekund). Metody są przydatne w sytuacjach interakcyjnych miejscu na urządzeniu działa tylko wtedy, gdy urządzenie jest online i odbierania poleceń, takich jak włączenie światło z telefonu. W poniższych scenariuszach chcesz wyświetlanie natychmiastowej sukces lub Niepowodzenie w celu usług w chmurze mogą działać w wyniku tak szybko, jak to możliwe. Urządzenie może zwracać niektórych treść wiadomości w wyniku metody, ale nie jest wymagane dla metody to zrobić. Istnieje gwarantowana z kolejnością lub dowolny znaczeń właściwych współbieżności na metody połączenia.

Wywołania metody urządzenia są HTTP tylko ze strony chmurze i tylko do MQTT ze strony urządzenia.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji o używaniu bezpośrednich metod.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Wywoływanie metody bezpośrednio z aplikacji wewnętrznej

### <a name="method-invocation"></a>Wywoływanie metody

Metoda bezpośrednia wywołań na urządzeniu są połączeń protokołu HTTP, które obejmują:

- *Identyfikator URI* specyficzne dla urządzenia (`{iot hub}/twins/{device id}/methods/`)
- WPIS *metody*
- *Nagłówki* zawierające autoryzacji, żądanie identyfikator, typ zawartości i kodowanie zawartości
- Przezroczysty JSON *treści* w następującym formacie:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Limit czasu jest w sekundach. Jeśli nie ustawiono limitu czasu, domyślnie 30 sekund.
  
### <a name="response"></a>Odpowiedź

Wewnętrznej otrzymuje odpowiedź, która obejmuje:

- *Kod stanu HTTP*, która jest używana pod kątem błędów pochodzące z Centrum IoT, w tym błąd 404 dla urządzeń nie są obecnie połączone
- *Nagłówki* , które zawierają etag, żądanie identyfikator, typ zawartości i kodowanie zawartości
- JSON *treści* w następującym formacie:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Obie `status` i `body` są dostarczane przez urządzenie i używane do odpowiedzi z kodem stanu dla urządzenia i/lub opis.

## <a name="handle-a-direct-method-on-a-devcie"></a>Uchwyt metoda bezpośrednia na devcie

### <a name="method-invocation"></a>Wywoływanie metody

Urządzenia odbierania żądań metoda bezpośrednia na temat MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

Treść, która odbiera urządzenie znajduje się w następującym formacie:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Metoda żądania są QoS 0.

### <a name="response"></a>Odpowiedź

Urządzenie wysyła odpowiedzi na `$iothub/methods/res/{status}/?$rid={request id}`, gdzie:

 - `status` Właściwość jest stan Dostarczone urządzenia wykonywania metody.
 - `$rid` Właściwości jest Identyfikatorem żądania od wywołania metody odebrana IoT koncentratora.

Jednostka jest ustawiany przez urządzenia i może być dowolny stan.

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] wyświetla różne języka SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT Centrum MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu bezpośrednich metod, może być w temacie przewodnik dewelopera:

- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Jeśli chcesz wypróbować niektóre pojęcia opisane w tym artykule, może być w następujących samouczek Centrum IoT:

- [Użyj metody chmury do urządzenia][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
