<properties
    pageTitle="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Python | Microsoft Azure"
    description="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Python"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Python

W tym temacie opisano, jak do listy, przekazywania i pobierania za pomocą interfejsu API Python obiektów blob. Przy użyciu interfejsu API Python opisane w Azure SDK możesz wykonywać następujące czynności:

- Tworzenie kontenera
- Przekazywanie obiektów blob do kontenera
- Pobieranie obiektów blob
- Lista obiektów blob w kontenerze
- Usuwanie obiektów blob

Aby uzyskać więcej informacji na temat korzystania z interfejsu API Python zobacz [jak korzystać z usługi Magazyn obiektów Blob Python](../storage/storage-python-how-to-use-blob-storage.md).

Wytyczne dotyczące technologii umożliwia przenoszenie danych do i/lub z magazynem obiektów Blob platformy Azure są połączone poniżej:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Jeśli używasz maszyn wirtualnych, który został skonfigurowany za pomocą skryptów dostarczony przez [maszyn wirtualnych nauki danych platformy Azure](machine-learning-data-science-virtual-machines.md)AzCopy jest już zainstalowany na maszyn wirtualnych.

> [AZURE.NOTE] Pełne wprowadzenie z magazynem obiektów blob platformy Azure odwoływać się do [Podstawy obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usługą [obiektów Blob platformy Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Wymagania wstępne

Ten dokument przyjęto założenie, że masz subskrypcję usługi Azure, konto miejsca do magazynowania i odpowiedni klucz miejsca do magazynowania dla tego konta. Przed przekazywania i pobierania danych, musisz znać Azure magazynowania nazwy i konta klucz konta.

- Aby skonfigurować Azure subskrypcji, zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).

- Aby uzyskać instrukcje dotyczące tworzenia konta miejsca do magazynowania i Uzyskiwanie konta i informacje o kluczu zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Przekazywanie danych do obiektów Blob

Dodaj poniższy fragment u góry dowolnego kodu Python, w którym chcesz programowy dostęp do magazynowania Azure:

    from azure.storage.blob import BlobService

Obiekt **BlobService** umożliwia pracę z kontenerów i obiektów blob. Poniższy kod tworzy obiekt BlobService przy użyciu klucza konta i nazwę konta magazynu. Zamień nazwę konta i klucz konta swoje konto rzeczywistą i klucza.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Aby przekazać danych do obiektów blob, należy skorzystać z następujących metod:

1. umieszczenie\_blok\_obiektów blob\_z\_path (przesłane zawartość pliku z określonej ścieżce)
2. umieszczenie\_block_blob\_z\_pliku (przesłane zawartość ze już otwarty plik/strumienia)
3. umieszczenie\_blok\_obiektów blob\_z\_bajtów (przekazywanie i tablicę bajtów)
4. umieszczenie\_blok\_obiektów blob\_z\_tekstu (przekazywanie wartość określony tekst przy użyciu określonego kodowania)

Następujący kod spowoduje przekazanie pliku lokalnego do kontenera:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Poniższy przykład kodu przekazywanie wszystkich plików (z wyłączeniem katalogów) w katalogu lokalnym magazynem obiektów blob:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Pobieranie danych z obiektów Blob

Pobieranie danych z obiektów blob za pomocą następujących metod:
1. Uzyskiwanie\_obiektów blob\_do\_ścieżki
2. Uzyskiwanie\_obiektów blob\_do\_pliku
3. Uzyskiwanie\_obiektów blob\_do\_bajtów
4. Uzyskiwanie\_obiektów blob\_do\_tekstu

Te metody, które wykonują niezbędne segmentu gdy rozmiar danych przekracza 64 MB.

Poniższy przykład kodu pobieranie zawartości obiektów blob w kontenerze do lokalnego pliku:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Poniższy przykład kodu pobiera wszystkie obiekty BLOB z kontenera. Ta lista\_obiektów blob, aby wyświetlić listę dostępnych obiektów blob w kontenerze i pobiera je do katalogu lokalnego.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
