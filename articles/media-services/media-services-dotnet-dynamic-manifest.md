<properties 
    pageTitle="Tworzenie filtrów za pomocą usługi multimediów Azure .NET SDK" 
    description="W tym temacie opisano sposób tworzenia filtrów w celu klienta można używać ich do poszczególnych sekcji strumienia strumienia. Usługi multimediów tworzy dynamiczne manifesty uzyskanie selektywne streaming." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Tworzenie filtrów za pomocą usługi multimediów Azure .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [POZOSTAŁE](media-services-rest-dynamic-manifest.md)

Począwszy od wersji 2.11, usługi multimediów umożliwia definiowanie filtrów dla aktywów. Filtry te są reguł po stronie serwera, które umożliwią klientów służących do wykonywania operacji, takich jak: odtwarzanie tylko do sekcji klipu wideo (zamiast odtwarzanie całego), lub określ tylko niektóre odwzorowań audio i wideo, obsługujące przez urządzenia klienta (zamiast wszystkich odwzorowań, które są skojarzone z elementu). Filtrowanie to środków trwałych zapewnia **Dynamiczne pojawiają**s utworzonym na żądanie klienta do strumieniowego przesyłania wideo według określonej filtry.

Bardziej szczegółowe informacje związane z filtrów i pojawiają dynamiczne Zobacz [Omówienie manifesty dynamiczne](media-services-dynamic-manifest-overview.md).

W tym temacie przedstawiono sposób multimediów usług .NET SDK umożliwia tworzenie, aktualizowanie i usuwanie filtrów. 


Uwaga Po zaktualizowaniu filtru może potrwać do 2 minut streaming punktu końcowego, aby odświeżyć reguł. Jeśli zawartość została obsługiwane za pomocą tego filtra (i w pamięci podręcznej serwery proxy i CDN pamięci podręcznej), aktualizowanie ten filtr może spowodować błędy odtwarzacza. Jest zalecane, aby wyczyścić pamięć podręczną po zaktualizowaniu filtr. Jeśli ta opcja nie jest możliwe, rozważ użycie inny filtr. 

##<a name="types-used-to-create-filters"></a>Typy używane do tworzenia filtrów

Następujące typy są używane podczas tworzenia filtrów: 

- **IStreamingFilter**.  Ten typ jest oparty na następujących interfejsu API usługi REST [filtru](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Ten typ jest oparty na następujących interfejsu API usługi REST [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Ten typ jest oparty na następujących interfejsu API usługi REST [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** i **IFilterTrackPropertyCondition**. Następujące typy są oparte na następujące interfejsy API pozostałych [FilterTrackSelect i FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Tworzenie/aktualizacja/przeczytane/usuwanie filtry globalne

Poniższy kod przedstawiono sposób .NET umożliwia tworzenie, aktualizowanie, przeczytaj i usuwanie filtrów zasobów.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Filtry Utwórz/Aktualizuj/odczytu/usuwania zawartości

Poniższy kod przedstawiono sposób .NET umożliwia tworzenie, aktualizowanie, przeczytaj i usuwanie filtrów zasobów.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Tworzenie streaming adresy URL, które korzystanie z filtrów

Aby uzyskać informacje na temat publikowania i przeprowadzania aktywów, zobacz [Dostarczania zawartości do przeglądu klientów](media-services-deliver-content-overview.md).


W poniższych przykładach pokazano, jak dodać filtry do przesyłania strumieniowego adresami URL.

**MPEG ŁĄCZNIKA** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live przesyłanie strumieniowe 4 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live przesyłanie strumieniowe V3 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Gładkie strumieniowego przesyłania**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**OBR. / MIN**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zobacz też 

[Omówienie manifesty dynamicznych](media-services-dynamic-manifest-overview.md)
 

