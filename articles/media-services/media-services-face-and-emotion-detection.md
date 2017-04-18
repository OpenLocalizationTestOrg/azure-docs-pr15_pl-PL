<properties
    pageTitle="Wykrywanie nominalnej i emocje z multimediów Azure analizy | Microsoft Azure"
    description="W tym temacie przedstawiono sposób wykrywanie buziek i emocje z analizy multimediów Azure."
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
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Wykrywanie nominalnej i emocje z analizy multimediów Azure

##<a name="overview"></a>Omówienie

Procesor multimediów **Azure multimediów nominalnej wykrywanie** (CR) umożliwia zliczanie, śledzenie ruchów, a nawet ocenić uczestnictwo odbiorców i reakcji za pośrednictwem min. Ta usługa zawiera dwie funkcje: 

- **Wykrywanie buźka**

    Wykrywanie nominalnej umożliwia znalezienie wyrazów i śledzenie ludzi powierzchni w pliku wideo. Wielu powierzchni można wykryć, a następnie śledzenia ich poruszania się po, czas oraz położenie metadanymi zwrócone w pliku JSON. Podczas śledzenia spróbuje udzielić jednolity identyfikator do samej nominalnej, gdy osoby jest poruszanie się po aplikacji na ekranie, nawet jeśli są zablokowane lub krótko pozostaw ramki.

    >[AZURE.NOTE]Tej usługi nie jest sprawdzana rozpoznawania min. Osoba, która opuszcza ramki lub staje się blokować dla zbyt długo będzie mieć nowy identyfikator gdy zostaną zwrócone.

- **Wykrywanie emocje**
    
    Wykrywanie emocje jest opcjonalny składnik procesora multimediów wykrywania nominalnej, która zwraca analizy na wiele atrybutów emocjonalne z powierzchni wykryte, takich jak szczęście, sadness, dostępność i gniew. 

**Wykrywanie nominalnej multimediów Azure** CR jest obecnie w podglądzie.

W tym temacie przedstawiono szczegółowe informacje o **Wykrywanie nominalnej multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET.

##<a name="face-detector-input-files"></a>Wykrywanie plików wejściowych buźkę

Pliki wideo. Obecnie obsługiwane są następujące formaty: MP4, MOV i WMV.

##<a name="face-detector-output-files"></a>Pliki wyjściowe wykrywanie buźkę

Interfejs API wykrywanie i śledzenie nominalnej zapewnia dużej dokładności nominalnej lokalizacji wykrywanie i śledzenia, który może wykryć maksymalnie 64 powierzchni ludzi w klipie wideo. Powierzchni czołowej umożliwiają uzyskać najlepsze wyniki, podczas boczne i małych powierzchni (mniejsze niż lub równe 24 x 24 piksele) może nie być tak dokładne.

Wykryto i śledzić powierzchni są zwracane o współrzędnych (od lewej, górnej, szerokość i wysokość) wskazujący lokalizację powierzchni obrazu w pikselach, a także numer identyfikacyjny nominalnej, wskazująca śledzenie tej osoby. Numery identyfikacyjne nominalnej są podatne na resetowanie warunkach, gdy czołowego nominalnej zostanie utracony lub nakłada w ramce, uzyskując niektóre osoby wprowadzenie przypisane wiele identyfikatorów.

###<a id="output_elements"></a>Elementy w pliku docelowym JSON

Wykrywanie nominalnej i śledzenie operacji wynik dane wyjściowe zawiera metadane z powierzchni w ramach danego pliku w formacie JSON.

Wykrywanie nominalnej i śledzenie JSON zawiera następujące atrybuty:

Element|Opis
---|---
Wersja|Odnosi się do wersji interfejsu API wideo.
Skala czasu|"Znaczniki" na sekundę klipu wideo.
Przesunięcie|Jest to przesunięcie czasu dla sygnatury czasowe. W wersji 1.0 interfejsów API klip wideo to będzie zawsze równa 0. W przyszłości scenariuszy, które obsługujemy, ta wartość może się zmienić.
FrameRate|Ramki na sekundę klipu wideo.
Fragmenty|Metadane jest dzielone się na różnych segmentów o nazwie fragmenty. Każdy fragment zawiera rozpoczęcia, czas trwania, liczbę interwałów i zdarzenia.
Rozpoczynanie|Godzina rozpoczęcia pierwsze zdarzenie "znaczniki".
Czas trwania|Długość fragmentu w "znaczniki".
Interwał|Interwał każdej pozycji zdarzenia w fragment w "znaczniki".
Zdarzenia|Każde zdarzenie zawiera powierzchni wykryte i śledzony w tym czas trwania. Jest tablicy tablica zdarzeń. Zewnętrzne tablicy reprezentuje jeden przedział czasu. Wewnętrzne tablicy składa się z 0 lub kolejnych zdarzeń, które wystąpiły w danym momencie. Puste nawias kwadratowy [] oznacza, że zostaną wykryte nie buźki.
IDENTYFIKATOR| Identyfikator powierzchnię, która jest śledzony. Ten numer mogą przypadkowo zmieniony, jeśli powierzchnię staje się niewykryte. Danej osoby powinny mieć taką samą identyfikator całej ogólnego klip wideo, ale nie można to zagwarantować z powodu ograniczeń w algorytm wykrywania (zamknięcia itp.)
X, Y|Lewy górny róg X i Y współrzędne nominalnej pole w skali znormalizowaną 0.0 do 1.0 ograniczenia. <br/>-X i Y współrzędne są względne do poziomej zawsze, więc jeśli masz pionowej wideo (lub nogami w przypadku iOS), należy transponować współrzędne w związku z tym.
Szerokość, wysokość|Szerokość i wysokość nominalnej pole w skali znormalizowaną 0.0 do 1.0 ograniczenia.
facesDetected|Znajduje się na końcu wyników JSON i przedstawiono liczbę powierzchni algorytmu wykrytych podczas wideo. Ponieważ identyfikatory mogą resetować przypadkowo Jeśli powierzchnię staje się niewykryte (np. nominalnej odbywa się poza ekranem, wygląd z dala od komputera), liczba ta nie zawsze równa PRAWDA liczbę powierzchni w klipie wideo.

