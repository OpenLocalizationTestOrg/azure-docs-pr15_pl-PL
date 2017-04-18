<properties
    pageTitle="Używanie Hadoop Oozie w HDInsight | Microsoft Azure"
    description="Za pomocą Hadoop Oozie w HDInsight, usługa duży danych. Dowiedz się, jak zdefiniować przepływu pracy Oozie i przesłać zadanie Oozie."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Za pomocą Oozie Hadoop można definiować i wykonywać przepływ pracy w HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Dowiedz się, jak za pomocą Apache Oozie definiowanie przepływu pracy i uruchomić przepływ pracy na HDInsight. Aby uzyskać informacje o koordynator Oozie, zobacz [Koordynator opartych na czasie Oozie Hadoop korzystanie z usługi HDInsight][hdinsight-oozie-coordinator-time]. Aby uzyskać Factory danych Azure, zobacz [Używanie i gałęzi z Factory danych][azure-data-factory-pig-hive].

Apache Oozie jest system przepływu pracy i można je zsynchronizować, która zarządza Hadoop zadania. Jest zintegrowany z stos Hadoop i obsługuje zadania Hadoop dla Apache MapReduce, świnka Apache gałęzi Apache i Apache Sqoop. Można również umożliwia za planowanie zadań, które są specyficzne dla systemu, takich jak programy Java lub skryptów powłoki.

Przepływ pracy, który wykona zgodnie z instrukcjami w tym samouczku zawiera dwie czynności:

![Diagram przepływu pracy][img-workflow-diagram]

1. Akcja gałęzi uruchamia skrypt HiveQL Zliczanie wystąpień każdego typu poziom rejestrowania w pliku log4j. Każdy plik log4j składa się z linią pola zawiera pole [poziom REJESTROWANIA], które zawiera typ i ważności, na przykład:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Dane wyjściowe skrypt gałęzi są podobne do:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Aby uzyskać więcej informacji na temat gałęzi, zobacz [Gałęzi korzystanie z usługi HDInsight][hdinsight-use-hive].

2.  Akcja Sqoop eksportuje dane wyjściowe HiveQL do tabeli w bazie danych programu Azure SQL. Aby uzyskać więcej informacji o Sqoop, zobacz [Hadoop Sqoop korzystanie z usługi HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Dla obsługiwanych wersji Oozie na klastrów HDInsight, zobacz [Co nowego w wersji klaster Hadoop dostarczony przez HDInsight?] [hdinsight-versions].

###<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Pracy z programem PowerShell Azure**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    Aby wykonać skrypty środowiska Windows PowerShell, możesz polecenie Uruchom jako administrator i ustawić zasady wykonywania *RemoteSigned*. Aby uzyskać więcej informacji, zobacz [skrypty środowiska Windows PowerShell Uruchom][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definiowanie przepływu pracy Oozie i powiązanych skrypt HiveQL

Oozie definicje przepływów pracy są zapisywane w hPDL (XML proces Definition Language). Domyślna nazwa pliku przepływ pracy jest *workflow.xml*. Poniżej przedstawiono plik przepływu pracy, który będzie używany w tym samouczku.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Istnieją dwie akcje zdefiniowane w przepływie pracy. Akcja do rozpoczęcia jest *RunHiveScript*. Jeśli akcja jest uruchamiany pomyślnie, kolejną czynnością jest *RunSqoopExport*.

RunHiveScript ma wiele zmiennych. Po przesłaniu zadania Oozie z miejscem pracy przy użyciu programu PowerShell Azure, przejdzie wartości.

<table border = "1">
<tr><th>Zmienne przepływu pracy</th><th>Opis</th></tr>
<tr><td>${jobTracker}</td><td>Określa adres URL śledzenia zadania Hadoop. Za pomocą <strong>jobtrackerhost:9010</strong> w wersji 3.0 i 2.1 HDInsight.</td></tr>
<tr><td>${nameNode}</td><td>Określa adres URL węzła nazwę Hadoop. Użyj domyślnego adresu system plików, na przykład <i>wasbs: / /&lt;Nazwa_kontenera&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${NazwaKolejki}</td><td>Nazwa kolejki, które zadania zostaną przesłane do. Użyj <strong>domyślnych</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Zmienna akcji gałęzi</th><th>Opis</th></tr>
<tr><td>${hiveDataFolder}</td><td>Określa katalog źródłowy polecenia gałęzi Create Table.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Określa folder wyjściowy instrukcji Wstawianie zastąpić.</td></tr>
<tr><td>${hiveTableName}</td><td>Nazwa tabeli gałęzi, która odwołuje się do plików danych log4j.</td></tr>
</table>

<table border = "1">
<tr><th>Zmienna akcji Sqoop</th><th>Opis</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Określa parametry połączenia bazy danych Azure SQL.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Umożliwia określenie tabeli bazy danych Azure SQL, do której zostaną wyeksportowane dane.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Określa folder wyjściowy instrukcji gałęzi Wstawianie zastąpić. To jest tego samego folderu do eksportowania Sqoop (Eksportuj dir).</td></tr>
</table>

Aby uzyskać więcej informacji na temat Oozie przepływu pracy i używania akcje przepływu pracy, zapoznaj się z [dokumentacją Apache Oozie 4.0] [ apache-oozie-400] (w przypadku HDInsight wersji 3.0) lub [dokumentacji Apache Oozie 3.3.2] [ apache-oozie-332] (w przypadku HDInsight wersji 2.1).


Plik skryptu HiveQL nawiąże połączenie z akcji gałęzi w przepływie pracy. Plik skryptu zawiera trzy instrukcje HiveQL:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **Instrukcja DROP TABLE** usuwa tabelę programu Hive log4j, jeśli istnieje.
2. **Instrukcja CREATE TABLE** tworzy tabelę zewnętrznych gałąź log4j się do lokalizacji pliku dziennika log4j. Ogranicznik pola jest ",". Domyślne ogranicznika wiersza jest "\n". Gałąź tabeli zewnętrznej jest używana w celu uniknięcia pliku danych zostanie usunięty z pierwotnej lokalizacji, jeśli chcesz uruchomić przepływ pracy Oozie wielokrotnie.
3. **Wstawianie zastąpić instrukcji** zliczenie wystąpień każdego typu poziom rejestrowania z tabeli gałąź log4j i zapisuje dane wyjściowe obiektów blob w magazynie Azure.


Istnieją trzy zmiennych skrypt:

- ${hiveTableName}
- ${hiveDataFolder}
- ${hiveOutputFolder}

Plik definicji przepływu pracy (workflow.xml w tym samouczku) przekazuje te wartości do tego skryptu HiveQL w czasie wykonywania.

Zarówno plik przepływu pracy i plik HiveQL są przechowywane w kontenerze obiektów blob.  Skrypt programu PowerShell, które będą w dalszej części tego samouczka skopiuje oba pliki do domyślnego konta miejsca do magazynowania. 

##<a name="submit-oozie-jobs-using-powershell"></a>Przesyłanie zadań Oozie przy użyciu programu PowerShell

Do definiowania zadań Oozie Azure programu PowerShell obecnie nie zapewnia dowolnego polecenia cmdlet. Polecenia cmdlet **Wywołaj RestMethod** umożliwia wywołania Oozie usług sieci web. Interfejs API usług sieci web Oozie jest interfejs API HTTP pozostałych JSON. Aby uzyskać więcej informacji o usługach sieci web Oozie interfejsu API, zapoznaj się z [dokumentacją Apache Oozie 4.0] [ apache-oozie-400] (w przypadku HDInsight wersji 3.0) lub [dokumentacji Apache Oozie 3.3.2] [ apache-oozie-332] (w przypadku HDInsight wersji 2.1).

Skrypt programu PowerShell w tej sekcji wykonuje następujące czynności:

1. Połącz ze Azure.
2. Tworzenie grupy usługi Azure zasobów. Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).
3. Tworzenie serwera bazy danych SQL Azure, bazy danych programu Azure SQL i dwie tabele. Są one używane przez akcję Sqoop w przepływie pracy.

    Nazwa tabeli jest *log4jLogCount*.

