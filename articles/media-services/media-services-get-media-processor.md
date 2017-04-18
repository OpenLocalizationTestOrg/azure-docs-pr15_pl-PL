<properties 
    pageTitle="Jak utworzyć procesor multimediów | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć składnik procesora multimediów do kodowania, konwertowanie formatu, szyfrowanie lub odszyfrowywanie zawartości multimedialnej dla usługi multimediów Azure. Przykłady kodu są zapisane w C# i używanie SDK usługi multimediów dla środowiska .NET." 
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


#<a name="how-to-get-a-media-processor-instance"></a>Jak: pobieranie wystąpienia procesor multimediów

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [POZOSTAŁE](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Omówienie

W usługach multimediów, procesor multimediów jest składnik, który obsługuje przetwarzanie określonego zadania, na przykład kodowanie formatowanie konwersji, ponieważ do szyfrowania lub odszyfrowywania zawartości multimedialnej. Podczas tworzenia zadania kodowanie, szyfrowanie lub konwertować formatu zawartości multimedialnej przeważnie tworzą procesor multimediów.

Poniższa tabela zawiera nazwę i opis poszczególnych procesorów dostępnych multimediów.

Nazwa procesora multimediów|Opis|Więcej informacji
---|---|---
Media Encoder standardowe|Zapewnia możliwości standard kodowania na żądanie. |[Omówienie i porównanie Azure na żądanie kodery multimediów](media-services-encode-asset.md)
Media Encoder Premium w przepływu pracy|Umożliwia uruchamianie kodowania zadań przy użyciu przepływu pracy programu Media Encoder Premium.|[Omówienie i porównanie Azure na żądanie kodery multimediów](media-services-encode-asset.md)
Indeksowanie multimediów Azure| Umożliwia udostępnianie plików multimedialnych i treści można wyszukiwać, a także do generowania ścieżki podpisów kodowanych i słów kluczowych.|[Indeksowanie multimediów Azure](media-services-index-content.md)
Azure Hyperlapse multimediów (wersja preview)|Umożliwia wygładzanie "nierówności" w pliku wideo z stabilizacji wideo. Umożliwia także przyspieszyć zawartości do klipu eksploatacyjnych.|[Hyperlapse multimediów Azure](media-services-hyperlapse-content.md)
Azure Media Encoder|Amortyzacji
Odszyfrowywanie miejsca do magazynowania| Amortyzacji|
Pakowarki multimediów Azure|Amortyzacji|
Szyfrujący multimediów Azure|Amortyzacji|

##<a name="get-media-processor"></a>Uzyskiwanie procesor multimediów

Poniższej metody pokazano, jak uzyskać wystąpienia procesor multimediów. W przykładzie kodu założono, użyj zmiennej poziomie modułu o nazwie **_context** zostać utworzone odwołanie kontekstu serwera, zgodnie z opisem w sekcji [jak: nawiązywanie połączenia z programowy usługi multimediów](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Następne kroki

Teraz, gdy wiesz, jak uzyskać wystąpienia procesor multimediów, przejdź do tematu [jak kodowanie środka trwałego](media-services-dotnet-encode-with-media-encoder-standard.md) , którego procedurach pokazano, jak za pomocą Media Encoder Standard kodowania środka trwałego.


