<properties
 pageTitle="Przewodnik dewelopera — SDK Centrum IoT | Microsoft Azure"
 description="Azure IoT Centrum deweloperów guide - informacje i łącza do różnych SDK urządzenia i usługi Azure IoT Centrum."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>Centrum IoT SDK

## <a name="iot-hub-device-sdks"></a>Urządzenie koncentratora IoT SDK

Urządzenie Microsoft Azure IoT SDK zawiera kod, który ułatwia tworzenie urządzeń i aplikacje, które nawiązywanie połączenia i zarządza Azure IoT Centrum usług.

Następujące urządzenia IoT SDK są dostępne do pobrania z GitHub:

- [Azure urządzenia IoT SDK C] [ lnk-c-device-sdk] napisana ANSI C (C99) dla przenośność i zgodność wielu platform.
- [Azure urządzenia IoT SDK dla środowiska .NET][lnk-dotnet-device-sdk]
- [Azure urządzenia IoT SDK dla języka Java][lnk-java-device-sdk]
- [Azure urządzenia IoT SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT urządzenia SDK Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Zobacz pliki readme w repozytoria GitHub informacji na temat używania języka oraz pakiet specyficzny dla platformy menedżerów instalacji na komputerze opracowywania plików binarnych i zależności.

## <a name="os-platforms-and-hardware-compatibility"></a>Platformy systemu operacyjnego i sprzęt zgodności

Aby uzyskać więcej informacji na temat zgodności SDK z określonego urządzenia, zobacz [Azure certyfikowane dla wykazu urządzenia IoT][lnk-certified].

## <a name="iot-hub-service-sdks"></a>Centrum IoT usługi SDK

SDK usługi Microsoft Azure IoT zawiera kod, który ułatwia budowanie aplikacji współpracujących bezpośrednio z Centrum IoT do zarządzania urządzeniami i zabezpieczeń.

Następujące usługi IoT SDK są dostępne do pobrania z GitHub:

- [Azure IoT usługi SDK dla środowiska .NET][lnk-dotnet-service-sdk]
- [Azure IoT usługi SDK Node.js][lnk-node-service-sdk]
- [Azure IoT usługi SDK dla języka Java][lnk-java-service-sdk]

> [AZURE.NOTE] Zobacz pliki readme w repozytoria GitHub informacji na temat używania języka oraz pakiet specyficzny dla platformy menedżerów instalacji na komputerze opracowywania plików binarnych i zależności.

## <a name="azure-iot-gateway-sdk"></a>Azure bramy IoT SDK

Zestaw SDK bramy IoT Azure zawiera infrastruktury i moduły do tworzenia IoT bramy rozwiązań. Można rozszerzyć SDK tworzenie bram dostosowanych do dowolnego scenariusza końcu do końca.

Możesz pobrać [Azure IoT bramy SDK] [ lnk-gateway-sdk] z GitHub.

## <a name="online-api-reference-documentation"></a>Interfejs API dokumentacji odwołania

Poniżej znajduje się lista łączy do dokumentacji online odwołanie interfejsu API dla urządzenia, usługi i bibliotek bramy Azure IoT:

- [Internet czynności (IoT) .NET][lnk-dotnet-ref]
- [Centrum IoT REST][lnk-rest-ref]
- [Azure urządzenia IoT SDK C][lnk-c-ref]
- [Azure urządzenia IoT SDK dla języka Java][lnk-java-ref]
- [Azure IoT usługi SDK dla języka Java][lnk-java-service-ref]
- [Azure urządzenia IoT SDK Node.js][lnk-node-ref]
- [Azure IoT usługi SDK Node.js][lnk-node-service-ref]
- [Azure bramy IoT SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Następne kroki

Inne tematy odwołanie w tym przewodniku Deweloper IoT Centrum obejmują:

- [Centrum IoT punkty końcowe][lnk-devguide-endpoints]
- [Języka kwerend twins, metody i zadania][lnk-devguide-query]
- [Przydziałów i ograniczanie][lnk-devguide-quotas]
- [IoT MQTT Centrum pomocy technicznej][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md