4. Utwórz klaster HDInsight do uruchomienia zadań Oozie.

    Aby zbadać klaster, możesz użyć Azure portal lub Azure programu PowerShell.

5. Skopiuj plik oozie przepływu pracy i plik skryptu HiveQL do domyślnego systemu plików.

    Oba pliki są przechowywane w kontenerze obiektów Blob publicznej.
    
    - Skopiuj skrypt HiveQL (useoozie.hql) do miejsca do magazynowania Azure (wasbs:///tutorials/useoozie/useoozie.hql).
    - Kopiowanie workflow.xml do wasbs:///tutorials/useoozie/workflow.xml.
    - Skopiuj plik danych (-example/data/sample.log) do wasbs:///tutorials/useoozie/data/sample.log.
     
6. Prześlij zadanie Oozie.

    Aby przejrzeć wyniki zadania OOzie, za pomocą programu Visual Studio lub innych narzędzi nawiązywania połączenia z bazą danych SQL Azure.

Oto skrypt.  Skrypt można uruchamiać z Windows PowerShell ISE. Musisz skonfigurować pierwsze 7 zmiennych.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
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
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
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
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
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
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
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
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>
    
    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>
    
    <property>
        <name>queueName</name>
        <value>default</value>
    </property>
    
    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>
    
    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>
    
    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>
    
    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>
    
    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>
    
    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>
    
    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>
    
    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Aby ponownie uruchomić samouczka**

Aby ponownie uruchomić przepływ pracy, należy usunąć następujące czynności:

- W pliku docelowym skrypt gałęzi
- Dane w tabeli log4jLogsCount

Oto przykładowy skrypt programu PowerShell, którego można używać:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Następne kroki
W tym samouczku wiesz, jak zdefiniować przepływu pracy Oozie i jak uruchomić zadanie Oozie przy użyciu programu PowerShell. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Użyj opartych na czasie Oozie koordynatora z usługi HDInsight][hdinsight-oozie-coordinator-time]
- [Wprowadzenie do korzystania z Hadoop z gałąź w HDInsight do analizy użycia słuchawki urządzeń przenośnych][hdinsight-get-started]
- [Magazyn obiektów Blob platformy Azure za pomocą usługi HDInsight][hdinsight-storage]
- [Administrowanie HDInsight przy użyciu programu PowerShell][hdinsight-admin-powershell]
- [Przekazywanie danych dla zadań Hadoop w HDInsight][hdinsight-upload-data]
- [Używanie Sqoop z Hadoop w HDInsight][hdinsight-use-sqoop]
- [Gałąź za pomocą Hadoop na HDInsight][hdinsight-use-hive]
- [Używanie świnka z Hadoop na HDInsight][hdinsight-use-pig]
- [Można opracowywać programy Java MapReduce dla HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
