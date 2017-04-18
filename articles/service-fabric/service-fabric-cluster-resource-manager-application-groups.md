<properties
   pageTitle="Menedżer zasobów usługi tkaninie klaster — grupy aplikacji | Microsoft Azure"
   description="Omówienie funkcji grupy aplikacji w usługi tkaninie klaster Menedżera zasobów"
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

# <a name="introduction-to-application-groups"></a>Wprowadzenie do grup aplikacji
Menedżer zasobów klaster usługi tkaninie zazwyczaj zarządza zasoby klastrów przez rozłożenie obciążenia (reprezentowane przez metryki) równomiernie w grupie. Usługa tkaninie zarządza także możliwości węzłów na klaster i klaster jako całość za pośrednictwem pojęcia pojemności. Jest to możliwe świetnie dla wielu różnych typów obciążenia, ale wzorców składających się na użycie różnych wystąpień aplikacji tkaninie usługi czasami przybliżenie dodatkowe wymagania. Kilka dodatkowych wymagań są zwykle:

- Możliwość rezerwowanie wydajność usługi wystąpienie aplikacji na niektórych liczby węzłów
- Możliwość ograniczania całkowitej liczby węzłów, które może być uruchamiane określony zestaw usług aplikacji
- Definiowanie możliwości na aplikację się w celu ograniczenia liczby lub zużycie zasobów sumy usług w jego obrębie

W celu spełnienia tych wymagań, opracowywania obsługę to, co nazywamy grupy aplikacji.

## <a name="managing-application-capacity"></a>Zarządzanie możliwości aplikacji
Wydajność aplikacji umożliwia ograniczanie liczby węzłów objęte aplikacji, a także całkowite obciążenie metryczne tego wystąpienia aplikacji na poszczególnych węzłach. Można również umożliwia za rezerwowanie zasobów w klastrze aplikacji.

Wydajność aplikacji można ustawić dla nowych aplikacji, kiedy są tworzone; może on również aktualizowany dla istniejących aplikacji, które zostały utworzone bez określania możliwości aplikacji.

### <a name="limiting-the-maximum-number-of-nodes"></a>Ograniczanie maksymalnej liczby węzłów
Najłatwiejszym przypadków użycia zdolności aplikacji jest w przypadku wystąpienia aplikacji musi być ograniczone tylko do niektórych maksymalną liczbę węzłów. Jeśli określono nie możliwości aplikacji Menedżera zasobów usługi tkaninie klaster będzie wystąpienia repliki regułami normalnego (równoważenia lub defragmentacji), co zwykle oznacza, że jego usługi zostaną rozmieszczone na wszystkich dostępnych węzłach w klastrze, czy defragmentacja jest włączona w niektórych dowolnego, ale mniejsze liczby węzłów.

Poniższa ilustracja przedstawia potencjalne położenie wystąpienie aplikacji bez maksymalnej liczby węzłów zdefiniowane i Ustaw następnie samej aplikacji z maksymalną liczbę węzłów. Zauważ, że jest nie gwarancji o które repliki lub wystąpienia, w których usług będzie uzyskiwanie umieszczony razem.

![Definiowanie maksymalną liczbę węzłów wystąpienia aplikacji][Image1]

W tym przykładzie po lewej stronie aplikacji nie ma możliwości aplikacji ustaw, a ma trzy usługi. CRM wprowadził logiczne decyzję rozciągnąć się wszystkie repliki sześć dostępne węzły w celu osiągnięcia równowagi zalecane w klastrze. W tym przykładzie prawo widzimy tej samej aplikacji, która jest ograniczona na trzy węzły i miejsce, w którym usługi tkaninie CRM osiągnął najlepszy stosunek replik aplikacji usług.

Parametr, który steruje to zachowanie jest nazywany MaximumNodes. Ten parametr można ustawić podczas tworzenia aplikacji lub aktualizacja dla aplikacji wystąpienie, które jest już uruchomiona, w której przypadku CRM tkaninie usługi będzie ograniczyć repliki aplikacji usług zdefiniowana maksymalna liczba węzłów.

Programu PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Metryki aplikacji, obciążenia i wydajność
Grupy aplikacji umożliwia również definiowanie metryki skojarzone z wystąpieniem danej aplikacji, a także możliwości stosowania w odniesieniu do tych metryki. Aby na przykład można zdefiniować którego zgodnie z wielu usług zgodnie z oczekiwaniami może zostać utworzony w

Dla każdego metryki istnieją wartości 2, które można ustawić w celu opisania możliwości dla danego wystąpienia aplikacji:

-   Suma aplikacji wydajność — reprezentuje pojemność wniosku o określonej metryki. Usługa tkaninie CRM próbuje ograniczyć suma obciążeń metryczne tej aplikacji usług do określonej wartości; Ponadto jeśli usługi aplikacji są już przez inne obciążenia maksymalnie ten limit, Menedżer zasobów klaster tkaninie usługi hasł tworzenia nowych usług lub partycje, które może spowodować całkowite obciążenie przekroczy ten limit.
-   Maksymalna liczba wydajności węzła — określa maksymalną obciążenie sumy replik aplikacji usług w jednym węźle. Jeśli całkowite obciążenie w węźle omówiono tej pojemności, CRM tkaninie usługi próbuje przenieść repliki do innych węzłów, tak aby ograniczenia możliwości jest zachowane.

