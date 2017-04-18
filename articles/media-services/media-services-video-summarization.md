<properties
    pageTitle="Tworzenie podsumowania wideo za pomocą miniatury wideo multimediów Azure | Microsoft Azure"
    description="Wideo podsumowania umożliwia tworzenie podsumowania długie klipów wideo automatycznie wybierając interesujące wstawki z źródła wideo. Jest to przydatne, gdy chcesz podać krótkie omówienie czego można oczekiwać w długich wideo."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Tworzenie podsumowania wideo za pomocą miniatury wideo multimediów Azure
##<a name="overview"></a>Omówienie

Procesor multimediów **Azure multimediów wideo miniatury** (CR) umożliwia tworzenie podsumowanie klip wideo, który jest przydatna dla klientów, którzy po prostu chcesz wyświetlić podgląd podsumowanie długi klip wideo. Na przykład klienci mogą być wyświetlane krótkiej "Podsumowanie wideo" po ich umieść wskaźnik myszy na miniaturze. Przy zmienianiu parametry **miniatury Azure Media Video** przez ustawienie konfiguracji, umożliwia CR zaawansowanych zrzut wykrywanie i łączenia technologii algorithmically Generowanie subclip opisową.  

**Miniatura wideo multimediów Azure** CR jest obecnie w podglądzie.

W tym temacie przedstawiono szczegółowe informacje o **Miniatura wideo multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET

##<a name="video-summary-example"></a>Przykład podsumowanie wideo 

Oto kilka przykładów procesor multimediów Azure Media Video miniatury, jakie możliwości:

###<a name="original-video"></a>Oryginalnego wideo

[Oryginalnego wideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Wynik miniatury wideo

[Wynik miniatury wideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Konfiguracja zadania (ustawienia domyślne)

Podczas tworzenia wideo zadania miniatury z **Wideo miniaturami multimediów Azure**, musisz określić ustawienia domyślne konfiguracji. Powyżej przykład miniatur został utworzony przy użyciu następującej konfiguracji JSON podstawowe:

    {"version":"1.0"}

Obecnie możesz zmienić następujące parametry:

Parametr|Opis
---|---
outputAudio|Określa, czy Wynikowy klip wideo zawiera dźwięku. <br/>Dozwolone wartości są: wartość PRAWDA lub FAŁSZ. Domyślne ma wartość PRAWDA.
fadeInFadeOut|Umożliwia określenie, czy przejścia zanikanie są używane między miniatury osobnych ruchu.  <br/>Dozwolone wartości są: wartość PRAWDA lub FAŁSZ.  Domyślne ma wartość PRAWDA.
maxMotionThumbnailDurationInSecs|Liczba całkowita określająca, jak długo będzie całe wynikowy wideo.  Domyślne zależy od oryginalnego wideo czas trwania.


W poniższej tabeli opisano domyślny czas trwania, gdy nie jest używany **maxMotionThumbnailInSecs** .

||||
---|---|---|---|---
Czas trwania wideo|d < 3 min|3 min < d < 15 min|15 min < d < 30 minut| 30 minut < d
Czas trwania miniatur|15 s (2-3 scen)| 30 sekund (scen 3-5)|60 s (scen 5-10)|s 90 (scen 10-15)


Następujące JSON ustawia dostępne parametry.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Tworzy zadanie z zadaniem miniatur wideo na podstawie pliku konfiguracji, która zawiera następujące ustawienie json. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Pobieranie plików wyjściowych. 

###<a name="net-code"></a>Kod .NET
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            static void Main(string[] args)
            {
    
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Wyjście miniatury wideo

[Wyjście miniatury wideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)