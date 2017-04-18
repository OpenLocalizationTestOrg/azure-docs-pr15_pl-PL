<properties
    pageTitle="Samouczek — Wprowadzenie do klienta Python wsadowe Azure | Microsoft Azure"
    description="Podstawowe pojęcia partii Azure i opracowywaniu usługi partii o Prosty scenariusz"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Rozpoczynanie pracy z klientem Python partii Azure

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Podstawy [Partia Azure] [ azure_batch] i [Python partii] [ py_azure_sdk] klienta jako omówimy mała aplikacja partii napisana Python. Przyjrzymy się jak dwa przykładowe skrypty Użyj usługi partii przetwarzania równoległe obciążenie pracą, w przypadku maszyn wirtualnych Linux w chmurze i sposobem interakcji z [Azure miejsca do magazynowania](./../storage/storage-introduction.md) dla robocze i pobierania plików. Będzie więcej typowych przepływu pracy aplikacji partii i uzyskanie podstawowych zrozumienia główne składniki partii takie jak zadania, zadania, pul i obliczyć węzły.

![Przepływ pracy rozwiązanie partii (basic)][11]<br/>

## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że znają Python i znajomości Linux. Przyjęto założenie, że jesteś w stanie spełnić wymagania dotyczące tworzenia konta, określonych poniżej Azure i usług partii i miejsca do magazynowania.

### <a name="accounts"></a>Konta

