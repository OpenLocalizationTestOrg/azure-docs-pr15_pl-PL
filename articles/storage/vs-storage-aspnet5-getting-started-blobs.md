<properties
    pageTitle="Wprowadzenie do obiektów blob miejsca do magazynowania i Visual Studio połączenia usług (ASP.NET 5) | Microsoft Azure"
    description="Jak rozpocząć pracę przy użyciu magazyn obiektów Blob platformy Azure w projekcie programu Visual Studio ASP.NET 5 po utworzeniu konta miejsca do magazynowania, za pomocą usług programu Visual Studio połączenia"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Wprowadzenie do obiektów Blob platformy Azure miejsca do magazynowania i Visual Studio połączenia usług (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Omówienie

W tym artykule opisano, jak rozpocząć korzystanie magazyn obiektów Blob platformy Azure w programie Visual Studio, po utworzony lub odwołanie do konta Azure miejsca do magazynowania w projekcie ASP.NET 5 przy użyciu okna dialogowego Visual Studio Dodaj połączenie usługi.

Magazyn obiektów Blob platformy Azure to usługa do przechowywania dużych ilości danych niestrukturalne, które są dostępne z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. Pojedynczy obiektów blob może mieć dowolne rozmiary. Blob może być elementów takich jak obrazy, pliki audio i wideo, dane i plików dokumentów. W tym artykule opisano, jak rozpocząć pracę z magazynem obiektów blob po utworzeniu konta magazynu platformy Azure za pomocą okna dialogowego Visual Studio **Dodaj usługi połączone** w projekcie ASP.NET 5.

Tak, jak pliki są przechowywane w folderach, magazyn obiektów blob na żywo w kontenerach. Po utworzeniu miejsca do magazynowania, możesz utworzyć jeden lub więcej kontenerów w magazynie. Na przykład w magazynie o nazwie "Wycinkami", można tworzyć kontenery w magazynie o nazwie "obrazy" w celu przechowywania obrazów i innej o nazwie "audio" do przechowywania plików dźwiękowych. Po utworzeniu kontenery, możesz przekazać pliki poszczególnych obiektów blob do nich. Aby uzyskać więcej informacji na programowy manipulowania obiektami blob, zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Kontenery obiektów blob dostęp w kodzie

Aby programowy dostęp do obiektów blob w projektach ASP.NET 5, musisz dodać następujące elementy, gdy nie są one już Prezentuj.

1. Dodaj następujące deklaracje nazw kodu do początku dowolnego pliku C# w którym chcesz programowy dostęp do magazynowania Azure.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i informacje o koncie miejsca do magazynowania z konfigurację usługi Azure usługi.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Uwaga:** W poniższych sekcjach za pomocą cały kod powyżej przed kodem.


3. Za pomocą obiektu **CloudBlobClient** odwołać **CloudBlobContainer** do istniejącego kontenera na koncie miejsca do magazynowania.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Tworzenie kontenera w kodzie

Za pomocą **CloudBlobClient** Aby utworzyć kontener na koncie miejsca do magazynowania. Wszystko, co należy zrobić jest dodanie połączenia **CreateIfNotExistsAsync** tak jak poniższy kod:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Uwaga:** Interfejsy API, które wykonują połączenia z magazynem Azure 5 ASP.NET są asynchroniczne. Aby uzyskać więcej informacji, zobacz [asynchroniczno programowania z asynchroniczne i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kod poniżej założono, że metody programowania asynchroniczne są używane.

Aby udostępnić pliki w kontenerze dla wszystkich osób, możesz ustawić kontenera, tak aby była publicznych przy użyciu następującego kodu.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Aby przekazać plik obiektów blob do kontenera, odwołać kontenera i używać go do odwołać obiektów blob. Po umieszczeniu odwołania do obiektów blob, możesz przekazać wszelkie strumienia danych do niego przez wywołanie metody **UploadFromStreamAsync** . Operacja tworzy to, jeśli jeszcze nie jest lub zastępuje go, jeśli istnieje. W poniższym przykładzie pokazano, jak przekazywać obiektów blob w kontenerze i przyjęto założenie, że kontener został już utworzony.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, najpierw odwołać kontener. Następnie możesz zadzwonić kontenera **ListBlobsSegmentedAsync** metoda pobierania obiektów blob i/lub katalogi znajdujące się w nim. Aby uzyskać dostęp z bogatego zestawu właściwości i metod dla zwracane **IListBlobItem**, możesz zrzutowania go do obiektu **CloudBlockBlob**, **CloudPageBlob**lub **CloudBlobDirectory** . Jeśli nie znasz typu obiektów blob umożliwia kontrolę typu dokonać zrzutowania go do. Poniższy kod ilustruje sposób pobierania i wyjściowy identyfikator URI każdego elementu w kontenerze.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Istnieją inne sposoby na wyświetlanie zawartości kontenera obiektów blob. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Pobieranie obiektów blob
Aby pobrać obiektów blob, najpierw odwołać się do obiektów blob, a następnie wywołać metodę **DownloadToStreamAsync** . W poniższym przykładzie użyto metody **DownloadToStreamAsync** , aby przenieść zawartość obiektów blob do obiektu strumienia, który można następnie zapisać jako plik lokalny.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Istnieją inne sposoby zapisywania obiektów blob jako pliki. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Usuwanie obiektów blob
Aby usunąć obiektów blob, najpierw odwołać się do obiektów blob, a następnie wywołać metodę **DeleteAsync** nad nim.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
