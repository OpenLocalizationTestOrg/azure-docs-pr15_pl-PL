<properties
    pageTitle="Zadanie przygotowanie i oczyszczanie w partii | Microsoft Azure"
    description="Minimalizowanie transfer danych do węzłów obliczeń partii Azure za pomocą zadania przygotowawcze poziom zadania, a następnie zwolnij zadania oczyszczania węzeł u ukończenia zadania."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Uruchamianie przygotowanie i zakończenia zadania w węzłach do uruchamiania partię Azure

 Azure zadania wymaga często niektóre formularza ustawienia przed jego zadania są wykonywane, a po zadania konserwacji po zakończeniu swoich zadań. Może być konieczne pobrać dane wejściowe typowe zadania na węzły obliczeń albo przekazać dane wyjściowe zadania do magazynu Azure, po zakończeniu zadania. Za pomocą zadania **przygotowanie zadania** i **Zwolnij zadanie** do wykonania tych operacji.

## <a name="what-are-job-preparation-and-release-tasks"></a>Co to są przygotowanie zadania i zwolnij zadań?

Przed uruchomieniem zadania, zadanie przygotowanie zadania będzie uruchamiane na wszystkich węzłach obliczeń zaplanowane co najmniej jedno zadanie. Po zakończeniu zadania, zadanie Zwolnij zadanie jest uruchamiany w każdym węźle puli wykonać co najmniej jedno zadanie. Podobnie jak w przypadku normalnego partii zadań, można określić wiersz polecenia do wywołania po uruchomieniu przygotowania lub wersji zadania.

Przygotowanie i wersji zadania oferują znanych funkcji partii zadań, takich jak pobieranie pliku ([plików zasobów][net_job_prep_resourcefiles]), pełnymi wykonanie, zmienne środowiska niestandardowe wykonanie maksymalny czas trwania, liczba powtórzeń i czas przechowywania pliku.

W poniższych sekcjach można będzie Dowiedz się, jak za pomocą [JobPreparationTask] [ net_job_prep] i [JobReleaseTask] [ net_job_release] klasy znaleziony w [Partii .NET] [ api_net] biblioteki.

> [AZURE.TIP] Przygotowanie i wersji zadania są szczególnie przydatne w środowiskach "udostępnione puli", w której puli węzłów obliczeń będzie nadal występował między uruchomionych zadań i jest używany przez wiele zadania.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Kiedy używać przygotowanie zadania i zwolnij zadania

Przygotowanie zadania i wersji zadania są właściwie dobierane w następujących sytuacjach:

**Pobieranie danych typowych zadań**

Zadań często wymagają wspólny zestaw danych jako danych wejściowych dla zadania. Na przykład w dzienny obliczeń analizy ryzyka, rynku dane są specyficzne dla zadania, jeszcze wspólne dla wszystkich zadań w zadaniu. Te dane rynku, rozmiar, często kilku gigabajtów powinny być pobierane na każdym węźle obliczeń tylko raz, aby można go używać dowolne zadanie, które jest uruchamiany w węźle. Aby pobrać te dane do każdego węzła przed wykonywania zadania przez inne zadania za pomocą **przygotowanie zadania** .

**Usuń dane wyjściowe i zadań**

W środowisku "udostępnione puli", w którym węzły obliczeń puli nie są likwidowanego między zadaniami, może być konieczne usunięcie danych zadania między działa. Może być konieczne zaoszczędzić miejsce na dysku w węzłach lub spełniają zasady zabezpieczeń Twojej organizacji. Aby usunąć dane, które zostało pobrane przez zadanie przygotowania zadania lub wygenerowane podczas wykonywania zadania, użyj **wersji zadania** .

**Przechowywanie dziennika**

Warto zachować kopię plików dziennika, generujących zadań lub być może ulec awarii zrzutu plików, które mogą być generowane przez aplikacje nie powiodło się. Za pomocą **zadania Zwolnij zadanie** w takich przypadkach kompresowanie i przekazać te dane do [Magazynowania Azure] [ azure_storage] konta.

>[AZURE.TIP] Innym sposobem utrzymują dzienniki i innych poleceń i zadań wyjściowy danych jest użycie biblioteki [Konwencje plik wsadowy Azure](batch-task-output.md) .

## <a name="job-preparation-task"></a>Przygotowanie zadania

Przed wykonaniem zadania partii wykonuje przygotowanie zadania w każdym węźle obliczeń, która jest zaplanowana do wykonania zadania. Domyślnie oczekiwania usługi partii przygotowania zadania do wykonania przed uruchomieniem zadania według harmonogramu do wykonania w węźle. Można jednak skonfigurować usługę nie chcesz czekać. Jeśli węzeł uruchomieniu ponownym uruchomieniu przygotowania zadania, ale można także wyłączyć to zachowanie.

Przygotowanie zadania jest wykonywane tylko na węzłach, które zaplanowane zadania. Dzięki temu niepotrzebne realizacji zadań przygotowania na wypadek, gdyby węzeł nie jest przypisana zadania. Dzieje się tak, jeśli liczba zadań dla zadania jest mniejsza niż liczby węzłów w puli. Dotyczy również włączenie [Wykonywanie zadań jednocześnie](batch-parallel-node-tasks.md) , dzięki czemu niektóre węzły bezczynne, jeśli liczba zadań jest mniejsza niż całkowita liczba możliwych zadań jednocześnie. Uruchamiając nie przygotowanie zadania w węzłach bezczynne, trzeba poświęcić mniej pieniędzy na opłaty za transfer danych.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] różni się od [CloudPool.StartTask] [ pool_starttask] w tym JobPreparationTask wykonuje się na początku każdego zadania, należy StartTask wykonuje tylko wtedy, gdy węzeł obliczeń najpierw łączy puli lub uruchomieniu.

