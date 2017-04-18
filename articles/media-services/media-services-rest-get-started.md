<properties 
    pageTitle="Wprowadzenie do dostarczania zawartości na żądanie za pomocą usługi REST | Microsoft Azure" 
    description="Ten samouczek przeprowadzi Cię przez kroki implementacji aplikację dostarczania zawartości na żądanie z usługi multimediów Azure za pomocą interfejsu API usługi REST." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Wprowadzenie do dostarczania zawartości na żądanie za pomocą usługi REST 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F). 


Ten szybki start opisano kroki stosowania aplikację dostarczania zawartości usługi wideo na żądanie (VoD) przy użyciu interfejsów API pozostałych usługi multimediów Azure (AMS). 

Samouczek wprowadza podstawowe przepływu pracy usługi multimediów i najczęściej używanych obiektów programowania i zadania wymagane rozwoju usługi multimediów. Po zakończeniu samouczka będą mogli przesyłać strumieniowo lub stopniowo pobrać przykładowy plik multimedialny, przekazane, zakodowany i pobrać ustawienia.  

## <a name="prerequisites"></a>Wymagania wstępne
Aby rozpocząć tworzenie oprogramowania z usługi multimediów z pozostałych interfejsy API są wymagane następujące wymagania wstępne.

- Opis sposobu rozwinąć za pomocą interfejsu API usługi REST usługi multimediów. Aby uzyskać więcej informacji zobacz [pozostałych omówienie, media usług w](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Aplikacja, który można wysłać żądania HTTP i odpowiedzi. Samouczku [Fiddler](http://www.telerik.com/download/fiddler). 

Następujące zadania są wyświetlane w tym Szybki Start.

1.  Utwórz konto usługi multimediów (za pomocą portalu Azure).
2.  Skonfiguruj przesyłanie strumieniowe punkt końcowy (za pomocą portalu Azure).
1.  Nawiązywanie połączenia z kontem usługi multimediów z interfejsu API usługi REST.
1.  Utwórz nowy trwały i przekaż plik wideo z interfejsu API usługi REST.
1.  Skonfiguruj przesyłanie strumieniowe jednostki z interfejsu API usługi REST.
2.  Kodowanie plik źródłowy w zestawie adaptacyjne szybkość transmisji bitów MP4 plików za pomocą interfejsu API usługi REST.
1.  Publikowanie zawartości i uzyskiwanie streaming i adresy URL pobierania progresywnego z interfejsu API usługi REST. 
1.  Odtwarzanie zawartości. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Tworzenie konta usługi multimediów Azure za pomocą portalu Azure

W tej sekcji pokazano, jak utworzyć konto AMS.

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowy** > **multimediów + CDN** > **usługi multimediów**.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Tworzenie** konta usługi multimediów wprowadź żądane wartości.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. W polu **Nazwa konta**wpisz nazwę nowego konta AMS. Nazwa konta usługi multimediów są wszystkie liczby litera lub litery bez spacji i długości 3-24 znaków.
    2. W subskrypcji wybierz jeden z różnych subskrypcjach Azure, które mają dostęp do.
    
    2. **Grupa zasobów**wybierz zasób nowym lub istniejącym.  Grupa zasobów to zbiór zasobów, które współużytkują cyklu życia, uprawnień i zasady. Dowiedz się, [w tym miejscu](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. W **lokalizacji**wybierz pozycję regionu geograficznego służy do przechowywania rekordów multimedia i metadanych dla Twojego konta usługi multimediów. Ten obszar jest używany do przetwarzania i przesyłanie strumieniowe multimediów. Tylko dostępne regiony usługi multimediów są wyświetlane w polu listy rozwijanej. 
    
    3. **Miejsca do magazynowania konta**wybierz konto miejsca do magazynowania o podanie magazyn obiektów blob zawartości multimedialnej z konta usługi multimediów. Można wybrać istniejące konto miejsca do magazynowania w tym samym regionu geograficznego jako konta usługi multimediów, lub można utworzyć konto miejsca do magazynowania. W tym samym regionie tworzone jest nowe konto miejsca do magazynowania. Zasady przechowywania nazwy kont są takie same jak w przypadku kont usługi multimediów.

        Dowiedz się więcej o miejsca do magazynowania [w tym miejscu](storage-introduction.md).

    4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , aby wyświetlić postęp wdrożenia konta.
    
7. Kliknij przycisk **Utwórz** w dolnej części formularza.

    Po pomyślnym utworzeniu konta zostanie zmieniony na **Uruchamianie**. 

    ![Ustawienia usługi multimediów](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Aby zarządzać swoim kontem AMS (na przykład przekazywanie klipów wideo, kodowanie składników majątku, monitorować postęp zadania) za pomocą okna **Ustawienia** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurowanie przesyłania strumieniowego punkty końcowe za pomocą portalu Azure

Podczas pracy z jedną z najbardziej typowe scenariusze jest przedstawiania wideo za pośrednictwem szybkość transmisji bitów adaptacyjne streaming dla klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

Usługi multimediów zawiera opakowania dynamiczne, umożliwiające dostarczanie usługi adaptacyjne szybkość transmisji bitów zawartości MP4 zakodowany w streaming formaty obsługiwane przez usługi multimediów (KRESKOWANIA MPEG, HLS, gładkie Streaming, obr. / min) tylko na czas, bez konieczności przechowywanie wersji wstępnie detaliczny każdego z nich streaming format.

Aby skorzystać z opakowania dynamiczne, należy wykonać następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu plików adaptacyjne szybkość transmisji bitów MP4 (kodowania czynności przedstawiono w dalszej części tego samouczka).  
- Tworzenie co najmniej jeden jednostki przesyłanie strumieniowe *Przesyłanie strumieniowe punktu końcowego* z której zamierzasz dostarczania zawartości. Poniżej pokazano, sposób zmieniania liczby jednostek przesyłanie strumieniowe.

Dynamiczne opakowania, potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta.

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:


1. W oknie **Ustawienia** kliknij pozycję **punkty końcowe strumieniowych**. 

2. Kliknij przycisk domyślne streaming punktu końcowego. 

    Zostanie wyświetlone okno **Domyślne STREAMING szczegóły punktu KOŃCOWEGO** .

3. Aby określić liczbę jednostek przesyłanie strumieniowe, przesuń suwak **Streaming jednostki** .

    ![Przesyłanie strumieniowe jednostki](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknij przycisk **Zapisz** , aby zapisać zmiany.

    >[AZURE.NOTE]Przydział wszelkie nowe jednostki może potrwać do 20 minut.

## <a id="connect"></a>Nawiązywanie połączenia z kontem usługi multimediów z interfejsu API usługi REST

Dwie czynności są wymagane podczas uzyskiwania dostępu do usługi multimediów Azure: token dostępu dostarczony przez Azure dostępu usługi ACS (Control), a identyfikator URI usługi multimediów się. Możesz użyć środków, która ma być podczas tworzenia te żądania, dopóki określić wartości poprawny nagłówek i przekaż tokenu dostępu poprawnie zadzwonisz do usługi multimediów.

W poniższej procedurze opisano najczęstsze przepływu pracy w przypadku połączyć się z usługami multimediów za pomocą interfejsu API usługi REST usługi multimediów:

1. Pobieranie token dostępu. 
2. Nawiązywanie połączenia z usługi multimediów identyfikatora URI.  

    Należy pamiętać, że po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI. Odpowiedź HTTP/1.1 200, która zawiera opis metadanych interfejsu API ODATA może zostać wyświetlony.
3. Publikowanie wezwań interfejsu API do nowego adresu URL. 
    
    Na przykład jeśli po wypróbowaniu nawiązywania połączenia, masz następujące czynności:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Czy publikowanie wezwań interfejsu API do https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Wprowadzenie token dostępu

Aby uzyskać dostęp do usługi multimediów bezpośrednio za pomocą interfejsu API usługi REST, pobierają token dostępu z ACS i używać go podczas każdego żądania HTTP, wprowadzone w usłudze. Przed nawiązaniem połączenia bezpośrednio usługi multimediów nie ma potrzeby inne wymagania wstępne.

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

W przypadku używania go jako wartość client_secret żądanie token dostępu AccountKey dla Twojego konta usługi multimediów musi być zakodowany adres URL.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Na przykład: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


W poniższym przykładzie pokazano tokenu w treści odpowiedzi odpowiedź HTTP, która zawiera takie uprawnienia dostępu.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Zalecane jest pamięci podręcznej wartości "access_token" i "expires_in" do zewnętrznych magazynu. Później być pobierane z magazynu i ponownie używana w połączenia interfejsu API usługi REST usługi multimediów tokenu danych. To jest szczególnie przydatne w sytuacjach, gdzie tokenu można bezpiecznie udostępniać wielu procesów lub komputerach.

Upewnij się monitorować wartości "expires_in" token dostępu z i zaktualizować połączenia interfejsu API usługi REST nowe tokeny stosownie do potrzeb.

###<a name="connecting-to-the-media-services-uri"></a>Nawiązywanie połączenia z usługi multimediów identyfikator URI

Główny identyfikator URI dla usługi multimediów jest https://media.windows.net/. Początkowo należy łączyć się ten identyfikator URI, a jeśli zostanie wyświetlony 301 przekierowanie w odpowiedzi, należy kolejnych zaproszeń do nowego identyfikatora URI. Ponadto nie należy używać dowolnego logika automatycznego — przekierowanie/Flaga w wezwaniach na. Zleceń HTTP i żądanie organów nie zostaną przekazane do nowego identyfikatora URI.

Pierwiastek URI do przekazywania i pobierania plików zawartości jest https://yourstorageaccount.blob.core.windows.net/ miejsce, w którym nazwę konta magazynu jest taki sam, użytą podczas konfiguracji konta usługi multimediów.

Poniższy przykład pokazuje żądania HTTP do głównego usługi multimediów identyfikator URI (https://media.windows.net/). Żądanie otrzymuje 301 przekierowanie w odpowiedzi. Następne żądanie używa nowy identyfikator URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Żądanie HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Od tej pory nowy identyfikator URI jest używana w tym samouczku.

## <a id="upload"></a>Utwórz nowy trwały i przekaż plik wideo z interfejsu API usługi REST

Usługi multimediów możesz przekazać pliki cyfrowy do środka trwałego. Jednostki **zasobów** może zawierać klip wideo, audio, obrazy, miniatur zbiorów, tekst ścieżek i napisy plików (i metadanych dotyczących tych plików.)  Gdy pliki są przekazywane do elementu, zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego. 

Jedną z wartości, które należy podać podczas tworzenia środka trwałego jest opcje tworzenia zawartości. Właściwość **opcji** jest wartością wyliczenia, który opisuje opcje szyfrowania, które mogą być tworzone środka trwałego za. Prawidłową wartość jest jednym z wartości z listy poniżej, a nie kombinacja wartości z tej listy:

 
- **Brak** = **0** - szyfrowanie nie jest używany. Podczas korzystania z tej opcji zawartości nie jest chroniony, podczas przesyłania lub spoczywa w magazynie.
    Jeśli planujesz do przeprowadzania MP4 za pomocą stopniowego pobierania, użyj tej opcji. 
- **StorageEncrypted** = **1** — są szyfrowane Wyczyść zawartość lokalnie, przy użyciu szyfrowania bitowego AES-256 i przekazuje go do magazynu Azure miejsce, w którym jest przechowywany szyfrowane na pozostałych. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć pliki multimedialne wprowadzania wysokiej jakości z silnego szyfrowania spoczynku na dysku.
- **CommonEncryptionProtected** = **2** — używanie tę opcję, jeśli przekazujesz zawartość, która już szyfrowane i chronione za pomocą szyfrowania typowych lub DRM PlayReady (na przykład wygładzonymi przesyłanie strumieniowe chroniony z PlayReady DRM).
- **EnvelopeEncryptionProtected** = **4** — Użyj tej opcji, jeśli wysyłasz HLS zaszyfrowanych za pomocą AES. Należy zakodowany pliki i szyfrowane przez Menedżera Przekształcanie.

### <a name="create-an-asset"></a>Tworzenie środka trwałego

Zasób jest kontenerem dla wielu typów lub zestawy obiektów w usługach multimediów, takich jak klipy wideo, audio, obrazy, miniatur zbiorów, ścieżek tekstowych i napisy plików. W interfejsie API usługi REST Tworzenie środka trwałego wymaga wysłanie żądania WPIS usługi multimediów i wprowadzania wszelkie informacje o swojej zawartości w treści wezwania.

W poniższym przykładzie pokazano, jak utworzyć środka trwałego.

**Żądanie HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Tworzenie AssetFile

Jednostka [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) reprezentuje pliku audio lub wideo, który jest przechowywany w kontenerze obiektów blob. Plik zawartości są zawsze skojarzone z środka trwałego, a środka trwałego może zawierać jeden lub wiele AssetFiles. Zadanie Media Encoder usług nie powiedzie się, czy obiekt plik zawartości nie jest skojarzone z plikiem cyfrowego w kontenerze obiektów blob.

Po przekazaniu pliku multimediami cyfrowymi kontener obiektów blob, umożliwia żądania HTTP **SCALANIE** zaktualizuj AssetFile z informacjami o pliku multimediów (jak pokazano w dalszej części tematu). 

**Żądanie HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

Przed przekazaniem wszystkie pliki do magazyn obiektów blob, Ustaw dostęp prawa zasad pomocne przy tworzeniu do środka trwałego. Aby to zrobić, PUBLIKOWAĆ żądania HTTP zestaw jednostek AccessPolicies. Definiowanie wartości DurationInMinutes po utworzeniu lub otrzymujesz komunikat o błędzie 500 wewnętrznego serwera w odpowiedzi. Aby uzyskać więcej informacji o AccessPolicies zobacz [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

W poniższym przykładzie pokazano, jak utworzyć AccessPolicy:
        
**Żądanie HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Odpowiedź HTTP**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>Uzyskać adres URL przekazywania

Aby uzyskać adres URL przekazywania rzeczywiste, Utwórz Locator skojarzeń zabezpieczeń. Locator określić czas rozpoczęcia i typ punktu końcowego połączenia dla klientów, którzy mają dostęp do plików w środka trwałego. Możesz utworzyć wiele jednostek Locator dla danej pary AccessPolicy i środków trwałych do obsługi żądania innego klienta i wymagań. Aby określić czas, można użyć adresu URL każdej z tych Locator używane wartości czas rozpoczęcia oraz wartość DurationInMinutes AccessPolicy. Aby uzyskać więcej informacji zobacz [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Adres URL skojarzeń zabezpieczeń ma następujący format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Niektóre kwestie:

- Nie może mieć więcej niż pięciu unikatowych Locator skojarzone z danego składnika majątku w tym samym czasie. Aby uzyskać więcej informacji zobacz Locator.
- Jeśli chcesz od razu Przekaż pliki, należy ustawić wartość czas rozpoczęcia do pięciu minut przed bieżącą godzinę. To może być zegar skośność między usługami multimediów i komputer kliencki. Ponadto wartość czas rozpoczęcia musi być w następującym formacie daty i godziny: YYYY-MM-DDTHH:mm:ssZ (na przykład "2014-05-23T17:53:50Z").   
- Czasami może występować na sekundę 30-40 opóźnienia po utworzeniu Locator, gdy jest gotowy do użycia. Ten problem dotyczy do adresu URL skojarzeń zabezpieczeń i Locator pochodzenia.

W poniższym przykładzie pokazano sposób tworzenia skojarzenia zabezpieczeń Locator adresu URL, zdefiniowane przez właściwości Typ w treści wezwania ("1" dla locator skojarzenia zabezpieczeń) i "2" dla locator origin na żądanie. Właściwość **Path** zwracany zawiera adres URL, który należy użyć, aby przekazać plik.
    
**Żądanie HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpowiedź HTTP**

Jeśli powodzenia następujące jest zwracana: HTTP/1.1 204 Brak zawartości

## <a name="delete-the-locator-and-accesspolicy"></a>Usuwanie Locator i AccessPolicy 

**Żądanie HTTP**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:

    HTTP/1.1 204 No Content 
    ...

**Żądanie HTTP**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Konfigurowanie przesyłania strumieniowego jednostki z interfejsu API usługi REST

Podczas pracy z jedną z najbardziej typowe scenariusze dostarczane szybkość transmisji bitów adaptacyjne streaming do klientów usługi multimediów Azure. W przypadku przesyłania strumieniowego adaptacyjne szybkość transmisji bitów klienta można przełączyć się strumienia wyższych lub niższych szybkość transmisji bitów, jak wideo są wyświetlane na podstawie bieżącej przepustowości sieci, użycie Procesora i innych czynników. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp). 

Usługi multimediów zawiera opakowań dynamiczne pozwala na przedstawianie zawartości adaptacyjne szybkość transmisji bitów MP4 lub gładkie Streaming zakodowany w streaming formaty obsługiwane przez usługi multimediów (KRESKOWANIA MPEG, HLS, gładkie Streaming, obr. / min) bez konieczności ponownie utworzyć pakiet do tych streaming format. 

Aby skorzystać z opakowania dynamiczne, należy wykonać następujące czynności:

- uzyskać co najmniej jedną jednostkę przesyłanie strumieniowe **Przesyłanie strumieniowe punktu końcowego **z której zamierzasz dostarczania zawartości (opisane w tej sekcji).
- kodowanie lub kod transakcji usługi kodery (źródło) plików na zbiór adaptacyjne szybkość transmisji bitów MP4 plików lub adaptacyjne szybkość transmisji bitów wygładzonymi Streaming (kodowania czynności przedstawiono w dalszej części tego samouczka),  

Dynamiczne opakowania, potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta. 


>[AZURE.NOTE] Aby uzyskać informacje o cenach szczegóły zobacz [Szczegóły cennik usługi multimediów](http://go.microsoft.com/fwlink/?LinkId=275107).

Aby zmienić liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Uzyskiwanie przesyłanie strumieniowe punktu końcowego, który chcesz zaktualizować

Na przykład korzystaj pierwszego punktu końcowego strumieniowych na koncie (może być do dwóch przesyłanie strumieniowe punkty końcowe w bieżącej stan w tym samym czasie).

**Żądanie HTTP**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Odpowiedź HTTP**
    
Jeśli kończy się pomyślnie, jest zwracany następujące czynności:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Skala przesyłanie strumieniowe punktu końcowego
 
**Żądanie HTTP**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**Odpowiedź HTTP**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Sprawdzanie stanu operacji długim

Przydział wszelkie nowe jednostki wymaga około 20 minut do wykonania. Aby sprawdzić stan operację, użyj metody **operacji** i określ identyfikator operacji. Operacja identyfikator został zwrócony w odpowiedzi na wezwania **Skala** .

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**Żądanie HTTP**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**Odpowiedź HTTP**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Kodowanie pliku źródłowego do zestawu plików MP4 adaptacyjne szybkość transmisji bitów

Po ingesting, które mogą być kodowane elementy zawartości do usługi multimediów, multimediów, transmuxed oznaczenie znakiem wodnym i tak dalej przed przekazaniem do klientów. Działania te są planowane i wykonywane wielu wystąpień roli tła zapewnienie wysokiej wydajności i dostępności. Działania te są nazywane zadań i każdego [zadania](http://msdn.microsoft.com/library/azure/hh974289.aspx) składa się z Atomowej zadania, które wykonać pracy rzeczywistej w pliku zasobów. 

Jak wspomniano wcześniej, podczas pracy z multimediów Azure jest jednym z najbardziej typowych scenariuszy przedstawiania szybkość transmisji bitów adaptacyjne streaming dla klientów usług. Usługi multimediów dynamicznie spakować zestawu adaptacyjne szybkość transmisji bitów MP4 plików do jednego z następujących formatów: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp). 

Aby skorzystać z opakowania dynamiczne, należy wykonać następujące czynności:

- kodowanie lub kod transakcji usługi kodery (źródło) plików na zbiór adaptacyjne szybkość transmisji bitów MP4 plików lub adaptacyjne szybkość transmisji bitów Streaming gładkie,  
- Uzyskaj co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. 

Sekcji poniżej pokazano, jak utworzyć zadanie, które zawiera jedno zadanie kodowania. Zadania określa kod transakcji plik kodery w zestawie adaptacyjne szybkość transmisji bitów MP4s przy użyciu **Media Encoder standardowy**. W sekcji przedstawiono również sposobów monitorowania przetwarzanie informacji o postępie zadań. Po zakończeniu zadania, czy można utworzyć Locator, które są potrzebne, aby uzyskać dostęp do aktywów. 

### <a name="get-a-media-processor"></a>Uzyskiwanie procesor multimediów

W usługach multimediów procesor multimediów jest składnik, który obsługuje przetwarzanie określonego zadania, na przykład kodowanie konwersję formatu zawartości multimedialnej szyfrowania lub odszyfrowywania. Do kodowania zadania wyświetlane w tym samouczku firma Microsoft będzie używany Media Encoder standardowy.

Poniższy kod żąda kodera identyfikator. 

**Żądanie HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Odpowiedź HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
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

### <a name="create-a-job"></a>Tworzenie zadania

Każde zadanie może zawierać jedno lub więcej zadań w zależności od typu przetwarzania, którą chcesz wykonać. Za pośrednictwem interfejsu API usługi REST, możesz utworzyć zadań i ich powiązanych zadań w jeden z dwóch sposobów: zadania mogą być zdefiniowane w tekście przez właściwość nawigacji zadania jednostki zadania lub w ramach przetwarzanie wsadowe OData. Media SDK usług używa przetwarzanie wsadowe. Aby zwiększyć czytelność przykłady kodu, w tym temacie, zadania są zdefiniowane w tekście. Aby uzyskać informacje na przetwarzanie wsadowe Zobacz [Przetwarzanie wsadowe protokołu Open Data (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

W poniższym przykładzie pokazano, jak tworzyć i publikować zadanie z jednym ustawione zadania do kodowania wideo w określonej rozdzielczości i jakość. W poniższej sekcji dokumentacji zawiera listę wszystkich [ustawień wstępnych zadań](http://msdn.microsoft.com/library/mt269960) obsługiwanych przez procesor Media Encoder standardowy.  

**Żądanie HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Istnieje kilka ważnych uwag w przypadku każdego wezwania zadania:

- Aby określić liczbę wejścia lub wyjścia zasoby, które są używane przez zadanie TaskBody właściwości należy użyć literałów XML. Temat zadania zawiera definicji schematu XML XML.
- W definicji TaskBody każdego wewnętrzne wartość dla <inputAsset> i <outputAsset> musi być ustawiony jako JobInputAsset(value) lub JobOutputAsset(value).
- Zadania może mieć wielu zasobów dane wyjściowe. Jeden JobOutputAsset(x) można używać tylko raz jako wynik zadania w ramach zadania.
- JobInputAsset lub JobOutputAsset można określić jako środka trwałego wprowadzania zadania.
- Zadania nie mogą stanowić cyklu.
- Parametr wartości, które należy przekazać do JobInputAsset lub JobOutputAsset reprezentuje wartość indeksu dla środka trwałego. Zasoby rzeczywiste są definiowane w właściwości nawigacji InputMediaAssets i OutputMediaAssets w definicji jednostki zadania. 

>[AZURE.NOTE] Ponieważ usługi multimediów jest oparty na protokołu OData v3, pojedyncze środkami InputMediaAssets i OutputMediaAssets kolekcje właściwości nawigacji istnieją odwołania za pośrednictwem "__metadata: identyfikator uri" para wartości z pola Nazwa. 

- InputMediaAssets mapowany na jeden lub więcej zasobów, które zostały utworzone w usługach multimediów. OutputMediaAssets są tworzone przez system. Nie odwołują się do istniejącej zawartości.
- Może być nazwany OutputMediaAssets za pomocą atrybutu assetName. Jeśli ten atrybut nie występuje, a następnie nazwę OutputMediaAsset jest niezależnie od wewnętrzne wartość tekstowa <outputAsset> element jest z sufiksem wartości z pola Nazwa zadania lub wartość Identyfikator zadania (w przypadku, w którym nie jest zdefiniowana właściwość Name). Na przykład jeśli ustawisz wartość assetName "Próby", następnie właściwość OutputMediaAsset Name chcesz ustawić "Próby". Jednak jeśli nie ustawił wartość assetName, ale ustawić nazwę zadania, która "NewJob", nazwą OutputMediaAsset byłaby "_NewJob JobOutputAsset (wartości)". 

    W poniższym przykładzie pokazano, jak ustawić atrybut assetName:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Aby włączyć łączenia zadań:

    - Zadanie musi mieć co najmniej dwa zadania
    - Musi istnieć co najmniej jedno zadanie, w których dane wejściowe są dane wyjściowe innego zadania w zadaniu.

Aby uzyskać więcej informacji, zobacz [Tworzenie kodowanie zadania z interfejsu API usługi REST usługi multimediów](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Monitorowanie postępu przetwarzania

Stan zadania można pobrać przy użyciu właściwości stan, jak pokazano w poniższym przykładzie. 

**Żądanie HTTP**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Anulowanie zadania

Usługi multimediów umożliwia anulowanie uruchomionego zadań przy użyciu funkcji CancelJob. To wywołanie zwraca kod błędu 400 przy próbie anulowanie zadania po anulowaniu stanu, anulowania, błąd lub zakończone.

W poniższym przykładzie pokazano sposób wywoływania CancelJob.


**Żądanie HTTP**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Jeśli kończy się pomyślnie, zwracany jest kod 204 odpowiedzi z treści wiadomości.

>[AZURE.NOTE] Należy kodowanie adresu URL identyfikator zadania (zwykle nb:jid:UUID: somevalue) podczas przekazywania go jako parametr do CancelJob.


### <a name="get-the-output-asset"></a>Pobieranie zawartości wyjścia 

Poniższy kod przedstawia jak zażądać trwałego dane wyjściowe identyfikatora. 


**Żądanie HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Odpowiedź HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Publikowanie zawartości i uzyskiwanie streaming i adresy URL pobierania progresywnego z interfejsu API usługi REST

Przesyłanie strumieniowe lub pobieranie środka trwałego, należy najpierw "przed opublikowaniem", tworząc locator. Locator zapewniają dostęp do plików przechowywanych w elementu. Usługi multimediów obsługuje dwa typy Locator: Locator OnDemandOrigin, używanym do przesyłania strumieniowego multimediów (na przykład MPEG KRESKI, HLS lub gładkie Streaming) i Locator podpis programu Access (SA) używany do pobierania plików multimedialnych.

Po utworzeniu Locator, można tworzyć adresy URL, które są używane do przesyłania strumieniowego lub pobierania plików. 


Gładkie przesyłanie strumieniowe przesyłanie strumieniowe adres URL ma następujący format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Przesyłanie strumieniowe adresem URL HLS ma następujący format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Przesyłanie strumieniowe adresem URL MPEG ŁĄCZNIKA ma następujący format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adres URL skojarzeń zabezpieczeń używany do pobierania plików ma następujący format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

W tej sekcji przedstawiono sposób wykonywania następujących zadań "opublikowanie" aktywów.  

- Tworzenie AccessPolicy za pomocą uprawnień odczytu 
- Tworzenie skojarzeń zabezpieczeń adresu URL dla pobierania zawartości 
- Tworzenie źródłowy adres URL dla strumieniowego przesyłania zawartości 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Tworzenie AccessPolicy za pomocą uprawnień odczytu

Przed pobraniem pliku streaming zawartość multimediów, definiowanie AccessPolicy z uprawnienia do odczytu i utworzyć odpowiednią jednostkę Locator, który określa typ mechanizmu dostarczenia, które chcesz włączyć dla klientów. Aby uzyskać więcej informacji na właściwości dostępne zobacz [Właściwości obiektu AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

W poniższym przykładzie pokazano sposób określania AccessPolicy dla uprawnienia do odczytu dla danego środka trwałego.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Jeśli kończy się pomyślnie, zwracany jest kod powodzenia 201 opisem jednostki AccessPolicy, który został utworzony. Następnie należy użyć identyfikatora AccessPolicy wraz z identyfikator środka trwałego środka, zawierającą plik, który chcesz dostarczać (na przykład środka trwałego dane wyjściowe) Tworzenie jednostki Locator.

>[AZURE.NOTE]
Podstawowe przepływu pracy jest taka sama, jak przekazywania pliku po ingesting środka trwałego (opisane został wcześniej w tym temacie). Również takich jak przekazywanie plików, jeśli użytkownik (lub klientami) konieczna natychmiast dostęp do plików, wartość usługi Czas rozpoczęcia pięć minut przed bieżącą godzinę. Ta akcja jest konieczne, ponieważ może istnieć zegar skośność między klientem a usługi multimediów. Wartość czas rozpoczęcia musi być w następującym formacie daty i godziny: YYYY-MM-DDTHH:mm:ssZ (na przykład "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Tworzenie skojarzeń zabezpieczeń adresu URL dla pobierania zawartości 

Poniższy kod przedstawia sposób uzyskiwania adresu URL, który może być używany do pobrania pliku multimedialnego utworzone i przekazane wcześniej. Zestaw uprawnień odczytu ma AccessPolicy i ścieżkę Locator odwołuje się do adresu URL pobierania skojarzeń zabezpieczeń.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Zwrócone właściwość **Path** zawiera adres URL skojarzeń zabezpieczeń.

>[AZURE.NOTE]
Jeśli pobierzesz miejsca do magazynowania szyfrowane zawartość, musisz ręcznie odszyfrowywanie go przed wydaniem go, lub za pomocą MediaProcessor odszyfrowywanie miejsca do magazynowania w zadaniu przetwarzanie przetworzone pliki w Wyczyść, aby OutputAsset wyjściowe, a następnie pobrać z tego zasobu. Aby uzyskać więcej informacji na temat przetwarzania Zobacz Tworzenie zadania kodowanie z interfejsu API usługi REST usługi multimediów. Ponadto nie można zaktualizować Locator adresu URL skojarzeń zabezpieczeń, po ich utworzeniu. Na przykład można ponownie użyć samego Locator z zaktualizowaną wartość czas rozpoczęcia. Jest to ze względu na sposób tworzenia skojarzenia zabezpieczeń adresów URL. Jeśli chcesz uzyskać dostęp do zawartości do pobrania, po upływie Locator, musisz utworzyć nową z nowy czas rozpoczęcia.

###<a name="download-files"></a>Pobieranie plików

Po umieszczeniu AccessPolicy, a także ustawić, możesz pobrać pliki przy użyciu API pozostałych Azure miejsca do magazynowania.  

>[AZURE.NOTE] Należy dodać nazwę pliku dla pliku, który chcesz pobrać na wartość Locator **ścieżka** otrzymanych w poprzedniej sekcji. Na przykład https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Aby uzyskać więcej informacji na temat pracy z obiektami blob Azure miejsca do magazynowania zobacz [Interfejsu API usługi REST usługi obiektów Blob](http://msdn.microsoft.com/library/azure/dd135733.aspx).

W wyniku kodowania zadanie, które możesz wykonać wcześniejszych (kodowanie w zestawie adaptacyjne MP4) masz wiele plików MP4, które można pobrać stopniowo. Na przykład:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Tworzenie przesyłanie strumieniowe adresu URL dla strumieniowego przesyłania zawartości


Poniższy kod przedstawia sposób tworzenia przesyłanie strumieniowe Locator adresu URL:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Jeśli kończy się pomyślnie, zwracany jest następującą odpowiedź:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Aby przesyłać strumieniowo wygładzonymi Streaming źródłowy adres URL przy użyciu przesyłanie strumieniowe odtwarzacza multimedialnego, można dołączyć ścieżkę właściwość o nazwie wygładzonymi strumieniowych pojawiają plik, a następnie "/ pojawiają".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Aby przesyłać strumieniowo HLS, należy dołączyć (format = m3u8 aapl) po "/ pojawiają".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Aby przesyłać strumieniowo ŁĄCZNIKA MPEG, Dołącz (format = mpd czasu csf) po "/ pojawiają".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Odtwarzanie zawartości  

Do strumieniowego przesyłania wideo, za pomocą [Usługi Azure odtwarzacza](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Aby przetestować stopniowego pobierania, wklej adres URL w przeglądarce (na przykład programu Internet Explorer, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Następne kroki: Ścieżki szkoleniowe dla użytkowników usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Szukasz czegoś innego?

Jeśli w tym temacie nie zawiera, co możesz zostały oczekiwano, czy nie brakuje czegoś lub w inny sposób nie spełnia określonych wymagań, podaj nam swoją opinię za pomocą wątku Disqus poniżej.



