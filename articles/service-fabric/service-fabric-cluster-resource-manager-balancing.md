<properties
   pageTitle="Klaster z Azure usługi tkaninie klaster Menedżer zasobów równoważenia | Microsoft Azure"
   description="Wprowadzenie do równoważenia klaster przy użyciu usługi tkaninie klaster Menedżera zasobów."
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

# <a name="balancing-your-service-fabric-cluster"></a>Klaster tkaninie usługi równoważenia
Menedżer zasobów usługi tkaninie klaster umożliwia raportowanie dynamiczne obciążenie, reakcji na zmiany w grupie poprawiania naruszenia ograniczenia i ponowne równoważenie klaster w razie potrzeby. Jak często czy kwestie, o których a co je uruchamia? Istnieje kilka kontrolek na ten.

Pierwszy zestaw kontrolek wokół równoważenia to zbiór liczniki czasu. Te liczniki czasu określają, jak często Menedżera zasobów klaster sprawdza stan klaster elementów, które należy uwzględnić. Istnieją trzy różne kategorie pracy, każda z własnych odpowiednich czasomierza. Są to:

1.  Położenie — tym etapie dotyczy umieszczenie stanowe repliki ani stateless wystąpienia, które nie są widoczne. Obejmuje nowych usług i obsługi stanowe replik lub stateless wystąpienia, które nie powiodła się i konieczność ich ponownego utworzenia. Usuwanie i upuszczanie replik lub wystąpienia również odbywa się w tym miejscu.
2.  Ograniczenia sprawdza — w tym etapie sprawdza, czy i poprawia naruszenia ograniczenia różnych położenie (reguły) w systemie. Przykłady reguł są elementy takie jak zapewnienie, że węzły nie są na wydajność i spełnienia ograniczeń położenie usługi (więcej informacji na temat tych nowszy).
3.  Równoważenia — tym etapie sprawdza, czy aktywne ponowne równoważenie może być konieczne na podstawie skonfigurowanych żądany poziom saldo dla różnych metryki i ewentualnie próbuje znaleźć rozmieszczenia w klastrze, który jest bardziej strategie.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurowanie Menedżera zasobów klaster czynności i liczniki czasu
Każdego z tych różnych typów poprawek, które można wprowadzić klaster Menedżera zasobów sterują różnych czasomierza, któremu częstotliwość. Aby na przykład jeśli chcesz postępować z umieszczając nowy obciążenia usługi w klastrze, co godzinę (aby wsadowe je w górę), ale chcę zwykła równoważenia sprawdza co kilka sekund, można skonfigurować to zachowanie. Po każdym czasomierza, zadanie zostaje zaplanowane. Domyślnie Menedżer zasobów skanuje stanu i stosuje aktualizacje (tworzeniu partii wszystkie zmiany, które wystąpiły od czasu ostatniego skanowania, takich jak po raz pierwszy węzeł nie działa) co 1/10 sekundy, zestawy położenie i ograniczenia Sprawdź flagi co sekundę i równoważenia flaga 5 sekund.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Dzisiaj firma Microsoft tylko wykonaj jedną z następujących czynności w danym momencie, kolejno (dlatego możemy odwołują się do tych konfiguracji "minimalne interwałem")). To jest tak, aby na przykład możemy zostały już odpowiedzi na żądań oczekiwanie do tworzenia nowych replik przed możemy przejść do równoważenia klaster. Jak widać według przedziałów czasu domyślny określonych, firma Microsoft może Przejrzyj i sprawdź nic trzeba bardzo często, co oznacza, że zestaw zmian udzielamy na końcu każdego kroku jest zwykle mniejsze: Firma Microsoft nie jest skanowania za pośrednictwem godzin zmiany w grupie i próby poprawianie w całym dokumencie, możemy próbujesz uchwyt poleceń więcej lub mniej po ich wprowadzeniu, ale niektóre tworzeniu partii w przypadku wielu elementów tym samym czasie. Dzięki temu zasobu usługi tkaninie Menedżer bardzo odpowiada co się dzieje w klastrze.

Podczas gdy większość z tych zadań są proste (jeśli istnieje ograniczenie naruszenia, poprawki, w przypadku usług, który zostanie utworzony, utwórz je), Menedżer zasobów klaster musi także pewne dodatkowe informacje, aby ustalić, czy klaster imbalanced. W tym mamy dwie części konfiguracji: *Równoważenia progi* i *Progi aktywności*.

