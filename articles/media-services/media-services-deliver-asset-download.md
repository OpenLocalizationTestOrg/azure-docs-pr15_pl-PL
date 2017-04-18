<properties 
    pageTitle="Pobierz multimedialnych" 
    description="Dowiedz się o mnie pobrać składniki majątku z komputerem. Przykłady kodu są zapisane w C# i używanie SDK usługi multimediów dla środowiska .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Jak: przedstawianie środka trwałego za plik do pobrania

W tym temacie opisano opcje dostarczania multimedialnych przekazane do usługi multimediów. Usługi multimediów zawartości zapewnia w wielu scenariuszy aplikacji. Możesz pobrać multimedialnych lub uzyskiwać do nich dostęp za pomocą locator. Możesz wysłać zawartości multimedialnej do innej aplikacji lub do innego dostawcy zawartości. Zwiększona wydajność i skalowalność może również udostępniać zawartość przy użyciu sieci dostarczania zawartości (CDN).

W tym przykładzie pokazano sposób pobrać multimedialnych usługi multimediów na komputer lokalny. Kod kwerendy zadania skojarzone z kontem usługi multimediów przez identyfikator zadania i uzyskuje dostęp do pobrania **OutputMediaAssets** (jest to zestaw co najmniej jeden wynik multimedialnych będącej wynikiem wykonywania zadania). W tym przykładzie pokazano, jak pobrać dane wyjściowe multimedialnych przy użyciu zadania, ale możesz zastosować to samo podejście do pobrania innych aktywów.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Zobacz też 

[Przedstawianie zawartości strumieniowych](media-services-deliver-streaming-content.md)

