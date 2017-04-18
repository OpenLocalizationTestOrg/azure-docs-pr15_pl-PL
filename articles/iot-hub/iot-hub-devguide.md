<properties
 pageTitle="Deweloper przewodnik tematy dotyczące Centrum IoT | Microsoft Azure"
 description="Azure IoT Centrum deweloperów przewodnik, który zawiera Centrum IoT punkty końcowe, zabezpieczeń, urządzenia tożsamości rejestru, zarządzania urządzeniami i wiadomości"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Centrum deweloperów przewodnika

Azure Centrum IoT jest w pełni zarządzanych usług, który ułatwia włączanie i niezawodności komunikacji dwukierunkowego między miliony IoT urządzeń i zakończyć aplikację ponownie.

Centrum IoT Azure umożliwia:

* Bezpieczna komunikacja przy użyciu poświadczeń zabezpieczeń na urządzenie i kontroli dostępu.
* Niezawodne urządzenia w chmurze i w chmurze do urządzenia wyraźny wiadomości.
* Łatwe urządzenia łączność z biblioteki urządzenia do najpopularniejszych języków i platformach.

Ten przewodnik Deweloper Centrum IoT zawiera następujące artykuły:

- [Wysyłanie i odbieranie wiadomości z Centrum IoT] [ devguide-messaging] w tym artykule opisano funkcje wiadomości (urządzenia w chmurze i chmury do urządzenia), które udostępnia IoT Centrum. Artykuł zawiera także informacje dotyczące formatów wiadomości i protokoły komunikacyjnym i numery portów, które mogą używać.
- [Przekazywanie plików z urządzenia] [ devguide-upload] w tym artykule opisano, jak możesz przekazać pliki z urządzenia. Artykuł zawiera także informacje na tematy, takie jak powiadomienia, że proces uplaod można wysłać.
- [Zarządzanie tożsamościami urządzenia w Centrum IoT] [ devguide-identities] w tym artykule opisano co przechowuje każdego centrum IoT urządzenia tożsamości rejestru informacji i jak można uzyskać dostęp do i zmodyfikuj ją.
- [Kontrolowanie dostępu do Centrum IoT] [ devguide-security] w tym artykule opisano model zabezpieczeń umożliwia udzielanie dostępu do funkcji Centrum IoT dla obu urządzeń i składniki w chmurze. Artykuł zawiera informacje dotyczące używania tokenów i certyfikaty X.509 i szczegóły dotyczące uprawnień, które można udzielać.
- [Synchronizowanie stanu i konfiguracji za pomocą urządzenia twins] [ devguide-device-twins] w tym artykule opisano funkcje prezentuje ona takie jak synchronizacja z urządzenia z jego podwójnego i koncepcja *podwójnego urządzenia* . Artykuł zawiera informacje o danych przechowywanych w podwójnego urządzenia.
- [Wywoływanie metody bezpośrednio na urządzeniu] [ devguide-directmethods] w tym artykule opisano do zarządzania cyklem życia metoda bezpośrednia informacji na temat sposobu wywołania metody na urządzeniu z Twojej aplikacji wewnętrznej i obsługiwać metodę bezpośrednio na urządzeniu.
- [Planowanie zadań na wielu urządzeniach] [ devguide-jobs] w tym artykule opisano, jak można planować zadania na wielu urządzeniach. Artykuł w tym artykule opisano sposób przesyłania zadań zadań, takich jak wykonywanie bezpośrednie metody aktualizowania devcie przy użyciu podwójnego devcie. Również zawiera opis sposobu stanu zadania.
- [Odwołanie — Centrum IoT punkty końcowe] [ devguide-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania. W artykule opisano również korzystaniem bramy pole umożliwiające niektórych urządzeń nawiązać połączenie Centrum IoT punktów końcowych.
- [Odwołanie — języka kwerend zadań, metody i twins] [ devguide-query] w tym artykule opisano danego języka kwerendy, która umożliwia pobieranie informacji z usługi Centrum o twins urządzenia i zadania.
- [Odwołanie — przydziały i ograniczania] [ devguide-quotas] zawiera podsumowanie przydziałów w usłudze Centrum IoT i ograniczania zachowanie można oczekiwać, aby zobaczyć, gdy przekracza normę.
- [Odwołanie — urządzenia i usługi SDK] [ devguide-sdks] list SDK, możesz użyć opracowywania aplikacji zarówno urządzeń i usług, współpracujących z Twoim Centrum IoT. Artykuł zawiera łącza do dokumentacji online interfejsu API.
- [Odwołanie — Obsługa IoT Centrum MQTT] [ devguide-mqtt] zawiera szczegółowe informacje dotyczące sposobu Centrum IoT obsługuje protokół MQTT. Tym artykule opisano obsługę protokołu MQTT wbudowana SDK i informacje na temat bezpośrednio przy użyciu protokołu MQTT.
- [Słownik] [ devguide-glossary] listę typowych terminów związanych z Centrum IoT.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

