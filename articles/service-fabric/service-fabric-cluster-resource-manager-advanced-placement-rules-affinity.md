<properties
   pageTitle="Menedżer zasobów usługi tkaninie klaster — koligacji | Microsoft Azure"
   description="Omówienie konfigurowania koligacji dla usług tkaninie usługi"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurowanie i używanie usługi koligacji w tkaninie usługi

Koligacja jest formantem podających głównie ułatwiające ułatwić przejście większych aplikacji wbudowanymi w chmurze i microservices świecie. Który te go można również w niektórych przypadkach jako wiarygodnych optymalizacji dotyczące przyspieszania usług, ale to może mieć wpływ po stronie.

Załóżmy, że w przypadku wprowadzenia większych aplikacji lub stronę, którą właśnie nie został zaprojektowany z microservices Pamiętaj, aby tkaninie usługi. To przejście jest faktycznie popularne, a firma Microsoft przeglądały kilku klientów (wewnętrznych i zewnętrznych) w takiej sytuacji. Należy zacząć od podnoszenie całej aplikacji w środowisku, to spakowany i pracy w górę. Następnie możesz rozpocząć podziału w różnych usługach mniejszy, że wszystkie porozmawiać ze sobą.

Następnie jest "Oops...". "Oops" mieści się zazwyczaj w jednej z tych kategorii:

1. Niektóre części X w aplikacji wbudowanymi miał zależność nieudokumentowanych na składnik Y, a firma Microsoft włączone tylko te do poszczególnych usług. Ponieważ te są uruchomione na różnych węzłach w klastrze, są one przerwane.
2.  Kwestie, o których komunikowanie się za pośrednictwem (lokalny nazwanych potoków | udostępnione pamięci | pliki na dysku), ale naprawdę trzeba móc je zaktualizować niezależne aby odrobinę nieco. Będzie można usunąć słabo zależności w późniejszym czasie.
3.  Wszystko działa poprawnie, ale okaże się, czy te dwa składniki są faktyczną bardzo chatty i wydajności poufne. Jeśli są przenoszone do poszczególnych usług ogólnej wydajności aplikacji tanked lub opóźnienia zwiększona. W wyniku ogólnego aplikacji nie spełnia oczekiwań.

W takich przypadkach możemy zachowywanie refaktoryzacji pracę i nie chcesz wrócić do monolityczna, ale jest potrzebna niektórych sens lokalizacji. Pozostanie aż możemy projektowania składników w sposób naturalny pracy usług lub do momentu firma Microsoft rozwiązuje wydajności oczekiwań w inny sposób, jeśli to możliwe.

Co należy zrobić? Dobrze spróbuj Włączenie koligacji.

## <a name="how-to-configure-affinity"></a>Jak skonfigurować koligacji
Aby skonfigurować koligacji, należy zdefiniować koligacji powiązanie dwóch różnych usługach. Można traktować koligacji jako "wskazującego" jedną usługę innej i informacją "tej usługi można uruchamiać tylko miejsce, w którym działa usługa." Czasami nosi koligacji relacji nadrzędny podrzędny (gdzie wskazaniu podrzędne nadrzędny). Koligacja zapewnia replik lub wystąpień jednej usługi są umieszczane na tym samym węzłach jako repliki lub innego wystąpienia.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Koligacja różnych opcji
Koligacja jest reprezentowany przez jeden z kilku schematów korelacji i ma dwa różne tryby. Zwykle funkcja koligacja jest to, co nazywamy NonAlignedAffinity. W NonAlignedAffinity replik lub wystąpienia różnych usług są umieszczane na tym samym węzły. Inne tryb jest AlignedAffinity. Wyrównane koligacji przydaje się tylko z usługami stanowe. Konfigurowanie dwóch stanowe usług do jest wyrównana koligacji gwarantuje, że kolory podstawowe tych usług są umieszczane na węzły siebie. Powoduje także każdej pary pomocnicze do umieszczenia w tym samym węzły tych usług. Istnieje także możliwość (jeśli są mniej typowe) do konfigurowania NonAlignedAffinity dla pełnowartościową usług. Dla NonAlignedAffinity różnych replikach dwóch usług stanowe czy collocated na tym samym węzły, ale nie będzie podejmowana próba wyrównywanie ich kolory podstawowe lub pomocnicze.

![Tryby koligacji i ich efekty][Image1]

### <a name="best-effort-desired-state"></a>Najlepsze stan nakładu potrzeby
Istnieje kilka różnic między koligacji i architektury wbudowanymi. Wiele z nich są, ponieważ relacją koligacji to optymalne rozwiązanie. Usługi w relacji koligacji są różni się jednostki, które może się nie powieść i ich. Dostępne są także przyczyny dla dlaczego może przerwać relacją koligacji. Na przykład zdolności ograniczenia miejsce, w którym można tylko część obiektów usługi w relacji koligacji mieści się na danym węźle. W tych przypadkach mimo że istnieje relacja koligacji, w miejscu jej nie można wymusić ze względu na innych ograniczeń. Jeśli jest to możliwe wymusić wszystkie ograniczenia i koligacji w późniejszym czasie naruszenie ograniczenia koligacji będzie poprawiany automatycznie.  

### <a name="chains-vs-stars"></a>Łańcuchów a gwiazdek
Obecnie nie możemy łańcuchów modelu koligacji relacji. Co to jest usługa, która jest podrzędnego w jedną relację koligacja nie może być element nadrzędny w innej relacji koligacji. Jeśli chcesz modelować relacji tego typu, masz skuteczne modelu go jako gwiazdy zamiast łańcuch. W tym celu najniższej podrzędne może być elementem nadrzędnym do "drugie imię" podrzędne nadrzędne zamiast tego. W zależności od rozmieszczenie usług może to wymagać Tworzenie usługi "symbol zastępczy" służyć jako nadrzędne dla wielu elementów podrzędnych.

![Łańcuchów a gwiazdek w kontekście koligacji relacji][Image2]

Niekiedy uwagi na temat relacji koligacji dzisiaj jest to kierunkowa. Oznacza to, że reguły "koligacja" wymusza tylko podrzędne to miejsce, w którym jest element nadrzędny. Jeśli na przykład nadrzędny nieoczekiwanie kończy się niepowodzeniem na inny węzeł następnie Menedżera zasobów klaster nie faktycznie uważa zawiera coś, co problem do momentu powiadomienia, że podrzędne nie znajduje się z element nadrzędny; relacji nie są od razu wymuszane.

### <a name="partitioning-support"></a>Obsługa podziału
Ostateczne kroki, aby uwagi dotyczące koligacja jest tego koligacji, które relacje nie są obsługiwane w miejsce, w którym jest podzielona nadrzędnej. Jest to firma Microsoft może obsługiwać po pewnym czasie, ale obecnie nie jest dozwolone.

## <a name="next-steps"></a>Następne kroki
- Więcej informacji na temat innych opcji dostępnych do skonfigurowania usług wyewidencjonowanie temat z konfiguracji Menedżera zasobów klaster dostępne [informacje na temat konfigurowania usług](service-fabric-cluster-resource-manager-configure-services.md)
- Wiele przyczyn, dla których użytkownicy korzystają z koligacje, takich jak ograniczenia usług do małych Ustaw maszyn i próby agregowanie Załaduj zbiór usług, są lepiej obsługiwane za pośrednictwem grup aplikacji. Zapoznaj się z [Grupy aplikacji](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
