<properties
    pageTitle="Zarządzanie klastrów Hadoop w HDInsight z .NET SDK | Microsoft Azure"
    description="Dowiedz się, jak wykonywać zadania administracyjne dla klastrów Hadoop w HDInsight przy użyciu zestawu SDK .NET HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Zarządzanie klastrów Hadoop w HDInsight przy użyciu zestawu SDK usługi .NET

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Dowiedz się, jak zarządzać klastrów HDInsight przy użyciu [Zestawu SDK HDInsight.NET](https://msdn.microsoft.com/library/mt271028.aspx).


**Wymagania wstępne**

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).


##<a name="connect-to-azure-hdinsight"></a>Nawiązywanie połączenia z usługa Azure HDInsight

Potrzebne są następujące pakiety Nuget:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

Poniższy przykład kodu pokazano, jak przed nawiązać połączenie Azure HDInsight klastrów można administrować w obszarze Azure subskrypcji.

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
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
        }
    }

Zobaczysz monit podczas uruchamiania tego programu.  Jeśli nie chcesz wyświetlać na monit, zobacz [Tworzenie uwierzytelniania - interactive .NET HDInsight aplikacji](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

##<a name="create-clusters"></a>Tworzenie klastrów

Zobacz [systemem Linux oraz tworzenie klastrów w HDInsight przy użyciu zestawu SDK .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

##<a name="list-clusters"></a>Lista klastrów

Poniższy fragment kodu wyświetla klastrów oraz niektóre właściwości:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

##<a name="delete-clusters"></a>Usuwanie klastrów

Usuwanie klastrze synchronicznie lub asynchroniczne przy użyciu następujący fragment kodu: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
            
##<a name="scale-clusters"></a>Skala klastrów
Klaster skalowania funkcji umożliwia zmianę liczby węzłów pracownik używane przez klaster, na którym działa usługa Azure HDInsight bez konieczności ponownego tworzenia klaster.

>[AZURE.NOTE] Tylko klastrów z usługi HDInsight wersji 3.1.3 lub wyższej są obsługiwane. Jeśli masz pewności co do wersji klaster, można sprawdzić na stronie właściwości.  Zobacz [listę i Pokaż klastrów](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Wpływ zmian liczby węzłów danych dla każdego typu klaster obsługiwanych przez HDInsight:

- Hadoop

    Bezproblemowa można zwiększyć liczby węzłów pracownika w klastrze Hadoop, który jest uruchomiony bez wpływania zadania Oczekiwanie lub nie działa. Nowe zadania można również przedstawiane w trakcie tej operacji. Błędy w operacji skalowania bezpiecznie są obsługiwane, tak aby klaster zawsze pozostanie w stanie działać.

    Podczas skalowania klastrze Hadoop w dół poprzez zmniejszenie liczby węzłów danych są ponownie uruchamiane niektóre z tych usług w klastrze. Spowoduje to wszystko działa i zaległe zadania kończy się niepowodzeniem po zakończeniu operacji skalowania. Można jednak Prześlij ponownie zadania po wykonaniu operacji.

- HBase

    Bezproblemowa można dodawać lub usunąć węzły klaster HBase, gdy jest uruchomiony. Serwery regionalne są automatycznie zbilansowane w ciągu kilku minut wykonywania operacji skalowania. Jednak można też ręcznie saldo serwery regionalne, logując się do headnode klastrze i uruchomione następujące polecenia w oknie wiersza polecenia:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Burza

    Można bezproblemowo dodawać i usuwać węzłów danych do klaster burza, gdy jest uruchomiony. Ale po pomyślnym zakończeniu operacji skalowania, będzie konieczne wyrównać topologii.

    Ponowne równoważenie można zrobić na dwa sposoby:

    * Interfejs użytkownika sieci web Burza
    * Narzędzie interfejsu wiersza polecenia (polecenie)

    Można znaleźć w [dokumentacji Burza Apache](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) uzyskać więcej szczegółowych informacji.

    Interfejs użytkownika sieci web Burza jest dostępna w klastrze HDInsight:

    ![ponownego równoważenia skali Burza hdinsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Oto przykład jak wyrównać topologii Burza przy użyciu polecenia polecenie:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Poniższy fragment kodu przedstawia sposób zmieniania rozmiaru klastrze synchronicznie lub asynchroniczne:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    

##<a name="grantrevoke-access"></a>Udzielanie i revoke programu access

Usługa HDInsight klastrów są następujące usługi sieci web HTTP (wszystkie te usługi mają RESTful punkty końcowe):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Domyślnie te usługi uzyskują dostęp. Możesz można revoke i udzielanie dostępu. Aby odebrać:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

Aby udzielić:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


>[AZURE.NOTE] Przez udzielanie i odwoływanie dostępu, możesz przywrócić klaster nazwa użytkownika i hasło.

Można to także zrobić za pośrednictwem portalu. Zobacz [Administrowanie HDInsight przy użyciu Azure Portal][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>Aktualizowanie poświadczeń użytkownika HTTP

Jest tę samą procedurę [Grant/revoke HTTP](#grant/revoke-access)programu Access. Jeśli klaster przyznano dostęp HTTP, należy go najpierw odwołać.  A następnie udzielić dostępu za pomocą nowych poświadczeń użytkownika HTTP.


##<a name="find-the-default-storage-account"></a>Znajdowanie domyślne konto miejsca do magazynowania

Poniższy fragment kodu przedstawiono sposób uzyskiwania nazwę konta magazynu domyślnego i domyślny klucz konta miejsca do magazynowania dla klastrów.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


##<a name="submit-jobs"></a>Przesyłanie zadań

**Aby przesłać MapReduce zadania**

Zobacz [Przykłady uruchamianie Hadoop MapReduce w HDInsight](hdinsight-hadoop-run-samples-linux.md).

**Aby przesłać gałęzi zadania** 

Zobacz [uruchomić gałęzi zapytań przy użyciu zestawu SDK .NET](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**Aby przesłać świnka zadania**

Zobacz [uruchomić świnka zadań przy użyciu zestawu SDK usługi .NET](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**Aby przesłać Sqoop zadania**

Zobacz [Sqoop korzystanie z usługi HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**Aby przesłać Oozie zadania**

Zobacz [Oozie korzystanie z Hadoop zdefiniować i uruchomić przepływ pracy programu HDInsight](hdinsight-use-oozie-linux-mac.md).

##<a name="upload-data-to-azure-blob-storage"></a>Przekazywanie danych z magazynem obiektów Blob platformy Azure
Zobacz [przekazywanie danych do HDInsight][hdinsight-upload-data].


## <a name="see-also"></a>Zobacz też
* [Usługa HDInsight .NET SDK dokumentacji](https://msdn.microsoft.com/library/mt271028.aspx)
* [Administrowanie przy użyciu Azure Portal HDInsight][hdinsight-admin-portal]
* [Administrowanie HDInsight przy użyciu interfejsu wiersza polecenia][hdinsight-admin-cli]
* [Tworzenie klastrów HDInsight][hdinsight-provision]
* [Przekazywanie danych do HDInsight][hdinsight-upload-data]
* [Wprowadzenie do usługi HDInsight platformy Azure][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


