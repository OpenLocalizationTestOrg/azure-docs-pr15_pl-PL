<properties 
    pageTitle="Konfigurowanie zasad dostarczania zawartości za pomocą interfejsu API usługi REST usługi multimediów | Microsoft Azure" 
    description="W tym temacie pokazano, jak skonfigurować zasady dostarczania różnych elementów zawartości za pomocą interfejsu API usługi REST usługi multimediów." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Konfigurowanie zasad dostarczania zawartości

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Jeśli planujesz do przeprowadzania dynamicznie zaszyfrowanych aktywów jeden z kroków w przepływie pracy dostarczania zawartości usługi multimediów jest konfigurowanie zasad dostarczania trwałych. Zasady dostarczania zawartości informuje usługi multimediów sposobu dla swojego trwałego dostarczanych: do których streaming protokołu należy do zasobów dynamicznie umieszczane (na przykład, kreska MPEG, HLS, gładkie Streaming lub wszystkie), czy chcesz zaszyfrować dynamicznie do zawartości i w jaki sposób (kopert lub szyfrowania typowych).

W tym temacie omówiono dlaczego i jak utworzyć i skonfigurować zasady dostarczania zawartości.

>[AZURE.NOTE]Aby móc używać dynamiczne opakowania i szyfrowania dynamiczne, należy się upewnić mieć co najmniej jedną jednostkę skali (nazywane także przesyłanie strumieniowe jednostka). Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).
>
>Ponadto do zawartości musi zawierać zestaw adaptacyjne szybkość transmisji bitów MP4s lub adaptacyjne szybkość transmisji bitów wygładzonymi strumieniowego przesyłania plików.

Na tym samym trwały, można zastosować różne zasady. Na przykład można zastosować PlayReady szyfrowania do szyfrowania wygładzonymi przesyłanie strumieniowe i AES koperty do ŁĄCZNIKA MPEG i HLS. Protokołów, które nie są zdefiniowane w zasadzie dostarczenia (na przykład możesz dodać pojedynczy zasadę tylko określająca HLS jako protokół) będą zablokowanie streaming. Wyjątek to, czy masz określono na wszystkich zasad dostarczania zawartości. Następnie wszystkie protokoły będą mogli w Wyczyść.

Jeśli chcesz przedstawić zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy. Na przykład aby zapewnić usługi trwałego zaszyfrowane kluczem szyfrowania koperty szyfrowania AES (Advanced Standard), Ustaw typ zasad **DynamicEnvelopeEncryption**. Aby usunąć szyfrowanie miejsca do magazynowania i przesyłać strumieniowo zawartości w Wyczyść, Ustaw typ zasad **NoDynamicEncryption**. Przykłady pokazano, jak skonfigurować następujące typy zasad wykonaj.

W zależności od konfiguracji zasad dostarczania zawartości będzie można dynamicznie pakowanie, dynamicznie szyfrowanie i przesyłać strumieniowo następujących protokołów: strumienie wygładzonymi Streaming, HLS KRESKOWANIA MPEG i obr. / min.

Na poniższej liście przedstawiono formaty umożliwia strumienia gładkie, HLS, kreska i obr. / min.

Przesyłanie strumieniowe wygładzonymi:

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG ŁĄCZNIKA

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