## <a name="job-release-task"></a>Zadanie Zwolnij zadanie

Gdy zadanie zostanie oznaczone jako zakończone, zwolnij zadanie zadania jest wykonywane w każdym węźle puli wykonać co najmniej jedno zadanie. Możesz oznaczyć zadanie jako ukończone, wysyłając żądanie zakończenia. Następnie usługa partii ustawia stan zadania do *zakończenia*, kończy żadnych zadań aktywne lub nie działa, skojarzone z danym zadaniem i uruchamia zadania Zwolnij zadanie. Zadanie następnie przechodzi do stanu *wykonane* .

> [AZURE.NOTE] Usunięcie zadania wykonuje również Zwolnij zadanie zadania. Jednak jeśli zadanie zostało już zakończone, zwolnij zadanie jest nie będą uruchamiane po raz drugi później usunięcie zadania.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Zadania przedprodukcyjnym i zwolnij zadań za pomocą .NET partii

Umożliwia przygotowanie obowiązków przydzielanie [JobPreparationTask] [ net_job_prep] obiekt zadania kopii [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] właściwości. Podobnie, zainicjować [JobReleaseTask] [ net_job_release] i przypisać ją do zadania [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] właściwości, aby ustawić wersji zadania.

W poniższym przykładzie `myBatchClient` jest wystąpieniem [BatchClient][net_batch_client], i `myPool` jest istniejącej puli w ramach konta partię.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Jak wspomniano wcześniej, zwolnij zadanie jest wykonywane po zadanie jest zakończone lub usunięte. Zakończ zadanie z [JobOperations.TerminateJobAsync][net_job_terminate]. Usuwanie zadania z [JobOperations.DeleteJobAsync][net_job_delete]. Zwykle Zakończ lub usuwanie zadania po wykonaniu jej zadań lub osiągnięto limit czasu, które zostały zdefiniowane.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Przykładowy kod na GitHub

Aby wyświetlić zadania przygotowanie i zwolnij zadań w działaniu, zapoznaj się z [JobPrepRelease] [ job_prep_release_sample] przykładowy projekt na GitHub. Ta aplikacja konsoli wykonuje następujące czynności:

1. Tworzy puli z dwóch węzłów "małe".
2. Tworzy zadanie z przygotowanie zadania, wersji i zadań standardowych.
3. Uruchamia przygotowanie zadania, najpierw zapisuje identyfikator węzła do pliku tekstowego w katalogu "udostępnione" węzła.
4. Uruchamia zadania na każdym węźle zapisuje jej identyfikator zadania do tego samego pliku tekstowego.
5. Po zakończeniu wszystkich zadań (lub osiągnięciu limitu czasu), jest drukowany zawartości pliku tekstowego każdego węzła do konsoli.
6. Po zakończeniu zadania działa zadania Zwolnij zadanie, aby usunąć plik z węzła.
6. Umożliwia wydrukowanie kody zakończenia zadań przygotowaniu i wersji zadania dla każdego węzła, na którym są wykonywane.
7. Wykonywanie przerwy w działaniu umożliwia potwierdzenie usunięcia zadania i/lub puli.

Dane wyjściowe z przykładowej aplikacji są podobne do następujących:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Ze względu na zmiennych tworzenia i zacznij czasu węzłów na nową pulę (niektóre węzły są gotowe do zadań przed inne osoby) można zobaczyć różne wyniki. W szczególności ponieważ szybkiego wykonania zadań jednego z tej puli węzłów może wykonywać wszystkie zadania to zadanie. W takim przypadku można zauważyć, że przygotowywanie zadania i zadania wersji nie istnieją węzła, który wykonywane żadnych zadań.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Przeprowadzanie inspekcji przygotowaniu zadania i zadania wersji w portalu Azure

Po uruchomieniu aplikacji przykładowej, można korzystać z [Azure portal] [ portal] Aby wyświetlić właściwości zadania i jego zadań lub nawet pobrać plik tekstowy udostępniony, który jest zmodyfikowany przez zlecenia zadania.

Zrzut ekranu poniżej **Przygotowanie karta zadania** są wyświetlane w portalu Azure po uruchomieniu aplikacji przykładowej. Przejdź do właściwości *JobPrepReleaseSampleJob* po rozwiązaniu zadań (ale przed usunięciem zadania i puli usługi), a następnie kliknij pozycję **zadania przygotowawcze** lub **wersji zadania** , aby wyświetlić jego właściwości.

![Właściwości przygotowanie zadania w Azure portal][1]

## <a name="next-steps"></a>Następne kroki

### <a name="application-packages"></a>Pakiety aplikacji

Oprócz przygotowanie zadania umożliwia także funkcję [pakietów aplikacji](batch-application-packages.md) partii do przygotowania węzły obliczeń do wykonania zadania. Ta funkcja jest szczególnie przydatne w przypadku wdrażania aplikacji, które nie wymagają uruchomiony Instalator, aplikacje, które zawierają wielu plików (100 +) lub aplikacje, które wymagają kontroli ścisła wersja.

### <a name="installing-applications-and-staging-data"></a>Instalowanie aplikacji i danych przemieszczenia

Ten wpis na forum w witrynie MSDN omówiono kilka metod przygotowania węzły uruchamianie zadań:

[Instalowanie aplikacji i danych na partię przemieszczenia obliczyć węzły][forum_post]

Napisane przez jednego z członków zespołu partii Azure, omówiono kilka technik, które umożliwia wdrażanie aplikacji i danych, aby obliczyć węzły.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

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

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
