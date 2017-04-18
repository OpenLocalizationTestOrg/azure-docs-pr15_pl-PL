<properties
    pageTitle="Tworzenie klastrów HBase w wirtualnej sieci | Microsoft Azure"
    description="Wprowadzenie do korzystania z HBase w Azure HDInsight. Dowiedz się, jak utworzyć HDInsight HBase klastrów w wirtualnej sieci Azure."
    keywords=""
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
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Tworzenie klastrów HBase w wirtualnej sieci Azure 

Dowiedz się, jak utworzyć klastrów Azure HDInsight HBase w [Wirtualnej sieci Azure][1].

W przypadku integracji wirtualną sieć klastrów HBase mogą być rozmieszczone do tej samej sieci wirtualnego jako aplikacji tak, aby aplikacje mogą komunikować się z HBase bezpośrednio. Zalety:

- Bezpośrednie łączność aplikacji sieci web z węzłów klaster HBase, który umożliwia komunikacja za pomocą procedury zdalnego HBase Java połączeń interfejsy API (RPC).
- Zwiększona wydajność nie ma potrzeby ruchu przejdź na wielu bram i urządzenia do równoważenia obciążenia.
- Możliwość przetwarzania poufnych informacji w sposób zabezpieczyć bez ujawniania publicznej punktu końcowego.

###<a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracy z programem PowerShell Azure**. Zobacz [Instalowanie i używanie Azure programu PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Tworzenie HBase klaster w wirtualnej sieci

