<properties
    pageTitle="Dostępność Hadoop klastrów w HDInsight | Microsoft Azure"
    description="Usługa HDInsight wdraża wysoce dostępne i niezawodne klastrów zawierających dodatkowe węzła głównego."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Dostępność i niezawodność klastrów opartych na systemie Windows Hadoop w HDInsight


>[AZURE.NOTE] Kroki wymagane do tego dokumentu są specyficzne dla klastrów HDInsight systemu Windows. Jeśli korzystasz z systemem Linux klaster, zobacz [dostępności i niezawodności klastrów systemem Linux Hadoop w HDInsight](hdinsight-high-availability-linux.md) dla informacje specyficzne dla Linux.

Usługa HDInsight umożliwia klientom wdrażanie różnych typów klaster dla obciążenia analizy danych. Typy klastrów oferowanych obecnie są klastrów Hadoop w celu kwerendy i analizy obciążeń pracą, klastrów HBase w celu NoSQL obciążenia i klastrów Burza w celu przetwarzania zdarzeń w czasie rzeczywistym obciążenia. W ramach danego typu dany klaster istnieją różne role użytkownika dla różnych węzłów. Na przykład:



- Klastrów Hadoop w celu HDInsight zostaną rozmieszczone wraz z dwóch ról:
    - Węzeł głowy (węzłach 2)
    - Węzeł danych (węzeł co najmniej 1)

- Klastrów HBase w celu HDInsight zostaną rozmieszczone wraz z trzech ról:
    - Szef serwerów (węzłów 2)
    - Serwery region (węzeł co najmniej 1)
    - Węzły wzorca/Zookeeper (węzły 3)

- Burza klastrów w celu HDInsight zostaną rozmieszczone wraz z trzech ról:
    - Węzły nimbus (węzły 2)
    - Serwery opiekuna (węzeł co najmniej 1)
    - Węzły zookeeper (węzły 3)

Standardowy implementacji klastrów Hadoop powodują pojedynczego węzła głównego. Usługa HDInsight usuwa ten pojedynczy punkt awarii z dodatkiem /head pomocniczej węzła głównego węzła serwera/Nimbus zwiększenie dostępności i niezawodności potrzebne do zarządzania obciążeń pracą usługi. Te węzły serwerów Nimbus głowy węzły/Szef są przeznaczone do zarządzania błąd węzłów pracownik sprawniej, ale wszelkie dostawie świadczenie usług uruchomionych węzła głównego może spowodować klaster przestaną działać.


Węzły [zooKeeper](http://zookeeper.apache.org/ ) (ZKs) zostały dodane i są używane do wyboru znak wiodący węzłów głowy i aby mieć pewność, że węzły i bram (GWs) o tym, kiedy przechodzić do pomocniczej węzła głównego (Szef Węzeł1) po aktywna aktywnego węzła głównego (Szef Node0).

![Diagram wysoce niezawodne węzły głowy w celu wykonania HDInsight Hadoop.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Sprawdź stan usługi active węzła głównego
Do określenia, które węzła głównego jest aktywny i sprawdzić stan usług uruchomionych dla tego węzła głównego, należy połączyć się z klastrem Hadoop przy użyciu protokołu RDP (Remote Desktop). Aby uzyskać instrukcje RDP zobacz [klastrów zarządzanie Hadoop w HDInsight za pomocą portalu Azure](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Po połączeniu z klastrem, kliknij dwukrotnie ikonę **Dostępne usługi Hadoop** znajduje się na pulpicie, aby uzyskać status, o których węzła głównego Namenode, Jobtracker, Templeton Oozieservice, Metastore i Hiveserver2 usługi są uruchomione lub usługi HDI 3.0, Namenode, Menedżer zasobów, Historia serwera, Templeton, Oozieservice, Metastore i Hiveserver2.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Zrzut ekranu aktywny węzeł głowy o *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Pliki dziennika programu Access na pomocniczym węzła głównego

Aby uzyskać dostęp do zadania dzienniki na pomocniczym węzła głównego w przypadku, gdy stało się aktywne węzła głównego, przeglądanie interfejs JobTracker nadal działa, co dla podstawowego węzła aktywnego. Aby uzyskać dostęp do JobTracker, należy połączyć z klastrem Hadoop przy użyciu RDP, zgodnie z opisem w poprzedniej sekcji. Po umieszczeniu węzłach w grupie, kliknij dwukrotnie ikonę **Stanu węzła Hadoop nazwa** znajduje się na pulpicie, a następnie kliknij polecenie **NameNode dzienniki** w celu uzyskiwania dostępu do katalogu dzienniki na pomocniczym węzła głównego.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Konfigurowanie rozmiaru węzła głównego
Węzły głowy są przydzielane jako dużych wirtualnych maszyn domyślnie. Rozmiar jest odpowiednia w przypadku zarządzania większości zadań Hadoop uruchamiania w klastrze. Ale istnieją scenariusze, w których może być bardzo duże maszyny wirtualne dla głowy węzłów. Przykładem jest po klaster zarządzanie dużą liczbą zadań małych Oozie.

Bardzo duże maszyny wirtualne można skonfigurować przy użyciu poleceń cmdlet środowiska PowerShell Azure lub HDInsight SDK.

Tworzenie i inicjowania obsługi administracyjnej klastrze przy użyciu programu PowerShell Azure opisano w [HDInsight administrować przy użyciu programu PowerShell](hdinsight-administer-use-powershell.md). Konfiguracja bardzo duże węzła głównego wymaga dodanie `-HeadNodeVMSize ExtraLarge` parametr `New-AzureRmHDInsightcluster` polecenia cmdlet używane w tym kodu.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Dla zestawu SDK przypomina sekcji. Tworzenie i inicjowania obsługi administracyjnej klastrze przy użyciu zestawu SDK opisano w [Przy użyciu HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk). Konfiguracja bardzo duże węzła głównego wymaga dodanie `HeadNodeSize = NodeVMSize.ExtraLarge` parametr `ClusterCreateParameters()` metodę w tym kodzie.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Następne kroki

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Nawiązywanie połączenia przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp)
- [Przy użyciu zestawu SDK .NET HDInsight](hdinsight-provision-clusters.md#sdk)
