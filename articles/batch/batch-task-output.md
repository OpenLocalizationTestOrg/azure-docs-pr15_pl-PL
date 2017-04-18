<properties
    pageTitle="Pozostawanie w partii Azure wyjściowe zadania i zadania | Microsoft Azure"
    description="Dowiedz się, jak używać magazyn Azure jako trwałe sklepu partii zadania i zadania wyjściowe, a także włączyć wyświetlanie tego raportu trwałe w Azure portal."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Zmiana jest zachowywana partii Azure wyjściowe poleceń i zadań

Zadania, które są uruchamiane w partii zwykle da wynik, który musi być przechowywane i później pobrane przez inne zadania w zadaniu z aplikacją kliencką, które wykonywane zadania lub obie. Te dane wyjściowe mogą być pliki utworzone przez przetwarzania danych wejściowych lub skojarzone z wykonywanie zadań pliki dziennika. W tym artykule przedstawiono Biblioteka klas .NET używającej techniką oparte na konwencjach pozostać taki produkt zadania z magazynem obiektów Blob platformy Azure, udostępnianie go nawet, po usunięciu z pul zadań i obliczyć węzły.

Za pomocą techniki w tym artykule, można także wyświetlić danych wyjściowych zadania w **plików wyjściowych zapisane** i **Dzienniki zapisane** w [Azure portal][portal].

![Pliki wyjściowe Saved i selektory Dzienniki zapisane w portalu][1]

>[AZURE.NOTE] [Konwencje plik wsadowy Azure] [ nuget_package] Biblioteka klas .NET omawiane w tym artykule jest obecnie w podglądzie. Niektóre z opisanych tutaj funkcji mogą ulec zmianie przed ogólnodostępną.

## <a name="task-output-considerations"></a>Zagadnienia dotyczące dane wyjściowe zadania

Podczas projektowania rozwiązania partii, należy rozważyć kilka kwestii związanych z wyjściowe poleceń i zadań.

* **Obliczanie istnienia węzeł**: obliczenia są często przejściowych, zwłaszcza w obsługiwanych autoscale pul. Wydajność zadania, które będą uruchamiane w węźle są dostępne tylko wtedy, gdy istnieje węzeł i tylko w czasie przechowywania pliku ustawiony dla zadania. Aby upewnić się, że dane wyjściowe zadania jest zachowywany, zadań musi w związku z tym przekazywać pliki wyjścia do sklepu trwałe, na przykład magazyn Azure.

* **Miejsca do magazynowania danych wyjściowych**: aby dane wyjściowe zadania do magazynu trwałego, umożliwia [Azure SDK miejsca do magazynowania](../storage/storage-dotnet-how-to-use-blobs.md) w kodzie zadania przekazywanie wyjście zadań do kontenera magazyn obiektów Blob. W przypadku zastosowania kontener i Konwencja nazewnicza plików, aplikacji klienckiej lub inne zadania można zlokalizować i Pobierz te dane wyjściowe na podstawie Konwencji.

* **Pobieranie danych wyjściowych**: Jeśli zadań pozostają jej wynik można pobrać wyjście zadań bezpośrednio z węzłów obliczeń w puli użytkownika lub magazyn Azure. Aby pobrać dane wyjściowe zadania bezpośrednio z poziomu węzła obliczeń, należy nazwę pliku i jego lokalizację danych wyjściowych w węźle. Jeżeli utrzymują wyjścia do magazynu Azure, podrzędne zadania lub aplikację klienta musi mieć pełną ścieżkę do pliku w magazynie Azure, aby go pobrać przy użyciu zestawu SDK miejsca do magazynowania Azure.

* **Wyświetlanie danych wyjściowych**: Przejdź do zadania partii w portalu Azure i wybierz pozycję **pliki w węźle**, dostępne są wszystkie pliki skojarzone z tym zadaniem, nie tylko plików wyjściowych Cię interesuje. Ponownie pliki w węzłach obliczeń są dostępne tylko wtedy, gdy istnieje węzeł i tylko w czasie przechowywania pliku ustawiony dla zadania. Aby wyświetlić dane wyjściowe zadania zostały zachowywane do magazynu Azure w portalu lub aplikację taką jak [Eksplorator magazynu Azure][storage_explorer], należy ustalić lokalizacji i przejść bezpośrednio do pliku.

## <a name="help-for-persisted-output"></a>Pomoc dla istniejących danych wyjściowych

Ułatwiające więcej łatwo utrzymują i zadań wyjściowy, zespołu partii zdefiniowane i wdrożone zestawu konwencje nazewnictwa, a także Biblioteka klas .NET, [Konwencje plik wsadowy Azure] [ nuget_package] biblioteki, używanego w aplikacjach partię. Ponadto Azure portal jest pamiętać o następujących konwencje nazewnictwa, tak, aby można było łatwo znajdować pliki, które jest zapisany za pomocą biblioteki.

