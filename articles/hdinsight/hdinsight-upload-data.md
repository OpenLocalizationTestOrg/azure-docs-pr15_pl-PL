<properties
    pageTitle="Przekazywanie danych dla zadania Hadoop HDInsight | Microsoft Azure"
    description="Dowiedz się, jak przekazać i uzyskać dostęp do danych dla zadań Hadoop w HDInsight przy użyciu interfejsu wiersza polecenia Azure, Eksplorator magazynu Azure, Azure programu PowerShell, wiersza polecenia Hadoop lub Sqoop."
    services="hdinsight,storage"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Przekazywanie danych dla zadań Hadoop w HDInsight

Usługa Azure HDInsight zapewnia wszechstronna distributed system plików usługi Hadoop (HDFS) przez magazyn obiektów Blob platformy Azure. Zaprojektowane jako rozszerzenie HDFS dostarczanie płynną obsługę klientom. Umożliwia pełny zestaw składników w ekosystemie Hadoop działania bezpośrednio na dane, które zarządza. Azure magazyn obiektów Blob i HDFS są systemów odrębnych plików, są zoptymalizowane pod kątem przechowywania danych i obliczeń na tych danych. Aby uzyskać informacji na temat zalet korzystania z magazynem obiektów Blob platformy Azure, zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight][hdinsight-storage].

**Wymagania wstępne**

Przed rozpoczęciem należy zwrócić uwagę następujące wymagania:

* Klaster Azure HDInsight. Aby uzyskać instrukcje, zobacz [Wprowadzenie do usługi Azure HDInsight] [ hdinsight-get-started] lub [klastrów świadczenia usługi HDInsight][hdinsight-provision].

##<a name="why-blob-storage"></a>Dlaczego blob miejsca do magazynowania?

Azure HDInsight klastrów są zwykle wdrażane uruchamianie zadań MapReduce i klastrów są usuwane po zakończeniu tych zadań. Przechowywanie danych w HDFS klastrów po zakończeniu obliczeń będzie drogich sposobem przechowywania danych. Magazyn obiektów Blob platformy Azure jest wysokiej dostępności, wysoce skalowalna, wysoka wydajność, opcja niski koszt i współużytkowane magazynowania danych, który ma być przetwarzane przy użyciu usługi HDInsight. Dane są przechowywane w obiekcie blob umożliwia klastrów HDInsight, które są używane do obliczeń bezpiecznie wydawane bez utraty danych.

###<a name="directories"></a>Katalogi

Azure kontenerów magazyn obiektów Blob przechowywania danych jako pary klucz wartość, a nie jest bez hierarchii katalogów. Jednak znak "/" może służyć w nazwie klucza był tak, jakby plik jest przechowywany w strukturze katalogu. HDInsight widzi te, jak gdyby były rzeczywistych katalogów.

Na przykład klucz obiektów blob może być *input/log1.txt*. Istnieje nie katalogowych "wprowadzania", ale ze względu na obecność znak "/" w polu Nazwa klucza, ma wygląd ścieżki pliku.

Ze względu na to jeśli korzystasz z narzędzia Azure Explorer można zauważyć, niektóre pliki 0 bajtów. Te pliki używane w dwóch celach:

- W przypadku pustych folderów Oznacz istnienia folder. Magazyn obiektów Blob platformy Azure to inteligentne dowiedzieć się, że jeśli obiektów blob o nazwie foo i pasek istnieje, jest folder o nazwie **foo**. Ale jest jedynym sposobem oznaczają Opróżnij folder o nazwie **foo** przez ten plik specjalne 0 bajtów w miejscu.

- Posiadają specjalne metadanych, które są wymagane przez system plików usługi Hadoop, szczególnie uprawnień i właścicieli dla folderów.

##<a name="command-line-utilities"></a>Narzędzia wiersza polecenia

Firma Microsoft udostępnia następujące narzędzia do pracy z magazynem obiektów Blob platformy Azure:

