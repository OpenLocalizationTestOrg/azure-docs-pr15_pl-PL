<properties
    pageTitle="Tworzenie klastrów Hadoop, HBase lub Burza na Linux w HDInsight przy użyciu polecenie Azure i platform | Microsoft Azure"
    description="Dowiedz się, jak utworzyć klastrów systemem Linux HDInsight przy użyciu polecenie Azure i platform, szablony Menedżera zasobów Azure i interfejsu API usługi REST Azure. Można określić typ klaster (Hadoop, HBase lub Burza) lub za pomocą skryptów można zainstalować składniki niestandardowe."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Tworzenie klastrów systemem Linux w za pomocą interfejsu wiersza polecenia Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Polecenie Azure to narzędzie wiersza polecenia i platform, które umożliwia zarządzanie usługi Azure. Go można, wraz z szablony zarządzania Azure zasobów, aby utworzyć klastrze HDInsight wraz z konta skojarzonego miejsca do magazynowania i innych usług.

Azure szablony zarządzania zasobami są JSON dokumentów, które opisują __Grupa zasobów__ i wszystkie zasoby w nim (na przykład HDInsight). Tej metody oparty na szablonie pozwala na zdefiniowanie wszystkie zasoby, które są potrzebne do HDInsight w jednym szablonie. Umożliwia także zarządzanie zmianami do grupy jako całość do __wdrożenia__, które mają zastosowanie zmian do całej grupy.

Proces tworzenia klastrze HDInsight przy użyciu szablonu i polecenie Azure mógłby wykonać kroki opisane w tym dokumencie.

> [AZURE.IMPORTANT] Kroki opisane w tym dokumencie oznaczać klaster HDInsight domyślnej liczby węzłów pracownika (4). Jeśli planujesz na więcej niż 32 węzły pracownika (podczas tworzenia klaster lub skalowania klaster), musisz wybrać odpowiedni rozmiar węzła głównego z co najmniej 8 rdzeni i 14 GB pamięci ram.
>
> Aby uzyskać więcej informacji na węzeł rozmiarów i koszty zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Polecenie azure__. Kroki opisane w tym dokumencie ostatnio zostały przetestowane z polecenie Azure wersji 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Zaloguj się do subskrypcji usługi Azure

Wykonaj kroki opisane w [Nawiązywanie połączenia z subskrypcji usługi Azure z interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-connect.md) i Połącz z subskrypcji przy użyciu metody __logowania__ .

##<a name="create-a-cluster"></a>Utworzyć klaster

Poniższe czynności należy wykonać z wiersza polecenia, powłoki lub sesja po zainstalowaniu i skonfigurowaniu polecenie Azure.

1. Użyj następującego polecenia do uwierzytelniania Azure subskrypcji:

        azure login

    Zostanie wyświetlony monit o podanie nazwy i hasła usługi. Jeśli masz wiele subskrypcji Azure za pomocą `azure account set <subscriptionname>` ustawić subskrypcję, do której za pomocą poleceń interfejsu Azure wiersza polecenia.

3. Przełącz się do trybu Menedżer zasobów Azure za pomocą następujące polecenie:

        azure config mode arm

4. Tworzenie grupy zasobów. Ta grupa zasobów zawiera klaster HDInsight i skojarzone konta miejsca do magazynowania.

        azure group create groupname location
        
    * Zamień __grupy__ unikatową nazwę grupy. 
    * Zamień __lokalizacji__ regionu geograficznego, który chcesz utworzyć grupę. 
    
        Aby uzyskać listę prawidłowych lokalizacji za pomocą `azure location list` polecenia, a następnie użyj jednej z lokalizacji w kolumnie __Nazwa__ .

5. Utwórz konto miejsca do magazynowania. To konto miejsca do magazynowania będzie używana jako magazyn domyślny dla klastrów HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Zamień __grupy__ Nazwa grupy utworzony w poprzednim kroku.
     * Zamień __lokalizacji__ na tej samej lokalizacji używane w poprzednim kroku. 
     * Zamień __storagename__ unikatową nazwę konta magazynu.
     
     > [AZURE.NOTE] Aby uzyskać więcej informacji o parametry używane w tym poleceniu, za pomocą `azure storage account create -h` wyświetlić Pomoc dla tego polecenia.

5. Pobieranie klucz użyty do uzyskania dostępu do konta miejsca do magazynowania.

        azure storage account keys list -g groupname storagename
        
    * Zamień __grupy__ Nazwa grupy zasobów.
    * Zamień __storagename__ nazwę konta magazynu.
    
    W ramach danych, który został zwrócony Zapisz wartość __klucza__ __klucz1__.

6. Utwórz klaster HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Zamień __grupy__ Nazwa grupy zasobów.

    * Zamień __Hadoop__ typ klaster, który chcesz utworzyć. Na przykład `Hadoop`, `HBase`, `Storm` lub `Spark`.

        > [AZURE.IMPORTANT] Usługa HDInsight klastrów są dostępne w różnych typów, które odpowiadają obciążenie pracą lub technologii, którą klaster jest dostosowanych do. Istnieje obsługiwana metoda aby utworzyć klaster, który łączy wiele typów, takie jak Burza i HBase na jeden klaster. 

    * Zamień __lokalizacji__ na tej samej lokalizacji używane w poprzednich krokach.

    * Zamień __storagename__ nazwę konta magazynu.

    * Zamień __atrybutu storagekey__ klucz uzyskaną w poprzednim kroku. 

    * Aby uzyskać `--defaultStorageContainer` parametr, użyj tej samej nazwie jak jest używany klaster.

    * Zamień __Administrator__ , a __httppassword__ nazwę i hasło, które mają być używane podczas uzyskiwania dostępu do klaster za pośrednictwem protokołu HTTPS.

    * Zastąp __sshuser__ i __sshuserpassword__ nazwę użytkownika i hasło, które mają być używane podczas uzyskiwania dostępu do klaster przy użyciu SSH

    Może potrwać kilka minut na zakończenie procesu tworzenia klaster. Zazwyczaj około 15.

##<a name="next-steps"></a>Następne kroki

Teraz, gdy został pomyślnie utworzony klaster HDInsight przy użyciu polecenie Azure, Dowiedz się, jak pracować z klaster należy wykonać następujące kroki:

###<a name="hadoop-clusters"></a>Klastrów Hadoop

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>Klastrów HBase

* [Rozpoczynanie pracy z HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Można opracowywać aplikacje Java dla HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Klastrów Burza

* [Można opracowywać topologii Java dla Burza na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Używanie składników Python w Burza na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Wdrażanie i monitorowanie topologii z Burza na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
