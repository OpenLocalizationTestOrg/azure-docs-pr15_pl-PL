<properties 
    pageTitle="Przekazywanie plików do konta usługi multimediów za pomocą .NET | Microsoft Azure" 
    description="Dowiedz się, jak pobrać zawartość multimedialną do usługi multimediów przez tworzenie i przekazywanie aktywów." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Przekazywanie plików do konta usługi multimediów za pomocą .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [POZOSTAŁE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Usługi multimediów możesz przekazać (lub mogły zjeść tej ostatniej) cyfrowych plików do środka trwałego. Jednostki **zasobów** może zawierać klip wideo, audio, obrazy, miniatur zbiorów, tekst ścieżek i napisy plików (i metadanych dotyczących tych plików.)  Po przesłaniu są pliki zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego.

Pliki w tym składniku aktywów są nazywane **Plikami zasobów**. Wystąpienie **AssetFile** i plik multimedialny rzeczywiste są dwa różne obiekty. Wystąpienia AssetFile zawiera metadanych dotyczących plików multimedialnych, gdy plik multimedialny z zawartością rzeczywiste.

>[AZURE.NOTE]Następujące kwestie podczas wybierania nazwy pliku zawartości:
>
>- Usługi multimediów używa wartości właściwości IAssetFile.Name podczas tworzenia adresy URL dla strumieniowego przesyłania zawartości (na przykład http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowanie nie jest dozwolone. Wartość właściwości **nazwy** nie mogą zawierać następujące [wartości procentowej kodowanie zastrzeżone znaki](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Ponadto może istnieć tylko jeden ".' dla rozszerzenia nazwy pliku.
>
>- Długość nazwy nie powinna być większa niż 260 znaków.

Po utworzeniu elementy zawartości można określić następujące opcje szyfrowania. 

- **Brak** — bez szyfrowania jest używany. Jest to wartość domyślna. Należy zauważyć, że podczas korzystania z tej opcji zawartości nie jest chroniony podczas przesyłania lub spoczywa w magazynie.
Jeśli planujesz do przeprowadzania MP4 za pomocą stopniowego pobierania, użyj tej opcji. 
- **CommonEncryption** - Użyj tej opcji, jeśli przekazujesz zawartość, która już szyfrowane i chronione za pomocą szyfrowania typowych lub DRM PlayReady (na przykład wygładzonymi przesyłanie strumieniowe chroniony z PlayReady DRM).
- **EnvelopeEncrypted** — Użyj tej opcji, jeśli wysyłasz HLS zaszyfrowanych za pomocą AES. Należy zauważyć, że pliki musi zostały zakodowany i szyfrowane przez Menedżera Przekształcanie.
- **StorageEncrypted** — są szyfrowane Wyczyść zawartość lokalnie, spoczynku przy użyciu szyfrowania AES-256 bitów, a następnie przekazanie go do magazynu Azure, w której jest przechowywany szyfrowane. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć wysokiej jakości plików multimedialnych wprowadzania danych przy użyciu silnego szyfrowania spoczynku na dysku.

    Usługi multimediów zapewnia szyfrowanie na ilość miejsca do magazynowania dla aktywów, nie przez przewodowy, takich jak Menedżer prawami cyfrowymi (DRM).

    Jeśli do zawartości jest szyfrowane miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Aby uzyskać więcej informacji, zobacz [zasady dostarczania zawartości Konfigurowanie](media-services-dotnet-configure-asset-delivery-policy.md).

Jeśli użytkownik określi dla swojego trwałego były szyfrowane za pomocą opcji **CommonEncrypted** lub opcję **EnvelopeEncypted** , należy skojarzyć z zawartości **ContentKey**. Aby uzyskać więcej informacji zobacz [Tworzenie ContentKey](media-services-dotnet-create-contentkey.md). 

Jeśli użytkownik określi dla swojego trwałego były szyfrowane za pomocą opcji **StorageEncrypted** , Media SDK usług dla środowiska .NET utworzy **StorateEncrypted** **ContentKey** dla swojej zawartości.


W tym temacie przedstawiono sposób przekazywania plików do zawartości usługi multimediów za pomocą SDK .NET usługi multimediów, a także rozszerzenia SDK .NET usługi multimediów.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Przekazywanie pliku jednego z SDK .NET usługi multimediów 

Poniższy przykładowy kod używa .NET SDK umożliwia wykonywanie następujących zadań: 

- Tworzy puste zawartości.
- Tworzy wystąpienie AssetFile interesujący skojarzyć z elementu.
- Tworzy wystąpienie AccessPolicy definiujące uprawnienia i czas trwania dostępu do elementu.
- Tworzy wystąpienie Locator, która zapewnia dostęp do elementu.
- Przekazywanie pliku multimedialnego pojedynczego do usługi multimediów. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Przekazywanie wielu plików z zestawu SDK .NET usługi multimediów 

Poniższy kod przedstawia sposób tworzenia zawartości i przekazywanie wielu plików.

Kod wykonuje następujące czynności:
    
-   Tworzy puste aktywów przy użyciu metody CreateEmptyAsset zdefiniowane w poprzednim kroku.
    
-   Tworzy wystąpienie **AccessPolicy** definiujące uprawnienia i czas trwania dostępu do elementu.
    
-   Tworzy wystąpienie **Locator** , która zapewnia dostęp do elementu.
    
-   Tworzy wystąpienie **BlobTransferClient** . Ten typ reprezentuje klientem działająca na Azure obiektów blob. W tym przykładzie używamy klienta monitorowanie postępu przekazywania. 
    
-   Wylicza pliki w określonym katalogu i tworzy wystąpienie **AssetFile** dla każdego pliku.
    
-   Przekazywanie plików do usługi multimediów przy użyciu metody **UploadAsync** . 
    
>[AZURE.NOTE] Użyj metody UploadAsync, aby upewnić się, że nie blokują połączeń i pliki są przekazywane równolegle.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Podczas przekazywania dużej liczby elementów, należy rozważyć następujące kwestie.

- Utwórz nowy obiekt **CloudMediaContext** na wątku. Klasa **CloudMediaContext** nie jest wielowątkowość.
 
- Zwiększanie NumberOfConcurrentTransfers z wartością domyślną 2 na wartość większą, takich jak 5. Ustawienie tej właściwości ma wpływ na wszystkie wystąpienia **CloudMediaContext**. 
 
- Zachowaj ParallelTransferThreadCount wartość domyślną z 10.
 
##<a id="ingest_in_bulk"></a>Ingesting środkami zbiorczych za pomocą SDK .NET usługi multimediów 

Przekazywanie zawartości dużych plików mogą być gardło podczas tworzenia zawartości. Ingesting środkami zbiorcze lub "Zbiorcze Ingesting", obejmuje rozdzielenie tworzenia zawartości za pomocą procesu. Aby użyć zbiorcze ingesting podejście, Utwórz opis elementu i skojarzone z nim pliki manifest (IngestManifest). Następnie użyj metody przesyłania wybranych przez użytkownika, aby przekazać skojarzonych plików do kontenera obiektów blob manifestu. Usługi multimediów Azure Microsoft oczekuje kontenera obiektów blob skojarzone z manifestu. Po przekazaniu pliku do kontenera obiektów blob usługi multimediów Azure Microsoft wykonuje tworzenia zawartości na podstawie konfiguracji środka trwałego w manifeście (IngestManifestAsset).


Aby utworzyć nowe połączenie IngestManifest metody Create ujawnionego przez kolekcji IngestManifests na CloudMediaContext. Ta metoda spowoduje utworzenie nowego IngestManifest o nazwie manifestu podane.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Tworzenie trwałych, które będą skojarzone z zbiorcze IngestManifest. Konfigurowanie opcji szyfrowania odpowiedniej trwałego dla ingesting zbiorczo.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset przypisuje środka trwałego zbiorcze IngestManifest dla ingesting zbiorczo. Kojarzy AssetFiles, tworzących poszczególnych elementów zawartości. Aby utworzyć IngestManifestAsset, użyj metody tworzenie kontekstu serwera.

Poniższy przykład pokazuje, dodawanie dwie nowe IngestManifestAssets, które skojarzyć dwa składniki majątku utworzone wcześniej do zbiorcze manifest mogły zjeść tej ostatniej. Każdy IngestManifestAsset również powiązanie zestawu plików, które zostaną przekazane dla każdego środka trwałego podczas ingesting zbiorczo.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Możesz użyć dowolnej aplikacji klienckiej szybkich stanie przekazywania plików zasobów w kontenerze magazyn obiektów blob dostarczony przez właściwość **IIngestManifest.BlobStorageUriForUpload** IngestManifest identyfikator URI. Jedna godne uwagi Wysoka szybkość przekazywania usługa jest [Aspera na żądanie stosowania Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Można także napisać kod, aby przekazać pliki składników majątku, jak pokazano w poniższym przykładzie.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Kod przekazywania plików trwałego dla próbki używane w tym temacie przedstawiono w poniższym przykładzie.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Można określić postępu ingesting zbiorczo dla wszystkich trwałych skojarzonych z **IngestManifest** przez sondowanie właściwość statystyki **IngestManifest**. Aby zaktualizować informacje o postępie, należy użyć nowego **CloudMediaContext** za każdym razem ankieta właściwość statystyki.

Poniższy przykład ilustruje sondowanie IngestManifest za pomocą jego **identyfikator**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Przekazywanie plików przy użyciu rozszerzeń SDK .NET 

W poniższym przykładzie pokazano, jak przekazywać pojedynczy plik, używając rozszerzenia SDK .NET. W tym przypadku metodę **CreateFromFile** , ale asynchroniczne wersja jest również dostępne (**CreateFromFileAsync**). Metoda **CreateFromFile** pozwala określić nazwę pliku, opcję szyfrowania i zwrotnego, aby można było raportować postęp przekazywania pliku.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

W poniższym przykładzie funkcja UploadFile i umożliwia określenie miejsca do magazynowania szyfrowania jako opcję tworzenia zawartości.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Następny krok

Teraz, gdy środka trwałego zostały przekazane do usługi multimediów, przejdź do tematu [jak uzyskać procesor multimediów][] .

[Jak uzyskać procesor multimediów]: media-services-get-media-processor.md
 
