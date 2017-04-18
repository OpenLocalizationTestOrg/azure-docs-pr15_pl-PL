<properties
    pageTitle="Współzależności w partii Azure zadań | Microsoft Azure"
    description="Tworzenie zadań, które są zależne od pomyślnego zakończenia innych zadań związanych z przetwarzania styl MapReduce i podobne dane duży obciążeń pracą w partii Azure."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Współzależności zadań w partii Azure

Funkcja współzależności zadań partii Azure jest właściwie dobierane przetwarzania:

- Styl MapReduce obciążenia w chmurze.
- Zadania, których zadania przetwarzania danych można wyrazić jako wykres acykliczne bezpośrednie (AG).
- Każde inne zadanie, w którym podrzędne zadania zależą od tego wynik zadania nadrzędnego.

Współzależności zadań wsadowe umożliwiają tworzenie zadań zaplanowanych do wykonania w węzłach obliczeń dopiero po pomyślnym zakończeniu jedno lub więcej zadań. Na przykład można utworzyć zadanie, które są renderowane za pomocą każdej ramki filmu 3-w z osobnych, równoległe zadań. Ostatni zadań — "korespondencji seryjnej zadania" — scala renderowanych ramki do cały film tylko wtedy, gdy wszystkie ramki została uznana pomyślnie.

Można tworzyć zadania, które są zależne od innych zadań w relacji jeden lub jeden do wielu. Można nawet tworzyć współzależności zakresu, gdzie zadania zależy od pomyślnego zakończenia zadań w określonym zakresie identyfikatorów zadań grupy. Można połączyć te trzy podstawowe scenariusze w celu utworzenia relacji wiele do wielu.

## <a name="task-dependencies-with-batch-net"></a>Współzależności zadań przy użyciu partii .NET

