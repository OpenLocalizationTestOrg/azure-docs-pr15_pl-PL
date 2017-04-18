<properties
    pageTitle="Używanie Hadoop Sqoop w HDInsight | Microsoft Azure"
    description="Dowiedz się, jak za pomocą programu PowerShell Azure na stacji roboczej uruchom Sqoop importowanie i eksportowanie między klaster Hadoop i bazy danych programu Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Używanie Sqoop z Hadoop w HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Dowiedz się, jak używać Sqoop w HDInsight do importowania i eksportowania między HDInsight klaster i baza danych Azure SQL lub bazy danych programu SQL Server.

Mimo że Hadoop jest naturalne wybór przetwarzania danych semistrukturalne i niestrukturalne, takie jak dzienniki i pliki, może również być potrzebne przetwarzania danych strukturalnych, który jest przechowywany w relacyjnych baz danych.

[Sqoop] [ sqoop-user-guide-1.4.4] to narzędzie przeznaczone do przenoszenia danych między klastrów Hadoop i relacyjnych baz danych. Jego umożliwia importowanie danych z relacyjnej bazy danych systemu zarządzania (RDBMS), takie jak SQL Server, MySQL lub Oracle do distributed system plików usługi Hadoop (HDFS), przekształcania danych w Hadoop MapReduce lub gałęzi, a następnie wyeksportuj dane do RDBMS. W tym samouczku używasz bazy danych programu SQL Server dla relacyjnej bazy danych.

