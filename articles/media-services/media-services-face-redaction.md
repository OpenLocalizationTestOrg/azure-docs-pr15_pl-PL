<properties
    pageTitle="Buźkę redakcyjny z multimediów Azure analizy | Microsoft Azure"
    description="W tym temacie przedstawiono sposób Zredaguj powierzchni z multimediów Azure analizy."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Redakcyjny nominalnej z analizy multimediów Azure

##<a name="overview"></a>Omówienie

**Redactor multimediów Azure** jest procesor multimediów [Azure analizy multimediów](media-services-analytics-overview.md) (CR) oferująca redakcyjny skalowalna nominalnej w chmurze. Redakcyjny nominalnej umożliwia modyfikowanie ustawień wideo w celu Rozmycie powierzchni wybrane osoby. Warto korzystać z usługi redakcyjny nominalnej w publicznej scenariusze bezpieczeństwa i multimediów wiadomości. Kilka minut materiału zawierającego wiele powierzchni może potrwać godzin Zredaguj ręcznie, ale z tej usługi proces redakcyjny nominalnej wymaga tylko kilku prostych krokach. Aby uzyskać więcej informacji zobacz [tym](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

W tym temacie przedstawiono szczegółowe informacje o **Redactor multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET.

**Redactor multimediów Azure** CR jest obecnie w podglądzie.

## <a name="face-redaction-modes"></a>Tryby redakcyjny buźka

Min redakcyjny polega na wykrywanie powierzchni w każdej ramce wideo i śledzenie wiadomości przesłanych dalej i Wstecz obiektu nominalnej zarówno w czasie, tak aby tę samą osobę można rozmyciu kątami innych także. Proces automatycznego redakcyjny jest bardzo złożony i czy nie zawsze produktu 100% żądany z tego powodu analizy multimedia udostępnia na kilka sposobów, aby zmodyfikować końcowych danych wyjściowych.

Oprócz w pełni automatycznym zawiera dwa przebieg przepływu pracy, dzięki czemu zaznaczenia-dezaktywuje-selection: powierzchni znalezionego przez listę identyfikatorów. Ponadto nawiązać dowolnego na ramkę dopasowania CR używa metadanych pliku w formacie JSON. Ten przepływ pracy jest podzielony na **Analizowanie** oraz **Redact** . Można połączyć dwa tryby w jednym przebiegu uruchamianej obu zadań w jedno zadanie. w tym trybie jest określana mianem **nomenklatury**.

###<a name="combined-mode"></a>Tryb Scalonej

Zapewni zredagowanym mp4 automatycznie bez dowolnego ręcznego wprowadzania.

Etap|Nazwa pliku|Notatki
---|---|---
Wprowadzania zawartości|foo.bar|Klip wideo w formacie WMV, MOV i MP4
Wprowadzania konfiguracji|Ustawienia konfiguracji zadań|{"wersji": "1.0',"Opcje": {"tryb":"łączone"}}
Dane wyjściowe zawartości|foo_redacted.mp4|Klip wideo z Rozmycie zastosowane

####<a name="input-example"></a>Przykład wprowadzania:

[Obejrzyj ten klip wideo](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Przykład danych wyjściowych:

[Obejrzyj ten klip wideo](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analizowanie tryb

Przebiegu **Analizowanie** dwóch przebieg przepływu pracy trwa wejście wideo i tworzy plik JSON lokalizacji nominalnej i obrazów jpg każdego wykryto powierzchni.

Etap|Nazwa pliku|Notatki
---|---|----
Wprowadzania zawartości|foo.bar|Klip wideo w formacie WMV i MPV, MP4
Wprowadzania konfiguracji|Ustawienia konfiguracji zadań|{"wersji": "1.0',"Opcje": {"tryb":"analizowanie"}}
Dane wyjściowe zawartości|foo_annotations.JSON|Dane adnotacji nominalnej lokalizacje w formacie JSON. To może być edytowany przez użytkownika w celu modyfikowania Rozmycie obwiednie. Zobacz przykładowy poniżej.
Dane wyjściowe zawartości|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Przycięte jpg każdego wykryte buźka, gdzie liczba wskazuje etykiety powierzchni

####<a name="output-example"></a>Przykład danych wyjściowych:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… obcięte


###<a name="redact-mode"></a>Zredaguj tryb

Drugi przebieg przepływ pracy ma większą liczbą danych wejściowych, które muszą być połączone w pojedynczego zasobu.

To zawiera listę identyfikatorów do Rozmycie, oryginalnego wideo i adnotacje JSON. W tym trybie używa adnotacje stosowanie Rozmycie na wideo wejściowe.

Dane wyjściowe przebiegu analiza nie zawiera oryginalnego wideo. Klip wideo musi być przekazane do wprowadzania zasobów dla zadań trybu Redact i zaznaczone jako plik podstawowy.

Etap|Nazwa pliku|Notatki
---|---|---
Wprowadzania zawartości|foo.bar|Klip wideo w formacie WMV, MPV lub MP4. Samego obrazu wideo, tak jak w kroku 1.
Wprowadzania zawartości|foo_annotations.JSON|Plik metadanych adnotacje z fazy, ze zmianami opcjonalne.
Wprowadzania zawartości|foo_IDList.txt (opcjonalnie)|Lista nominalnej identyfikatorów do Zredaguj rozdzielonych opcjonalne nowy wiersz. Jeśli puste, to pozwala rozmyć wszystkich powierzchni.
Wprowadzania konfiguracji|Ustawienia konfiguracji zadań|{"wersji": "1.0',"Opcje": {"tryb":"Zredaguj"}}
Dane wyjściowe zawartości|foo_redacted.mp4|Klip wideo z Rozmycie zastosowane według adnotacji

####<a name="example-output"></a>Przykładowe dane wyjściowe

Jest to wynikiem IDList z jednego Identyfikatora zaznaczone.

[Obejrzyj ten klip wideo](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Opisy atrybutów

CR redakcyjny zawiera dużej dokładności nominalnej lokalizacji wykrywanie i śledzenia, który może wykryć maksymalnie 64 powierzchni ludzi w ramce wideo. Powierzchni czołowej zapewniają uzyskać najlepsze wyniki, podczas boczne i małych powierzchni (mniejsze niż lub równe 24 x 24 piksele) są trudne.

Powierzchni Wykryto i śledzić są zwracane o współrzędnych wskazujący lokalizację strony, a także powierzchnię numeru Identyfikacyjnego wskazująca śledzenie tej osoby. Numery identyfikacyjne nominalnej są podatne na zresetować w przypadku, gdy czołowego nominalnej zostanie utracony lub nakłada w ramce, uzyskując niektóre osoby wprowadzenie przypisane wiele identyfikatorów.

Aby uzyskać szczegółowe informacje dla atrybutów zobacz temat [nominalnej wykrywanie i emocje z analizy multimediów Azure](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Utwórz zadanie z zadaniem redakcyjny nominalnej oparty na pliku konfiguracji, która zawiera następujące ustawienie json. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Pobieranie plików JSON wyjściowych. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
