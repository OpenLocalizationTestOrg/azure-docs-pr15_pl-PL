<properties
    pageTitle="Uruchamianie aplikacji MPI w partii Azure z wielu wystąpień zadania | Microsoft Azure"
    description="Dowiedz się, jak wykonać aplikacji wiadomości przechodzące Interface (MPI) przy użyciu wielu wystąpień typ zadania w partii Azure."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Uruchamianie aplikacji wiadomości przechodzące Interface (MPI) w partii Azure za pomocą wielu wystąpień zadania

Wielu wystąpień zadania umożliwia działać jednocześnie zadania partii Azure na wielu węzłach obliczeń. Te zadania włączyć wysoka obliczeniową scenariuszy, takich jak aplikacje wiadomości przechodzące Interface (MPI) w partii. W tym artykule opisano sposób wykonywanie wielu wystąpień zadań przy użyciu [.NET partii] [ api_net] biblioteki.

>[AZURE.NOTE] W przykładach w tym artykule skoncentrować się na partię .NET, MS-MPI i Windows obliczyć węzły, wielu wystąpień zadania omówione w tym miejscu są stosowane do innych platform i technologii (Python i MPI firmy Intel w węzłach Linux, na przykład).

## <a name="multi-instance-task-overview"></a>Omówienie zadań wielu wystąpień

W partii, każdego zadania jest zwykle wykonywana w węźle pojedynczy obliczeń — przesyłanie wielu zadań do zadania, a usługa partii planuje każdego zadania do wykonania w węźle. Jednak konfigurując zadania **Ustawienia wielu wystąpień**, możesz określić partii zamiast tworzyć jedno zadanie podstawowego i kilka podzadań, które następnie są wykonywane na wielu węzłach.

![Omówienie zadań wielu wystąpień][1]

Po przesłaniu zadania przy użyciu ustawień wielu wystąpień do zadania partii wykonuje kilka czynności unikatowe do wielu wystąpień zadań:

1. Usługa partii tworzy jeden **podstawowy** i kilka **podzadań** na podstawie wielu wystąpień ustawień. Całkowita liczba zadań (podstawowe oraz wszystkie podzadania) zastępuje liczbę **wystąpień** (obliczeń węzłach) określ ustawienia wielu wystąpień.
1. Partia wyznacza jednego z węzłów obliczeń jako **wzorca**i zaplanowanie podstawowego zadania do wykonania na wzorcu. Planowana podzadania do wykonania w pozostałych węzłach obliczeń przydzielonych do zadania wielu wystąpień jednego podzadania na węzeł.
1. Podstawowy i wszystkie podzadania Pobierz wszystkie **Typowe pliki zasobów** , które określone w ustawieniach wielu wystąpień.
1. Po typowych zasobów pobrano pliki, podstawowych i podzadań wykonywanie **polecenia można je zsynchronizować** określone w ustawieniach wielu wystąpień. Polecenie można je zsynchronizować zwykle jest używane do przygotowania węzły do wykonywania zadania. Dotyczy to także uruchamiania usług tła (takich jak [Microsoft MPI][msmpi_msdn]i `smpd.exe`) i weryfikowania, że węzły są gotowe do przetwarzania wiadomości między węzeł.
1. Podstawowe zadania wykonuje **polecenie aplikacji** na węzeł wzorca *po* pomyślnym zakończeniu polecenia można je zsynchronizować przez podstawowy i wszystkie podzadania. Polecenie aplikacji jest wiersza polecenia zadania wielu wystąpień się i są wykonywane tylko przez głównym zadaniem. W [MS-MPI][msmpi_msdn]-podstawie rozwiązanie, jest to miejsce, w którym wykonywanie za pomocą aplikacji obsługującej MPI `mpiexec.exe`.

> [AZURE.NOTE] Chociaż jest funkcjonalny odrębnych "zadanie wielu wystąpień" nie jest typ zadania unikatowe, takich jak [StartTask] [ net_starttask] lub [JobPreparationTask][net_jobprep]. Zadanie wielu wystąpień jest po prostu partii zadania standardowego ([CloudTask] [ net_task] w partii .NET) którego ustawienia wielu wystąpień zostały skonfigurowane. W tym artykule nosi to **wielu wystąpień zadania**.

