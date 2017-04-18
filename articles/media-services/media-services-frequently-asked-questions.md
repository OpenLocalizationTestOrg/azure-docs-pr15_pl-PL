<properties 
    pageTitle="Często zadawane pytania | Microsoft Azure" 
    description="Często zadawane pytania" 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Często zadawane pytania

##<a name="general-ams-faqs"></a>Często zadawane pytania ogólne AMS

P: jak można skalować, indeksowania?

O: zastrzeżone jednostki są takie same dla zadania kodowanie i indeksowania. Wykonaj instrukcje na [temat kodowanie zastrzeżone skali](media-services-scale-media-processing-overview.md). **Uwaga** Aby wydajność indeksatora nie ma wpływu zastrzeżone typ jednostki.

P: I przekazane, zakodowany i opublikowany klipu wideo. Co to jest przyczyna wideo nie są odtwarzane podczas próby strumienia go?

O: spośród najbardziej typowe przyczyny jest nie masz co najmniej jeden zastrzeżone jednostki strumieniowych na przesyłanie strumieniowe punktu końcowego, z której ma zostać uznany za odtwarzania.  Wykonaj instrukcje na [temat Streaming zastrzeżone skali](media-services-portal-scale-streaming-endpoints.md).

P: Czy zrobić składanie na żywo strumienia?

Odpowiedzi składanie na żywo strumienie obecnie nie ma liście w usługi multimediów Azure, więc należy utworzyć wstępnie na Twoim komputerze.

P: czy mogę korzystać z Live Streaming Azure CDN?

O: usługi multimediów Obsługa integracji z Azure CDN (Aby uzyskać więcej informacji, zobacz [jak zarządzać Streaming punkty końcowe na koncie usługi multimediów](media-services-portal-manage-streaming-endpoints.md)).  Możesz użyć Live streaming z sieci CDN. Usługi multimediów Azure udostępnia wygładzonymi wyjściowe strumieniowych, HLS i MPEG-kreska. Te formaty do przesyłania danych za pomocą protokołu HTTP i uzyskiwanie korzyści wynikających z pamięci podręcznej HTTP. Na żywo streaming rzeczywistych danych audio/wideo jest podzielony na fragmenty i tym poszczególne fragmenty uzyskiwanie przechowywanych w pamięci podręcznej, w sieci CDN. Tylko danych musi zostać odświeżone jest manifestu danych. Sieci CDN okresowo odświeża dane manifestu.

Usługi multimediów Azure czy p: obsługuje przechowywania obrazów?

O: Jeśli szukasz tylko przechowywania obrazów JPEG lub PNG, należy zachować znajdującymi się w magazyn obiektów Blob platformy Azure. Nie ma żadnych korzyści zapisane na koncie usługi multimediów, chyba że chcesz zachować je skojarzone z elementy zawartości Audio lub wideo. Lub jeśli zachodzi potrzeba, aby użyć obrazów jako nakładki w encoder wideo. Media Encoder standardowy obsługuje nakładanie obrazy na bieżąco klipy wideo i to, co zawiera JPEG i PNG jako obsługiwane formaty wprowadzania. Aby uzyskać więcej informacji zobacz [Tworzenie nakładki](media-services-custom-mes-presets-with-dotnet.md#overlay).

P: jak mogę skopiować elementy zawartości z jednego konta usługi multimediów do innego.

O: Aby skopiować elementy zawartości z jednego konta usługi multimediów za pomocą .NET, użyj metody rozszerzenia [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) dostępne w repozytorium [Azure multimediów usługi .NET SDK rozszerzenia](https://github.com/Azure/azure-sdk-for-media-services-extensions/) . Aby uzyskać więcej informacji zobacz [tego](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) wątku forum.

P: co to są obsługiwane znaki nazewnictwa plików podczas pracy z AMS?

O: usługi multimediów używa wartości właściwości IAssetFile.Name podczas tworzenia adresy URL dla strumieniowego przesyłania zawartości (na przykład http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowanie nie jest dozwolone. Wartość właściwości **nazwy** nie mogą zawierać następujące [wartości procentowej kodowanie zastrzeżone znaki](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Ponadto może istnieć tylko jeden "." dla rozszerzenia nazwy pliku.


P: jak nawiązywanie połączenia przy użyciu pozostałych?

O: po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą usługi multimediów za pomocą interfejsu API usługi REST](media-services-rest-connect-programmatically.md). 


P: jak obracanie klipu wideo podczas kodowania.

O: [Media Encoder standardowy](media-services-dotnet-encode-with-media-encoder-standard.md) obsługuje obrotu kątami 90/180/270 stopni. Domyślne zachowanie jest "Automatyczny", gdzie próbie wykrywać metadanych obrotu w importowanego pliku MP4/MOV i wyrównania go. Obejmują następujący element **źródeł** do jednego z json ustawienia wstępne zdefiniowane [poniżej](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
