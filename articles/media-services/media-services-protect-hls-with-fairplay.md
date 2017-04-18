<properties 
    pageTitle="Ochrona usługi HLS zawartości z Apple FairPlay i/lub Microsoft PlayReady | Microsoft Azure" 
    description="W tym temacie przedstawiono omówienie i pokazano, jak za pomocą usługi multimediów Azure dynamicznie szyfrowanie zawartości HTTP Live Streaming (HLS) z FairPlay firmy Apple. Pokazuje także jak używać usługi dostawy licencji usługi multimediów do przeprowadzania FairPlay licencji dla klientów." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Ochrona usługi HLS zawartości z Apple FairPlay i/lub Microsoft PlayReady

Usługi multimediów Azure umożliwia dynamiczne szyfrowanie treści HTTP Live Streaming (HLS) przy użyciu następujących formatów:  

- **Koperta AES-128 wyczyść klucza** 

    Cały fragmentu jest zaszyfrowany przy użyciu trybu **CBC AES-128** . Odszyfrowywanie strumienia jest obsługiwany przez systemów iOS i odtwarzacza OSX oryginalnie. Aby uzyskać więcej informacji zobacz [Ten artykuł](media-services-protect-with-aes128.md).

- **FairPlay firmy Apple** 

    Poszczególne próbki audio i wideo są szyfrowane przy użyciu trybu **CBC AES-128** . **Przesyłanie strumieniowe FairPlay** (K/s) jest zintegrowany z systemów operacyjnych urządzenia z obsługą natywnych na iOS i telewizji firmy Apple. Safari w systemie OS X umożliwia korzystanie z obsługi interfejsu rozszerzenia szyfrowane multimediów (EME) k/s.
- **Microsoft PlayReady**

Poniższa ilustracja przedstawia **HLS + FairPlay i/lub PlayReady szyfrowania dynamiczne** przepływu pracy.