| Narzędzie | Linux | SYSTEMU OS X | Systemu Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure interfejsu wiersza polecenia][azurecli] | ✔ | ✔ | ✔ |
| [Azure programu PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Polecenie Hadoop](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Gdy Azure interfejsu wiersza polecenia programu PowerShell Azure i AzCopy można wszystkie można używać z zewnętrznego Azure, Hadoop polecenie jest dostępne tylko w grupie HDInsight i tylko umożliwia ładowanie danych z lokalnego systemu plików do magazynu obiektów Blob platformy Azure.

###<a id="xplatcli"></a>Polecenie Azure

Polecenie Azure to narzędzie i platform, które umożliwia zarządzanie usług Azure. Aby przekazać danych do magazynu obiektów Blob platformy Azure, wykonaj następujące czynności:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Zainstalować i skonfigurować dla komputerów Mac, Linux i systemu Windows Azure interfejsu wiersza polecenia](../xplat-cli-install.md).

2. Otwórz wiersz polecenia, imprezie lub innego powłoki i służą do uwierzytelniania do subskrypcji usługi Azure.

        azure login

    Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło dla subskrypcji.

3. Wpisz następujące polecenie, aby wyświetlić listę kont miejsca do magazynowania dla subskrypcji:

        azure storage account list

4. Wybierz konto miejsca do magazynowania, które zawiera blob, którą chcesz pracować z, a następnie użyj następującego polecenia pobrać klucza dla tego konta:

        azure storage account keys list <storage-account-name>

    To powinna zwrócić kluczy **podstawowych** i **pomocniczych** . Skopiuj wartości klucza **podstawowego** , ponieważ będzie można używać w następnych kroków.

5. Aby pobrać listę kontenerów obiektów blob w ramach konta miejsca do magazynowania, użyj następującego polecenia:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Przekazywanie i pobieranie plików do obiektów blob za pomocą następujących poleceń:

    * Aby przekazać plik:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Aby pobrać plik:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Jeśli będziesz zawsze pracować z tym samym kontem miejsca do magazynowania, można ustawić następujące zmienne środowiska zamiast określanie konta i kluczowe dla każdego polecenia:
>
> * **AZURE\_miejsca do magazynowania\_konta**: nazwę konta magazynu
>
> * **AZURE\_miejsca do magazynowania\_dostępu\_klucza**: klucz konta miejsca do magazynowania

###<a id="powershell"></a>Azure programu PowerShell

Azure PowerShell to środowisko skryptów, używanego do kontrolowania i automatycznego wdrożenia i zarządzania z obciążeń pracą w Azure. Uzyskać informacje o konfigurowaniu miejscu pracy, aby uruchomić Azure programu PowerShell, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Aby przekazać plik lokalny magazynem obiektów Blob platformy Azure**

1. Otwórz konsolę Azure programu PowerShell, zgodnie z instrukcjami w [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md).
2. Ustawianie wartości pięć pierwszych zmiennych w następujący skrypt:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Wklej skrypt do konsoli Azure programu PowerShell, aby uruchomić go, aby skopiować plik.

