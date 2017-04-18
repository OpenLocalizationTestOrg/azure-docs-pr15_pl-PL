<properties 
    pageTitle="Nawiązywanie połączenia z kontem usługi multimediów przy użyciu interfejsu API usługi REST | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób nawiązywania połączenia z usługi multimediów uisng interfejsu API usługi REST." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Nawiązywanie połączenia z kontem usługi multimediów przy użyciu interfejsu API usługi REST usługi multimediów

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [POZOSTAŁE](media-services-rest-connect-programmatically.md)

W tym temacie opisano sposób uzyskiwania połączenia programowych usługi multimediów Azure firmy Microsoft są programowania przy użyciu interfejsu API usługi REST usługi multimediów.

Dwie czynności są wymagane podczas uzyskiwania dostępu do usługi multimediów Azure firmy Microsoft: token dostępu dostarczony przez Azure dostępu usługi ACS (Control), a identyfikator URI usługi multimediów się. Możesz użyć środków, która ma być podczas tworzenia te żądania, dopóki określić wartości poprawny nagłówek i przekaż tokenu dostępu poprawnie zadzwonisz do usługi multimediów.

W poniższej procedurze opisano najczęstsze przepływu pracy w przypadku połączyć się z usługami multimediów za pomocą interfejsu API usługi REST usługi multimediów:

1. Wprowadzenie token dostępu 
2. Nawiązywanie połączenia z usługi multimediów identyfikator URI 

    >[AZURE.NOTE] Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI.
Odpowiedź HTTP/1.1 200, która zawiera opis metadanych interfejsu API ODATA może zostać wyświetlony.

3. Publikowanie wezwań interfejsu API do nowego adresu URL. 

    Na przykład jeśli po wypróbowaniu nawiązywania połączenia, masz następujące czynności:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Czy publikowanie wezwań interfejsu API do https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Adres kontroli dostępu

Adres kontroli dostępu do usługi multimediów jest https://wamsprodglobal001acs.accesscontrol.windows.net, z wyjątkiem Chin Północnej regionu, tam, gdzie jest https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Wprowadzenie token dostępu

Aby uzyskać dostęp do usługi multimediów bezpośrednio za pomocą interfejsu API usługi REST, pobierają token dostępu z ACS i używać go podczas każdego żądania HTTP, wprowadzone w usłudze. Token ten jest podobne do innych tokenów dostarczony przez ACS oparte na oświadczeniach dostęp w nagłówku żądania HTTP i za pomocą protokołu OAuth w wersji 2. Przed nawiązaniem połączenia bezpośrednio usługi multimediów nie ma potrzeby inne wymagania wstępne.

W poniższym przykładzie pokazano nagłówka żądania HTTP i treści używana do pobierania tokenu.

**Nagłówek**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Treści**:

Musisz udowodnić wartości client_id i client_secret w treści tej prośby. client_id i client_secret odpowiadają wartości Nazwa konta i AccountKey. Wartości te są dostarczane użytkownikowi przez usługi multimediów podczas konfigurowania konta. 

Pamiętaj, że AccountKey dla Twojego konta usługi multimediów musi być zakodowany adres URL (patrz [Kodowanie](http://tools.ietf.org/html/rfc3986#section-2.1) podczas używania go jako wartość client_secret żądanie token dostępu.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Na przykład: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


W poniższym przykładzie pokazano token w treści odpowiedzi odpowiedź HTTP, która zawiera takie uprawnienia dostępu.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Zalecane jest pamięci podręcznej wartości "access_token" i "expires_in" do zewnętrznych magazynu. Token danych można później pobrane z magazynu i ponownie używana w połączenia interfejsu API usługi REST usługi multimediów. To jest szczególnie przydatne w przypadku scenariuszy miejsce, w którym tokenu można bezpiecznie udostępniać wielu procesów lub komputerach.

Upewnij się monitorować wartości "expires_in" token dostępu z i zaktualizować połączenia interfejsu API usługi REST nowe tokeny stosownie do potrzeb.

###<a name="connecting-to-the-media-services-uri"></a>Nawiązywanie połączenia z usługi multimediów identyfikator URI

Główny identyfikator URI dla usługi multimediów jest https://media.windows.net/. Początkowo należy łączyć się ten identyfikator URI, a jeśli zostanie wyświetlony 301 przekierowanie w odpowiedzi, należy kolejnych zaproszeń do nowego identyfikatora URI. Ponadto nie należy używać dowolnego logika automatycznego — przekierowanie/Flaga w wezwaniach na. Zleceń HTTP i żądanie organów nie zostaną przekazane do nowego identyfikatora URI.

Należy zauważyć, że główny identyfikator URI do przekazywania i pobierania plików trwałego https://yourstorageaccount.blob.core.windows.net/ miejsce, w którym nazwę konta magazynu jest taki sam, użytą podczas konfiguracji konta usługi multimediów.

Poniższy przykład pokazuje żądania HTTP do głównego usługi multimediów identyfikator URI (https://media.windows.net/). Żądanie otrzymuje 301 przekierowanie w odpowiedzi. Następne żądanie używa nowy identyfikator URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Żądanie HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**Odpowiedź HTTP**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**Żądanie HTTP** (za pomocą nowego identyfikatora URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**Odpowiedź HTTP**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Gdy zostanie wyświetlony nowy identyfikator URI, to jest identyfikator URI, których należy używać można komunikować się z usługi multimediów. 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
