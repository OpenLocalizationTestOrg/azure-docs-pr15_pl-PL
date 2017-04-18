<properties 
    pageTitle="Jak utworzyć procesor multimediów | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć składnik procesora multimediów do kodowania, konwertowanie formatu, szyfrowanie lub odszyfrowywanie zawartości multimedialnej dla usługi multimediów Azure." 
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

##<a name="get-mediaprocessor"></a>Uzyskiwanie MediaProcessor

>[AZURE.NOTE] Praca z interfejsu API usługi REST usługi multimediów, następujące kwestie:
>
>Podczas uzyskiwania dostępu do obiektów w usługach multimediów, należy ustawić określone nagłówek pola i wartości w wezwaniach na HTTP. Aby uzyskać więcej informacji zobacz [Ustawienia dotyczące multimediów usługi REST interfejsu API rozwoju](media-services-rest-how-to-use.md).

>Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą usługi multimediów za pomocą interfejsu API usługi REST](media-services-rest-connect-programmatically.md). 


Poniższe wywołanie pozostałych pokazano, jak uzyskać wystąpienia procesor multimediów według nazwy (w tym przypadku **Media Encoder standardowe**). 



    
Żądanie:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Odpowiedź:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Następne kroki

Teraz, gdy wiesz, jak uzyskać wystąpienia procesor multimediów, przejdź do tematu [jak kodowanie środka trwałego](media-services-rest-get-started.md) , którego procedurach pokazano, jak za pomocą Media Encoder Standard kodowania środka trwałego.
