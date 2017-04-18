<properties 
    pageTitle="Kodowanie środka trwałego przy użyciu .NET Media Encoder Standard | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób kodowanie środka trwałego za pomocą Media Encoder Strandard za pomocą .NET." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Kodowanie środka trwałego przy użyciu .NET Media Encoder Standard

Zadania kodowania to jeden z najbardziej typowych operacji w usługach multimediów. Tworzenie zadań kodowania do konwersji plików multimedialnych z jednego kodowania do innego. Podczas kodowania, można użyć wbudowanego Media Encoder usługi multimediów. Umożliwia także kodera dostarczony przez partnera usługi multimediów; kodery innych firm są dostępne za pośrednictwem usługi Azure Marketplace. 

W tym temacie przedstawiono sposób kodowanie aktywów z Media Encoder standardowy (MES) za pomocą .NET. Media Encoder standardowy jest skonfigurowana przy użyciu jednego z kodera ustawień wstępnych opisane [poniżej](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Zalecane jest zawsze kodowanie plików kodery do adaptacyjne szybkość transmisji bitów MP4 Ustaw, a następnie przekonwertowanie zestawu na wybranym formacie przy użyciu [Dynamiczne opakowań](media-services-dynamic-packaging-overview.md). Aby skorzystać z opakowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak skala usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

Jeśli z zasobów danych wyjściowych jest szyfrowane miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Aby uzyskać więcej informacji, zobacz [zasady dostarczania zawartości Konfigurowanie](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MES tworzy plik docelowy z nazwy, która zawiera pierwsze 32 znaki nazwę pliku wejściowego. Nazwa jest oparty na opisem w istniejących plików. Na przykład "nazwa_pliku": "_ {Index} {rozszerzenia} {Basename}". {Basename} jest zastępowana przez pierwsze 32 znaki nazwę pliku wejściowego.

###<a name="mes-formats"></a>Formaty MES

[Kodery-dekodery i formaty](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Ustawienia wstępne MES

Media Encoder standardowy jest skonfigurowana przy użyciu jednego z kodera ustawień wstępnych opisane [poniżej](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Wejściowe i wyjściowe metadanych

Podczas kodowania środka trwałego wprowadzania (lub składniki majątku) za pomocą MES, środka trwałego dane wyjściowe zostanie wyświetlony po pomyślnym zakończeniu tego kodowanie zadań. Trwały wynik zawiera klip wideo, audio, miniatury, manifest, itd., w oparciu o te ustawienia kodowania, którego używasz.

Trwały wynik zawiera również plik z metadanych dotyczących wprowadzania zasobów. Nazwa pliku XML metadanych ma następujący format: < asset_id > _metadata.xml (na przykład 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), gdzie < asset_id > to wartość IDzasobu środka wprowadzania danych. Schemat wprowadzania metadanych XML jest opisany [poniżej](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Trwały wynik zawiera również pliku metadanymi o trwałym dane wyjściowe. Nazwa pliku XML metadanych ma następujący format: < source_file_name > _manifest.xml (na przykład BigBuckBunny_manifest.xml). Schemat metadanych dane wyjściowe XML jest opisany [poniżej](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Jeśli chcesz sprawdzić jedną z dwóch pliki metadanych, można utworzyć locator skojarzeń zabezpieczeń i pobrać plik na komputerze lokalnym. Przykład można znaleźć na temat tworzenia locator skojarzeń zabezpieczeń i pobrać plik przy użyciu rozszerzenia SDK .NET usługi multimediów.

##<a name="download-sample"></a>Pobieranie przykładowych

Pobieranie i uruchamianie próbki [tutaj](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Przykład

W poniższym przykładzie użyto SDK .NET usługi multimediów umożliwia wykonywanie następujących zadań:

- Tworzenie kodowania zadania.
- Odwołać się do kodera Media Encoder standardowy.
- Określ, aby użyć "H264 wielu szybkości transmisji bitów 720p" wstępnie ustawionych. Można wyświetlić wszystkie ustawienia wstępne [w tym miejscu](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Można również przyjrzeć się schematu, do którego muszą spełniać następujące ustawienia wstępne [poniżej](https://msdn.microsoft.com/library/mt269962.aspx) tematu.
- Dodawanie pojedynczego zadania kodowania do zadania. 
- Określ wprowadzania środków trwałych ma być zakodowany.
- Tworzenie środka trwałego dane wyjściowe, zawierające zakodowany zawartości.
- Dodawanie zdarzeń, aby sprawdzić postęp zadania.
- Przesyłanie zadania.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
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


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też 

[Jak wygenerować Miniatura za pomocą Media Encoder standardowy .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Omówienie kodowanie usług multimediów](media-services-encode-asset.md)
