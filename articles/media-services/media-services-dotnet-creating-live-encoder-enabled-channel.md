<properties 
    pageTitle="Jak przeprowadzić live streaming tworzenie szybkość transmisji bitów wielu strumieni program .NET za pomocą usługi multimediów Azure | Microsoft Azure" 
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia kanał, który odbiera strumień na żywo szybkość transmisji bitów pojedyncze i kodowane strumieniu szybkość transmisji bitów wielu przy użyciu zestawu SDK .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Jak przeprowadzić live streaming tworzenie szybkość transmisji bitów wielu strumieni program .NET za pomocą usługi multimediów Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Omówienie

Ten samouczek przeprowadzi Cię przez kroki tworzenia **kanału** , który odbiera strumień na żywo szybkość transmisji bitów pojedyncze i kodowane strumieniu szybkość transmisji bitów wielokrotne.

Uzyskać więcej informacji pojęć związanych z kanałów, do których włączono usługę live kodowanie zobacz [Przesyłanie strumieniowe przy użyciu usługi multimediów Azure, aby utworzyć szybkość transmisji bitów wielu strumieni na żywo](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Typowy scenariusz przesyłanie strumieniowe Live

W poniższej procedurze opisano zadań związanych z tworzenia live strumieniowo aplikacje.

>[AZURE.NOTE] Zalecane maksymalny czas trwania zdarzenia na żywo jest obecnie, 8 godzin. Jeśli jest potrzebne do uruchamiania kanału przez dłuższy czas, skontaktuj się z amslived z witryny Microsoft.com.

1. Połącz kamery wideo do komputera. Uruchamianie i Konfigurowanie kodera live lokalnego można wyjściowy strumienia pojedynczy szybkość transmisji bitów w jednym z następujących protokołów: RTMP, gładkie Streaming lub RTP (MPEG-TS). Aby uzyskać więcej informacji zobacz [obsługę RTMP usługi multimediów Azure i kodery Live](http://go.microsoft.com/fwlink/?LinkId=532824).

Ponadto można wykonać ten krok, po utworzeniu kanału.

1. Tworzenie i zacznij kanału.

1. Pobierz kanał mogły zjeść tej ostatniej adresu URL.

Adres URL ingest jest używana przez koder live wysyłanie strumienia do kanału.

1. Pobieranie adres URL podglądu kanału.

Aby sprawdzić, czy kanału poprawnie odbiera strumieniem na żywo za pomocą tego adresu URL.

2. Tworzenie środka trwałego.
3. Jeśli chcesz trwałego dynamicznie szyfrowania podczas odtwarzania, wykonaj następujące czynności:
1. Tworzenie zawartości klucza.
1. Konfigurowanie zasad autoryzacji klucz zawartości.
1. Konfigurowanie zasad dostarczania zawartości (używane przez dynamiczne opakowań i szyfrowania dynamiczne).
3. Tworzenie programu i określić użycie zasobów, utworzone przez Ciebie.
1. Publikowanie zawartości skojarzonej z programem, tworząc locator na żądanie.

Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.

1. Uruchom program po zacząć strumieniowych i archiwizacji.
2. Opcjonalnie live encoder można sygnalizowane zacząć reklamę. Ogłoszenie zostanie wstawiony w strumienia wyjściowego.
1. Zatrzymać program, gdy chcesz zatrzymać strumieniowych i archiwizacji wydarzenia.
1. Usuwanie programu (i opcjonalnie można usunąć elementu).

## <a name="what-youll-learn"></a>Dowiesz się

W tym temacie pokazano, jak wykonywać różne operacje na kanałów i programy przy użyciu zestawu SDK .NET usługi multimediów. Ponieważ wiele operacji są długotrwałe interfejsy API .NET, którzy zarządzają długotrwałe uruchamianie operacji są używane.

Temat pokazano, jak wykonać następujące czynności:

1. Tworzenie i zacznij kanału. Interfejsy API długim są używane.
1. Uzyskiwanie kanałów mogły zjeść tej ostatniej końcowy (wejście danych). Ten punkt końcowy należy podać do kodera, które może przesyłać strumień na żywo pojedynczy szybkość transmisji bitów.
1. Uzyskaj punkt końcowy Podgląd. Aby wyświetlić podgląd swojego strumienia jest używany tego punktu końcowego.
1. Tworzenie elementów zawartości, która będzie używana do przechowywania zawartości. Zasady dostarczania zawartości należy skonfigurować także, jak pokazano w poniższym przykładzie.
1. Utwórz program i określ używać elementu, który został utworzony wcześniej. Uruchom program. Interfejsy API długim są używane.
1. Utwórz locator dla trwałego, aby zawartość zostanie i mogą być przesyłane strumieniowo do klientów.
1. Pokazywanie i ukrywanie kreda. Uruchamianie i zatrzymywanie reklam. Interfejsy API długim są używane.
1. Oczyść kanału i skojarzonych z nimi zasobów.


##<a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do zakończenia tego samouczka.

- Aby użyć tego samouczka, potrzebne jest konto Azure.

Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F). Uzyskaj środków używane wypróbować płatnych usług Azure. Nawet w przypadku, gdy środki są używane w górę, możesz zachować konta i za pomocą bezpłatnej usługi Azure i funkcje, takie jak funkcja aplikacji sieci Web w usłudze Azure aplikacji.
- Konto usługi multimediów. Aby utworzyć konto usługi multimediów, zobacz [Tworzenie konta](media-services-portal-create-account.md).
- Visual Studio 2010 z dodatkiem SP1 (Professional, Premium, Ultimate lub Express) lub nowszym.
- Należy użyć multimediów usług .NET SDK wersji 3.2.0.0 lub nowszej.
- Kamery internetowej i kodera, które może przesyłać strumień na żywo pojedynczy szybkość transmisji bitów.

##<a name="considerations"></a>Zagadnienia dotyczące

- Maksymalny czas trwania zalecane zdarzenia na żywo jest obecnie, 8 godzin. Jeśli jest potrzebne do uruchamiania kanału przez dłuższy czas, skontaktuj się z amslived z witryny Microsoft.com.
- Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.

##<a name="download-sample"></a>Pobieranie przykładowych

Pobieranie i uruchamianie próbki [tutaj](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Konfigurowanie rozwoju z SDK usługi multimediów dla środowiska .NET

1. Tworzenie aplikacji konsoli przy użyciu programu Visual Studio.
1. Dodawanie SDK usługi multimediów dla środowiska .NET do konsoli aplikację za pomocą pakietu NuGet usługi multimediów.

##<a name="connect-to-media-services"></a>Łączenie się z usługami multimediów
Zgodnie z zaleceniami dotyczącymi pliku app.config należy używać do przechowywania klucza nazwy i konto usługi multimediów.

>[AZURE.NOTE]Aby znaleźć nazwę i klucz wartości, przejdź do portalu Azure i wybierz swoje konto. Po prawej stronie zostanie wyświetlone okno Ustawienia. W oknie Ustawienia wybierz pozycję klawiszy. Klikając ikonę obok każdego pola tekstowego kopiuje wartość do Schowka systemu.

Dodawanie sekcji appSettings do pliku app.config i ustaw wartości dla klucz konta i nazwę konta usługi multimediów.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Przykład kodu

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
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
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
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
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Szukasz czegoś innego?

Jeśli w tym temacie nie zawiera, co możesz zostały oczekiwano, czy nie brakuje czegoś lub w inny sposób nie spełnia określonych wymagań, podaj Ci opinii za pomocą wątku Disqus poniżej.
