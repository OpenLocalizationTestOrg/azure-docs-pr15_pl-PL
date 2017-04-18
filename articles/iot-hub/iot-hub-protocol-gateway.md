<properties
   pageTitle="Azure bramy protokołu IoT | Microsoft Azure"
   description="Informacje dotyczące używania bramy protokołu Azure IoT rozszerzenie możliwości i obsługi protokołu Centrum IoT Azure."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Obsługa protokołów dodatkowych koncentratora IoT

Azure Centrum IoT oryginalnie obsługuje komunikację przez protokoły MQTT, AMQP i HTTP. W niektórych przypadkach urządzeń bram pole może nie być możliwe użyć jednego z tych standardowych protokołów i wymaga dostosowania Protocol (protokół). W takich przypadkach można użyć niestandardowych bramy. Brama niestandardowe można włączyć dostosowania protokołu dla punktów końcowych Centrum IoT łączące ruchu do i z Centrum IoT. Za pomocą [bramy protokołu Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) jako brama niestandardowe umożliwiające dostosowanie protokół IoT Centrum.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protokół bramy

Brama protokołu Azure IoT to ramy protokołu dostosowanie, które jest przeznaczony dla wysokiej klasy, dwukierunkowy urządzenia komunikowanie się z Centrum IoT. Brama Protocol (protokół) jest składnik przekazującej, który akceptuje połączenia urządzenia przez określonego protokołu. Tworzy ono ruch do koncentratora IoT nad AMQP 1.0. Brama protokołu Azure IoT jest dostępna jako projekt oprogramowanie open source, aby zapewnić elastyczność do dodawania obsługi różnych protokoły i wersje Protocol (protokół).

Należy wdrożyć protokół bramy w Azure w sposób wysoce skalowalna przy użyciu usług w chmurze Azure pracownik role. Ponadto bramy protokołu mogą być rozmieszczone w środowiskach lokalnych, takich jak pola bramy.

Brama protokołu Azure IoT zawiera karty protokół MQTT, która umożliwia Dostosuj zachowanie protokołu MQTT, jeśli to konieczne. Ponieważ IoT Centrum udostępnia wbudowaną obsługę protokołu v3.1.1 MQTT, tylko rozważ przy użyciu karty protokół MQTT, jeśli masz potrzebę dostosowania Protocol (protokół) lub szczegółowe wymagania, aby uzyskać dodatkowe funkcje.

Karta MQTT ilustruje też modelu programowania dotyczące tworzenia kart protokół dla innych protokołów. Ponadto model programowania bramy protokołu Azure IoT umożliwia Podłącz niestandardowe składniki specjalistyczne przetwarzania, takich jak niestandardowe uwierzytelnianie, przekształcenia wiadomości, kompresja/dekompresja lub Szyfrowanie/odszyfrowywanie ruch między urządzeniami i Centrum IoT.

Elastyczność protokół bramy i wdrażanie MQTT znajdują się w projekcie oprogramowanie open source. Pozwala na dostosowanie wykonania, stosownie do potrzeb.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat bramy protokołu Azure IoT i jak używać i wdrożyć go jako część rozwiązania IoT, zobacz:

* [Azure repozytorium bramy w GitHub IoT Protocol (protokół)](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Przewodnik dewelopera bramy Azure IoT Protocol (protokół)](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Aby dowiedzieć się więcej na temat planowania wdrożenia Centrum IoT, zobacz:

- [Porównanie z koncentratorów zdarzenia][lnk-compare]
- [Skalowanie, HA i DR][lnk-scaling]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
