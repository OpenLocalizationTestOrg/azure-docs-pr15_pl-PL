<properties 
    pageTitle="Dostarczanie Widevine licencji usługi multimediów Azure za pomocą Axinom | Microsoft Azure" 
    description="W tym artykule opisano, jak usługi multimediów Azure (AMS) umożliwia przedstawianie strumień dynamicznie są szyfrowane za pomocą AMS PlayReady i Widevine DRMs. Licencja PlayReady pochodzi z serwera licencji PlayReady usługi multimediów i licencji Widevine został dostarczony przez serwer licencji Axinom." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Dostarczanie Widevine licencji usługi multimediów Azure za pomocą Axinom  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Omówienie

Usługi multimediów Azure (AMS) został dodany Google Widevine dynamiczną ochronę (zobacz [Mingfei w blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) , aby uzyskać szczegółowe informacje). Ponadto Azure Media Player (AMP) dodano również pomocy technicznej Widevine (zobacz [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) , aby uzyskać szczegółowe informacje). W przeglądarkach nowoczesny wyposażony MSE i EME jest głównych wykonywaniu w strumieniowego przesyłania zawartości ŁĄCZNIKA chroniony przez CENC z wielu-native-DRM (PlayReady i Widevine).

Począwszy od multimediów usług .NET SDK wersji 3.5.2 usługi multimediów umożliwia konfigurowanie szablonu licencji Widevine i się Widevine licencji. Umożliwia także następujące partnerów AMS ułatwiające przeprowadzania licencji Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

W tym artykule opisano, jak zintegrować i przetestować serwera licencji Widevine zarządzane przez Axinom. W szczególności obejmuje:  

- Konfigurowanie dynamicznego typowych szyfrowania przy użyciu wielu DRM (PlayReady i Widevine) przy użyciu odpowiedniego adresu URL nabycia licencji;
- Generowanie tokenu JWT w celu spełnienia wymagań serwera licencji;
- Opracowywania aplikacji Azure Media Player, który obsługuje pobieranie licencji z uwierzytelnianiem token JWT;

Kompletny układ i przepływ zawartości, którą najważniejsze i klucza identyfikator klucza nasion, JTW token i jej roszczeń może być najlepiej opisywany przez Poniższy diagram.

