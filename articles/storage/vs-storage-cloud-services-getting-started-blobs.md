<properties
    pageTitle="Wprowadzenie do obiektów blob miejsca do magazynowania i Visual Studio połączone services (usługi w chmurze) | Microsoft Azure"
    description="Jak rozpocząć pracę z magazynem obiektów Blob platformy Azure w chmurze usługi projektu w programie Visual Studio po nawiązywanie połączenia z kontem miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure i Visual Studio połączenia usług (cloud services projektów)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

W tym artykule opisano, jak rozpocząć pracę z magazynem obiektów Blob platformy Azure po utworzeniu lub odwołanie do konta usługi Magazyn Azure za pomocą okna dialogowego Visual Studio **Dodaj usługi połączone** w projekcie programu Visual Studio cloud services. Pokażemy jak dostępu i tworzenia obiektów blob kontenerów i jak wykonywanie typowych zadań, takich jak przekazywanie, wyświetlanie i pobieranie obiektów blob. Przykłady są zapisywane w C\# i używania [Biblioteki klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Magazyn obiektów Blob platformy Azure to usługa do przechowywania dużych ilości danych niestrukturalne, które są dostępne z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. Pojedynczy obiektów blob może mieć dowolne rozmiary. Blob może być elementów takich jak obrazy, pliki audio i wideo, dane i plików dokumentów.

Tak, jak pliki są przechowywane w folderach, magazyn obiektów blob na żywo w kontenerach. Po utworzeniu miejsca do magazynowania, możesz utworzyć jeden lub więcej kontenerów w magazynie. Na przykład w magazynie o nazwie "Wycinkami", można tworzyć kontenery w magazynie o nazwie "obrazy" w celu przechowywania obrazów i innej o nazwie "audio" do przechowywania plików dźwiękowych. Po utworzeniu kontenery, możesz przekazać pliki poszczególnych obiektów blob do nich.

