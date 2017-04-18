<properties
    pageTitle="Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure (magazyn obiektów) przy użyciu programu .NET | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów Blob platformy Azure to usługa, która przechowuje niestrukturalne dane w chmurze jako obiekty i obiektów blob. Magazyn obiektów blob mogą zawierać dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.

### <a name="about-this-tutorial"></a>Informacje o tym samouczku

Ten samouczek pokazano, jak pisać kod .NET kilka typowych scenariuszy przy użyciu magazyn obiektów Blob platformy Azure. Przekazywanie, wyświetlanie, pobieranie i usuwanie obiektów blob są następujące scenariusze objęta.

**Szacowany czas:** 45 minut

**Prerequisities:**

- [Program Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Menedżer konfiguracji programu .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Konto Azure miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Więcej przykładów

Aby uzyskać dodatkowe przykłady przy użyciu magazyn obiektów Blob zobacz [Wprowadzenie do magazyn obiektów Blob Azure w .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Możesz pobrać aplikację próbki i uruchom go lub przeglądanie kodu na GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodawanie deklaracje przestrzeni nazw

Dodaj następujący `using` instrukcje na początek `program.cs` pliku:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Analizowanie parametry połączenia

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Tworzenie klienta usługi obiektów Blob

Klasa **CloudBlobClient** umożliwia pobieranie kontenerów i przechowywane w magazynie obiektów Blob obiektów blob. W tym miejscu jest jednym ze sposobów tworzenia klienta usługi:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Teraz możesz przystąpić do pisania kodu, który odczytuje i zapisuje dane z magazynem obiektów Blob.

## <a name="create-a-container"></a>Tworzenie kontenera

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

W tym przykładzie przedstawiono sposób tworzenia kontenera, jeśli jeszcze nie istnieje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Domyślnie nowe kontener jest prywatny, co oznacza, wprowadź klucz dostępu przestrzeni dyskowej, aby pobrać obiektów blob z tego kontenera. Jeśli chcesz udostępnić pliki w kontenerze dla wszystkich osób, można ustawić kontenera, tak aby dostępna publicznie, za pomocą następującego kodu:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Każda osoba w Internecie Zobacz obiektów blob w kontenerze publicznej, ale można modyfikować lub je usunąć tylko wtedy, gdy klawisz dostępu odpowiednie konto lub podpis udostępniania.

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Magazyn obiektów Blob platformy Azure obsługuje blob blok i obiektów blob strony.  W większości przypadków obiektów blob bloku jest zalecane typ ma być używany.

Aby przekazać plik do blob blok, odwołać kontenera i używać go Aby odwołać obiektów blob blok. Po umieszczeniu odwołania do obiektów blob, możesz przekazać strumieniem danych do niego przez wywołanie metody **UploadFromStream** . Operacja utworzy to go nie już istnieje, czy zastąpić ją, jeśli istnieje.

W poniższym przykładzie pokazano, jak przekazywać obiektów blob w kontenerze i przyjęto założenie, że kontener został już utworzony.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, najpierw odwołać kontener. Metoda **ListBlobs** kontenera umożliwia następnie pobrać obiektów blob i/lub katalogi znajdujące się w nim. Aby uzyskać dostęp z bogatego zestawu właściwości i metod dla zwracane **IListBlobItem**, możesz zrzutowania go do obiektu **CloudBlockBlob**, **CloudPageBlob**lub **CloudBlobDirectory** .  Jeśli typ jest nieznany, można dokonać zrzutowania go, aby za pomocą wyboru typu.  Poniższy kod przedstawia sposób pobierania i wyjściowy identyfikator URI każdego elementu w `photos` kontenera:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Jak pokazano powyżej, możesz nazw obiektów blob informacjami ścieżki w ich nazwy. Spowoduje to utworzenie strukturę katalogu wirtualnego, który można organizować i przechodzenie przez jak systemu plików tradycyjny. Należy zauważyć, że tylko strukturę katalogu jest wirtualna - tylko zasobów dostępnych w magazynie obiektów Blob są kontenerów i obiektów blob. Jednak biblioteka klienta miejsca do magazynowania ma obiektu **CloudBlobDirectory** katalog wirtualny i uprościć proces pracy z obiektami blob, które są zorganizowane w ten sposób.

Na przykład można rozważyć następujący zestaw obiektów blob bloku w kontenerze o nazwie `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Podczas połączenia **ListBlobs** w kontenerze "zdjęć" (tak jak w próbce powyżej), zwracany jest lista hierarchiczna. Prezentacja zawiera obiekty zarówno **CloudBlobDirectory** , jak i **CloudBlockBlob** reprezentujących katalogów i obiektów blob w kontenerze, odpowiednio. Dane wyjściowe wygląda:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Opcjonalnie można ustawić parametr **UseFlatBlobListing** metody **ListBlobs** na **wartość true**. W tym przypadku wszystkich obiektów blob w kontenerze są zwracane jako obiektu **CloudBlockBlob** . Połączenie **ListBlobs** zwraca listę prostym wygląda następująco:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

i wyniki wyglądać podobnie do następującej:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Pobieranie obiektów blob

Aby pobrać obiektów blob, najpierw pobrać odwołania do obiektów blob, a następnie wywołać metodę **DownloadToStream** . W poniższym przykładzie użyto metody **DownloadToStream** , aby przenieść zawartość obiektów blob do obiektu strumienia, który można następnie utrzymują się do lokalnego pliku.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Metoda **DownloadToStream** umożliwia również pobrać zawartość obiektów blob jako ciąg tekstowy.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Usuwanie obiektów blob

Aby usunąć obiektów blob, najpierw odwołać obiektów blob, a następnie wywołać metody **Delete** nad nim.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Lista obiektów blob na stronach asynchroniczne

Jeśli są pozycje dużej liczby obiektów blob lub chcesz kontrolować liczbę wyników zwrócenie w jednej operacji pozycję na liście, można wyświetlić listę obiektów blob na stronach wyników. W tym przykładzie pokazano sposób zwracało wyniki na stronach asynchroniczne tak, aby wykonanie nie jest zablokowane podczas oczekiwania, aby zwrócić duży zestaw wyników.

W tym przykładzie przedstawiono prostym blob, wskazując, ale można również wykonać hierarchiczną listę, ustawiając `useFlatBlobListing` parametr metody **ListBlobsSegmentedAsync** do `false`.

Ponieważ metoda przykładowe wywołuje metodę asynchroniczne, musisz poprzedzone z `async` słowo kluczowe, a musi zwracać obiekt **zadania** . Słowo kluczowe await określony dla metody **ListBlobsSegmentedAsync** zawiesza wykonywanie metody próbki, aż do zakończenia zadań Lista.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Zapisywanie obiektów blob dołączającej

Dołączanie obiektów blob jest nowego typu obiektów blob, wprowadzone w wersji 5.x Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET. Dołączanie obiektów blob jest zoptymalizowana pod kątem dołączającej operacji, takich jak rejestrowanie. Jak blob blok obiektów blob dołączającej składa się z bloków, ale można dodać nowy blok do obiektów blob dołączającej, zawsze jest dołączany do końca obiektów blob. Nie można zaktualizować lub usuń istniejący blok w obiektów blob Dołącz. Jak są one dla obiektów blob blok nie dostępne są blok identyfikatorów dla obiektów blob Dołącz.

Każdy blok w obiektów blob dołączającej może być inny rozmiar, maksymalnie 4 MB i obiektów blob dołączającej może zawierać maksymalnie 50 000 bloków. Maksymalny rozmiar obiektów blob dołączającej w związku z tym jest nieco więcej niż 195 GB (blokuje 4 MB X 50 000).

W poniższym przykładzie tworzy nowe blob dołączającej i dołącza niektóre dane, symulowanie operacji rejestrowania prosty.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Aby uzyskać więcej informacji o różnicach między trzy typy obiektów blob, zobacz [opis bloku blob, blob strony i dołączanie obiektów blob](https://msdn.microsoft.com/library/azure/ee691964.aspx) .

## <a name="managing-security-for-blobs"></a>Zarządzanie zabezpieczeniami dla obiektów blob

Domyślnie Magazyn Azure chroni dane przez ograniczanie dostępu do właściciela konta, która znajduje się posiadanie klawiszy dostępu do konta. Udostępnianie danych obiektów blob na koncie miejsca do magazynowania w razie potrzeby należy zrobić bez utraty bezpieczeństwa klawiszy dostępu do konta. Ponadto można zaszyfrować obiektów blob danych, aby upewnić się, że jest bezpieczne, przechodząc przez sieć i w magazynie Azure.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Kontrolowanie dostępu do danych typu blob

Domyślnie dane obiektów blob na koncie miejsca do magazynowania jest dostępna tylko dla właściciela konta miejsca do magazynowania. Uwierzytelnianie żądania magazyn obiektów Blob wymaga klucza dostępu do konta domyślnie. Jednak możesz udostępnić innym użytkownikom niektóre dane obiektów blob. Dostępne są dwie opcje:

- **Dostęp anonimowy:** Mogą być kontenera lub jego obiektów blob publicznie dostępne dla dostęp anonimowy. Aby uzyskać więcej informacji, zobacz [Zarządzanie anonimowe dostęp do odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md) .
- **Podpisy dostępu udostępnione:** Klientów można udostępnić podpis udostępniania (SA), który udostępnia delegowanej zasobów na koncie miejsca do magazynowania z uprawnień, które można określić i przedziale, określonym przez użytkownika. Aby uzyskać więcej informacji, zobacz [Za pomocą udostępnionego dostępu podpisów (SA)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Szyfrowanie danych obiektów blob

Azure magazynowania obsługuje szyfrowanie obiektów blob danych zarówno na komputerze klienckim, jak i na serwerze:

- **Szyfrowania po stronie klienta:** Biblioteka klienta miejsca do magazynowania dla środowiska .NET obsługuje szyfrowanie danych w aplikacjach klienckich przed przekazaniem do magazynu Azure i odszyfrowywanie danych podczas pobierania do klienta. Biblioteki obsługuje także integracja z magazynu klucza Azure dla zarządzania kluczami konta miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [Szyfrowania po stronie klienta przy użyciu .NET dla programu Microsoft Azure miejsca do magazynowania](storage-client-side-encryption.md) . Zobacz też [Samouczek: szyfrowania i odszyfrowywania obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure](storage-encrypt-decrypt-blobs-key-vault.md).
- **Szyfrowanie po stronie serwera**: Magazyn Azure obsługuje teraz szyfrowania po stronie serwera. Zobacz [szyfrowanie usługi Azure miejsca do magazynowania danych w pozostałych (wersja Preview)](storage-service-encryption.md).

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy magazyn obiektów Blob, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

### <a name="microsoft-azure-storage-explorer"></a>Eksplorator magazynu platformy Microsoft Azure
- [Microsoft Azure miejsca do magazynowania Eksploratora (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) to bezpłatne, aplikacja autonomicznego firmy Microsoft, który umożliwia pracę wizualnie Azure miejsca do magazynowania danych w systemie Windows, OS X i Linux.

### <a name="blob-storage-samples"></a>Przykłady magazyn obiektów blob

- [Wprowadzenie do programu magazynem obiektów Blob platformy Azure w .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Odwołanie magazyn obiektów blob

- [Biblioteka klienta miejsca do magazynowania dla odwołania .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Odwołanie interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Prowadnice koncepcyjny

- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
- [Wprowadzenie do przechowywania plików dla środowiska .NET](storage-dotnet-how-to-use-files.md)
- [Jak korzystać z zestawu SDK WebJobs magazyn obiektów blob platformy Azure](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
