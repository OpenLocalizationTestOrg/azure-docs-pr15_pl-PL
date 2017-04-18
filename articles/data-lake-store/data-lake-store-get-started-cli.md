<properties
   pageTitle="Rozpoczynanie pracy z magazynu Lake danych za pomocą interfejsu wiersza polecenia i platform | Microsoft Azure"
   description="Utworzenie konta magazynu Lake danych i wykonywania podstawowych operacji za pomocą wiersza polecenia i platform Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą wiersza polecenia Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak utworzyć konto Azure magazynu Lake danych i wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych za pomocą interfejsu wiersza polecenia Azure, usuwanie swojego konta, itp. Aby uzyskać więcej informacji na temat magazynu Lake danych zobacz [Omówienie pakietu Lake magazynu danych](data-lake-store-overview.md).

Polecenie Azure jest zaimplementowana w Node.js. Pozwala na dowolnej platformie, która obsługuje Node.js, łącznie z systemem Windows, Mac i Linux. Polecenie Azure jest Otwórz źródło. Kod źródłowy odbywa się w GitHub w <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. W tym artykule opisano, jak tylko za pomocą interfejsu wiersza polecenia Azure z magazynu Lake danych. Aby ogólne wskazówki dotyczące używania polecenie Azure, zobacz [jak polecenie Azure za pomocą] [azure-command-line-tools].


##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Polecenie azure** — zobacz [zainstalować i skonfigurować polecenie Azure](../xplat-cli-install.md) uzyskać informacje dotyczące instalacji i konfiguracji. Upewnij się, że możesz ponownie uruchom komputer po zainstalowaniu interfejsu wiersza polecenia.

## <a name="authentication"></a>Uwierzytelnianie

Ten artykuł jest używana prostsze uwierzytelniania z magazynu Lake danych miejsce, w którym możesz zalogować się jako użytkownik użytkowników końcowych. Poziom dostępu do konta i system plików podlega następnie poziom dostępu zalogowany użytkownik magazynu Lake danych. Istnieją inne metody także do uwierzytelnienia z magazynu Lake danych, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Zaloguj się do subskrypcji usługi Azure

1. Wykonaj kroki opisane w [Nawiązywanie połączenia z subskrypcji usługi Azure z interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-connect.md) i łączenie się z subskrypcji przy użyciu `azure login` metody.

2. Lista subskrypcji, które są skojarzone z przy użyciu konta `azure account list` polecenia.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Z powyższych danych wyjściowych **Azure-sub-1** jest włączona, a innej subskrypcji jest **Azure-sub-2**. 

3. Wybierz subskrypcję, którą chcesz pracować w obszarze. Jeśli chcesz pracować w obszarze subskrypcji Azure-sub-2, za pomocą `azure account set` polecenia.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Utwórz konto Azure magazynu Lake danych

Otwórz wiersz polecenia, powłoki lub takiej sesji i uruchom następujące polecenia.

2. Przełącz się do trybu Menedżer zasobów Azure za pomocą następujące polecenie:

        azure config mode arm


5. Tworzenie nowej grupy zasobów. W następujące polecenie Podaj wartości parametru, którego chcesz użyć.

        azure group create <resourceGroup> <location>

    Jeśli nazwa lokalizacji zawiera spacje, należy umieścić go w cudzysłowie. Na przykład "wschodniego US 2".

5. Tworzenie konta magazynu Lake danych.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Tworzenie folderów w sklepie Lake danych

Można tworzyć foldery w obszarze konta magazynu Lake Azure danych do przechowywania danych i zarządzanie nimi. Użyj następującego polecenia, aby utworzyć folder w katalogu głównym magazynie Lake danych o nazwie "mynewfolder".

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Na przykład:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Przekazywanie danych do sklepu Lake danych

Możesz przekazać danych do magazynu Lake danych bezpośrednio na poziomie głównym lub do folderu, który został utworzony w ramach tego konta. Wstawki poniżej pokazano, jak przekazać kilka przykładowych danych do folderu (**mynewfolder**) została utworzona w poprzedniej sekcji.

Jeśli szukasz kilka przykładowych danych do przekazania zostanie wyświetlony folder **Pogotowie danych** z [Repozytorium cyfra Lake danych Azure](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisać go w katalogu lokalnym na komputerze, na przykład C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Na przykład:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Lista plików w magazynie Lake danych

Użyj następującego polecenia, aby wyświetlić listę plików na koncie magazynu Lake danych.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Na przykład:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Wyniki powinny być podobny do następującego:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Zmienianie nazwy, pobieranie i usuwanie danych z magazynu Lake danych

* **Aby zmienić nazwę pliku**, użyj następującego polecenia:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Na przykład:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Do pobrania pliku**, użyj następującego polecenia. Upewnij się, że ścieżka docelowa już istnieje.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Na przykład:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Aby usunąć plik**, użyj następującego polecenia:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Na przykład:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Po wyświetleniu monitu wprowadź **Y** , aby usunąć element.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Wyświetlanie listy kontroli dostępu dla folderu w magazynie Lake danych

Użyj następującego polecenia, aby wyświetlić listy kontroli dostępu do folderu magazynu Lake danych. W bieżącej wersji ACL można ustawić tylko w katalogu głównym magazynie Lake danych. Tak poniżej parametr path jest zawsze główny (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Na przykład:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Usuwanie konta magazynu Lake danych

Użyj następującego polecenia, aby usunąć konto magazynu Lake danych.

    azure datalake store account delete <dataLakeStoreAccountName>

Na przykład:

    azure datalake store account delete mynewdatalakestore

Po wyświetleniu monitu wprowadź **Y** , aby usunąć konto.


## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
