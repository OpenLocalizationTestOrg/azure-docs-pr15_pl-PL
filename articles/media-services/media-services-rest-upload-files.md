<properties 
    pageTitle="Przekazywanie plików do konta usługi multimediów za pomocą usługi REST | Microsoft Azure" 
    description="Dowiedz się, jak pobrać zawartość multimedialną do usługi multimediów przez tworzenie i przekazywanie aktywów." 
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


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Przekazywanie plików do konta usługi multimediów za pomocą usługi REST

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [POZOSTAŁE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Usługi multimediów możesz przekazać pliki cyfrowy do środka trwałego. Jednostki [zasobów](https://msdn.microsoft.com/library/azure/hh974277.aspx) może zawierać klip wideo, audio, obrazy, miniatur zbiorów, tekst ścieżek i napisy plików (i metadanych dotyczących tych plików.)  Gdy pliki są przekazywane do elementu, zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego. 

>[AZURE.NOTE]Następujące kwestie podczas wybierania nazwy pliku zawartości:
>
>- Usługi multimediów używa wartości właściwości IAssetFile.Name podczas tworzenia adresy URL dla strumieniowego przesyłania zawartości (na przykład http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowanie nie jest dozwolone. Wartość właściwości **nazwy** nie mogą zawierać następujące [wartości procentowej kodowanie zastrzeżone znaki](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Ponadto może istnieć tylko jeden ".' dla rozszerzenia nazwy pliku.
>
>- Długość nazwy nie powinna być większa niż 260 znaków.

Podstawowe przepływu pracy dla przekazywania elementów jest podzielony na następujące sekcje:

- Tworzenie środka trwałego
- Szyfrowanie środka trwałego (opcjonalnie)
- Przekazywanie pliku z magazynem obiektów blob

AMS umożliwia przekazywanie składniki majątku zbiorczo. Aby uzyskać więcej informacji zobacz [w tej](media-services-rest-upload-files.md#upload_in_bulk) sekcji.

##<a name="upload-assets"></a>Przekazywanie składników majątku

###<a name="create-an-asset"></a>Tworzenie środka trwałego

>[AZURE.NOTE] Podczas pracy z interfejsu API usługi REST usługi multimediów należy uwzględnić następujące kwestie:
>
>Podczas uzyskiwania dostępu do obiektów w usługach multimediów, należy ustawić określone nagłówek pola i wartości w wezwaniach na HTTP. Aby uzyskać więcej informacji zobacz [Ustawienia dotyczące rozwoju multimediów usługi REST interfejsu API](media-services-rest-how-to-use.md).

>Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą za pomocą interfejsu API usługi REST usługi multimediów](media-services-rest-connect-programmatically.md). 
 
Zasób jest kontenerem dla wielu typów lub zestawy obiektów w usługach multimediów, takich jak klipy wideo, audio, obrazy, miniatur zbiorów, ścieżek tekstowych i napisy plików. W interfejsie API usługi REST Tworzenie środka trwałego wymaga wysłanie żądania WPIS usługi multimediów i wprowadzania wszelkie informacje o swojej zawartości w treści wezwania.

Jedną z właściwości, które można określić podczas tworzenia środka trwałego jest **Opcje**. **Opcje** jest wartością wyliczenia, który opisuje opcje szyfrowania, które mogą być tworzone środka trwałego za. Prawidłową wartość jest jedną z wartości z listy poniżej, a nie kombinacja wartości. 

- **Brak** = **0**: brak szyfrowania będzie używany. Jest to wartość domyślna. Należy zauważyć, że podczas korzystania z tej opcji zawartości nie jest chroniony podczas przesyłania lub spoczywa w magazynie.
    Jeśli planujesz do przeprowadzania MP4 za pomocą stopniowego pobierania, użyj tej opcji. 

- **StorageEncrypted** = **1**: Określ, czy chcesz się być szyfrowane za pomocą szyfrowania bitowego AES-256 przekazywania i przechowywania plików.

    Jeśli do zawartości jest szyfrowane miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Aby uzyskać więcej informacji, zobacz [zasady dostarczania zawartości Konfigurowanie](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: Określ, jeśli wysyłasz pliki chronione za pomocą wspólnej metody szyfrowania (na przykład PlayReady). 

- **EnvelopeEncryptionProtected** = **4**: Określ, jeśli wysyłasz HLS szyfrowane z plikami AES. Należy zauważyć, że pliki musi zostały zakodowany i szyfrowane przez Menedżera Przekształcanie.

>[AZURE.NOTE]Jeśli do zawartości będzie używać szyfrowania, należy utworzyć **ContentKey** i połączyć go z zawartości zgodnie z opisem w temacie:[jak utworzyć ContentKey](media-services-rest-create-contentkey.md). Należy zauważyć, że po przekazaniu pliku do elementu, należy zaktualizować właściwości szyfrowania w jednostce **AssetFile** z wartościami, które masz podczas szyfrowania **zawartości** . To zrobić za pomocą żądania HTTP **korespondencji seryjnej** . 


W poniższym przykładzie pokazano, jak utworzyć środka trwałego.

**Żądanie HTTP**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Tworzenie AssetFile

Jednostka [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) reprezentuje pliku audio lub wideo, który jest przechowywany w kontenerze obiektów blob. Plik zawartości są zawsze skojarzone z środka trwałego i środka trwałego może zawierać jednego lub wielu plików zawartości. Zadanie Media Encoder usług nie powiedzie się, czy obiekt plik zawartości nie jest skojarzone z plikiem cyfrowego w kontenerze obiektów blob.

Zauważ, że wystąpienie **AssetFile** oraz plik multimedialny rzeczywiste są dwa różne obiekty. Wystąpienia AssetFile zawiera metadanych dotyczących plików multimedialnych, gdy plik multimedialny z zawartością rzeczywiste.

Po wysłaniu do plików multimedialnych do kontenera obiektów blob użyje żądania HTTP **SCALANIE** zaktualizować AssetFile z informacjami o pliku multimediów (jak pokazano w dalszej części tematu). 

**Żądanie HTTP**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpowiedź HTTP**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Tworzenie AccessPolicy z uprawnieniami do zapisu. 

Przed przekazaniem wszystkie pliki do magazyn obiektów blob, Ustaw dostęp prawa zasad pomocne przy tworzeniu do środka trwałego. Aby to zrobić, PUBLIKOWAĆ żądania HTTP zestaw jednostek AccessPolicies. Definiowanie wartości DurationInMinutes po utworzeniu lub otrzymasz komunikat o błędzie 500 wewnętrznego serwera w odpowiedzi. Aby uzyskać więcej informacji o AccessPolicies zobacz [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

W poniższym przykładzie pokazano, jak utworzyć AccessPolicy:
        
**Żądanie HTTP**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Żądanie HTTP**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Uzyskać adres URL przekazywania

Aby uzyskać adres URL przekazywania rzeczywiste, Utwórz Locator skojarzeń zabezpieczeń. Locator określić czas rozpoczęcia i typ punktu końcowego połączenia dla klientów, którzy mają dostęp do plików w środka trwałego. Możesz utworzyć wiele jednostek Locator dla danej pary AccessPolicy i środków trwałych do obsługi żądania innego klienta i wymagań. Wartość czas rozpoczęcia oraz wartość DurationInMinutes AccessPolicy każdej z tych Locator służą do określania czas, można użyć adresu URL. Aby uzyskać więcej informacji zobacz [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Adres URL skojarzeń zabezpieczeń ma następujący format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Niektóre kwestie:

- Nie może mieć więcej niż pięciu unikatowych Locator skojarzone z danego składnika majątku w tym samym czasie. Aby uzyskać więcej informacji zobacz Locator.
- Jeśli chcesz od razu Przekaż pliki, należy ustawić wartość czas rozpoczęcia do pięciu minut przed bieżącą godzinę. To może być zegar skośność między usługami multimediów i komputer kliencki. Ponadto wartość czas rozpoczęcia musi być w następującym formacie daty i godziny: YYYY-MM-DDTHH:mm:ssZ (na przykład "2014-05-23T17:53:50Z").   
- Czasami może występować na sekundę 30-40 opóźnienia po utworzeniu Locator, gdy jest gotowy do użycia. Ten problem dotyczy do adresu URL skojarzeń zabezpieczeń i Locator pochodzenia.

W poniższym przykładzie pokazano sposób tworzenia skojarzenia zabezpieczeń Locator adresu URL, zdefiniowane przez właściwości Typ w treści wezwania ("1" dla locator skojarzenia zabezpieczeń) i "2" dla locator origin na żądanie. Właściwość **Path** zwracany zawiera adres URL, który należy użyć, aby przekazać plik.
    
**Żądanie HTTP**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Przekazywanie pliku do kontenera magazyn obiektów blob
    
Po umieszczeniu AccessPolicy, a także ustawić rzeczywisty plik jest przekazywany do kontenera magazyn obiektów Blob platformy Azure za pomocą interfejsów API pozostałych miejsca do magazynowania Azure. Możesz przekazać na stronie lub zablokować obiektów blob. 

>[AZURE.NOTE] Należy dodać nazwę pliku dla pliku, który chcesz przekazać wartość Locator **ścieżka** otrzymanych w poprzedniej sekcji. Na przykład https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Aby uzyskać więcej informacji na temat pracy z obiektami blob Azure miejsca do magazynowania zobacz [Interfejsu API usługi REST usługi obiektów Blob](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Aktualizacja AssetFile 

Teraz, gdy przekazane przez Ciebie pliku, zaktualizuj informacje o FileAsset rozmiar (i inne). Na przykład:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpowiedź HTTP**

Jeśli powodzenia następujące jest zwracana: HTTP/1.1 204 Brak zawartości

### <a name="delete-the-locator-and-accesspolicy"></a>Usuwanie Locator i AccessPolicy 

**Żądanie HTTP**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:

    HTTP/1.1 204 No Content 
    ...

**Żądanie HTTP**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Przekazywanie środkami zbiorcze

###<a name="create-the-ingestmanifest"></a>Tworzenie IngestManifest

IngestManifest jest kontenerem zestawu aktywów, pliki zawartości oraz statystykę, który może być używany do określenia postępu zbiorcze ingesting dla zestawu.


**Żądanie HTTP**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Tworzenie składników majątku

Przed utworzeniem IngestManifestAsset, należy utworzyć zawartości, która zostanie wykonana przy użyciu ingesting zbiorczej. Zasób jest kontenerem dla wielu typów lub zestawy obiektów w usługach multimediów, takich jak klipy wideo, audio, obrazy, miniatur zbiorów, ścieżek tekstowych i napisy plików. W interfejsie API usługi REST Tworzenie środka trwałego wymaga wysłanie żądania HTTP POST usługi multimediów Azure firmy Microsoft i wprowadzania wszelkie informacje o swojej zawartości w treści wezwania. W tym przykładzie jest tworzona środka trwałego za pomocą opcji StorageEncrption(1) dołączone do treści żądania.

**Odpowiedź HTTP**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Tworzenie IngestManifestAssets

IngestManifestAssets reprezentują aktywów w IngestManifest, które są używane z ingesting zbiorczo. Zasadniczo łączenie elementu z manifestu. Usługi multimediów Azure wewnętrznie oczekuje przekazywania pliku według zbioru IngestManifestFiles związane z IngestManifestAsset. Po przesłaniu tych plików są elementu zostanie zakończone. Możesz utworzyć nowy IngestManifestAsset z żądaniem HTTP POST. W treści wezwania obejmują identyfikator IngestManifest i IngestManifestAsset należy połączyć ze sobą ingesting zbiorcze identyfikator zasobów.

**Odpowiedź HTTP**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Tworzenie IngestManifestFiles dla poszczególnych elementów zawartości

IngestManifestFile reprezentuje obiekt rzeczywisty obiektów blob audio lub wideo, który zostanie przekazany jako część zbiorczej ingesting dla środka trwałego. Właściwości nie są wymagane, chyba że elementu jest za pomocą opcji szyfrowania związane z szyfrowania. Przykład używane w tej sekcji przedstawiono tworzenie IngestManifestFile używającego StorageEncryption trwałego utworzone wcześniej.


**Odpowiedź HTTP**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Przekaż pliki do magazyn obiektów Blob

Możesz użyć dowolnej aplikacji klienckiej szybkich stanie przekazywania plików zasobów w kontenerze magazyn obiektów blob Uri dostarczony przez właściwość BlobStorageUriForUpload IngestManifest. Jedna godne uwagi Wysoka szybkość przekazywania usługa jest [Aspera na żądanie stosowania Azure](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Zbiorcze monitorowanie postępu mogły zjeść tej ostatniej

Można monitorować postęp zbiorcze ingesting operacji IngestManifest przez sondowanie właściwość statystyki IngestManifest. Ta właściwość jest typu złożonego [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Aby skorzystać z tej właściwości statystyki, Prześlij żądanie HTTP GET przekazywanie identyfikatora IngestManifest.
 

##<a name="create-contentkeys-used-for-encryption"></a>Tworzenie ContentKeys używany do szyfrowania

Jeśli do zawartości będzie używać szyfrowania, należy utworzyć ContentKey może być używany na potrzeby szyfrowania przed utworzeniem plików zawartości. Do szyfrowania miejsca do magazynowania następujące właściwości powinien zostać uwzględniony w treści wezwania.
 
Żądanie właściwości treści   | Opis
---|---
Identyfikator | Identyfikator ContentKey, który pracujemy nad wygenerować w następującym formacie "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Typ zawartości klucza jest jako liczbę całkowitą dla tego klucza zawartości. Firma Microsoft przekazać wartość 1 do szyfrowania miejsca do magazynowania.
EncryptedContentKey | Możemy utworzyć nową wartość klucza zawartości jest to wartość 256-bitowy (32 bajt). Klucz jest zaszyfrowany przy użyciu certyfikat X.509 szyfrowania miejsca do magazynowania, który pobieranie z usługi multimediów Azure firmy Microsoft przez wykonywanie żądanie HTTP GET dla GetProtectionKeyId i GetProtectionKey metody.
ProtectionKeyId | To jest identyfikator klucza ochrony certyfikat X.509 szyfrowania miejsca do magazynowania, którego użyto do zaszyfrowania naszych zawartości klucza.
ProtectionKeyType | Jest to typ szyfrowania klucza ochrony, którego użyto do zaszyfrowania klucza zawartości. Ta wartość jest StorageEncryption(1) w naszym przykładzie.
Suma kontrolna |MD5 obliczoną sumę kontrolną dla zawartości klucza. Jest ona obliczana przez zaszyfrowanie zawartości identyfikator kluczem zawartości. Kod przykład przedstawia sposób obliczania sumy kontrolnej.


**Odpowiedź HTTP**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Łącze ContentKey do środka

ContentKey jest skojarzony jeden lub więcej zasobów przez wysłanie żądania HTTP POST. Następujące żądanie jest przykład utworzyć łącze w przykładzie ContentKey aktywów przykład przez identyfikatora.

**Odpowiedź HTTP**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**Odpowiedź HTTP**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