OBR. / MIN

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Aby uzyskać instrukcje, jak publikować środka trwałego i tworzyć przesyłanie strumieniowe adresu URL zobacz [Tworzenie przesyłanie strumieniowe adresu URL](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Zagadnienia dotyczące

- Nie można usunąć AssetDeliveryPolicy skojarzone z środka trwałego podczas locator (przesyłanie strumieniowe) na żądanie nie istnieje dla tego zasobu. Zaleca się usunąć zasady z elementu przed usunięciem zasad.
- Nie można utworzyć przesyłanie strumieniowe locator trwałego zaszyfrowanych miejsca do magazynowania, jeśli nie zasady dostarczania zawartości są ustawione.  Jeśli elementu nie jest zaszyfrowany miejsca do magazynowania, system pozwoli locator tworzenie i przesyłanie strumieniowe zawartości w wyczyść bez zasad dostarczania zawartości.
- Można mieć wiele zasad dostarczania zawartości skojarzonych z pojedynczego zasobu, ale można określić tylko jeden sposób, aby obsługiwać danej AssetDeliveryProtocol.  Co oznacza, spróbuj połączyć dwie zasady dostarczenia, które określają protokół AssetDeliveryProtocol.SmoothStreaming, który będzie wynikiem jest błąd, ponieważ system nie wie, której jeden ma zostać zastosowany, gdy klient wysyła żądanie Streaming gładkie.
- Jeśli masz środka trwałego z istniejących locator przesyłanie strumieniowe nie łącze nowych zasad do elementu, odłączyć istniejące zasady z elementu lub aktualizowanie zasady dostarczania skojarzonych z trwałym.  Najpierw musisz usunąć przesyłanie strumieniowe locator, dostosowywanie zasad i ponownie utworzyć przesyłanie strumieniowe locator.  Za pomocą samego locatorId gdy odtworzenie przesyłanie strumieniowe locator, ale należy upewnić się, że nie który powodować problemy dla klientów, ponieważ zawartości mogą być buforowane pochodzenie lub CDN za.

>[AZURE.NOTE] Praca z interfejsu API usługi REST usługi multimediów, następujące kwestie:
>
>Podczas uzyskiwania dostępu do obiektów w usługach multimediów, należy ustawić określone nagłówek pola i wartości w wezwaniach na HTTP. Aby uzyskać więcej informacji zobacz [Ustawienia dotyczące multimediów usługi REST interfejsu API rozwoju](media-services-rest-how-to-use.md).

>Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą usługi multimediów za pomocą interfejsu API usługi REST](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Zasady dostarczania wyczyść składników majątku

###<a id="create_asset_delivery_policy"></a>Tworzenie zasad dostarczania zawartości
Następujące żądanie HTTP tworzy zasady dostarczania zawartości, która określa się nie stosuj dynamiczne szyfrowania i dostarczyć strumienia w dowolnej z następujących protokołów: ŁĄCZNIKA MPEG, HLS i wygładzonymi Streaming protokołów. 

Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .   


Żądanie:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Odpowiedź:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Łącze zawartości z zasad dostarczania zawartości

Następujące żądanie HTTP linki do określonego elementu do zasady dostarczania zawartości.

Żądanie:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Odpowiedź:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zasady dostarczania zawartości DynamicEnvelopeEncryption 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Tworzenie zawartości klucza typu EnvelopeEncryption i połączyć go z elementu

Określając zasady dostarczania DynamicEnvelopeEncryption, musisz upewnij się utworzyć łącze do zawartości do klucz zawartości typu EnvelopeEncryption. Aby uzyskać więcej informacji, zobacz: [Tworzenie zawartości klucza](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Uzyskiwanie adresu URL dostarczenia

Uzyskać adres URL dostarczenia metody określonej dostarczania zawartości klucza utworzony w poprzednim kroku. Klient używa zwracany adres URL, aby zażądać klucza AES lub PlayReady licencję w celu odtwarzania zawartości chronionej.

Określ typ adresu URL, aby przejść w treści żądania HTTP. Czy są ochrony zawartości z PlayReady, żądanie adresu URL nabycia licencji PlayReady usługi multimediów, za pomocą 1 keyDeliveryType: {"keyDeliveryType": 1}. Jeśli są chroni zawartości z szyfrowania koperty, żądanie adresu URL nabycia klucza, określając 2 dla keyDeliveryType: {"keyDeliveryType": 2}.

Żądanie:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Odpowiedź:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Tworzenie zasad dostarczania zawartości

Następujące żądanie HTTP tworzy **AssetDeliveryPolicy** jest skonfigurowany do dotyczą protokołu **HLS** szyfrowania dynamiczne koperty (**DynamicEnvelopeEncryption**) (w tym przykładzie inne protokoły zostaną zablokowane z streaming). 


Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .   

Żądanie:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Odpowiedź:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Łącze zawartości z zasad dostarczania zawartości

Zobacz [Łącze zawartości z zasad dostarczania zawartości](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Zasady dostarczania zawartości DynamicCommonEncryption 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Tworzenie zawartości klucza typu CommonEncryption i połączyć go z elementu

Określając zasady dostarczania DynamicCommonEncryption, musisz upewnij się utworzyć łącze do zawartości do klucz zawartości typu CommonEncryption. Aby uzyskać więcej informacji, zobacz: [Tworzenie zawartości klucza](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Uzyskiwanie adresu URL dostarczenia

Uzyskiwanie adresu URL dostarczania dla PlayReady metoda dostarczania zawartości klucza utworzony w poprzednim kroku. Klient używa zwracany adres URL, aby zażądać licencji PlayReady w celu odtwarzania zawartości chronionej. Aby uzyskać więcej informacji zobacz [Uzyskiwanie adresu URL dostarczenia](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Tworzenie zasad dostarczania zawartości

Następujące żądanie HTTP tworzy **AssetDeliveryPolicy** jest skonfigurowany do dotyczą dynamiczne szyfrowania typowych (**DynamicCommonEncryption**) **Wygładzonymi Streaming** protocol (w tym przykładzie inne protokoły zostaną zablokowane z streaming). 

Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .   


Żądanie:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Jeśli chcesz Ochrona zawartości za pomocą Widevine DRM zaktualizuj wartości AssetDeliveryConfiguration WidevineLicenseAcquisitionUrl (który ma wartość 7), a następnie określ adres URL usługi dostarczania licencji. Za pomocą następujących partnerów AMS ułatwiające przeprowadzania licencji Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Na przykład: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]W przypadku szyfrowania z Widevine, czy tylko można to zrobić przy użyciu ŁĄCZNIKA. Upewnij się określić ŁĄCZNIKA (2) w Protokół dostarczania zawartości.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Łącze zawartości z zasad dostarczania zawartości

Zobacz [Łącze zawartości z zasad dostarczania zawartości](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Typy używane podczas definiowania AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