Na przykład skryptów programu PowerShell współdziałające z usługi HDInsight, zobacz [Narzędzia HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy to narzędzie wiersza polecenia zaprojektowano w celu uproszczenia zadań przesyłania danych do i wylogowywanie się z konta usługi Azure miejsca do magazynowania. Można go używać, gdy narzędzie autonomicznej lub dołączyć to narzędzie w istniejącej aplikacji. [Pobierz AzCopy][azure-azcopy-download].

Składnia AzCopy jest następująca:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Aby uzyskać więcej informacji, zobacz [AzCopy - przekazywanie i pobieranie plików dla obiektów blob platformy Azure][azure-azcopy].


###<a id="commandline"></a>Wiersz polecenia Hadoop

Wiersz polecenia Hadoop tylko jest przydatne w przypadku przechowywania danych do magazyn obiektów blob, gdy dane znajdują się już na głowy węzła.

Aby można było użyć polecenia Hadoop, najpierw należy połączyć headnode przy użyciu jednej z następujących metod:

* **Usługa HDInsight systemu Windows**: [Nawiązywanie połączenia przy użyciu pulpitu zdalnego](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Usługa HDInsight systemem Linux**: nawiązać połączenie za pomocą SSH ([polecenie SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) lub [Kit](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Po nawiązaniu połączenia można za pomocą następującej składni przekazywanie pliku do miejsca do magazynowania.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Na przykład`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Ponieważ domyślnego systemu plików dla HDInsight jest magazyn obiektów Blob platformy Azure, /example/data.txt jest faktycznie w magazynie obiektów Blob platformy Azure. Można także odwołać się do pliku w formacie:

    wasbs:///example/data/data.txt

lub

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Aby uzyskać listę innych poleceń Hadoop Praca z plikami zobacz [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Na klastrów HBase rozmiaru używane w przypadku zapisywania danych 256KB bloku domyślny. Gdy to działa prawidłowo podczas korzystania z interfejsów API HBase lub interfejsów API pozostałych, za pomocą `hadoop` lub `hdfs dfs` poleceń, aby zapisać większych niż ~ 12GB danych kończy się błędem. Zobacz sekcję [wyjątku miejsca do magazynowania dla zapisu na obiektów blob](#storageexception) poniżej, aby uzyskać więcej informacji.

##<a name="graphical-clients"></a>Graficzne klientów

Istnieje kilka aplikacji, które mają graficzny interfejs do pracy z magazyn Azure. Oto lista kilka następujących aplikacji:

| Klienta | Linux | SYSTEMU OS X | Systemu Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Eksplorator magazynu platformy Azure](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Chmura miejsca do magazynowania Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Eksplorator Azure](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for HDInsight

Aby uzyskać więcej informacji zobacz [Przechodzenie połączonych zasobów](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Eksplorator magazynu platformy Azure

*Eksplorator magazynu Azure* jest przydatne narzędzie Sprawdzanie i zmienianie danych w obiektów blob. Jest to narzędzie bezpłatne, Otwórz źródło, które można pobrać z [http://storageexplorer.com/](http://storageexplorer.com/). Kod źródłowy jest dostępna z również tego łącza.

Przed rozpoczęciem korzystania z narzędzia, musisz znać Azure magazynowania nazwy i konta klucz konta. Aby uzyskać instrukcje dotyczące uzyskiwanie informacji na ten temat, zobacz "jak: widok, Kopiuj i przechowywania wyniku klawisze dostępu" sekcja [Tworzenie, zarządzanie, lub usunąć konto miejsca do magazynowania][azure-create-storage-account].  

1. Uruchom Eksploratora magazynu Azure. Jeśli po raz pierwszy, musisz uruchomiono Eksploratora magazynu, zostanie wyświetlony monit dla ___nazwę konta magazynu__ i __klucz konta miejsca do magazynowania__. Jeśli uruchomiono go przed użyciem przycisk __Dodaj__ , aby dodać nową nazwę konta magazynu i klucza.

    Wprowadź nazwę i używane przez klaster HDinsight klucz konta miejsca do magazynowania, a następnie wybierz pozycję __ZAPISZ i otwórz__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Na liście kontenerów po lewej stronie interfejsu kliknij nazwę kontenera, który jest skojarzony z klaster HDInsight. Domyślnie to jest nazwą klaster HDInsight, ale mogą być inne, jeśli wprowadzono nazwę określonych podczas tworzenia klaster.

6. Na pasku narzędzi wybierz ikonę przekazywania.

    ![Pasek narzędzi z wyróżnioną ikoną przekazywania](./media/hdinsight-upload-data/toolbar.png)

7. Określ plik do przekazania, a następnie kliknij przycisk **Otwórz**. Po wyświetleniu monitu wybierz pozycję __Przekaż__ przekazać plik do głównego kontenera magazynu. Jeśli chcesz przekazać plik do określonej ścieżki, wpisz ścieżkę w polu __docelowym__ , a następnie wybierz pozycję __Przekaż__.

    ![Okno dialogowe przekazywania pliku](./media/hdinsight-upload-data/fileupload.png)
    
    Po zakończeniu przekazywania, możesz go używać z zadań w klastrze HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Magazyn obiektów Blob Azure instalacji jako dysk lokalny

Zobacz [Magazyn obiektów Blob Azure instalacji jako dysk lokalny](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Usług

###<a name="azure-data-factory"></a>Factory Azure danych

Usługa Azure Factory danych jest w pełni zarządzane usługi dla tworzenia usługi przepływu danych miejsca do magazynowania, przetwarzania danych i danych na części produkcji usprawniony, skalowalna i wiarygodnych danych.

Aby przenieść dane do magazyn obiektów Blob platformy Azure lub utworzyć kanał dane bezpośrednio przy użyciu funkcji HDInsight, takich jak gałąź i świnka można używać Azure Factory danych.

Aby uzyskać więcej informacji zobacz [dokumentację Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop to narzędzie przeznaczone do przenoszenia danych między Hadoop i relacyjnych baz danych. Jego umożliwia importowanie danych z relacyjnej bazy danych systemu zarządzania (RDBMS), takie jak SQL Server, MySQL lub Oracle do distributed system plików usługi Hadoop (HDFS), przekształcania danych w Hadoop MapReduce lub gałęzi, a następnie wyeksportuj dane do RDBMS.

Aby uzyskać więcej informacji, zobacz [Sqoop korzystanie z usługi HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>SDK opracowywania

Magazyn obiektów Blob platformy Azure można również uzyskać dostęp za pomocą SDK Azure następujące języki programowania:

* .NET
* Java
* Node.js
* PHP
* Python
* Dopiskiem

Aby uzyskać więcej informacji na temat instalowania SDK Azure zobacz [Pobieranie Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a id="storageexception"></a>Wyjątek miejsca do magazynowania dla zapisu na obiektów blob

__Objawów__: podczas korzystania z `hadoop` lub `hdfs dfs` poleceń, aby zapisywać pliki, które są ~ 12 GB lub większy w klastrze HBase może się pojawić następujący komunikat o błędzie: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Przyczyna__: HBase na HDInsight klastrów domyślny rozmiar bloku 256 KB podczas zapisywania Azure miejsca do magazynowania. Gdy to sprawdza się we interfejsy API HBase lub interfejsów API pozostałych, spowoduje błąd podczas korzystania z `hadoop` lub `hdfs dfs` narzędzi wiersza polecenia.

__Rozdzielczość__: używanie `fs.azure.write.request.size` Aby określić rozmiar bloku. Możesz to zrobić na podstawie użycia przy użyciu `-D` parametru. Oto przykład użycia parametru za pomocą `hadoop` polecenie:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Można również zwiększyć wartość `fs.azure.write.request.size` globalnie przy użyciu Ambari. Poniższe czynności można zmienić wartość w Interfejsie użytkownika witryny sieci Web Ambari:

1. W przeglądarce przejdź do Interfejsu sieci Web Ambari klaster. To https://CLUSTERNAME.azurehdinsight.net, gdzie __NAZWAKLASTRA__ to nazwa klaster.

    Po wyświetleniu monitu wprowadź nazwę administratora i hasło klaster.

2. Z lewej strony ekranu wybierz pozycję __HDFS__, a następnie wybierz kartę __podawać__ .

3. W polu __Filtr__ wprowadź `fs.azure.write.request.size`. Spowoduje to wyświetlenie pola i bieżącą wartość na środku strony.

4. Zmień wartość z 262144 (256KB) na nową wartość. Na przykład 4194304 (4MB).

![Obraz zmiany wartości za pośrednictwem Interfejsu Ambari sieci Web](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Aby uzyskać więcej informacji na temat korzystania z Ambari zobacz [Zarządzanie HDInsight klastrów za pomocą Interfejsu sieci Web Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Następne kroki

Teraz, gdy wiesz, jak mają zostać pobrane dane do usługi HDInsight, przeczytaj następujące artykuły, aby dowiedzieć się, jak przeprowadzić analizę:

* [Wprowadzenie do usługi HDInsight platformy Azure][hdinsight-get-started]
* [Przesyłanie zadań Hadoop programowy][hdinsight-submit-jobs]
* [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
* [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
