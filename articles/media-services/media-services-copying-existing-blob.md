<properties 
    pageTitle="Kopiowanie istniejącego Blob do nośnika usług składników majątku | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób Skopiuj istniejący blob do usług multimedialnej." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/13/2016" 
    ms.author="juliako"/>

#<a name="copying-an-existing-blob-into-a-media-services-asset"></a>Kopiowanie istniejącego Blob do usług multimedialnej

W tym temacie przedstawiono sposób skopiuj obiektów blob z konta miejsca do magazynowania do nowego środka usługi multimediów Azure firmy Microsoft.

Do obiektów blob może znajdować się na konto miejsca do magazynowania, który jest skojarzony z konta usługi multimediów lub konta miejsca do magazynowania, który nie jest skojarzony z kontem usługi multimediów. W tym temacie przedstawiono sposób skopiować obiektów blob z konta miejsca do magazynowania do usług multimedialnej. Zauważ, że możesz również skopiować w centrach danych. Jednak czasami może występować koszty poniesione w ten sposób. Aby uzyskać więcej informacji o cenach zobacz [Przesyłanie danych](https://azure.microsoft.com/pricing/#header-11).

>[AZURE.NOTE] Nie należy próbować zmienić zawartość kontenerów obiektów blob, wygenerowane za pomocą usługi multimediów bez użycia interfejsów API usługi multimediów.

##<a name="download-sample"></a>Pobieranie przykładowych

Pobieranie i uruchamianie próbki [tutaj](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).

##<a name="prerequisites"></a>Wymagania wstępne

- Dwa konta usługi multimediów Azure subskrypcji nowej lub istniejącej. Zobacz temat [jak utworzyć konto usługi multimediów](media-services-portal-create-account.md).
- Systemy operacyjne: Systemu Windows 10, Windows 7, Windows 2008 R2 lub Windows 8.
- .NET framework 4,5.
- Visual Studio 2010 z dodatkiem SP1 (Professional, Premium, Ultimate lub Express) lub nowszym.

##<a name="set-up-your-project"></a>Konfigurowanie projektu

W tej sekcji możesz utworzyć i skonfigurować projekt aplikacji konsoli C#.

1. Visual Studio umożliwia utworzenie nowego rozwiązania zawierający projekt aplikacji konsoli C#. 
2. Wprowadź CopyExistingBlobsIntoAsset dla nazwy, a następnie kliknij przycisk OK.
1. Nuget umożliwia dodawanie odwołania do powiązanych DLL usługi multimediów. W Menu głównym Visual Studio, wybierz pozycję Narzędzia -> Menedżer pakietów biblioteki -> konsoli Menedżera pakietów. W oknie konsoli wpisz pakiet instalacyjny windowsazure.mediaservices i naciśnij klawisz enter.
1. Dodawanie innych odwołaniami, które są wymagane dla tego projektu: System.Configuration.
1. Zastępowanie przy użyciu instrukcji, które zostały dodane do pliku Programs.cs domyślnie z nich następujące czynności:
        
        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Collections.Generic;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure;
        using System.Web;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Auth;

1. Dodawanie sekcji appSettings do pliku .config i aktualizowanie wartości według klucza usługi multimediów i miejsca do magazynowania i nazwę wartości. 

        <appSettings>
          <add key="MediaServicesAccountName" value="Media-Services-Account-Name"/>
          <add key="MediaServicesAccountKey" value="Media-Services-Account-Key"/>
          <add key="MediaServicesStorageAccountName" value="Media-Services-Storage-Account-Name"/>
          <add key="MediaServicesStorageAccountKey" value="Media-Services-Storage-Account-Key"/>
          <add key="ExternalStorageAccountName" value="External-Storage-Account-Name"/>
          <add key="ExternalStorageAccountKey" value="External-Storage-Account-Key"/>
        </appSettings>


##<a name="copy-blobs-from-a-storage-account-into-a-media-services-asset"></a>Kopiowanie obiektów blob z konta miejsca do magazynowania w usługach multimedialnej

W poniższym przykładzie kodu wykonuje następujące zadania:

1. Tworzy wystąpienie CloudMediaContext. 
1. Tworzy wystąpienia CloudStorageAccount: _sourceStorageAccount i _destinationStorageAccount.
1. Wysyła wygładzonymi Streaming pliki z katalogu lokalnego kontener obiektów blob, który znajduje się w _sourceStorageAccount. 
1. Tworzy nowy trwały. Kontener obiektów blob, że jest tworzony ten element znajduje się w _destinationStorageAccount. 
1. Używa Azure SDK miejsca do magazynowania w celu skopiowania określonych obiektów blob w kontenerze skojarzonych z trwałym.

    >[AZURE.NOTE]Operacja kopiowania nie zostać zgłoszony wyjątek, jeśli wygasła wskaźniku.

1. Ponieważ w tym przykładzie, że Trwa kopiowanie wygładzonymi przesyłanie strumieniowe plików, w przykładzie jak ustawić plik .ism do pliku podstawowego. Jeśli na przykład skopiowane pliku MP4, pliku mp4 chcesz ustawić jako pliku podstawowego.
1. Tworzy wygładzonymi Streaming adresu URL dla locator OnDemandOrigin skojarzonych z trwałym. 
            
        class Program
        {
            // Read values from the App.config file. 
            static string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
            static string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            static string _storageAccountName = ConfigurationManager.AppSettings["MediaServicesStorageAccountName"];
            static string _storageAccountKey = ConfigurationManager.AppSettings["MediaServicesStorageAccountKey"];
            static string _externalStorageAccountName = ConfigurationManager.AppSettings["ExternalStorageAccountName"];
            static string _externalStorageAccountKey = ConfigurationManager.AppSettings["ExternalStorageAccountKey"];

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            private static CloudStorageAccount _sourceStorageAccount = null;
            private static CloudStorageAccount _destinationStorageAccount = null;

            static void Main(string[] args)
            {
            _cachedCredentials = new MediaServicesCredentials(
                    _accountName,
                    _accountKey);
            // Use the cached credentials to create CloudMediaContext.
            _context = new CloudMediaContext(_cachedCredentials);

            // In this example the storage account from which we copy blobs is not 
            // associated with the Media Services account into which we copy blobs.
            // But the same code will work for coping blobs from a storage account that is 
            // associated with the Media Services account.
            //
            // Get a reference to a storage account that is not associated with a Media Services account
            // (an external account).  
            StorageCredentials externalStorageCredentials =
                new StorageCredentials(_externalStorageAccountName, _externalStorageAccountKey);
            _sourceStorageAccount = new CloudStorageAccount(externalStorageCredentials, true);

            //Get a reference to the storage account that is associated with a Media Services account. 
            StorageCredentials mediaServicesStorageCredentials =
                new StorageCredentials(_storageAccountName, _storageAccountKey);
            _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

            // Upload Smooth Streaming files into a storage account.
            string localMediaDir = @"C:\supportFiles\streamingfiles";
            CloudBlobContainer blobContainer =
                UploadContentToStorageAccount(localMediaDir);

            // Create a new asset and copy the smooth streaming files into 
            // the container that is associated with the asset.
            IAsset asset = CreateAssetFromExistingBlobs(blobContainer);

            // Get the streaming URL for the smooth streaming files 
            // that were copied into the asset.   
            string urlForClientStreaming = CreateStreamingLocator(asset);
            Console.WriteLine("Smooth Streaming URL: " + urlForClientStreaming);

            Console.ReadLine();
            }

            /// <summary>
            /// Uploads content from a local directory into the specified storage account.
            /// In this example the storage account is not associated with the Media Services account.
            /// </summary>
            /// <param name="localPath">The path from which to upload the files.</param>
            /// <returns>The container that contains the uploaded files.</returns>
            static public CloudBlobContainer UploadContentToStorageAccount(string localPath)
            {
            CloudBlobClient externalCloudBlobClient = _sourceStorageAccount.CreateCloudBlobClient();

            CloudBlobContainer externalMediaBlobContainer = externalCloudBlobClient.GetContainerReference("streamingfiles");

            externalMediaBlobContainer.CreateIfNotExists();

            // Upload files to the blob container.  
            DirectoryInfo uploadDirectory = new DirectoryInfo(localPath);
            foreach (var file in uploadDirectory.EnumerateFiles())
            {
                CloudBlockBlob blob = externalMediaBlobContainer.GetBlockBlobReference(file.Name);

                blob.UploadFromFile(file.FullName, FileMode.Open);
            }

            return externalMediaBlobContainer;
            }

            /// <summary>
            /// Creates a new asset and copies blobs from the specifed storage account.
            /// </summary>
            /// <param name="mediaBlobContainer">The specified blob container.</param>
            /// <returns>The new asset.</returns>
            static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer mediaBlobContainer)
            {
            // Create a new asset. 
            IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);

            IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                TimeSpan.FromHours(24), AccessPermissions.Write);
            ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);

            CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

            // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
            string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];

            CloudBlobContainer assetContainer =
                destBlobStorage.GetContainerReference(destinationContainerName);

            if (assetContainer.CreateIfNotExists())
            {
                assetContainer.SetPermissions(new BlobContainerPermissions
                {
                PublicAccess = BlobContainerPublicAccessType.Blob
                });
            }

            var blobList = mediaBlobContainer.ListBlobs();
            foreach (var sourceBlob in blobList)
            {
                var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);
                CopyBlob(sourceBlob as ICloudBlob, assetContainer);
                assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                assetFile.Update();
            }

            asset.Update();

            destinationLocator.Delete();
            writePolicy.Delete();

            // Since we copied a set of Smooth Streaming files, 
            // set the .ism file to be the primary file. 
            // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
            SetISMFileAsPrimary(asset);

            return asset;
            }

            /// <summary>
            /// Creates the OnDemandOrigin locator in order to get the streaming URL.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            /// <returns>The streaming URL.</returns>
            static public string CreateStreamingLocator(IAsset asset)
            {
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).First();

            // Create a 30-day readonly access policy. 
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                TimeSpan.FromDays(30),
                AccessPermissions.Read);

            // Create a locator to the streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                policy,
                DateTime.UtcNow.AddMinutes(-5));

            return originLocator.Path + ismAssetFile.Name + "/manifest";
            }

            /// <summary>
            /// Copies the specified blob into the specified container.
            /// </summary>
            /// <param name="sourceBlob">The source container.</param>
            /// <param name="destinationContainer">The destination container.</param>
            static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
            {
            var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
            {
                Permissions = SharedAccessBlobPermissions.Read,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
            });

            ICloudBlob destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);

            if (destinationBlob.Exists())
            {
                Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
            }
            else
            {

                // Display the size of the source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                // The StartCopyFromBlob is an async operation, 
                // so we want to check if the copy operation is completed before proceeding. 
                // To do that, we call FetchAttributes on the blob and check the CopyStatus. 
                destinationBlob.FetchAttributes();
                if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                {
                    break;
                }
                //It's still not completed. So wait for some time.
                System.Threading.Thread.Sleep(1000);
                }


                // Display the size of the destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
            }

            /// <summary>
            /// Sets a file with the .ism extension as a primary file.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            static private void SetISMFileAsPrimary(IAsset asset)
            {

            //If you expect the asset to contain the .ism asset file, set the .ism file as the primary file.
            var ismAssetFiles = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

            // The following code assigns the first .ism file as the primary file in the asset.
            // An asset should have one .ism file.  
            ismAssetFiles.First().IsPrimary = true;
            ismAssetFiles.First().Update();
            }
        }

 

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
