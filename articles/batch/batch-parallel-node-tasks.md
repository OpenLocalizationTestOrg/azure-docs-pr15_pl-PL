<properties
    pageTitle="Maksymalizowanie partii węzeł korzystanie z zadaniami równoległe | Microsoft Azure"
    description="Zwiększyć wydajność i niższe koszty przy użyciu mniejszej liczby węzłów obliczeń i uruchamianie równoczesne zadań w każdym węźle puli partii Azure"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Maksymalizowanie użycie zasobu wsadowe Azure obliczeń z zadaniami węzeł równoczesne

Uruchamiając więcej niż jednego zadania jednocześnie w każdym węźle obliczeń w puli usługi Azure partię, można zmaksymalizować zużycie mniejsze liczby węzłów w puli zasobów. Dla niektórych obciążenia to może spowodować krótszy czas zadania i niższe koszty.

Podczas niektórych scenariuszach dotyczących korzystania z przypisanie wszystkie zasoby węzła pojedynczego zadania, wielu sytuacjach korzystać z oferujący wielu zadań udostępnić te zasoby:

 - **Minimalizowanie transfer danych** , gdy zadania są mogli udostępniać dane. W tym scenariuszu może znacznie zmniejszyć opłat za przesyłanie danych termicznego, kopiując udostępnionych danych do mniejszej liczby węzłów i wykonywania zadania równolegle w każdym węźle. Dotyczy to zwłaszcza jeśli dane do skopiowania na każdym węźle muszą zostać przeniesione między regionach geograficznych.

 - **Użycie pamięci Maximizing** podczas zadania wymagają dużej ilości pamięci, ale tylko okresach krótki czas, a następnie w czasie zmiennych podczas wykonywania. Można stosować węzły obliczeń mniej, ale większe, za pomocą więcej pamięci do efektywnej obsługi takich tych najwyższych wartościach. Te węzły woli wielu zadań działającego równolegle w każdym węźle, ale każdego zadania będzie korzystać węzłów dużej ilości pamięci w innym czasie.

 - Jeśli komunikacji między węzeł jest wymagane w puli **łagodzenia ograniczenia liczby węzłów** . Obecnie pul skonfigurowane dla komunikacji między węzeł są ograniczone do 50 węzły obliczeń. W przypadku mogą wykonywać zadania równolegle każdy węzeł w takich puli większej liczby zadania mogą być wykonywane jednocześnie.

 - **Replikacja lokalnego obliczyć klaster**, takich jak po pierwszym poruszeniu środowisku obliczeń Azure. Jeśli bieżący rozwiązania lokalnego wykonuje wielu zadań na węzeł obliczeń, można zwiększyć maksymalną liczbę zadań węzeł w celu lepszego lustrzane tej konfiguracji.

## <a name="example-scenario"></a>Przykładowy scenariusz

Na przykład aby przedstawić zalety wykonywanie zadań równoległych Załóżmy, że aplikacja zadania zawiera wymagania dotyczące Procesora i pamięci tak, aby [Standardowy\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) węzły są wystarczające. Jednak aby ukończyć to zadanie w czasie wymagane, 1000 węzły te są wymagane.

Zamiast używać standardowego\_węzły D1, które mają 1 core Procesora, można użyć [Standardowy\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) węzły, które mają 16 rdzenie i umożliwić wykonywanie zadań równoległych. W związku z tym można *16 godzin mniejszej liczby węzłów* — zamiast węzły 1000, tylko 63 mogą być wymagane. Ponadto jeśli aplikacji dużych plików lub danych źródłowych są wymagane dla każdego węzła, czas trwania zadania i efektywności ponownie zwiększona ponieważ dane są kopiowane do węzłów tylko 16.

## <a name="enable-parallel-task-execution"></a>Włącz wykonywanie zadań równoległych

Węzły obliczeń do wykonania zadania równoległe można skonfigurować na poziomie puli. Z biblioteką .NET partii Ustawianie [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] właściwości po utworzeniu puli. Jeśli korzystasz z interfejsu API usługi REST partię, ustaw [maxTasksPerNode] [ rest_addpool] elementu w treści wezwania podczas tworzenia puli.

Azure partii umożliwia ustawienie maksymalna zadania na węzeł maksymalnie cztery razy (4 x) liczby rdzeni węzeł. Na przykład jeśli puli skonfigurowano węzły rozmiaru "Duże" (cztery rdzenie), następnie `maxTasksPerNode` jest ustawiona na 16. Aby uzyskać szczegółowe informacje na liczby rdzeni dla każdego z rozmiarów węzeł zobacz [rozmiarów usług w chmurze](../cloud-services/cloud-services-sizes-specs.md). Aby uzyskać więcej informacji na ograniczenia usługi zobacz [przydziałów i limity dotyczące usługi Azure partię](batch-quota-limit.md).

