<properties 
    pageTitle="Zaawansowane kodowanie w przepływie pracy programu Media Encoder Premium | Microsoft Azure" 
    description="Dowiedz się, jak kodowanie w Media Encoder Premium w przepływie pracy. Przykłady kodu są zapisane w C# i używanie SDK usługi multimediów dla środowiska .NET." 
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

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Zaawansowane kodowanie w Media Encoder Premium w przepływie pracy

>[AZURE.NOTE] Procesor multimediów Media Encoder Premium przepływu omówione w tym temacie nie jest dostępna w Chinach.

Aby pytania encoder premium poczty e-mail mepd z witryny Microsoft.com.

##<a name="overview"></a>Omówienie

Usługi multimediów Azure firmy Microsoft jest wprowadzenie procesor multimediów **Media Encoder Premium w przepływu pracy** . Tej oferty procesor advance kodowanie możliwości przepływów pracy na żądanie premium. 

W poniższych tematach przedstawiono szczegółowe informacje dotyczące **Media Encoder Premium w przepływu pracy**: 

- [Obsługiwane formaty przez przepływ pracy Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) — Discusses kodery-dekodery i formaty plików obsługiwane przez **Przepływ pracy programu Media Encoder Premium**.

- [Porównanie kodery](media-services-encode-asset.md#compare_encoders) sekcji porównano kodowania możliwości **Media Encoder Premium przepływu** i **Media Encoder standardowy**.

W tym temacie przedstawiono sposób kodowanie z **Media Encoder Premium w przepływu pracy** przy użyciu .NET.

Kodowania zadania **Przepływu pracy programu Media Encoder Premium** wymagają pliku konfiguracji osobnych o nazwie pliku przepływu pracy. Te pliki mają rozszerzenie .workflow i są tworzone przy użyciu narzędzia [Projektant przepływów pracy](media-services-workflow-designer.md) .

##<a name="encode"></a>Kodowanie

Kodowania zadania **Przepływu pracy programu Media Encoder Premium** wymagają pliku konfiguracji osobnych o nazwie pliku przepływu pracy. Te pliki mają rozszerzenie .workflow i są tworzone przy użyciu narzędzia [Projektant przepływów pracy](media-services-workflow-designer.md) .


Możesz również wyświetlić domyślny pliki przepływu pracy [w tym miejscu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Folder zawiera również opis tych plików.

Pliki przepływu pracy należy przekazać do konta usługi multimediów jako środka trwałego i tego trwałego należy przekazać do kodowania zadania.

Poniższy przykład ilustruje kodowanie w **Media Encoder Premium w przepływie pracy**. 

Wykonywane są następujące czynności: 
 
1. Tworzenie środka trwałego i przekaż plik przepływu pracy. 
2. Tworzenie środka trwałego i przekaż plik multimedialny źródła.
3. Uzyskaj procesor multimediów "Media Encoder Premium przepływu pracy".
4. Tworzenie zadania i zadania. 

    W większości przypadków jest pusty ciąg konfiguracji dla zadania (na przykład w poniższym przykładzie). Istnieje kilka zaawansowanych scenariuszy (które wymagają ustawiania właściwości środowisko uruchomieniowe dynamicznie) w tym przypadku zapewni ciągu XML do kodowania zadania. Przykładowe scenariusze tego typu: tworzenie nakładki, media równoległych lub kolejnych łączenia, napisy.
5. Dodawanie dwóch aktywów wprowadzania danych do zadania.
    
    . 1 — elementu przepływu pracy.

    b. 2 — zawartości wideo.
    
    **Uwaga**: trwały przepływ pracy musi zostać dodana do zadania przed multimedialnej. Ciąg konfiguracji dla tego zadania powinna być pusta. 

6. Prześlij kodowania zadanie.

Poniżej przedstawiono przykład ukończone. Aby uzyskać informacje na temat konfiguracji rozwoju .NET usługi multimediów, zobacz [opracowywania usługi multimediów z .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
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
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Aby pytania encoder premium poczty e-mail mepd z witryny Microsoft.com.

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
