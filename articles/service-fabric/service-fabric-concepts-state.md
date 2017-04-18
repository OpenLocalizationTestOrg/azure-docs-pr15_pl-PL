<properties
   pageTitle="Definiowanie i stan zarządzania | Microsoft Azure"
   description="Jak zdefiniować i zarządzanie stan usługi w tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="service-state"></a>Stan usługi
**Stan usługi** odwołuje się do danych usługi wymaga działania. Zawiera struktury danych i zmiennych, które Usługa odczytuje i zapisuje pracę.

Należy rozważyć, czy usługi prostego kalkulatora, na przykład. Ta usługa ma dwie liczby i zwraca ich sumy. To jest czysto stateless usługa, która nie zawiera danych skojarzone z nim.

Teraz należy rozważyć, czy samej Kalkulatora, ale oprócz komputerowych Suma, również ma metodę zwracanie ostatniej Suma, który ma go obliczona. Ta usługa jest teraz stateful — zawiera kilka Województwo, z którego zapisuje w (jeśli ją oblicza sumę nowe) i Odczyt z (gdy zwraca ostatni obliczana suma).

W tkaninie usługi Azure pierwszej usługi nosi stateless usługi. Druga usługa jest wywoływana akumulującej stan usługi.

## <a name="storing-service-state"></a>Przechowywanie stanu usługi
Stan można externalized lub wspólną lokalizację kod, który jest manipulowania stanu. Externalization stan odbywa się zwykle za pomocą zewnętrznej bazy danych lub magazynu. W naszym przykładzie kalkulatora może to być z bazą danych SQL, w której bieżący wynik jest przechowywany w tabeli. Każde żądanie, aby obliczyć sumę wykonuje aktualizację w tym wierszu.

Stan można także znajdują się w tym obsługujące ten kod. Stanowe usług w tkaninie usługi utworzono przy użyciu tego modelu. Usługa tkaninie udostępnia infrastruktury w celu zapewnienia, że ten stan jest wysoce dostępne i odporność na uszkodzenia w przypadku awarii.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na tkaninie usługi pojęcia zobacz:

- [Dostępność usług tkaninie usługi](service-fabric-availability-services.md)

- [Skalowalność usług tkaninie usługi](service-fabric-concepts-scalability.md)

- [Podziału usług tkaninie usługi](service-fabric-concepts-partitioning.md)
