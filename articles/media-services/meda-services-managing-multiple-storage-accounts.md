<properties 
    pageTitle="Zarządzanie multimediami usług składników majątku między wieloma kontami miejsca do magazynowania | Microsoft Azure" 
    description="W tym artykule stanowić wskazówki dotyczące zarządzania usług multimedialnych w ramach wielu kont miejsca do magazynowania." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


#<a name="managing-media-services-assets-across-multiple-storage-accounts"></a>Zarządzanie nośnikami usług składników majątku między wieloma kontami miejsca do magazynowania

Począwszy od firmy Microsoft Azure Media Services 2.2, można dołączyć wiele kont miejsca do magazynowania dla jednego konta usługi multimediów. Możliwość dołączyć wiele kont miejsca do magazynowania na konto usługi multimediów zapewnia następujące korzyści:

- Załaduj równoważenia aktywów w ramach wielu kont miejsca do magazynowania.
- Multimedia skalowania usług dla dużej ilości procesu przetwarzania zawartości (jak obecnie konto jednego miejsca do magazynowania ma maksymalną dozwoloną liczbę 500 TB). 

W tym temacie przedstawiono sposób dołączyć wiele kont miejsca do magazynowania na konto usługi multimediów za pomocą interfejsu API usługi REST zarządzania usługi Azure. Pokazuje także określania magazynu innego konta, tworząc elementy zawartości przy użyciu zestawu SDK usługi multimediów. 

##<a name="considerations"></a>Zagadnienia dotyczące

Dołączanie wielu kont miejsca do magazynowania do swojego konta usługi multimediów, następujące kwestie:

- Wszystkie konta miejsca do magazynowania dołączone do konta usługi multimediów muszą być w tym samym centrum danych za pomocą konta usługi multimediów.
- Obecnie konto miejsca do magazynowania dołączony do określonego konta usługi multimediów go nie może być odłączony.
- Podstawowy konto jest wskazywana podczas tworzenia konta usługi multimediów. Obecnie nie można zmienić domyślnego konta miejsca do magazynowania. 

Inne zagadnienia:

Usługi multimediów używa wartości właściwości **IAssetFile.Name** podczas tworzenia adresy URL dla strumieniowego przesyłania zawartości (na przykład http://{WAMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowanie nie jest dozwolone. Wartość właściwości nazwy nie mogą zawierać następujące [wartości procentowej kodowanie zastrzeżone znaki](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Ponadto może istnieć tylko jeden "." dla rozszerzenia nazwy pliku.

##<a name="to-attach-a-storage-account-with-azure-service-management-rest-api"></a>Aby dołączyć konto miejsca do magazynowania z interfejsu API usługi REST zarządzania usługi Azure

Jedynym sposobem na dołączanie wielu kont miejsca do magazynowania jest obecnie, za pomocą [Interfejsu API usługi REST zarządzania usługi Azure](http://msdn.microsoft.com/library/azure/dn167014.aspx). Przykładowy kod w [jak: interfejsu API usługi REST zarządzania usługi multimediów Użyj](https://msdn.microsoft.com/library/azure/dn167656.aspx) tematu definiuje metodę **AttachStorageAccountToMediaServiceAccount** łączący konto miejsca do magazynowania z określonego konta usługi multimediów. Kod w tym samym tematem definiuje metodę **ListStorageAccountDetails** , która zawiera listę wszystkich kont miejsca do magazynowania dołączone do określonego konta usługi multimediów.


##<a name="to-manage-media-services-assets-across-multiple-storage-accounts"></a>Do zarządzania zasobami usługi multimediów w ramach wielu kont miejsca do magazynowania

Poniższy kod używa najnowszych SDK usługi multimediów do wykonywania następujących zadań:

1. Wyświetlanie wszystkich kont miejsca do magazynowania skojarzone z określonego konta usługi multimediów.
1. Pobieranie nazwę konta magazynu domyślnego.
1. Tworzenie nowego środka w domyślne konto miejsca do magazynowania.
1. Tworzenie środka trwałego dane wyjściowe kodowania zadania w oknie konta określonego miejsca do magazynowania.
    
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace MultipleStorageAccounts
        {
            class Program
            {
                // Location of the media file that you want to encode. 
                private static readonly string _singleInputFilePath =
                    Path.GetFullPath(@"../..\supportFiles\multifile\interview2.wmv");
        
                private static readonly string MediaServicesAccountName = 
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string MediaServicesAccountKey = 
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static CloudMediaContext _context;
                private static MediaServicesCredentials _cachedCredentials = null;
    
                static void Main(string[] args)
                {
    
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    MediaServicesAccountName,
                                    MediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
    
        
                    // Display the storage accounts associated with 
                    // the specified Media Services account:
                    foreach (var sa in _context.StorageAccounts)
                        Console.WriteLine(sa.Name);
        
                    // Retrieve the name of the default storage account.
                    var defaultStorageName = _context.StorageAccounts.Where(s => s.IsDefault == true).FirstOrDefault();
                    Console.WriteLine("Name: {0}", defaultStorageName.Name);
                    Console.WriteLine("IsDefault: {0}", defaultStorageName.IsDefault);
        
                    // Retrieve the name of a storage account that is not the default one.
                    var notDefaultStroageName = _context.StorageAccounts.Where(s => s.IsDefault == false).FirstOrDefault();
                    Console.WriteLine("Name: {0}", notDefaultStroageName.Name);
                    Console.WriteLine("IsDefault: {0}", notDefaultStroageName.IsDefault);
                    
                    // Create the original asset in the default storage account.
                    IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, 
                        defaultStorageName.Name, _singleInputFilePath);
                    Console.WriteLine("Created the asset in the {0} storage account", asset.StorageAccountName);
                    
                    // Create an output asset of the encoding job in the other storage account.
                    IAsset outputAsset = CreateEncodingJob(asset, notDefaultStroageName.Name, _singleInputFilePath);
                    if(outputAsset!=null)
                        Console.WriteLine("Created the output asset in the {0} storage account", outputAsset.StorageAccountName);
        
                }
        
                static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string storageName, string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    
                    // If you are creating an asset in the default storage account, you can omit the StorageName parameter.
                    var asset = _context.Assets.Create(assetName, storageName, assetCreationOptions);
        
                    var fileName = Path.GetFileName(singleFilePath);
        
                    var assetFile = asset.AssetFiles.Create(fileName);
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    return asset;
                }
        
                static IAsset CreateEncodingJob(IAsset asset, string storageName, string inputMediaFilePath)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My encoding job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset", storageName,
                        AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish. 
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();
        
                    // Get an updated job reference.
                    job = GetJob(job.Id);
        
                    // If job state is Error the event handling 
                    // method for job progress should log errors.  Here we check 
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        Console.WriteLine("\nExiting method due to job error.");
                        return null;
                    }
        
                    // Get a reference to the output asset from the job.
                    IAsset outputAsset = job.OutputMediaAssets[0];
        
                    return outputAsset;
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                private static void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("********************");
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine("Please wait while local tasks or downloads complete...");
                            Console.WriteLine("********************");
                            Console.WriteLine();
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            Console.WriteLine("An error occurred in {0}", job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
                static IJob GetJob(string jobId)
                {
                    // Use a Linq select query to get an updated 
                    // reference by Id. 
                    var jobInstance =
                        from j in _context.Jobs
                        where j.Id == jobId
                        select j;
                    // Return the job reference as an Ijob. 
                    IJob job = jobInstance.FirstOrDefault();
        
                    return job;
                }
            }
        }
 

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
