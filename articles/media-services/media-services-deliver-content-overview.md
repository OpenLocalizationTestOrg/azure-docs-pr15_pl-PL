<properties
    pageTitle="Dostarczania zawartości klientom | Microsoft Azure"
    description="Ten temat zawiera omówienie co należy zrobić w dostarczania zawartości przy użyciu usługi multimediów Azure."
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


# <a name="deliver-content-to-customers"></a>Dostarczania zawartości dla klientów
Podczas przedstawiania zawartości przesyłanie strumieniowe lub usługi wideo na żądanie dla klientów, celem jest dostarczanie wideo o wysokiej jakości różnych urządzeń warunkach innej sieci.

Aby osiągnąć ten cel, można wykonywać następujące czynności:

- Kodowanie strumienia ze strumieniem wideo szybkość wielokrotne transmisji bitów (adaptacyjne szybkość transmisji bitów). Spowoduje to przejmują warunków jakości i sieci.
- Za pomocą usługi multimediów Azure Microsoft [dynamiczne opakowania](media-services-dynamic-packaging-overview.md) dynamicznie ponownie pakowanie strumienia do różnych protokołów. To będzie zajmować się przesyłanie strumieniowe na różnych urządzeniach. Usługi multimediów obsługuje dostarczenie następujących adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming, MPEG-kreska i obr. / min (w przypadku tylko licencjobiorców Adobe Primetime-dostęp).

Ten artykuł zawiera omówienie dostarczania zawartości ważnych pojęć.

Aby sprawdzić znanych problemów, zobacz [znane problemy](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamiczne opakowań

Dynamiczne opakowania zawierającego usługi multimediów, będzie możliwe przedstawienie zawartości adaptacyjne szybkość transmisji bitów MP4 lub gładkie Streaming zakodowany w streaming formaty obsługiwane przez usługi multimediów (MPEG-kreska, HLS, gładkie Streaming, obr. / min) bez konieczności ponownego pakietów do tych streaming format. Firma Microsoft zaleca dostarczania zawartości z opakowania dynamiczne.

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu adaptacyjne szybkość transmisji bitów MP4 pliki lub pliki wygładzonymi Streaming adaptacyjne szybkość transmisji bitów.
- Uzyskaj co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak skalowanie streaming zastrzeżone jednostki na żądanie](media-services-portal-manage-streaming-endpoints.md).

Z opakowania dynamiczne, przechowywanie i zapłacić za pliki w formacie jednego miejsca do magazynowania. Usługi multimediów będą tworzyć i służą właściwej reakcji na wezwaniach na podstawie.

Oprócz zapewnienia dostępu do funkcji Pakowanie dynamiczne, na żądanie streaming zastrzeżone jednostki umożliwiają zdolności dedykowane wyjściowe, które można kupić w przyrostach 200 MB/s. Domyślnie streaming na żądanie jest skonfigurowany w modelu udostępnione wystąpienie, które serwer zasobów (na przykład pojemność obliczeń lub wyjściowego) są udostępniane innym użytkownikom. Możesz poprawić przepustowość strumieniowych na żądanie kupując przesyłanie strumieniowe jednostki zastrzeżone na żądanie.