## <a name="reserving-capacity"></a>Rezerwowanie zdolności
Inne typowe zastosowanie dla grup aplikacji jest upewnij się, że zasoby w klastrze są zarezerwowane dla wystąpienia danej aplikacji, nawet w przypadku wystąpienia aplikacji nie ma jeszcze usługi znajdujące się w nim lub nawet wtedy, gdy ta osoba nie są jeszcze używające zasobów. Spójrzmy na jak, w której będzie działać.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Określanie minimalny liczby węzłów i rezerwacji zasobów
Rezerwowania zasobów dla wystąpienie aplikacji wymaga, określając kilka dodatkowych parametrów: *MinimumNodes* i *NodeReservationCapacity*

- MinimumNodes — tak jak określanie docelowej maksymalnej liczby węzłów, które usługi aplikacji może zostać uruchomiony, możesz także określić minimalną liczbę węzłów aplikacji powinna działać w systemie. To ustawienie określa skuteczne liczbę węzłów na można zarezerwować zasobów, co najmniej gwarantującego zdolności w klastrze jako część tworzenia aplikację.
- NodeReservationCapacity - NodeReservationCapacity można zdefiniować dla każdego metryki w tej aplikacji. Definiuje ilość ładowania metryczne zastrzeżone dla aplikacji na dowolnym węźle rozmieszczenie wszystkie repliki lub wystąpień usług znajdujące się w nim.

Spójrzmy na przykład rezerwacji możliwości:

![Definiowanie zastrzeżone zdolności wystąpień aplikacji][Image2]

W tym przykładzie po lewej stronie aplikacji nie ma wszelkie możliwości aplikacji zdefiniowane. Menedżer zasobów klaster tkaninie usługi będzie saldo repliki usług podrzędne aplikacji i wystąpienia razem z innymi usługami (poza aplikacji) do zapewnienia równowagi w klastrze.

W tym przykładzie po prawej stronie Załóżmy, że aplikacja została utworzona z MinimumNodes ustawiona na wartość 2, MaximumNodes ustawionym na wartość 3 i aplikacja metryki zdefiniowane za pomocą zastrzeżenia 20, pojemności w węźle 50 i pojemności sumy aplikacji 100, usługi będzie rezerwowanie zdolności na dwóch węzłów na podstawie niebieski i nie jest możliwe innymi replikami w klastrze używają pojemności. Tej pojemności zastrzeżone aplikacji jest uważany zużyte i zliczone przed pozostałą pojemność klaster.

Po utworzeniu aplikacji z rezerwacji Menedżera zasobów klaster będzie rezerwowanie zdolności równej MinimumNodes * NodeReservationCapacity w grupie, ale nie zarezerwować zdolności na określonych węzłów, aż repliki usługi aplikacji są tworzone i umieszczony. Dzięki temu elastyczność, ponieważ węzły zostały wybrane dla nowych replik tylko po ich utworzeniu. Wydajność jest zastrzeżona w określonym węźle, gdy co najmniej jednej replice jest umieszczony nad nim.

## <a name="obtaining-the-application-load-information"></a>Uzyskiwanie informacji o ładowania aplikacji
Dla każdej aplikacji ma pojemność aplikacji zdefiniowania możesz można uzyskać informacje na temat agregacji Załaduj zgłaszane przez repliki jego usług. Usługa tkaninie zawiera kwerend programu PowerShell i zarządzanego interfejsu API do tego celu.

Na przykład obciążenia mogą być pobierane przy użyciu następującego polecenia cmdlet programu PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Wynik kwerendy będzie zawierać podstawowe informacje o możliwości aplikacji, określone dla aplikacji, takiej jak węzłów minimalne i maksymalne węzłów. Będzie również informacji na temat liczby węzłów obecnie używane w aplikacji. W związku z tym dla każdego metryki ładowania będzie informacji o:
- : Metryka Nazwa metryki.
-   Wydajność rezerwacji: Wydajność klaster zarezerwowane w klastrze dla tej aplikacji.
-   Ładowanie aplikacji: Całkowite obciążenie replik podrzędne tej aplikacji.
-   Pojemność aplikacji: Maksymalna dozwolona wartość obciążenia aplikacji.

## <a name="removing-application-capacity"></a>Usuwanie możliwości aplikacji
Po ustawieniu parametrów przepustowości aplikacji dla aplikacji, można je usunąć za pomocą polecenia cmdlet programu PowerShell lub aktualizacja aplikacji API. Na przykład:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Tego polecenia spowoduje usunięcie wszystkich parametrów przepustowości aplikacji z poziomu aplikacji, a Menedżer zasobów klaster tkaninie usługi rozpocznie się traktowanie jako dowolnej innej aplikacji w klastrze, który nie ma tych parametrów zdefiniowanych tej aplikacji. Polecenie powoduje natychmiastowy i klaster Menedżera zasobów spowoduje usunięcie wszystkich parametrów aplikacji przepustowości dla tej aplikacji; Określanie je ponownie wymagają aktualizacji aplikacji API zwany z odpowiednimi parametrami.

