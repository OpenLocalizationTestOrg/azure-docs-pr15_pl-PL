<properties
    pageTitle="Zarządzanie za pomocą interfejsu wiersza polecenia Azure klastrów Hadoop | Microsoft Azure"
    description="Jak za pomocą interfejsu wiersza polecenia Azure Zarządzanie klastrów Hadoop w HDIsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Zarządzanie klastrów Hadoop w za pomocą interfejsu wiersza polecenia Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Dowiedz się, jak zarządzanie klastrów Hadoop w Azure HDInsight za pomocą [interfejsu wiersza polecenia Azure](../xplat-cli-install.md) . Polecenie Azure jest zaimplementowana w Node.js. Pozwala na dowolnej platformie, która obsługuje Node.js, łącznie z systemem Windows, Mac i Linux.

W tym artykule opisano, jak tylko użyciu polecenie Azure HDInsight. Aby uzyskać ogólne wskazówki dotyczące używania polecenie Azure, zobacz [zainstalować i skonfigurować polecenie Azure][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Polecenie azure** — zobacz [zainstalować i skonfigurować polecenie Azure](../xplat-cli-install.md) uzyskać informacje dotyczące instalacji i konfiguracji.
- **Nawiązywanie połączenia z Azure**, używając następujące polecenie:

        azure login

    Aby uzyskać więcej informacji o uwierzytelnianiu za pomocą konta służbowego zobacz [Łączenie się Azure subskrypcję polecenie Azure](xplat-cli-connect.md).
    
- **Przełącz do trybu Menedżer zasobów Azure**, przy użyciu następującego polecenia:

        azure config mode arm

Aby uzyskać pomoc, należy użyć przełącznika **-h** .  Na przykład:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Tworzenie klastrów

Zobacz [systemem Linux oraz tworzenie klastrów w HDInsight za pomocą interfejsu wiersza polecenia Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Listy i Pokaż szczegóły klaster
Listy i Pokaż szczegóły klaster, należy użyć następujących poleceń:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Usuwanie klastrów

Aby usunąć klaster, użyj następującego polecenia:

    azure hdinsight cluster delete <Cluster Name>

Możesz również usunąć klaster, usuwając grupa zasobów, która zawiera klaster. Należy pamiętać, spowoduje to usunięcie wszystkich zasobów w grupie, w tym domyślne konto miejsca do magazynowania.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Skala klastrów

Aby zmienić rozmiar klaster Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Włącz lub wyłącz dostęp HTTP dla klastrów

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Włącz lub wyłącz dostęp RDP dla klastrów

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Następne kroki
W tym artykule zapoznaniu do wykonywania różnych zadań administracyjnych klaster HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Administrowanie przy użyciu Azure Portal HDInsight] [hdinsight-admin-portal]
* [Administrowanie przy użyciu programu PowerShell Azure HDInsight] [hdinsight-admin-powershell]
* [Wprowadzenie do usługi HDInsight platformy Azure] [hdinsight-get-started]
* [Jak używać polecenie Azure] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Listy i Pokaż klastrów"
