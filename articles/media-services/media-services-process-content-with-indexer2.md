<properties
    pageTitle="Indeksowanie plików multimedialnych z podglądem indeksatora 2 multimediów Azure | Microsoft Azure"
    description="Azure indeksatora multimediów można nawiązać można wyszukiwać zawartość plików multimedialnych, a także do generowania zapis pełnotekstowym dla podpisy kodowane oraz słów kluczowych. W tym temacie przedstawiono sposób Podgląd 2 indeksatora multimediów."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indeksowanie plików multimedialnych z podglądem indeksatora 2 multimediów Azure

##<a name="overview"></a>Omówienie

Procesor multimediów **Azure multimediów indeksatora 2 Preview** (CR) umożliwia udostępnianie plików multimedialnych i treści można wyszukiwać, a także do generowania ścieżki podpisów kodowanych. W porównaniu z poprzednią wersję [Indeksatora multimediów Azure](media-services-index-content.md)system **Podgląd 2 indeksatora multimediów Azure** wykonuje szybsze indeksowania i oferuje pełniejsza obsługę języka. Obsługiwane języki zawierać angielski, hiszpański, francuski, niemiecki, włoski, chiński, portugalski i arabski.

**Podgląd 2 indeksatora multimediów Azure** CR jest obecnie w podglądzie.

W tym temacie przedstawiono sposób tworzenia indeksowania z **Podglądem 2 indeksatora multimediów Azure**.

>[AZURE.NOTE]Następujące kwestie:
>
>Indeksowanie 2 nie jest obsługiwane w Chinach Azure i dla instytucji rządowych Azure.
>
>Podgląd jest ograniczone do ~ 10 minut przetwarzania, ale są bezpłatne dla wszystkich klientów.
>
>Podczas indeksowania zawartości, upewnij się użyć pliki multimedialne, które mają bardzo jasne mowy (bez muzyki w tle, hałasu, efekty i mikrofon szumów). Oto niektóre przykłady odpowiedniej zawartości: zarejestrowanego spotkań, wykładów lub prezentacji. Następująca zawartość może nie być odpowiedni dla indeksowania: filmy i programy telewizyjne, cokolwiek z mieszanych audio i efekty dźwiękowe źle zarejestrowanego zawartości za pomocą szum w tle (szumów).


W tym temacie przedstawiono szczegółowe informacje o **Podgląd 2 indeksatora multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET

##<a name="input-and-output-files"></a>Wejściowe i wyjściowe plików

###<a name="input-files"></a>Pliki wejściowe

Pliki audio lub wideo

###<a name="output-files"></a>Pliki wyjściowe

Indeksowania zadania można wygenerować podpisów kodowanych pliki w następujących formatach:  

- **Sami**
- **TTML**
- **WebVTT**

Zamknięty podpis (DW) pliki w następujących formatach można udostępnić plików audio i wideo dla osoby niepełnosprawne słuchu.

##<a name="task-configuration-preset"></a>Konfiguracja zadania (ustawienia domyślne)

Podczas tworzenia indeksowania zadań z **Podglądem 2 indeksatora multimediów Azure**, musisz określić ustawienia domyślne konfiguracji.

Następujące JSON ustawia dostępne parametry.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Obsługiwane języki  

Azure multimediów indeksatora 2 Preview obsługuje mowy na tekst dla następujących języków (podczas określania nazwy języka w konfiguracji zadanie, użyj 4-znakowy kod w nawiasach, jak pokazano poniżej):

- Angielski [EnUs]
- Hiszpański [EsEs]
- Chiński [ZhCn]
- Francuski [FrFr]
- Niemiecki [DeDe]
- Włoski [ItIt]
- Portugalski [PtBr]
- Arabski (egipska) [ArEg]


## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Tworzy zadanie z zadaniem indeksowania na podstawie pliku konfiguracji, która zawiera następujące ustawienie json.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Pobieranie plików wyjściowych. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)