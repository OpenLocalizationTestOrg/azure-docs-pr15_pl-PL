<properties
    pageTitle="Węzły Linux w partii Azure pul | Microsoft Azure"
    description="Dowiedz się, jak procesu usługi obciążenia równoległe obliczeń na pul maszyn wirtualnych Linux w partii Azure."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Inicjowanie obsługi Linux obliczeń węzłów na pul partii Azure

Partia Azure umożliwia uruchamianie obciążenia równoległe obliczeń w przypadku maszyn wirtualnych zarówno Linux i systemu Windows. W tym artykule wyszczególniono jak tworzyć pule węzłów obliczeń Linux w usłudze partii przy użyciu [Python partii] [ py_batch_package] i [.NET partii] [ api_net] bibliotek klienta.

> [AZURE.NOTE] [Pakiety aplikacji](batch-application-packages.md) obecnie są obsługiwane w węzłach obliczeń Linux.

## <a name="virtual-machine-configuration"></a>Konfiguracja maszyn wirtualnych

Po utworzeniu puli węzłów obliczeń w partii istnieją dwie opcje, z których można wybrać rozmiar węzeł i system operacyjny: Konfiguracja usług w chmurze i konfiguracji maszyn wirtualnych.

**Konfiguracja usługi chmury** zawiera Windows węzły *tylko*obliczenia. Rozmiary węzeł obliczeń dostępne są wymienione w [rozmiarów usług w chmurze](../cloud-services/cloud-services-sizes-specs.md), a dostępnych systemów operacyjnych, znajdują się w [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](../cloud-services/cloud-services-guestos-update-matrix.md). Po utworzeniu puli, która zawiera węzły usług w chmurze Azure należy określić tylko rozmiar węzeł i jego "OS rodziny" znajdują się w artykułach opisanych powyżej. Dla pul systemu Windows obliczyć węzły, usług w chmurze jest najczęściej używane.

