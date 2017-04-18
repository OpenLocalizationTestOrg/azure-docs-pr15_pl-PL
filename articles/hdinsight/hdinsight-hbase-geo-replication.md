<properties 
   pageTitle="Konfigurowanie replikacji HBase między dwoma centrach danych | Microsoft Azure" 
   description="Dowiedz się, jak skonfigurować replikacji HBase przez centra dwóch danych i o przypadków użycia replikacji klaster." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Konfigurowanie replikacji geo HBase w HDInsight

> [AZURE.SELECTOR]
- [Konfigurowanie połączenia VPN](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurowanie systemu DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurowanie replikacji HBase](hdinsight-hbase-geo-replication.md) 
 
Dowiedz się, jak skonfigurować replikacji HBase w centrach danych dwóch. Niektóre za pomocą przypadkach dla replikacji klaster obejmują:

- Wykonywanie kopii zapasowych i odtwarzania po awarii
- Agregacji danych
- Rozkład danych geograficznych
- Spożyciu online dane połączone z analizy danych w trybie offline

Replikacja klaster używa metodologii wypychanych źródła. Klaster HBase mogą być źródłem miejsca docelowego, lub może wykonać obie role jednocześnie. Replikacja jest asynchroniczne, a celem replikacji jest ewentualne spójności. Gdy źródła otrzymuje edycji do rodziny kolumny z replikacją włączone, tej edycji jest propagowana do wszystkich klastrów miejsca docelowego. Dane są replikowane z jednego klaster do innego, są śledzone klaster źródła i wszystkich klastrów, które już zostały wykorzystane dane aby zapobiec replikacji pętli. Aby uzyskać więcej informacji w tym samouczku skonfigurujesz replikacji źródłowego docelowego.  Dla innych topologii klastrów zobacz [Przewodnik po HBase Apache](http://hbase.apache.org/book.html#_cluster_replication).

Jest to trzecia część serii:

- [Konfigurowanie połączenia VPN między dwoma wirtualnych sieci][hdinsight-hbase-replication-vnet]
- [Konfigurowanie systemu DNS dla sieci wirtualnych][hdinsight-hbase-replication-dns]
- Konfigurowanie replikacji geo HBase (tego samouczka)

Na poniższym diagramie przedstawiono dwa wirtualnych sieci i łączność sieciowa utworzone w [artykule Konfigurowanie połączenia VPN między dwiema sieciami virtual] [ hdinsight-hbase-geo-replication-vnet] i [Konfigurowanie usługi DNS dla sieci wirtualnych][hdinsight-hbase-replication-dns]: 

![Diagram sieciowy wirtualnej replikacji HDInsight HBase][img-vnet-diagram]

## <a id="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracy z programem PowerShell Azure**.

    Wykonywanie skryptów programu PowerShell, możesz uruchomić Azure programu PowerShell jako administrator i ustawić zasady wykonywania *RemoteSigned*. Zobacz, przy użyciu polecenia cmdlet Set-ExecutionPolicy.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Dwa Azure wirtualnej sieci z połączenia VPN i DNS skonfigurowane**.  Aby uzyskać instrukcje, zobacz [Konfigurowanie połączenia VPN między dwoma Azure wirtualnych sieci][hdinsight-hbase-replication-vnet]i [Konfigurowanie usługi DNS między dwoma Azure wirtualnych sieci][hdinsight-hbase-replication-dns].


    Przed uruchomieniem skryptów programu PowerShell, upewnij się, że są podłączone do subskrypcji usługi Azure za pomocą następującego polecenia cmdlet:

        Add-AzureAccount

    Jeśli masz wiele subskrypcji Azure, użyj następującego polecenia cmdlet ustawić bieżącej subskrypcji:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Inicjowanie obsługi klastrów HBase w HDInsight

W [artykule Konfigurowanie połączenia VPN między dwoma Azure wirtualnych sieci][hdinsight-hbase-replication-vnet], wirtualną sieć została utworzona w Europie centrum danych i wirtualnej sieci w centrum danych, USA. Dwa wirtualną sieć są połączone za pośrednictwem sieci VPN. W tej sesji będzie obsługi administracyjnej klastrze HBase w każdej wirtualnych sieci. W dalszej części tego samouczka można będzie utworzyć klastrów HBase replikacji grupie HBase.

Portal klasyczny Azure nie obsługuje inicjowania obsługi administracyjnej klastrów HDInsight przy użyciu opcji Konfiguracja niestandardowa. Na przykład ustaw *hbase.replication* na *wartość true*. Jeśli ustawisz wartość w pliku konfiguracji po zainicjowano obsługę administracyjną klastrze, ustawienie zostaną utracone po klaster jest podejmowana reimaged. Aby uzyskać więcej informacji, zobacz [klastrów należy Hadoop w HDInsight][hdinsight-provision]. Jedną z opcji do zapewniania obsługi HDInsight klaster przy użyciu opcji niestandardowych używa Azure programu PowerShell.


**Inicjowanie obsługi klaster HBase Contoso-VNet-Unii Europejskiej** 

1. W miejscu pracy Otwórz Windows PowerShell ISE.
2. Ustawienie zmiennych na początku skrypt, a następnie uruchom skrypt.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Inicjowanie obsługi klaster HBase Contoso VNet USA** 

- Skorzystaj z tego samego skryptu z następujących wartości:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Ponieważ został już podłączony do konta Azure, nie musisz działają comlets następujące czynności:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Konfigurowanie przesyłania warunkowe

W [Systemie DNS Konfigurowanie wirtualnych sieci][hdinsight-hbase-replication-dns], skonfigurowano serwery DNS dwóch sieci. Klastrów HBase mają sufiksy innej domeny. Dlatego należy skonfigurować dodatkowe DNS usługi warunkowego przesyłania dalej.

Aby skonfigurować usługę warunkowego przesyłania dalej, należy wiedzieć sufiksów domeny dwóch klastrów HBase. 

**Aby znaleźć sufiksów domeny dwóch klastrów HBase**

1. RDP do **Firmy Contoso-HBase-Unii Europejskiej**.  Aby uzyskać instrukcje, zobacz [Zarządzanie Hadoop klastrów w HDInsight za pomocą portalu klasyczny Azure][hdinsight-manage-portal]. Jest faktycznie headnode0 klaster.
2. Otwieranie konsoli programu Windows PowerShell lub wiersz polecenia.
3. Uruchom **ipconfig**i zanotuj **sufiks DNS specyficzne dla połączenia**.
4. Nie Zamknij sesja RDP.  Należy go później, aby przetestować rozpoznawanie nazw domen.
5. Powtórz te same kroki, aby dowiedzieć się, **sufiks DNS konkretnego połączenia** **Contoso HBase USA**.


**Aby skonfigurować przesyłanie dalej DNS**
 
1.  RDP do **Firmy Contoso-DNS-Unii Europejskiej**. 
2.  Kliknij klawisz systemu Windows w lewym dolnym.
2.  Kliknij pozycję **Narzędzia administracyjne**.
3.  Kliknij pozycję **DNS**.
4.  W okienku po lewej stronie rozwiń **powiadomienie o stanie dostarczenia**, **Contoso-DNS-Unii Europejskiej**.
5.  Kliknij prawym przyciskiem myszy **Usługi warunkowego przesyłania dalej**, a następnie kliknij **Nowy warunkowych usługi przesyłania dalej**. 
5.  Wprowadź następujące informacje:
    - **Domeny DNS**: wprowadź sufiks DNS USA HBase Contoso. Na przykład: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **Adresy IP serwerów głównych**: wprowadź 10.2.0.4, czyli adres IP Contoso-DNS-USA firmy. Sprawdź, czy IP. Swojego serwera DNS może mieć inny adres IP.
6.  Naciśnij klawisz **ENTER**, a następnie kliknij **przycisk OK**.  Teraz można rozpoznać adresu IP Contoso-DNS-USA osoby z firmy Contoso-DNS-Unii Europejskiej.
7.  Powtórz kroki, aby dodać warunkowego przesyłania z usługą DNS na komputerze wirtualnych Contoso DNS USA z następujących wartości:
    - **Domeny DNS**: wprowadź sufiks DNS Unii HBase Contoso. 
    - **Adresy IP serwerów głównych**: wprowadź 10.2.0.4, czyli Contoso-DNS-Unii Europejskiej na adres IP.

**Aby przetestować rozpoznawanie nazwy domeny**

1. Przełączanie do okna RDP Contoso-HBase-Unii Europejskiej.
2. Otwórz wiersz polecenia.
3. Uruchom polecenie ping:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Protokół dopasowanie kolorów jest włączona węzłów pracownik klastrów HBase

4. Nie zamykaj sesja RDP. Nadal potrzebujesz go w dalszej części samouczka.
5. Powtórz te same kroki, aby sprawdzić, czy headnode0 Contoso-HBase-Unii Europejskiej z USA HBase Contoso.

>[AZURE.IMPORTANT] DNS musi działać przed przejściem do następnych kroków.

## <a name="enable-replication-between-hbase-tables"></a>Włączanie replikacji między tabelami HBase

Teraz można utworzyć tabelę HBase próbki, włączanie replikacji i przetestowanie zawierającego dane. Przykładowej tabeli będą występują dwie grupy kolumn: Personal i Office. 

W tym samouczku wprowadzisz klaster Europe HBase jako klaster źródło i klaster HBase USA jako klaster docelowy.

Tworzenie tabel HBase przy użyciu takich samych nazwach i rodzin kolumny na klastrów źródłowa i docelowa, tak, aby klaster docelowy miejsce do przechowywania danych, który zostanie wyświetlony. Aby uzyskać więcej informacji na temat korzystania z powłoki HBase, zobacz [Wprowadzenie do programu Apache HBase w HDInsight][hdinsight-hbase-get-started].

**Aby utworzyć tabelę HBase na Contoso-HBase-Unii Europejskiej**

1. Przełączanie do okna RDP **Contoso-HBase-Unii Europejskiej** .
2. Na komputerze kliknij pozycję **Wiersz polecenia Hadoop**.
2. Zmienianie folderu do katalogu macierzystego HBase:

        cd %HBASE_HOME%\bin
3. Otwórz powłoki HBase:

        hbase shell
4. Tworzenie tabeli HBase:

        create 'Contacts', 'Personal', 'Office'
5. Pozostaw sesja RDP ani okno wiersza polecenia Hadoop. Nadal będzie potrzebne w dalszej części samouczka.
    
**Aby utworzyć tabelę HBase na Contoso HBase USA**

- Powtórz te same kroki, aby utworzyć tę samą tabelę na Contoso HBase USA.


**Aby dodać Contoso HBase USA jako funkcja replikacji**

1. Przełączanie do okna RDP **Contso HBase_EU** .
2. W oknie powłoki HBase należy dodać klaster docelowej (Contoso-HBase-pl) jako partner, na przykład:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    W próbce sufiks domeny jest *contoso-hbase-us.d4.internal.cloudapp.net*. Musisz zaktualizować zgodnie z sufiks domeny klastrze HBase nam. Upewnij się, że jest bez spacji między nazwy hostów.

**Aby skonfigurować każdej rodziny kolumny powinny być replikowane na klaster źródła**

1. W oknie powłoki HBase sesja RDP **Contso-HBase-Unii Europejskiej** skonfiguruj każdej rodziny kolumny powinny być replikowane:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Aby zbiorczo przekazania danych do tabeli HBase**

Przykładowy plik danych został przekazany do publicznej kontenera obiektów Blob platformy Azure z następujący adres URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Zawartość pliku:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Możesz przekazać tego samego pliku danych do klaster HBase i zaimportować dane z tego poziomu.

1. Przełączanie do okna RDP **Contoso-HBase-Unii Europejskiej** .
2. Na komputerze kliknij pozycję **Wiersz polecenia Hadoop**.
3. Zmienianie folderu do katalogu macierzystego HBase:

        cd %HBASE_HOME%\bin

4. Przekaż dane:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Upewnij się, że odbywa się replikacji danych

Można sprawdzić, czy replikacja odbywa się Skanując tabele z obu klastrów z następujących poleceń powłoki HBase:

        Scan 'Contacts'


## <a name="next-steps"></a>Następne kroki

W tym samouczku zapoznaniu przed skonfigurowaniem replikacji HBase przez dwie centrach danych. Aby dowiedzieć się więcej na temat HDInsight i HBase, zobacz:

- [Strona usługi HDInsight](https://azure.microsoft.com/services/hdinsight/)
- [Dokumentacja HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Wprowadzenie do programu Apache HBase w HDInsight][hdinsight-hbase-get-started]
- [Omówienie HDInsight HBase][hdinsight-hbase-overview]
- [Inicjowanie obsługi klastrów HBase w wirtualnej sieci Azure][hdinsight-hbase-provision-vnet]
- [Analizowanie w czasie rzeczywistym upodobania Twitter z HBase][hdinsight-hbase-twitter-sentiment]
- [Analizowanie danych czujnik z Burza i HBase w HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
