<properties
    pageTitle="Lista wydajność kwerend w partii Azure | Microsoft Azure"
    description="Zwiększanie wydajności, filtrując zapytań żądające informacji na temat partii zasoby takie jak pul, zadania i zadania, a obliczenia węzły."
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

# <a name="query-the-azure-batch-service-efficiently"></a>Efektywne kwerendy usługi Azure partii

Tutaj możesz dowiedzieć się, jak zwiększyć wydajność aplikacji partii Azure przez zmniejszenie ilości danych, który został zwrócony przez usługę kwerend zadania, zadania i obliczyć węzły za pomocą [.NET partii] [ api_net] biblioteki.

Prawie wszystkie aplikacje partii muszą wykonać różnego rodzaju monitorowania lub innych operacji, która korzysta z usługi partii, często w regularnych odstępach. Na przykład aby określić, czy istnieją kolejce zadań pozostały w zadaniu, należy najpierw uzyskać danych na każde zadanie w zadaniu. Aby sprawdzić stan węzłów w puli użytkownika, zostanie wyświetlony danych w każdym węźle w puli. W tym artykule wyjaśniono, jak wykonywać takie kwerendy w najskuteczniejszą metodą.

## <a name="meet-the-detaillevel"></a>Spełniają DetailLevel

Produkcji partii aplikacji obiektów, takich jak zadania, zadania i węzły obliczeń można ponumerować w tysięcy. Gdy użytkownik zażąda informacji na temat tych zasobów, potencjalnie dużych ilości danych musi "krzyżowy sieci" w usłudze partii w aplikacji dla każdej kwerendy. Ograniczając liczbę elementów i typ informacji, który został zwrócony przez kwerendę, można zwiększyć szybkość zapytań i w związku z tym wydajność aplikacji.

Tej [Partii .NET] [ api_net] wstawkę kodu interfejsu API listy *każdego* zadania skojarzonego z zadaniem, wraz z *wszystkich* właściwości każdego zadania:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Wydajniejsza kwerendy listy, można wykonywać, stosując "poziom szczegółów" do zapytania. W tym celu dostarczania [ODATADetailLevel] [ odata] obiektu w celu [JobOperations.ListTasks] [ net_list_tasks] metody. Ten fragment zwraca tylko identyfikator, wiersza polecenia i właściwości informacji węzła obliczeń wykonanych zadań:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

