<properties
    pageTitle="Przy użyciu szyfrowania dynamiczne AES-128 i klucza usługi | Microsoft Azure"
    description="Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie zawartości zaszyfrowanych za pomocą kluczy szyfrowania AES 128-bitowego. Usługi multimediów udostępnia również usługę dostarczania klucza, która udostępnia kluczy szyfrowania dla autoryzowanych użytkowników. W tym temacie przedstawiono sposoby dynamicznego Szyfruj przy użyciu AES-128 i korzystania z usług dostarczania klucza."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Przy użyciu szyfrowania dynamiczne AES-128 i klucza usługi

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Omówienie

>[AZURE.NOTE] Zobacz [ten](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) klip wideo zawiera omówienie sposobu ochrony zawartości z multimediów z szyfrowania AES.

Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie Http-Live — Streaming (HLS) i wygładzonymi strumieni zaszyfrowany przy użyciu szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego). Usługi multimediów udostępnia również usługę dostarczania klucza, która udostępnia kluczy szyfrowania dla autoryzowanych użytkowników. Usługi multimediów do szyfrowania środka trwałego, należy skojarzyć kluczem szyfrowania elementu, a także skonfigurować zasady autoryzacji klucza. W przypadku zażądania strumienia przez odtwarzacz, usługi multimediów używa określonego klucza dynamicznie szyfrowanie treści przy użyciu szyfrowania AES. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi klucza dostarczenia. Określanie, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasady autoryzacji, które określone dla klucza.