![Ochrona z FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

W tym temacie przedstawiono sposób dynamicznie szyfrowanie zawartości HLS z FairPlay firmy Apple za pomocą usługi multimediów Azure. Pokazuje także jak używać usługi dostawy licencji usługi multimediów do przeprowadzania FairPlay licencji dla klientów.

>[AZURE.NOTE] Jeśli chcesz również zaszyfrować zawartość HLS z PlayReady, należy utworzyć klucz zawartości typowych i skojarzyć z zasobów. Trzeba również skonfigurować zasady autoryzacji klucz zawartości, zgodnie z opisem w temacie [przy użyciu PlayReady dynamiczne typowych szyfrowania](media-services-protect-with-drm.md) .

    
## <a name="requirements-and-considerations"></a>Wymagania i zagadnienia

- Poniżej przedstawiono wymagane podczas używania AMS, do przeprowadzania HLS zaszyfrowanych za pomocą FairPlay i do przeprowadzania FairPlay licencji.

    - Konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Konto usługi multimediów. Aby utworzyć konto usługi multimediów, zobacz [Tworzenie konta](media-services-portal-create-account.md).
    - Utwórz konto w programie [rozwoju firmy Apple](https://developer.apple.com/).
    - Apple wymaga właściciel zawartości w celu uzyskania [pakietu wdrażania](https://developer.apple.com/contact/fps/). Województwo żądania, że już wdrożone KSM (moduł zabezpieczeń klucza) przy użyciu usługi multimediów Azure i żąda pakietu k/s ostateczne. Będzie instrukcje w pakiecie ostateczne k/s do generowania certyfikacji i uzyskać ASK, który będzie używany do konfigurowania FairPlay. 

    - Azure multimediów usług .NET SDK wersji **3.6.0** lub nowszej.

- Następujące czynności należy ustawić na stronie klucza dostarczenia AMS:
    - **Aplikacja certyfikatu (KR)** - plik PFX zawierający klucz prywatny. Ten plik jest utworzony przez klienta i zaszyfrowany przy użyciu hasła przez tego samego klienta. 
        
        Po klienta konfiguruje zasady dostarczania klucza, musisz podać hasło i pfx w formacie base64.

        W poniższej procedurze opisano sposób generowanie certyfikatu pfx do FairPlay.
        
        1. Instalowanie OpenSSL z https://slproweb.com/products/Win32OpenSSL.html
        
            Przejdź do folderu, w których certyfikat FairPlay i innych plików większością firmy Apple.
        
        2. Wiersz polecenia, aby przekonwertować cer pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-informuje der — w fairplay.cer-się fairplay out.pem
        
        3. Wiersz polecenia, aby przekonwertować pem pfx z kluczem prywatnym (hasło do pliku pfx następnie o OpenSSL).
        
            Pkcs12 "C:\OpenSSL-Win32\bin\openssl.exe" — Eksportowanie — się fairplay out.pfx-inkey privatekey.pem — w file:privatekey-pem-pass.txt - passin fairplay out.pem 
        
    - **Hasło certyfikatu aplikacji** — hasło klienta do tworzenia plik pfx.
    - **Identyfikator hasła aplikacji certyfikat** — klienta należy przekazać podobnie jak przekazywanie kluczy AMS i używanie wartość wyliczenia **ContentKeyType.FairPlayPfxPassword** hasła. W wyniku otrzymają identyfikator AMS, który jest muszą używać wewnątrz opcji zasad klucza dostarczenia.
    - **iv** - 16 bajtów losowe wartości, musi odpowiadać iv w zasady dostarczania zawartości. Klient generuje IV i umieszcza go w obu tych miejscach: trwałego dostarczenia zasad i klucza zasad opcja dostarczania. 
    - **ASK** - ASK (klucz tajny aplikacji) jest Odebrano wygenerować certyfikacji za pomocą portalu firmy Apple Deweloper. Każdy do zespołu opracowującego otrzymają unikatowe ASK. Zapisz kopię ZADAJ i zapisać go w bezpiecznym miejscu. Konieczne będzie skonfigurowanie ASK jako FairPlayAsk do usługi multimediów Azure później. 
    -  **Poproś ID** — są uzyskiwane, gdy klient wysyła ASk do AMS. Klienta należy przekazać ASk przy użyciu wartość wyliczenia **ContentKeyType.FairPlayASk** . W wyniku zwracany jest identyfikator AMS i to przeznaczenie, ustawiając opcję zasad klucza dostarczenia.

- Po stronie klienta k/s musi ustawić następujące czynności:
    - **Aplikacja certyfikatu (KR)** -.cer/.der plik zawierający klucz publiczny, używający do szyfrowania niektórych ładunku systemu operacyjnego. AMS należy wiedzieć o go, ponieważ jest to wymagane przez odtwarzacza. Usługi klucza dostawy odszyfrowuje je za pomocą odpowiedni klucz prywatny.

- Do odtwarzania strumienia zaszyfrowanych FairPlay musisz uzyskać rzeczywisty ASK pierwszego, a następnie wygenerować rzeczywistą certyfikatu. Ten proces tworzy wszystkie części 3:

    -  .der, 
    -  PFX i 
    -  hasło PFX.
 
- Klienci, którzy obsługują HLS z szyfrowania **AES 128 CBC** : Safari w systemie OS X, telewizji Apple iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Instrukcje dotyczące konfigurowania FairPlay dynamiczne licencji i szyfrowania dostarczania usług

Poniżej przedstawiono ogólne kroki, które są potrzebne do wykonania, gdy ochrona aktywów z FairPlay za pomocą usługi dostawy licencji usługi multimediów, a także używa szyfrowania dynamiczne.

1. Tworzenie środka trwałego i przekazywanie plików do elementu. 
1. Kodowanie zawartości z plikiem adaptacyjne szybkość transmisji bitów ustawione MP4.
1. Tworzenie zawartości klucza i skojarzyć zakodowany zawartości.  
1. Konfigurowanie zasad autoryzacji klucz zawartości. Podczas tworzenia zasad zawartości autoryzacji klucza, musisz określić następujące ustawienia: 
    
    - Metoda dostarczania (w tym przypadku FairPlay), 
    - Konfiguracja opcji zasad FairPlay. Aby uzyskać szczegółowe informacje dotyczące konfigurowania FairPlay Zobacz metody ConfigureFairPlayPolicyOptions() w przykładzie poniżej.
    
        >[AZURE.NOTE] Zazwyczaj czy chcesz skonfigurować opcje zasad FairPlay tylko raz, ponieważ masz tylko jeden zbiór certyfikacji i ASK.
-ograniczenia (Otwórz lub token) - i informacje dotyczące typ klucza dostawy definiujące sposób klucz został dostarczony do klienta. 
    
2. Konfigurowanie zasad dostarczania zawartości. Konfiguracja zasad dostarczania obejmuje: 

    - Protokół dostarczenia (HLS) 
    - Typ szyfrowania dynamiczne (typowe CBC szyfrowanie) 
    - adres URL nabycia licencji. 
    
    >[AZURE.NOTE]Jeśli chcesz przedstawić strumienia, który jest zaszyfrowany przy użyciu FairPlay + innego DRM, należy skonfigurować zasady osobnych dostarczenia 
    >
    >- Jeden IAssetDeliveryPolicy do konfigurowania ŁĄCZNIKA z CENC (PlayReady + WideVine) i sprawne z PlayReady. 
    >- Inny IAssetDeliveryPolicy, aby skonfigurować FairPlay dla HLS

1. Tworzenie locator na żądanie, aby uzyskać adres URL przesyłanie strumieniowe.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Za pomocą klucza dostarczania FairPlay przez aplikacje odtwarzacza klienta

Klientów można opracowywać aplikacje odtwarzacza przy użyciu zestawu SDK systemu iOS. Aby można było odtwarzać zawartość FairPlay klienci mają do wykonania protokołu exchange licencji. Protokół exchange licencji nie jest określona przez firmy Apple. Jest poszczególnych aplikacjach wysyłania żądania dostarczenia klucza. Usługi dostarczania klucza AMS FairPlay oczekuje SPC do kontaktu jako www-form-url zakodowany ogłoszenie w następującym formacie: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player nie obsługuje odtwarzania FairPlay po ich otrzymaniu. Klienci muszą uzyskać odtwarzacza próbki z konta dewelopera Apple uzyskanie FairPlay odtwarzania w systemie MAC OSX. 
 
##<a name="streaming-urls"></a>Przesyłanie strumieniowe adresy URL

Jeśli do zawartości zaszyfrowanych z więcej niż jeden DRM, należy użyć tag szyfrowania w adresie URL przesyłanie strumieniowe: (format = "aapl m3u8", szyfrowania = 'xxx').

Następujące kwestie:

- Można określić tylko zero lub jeden typ szyfrowania.
- Typ szyfrowania nie ma określonego w adresie url, jeśli tylko jeden szyfrowania została zastosowana do środka.
- Typ szyfrowania jest uwzględniana wielkość liter.
- Można określić następujące typy szyfrowania:  
    - **cenc**: typowe szyfrowanie (Playready lub Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: szyfrowania koperty AES.


##<a name="net-example"></a>Przykład .NET


Poniższy przykład przedstawia funkcję, która została wprowadzona w SDK usługi multimediów Azure dla środowiska .net — wersja 3.6.0 (możliwość używania usługi multimediów Azure do dostarczania zawartości zaszyfrowanych za pomocą FairPlay). Następujące polecenie Pakiet Nuget został użyty do zainstalowania pakietu:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Tworzenie projektu konsoli.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Następne kroki: Ścieżki szkoleniowe dla użytkowników usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