- Aby uzyskać więcej informacji na programowy manipulowania obiektami blob zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md).
- Ogólne informacje dotyczące magazynu Azure [przechowywania](https://azure.microsoft.com/documentation/services/storage/)dokumentacji.
- Aby uzyskać ogólne informacje na temat usług w chmurze Azure zobacz [dokumentacji usług w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/).
- Aby uzyskać więcej informacji na temat programowania aplikacji ASP.NET zobacz [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Kontenery obiektów blob dostęp w kodzie

Aby programowy dostęp do obiektów blob w projektach usługi w chmurze, musisz dodać następujące elementy, gdy nie są one już Prezentuj.

1. Dodaj następujące deklaracje nazw kodu do początku dowolnego pliku C# w którym chcesz programowy dostęp do magazynowania Azure.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i informacje o koncie miejsca do magazynowania z konfigurację usługi Azure usługi.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Uzyskaj obiekt **CloudBlobClient** zostać utworzone odwołanie istniejącego kontenera na koncie miejsca do magazynowania.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Uzyskaj obiekt **CloudBlobContainer** , aby odwołać kontenera określonych obiektów blob.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Za pomocą cały kod pokazano w poprzedniej procedurze przed kodem pokazane w poniższych sekcjach.

## <a name="create-a-container-in-code"></a>Tworzenie kontenera w kodzie

> [AZURE.NOTE] Niektóre funkcje interfejsu API, które wykonują połączenia się z magazynem Azure w programie ASP.NET są asynchroniczne. Aby uzyskać więcej informacji, zobacz [asynchroniczno programowania z asynchroniczne i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kod w poniższym przykładzie założono, że używasz metody programowania asynchroniczne.

Aby utworzyć kontener na koncie miejsca do magazynowania, jest wszystko, co należy zrobić, dodać wywołanie **CreateIfNotExistsAsync** tak jak poniższy kod:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Aby udostępnić pliki w kontenerze dla wszystkich osób, możesz ustawić kontenera, tak aby była publicznych przy użyciu następującego kodu.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Każda osoba w Internecie Zobacz obiektów blob w kontenerze publicznej, ale można modyfikować lub je usunąć tylko wtedy, gdy masz klucz odpowiedni dostęp.

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Magazyn Azure obsługuje blob blok i obiektów blob strony. W większości przypadków obiektów blob bloku jest zalecane typ ma być używany.

Aby przekazać plik do blob blok, odwołać kontenera i używać go Aby odwołać obiektów blob blok. Po umieszczeniu odwołania do obiektów blob, możesz przekazać wszelkie strumienia danych do niego przez wywołanie metody **UploadFromStream** . Operacja tworzy to, jeśli wcześniej nie istnieje lub zastępuje go, jeśli istnieje. W poniższym przykładzie pokazano, jak przekazywać obiektów blob w kontenerze i przyjęto założenie, że kontener został już utworzony.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, najpierw odwołać kontener. Metoda **ListBlobs** kontenera umożliwia następnie pobrać obiektów blob i/lub katalogi znajdujące się w nim. Aby uzyskać dostęp z bogatego zestawu właściwości i metod dla zwracane **IListBlobItem**, możesz zrzutowania go do obiektu **CloudBlockBlob**, **CloudPageBlob**lub **CloudBlobDirectory** . Jeśli typ jest nieznany, umożliwia kontrolę typu dokonać zrzutowania go do. Poniższy kod przedstawia sposób pobierania i wyjściowe identyfikator URI każdego elementu w kontenerze **zdjęcia** :

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

Jak pokazano w poprzednim przykładzie kodu, usługa obiektów blob ma pojęcia katalogów w kontenerach, a także. Jest to, dzięki czemu można uporządkować z obiektami BLOB w strukturze więcej przypominających dla folderu. Na przykład można rozważyć następujący zestaw obiektów blob bloku w kontenerze o nazwie **zdjęcia**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Podczas połączenia **ListBlobs** w kontenerze (tak jak w poprzednim próbka), Kolekcja zwracana zawiera obiekty **CloudBlobDirectory** i **CloudBlockBlob** reprezentujące katalogów i obiektów blob zawartych na najwyższym poziomie. Oto wyniki:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Opcjonalnie można ustawić parametr **UseFlatBlobListing** metody **ListBlobs** na **wartość true**. Powoduje wszystkich obiektów blob zwracanych jako **CloudBlockBlob**, bez względu na to katalogu. Oto wywołanie **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a Oto wyniki:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Aby uzyskać więcej informacji zobacz [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Pobieranie obiektów blob

Aby pobrać obiektów blob, najpierw pobrać odwołania do obiektów blob, a następnie wywołać metodę **DownloadToStream** . W poniższym przykładzie użyto metody **DownloadToStream** , aby przenieść zawartość obiektów blob do obiektu strumienia, który można następnie utrzymują się do lokalnego pliku.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Metoda **DownloadToStream** umożliwia również pobrać zawartość obiektów blob jako ciąg tekstowy.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Usuwanie obiektów blob

Aby usunąć obiektów blob, najpierw odwołać obiektów blob, a następnie wywołać metody **Delete** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Lista obiektów blob na stronach asynchroniczne

Jeśli są pozycje dużej liczby obiektów blob lub chcesz kontrolować liczbę wyników zwrócenie w jednej operacji pozycję na liście, można wyświetlić listę obiektów blob na stronach wyników. W tym przykładzie pokazano sposób zwracało wyniki na stronach asynchroniczne tak, aby wykonanie nie jest zablokowane podczas oczekiwania, aby zwrócić duży zestaw wyników.

W tym przykładzie przedstawiono prostym blob, wskazując, ale można również wykonać hierarchiczną listę, ustawiając parametr **useFlatBlobListing** metody **ListBlobsSegmentedAsync** na **wartość false**.

Ponieważ metoda przykładowe wywołuje metodę asynchroniczne, musi być nazwą **asynchroniczne** słów kluczowych i musi zwracać obiekt **zadania** . Słowo kluczowe await określony dla metody **ListBlobsSegmentedAsync** zawiesza wykonywanie metody próbki, aż do zakończenia zadań Lista.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
