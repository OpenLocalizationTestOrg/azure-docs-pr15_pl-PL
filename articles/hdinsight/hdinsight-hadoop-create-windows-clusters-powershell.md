<properties
   pageTitle="Tworzenie klastrów opartych na systemie Windows Hadoop w przy użyciu programu PowerShell Azure HDInsight | Microsoft Azure"
    description="Dowiedz się, jak utworzyć klastrów dla usługi Azure HDInsight przy użyciu programu PowerShell Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Tworzenie klastrów opartych na systemie Windows Hadoop w przy użyciu programu PowerShell Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Dowiedz się, jak utworzyć klastrów HDInsight przy użyciu programu PowerShell Azure. Azure PowerShell to modułu, który zawiera polecenia cmdlet, aby zarządzać Azure przy użyciu programu Windows PowerShell. Do tworzenia innych klaster narzędzia i funkcje kliknij pozycję Wybierz kartę u góry tej strony lub zobacz [metody tworzenia klaster](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Wymagania wstępne dotyczące:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Przed rozpoczęciem z instrukcjami podanymi w tym artykule, musisz mieć następujące czynności:

- Azure subskrypcji. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure programu PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Tworzenie klastrów
Azure PowerShell to zaawansowane środowisko skryptów, używanego do kontrolowania i automatycznego wdrożenia i zarządzania z obciążeń pracą w Azure. Ta sekcja zawiera instrukcje dotyczące tworzenia klaster HDInsight przy użyciu programu PowerShell Azure. Aby uzyskać informacje na temat konfigurowania pracy, aby uruchomić polecenia cmdlet programu Windows PowerShell HDInsight, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md). Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight Azure programu PowerShell zobacz [Administrowanie HDInsight przy użyciu programu PowerShell](hdinsight-administer-use-powershell.md). Aby uzyskać listę poleceń cmdlet programu Windows PowerShell usługi HDInsight zobacz [informacje dotyczące poleceń cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Poniższe procedury są potrzebne do utworzenia klaster HDInsight przy użyciu programu PowerShell Azure:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Tworzenie klastrów przy użyciu szablonu ARM

Za pomocą programu PowerShell Azure wdrożenia szablonu ARM, który tworzy klaster HDInsight.  Zobacz [Nawiązywanie połączenia przy użyciu programu PowerShell Azure szablonów](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Dostosowywanie klastrów

- Zobacz [Dostosowywanie HDInsight klastrów za pomocą uruchamiania](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Zobacz [klastrów HDInsight systemu Windows dostosowywanie przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Następne kroki
W tym artykule kiedy znasz już istnieje kilka sposobów tworzenia klaster HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Wprowadzenie do usługi HDInsight Azure](hdinsight-hadoop-linux-tutorial-get-started.md) — Dowiedz się, jak rozpocząć pracę z klaster HDInsight
* [Przesyłanie Hadoop zadań programowy](hdinsight-submit-hadoop-jobs-programmatically.md) — Dowiedz się, jak programowo przesyłać zadania do HDInsight
* [Zarządzanie Hadoop klastrów w HDInsight przy użyciu programu PowerShell](hdinsight-administer-use-powershell.md) — Dowiedz się, jak pracować z usługi HDInsight przy użyciu programu PowerShell Azure
* [Azure dokumentacji HDInsight SDK]  [ hdinsight-sdk-documentation] -wykrywania HDInsight SDK




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
