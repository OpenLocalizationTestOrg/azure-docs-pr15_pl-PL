<properties 
    pageTitle="Jak do środka trwałego za pomocą Media Encoder Standard kodowania | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą Media Encoder Standard kodowania zawartości multimedialnej w usługi multimediów. Przykłady kodu za pomocą interfejsu API usługi REST." 
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


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Jak kodowanie środka trwałego za pomocą Media Encoder standardowy


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [POZOSTAŁE](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Omówienie
W celu przekazywania wideo za pośrednictwem Internetu możesz Kompresuj multimedia. Cyfrowe pliki wideo są bardzo duże i może być zbyt duży, aby dostarczyć przez internet lub dla urządzeń klientów wyświetlić poprawnie. Kodowanie jest proces kompresji wideo i audio, aby klienci mogą wyświetlić multimediów.

Zadania kodowania to jeden z najbardziej typowych operacji w usługach multimediów. Tworzenie zadań kodowania do konwersji plików multimedialnych z jednego kodowania do innego. Podczas kodowania, umożliwia koder wbudowane usługi multimediów (Media Encoder Standard). Umożliwia także kodera dostarczony przez partnera usługi multimediów; kodery innych firm są dostępne za pośrednictwem usługi Azure Marketplace. Możesz określić szczegóły kodowanie zadań przy użyciu wstępnie ustawionych ciągów zdefiniowanych dla swojego encoder lub przy użyciu wstępnie ustawionych konfiguracji plików. Aby wyświetlić typy ustawienia wstępne, które są dostępne, zobacz [Ustawienia zadania Media Encoder standardowy](http://msdn.microsoft.com/library/mt269960).

Każde zadanie może zawierać jedno lub więcej zadań w zależności od typu przetwarzania, którą chcesz wykonać. Za pośrednictwem interfejsu API usługi REST można utworzyć zadań i ich powiązanych zadań w jeden z dwóch sposobów:

- Zadania mogą być zdefiniowane w tekście za pomocą właściwości nawigacji zadania na podmioty zadania lub
- za pośrednictwem przetwarzanie wsadowe OData.


Zalecane jest zawsze kodowanie plików kodery do adaptacyjne szybkość transmisji bitów MP4 Ustaw, a następnie przekonwertowanie zestawu na wybranym formacie przy użyciu [Dynamiczne opakowań](media-services-dynamic-packaging-overview.md). Aby skorzystać z opakowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak skala usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

Jeśli z zasobów danych wyjściowych jest szyfrowane miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Aby uzyskać więcej informacji, zobacz [zasady dostarczania zawartości Konfigurowanie](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Zanim zaczniesz, odwoływanie się do procesorów multimediów, sprawdź, czy masz poprawny nośnik identyfikatora procesor. Aby uzyskać więcej informacji zobacz [Uzyskiwanie procesory multimediów](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Tworzenie zadania z pojedynczego zadania kodowania

>[AZURE.NOTE] Praca z interfejsu API usługi REST usługi multimediów, następujące kwestie:
>
>Podczas uzyskiwania dostępu do obiektów w usługach multimediów, należy ustawić określone nagłówek pola i wartości w wezwaniach na HTTP. Aby uzyskać więcej informacji zobacz [Ustawienia dotyczące multimediów usługi REST interfejsu API rozwoju](media-services-rest-how-to-use.md).

>Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą usługi multimediów za pomocą interfejsu API usługi REST](media-services-rest-connect-programmatically.md).
>
>Gdy za pomocą JSON i określanie, aby użyć słowa kluczowego **__metadata** w wezwaniu na (na przykład, aby odwołuje się do obiektu połączonego) nagłówka **Accept** należy ustawić format [JSON pełne](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Zaakceptuj: aplikacji i json; odata = pełne.

W poniższym przykładzie pokazano, jak tworzyć i publikować zadanie z jednym ustawione zadania do kodowania wideo w określonej rozdzielczości i jakość. Podczas kodowania z Media Encoder standardowy, można użyć ustawienia konfiguracji zadania określone [w tym miejscu](http://msdn.microsoft.com/library/mt269960).

Żądanie:

WPIS https://media.windows.net/API/Jobs typ zawartości HTTP/1.1: aplikacji i json; odata = pełne Zaakceptuj: aplikacji i json; odata = pełne DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 x-ms wersja: 2.11 autoryzacji: okaziciela <token value> 
 x-ms klienta żądanie id: 00000000-0000-0000-0000-000000000000 hosta: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Odpowiedź:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Ustawianie nazwę zasobu wyjścia

W poniższym przykładzie pokazano, jak ustawić atrybut assetName:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Zagadnienia dotyczące

- Aby określić liczbę wejścia lub wyjścia zasoby, które będą używane przez zadanie TaskBody właściwości należy użyć literałów XML. Temat zadania zawiera definicji schematu XML XML.
- W definicji TaskBody każdego wewnętrzne wartość dla <inputAsset> i <outputAsset> musi być ustawiony jako JobInputAsset(value) lub JobOutputAsset(value).
- Zadania może mieć wielu zasobów dane wyjściowe. Jeden JobOutputAsset(x) można używać tylko raz jako wynik zadania w ramach zadania.
- JobInputAsset lub JobOutputAsset można określić jako środka trwałego wprowadzania zadania.
- Zadania nie mogą stanowić cyklu.
- Parametr wartości, które należy przekazać do JobInputAsset lub JobOutputAsset reprezentuje wartość indeksu dla środka trwałego. Zasoby rzeczywiste są definiowane w właściwości nawigacji InputMediaAssets i OutputMediaAssets w definicji jednostki zadania. 
- Ponieważ usługi multimediów jest oparty na protokołu OData v3, pojedyncze środkami InputMediaAssets i OutputMediaAssets kolekcje właściwości nawigacji istnieją odwołania za pośrednictwem "__metadata: identyfikator uri" para wartości z pola Nazwa.
- InputMediaAssets mapowany na jeden lub więcej zasobów, które zostały utworzone w usługach multimediów. OutputMediaAssets są tworzone przez system. Nie odwołują się do istniejącej zawartości.
- Może być nazwany OutputMediaAssets za pomocą atrybutu assetName. Jeśli ten atrybut nie występuje, a następnie nazwa OutputMediaAsset będzie niezależnie od wewnętrzne wartość tekstowa <outputAsset> element jest z sufiksem wartości z pola Nazwa zadania lub wartość Identyfikator zadania (w przypadku, w którym nie jest zdefiniowana właściwość Name). Na przykład jeśli ustawisz wartość assetName "Próby", następnie właściwość OutputMediaAsset Name chcesz ustawić "Próby". Jednak jeśli nie ustawił wartość assetName, ale ustawić nazwę zadania, która "NewJob", nazwą OutputMediaAsset byłaby "_NewJob JobOutputAsset (wartości)". 


##<a name="create-a-job-with-chained-tasks"></a>Utwórz zadanie z zadaniami łańcuchowej

W wielu scenariuszy aplikacji deweloperzy chcesz utworzyć serię przetwarzania zadania. W usługach multimediów możesz utworzyć serii zadań łańcuchowej. Każdego zadania wykonuje przetwarzanie różne kroki i użyć innego nośnika procesorów. Zadania łańcuchowej oddać środka trwałego z jednego zadania do innego, wykonywanie liniowej sekwencji zadań dotyczących elementu. Jednak zadań wykonywanych w ramach zadania nie muszą znajdować się w sekwencji. Po utworzeniu zadania łańcuchowej łańcuchowej obiekty **ITask** są tworzone w jeden obiekt **IJob** .

>[AZURE.NOTE] Jest obecnie limit 30 zadań na zadanie. Jeśli potrzebujesz rowerowy ponad 30 zadań, należy utworzyć więcej niż jednego zadania zawiera zadania.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Zagadnienia dotyczące

Aby włączyć łączenia zadań:

- Zadanie musi mieć co najmniej 2 zadania
- Musi istnieć co najmniej jedno zadanie, w których dane wejściowe są dane wyjściowe innego zadania w zadaniu.

## <a name="use-odata-batch-processing"></a>Przetwarzanie wsadowe OData 

W poniższym przykładzie pokazano, jak za pomocą przetwarzanie wsadowe OData Tworzenie zadania i zadań. Aby uzyskać informacje na przetwarzanie wsadowe Zobacz [Przetwarzanie wsadowe protokołu Open Data (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Utwórz zadanie, używając obiekt JobTemplate


Podczas przetwarzania wielu zasobów przy użyciu zestawu typowych zadań, JobTemplates są przydatne Określanie ustawień domyślnych zadań, zlecenia zadań, i tak dalej.

W poniższym przykładzie pokazano, jak utworzyć obiekt JobTemplate w tekście TaskTemplate zdefiniowane. TaskTemplate używa jako MediaProcessor Media Encoder standardowy do kodowania pliku zawartości; jednak inne MediaProcessors mogą służyć także. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]W przeciwieństwie do innych obiektów usługi multimediów możesz Zdefiniuj nowy identyfikator GUID dla każdego TaskTemplate i umieszczanie go w taskTemplateId i Właściwość Id w swojej treści wezwania. Schemat identyfikacji zawartości, należy wykonać schematu opisanego w identyfikowanie jednostek usługi multimediów Azure. Ponadto nie można zaktualizować JobTemplates. Zamiast tego musisz utworzyć nową z wprowadzonymi zmianami zaktualizowane.
 

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:
    
    HTTP/1.1 201 Created
    
    . . .


W poniższym przykładzie pokazano, jak utworzyć zadanie odwoływanie się do identyfikatora obiekt JobTemplate:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Następne kroki
Teraz, gdy wiesz, jak utworzyć zadanie do kodowania assset, przejdź do tematu [Jak do zaznacz zadanie postęp z usługi multimediów](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Zobacz też

[Uzyskiwanie procesorów multimediów](media-services-rest-get-media-processor.md)
