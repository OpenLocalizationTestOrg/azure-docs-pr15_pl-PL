<properties
    pageTitle="Jak używać magazyn obiektów blob (miejsca przechowywania obiektu) z C++ | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Jak używać magazyn obiektów Blob z C++  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów Blob platformy Azure to usługa, która przechowuje niestrukturalne dane w chmurze jako obiekty i obiektów blob. Magazyn obiektów blob mogą zawierać dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.

Ten przewodnik przedstawi się, jak wykonywać typowe scenariusze przy użyciu usługi Magazyn obiektów Blob platformy Azure. Przykłady są zapisywane w języku C++ i [Biblioteka klienta Azure miejsca do magazynowania dla C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariusze, w których zawierać **przekazywanie**, **Wyświetlanie**, **Pobieranie**i **Usuwanie** obiektów blob.  

>[AZURE.NOTE] Ten przewodnik jest przeznaczony dla biblioteki klienta miejsca do magazynowania Azure C++ w wersji 1.0.0 i nowszych. Zalecane wersja jest biblioteka klienta miejsca do magazynowania 2.2.0, która jest dostępna za pośrednictwem [NuGet](http://www.nuget.org/packages/wastorage) lub [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Tworzenie aplikacji C++
W tym przewodniku należy użyć funkcji miejsca do magazynowania, które mogą być uruchamiane w aplikacji C++.  

W tym celu należy zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++ i utworzyć konto Azure miejsca do magazynowania w ramach subskrypcji Azure.   

Aby zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++, można skorzystać następujących metod:

-   **Linux:** Postępuj zgodnie z instrukcjami na stronie [Biblioteka klienta Azure miejsca do magazynowania dla C++ — Plik README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Systemu Windows:** W programie Visual Studio, kliknij przycisk **Narzędzia > Menedżer pakietów NuGet > Menedżer pakietów konsoli**. Wpisz następujące polecenie w [konsoli Menedżera pakietów NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , a następnie naciśnij klawisz **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazyn obiektów Blob  
Dodaj następujące instrukcje na początek pliku C++, w której chcesz uzyskać dostęp do obiektów blob za pomocą Azure magazynowania interfejsy API obejmują:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania
Klient Azure magazynu używa parametrów połączenia miejsca do magazynowania do przechowywania punkty końcowe i poświadczenia dostępu do usług zarządzania danymi. Podczas działa w aplikacji klienckiej, należy podać parametry połączenia miejsca do magazynowania w następującym formacie, przy użyciu nazwy konta miejsca do magazynowania i klawisz dostępu miejsca do magazynowania dla konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com) wartości *Nazwa konta* i *AccountKey* na liście. Aby uzyskać informacje na kontach miejsca do magazynowania i klawisze dostępu Zobacz [O Azure miejsca do magazynowania konta](storage-create-storage-account.md). W tym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Aby przetestować aplikację w lokalnym komputerze z systemem Windows, możesz użyć Microsoft Azure [emulatora miejsca do magazynowania](storage-use-emulator.md) jest zainstalowany wraz z [Azure SDK](https://azure.microsoft.com/downloads/). Emulator miejsca do magazynowania jest narzędziem symuluje dostępne platformy Azure na komputer lokalny rozwój usługi obiektów Blob, kolejki i tabeli. W poniższym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia do swojego emulatora magazynu lokalnego:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Aby rozpocząć emulatora Azure miejsca do magazynowania, wybierz przycisk **Start** , lub naciśnij klawisz **systemu Windows** . Zacznij wpisywać **Azure emulatora miejsca do magazynowania**, a z listy aplikacji wybierz pozycję **Microsoft Azure miejsca do magazynowania emulatora** .  

Następujące próbki przyjęto założenie, że używasz jednej z tych dwóch metod uzyskiwania parametrów połączenia miejsca do magazynowania.  

## <a name="retrieve-your-connection-string"></a>Pobieranie ciągu połączenia
Za pomocą klasy **cloud_storage_account** reprezentować informacji o koncie miejsca do magazynowania. Aby pobrać informacji o koncie miejsca do magazynowania z parametrów połączenia miejsca do magazynowania, można użyj metody **analizy** .  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Następnie odwołać się do klasy **cloud_blob_client** jako pozwala pobrać obiekty reprezentujące kontenerów i przechowywane w usłudze magazyn obiektów Blob obiektów blob. Poniższy kod tworzy obiekt **cloud_blob_client** przy użyciu obiektu konta miejsca do magazynowania, możemy pobierane powyżej:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Jak: Tworzenie kontenera

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

W tym przykładzie przedstawiono sposób tworzenia kontenera, jeśli jeszcze nie istnieje:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Domyślnie nowego kontenera jest prywatna, a następnie wprowadź klucz dostępu przestrzeni dyskowej, aby pobrać obiektów blob z tego kontenera. Jeśli chcesz udostępnić pliki (BLOB) w kontenerze wszystkim użytkownikom, można ustawić kontenera, tak aby dostępna publicznie, za pomocą następującego kodu:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Każda osoba w Internecie Zobacz obiektów blob w kontenerze publicznej, ale można modyfikować lub je usunąć tylko wtedy, gdy masz klucz odpowiedni dostęp.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Jak: przekazywanie obiektów blob do kontenera
Magazyn obiektów Blob platformy Azure obsługuje blob blok i obiektów blob strony. W większości przypadków obiektów blob bloku jest zalecane typ ma być używany.  

Aby przekazać plik do blob blok, odwołać kontenera i używać go Aby odwołać obiektów blob blok. Po umieszczeniu odwołania do obiektów blob, możesz przekazać wszelkie strumienia danych do niego przez wywołanie metody **upload_from_stream** . Operacja utworzy to go nie już istnieje, czy zastąpić ją, jeśli istnieje. W poniższym przykładzie pokazano, jak przekazywać obiektów blob w kontenerze i przyjęto założenie, że kontener został już utworzony.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Możesz też Metoda **upload_from_file** umożliwia przekazywanie pliku blob blok.

## <a name="how-to-list-the-blobs-in-a-container"></a>Jak: Lista obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, najpierw odwołać kontener. Metoda **list_blobs** kontenera umożliwia następnie pobrać obiektów blob i/lub katalogi znajdujące się w nim. Aby uzyskać dostęp z bogatego zestawu właściwości i metody zwracane **list_blob_item**, musisz wywołać metodę **list_blob_item.as_blob** uzyskać obiektu **cloud_blob** lub metody **list_blob.as_directory** uzyskać obiektu cloud_blob_directory. Poniższy kod przedstawia sposób pobierania i wyjściowe identyfikator URI każdego elementu w kontenerze **Moje kontenera przykładowe** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Aby uzyskać więcej informacji o wyświetlaniu listy operacji zobacz [Zasoby dla listy Azure miejsca do magazynowania w języku C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Jak: pobieranie obiektów blob
Aby pobrać obiektów blob, najpierw pobrać odwołania do obiektów blob, a następnie wywołać metodę **download_to_stream** . W poniższym przykładzie użyto metody **download_to_stream** , aby przenieść zawartość obiektów blob do obiektu strumienia, który można następnie utrzymują się do lokalnego pliku.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Za pomocą metody **download_to_file** można również pobrać zawartość obiektów blob do pliku.
Ponadto można również użyj metody **download_text** Aby pobrać zawartość obiektów blob jako ciąg tekstowy.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Jak: usuwanie obiektów blob
Aby usunąć obiektów blob, najpierw odwołać obiektów blob, a następnie wywołać metodę **delete_blob** nad nim.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Następne kroki
Teraz, gdy znasz już podstawy magazyn obiektów blob, wykonaj te łącza, aby dowiedzieć się więcej o magazyn Azure.  

-   [Jak używać magazyn kolejek z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Jak używać magazyn tabel z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Zasoby dla listy Azure miejsca do magazynowania w języku C++](storage-c-plus-plus-enumeration.md)
-   [Biblioteka klienta miejsca do magazynowania dla odwołania C++](http://azure.github.io/azure-storage-cpp)
-   [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
