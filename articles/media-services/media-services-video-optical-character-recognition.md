<properties
    pageTitle="Konwertowanie tekstu zawartości plików wideo cyfrowego tekstu za pomocą analizy multimediów Azure | Microsoft Azure"
    description="Azure multimediów analizy optycznego rozpoznawania znaków (OCR) umożliwia konwertowanie tekstu zawartości plików wideo można wyszukiwać, które można edytować tekst cyfrowy.  Dzięki temu będzie można zautomatyzować wyodrębnianie metadanych zrozumiałej z sygnału wideo multimediów."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Konwertowanie tekstu zawartości plików wideo cyfrowego tekstu za pomocą analizy multimediów Azure 

##<a name="overview"></a>Omówienie

Jeśli potrzebujesz wyodrębnić tekst z plików wideo oraz generowanie wyszukiwania, które można edytować tekst cyfrowy, należy używać Azure multimediów analizy optycznego rozpoznawania znaków (OCR). Ten procesor multimediów Azure wykryje zawartości tekstowej w przypadku pliki wideo i generuje pliki tekstowe przeznaczony do użytku. OCR można zautomatyzować wyodrębnianie metadanych zrozumiałej z sygnału wideo multimediów.

Gdy jest używana w połączeniu z aparatu wyszukiwania, możesz łatwo indeksu multimediów według tekstu i rozszerza możliwości odnajdowania zawartości. Jest to bardzo przydatne w wideo wysoce tekstowe, takie jak nagrania wideo lub przechwycony ekran prezentacji pokazu slajdów. Procesor multimediów Azure OCR jest zoptymalizowana pod kątem tekst cyfrowy.

Procesor multimediów **Azure multimediów OCR** jest obecnie w podglądzie.

W tym temacie przedstawiono szczegółowe informacje o **OCR multimediów Azure** i pokazano, jak z niego korzystać z zestawu SDK usługi multimediów dla środowiska .NET. Aby uzyskać dodatkowe informacje oraz przykłady zobacz [tym blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Pliki wejściowe OCR

Pliki wideo. Obecnie obsługiwane są następujące formaty: MP4, MOV i WMV.

##<a name="task-configuration"></a>Konfiguracja zadania 

Konfiguracja zadania (ustawienia domyślne). Podczas tworzenia zadania przy **Użyciu funkcji OCR Azure multimediów**, musisz określić konfiguracji wstępnie ustawione przy użyciu JSON lub XML. 

###<a name="attribute-descriptions"></a>Opisy atrybutów

Nazwa atrybutu|Opis
---|---
Język|(opcjonalnie) w tym artykule opisano języka tekstu do wyszukania. Jedną z następujących czynności: Autowykrywanie (domyślne), arabski, ChineseSimplified, ChineseTraditional, czeski, duński, holenderski, angielski, fiński, francuski, niemiecki, grecki, węgierski, włoski, japoński, koreański, norweski, Polski, portugalski, rumuński, rosyjski, SerbianCyrillic, SerbianLatin, słowackim, hiszpański, szwedzki, turecki.
TextOrientation|(opcjonalnie) w tym artykule opisano orientacji tekstu do wyszukania.  "W lewo" oznacza, że na początek wszystkie litery są skierowane po lewej stronie.  Domyślny tekst (na przykład tych, które można znaleźć w książce) może wymagać "" orientacji.  Jedną z następujących czynności: Autowykrywanie (domyślne), nawet, w prawo, Strzałka w dół, w lewo.
Parametru TimeInterval|(opcjonalnie) w tym artykule opisano stopa próbki.  Domyślna to co sekundę 1/2.<br/>Format JSON — hh: mm:. Usługa (domyślny 00:00:00.500)<br/>Format XML — pierwotny czas trwania W3C XSD (domyślnie PT0.5)
DetectRegions|(opcjonalnie) Tablica obiektów DetectRegion Określanie regionów w ramkę wideo, w którym ma zostać wykrywa tekstu.<br/>Obiekt DetectRegion składa się z następujących wartości całkowitych cztery:<br/>Lewej — pikseli od lewego marginesu<br/>Górny — piksele z margines górny<br/>Szerokość — szerokości obszaru w pikselach<br/>Wysokość — wysokość w pikselach obszaru

####<a name="json-preset-example"></a>Przykład wstępnie ustawionych JSON
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Przykład wstępnie ustawionych XML

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>Pliki wyjściowe OCR

Wynik procesora multimediów OCR jest plikiem JSON.

###<a name="elements-of-the-output-json-file"></a>Elementy w pliku docelowym JSON

Wynik OCR wideo udostępnia dane części czasu znaków znaleziony w klipie wideo.  Za pomocą atrybuty, takie jak język lub orientacji rozmów telefonicznych w dokładnie słowa wybrane w analizie. 

Wynik zawiera następujące atrybuty:

Element|Opis
---|---
Skala czasu|"znaczniki" na sekundę klipu wideo
Przesunięcie|czas przesunięcie dla sygnatury czasowe. W wersji 1.0 interfejsów API klip wideo to będzie zawsze równa 0.
FrameRate|Ramki na sekundę klipu wideo
Szerokość|Szerokość klipu wideo w pikselach
wysokość|wysokość w pikselach
Fragmenty|Tablica opartych na czasie fragmentów wideo, do którego jest pakietowego metadanych
Rozpoczynanie|Początek fragment w "znaczniki"
czas trwania|Długość fragment w "znaczniki"
Interwał|Interwał każde zdarzenie w ramach danej fragment
zdarzenia|Tablica zawierająca regionów
region|Obiekt reprezentujący wykryte wyrazów lub fraz
język|języka tekstu w regionie
Orientacja|Orientacja tekstu w regionie
linie|Tablica wierszy tekstu w regionie
tekst|tekst

###<a name="json-output-example"></a>Przykładowe dane wyjściowe JSON

W poniższym przykładzie wynik zawiera ogólne informacje wideo i kilka fragmentów wideo. W każdej wideo fragmentu zawiera każdego regionu, który zostanie wykryte przez CR OCR z językiem i orientacji tekstu. Obszar zawiera również każdy wiersz programu word, w tym regionie z wiersza tekstu, położenie linii i wszystkich informacji programu word (zawartości programu word, położenia i UFNOŚĆ) w tym wierszu. Poniżej przedstawiono przykładowy i został umieszczony w tekście niektóre komentarze.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Przykładowy kod

Pokazuje program następujące sposoby:

1. Tworzenie środka trwałego i przekaż plik multimedialny do elementu.
1. Tworzy zadanie z pliku konfiguracji i ustawienie OCR.
1. Pobiera dane wyjściowe pliki JSON. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
