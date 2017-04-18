<properties 
    pageTitle="Konfigurowanie zasad autoryzacji klucza zawartości przy użyciu zestawu SDK .NET usługi multimediów | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować zasady autoryzacji klucza zawartości przy użyciu zestawu SDK .NET usługi multimediów." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamiczne szyfrowania: Konfigurowanie zasad zawartości autoryzacji klucza

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Omówienie

Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie strumienie MPEG-kreska, gładkie Streaming i HTTP-Live — Streaming (HLS) chronione za pomocą szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego) lub [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS umożliwia przedstawianie strumienie ŁĄCZNIKA zaszyfrowanych za pomocą Widevine DRM. Zarówno PlayReady i Widevine są szyfrowane na specyfikacji typowych szyfrowania (ISO/IEC CENC 23001-7).

Usługi multimediów udostępnia również **Klucza-licencji usługi** , z którego klienci mogą uzyskiwać PlayReady-Widevine licencji do odtwarzania zawartości zaszyfrowane lub kluczy AES.

Usługi multimediów do szyfrowania środka trwałego, należy skojarzyć klucza szyfrowania (**CommonEncryption** lub **EnvelopeEncryption**) zawartości (w sposób opisany [poniżej](media-services-dotnet-create-contentkey.md)), a także skonfigurować zasady autoryzacji klucza (zgodnie z opisem w tym artykule).

W przypadku zażądania strumienia przez odtwarzacz, usługi multimediów używa określonego klucza dynamicznie szyfrowanie treści przy użyciu szyfrowania AES lub DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi klucza dostarczenia. Określanie, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasady autoryzacji, które określone dla klucza.

Usługi multimediów obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: **Otwórz** lub **token** ograniczeń. Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w **Prostej tokeny sieci Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) a formatem **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)).

Usługi multimediów nie umożliwia zabezpieczanie usług tokenu. Można tworzyć niestandardowe STS lub korzystać z usługi Microsoft Azure ACS do tokenów problem. Usługi STS musi być skonfigurowany do tworzenia tokenu zalogowani za pomocą określonego oświadczeniach problem i klucza, określone w konfiguracji token ograniczeń (zgodnie z opisem w tym artykule). Dostarczenie klucza usługi multimediów zwróci klucza szyfrowania do klienta, jeśli tokenu jest ważna, a oświadczeniach tokenu pasują do zawartości klucza.

Aby uzyskać więcej informacji zobacz

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integracja MVC OWIN usługi multimediów Azure podstawie aplikacji z usługą Azure Active Directory, a także ograniczać dostarczania zawartości klucza oparte na oświadczeniach JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Użyj Azure ACS do tokenów problem](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Niektóre kwestie:

- Aby można było używać dynamiczne opakowań i szyfrowania dynamiczne, należy się upewnić mieć co najmniej jeden streaming zastrzeżone jednostek. Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).
- Do zawartości musi zawierać zestaw adaptacyjne szybkość transmisji bitów MP4s lub adaptacyjne szybkość transmisji bitów wygładzonymi strumieniowego przesyłania plików. Aby uzyskać więcej informacji zobacz [kodowanie środka trwałego](media-services-encode-asset.md).
- Przekazywanie i kodowanie aktywów za pomocą opcji **AssetCreationOptions.StorageEncrypted** .
- Jeśli planujesz wielu kluczy zawartości, które wymagają tej samej konfiguracji zasad, zdecydowanie zalecane jest tworzenie zasad jednego pozwolenia i użyć go ponownie z wielu kluczy zawartości.
- Usługa dostarczania klucza buforowanie ContentKeyAuthorizationPolicy i jej powiązanych obiektów (opcje zasad i ograniczeń) na 15 minut.  Jeśli tworzysz ContentKeyAuthorizationPolicy i określ umożliwia ograniczenie "Tokenu", przetestowanie i następnie zaktualizować zasady ograniczeń "Otwórz", trwa około 15 minut, zanim zasady przełączy się do wersji "Otwarte" zasady.
- Jeśli Dodaj lub zaktualizuj zasady dostarczania składnika aktywów, należy usunąć istniejące locator (jeśli istnieją) i tworzenie nowej lokalizacji.
- Obecnie nie możesz zaszyfrować obr. / min streaming format lub stopniowe do pobrania.


##<a name="aes-128-dynamic-encryption"></a>Dynamiczne szyfrowania AES 128

###<a name="open-restriction"></a>Otwieranie ograniczeń

Otwieranie ograniczeń oznacza, że system będzie przedstawiana klucz dla każdego, kto wykonuje żądanie klucza. To ograniczenie może być przydatne podczas testowania.