## <a name="using-the-file-conventions-library"></a>Korzystanie z biblioteki konwencje pliku

[Azure konwencje plik wsadowy] [ nuget_package] jest biblioteka klas .NET, które aplikacji .NET partii umożliwiają łatwe przechowywanie i pobieranie wyjściowe zadania do i z miejsca do magazynowania Azure. Jest ona przeznaczona do użycia w kodzie zarówno zadania, jak i kliencie — w kodzie zadania utrwalanie plików i w kodzie klienta do listy i pobieranie ich. Zadania można także użyć biblioteki do pobierania wyjściowe nadrzędny zadań, takich jak w scenariuszu [współzależności zadań](batch-task-dependencies.md) .

Biblioteka konwencje trwa istotnych zapewnienie, że kontenerów miejsca do magazynowania i zadania wyjściowy pliki są nazywane zgodnie z Konwencji i są przekazywane do właściwych miejsc po umieszczone magazyn Azure. Podczas pobierania wyników, możesz łatwo znaleźć wyjściowe dla danego zadania lub zadania, pozycje lub pobieranie wyjściowe według identyfikatorów i przeznaczenie zamiast nazw plików lub w którym istnieje w magazynie.

Na przykład umożliwia biblioteki "Lista wszystkich plików pośrednie dla zadania 7" lub "Pobierz mnie podglądu miniatur dla zadania *mymovie*," bez znajomości nazwy plików lub lokalizacji w ramach konta miejsca do magazynowania.

### <a name="get-the-library"></a>Pobieranie biblioteki

Można uzyskać biblioteki, która zawiera nowe klasy i rozszerza [CloudJob] [ net_cloudjob] i [CloudTask] [ net_cloudtask] klasy z nowych metod z [NuGet][nuget_package]. Możesz dodać je do projektu programu Visual Studio za pomocą [Menedżera pakietów biblioteki NuGet][nuget_manager].

>[AZURE.TIP] Możesz znaleźć [kodu źródłowego] [ github_file_conventions] dla biblioteki konwencje plik wsadowy Azure w GitHub w zestawie Microsoft Azure SDK repozytorium .NET.

## <a name="requirement-linked-storage-account"></a>Wymóg: magazynowania połączonego konta

