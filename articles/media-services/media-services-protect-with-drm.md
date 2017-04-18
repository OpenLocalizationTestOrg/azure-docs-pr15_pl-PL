<properties
    pageTitle="Przy użyciu PlayReady i/lub Widevine dynamiczne szyfrowania typowych | Microsoft Azure"
    description="Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie strumienie MPEG-kreska, gładkie Streaming i Http-Live — Streaming (HLS) chronione za pomocą programu Microsoft PlayReady DRM. Można również do dostarczania ŁĄCZNIKA zaszyfrowanych za pomocą Widevine DRM. W tym temacie przedstawiono sposoby dynamicznego Szyfruj przy użyciu PlayReady i Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Przy użyciu PlayReady i/lub Widevine dynamiczne szyfrowania typowych

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie strumienie MPEG-kreska, gładkie Streaming i HTTP-Live — Streaming (HLS) chronione za pomocą [Programu Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Można również do przeprowadzania zaszyfrowany strumień ŁĄCZNIKA z Widevine DRM licencji. Zarówno PlayReady i Widevine są szyfrowane na specyfikacji typowych szyfrowania (ISO/IEC CENC 23001-7). [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (począwszy od wersji 3.5.1) lub interfejsu API usługi REST służy do konfigurowania programu AssetDeliveryConfiguration, aby użyć Widevine.

Usługi multimediów udostępnia usługę dostarczania PlayReady i Widevine DRM licencji. Usługi multimediów znajdują się również zawartości chronionej interfejsów API, które umożliwiają skonfigurowanie prawa i ograniczenia, które chcesz Runtime PlayReady lub Widevine DRM wymusić, gdy użytkownik odtwarzane. Gdy użytkownik zażąda zawartość DRM chronione, aplikacja odtwarzacza zażąda licencji w usłudze AMS licencji. Usługa licencji AMS będą wydawać licencji do odtwarzacza, czy jest autoryzowany. Licencja PlayReady lub Widevine zawiera klucz odszyfrowywania, który może być używany przez odtwarzacz klienta odszyfrowywanie i przesyłać strumieniowo zawartość.


Umożliwia także następujące partnerów AMS ułatwiające przeprowadzania licencji Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Aby uzyskać więcej informacji, zobacz: integracja z [Axinom](media-services-axinom-integration.md) i [castLabs](media-services-castlabs-integration.md).

Usługi multimediów obsługuje wiele sposobów autoryzowanie użytkowników, którzy Tworzenie żądania klucza. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: Otwórz lub token ograniczeń. Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w [Prostej tokeny sieci Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a formatem [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Aby uzyskać więcej informacji, zobacz Konfigurowanie zasad autoryzacji klucz zawartości.

Aby skorzystać z dynamicznego szyfrowania, należy następująco skonfigurować zasób zawiera zestaw szybkość transmisji bitów wielokrotne MP4 pliki lub szybkość transmisji bitów wielokrotne wygładzonymi przesyłanie strumieniowe źródła. Musisz również skonfigurować zasady dostarczania zawartości (opisane w dalszej części tego tematu). Następnie w zależności od formatu określonego w adresie URL przesyłanie strumieniowe, na żądanie Streaming server zapewni dostarczenie strumienia przez protokół, który został wybrany. W wyniku potrzebne do przechowywania i płatności dla plików w formacie jednego miejsca do magazynowania i usługi multimediów będzie tworzyć i obsługiwać odpowiednią odpowiedź HTTP na podstawie każdego żądania od klienta.

W tym temacie będą przydatne dla deweloperów, które działają w aplikacji, które dostarczają multimediów chronione za pomocą wielu DRMs, takich jak PlayReady i Widevine. Temat pokazano, jak skonfigurować usługę dostarczania licencji PlayReady z zasady autoryzacji, tak aby tylko autoryzowanych klientów otrzymują PlayReady lub Widevine licencji. Pokazuje także jak używać szyfrowania dynamiczne szyfrowania z PlayReady lub Widevine DRM nad ŁĄCZNIKA.

>[AZURE.NOTE]Aby zacząć używać szyfrowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę skali (nazywane także przesyłanie strumieniowe jednostka). Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Pobieranie przykładowych

Możesz pobrać przykładowy opisane w tym artykule [tutaj](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfigurowanie dynamicznego szyfrowania typowych i usług dostarczania licencji DRM

Poniżej przedstawiono ogólne kroki, które są potrzebne do wykonania, gdy ochrona aktywów z PlayReady za pomocą usługi dostawy licencji usługi multimediów, a także używa szyfrowania dynamiczne.

1. Tworzenie środka trwałego i przekazywanie plików do elementu.
1. Kodowanie zawartości z plikiem adaptacyjne szybkość transmisji bitów ustawione MP4.
1. Tworzenie zawartości klucza i skojarzyć zakodowany zawartości. Usługi multimediów klucz zawartości zawiera klucza szyfrowania zasobu.
1. Konfigurowanie zasad autoryzacji klucz zawartości. Zasady zawartości autoryzacji klucza należy skonfigurowany przez Ciebie i spełnione przez klienta w kolejności klucz zawartości do dostarczenia do klienta.

Podczas tworzenia zasad zawartości autoryzacji klucza, musisz określić następujące czynności: metody (PlayReady lub Widevine), ograniczeń dostarczenia (otwarty lub token), a informacje dotyczące typ klucza dostawy definiujące sposób klucz został dostarczony do klienta (szablon licencji[PlayReady](media-services-playready-license-template-overview.md) lub [Widevine](media-services-widevine-license-template-overview.md) ).
1. Konfigurowanie zasad dostarczania dla środka trwałego. Konfiguracja zasad dostarczania obejmuje: Protokół dostarczenia (na przykład MPEG KRESKI, HLS, obr. / min, gładkie Streaming lub wszystkie), typ szyfrowania dynamiczne (na przykład typowe szyfrowanie) PlayReady lub adres URL nabycia licencji Widevine.

Inne zasady można zastosować do każdego protokołu na tym samym zawartości. Na przykład szyfrowania PlayReady może dotyczą gładkie/ŁĄCZNIKA i AES kopertę HLS. Protokołów, które nie są zdefiniowane w zasadzie dostarczenia (na przykład możesz dodać pojedynczy zasadę tylko określająca HLS jako protokół) będą zablokowanie streaming. Wyjątek to, czy masz określono na wszystkich zasad dostarczania zawartości. Następnie wszystkie protokoły będą mogli w Wyczyść.
1. Tworzenie locator na żądanie, aby można było uzyskiwać przesyłanie strumieniowe adresu URL.

Znajdziesz rozbudowany przykład .NET na końcu tematu.

Poniższy obraz przedstawia przepływ pracy opisany powyżej. Tutaj tokenu jest używany do uwierzytelniania.

![Ochrona z PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Pozostałej części tego tematu udostępnia uzyskać szczegółowe informacje, przykłady kodu i łączy do tematów, pokazujące, jak osiągnąć zadania opisane powyżej.

##<a name="current-limitations"></a>Ograniczenia w bieżącym

Jeśli dodajesz lub aktualizowanie zasad dostarczania zawartości, należy usunąć skojarzony locator (jeśli istnieją) i tworzenie nowej lokalizacji.

Ograniczenia w przypadku szyfrowania z Widevine z usługi multimediów Azure: obecnie wielu kluczy zawartości nie są obsługiwane.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Tworzenie środka trwałego i przekazywanie plików do elementu

Aby można było zarządzać, kodowanie i przesyłanie strumieniowe wideo, możesz przekazać zawartość do usługi multimediów Azure firmy Microsoft. Po przesłaniu zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego.

Aby uzyskać szczegółowe informacje zobacz [Przekazywanie plików do konta usługi multimediów](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Kodowanie zawartości z plikiem adaptacyjne ustawione MP4 szybkość transmisji bitów

Dynamiczne szyfrowania będzie potrzebne jest utworzenie składnika aktywów, która zawiera zestaw szybkość transmisji bitów wielokrotne MP4 pliki lub szybkość transmisji bitów wielokrotne płynnego przesyłania strumieniowego źródła. Następnie zgodnie z określonym formatem w wezwaniu manifest i fragment, na żądanie Streaming server zapewni odbierać strumień przez protokół, który został wybrany. W wyniku potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta. Aby uzyskać więcej informacji zobacz temat [Dynamiczny przegląd opakowania](media-services-dynamic-packaging-overview.md) .

Aby uzyskać instrukcje dotyczące kodowania, zobacz [jak do środka trwałego za pomocą Media Encoder Standard kodowania](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Tworzenie zawartości klucza i skojarzyć zakodowany składników majątku

Usługi multimediów klucz zawartości zawiera klucz, który chcesz zaszyfrować środka trwałego z.

Aby uzyskać szczegółowe informacje zobacz [Tworzenie klucz zawartości](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Konfigurowanie zasad autoryzacji klucz zawartości

Usługi multimediów obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza. Zasady zawartości autoryzacji klucza należy skonfigurowany przez Ciebie i spełnione przez klienta (player) klucz do dostarczenia do klienta. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: Otwórz lub token ograniczeń.

Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie zasad autoryzacji klucza zawartości](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Konfigurowanie zasad dostarczania zawartości 

Konfigurowanie zasad dostarczania dla swojej zawartości. Kwestie, które zawiera konfiguracji zasad dostarczania zawartości:

- Adres URL nabycia licencji DRM. 
- Protokół dostarczania zawartości (na przykład MPEG KRESKI, HLS, obr. / min, gładkie Streaming lub wszystkie). 
- Typ dynamiczne szyfrowania (w tym przypadku typowych szyfrowania). 

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

##<a id="example"></a>Przykład


Poniższy przykład przedstawia funkcję, która została wprowadzona w SDK usługi multimediów Azure dla środowiska .net — wersja 3.5.2 (w szczególności możliwość Definiowanie szablonu licencji Widevine i poproś licencji Widevine usługi multimediów Azure). Następujące polecenie Pakiet Nuget został użyty do zainstalowania pakietu:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Tworzenie nowego projektu konsoli.
1. Aby zainstalować i dodać SDK .NET usługi multimediów Azure za pomocą NuGet.
2. Dodaj dodatkowe informacje: System.Configuration.
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

1. Uzyskaj co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji, zobacz: [Konfigurowanie przesyłania strumieniowego punktów końcowych](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Zastąp kod w pliku Plik Program.cs kod wyświetlany w tej sekcji.
    
    Upewnij się zaktualizować zmienne wskaż miejsce, w którym znajdują się pliki wprowadzania folderów.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zobacz też

[CENC z wieloma DRM i kontroli dostępu](media-services-cenc-with-multidrm-access-control.md)

[Konfigurowanie opakowań Widevine z AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Informowanie o Google Widevine licencji dostarczania usług w usługi multimediów Azure](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
