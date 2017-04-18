<properties
    pageTitle="Wykrywanie ruchów z multimediów Azure analizy | Microsoft Azure"
    description="Procesor multimediów Azure multimediów ruchu wykrywanie (CR) umożliwia efektywne identyfikują sekcje miejsc, w przeciwnym razie długie i procesu klip wideo."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Wykrywanie ruchów z analizy multimediów Azure

##<a name="overview"></a>Omówienie

Procesor multimediów **Azure multimediów ruchu wykrywanie** (CR) umożliwia efektywne identyfikują sekcje miejsc, w przeciwnym razie długie i procesu klip wideo. Wykrywanie ruchu może służyć na materiału statyczne kamery do identyfikowania sekcje wideo wykrycie ruchu. Generuje plik JSON zawierający metadane z sygnatury czasowe i dopasowania region, w którym wystąpiło zdarzenie.

Przeznaczona do zabezpieczenia źródła wideo, ta technologia będzie mógł przyporządkować ruchu do odpowiednich wydarzeń i wyniki fałszywie dodatnie, takie jak cienie i zmian oświetlenia. Umożliwia generowanie alertów zabezpieczeń na podstawie źródeł kamery bez otrzymywania wiadomości-śmieci nieograniczone zdarzenia nie ma znaczenia, podczas możliwość wyodrębniania chwil zainteresowania bardzo długo nadzoru klipy wideo.

**Wykrywanie ruchu multimediów Azure** CR jest obecnie w podglądzie.

W tym temacie przedstawiono szczegółowe informacje o **Wykrywanie ruchu multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET


##<a name="motion-detector-input-files"></a>Pliki wejściowe wykrywanie ruchu

Pliki wideo. Obecnie obsługiwane są następujące formaty: MP4, MOV i WMV.

##<a name="task-configuration-preset"></a>Konfiguracja zadania (ustawienia domyślne)

Podczas tworzenia zadania z **Wykrywanie ruchu multimediów Azure**, musisz określić ustawienia domyślne konfiguracji. 

###<a name="parameters"></a>Parametry

Można używać następujących parametrów:

Nazwa|Opcje|Opis|Domyślne
---|---|---|---
sensitivityLevel|Ciąg: "Niski", "Średni", "Wysoki"|Zestawy charakter, w których ruchów jest zgłoszone. Dostosuj tę opcję, aby dostosować ilość wyniki fałszywie dodatnie.|"Średni"
frameSamplingValue|Dodatniej liczby całkowitej|Określa częstotliwość, w której działa algorytmu. Każda ramka jest równa 1, 2 oznacza, że każdy ramki 2 i tak dalej.|1
detectLightChange|Warunek: "prawda", "false"|Określa, czy uproszczonej zmiany są zgłaszane w wynikach|"False"
mergeTimeThreshold|Czas znaków x: Hh: mm:<br/>Przykład: 00:00:03|Umożliwia określenie przedziału czasu między zdarzeniami ruchu miejsce, w którym łączenia i zgłoszone jako 1 2 zdarzenia.|00:00:00
detectionZones|Tablica strefy wykrywania:<br/>-Strefa wykrywania jest tablicą 3 lub więcej punktów<br/>-Punktowej jest x i y współrzędnych z przedziału od 0 do 1.|W tym artykule opisano wykaz stref wielokątne wykrywania może być używany.<br/>Wyniki jest informowany o strefach jako identyfikator z pierwszy z nich jest "identyfikator": 0|Pojedynczej strefy, która obejmuje całą ramkę.

###<a name="json-example"></a>Przykład JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Pliki wyjściowe wykrywanie ruchu

Zadanie wykrywania ruchu zwróci pliku JSON w aktywów wynik, który opisuje alertów ruchu i ich kategorie w pliku wideo. Plik będzie zawierać informacje o oraz czas trwania ruchu wykryte w klipie wideo.

Interfejs API wykrywanie ruchu zawiera wskaźniki po ruchu w stałej tła wideo (np. nadzoru, wideo) są obiekty. Wykrywanie ruchu przygotowaniu w celu zmniejszenia alarmy wartość FAŁSZ, takich jak oświetlenie i zmiany cienia. Bieżący ograniczenia dotyczące algorytmów zawiera klipy wideo wzroku nocy, półprzezroczysty obiektów i małych obiektów.