W tym scenariuszu przykładzie, jeśli istnieją tysiące zadania, wyniki z drugiego zapytania zwykle zwracaną duża szybciej niż pierwszy wiersz. Więcej informacji o używaniu ODATADetailLevel, gdy lista elementów za pomocą interfejsu API .NET partii jest dołączany [poniżej](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Zdecydowanie zaleca się, że możesz *zawsze* podać obiektu ODATADetailLevel programu .NET listy interfejsu API zapewniające maksymalną wydajność i wydajność aplikacji. Określając poziom szczegółów, pomaga zmniejszyć czas reakcji usługi partii, poprawić wykorzystanie sieci i zminimalizować użycie pamięci przez aplikacje klienckie.

## <a name="filter-select-and-expand"></a>Filtrowanie, wybierz i rozwiń

[.NET partii] [ api_net] i [Zatrzymaj partii] [ api_rest] interfejsy API umożliwiają zmniejszyć liczbę elementów, które są zwracane na liście, a także ilość informacji, który został zwrócony dla każdej z nich. Można to zrobić, określając **Filtr**, **Wybierz**i **Rozwiń ciągów** podczas wykonywania kwerend listy.

### <a name="filter"></a>Filtr
Ciąg filtru jest wyrażenie, które ogranicza liczbę zwracanych elementów. Na przykład listy zadania uruchomione dla zadania lub listy tylko węzły obliczeń, które są gotowe do wykonywania zadań.

- Ciąg filtru składa się z jednego lub wielu wyrażeń z wyrażenie, które zawiera nazwę właściwości, operator i wartość. Właściwości, które mogą być określone są specyficzne dla każdego typu obiektu, który kwerenda, podobnie jak operatory, które są obsługiwane dla każdej właściwości.
- Wiele wyrażeń można łączyć przy użyciu operatorów logicznych `and` i `or`.
- W tym przykładzie filtrowanie list ciągów tylko do pracy "renderowania" zadań: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Wybierz pozycję
Wybierz pozycję ciąg ogranicza wartości właściwości, które są zwracane dla każdego elementu. Określ listę nazw właściwości, a wartości tylko tych właściwości są zwracane elementów w wynikach kwerendy.

- Wybierz pozycję ciąg składa się z rozdzielaną średnikami listę nazw właściwości. Możesz określić, czy jakieś właściwości dla typu obiektu, który jest kwerenda.
- Ten przykład ciąg wybierz Określa, że powinny być zwrócone tylko trzy wartości właściwości dla każdego zadania: `id, state, stateTransitionTime`.

### <a name="expand"></a>Rozwiń
Ciąg Rozwiń powoduje zmniejszenie liczby połączeń interfejsu API, które są wymagane do uzyskania określonych informacji. Gdy używasz ciąg rozwijania, można uzyskać więcej informacji na temat poszczególnych elementów przy użyciu jednego interfejsu API rozmowy. Zamiast pierwszego uzyskania na liście obiektów, następnie żąda informacji dla każdego elementu na liście umożliwia ciąg rozwiń uzyskać te same informacje w jednym połączenia interfejsu API. Mniej interfejsu API oznacza lepszą wydajność.

- Podobnie jak wybierz ciąg, ciąg rozwiń Określa, czy niektóre dane są uwzględniane w wynikach kwerendy listy.
- Ciąg rozwiń jest obsługiwana tylko w przypadku, gdy jest używana na liście zadań, harmonogramy zadań, zadań i pule. Obecnie ją obsługuje tylko informacje statystyczne.
- Gdy wszystkie właściwości są wymagane i nie wybierz podano ciągu, rozwiń ciągu, *należy* używać do pobierania informacje statystyczne. Jeśli ciąg wybierz jest używana do uzyskiwania podzbiór właściwości, następnie `stats` można określić w ciągu wybierz, a ciąg rozwiń nie musi być określony.
- W tym przykładzie rozwiń ciąg Określa, że informacje statystyczne powinny zostać zwrócone dla każdego elementu na liście: `stats`.

> [AZURE.NOTE] Podczas tworzenia dowolnego typu ciąg kwerendy trzy (filtrowanie, wybierz i rozwiń), należy się upewnić, że właściwość nazwy i litery odpowiadają ich odpowiedników elementów interfejsu API usługi REST. Na przykład podczas pracy z klasą.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) , podaj **Stan** zamiast **Województwo**, nawet jeśli właściwość .NET jest [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Zobacz w poniższych tabelach mapowania właściwości między .NET oraz interfejsy API pozostałych.

### <a name="rules-for-filter-select-and-expand-strings"></a>Zasady filtrowania, wybierz pozycję, a następnie rozwiń listę ciągów

- Nazwy właściwości w filtrze, wybierz pozycję, a następnie rozwiń listę ciągów powinien zostać wyświetlony tak, jak w [Pozostałych partii] [ api_rest] interfejsu API — nawet wtedy, gdy używasz [.NET partii] [ api_net] lub jeden z innych SDK partię.
- Wszystkie nazwy właściwości jest uwzględniana wielkość liter, ale wartości właściwości jest uwzględniana wielkość liter.
- Data/Godzina ciągów może mieć jedną z dwóch formatów i musi być poprzedzony z `DateTime`.

  - Przykład formatu W3C DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Przykład formatu RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Ciągi logiczne są `true` lub `false`.
- Jeśli określono nieprawidłowe właściwości lub operator, `400 (Bad Request)` spowoduje błąd.

## <a name="efficient-querying-in-batch-net"></a>Wydajność kwerend w partii .NET

W [Partii .NET] [ api_net] interfejsu API, [ODATADetailLevel] [ odata] klasa jest używana do dostarczania filtr wybierz, a następnie rozwiń ciągów do listy operacji. Klasa ODataDetailLevel zawiera trzy właściwości ciąg publicznego, które mogą być określone w konstruktorze lub ustawić bezpośrednio obiektu. Możesz następnie przekazać obiektu ODataDetailLevel jako parametr do różnych operacji listy, takie jak [ListPools][net_list_pools], [ListJobs][net_list_jobs]i [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Ograniczyć liczbę elementów, które są zwracane.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Określ wartości właściwości, które są zwracane z każdego elementu.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Pobierz dane dla wszystkich elementów w jeden interfejs API połączeń zamiast oddzielnych połączeń dla każdego elementu.

Poniższy fragment kodu używa interfejsu API .NET partii wydajność kwerendy usługi partii statystyki określonego zestawu pul. W tym scenariuszu użytkownik partii ma pul zarówno badań i produkcji. Test puli identyfikatorów są prefiksem "test", a puli produkcji identyfikatory są prefiksem "zlecenie". W wstawkę kodu *myBatchClient* jest prawidłowo zainicjowany wystąpienie klasy [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Wystąpienie [ODATADetailLevel] [ odata] skonfigurowanego z wybierz i rozwiń klauzule także mogą być przekazywane do odpowiedniej metody Get, takich jak [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), aby ograniczyć ilość danych, który został zwrócony.

## <a name="batch-rest-to-net-api-mappings"></a>Partia DOSTĘPNĄ do mapowania interfejsu API .NET

Nazwy właściwości w filtrze, wybierz pozycję, a następnie rozwiń listę ciągów *musi* odpowiadać ich odpowiedniki interfejsu API usługi REST, zarówno w polach Nazwa i wielkość liter. Poniższe tabele zawierają mapowań między odpowiedników .NET i interfejsu API usługi REST.

### <a name="mappings-for-filter-strings"></a>Mapowania ciągów filtru

- **Metody .NET listy**: każda z metod interfejsu API .NET w tej kolumnie akceptuje [ODATADetailLevel] [ odata] w postaci obiektu.
- **Żądania listy pozostałych**: strona każdego interfejsu API usługi REST połączone w tej kolumnie zawiera tabelę, która określa właściwości i operacji, które są dozwolone w ciągach *filtru* . Może być używana tych nazw właściwości i operacje podczas konstruowania [ODATADetailLevel.FilterClause] [ odata_filter] ciąg.

| Metody listy .NET | Żądania listy usługi REST |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Lista certyfikatów konta][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Lista plików skojarzonego z zadaniem][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Wyświetl stan zadania przygotowanie i wersji zadania dla zadania][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Lista zadań dla konta][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Lista plików w węźle][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Lista zadania związane z zadaniem][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Harmonogramy zadań przy użyciu konta z listy][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Lista zadań skojarzonych z planowania zadań][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Lista węzłów obliczeń w puli][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Lista pul konta][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Mapowania ciągów select

- **Typy .NET partii**: typy interfejsu API .NET partię.
- **Jednostki interfejsu API usługi REST**: każdej strony w tej kolumnie zawiera jedną lub więcej tabel zawierających listę nazw właściwości interfejsu API usługi REST dla danego typu. Te nazwy właściwości są używane podczas tworzenia ciągów *Wybierz* . Podczas konstruowania [ODATADetailLevel.SelectClause] użyje tych samych nazw właściwość[ odata_select] ciąg.

| Typy .NET partii | Jednostki interfejsu API usługi REST |
|---|---|
| [Certyfikat][net_cert] | [Uzyskiwanie informacji na temat certyfikatu][rest_get_cert] |
| [CloudJob][net_job] | [Uzyskiwanie informacji o zadaniu][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Uzyskiwanie informacji na temat planowania zadań][rest_get_schedule] |
| [ComputeNode][net_node] | [Uzyskiwanie informacji na temat węzeł][rest_get_node] |
| [CloudPool][net_pool] | [Uzyskiwanie informacji na temat puli][rest_get_pool] |
| [CloudTask][net_task] | [Uzyskiwanie informacji o zadaniu][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Przykład: skonstruować ciąg filtru

Gdy skonstruować ciąg filtru dla [ODATADetailLevel.FilterClause][odata_filter], można znaleźć w powyższej tabeli w obszarze "Mapowania ciągów filtr" Znajdź interfejsu API usługi REST dokumentacji stronę, która odpowiada operację listy, którą chcesz wykonać. W pierwszej tabeli multirow na tej stronie znajdziesz podatne właściwości i ich obsługiwane operatory. Jeśli chcesz pobrać wszystkimi zadaniami, do którego kod wyjścia została niezerową, na przykład wiersz na [liście zadań skojarzonego z zadaniem] [ rest_list_tasks] Określa ciąg stosownych właściwości i operatory dozwolony:

| Właściwość | Dozwolone operacje | Typ |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

W związku z tym będzie ciąg filtru do wyświetlania listy wszystkich zadań z kodem niezerową zakończenia:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Przykład: skonstruować ciąg select

Aby utworzyć [ODATADetailLevel.SelectClause][odata_select], można znaleźć w powyższej tabeli w obszarze "Mapowania ciągów wybierz pozycję" i przejdź do strony interfejsu API usługi REST odpowiadający typ obiektu, które są pozycje. W pierwszej tabeli multirow na tej stronie znajdziesz możliwe do wybrania właściwości i ich obsługiwane operatory. Jeśli chcesz pobrać tylko identyfikator i wiersza polecenia dla każdego zadania na liście, na przykład, znajdziesz te wiersze w tabeli dotyczy na [Uzyskiwanie informacji o zadaniu][rest_get_task]:

| Właściwość | Typ | Notatki |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Następnie będzie ciąg wybierz tylko identyfikator i wiersza polecenia z każdej listy zadań, między innymi:

`id, commandLine`

## <a name="code-samples"></a>Przykłady kodu

### <a name="efficient-list-queries-code-sample"></a>Przykładowy kod listy wydajność kwerend

Zapoznaj się z [EfficientListQueries] [ efficient_query_sample] przykładowy projekt na GitHub, aby zobaczyć, jak wydajność kwerend listy może mieć wpływ na wydajność w aplikacji. Ta aplikacja konsoli C# tworzy i dodaje dużą liczbą zadań do zadania. Następnie ułatwia wielu wywołania [JobOperations.ListTasks] [ net_list_tasks] metody i przechodzi [ODATADetailLevel] [ odata] obiektów, które zostały skonfigurowane dla wartości nieruchomości różne zmiany ilości danych, które mają zostać zwrócone. Daje wynik podobny do następującego:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Jak pokazano na czas, można znacznie zmniejszyć czas reakcji kwerendy, ograniczając właściwości i liczbę elementów, które są zwracane. To i innych projektów przykładowe można znaleźć w [azure partii prób] [ github_samples] repozytorium na GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>Przykładowa Biblioteka i kod BatchMetrics

Oprócz powyższych przykładowy kod EfficientListQueries można znaleźć [BatchMetrics] [ batch_metrics] projektu w [azure partii prób] [ github_samples] GitHub repozytorium. Przykładowy projekt BatchMetrics pokazuje, jak efektywne monitorowanie postępu zadania partii Azure za pomocą interfejsu API partię.

[BatchMetrics] [ batch_metrics] próbki zawiera projektu biblioteki klas .NET, który można dołączyć do własnych projektów i prosty program wiersza polecenia do wykonywania i przedstawimy sposób korzystania z biblioteki.

Aplikacja przykładowa w projekcie prezentuje następujące operacje:

1. Wybieranie określonych atrybutów do pobrania właściwości, które są potrzebne
2. Filtrowanie czasu przejścia stanu w celu Pobierz tylko zmiany od ostatniej kwerendy

Na przykład poniższa metoda jest wyświetlana w bibliotece BatchMetrics. Zwraca ODATADetailLevel, która określa, że tylko `id` i `state` właściwości należy uzyskać obiekty, które będą proszeni. Protokół również określa, że tylko jednostki, w których stan zmienił się od określonej `DateTime` powinny zostać zwrócone parametru.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Następne kroki

### <a name="parallel-node-tasks"></a>Węzeł równoległe zadania

[Maksymalizowanie partii Azure obliczyć użycie zasobu z zadania równoczesne węzeł](batch-parallel-node-tasks.md) jest inny artykuł związanych z wydajnością partię. Niektóre typy obciążenia mogą korzystać z wykonywania zadań równoległych na większych — ale mniej — obliczenia węzły. Zapoznaj się z tego [przykładu](batch-parallel-node-tasks.md#example-scenario) w artykule szczegółowe informacje na temat takiej sytuacji.

### <a name="batch-forum"></a>Forum partii

[Forum partii Azure] [ forum] w witrynie MSDN to dobre miejsce do omówienia partii oraz zadawać pytania dotyczące usługi. Drugi na nad dla pomocne wpisy "lepkich" i Publikuj pytania, gdy występują one podczas tworzenia rozwiązanie partię.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