## <a name="balancing-thresholds"></a>Progi równoważenia
Próg równoważenia jest formantu dla powodujące aktywne ponowne równoważenie (należy pamiętać, że Czasomierz jest tylko dla częstotliwości Menedżera zasobów klaster należy sprawdzić - nie oznacza, że nic się wydarzy). Próg równoważenia Określa, jak imbalanced klaster musi być dla konkretnej metryki w kolejności dla Menedżera zasobów klaster brać pod uwagę go imbalanced i równoważenia wyzwalacza.

Progi równoważenia są definiowane na podstawie na metryczne jako część definicji klaster. Aby uzyskać więcej informacji na metryki Dowiedz się, [w tym artykule](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Próg równoważenia metryki jest współczynnik. Stopień Załaduj węzeł najbardziej załadowanego podzielona przez ilość obciążenia w węźle najmniej załadowanego przekroczy tę wartość klaster jest traktowany jako imbalanced, a następnie równoważenia zostanie wyzwolony przy następnym Menedżera zasobów klaster sprawdza (domyślnie kiedykolwiek 5 sekund, jako której działalność MinLoadBalancingInterval, jak pokazano powyżej).

![Przykład próg równoważenia][Image1]

W tym przykładzie prosty każdej usługi jest przez inne jednostki metryczne niektórych. W tym przykładzie górny maksymalne obciążenie w węźle jest 5 i minimalna jest 2. Załóżmy, że próg równoważenia dla tej metryki jest 3. W związku z tym, w tym przykładzie górny klaster jest traktowany jako strategie i równoważenia nie zostanie wyzwolony w przypadku Menedżer zasobów klaster sprawdza (ponieważ stosunek w klastrze jest 5/2 = 2,5 to jest mniejsza niż określony próg równoważenia 3).

W tym przykładzie dołu max obciążenie węzła wynosi 10, gdy minimum jest 2, uzyskując stopień 5. Spowoduje to umieszczenie klaster ponad próg równoważenia zaprojektowane 3 dla tej metryki. W wyniku globalnego Uruchom rebalancing będzie zaplanowane następnym równoważenia czasomierz jest uruchamiana. Należy zauważyć, że fakt, że równoważenia wyszukiwania jest kopać niezrozumiały spowoduje przeniesienie — czasami klaster jest imbalanced, ale nie może być poprawiona sytuacji -, ale w przypadku podobny do tego (co najmniej domyślnie) niektóre Załaduj prawie na pewno jest dzielona Węzeł3. Należy zauważyć, że ponieważ nie użyto podejście intensywnie niektórych ładowania może również rozdzielane Węzeł2 od którego spowoduje zmniejszenie ogólnego różnice węzłów, ale mogą spodziewać, że większość Załaduj czy przepływ do Węzeł3.

![Akcje przykład próg równoważenia][Image2]

Uwaga Aby pobieranie poniżej progu równoważenia nie jest jawnie cel — równoważenia progi są tylko *wyzwalacza* informuje Menedżera zasobów klaster, który powinien wyglądać w klastrze określenia jakie ulepszenia go.

## <a name="activity-thresholds"></a>Progi aktywności
Czasami węzły są stosunkowo imbalanced, *łączną liczbę obciążenia w klastrze* jest niska. Może być tylko ze względu na godzinę lub ponieważ klaster jest nowe i tylko pobieranie załadować. W obu przypadkach mogą nie chcesz poświęcać czasu na klaster równoważenia, ponieważ jest faktycznie bardzo mało, które można uzyskać — można będzie tylko poświęcać sieci i obliczeń zasoby, aby przenosić elementy wokół, bez wprowadzania bezwzględne różnic. Ponieważ chcemy tego unikać jest formantem wewnątrz menedżera zasobów, nazywanych progów aktywności, dzięki czemu można określić kilka bezwzględne Granica dolna wykonania — Jeśli żaden węzeł zawiera co najmniej to dużo ładowania, a następnie równoważenia będzie nie zostać wyzwolone nawet wtedy, jeśli jest spełniony progu równoważenia.

Jako przykład załóżmy, że mamy raportów przy użyciu następujących sumy zużycia na tych węzłach. Załóżmy też możemy zachować naszych równoważenia progu 3 dla tej metryki, że mamy także próg aktywności 1536. W pierwszym przypadku gdy klaster jest imbalanced na progu równoważenia żadnego węzła spełnia tego minimalny próg aktywności więc możemy Opuść czynności samodzielnie. W tym przykładzie dołu Węzeł1 to sposób ponad próg aktywności, równoważenia zostanie wykonana (ponieważ przekroczono są zarówno progu równoważenia i progu aktywności metryki)

![Przykład próg aktywności][Image3]

Podobnie jak w przypadku progi równoważenia progi aktywności są zdefiniowane na metryczne za pośrednictwem definicji klaster:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Należy zauważyć, że progi równoważenia i aktywności oba powiązane metryki - równoważenia zostanie tylko wyzwolony Jeśli progi równoważenia i aktywności są przekroczył tę samą metrykę. W związku z tym jeśli firma Microsoft przekracza progu równoważenia pamięci i progu aktywności dla Procesora, równoważenia nie wywoła jak pozostałe progi (równoważenia próg dla Procesora) i próg aktywności pamięci nie został przekroczony.

## <a name="balancing-services-together"></a>Usług bilansowania razem
Jest coś, co interesujące pamiętać, że czy klaster został imbalanced jest decyzja całym, ale tak, możemy przejść o naprawienie go powoduje przenoszenie repliki poszczególnych usług i wystąpienia wokół. Jest to właściwe rozwiązanie, prawy? Jeśli pamięć jest ułożonych w stos w górę w jednym węźle wielu replikach lub wystąpienia może być uczestniczy go, więc go może wymagać przenoszenia stanowe repliki lub stateless wystąpienia, które za pomocą metryki imbalanced, którego dotyczy problem.

Czasami, jeśli klient nawiąże połączenie z nami w górę lub plik bilet informujący, że usługa nie została imbalanced została przeniesiona. Jak może się zdarzyć że usługa przeniesieniu nawet wtedy, gdy wszystkie metryki tej usługi zostały strategie, nawet dokładnie tak, w tym czasie innych równowagi? Zobaczmy!

Cztery usług, Service1 roboczego2, Service3 i Service4, na przykład potrwać. Service1 raportów przed metryki Metric1 i Metric2, klienta2 przed Metric2 i Mmetric3 Service3 przed Metric3 i Metric4 i Service4 przed niektórych metryczne Metric99. Z pewnością można zobaczyć, gdzie możemy ma tutaj. Mamy łańcuch! Z perspektywy z Menedżera zasobów klaster nie naprawdę mamy cztery usługi niezależnych, mamy wiele usług, które są powiązane (Service1, klienta2 i Service3) oraz takie, które jest wyłączona samodzielnie.

![Usług bilansowania razem][Image4]

Jest to możliwe, że brak równowagi w Metric1 może spowodować replik lub wystąpienia należące do Service3 do poruszania się po. Zazwyczaj przesunięcia te są bardzo ograniczone, ale może być większy w zależności od dokładnie tak jak imbalanced Metric1 masz i jakie zmiany były konieczne w klastrze, aby poprawić. Firma Microsoft może również przy użyciu pewność, że brak równowagi w metryki 1, 2 lub 3 nigdy nie spowoduje ruchów w Service4 — od przenoszenia repliki może być żaden punkt lub wystąpienia należące do Service4 wokół mogą nic naprawdę wpływu saldo metryki 1, 2 lub 3.

Menedżer zasobów klaster danych liczbowych automatycznie się, jakie usługi są powiązane, ponieważ usługi został dodany, usunięciu lub była zmiana ich konfiguracji metryczne — na przykład, między dwiema działa równoważenia klienta2 może zostały skonfigurowane ponownie i usuwanie Metric2. Spowoduje to przerwanie łańcuch między Service1 i klienta2. Zamiast dwóch grup usługi, dostępne są trzy:

![Usług bilansowania razem][Image5]

## <a name="next-steps"></a>Następne kroki
- Metryki przedstawiono, jak Menedżer zasobów usługi tkaninie klaster zarządza zużycie i pojemnością w klastrze. Aby dowiedzieć się więcej na temat ich i sposobie ich konfigurowania zapoznaj się z [tego artykułu](service-fabric-cluster-resource-manager-metrics.md)
- Koszt przepływu jest jednym ze sposobów sygnalizacji do Menedżera zasobów klaster, że niektórych usług są bardziej drogich przenieść niż inne. Aby dowiedzieć się więcej o koszt ruchu, należy zapoznać się z [tego artykułu](service-fabric-cluster-resource-manager-movement-cost.md)
- Menedżer zasobów klaster zawiera kilka ograniczenia, które można konfigurować w celu spowolnić pochodząca w klastrze. Nie są one wymaga zwykle, ale w razie potrzeby można dowiedzieć się o ich [w tym miejscu](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