Wykrywanie nominalnej używa technik fragmentacji (gdzie metadane można podzielić na fragmenty opartych na czasie i można pobrać tylko potrzebnych) i segmentacji (gdzie zdarzenia są podzielony na wypadek, gdyby otrzymają jest za duży). Kilka prostych obliczeń może ułatwić przekształcania danych. Na przykład zdarzenie wprowadzenie u 6300 (znaczniki), skali czasu 2997 (znaczniki/s) i framerate 29,97 (ramek/s), następnie:

- Rozpoczęcie-skali czasu = 2.1 sekund
- Sekundy x (Framerate/Skala czasu) = 63 ramek

Poniżej przedstawiono prosty przykład JSON do wyodrębniania na format ramki wykrywania nominalnej i śledzenie:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Buźkę wykrywania dane wejściowe i wyjściowe przykład

###<a name="input-video"></a>Klip wideo wprowadzania danych

[Klip wideo wprowadzania danych](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfiguracja zadania (ustawienia domyślne)

Podczas tworzenia zadania z **Wykrywanie nominalnej multimediów Azure**, musisz określić ustawienia domyślne konfiguracji. Następujące ustawienie konfiguracji jest tylko do wykrywania nominalnej.

    {"version":"1.0"}

###<a name="json-output"></a>Wynik JSON

Poniższy przykład danych wyjściowych JSON została obcięta.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Wykrywanie emocje wejściowe i wyjściowe przykład


###<a name="input-video"></a>Klip wideo wprowadzania danych

[Klip wideo wprowadzania danych](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfiguracja zadania (ustawienia domyślne)

Podczas tworzenia zadania z **Wykrywanie nominalnej multimediów Azure**, musisz określić ustawienia domyślne konfiguracji. Następujące ustawienie konfiguracji określa, aby utworzyć JSON według wykrywania emocje.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Opisy atrybutów

Nazwa atrybutu|Opis
---|---
Tryb|Powierzchni: Tylko buźkę wykrywania <br/>AggregateEmotion: Zwrotu emocje średniej wartości dla wszystkich powierzchni w ramce.
AggregateEmotionWindowMs|Użyj, jeśli wybrany tryb AggregateEmotion. Określa długość wideo używane da wynik agregacji, (w milisekundach).
AggregateEmotionIntervalMs|Użyj, jeśli wybrany tryb AggregateEmotion. Określa, częstotliwość w celu uzyskania wyników agregacji.

####<a name="aggregate-defaults"></a>Wartości domyślnych agregacji

Poniżej wartości zagregowanych ustawień okna i interwału zaleca się. AggregateEmotionWindowMs powinna być większa niż AggregateEmotionIntervalMs.

   |Domyślne (s)|MAX(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>Wynik JSON

JSON wyniki dla agregacji emocje (obcinane):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Ograniczenia

- Obsługiwane formaty wideo wprowadzania zawierać MP4, MOV i WMV.
- Zakres rozmiaru wykrywalna nominalnej jest 24 x 24 do 2048 x 2048 pikseli. Powierzchni poza ten zakres nie zostanie wykryty.
- Dla każdego wideo maksymalna liczba powierzchni, zwracany jest 64.
- Niektóre powierzchni może nie być wykrywana z powodu problemów technicznych; przykład bardzo duże kątami nominalnej (Szef ułożenia) i zamknięcia dużych.. Powierzchni czołowej i u pełna mają najlepsze wyniki.


## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Tworzy zadanie z zadaniem wykrywania nominalnej oparty na pliku konfiguracji, która zawiera następujące ustawienie json. 
                    
        {
            "version": "1.0"
        }

1. Pobiera dane wyjściowe pliki JSON. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)