W tym artykule omówimy, jak skonfigurować współzależności zadań przy użyciu [.NET partii] [ net_msdn] biblioteki. Najpierw pokażemy, jak [włączyć współzależności zadań](#enable-task-dependencies) na zadań, a następnie wykazać sposobu [konfigurowania zadań ze współzależnościami](#create-dependent-tasks). Na koniec omówimy [scenariusze współzależności](#dependency-scenarios) obsługuje partię.

## <a name="enable-task-dependencies"></a>Włączanie zależności między zadaniami

Aby użyć współzależności zadań w aplikacji partię, musisz najpierw wiadomo, usługę partii używanego współzależności zadań w zadaniu. W partii .NET włączyć ją w swojej [CloudJob] [ net_cloudjob] przez ustawienie jego [UsesTaskDependencies] [ net_usestaskdependencies] właściwości `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

W poprzednim wstawkę kodu, "batchClient" jest wystąpieniem [BatchClient] [ net_batchclient] zajęć.

## <a name="create-dependent-tasks"></a>Tworzenie zadania zależne

Aby utworzyć zadanie, które jest zależne od pomyślnego zakończenia jedno lub więcej zadań, możesz określić partii że zadanie "zależy od" innych zadań. W partii .NET Konfigurowanie [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] właściwości z wystąpieniem [TaskDependencies] [ net_taskdependencies] klasy:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Ten wstawkę kodu tworzy zadanie z Identyfikatorem "Kwiaty", które są planowane do uruchomienia w węźle obliczeń tylko wtedy, gdy zadań za pomocą identyfikatorów "Opady deszczu" i "N" została ukończona pomyślnie.

 > [AZURE.NOTE] Zadanie jest traktowany jako wykonane, gdy jest on w stanie **wykonane** i **Kod zakończenia** będzie `0`. W partii .NET, oznacza to, [CloudTask][net_cloudtask]. [Województwo] [net_taskstate] wartości właściwości `Completed` i CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] wartość właściwości jest `0`.

## <a name="dependency-scenarios"></a>Scenariusze współzależności

Istnieją trzy scenariusze współzależności podstawowe zadania, które są dostępne w partii Azure: jeden, jeden do wielu i identyfikator zadania zakresu zależności. Te mogą być połączone zapewnienie scenariuszu czwartym wiele do wielu.

 Scenariusz&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Przykład | |
 :-------------------: | ------------------- | -------------------
 [Jeden](#one-to-one) | *taskB* zależy od *taskA* <p/> *taskB* nie zostanie zaplanowane do wykonania, aż *taskA* zakończyło się pomyślnie | ![Diagram: współzależności zadań jeden][1]
 [Jeden do wielu](#one-to-many) | *taskC* zależy od zarówno *taskA* , jak i *taskB* <p/> *taskC* nie będzie można zaplanować wykonywanie, aż zarówno *taskA* , jak i *taskB* została ukończona pomyślnie | ![Diagram: współzależności zadań jeden do wielu][2]
 [Zakres identyfikator zadania](#task-id-range) | *taskD* zależy od zakresu zadań <p/> *taskD* nie będzie można zaplanować wykonywania, aż zadań za pomocą identyfikatorów od *1* do *10* została ukończona pomyślnie | ![Diagram: Współzależności identyfikator zakresu][3]

>[AZURE.TIP] Możesz utworzyć relacji **wiele do wielu** , takich jak miejsce, w którym zadania C, D, E i F każdego zależne od zadań A i B. Jest to przydatne, na przykład w parallelized scenariusze wstępnego przetwarzania miejsce, w którym podrzędne zadań zależy od wyniku wielu zadań nadrzędnego.

### <a name="one-to-one"></a>Jeden

Aby utworzyć zadanie, które ma zależność od pomyślnego zakończenia innych zadań, podać nazwę pojedynczego zadania, aby [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] metody statycznej podczas wypełniania [DependsOn] [ net_dependson] właściwości [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Jeden do wielu

Aby utworzyć zadanie, które ma zależność od pomyślnego zakończenia wielu zadań, podać zbioru zadań identyfikatorów do [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] metody statycznej podczas wypełniania [DependsOn] [ net_dependson] właściwości [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Zakres identyfikator zadania

Aby utworzyć zadanie, które ma zależność od pomyślnego zakończenia grupy zadań, których znajdują się identyfikatory w zakresie, podać pierwszej i ostatniej zadań identyfikatorów w zakresie, aby [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] metody statycznej podczas wypełniania [DependsOn] [ net_dependson] właściwości [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Użycie zakresy identyfikatorów zadań dla swojego zależności, identyfikatorów zadań w zakresie *musi* być ciąg reprezentacji wartości całkowitych. Ponadto każdego zadania w zakresie musi zostać pomyślnie ukończona zależne na być zaplanowane do wykonania zadania.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Przykładowy kod

[TaskDependencies] [ github_taskdependencies] przykładowy projekt jest jednym z [Przykłady kodu partii Azure] [ github_samples] na GitHub. Tego rozwiązania programu Visual Studio 2015 pokazuje, jak włączyć współzależności zadań dla zadania, tworzenie zadań, które są zależne od innych zadań i wykonanie tych zadań w puli węzłów obliczeń.

## <a name="next-steps"></a>Następne kroki

### <a name="application-deployment"></a>Wdrażanie aplikacji

Funkcja [pakietów aplikacji](batch-application-packages.md) partii zapewnia łatwy sposób zarówno wdrażanie i wersji aplikacji, które zadania wykonywane na obliczanie węzły.

### <a name="installing-applications-and-staging-data"></a>Instalowanie aplikacji i danych przemieszczenia

Zapoznaj się z [instalowania aplikacji i przemieszczenia danych na partię obliczyć węzły] [ forum_post] opublikować na forum wsadowe Azure omówiono różne metody, aby przygotować węzły do wykonywania zadań. Zapisane przez jednego z członków zespołu partii Azure, ten wpis to dobre Elementarz na różne sposoby pobierania plików (w tym aplikacji i wprowadzania danych zadań) na węzły obliczeń.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: zależność jeden"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: współzależności jeden do wielu"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: współzależności identyfikator zakresu"