Usługi multimediów obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: Otwórz lub token ograniczeń. Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w [Prostej tokeny sieci Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a formatem [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Aby uzyskać więcej informacji zobacz [Konfigurowanie zasad autoryzacji klucz zawartości](media-services-protect-with-aes128.md#configure_key_auth_policy).

Aby skorzystać z dynamicznego szyfrowania, należy następująco skonfigurować zasób zawiera zestaw szybkość transmisji bitów wielokrotne MP4 pliki lub szybkość transmisji bitów wielokrotne wygładzonymi przesyłanie strumieniowe źródła. Musisz również skonfigurować zasady dostarczania zawartości (opisane w dalszej części tego tematu). Następnie w zależności od formatu określonego w adresie URL przesyłanie strumieniowe, na żądanie Streaming server zapewni dostarczenie strumienia przez protokół, który został wybrany. W wyniku potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta.

W tym temacie będą przydatne dla deweloperów, którzy opracowywać aplikacje, które dostarczają chronionego multimediów. Temat pokazano, jak skonfigurować usługę dostarczania klucza z zasady autoryzacji tak, aby tylko autoryzowanych klientów otrzymują kluczy szyfrowania. Pokazuje także jak używać szyfrowania dynamiczne.

>[AZURE.NOTE]Aby zacząć używać szyfrowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę skali (nazywane także przesyłanie strumieniowe jednostka). Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dynamiczne szyfrowania AES 128 i przepływ pracy usługi dostarczania klucza

Poniżej przedstawiono ogólne kroki, które należy wykonać podczas szyfrowania aktywów z AES, za pomocą usługi dostawy klucza usługi multimediów, a także używa szyfrowania dynamiczne.

1. [Tworzenie środka trwałego i przekazywanie plików do elementu](media-services-protect-with-aes128.md#create_asset).
1. [Kodowanie zawartości z plikiem adaptacyjne szybkość transmisji bitów ustawione MP4](media-services-protect-with-aes128.md#encode_asset).
1. [Tworzenie zawartości klucza i kojarzenie go z zakodowany trwałym](media-services-protect-with-aes128.md#create_contentkey). Usługi multimediów klucz zawartości zawiera klucza szyfrowania zasobu.
1. [Konfigurowanie zasad autoryzacji klucz zawartości](media-services-protect-with-aes128.md#configure_key_auth_policy). Zasady zawartości autoryzacji klucza należy skonfigurowany przez Ciebie i spełnione przez klienta w kolejności klucz zawartości do dostarczenia do klienta.
1. [Konfigurowanie zasad dostarczania zawartości](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfiguracja zasad dostarczania obejmuje: główne nabycia adresu URL i wektor inicjowania (IV) (AES 128 wymaga samej IV dostarczyć przy szyfrowania i odszyfrowywania), protokół dostarczenia (na przykład MPEG KRESKI, HLS, obr. / min, gładkie Streaming lub wszystkie), typ dynamiczne szyfrowania (na przykład kopert lub bez szyfrowania dynamiczne).

Inne zasady można zastosować do każdego protokołu na tym samym zawartości. Na przykład szyfrowania PlayReady może dotyczą gładkie/ŁĄCZNIKA i AES kopertę HLS. Protokołów, które nie są zdefiniowane w zasadzie dostarczenia (na przykład możesz dodać pojedynczy zasadę tylko określająca HLS jako protokół) będą zablokowanie streaming. Wyjątek to, czy masz określono na wszystkich zasad dostarczania zawartości. Następnie wszystkie protokoły będą mogli w Wyczyść.

1. [Tworzenie locator na żądanie](media-services-protect-with-aes128.md#create_locator) , aby można było uzyskiwać przesyłanie strumieniowe adresu URL.

Temat również pokazano, [jak aplikacja klienta można zażądać klucza z usługi dostarczania klucza](media-services-protect-with-aes128.md#client_request).

Znajdziesz pełny.NET [przykład](media-services-protect-with-aes128.md#example) na końcu tematu.

Poniższy obraz przedstawia przepływ pracy opisany powyżej. Tutaj tokenu jest używany do uwierzytelniania.

![Ochrona z AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Pozostałej części tego tematu udostępnia uzyskać szczegółowe informacje, przykłady kodu i łączy do tematów, pokazujące, jak osiągnąć zadania opisane powyżej.

##<a name="current-limitations"></a>Ograniczenia w bieżącym

Jeśli Dodaj lub zaktualizuj zasady dostarczania składnika aktywów, należy usunąć istniejące locator (jeśli istnieją) i tworzenie nowej lokalizacji.

##<a id="create_asset"></a>Tworzenie środka trwałego i przekazywanie plików do elementu

Aby można było zarządzać, kodowanie i przesyłanie strumieniowe wideo, możesz przekazać zawartość do usługi multimediów Azure firmy Microsoft. Po przesłaniu zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego. 

Aby uzyskać szczegółowe informacje zobacz [Przekazywanie plików do konta usługi multimediów](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Kodowanie zawartości z plikiem adaptacyjne ustawione MP4 szybkość transmisji bitów

Dynamiczne szyfrowania będzie potrzebne jest utworzenie składnika aktywów, która zawiera zestaw szybkość transmisji bitów wielokrotne MP4 pliki lub szybkość transmisji bitów wielokrotne płynnego przesyłania strumieniowego źródła. Następnie zgodnie z określonym formatem w wezwaniu manifest lub fragment Streaming na żądanie serwera zapewni odbierać strumień przez protokół, który został wybrany. W wyniku potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta. Aby uzyskać więcej informacji zobacz temat [Dynamiczny przegląd opakowania](media-services-dynamic-packaging-overview.md) .

Aby uzyskać instrukcje dotyczące kodowania, zobacz [jak do środka trwałego za pomocą Media Encoder Standard kodowania](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Tworzenie zawartości klucza i skojarzyć zakodowany składników majątku

Usługi multimediów klucz zawartości zawiera klucz, który chcesz zaszyfrować środka trwałego z.

Aby uzyskać szczegółowe informacje zobacz [Tworzenie klucz zawartości](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Konfigurowanie zasad autoryzacji klucz zawartości

Usługi multimediów obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza. Zasady zawartości autoryzacji klucza należy skonfigurowany przez Ciebie i spełnione przez klienta (player) klucz do dostarczenia do klienta. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: otwieranie, token ograniczeń ani ograniczeń adresów IP.

Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie zasad autoryzacji klucza zawartości](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Konfigurowanie zasad dostarczania zawartości 

Konfigurowanie zasad dostarczania dla swojej zawartości. Kwestie, które zawiera konfiguracji zasad dostarczania zawartości:

- Adres URL nabycia klucza. 
- Inicjowanie wektorowa IV służących do szyfrowania koperty. AES 128 wymaga samej IV dostarczyć przy szyfrowania i odszyfrowywania. 
- Protokół dostarczania zawartości (na przykład MPEG KRESKI, HLS, obr. / min, gładkie Streaming lub wszystkie).
- Typ szyfrowania dynamiczne (na przykład Koperta AES) lub bez szyfrowania dynamiczne. 

Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie zasad dostarczania zawartości ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Tworzenie na żądanie streaming locator, aby można było uzyskiwać przesyłanie strumieniowe adresu URL

Konieczne będzie podanie użytkownika ze strumieniowych adresu URL gładkie, ŁĄCZNIKA lub HLS.

>[AZURE.NOTE]Jeśli Dodaj lub zaktualizuj zasady dostarczania składnika aktywów, należy usunąć istniejące locator (jeśli istnieją) i tworzenie nowej lokalizacji.

Aby uzyskać instrukcje, jak publikować środka trwałego i tworzyć przesyłanie strumieniowe adresu URL zobacz [Tworzenie przesyłanie strumieniowe adresu URL](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Uzyskiwanie odpowiedzi w teście tokenu

Uzyskiwanie odpowiedzi w teście tokenu oparte na to ograniczenie tokenu użytego zasady autoryzacji klucza.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

[Odtwarzacza AMS](http://amsplayer.azurewebsites.net/azuremediaplayer.html) umożliwia testowanie strumienia.

##<a id="client_request"></a>Jak klient żądanie klucz z usługi dostarczania klucza

W poprzednim kroku tworzona jest adres URL wskazujący plik manifestu. Klient musi wyodrębnić niezbędne informacje z przesyłanie strumieniowe pliki manifestu Aby złożyć wniosek z usługą dostarczania klucza.

###<a name="manifest-files"></a>Pojawiają plików

Klient musi wyodrębnić adres URL (zawierający także klucz zawartości identyfikator (dla dzieci)) wartość z pliku manifestu. Klient próbuje uzyskać klucza szyfrowania z usługi klucza dostarczenia. Klient musi wyodrębnić wartości IV i użyj odszyfrowywanie strumienia. Poniższej wstawek <Protection> element manifestu Streaming gładkie.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

W przypadku HLS manifest główny jest podzielone na części plików. 

Na przykład manifest główny to: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) i zawiera listę nazw plików części.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Po otwarciu pliku części w edytorze tekstów (na przykład http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), powinien zawierać #EXT-X-klucz, który wskazuje, że plik jest zaszyfrowany.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Poproś klucz usługi dostarczania klucza

Poniższy kod ilustruje wysłać żądanie w usłudze klucza dostarczenia usługi multimediów, przy użyciu klucza dostarczenia identyfikator Uri (zostało wyodrębnione z manifest) i tokenu (w tym temacie nie porozmawiać o sposób uzyskiwania prosty tokeny sieci Web z bezpiecznego usługi tokenu).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Przykład

1. Tworzenie nowego projektu konsoli.
1. Aby zainstalować i dodać rozszerzenia SDK .NET usług multimediów Azure za pomocą NuGet. Również instalowania tego pakietu, instaluje multimediów usług .NET SDK oraz dodaje wszystkie wymagane zależności.
2. Dodawanie pliku konfiguracji, który zawiera nazwę konta i informacje o kluczu:

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Zastąp kod w pliku Plik Program.cs kod wyświetlany w tej sekcji.
    
    Upewnij się zaktualizować zmienne wskaż miejsce, w którym znajdują się pliki wprowadzania folderów.
            
        
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
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
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
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
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
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