Aby uzyskać więcej informacji zobacz [pakowanie dynamiczne](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtry i manifesty dynamicznych

Filtry można zdefiniować trwałych przy użyciu usługi multimediów. Filtry te są reguły po stronie serwera, które pomogą klientom wykonywania operacji, takich jak odtwarzanie do określonej sekcji klipu wideo lub Określ podzestaw odwzorowań audio i wideo, obsługujące przez urządzenia klienta (zamiast wszystkich odwzorowań, które są skojarzone z elementu). Filtrowanie to zapewnia *dynamiczne manifesty* są tworzone, gdy klient zażąda do strumieniowego przesyłania wideo oparte na jeden lub więcej określone filtry.

Aby uzyskać więcej informacji zobacz [filtry i manifesty dynamiczne](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locator

Aby zapewnić użytkownika adres URL, który może służyć do strumienia lub pobrać zawartość, należy najpierw publikowanie do zawartości, tworząc locator. Locator zapewnia punktu wejścia, aby uzyskać dostęp do plików przechowywanych w środka trwałego. Usługi multimediów obsługuje dwa typy Locator:

- Locator OnDemandOrigin. Te są używane do przesyłania strumieniowego multimediów (na przykład MPEG-KRESKI, HLS lub gładkie Streaming) lub stopniowo pobierania plików.
-  Udostępniania Locator adresu URL podpisu (SA). Są one używane do pobierania plików multimedialnych do komputera lokalnego.

*Zasady dostępu* jest używane do określania uprawnień (na przykład Odczyt, zapis i listy) i czasu trwania, dla której klient ma dostęp do określonych elementów zawartości. Należy zauważyć, że nie należy używać podczas tworzenia locator OrDemandOrigin uprawnień listy (AccessPermissions.List).

Locator znajduje się data wygaśnięcia. Azure portal ustawia datę wygaśnięcia 100 lat w przyszłości Locator.

>[AZURE.NOTE] Jeśli Azure portal umożliwia tworzenie Locator przed marca 2015 r, te Locator zostały ustawione tak, aby wygasało po dwóch latach.

Aby zaktualizować datę wygaśnięcia na uchwycie, użyj [pozostałych](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) lub interfejsów API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Należy zauważyć, że po zaktualizowaniu datę wygaśnięcia locator skojarzeń zabezpieczeń adres URL zmienia się.

Locator nie są przeznaczone do zarządzania kontrola dostępu użytkownika. Za pomocą rozwiązań zarządzania prawami cyfrowymi (DRM), można nadać różne uprawnienia dla poszczególnych użytkowników. Aby uzyskać więcej informacji zobacz [Zabezpieczanie multimediów](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Po utworzeniu locator może być 30-sekundowe opóźnienie z powodu wymagane miejsca do magazynowania i procesów propagowanie w magazynie Azure.


## <a name="adaptive-streaming"></a>Adaptacyjna strumieniowego przesyłania

Technologie adaptacyjne szybkość transmisji bitów Zezwalaj aplikacjom odtwarzacza wideo określają warunki sieci, a następnie wybierz z kilku bitrates. Podczas komunikacji sieciowej obniża, klienta można wybrać dolnym szybkość transmisji bitów, odtwarzania można kontynuować niskiej jakości wideo. Jak poprawić parametry sieci, klienta można przełączyć się szybkość transmisji bitów wyższą jakość wideo ulepszone. Usługi multimediów Azure obsługuje następujące technologie adaptacyjne szybkość transmisji bitów: HTTP Live Streaming (HLS), gładkie Streaming MPEG-kreska i obr. / min.

Aby umożliwić użytkownikom z streaming adresy URL, należy najpierw utworzyć locator OnDemandOrigin. Tworzenie wskaźniku umożliwia ścieżki bazowej do zawartości, która zawiera zawartość, którą chcesz przesyłać strumieniowo. Jednak aby można było przesyłać strumieniowo tę zawartość, należy zmodyfikować następującą ścieżkę dodatkowo. Do utworzenia pełnego adresu URL do pliku manifestu przesyłanie strumieniowe, możesz ZŁĄCZ.teksty wartość ścieżki locator i manifest (filename.ism) nazwę pliku. Następnie **dołącz/pojawiają** i odpowiedni format (w razie potrzeby) do ścieżki lokalizacji.

>[AZURE.NOTE]Możesz również przesyłać strumieniowo zawartości za pośrednictwem połączenia SSL. Aby to zrobić, upewnij się, że przesyłanie strumieniowe adresami URL początkowych HTTPS.

Możesz tylko przesyłać strumieniowo przez SSL Jeśli przesyłanie strumieniowe punktu końcowego, z której dostarczyć zawartość została utworzona po 10 września 2014 r. Jeśli przesyłanie strumieniowe adresami URL są oparte na przesyłanie strumieniowe punkty końcowe utworzone po 10 września 2014 r adres URL zawiera "streaming.mediaservices.windows.net." Przesyłanie strumieniowe adresy URL, które zawierają "origin.mediaservices.windows.net" (stary format) nie obsługują protokołu SSL. Jeśli adres URL jest w starym formacie i chcesz mieć możliwość strumieniowego przesyłania przez SSL, należy utworzyć nowy punkt końcowy przesyłanie strumieniowe. Za pomocą adresów URL oparte na nowy punkt końcowy przesyłanie strumieniowe przesyłanie strumieniowe zawartości przez SSL.


## <a name="streaming-url-formats"></a>Przesyłanie strumieniowe formatów adresu URL

### <a name="mpeg-dash-format"></a>Format MPEG-kreska

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=mpd-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Format 4 Apple HTTP Live Streaming (HLS)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Format V3 Apple HTTP Live Streaming (HLS)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Format firmy Apple HTTP Live Streaming (HLS) z zastosowanym filtrem tylko audio

Domyślnie tylko audio ścieżek znajdują się w HLS manifest. Jest to wymagane do sklepu Apple kwalifikacji dla sieci komórkowej. W tym przypadku jeśli klient nie ma wystarczających przepustowości lub jest połączony za pośrednictwem połączenia 2G, odtwarzanie przełącza się na tylko audio. Dzięki temu strumieniowego przesyłania zawartości bez buforowania, ale nie ma żadnego obrazu. W niektórych scenariuszach odtwarzacza buforowanie może mieć pierwszeństwo tylko audio. Jeśli chcesz usunąć śledzenie tylko audio, Dodaj **tylko audio = false** do adresu URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3,audio-Only=false)

Aby uzyskać więcej informacji zobacz [dynamiczne skład pojawiają pomocy technicznej i HLS wyjściowy dodatkowe funkcje](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Smooth Streaming format

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Przykład:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Gładkie Streaming 2.0 manifest (manifest starsze)

Domyślnie wygładzonymi Streaming format manifestu zawierającym dany znacznik powtarzania (r znacznik). Jednak niektóre odtwarzacze nie obsługuje tagu r. Klienci z graczami te mogą używać formatu, który powoduje wyłączenie tagu r:

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>Obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostępu)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Pobieranie stopniowe

Pobieranie stopniowe pozwala na rozpoczęcie odtwarzania multimediów, zanim pobraniu całego pliku. Nie można pobrać stopniowo .ism * (ismv isma, ismt pliki lub ismc).

Aby stopniowo pobieranie zawartości, użyj typu OnDemandOrigin locator. W poniższym przykładzie pokazano adres URL, który jest oparty na typie OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Musi odszyfrować dowolny szyfrowane miejsca do magazynowania zbiór, który chcesz przesyłać strumieniowo przez usługę origin do pobierania progresywnego.

## <a name="download"></a>Plik do pobrania

Aby pobrać zawartość na urządzeniu klienta, możesz utworzyć Locator skojarzeń zabezpieczeń. Locator skojarzeń zabezpieczeń umożliwia dostęp do kontenera magazynu Azure miejsce, w którym znajduje się plik. Do utworzenia adresu URL pobierania, masz do osadzania nazwy pliku między hostem i podpis skojarzeń zabezpieczeń.

W poniższym przykładzie pokazano adres URL, który jest oparty na wskaźniku skojarzenia zabezpieczeń:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Następujące kwestie:

- Musi odszyfrować dowolny szyfrowane miejsca do magazynowania zbiór, który chcesz przesyłać strumieniowo przez usługę origin do pobierania progresywnego.
- Pobieranie, które nie zostało zakończone w ciągu 12 godzin nie powiedzie się.

## <a name="streaming-endpoints"></a>Przesyłanie strumieniowe punkty końcowe

Przesyłanie strumieniowe punkt końcowy reprezentuje przesyłanie strumieniowe usługa, która zapewnia zawartości bezpośrednio do aplikacji klienckiej odtwarzacza lub sieci dostarczania zawartości (CDN) do dalszej dystrybucji. Strumień ruchu wychodzącego z przesyłanie strumieniowe usługi punkt końcowy może być strumień na żywo lub zasób usługi wideo na żądanie na koncie usługi multimediów. Można także kontrolować możliwości z przesyłanie strumieniowe usługi obsługi rosnących wymagania dotyczące przepustowości za pomocą dostosowania streaming zastrzeżone jednostki. Należy przydzielić co najmniej jedną jednostkę zastrzeżone dla aplikacji w środowisku produkcyjnym. Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Znane problemy

### <a name="changes-to-smooth-streaming-manifest-version"></a>Zmiany w wygładzonymi Streaming pojawiają wersję

Przed wprowadzeniem usługi lipca 2016 — po składniki majątku tworzone przez Media Encoder standardowy, Media Encoder Premium przepływu lub wcześniejszej Azure Media Encoder zostały strumieniowo przy użyciu dynamiczne opakowań — wygładzonymi Streaming manifest zwracane czy są zgodne z wersji 2.0. W wersji 2.0 czasy trwania fragment nie należy używać znaczników powtarzanie tak (r). Na przykład:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

W wersji usługi 2016 lipca wygenerowanego manifestu wygładzonymi Streaming odpowiada wersję 2.2, z czasem trwania fragment przy użyciu tagów powtarzania. Na przykład:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Niektóre starszych klientów wygładzonymi Streaming może nie obsługuje powtarzania znaczniki i zakończy się niepowodzeniem załadować manifestu. Zmniejszeniu tego problemu, można użyć parametru starszy format manifestu **(format = fmp4 v20)** lub zaktualizuj klienta do najnowszej wersji, który obsługuje powtarzania znaczniki. Aby uzyskać więcej informacji zobacz [wygładzonymi Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Tematy pokrewne

[Aktualizowanie Locator usługi multimediów po stopniowych klawiszy miejsca do magazynowania](media-services-roll-storage-access-keys.md)
