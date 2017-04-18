<properties 
    pageTitle="Monitorowanie usługi multimediów zadania powiadomienia z .NET za pomocą magazyn kolejek Azure | Microsoft Azure" 
    description="Dowiedz się, jak używać magazyn kolejek Azure monitorowanie powiadomienia zadania usługi multimediów. Przykładowy kod jest zapisywany w języku C# i używa SDK usługi multimediów dla środowiska .NET." 
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
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Monitorowanie powiadomienia zadania usługi multimediów z .NET za pomocą magazyn kolejek Azure

Po uruchomieniu zadań często wymagają umożliwia śledzenie postępu zadania. Możesz sprawdzić postęp przy użyciu magazyn kolejek Azure monitorowanie powiadomienia zadania usługi multimediów (zgodnie z opisem w tym temacie) lub definiowania obsługi zdarzeń StateChanged (zgodnie z opisem w [tym](media-services-check-job-progress.md) temacie.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Monitorowanie usługi multimediów zadania powiadomienia za pomocą magazyn kolejek Azure

Usługi multimediów Azure Microsoft ma możliwość tworzenia powiadomieniami [magazyn kolejek Azure](../storage-dotnet-how-to-use-queues.md#what-is) podczas przetwarzania zadania multimediów. W tym temacie przedstawiono sposób uzyskiwania SetWarnings powiadomienie z magazynu w kolejce.

Dostarczane do magazynu kolejki wiadomości są dostępne z dowolnego miejsca na świecie. Kolejka Azure wiadomości architektura jest wiarygodnych i wysoce skalowalna. Sondowanie magazyn kolejek zaleca się nad innymi metodami. 

Język, który jest typowy scenariusz dla słuchanie powiadomienia usługi multimediów, jeśli tworzysz systemu zarządzania zawartością, który należy wykonać kilka dodatkowych zadanie po zadaniu kodowania wykonuje (na przykład wyzwalacza następny krok w przepływie pracy lub publikować zawartość). 

###<a name="considerations"></a>Zagadnienia dotyczące

Podczas opracowywania aplikacji usługi multimediów, korzystających kolejki Azure miejsca do magazynowania, należy uwzględnić następujące czynności.

- Usługa kolejek nie zapewnia gwarancję pierwszego w pierwszym out (FIFO) zamówiono dostarczenia. Aby uzyskać więcej informacji zobacz [kolejki Azure i porównaniu kolejek Bus usługi Azure i Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure kolejek miejsca do magazynowania nie jest usługa wypychanych; musisz skorzystać kolejki. 
- Mogą zawierać dowolną liczbę kolejek. Aby uzyskać więcej informacji zobacz [Interfejsu API usługi REST usługi kolejki](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure kolejek miejsca do magazynowania ma pewne ograniczenia i mające opisane w artykule: [kolejki Azure i porównaniu kolejek Bus usługi Azure i Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Przykład kodu

Przykład kodu w tej sekcji wykonuje następujące czynności:

1. Określa klasę **EncodingJobMessage** , która map do formatu wiadomości powiadomień. Kod deserializes wiadomości otrzymane z kolejki do obiektów typu **EncodingJobMessage** .
1. Ładuje informacje o koncie usługi multimediów i miejsca do magazynowania z pliku app.config. Aby utworzyć obiekty **CloudMediaContext** i **CloudQueue** korzysta z tych informacji.
1. Tworzy kolejka odbierająca powiadomieniami o zadaniu kodowania.
1. Tworzy punkt końcowy powiadomienie mapowanego do kolejki.
1. Dołącza punktu końcowego powiadomienie o zadaniu i przesyła kodowania zadania. Można mieć wiele punktów końcowych powiadomienie dołączonych do zadania.
1. W tym przykładzie opiszemy tylko Państw ostateczne przetwarzania zadania, więc możemy przekazać **NotificationJobState.FinalStatesOnly** metody **AddNew** . 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Przekazując NotificationJobState.All spodziewać Uzyskiwanie powiadomienia o zmianie stanu wszystkich: w kolejce -> zaplanowane -> Przetwarzanie -> zakończone. Jednak jak wspomniano wcześniej, usługa Azure miejsca do magazynowania kolejki nie daje gwarancji uporządkowane dostarczanie. Za pomocą właściwości sygnatury czasowej (zdefiniowane w typie EncodingJobMessage w poniższym przykładzie) do wiadomości kolejności. Istnieje możliwość, możesz uzyskać zduplikowanych powiadomień. Aby wyszukać duplikaty, należy użyć właściwości ETag (zdefiniowane w typie EncodingJobMessage). Jest również możliwe, że niektóre powiadomienia o zmianie stanu zostaną pominięte. 
1. Oczekuje na zadania przejść do postaci gotowej, zaznaczając pole wyboru kolejki co 10 sekund. Usuwa wiadomości, gdy są przetwarzane.
1. Usunięcie kolejki i punkt końcowy powiadomienie.

>[AZURE.NOTE]Zalecany sposób monitorować stan zadania jest słuchanie powiadomień, jak pokazano w poniższym przykładzie.
>
>Można również sprawdzić stanu zadania, przy użyciu właściwości **IJob.State** .  Powiadomienie o ukończeniu zadania może otrzymują przed stanu **IJob** jest ustawiona na **zakończone**. Właściwość **IJob.State** odzwierciedla dokładne Państwo niewielkie opóźnienie.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
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

W powyższym przykładzie wyprodukowano następujące wyniki. Możesz wartości są różne.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Następny krok

Przeglądanie ścieżki szkoleniowe dla użytkowników usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