Aby przechowywać wyjściowe do magazynu trwałego za pośrednictwem biblioteki konwencje plik, a następnie wyświetlać je w Azure portal, należy [łącze konta magazynu platformy Azure](batch-application-packages.md#link-a-storage-account) do konta w partii. Jeśli jeszcze tego nie zrobiono, Połącz konto miejsca do magazynowania ze swoim kontem partii przy użyciu Azure portal:

Karta **partii konto** > **Ustawienia** > **Konta miejsca do magazynowania** > **Konta miejsca do magazynowania** (Brak) > Wybierz konto miejsca do magazynowania w subskrypcji

Aby uzyskać bardziej szczegółowe omówienie na łączenie konta miejsca do magazynowania zobacz [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md).

## <a name="persist-output"></a>Zmiana jest zachowywana wyjścia

Istnieją dwa podstawowe akcje do wykonania podczas zapisywania danych wyjściowych poleceń i zadań z biblioteką konwencje pliku: Tworzenie kontenera magazynu i zapisywanie danych wyjściowych w kontenerze.

>[AZURE.WARNING] Ponieważ wszystkie wyjściowe poleceń i zadań są przechowywane w tym samym kontenerze, [miejsca do magazynowania ograniczenie](../storage/storage-performance-checklist.md#blobs) może być wymuszane Jeśli wypróbować dużą liczbą zadań pliki w tym samym czasie.

### <a name="create-storage-container"></a>Tworzenie kontenera magazynu

Przed zadań rozpocząć utrwalanie wyjścia do magazynu, należy utworzyć kontenera magazynu obiektów blob, do którego zostanie przekazany ich produkcji. W tym celu nawiązywania połączeń z [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Ta metoda rozszerzenia przyjmuje [CloudStorageAccount] [ net_cloudstorageaccount] obiekt jako parametr i tworzy kontenera wymienionych w taki sposób, że jego zawartość jest odnajdowania przez Azure portal i metody pobierania omówiony w dalszej części tego artykułu.

Zazwyczaj umieścić ten kod w aplikacji klienckiej — aplikacji, która tworzy pule, zadań i zadania.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Wyniki zadań ze sklepu

Teraz, gdy została przygotowana kontenera w magazynie obiektów blob, zadania można zapisać dane wyjściowe do kontenera przy użyciu [TaskOutputStorage] [ net_taskoutputstorage] klasy znaleziony w bibliotece konwencje pliku.

W kodzie zadania, należy najpierw utworzyć [TaskOutputStorage] [ net_taskoutputstorage] obiekt, a następnie po zakończeniu pracy zadania połączeń [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] metody, aby zapisać dane wyjściowe magazyn Azure.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Parametr "wyjścia typu" kategoryzuje istniejących plików. Istnieją cztery wstępnie zdefiniowane [TaskOutputKind] [ net_taskoutputkind] typów: "TaskOutput", "TaskPreview", "TaskLog" i "TaskIntermediate." Można także zdefiniować niestandardowe typy, jeśli może być przydatne w przepływie pracy.

Typy danych wyjściowych pozwalają określić typ wyników, aby wyświetlić listę później kwerendy partii dla obrazów trwałych danego zadania. Innymi słowy po ponownym wyjściowe dla zadania, można filtrować listy na jeden z typów danych wyjściowych. Na przykład "można uzyskać *Podgląd* danych wyjściowych dla zadania *109*." Więcej na wyświetlanie i pobieranie wyjściowe zostanie wyświetlone [pobrać danych wyjściowych](#retrieve-output) w dalszej części tego artykułu.

>[AZURE.TIP] Typ danych wyjściowych wskazuje, gdzie w portalu Azure określony plik znajduje się: *TaskOutput*-podzielonych na kategorie plików są wyświetlane w "Pliki wyjściowe zadania", a *TaskLog* plików są wyświetlane w "Dzienniki zadania".

### <a name="store-job-outputs"></a>Przechowywanie wyjściowe zadania

Oprócz przechowywania wyjściowe zadań, można przechowywać wyjściowe skojarzone z całe zadanie. Na przykład w zadaniu korespondencji seryjnej zadania renderowania filmu może zachowywane pełni renderowanego film jako wynik zadania. Po zakończeniu pracy z aplikacją kliencką po prostu można wyświetlić listę i pobierania wyników dla zadania i nie trzeba kwerendy poszczególne zadania.

Przechowywanie danych wyjściowych zadania, dzwoniąc [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] metody, i określ [JobOutputKind] [ net_joboutputkind] i nazwę pliku:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Zgodnie z TaskOutputKind dla obrazów zadanie, użyj [JobOutputKind] [ net_joboutputkind] parametru kategoryzowania zadanie jest zachowywane plików. Ten parametr umożliwia kwerendy dla określonego typu danych wyjściowych (listy). JobOutputKind zawiera typy zarówno dane wyjściowe i Podgląd i obsługuje tworzenie niestandardowe typy.

### <a name="store-task-logs"></a>Przechowaj dzienniki zadania

Oprócz utrzymuje pliku do magazynu trwałego po ukończeniu zadania lub zadania, użytkownik może być konieczne pozostać pliki, które są aktualizowane podczas wykonywania zadania — pliki dziennika lub `stdout.txt` i `stderr.txt`, na przykład. W tym celu biblioteki konwencje plik wsadowy Azure udostępnia [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] metody. Z [SaveTrackedAsync][net_savetrackedasync], można śledzić aktualizacje do pliku w węźle (interwałem określonym przez użytkownika) i utrzymują te aktualizacje do magazynu Azure.

W następujących wstawkę kodu, firma Microsoft korzysta z [SaveTrackedAsync] [ net_savetrackedasync] aktualizacji `stdout.txt` w magazynie Azure co 15 sekund podczas wykonywania zadania:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`jest po prostu symbolu zastępczego kod, który zwykle przeprowadza się zadania. Na przykład może być kod, który pobiera dane z magazynu Azure i wykonuje on przekształcenie lub obliczeń. Ważne część tej wstawki kodu jest pokazujące, jak możesz zawinąć takiego kodu w `using` blok okresowo aktualizować pliku z [SaveTrackedAsync][net_savetrackedasync].

`Task.Delay` Jest wymagane na końcu niniejszego `using` blok, aby upewnić się, że agent węzeł ma czasu na zapisywanie zawartości w nowym oknie standardowy plik stdout.txt w węźle (agent węzeł jest program, który jest uruchamiany w każdym węźle puli i interfejs poleceń i kontroli między węzeł a usługą partii). Ten niezwłocznie prawdopodobnie przeoczyć ostatniej kilka sekund danych wyjściowych. Opóźnienie to nie może być wymagane dla wszystkich plików.

>[AZURE.NOTE] Po włączeniu śledzenia z SaveTrackedAsync pliku tylko *dołącza* do śledzonych pliku są zachowywane do magazynu Azure. Użyj tej metody można użyć tylko w przypadku śledzenia-obracanie plików dziennika lub innych plików, które zostaną dołączone do, oznacza to, dane tylko dodawane jest na końcu pliku, gdy zostanie zaktualizowana.

## <a name="retrieve-output"></a>Pobieranie danych wyjściowych

Podczas pobierania wyprowadzania trwałe za pośrednictwem biblioteki konwencje plik wsadowy Azure możesz zrobić w sposób zadania i zadania zorientowane. Możesz zażądać wynik dla danego zadania lub zadania bez znajomości ścieżki magazyn obiektów blob lub nawet jego nazwa pliku. Można po prostu powiedzieć "Podaj plików wyjściowych dla zadania *109*."

Poniższy fragment kodu iterację wszystkie zadania, drukowany jako pewne informacje na temat plików wyjściowych dla zadania, a następnie pobierania plików z magazynu.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Wyjściowe zadania i Azure portal

Azure portal Wyświetla wyjściowe zadania i dzienników, które są zachowywane połączony klient magazyn Azure za pomocą konwencje nazewnictwa znaleziony w [Azure partii plik konwencje — Plik README][github_file_conventions_readme]. Można zaimplementować tych konwencji samodzielnie w języku wybrane, lub za pomocą biblioteki konwencje plików w aplikacjach .NET.

### <a name="enable-portal-display"></a>Włączanie wyświetlaną portalu

Aby włączyć wyświetlanie swojego wyjściowe w portalu, musi spełniać następujące wymagania:

 1. [Łącze konta magazynu platformy Azure](#requirement-linked-storage-account) do konta w partii.
 2. Gdy utrzymuje wyjściowe zgodny wstępnie zdefiniowanych konwencje nazewnictwa dla kontenerów miejsca do magazynowania i plików. Definicja tych konwencji można znaleźć w bibliotece konwencje plik [README][github_file_conventions_readme]. Jeśli używasz [Konwencje plik wsadowy Azure] [ nuget_package] biblioteki do innej witryny danych wyjściowych to wymaganie jest spełnione.

### <a name="view-outputs-in-the-portal"></a>Wyświetlanie wyników w portalu

Aby wyświetlić wyjściowe zadania i dzienniki Azure portal, przejdź do zadania których produkcja możesz interesuje, a następnie kliknij **pliki wyjściowe zapisane** lub **zapisane dzienniki**. Ten obraz przedstawia **zapisane pliki wyjściowe** zadania o identyfikatorze "007":

![Karta w portalu Azure wyniki zadań][2]

## <a name="code-sample"></a>Przykładowy kod

[PersistOutputs] [ github_persistoutputs] przykładowy projekt jest jednym z [Przykłady kodu partii Azure] [ github_samples] na GitHub. Tego rozwiązania programu Visual Studio 2015 przedstawiono sposób użycia biblioteki konwencje plik wsadowy Azure pozostać wyjście zadań do magazynu trwałego. Aby uruchomić próbki, wykonaj następujące czynności:

1. Otwórz projekt w **Visual Studio 2015 r**.
2. Dodawanie partii i przechowywanie **poświadczeń konta** do **AccountSettings.settings** w programie project Microsoft.Azure.Batch.Samples.Common.
3. **Tworzenie** (ale nie będą uruchamiane) rozwiązanie. Przywracanie wszystkie pakiety NuGet, jeśli zostanie wyświetlony monit.
4. Aby przekazać [pakiet aplikacji](batch-application-packages.md) do **PersistOutputsTask**za pomocą portalu Azure. Dołączanie `PersistOutputsTask.exe` i jego zestawy zależne w pakiecie zip Ustaw identyfikator aplikacji do "PersistOutputsTask" i "1.0" wersja pakietu aplikacji.
5. **Rozpoczynanie** Projekt, (Uruchom) **PersistOutputs** .

## <a name="next-steps"></a>Następne kroki

### <a name="application-deployment"></a>Wdrażanie aplikacji

Funkcja [pakietów aplikacji](batch-application-packages.md) partii zapewnia łatwy sposób zarówno wdrażanie i wersji aplikacji, które zadania wykonywane na obliczanie węzły.

### <a name="installing-applications-and-staging-data"></a>Instalowanie aplikacji i danych przemieszczenia

Zapoznaj się z [instalowania aplikacji i tymczasowej danych na partię obliczyć węzły] [ forum_post] opublikować na forum partii Azure omówiono różne metody przygotowania węzły uruchamianie zadań. Napisany przez jednego z członków zespołu partii Azure, ten wpis jest dobrym Elementarz na różne sposoby pobierania plików (w tym aplikacji i wprowadzania danych zadań) na węzły obliczeniowej, a niektóre uwagi dla każdej z tych metod.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Pliki wyjściowe Saved i selektory Dzienniki zapisane w portalu"
[2]: ./media/batch-task-output/task-output-02.png "Karta w Azure portal wyniki zadań"