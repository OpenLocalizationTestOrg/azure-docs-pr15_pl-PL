<properties
    pageTitle="Samouczek — Wprowadzenie do biblioteki .NET wsadowe Azure | Microsoft Azure"
    description="Informacje podstawowe pojęcia partii Azure i opracowywaniu usługi partii z przykładu."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Rozpoczynanie pracy z biblioteką partii Azure dla środowiska .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Podstawy [Partia Azure] [ azure_batch] i [.NET partii] [ net_api] biblioteki, w tym artykule jak omówimy C# przykładową aplikację krok po kroku. Przyjrzymy jak ta aplikacja przykładowa wykorzystuje usługę partii przetwarzania równoległe obciążenie pracą w chmurze, oraz jak interakcji z [Azure miejsca do magazynowania](../storage/storage-introduction.md) dla robocze i pobierania plików. Dowiesz typowych technik przepływu pracy aplikacji partię. Można również uzyskać podstawowy opis głównych elementów partii, takie jak zadania, zadania, pule i obliczyć węzły.

![Przepływ pracy rozwiązanie partii (basic)][11]<br/>

## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że znają C# i Visual Studio. Przyjęto założenie, że jesteś w stanie spełnić wymagania dotyczące tworzenia konta, określonych poniżej Azure i usług partii i miejsca do magazynowania.

### <a name="accounts"></a>Konta