Dla wersji Sqoop, które są obsługiwane w klastrów HDInsight, zobacz [Co nowego w wersji klaster dostarczony przez HDInsight?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Opis tego scenariusza

Klaster HDInsight zawiera kilka przykładowych danych. Będą używane następujące dwie próbki:

- Log4j plik dziennika, który znajduje się w */example/data/sample.log*. Następujące pliki dziennika są wyodrębniane z pliku:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Gałąź tabeli o nazwie *hivesampletable*, która odwołuje się do pliku danych znajdują się w */hive/warehouse/hivesampletable*. Tabela zawiera danych na urządzeniu przenośnym. 

  	| Pole | Typ danych |
  	| ----- | --------- |
  	| ClientID | ciąg |
  	| querytime | ciąg |
  	| rynku | ciąg |
  	| deviceplatform | ciąg |
  	| devicemake | ciąg |
  	| devicemodel | ciąg |
  	| Województwo | ciąg |
  	| kraj | ciąg |
  	| querydwelltime | podwójne |
  	| Identyfikator sesji | bigint |
  	| sessionpagevieworder | bigint |

Zostanie najpierw wyeksportować *sample.log* i *hivesampletable* z bazą danych Azure SQL lub SQL Server, a następnie zaimportuj tabelę, która zawiera dane urządzenia przenośnego do usługi HDInsight przy użyciu następującą ścieżkę:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Tworzenie klaster i baza danych SQL

W tej sekcji pokazano, jak utworzyć klaster i schematy bazy danych SQL do uruchamiania samouczka przy użyciu Azure portal i szablonu Azure Menedżera zasobów.  Jeśli wolisz używać Azure programu PowerShell, zobacz [dodatek A](#appendix-a---a-powershell-sample).

1. Kliknij, aby otworzyć szablon Menedżera zasobów w Azure Portal następujące obraz.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Szablon Menedżera zasobów znajduje się w kontenerze publicznej obiektów blob, *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    Szablon Menedżera zasobów połączeń pakiet Plecak wdrożenia schematy tabeli do bazy danych SQL.  Pakiet Plecak znajduje się również w kontenerze publicznej obiektów blob, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Jeśli chcesz użyć dla plików Plecak Kontener prywatny, użyj następujące wartości w szablonie:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Z karta Parametry wprowadź następujące informacje:

    - **NazwaKlastra**: Wprowadź nazwę klaster Hadoop, który ma zostać utworzony.
    - **Klaster nazwę logowania i hasło**: domyślna nazwa logowania to administrator.
    - **SSH nazwy użytkownika i hasła**.
    - **Nazwa logowania serwera bazy danych SQL i hasło**.

    Ustalony w sekcji zmienne są następujące wartości:
    
  	|Nazwę konta magazynu domyślnego|<CluterName>przechowywanie|
  	|----------------------------|-----------------|
  	|Nazwa serwera bazy danych SQL Azure|<ClusterName>dbserver|
  	|Nazwa bazy danych SQL Azure|<ClusterName>bazy danych|
    
    Zapisz te wartości.  Należy je w dalszej części samouczka.
    
3.w kliknij **przycisk OK** , aby zapisać parametry.

4. z karta **wdrożenia niestandardowe** kliknij pole listy rozwijanej **Grupa zasobów** , a następnie kliknij **Nowy** , aby utworzyć nową grupę zasobów. Grupa zasobów jest kontenerem grupującego klastrem, konto zależne miejsca do magazynowania i innych zasobów połączonych.

5 kliknij **warunki prawne**, a następnie kliknij przycisk **Utwórz**.

6 kliknij przycisk **Utwórz**. Zostanie wyświetlony fragment zatytułowany wdrożenia Submitting wdrożenia szablonu. Wystarczy o około 20 minut utworzyć klaster i baza danych SQL.

Jeśli zdecydujesz się na użycie istniejącej bazy danych Azure SQL lub Microsoft SQL Server

- **Baza danych Azure SQL**: należy skonfigurować reguły zapory dla serwera bazy danych Azure SQL umożliwić dostęp w miejscu pracy. Aby uzyskać instrukcje dotyczące tworzenia Azure SQL bazy danych i konfigurowanie zapory, zobacz [Rozpoczynanie pracy z bazą danych Azure SQL][sqldatabase-get-started]. 

    > [AZURE.NOTE] Domyślnie bazy danych programu Azure SQL zezwala na połączenia z Azure usług, takich jak usługa Azure HDInsight. Jeśli ustawienie Zapora jest wyłączone, jeśli włączono go z portalu Azure. Aby uzyskać instrukcje dotyczące tworzenia bazy danych programu Azure SQL i konfigurowania reguły zapory, zobacz [Tworzenie i Konfigurowanie bazy danych SQL][sqldatabase-create-configue].

- **Program SQL Server**: klaster HDInsight znajduje się w tej samej sieci wirtualnej platformy Azure co program SQL Server, można wykonaj czynności podane w tym artykule, importowanie i eksportowanie danych do bazy danych programu SQL Server.

    > [AZURE.NOTE] Usługa HDInsight obsługuje tylko opartych na lokalizacji wirtualnych sieci, a obecnie działa z oparte na grupach koligacji wirtualnych sieci.

    * Aby utworzyć i skonfigurować wirtualnej sieci, zobacz [Tworzenie wirtualnych sieci przy użyciu Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Gdy korzystasz z programu SQL Server w centrum danych, wirtualną sieć należy skonfigurować jako *witryny do witryny* lub *punkt do witryny*.

            > [AZURE.NOTE] **Punkt do witryny** wirtualnych sieci programu SQL Server musi być zainstalowany klienta VPN aplikację konfiguracji, która jest dostępna z **pulpitu nawigacyjnego** konfiguracji Azure wirtualnej sieci.

        * Podczas korzystania z programu SQL Server na Azure maszyn wirtualnych żadnej konfiguracji wirtualną sieć umożliwia maszyny wirtualnej hostingu programu SQL Server jest członkiem wirtualnej sieci HDInsight.

    * Aby utworzyć klaster HDInsight w wirtualnej sieci, zobacz [Tworzenie Hadoop klastrów w HDInsight za pomocą opcji niestandardowych](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] Program SQL Server należy także zezwolić uwierzytelniania. Wykonaj kroki opisane w tym artykule należy użyć logowania programu SQL Server.


## <a name="run-sqoop-jobs"></a>Uruchamianie Sqoop zadań

Usługa HDInsight można uruchamiać zadania Sqoop za pomocą różnych metod. Skorzystaj z poniższej tabeli zdecydować, której metody jest dla Ciebie, a następnie kliknij łącze, aby uzyskać instrukcje.

| **Użyj tego ustawienia** , jeśli chcesz...                                   | .. .an **interakcyjnych** powłoki | ... Przetwarzanie **wsadowe** | .. .with **klaster system operacyjny** | .. .from **system operacyjny klienta** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Zestaw SDK programu .NET dla Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Windows (na razie)                        |
| [Azure programu PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Systemu Windows                                  |

##<a name="limitations"></a>Ograniczenia

* Zbiorcze export - systemem Linux z usługi HDInsight, łącznik Sqoop umożliwia eksportowanie danych do programu Microsoft SQL Server lub bazy danych SQL Azure nie obsługuje obecnie wstawia zbiorczo.

* Tworzeniu partii — z systemem Linux HDInsight podczas korzystania z `-batch` przełączanie podczas wykonywania instrukcji INSERT, Sqoop wykona wielu wstawia zamiast tworzeniu partii operacje wstawiania.

##<a name="next-steps"></a>Następne kroki

Teraz zapoznaniu umożliwia Sqoop. Aby uzyskać więcej informacji, zobacz:

- [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
- [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
- [Korzystanie z usługi HDInsight Oozie][hdinsight-use-oozie]: Sqoop użyj akcji w przepływie pracy Oozie.
- [Analizowanie danych opóźnienia lotów przy użyciu HDInsight][hdinsight-analyze-flight-data]: używanie gałęzi do analizowania lotu opóźnienia danych, a następnie użyj Sqoop Aby wyeksportować dane do bazy danych programu Azure SQL.
- [Przekazywanie danych do HDInsight][hdinsight-upload-data]: znajdowanie innych metod przekazywania danych z magazynem obiektów Blob usługi HDInsight/Azure.


## <a name="appendix-a---a-powershell-sample"></a>Dodatek A - próbki programu PowerShell

Przykładowy programu PowerShell wykonuje następujące czynności:

1. Połącz ze Azure.
2. Tworzenie grupy usługi Azure zasobów. Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)
3. Tworzenie serwera bazy danych SQL Azure, bazy danych programu Azure SQL i dwie tabele. 

    Jeśli zamiast tego użyj programu SQL Server umożliwiają tworzenie tabel następujące instrukcje:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    Najprostszym sposobem na sprawdzenie bazę danych i tabele jest za pomocą programu Visual Studio. Serwer bazy danych i bazy danych może sprawdzić za pomocą portalu Azure.

4. Utwórz klaster HDInsight.

    Aby zbadać klaster, możesz użyć Azure portal lub Azure programu PowerShell.

5. Wstępne przetwarzanie pliku danych źródłowych.

    W tym samouczku spowoduje wyeksportowanie pliku dziennika log4j (rozdzielany plik) i tabelę programu Hive z bazą danych programu Azure SQL. Plik rozdzielany nosi nazwę */example/data/sample.log*. Wcześniej w samouczku pokazano kilka przykładów log4j dzienniki. W pliku dziennika istnieją pewne puste wiersze i niektóre linie podobne do następujących:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    To jest prawidłowy dla inne przykłady użycia tych danych, ale przed możemy można zaimportować do bazy danych Azure SQL lub SQL Server firma Microsoft należy usunąć te wyjątki. Eksportowanie Sqoop zakończy się niepowodzeniem, jeśli występuje ciąg pusty wiersz z mniejszą liczbę elementów niż liczba pól zdefiniowanych w tabeli bazy danych Azure SQL. Tabela log4jlogs zawiera 7 pola typu ciąg.

    Ta procedura tworzy nowy plik w klastrze: tutorials/usesqoop/data/sample.log. Aby zbadać pliku modyfikacji danych, można użyć Azure portal, narzędzia Eksploratora magazyn Azure lub Azure programu PowerShell. [Wprowadzenie do usługi HDInsight] [ hdinsight-get-started] zawiera przykładowy kod do przy użyciu programu PowerShell Azure pobierania pliku i wyświetlić zawartość pliku.

6. Eksportowanie pliku danych do bazy danych Azure SQL.

    Plik źródłowy jest tutorials/usesqoop/data/sample.log. Miejsce, w którym dane są eksportowane do tabeli jest nazywane log4jlogs.
    
    > [AZURE.NOTE] Oprócz informacje parametrów połączenia czynności opisane w tej sekcji powinny działać dla bazy danych programu Azure SQL lub SQL Server. Poniższe czynności, zostały przetestowane przy użyciu następującej konfiguracji:
    >
    > * **Konfigurowanie punktu do witryny Azure sieci wirtualnej**: wirtualnej sieci połączony klaster HDInsight programu SQL Server w prywatnej centrum danych. Aby uzyskać więcej informacji, zobacz [Konfigurowanie sieci VPN punktu do witryny, w portalu zarządzania](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure HDInsight 3.1**: zobacz [Tworzenie Hadoop klastrów w HDInsight za pomocą opcji niestandardowe](hdinsight-provision-clusters.md) informacje o tworzeniu klaster w wirtualnej sieci.
    > * **Program SQL Server 2014**: skonfigurowaną umożliwia uwierzytelnianie i uruchamianie klienta VPN konfiguracji pakietu bezpieczne łączenie z wirtualnej sieci.

7. Eksportowanie tabeli gałęzi z bazą danych Azure SQL.

8. Importowanie tabeli mobiledata do klastrów HDInsight.

    Aby zbadać pliku modyfikacji danych, można użyć Azure portal, narzędzia Eksplorator magazyn Azure lub Azure programu PowerShell.  [Wprowadzenie do usługi HDInsight] [ hdinsight-get-started] zawiera przykłady kodu dotyczące przy użyciu programu PowerShell Azure, aby pobrać plik i wyświetlić zawartość pliku.


### <a name="the-powershell-sample"></a>Przykładowy programu PowerShell

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
