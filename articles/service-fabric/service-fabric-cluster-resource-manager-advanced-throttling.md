<properties
   pageTitle="Ograniczanie w Menedżerze zasobów klaster tkaninie usługi | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować limity podany przez usługę tkaninie klaster Menedżera zasobów."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Ograniczanie zachowanie z usługi tkaninie klaster Menedżera zasobów
Nawet jeśli klaster Menedżera zasobów zostały skonfigurowane poprawnie, można uzyskać zakłócenia klaster. Na przykład może być jednoczesne węzeł lub błąd domeny błędy — co się stanie, jeśli które wystąpiły podczas uaktualniania? Menedżer zasobów będzie próba rozwiązania wszystko najlepsza, ale w czasie w następujący sposób warto rozważyć backstop tak, aby klastrem możliwość stabilizację (węzły, które mają pochodzić wykonaj ponownie, parametry sieci naprawę, są wdrażane poprawioną bitów). Ułatwiające tego rodzaju sytuacjach, Menedżera zasobów usługi tkaninie klaster zawiera kilka limity. Uwaga, że te limity są one dość i zwykle nie powinny być używane, o ile było niektóre staranne obliczenia wykonywane wokół ilości pracy równoległe, które rzeczywiście można wykonywać w grupie, a także często musisz odpowiedzieć na tych sortuje z (ahem) zdarzenia nieplanowanego makroskopowe zmiany konfiguracji (AKA: "Bardzo nieprawidłowe dni").

Ogólnie rzecz biorąc zaleca się unikanie bardzo nieprawidłowe dni przez inne opcje (na przykład kod regularnych aktualizacji i unikanie overscheduling klaster będzie zaczynać się) zamiast ograniczania klaster, aby uniemożliwić korzystanie z zasobów próbach ustalenie się). Limity miały wartość domyślną, znalezionych do obsługi jako wartości ok domyślnych, które należy prawdopodobnie zapoznaj się z i dostosowywanie ich do Twojej oczekiwanych rzeczywiste obciążenie. Podczas nie nadmiernie ograniczając lub ładowania, które klaster jest dobrym rozwiązaniem, które może ustalić, czy przypadkach której (do momentu można je usunąć) miejsce, w którym należy przygotować kilka limity w miejscu, nawet jeśli to klaster wydłuży się ustabilizuje.

##<a name="configuring-the-throttles"></a>Konfigurowanie limity
Ograniczenia, które są domyślnie dostępne są następujące:

-   GlobalMovementThrottleThreshold — ta opcja kontroluje całkowitą liczbę ruchów w klastrze niektórych czasie (zdefiniowany jako GlobalMovementThrottleCountingInterval, wartość w sekundach)
-   MovementPerPartitionThrottleThreshold — ta opcja kontroluje całkowitą liczbę ruchów dla dowolnego partition usługi przez pewien czas (MovementPerPartitionThrottleCountingInterval, wartość w sekundach)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Należy pamiętać, że w większości przypadków możemy zobaczył klientów za pomocą te limity, który był ponieważ zostały one już w środowisku zasobów ograniczona (takie jak przepustowość sieci ograniczone do pojedynczych węzłów lub dyski, które nie zostały do wymagań replice równoległe tworzy są umieszczone na ich) którego miała tych operacji nie powiodła się lub jest powolne mimo to.  W następujących sytuacjach klientów były doświadczenia wiedzy, która potencjalnie rozszerzeniu ilość czasu, który chcesz wykonać klaster do osiągnięcia stabilny stan, łącznie z wiedzy, która może skończyć uruchomienia w dolnym ogólnego niezawodności podczas zostały one ograniczenie.

## <a name="next-steps"></a>Następne kroki
- Aby dowiedzieć się o jak Menedżer zasobów klaster zarządza i sald obciążenia w klastrze, zapoznaj się z artykułem na [równoważenia obciążenia](service-fabric-cluster-resource-manager-balancing.md)
- Menedżer zasobów klaster zawiera wiele opcji opisu klaster. Aby dowiedzieć się więcej na temat ich zapoznaj się z niniejszego artykułu [opisem klastrze tkaninie usługi](service-fabric-cluster-resource-manager-cluster-description.md)