## <a name="requirements-for-multi-instance-tasks"></a>Wymagania dotyczące wielu wystąpień zadania

Wielu wystąpień zadania wymagają puli z **komunikacji między węzeł włączone**i **Wykonywanie zadań jednocześnie wyłączone**. Jeśli próbujesz uruchomić wielu wystąpień zadania w puli z komunikacji między węzłami wyłączone lub z *maxTasksPerNode* wartość większą niż 1, zadanie nigdy nie zostaje zaplanowane — pozostaje czas nieokreślony "aktywny". Poniższym przykładzie pokazano tworzenie puli za pośrednictwem biblioteki .NET partię.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Ponadto wielu wystąpień zadania można wykonywać *tylko* w węzłach **pul utworzonych po 14 grudnia 2015 r**.

### <a name="use-a-starttask-to-install-mpi"></a>Należy zainstalować MPI StartTask

Aby uruchomić aplikacji MPI z wielu wystąpień zadania, najpierw należy zainstalować implementacja MPI (MS-MPI lub MPI firmy Intel, na przykład) w węzłach obliczeń w puli. Jest to dobre czasu ma być używany [StartTask][net_starttask], który wykonuje przy każdym węźle łączy puli lub uruchomieniu. Ten wstawkę kodu tworzy StartTask, określająca pakiet instalacyjny MS-MPI jako [plik zasobu][net_resourcefile]. Wiersz polecenia zadania Rozpoczęcie jest wykonywane po pobraniu pliku zasobu do węzła. W takim przypadku polecenie wykonuje instalację nienadzorowanego MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Zdalny bezpośredni dostęp do pamięci (RDMA)

Po wybraniu [RDMA dostępem do rozmiaru](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) , takich jak A9 węzły obliczeń w puli usługi partii, aplikacja MPI można korzystać z Azure w wysokiej wydajności i małych opóźnień zdalnego pamięci bezpośredni dostęp (RDMA) sieci.

Należy poszukać rozmiarów określony jako "RDMA stanie" w następujących artykułach:

* Pul **CloudServiceConfiguration**

  * [Rozmiary usług w chmurze](../cloud-services/cloud-services-sizes-specs.md) (Tylko w systemie Windows)