W tej sekcji utworzysz klastrze systemem Linux HBase zależne konto Azure miejsca do magazynowania w Azure wirtualnej sieci przy użyciu [szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md). Inne metody tworzenia klaster i opis ustawień, zobacz [Tworzenie HDInsight klastrów](hdinsight-hadoop-provision-linux-clusters.md). Aby uzyskać więcej informacji na temat Tworzenie klastrów Hadoop w HDInsight za pomocą szablonu zobacz [Tworzenie Hadoop klastrów w korzystania z szablonów Menedżera zasobów Azure HDInsight](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Niektóre właściwości zostały wpisane bezpośrednio do szablonu. Na przykład:
>
> * __Lokalizacja__: USA wschodniego 2
> * Wersja __Cluster: 3.4
> * __Liczba węzeł pracownik klastrów__: 4
> * __Domyślne konto miejsca do magazynowania__: &lt;nazwy klaster > przechowywania
> * __Nazwa sieciowa wirtualnych__: &lt;nazwy klaster >-vnet
> * __Przestrzeń adresów wirtualnych sieci__: 10.0.0.0/16
> * __Nazwa podsieci__: domyślne
> * __Zakres adresów podsieci__: 10.0.0.0/24
>
> &lt;Klaster nazwa > zostaną zastąpione nazwę klaster udzielać za pomocą tego szablonu.

1. Kliknij, aby otworzyć szablon w portalu Azure następujące obraz. Szablon znajduje się w kontenerze publicznej obiektów blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z karta **wdrożenia niestandardowe** wprowadź następujące informacje:

    - **Subskrypcja**: Wybierz subskrypcję usługi Azure użyte do utworzenia klastrem HDInsight, zależne konta miejsca do magazynowania i Azure wirtualnej sieci.
    - **Grupa zasobów**: wybierz opcję **Utwórz nowy**, a następnie określ nazwę nowej grupy zasobów.
    - **Lokalizacja**: Wybierz lokalizację dla grupy zasobów.
    - **NazwaKlastra**: Wprowadź nazwę klaster Hadoop, który ma zostać utworzony.
    - **Klaster nazwę logowania i hasło**: domyślna nazwa logowania to **administratora**.
    - **SSH nazwy użytkownika i hasła**: domyślna nazwa użytkownika to **sshuser**.  Można zmienić jej nazwę. 
    - **Zgodę na warunki i postanowienia podanych powyżej**: (Wybierz)
    

3. Kliknij przycisk **Kup**. Wystarczy o około 20 minut Aby utworzyć klaster. Po utworzeniu klaster, możesz kliknąć pozycję Karta klaster w portalu, aby go otworzyć.

Po ukończeniu samouczka można usunąć klaster. Z usługi HDInsight dane są przechowywane w magazynie Azure, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Możesz również są naliczane dla klastrów HDInsight nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za miejsca do magazynowania, warto ekonomicznych usuwanie klastrów, gdy nie są one używane. Aby uzyskać instrukcje dotyczące usuwania klastrze zobacz [Zarządzanie Hadoop klastrów w HDInsight za pomocą portalu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

Aby rozpocząć pracę z nowego klaster HBase, możesz użyć procedury w [rozpocząć korzystanie z HBase z Hadoop w HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Połącz się z klastrem HBase za pomocą interfejsów API RPC Java HBase

1.  Tworzenie infrastruktury jako maszyny wirtualnej usługi (IaaS) w tym samym Azure wirtualnej sieci i tej samej podsieci. Aby uzyskać instrukcje dotyczące tworzenia nowej maszyny wirtualnej IaaS zobacz [Tworzenie maszyn wirtualnych uruchamiania systemu Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Gdy wykonując czynności opisane w tym dokumencie, należy użyć następujących konfiguracji sieci:

    - __Wirtualna sieć__: &lt;nazwy klaster >-vnet
    - __Podsieci__: domyślne

    > [AZURE.IMPORTANT] Zamienianie &lt;nazwy klaster > z nazwą używaną podczas tworzenia klaster HDInsight w poprzednich krokach.

    Za pomocą tych wartości skonfiguruje maszyny wirtualnej, aby użyć tej samej wirtualnej sieci lub podsieci jako klaster HDInsight. Dzięki temu będzie ich bezpośrednie komunikowanie się ze sobą.

2.  Nawiązywanie połączenia z HBase za pomocą aplikacji języka Java, należy użyć w pełni kwalifikowaną nazwę domeny (FQDN). Aby to ustalić, należy najpierw uzyskać klaster HBase sufiks DNS specyficzne dla połączenia. Aby to zrobić, użyj jednej z następujących metod:

    * Nawiązywanie połączenia Ambari za pomocą przeglądarki sieci Web:
    
        Przejdź do https://&lt;NazwaKlastra >.azurehdinsight.net/api/v1/clusters/&lt;NazwaKlastra >-hosts?minimal_response = true. Powoduje JSON plikiem sufiksy DNS.

    * Użyj Ambari witryny sieci Web:

        1. Przejdź do https://&lt;NazwaKlastra >. azurehdinsight.net.
        2. Kliknij pozycję **Hosts** z górnym menu.

    * Użyj zwinięcie na potrzeby połączeń REST:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Zwrócone dane notacji obiektu JavaScript (JSON) Znajdź pozycję "host_name". To będzie zawierać FQDN dla węzłów w klastrze. Na przykład:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Część nazwy domeny rozpoczynając od nazwy klaster jest sufiks DNS. Na przykład mycluster.b1.cloudapp.net.

    * Przy użyciu Azure
    
        Skorzystaj z tego skryptu programu PowerShell Azure zarejestrować funkcji **Get-ClusterDetail** , które mogą być używane w celu zwrócenia sufiks DNS:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Po uruchomieniu skrypt programu Azure PowerShell, użyj następującego polecenia, aby zwrócić sufiks DNS za pomocą funkcji **Get-ClusterDetail** . Określ nazwę klaster HDInsight HBase, nazwę administratora i hasło administratora w przypadku przy użyciu tego polecenia.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Spowoduje to przywrócenie sufiks DNS. Na przykład **yourclustername.b4.internal.cloudapp.net**.

    * Używanie RDP
    
        Umożliwia także pulpitu zdalnego do nawiązania połączenia z klastrem HBase (nastąpi połączenie do węzła głównego) i uruchamianie **ipconfig** w wierszu polecenia w celu uzyskania sufiks DNS. Aby uzyskać instrukcje dotyczące włączania protokołu RDP (Remote Desktop) i z klastrem łączą się przy użyciu RDP, zobacz [Zarządzanie Hadoop klastrów w za pomocą portalu Azure HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Aby sprawdzić, czy maszyna wirtualna może komunikować się z klastrem HBase, użyj polecenia `ping headnode0.<dns suffix>` z maszyny wirtualnej. Na przykład polecenie ping headnode0.mycluster.b1.cloudapp.net.

Aby użyć tych informacji w aplikacji Java, można wykonaj kroki opisane w [Za pomocą środowiska Maven do tworzenia aplikacji Java, które używają HBase z usługi HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) , aby utworzyć aplikację. Aby masz połączenie z serwerem zdalnym HBase aplikacji, należy zmodyfikować plik **hbase site.xml** w tym przykładzie, aby użyć nazwy FQDN dla Zookeeper. Na przykład:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat rozpoznawania nazw w Azure wirtualnej sieci, w tym jak za pomocą własnego serwera DNS, zobacz [Rozpoznawanie nazw (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak utworzyć klaster HBase. Aby uzyskać więcej informacji, zobacz:

- [Wprowadzenie do usługi HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Konfigurowanie replikacji HBase w HDInsight](hdinsight-hbase-geo-replication.md)
- [Tworzenie klastrów Hadoop w HDInsight](hdinsight-provision-clusters.md)
- [Wprowadzenie do korzystania z Hadoop w HDInsight HBase](hdinsight-hbase-tutorial-get-started.md)
- [Analizowanie upodobania Twitter z HBase w HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Omówienie wirtualnych sieci][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Obsługa administracyjna szczegóły dotyczące nowy klaster HBase"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Dostosowywanie klaster HBase przy użyciu akcji skryptu"

[azure-preview-portal]: https://portal.azure.com
