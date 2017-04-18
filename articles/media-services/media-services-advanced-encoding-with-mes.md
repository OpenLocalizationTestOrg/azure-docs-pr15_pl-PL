<properties 
    pageTitle="Zaawansowane kodowanie z Media Encoder standardowy" 
    description="W tym temacie przedstawiono sposób wykonywania zaawansowanych kodowanie, dostosowując Media Encoder standardowe ustawienia zadania. Temat pokazano, jak używać multimediów usług .NET SDK do tworzenia kodowania zadań i zadania. Pokazuje także sposobu dostarczania niestandardowe ustawienia wstępne do kodowania zadania." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Zaawansowane kodowanie z Media Encoder standardowy

##<a name="overview"></a>Omówienie

W tym temacie przedstawiono sposób wykonywania zaawansowanych zadań kodowania z Media Encoder standardowy. Temat pokazano, [jak używać .NET do tworzenia kodowania zadania i zadania, aby wykonać to zadanie](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Pokazuje także sposobu dostarczania niestandardowe ustawienia wstępne do kodowania zadania. Aby zapoznać się z opisem elementów, które są używane przez ustawienia wstępne zobacz [Ten dokument](https://msdn.microsoft.com/library/mt269962.aspx). 

Niestandardowe ustawienia wstępne, które wykonywać następujące zadania kodowania przedstawiono:

- [Generowanie miniatur](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Przycinanie klipu wideo (wycinek)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Tworzenie nakładki](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Wstawianie odbiorcze ścieżki dźwiękowej po wprowadzania bez dźwięku](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Wyłączanie automatycznego usuwania przeplot](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Ustawienia wstępne tylko audio](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [ZŁĄCZ.teksty dwóch lub więcej plików wideo](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Przycinanie klipów wideo z Media Encoder standardowy](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Wstawianie ścieżki wideo podczas wprowadzania nie ma karty wideo](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Obracanie klipu wideo](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Kodowanie z usługi multimediów .NET SDK

W poniższym przykładzie użyto SDK .NET usługi multimediów umożliwia wykonywanie następujących zadań:

- Tworzenie kodowania zadania.
- Odwołać się do kodera Media Encoder standardowy.
- Ładowanie niestandardowy kod XML lub wstępnie ustawione JSON. Możesz zapisać XML lub JSON (na przykład [XML](media-services-custom-mes-presets-with-dotnet.md#xml) lub [JSON](media-services-custom-mes-presets-with-dotnet.md#json) w pliku i użyj poniższy kod załadować pliku.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Dodaj zadanie kodowania do zadania. 
- Określ wprowadzania środków trwałych ma być zakodowany.
- Tworzenie środka trwałego dane wyjściowe, zawierający zakodowany zawartości.
- Dodawanie zdarzeń, aby sprawdzić postęp zadania.
- Przesyłanie zadania.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Obsługa rozmiarów względne

Podczas generowania miniatury go, nie musisz zawsze określić dane wyjściowe szerokość i wysokość w pikselach. Można je określić w procentach w zakresie [1%,..., 100%].

###<a name="json-preset"></a>Ustawienie wstępne JSON 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>Ustawienie wstępne XML
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Generowanie miniatur

W tej sekcji pokazano, jak dostosować ustawienia domyślne generowany przez miniatury. Ustawienie określone poniżej zawiera informacje dotyczące sposobu kodowania do pliku, a także informacje niezbędne do utworzenia miniatury. Możesz wykonać dowolną z MES ustawień wstępnych udokumentowanych [tutaj](https://msdn.microsoft.com/library/mt269960.aspx) i Dodaj kod, który umożliwia generowanie miniatury.  

>[AZURE.NOTE]Ustawienie **SceneChangeDetection** w tylko następujące wstępnie ustawionych może być wartość PRAWDA, jeśli są kodowania do pojedynczej szybkość transmisji bitów wideo. Jeśli kodowania do klipu wideo szybkość transmisji bitów wielokrotne i ustawiona na PRAWDA **SceneChangeDetection** koder zwraca błąd.  


Aby uzyskać informacje o schemacie zobacz [w tym](https://msdn.microsoft.com/library/mt269962.aspx) temacie.

Upewnij się, zapoznaj się z sekcją [zagadnienia](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>Ustawienie wstępne JSON


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>Ustawienie wstępne XML


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Zagadnienia dotyczące

Następujące kwestie:

- Wykorzystywanie jawne sygnatury czasowe Start-krok/zakres zakłada, że źródła danych wejściowych jest co najmniej 1 minutę.
- Elementy jpg i Png-BmpImage uruchamianie, kroku i zakresu atrybuty ciągu — mogą być interpretowane jako:

    - Ramka numer, jeśli są one nieujemna liczby całkowite, na przykład "Uruchom": "120"
    - Względem źródło czasu trwania, jeśli wyrażone jako umieszczona %, na przykład "Uruchom": "15%"; lub
    - Sygnatura czasowa, jeśli wyrażoną w postaci hh: mm:... formatowanie, na przykład "Uruchom": "00: 01:00"

    Można mieszać i sprawdź dopasowywać notacji podczas.
    
    Ponadto Start obsługuje specjalnie makra: {zalecane}, która próbuje określić pierwszy ramki "interesujące" zawartości notatki: (krok i zakresu są ignorowane, gdy Start jest ustawiona na {najważniejsze})
    
    - Domyślne: Rozpoczynanie: {najlepsze}
- Format docelowy musi jawnie udostępnione dla każdego format obrazu: Jpg i Png-BmpFormat. Jeśli są dostępne, MES zastępuje JpgVideo do JpgFormat i tak dalej. OutputFormat wprowadza nowe makro określonych koder-dekoder obraz: {Index}, które musi zostać prezentowanie (raz i tylko raz) dla formatów wyjściowych obrazu.

##<a id="trim_video"></a>Przycinanie klipu wideo (wycinek)

Ta sekcja zawiera informacje o modyfikowanie ustawień wstępnych encoder klipu lub Przycinanie wideo wprowadzania, gdzie dane wejściowe są tak kodery pliku lub plików na żądanie. Koder służą do klipu lub przycinanie środka trwałego, co jest przechwycone lub zarchiwizowanych przy użyciu strumień na żywo — szczegóły tego są dostępne w [tym blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Przyciąć filmy wideo, możesz wykonać dowolną z MES ustawień wstępnych udokumentowanych [tutaj](https://msdn.microsoft.com/library/mt269960.aspx) i modyfikowanie elementu **źródła** (jak pokazano poniżej). Wartość czas rozpoczęcia musi zgodnie z bezwzględnych sygnatury czasowe wprowadzania wideo. Na przykład jeśli pierwsza klatka wprowadzania klip wideo zawiera sygnatury czasowej 12:00:10.000, następnie czas rozpoczęcia powinny być co najmniej 12:00:10.000 lub nowszej. W poniższym przykładzie przyjęto założenie, że wprowadzania klip wideo zawiera początkowe sygnatura czasowa zero. **Źródła** powinny znajdować się na początku ustawienie. 
 
###<a id="json"></a>Ustawienie wstępne JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>Ustawienie wstępne XML
    
Przyciąć filmy wideo, możesz wykonać dowolną z MES ustawień wstępnych udokumentowanych [tutaj](https://msdn.microsoft.com/library/mt269960.aspx) i modyfikowanie elementu **źródła** (jak pokazano poniżej).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Tworzenie nakładki

Media Encoder standardowe umożliwia nakładki obraz do istniejącego klipu wideo. Obecnie obsługiwane są następujące formaty: png, jpg, gif, bmp i. Ustawienie określone poniżej jest prosty przykład nakładania wideo.

Oprócz definiowania wstępnie ustawionych pliku, masz również umożliwić usługi multimediów o pliku, który w elementu jest obraz nakładki i plik, który jest źródłem wideo na którym chcesz nałożyć obrazu. Plik wideo musi być pliku **podstawowego** . 

W powyższym przykładzie .NET definiuje dwie funkcje: **UploadMediaFilesFromFolder** i **EncodeWithOverlay**. Funkcja UploadMediaFilesFromFolder przekazywania plików z folderu (na przykład BigBuckBunny.mp4 i Image001.png) i ustawia plik mp4, który można plik podstawowy w elementu. Funkcja **EncodeWithOverlay** używa pliku wstępnie ustawionych niestandardowej przekazano do niego (na przykład ustawienie występujący) aby utworzyć zadanie kodowania. 

>[AZURE.NOTE]Bieżące ograniczenia:
>
>Ustawienie przezroczystość nakładki nie jest obsługiwane.
>
>Źródłowego pliku wideo i pliku obrazu nakładki muszą znajdować się w tym samym zawartości, a plik wideo musi być ustawiona jako plik podstawowy w tej zawartości.

###<a name="json-preset"></a>Ustawienie wstępne JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>Ustawienie wstępne XML
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Wstawianie odbiorcze ścieżki dźwiękowej po wprowadzania bez dźwięku

Domyślnie jeśli wysyłasz dane wejściowe do kodera zawierający tylko, audio i wideo nie, wówczas trwałego wynik zawiera pliki, które zawierają dane tylko wideo. Niektóre odtwarzacze nie można obsługiwać takie strumieni wyjściowych. To ustawienie służy do wymusić encoder, aby dodać odbiorcze ścieżki dźwiękowej do wyniku w tym scenariuszu.

Aby wymusić encoder da środka trwałego, zawierającą odbiorcze ścieżki dźwiękowej, kiedy wejściowy bez dźwięku, określ wartość "InsertSilenceIfNoAudio".

Można wykonać dowolną z MES ustawień wstępnych udokumentowane [poniżej](https://msdn.microsoft.com/library/mt269960.aspx), a następnie wprowadź następujące zmiany:

###<a name="json-preset"></a>Ustawienie wstępne JSON

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>Ustawienie wstępne XML

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Wyłączanie automatycznego usuwania przeplot

Klienci nie musisz nic robić, jeśli ich zawartość przeplotu, aby się automatycznie, usuń zaznaczenie pola z przeplotem, takich jak. Po włączeniu przeplotu wyłączyć automatyczne (ustawienie domyślne) żądania uznania powoduje automatyczne wykrywanie ramek z przeplotem i tylko Anuluj interlaces ramki oznaczone jako przeplotu.

Możesz wyłączyć przeplotu wyłączyć automatyczne. Ta opcja nie jest zalecane.

###<a name="json-preset"></a>Ustawienie wstępne JSON
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>Ustawienie wstępne XML
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Ustawienia wstępne tylko audio

W tej sekcji przedstawiono dwa ustawienia MES tylko audio: AAC Audio i AAC dobrej jakości dźwięku.

###<a name="aac-audio"></a>AAC Audio 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC dobrej jakości dźwięku

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>ZŁĄCZ.teksty dwóch lub więcej plików wideo

Poniższy przykład pokazuje, jak można wygenerować ustawienie służy do łączenia dwóch lub więcej plików wideo. Najbardziej typowy scenariusz jest, gdy chcesz dodać nagłówek i centralną do głównego pliku wideo. Przeznaczenie jest udostępniający właściwości (rozdzielczości szybkość odtwarzania, Zliczanie ścieżki dźwiękowej, itp.), pliki wideo edytowany razem. Należy zadbać o mieszać klipów wideo różne szybkości odtwarzania lub inną liczbę ścieżki audio.

###<a name="requirements-and-considerations"></a>Wymagania i zagadnienia

- Klipy wideo wprowadzania danych powinien mieć tylko jeden ścieżki dźwiękowej.
- Klipy wideo wprowadzania danych powinien mieć samej szybkość odtwarzania.
- Należy przekazać pliki wideo w osobnym aktywa i skonfigurować klipy wideo jako plik podstawowy w poszczególnych elementów zawartości.
- Należy ustalić czas trwania swoich klipów wideo.
- Wstępnie ustawionych poniższych przykładach założono, że wszystkie klipy wideo wprowadzania początkowych sygnatura czasowa zero. Należy zmodyfikować wartości czas rozpoczęcia, jeśli klipy wideo mają różne początkowe sygnatury czasowej, tak jak zwykle w przypadku z archiwami live.
- Ustawienie JSON sprawia, że jawne odwołania do wartości IDzasobu wprowadzania środków trwałych.
- W przykładowym kodzie przyjęto założenie, że ustawienie JSON został zapisany plik lokalny, takie jak "C:\supportFiles\preset.json". Zakłada się również, że utworzono dwa elementy zawartości, przekazując dwa pliki wideo i że wiesz, wynikowy wartości IDzasobu.
- Wstawkę kodu i JSON ustawienie wstępne przedstawiono przykład łączenia dwóch plików wideo. Można go rozszerzyć na więcej niż dwa klipy wideo przez:

    1. Zadanie telefonicznej. InputAssets.Add() wielokrotnie, aby dodać więcej klipów wideo w kolejności.
    2. Tworzenie odpowiadającą edytuje do elementu "Źródeł" w formacie JSON, dodając więcej wpisów w tej samej kolejności. 


###<a name="net-code"></a>Kod .NET

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>Ustawienie wstępne JSON

Aktualizowanie niestandardowe wstępnie ustawione identyfikatorami środków trwałych, które chcesz połączyć, a z segmentu odpowiedni czas dla każdego wideo.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Przycinanie klipów wideo z Media Encoder standardowy

Zobacz temat [przycinania klipów wideo z Media Encoder standardowy](media-services-crop-video.md) .

##<a id="no_video"></a>Wstawianie ścieżki wideo podczas wprowadzania nie ma karty wideo

Domyślnie jeśli wysyłasz dane wejściowe do kodera zawierający tylko audio i wideo nie wówczas trwałego wynik zawiera pliki, które zawierają dane tylko audio. Niektóre odtwarzacze tym Azure Media Player (zobacz [ten](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nie można obsługiwać takie strumieni. To ustawienie służy do wymusić encoder, aby dodać czarno-białych Ścieżka wideo do wyniku w tym scenariuszu. 

>[AZURE.NOTE]Wymuszanie encoder, aby wstawić Ścieżka wideo wynik zwiększa rozmiar wynik trwały, i w związku z tym koszty poniesione w związku z kodowania zadania. Należy uruchomić badań w celu sprawdzenia, czy ten wynikowy wzrost ma jedynie niewielkie wpływ na opłaty miesięczne.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Wstawianie wideo tylko najniższe szybkość transmisji bitów

Załóżmy, że korzystasz z wielu szybkości transmisji bitów kodowanie ustawień, takich jak ["H264 wielu szybkości transmisji bitów 720p"](https://msdn.microsoft.com/library/mt269960.aspx) do kodowania całego wprowadzania katalogu do przesyłania strumieniowego, zawierające pliki wideo i pliki audio. W tym scenariuszu podczas wejściowy nie wideo może chcesz wymusić encoder, aby wstawić czarno-białych Ścieżka wideo tylko najniższe szybkość transmisji bitów, zamiast Wstawianie wideo w każdej szybkość transmisji bitów danych wyjściowych. Aby osiągnąć ten cel, musisz określić flagi "InsertBlackIfNoVideoBottomLayerOnly".

Można wykonać dowolną z MES ustawień wstępnych udokumentowane [poniżej](https://msdn.microsoft.com/library/mt269960.aspx), a następnie wprowadź następujące zmiany:

#### <a name="json-preset"></a>Ustawienie wstępne JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Ustawienie wstępne XML

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Wstawianie wideo w ogóle wyjściowy bitrates

Załóżmy, że korzystasz z wielu szybkości transmisji bitów kodowanie ustawień, takich jak ["H264 wielu szybkości transmisji bitów 720p](https://msdn.microsoft.com/library/mt269960.aspx) do kodowania całego wprowadzania katalogu do przesyłania strumieniowego, zawierające pliki wideo i pliki audio. W tym scenariuszu podczas wejściowy nie wideo może chcesz wymusić encoder, aby wstawić czarno-białych Ścieżka wideo na wszystkich bitrates dane wyjściowe. Dzięki temu wydruku są wszystkie jednorodnych w odniesieniu do liczby ścieżek wideo i ścieżki audio. Aby osiągnąć ten cel, musisz określić flagi "InsertBlackIfNoVideo".

Można wykonać dowolną z MES ustawień wstępnych udokumentowane [poniżej](https://msdn.microsoft.com/library/mt269960.aspx), a następnie wprowadź następujące zmiany:

#### <a name="json-preset"></a>Ustawienie wstępne JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Ustawienie wstępne XML
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Obracanie klipu wideo

[Media Encoder standardowy](media-services-dotnet-encode-with-media-encoder-standard.md) obsługuje obrotu kątami 0-90/180/270 stopni. Domyślne zachowanie jest "Automatyczny", gdzie próbie wykrywać metadanych obrotu w importowanego pliku wideo i wyrównania go. Obejmują następujący element **źródeł** do jednego z ustawień wstępnych zdefiniowane [poniżej](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>Ustawienie wstępne JSON

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>Ustawienie wstępne XML

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Zobacz też [w tym](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) temacie, aby uzyskać więcej informacji na koder interpretowanie ustawień wysokość i szerokość w ustawienie wstępne, gdy zostanie wywołana rekompensaty obrotu.

Za pomocą wartość "0" zasygnalizować encoder, aby zignorować metadanych obrót, jeśli jest dostępna w danych wejściowych wideo. 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też 

[Omówienie kodowania usługi multimediów](media-services-encode-asset.md)