###<a id="output_elements"></a>Elementy w pliku docelowym JSON

>[AZURE.NOTE]W najnowszej wersji format wyjściowy JSON został zmieniony i mogą stanowić zmiany podziału w przypadku niektórych klientów.

W poniższej tabeli opisano elementy w pliku docelowym JSON.

Element|Opis
---|---
Wersja|Odnosi się do wersji interfejsu API wideo. Bieżąca wersja jest 2.
Skala czasu|"Znaczniki" na sekundę klipu wideo.
Przesunięcie|Przesunięcie czasu dla sygnatury czasowe w "znaczniki". W wersji 1.0 interfejsów API klip wideo to będzie zawsze równa 0. W przyszłości scenariuszy, które obsługujemy, ta wartość może się zmienić.
FrameRate|Ramki na sekundę klipu wideo.
Szerokość, wysokość|Odwołuje się do szerokość i wysokość w pikselach.
Rozpoczynanie|Sygnatura czasowa rozpoczęcia w "znaczniki".
Czas trwania|Długość zdarzenie, na "znaczniki".
Interwał|Interwał każdej pozycji, w przypadku w "znaczniki".
Zdarzenia|Każdy fragment zdarzenia zawiera ruchu w tym czas trwania.
Typ|W bieżącej wersji jest zawsze "2" dla ogólnego ruchu. Dzięki temu etykiety wideo API elastyczność kategoryzowania ruchu w przyszłych wersjach.
RegionID|Jak wyjaśniono powyżej, to będzie zawsze równa 0 w tej wersji. Etykieta zapewnia interfejs API wideo elastyczność znajdowanie ruchu w różnych regionów w przyszłych wersjach.
Obszary|Odwołuje się do obszaru w klipie wideo, gdzie interesuje ruchu. <br/><br/>-"identyfikator" reprezentuje obszar region — w tej wersji jest tylko jeden identyfikator 0. <br/>-"typ" reprezentuje kształt obszaru, który Cię interesuje dla ruchu. Obecnie obsługiwane są "prostokąt" i "Wielokąt".<br/> Jeśli określono "prostokąt" obszaru ma wymiary w X, Y, szerokość i wysokość. Współrzędne X i Y reprezentują współrzędnych XY lewy górny obszaru w skali znormalizowaną 0.0 do 1.0. Szerokość i wysokość reprezentują rozmiar obszaru w skali znormalizowaną 0.0 do 1.0. W bieżącej wersji X, Y, szerokość i wysokość jest zawsze ustalone na 0, 0 i 1, 1. <br/>Jeśli określono "Wielokąt" obszaru ma wymiary w punktach. <br/>
Fragmenty|Metadane jest dzielone się na różnych segmentów o nazwie fragmenty. Każdy fragment zawiera rozpoczęcia, czas trwania, liczbę interwałów i zdarzenia. Fragment z żadne zdarzenia oznacza, że nie ruchu wykryto podczas tej czas rozpoczęcia i czas trwania.
Nawiasy kwadratowe]|Każdy nawias reprezentuje jeden interwał w zdarzeniu. Opróżnij nawiasów do tego interwału oznacza, że nie ruchu wykryto.
lokalizacje|Ten nowy wpis w obszarze zdarzenia zawiera listę lokalizacji, w którym wystąpił ruchu. To jest bardziej szczegółowy niż strefy wykrywania.

Oto przykładowe dane wyjściowe JSON

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Ograniczenia

- Obsługiwane formaty wideo wprowadzania zawierać MP4, MOV i WMV.
- Wykrywanie ruchu jest zoptymalizowana pod kątem stacjonarnych tła wideo. Algorytm omówiono zmniejszania alarmy wartość FAŁSZ, takie jak zmiany oświetlenie i cienie.
- Niektóre ruchu może nie być wykrywana z powodu problemów technicznych; przykład nocy wzroku wideo, półprzezroczysty obiektów i małych obiektów.


## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Tworzy zadanie z zadaniem wykrywania ruchu wideo na podstawie pliku konfiguracji, która zawiera następujące ustawienie json. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Pobiera dane wyjściowe pliki JSON. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Blog wykrywanie ruchu usługi multimediów Azure](https://azure.microsoft.com/blog/motion-detector-update/)

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
