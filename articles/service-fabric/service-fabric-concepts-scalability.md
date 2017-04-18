<properties
   pageTitle="Skalowalność usług tkaninie usługi | Microsoft Azure"
   description="Zawiera opis sposobu skalowania usług tkaninie usługi"
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

# <a name="scaling-service-fabric-applications"></a>Skalowanie tkaninie usługi aplikacji
Azure tkaninie usługi ułatwia tworzenie skalowalna aplikacji usług, partycje i repliki równoważenia obciążenia na wszystkich węzłach w klastrze. Dzięki temu wykorzystania maksymalna zasobów.

Duży skala dla aplikacji usługi tkaninie uzyskuje się na dwa sposoby:

1. Skalowanie na poziomie partition

2. Skalowanie na poziomie Nazwa usługi

## <a name="scaling-at-the-partition-level"></a>Skalowanie na poziomie partition
Usługa tkaninie obsługuje dzielony poszczególnych usług na kilka mniejszych partycje. [Omówienie podziału](service-fabric-concepts-partitioning.md) zawiera informacje dotyczące typów systemów podziału, które są obsługiwane. Repliki każdego partition są rozmieszczony węzłów w klastrze. Należy rozważyć, czy usługa, która używa ranged schematu podziału z kluczem niskim 0, klucz wysoka 99 i cztery partycje. W klastrze trzy usługi mogą być rozmieszczone z czterech replikami, które współużytkują zasoby w każdym węźle, jak pokazano poniżej:

![Układ partycją z trzy węzły](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Zwiększenie liczby węzłów umożliwia tkaninie usługi korzystanie z zasobów na nowe węzły przez przeniesienie niektórych repliki do pustego węzły. Zwiększenie liczby węzłów cztery, usługa ma trzy repliki uruchomione na każdym węźle (różne partycje), co pozwala na lepsze wykorzystanie zasobów i wydajności.

![Układ partycją z czterech węzłów](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Skalowanie na poziomie Nazwa usługi
W przypadku wystąpienia usługi jest konkretnego wystąpienia nazwy aplikacji i nazwa typu usługi (zobacz [cyklu życia aplikacji usługi tkanina](service-fabric-application-lifecycle.md)). Podczas tworzenia usługi, określ partycją systemu (zobacz [usługi podziału tkaninie Service](service-fabric-concepts-partitioning.md)) może być używany.

Pierwszy poziom skalowania jest według nazwy usługi. Można tworzyć nowe wystąpienia usługi, z różnych poziomów podziału jako usługi starsze wystąpień usługi są zamieniane na zajęty. Dzięki temu nowych klientów usługi do użycia wystąpień usługi zajęty mniej zamiast nich coraz bardziej zajęty.

Jedną z opcji na zwiększenie pojemności, jak również liczby partition rosnącej lub malejącej, jest na tworzenie nowego wystąpienia usług nowy schemat partycją. Spowoduje to dodanie złożoności, jednak jako dużo potrzeby klientów wiedzieć, kiedy i jak używać inaczej nazwaną usługę.

### <a name="example-scenario-embedded-dates"></a>Scenariusz przykładowy: osadzone dat
Jeden scenariusz możliwe jest używanie informacje o dacie jako część nazwy usługi. Na przykład można było używać wystąpienie usługi przy użyciu określonej nazwy dla wszystkich klientów, którzy sprzężone w 2013 oraz inną nazwę dla klientów, którzy w 2014 r. Ten nazewnictwa pozwala na programowy zwiększenie imiona i nazwiska w zależności od daty (jako metod 2014 wystąpienie usługi 2014 r można utworzyć na żądanie).

Jednak to podejście jest oparty na klientach przy użyciu informacji nazewnictwa specyficzne dla aplikacji, który wykracza poza zakres wiedzy tkaninie usługi.

- *Za pomocą konwencji nazewnictwa*: W 2013, gdy aplikacja przechodzi live, można tworzyć usługi o nazwie tkaninie: / aplikacji/service2013. W drugim kwartale 2013, możesz utworzyć innej usługi o nazwie tkaninie: / aplikacji/service2014. Obie te usługi są tego samego typu usługi. W tej metody klient należy stosować logiki do konstruowania nazwę odpowiedniej usługi na podstawie roku.

- *Korzystanie z usługi wyszukiwania*: innego wzorca jest zapewnienie Usługa wyszukiwania pomocniczą, która można podać nazwę Usługa inny klawisz. Następnie można utworzyć nowe wystąpienia usług przez usługę wyszukiwania. Sama usługa wyszukiwania nie zachować wszelkie dane aplikacji, a tylko dane dotyczące nazwy usług, które tworzy. W związku z tym na podstawie roku powyższym przykładzie klienta czy najpierw skontaktować się z usługą wyszukiwania, aby dowiedzieć się, nazwa usługi obsługi danych dla danego roku, a następnie użyj tej nazwy usługi do wykonywania bieżącej operacji. Wynik wyszukiwania pierwszego mogą być buforowane.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na tkaninie usługi pojęcia zobacz:

- [Dostępność usług tkaninie usługi](service-fabric-availability-services.md)

- [Podziału usług tkaninie usługi](service-fabric-concepts-partitioning.md)

- [Definiowanie i stan zarządzania](service-fabric-concepts-state.md)
