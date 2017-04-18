<properties 
    pageTitle="Informacje o wersji usługi multimediów | Microsoft Azure" 
    description="Informacje o wersji usługi multimediów" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Informacje o wersji usługi multimediów Azure

Te informacje o wersji podsumowywanie zmiany z poprzednich wersji i znanych problemów.

>[AZURE.NOTE] Chcemy usłyszeć klientów i skoncentrowanie się na rozwiązywanie problemów, których dotyczy. Aby zgłosić problem lub zadawać pytania, opublikuj na [Forum MSDN usługi multimediów Azure].


##<a id="issues"></a>Obecnie znane problemy

### <a id="general_issues"></a>Problemy ogólne usługi multimediów

Problem|Opis
---|---
Kilka typowych nagłówki HTTP nie są dostarczane w interfejsie API usługi REST.|Programiści aplikacji usługi multimediów przy użyciu interfejsu API usługi REST stwierdzisz, że niektóre standardowych pól nagłówków HTTP (łącznie z klienta-żądanie-ID identyfikator ŻĄDANIA i RETURN-klienta-żądanie-ID) nie są obsługiwane. Nagłówki zostaną dodane w przyszłych aktualizacji.
Kodowanie nie jest dozwolone.|Usługi multimediów używa wartości właściwości IAssetFile.Name podczas tworzenia adresy URL dla strumieniowego przesyłania zawartości (na przykład http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowanie nie jest dozwolone. Wartość właściwości **nazwy** nie mogą zawierać następujące [wartości procentowej kodowanie zastrzeżone znaki](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Ponadto może istnieć tylko jeden "." dla rozszerzenia nazwy pliku.
Metoda ListBlobs, który jest częścią 3.x wersji SDK miejsca do magazynowania Azure kończy się niepowodzeniem.|Usługi multimediów generuje skojarzeń zabezpieczeń adresów URL, zależnie od używanej wersji [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) . Za pomocą Azure SDK magazyn obiektów blob listy w kontenerze obiektów blob, należy użyć metodę [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) , która jest częścią Azure SDK przechowywania wersji 2.x. Metoda ListBlobs, która wchodzi w wersji SDK miejsca do magazynowania Azure 3.x nie powiedzie się.
Usługi multimediów ograniczania mechanizmu ogranicza użycie zasobów dla aplikacji, które wprowadzić nadmiarowe żądanie usługi. Usługa może zwrócić kodu stanu HTTP Usługa niedostępna (503).|Aby uzyskać więcej informacji zobacz opis kod stanu HTTP 503 w temacie [Kody błędów usługi multimediów Azure](http://msdn.microsoft.com/library/azure/dn168949.aspx) .
Podczas wysyłania kwerend jednostki, istnieje ograniczenie 1000 jednostek zwracane w tym samym czasie, ponieważ publicznej pozostałych wersji 2 ogranicza wyniki kwerendy do 1000 wyników. | Należy użyć **Pomiń** i **wykonać** (.NET) / **góry** (REST) jako opisanych w [tym przykładzie .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [w tym przykładzie interfejsu API usługi REST](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
Niektórzy klienci mogą pochodzić przez problemem powtarzania znacznika w manifeście Streaming gładkie.|Aby uzyskać więcej informacji zobacz [w tej](media-services-deliver-content-overview.md#known-issues) sekcji.
Obiekty SDK .NET usługi multimediów Azure nie można szeregować i w wyniku nie działa z pamięci podręcznej Azure.|Jeśli spróbujesz szeregować obiektu SDK AssetCollection, aby dodać go do pamięci podręcznej Azure, wyjątek.
Zadania kodowania kończą się niepowodzeniem z ciągiem wiadomość "etap: DownloadFile. Kod: System.NullReferenceException ".|Typowe kodowania przepływ pracy jest przekazywanie plików wideo wprowadzania danych do środka trwałego wprowadzania i Prześlij jedno lub więcej zadań kodowania dotyczących tej wprowadzania trwałych, bez dalszego modyfikowania wprowadzania zasobów. Jednak jeśli zmodyfikować wprowadzania środków trwałych (na przykład przez dodawanie/usuwanie i zmienianie nazw plików w obrębie elementu), kolejne zadania może nie z błędem DownloadFile. Obejście jest usuwanie wprowadzania zasobów i ponownie przekazać pliki wprowadzania danych do nowej zawartości. 

##<a id="rest_version_history"></a>Historia wersji interfejsu API REST

Uzyskać informacji o historii wersji interfejsu API usługi REST usługi multimediów zobacz [Azure multimediów usługi REST API Reference].

##<a id="july_changes16"></a>Wersji 2016 lipca

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Aktualizacje ujawnienia pliku (*. ISM) wygenerowane przez kodowanie zadań

Przesyłanej kodowania zadania do Media Encoder standardowy lub Azure Media Encoder kodowania zadań generuje [Przesyłanie strumieniowe pliku manifestu](media-services-deliver-content-overview.md) (* .ism) plik w wyniku kwerendy zawartości. Z najnowszej wersji usługi została zaktualizowana składnia przesyłanie strumieniowe pliku manifestu.

>[AZURE.NOTE]Składnia przesyłanie strumieniowe pliku manifestu (.ism) jest zarezerwowane do użytku wewnętrznego i może ulec zmianie w przyszłych wersjach. Nie zmodyfikuj lub modyfikować zawartość tego pliku.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nowe manifest klienta (*. Plik ISMC) jest generowany w wyniku kwerendy zawartości, gdy jeden lub więcej plików MP4 wyniki kodowania zadań

Zaczynając od najnowszej wersji usługi po zakończeniu kodowania zadanie, które generuje go więcej plików MP4, wynik trwałego także będzie zawierać pliku strumieniowego manifest (*.ismc) klienta. Plik .ismc pomaga zwiększyć wydajność programu streaming dynamiczne. 

>[AZURE.NOTE]Składnia pliku (.ismc) manifest klienta są zarezerwowane do użytku wewnętrznego i może ulec zmianie w przyszłych wersjach. Nie zmodyfikuj lub modyfikować zawartość tego pliku.

Aby uzyskać więcej informacji zobacz [tym](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Znane problemy

Niektórzy klienci mogą pochodzić wprowadzono problemem powtarzania znacznika w manifeście Streaming gładkie. Aby uzyskać więcej informacji zobacz [w tej](media-services-deliver-content-overview.md#known-issues) sekcji.

##<a id="apr_changes16"></a>Wersji 2016 kwietnia

### <a name="azure-media-analytics"></a>Analizy multimediów Azure

Azure Servces multimediów wprowadzona analizy multimediów Azure dla zaawansowanych analiz wideo. Aby uzyskać szczegółowe informacje zobacz [Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (wersja Preview)

Usługi multimediów Azure umożliwia teraz dynamicznie szyfrowanie usługi HTTP Live Streaming (HLS) zawartość z FairPlay firmy Apple. Za pomocą usługi AMS licencji do przeprowadzania FairPlay licencji dla klientów. Aby uzyskać bardziej szczegółowe informacje zobacz [usługi multimediów Azure Użyj do strumieniowego przesyłania usługi HLS zawartości chronionego z FairPlay firmy Apple ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Wersji 2016 luty

Najnowsza wersja pakietu SDK usługi multimediów Azure dla środowiska .NET (3.5.3) zawiera poprawkę błędu powiązanych Widevine. Problem został: AssetDeliveryPolicy nie można ponownie dla wielu trwałych zaszyfrowanych za pomocą Widevine. W ramach tej poprawki błędów następujące właściwości został dodany do zestawu SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Wersji stycznia 2016

Kodowanie zastrzeżone jednostki, której nazwę zmieniono, aby uniknąć pomyłek z nazwami kodera.

Podstawowe, Standard i Premium jednostki zastrzeżone kodowania są zmieniane na S1, S2, i S3 odpowiednio zastrzeżone jednostki.  Klienci korzystający z podstawowe RUs kodowanie dzisiaj są widoczne S1 jako etykiety Azure Portal (a w zestawieniu), podczas standardowe a Premium zostaną wyświetlone etykiety S2 i S3 odpowiednio. 

##<a id="dec_changes_15"></a>Wersji grudnia 2015 r.

Zespołu Azure SDK opublikowana nowa wersja pakiet [SDK Azure dla PHP](http://github.com/Azure/azure-sdk-for-php) , który zawiera aktualizacje i nowe funkcje dla usługi multimediów Azure firmy Microsoft. W szczególności SDK usługi multimediów Azure dla PHP obsługuje teraz najnowszych funkcji [ochrony zawartości](media-services-content-protection-overview.md) : dynamiczne szyfrowania AES i DRM (PlayReady i Widevine) z i bez ograniczeń Token. Obsługuje ona również skalowania [Kodowanie jednostki](media-services-dotnet-encoding-units.md).

Aby uzyskać więcej informacji zobacz:

- [Zestaw SDK usługi multimediów Azure firmy Microsoft dla PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogu.
- Szybkie następujące [Przykłady kodu](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) , które ułatwiają rozpoczęcie pracy:
    - **vodworkflow_aes.php**: jest to plik PHP, która pokazuje, jak używać szyfrowania dynamiczne AES-128 i usługa klucza. Jest on oparty na próbce .NET opisano w [tym](media-services-protect-with-aes128.md) artykule.
    - **vodworkflow_aes.php**: jest to plik PHP, która pokazuje, jak używać szyfrowania dynamiczne PlayReady i usługa dostarczania licencji. Jest on oparty na próbce .NET opisano w [tym](media-services-protect-with-drm.md) artykule.
    - **scale_encoding_units.php**: jest to plik PHP, przedstawiający sposobu skalowania kodowania zastrzeżone jednostek.


##<a id="nov_changes_15"></a>Wersji listopada 2015 r.

Usługi multimediów Azure oferuje Google Widevine licencji usługi w chmurze. Aby uzyskać więcej informacji zapoznaj się z [tym blogu zawiadomienie o](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Zobacz też [tego samouczka](media-services-protect-with-drm.md) i [GitHub repozytorium](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Należy zauważyć, że Widevine licencji dostarczania usług udostępnianych przez usług w transporcie multimediów Azure w podglądzie. Aby uzyskać więcej informacji, zobacz [tym blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Wersji października 2015 r.

Usługi multimediów Azure (AMS) jest teraz live w następujących centrach danych: Południowej Brazylia, zachód Indie, Indie południe i Indie centralnej. Można teraz korzystać z portalu klasyczny Azure do [tworzenia kont usługi multimediów](media-services-portal-create-account.md) i wykonywać różne zadania opisane [w tym miejscu](https://azure.microsoft.com/documentation/services/media-services/). Jednak Live kodowanie nie jest włączona w następujących centrach danych. Ponadto nie wszystkie typy jednostek zastrzeżone kodowanie są dostępne w tych centrach danych.

- Brazylia południe: Tylko standardowe i podstawowe kodowanie zastrzeżone jednostki są dostępne
- Indie Zachód, Indie południe i Indie centralnej: dostępne są tylko podstawowe kodowanie zastrzeżone jednostki


##<a id="september_changes_15"></a>Wersji września 2015 r. 

- AMS oferuje możliwość ochrony zarówno usługi wideo na żądanie (VOD), jak i strumieni na żywo z technologią DRM moduły Widevine. Za pomocą następujących partnerów usługi dostarczania ułatwiające przeprowadzania licencji Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Aby uzyskać więcej informacji zobacz [tym blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (począwszy od wersji 3.5.1) lub interfejsu API usługi REST służy do konfigurowania programu AssetDeliveryConfiguration, aby użyć Widevine.  

- AMS dodane obsługę Apple ProRes wideo. Teraz możesz przekazać plików wideo QuickTime źródła używających Apple ProRes lub inne kodery-dekodery. Aby uzyskać więcej informacji zobacz [tym blogu](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Teraz można Media Encoder standardowy wykonaj częściowej wycinek i rzeczywistych wyodrębniania archiwum. Aby uzyskać więcej informacji zobacz [tym blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Wprowadzono następujące aktualizacje filtrowania: 

    - Można teraz Użyj formatu Apple HTTP Live Streaming (HLS) z zastosowanym filtrem tylko audio. Ta aktualizacja umożliwia usunięcie śledzenie tylko audio, określając (tylko audio = false) w adresie URL.
    - Podczas definiowania filtrów dla aktywów, mają teraz możliwość połączenia wielokrotności (maksymalnie 3) filtrów w jednym adresu URL.

    Aby uzyskać więcej informacji, zobacz [tym](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

- AMS obsługuje teraz — ramki w HLS 4. Pomoc techniczna — ramki optymalizuje operacje szybkie przewijanie do przodu i do tyłu. Domyślnie wszystkie wyjściowe 4 HLS zawierają odtwarzania ramki (EXT-X-I-FRAME-STREAM-INF).
 
    Aby uzyskać więcej informacji, zobacz [tym](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

##<a id="august_changes_15"></a>Wersji sierpnia 2015 r.

- Azure SDK usługi multimediów dla wersji Java V0.8.0 i nowe przykłady są teraz dostępne. Aby uzyskać więcej informacji zobacz:

    - [Wpis w blogu](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Repozytorium próbki Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure aktualizację Media Player z obsługą strumieniu wielokrotnego audio. Aby uzyskać więcej informacji zobacz:
    - [Wpis w blogu](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Wersji lipca 2015 r.

- Informowanie o ogólnodostępną Media Encoder standardu. Aby uzyskać więcej informacji zobacz [Ten wpis w blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder standardowy korzysta z ustawień wstępnych opisane w [tej](http://go.microsoft.com/fwlink/?LinkId=618336) sekcji. Należy zauważyć, że po przy użyciu ustawienia domyślne dla 4k kodowane, otrzymasz typ jednostki **Premium** zastrzeżone. Aby uzyskać więcej informacji zobacz [jak kodowanie Skala](media-services-scale-media-processing-overview.md).
- Live podpisów w czasie rzeczywistym przy użyciu usługi multimediów Azure i odtwarzacza. Aby uzyskać więcej informacji zobacz [Ten wpis w blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>.NET SDK aktualizacji usługi multimediów

Azure multimediów usług .NET SDK jest teraz wersji 3.4.0.0. Następujące funkcje został dodany w tej wersji:  

- Obsługa wdrożony live archiwum. Należy zauważyć, że nie można pobrać środka trwałego, zawierający live archiwum.
- Obsługa wdrożony dynamiczne filtry.
- Funkcja wdrożony pozwala zachować kontenera magazynu podczas usuwania zasobów.
- Poprawki związane z ponów próbę zasad w kanałach.
- Włączone **Media Encoder Premium w przepływu pracy**.

##<a id="june_changes_15"></a>Wersji czerwca 2015 r.

###<a name="media-services-net-sdk-updates"></a>.NET SDK aktualizacji usługi multimediów

Azure multimediów usług .NET SDK jest teraz wersję 3.3.0.0. Następujące funkcje został dodany w tej wersji:  

- Obsługa Specyfikacja odnajdowania łączenie OpenId
- obsługę najazd klawiszy na stronie dostawcy tożsamości. 

Jeśli korzystasz z dostawcą tożsamości, która udostępnia OpenID Łączenie dokumentu odnajdowania (jak następujących dostawców: usługi Azure Active Directory, Google, Salesforce), polecić usługi multimediów Azure uzyskanie klucze podpisywania dla sprawdzania poprawności tokenu JWT z OpenID połączenia Specyfikacja odnajdowania. 

Aby uzyskać więcej informacji zobacz [Za pomocą klawiszy sieci Web Json z połączenia OpenID Specyfikacja odnajdowania do pracy z JWT token uwierzytelniania usługi multimediów Azure](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Wersji maj 2015 r.

Informowanie o następujące nowe funkcje:

- [Podgląd na żywo kodowania z usługi multimediów](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamiczne manifest](media-services-dynamic-manifest-overview.md)
- [Podgląd procesor multimediów Hyperlapse multimediów Azure](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Wersji kwietnia 2015 r.

 ###<a name="general-media-services-updates"></a>Aktualizacje usług multimediów ogólne

- [Informowanie o Azure odtwarzacza](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Zaczynając od 2.10 multimediów do pozostałych usług, kanały, które są skonfigurowane do mogły zjeść tej ostatniej protokołu RTMP są tworzone z głównego i pomocniczego mogły zjeść tej ostatniej adresy URL. Aby uzyskać więcej informacji zobacz [kanału mogły zjeść tej ostatniej konfiguracji](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Aktualizacje indeksatora multimediów Azure
- Obsługa języka hiszpańskiego
- Nowy format xml konfiguracji

Aby uzyskać więcej informacji, zobacz [tym blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>.NET SDK aktualizacji usługi multimediów

Azure multimediów usług .NET SDK jest teraz wersję 3.2.0.0.

Poniżej przedstawiono niektóre klienta przeciwległych aktualizacji:

- **Przerywanie Zmień**: zmieniono **TokenRestrictionTemplate.Issuer** i **TokenRestrictionTemplate.Audience** typu ciąg.
- Aktualizacje dotyczące tworzenia niestandardowych ponowienia zasady.
- Poprawki dotyczące przekazywania i pobierania plików.
- Klasy **MediaServicesCredentials** akceptuje punkt końcowy sterowania głównego i pomocniczego dostęp do uwierzytelniania.



##<a id="march_changes_15"></a>Wersji marca 2015 r.

### <a name="general-media-services-updates"></a>Aktualizacje usług multimediów ogólne

- Usługi multimediów zapewnia integrację Azure CDN. Aby obsługiwać integracji, właściwość **CdnEnabled** został dodany do **StreamingEndpoint**.  **CdnEnabled** mogą być używane z pozostałych interfejsy API począwszy od wersji 2.9 (Aby uzyskać więcej informacji, zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** mogą być używane z .NET SDK począwszy od wersji 3.1.0.2 (Aby uzyskać więcej informacji, zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Zawiadomienie o przepływu pracy, **Media Encoder Premium**. Aby uzyskać więcej informacji zobacz [Wprowadzenie do Premium kodowanie w usługi multimediów Azure](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Wersji lutego 2015 r.

### <a name="general-media-services-updates"></a>Aktualizacje usług multimediów ogólne

Interfejsu API usługi REST usługi multimediów jest teraz wersji 2.9. Począwszy od tej wersji, możesz włączyć integrację Azure CDN z streaming punkty końcowe. Aby uzyskać więcej informacji zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Wersji stycznia 2015 r.

### <a name="general-media-services-updates"></a>Aktualizacje usług multimediów ogólne

Zawiadomienie o z (GA, General Availability) ochrony zawartości z dynamicznego szyfrowania. Aby uzyskać więcej informacji zobacz [usługi multimediów Azure usprawnia przesyłanie strumieniowe zabezpieczeń z technologią ogólne dostępność DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>.NET SDK aktualizacji usługi multimediów

Azure multimediów usług .NET SDK jest teraz wersję 3.1.0.1.

Ta wersja oznaczone Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Konstruktor jako przestarzałego. Konstruktora new trwa TokenType jako argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Wersja grudnia 2014

###<a name="general-media-services-updates"></a>Aktualizacje usług multimediów ogólne

- Niektóre aktualizacje i nowe funkcje zostały dodane do procesora Azure indeksatora multimediów. Aby uzyskać więcej informacji zobacz [Wersję indeksatora multimediów Azure 1.1.6.7 informacje o wersji](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Dodane nowego interfejsu API usługi REST, która umożliwia aktualizowanie kodowanie zastrzeżone jednostki: [EncodingReservedUnitType z pozostałych](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Dodano CORS pomocy technicznej dotyczącej usługi dostarczania klucza.
- Ulepszenia dotyczące wydajności części kwerendy opcje zasad autoryzacji były wykonywane.
- W Chinach centrum danych, adres [URL dostarczenia klucza](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) jest teraz na klienta (podobnie jak w innych centrach danych).
- Dodano HLS automatyczne docelowej czas trwania. W trakcie live streaming, HLS zawsze jest dostarczana dynamiczne. Domyślnie usługi multimediów oblicza automatycznie na podstawie kluczową interwału (KeyFrameInterval), nazywane także grupowanie obrazów — GOP, odbieranego z kodera Live HLS części stosunek opakowań (FragmentsPerSegment). Aby uzyskać więcej informacji zobacz [Praca z Azure usług Live strumieniowego przesyłania].
 
###<a name="media-services-net-sdk-updates"></a>.NET SDK aktualizacji usługi multimediów

- [Usługi multimediów Azure zestaw SDK .NET](http://www.nuget.org/packages/windowsazure.mediaservices/) jest teraz wersję 3.1.0.0.
- Uaktualnione współzależności .net SDK do .NET Framework 4,5.
- Dodany nowy interfejs API, który umożliwia aktualizowanie kodowania zastrzeżone jednostek. Aby uzyskać więcej informacji zobacz [Aktualizowanie zastrzeżone typ jednostki i zwiększanie RUs kodowanie za pomocą .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Dodano JWT (JSON Web Token) obsługę token uwierzytelniania. Aby uzyskać więcej informacji zobacz [JWT token uwierzytelniania usługi multimediów Azure i szyfrowania dynamiczne](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Dodano względne przesunięcia BeginDate i ExpirationDate w szablonie licencji PlayReady.


##<a id="november_changes_14"></a>Wersja z listopada 2014

- Usługi multimediów umożliwia teraz mogły zjeść tej ostatniej wygładzonymi Streaming (FMP4) zawartości na żywo za pośrednictwem połączenia SSL. Aby mogły zjeść tej ostatniej przez SSL, upewnij się zaktualizować adres URL ingest na HTTPS.  Aby uzyskać więcej informacji na temat przesyłanie strumieniowe na żywo, zobacz [Praca z Azure usługi Live strumieniowego przesyłania].
- Należy zauważyć, że obecnie, możesz nie mogły zjeść tej ostatniej strumienia RTMP za pośrednictwem połączenia SSL.
- Możesz również przesyłać strumieniowo zawartości za pośrednictwem połączenia SSL. Aby to zrobić, upewnij się, że przesyłanie strumieniowe adresami URL początkowych HTTPS.
- Zauważ, że możesz tylko przesyłać strumieniowo przez SSL Jeśli przesyłanie strumieniowe punktu końcowego, z której dostarczyć zawartość została utworzona po 10 września 2014 r. Jeśli przesyłanie strumieniowe adresami URL są oparte na przesyłanie strumieniowe punkty końcowe utworzone po 10 września, adres URL zawiera "streaming.mediaservices.windows.net" (nowy format). Przesyłanie strumieniowe adresy URL, które zawierają "origin.mediaservices.windows.net" (stary format) nie obsługują protokołu SSL. Jeśli adres URL jest w starym formacie i chcesz mieć możliwość strumieniowego przesyłania przez SSL, [utworzyć nowy punkt końcowy przesyłanie strumieniowe](media-services-portal-manage-streaming-endpoints.md). Za pomocą adresy utworzone w oparciu o nowy punkt końcowy przesyłanie strumieniowe przesyłanie strumieniowe zawartości przez SSL.

##<a id="october_changes_14"></a>Wersja października 2014

### <a id="new_encoder_release"></a>Usługi Media Encoder wersji

Informowanie o nowej wersji multimediów usług Azure Media Encoder. Z najnowszymi Azure Media Encoder tylko obciążona opłatą za wyjściowy GB, ale w przeciwnym razie nowy koder jest zgodny z poprzedniego encoder funkcji. Aby uzyskać więcej informacji [Szczegóły cennik usługi multimediów]).

### <a id="oct_sdk"></a>Usługi multimediów .NET SDK 

Zestaw SDK usługi multimediów dla rozszerzenia .NET jest teraz wersji 2.0.0.3.

Media SDK usług dla środowiska .NET jest teraz wersji 3.0.0.8.

Wprowadzono następujące zmiany:

- Refaktoryzacja przedmiotów zasad ponów próbę.
- Dodawanie agenta użytkownika do nagłówków żądania http.
- Dodawanie kroku kompilacji przywracania nuget.
- Ustalanie scenariusz badań x509 za pomocą certyfikatu z repozytorium.
- Po zakończeniu uaktualniania kanału i przesyłanie strumieniowe sprawdzania poprawności ustawień.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Nowe repozytorium GitHub do hosta usługi multimediów próbek

Przykłady znajdują się w [repozytorium GitHub próbki usługi multimediów Azure](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Wersja września 2014

Metadane pozostałych usług multimediów jest teraz wersji 2.7. Aby uzyskać więcej informacji o najnowszych aktualizacjach pozostałych zobacz [Azure multimediów usług pozostałych interfejsu API].

Media SDK usług dla środowiska .NET jest teraz wersji 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Przerywanie zmian

* **Origin** zmieniono na [StreamingEndpoint].
* Zmiany w domyślne zachowanie podczas przy użyciu **Klasycznej Portal Azure** kodowanie, a następnie opublikować pliki MP4.

Wcześniej, gdy za pomocą portalu klasyczny Azure publikowanie zawartości wideo MP4 adresu URL skojarzeń zabezpieczeń Jednoplikowa prawdopodobnie zostanie utworzona (adresy URL skojarzeń zabezpieczeń umożliwiają pobieranie klipu wideo z magazynem obiektów blob). Obecnie gdy Portal klasyczny Azure umożliwia kodowanie, a następnie opublikować Jednoplikowa zawartości wideo MP4, wygenerowane adres URL wskazuje streaming punktu końcowego usługi multimediów Azure.  Ta zmiana nie wpływa na wideo MP4, bezpośrednio przekazane do usługi multimediów i opublikowanych bez jest kodowane przy usługi multimediów Azure.

Obecnie masz dwóch poniższych opcji, aby rozwiązać ten problem.

* Włącz przesyłanie strumieniowe jednostki i używanie dynamiczne opakowań do strumieniowego przesyłania zawartości MP4 wygładzonymi prezentacji przesyłanie strumieniowe.

* Tworzenie skojarzeń zabezpieczeń adresu url do pobrania (lub stopniowo odtwarzanie) MP4. Aby uzyskać więcej informacji na temat tworzenia locator skojarzeń zabezpieczeń zobacz [Dostarczania zawartości].


### <a id="sept_14_GA_changes"></a>Zwolnij nowych funkcji i scenariuszy, które są częścią GA

* **Procesor multimediów indeksowania**. Aby uzyskać więcej informacji, zobacz [Indeksowania plików multimedialnych z indeksatora multimediów Azure].

* Jednostka [StreamingEndpoint] umożliwia teraz Dodaj nazwy domeny niestandardowej (hosta).

    Dla niestandardowej nazwy domeny może być używany jako nazwa punktu końcowego przesyłanie strumieniowe usługi multimediów musisz dodać nazwy hostów niestandardowych do przesyłania strumieniowego punktu końcowego. Służy do dodawania nazwy hostów niestandardowych interfejsy API pozostałych usług multimediów lub .NET SDK.
    
    Następujące kwestie:
    
    * Musisz mieć praw własności niestandardowej nazwy domeny.
    
    * Własności nazwy domeny musi być sprawdzone przez usługi multimediów Azure. Aby sprawdzić poprawność domeny, Utwórz CName mapowany <MediaServicesAccountId>.<parent domain> Aby verifydns. < ruch strefy mediaservices dns >. 
    
    * Musisz utworzyć innego CName, który mapy nazwa hosta niestandardowej (na przykład sports.contoso.com) do nazwy hosta StreamingEndpont usługi multimediów (na przykład amstest.streaming.mediaservices.windows.net).


    Aby uzyskać więcej informacji zobacz właściwość **CustomHostNames** w temacie [StreamingEndpoint] .

### <a id="sept_14_preview_changes"></a>Nowe funkcje scenariuszy, które są częścią wersji preview publicznej

* Przesyłanie strumieniowe Podgląd na żywo. Aby uzyskać więcej informacji zobacz [Praca z Azure usługi Live strumieniowego przesyłania].

* Usługa klucza. Aby uzyskać więcej informacji zobacz [przy użyciu szyfrowania dynamiczne AES-128 i klucza usługi].

* Dynamiczne szyfrowania AES. Aby uzyskać więcej informacji zobacz [przy użyciu szyfrowania dynamiczne AES-128 i klucza usługi].

* Usługa dostarczania PlayReady licencji. Aby uzyskać więcej informacji zobacz [przy użyciu szyfrowania dynamiczne PlayReady i usługa dostarczania licencji].

* Szyfrowanie PlayReady dynamiczne. Aby uzyskać więcej informacji zobacz [przy użyciu szyfrowania dynamiczne PlayReady i usługa dostarczania licencji].

* Szablon PlayReady licencji usługi multimediów. Uzyskać więcej informacji zobacz [Omówienie szablonu licencji PlayReady usługi multimediów].

* Przesyłanie strumieniowe miejsca do magazynowania szyfrowane aktywów. Aby uzyskać więcej informacji zobacz [Zawartości szyfrowane Streaming miejsca do magazynowania].

##<a id="august_changes_14"></a>Wersji sierpnia 2014 r.

Podczas kodowania środka trwałego środka trwałego wynik jest tworzony po zakończeniu kodowania zadania. Poczekaj, aż tej wersji koder usługi multimediów Azure wyprodukowano metadanych dotyczących składników majątku dane wyjściowe. Począwszy od tej wersji kodera również daje w wyniku metadanych dotyczących wprowadzania aktywów. Aby uzyskać więcej informacji zobacz tematy [Metadanych dane wejściowe] i [Wyjściowe metadanych] .


##<a id="july_changes_14"></a>Wersja z lipca 2014

Wprowadzono następujące poprawki Pakowarki usługi multimediów Azure i szyfrujący:

* Tylko dźwięk jest odtwarzany po transmuxing zasób live archiwum w HTTP Live Streaming — ten problem usunięto i teraz odtwarzać audio i wideo.

* Gdy opakowania środka trwałego do Live przesyłania strumieniowego HTTP i szyfrowania AES 128-bitowego koperty, detaliczny strumieni nie są odtwarzane na urządzeniach z systemem Android — ten problem został rozwiązany i detaliczny strumienia odtwarzane na urządzeniach z systemem Android, obsługujące HTTP Live strumieniowego przesyłania.

##<a id="may_changes_14"></a>Wersja z maja 2014

### <a id="may_14_changes"></a>Aktualizacje usług multimediów ogólne

Teraz można [Dynamiczne opakowań] strumienia v3 HTTP Live Streaming (HLS). Do strumienia HLS v3, Dodaj następujący format do ścieżki locator origin: * .ism/manifest(format=m3u8-aapl-v3). Aby uzyskać więcej informacji zobacz [Blog Nick Drouin].

Dynamiczne opakowań teraz również obsługuje przedstawiania HLS (3 i 4) zaszyfrowanych za pomocą PlayReady oparte na gładkie Streaming statycznie zaszyfrowanych za pomocą PlayReady. Aby uzyskać informacje na temat szyfrowania wygładzonymi Streaming z PlayReady zobacz [Ochrona wygładzonymi strumienia z PlayReady].

### <a name="may_14_donnet_changes"></a>.NET SDK aktualizacji usługi multimediów

Następujące ulepszenia są uwzględniane w wersji Media usług .NET SDK 3.0.0.5:

* Lepsze szybkości i odporność multimedialnych przekazywania i pobierania.

* Ulepszenia w ponów próbę logicznych i zmiennych obsługi wyjątków: 

    * Logika wykrywania i spróbuj ponownie przejściowych błędu zostały poprawione wyjątki wynikające z kwerendy, zapisywanie zmian, przekazywania lub pobierania plików. 
    
    * Podczas pobierania wyjątków w sieci Web (na przykład podczas żądania tokenu ACS), można zauważyć, że błędy krytyczne występują szybciej teraz.

Aby uzyskać więcej informacji zobacz [Ponów próbę logicznych w SDK usługi multimediów dla środowiska .NET].

##<a id="april_changes_14"></a>Wersja kodera kwietnia 2014

### <a name="april_14_enocer_changes"></a>Media Encoder aktualizacje usług

* Dodano obsługę ingesting pliki AVI utworzony przy użyciu edytora nieliniowa EDIUS doliny trawy, gdzie klip wideo jest lekko skompresowane, za pomocą doliny trawy HQ-HQX koder-dekoder. Aby uzyskać więcej informacji zobacz [Trawy doliny informuje o EDIUS 7 Streaming do chmury].

* Dodano obsługę określającą konwencję nazewnictwa dla pliki tworzone przez Media Encoder. Aby uzyskać więcej informacji zobacz [Sterowanie multimediami usługi Encoder nazw plików wyjściowych].

* Dodano obsługę nakładki wideo lub audio. Aby uzyskać więcej informacji zobacz [Tworzenie nakładki].

* Dodano obsługę łączenia ze sobą wiele segmentów wideo. Aby uzyskać więcej informacji zobacz [Łączenia segmentów wideo].

* Stałe błędu związane z przekodowanie MP4s miejsce, w którym został zakodowany plik audio z MPEG-1 Audio layer 3 (czyli MP3).


##<a id="jan_feb_changes_14"></a>Styczeń luty 2014 wersjach

### <a name="jan_fab_14_donnet_changes"></a>Usługi multimediów Azure SDK .NET 3.0.0.1, 3.0.0.2 i 3.0.0.3

Zmiany w 3.0.0.1 i 3.0.0.2 obejmują:

* Stały problemy związane z zastosowania zapytań LINQ OrderBy instrukcji.

* Podziel test rozwiązania w [GitHub] na podstawie jednostki badania i badania oparte na scenariuszach.

Aby uzyskać więcej informacji na temat możliwych zmian, zobacz: [udostępnia Azure multimediów usług .NET SDK 3.0.0.1 i 3.0.0.2].

W 3.0.0.3 wprowadzono następujące zmiany:

* Uaktualnione zależności magazynu Azure przy użyciu wersji 3.0.3.0. 

* Problem zgodności z poprzednimi wersjami stały 3.0. *.* aktualizacje. 


##<a id="december_changes_13"></a>Wersji grudnia 2013 r

### <a name="dec_13_donnet_changes"></a>Usługi multimediów Azure .NET SDK 3.0.0.0

>[AZURE.NOTE] wersje 3.0.x.x nie są zgodne z poprzednimi wersjami w wersjach 2.4.x.x.

Najnowszą wersję pakietu SDK usługi multimediów jest teraz 3.0.0.0. Można pobrać najnowszy pakiet Nuget lub uzyskaj bity z [GitHub].

Począwszy od SDK usługi multimediów wersji 3.0.0.0, można ponownie użyć tokeny [Azure Active Directory programu Access sterowania ACS (Service)] . Aby uzyskać więcej informacji zobacz sekcję "Ponowne używanie programu Access Control usługi tokeny" w temacie [Łączenie się z usługi multimediów z Media SDK usług dla środowiska .NET] .

### <a name="dec_13_donnet_ext_changes"></a>Usługi multimediów Azure rozszerzenia SDK .NET 2.0.0.0

Rozszerzenia SDK .NET usługi multimediów Azure to zestaw funkcji pomocy, które będą Uprość kod i ułatwić rozwinąć za pomocą usługi multimediów Azure i metody rozszerzenia. Możesz uzyskać najnowszą bitów z [Rozszerzenia SDK .NET usług multimediów Azure].

##<a id="november_changes_13"></a>Wersji listopada 2013

### <a name="nov_13_donnet_changes"></a>Usługi multimediów Azure zmiany SDK .NET

Począwszy od tej wersji, Media SDK usług dla środowiska .NET obsługuje błędy przejściowych błędów, które mogą wystąpić podczas nawiązywania połączenia do warstwy interfejsu API usługi REST usługi multimediów.

##<a id="august_changes_13"></a>Wersji sierpień 2013

### <a name="aug_13_powershell_changes"></a>Multimedia usług poleceń cmdlet wchodzi w skład narzędzi Sdk Azure

Następujące polecenia cmdlet programu PowerShell usługi multimediów znajdują się teraz w [Narzędzia w przypadku zestawu sdk azure].

* Get-AzureMediaServices 

    Na przykład `Get-AzureMediaServicesAccount`.

* Nowy AzureMediaServicesAccount 

    Na przykład `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Nowy AzureMediaServicesKey 

    Na przykład `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Usuń AzureMediaServicesAccount 

    Na przykład `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Wersji czerwca 2013

### <a name="june_13_general_changes"></a>Zmiany usługi multimediów Azure

Zmiany wymienione w tej sekcji są aktualizacje zawarte w wersjach usługi multimediów czerwca 2013.

* Możliwość łączenie wielu kont miejsca do magazynowania z konta usługi multimediów. 

    StorageAccount
    
    Asset.StorageAccountName i Asset.StorageAccount

* Możliwość aktualizacji Job.Priority. 

* Powiadomienie dotyczące obiektów i właściwości: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Zadanie

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Zmiany SDK .NET usługi multimediów Azure

Następujące zmiany są uwzględniane w czerwca 2013 udostępnia SDK usługi multimediów. Najnowsze SDK usługi multimediów jest dostępna na GitHub.

* Obsługa SDK usługi multimediów łączenie wielu miejsca do magazynowania począwszy od wersji 2.3.0.0 konta do konta usługi multimediów. Następujące obsługują tę funkcję:
    
    Typ IStorageAccount.
    
    Właściwość Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    Właściwość StorageAccount.
    
    Właściwość StorageAccountName.
    
    Aby uzyskać więcej informacji zobacz [Zarządzanie zasoby multimedialne usług między wieloma kontami miejsca do magazynowania].

* Powiadomienie dotyczące interfejsów API. Począwszy od wersji 2.2.0.0, że masz możliwość odsłuchiwanie kolejki Azure miejsca do magazynowania powiadomienia. Aby uzyskać więcej informacji, zobacz [Obsługa powiadomień zadania usługi multimediów].
    
    Właściwość Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* Zależność od klienta Azure magazynowania SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Zależność od OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Wersji grudniem 2012

### <a name="dec_12_dotnet_changes"></a>Zmiany SDK .NET usługi multimediów Azure

* IntelliSense: Dodane brakujące dokumentacji Intellisense dla wielu typów.

* Microsoft.Practices.TransientFaultHandling.Core: Rozwiązanie problemu związanego, gdy zestawu SDK nadal miały zależność od starszej wersji tego zestawu. Zestaw SDK odwołuje się teraz wersję 5.1.1209.1 tego zestawu.

Rozwiązania problemów dotyczących znaleziony w listopad 2012 SDK:

* IAsset.Locators.Count: Liczba ta jest teraz prawidłowo zgłoszone na nowe interfejsy IAsset po usunięciu wszystkich Locator.

* IAssetFile.ContentFileSize: Wartość ta jest teraz poprawnie ustawiona po przesłaniu przez IAssetFile.Upload(filepath).

* IAssetFile.ContentFileSize: Ta właściwość teraz można ustawić, podczas tworzenia pliku zasobów. Była wcześniej tylko do odczytu.

* IAssetFile.Upload(filepath): Rozwiązanie problemu związanego, w której ta metoda przekazywania obraz został generowania następujący komunikat o błędzie przekazywanie wielu plików do elementu. Błąd "nie można uwierzytelnić żądania serwer. Upewnij się, że wartość nagłówka autoryzacji został utworzony poprawnie w tym podpis. "

* IAssetFile.UploadAsync: Rozwiązanie problemu związanego, w którym nie więcej niż 5 plików można przekazać jednocześnie.

* IAssetFile.UploadProgressChanged: To zdarzenie jest teraz udostępniany przez zestawu SDK.

* IAssetFile.DownloadAsync (ciąg, BlobTransferClient, ILocator, CancellationToken): Ta metoda przeciążeń znajduje się teraz.

* IAssetFile.DownloadAsync: Rozwiązanie problemu związanego miejsce, w którym nie więcej niż 5 plików można pobrać jednocześnie.

* IAssetFile.Delete(): Rozwiązanie problemu związanego, gdzie Usuń połączeń może zostać zgłoszony wyjątek Jeśli plik nie został przekazany dla IAssetFile.

* Zadania: Rozwiązanie problemu związanego miejsce, w którym łączenia "MP4 wygładzonymi strumienie zadania" z "Zadania ochrony PlayReady" przy użyciu szablonu zadania nie spowodować żadnych zadań w ogóle.

* EncryptionUtils.GetCertificateFromStore(): Ta metoda nie jest już zgłasza wyjątek odwołania zerowego ze względu na błędy, znajdowania certyfikatu opartego na problemy z konfiguracją certyfikatu.

##<a id="november_changes_12"></a>Wersji listopad 2012

Zmiany, wymienione w tej sekcji zostały aktualizacje zawarte w listopad 2012 (wersja 2.0.0.0) SDK. Te zmiany mogą wymagać dowolnego kodu napisanego Podgląd czerwca 2012 SDK Zwolnij do modyfikacji lub napisać od nowa.

* Składniki majątku
    
    IAsset.Create(assetName) jest funkcją tworzenia zawartości tylko. IAsset.Create nie jest już wysyła pliki w ramach połączenia metody. Za pomocą IAssetFile pobierania.
    
    Metoda IAsset.Publish i wartość wyliczenia AssetState.Publish zostały usunięte z zestawu SDK usług. Cały kod, który zależy od tej wartości musi być ponownie pisanych.

* FileInfo

    Ta klasa została usunięta i zastępuje IAssetFile.

    IAssetFiles

    IAssetFile zastępuje FileInfo i działa inaczej. Aby użyć go, wystąpienia obiektu IAssetFiles, a po niej przekazywania pliku za pomocą SDK usługi multimediów albo Azure SDK miejsca do magazynowania. Można używać następujących overloads IAssetFile.Upload:

    * IAssetFile.Upload(filePath): Synchroniczne metoda, która blokuje wątku i jest zalecane tylko wtedy, gdy przekazywanie jednego pliku.
    
    * IAssetFile.UploadAsync (ścieżka pliku, blobTransferClient, locator, cancellationToken): metod asynchronicznych. Jest to mechanizm preferowanej przekazywania. 

    Znane błędu: za pomocą cancellationToken faktycznie anulować przekazywanie; jednak stan anulowanie zadań może być dowolna z liczba stanów. Należy poprawnie efektywnej i obsługiwać wyjątki.

* Locator
    
    Usunięto wersjach pochodzenie specyficznych. Kontekst specyficzne dla skojarzeń zabezpieczeń. Locators.CreateSasLocator (zawartości, accessPolicy) zostaną oznaczone uznane za przestarzałe lub usunięta przez GA. W sekcji Locator w obszarze nowe funkcje zachowanie zaktualizowane.


##<a id="june_changes_12"></a>Wersji Preview czerwca 2012

Następujące funkcje jest nowego w wersji listopada zestawu SDK.

* Usuwanie obiektów

    IAsset, IAssetFile, ILocator, IAccessPolicy, obiekty IContentKey teraz są usuwane na poziomie obiektu, to znaczy IObject.Delete() zamiast Usuń w kolekcji, która jest cloudMediaContext.ObjCollection.Delete(objInstance).

* Locator

    Locator muszą zostać utworzone za pomocą metody CreateLocator i używanie wartości wyliczenia LocatorType.SAS lub LocatorType.OnDemandOrigin jako argumentu dla określonego typu locator, który chcesz utworzyć.

    Dodano nowe właściwości Locator, aby ułatwić uzyskanie użyteczne URI dla zawartości. Ten zmianom Locator miały zapewnić większą elastyczność dla przyszłych rozszerzeń innych firm i zwiększanie ułatwienia użycia w przypadku aplikacji klienckich multimediów.

* Pomoc dotycząca metod asynchronicznych

    Dodano obsługę asynchroniczne do wszystkich metod.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Forum w witrynie MSDN usługi multimediów Azure]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[POZOSTAŁE odwołania interfejsu API usługi multimediów Azure]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Szczegółowe informacje o cenach usługi multimediów]: http://azure.microsoft.com/pricing/details/media-services/
[Wprowadzanie metadanych]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Dane wyjściowe metadanych]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Dostarczania zawartości]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Indeksowanie plików multimedialnych z indeksatora multimediów Azure]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Praca z usługi multimediów Azure przesyłanie strumieniowe na żywo]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Przy użyciu dynamiczne szyfrowania AES 128 i klucza usługi]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Przy użyciu szyfrowania dynamiczne PlayReady i usługa dostarczania licencji]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Omówienie szablonu PlayReady licencji usługi multimediów]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Przesyłanie strumieniowe miejsca do magazynowania szyfrowane zawartości]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamiczne opakowań]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Blog Nick Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Ochrona wygładzonymi strumienia z PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Ponów próbę logicznych w usługach Media SDK dla środowiska .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Doliny trawy informuje o EDIUS 7 Streaming do chmury]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Usług sterowania Media Encoder nazw plików wyjściowych]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Tworzenie nakładki]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Łączenie segmentów wideo]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Wersjach SDK 3.0.0.1 i 3.0.0.2 .NET usługi multimediów Azure]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Usługa Kontrola dostępu usługi Azure Active Directory (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Nawiązywanie połączenia z usługi multimediów z usługi multimediów SDK dla środowiska .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Usługi multimediów Azure rozszerzenia SDK .NET]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[narzędzia w przypadku zestawu sdk Azure]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Zarządzanie nośnikami usług składników majątku między wieloma kontami miejsca do magazynowania]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Obsługa multimediów usług powiadomienia zadania]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
