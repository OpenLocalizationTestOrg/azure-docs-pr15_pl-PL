<properties
   pageTitle="Tworzenie klastrów HDInsight z magazynu Lake danych Azure przy użyciu programu PowerShell | Azure"
   description="Tworzenie i używanie klastrów HDInsight z Lake danych Azure za pomocą programu PowerShell Azure"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Tworzenie klastrze HDInsight z magazynu Lake danych przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Za pomocą portalu](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Przy użyciu programu PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Korzystanie z Menedżera zasobów](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Dowiedz się, jak skonfigurować klaster HDInsight (Hadoop, HBase lub Burza) z dostępem do magazynu Lake danych Azure za pomocą programu PowerShell Azure. Ważne informacje dotyczące tej wersji:

* **Klastrów dla Spark (Linux) i klastrów Hadoop-Burza (Windows i Linux)**, magazynu Lake danych można używać tylko jako konto dodatkowego miejsca do magazynowania. Domyślne konto miejsca do magazynowania dla klastrów takie będą nadal Azure magazyn obiektów blob (WASB).

* **HBase dla klastrów (Windows i Linux)**, magazynu Lake danych może służyć jako domyślnego miejsca do magazynowania lub dodatkowego miejsca do magazynowania.

> [AZURE.NOTE] Kilka ważnych uwag do noty. 
> 
> * Opcja tworzenia HDInsight klastrów z dostępem do magazynu Lake danych jest dostępna tylko w przypadku wersji HDInsight 3,2 i 3.4 (w przypadku Hadoop, HBase i Burza klastrów na systemie Windows, a także Linux). Dla klastrów Spark w systemie Linux ta opcja jest dostępna na klastrów HDInsight 3.4.
>
> * Jak wcześniej wspomniano, magazynu Lake danych jest dostępna jako domyślnego miejsca do magazynowania dla niektórych typów klaster (HBase) i dodatkowego miejsca do magazynowania dla innych typów klaster (Hadoop, Spark Burza). Przy użyciu magazynu Lake danych przy użyciu konta dodatkowego miejsca do magazynowania nie ma wpływu na wydajność lub możliwość odczytu/zapisu do magazynu w grupie. Scenariusz z używania magazynu Lake danych jako dodatkowego miejsca do magazynowania związanych z klastrem pliki (na przykład dzienniki itp.) są zapisywane w do domyślnego magazynu (BLOB Azure), podczas przetwarzania danych mogą być przechowywane w konta magazynu Lake danych.


W tym artykule firma Microsoft obsługi administracyjnej klastrze Hadoop z magazynu Lake danych jako dodatkowego miejsca do magazynowania.

Konfigurowanie usługi HDInsight do pracy z magazynu Lake danych przy użyciu programu PowerShell obejmuje następujące kroki:

* Tworzenie magazynu Lake danych Azure
* Konfigurowanie uwierzytelniania opartego na rolach dostępu do magazynu Lake danych
* Tworzenie HDInsight klaster z uwierzytelnianiem w magazynie Lake danych
* Uruchamianie zadania testowego w klastrze

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell wersji 1.0 lub nowszej**. Dowiedz się, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

- **Zestaw SDK systemu Windows**. [Tutaj](https://dev.windows.com/en-us/downloads)możesz go zainstalować. Umożliwia to tworzenie certyfikatu zabezpieczeń.

- **Azure Active Directory usługi kapitału**. Kroki opisane w tym samouczku podano instrukcje dotyczące tworzenia wystawcy usługi w Azure AD. Jednak musisz być administratorem Azure AD, aby można było utworzyć wystawcy usługi. Jeśli jesteś administratorem Azure AD, możesz pominąć te wymagania wstępne i kontynuować samouczek.
    
    **Jeśli nie jesteś administratorem Azure AD**, nie można wykonać czynności wymagane do utworzenia wystawcy usługi. W takim przypadku administrator Azure AD należy najpierw utworzyć wystawcy usługi przed utworzeniem klastrze HDInsight z magazynu Lake danych. Ponadto wystawcy usługi należy utworzyć za pomocą certyfikatu, zgodnie z opisem w [Tworzenie głównej przy użyciu certyfikatu usługi](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Tworzenie magazynu Lake danych Azure

Wykonaj poniższe czynności, aby utworzyć magazynu Lake danych.

1. Z pulpitu Otwórz nowe okno programu PowerShell Azure, a następnie wprowadź następujące wstawek. Gdy zostanie wyświetlony monit, aby się zalogować, upewnij się, że logujesz się w jednym z admininistrators właścicielem subskrypcji:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Jeśli zostanie wyświetlony komunikat o błędzie podobny do `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` podczas rejestrowania dostawcy zasobów magazynu Lake danych, jest możliwe, że usługi subsrcription nie jest whitelisted dla magazynu Lake danych Azure. Upewnij się, że możesz włączyć subskrypcję Azure Podgląd publicznej magazynu Lake danych, wykonując następujące [instrukcje](data-lake-store-get-started-portal.md#signup).

3. Konto Azure magazynu Lake danych jest skojarzony z grupą zasobów Azure. Rozpoczyna się od utworzenia Azure grupa zasobów.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Tworzenie grupy zasobów Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Tworzenie grupy zasobów Azure")

2. Utwórz konto Azure magazynu Lake danych. Nazwę konta, które można określić musi zawierać tylko małe litery i cyfry.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Utwórz konto Azure Lake danych] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Utwórz konto Azure Lake danych")

3. Sprawdź, czy konto jest pomyślnie utworzone.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Wynik to powinien być **spełnione**.

4. Przekaż kilka przykładowych danych do Lake danych Azure. Użyjemy to w dalszej części tego artykułu, aby sprawdzić, czy dane są dostępne w klastrze HDInsight. Jeśli szukasz kilka przykładowych danych do przekazania zostanie wyświetlony folder **Pogotowie danych** z [Repozytorium cyfra Lake danych Azure](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Konfigurowanie uwierzytelniania opartego na rolach dostępu do magazynu Lake danych

Co Azure subskrypcji jest skojarzony z usługi Azure Active Directory. Użytkowników i usług, które uzyskać dostęp do zasobów subskrypcję za pomocą klasycznej Portal Azure lub interfejsu API Menedżera zasobów Azure należy najpierw uwierzytelnić z tej usługi Azure Active Directory. Dostęp jest udzielane Azure subskrypcje i usług przez przypisywanie ich odpowiednią rolę Azure zasobu.  W przypadku usług wystawcy usługi identyfikuje usługę w Azure Active Directory (AAD). W tej sekcji pokazano, jak udzielić usługi aplikacji, takiej jak usługa HDInsight, dostępu do zasobu Azure (konto Azure magazynu Lake danych został utworzony wcześniej) przez tworzenie wystawcy usługi aplikacji i przypisywanie ról w tym programie Azure PowerShell.

Aby skonfigurować uwierzytelniania usługi Active Directory dla Azure Lake danych, należy wykonać następujące zadania.

* Tworzenie certyfikatu z podpisem własnym
* Tworzenie aplikacji usługi Azure Active Directory i wystawcy usługi

### <a name="create-a-self-signed-certificate"></a>Tworzenie certyfikatu z podpisem własnym

Upewnij się, że masz zainstalowany przed kontynuowaniem czynności opisane w tej sekcji [SDK systemu Windows](https://dev.windows.com/en-us/downloads) . Możesz również tworzyć katalogu, takich jak **C:\mycertdir**, gdzie zostanie utworzony certyfikat.

1. W oknie programu PowerShell, przejdź do lokalizacji, w którym zainstalowano zestaw SDK systemu Windows (zazwyczaj `C:\Program Files (x86)\Windows Kits\10\bin\x86` i używanie [MakeCert] [ makecert] narzędzie, aby utworzyć certyfikat z podpisem własnym i klucz prywatny. Za pomocą następujących poleceń.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Pojawi się wprowadź hasło klucza prywatnego. Po pomyślnym wykonaniu polecenia powinna być widoczna **CertFile.cer** i **mykey.pvk** w katalogu certyfikatu podanego.

4. Za pomocą [Pvk2Pfx] [ pvk2pfx] narzędzie do konwersji plików .pvk i cer utworzone przez MakeCert na plik pfx. Uruchom następujące polecenie.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Po wyświetleniu monitu wprowadź hasło klucza prywatnego określonej wcześniej. Wartość, która parametr **-po** jest hasłem, który jest skojarzony z plik pfx. Po pomyślnym zakończeniu polecenia należy również obserwować CertFile.pfx w katalogu certyfikatu podanego.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Tworzenie usługi Azure Active Directory i wystawcy usługi

W tej sekcji należy wykonać kroki do tworzenia usługi kapitału dla aplikacji usługi Azure Active Directory, przypisywanie roli do głównej usługi i uwierzytelnienia jako podstawowe usługi, dostarczając certyfikatu. Uruchom następujące polecenia do tworzenia aplikacji w usłudze Azure Active Directory.

1. Wklej następujące polecenia cmdlet w oknie konsoli programu PowerShell. Upewnij się, że wartość określona dla właściwości **- DisplayName** jest unikatowy. Ponadto wartości **— Strona główna** i **- IdentiferUris** są wartościami symbol zastępczy, a nie są sprawdzane.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Tworzenie wystawcy usługi przy użyciu identyfikatora aplikacji.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Udzielanie dostępu głównej usługi do magazynu Lake danych plik/folder, który będzie można uzyskać dostęp z klaster HDInsight. Poniżej wstawek zapewnia dostęp do głównego konta magazynu Lake danych.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    W wierszu polecenia wpisz **Y** , aby potwierdzić.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Tworzenie klastrze HDInsight z uwierzytelnianiem w magazynie Lake danych

W tej sekcji możemy utworzyć klaster HDInsight Hadoop. W tej wersji klaster HDInsight i magazynie Lake danych muszą być w tej samej lokalizacji (USA wschodniego 2).

1. Rozpoczynanie z pobieranie identyfikatora subskrypcji dzierżawy. Który jest potrzebna później.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. W tej wersji dla klastrze Hadoop magazynu Lake danych można używać tylko jako dodatkowego miejsca do magazynowania dla klaster. Magazyn domyślny będzie nadal obiektów blob platformy Azure przestrzeni dyskowej (WASB). Tak najpierw utworzymy konto miejsca do magazynowania i kontenerów miejsca do magazynowania wymagane dla klaster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Utwórz klaster HDInsight. Za pomocą następujące polecenia cmdlet.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Po pomyślnym zakończeniu polecenia cmdlet powinien zostać wyświetlony następujący wynik:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Uruchamianie zadania testowe w klastrze HDInsight w celu używania magazynu Lake danych

Po skonfigurowaniu klaster HDInsight można uruchamiać zadania test w klastrze, aby sprawdzić, czy klaster HDInsight dostęp do magazynu Lake danych. Aby to zrobić, możemy uruchomi zadanie gałęzi próbki, które tworzy tabelę przy użyciu przekazanego wcześniej do sklepu Lake danych przykładowych danych.

### <a name="for-a-linux-cluster"></a>Klaster Linux

W tej sekcji będą SSH w klastrze i uruchom przykładowa kwerenda gałęzi. System Windows nie udostępnia wbudowanego klienta SSH. Firma Microsoft zaleca używanie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji na temat korzystania z Kit Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Po połączeniu, uruchom polecenie gałęzi przy użyciu następującego polecenia:

        hive

2. Za pomocą interfejsu wiersza polecenia, wprowadź poniższe instrukcje, aby utworzyć nową tabelę o nazwie **pojazdy** przy użyciu przykładowych danych w magazynie Lake danych:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Powinien zostać wyświetlony wynik podobny do następującego:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Klaster systemu Windows

Aby uruchomić kwerendę gałęzi za pomocą następujące polecenia cmdlet. W tej kwerendzie możemy utworzyć tabelę z danych w magazynie Lake danych, a następnie uruchom kwerendę wybierającą na utworzonej tabeli.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Ma to następujące wyniki. **ExitValue** 0 w wyniku kwerendy sugerowanie pomyślnie wykonano zadanie.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Pobierz dane wyjściowe z zadania przy użyciu następującego polecenia cmdlet:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Wynik zadania podobny do następującego:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Magazyn Lake danych programu Access przy użyciu polecenia HDFS

Po skonfigurowaniu klaster HDInsight w celu używania magazynu Lake danych, możesz uzyskać dostępu do magazynu za pomocą poleceń powłoki HDFS.

### <a name="for-a-linux-cluster"></a>Klaster Linux

W tej sekcji można będzie SSH w klastrze i uruchomienie poleceń HDFS. System Windows nie udostępnia wbudowanego klienta SSH. Firma Microsoft zaleca używanie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji na temat korzystania z Kit Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Po nawiązaniu połączenia użyj następującego polecenia system plików HDFS, aby wyświetlić listę plików w magazynie Lake danych.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

To powinien zawierać plik, który wcześniej przekazane do sklepu Lake danych.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Można również użyć `hdfs dfs -put` polecenie, aby przekazać pliki do magazynu Lake danych, a następnie użyj `hdfs dfs -ls` Aby sprawdzić, czy pliki zostały pomyślnie przekazane.


### <a name="for-a-windows-cluster"></a>Klaster systemu Windows

1. Logowanie się do nowego [Azure Portal](https://portal.azure.com).

2. Kliknij przycisk **Przeglądaj**, kliknij pozycję **klastrów HDInsight**, a następnie kliknij utworzony klaster HDInsight.

3. W karta klaster kliknij **Pulpitu zdalnego**, a następnie w karta **Pulpitu zdalnego** , kliknij przycisk **Połącz**.

    ![Zdalny w klaster HDI] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Tworzenie grupy zasobów Azure")

    Po wyświetleniu monitu wprowadź poświadczenia, podanych dla użytkownika na pulpicie zdalnym.

4. W sesji zdalnej Uruchom program Windows PowerShell, a lista plików w magazynie Lake Azure danych przy użyciu poleceń systemu plików HDFS.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    To powinien zawierać plik, który wcześniej przekazane do sklepu Lake danych.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Można również użyć `hdfs dfs -put` polecenie, aby przekazać pliki do magazynu Lake danych, a następnie użyj `hdfs dfs -ls` Aby sprawdzić, czy pliki zostały pomyślnie przekazane.

## <a name="see-also"></a>Zobacz też

* [Portal: Tworzenie klaster HDInsight w celu używania magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
