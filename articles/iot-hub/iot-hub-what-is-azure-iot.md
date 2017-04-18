<properties
 pageTitle="Azure rozwiązań dla Internet rzeczy | Microsoft Azure"
 description="Omówienie IoT Azure tym architekturę rozwiązania próbki i jak go odnosi się do Centrum IoT Azure, SDK urządzenia i wstępnie rozwiązań"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Następne kroki

Azure Centrum IoT jest usługą Azure, która umożliwia bezpieczne i z powrotem dwukierunkowego komunikacji między aplikacją zakończenia i miliony urządzeń. Umożliwia wewnętrznej aplikacji, aby:

- Odbieranie telemetrycznego w skali na swoich urządzeniach.
- Przesyłanie danych z urządzenia do procesor zdarzeń strumienia.
- Odbieranie przekazywania plików z urządzeń.
- Chmura do urządzenia polecenia Wyślij do określonych urządzeń.

Za pomocą Centrum IoT do wykonania własnej wewnętrznej rozwiązanie. Ponadto Centrum IoT zawiera rejestru tożsamości urządzenia używane do zapewniania obsługi urządzeń, poświadczeń zabezpieczeń i praw nawiązywania połączenia z poziomu Centrum. Aby dowiedzieć się więcej o Centrum IoT, zobacz [Co to jest Centrum IoT?] [lnk-iot-hub].

Aby dowiedzieć się, jak Centrum IoT Azure umożliwia zgodne ze standardami zarządzania urządzeniami IoT umożliwiają zdalnego zarządzania, konfigurowanie i aktualizowanie urządzenia, zobacz [Omówienie Azure IoT Centrum zarządzania urządzeniami][lnk-device-management].

Aby wdrożyć aplikacje klienckie na wielu różnych platform sprzętu urządzenia i systemy operacyjne, możesz użyć urządzenia IoT SDK. Urządzenie IoT SDK zawierać bibliotek, ułatwiające wysyłanie telemetrycznego do koncentratora IoT i odbierania poleceń w chmurze do urządzenia. Gdy używasz SDK, możesz wybrać z kilku protokołów sieciowych można komunikować się z Centrum IoT. Aby dowiedzieć się więcej, zobacz [informacje dotyczące urządzenia SDK][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchamianie niektóre przykłady, zobacz [Wprowadzenie do Centrum IoT] [ lnk-getstarted] samouczka.

Możesz również skorzystać pakietu [IoT Azure][lnk-iot-suite], czyli zbiór wstępnie rozwiązań. Pakiet IoT umożliwia szybkie rozpoczęcie pracy i skalowanie projektów IoT typowe scenariusze IoT — na przykład zdalnego monitorowania, zarządzanie zasobami i przewidywanych konserwacji.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md