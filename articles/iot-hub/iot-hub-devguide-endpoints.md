<properties
 pageTitle="Przewodnik dewelopera — punkty końcowe Centrum IoT | Microsoft Azure"
 description="Przewodnik Azure Deweloper Centrum IoT — informacje na temat Centrum IoT punkty końcowe"
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

# <a name="reference---iot-hub-endpoints"></a>Odwołanie — Centrum IoT punkty końcowe

## <a name="list-of-iot-hub-endpoints"></a>Lista Centrum IoT punkty końcowe

Centrum IoT Azure to usługa wielu dzierżawy, która udostępnia ich funkcjonalność poszczególnych podmiotów. Na poniższym diagramie pokazano różne punkty końcowe, że Centrum IoT udostępnia.

![Centrum IoT punkty końcowe][img-endpoints]

Oto opis punkty końcowe:

* **Dostawcy zasobów**. Dostawca IoT Centrum zasobów udostępnia [Menedżera zasobów Azure] [ lnk-arm] interfejs, który umożliwia właściciele subskrypcji Azure do tworzenia i usuwania koncentratorów IoT i aktualizowania właściwości koncentratora IoT. Właściwości Centrum IoT decydujących [Zasady zabezpieczeń na poziomie Centrum][lnk-accesscontrol], zamiast kontroli dostępu na poziomie urządzenia i funkcjonalności opcje wiadomości w chmurze do urządzenia i urządzenia w chmurze. Dostawcy IoT Centrum zasobów umożliwia do [wyeksportowania tożsamości urządzenia][lnk-importexport].
* **Zarządzanie tożsamościami urządzenia**. Każdy Centrum IoT udostępnia zestaw pozostałych HTTP punktów końcowych do zarządzania tożsamością urządzenia (Tworzenie, pobieranie, aktualizowanie i usuwanie). [Urządzenie tożsamości] [ lnk-device-identities] są używane do urządzenia uwierzytelnianie i kontrola dostępu.
* **Zarządzanie urządzeniami podwójnego**. Każdy Centrum IoT udostępnia zestaw punkt końcowy pozostałych HTTP usługi skierowaną do kwerendy, a następnie zaktualizuj [twins urządzenia] [ lnk-twins] (wymagana aktualizacja znaczników i właściwości).
* **Zarządzanie zadaniami**. Każdy Centrum IoT udostępnia zestaw punktu końcowego HTTP pozostałych usług skierowaną do wykonywania kwerend i zarządzanie [zadaniami][lnk-jobs].
* **Punkty końcowe urządzenia**. Dla każdego urządzenia obsługi administracyjnej w rejestrze tożsamości urządzenia Centrum IoT udostępnia zestaw punkty końcowe, które urządzenie służy do wysyłania i odbierania wiadomości:
    - *Wysyłanie wiadomości urządzenia w chmurze*. Użyj tego punktu końcowego do [wysyłania wiadomości urządzenia w chmurze][lnk-d2c].
    - *Odbieranie wiadomości chmury do urządzenia*. Urządzenie używa tego punktu końcowego do odbierania wybranych [wiadomości w chmurze do urządzenia][lnk-c2d].
    - *Inicjowanie wysyłania plików*. Urządzenie używa tego punktu końcowego do odbierania identyfikatora URI Azure miejsca do magazynowania skojarzeń zabezpieczeń z Centrum IoT, aby [przekazać plik][lnk-upload].
    - *Pobieranie i właściwości podwójnego aktualizacji*. Urządzenie korzysta ten punkty końcowe, aby uzyskać dostęp do jej [podwójnego urządzenia][lnk-twins]jego właściwości.
    - *Odbierz bezpośrednie metody żądania*. Urządzenie używa tej punkty końcowe słuchać [bezpośrednich metod][lnk-methods]na żądania.

    Te punkty końcowe są dostępne za pomocą [MQTT v3.1.1][lnk-mqtt], protokołu HTTP 1.1 i [AMQP 1.0] [ lnk-amqp] protokołów. Należy zauważyć, że AMQP jest również dostępne na [WebSockets] [ lnk-websockets] na porcie 443.
    
    Twins i metody endpoins są dostępne tylko przy użyciu [MQTT v3.1.1][lnk-mqtt].

* **Punkty końcowe usługi**. Każdy Centrum IoT udostępnia zestaw punkty końcowe, które sieci wewnętrznej aplikacji umożliwia komunikowanie się za pomocą urządzenia. Te punkty końcowe są obecnie tylko dostępne za pomocą [AMQP] [ lnk-amqp] Protocol (protokół), z wyjątkiem punkt końcowy wywoływanie metody uwidaczniany za pośrednictwem protokołu HTTP 1.1.
    - *Odbieranie wiadomości urządzenia w chmurze*. Ten punkt końcowy jest zgodny z [Koncentratorów zdarzenia Azure][lnk-event-hubs]. Usługa wewnętrznej służy do Czytaj wszystkie [wiadomości urządzenia w chmurze] [ lnk-d2c] wysyłane przez urządzenia.
    - *Wysyłanie wiadomości w chmurze do urządzenia i odbieranie potwierdzenia dostarczenia*. Te punkty końcowe włączyć usługi aplikacji wewnętrznej wysyłania wiarygodnych [wiadomości w chmurze do urządzenia][lnk-c2d]i uzyskanie odpowiedniego dostarczenia lub potwierdzenia wygaśnięcia.
    - *Powiadomienia o odbierania plików*. Ten punkt końcowy wiadomości umożliwia otrzymywanie powiadomień o z urządzenia pomyślnie przekazać plik. 
    - *Wywoływanie metody bezpośredniego*. Umożliwia tego punktu końcowego usługi wewnętrznej wywoływanie [Metoda bezpośrednia] [ lnk-methods] na urządzeniu.

[Interfejsy API Centrum IoT i SDK] [ lnk-sdks] artykule opisano różne sposoby, aby uzyskać dostęp do tych punktów końcowych.

Na koniec należy zauważyć, że wszystkie Centrum IoT punkty końcowe za pomocą [protokołu TLS] jest[ lnk-tls] Protocol (protokół), a nie punkt końcowy kiedykolwiek są wyświetlane w kanałach bez szyfrowania niezabezpieczoną.

## <a name="field-gateways"></a>Pole bram

W rozwiązaniu IoT *bramy pole* znajduje się między urządzeniami i Centrum IoT punktów końcowych. Jest zazwyczaj umieszczane zbliżony urządzenia. Urządzenia komunikowanie się bezpośrednio z bramą pola za pomocą protokołu obsługiwanego przez urządzenia. Brama pola łączy do punktu końcowego Centrum IoT przy użyciu protokołu, który jest obsługiwany przez Centrum IoT. Bramy pole może być bardzo specjalistyczne sprzętu lub komputera power niskim z oprogramowaniem, wykonująca scenariusz zakończenia do końca, dla którego jest przeznaczona bramy.

Możesz użyć [Azure IoT bramy SDK] [ lnk-gateway-sdk] do wykonania bramy pola. Zestaw SDK oferuje określonych funkcji, takich jak możliwość Multipleksowanie komunikacji z wielu urządzeń na to samo połączenie do koncentratora IoT.

## <a name="next-steps"></a>Następne kroki

Inne tematy odwołanie w tym przewodniku Deweloper IoT Centrum obejmują:

- [Języka kwerend twins, metody i zadania][lnk-devguide-query]
- [Przydziałów i ograniczanie][lnk-devguide-quotas]
- [IoT MQTT Centrum pomocy technicznej][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md