- **Konto Azure**: Jeśli nie masz jeszcze Azure subskrypcji, [Utwórz bezpłatne konto Azure][azure_free_account].
- **Konto wsadowe**: po umieszczeniu Azure subskrypcji, [Utwórz konto Azure partię](batch-account-create-portal.md).
- **Konto miejsca do magazynowania**: zobacz [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Partia obecnie obsługuje *tylko* typ konta magazynu **uniwersalny** , zgodnie z opisem w kroku #5 [Utwórz konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Programu Visual Studio

Musi być **Visual Studio 2015** do utworzenia przykładowego projektu. Bezpłatnych i wersji próbnej wersji programu Visual Studio można znaleźć w [Omówienie produktów Visual Studio 2015][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Przykładowy kod *DotNetTutorial*

[DotNetTutorial] [ github_dotnettutorial] próbki jest jednym z wielu przykłady kodu znaleziony w [azure partii prób] [ github_samples] repozytorium na GitHub. Można pobrać próbki, klikając przycisk **Pobierz ZIP** na stronie głównej repozytorium lub klikając [azure wsadowe próbki master.zip] [ github_samples_zip] łącze bezpośrednie. Gdy po wyodrębnieniu zawartości pliku ZIP, można znaleźć rozwiązanie w następującym folderze:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Eksploratora partii (opcjonalnie)

[Eksplorator partii Azure] [ github_batchexplorer] to bezpłatne narzędzie, które znajduje się w [azure partii prób] [ github_samples] repozytorium na GitHub. Gdy nie jest wymagane do użycia tego samouczka, może być przydatne podczas projektowania i debugowania rozwiązanie partii.

## <a name="dotnettutorial-sample-project-overview"></a>Przegląd projektów przykładowe DotNetTutorial

Przykładowy kod *DotNetTutorial* jest rozwiązaniem Visual Studio 2015, która obejmuje dwa projekty: **DotNetTutorial** i **TaskApplication**.

- **DotNetTutorial** jest z aplikacją kliencką interakcji z usługami partii i miejsca do magazynowania do wykonania równoległe obciążenie pracą na obliczanie węzły (maszyn wirtualnych). DotNetTutorial jest uruchamiany w miejscu pracy lokalnego.

- **TaskApplication** to program, który jest uruchamiany w węzłach obliczeń w Azure do wykonywania pracy rzeczywistej. W próbce `TaskApplication.exe` analizowanie tekstu w pliku pobrane z magazynu Azure (plik wprowadzania). Następnie tworzy plik tekstowy (w pliku docelowym), który zawiera listę górny trzy słowa, które pojawiają się w pliku wejściowego. Po utworzeniu pliku wyjściowym, TaskApplication wysyła plik do magazynu Azure. Udostępnia się z aplikacją kliencką do pobrania. TaskApplication działa równolegle na wiele węzłów obliczeń w usłudze partię.

Na poniższej ilustracji przedstawiono podstawowy czynności wykonywanych przez aplikację klienta, *DotNetTutorial*i aplikacji, w której jest wykonywane przez zadań, *TaskApplication*. Typowe wiele rozwiązań obliczeń, które są tworzone przy użyciu partii jest to podstawowe przepływ pracy. Podczas jej nie wykazuje wszystkich funkcji dostępnych w usłudze wsadowe niemal każdego scenariusz wsadowe zawiera podobne procesy.

![Partia przykładowego przepływu pracy][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) Tworzenie **kontenerów** w magazynie obiektów Blob platformy Azure.<br/>
[**Krok 2.**](#step-2-upload-task-application-and-data-files) Przekaż pliki aplikacji zadania i wprowadzania danych do kontenerów.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Tworzenie partii **puli**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Pula **StartTask** pobiera plików binarnych zadania (TaskApplication) do węzłów jako dołączenie do puli.<br/>
[**Krok 4.**](#step-4-create-batch-job) Tworzenie partii **zadania**.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Dodawanie **zadań** do zadania.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Zadania są planowane do wykonania w węzłach.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5B.** Każdego zadania pobiera dane wejściowe z magazynu Azure, a następnie rozpoczyna się wykonywanie.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Monitorowanie zadań.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jak zadania są wykonane, one przekazać swoje dane wyjściowe do magazyn Azure.<br/>
[**W kroku 7.**](#step-7-download-task-output) Pobierz dane wyjściowe zadania z magazynu.

Jak wspomniano, nie każdy rozwiązanie wsadowe wykonuje następujące szczegółowe instrukcje, a może zawierać wiele innych, ale aplikacja przykładowa *DotNetTutorial* prezentuje typowymi procesami znaleziony w partii rozwiązanie.

## <a name="build-the-dotnettutorial-sample-project"></a>Tworzenie przykładowego *DotNetTutorial* projektu

Przed zostanie pomyślnie uruchomiona próbki, należy określić zarówno partii i magazynu poświadczeń konta w programie project *DotNetTutorial* `Program.cs` pliku. Jeśli jeszcze tego nie zrobiono, otwórz rozwiązanie w programie Visual Studio, klikając dwukrotnie `DotNetTutorial.sln` pliku rozwiązania. Lub otworzyć z poziomu programu Visual Studio przy użyciu **Plik > Otwórz > projektu i rozwiązanie** menu.

Otwórz `Program.cs` w programie project *DotNetTutorial* . Następnie Dodaj poświadczenia określone w górnej części pliku:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Jak wcześniej wspomniano, obecnie wprowadź poświadczenia konta miejsca do magazynowania **uniwersalny** w magazynie Azure. Magazyn obiektów blob w ramach konta miejsca do magazynowania **uniwersalny** za pomocą aplikacji partii. Nie można określić poświadczenia konta miejsca do magazynowania, został utworzony, wybierając typ konta *Magazyn obiektów Blob* .

Swoje poświadczenia konta partii i miejsca do magazynowania w ramach karta konta poszczególnych usług można znaleźć w [Azure portal][azure_portal]:

![Partii poświadczeń w portalu][9]
![magazynu poświadczeń w portalu][10]<br/>

Po zaktualizowaniu projektu przy użyciu poświadczeń, kliknij prawym przyciskiem myszy rozwiązanie w Eksploratorze rozwiązań i kliknij przycisk **Konstruuj rozwiązanie**. Przywracanie opakowań NuGet, upewnij się, jeśli zostanie wyświetlony monit.

> [AZURE.TIP] Jeśli pakiety NuGet nie są automatycznie przywracane lub Jeśli widzisz błędy o błędzie przywrócenie pakiety, upewnij się, że masz [Menedżera pakietów NuGet] [ nuget_packagemgr] zainstalowany. Następnie należy włączyć pobieranie brakujących pakietów. Zobacz [Włączanie pakiet Przywracanie podczas tworzenia] [ nuget_restore] umożliwiający pobierania pakietu.

W poniższych sekcjach możemy podziału przykładowej aplikacji do czynności, które wykonuje przetwarzania obciążenie pracą w usłudze partię, a te kroki szczegółowo omówiono. Zachęcamy do odwołują się do otwartego rozwiązania w programie Visual Studio podczas pracy jak do dalszej części tego artykułu, ponieważ został omówiony nie każdy wiersz kodu w próbce.

Przejdź na początek `MainAsync` metody w programie project *DotNetTutorial* `Program.cs` plik, aby uruchomić z kroku 1. Każdy krok poniżej, a następnie około następuje przejście wywołania metody w `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Krok 1: Tworzenie kontenerów miejsca do magazynowania

![Tworzenie kontenerów w magazynie Azure][1]
<br/>

Wsadowe obsługuje wbudowane interakcji z magazyn Azure. Kontenerów na koncie miejsca do magazynowania zapewni plików wymaganych przez zadania, które będą uruchamiane na koncie partię. Kontenery zapewniają również miejsce do przechowywania danych wyjściowych, które zadania warzywa. Najpierw z aplikacją kliencką *DotNetTutorial* oznacza to, Utwórz trzy kontenery w [Magazynie obiektów Blob platformy Azure](../storage/storage-introduction.md):

- **aplikacja**: ten kontener będzie przechowywana aplikacji, uruchamiając zadania, a także wszystkich zależności, takich jak dll.
- **wprowadzania**: zadań zostaną pobrane pliki danych do przetwarzania w kontenerze *wprowadzania* .
- **wynik**: podczas zadania wykonane przetwarzanie pliku wejściowego, będzie ich przekazywanie wyników w kontenerze *dane wyjściowe* .

Aby interakcje z kontem miejsca do magazynowania i tworzyć kontenery, firma Microsoft korzysta z [Biblioteki klienta Azure miejsca do magazynowania dla środowiska .NET][net_api_storage]. Tworzymy odwołanie do konta z [CloudStorageAccount][net_cloudstorageaccount]i na które tworzyć [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Firma Microsoft korzysta z `blobClient` odwołanie w aplikacji, a następnie przekazać go jako parametr do kilku metod. Na przykład znajduje się w bloku kodu, poniższą powyżej, gdzie nazywamy `CreateContainerIfNotExistAsync` faktycznie tworzenie kontenerów.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Po utworzeniu kontenerów aplikacji teraz przekazać pliki, które będą używane przez zadania.

> [AZURE.TIP] [Jak używać magazyn obiektów Blob z .NET](../storage/storage-dotnet-how-to-use-blobs.md) zawiera omówienie pracy z kontenerami Azure magazyn obiektów blob. Po rozpoczęciu pracy z partii powinna być u góry listy odczytu.

## <a name="step-2-upload-task-application-and-data-files"></a>Krok 2: Przekazywanie plików aplikacji i danych zadań

![Przekazywanie aplikacji zadań i plików danych wejściowych (dane) do kontenerów][2]
<br/>

W operacji przekazywania plików *DotNetTutorial* najpierw definiuje kolekcje ścieżki pliku **aplikacji** i **wprowadzania** , jakie istnieją na komputerze lokalnym. Następnie wysyła te pliki do kontenerów, które zostały utworzone w poprzednim kroku.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Istnieją dwie metody w `Program.cs` które uczestniczą w procesie przekazywania:

- `UploadFilesToContainerAsync`: Ta metoda zwraca kolekcję [ResourceFile] [ net_resourcefile] obiektów (omówiony poniżej) i wewnętrznie połączeń `UploadFileToContainerAsync` przekazywania każdego pliku, który jest przekazywany w parametrze *filePaths* .
- `UploadFileToContainerAsync`: Jest to metoda faktycznie wykonuje wysyłanie pliku i tworzy [ResourceFile] [ net_resourcefile] obiektów. Po przekazaniu pliku, uzyskuje podpis udostępniania (SA) do pliku i zwraca obiekt ResourceFile, który je reprezentuje. Udostępniane uprawnienia dostępu, jakie podpisy zostały również opisane poniżej.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ net_resourcefile] zawiera zadania w partii z adresem URL do pliku w magazynie Azure, pobranych do węzła obliczeń przed uruchomieniu tego zadania. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] właściwość określa pełnego adresu URL pliku, ponieważ istnieje w magazynie Azure. Adres URL może także zawierać podpis udostępniania (SA), który zapewnia bezpieczny dostęp do pliku. Większości typów zadań w partii .NET zawiera właściwość *ResourceFiles* , w tym:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Nie korzysta z aplikacji przykładowej DotNetTutorial JobPreparationTask lub JobReleaseTask typów zadań, ale można więcej informacji o nich w [Uruchom przygotowanie i zakończenia zadania w partii Azure obliczyć węzły](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Podpis udostępniania (SA)

Ciągi udostępniania są podpisy, które — po dostępne jako część adresu URL — bezpieczny dostęp do kontenerów i BLOB w magazynie Azure. DotNetTutorial aplikacji jest używane zarówno obiektów blob, jak i kontener udostępnione uzyskać dostęp do adresów URL podpisu, a dowiesz się, jak uzyskać te ciągi podpisu udostępniania z usługą Magazyn.

- **Podpisy dostępu do udostępnionych obiektów blob**: StartTask w puli w DotNetTutorial używa podpisów dostępu do udostępnionych obiektów blob podczas pobierania plików binarnych aplikacji i danych wejściowych plików z magazynu (zobacz krok nr 3 poniżej). `UploadFileToContainerAsync` Metoda na DotNetTutorial `Program.cs` zawiera kod, który uzyskuje poszczególnych obiektów blob udostępnienia podpisu. Oznacza to, dzwoniąc [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Kontener udostępnione podpisów dostępu**: podczas każdego zadania zakończy pracę w węźle obliczeń, przekazywanie jej plik docelowy w kontenerze *wyjścia* w magazynie Azure. W tym celu TaskApplication używa podpis udostępniania kontenera, która zapewnia dostęp do zapisu w kontenerze jako część ścieżki, gdy go wysyła plik. Uzyskiwanie podpisu udostępnienia kontenera odbywa się w podobny sposób jak podczas uzyskiwania to udostępniony podpis programu access. W DotNetTutorial, będzie stwierdzisz, że `GetContainerSasUrl` metoda Pomocnik wywołuje [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] to zrobić. Będzie więcej więcej informacji o używaniu kontenerze TaskApplication udostępnione w podpisie dostępu "krok 6: zadania monitora."

> [AZURE.TIP] Zapoznaj się z serii dwuczęściowe na podpisów udostępnienia [część 1: opis modelu podpisu (SA) udostępniania](../storage/storage-dotnet-shared-access-signature-part-1.md) i [część 2: tworzenie i używanie podpisu udostępniania (SA) z magazynem obiektów Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), aby dowiedzieć się więcej na temat zapewnianie bezpiecznego dostępu do danych na swoim koncie miejsca do magazynowania.

## <a name="step-3-create-batch-pool"></a>Krok 3: Tworzenie puli partii

![Tworzenie puli partii][3]
<br/>

Wsadowe **puli** to zbiór węzłów obliczeń (maszyn wirtualnych), na których wsadowe wykonuje zadania.

Po jego przekazywania plików aplikacji i danych na koncie miejsca do magazynowania, *DotNetTutorial* uruchamia jego interakcji z usługą partii za pomocą biblioteki .NET partii. W tym celu [BatchClient] [ net_batchclient] utworzenia:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Następnie puli węzłów obliczeń zostanie utworzona w konta partii z połączenia do `CreatePoolAsync`. `CreatePoolAsync`używa [BatchClient.PoolOperations.CreatePool] [ net_pool_create] sposobem faktycznie tworzenie puli w usłudze partii.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Po utworzeniu puli z [CreatePool][net_pool_create], określ kilku parametrów takich jak liczba węzłów obliczeń, [rozmiar węzłów](../cloud-services/cloud-services-sizes-specs.md)i system operacyjny węzły. W *DotNetTutorial*, firma Microsoft korzysta z [CloudServiceConfiguration] [ net_cloudserviceconfiguration] Określ Windows Server 2012 R2 z [Usług w chmurze](../cloud-services/cloud-services-guestos-update-matrix.md). Jednak, określając [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] zamiast tego możesz utworzyć pul węzły utworzone na podstawie obrazy Marketplace, zawierające obrazy w systemach Windows i Linux — zobacz [Linux należy obliczyć węzłów na pul partii Azure](batch-linux-nodes.md) , aby uzyskać więcej informacji.

> [AZURE.IMPORTANT] Zajmujesz zasobów obliczeń w partii. Aby zminimalizować koszty, można zmniejszyć `targetDedicated` 1 przed uruchomieniem próbki.

Wraz z tych właściwości fizycznego węzła można też określić [StartTask] [ net_pool_starttask] puli. StartTask wykonuje w każdym węźle tego węzła łączy puli, a każdym uruchomieniu węzła. StartTask jest szczególnie przydatne w przypadku instalowania aplikacji w węzłach obliczeń przed wykonywaniu zadań. Na przykład jeśli zadań przetwarzania danych przy użyciu skryptów Python można StartTask do zainstalowania Python w węzłach obliczeń.

W tej aplikacji przykładowych StartTask skopiuje go do pobrania z magazynu pliki (określonej za pomocą [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] właściwości) z katalogu roboczego StartTask do katalogu udostępnionego dostępnej dla *wszystkich* zadań uruchomionych w węźle. Przede wszystkim, spowoduje to skopiowanie `TaskApplication.exe` i jego zależności do katalogu udostępnionego w każdym węźle jako węzeł dołączy puli, tak, aby wszystkie zadania uruchamiana w węźle można do niego dostęp.

> [AZURE.TIP] Funkcja **pakietów aplikacji** partii Azure udostępnia innym sposobem uzyskania aplikacji na węzły obliczeń w puli. Aby uzyskać szczegółowe informacje, zobacz [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md) .

Godny uwagi w powyższych wstawkę kodu jest także stosowania dwóch zmiennych środowiska we właściwości *wiersza polecenia* StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` i `%AZ_BATCH_NODE_SHARED_DIR%`. Każdy węzeł obliczeń w puli partię automatycznie skonfigurowano wiele zmiennych środowiska, które są specyficzne dla partii. Każdy proces, który jest wykonywane przez zadanie ma dostęp do tych zmiennych środowiska.

> [AZURE.TIP] Aby dowiedzieć się więcej o środowisku czynników, które są dostępne w węzłach obliczeń w puli partii i informacji na temat zadań pracy katalogów, zobacz [Ustawienia środowiska dla zadań](batch-api-basics.md#environment-settings-for-tasks) i [pliki i katalogi](batch-api-basics.md#files-and-directories) w [partii omówienie funkcji dla deweloperów](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Krok 4: Tworzenie zadania

![Tworzenie zadania][4]<br/>

Wsadowe **zadania** to zbiór zadań i jest skojarzony z puli węzłów obliczeń. Wykonywanie zadań w ramach zadania w węzłach obliczeń puli skojarzone.

Możesz użyć zadania, nie tylko dla organizowanie i śledzenie zadań w powiązanych obciążenia, ale również dla nakładające pewne ograniczenia — na przykład maksymalnej wykonywania zadania (i przez rozszerzenie swoich zadań) oraz priorytet zadania względem innych zadań w oknie konta partii. W tym przykładzie jednak zadanie jest skojarzony tylko z puli, który został utworzony w kroku #3. Nie dodatkowych właściwości są skonfigurowane.

Wszystkie zadania wsadowe są skojarzone z określonej puli. To skojarzenie wskazuje węzły, które zadania będzie wykonywana na. Określanie to za pomocą [CloudJob.PoolInformation] [ net_job_poolinfo] właściwości, jak pokazano poniżej wstawkę kodu.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Teraz, gdy zadanie zostało utworzone, zadania są dodawane do wykonywania pracy.

## <a name="step-5-add-tasks-to-job"></a>Krok 5: Dodawanie zadań do zadania

![Dodawanie zadań do zadania][5]<br/>
*(1) zadania są dodawane do zadania, (2 zadania zaplanowane w węzłach i (3) zadania pobierania plików danych do przetwarzania*

Partia **zadania** są poszczególnych jednostki pracy, które wykonują w węzłach obliczeń. Zadanie ma wiersz polecenia i uruchamiania skryptów lub plików wykonywalnych, określonym przez użytkownika w tym wierszu polecenia.

Aby dokonać pracy, zadania musi zostać dodana do zadania. Każdy [CloudTask] [ net_task] to skonfigurować przy użyciu właściwości wiersza polecenia i [ResourceFiles] [ net_task_resourcefiles] (tak jak z tej puli StartTask) którego zadanie do pobrania do węzła przed jego wiersza polecenia są wykonywane automatycznie. W programie project próbki *DotNetTutorial* każdego zadania przetwarza tylko jeden plik. W związku z tym jego kolekcja ResourceFiles zawiera pojedynczego elementu.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Gdy będą uzyskiwać dostęp do środowiska zmiennych takich jak `%AZ_BATCH_NODE_SHARED_DIR%` lub wykonywanie aplikacji nie można odnaleźć w węźle `PATH`, musi być poprzedzony wierszy polecenia zadania `cmd /c`. Spowoduje to jawnie wykonywanie interpretera poleceń i poinstruuj zakończenie po przeprowadzeniu polecenia. To wymaganie nie jest konieczne, jeśli zadań Uruchom aplikację w węźle `PATH` (na przykład *robocopy.exe* lub *powershell.exe*), a nie zmiennych środowiska.

W ramach `foreach` pętli w powyższych wstawkę kodu, widać, że wiersza polecenia dla zadania jest tworzona w taki sposób, że do *TaskApplication.exe*przekazywane są trzy argumenty wiersza polecenia:

1. **Pierwszy argument** jest ścieżka pliku do procesu. Jest lokalną ścieżkę do pliku, ponieważ istnieje w węźle. Gdy obiekt ResourceFile w `UploadFileToContainerAsync` została utworzona powyżej, nazwy pliku użyto tej właściwości (jako parametr do konstruktora ResourceFile). Oznacza to, że plik można znaleźć w tym samym katalogu co *TaskApplication.exe*.

2. **Drugi argument** Określa, że górny wyrazy *N* powinny być zapisywane w pliku wyjściowym. W próbce to jest stałe tak, aby górny trzy słowa są zapisywane w pliku wyjściowym.

3. **Trzeci argument** jest podpis udostępniania (SA), który zapewnia dostęp do zapisu w kontenerze **wyjścia** w magazynie Azure. *TaskApplication.exe* używa ten adres URL podpisu udostępniania, gdy zostanie przesłany w pliku docelowym do magazynu Azure. Kod można znaleźć w tym w `UploadFileToContainer` metody w programie project TaskApplication `Program.cs` pliku:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Zadania monitora

![Zadania monitora][6]<br/>
*Z aplikacją kliencką (1) monitoruje zadań do wykonania i sukcesu stanu i (2 zadania przekazywanie dane wynikowe do magazynu platformy Azure*

Po dodaniu zadań do zadania są automatycznie w kolejce i zaplanowane do wykonania w węzłach obliczeń w puli skojarzone z danym zadaniem. W zależności od ustawień, określanych, partii obsługuje Kolejkowanie wszystkie zadania, planowanie, ponawianie i innych funkcji Administracja zadania dla Ciebie. Istnieje wiele metod monitorowania wykonywanie zadań. DotNetTutorial przedstawiono prosty przykład zawierający raporty tylko na błąd wykonania i zadań lub sukcesu Stany.

W ramach `MonitorTasks` metoda DotNetTutorial w `Program.cs`, dostępne są trzy .NET partii zagadnienia gwarantuje dyskusji. Są one wymienione poniżej w kolejności ich wyświetlania:

1. **ODATADetailLevel**: Określanie [ODATADetailLevel] [ net_odatadetaillevel] na liście jest niezbędne w celu zapewnienia wydajności aplikacji partii operacji (takich jak uzyskiwanie listy zadania). Dodawanie [wydajność kwerendy usługi Azure partii](batch-efficient-list-queries.md) do odczytu listy Jeśli planujesz wykonując dowolną sortowania stanu monitorowania w aplikacjach partii.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] oferuje aplikacji .NET partii przy użyciu narzędzia Pomocnik do monitorowania stany zadań. W `MonitorTasks`, *DotNetTutorial* czeka dla wszystkich zadań do osiągnięcia [TaskState.Completed] [ net_taskstate] w terminie. Następnie kończy zadanie.

3. **TerminateJobAsync**: Kończenie zadanie z [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (lub blokowania JobOperations.TerminateJob) oznacza to zadanie jako wykonane. Jest to zrobić, jeśli rozwiązanie partii korzysta z [JobReleaseTask][net_jobreltask]. Jest to specjalny typ zadania, które opisano w [przygotowaniu i ukończenia zadania](batch-job-prep-release.md).

`MonitorTasks` Metody *DotNetTutorial* `Program.cs` wyświetlany poniżej:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Krok 7: Pobierz dane wyjściowe zadania

![Pobierz dane wyjściowe zadania z magazynu][7]<br/>

Teraz, gdy zadanie jest wykonane, dane wyjściowe zadania można pobrać z magazynu Azure. Jest to połączenie do `DownloadBlobsFromContainerAsync` w *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Połączenie `DownloadBlobsFromContainerAsync` w *DotNetTutorial* aplikacja określa, że pliki powinny być pobierane do programu `%TEMP%` folder. Zachęcamy do modyfikowania tego lokalizację danych wyjściowych.

## <a name="step-8-delete-containers"></a>Krok 8: Kontenery Usuń

Ponieważ jest naliczany dla danych, który znajduje się w magazynie Azure, zawsze jest dobrym pomysłem, aby usunąć wszelkie blob, które nie są już potrzebne dla swojego zadań. W jego DotNetTutorial `Program.cs`, to połączeń trzy metody Pomocnik `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Metoda się jedynie pobiera odwołanie do kontenera, a następnie wywołuje [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Krok 9: Usuwanie zadania i puli

W ostatnim kroku użytkownika jest monit o usunięcie zadania i puli, które zostały utworzone przez aplikację DotNetTutorial. Mimo że nie jest rozliczana według zadań i zadań, *są* pobierane za węzły obliczeń. Dlatego zalecamy przydzielić węzły, tylko w razie potrzeby. Usuwanie nieużywanych pul może być procesie konserwacji.

BatchClient [JobOperations] [ net_joboperations] i [PoolOperations] [ net_pooloperations] obydwie odpowiedniej metody usuwania, które są nazywane, jeśli użytkownik potwierdza usunięcie:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Należy pamiętać, która jest rozliczana dla zasobów obliczeń — usuwanie nieużywanych pul zostanie zminimalizowane koszt. Ponadto należy pamiętać, że usunięcie puli powoduje usunięcie wszystkich węzłach obliczeń w tej puli, a wszystkie dane w węzłach będzie odzyskać po usunięciu puli.

## <a name="run-the-dotnettutorial-sample"></a>Uruchamianie przykładowych *DotNetTutorial*

Po uruchomieniu aplikacji przykładowe dane wyjściowe z konsoli będzie podobny do następującego. Podczas wykonywania, będą wrażenia Wstrzymaj na `Awaiting task completion, timeout in 00:30:00...` podczas węzły obliczeń w puli są uruchamiane. [Azure portal] za pomocą[ azure_portal] monitorowanie użytkownika, obliczyć węzły, zadania i zadania podczas i po wykonaniu. [Azure portal] za pomocą[ azure_portal] lub [Eksploratora magazynu Azure] [ storage_explorers] do wyświetlania zasobów miejsca do magazynowania (kontenerów i obiektów blob), które zostały utworzone przez aplikację.

Typowe czasu wykonania jest **około 5 minut** po uruchomieniu aplikacji w konfiguracji domyślnej.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Następne kroki

Zachęcamy wprowadzić zmiany w *DotNetTutorial* i *TaskApplication* , aby poeksperymentować z obliczeń w różnych scenariuszach. Na przykład, spróbuj dodać opóźnienie wykonanie *TaskApplication*, takich jak z [Thread.Sleep][net_thread_sleep], aby symulować długim zadań i monitorować je w portalu. Spróbuj dodając więcej zadań lub dostosowanie liczby węzłów obliczeń. Dodawanie logiki na sprawdzanie dostępności i zezwala na użycie istniejącej puli na czas wykonywania szybkości (*Wskazówka*: Zapoznaj się z `ArticleHelpers.cs` w [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektu w [azure partii prób][github_samples]).

Teraz, gdy znasz podstawowe przepływu pracy rozwiązania partii nadszedł czas na Drąż celu dodatkowe funkcje usługi partii.

- Przeczytaj [Omówienie funkcji wsadowe dla deweloperów](batch-api-basics.md), które zalecamy dla wszystkich nowych użytkowników partię.
- Rozpoczynanie od innych partii rozwoju artykułów w **rozwoju szczegółowo** w [partii ścieżka nauki][batch_learning_path].
- Zapoznaj się z różnych stosowania przetwarzania obciążenie pracą "góry N wyrazy" przy użyciu partii w [TopNWords] [ github_topnwords] próbki.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Tworzenie kontenerów w magazynie Azure"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Przekazywanie aplikacji zadań i plików danych wejściowych (dane) do kontenerów"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Tworzenie puli partii"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Tworzenie zadania"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Dodawanie zadań do zadania"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Zadania monitora"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Pobierz dane wyjściowe zadania z magazynu"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Przepływ pracy rozwiązanie partii (pełna diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Poświadczenia partii w portalu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Magazyn poświadczeń w portalu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Przepływ pracy rozwiązanie partii (minimalnego diagram)"