- **Konto Azure**: Jeśli nie masz jeszcze Azure subskrypcji, [Utwórz bezpłatne konto Azure][azure_free_account].
- **Konto partii**: po umieszczeniu Azure subskrypcji, [Utwórz konto Azure partię](batch-account-create-portal.md).
- **Konto miejsca do magazynowania**: zobacz [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Przykładowy kod

Samouczki Python [Przykładowy kod] [ github_article_samples] jedną z wielu przykłady kodu partia znajduje się w [azure partii prób] [ github_samples] repozytorium na GitHub. Pobierz wszystkie próbki, klikając **klonowanie lub pobieranie > Pobierz ZIP** na stronie głównej repozytorium lub klikając [azure wsadowe próbki master.zip] [ github_samples_zip] łącze bezpośrednie. Gdy po wyodrębnieniu zawartości pliku ZIP dwóch skryptów dla tego samouczka znajdują się w `article_samples` katalogu:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Środowisko Python

Aby uruchomić przykładowy skrypt *python_tutorial_client.py* na lokalnej stacji roboczej, należy **interpretera Python** zgodny z wersji **2.7** lub **3.3 +**. Skrypt przetestowano na systemu Windows i Linux.

### <a name="cryptography-dependencies"></a>zależności kryptograficzny

Konieczne jest zainstalowanie zależności [kryptograficzny] [ crypto] biblioteki, wymagane przez `azure-batch` i `azure-storage` Python pakietów. Wykonaj jedną z następujących czynności odpowiednie dla swojej platformy lub odwoływać się do [instalacji kryptograficzny] [ crypto_install] szczegóły, aby uzyskać więcej informacji:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES-OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Systemu Windows

    `pip install cryptography`

>[AZURE.NOTE] Jeśli instalacja Python 3.3 + w systemie Linux, za pomocą odpowiedników python3 dla zależności Python. Na przykład w Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Pakiety Azure

Następnie zainstalować pakiety **Partii Azure** i Python **Magazyn Azure** . Można to zrobić przy użyciu **pip** i *requirements.txt* znaleźć tutaj:

`/azure-batch-samples/Python/Batch/requirements.txt`

Problem, wykonując polecenie **pip** , aby zainstalować pakiety partii i miejsca do magazynowania:

`pip install -r requirements.txt`

Lub możesz zainstalować [partii azure] [ pypi_batch] i [Magazyn azure] [ pypi_storage] Python pakiety ręcznie:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Może być konieczne Prefiks polecenia z `sudo` Jeśli używasz konta nie posiadająca uprawnień. Na przykład `sudo pip install -r requirements.txt`. Aby uzyskać więcej informacji na temat instalowania pakietów Python, zobacz [Instalowanie pakietów] [ pypi_install] na readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Przykładowy kod samouczka Python wsadowe

Przykładowy kod samouczka Python partia składa się z dwóch skryptów Python i kilka plików danych.

- **python_tutorial_client.PY**: interakcji z usługami partii i miejsca do magazynowania na wykonanie równoległe obciążenie pracą w węzłach obliczeń (maszyn wirtualnych). Skrypt *python_tutorial_client.py* jest uruchamiany w miejscu pracy lokalnego.

- **python_tutorial_task.PY**: skrypt uruchamianej na obliczanie węzłów na Azure do wykonywania pracy rzeczywistej. W próbce *python_tutorial_task.py* analizuje tekst w pliku pobrane z magazynu Azure (plik wprowadzania). Następnie tworzy plik tekstowy (w pliku docelowym), który zawiera listę górny trzy słowa, które pojawiają się w pliku wejściowego. Po utworzeniu pliku wyjściowym, *python_tutorial_task.py* wysyła plik do magazynu Azure. Dzięki temu dostępne do pobrania skrypt klienta uruchomiony w miejscu pracy. Skrypt *python_tutorial_task.py* jest uruchamiany równolegle na wielu węzłach obliczeń w usłudze partię.

- **./data/taskdata\*txt**: te pliki tekstowe trzy podanie danych wejściowych dla zadania, które będą uruchamiane w węzłach obliczeń.

Na poniższej ilustracji przedstawiono podstawowy czynności wykonywanych przez skrypty klienta i zadania. Typowe wiele rozwiązań obliczeń, które są tworzone przy użyciu partii jest to podstawowe przepływ pracy. Podczas jej nie wykazuje wszystkich funkcji dostępnych w usłudze partii niemal każdego scenariusz partia zawiera fragmenty tego przepływu pracy.

![Partia przykładowego przepływu pracy][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) Tworzenie **kontenerów** w magazynie obiektów Blob platformy Azure.<br/>
[**Krok 2.**](#step-2-upload-task-script-and-data-files) Przekaż pliki skryptu i wprowadzanie zadań do kontenerów.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Tworzenie partii **puli**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Skrypt zadania (python_tutorial_task.py) do pobrania w puli **StartTask** węzły podczas dołączania puli.<br/>
[**Krok 4.**](#step-4-create-batch-job) Tworzenie partii **zadania**.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Dodawanie **zadań** do zadania.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Zadania są planowane do wykonania w węzłach.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5B.** Każdego zadania pobiera dane wejściowe z magazynu Azure, a następnie rozpoczyna się wykonywanie.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Monitorowanie zadań.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jak zadania są wykonane, one przekazać swoje dane wyjściowe do magazyn Azure.<br/>
[**W kroku 7.**](#step-7-download-task-output) Pobierz dane wyjściowe zadania z magazynu.

Jak wcześniej wspomniano, nie każdy rozwiązanie wsadowe wykonuje następujące szczegółowe instrukcje, a może zawierać wiele więcej, ale tym przykładzie pokazano, typowymi procesami znaleziony w partii rozwiązanie.

## <a name="prepare-client-script"></a>Przygotuj skrypt klienta

Przed uruchomieniem próbki Dodaj swoje poświadczenia konta partii i miejsca do magazynowania do *python_tutorial_client.py*. Jeśli jeszcze tego nie zrobiono, otwórz plik w edytorze Ulubione i zaktualizuj następujące wiersze przy użyciu poświadczeń.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Swoje poświadczenia konta partii i miejsca do magazynowania w ramach karta konta poszczególnych usług można znaleźć w [Azure portal][azure_portal]:

![Partii poświadczeń w portalu][9]
![magazynu poświadczeń w portalu][10]<br/>

W poniższych sekcjach możemy analizowanie kroki używane przez skrypty przetwarzania obciążenie pracą w usłudze partię. Zachęcamy dotyczyć regularnie skryptów w edytorze podczas pracy jak do pozostałej części tego artykułu.

Przejdź do następującego w **python_tutorial_client.py** rozpoczynać krok 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Krok 1: Tworzenie kontenerów miejsca do magazynowania

![Tworzenie kontenerów w magazynie Azure][1]
<br/>

Partia obsługuje wbudowane interakcji z magazyn Azure. Kontenerów na koncie miejsca do magazynowania zapewni plików wymaganych przez zadania, które będą uruchamiane na koncie partię. Kontenery zapewniają również miejsce do przechowywania danych wyjściowych, które zadania warzywa. Najpierw skrypt *python_tutorial_client.py* wykonuje jest tworzenie trzy kontenery w [Magazynie obiektów Blob platformy Azure](../storage/storage-introduction.md#blob-storage):

- **aplikacja**: ten kontener będzie przechowywana skryptu Python, uruchamiając zadania *python_tutorial_task.py*.
- **wprowadzania**: zadań zostaną pobrane pliki danych do przetwarzania z *wprowadzania* kontenera.
- **wynik**: podczas zadania wykonane przetwarzanie pliku wejściowego, będzie ich przekazywanie wyników w kontenerze *dane wyjściowe* .

Aby korzystać z kontem miejsca do magazynowania i tworzyć kontenery, firma Microsoft korzysta z [Magazyn azure] [ pypi_storage] pakietu, aby utworzyć [BlockBlobService] [ py_blockblobservice] obiektu — "klient obiektów blob". Następnie tworzymy trzy kontenerów na koncie miejsca do magazynowania przy użyciu klienta obiektów blob.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Po utworzeniu kontenerów aplikacji teraz przekazać pliki, które będą używane przez zadania.

> [AZURE.TIP] [Jak używać magazyn obiektów Blob platformy Azure z Python](../storage/storage-python-how-to-use-blob-storage.md) zawiera omówienie pracy z kontenerami Azure magazyn obiektów blob. Po rozpoczęciu pracy z partii powinna być u góry listy odczytu.

## <a name="step-2-upload-task-script-and-data-files"></a>Krok 2: Przekazywanie plików skryptu i danych zadań

![Przekazywanie aplikacji zadań i plików wejściowy (dane) do kontenerów][2]
<br/>

W operacji przekazywania plików *python_tutorial_client.py* najpierw definiuje kolekcje ścieżki pliku **aplikacji** i **wprowadzania danych** , jakie istnieją na komputerze lokalnym. Następnie wysyła te pliki do kontenerów, które zostały utworzone w poprzednim kroku.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Za pomocą listy zrozumienia, `upload_file_to_container` funkcja jest wywoływana dla każdego pliku w kolekcje i dwóch [ResourceFile] [ py_resource_file] zbiory są wypełnione. `upload_file_to_container` Funkcji pojawia się poniżej:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ py_resource_file] zawiera zadania w partii z adresem URL do pliku w magazynie Azure, pobranych do węzła obliczeń przed uruchomieniu tego zadania. [ResourceFile][py_resource_file]. Właściwość **blob_source** określa pełnego adresu URL pliku, ponieważ istnieje w magazynie Azure. Adres URL może także zawierać podpis udostępniania (SA), który zapewnia bezpieczny dostęp do pliku. Większości typów zadań w partii zawiera właściwość *ResourceFiles* , w tym:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

W tym przykładzie nie korzysta z typów zadań JobPreparationTask lub JobReleaseTask, ale można więcej informacji o nich w [Uruchom przygotowanie i zakończenia zadania w partii Azure obliczyć węzły](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Podpis udostępniania (SA)

Podpisy udostępniania są ciągami, których bezpieczny dostęp do kontenerów i obiektów blob w magazynie Azure. Skrypt *python_tutorial_client.py* używa obu obiektów blob i kontener udostępnione podpisy dostęp, a dowiesz się, jak uzyskać te ciągi podpisu udostępniania z usługą Magazyn.

- **Podpisy dostępu do udostępnionych obiektów blob**: zastosowania StartTask w puli blob udostępnienia podpisy podczas do pobrania pliki zadań skryptu i wprowadzania danych z magazynu (zobacz [krok nr 3](#step-3-create-batch-pool) poniżej). `upload_file_to_container` Funkcja w *python_tutorial_client.py* zawiera kod, który uzyskuje poszczególnych obiektów blob udostępnienia podpisu. Oznacza to, dzwoniąc [BlockBlobService.make_blob_url] [ py_make_blob_url] w module miejsca do magazynowania.

- **Kontener udostępnienia podpisu**: podczas każdego zadania zakończy pracę w węźle obliczeń, przekazywanie jej plik docelowy w kontenerze *wyjścia* w magazynie Azure. W tym celu *python_tutorial_task.py* używa podpis udostępnienia kontenera, który zapewnia dostęp do zapisu w kontenerze. `get_container_sas_token` Funkcja w *python_tutorial_client.py* uzyskuje podpisu udostępnienia kontenera, który jest następnie przekazywany jako argument wiersza polecenia do zadań. Krok #5, [Dodawanie zadań do zadania](#step-5-add-tasks-to-job), w tym artykule omówiono zastosowania kontenera skojarzeń zabezpieczeń.

> [AZURE.TIP] Zapoznaj się z serii dwuczęściowe na podpisów udostępnienia [część 1: opis modelu skojarzeń zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-1.md) i [część 2: tworzenie i używanie skojarzeń zabezpieczeń z usługą obiektów Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), aby dowiedzieć się więcej na temat zapewnianie bezpiecznego dostępu do danych na swoim koncie miejsca do magazynowania.

## <a name="step-3-create-batch-pool"></a>Krok 3: Tworzenie puli partii

![Tworzenie puli wsadowe][3]
<br/>

Partia **puli** to zbiór węzłów obliczeń (maszyn wirtualnych), na których partii wykonuje zadania.

Po jego wysyła pliki skryptu i danych zadań na koncie miejsca do magazynowania, *python_tutorial_client.py* uruchamia jego interakcji z usługą partii przy użyciu modułu Python partię. W tym celu [BatchServiceClient] [ py_batchserviceclient] zostanie utworzona:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Następnie puli węzłów obliczeń zostanie utworzona w konta partii z połączenia do `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Po utworzeniu puli definiowanie [PoolAddParameter] [ py_pooladdparam] , który określa niektóre właściwości puli:

- **Identyfikator** puli (*id* — wymagane)<p/>Podobnie jak w przypadku większości obiektów w partii do nowej puli musi mieć unikatowy identyfikator w ramach konta partię. Kod, który odwołuje się do tej puli, przy użyciu Identyfikatora, która jest jak rozpoznać puli w [portal]Azure[azure_portal].

- **Liczba węzłów obliczeń** (*target_dedicated* — wymagane)<p/>Ta właściwość określa, ile maszyny wirtualne powinny zostać wdrożone w puli. Należy pamiętać, że wszystkie konta partii domyślny **Przydział** ogranicza liczbę **rdzenie** (i w związku z tym węzły obliczeń) na koncie partię. Domyślne przydziały i instrukcje na temat, aby [zwiększyć przydział](batch-quota-limit.md#increase-a-quota) (na przykład maksymalna liczba rdzeni na koncie wsadowe) można znaleźć w [przydziałami i limity dotyczące usługi Azure partię](batch-quota-limit.md). Jeśli okaże się, że pytaniem "Dlaczego nie Moje puli osiągnięcia więcej niż X węzły?" Przyczyną może być przydział core.

- **System operacyjny** dla węzłów (*virtual_machine_configuration* **lub** *cloud_service_configuration* — wymagane)<p/>W *python_tutorial_client.py*tworzymy puli węzłów Linux przy użyciu [VirtualMachineConfiguration][py_vm_config]. `select_latest_verified_vm_image_with_node_agent_sku` Działać w `common.helpers` ułatwia pracę z [Azure Marketplace maszyn wirtualnych] [ vm_marketplace] obrazów. Aby uzyskać więcej informacji na temat używania obrazów Marketplace, zobacz [Linux należy obliczyć węzłów na pul partii Azure](batch-linux-nodes.md) .

- **Rozmiar węzłów obliczeń** (*vm_size* — wymagane)<p/>Ponieważ firma Microsoft jest określanie węzły Linux oraz dla naszych [VirtualMachineConfiguration][py_vm_config], możemy określić rozmiar pamięci Wirtualnej (`STANDARD_A1` w tym przykładzie) z [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-linux-sizes.md). Ponownie zobacz [Linux należy obliczyć węzłów na pul partii Azure](batch-linux-nodes.md) Aby uzyskać więcej informacji.

- **Rozpoczynanie zadania** (*start_task* - nie są wymagane)<p/>Razem z powyższych właściwości fizycznego węzła, można też określić [StartTask] [ py_starttask] puli (nie jest to wymagane). StartTask wykonuje w każdym węźle węzeł łączy puli, a każdym uruchomieniu węzeł. StartTask jest szczególnie przydatne dla przygotowywanie węzły obliczeń dla realizacji zadań, takich jak instalowanie aplikacje, których zadań.<p/>W tej aplikacji przykładowych StartTask skopiuje go do pobrania z magazynu (które określono przy użyciu właściwości **resource_files** StartTask) z StartTask *katalog roboczy* do katalogu *udostępnionego* , dostępnej dla wszystkich zadań uruchomionych w węźle pliki. Przede wszystkim, spowoduje to skopiowanie `python_tutorial_task.py` do katalogu udostępnionego w każdym węźle jako węzeł dołączy puli, tak, aby wszystkie zadania uruchamiana w węźle do niego dostęp.

Może się okazać połączenie `wrap_commands_in_shell` funkcja Pomocnik. Ta funkcja ma zbiór osobne polecenia i tworzy odpowiednie właściwości wiersza polecenia zadania z jednego wiersza polecenia.

Godny uwagi w powyższych wstawkę kodu jest także stosowania dwóch zmiennych środowiska we właściwości **Wiersz polecenia** StartTask: `AZ_BATCH_TASK_WORKING_DIR` i `AZ_BATCH_NODE_SHARED_DIR`. Każdy węzeł obliczeń w puli partię automatycznie skonfigurowano wiele zmiennych środowiska, które są specyficzne dla partii. Każdy proces, który jest wykonywane przez zadanie ma dostęp do tych zmiennych środowiska.

> [AZURE.TIP] Aby dowiedzieć się więcej o środowisku zmiennych, które są dostępne w węzłach obliczeń w puli partię, a także informacji na temat zadań pracy katalogów, zobacz **Ustawienia środowiska dla zadań** i **pliki i katalogi** w [Omówienie funkcji partii Azure](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Krok 4: Tworzenie zadania

![Tworzenie zadania][4]<br/>

Wsadowe **zadania** to zbiór zadań i jest skojarzony z puli węzłów obliczeń. Wykonywanie zadań w ramach zadania w węzłach obliczeń puli skojarzone.

Za pomocą zadania nie tylko do organizowania i śledzenie zadań w powiązanych obciążenia, ale również dla nakładające pewne ograniczenia — na przykład maksymalnej wykonywania zadania (i przez rozszerzenie swoich zadań) i zadań priorytet względem innych zadań w partii konta. W tym przykładzie jednak zadanie jest skojarzony tylko z puli, który został utworzony w kroku #3. Nie dodatkowych właściwości są skonfigurowane.

Wszystkie zadania wsadowe są skojarzone z określonej puli. To skojarzenie wskazuje węzły, które zadania wykonać na. Określanie puli za pomocą [PoolInformation] [ py_poolinfo] właściwości, jak pokazano poniżej wstawkę kodu.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Teraz, gdy zadanie zostało utworzone, zadania są dodawane do wykonywania pracy.

## <a name="step-5-add-tasks-to-job"></a>Krok 5: Dodawanie zadań do zadania

![Dodawanie zadań do zadania][5]<br/>
*(1) zadania są dodawane do zadania, (2 zadania zaplanowane w węzłach i (3) zadania pobierać pliki danych do przetwarzania*

Partia **zadania** są poszczególnych jednostki pracy, które wykonują w węzłach obliczeń. Zadanie ma wiersz polecenia i uruchamiania skryptów lub plików wykonywalnych, określonym przez użytkownika w tym wierszu polecenia.

Aby wykonują faktyczną pracę, należy dodać zadania do zadania. Każdy [CloudTask] [ py_task] skonfigurowano właściwość wiersza polecenia i [ResourceFiles] [ py_resource_file] (tak jak z tej puli StartTask) którego zadanie do pobrania do węzła przed jego wiersza polecenia są wykonywane automatycznie. W próbce każdego zadania przetwarza tylko jeden plik. W związku z tym jego kolekcja ResourceFiles zawiera pojedynczego elementu.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Gdy będą uzyskiwać dostęp do środowiska zmiennych takich jak `$AZ_BATCH_NODE_SHARED_DIR` lub wykonywanie aplikacji nie można odnaleźć w węźle `PATH`, wiersze polecenia zadania należy wywołać powłokę jawnie, takich jak z `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. To wymaganie dotyczy niepotrzebny, jeśli zadań Uruchom aplikację w węźle `PATH` i nie odwołuje się do dowolnych zmiennych środowiska.

W ramach `for` pętli w powyższych wstawkę kodu, widać, że wiersza polecenia dla zadania jest tworzona z argumenty wiersza polecenia, które są przekazywane do *python_tutorial_task.py*:

1. **ścieżka pliku**: jest to lokalne ścieżkę do pliku, ponieważ istnieje w węźle. Gdy obiekt ResourceFile w `upload_file_to_container` został utworzony w kroku 2 powyżej została użyta nazwa pliku dla tej właściwości ( `file_path` parametr w Konstruktorze ResourceFile). Oznacza to, że plik można znaleźć w tym samym katalogu w węźle jako *python_tutorial_task.py*.

2. **NUMWORDS**: najważniejsze słowa *N* powinny być zapisywane w pliku wyjściowym.

3. **storageaccount**: Nazwa konta miejsca do magazynowania, które jest właścicielem kontenera, do której należy przesłać wyjście zadań.

4. **storagecontainer**: nazwa kontenera magazynu, do którego można przekazywać pliki wyjściowe.

5. **sastoken**: podpis udostępniania (SA), który zapewnia dostęp do zapisu w kontenerze **wyjścia** w magazynie Azure. Skrypt *python_tutorial_task.py* używa tego podpisu udostępnienia po tworzy jej odwołaniem BlockBlobService. Zapewnia dostęp do zapisu w kontenerze bez konieczności klawisz dostępu do konta miejsca do magazynowania.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Zadania monitora

![Zadania monitora][6]<br/>
*Skrypt (1) monitoruje stan ukończenia zadania i (2 zadania przekazywanie dane wynikowe do magazynowania Azure*

Po dodaniu zadań do zadania są automatycznie w kolejce i zaplanowane do wykonania w węzłach obliczeń w obrębie puli skojarzone z danym zadaniem. W zależności od ustawień, określanych, partii obsługuje Kolejkowanie wszystkie zadania, planowanie, ponawianie i innych funkcji Administracja zadania dla Ciebie.

Istnieje wiele metod monitorowania wykonywanie zadań. `wait_for_tasks_to_complete` Funkcja w *python_tutorial_client.py* udostępnia prosty przykład monitorowania zadania dla niektórych stanu, w tym przypadku [wykonane] [ py_taskstate] stan.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Krok 7: Pobierz dane wyjściowe zadania

![Pobierz dane wyjściowe zadania z magazynu][7]<br/>

Teraz, gdy zadanie jest wykonane, dane wyjściowe zadania można pobrać z magazynu Azure. Jest to połączenie do `download_blobs_from_container` w *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Połączenie `download_blobs_from_container` w *python_tutorial_client.py* Określa, że pliki powinny być pobierane do katalogu macierzystego. Zachęcamy do modyfikowania tego lokalizację danych wyjściowych.

## <a name="step-8-delete-containers"></a>Krok 8: Kontenery Usuń

Ponieważ jest naliczany dla danych, który znajduje się w magazynie Azure, zawsze jest dobrym pomysłem, aby usunąć wszelkie blob, które nie są już potrzebne dla swojego zadań. W *python_tutorial_client.py*to zrobić przy użyciu trzech wywołań [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Krok 9: Usuwanie zadania i puli

W ostatnim kroku zostanie wyświetlony monit o usunięcie zadania i puli, które zostały utworzone przez skrypt *python_tutorial_client.py* . Mimo że nie zajmujesz zadań i zadań, *są* naliczany dla węzłów obliczeń. Dlatego zalecamy przydzielić węzły, tylko w razie potrzeby. Usuwanie nieużywanych pul może być procesie konserwacji.

BatchServiceClient [JobOperations] [ py_job] i [PoolOperations] [ py_pool] mają odpowiednie metody usuwania, które są nazywane potwierdzenie usunięcia:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Należy pamiętać, który zajmujesz dla zasobów obliczeń — usuwanie nieużywanych pul zostanie zminimalizowane koszt. Ponadto należy pamiętać, że usunięcie puli powoduje usunięcie wszystkich węzłach obliczeń w tej puli, a wszystkie dane w węzłach będzie odzyskać po usunięciu puli.

## <a name="run-the-sample-script"></a>Uruchomić przykładowy skrypt

Po uruchomieniu skryptu *python_tutorial_client.py* z samouczka [Przykładowy kod][github_article_samples], dane wyjściowe z konsoli jest podobny do następującego. Ma nastąpić przerwa na `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` podczas tworzenia węzły obliczeń w puli, pracę, a polecenia w puli rozpoczęcia zadania są wykonywane. [Azure portal] za pomocą[ azure_portal] monitorowanie użytkownika, obliczyć węzły, zadania i zadania podczas i po wykonaniu. [Azure portal] za pomocą[ azure_portal] lub [Eksploratora magazynu usługi Microsoft Azure] [ storage_explorer] do wyświetlania zasobów miejsca do magazynowania (kontenerów i obiektów blob), które zostały utworzone przez aplikację.

>[AZURE.TIP] Uruchamianie skryptu *python_tutorial_client.py* z poziomu `azure-batch-samples/Python/Batch/article_samples` katalogu. Ta ścieżka względna dla `common.helpers` importowanie modułów, więc może się pojawić `ImportError: No module named 'common'` po uruchomieniu nie skrypt w tym katalogu.

Czas wykonywania typowych jest **około 5 do 7 minut** po uruchomieniu próbki w konfiguracji domyślnej.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Następne kroki

Zachęcamy wprowadzić zmiany w *python_tutorial_client.py* i *python_tutorial_task.py* wypróbowanie różnych obliczeń scenariuszy. Na przykład spróbuj dodać opóźnienie wykonanie *python_tutorial_task.py* zasymulowania długim zadań i Monitoruj je w portalu. Spróbuj dodając więcej zadań lub dostosowanie liczby węzłów obliczeń. Dodawanie logiki na sprawdzanie dostępności i zezwala na użycie istniejącej puli na czas wykonywania szybkości.

Teraz, gdy znasz podstawowe przepływu pracy rozwiązania wsadowe nadszedł czas na Drąż dodatkowe funkcje usługi partii.

- Przejrzyj artykułu [Funkcje partii omówienie Azure](batch-api-basics.md) zaleca się, jeśli jesteś nowym użytkownikiem usługi.
- Rozpoczynanie od innych partii rozwoju artykułów w **rozwoju szczegółowo** w [partii ścieżka nauki][batch_learning_path].
- Zapoznaj się z różnych stosowania przetwarzania obciążenie pracą "góry N wyrazy" z partii w [TopNWords] [ github_topnwords] próbki.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Tworzenie kontenerów w magazynie Azure"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Przekazywanie aplikacji zadań i plików wejściowy (dane) do kontenerów"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Tworzenie puli partii"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Tworzenie zadania"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Dodawanie zadań do zadania"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Zadania monitora"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Pobierz dane wyjściowe zadania z magazynu"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Przepływ pracy rozwiązanie partii (pełna diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Poświadczenia partii w portalu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Magazyn poświadczeń w portalu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Przepływ pracy rozwiązanie partii (minimalnego diagram)"
