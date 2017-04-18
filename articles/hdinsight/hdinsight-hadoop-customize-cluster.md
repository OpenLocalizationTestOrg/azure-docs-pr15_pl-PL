<properties
    pageTitle="Dostosowywanie przy użyciu akcji skryptu klastrów HDInsight | Microsoft Azure"
    description="Dowiedz się, jak dostosować klastrów HDInsight przy użyciu akcji skryptów."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Dostosowywanie przy użyciu akcji skryptu klastrów HDInsight systemu Windows

**Akcja skrypt** może służyć do wywołania [skrypty](hdinsight-hadoop-script-actions.md) w trakcie procesu tworzenia klaster dotyczące instalowania dodatkowego oprogramowania w klastrze.

Informacje w tym artykule są specyficzne dla klastrów HDInsight systemu Windows. W przypadku klastrów z systemem Linux zobacz [klastrów HDInsight systemem Linux dostosowywanie przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md). 

Klastrów HDInsight można dostosować na różne inne sposoby także, takich jak tym kolejne konta magazynu platformy Azure, zmieniając Hadoop plików konfiguracji (podstawowe site.xml, gałąź site.xml itp.) lub biblioteki (na przykład gałęzi, Oozie) dodawanie udostępnione do lokalizacji wspólnej w klastrze. Dostosowania może być wykonywane przez Azure programu PowerShell, Azure HDInsight .NET SDK lub Azure Portal. Aby uzyskać więcej informacji, zobacz [Tworzenie Hadoop klastrów w HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Akcja skrypt w procesie tworzenia klaster

Akcja skrypt jest używany tylko podczas klastrów jest w toku. Na poniższym diagramie przedstawiono, gdy skrypt akcja jest wykonywana w trakcie procesu tworzenia:

![Dostosowywanie klaster HDInsight i etapów podczas tworzenia klaster][img-hdi-cluster-states]

Uruchamiając skrypt, klaster wprowadza etapu **ClusterCustomization** . Na tym etapie skrypt jest uruchamiane przy użyciu konta administratora systemu, równolegle na określonych węzłach w klastrze i udostępnia uprawnienia administratora pełny w węzłach.

> [AZURE.NOTE] Ponieważ masz uprawnienia administratora w węzłach etapie **ClusterCustomization** służy skrypt do wykonywania operacji, takich jak zatrzymywania i uruchamiania usługi, w tym usługi związane z Hadoop. Tak, jako część skryptu należy się upewnić że usług Ambari i innych usług związanych z Hadoop i rozpocząć pracę przed skrypt zakończeniu. Te usługi są wymagane ustalenie pomyślnie zdrowia i stan klaster podczas jej tworzenia. Jeśli zmienisz dowolne konfiguracji klaster ma wpływ na tych usług, należy użyć funkcji pomocy, które są dostarczane. Aby uzyskać więcej informacji na temat funkcji Pomocnik zobacz [Skrypty można opracowywać Akcja skrypt do HDInsight][hdinsight-write-script].

Dane wyjściowe i dzienników błędów skryptu są przechowywane w domyślne konto miejsca do magazynowania, który określony jako klaster. Dzienniki znajdują się w tabeli o nazwie **u < \cluster-name-fragment >< \time-stamp > setuplog**. Są to agregacji dzienniki skryptu działa we wszystkich węzłach (węzła głównego i węzły pracownika) w grupie.

Każdy klaster może zaakceptować kilka akcji skryptu wywoływane w kolejności, w której są określone. Skrypt można uruchomiono na węzła głównego i/lub węzły pracownika.

Usługa HDInsight zawiera kilka skryptów w celu zainstalowania następujących składników na HDInsight klastrów:

Nazwa | Skrypt
----- | -----
**Instalowanie Spark** | https://hdiconfigactions.blob.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zobacz [Instalowanie i używanie wzmóc dotyczących klastrów HDInsight][hdinsight-install-spark].
**Instalowanie R** | https://hdiconfigactions.blob.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zobacz [Instalowanie i używanie R na klastrów HDInsight][hdinsight-install-r].
**Instalowanie Solr** | https://hdiconfigactions.blob.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zobacz [Instalowanie i używanie klastrów Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalowanie Giraph** | https://hdiconfigactions.blob.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zobacz [Instalowanie i używanie klastrów Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).
| **Wstępnie załadować bibliotek gałęzi** | https://hdiconfigactions.blob.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Zobacz [Dodawanie gałęzi biblioteki na klastrów HDInsight](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Nawiązywanie połączenia przy użyciu Azure Portal skryptów

**Z poziomu portalu Azure**

1. Rozpocznij tworzenie klastrze, zgodnie z opisem w [klastrów tworzenie Hadoop w HDInsight](hdinsight-provision-clusters.md#portal).
2. W obszarze opcjonalnym dla karta **Akcje skrypt** kliknij **Dodaj akcję skryptu** o podanie szczegółowych informacji o akcji skryptu, tak jak pokazano poniżej:

    ![Używanie akcji skrypt dostosowywania klastrze] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Używanie akcji skrypt dostosowywania klastrze")

    <table border='1'>
        <tr><th>Właściwość</th><th>Wartość</th></tr>
        <tr><td>Nazwa</td>
            <td>Określ nazwę akcji skryptów.</td></tr>
        <tr><td>Skrypt identyfikator URI</td>
            <td>Określ identyfikator URI skrypt, który jest wywoływana, aby dostosować klaster. s</td></tr>
        <tr><td>Głowy/pracownika</td>
            <td>Określ węzły (**głowy** lub **pracownika**) na jest uruchamiany skrypt dostosowywania. </b>.
        <tr><td>Parametry</td>
            <td>Określ parametry, jeśli jest to wymagane przez skrypt.</td></tr>
    </table>

    Naciśnij klawisz ENTER, aby dodać więcej niż jedną akcję skrypt do zainstalowania wielu składników w klastrze.

3. Kliknij przycisk **Wybierz** , aby zapisać konfigurację akcji skryptu i kontynuować tworzenie klaster.

## <a name="call-scripts-using-azure-powershell"></a>Nawiązywanie połączenia przy użyciu programu PowerShell Azure skryptów

Tego skryptu programu PowerShell pokazano, jak zainstalować Spark na podstawie klaster HDInsight systemu Windows.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Aby zainstalować inne oprogramowanie, należy zastąpić plik skryptu w skrypt:


Po wyświetleniu monitu wprowadź poświadczenia, aby klaster. Może potrwać kilka minut, zanim klaster jest tworzony.

## <a name="call-scripts-using-net-sdk"></a>Nawiązywanie połączenia przy użyciu zestawu SDK .NET skryptów 

Poniższy przykład pokazuje, jak zainstalować Spark na podstawie klaster HDInsight systemu Windows. Aby zainstalować inne oprogramowanie, należy zamienić plik skryptu w kodzie.

**Aby utworzyć klaster HDInsight z Spark** 

1. Tworzenie aplikacji konsoli C# w programie Visual Studio.
2. Z poziomu konsoli Menedżera pakietów Nuget uruchom następujące polecenie.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Użyć następującej składni w pliku Plik Program.cs przy użyciu instrukcji:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Umieścić kod klasy z następujących czynności:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Naciśnij klawisz **F5** , aby uruchomić aplikację.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Obsługa oprogramowania Otwórz źródło klastrów HDInsight
Usługa Microsoft Azure HDInsight jest elastyczną platformę, który umożliwia tworzenie aplikacji duży danych w chmurze za pomocą ekosystem technologii Otwórz źródło utworzone wokół Hadoop. Microsoft Azure przewiduje ogólne poziom obsługi technologii Otwórz źródło, zgodnie z opisem w sekcji **Zakres pomocy technicznej** <a href="http://azure.microsoft.com/support/faq/" target="_blank">witryny sieci Web często zadawane pytania dotyczące obsługi Azure</a>. Usługa HDInsight zawiera dodatkowy poziom obsługi dla niektórych elementów, zgodnie z poniższym opisem.

Istnieją dwa typy składników Otwórz źródło, które są dostępne w usłudze HDInsight:

- **Wbudowane składniki** — składniki te są preinstalowane w systemie klastrów HDInsight i zapewniają podstawowe funkcje klaster. Na przykład ResourceManager PRZĘDZY, języka kwerend programu Hive (HiveQL) i biblioteki Mahout należą do tej kategorii. Pełna lista obejmuje składniki klaster jest dostępna w [Co nowego w wersji klaster Hadoop dostarczony przez HDInsight?](hdinsight-component-versioning.md) </a>.
- **Niestandardowe składniki** —, jako użytkownik klaster, można zainstalować lub użyć w swojej obciążenie pracą jakikolwiek składnik dostępny we Wspólnocie lub utworzonych przez Ciebie.

Wbudowane składniki są w pełni obsługiwane i Microsoft Support pomogą izolowanie i rozwiązywanie problemów związanych z tych składników.

> [AZURE.WARNING] Składniki dostarczony z klastrem HDInsight są w pełni obsługiwane i Microsoft Support pomogą izolowanie i rozwiązywanie problemów związanych z tych składników.
>
> Niestandardowe składniki otrzymają komercyjnego rozsądne pomocy technicznej, aby pomóc rozwiązać ten problem. Może to spowodować w rozwiązaniu problemu lub pytaniem nawiązanie dostępnych kanałów technologiami Otwórz źródło miejsce, w którym znajduje się głębokości specjalizacji tej technologii. Na przykład, istnieje wiele witryn społeczności, których można używać, takich jak: [forum w witrynie MSDN HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mieć także witryn projektów na [http://apache.org](http://apache.org), na przykład: [Hadoop](http://hadoop.apache.org/), [iskrowy](http://spark.apache.org/).

Usługa HDInsight udostępnia kilka sposobów używania składników niestandardowych. Niezależnie od tego, jak składnik jest używany lub zainstalowane w klastrze dotyczy taki sam poziom pomocy technicznej. Poniżej znajduje się lista najczęściej używanych metod, że niestandardowe składniki mogą być używane na klastrów HDInsight:

1. Przesyłanie zadania - Hadoop albo innych typów zadań, których wykonanie lub za pomocą niestandardowych składników mogły być przesyłane z klastrem.
2. Dostosowywanie klaster - podczas tworzenia klaster, można określić dodatkowe ustawienia i niestandardowe składniki zainstalowane na węzłach.
3. Przykłady - popularne niestandardowe składniki, firmy Microsoft i inne osoby mogą udostępniać próbki sposobu użycia tych składników na klastrów HDInsight. Te przykłady są udostępniane bez pomocy technicznej.

## <a name="develop-script-action-scripts"></a>Projektowania skryptów akcję skryptu

Zobacz [Skrypty można opracowywać Akcja skrypt do HDInsight][hdinsight-write-script].


## <a name="see-also"></a>Zobacz też

- [Tworzenie klastrów Hadoop w HDInsight] [ hdinsight-provision-cluster] zawiera instrukcje dotyczące tworzenia klaster HDInsight przy użyciu innych opcji niestandardowych.
- [Projektowania skryptów Akcja skrypt do HDInsight][hdinsight-write-script]
- [Instalowanie i używanie Spark na klastrów HDInsight][hdinsight-install-spark]
- [Instalowanie i używanie R na klastrów HDInsight][hdinsight-install-r]
- [Instalowanie i używanie klastrów Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalowanie i używanie klastrów Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Etapy podczas tworzenia klaster"