**Konfiguracja maszyn wirtualnych** przewiduje węzły obliczeń obrazów Linux i w systemie Windows. Rozmiary węzeł obliczeń dostępne są widoczne w [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) i [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Po utworzeniu puli, która zawiera węzły konfigurację maszyny wirtualnej, musisz określić rozmiar węzły, odwołanie obraz maszyn wirtualnych i agenta węzeł partii SKU musi być zainstalowany na węzłach.

### <a name="virtual-machine-image-reference"></a>Odwołanie maszyn wirtualnych

Usługa partii używa [Ustawia skali maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) o podanie węzły obliczeń Linux. Obrazy systemu operacyjnego dla tych maszyn wirtualnych są dostarczane przez [Azure Marketplace][vm_marketplace]. Po skonfigurowaniu odwołanie obraz maszyn wirtualnych, należy określić właściwości obrazu maszyn wirtualnych Marketplace. Po utworzeniu odwołanie obraz maszyn wirtualnych wymagane są następujące właściwości:

| **Właściwości odwołania obrazu** | **Przykład** |
| ----------------- | ------------------------ |
| Program Publisher         | Kanoniczny                |
| Oferty             | UbuntuServer             |
| JEDNOSTKA SKU               | 14.04.4-LTS              |
| Wersja           | najnowsze                   |

> [AZURE.TIP] Przedstawiono więcej informacji na temat tych właściwości i jak lista Marketplace obrazów w [Nawigacja i wybierz pozycję obrazy maszyn wirtualnych Linux Azure za pomocą programu PowerShell lub interfejsu wiersza polecenia](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Należy zauważyć, że nie wszystkie obrazy Marketplace są obecnie zgodne z partii. Aby uzyskać więcej informacji zobacz [agent węzeł SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Agent węzeł SKU

Agent węzeł partii jest program, który jest uruchamiany w każdym węźle puli i interfejs poleceń i kontroli między węzeł a usługą partię. Istnieją różne implementacji agentem węzeł znana pod nazwą wersji produktu, dla różnych systemów operacyjnych. Przede wszystkim podczas tworzenia konfiguracji maszyn wirtualnych najpierw określić odwołanie obraz maszyn wirtualnych, a następnie określ agenta węzeł zainstalować na obraz. Zazwyczaj każdego agenta węzeł, SKU jest zgodny z wielu obrazów maszyn wirtualnych. Oto kilka przykładów agenta węzeł wersji produktu:

* Batch.node.ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [AZURE.IMPORTANT] Nie wszystkie obrazy maszyn wirtualnych, które są dostępne na rynku są zgodne z dostępnych agentów węzeł partii. Należy użyć SDK partii do listy agenta dostępnego węzła wersji produktu i obrazy maszyn wirtualnych, z którymi są zgodne. Zobacz [listę maszyn wirtualnych obrazów](#list-of-virtual-machine-images) w dalszej części tego artykułu, aby uzyskać więcej informacji.

## <a name="create-a-linux-pool-batch-python"></a>Tworzenie puli Linux: Python wsadowe

Poniższy fragment kodu przedstawiono sposób używania [Biblioteki klienta partii Microsoft Azure, Python] [ py_batch_package] tworzenie puli serwera Ubuntu węzły obliczeń. Dokumentacji modułu Python partii można znaleźć w [pakiet azure.batch] [py_batch_docs] odczytu dokumenty.

Ten fragment tworzy [ImageReference] [ py_imagereference] jawnie i określić każdego z jego właściwości (wydawcę, oferty, SKU, wersji). W kodzie produkcyjnym, jednak zaleca się użycie [list_node_agent_skus] [ py_list_skus] metodę w celu określenia, a następnie wybierz z dostępnych obrazów i węzeł agenta SKU kombinacji w czasie rzeczywistym.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Jak już wspomniano, zalecamy zamiast [ImageReference] [ py_imagereference] jawnie, użyj [list_node_agent_skus] [ py_list_skus] metoda, aby wybrać dynamiczne z kombinacji obraz agent-Marketplace węzeł obecnie obsługiwane. Poniższy fragment Python przedstawiono użycie tej metody.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Tworzenie puli Linux: .NET partii

Poniższy fragment kodu przedstawiono sposób używania [.NET partii] [ nuget_batch_net] Biblioteka klienta, aby utworzyć puli serwera Ubuntu węzły obliczeń. Można znaleźć w [dokumentacji .NET partii] [ api_net] w witrynie MSDN.

Poniższy fragment kodu używa [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] metoda, aby wybrać z listy obecnie obsługiwane Marketplace obrazu i węzeł agenta SKU kombinacji. Ta metoda jest pożądane, ponieważ listy obsługiwanych kombinacji może ulec zmianie od czasu do czasu. Najczęściej dodawane są obsługiwane kombinacje.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Chociaż wstawkę kodu poprzedniego korzysta z [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] metody dynamicznie listy i wybierz z obsługiwane kombinacji SKU obrazu i węzeł agenta (zalecane), można również skonfigurować [ImageReference] [ net_imagereference] jawnie:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lista obrazów maszyn wirtualnych

W poniższej tabeli wymieniono obrazów maszyn wirtualnych Marketplace, które są zgodne z dostępnych agentów węzeł partii po ostatniej aktualizacji w tym artykule. Należy pamiętać, że ta lista nie jest ostateczne, ponieważ obrazy i czynników węzeł może dodać lub usunąć w dowolnym momencie. Zaleca się partii aplikacje i usługi zawsze używaj [list_node_agent_skus] [ py_list_skus] (Python) i [ListNodeAgentSkus] [ net_list_skus] (partii .NET), aby określić, a następnie wybierz z dostępnych wersji produktu.

> [AZURE.WARNING] Poniższa lista może się zmienić w dowolnym momencie. Zawsze używaj metody **agenta węzeł listy SKU** dostępne w partii interfejsy API do listy, a następnie wybierz z zgodne maszyn wirtualnych i agenta węzeł wersji produktu, po uruchomieniu programu zadań.

| **Program Publisher** | **Oferty** | **Obraz SKU** | **Wersja** | **Agent węzeł identyfikator SKU** |
| ------- | ------- | ------- | ------- | ------- |
| Kanoniczny | UbuntuServer | 14.04.0-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 14.04.1-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 14.04.2-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 14.04.3-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 14.04.4-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 14.04.5-LTS | najnowsze | Batch.node.ubuntu 14.04 |
| Kanoniczny | UbuntuServer | 16.04.0-LTS | najnowsze | Batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | najnowsze | Batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | najnowsze | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | najnowsze | Batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | najnowsze | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | najnowsze | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | najnowsze | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | najnowsze | Batch.node.opensuse 13.2 |
| SUSE | openSUSE przestępnych | 42.1 | najnowsze | Batch.node.opensuse 42.1 |
| SUSE | SLES HPC | 12 | najnowsze | Batch.node.opensuse 42.1 |
| SUSE | SLES | 12-Z DODATKIEM SP1 | najnowsze | Batch.node.opensuse 42.1 |
| Microsoft AD | standardowe — danych — nauki-maszyn wirtualnych | standardowe — danych — nauki-maszyn wirtualnych | najnowsze | Batch.node.Windows amd64 |
| Microsoft AD | Linux danych — nauki-maszyn wirtualnych | linuxdsvm | najnowsze | Batch.node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | najnowsze | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Centrum danych 2012 | najnowsze | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2 — centrum danych | najnowsze | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Technical Preview, systemu Windows Server w- | najnowsze | Batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Nawiązywanie połączenia z węzły Linux

Podczas opracowywania lub podczas rozwiązywania problemów może być konieczne zalogować się do węzłów na użytkownika. W odróżnieniu od węzły do uruchamiania systemu Windows można nawiązać węzły Linux za pomocą protokołu RDP (Remote Desktop). Zamiast tego usługa partii umożliwia dostęp SSH w każdym węźle połączenia zdalnego.

Poniższy fragment kodu Python tworzy użytkownika w każdym węźle pulę, która jest wymagane połączenia zdalnego. Informacje o połączeniu bezpiecznego shell (SSH) dla każdego węzła zostanie następnie wydrukowany.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Oto przykładowe dane wyjściowe poprzedniego kodu puli, która zawiera cztery węzły Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Należy zauważyć, że zamiast hasła, można określić klucz publiczny SSH po utworzeniu użytkownika w węźle. W zestawie SDK Python to przy użyciu parametru **ssh_public_key** na [ComputeNodeUser][py_computenodeuser]. W .NET to za pomocą [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] właściwości.

## <a name="pricing"></a>Cennik

Azure partii jest oparta na technologii usług w chmurze Azure i maszyn wirtualnych Azure. Sama usługa partii jest oferowane bezpłatnie, co oznacza, że zajmujesz tylko dla zasobów obliczeń że rozwiązanie wsadowe korzystać. Po wybraniu **Konfiguracja usług w chmurze**, zostanie naliczona oparte na [ceny usług w chmurze] [ cloud_services_pricing] struktury. Po wybraniu **Konfigurację maszyny wirtualnej**zostanie naliczona oparte na [ceny maszyn wirtualnych] [ vm_pricing] strukturę.

## <a name="next-steps"></a>Następne kroki

### <a name="batch-python-tutorial"></a>Samouczek Python wsadowe

Więcej informacji na temat samouczek dotyczący sposobu pracy z partii przy użyciu Python zapoznaj się [rozpocząć pracę z klientem Python wsadowe Azure](batch-python-tutorial.md). Jego companion [Przykładowy kod] [ github_samples_pyclient] zawiera funkcję Pomocnik `get_vm_config_for_distro`, przedstawiająca inną techniką uzyskanie konfigurację maszyny wirtualnej.

### <a name="batch-python-code-samples"></a>Przykłady kodu Python partii

Zapoznaj się z innymi [Python kod próbki] [ github_samples_py] w [azure partii prób] [ github_samples] repozytorium na GitHub kilka skryptów, które przedstawiono sposób wykonywania typowych operacji partię, takich jak puli, zadania i tworzenia zadań. [Plik README] [ github_py_readme] który towarzyszy Python próbki zawiera szczegółowe informacje dotyczące sposobu instalowania wymagane pakietów.

### <a name="batch-forum"></a>Forum partii

[Forum partii Azure] [ forum] w witrynie MSDN to dobre miejsce do omówienia partii oraz zadawać pytania dotyczące usługi. Przeczytaj wpisy pomocne "stickied", a ogłaszać pytania, gdy występują one podczas tworzenia rozwiązanie partię.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
