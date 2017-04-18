<properties
    pageTitle="Zarządzanie klastrów Hadoop w HDInsight przy użyciu programu PowerShell | Microsoft Azure"
    description="Dowiedz się, jak wykonywać zadania administracyjne dla klastrów Hadoop w HDInsight przy użyciu programu PowerShell Azure."
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
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Zarządzanie klastrów Hadoop w HDInsight przy użyciu programu PowerShell Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell to zaawansowane środowisko skryptów, używanego do kontrolowania i automatycznego wdrożenia i zarządzania z obciążeń pracą w Azure. W tym artykule dowiesz się, jak zarządzać klastrów Hadoop w Azure HDInsight za pomocą lokalnej konsoli programu PowerShell Azure za pomocą programu Windows PowerShell. Aby uzyskać listę poleceń cmdlet programu PowerShell usługi HDInsight, zobacz [informacje dotyczące poleceń cmdlet HDInsight][hdinsight-powershell-reference].



**Wymagania wstępne**

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##<a name="install-azure-powershell"></a>Instalowanie programu PowerShell Azure

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Jeśli zainstalowano Azure programu PowerShell wersji 0,9 x, należy go odinstalować przed zainstalowaniem nowszej wersji.

Aby sprawdzić wersję zainstalowanego programu PowerShell:

    Get-Module *azure*
    
Aby odinstalować starszą wersję, należy uruchomić programy i funkcje w Panelu sterowania. 


##<a name="create-clusters"></a>Tworzenie klastrów

Zobacz [systemem Linux oraz tworzenie klastrów w przy użyciu programu PowerShell Azure HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

##<a name="list-clusters"></a>Lista klastrów
Użyj następującego polecenia, aby wyświetlić listę wszystkich klastrów w bieżącej subskrypcji:

    Get-AzureRmHDInsightCluster

##<a name="show-cluster"></a>Pokazywanie klaster

Użyj następującego polecenia zawiera szczegółowe informacje dotyczące określonych klaster w bieżącej subskrypcji:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##<a name="delete-clusters"></a>Usuwanie klastrów

Aby usunąć klaster, użyj następującego polecenia:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Możesz również usunąć klaster, usuwając grupa zasobów, która zawiera klaster. Należy pamiętać, spowoduje to usunięcie wszystkich zasobów w grupie, w tym domyślne konto miejsca do magazynowania.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>
            
##<a name="scale-clusters"></a>Skala klastrów
Klaster skalowania funkcji umożliwia zmianę liczby węzłów pracownik używane przez klaster, na którym działa usługa Azure HDInsight bez konieczności ponownego tworzenia klaster.

>[AZURE.NOTE] Tylko klastrów z usługi HDInsight wersji 3.1.3 lub wyższej są obsługiwane. Jeśli masz pewności co do wersji klaster, można sprawdzić na stronie właściwości.  Zobacz [listę i Pokaż klastrów](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Wpływ zmian liczby węzłów danych dla każdego typu klaster obsługiwanych przez HDInsight:

- Hadoop

    Bezproblemowa można zwiększyć liczby węzłów pracownika w klastrze Hadoop, który jest uruchomiony bez wpływania zadania Oczekiwanie lub nie działa. Nowe zadania można również przesyłać w trakcie tej operacji. Błędy w operacji skalowania bezpiecznie są obsługiwane, tak aby klaster zawsze pozostanie w stanie działać.

    Podczas skalowania klastrze Hadoop w dół poprzez zmniejszenie liczby węzłów danych są ponownie uruchamiane niektóre z tych usług w klastrze. Spowoduje to wszystko działa i zaległe zadania kończy się niepowodzeniem po zakończeniu operacji skalowania. Można jednak Prześlij ponownie zadania po wykonaniu operacji.

- HBase

    Bezproblemowa można dodawać lub usunąć węzły klaster HBase, gdy jest uruchomiony. Serwery regionalne są automatycznie zbilansowane w ciągu kilku minut wykonywania operacji skalowania. Jednak można też ręcznie saldo serwery regionalne logowanie do headnode klastrze i uruchamiając następujące polecenia w oknie wiersza polecenia:

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

    ![Ponownego równoważenia skali Burza HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Oto przykład jak wyrównać topologii Burza przy użyciu polecenia polecenie:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Aby zmienić rozmiar klaster Hadoop przy użyciu programu PowerShell Azure, uruchom następujące polecenie na komputerze klienckim:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    

##<a name="grantrevoke-access"></a>Udzielanie i revoke programu access

Usługa HDInsight klastrów są następujące usługi sieci web HTTP (wszystkie te usługi mają RESTful punkty końcowe):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Domyślnie te usługi uzyskują dostęp. Możesz można revoke i udzielanie dostępu. Aby odebrać:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Aby udzielić:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
    
    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] Przez udzielanie i odwoływanie dostępu, możesz przywrócić klaster nazwa użytkownika i hasło.

Można to także zrobić za pośrednictwem portalu. Zobacz [Administrowanie HDInsight za pomocą portalu Azure][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>Aktualizowanie poświadczeń użytkownika HTTP

Jest tę samą procedurę [Grant/revoke HTTP](#grant/revoke-access)programu Access. Jeśli klaster przyznano dostęp HTTP, należy go najpierw odwołać.  A następnie udzielić dostępu za pomocą nowych poświadczeń użytkownika HTTP.


##<a name="find-the-default-storage-account"></a>Znajdowanie domyślne konto miejsca do magazynowania

Poniższy skrypt programu Powershell przedstawiono sposób uzyskiwania nazwę konta magazynu domyślnego i domyślny klucz konta miejsca do magazynowania dla klastrów.

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##<a name="find-the-resource-group"></a>Znajdowanie grup zasobów

W trybie Menedżera zasobów każdy klaster HDInsight należy do grupy Azure zasobów.  Aby znaleźć grupa zasobów:

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


##<a name="submit-jobs"></a>Przesyłanie zadań

**Aby przesłać MapReduce zadania**

Zobacz [Przykłady uruchamianie Hadoop MapReduce w HDInsight systemu Windows](hdinsight-run-samples.md).

**Aby przesłać gałęzi zadania** 

Zobacz [uruchomić gałęzi zapytań przy użyciu programu PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**Aby przesłać świnka zadania**

Zobacz [uruchomić świnka zadań przy użyciu programu PowerShell](hdinsight-hadoop-use-pig-powershell.md).

**Aby przesłać Sqoop zadania**

Zobacz [Sqoop korzystanie z usługi HDInsight](hdinsight-use-sqoop.md).

**Aby przesłać Oozie zadania**

Zobacz [Oozie korzystanie z Hadoop zdefiniować i uruchomić przepływ pracy programu HDInsight](hdinsight-use-oozie.md).

##<a name="upload-data-to-azure-blob-storage"></a>Przekazywanie danych z magazynem obiektów Blob platformy Azure
Zobacz [przekazywanie danych do HDInsight][hdinsight-upload-data].


## <a name="see-also"></a>Zobacz też
* [Usługa HDInsight polecenia cmdlet dokumentacji][hdinsight-powershell-reference]
* [Administrowanie za pomocą portalu Azure HDInsight][hdinsight-admin-portal]
* [Administrowanie HDInsight przy użyciu interfejsu wiersza polecenia][hdinsight-admin-cli]
* [Tworzenie klastrów HDInsight][hdinsight-provision]
* [Przekazywanie danych do HDInsight][hdinsight-upload-data]
* [Przesyłanie zadań Hadoop programowy][hdinsight-submit-jobs]
* [Wprowadzenie do usługi HDInsight platformy Azure][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