## <a name="restrictions-on-application-capacity"></a>Ograniczenia dotyczące możliwości aplikacji
Istnieje kilka ograniczeń parametrów przepustowości aplikacji, które muszą zostać zachowane. W przypadku wystąpienia błędów sprawdzania poprawności Tworzenie lub aktualizowanie aplikacji zakończy się komunikat o błędzie.
Wszystkie parametry całkowitą muszą być liczbami nieujemna.
Ponadto dla poszczególnych parametrów ograniczenia są następujące:

-   MinimumNodes nigdy nie może być większa niż MaximumNodes.
-   Jeśli zdefiniowano zdolności metryki obciążenia, są one musi wykonać następujące czynności:
  - Węzeł rezerwacji pojemność nie może być większa niż maksymalna pojemność węzeł. Nie można na przykład ograniczyć możliwości dla metryki "Procesora" w węźle 2 jednostki i zarezerwuj jednostki 3 w każdym węźle.
  - Jeśli określono MaximumNodes, następnie iloczynu MaximumNodes i maksymalnej pojemności węzeł nie może być większa niż pojemność aplikacji. Na przykład jeśli ustawisz maksymalną wydajności węzła dla metryki obciążenia, które "CPU" do 8, a wartość maksymalna liczba węzłów 10, to pojemność aplikacji musi być większa niż 80 dla tej metryki ładowania.

Ograniczenia są wymuszane, zarówno podczas tworzenia aplikacji (po stronie klienta), jak i podczas aktualizacji aplikacji (po stronie serwera). Podczas tworzenia, jest to przykład wyczyść naruszenia wymagań od MaximumNodes < MinimumNodes i polecenie nie powiedzie się w kliencie przed żądanie nawet jest wysyłane do klastrów tkaninie usługi:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Przykład Nieprawidłowa aktualizacja wygląda następująco: Jeśli firma Microsoft zrobić istniejącą aplikację i aktualizowanie maksymalna liczba węzłów na jakieś wartości, przejdzie aktualizacji:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Po wykonaniu tej firma Microsoft może próba aktualizacji węzły minimalne:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Klient nie ma za mało kontekstu dotyczące aplikacji, więc umożliwia aktualizację w celu przekazania do klastrów tkaninie usługi. Jednak w klastrze, usługa będzie sprawdzać poprawność nowego parametru razem z istniejących parametrów i zakończy się niepowodzeniem operacji aktualizacji, ponieważ węzły minimalna foe wartość jest większa niż wartość w polu Maksymalna liczba węzłów. W tym przypadku parametrów przepustowości aplikacji pozostaną niezmienione.

Ograniczenia te są umieszczane w miejscu w kolejności dla Menedżera zasobów klaster Aby udzielić najlepsze rozmieszczenie replik aplikacji usług.

## <a name="how-not-to-use-application-capacity"></a>Jak nie za pomocą aplikacji wydajność

-   Nie należy używać możliwości aplikacji do ograniczyć aplikacji do określonego podzbioru węzłów: mimo że tkaninie usługi będzie zapewnienia, że maksymalna liczba węzłów jest dla każdej aplikacji, która ma wydajności aplikacji określonej, użytkownicy nie mogą zadecydować węzły, które będą występować na. Można to osiągnąć przy użyciu położenie ograniczenia dla usług.
-   Nie należy używać możliwości aplikacji aby upewnić się, że dwie usługi z tej samej aplikacji, zawsze znajdować się obok siebie. Można to osiągnąć przy użyciu koligacji relacji między usługami i koligacji może być ograniczone tylko do usług, które mają zostać umieszczone faktycznie razem.

## <a name="next-steps"></a>Następne kroki
- Więcej informacji na temat innych opcji dostępnych do skonfigurowania usług wyewidencjonowanie temat z konfiguracji Menedżera zasobów klaster dostępne [informacje na temat konfigurowania usług](service-fabric-cluster-resource-manager-configure-services.md)
- Aby dowiedzieć się o jak Menedżer zasobów klaster zarządza i sald obciążenia w klastrze, zapoznaj się z artykułem na [równoważenia obciążenia](service-fabric-cluster-resource-manager-balancing.md)
- Rozpoczynanie od początku i [Uzyskaj wprowadzenie do usługi tkaninie klaster Menedżera zasobów](service-fabric-cluster-resource-manager-introduction.md)
- Aby uzyskać więcej informacji na temat działania metryki zazwyczaj poczytaj na [Metryki ładowania tkaninie usługi](service-fabric-cluster-resource-manager-metrics.md)
- Menedżer zasobów klaster zawiera wiele opcji opisu klaster. Aby dowiedzieć się więcej na temat ich zapoznaj się z niniejszego artykułu [opisem klastrze tkaninie usługi](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