![KRESKA i CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Ochrona zawartości

Dotyczące konfigurowania dynamiczną ochronę i dostarczania kluczowych zasad, zobacz Mingfei w blogu: [jak skonfigurować opakowań Widevine z usługi multimediów Azure](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Możesz skonfigurować dynamiczną ochronę CENC z wieloma DRM dla ŁĄCZNIKA streaming posiadający obie z następujących czynności:

1. Ochrona PlayReady MS Edge i IE11, zawierających ograniczenia tokenu autoryzacji. Token zasady ograniczeniami muszą być dołączone token wystawiony przez bezpiecznego tokenu usługi (STS), takich jak usługi Azure Active Directory;
1. Ochrony Widevine Chrome, go wymaga uwierzytelniania tokenu z token wystawiony przez innego STS. 

Zobacz [Generowanie tokenu JWT](media-services-axinom-integration.md#jwt-token-generation) dlaczego usługi Azure Active Directory nie można użyć jako STS serwera licencji Widevine Axinom w sekcji.

###<a name="considerations"></a>Zagadnienia dotyczące

1. Należy użyć Axinom określonego klucza nasion (8888000000000000000000000000000000000000) i usługi wygenerowane lub wybranych identyfikator do wygenerowania zawartości klucza konfigurowania usługi dostarczania klucza klucza. Serwer licencji Axinom będą wydawać wszystkie licencje zawierające zawartości klawiszy oparte na tym samym nasion klucza nadaje się do testowania i produkcji.
1. Adres URL nabycia licencji Widevine badań: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP i HTTS są dozwolone.

##<a name="azure-media-player-preparation"></a>Przygotowanie odtwarzacza multimediów Azure

AMP v1.4.0 obsługuje odtwarzanie zawartości AMS, który jest dynamicznie spakowany PlayReady i Widevine DRM.
Jeśli serwer licencji Widevine nie wymaga uwierzytelniania token, nie ma dodatkowe, należy wykonać, aby przetestować zawartość ŁĄCZNIKA chroniony przez Widevine. Na przykład zespołu AMP udostępnia proste [próbki](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), gdzie można to sprawdzić pracy w krawędź i IE11 z PlayReady i Chrome z Widevine.
Serwer licencji Widevine dostarczony przez Axinom wymaga uwierzytelniania tokenu JWT. JWT token musi przesłany z żądanie licencji do nagłówka HTTP "X-AxDRM-komunikat". W tym celu należy dodać następujące javascript na stronie sieci web obsługującego AMP przed ustawieniem źródła:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Pozostałe kod AMP jest standardowy API AMP tak jak w dokumencie AMP [tutaj](http://amp.azure.net/libs/amp/latest/docs/).

Należy zauważyć, że powyżej kodu javascript w nagłówku autoryzacji niestandardowe ustawienie nadal podejście krótkim przed dzienniku wydania podejście długoterminową w AMP.

##<a name="jwt-token-generation"></a>Generowanie Token JWT

Axinom Widevine serwera licencji do testowania wymaga uwierzytelniania tokenu JWT. Ponadto jedną roszczeń tokenu JWT jest typu obiektu złożonych zamiast typ danych pierwotnych.

Niestety Azure AD może wydawać tokeny JWT z typów pierwotnych. Podobnie .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler i JwtPayload) tylko umożliwia wprowadzenie typu obiektu złożonych jako roszczeń. Jednak roszczeń są nadal szeregować jako ciąg. W związku z tym firma Microsoft nie korzysta z tych dwóch do wygenerowania tokenu JWT żądania licencji Widevine.


Jan Sheehan [JWT Nuget pakiet](https://www.nuget.org/packages/JWT) spełnia wymagania, więc możemy będzie używany ten pakiet Nuget.

Poniżej przedstawiono kod generowania tokenu JWT potrzebne roszczeń zgodnie z wymaganiami serwera licencji Axinom Widevine do testowania:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine serwera licencji

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Zagadnienia dotyczące

1.  Mimo że wymaga usługi dostarczania licencji AMS PlayReady "okaziciela =" poprzedni token uwierzytelniania, serwera licencji Axinom Widevine go nie używał.
2.  Klucz komunikacji Axinom jest używany jako klucz podpisywania. Należy zauważyć, że klucz ciąg szesnastkowy, jednak go muszą być traktowane jako seria bajtów nie ciągu podczas kodowania. Jest to osiągnąć przy użyciu metody ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Pobieranie Identyfikatora klucza

Można zauważyć, że w kodzie do wygenerowania JWT identyfikator tokenu, klucza jest wymagane. Ponieważ kluczowe JWT token musi znajdować się gotowy, zanim odtwarzacza ładowania AMP identyfikator potrzebuje mają być pobierane w celu wygenerowania tokenu JWT.

Oczywiście, który istnieje wiele sposobów uzyskiwanie przytrzymaj klucza identyfikator. Na przykład jeden może przechowywać identyfikator klucza razem z metadanych zawartości w bazie danych. Lub można pobrać identyfikator z pliku MPD ŁĄCZNIKA (opis prezentacji multimediów) klucza. Poniższy kod jest dla niego.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Podsumowanie

Przy użyciu najnowszych dodanie obsługi Widevine zarówno ochrony zawartości usługi multimediów Azure i Azure Media Player możemy zaimplementować streaming ŁĄCZNIKA + wielu-native-DRM (PlayReady + Widevine) z obu PlayReady usługą licencji w AMS i Widevine serwera licencji z Axinom następujących przeglądarkach Nowoczesny:

- Chrome
- Microsoft Edge w systemie Windows 10
- IE 11 w systemie Windows 8.1 i Windows 10
- Zarówno (wersja klasyczna) programu Firefox i Safari dla komputerów Mac (nie iOS) są również obsługiwane za pomocą programu Silverlight i tego samego adresu URL z Azure Media Player

Następujące parametry są wymagane w minimalnej rozwiązanie wykorzystując Axinom Widevine serwera licencji. Z wyjątkiem klucza ID, pozostałe parametry są dostarczane przez Axinom na podstawie ich konfiguracji serwera Widevine.


Parametr|Jak są one używane
---|---
Identyfikator klucza komunikacji|Musi być dołączone jako wartość roszczeń "com_key_id" tokenu JWT (zobacz [w tej](media-services-axinom-integration.md#jwt-token-generation) sekcji).
Klucz komunikacji|Musi być używany jako klucz podpisywania tokenu JWT (zobacz [w tej](media-services-axinom-integration.md#jwt-token-generation) sekcji).
Nasion klucza|Musi zostać użyty do wygenerowania klucz zawartości z zawartość danego Identyfikatora klucza (zobacz [w tej](media-services-axinom-integration.md#content-protection) sekcji).
Adres URL nabycia licencji Widevine|Należy używać podczas konfigurowania zasad dostarczania zawartości dla ŁĄCZNIKA streaming (zobacz [Ta](media-services-axinom-integration.md#content-protection) sekcja).
Identyfikator klucza zawartości|Musi być częścią wartości zastrzeżenia prawa wiadomości JWT token (zobacz [w tej](media-services-axinom-integration.md#jwt-token-generation) sekcji). 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potwierdzenia 

Chcemy Potwierdź następujące osoby, które przyczynić się do tworzenia ten dokument: Kristjan Jõgi z Axinom, Mingfei Yan i Rajput Amitowi.
