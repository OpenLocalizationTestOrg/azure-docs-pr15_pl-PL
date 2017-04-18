<properties
    pageTitle="Analizowanie danych opóźnienia lotów za Hadoop w HDInsight | Microsoft Azure"
    description="Dowiedz się, jak utworzyć klaster HDInsight, wykonywania zadania gałęzi, wykonywania zadania Sqoop i usunąć klaster za pomocą jednego skryptu programu Windows PowerShell."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analizowanie danych opóźnienia lotów przy użyciu gałąź w HDInsight

Gałąź umożliwia wykonywania zadań za pomocą skryptów języka SQL przypominających o nazwie Hadoop MapReduce * [HiveQL][hadoop-hiveql]*, które można stosować do podsumowywania, wyszukiwanie i analizowanie dużych ilości danych.

> [AZURE.NOTE] Kroki opisane w tym dokumencie wymagają klastrze HDInsight systemu Windows. Instrukcje, które współpracują z systemem Linux klaster zobacz [Analiza lotu opóźnienie danych przy użyciu gałąź w HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Jedną z głównych zalet Azure HDInsight polega na oddzielaniu magazynowanie danych i obliczeń. Usługa HDInsight używa magazyn obiektów Blob platformy Azure do przechowywania danych. Typowe zadania obejmuje trzy elementy:

1. **Przechowywanie danych w magazynie obiektów Blob platformy Azure.**  Na przykład pogodowe danych, dane czujnika dzienniki sieci web, a w tym przypadku danych opóźnienia lotów są zapisywane w magazyn obiektów Blob platformy Azure.
2. **Uruchamianie zadań.** Gdy nadchodzi czas przetwarzania danych, należy uruchomić skrypt programu Windows PowerShell (lub aplikacją kliencką) do tworzenia klaster HDInsight, zadań i usunąć klaster. Zadania Zapisz dane wyjściowe z magazynem obiektów Blob platformy Azure. Dane wyjściowe jest zachowywana nawet po klaster zostanie usunięty. W ten sposób możesz zapłacić tylko co możesz mieć zużyte.
3. **Pobieranie danych wyjściowych magazyn obiektów Blob platformy Azure**, lub w ramach tego samouczka, wyeksportuj dane do bazy danych programu Azure SQL.

Na poniższym diagramie przedstawiono scenariusza i struktury tego samouczka:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Uwaga**: numery na diagramie odpowiadają tytuły sekcji. **M** oznacza proces główny. **A** oznacza zawartość w dodatku.

Główne części samouczka pokazano, jak używać jednego skryptu programu Windows PowerShell do wykonywania następujących zadań:

- Utwórz klaster HDInsight.
- Uruchom zadanie gałęzi w klastrze do obliczenia średniej opóźnień na lotnisk. Dane opóźnienia lotów są przechowywane na koncie magazyn obiektów Blob platformy Azure.
- Uruchom zadanie Sqoop eksportowane do bazy danych programu Azure SQL zadania gałęzi.
- Usuń klaster HDInsight.

W dodatkach możesz znaleźć instrukcje dla przekazywania danych opóźnienia lotów, tworzenie i przekazywanie ciągu kwerendy gałęzi i przygotowywanie bazy danych Azure SQL zadania Sqoop.

> [AZURE.NOTE] Kroki opisane w tym dokumencie są specyficzne dla klastrów HDInsight systemu Windows. Instrukcje, które współpracują z systemem Linux klaster zobacz [danych opóźnienia lotów analiza przy użyciu gałąź w HDInsight (Linux).](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musi mieć następujące elementy:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Pliki używane w tym samouczku**

Samouczku wydajności na czas lotu samolotu danych z [badań i innowacyjnego Administracja technologii, Biuro statystyki transportu lub RICIE] [rita-website]. Kopiowanie danych zostało ono przekazane do kontenera magazyn obiektów Blob platformy Azure z uprawnieniami dostępu do publicznej obiektów Blob. Część skrypt programu PowerShell kopiuje dane w kontenerze obiektów blob publicznej do kontenera obiektów blob domyślne klaster. Skrypt HiveQL zostanie skopiowana również w tym samym kontenerze obiektów Blob. Jeśli chcesz dowiedzieć się, jak get przekazania danych do konta miejsca do magazynowania, jak tworzyć i przekaż plik skryptu HiveQL, zobacz [dodatek A](#appendix-a) i [B](#appendix-b).

W poniższej tabeli wymieniono pliki używane w tym samouczku:

<table border="1">
<tr><th>Pliki</th><th>Opis</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Plik skryptu HiveQL używana przez zadanie gałęzi. Ten skrypt zostało ono przekazane do konta magazyn obiektów Blob platformy Azure o dostępie. <a href="#appendix-b">Dodatek B</a> zawiera instrukcje dotyczące przygotowywania i przekazywaniu tego pliku do swojego konta magazyn obiektów Blob platformy Azure.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Danych wejściowych dla zadania gałęzi. Dane zostało ono przekazane do konta magazyn obiektów Blob platformy Azure o dostępie. <a href="#appendix-a">Dodatek A</a> zawiera instrukcje pobierania danych i przekazywania danych do swojego konta magazyn obiektów Blob platformy Azure.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Ścieżka danych wyjściowych dla zadania gałęzi. Domyślny kontener służy do przechowywania danych wyjściowych.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Gałąź folder stan zadania na domyślny kontener.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Klaster tworzenie i uruchamianie zadania gałęzi-Sqoop

Hadoop MapReduce jest przetwarzanie wsadowe. Najbardziej skutecznych sposobem wykonywania zadania gałąź jest utworzyć klaster dla zadania, a następnie usuń zadanie po zakończeniu zadania. Poniższy skrypt obejmuje całego procesu. Aby uzyskać więcej informacji na temat tworzenia klaster HDInsight i wykonywania zadań gałęzi, zobacz [Tworzenie Hadoop klastrów w HDInsight] [ hdinsight-provision] i [Gałęzi korzystanie z usługi HDInsight][hdinsight-use-hive].

**Aby wykonać kwerendy gałęzi przez Azure programu PowerShell**

1. Tworzenie bazy danych programu Azure SQL i tabeli wyników zadania Sqoop zgodnie z instrukcjami podanymi w [Dodatku C](#appendix-c).
3. Otwórz program Windows PowerShell ISE i uruchom następujący skrypt:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Nawiązywanie połączenia z bazą danych SQL i zobacz opóźnienia lotów średnia według miasta w tabeli AvgDelays:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Dodatek A - danych opóźnienia lotów przekazywania magazynem obiektów Blob platformy Azure
Przekazywanie pliku danych i plików skryptu HiveQL (zobacz [Dodatek B](#appendix-b)) wymaga, niektóre planowania. Koncepcja mają być przechowywane pliki danych i plik HiveQL przed utworzeniem klaster HDInsight i wykonywania zadania gałęzi. Dostępne są dwie opcje:

- **Za pomocą tego samego konta Azure miejsca do magazynowania, który będzie używany przez klaster HDInsight jako domyślnego systemu plików.** Ponieważ klaster HDInsight klucz dostępu do konta miejsca do magazynowania, nie musisz wprowadź ewentualne dodatkowe zmiany.
- **Użyj innego konta magazynu platformy Azure z usługi HDInsight klaster domyślnego systemu plików.** Jeśli jest to możliwe, należy zmodyfikować tworzenia część skrypt programu Windows PowerShell dostępny w [klaster HDInsight tworzenie i uruchamianie zadania gałęzi-Sqoop](#runjob) łączenie klienta miejsca do magazynowania jako konto dodatkowego miejsca do magazynowania. Aby uzyskać instrukcje, zobacz [Tworzenie Hadoop klastrów w HDInsight][hdinsight-provision]. Klaster HDInsight wie, następnie klawisz dostępu do konta miejsca do magazynowania.

>[AZURE.NOTE] Ścieżka magazyn obiektów Blob pliku danych jest trudne kodowane w pliku skryptu HiveQL. Musisz zaktualizować ją odpowiednio.

**Aby pobrać dane lotu**

1. Przejdź do [badań i technologii innowacyjnego Administracja Biuro statystyki transportu][rita-website].
2. Na stronie wybierz następujące wartości:

    <table border="1">
    <tr><th>Nazwa</th><th>Wartość</th></tr>
    <tr><td>Filtr rok</td><td>2013 </td></tr>
    <tr><td>Filtrowanie okresem.</td><td>Styczeń</td></tr>
    <tr><td>Pola</td><td>*Rok*, *flightdate (datalotu)*, *UniqueCarrier*, *przewoźnik*, *FlightNum*, *OriginAirportID*, *pochodzenia*, *OriginCityName*, *OriginState*, *DestAirportID*, *docelowy*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *największe*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (wyczyść pozostałe pola)</td></tr>
    </table>

3. Kliknij przycisk **Pobierz**.
4. Rozpakuj plik pliku do folderu **C:\Tutorials\FlightDelay\2013Data** . Każdy plik jest plik CSV i jest około 60GB.
5.  Zmień nazwę miesiąca, którą zawiera dane dla pliku. Na przykład plik zawierający dane stycznia może nosić nazwę *January.csv*.
6. Powtórz kroki 2 i 5 do pobrania pliku dla każdego z 12 miesięcy w 2013. Konieczne będzie co najmniej jeden plik, aby uruchomić samouczek.  

**Aby przekazać danych opóźnienia lotów magazynem obiektów Blob platformy Azure**

1. Przygotowywanie parametrów:

    <table border="1">
    <tr><th>Nazwa zmiennej</th><th>Notatki</th></tr>
    <tr><td>$storageAccountName</td><td>Miejsce, w którym chcesz przekazać dane, które mają konto Azure miejsca do magazynowania.</td></tr>
    <tr><td>$blobContainerName</td><td>Kontener obiektów Blob, której chcesz przekazać dane do.</td></tr>
    </table>
2. Otwórz Azure środowiska PowerShell ISE.
3. Wklej następujący skrypt do okienka Skrypt:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Naciśnij klawisz **F5** , aby uruchomić skrypt.

Jeśli chcesz skorzystać z innej metody dla przekazywania plików, upewnij się, że ścieżka pliku jest samouczki i flightdelay danych. Składnia do uzyskiwania dostępu do plików jest następująca:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Ścieżka samouczki i flightdelay dane są folder wirtualny utworzony po przekazaniu pliku. Upewnij się, że ma plików 12, po jednej dla każdego miesiąca.

>[AZURE.NOTE] Musisz zaktualizować kwerendy gałęzi do czytania z nowej lokalizacji.

> Kontener uprawnienia dostępu do publicznych lub powiązać konta miejsca do magazynowania z klastrem HDInsight albo należy skonfigurować. W przeciwnym razie ciągu kwerendy gałęzi nie będą mogli uzyskać dostęp do plików danych.

---
##<a id="appendix-b"></a>Dodatek B — tworzenie i przekazywanie skryptu HiveQL

Przy użyciu programu PowerShell Azure, można uruchomić instrukcje z wieloma HiveQL jeden pojedynczo lub pakowanie w pliku skryptu instrukcji HiveQL. W tej sekcji pokazano, jak utworzyć skrypt HiveQL i przekazać skrypt do magazyn obiektów Blob platformy Azure przy użyciu programu PowerShell Azure. Gałąź wymaga skryptów HiveQL mają być przechowywane w magazynie obiektów Blob platformy Azure.

Skrypt HiveQL będzie wykonywać następujące czynności:

1. **Usuwanie tabeli delays_raw**, w przypadku tabeli już istnieje.
2. **Tworzenie tabeli gałęzi zewnętrznych delays_raw** wskazująca lokalizacji przechowywania obiektów Blob z plikami opóźnienia lotów. Ta kwerenda określa, że pola są rozdzielone "," i zakończenia wierszy według "\n". Stanowi problem, gdy wartości pól zawiera przecinki, ponieważ gałąź nie można odróżnić przecinek, który jest ogranicznik pola, w którym znajduje się wartość pola (czyli w przypadku wartości pól pochodzenia\_MIASTA\_nazwę i DEST\_MIASTA\_nazwy). Aby rozwiązać ten, kwerenda tworzy TEMP kolumn w celu przechowywania danych, który jest niepoprawnie podzielony na kolumny.  
3. **Tabelę opóźnienia**, w przypadku tabeli już istnieje.
4. **Tworzenie tabeli opóźnienia**. Warto wyczyścić dane przed dalszego przetwarzania. Ta kwerenda tworzy nową tabelę, *opóźnienia*z tabeli delays_raw. Uwaga kolumn TEMP (jak już wspomniano) nie są kopiowane, oraz że funkcja **podciąg** jest używana do Usuwa znaki cudzysłowu z danych.
5. **Obliczyć opóźnienie średnia pogody i grupy wyniki według nazwy miasta.** Będzie również dane wyjściowe wyników, aby magazyn obiektów Blob. Zauważ, że kwerenda spowoduje usunięcie apostrofów danych i będzie wykluczyć wiersze, gdzie wartość **weather_delay** ma wartość null. Jest to konieczne, ponieważ Sqoop używane w dalszej części tego samouczka nie bezpiecznie obsługiwać te wartości domyślne.

Aby uzyskać pełną listę poleceń HiveQL, zobacz [Gałęzi Data Definition Language][hadoop-hiveql]. Każdego polecenia HiveQL musi zakończyć średnikami.

**Aby utworzyć plik skryptu HiveQL**

1. Przygotowywanie parametrów:

    <table border="1">
    <tr><th>Nazwa zmiennej</th><th>Notatki</th></tr>
    <tr><td>$storageAccountName</td><td>Miejsce, w którym chcesz przekazać skrypt HiveQL, aby konto Azure miejsca do magazynowania.</td></tr>
    <tr><td>$blobContainerName</td><td>Kontener obiektów Blob, gdzie chcesz przekazać skryptu HiveQL.</td></tr>
    </table>
2. Otwórz Azure środowiska PowerShell ISE.

3. Skopiuj i wklej następujący skrypt w okienku Skrypt:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Poniżej przedstawiono zmiennych skrypt:

    - **$hqlLocalFileName** - skrypt zapisze plik skryptu HiveQL lokalnie przed przesłaniem go z magazynem obiektów Blob. Jest to nazwa pliku. Wartość domyślna to <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** — to nazwa obiektów blob plik skryptu HiveQL używane w magazynie obiektów Blob platformy Azure. Wartość domyślna to tutorials/flightdelay/flightdelays.hql. Ponieważ plik zostanie zapisany bezpośrednio z magazynem obiektów Blob platformy Azure, nie jest "/" na początku nazwy obiektów blob. Jeśli chcesz uzyskać dostęp do pliku z magazynem obiektów Blob, będzie konieczne dodawanie "-" na początku nazwy pliku.
    - **$srcDataFolder** i **$dstDataFolder** - = "samouczki flightdelay-data" = "samouczki flightdelay wyjście"


---
##<a id="appendix-c"></a>Dodatek C - przygotowywanie bazy danych programu Azure SQL wyników zadania Sqoop
**Aby przygotować się do bazy danych SQL (korespondencji seryjnej to scenariusz Sqoop)**

1. Przygotowywanie parametrów:

    <table border="1">
    <tr><th>Nazwa zmiennej</th><th>Notatki</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Nazwa serwera bazy danych Azure SQL. Wprowadź nic, aby utworzyć nowy serwer.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Nazwa logowania na serwerze bazy danych Azure SQL. $SqlDatabaseServerName w przypadku istniejącego serwera, identyfikator logowania i hasło logowania są używane do uwierzytelniania na serwerze. W przeciwnym razie służą do tworzenia nowego serwera.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Hasło logowania na serwerze bazy danych Azure SQL.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Ta wartość jest używana tylko wtedy, gdy tworzysz nowy serwer Azure bazy danych.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Baza danych SQL użyte do utworzenia tabeli AvgDelays dla zadania Sqoop. Pozostawiając puste utworzy bazy danych o nazwie HDISqoop. Nazwa tabeli w wynikach zadania Sqoop jest AvgDelays. </td></tr>
    </table>
2. Otwórz Azure środowiska PowerShell ISE.
3. Skopiuj i wklej następujący skrypt w okienku Skrypt:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Skrypt używa usługi transfer (REST) przedstawicielskie stan, http://bot.whatismyipaddress.com, aby pobrać zewnętrzny adres IP. Adres IP służy do tworzenia reguły zapory dla Twojej bazy danych programu SQL server.  

    Poniżej przedstawiono niektóre zmiennych skrypt:

    - **$ipAddressRestService** — wartość domyślna to http://bot.whatismyipaddress.com. Jest publiczny adres IP usługi REST uzyskiwania zewnętrzny adres IP. Jeśli chcesz, możesz użyć innych usług. Zewnętrzny adres IP pobrane za pośrednictwem usługi będzie służyć do tworzenia reguły zapory, serwera bazy danych Azure SQL, aby umożliwić dostęp bazy danych w miejscu pracy (za pomocą skryptu programu Windows PowerShell).
    - **$fireWallRuleName** — jest to nazwa reguły zapory na serwerze bazy danych Azure SQL. Domyślna nazwa to <u>FlightDelay</u>. Jeśli chcesz, można ją zmienić.
    - **$sqlDatabaseMaxSizeGB** — ta wartość jest używana tylko wtedy, gdy tworzysz nowy serwer bazy danych Azure SQL. Wartość domyślna to 10GB. Do tego samouczka wystarczy 10GB.
    - **$sqlDatabaseName** — ta wartość jest używana tylko wtedy, gdy tworzysz nowej bazy danych Azure SQL. Wartość domyślna to HDISqoop. Jeśli zostanie zmieniona, należy zaktualizować skrypt programu Windows PowerShell Sqoop odpowiednio.

4. Naciśnij klawisz **F5** , aby uruchomić skrypt.
5. Sprawdź poprawność wyjściowe skrypt. Upewnij się, że skrypt działał poprawnie.

##<a id="nextsteps"></a>Następne kroki
Teraz wiesz, jak przekazać plik do magazyn obiektów Blob platformy Azure, jak wypełnić tabelę programu Hive przy użyciu danych z magazynem obiektów Blob platformy Azure, uruchamianie kwerendy gałęzi i jak za pomocą Sqoop eksportować dane z HDFS do bazy danych programu Azure SQL. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Wprowadzenie do usługi HDInsight][hdinsight-get-started]
* [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
* [Używanie Oozie z usługi HDInsight][hdinsight-use-oozie]
* [Używanie Sqoop z usługi HDInsight][hdinsight-use-sqoop]
* [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
* [Można opracowywać programy Java MapReduce dla HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
