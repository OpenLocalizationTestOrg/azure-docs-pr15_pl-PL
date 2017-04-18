<properties 
    pageTitle="Sondowanie długotrwałych operacji | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób ankieta długotrwałych operacji." 
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


#<a name="delivering-live-streaming-with-azure-media-services"></a>Dostarczanie Live Streaming z usługi multimediów Azure

##<a name="overview"></a>Omówienie

Usługi multimediów Azure Microsoft oferuje interfejsów API, które wysyłają żądania do usługi multimediów do rozpoczęcia działalności (na przykład: tworzenie, uruchomić, zatrzymać lub usuwanie kanału). Operacje te są długotrwałe.

Multimedia usług .NET SDK zawiera interfejsów API Wyślij wezwanie i poczekaj na zakończenie operacji (wewnętrznie za pośrednictwem interfejsów API jest sondowania dla postęp operacji niektórych odstępach). Na przykład podczas połączenia kanału. Metoda Start(), zwraca po uruchomieniu kanału. Można również używać asynchroniczne wersji: oczekują kanału. StartAsync() (Aby uzyskać informacji na temat opartym na zadaniu asynchroniczne deseniu, zobacz [NACIŚNIJ](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). Interfejsy API, które wysłać żądanie operacji, a następnie skorzystać stanu, aż do zakończenia tej operacji są nazywane "ankieta metod". Te metody (zwłaszcza wersji asynchroniczne) są zalecane dla aplikacji klienta wzbogaconego i/lub usług stanowe.

Istnieją scenariusze, gdzie aplikacji nie można czekać długotrwałych żądania http i chce informacji o postępie operacji ręcznie. Typowy przykład będzie przeglądarki interakcja z usługi sieci web bezstanowe: gdy przeglądarka żąda, aby utworzyć kanał, usługi sieci web inicjuje długotrwałych operacji i zwraca identyfikator operacji do przeglądarki. Przeglądarki można następnie zapytaj usługi sieci web, aby sprawdzić stan operacji według identyfikatora Multimedia usług .NET SDK zawiera interfejsów API, które są przydatne w tym scenariuszu. Te interfejsy API są nazywane "metod innych niż ankieta".
"Metody nie ankieta" mieć następujące wzorzec nazewnictwa: wysyłanie operacji*OperationName*(na przykład SendCreateOperation). Wysłanie*OperationName*operacji zwracają obiektu **IOperation** ; zwracany obiekt zawiera informacje, które może służyć do śledzenia tej operacji. Metody OperationAsync*OperationName*Wyślij zwracać **zadania<IOperation>**.

Następujące klasy obsługuje obecnie, bez ankieta metod: **kanału**, **StreamingEndpoint**i **Program**.

Aby skorzystać stanu operacji, użyj metody **GetOperation** klasy **OperationBaseCollection** . Sprawdzanie stanu operacji za pomocą następujących odstępach: w przypadku operacji **kanału** i **StreamingEndpoint** , użyj 30 sekund; w przypadku operacji **programu** za pomocą 10 sekund.


##<a name="example"></a>Przykład

W poniższym przykładzie zdefiniowano klasy o nazwie **ChannelOperations**. Ta definicja klasy może być punkt początkowy dla swojej klasy definicji usługi sieci web. Dla uproszczenia w poniższych przykładach użyto wersji innych niż asynchroniczna metod.

W przykładzie pokazano również wykorzystania tej klasy klienta.

###<a name="channeloperations-class-definition"></a>Definicja klasy ChannelOperations

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Kod klienta

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
