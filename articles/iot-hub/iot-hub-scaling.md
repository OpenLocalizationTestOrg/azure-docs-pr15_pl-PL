<properties
 pageTitle="Azure Centrum IoT skalowania | Microsoft Azure"
 description="Zawiera opis sposobu skalowania Centrum IoT Azure."
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
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Centrum IoT skalowania

Azure Centrum IoT może obsługiwać maksymalnie milionów jednocześnie podłączone urządzenia. Aby uzyskać więcej informacji, zobacz [Centrum IoT ceny][lnk-pricing]. Każdej jednostki Centrum IoT umożliwia określoną liczbę dzienny wiadomości.

Aby poprawnie przeskalować rozwiązanie, należy rozważyć, czy określonego korzystanie z Centrum IoT. W szczególności należy rozważyć przepustowość Szczyt wymagane dla kategorii następujące operacje:

* Urządzenia w chmurze wiadomości
* Wiadomości w chmurze do urządzenia
* Operacje rejestru tożsamości

Oprócz te informacje przepustowość zobacz [Centrum IoT przydziałów i limity][] i w związku z tym projektowanie rozwiązania.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Wydajność obsługi wiadomości urządzenia w chmurze i w chmurze do urządzenia

Najlepszym sposobem na rozwiązanie Centrum IoT rozmiaru jest ocena ruch na podstawie na jednostkę.

Urządzenia w chmurze wiadomości przestrzegaj następujących wytycznych stałej przepustowości.

| Warstwy | Trwały przepustowość | Wyślij stałej stopy |
| ---- | -------------------- | ------------------- |
| S1 | Maksymalnie minutę 1111 KB na jednostkę<br/>(1,5 GB/dzień/jednostka) | Średnia 278 minutę wiadomości na jednostkę<br/>(400 000 wiadomości dziennie na jednostkę) |
| S2 | Maksymalnie 16 MB na minutę na jednostkę<br/>(22.8 GB/dzień/jednostka) | Średnia 4167 wiadomości na minutę na jednostkę<br/>(6 milionów wiadomości dzień na jednostkę) |
| S3 | Maksymalnie minutę 814 MB na jednostkę<br/>(1144.4 GB/dzień/jednostka) | Średnia 208,333 wiadomości na minutę na jednostkę<br/>(300 milionów wiadomości dzień na jednostkę) |

## <a name="identity-registry-operation-throughput"></a>Przepustowość operacji rejestru tożsamości

Operacje rejestru tożsamości Centrum IoT nie powinna być wykonywania operacji, jak są one głównie związane z urządzenia inicjowania obsługi administracyjnej.

Dla określonej serii liczb wydajności Zobacz [Centrum IoT przydziałów i limity][].

## <a name="sharding"></a>Sharding

Podczas gdy jednym centrum IoT można skalować miliony urządzeń, czasami rozwiązania wymaga cech określonych wydajności, które nie daje gwarancji jednym centrum IoT. W takim przypadku zaleca się podziału urządzeniach na wiele koncentratorów IoT. Wiele koncentratorów IoT gładkim seria ruchu i Uzyskaj wymagane przepustowość lub stawki operacji, które są wymagane.

## <a name="next-steps"></a>Następne kroki

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[Centrum IoT przydziałami i limity]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
