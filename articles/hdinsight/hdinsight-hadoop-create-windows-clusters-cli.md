<properties
   pageTitle="Tworzenie klastrów opartych na systemie Windows Hadoop w za pomocą interfejsu wiersza polecenia Azure HDInsight"
    description="Dowiedz się, jak utworzyć klastrów dla Azure HDInsight za pomocą interfejsu wiersza polecenia Azure."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Tworzenie klastrów opartych na systemie Windows Hadoop w za pomocą interfejsu wiersza polecenia Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Dowiedz się, jak utworzyć klastrów HDInsight za pomocą interfejsu wiersza polecenia Azure. Do tworzenia innych klaster narzędzia i funkcje kliknij pozycję Wybierz kartę u góry tej strony lub zobacz [metody tworzenia klaster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Wymagania wstępne dotyczące:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Przed rozpoczęciem z instrukcjami podanymi w tym artykule, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Polecenie azure**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Nawiązywanie połączenia z platformy Azure

Aby nawiązać połączenie Azure, użyj następującego polecenia:

    azure login

Aby uzyskać więcej informacji o uwierzytelnianiu za pomocą konta służbowego zobacz [Łączenie się Azure subskrypcję polecenie Azure](../xplat-cli-connect.md).

Aby przełączyć do trybu ARM, użyj następującego polecenia:

    azure config mode arm

Aby uzyskać pomoc, należy użyć przełącznika **-h** .  Na przykład:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Tworzenie klastrów

Aby można było tworzyć klaster HDInsight musi mieć zarządzania zasobów Azure (ARM), a konto magazyn obiektów Blob platformy Azure. Aby utworzyć klaster HDInsight, musisz określić następujące czynności:

- **Grupa zasobów Azure**: musi zostać utworzone konto analizy Lake dane w grupie Azure zasobów. Azure Menedżera zasobów umożliwia pracę z zasobami w aplikacji jako grupy. Możesz wdrożyć, aktualizowanie lub usuwanie wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.

    Aby wyświetlić listę grup zasobów w ramach subskrypcji:

        azure group list

    Aby utworzyć nową grupę zasobów:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Nazwa klaster HDInsight**

- **Lokalizacja**: jeden z centrami danych Azure, obsługiwanych klastrów HDInsight. Aby uzyskać listę obsługiwanych lokalizacji zobacz [Usługa HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Domyślne konto miejsca do magazynowania**: Usługa HDInsight używa kontenerem magazyn obiektów Blob platformy Azure jako domyślnego systemu plików. Aby można było tworzyć klaster HDInsight wymagane jest konto Azure miejsca do magazynowania.

    Aby utworzyć nowe konto Azure miejsca do magazynowania:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Konto miejsca do magazynowania musi collocated z usługi HDInsight w centrum danych.
    > Typ konta magazynu można ZRS, ponieważ ZRS nie obsługuje tabeli.

    Informacje na temat tworzenia konta magazynu platformy Azure za pomocą Azure Portal, zobacz [tworzenie, zarządzanie i usuwanie konta miejsca do magazynowania] [azure — tworzenie storageaccount].

    Jeśli już masz konto miejsca do magazynowania, ale nie znasz nazwę konta i klucz konta, możesz pobrać informacji za pomocą następujących poleceń:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Aby uzyskać szczegółowe informacje dotyczące uzyskiwania informacji za pomocą Azure Portal zobacz sekcję "Zarządzać swoim kontem miejsca do magazynowania" rachunków [miejsca do magazynowania o Azure](../storage/storage-create-storage-account#manage-your-storage-account).

- **Kontener Blob domyślne (opcjonalnie)**: polecenie **klaster usługa azure hdinsight create** tworzy kontenera, jeśli nie istnieje. Jeśli chcesz utworzyć kontener wcześniej, umożliwiają następujące polecenie:

    Tworzenie kontenera magazynu Azure — nazwę konta "<Storage Account Name>" — klucz konta <Storage Account Key> [Nazwa_kontenera]

Po utworzeniu konta miejsca do magazynowania przygotować zechcesz utworzyć klaster:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Tworzenie klastrów korzystanie z plików konfiguracji
Zazwyczaj utworzyć klaster HDInsight, systemie zadań, a następnie usuń klaster zmniejszyć koszty. Interfejs wiersza polecenia udostępnia opcję zapisywania konfiguracji do pliku, tak, aby można ponownie użyć go każdorazowo utworzyć klaster.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Przykład: Tworzenie pliku konfiguracji, zawierającego akcję skrypt do uruchomienia podczas tworzenia klastrze.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Tworzenie klastrów z akcją skryptu

Tworzenie pliku konfiguracji, zawierającego akcję skrypt do uruchomienia podczas tworzenia klastrze.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Utworzyć klaster z akcją skryptu

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Uzyskać skrypt ogólne akcji informacji zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptu (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Tworzenie klastrów korzystania z szablonów ARM

Polecenie umożliwia tworzenie klastrów, dzwoniąc ARM szablonów. Zobacz, [Rozmieszczanie za pomocą interfejsu wiersza polecenia Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Zobacz też

- [Wprowadzenie do usługi HDInsight Azure](hdinsight-hadoop-linux-tutorial-get-started.md) — Dowiedz się, jak rozpocząć pracę z klaster HDInsight
- [Przesyłanie Hadoop zadań programowy](hdinsight-submit-hadoop-jobs-programmatically.md) — Dowiedz się, jak programowo przesyłać zadania do HDInsight
- [Zarządzanie klastrów Hadoop w za pomocą interfejsu wiersza polecenia Azure HDInsight](hdinsight-administer-use-command-line.md)
- [Za pomocą Azure interfejs wiersza polecenia dla komputerów Mac, Linux i systemu Windows z zarządzania usługą Azure](../virtual-machines-command-line-tools.md)
