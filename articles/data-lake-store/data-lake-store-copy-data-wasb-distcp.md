<properties
   pageTitle="Skopiuj dane do i z WASB do magazynu Lake danych przy użyciu Distcp | Microsoft Azure"
   description="Kopiowanie danych do i z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych za pomocą narzędzia Distcp"
   services="data-lake-store"
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Kopiowanie danych między obiektami blob miejsca do magazynowania Azure i magazynu Lake danych przy użyciu Distcp

> [AZURE.SELECTOR]
- [Przy użyciu DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Przy użyciu AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Po utworzeniu klaster HDInsight, które mają dostęp do konta magazynu Lake danych służy Hadoop ekosystem narzędzi, takich jak Distcp skopiuj dane **do i z** usługi HDInsight klaster miejsca do magazynowania (WASB) do konta magazynu Lake danych. Ten artykuł zawiera instrukcje dotyczące osiągnąć ten cel.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Włączanie Azure subskrypcji** dla magazynu Lake danych publicznych Podgląd. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).
- **Usługa azure HDInsight klaster** z dostępu do konta magazynu Lake danych. Zobacz [Tworzenie klastrze HDInsight z magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md). Upewnij się, że Włączanie pulpitu zdalnego dla klaster.

## <a name="do-you-learn-fast-with-videos"></a>Uzyskać szybkie z klipów wideo?

[Obejrzyj ten klip wideo](https://mix.office.com/watch/1liuojvdx6sie) na temat przenoszenia danych między blob miejsca do magazynowania Azure i magazynu Lake danych przy użyciu DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Używanie Distcp z pulpitu zdalnego (klaster systemu Windows) lub SSH (Linux klaster)

Klaster HDInsight zawiera narzędzia Distcp, które mogą być używane w celu skopiowania danych z różnych źródeł w klastrze HDInsight. Jeśli skonfigurowano klaster HDInsight w celu używania magazynu Lake danych jako dodatkowego miejsca do magazynowania, narzędzie Distcp mogą być używane z gotowych do Aby skopiować dane do i z na koncie magazynu Lake danych. W tej sekcji przyjrzymy się sposobu używania narzędzia Distcp.

1. Jeśli masz klastrze Windows zdalnego w klastrze HDInsight, który zawiera dostępu do konta magazynu Lake danych. Aby uzyskać instrukcje zobacz [Łączenie z klastrów przy użyciu RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). W grupie pulpitu Otwórz wiersza polecenia Hadoop.

    Jeśli masz klastrze Linux, połącz się z klastrem za pomocą SSH. Zobacz [Łączenie z klastrem HDInsight systemem Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Uruchom polecenia w wierszu SSH.

3. Sprawdź, czy masz dostęp do Azure magazyn obiektów blob (WASB). Uruchom następujące polecenie:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    To powinien zawierać spis treści w magazyn obiektów blob.

4. Podobnie Sprawdź, czy można uzyskać dostęp do konta magazynu Lake danych z klaster. Uruchom następujące polecenie:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    To powinien zawierać listę plików i folderów w oknie konta magazynu Lake danych.

5. Aby skopiować dane z WASB do konta magazynu Lake danych za pomocą Distcp.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Spowoduje to skopiowanie zawartość **** folderu/przykład/danych/gutenberg/w WASB do **/myfolder** w oknie konta magazynu Lake danych.

6. Podobnie Aby skopiować dane z konta magazynu Lake danych do WASB za pomocą Distcp.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Spowoduje to skopiowanie zawartości **/myfolder** w oknie konta magazynu Lake danych do **** folderu/przykład/danych/gutenberg/w WASB.

## <a name="see-also"></a>Zobacz też

- [Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych](data-lake-store-copy-data-azure-storage-blob.md)
- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