> [AZURE.TIP] Należy wziąć pod uwagę `maxTasksPerNode` wartość utworzenia [formuły autoscale] [ enable_autoscaling] dla użytkownika. Na przykład formuła zwracająca `$RunningTasks` może być znacznie dotyczy wzrost zadania na węzeł. Aby uzyskać więcej informacji, zobacz [automatycznie skali obliczyć węzły w puli partii Azure](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Podział zadań

Węzły obliczeń w puli można jednocześnie wykonywania zadań, należy określenie sposobu wyświetlania zadań, które mają zostać rozłożone na węzły w puli.

Za pomocą [CloudPool.TaskSchedulingPolicy] [ task_schedule] właściwości, możesz określić że zadania mają być przydzielane równomiernie we wszystkich węzłach w puli ("rozpraszania"). Lub można określić, że tyle możliwie mają być przydzielane zadania do każdego węzła przed zadania przydzielone do innego węzła w puli ("opakowania").

Na przykład jak ta funkcja jest przydatna, należy rozważyć, czy puli [Standardowy\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) węzłów (w powyższym przykładzie), które skonfigurowano [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] wartość 16. Jeśli [CloudPool.TaskSchedulingPolicy] [ task_schedule] skonfigurowano [ComputeNodeFillType] [ fill_type] z *dodatkiem Service Pack*, czy maksymalizowanie zastosowania wszystkich rdzeni 16 każdego węzła i zezwolić na [autoscaling puli](batch-automatic-scaling.md) , Oczyść nieużywane węzły z puli (węzłach bez żadnych zadań przypisanych). Spowoduje to minimalizuje użycie zasobu i zapisuje pieniądze.

## <a name="batch-net-example"></a>Przykład .NET partii

Tej [Partii .NET] [ api_net] fragment kodu interfejsu API zawiera żądanie, aby utworzyć pulę, która zawiera cztery węzły dużych maksymalnie czterech zadania na węzeł. Określa zadania planowania zasad wypełniania każdego węzła z zadaniami przed przydzielania zadań do innego węzła w puli. Aby uzyskać więcej informacji o dodawaniu pul za pomocą interfejsu API .NET partię, zobacz [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Przykład pozostałych partii

[Umieść wsadowe] [ api_rest] wstawek interfejsu API zawiera żądanie, aby utworzyć pulę, która zawiera dwa węzły dużych maksymalnie czterech zadania na węzeł. Aby uzyskać więcej informacji o dodawaniu pul za pomocą interfejsu API usługi REST, zobacz [Dodawanie puli z klientem][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Można ustawić `maxTasksPerNode` element i [MaxTasksPerComputeNode] [ maxtasks_net] właściwość tylko w czasie tworzenia puli. Nie można zmienić po utworzeniu już puli.

## <a name="code-sample"></a>Przykładowy kod

[ParallelNodeTasks] [ parallel_tasks_sample] projektu na GitHub ilustruje użycie [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] właściwości.

Ta aplikacja konsoli C# używa [.NET partii] [ api_net] biblioteki na tworzenie puli jeden lub więcej węzłów obliczeń. Można konfigurować liczbę zadań wykonywania dla tych węzłów w celu zasymulowania ładowania zmiennych. Dane wyjściowe aplikacji określa, które węzły wykonywane każdego zadania. Aplikacja zawiera również podsumowanie parametrów zadania i czas trwania. Podsumowanie części Wyjście z dwóch cyklów przykładowej aplikacji zostanie wyświetlony poniżej.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Przy pierwszym wykonaniu przykładowej aplikacji wskazuje, że z jednego węzła w puli i ustawieniem domyślnym jedno zadanie do każdego węzła, czas trwania zadania jest ponad 30 minut.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Drugi uruchomić pokazano przykładowe znaczący spadek czasu trwania zadania. Jest to spowodowane puli został skonfigurowany z czterech zadania na węzeł, co umożliwia wykonywanie zadań równoległych do zakończenia zadania w prawie kwartale czasu.

> [AZURE.NOTE] Czas trwania zadania w podsumowaniach powyżej nie powinno obejmować godzinę utworzenia puli. Każdy z powyższych zadania zostały przesłane do poprzednio utworzone pul, którego węzły obliczeń były stanu *bezczynności* w momencie przesyłania.

## <a name="next-steps"></a>Następne kroki

### <a name="batch-explorer-heat-map"></a>Partia Eksploratora konturowy

[Eksplorator partii Azure][batch_explorer], jedna z partii Azure [przykładowych aplikacji][github_samples], zawiera funkcja *Konturowy* umożliwiająca wizualizacji wykonywanie zadań. Gdy już wykonywania [ParallelTasks] [ parallel_tasks_sample] przykładowej aplikacji umożliwia funkcję konturowy łatwo wizualizowanie wykonywaniu zadań równoległych w każdym węźle.

![Partia Eksploratora konturowy][1]

*Partia Eksploratora Wykres konturowy przedstawiający puli węzłów cztery z każdego węzła aktualnie wykonywanych cztery zadania*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