Poniższy przykład tworzy zasady otwartego pozwolenia i dodaje ją do zawartości klucza.

statyczne AddOpenAuthorizationPolicy void publicznej (IContentKey contentKey) {/ / tworzenie ContentKeyAuthorizationPolicy z ograniczeniami Otwórz / / i tworzenie zasad autoryzacji zasad IContentKeyAuthorizationPolicy = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("zasady otwartego pozwolenia"). Wynik;

Lista<ContentKeyAuthorizationPolicyRestriction> ograniczenia = Nowa lista<ContentKeyAuthorizationPolicyRestriction>();
    
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


###<a name="token-restriction"></a>Ograniczenie tokenu

W tej sekcji opisano, jak utworzyć zasady zawartości autoryzacji klucza i skojarzyć klucz zawartości. Zasady autoryzacji opisano, jakie wymagania autoryzacji muszą być spełnione do określenia, jeśli użytkownik jest autoryzowany do odbierania klucz (na przykład, czy na liście "Weryfikacja klucz" zawiera klucza, podpisaną tokenu z).

Aby skonfigurować opcję token ograniczeń, należy używać danych XML opisujący tokenu określonych wymagań. Konfiguracja tokenu ograniczeń XML muszą być zgodne z następującego schematu XML.

####<a id="schema"></a>Ograniczenie token schematu
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Podczas konfigurowania **token** ograniczony zasad, musisz określić podstawowy parametry** klucza weryfikacji**, **wystawcy** i **odbiorców** . **Klucz podstawowy weryfikacji **zawiera klucz podpisaną tokenu z, **wystawcy** jest usługę tokenu bezpiecznego problemy tokenu. **Grupy odbiorców** (nazywane **zakresu**) w tym artykule opisano opcje tokenu lub zasób tokenu zezwala na dostęp do. Dostarczenie klucza usługi multimediów sprawdza, czy te wartości tokenu zgodne z wartościami w szablonie. 

Przy użyciu **Zestawu SDK usługi multimediów dla środowiska .NET**, możesz korzystać klasy **TokenRestrictionTemplate** do wygenerowania tokenu ograniczeń.
Poniższy przykład tworzy zasad autoryzacji z tokenu ograniczeń. W tym przykładzie woli klienta do prezentowania tokenu, który zawiera: logowanie klucza (VerificationKey), wystawcy tokenów i żądanych oświadczeń.
    
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

####<a id="test"></a>Test tokenu

Aby uzyskać tokenu test oparte na to ograniczenie tokenu użytego zasady autoryzacji klucza, wykonaj następujące czynności.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dynamiczne PlayReady szyfrowania 

Usługi multimediów umożliwia konfigurowanie prawa i ograniczenia, które chcesz Runtime PlayReady DRM wymusić, gdy użytkownik chce się odtwarzanie zawartości chronionej. 

Gdy Ochrona zawartości z PlayReady, jedną z rzeczy, które należy określić w zasad autoryzacji jest ciąg XML, który definiuje [PlayReady licencji szablonu](media-services-playready-license-template-overview.md). W SDK usługi multimediów dla środowiska .NET klas **PlayReadyLicenseResponseTemplate** i **PlayReadyLicenseTemplate** pomoże Ci Definiowanie szablonu licencji PlayReady.

[W tym temacie](media-services-protect-with-drm.md) przedstawiono sposób szyfrowanie zawartości z **PlayReady** i **Widevine**.

###<a name="open-restriction"></a>Otwieranie ograniczeń
    
Otwieranie ograniczeń oznacza, że system będzie przedstawiana klucz dla każdego, kto wykonuje żądanie klucza. To ograniczenie może być przydatne podczas testowania.

Poniższy przykład tworzy zasady otwartego pozwolenia i dodaje ją do zawartości klucza.

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
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Ograniczenie tokenu

Aby skonfigurować opcję token ograniczeń, należy używać danych XML opisujący tokenu określonych wymagań. Konfiguracja tokenu ograniczeń XML musi odpowiadać schematu XML wyświetlane w [tej](#schema) sekcji.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
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


Test tokenu oparte na to ograniczenie tokenu użytego do klucza autoryzacji zasad można znaleźć [w tej](#test) sekcji. 

##<a id="types"></a>Typy używane podczas definiowania ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Następny krok
Teraz, gdy skonfigurowano zasady autoryzacji klucz zawartości, przejdź do tematu [jak skonfigurować zasady dostarczania zawartości](media-services-dotnet-configure-asset-delivery-policy.md) .
 
