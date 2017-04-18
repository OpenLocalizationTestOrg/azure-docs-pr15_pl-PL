<properties
    pageTitle="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą AzCopy | Microsoft Azure"
    description="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą AzCopy"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą AzCopy

AzCopy to narzędzie wiersza polecenia przeznaczone do przekazywania, pobierania i kopiowanie danych do i z obiektów blob Microsoft Azure, plików i Magazyn tabel.

Aby uzyskać instrukcje dotyczące instalowania AzCopy i dodatkowe informacje dotyczące używania go z platformy Azure zobacz [Wprowadzenie do narzędzia wiersza polecenia AzCopy](../storage/storage-use-azcopy.md).

Wytyczne dotyczące technologii umożliwia przenoszenie danych do i/lub z magazynem obiektów Blob platformy Azure są połączone poniżej:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Jeśli używasz maszyn wirtualnych, który został skonfigurowany za pomocą skryptów dostarczony przez [maszyn wirtualnych nauki danych platformy Azure](machine-learning-data-science-virtual-machines.md)AzCopy jest już zainstalowany na maszyn wirtualnych.

> [AZURE.NOTE] Pełne wprowadzenie z magazynem obiektów blob platformy Azure odwoływać się do [Podstawy obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usługą [obiektów Blob platformy Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Wymagania wstępne

Ten dokument przyjęto założenie, że masz subskrypcję usługi Azure, konto miejsca do magazynowania i odpowiedni klucz miejsca do magazynowania dla tego konta. Przed przekazywania i pobierania danych, musisz znać Azure magazynowania nazwy i konta klucz konta.

- Aby skonfigurować Azure subskrypcji, zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).

- Aby uzyskać instrukcje dotyczące tworzenia konta miejsca do magazynowania i Uzyskiwanie konta i informacje o kluczu zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Uruchamianie polecenia AzCopy

Aby uruchomić polecenia AzCopy, Otwórz okno poleceń i przejdź do katalogu AzCopy instalacji na komputerze, w którym znajduje się plik wykonywalny AzCopy.exe. 

Podstawowa składnia polecenia AzCopy jest:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Można dodać lokalizację AzCopy do ścieżki systemowej, a następnie uruchom poleceń z dowolnego katalogu. Domyślnie AzCopy jest zainstalowany *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* lub *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Przekazywanie plików do obiektów blob platformy Azure

Aby przekazać plik, wykonaj następujące polecenie:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Pobieranie plików z obiektów blob platformy Azure

Aby pobrać plik z obiektów blob platformy Azure, użyj następującego polecenia:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Przenoszenie obiektów blob między Azure kontenerów

Aby przenieść obiektów blob między Azure kontenery, użyj następującego polecenia:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Porady dotyczące korzystania z AzCopy

> [AZURE.TIP]   
> 1. Podczas **przekazywania** plików */S* przekazywanie lokalizacji plików. Bez ten parametr nie są przekazać pliki w podkatalogów.  
> 2. **Pobieranie** pliku */S* wyszukiwania lokalizacji kontenera, poczekaj, aż wszystkie pliki w określonym katalogu i jego podkatalogów lub wszystkie pliki, które są zgodne z określonego wzorca w danym katalogu i jego podkatalogów, są pobierane.  
> 3.  Nie można określić **określonych obiektów blob plik** do pobrania przy użyciu parametru */Source* . Aby pobrać określonego pliku, określ nazwę pliku obiektów blob, aby pobrać przy użyciu parametru */Pattern* . Parametr **/S** można mieć AzCopy poszukaj lokalizacji deseniu nazwę pliku. Bez parametru deseniu AzCopy pobiera wszystkie pliki w tym katalogu.
