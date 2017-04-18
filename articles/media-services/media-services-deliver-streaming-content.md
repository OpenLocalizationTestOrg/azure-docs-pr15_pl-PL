<properties 
    pageTitle="Publikowanie zawartości usługi multimediów Azure za pomocą programu .NET" 
    description="Dowiedz się, jak utworzyć locator, używaną do utworzenia przesyłanie strumieniowe adresu URL. Przykłady kodu są zapisane w C# i używanie SDK usługi multimediów dla środowiska .NET." 
    authors="juliako" 
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


# <a name="publish-azure-media-services-content-using-net"></a>Publikowanie zawartości usługi multimediów Azure za pomocą programu .NET
 
> [AZURE.SELECTOR]
- [POZOSTAŁE](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Omówienie

Możesz przesyłać strumieniowo adaptacyjne szybkość transmisji bitów MP4 określonych przez tworzenie locator strumieniowych na żądanie oraz budowanie przesyłanie strumieniowe adresu URL. Temat [kodowanie środka trwałego](media-services-encode-asset.md) pokazano, jak do kodowania do adaptacyjne szybkość transmisji bitów ustawione MP4. 

>[AZURE.NOTE]Jeśli zawartość jest zaszyfrowany, skonfiguruj zasady dostarczania zawartości (zgodnie z opisem w [tym](media-services-dotnet-configure-asset-delivery-policy.md) temacie) przed utworzeniem locator. 

Za pomocą na żądanie streaming locator tworzyć adresy URL wskazujące pliki MP4, które mogą być pobierane stopniowo.  

W tym temacie przedstawiono sposób tworzenia na żądanie streaming locator, aby można było opublikować do zawartości i utworzyć gładkie, MPEG ŁĄCZNIKA i HLS streaming adresy URL. Pokazuje także najpopularniejsze, aby utworzyć adresy URL pobierania progresywnego. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Tworzenie na żądanie streaming locator

Aby utworzyć na żądanie, streaming locator i adresy URL, musisz wykonaj następujące czynności:

   1. Jeśli zawartość jest zaszyfrowany, Zdefiniuj zasady dostępu.
   2. Tworzenie na żądanie streaming locator.
   3. Jeśli planujesz strumienia, Uzyskaj przesyłanie strumieniowe pliku manifestu (.ism) w zawartości. 
        
    Jeśli zamierzasz stopniowe pobieranie pobrać nazw plików MP4 w elementu.  
   4. Tworzenie adresy URL do pliku manifestu lub plików MP4. 
   

###<a name="use-media-services-net-sdk"></a>Za pomocą usługi multimediów .NET SDK 

Tworzenie przesyłanie strumieniowe adresy URL 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Wyświetla kod:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Możesz również przesyłać strumieniowo zawartości za pośrednictwem połączenia SSL. Aby to zrobić, upewnij się, że przesyłanie strumieniowe adresami URL początkowych HTTPS. 

Tworzenie stopniowego pobierania adresów URL 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Wyświetla kod:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Należy używać rozszerzeń SDK .NET usługi multimediów

Poniższy kod wywołuje metody rozszerzenia .NET SDK, które tworzą locator i generowanie wygładzonymi Streaming, HLS i adresy URL MPEG-kreska adaptacyjne przesyłania strumieniowego.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Pobierz składniki majątku](media-services-deliver-asset-download.md)
[Konfigurowanie zasad dostarczania zawartości](media-services-dotnet-configure-asset-delivery-policy.md)
