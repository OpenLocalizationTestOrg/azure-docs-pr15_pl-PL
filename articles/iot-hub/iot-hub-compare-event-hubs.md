<properties
 pageTitle="Porównanie Azure Centrum IoT do koncentratorów Azure zdarzenia | Microsoft Azure"
 description="Porównanie usług Azure IoT Centrum i koncentratory zdarzenia Azure wyróżnianie różnice funkcjonalne i przypadków użycia."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Porównanie Azure Centrum IoT i koncentratory Azure zdarzenia

Jest jednym z przypadków użycia głównego koncentratora IoT zebrać telemetrycznego z urządzeń. Z tego powodu IoT koncentratora jest często porównywane [Koncentratory zdarzenia Azure][]. Jak Centrum IoT koncentratory zdarzenia to zdarzenie przetwarzania usługa, która umożliwia zdarzeń i telemetrycznego wchodzącego w chmurze na ogromną skalę z krótki czas oczekiwania i wysoka niezawodność.

Usługi jednak liczne różnice, które są wyszczególnione w poniższej tabeli.

| Obszar | Centrum IoT | Koncentratory zdarzenia |
| ---- | ------- | ---------- |
| Desenie komunikacji | Umożliwia urządzenia w chmurze i w chmurze do urządzenia wiadomości. | Tylko umożliwia ingress zdarzenia (zazwyczaj traktowane jako scenariuszach urządzenia w chmurze). |
| Obsługa protokołu urządzenia | Obsługuje MQTT, AMQP, AMQP przez WebSockets i HTTP. Ponadto Centrum IoT współdziała z [bramy protokołu Azure IoT][lnk-azure-protocol-gateway], implementacji bramy protokołu można dostosować do obsługi niestandardowy. | Obsługuje AMQP, AMQP WebSockets i HTTP. |
| Zabezpieczenia | Udostępnia na urządzeniu tożsamości i kontroli dostępu odwoływalny. Można znaleźć w [sekcji Zabezpieczenia IoT Centrum deweloperów przewodnika]. | Udostępnia koncentratory zdarzenia na poziomie [zasady dostępu udostępnione][Event Hubs - security], ograniczone odwołania obsługuje za pomocą [programu publisher zasad][Event Hubs publisher policies]. Rozwiązania IoT często należy wdrożyć niestandardowe rozwiązanie do obsługi poświadczenia na urządzenie i fałszowaniu anti miary. |
| Operacje monitorowania | Umożliwia rozwiązania IoT subskrybować bogatego zestawu urządzenia tożsamości łączności i zarządzania zdarzenia, na przykład błędy uwierzytelniania poszczególnych urządzeń, ograniczania i wyjątki nieprawidłowy format. Te zdarzenia umożliwiają szybkie identyfikowanie problemów z łącznością na poziomie urządzenia. | Udostępnia tylko agregacji metryki. |
| Skala | Jest zoptymalizowane pod kątem obsługi miliony urządzeń jednocześnie. | Może obsługiwać więcej ograniczoną liczbą połączenia — maksymalnie 5000 połączenia AMQP, jak [Bus usługi Azure przydziałów][]. Z drugiej strony koncentratory zdarzenia umożliwia określenie partycją dla każdej wysłanej wiadomości. |
| SDK urządzenia | Zawiera [urządzenia SDK] [ Azure IoT Hub SDKs] dla wielu różnych platform i języków. | Jest obsługiwana w .NET i C. Także, że AMQP i HTTP wysyłanie interfejsów. |
| Przekazywanie pliku | Umożliwia rozwiązania IoT przekazywania plików z urządzeń w chmurze. Zawiera punkt końcowy powiadomienie pliku do integracji przepływów pracy i operacje monitorowania kategorii Obsługa debugowania. | Wzór sprawdzania roszczeń umożliwia ręczne żądanie pliki z urządzeń i podaj urządzeń przy użyciu klucza miejsca do magazynowania dla transakcji. |

Podsumowując nawet wtedy, gdy tylko przypadków użycia ingress telemetrycznego urządzenia w chmurze IoT Centrum udostępnia usługę przeznaczone dla IoT urządzenia łączności. Nadal będzie rozwiń atrakcyjne oferty dla tych scenariuszy z funkcje specyficzne dla IoT. Koncentratory zdarzenie jest przeznaczony dla zdarzenia wchodzącego na ogromną skalę, zarówno w kontekście scenariusze między centrum danych i centrum danych wewnątrz.

Nie jest nietypowych jednoczesnego używania zarówno Centrum IoT i koncentratory zdarzenia w tym samym rozwiązaniu — miejsce, w którym Centrum IoT obsługuje komunikację urządzenia w chmurze i koncentratory zdarzenie obsługuje zdarzenia późniejszym etapie wchodzącego do aparatów przetwarzania w czasie rzeczywistym.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat planowania wdrożenia Centrum IoT, zobacz [Skalowanie, HA i DR][lnk-scaling].

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[Koncentratory Azure zdarzenia]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Sekcja Zabezpieczenia podręcznika IoT Centrum deweloperów]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure przydziałów Bus usługi]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
