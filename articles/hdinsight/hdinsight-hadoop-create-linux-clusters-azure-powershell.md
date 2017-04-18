<properties
    pageTitle="Tworzenie klastrów Hadoop, HBase, Burza lub Spark na Linux w przy użyciu programu PowerShell Azure HDInsight | Microsoft Azure"
    description="Dowiedz się, jak utworzyć Hadoop, HBase, Burza lub Spark klastrów w Linux oraz dla HDInsight przy użyciu programu PowerShell Azure."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Tworzenie klastrów systemem Linux w HDInsight przy użyciu programu PowerShell Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell to zaawansowane środowisko skryptów, który służy do kontrolowania i automatycznego wdrożenia i zarządzania z obciążeń pracą w Microsoft Azure. Ten dokument zawiera informacje o tworzeniu klastrze systemem Linux HDInsight przy użyciu programu PowerShell Azure. Zawiera również przykładowy skrypt.

> [AZURE.NOTE] Azure programu PowerShell jest dostępna tylko w klientach systemu Windows. Jeśli używasz klienta Linux, Unix lub systemu Mac OS X, zobacz [Tworzenie klaster systemem Linux HDInsight za pomocą interfejsu wiersza polecenia Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md) uzyskać informacji o używaniu polecenie Azure, aby utworzyć klaster.

## <a name="prerequisites"></a>Wymagania wstępne
Należy dysponować następującymi przed rozpoczęciem tej procedury:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure programu PowerShell.
    Aby uzyskać więcej informacji o korzystaniu z usługi HDInsight Azure programu PowerShell zobacz [Administrowanie HDInsight przy użyciu programu PowerShell](hdinsight-administer-use-powershell.md). Aby uzyskać listę poleceń cmdlet programu Windows PowerShell usługi HDInsight zobacz [informacje dotyczące poleceń cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Tworzenie klastrów

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Aby utworzyć klaster HDInsight przy użyciu programu PowerShell Azure, należy wykonać następujące procedury:

- Tworzenie grupy usługi Azure zasobów
- Tworzenie konta magazynu platformy Azure
- Tworzenie kontenera obiektów Blob platformy Azure
- Utwórz klaster HDInsight

Dwie najważniejsze parametry, które należy ustawić na tworzenie klastrów Linux są te, których określić typ systemu operacyjnego i SSH szczegóły użytkownika:

- Upewnij się, że zostanie użyty parametr **- OSType** jako **Linux**.
- Aby użyć SSH zdalnej sesji programu klastrów, można określić hasła użytkownika SSH lub kluczem publicznym SSH. Jeśli użytkownik określi SSH hasło użytkownika i SSH klucz publiczny, klucz będą ignorowane. Jeśli chcesz użyć klucza SSH sesji zdalnego, należy określić pustego hasła SSH po wyświetleniu monitu o jedną. Aby uzyskać więcej informacji o korzystaniu z usługi HDInsight SSH zobacz jeden z następujących artykułów:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Poniższy skrypt pokazano, jak utworzyć nowy klaster:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Wartości, które można określić dla **$clusterCredentials** są używane do tworzenia konta użytkownika Hadoop dla klaster. Nawiązywanie połączenia z klastrem za pomocą tego konta.

Wartości, które można określić dla **$sshCredentials** są używane do tworzenia użytkownika SSH klaster. Konto służy do zdalnej sesji SSH w klastrze uruchomienia zadania.

> [AZURE.IMPORTANT] W tym skrypt należy określić liczby węzłów pracownika, które będą w klastrze. Jeśli zamierzasz użyć więcej niż 32 węzły pracownika (lub podczas tworzenia klaster przez skalowania klaster po utworzeniu), możesz także określić rozmiar węzła głównego z co najmniej 8 rdzeni i 14 GB pamięci RAM.
>
> Aby uzyskać więcej informacji na węzeł rozmiarów i koszty zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

Może potrwać do 20 minut, aby utworzyć klaster.

Poniższy przykład pokazuje, jak dodać konto dodatkowego miejsca do magazynowania:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Dostosowywanie klastrów

- Zobacz [Dostosowywanie HDInsight klastrów za pomocą uruchamiania](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Zobacz [klastrów HDInsight systemu Windows dostosowywanie przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Usuwanie klaster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy został pomyślnie utworzony klaster HDInsight, skorzystaj z następujących zasobów, aby dowiedzieć się, jak pracować z klaster.

### <a name="hadoop-clusters"></a>Klastrów Hadoop

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Klastrów HBase

* [Rozpoczynanie pracy z HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Można opracowywać aplikacje Java dla HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Klastrów Burza

* [Można opracowywać topologii Java dla Burza na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Używanie składników Python w Burza na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Wdrażanie i monitorowanie topologii z Burza na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Klastrów Spark

* [Tworzenie autonomiczną aplikację za pomocą Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadania w klastrze Spark przy użyciu Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)
* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)