* Pul **VirtualMachineConfiguration**

  * [Rozmiary maszyn wirtualnych platformy Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux).

  * [Rozmiary maszyn wirtualnych platformy Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Aby móc skorzystać z RDMA na [Linux obliczyć węzły](batch-linux-nodes.md), należy użyć **MPI firmy Intel** w węzłach. Aby uzyskać więcej informacji na pul CloudServiceConfiguration i VirtualMachineConfiguration zobacz sekcję [puli](batch-api-basics.md#Pool) omówienie funkcji partii.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Tworzenie wielu wystąpień zadania program .NET partii

Teraz, gdy firma Microsoft zostały omówione wymagania puli i instalacja pakietu MPI, tworzenie wielu wystąpień zadania. W tym wstawek tworzymy standardowy [CloudTask][net_task], skonfiguruj jego [MultiInstanceSettings] [ net_multiinstance_prop] właściwości. Jak wspomniano wcześniej, wielu wystąpień zadanie nie jest typu różne zadania, ale skonfigurowany za pomocą ustawień wielu wystąpień zadania partii standardowego.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Podstawowe zadania i podzadania

Podczas tworzenia ustawień wielu wystąpień zadania określ liczbę węzłów obliczeń, które są do wykonania zadania. Po przesłaniu zadania, aby zadanie usługa partii tworzy jedno zadanie **podstawowego** i za mało **podzadania** spełniających liczby węzłów określonej.

Te zadania są przypisywane identyfikator liczba całkowita z zakresu od 0 do *numberOfInstances* - 1. Zadanie o identyfikatorze 0 jest głównym zadaniem, a wszystkie inne identyfikatory są podzadania. Na przykład jeśli utworzysz następujące ustawienia wielu wystąpień zadania głównym zadaniem woli identyfikator 0, a podzadania znajdują się identyfikatory 1 do 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Węzeł główny

Podczas przesyłania zadań wielu wystąpień usługa partii wyznacza jednego z węzłów obliczeń jako węzeł "głównego" i zaplanowanie podstawowego zadania do wykonania w węźle wzorca. Podzadania zostały zaplanowane do wykonania w pozostałych węzłach przydzielonych do zadania wielu wystąpień.

## <a name="coordination-command"></a>Polecenie można je zsynchronizować

**Polecenie można je zsynchronizować** jest wykonywane przez podstawowych i podzadania.

Blokuje wywołania polecenia można je zsynchronizować — nie działać polecenie aplikacji do momentu polecenia można je zsynchronizować zwrócił pomyślnie dla wszystkich podzadań. Polecenie można je zsynchronizować powinny w związku z tym uruchomienia usług wszelkie wymagane tła, sprawdź, czy są gotowe do użycia, a następnie zamknij. Na przykład tego polecenia można je zsynchronizować rozwiązanie przy użyciu wersji MS-MPI, że 7 uruchomienie usługi SMPD w węźle kończy pracę:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Zwróć uwagę na zastosowanie `start` w tym poleceniu można je zsynchronizować. Jest to wymagane, ponieważ `smpd.exe` aplikacji nie zwraca natychmiast po wykonaniu. Bez użycia [Rozpoczynanie] [ cmd_start] polecenia to polecenie można je zsynchronizować nie zwróci i w związku z tym umożliwia zablokowanie polecenia aplikacji w pracy.

## <a name="application-command"></a>Polecenia aplikacji

Podstawowe zadania i wszystkie podzadania zostały wykonane wykonywania polecenia można je zsynchronizować zadania wielu wystąpień wiersza polecenia są wykonywane przez głównym zadaniem *tylko*. Nazywamy to **polecenie aplikacji** , aby odróżnić go od polecenia można je zsynchronizować.

W przypadku aplikacji MS-MPI za pomocą polecenia aplikacji do wykonania z obsługą MPI aplikacji przy użyciu `mpiexec.exe`. Na przykład Oto polecenie aplikacji przy użyciu wersji 7 MS-MPI rozwiązania:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Ponieważ MS MPI `mpiexec.exe` używa `CCP_NODES` zmiennej przez domyślne (zobacz [Zmienne środowiska](#environment-variables)) przykład powyżej wiersza polecenia aplikacji, nie obejmuje go.

## <a name="environment-variables"></a>Zmienne środowiska

Partia tworzy kilka [zmiennych środowiska] [ msdn_env_var] specyficzne dla wielu wystąpień zadania w węzłach obliczeń przydzielonych do zadania wielu wystąpień. Wiersze polecenia można je zsynchronizować i aplikacji można odwoływać się do tych zmiennych środowiska jak skrypty i programy, które wykonują one.

Następujące zmienne środowiska są tworzone przez usługę partii do użycia przez wielu wystąpień zadania:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Aby uzyskać szczegółowe informacje na temat tych i innych partii obliczeń węzeł środowiska zmiennych, w tym ich zawartość i widoczność, zobacz [obliczyć zmienne środowiska węzeł][msdn_env_var].

>[AZURE.TIP] Przykładowy kod partii Linux MPI zawiera przykład opisano niektóre z tych zmiennych środowiska możliwości użycia. [Można je zsynchronizować cmd] [ coord_cmd_example] imprezie skrypt do pobrania typowych aplikacji i wprowadzania plików z magazynu Azure umożliwia udziału sieciowego systemu plików (NFS) na węzeł wzorca i konfiguruje inne węzły przydzielonych do zadania wielu wystąpień jako klientów NFS.

## <a name="resource-files"></a>Pliki zasobów

Istnieją dwa zestawy plików zasobów brać pod uwagę w przypadku zadań wielu wystąpień: **Typowe pliki zasobów** pobrać *Wszystkie* zadania (podstawowych i podzadania) oraz **pliki zasobów** , określone dla wielu wystąpień się do pobrania zadania *tylko podstawowych* zadań.

Można określić jeden lub kilka **typowych plików zasobów** w ustawieniach wielu wystąpień zadania. Poniższe wspólne pliki zasobów są pobierane z [Magazynu Azure](./../storage/storage-introduction.md) każdego węzła **zadania katalog udostępniony** przez podstawowy i wszystkie podzadania. Możesz skorzystać katalogu udostępnionego zadań przy użyciu aplikacji i można je zsynchronizować wiersze polecenia za pomocą `AZ_BATCH_TASK_SHARED_DIR` zmiennej środowiska. `AZ_BATCH_TASK_SHARED_DIR` Ścieżka jest identyczne na każdym węźle zostało przydzielonych do zadania wielu wystąpień, więc możesz udostępnić polecenia pojedynczy można je zsynchronizować między podstawowych i wszystkie podzadania. Partia nie "Udostępnij" katalog w ujęciu dostępu zdalnego, ale możesz go używać, gdy instalacji lub udostępnić punkt jak wspomniano wcześniej w Porada dla środowiska zmiennych.

Pliki zasobów, które można określić dla zadania wielu wystąpień sam są pobierane do katalogu roboczego danego zadania, `AZ_BATCH_TASK_WORKING_DIR`, domyślnie. Wymienionych w odróżnieniu od typowych pliki zasobów, głównym zadaniem pobiera określone zadania wielu wystąpień się pliki zasobów.

> [AZURE.IMPORTANT] Zawsze używaj zmiennych środowiska `AZ_BATCH_TASK_SHARED_DIR` i `AZ_BATCH_TASK_WORKING_DIR` Aby odwołać się do tych katalogów w wierszach do polecenia. Nie należy ręcznie utworzyć ścieżki.

## <a name="task-lifetime"></a>Czas użytkowania zadania

Czasu trwania zadania podstawowego kontrolki ważności całego wielu wystąpień zadania. Po zamknięciu podstawowych, są zamykane wszystkie podzadania. Kod zakończenia podstawowy jest kod zakończenia zadania i w związku z tym służy do określania powodzenie lub niepowodzenie zadania na potrzeby ponów próbę.

Jeśli dowolny z podzadań nie Kończenie pracy z wartością niezerową kod powrotu, na przykład całą wielu wystąpień zadanie nie powiedzie się. Zadanie wielu wystąpień jest następnie zakończone i próby wykonania do limitu ponów próbę.

Po usunięciu zadania wielu wystąpień podstawowej i wszystkie podzadania usuwane są również przez usługę partię. Wszystkie podzadania katalogi i ich pliki są usuwane z węzłów obliczeń, tak jak w przypadku zadania standardowego.

[TaskConstraints] [ net_taskconstraints] wielu wystąpień zadania, na przykład [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock]i [RetentionTime] [ net_taskconstraint_retention] właściwości są uznane dla zadania standardowego, i zastosować do podstawowej i wszystkie podzadania. Jednak w przypadku zmiany [RetentionTime] [ net_taskconstraint_retention] właściwość po dodaniu zadania wielu wystąpień zadania, ta zmiana zostanie zastosowany tylko do podstawowych zadań. Wszystkie podzadania nadal korzystać z oryginalnym [RetentionTime][net_taskconstraint_retention].

Listy zadań z ostatnich węzła obliczeń odzwierciedla identyfikator podzadania ostatnie zadanie stanowi część wielu wystąpień zadania.

## <a name="obtain-information-about-subtasks"></a>Uzyskiwanie informacji na temat podzadań

Aby uzyskać informacje na podzadania za pomocą biblioteki .NET partię, połączenie [CloudTask.ListSubtasks] [ net_task_listsubtasks] metody. Ta metoda zwraca informacje o wszystkich podzadań i informacje o węzeł obliczeń, które wykonywane zadania. Z tych informacji można określić katalog główny każdego podzadania, identyfikator puli, jego bieżącym stanie i kod zakończenia. Za pomocą tych informacji w połączeniu z [PoolOperations.GetNodeFile] [ poolops_getnodefile] sposobem uzyskania plików podzadania. Należy zauważyć, że ta metoda nie zwraca informacje o zadaniu podstawowego (identyfikator 0).

> [AZURE.NOTE] O ile inaczej, metody .NET partii, które działają w wielu wystąpień [CloudTask] [ net_task] *tylko* dotyczą sam głównym zadaniem. Na przykład, gdy nawiązujesz połączenie telefoniczne [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metody nad zadaniem wielu wystąpień, zwracane są tylko pliki głównym zadaniem.

Poniższy fragment kodu przedstawiono sposób uzyskiwania informacji o podzadania, a także żądanie zawartość pliku z węzłów, na których są wykonywane.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Przykładowy kod

[MultiInstanceTasks] [ github_mpi] przykładzie kodu na GitHub pokazano, jak używać wielu wystąpień zadania do przeprowadzania [MS-MPI] [ msmpi_msdn] aplikacji na partię obliczyć węzły. Postępuj zgodnie z instrukcjami [Przygotowanie](#preparation) i [wykonanie](#execution) , aby uruchomić przykład.

### <a name="preparation"></a>Przygotowanie

1. Postępuj zgodnie z instrukcjami pierwszych dwóch [sposobów kompilowania i uruchamianie programu MS-MPI proste][msmpi_howto]. Spełnia on prerequesites dla następny krok.
1. Tworzenie *wersji [MPIHelloWorld] * [ helloworld_proj] przykładowego programu MPI. To jest program, który zostanie uruchomiona w węzłach obliczeń przez wielu wystąpień zadania.
1. Tworzenie pliku zip zawierającego `MPIHelloWorld.exe` (który możesz utworzoną w kroku 2) i `MSMpiSetup.exe` (który można pobrać kroku 1). Ten plik zip zostanie przekazany jako pakiet aplikacji w następnym kroku.
1. [Azure portal] za pomocą[ portal] Aby utworzyć partię [aplikacji](batch-application-packages.md) o nazwie "MPIHelloWorld" i określić pliku zip został utworzony w poprzednim kroku jako wersji "1.0" pakiet aplikacji. Aby uzyskać więcej informacji, zobacz [przekazywanie i zarządzanie aplikacjami](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Tworzenie *wersji* `MPIHelloWorld.exe` tak, aby nie trzeba uwzględnić wszelkie dodatkowe zależności (na przykład `msvcp140d.dll` lub `vcruntime140d.dll`) do pakietu aplikacji.

### <a name="execution"></a>Wykonanie

1. Pobierz [azure partii prób] [ github_samples_zip] z GitHub.
1. Otwórz MultiInstanceTasks **rozwiązanie** w Visual Studio 2015 r. `MultiInstanceTasks.sln` Rozwiązanie plik znajduje się w:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Wprowadź swoje poświadczenia konta partii i miejsca do magazynowania w `AccountSettings.settings` w programie project **Microsoft.Azure.Batch.Samples.Common** .
1. **Tworzenie i uruchamianie** rozwiązanie MultiInstanceTasks wykonać MPI przykładowe aplikacji na obliczyć węzły w puli partię.
1. *Opcjonalnie*: [Azure portal] za pomocą[ portal] lub [Eksploratora partii] [ batch_explorer] przyjrzenie się przykładowe puli, zadania i zadania ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") przed Usuń zasoby.

>[AZURE.TIP] Możesz pobrać [Społeczności programu Visual Studio] [ visual_studio] bezpłatnie, jeśli nie masz programu Visual Studio.

Dane wyjściowe `MultiInstanceTasks.exe` jest podobny do następującego:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Następne kroki

- Blog HPC firmy Microsoft i zespołu partii Azure w tym artykule omówiono [MPI obsługę Linux partii Azure][blog_mpi_linux]oraz informacje na temat korzystania z [OpenFOAM] [ openfoam] z partii. Przykłady kodu Python można znaleźć, na [przykład OpenFOAM GitHub][github_mpi].

- Dowiedz się, jak [tworzyć pule węzłów obliczeń Linux](batch-linux-nodes.md) do użycia w rozwiązanie MPI partii Azure.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Omówienie wielu wystąpień"
