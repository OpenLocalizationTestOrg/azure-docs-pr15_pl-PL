<properties 
    pageTitle="Publikowanie zawartości usługi multimediów Azure za pomocą usługi REST" 
    description="Dowiedz się, jak utworzyć locator, używaną do utworzenia przesyłanie strumieniowe adresu URL. W kodzie za pomocą interfejsu API usługi REST." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-rest"></a>Publikowanie zawartości usługi multimediów Azure za pomocą usługi REST

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [POZOSTAŁE](media-services-rest-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Omówienie


Możesz przesyłać strumieniowo adaptacyjne szybkość transmisji bitów MP4 określonych przez tworzenie locator strumieniowych na żądanie oraz budowanie przesyłanie strumieniowe adresu URL. Temat [kodowanie środka trwałego](media-services-rest-encode-asset.md) pokazano, jak do kodowania do adaptacyjne szybkość transmisji bitów ustawione MP4. Jeśli zawartość jest zaszyfrowany, skonfiguruj zasady dostarczania zawartości (zgodnie z opisem w [tym](media-services-rest-configure-asset-delivery-policy.md) temacie) przed utworzeniem locator. 

Za pomocą na żądanie streaming locator tworzyć adresy URL wskazujące pliki MP4, które mogą być pobierane stopniowo.  

W tym temacie przedstawiono sposób tworzenia na żądanie streaming locator, aby można było opublikować do zawartości i utworzyć gładkie, MPEG ŁĄCZNIKA i HLS streaming adresy URL. Pokazuje także najpopularniejsze, aby utworzyć adresy URL pobierania progresywnego.

W [poniższej](#types) sekcji przedstawiono typy wyliczenia, którego wartości są używane w pozostałych połączeń.   
  
##<a name="create-an-ondemand-streaming-locator"></a>Tworzenie na żądanie streaming locator

Aby utworzyć na żądanie, streaming locator i adresy URL, musisz wykonaj następujące czynności:


   1. Jeśli zawartość jest zaszyfrowany, Zdefiniuj zasady dostępu.
   2. Tworzenie na żądanie streaming locator.
   3. Jeśli planujesz strumienia, Uzyskaj przesyłanie strumieniowe pliku manifestu (.ism) w zawartości. 
        
    Jeśli zamierzasz stopniowe pobieranie pobrać nazw plików MP4 w elementu. 
   4. Tworzenie adresy URL do pliku manifestu lub plików MP4. 
   5. Należy zauważyć, że nie można utworzyć przesyłanie strumieniowe locator, przy użyciu AccessPolicy, która zawiera Napisz lub usuwanie uprawnień.


###<a name="create-an-access-policy"></a>Tworzenie zasad programu access

Żądanie:
        
    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68
    
    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}
    
Odpowiedź:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

###<a name="create-an-ondemand-streaming-locator"></a>Tworzenie na żądanie streaming locator

Tworzenie locator dla określonej zawartości i zasady zawartości.

Żądanie:
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181
    
    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

Odpowiedź:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"http://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"http://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

###<a name="build-streaming-urls"></a>Tworzenie streaming adresy URL

Użyj wartości z pola **ścieżki** zwracane po utworzeniu wskaźniku utworzyć gładkie, HLS i adresy URL ŁĄCZNIKA MPEG. 

Wygładzonymi Streaming: **Ścieżka** + nazwa pliku manifestu + "/ pojawiają"

przykład:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

HLS: Nazwa pliku **ścieżki** + manifest + "-manifest(format=m3u8-aapl)"

przykład:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


ŁĄCZNIKA: **Ścieżka** + nazwa pliku manifestu + "-manifest(format=mpd-time-csf)"


przykład:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


###<a name="build-progressive-download-urls"></a>Tworzenie stopniowego pobierania adresów URL

Wartość **ścieżki** zwracane po utworzeniu wskaźniku użyć do utworzenia adresu URL pobierania progresywnego.   

Adres URL: **Ścieżka** + trwałego mp4 nazwa pliku

przykład:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

##<a id="types"></a>Typy wyliczenia

    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Konfigurowanie zasad dostarczania zawartości](media-services-rest-configure-asset-delivery-policy.md)
