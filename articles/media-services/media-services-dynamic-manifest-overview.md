<properties 
    pageTitle="Filtry i dynamiczne manifesty | Microsoft Azure" 
    description="W tym temacie opisano sposób tworzenia filtrów w celu klienta można używać ich do poszczególnych sekcji strumienia strumienia. Usługi multimediów tworzy dynamiczne manifesty archiwizowania selektywne streaming." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filtry i manifesty dynamicznych

Począwszy od wersji 2.11, usługi multimediów umożliwia definiowanie filtrów dla aktywów. Filtry te są reguł po stronie serwera, które umożliwią klientów służących do wykonywania operacji, takich jak: odtwarzanie tylko do sekcji klipu wideo (zamiast odtwarzanie całego), lub określ tylko niektóre odwzorowań audio i wideo, obsługujące przez urządzenia klienta (zamiast wszystkich odwzorowań, które są skojarzone z elementu). Filtrowanie to środków trwałych archiwizacji za pośrednictwem **Dynamicznego pojawiają**s utworzonym na żądanie klienta do strumieniowego przesyłania wideo według określonej filtry.

Ten tematy w tym artykule omówiono typowe scenariusze, w której przy użyciu filtrów jest bardzo korzystne dla Twoich klientów i łączy do tematów, pokazujących, jak utworzyć filtry programowy (obecnie, możesz utworzyć filtry z pozostałych interfejsy API tylko).

##<a name="overview"></a>Omówienie

Podczas dostarczania zawartości klientom (przesyłanie strumieniowe wydarzeń na żywo lub usługi wideo na żądanie) celem jest dostarczanie wideo o wysokiej jakości różnych urządzeń warunkach innej sieci. Aby osiągnąć ten cel, wykonaj następujące czynności:

- kodowanie strumienia do wielu-szybkość transmisji bitów ([adaptacyjne szybkość transmisji bitów](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) strumienia wideo (to będzie zajmować warunków jakości i sieci) i 
- za pomocą usługi multimediów [Dynamiczne opakowań](media-services-dynamic-packaging-overview.md) dynamicznie ponownie pakowanie strumienia do różnych protokołów (to będzie zajmować się przesyłanie strumieniowe na różnych urządzeniach). Usługi multimediów obsługuje dostarczenie następujących adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp). 

###<a name="manifest-files"></a>Pojawiają plików 

Podczas kodowania środka trwałego dla strumieniowego przesyłania adaptacyjne szybkość transmisji bitów jest tworzony plik **pojawiają** (odtwarzania) (plik jest oparte na tekst lub opartym na języku XML). Plik **pojawiają** zawiera strumieniowego przesyłania metadanych, takie jak: śledzenie typu (audio, wideo lub tekst), śledzenie nazwę, godzinę rozpoczęcia i zakończenia, szybkość transmisji bitów (właściwości), języków śledzenia okna prezentacji (przesuwania okna o stałym czasie trwania), kodera-dekodera wideo (FourCC). Informuje także odtwarzacza do pobierania następnego fragmentu, dostarczając informacji o następnej który można odtwarzać wideo fragmenty dostępne i sprawdź lokalizację kontaktu. Fragmenty (lub segmentów) są rzeczywiste "fragmentów" zawartość wideo.


Oto przykładowy plik manifestu: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Manifesty dynamicznych

Istnieją [scenariusze](media-services-dynamic-manifest-overview.md#scenarios) , gdy klient wymaga większej elastyczności niż opisano w pliku manifestu elementu domyślny. Na przykład:

- Urządzenie: dostarczyć tylko i\lub określonej wersji określonej wersji językowych, które są obsługiwane przez urządzenie, które jest używane do odtwarzania zawartości ("odwzorowanie filtrowanie"). 
- Zmniejsz manifest pokazanie klipu podrzędnego zdarzenia na żywo ("podrzędnych klip filtrowanie").
- Przycinanie początku klipu wideo ("przycinanie klipu wideo").
- Dostosowywanie okna prezentacji (DVR) w celu udostępniania ograniczona długość DVR okna programu Windows Media player ("Dostosowywanie prezentacji okno").
 
Aby osiągnąć ten elastyczność, usługi multimediów oferuje **Dynamiczne manifesty** oparte na wstępnie zdefiniowanych [filtrów](media-services-dynamic-manifest-overview.md#filters).  Po zdefiniowaniu filtrów klienci mogą przy ich użyciu strumienia określonej wersji lub podrzędne klipy wideo. Czy określają filtry w adresie URL przesyłanie strumieniowe. Filtry można zastosować do szybkość transmisji bitów adaptacyjne streaming protokołów obsługiwanych przez [Dynamiczne opakowań](media-services-dynamic-packaging-overview.md): HLS, MPEG-kreska Streaming wygładzonymi i obr. / min. Na przykład:

Adres URL ŁĄCZNIKA MPEG przy użyciu filtru

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Gładkie Streaming adres URL z filtru

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Aby uzyskać więcej informacji na temat dostarczania zawartości i tworzenie streaming adresy URL zobacz [Omówienie zawartości dostarczanie](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Należy zauważyć, że dynamiczne manifesty nie zmieniają się aktywa i manifeście domyślnego dla tego zasobu. Klienta można zażądać strumień z lub bez filtrów. 


###<a id="filters"></a>Filtry 

Istnieją dwa typy elementów zawartości filtrów: 

- Filtry globalne (mogą być stosowane do wszystkich aktywów na koncie usługi multimediów Azure, okres działania konta) i 
- Filtry lokalnych (mogą być stosowane tylko do środka trwałego, z którym filtr skojarzono po utworzeniu, okres działania środka). 

Typy filtrów globalnym i lokalnym mieć te same właściwości. Główna różnica między tymi dwoma jest dla jakie scenariusze jakiego rodzaju filer bardziej odpowiednie. Filtry globalne są ogólnie nadaje się do urządzenia profile (odwzorowanie filtrowanie) miejsce, w którym lokalne filtry można przyciąć określonych elementów zawartości.


##<a id="scenarios"></a>Typowe scenariusze 

Jak wspomniano wcześniej, podczas dostarczania zawartości klientom (przesyłanie strumieniowe wydarzeń na żywo lub usługi wideo na żądanie) celem jest dostarczanie wideo o wysokiej jakości różnych urządzeń warunkach innej sieci. Ponadto usługi mogą mieć inne wymagania, obejmujące filtrowanie aktywów i stosowanie s **Dynamiczne pojawiają**. Poniższe sekcje nadać krótkie omówienie różnych scenariuszach filtrowania.

- Określ tylko niektóre odwzorowań audio i wideo, obsługujące przez niektórych urządzeń (zamiast wszystkich odwzorowań, które są skojarzone z elementu). 
- Odtwarzanie tylko do sekcji klipu wideo (zamiast odtwarzanie całego).
- Dostosowywanie okna prezentacji DVR.

##<a name="rendition-filtering"></a>Filtrowanie odwzorowanie 

Można wybrać kodowanie usługi zawartości wielu bitrates jakości i kodowania profili (H.264 według planu bazowego, H.264 wysoki, AACL, AACH, Dolby Digital Plus). Nie wszystkie urządzenia klienckie będą obsługuje jednak profile i bitrates wszystkich do zawartości. Na przykład starszych urządzeń z systemem Android obsługuje tylko H.264 według planu bazowego + AACL. Wysyłanie wyższą bitrates na urządzenie, którego nie można uzyskać korzyści, odpady obliczeń przepustowość i urządzenia. Takie urządzenie musi dekodowanie wszystkie dane informacje tylko do skalować go w dół do wyświetlania.

W programie pojawiają dynamiczne, możesz utworzyć profile urządzeń przykład numer telefonu komórkowego, konsoli HD/SD itd., a obejmuje ścieżki i jakości, które mają być częścią każdego profilu.

 
![Przykład filtrowania odwzorowanie][renditions2]

W poniższym przykładzie kodera użytego do kodowania zawartości kodery do siedmiu ISO MP4s odwzorowań wideo (od 180p, aby 1080p). Zakodowany zawartości można dynamicznie umieszczane w dowolnej z następujących protokołów: HLS, gładkie ŁĄCZNIKA MPEG i obr. / min.  W górnej części diagramu przedstawiono manifest HLS zawartości bez filtrów (zawiera wszystkie wersje 7).  W lewym dolnym rogu manifest HLS, do którego zastosowano filtr o nazwie "ott" jest wyświetlana. Filtr "ott" Określa, aby usunąć wszystkie bitrates poniżej 1Mbps, które spowodowało dołu dwa poziomy jakości jest odłączony w odpowiedzi.  W prawym dolnym rogu manifest HLS, do którego zastosowano filtr o nazwie "przenośnych" jest wyświetlana. "Przenośnych" filtr Określa, aby usunąć odwzorowań miejsce, w którym jest większy niż 720p, które spowodowało dwóch rozdzielczość odwzorowań 1080p jest odłączony.

![Filtrowanie odwzorowanie][renditions1]

##<a name="removing-language-tracks"></a>Usuwanie wersji językowych

Aktywów mogą zawierać wiele języków audio, takich jak angielski, hiszpański, francuski, itp. Zazwyczaj wybór ścieżki dźwiękowej domyślny menedżerów Player SDK i dostępne audio śledzi na wybór użytkownika. Trudne do projektowania takie SDK odtwarzacza, wymaga różnych implementacji wielu specyficznych dla urządzenia odtwarzacza RAM. Ponadto na niektórych platformach interfejsy API odtwarzacza są ograniczone i nie zawiera funkcji audio zaznaczenia miejsce, w którym użytkownicy nie Wybieranie lub zmienianie domyślnej ścieżki dźwiękowej. Za pomocą filtrów zasobów można kontrolować zachowanie tworząc filtry, które zawierają tylko żądane języki audio.

![Filtrowanie wersji językowych][language_filter]


##<a name="trimming-start-of-an-asset"></a>Skracanie start środka trwałego 

W większości wydarzeń przesyłanie strumieniowe na żywo operatorów Uruchom niektóre testy przed zdarzeniem rzeczywiste. Na przykład mogą zawierać Ciemny błękit w następujący sposób przed rozpoczęciem wydarzenia: "Program rozpocznie chwilowo". Jeśli archiwizacji program badania i Ciemny błękit danych również są archiwizowane i zostaną uwzględnione w prezentacji. Jednak te informacje nie jest wyświetlana do klientów. Z pojawiają dynamiczne można utworzyć filtrem czas rozpoczęcia i usuń niepotrzebne dane ze manifest.

![Skracanie start][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Tworzenie klipów podrzędny (widoki) z archiwum live

Wiele wydarzeń na żywo są długie, uruchomiony i archiwum live mogą zawierać wiele zdarzeń. Po zdarzeniu live nadawców kończy się może chcesz podzielić archiwum live na uruchamianie programu logiczne i zatrzymywanie sekwencji. Następnie opublikować tych programów wirtualnego oddzielnie bez wpisu przetwarzania live archiwum, a nie tworzenie oddzielne aktywa (które nie otrzyma korzyści fragmentów istniejącej pamięci podręcznej w sieci CDN). Przykłady takich programów wirtualnych (klipy podrzędne) są kwartały piłka nożna lub gry Koszykówce, innings w baseball lub pojedynczych zdarzeń po południu programu Igrzysk Olimpijskich.

Z dynamicznego pojawiają można utworzyć filtry przy użyciu godziny rozpoczęcia i zakończenia i tworzenie widoków wirtualnych w górnej części live archiwum. 

![Filtr subclip][subclip_filter]

Filtrowania zawartości:

![Nart][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Dostosowywanie okna prezentacji (DVR)

Obecnie usługi multimediów Azure oferuje cykliczne archiwum miejsce, w którym można skonfigurować czas trwania między 5 minut - 25 godzin. Filtrowanie manifestu umożliwia tworzenie stopniowe okna DVR w górnej części archiwum, nie usuwając multimediów. Istnieje wiele scenariuszy miejsce, w którym chcesz podać ograniczone okna DVR, które przejście do krawędzi live i w tym samym czasie okno większy archiwizacji nadawców. Nadawca warto używać danych znajdujących się poza okno DVR, aby wyróżnić klipy lub he\she może być podanie różnych okien DVR dla różnych urządzeń. Na przykład większość urządzeń przenośnych nie obsługują duże okna DVR (dla klientów komputerowych może być okno DVR 2 minuty dla urządzeń przenośnych i 1 godzina).

![Okno DVR][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Dopasowywanie LiveBackoff (pozycja live)

Filtrowanie manifestu można usunąć kilka sekund od krawędzi live programu live. Dzięki temu nadawców oglądanie jej na punkt publikacji Podgląd i tworzyć ogłoszeń punkty wstawiania przed odbiorcom odbierać strumień (zazwyczaj kopii wyłączone 30 sekund). Nadawców można przekazać te reklam do ich RAM klienta w czasie ich otrzymanej i przetwarzanie informacji przed ogłoszeń szansy sprzedaży.

Oprócz obsługi ogłoszeń LiveBackoff może służyć do dostosowania pozycji Pobierz live klienta, tak, aby po klientów przemieścić i kliknij krawędź live fragmenty nadal można uzyskiwać z serwera zamiast występują błędy HTTP 404 lub 412.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Łączenie wielu reguł w pojedynczym filtrze

Można połączyć wiele reguł filtrowania w pojedynczym filtrze. Jako przykład można zdefiniować regułę zakres, aby usunąć Ciemny błękit z archiwum live i filtrując bitrates dostępne. Zasady filtrowania wielu wynik końcowy jest skład (tylko przecięcia) tych reguł.

![wiele reguł][multiple-rules]

##<a name="create-filters-programmatically"></a>Tworzenie filtrów programowy

Następujące temacie omówiono jednostki usługi multimediów, które są związane z filtrów. Temat również pokazano, jak programowo tworzenia filtrów.  

[Tworzenie filtrów za pomocą interfejsów API pozostałych](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Łączenie wielu filtrów (filtru skład)

Można także połączyć wiele filtrów w jednym adresie URL. 

Następującym scenariuszu zaprezentowano, dlaczego warto łączyć filtry:

1. Musisz Filtrowanie usługi jakość wideo dla urządzeń przenośnych, takich jak Android czy iPAD (w celu ograniczenia jakość wideo). Aby usunąć niepożądane jakości, należy utworzyć filtr globalny, które jest odpowiednie dla profile urządzeń. Jak wcześniej wspomniano, filtry globalne można używać dla wszystkich trwałych dla tego samego konta usługi multimediów bez dalszych skojarzenia. 
2. Chcesz również przyciąć godzinę rozpoczęcia i zakończenia środka trwałego. Aby osiągnąć ten cel, czy utworzyć filtr lokalne i ustaw czas rozpoczęcia i zakończenia. 
3. Chcesz połączyć oba te filtry (bez połączenia należy dodawanie jakości filtrowania w filtrze przycinanie, co spowoduje zastosowania filtru trudne).

Aby połączyć filtry, należy ustawić nazw filtrów do manifestu i odtwarzania adres URL z średnikami. Załóżmy, że masz filtru o nazwie *MyMobileDevice* jakości filtrów i że innego nazwany *MyStartTime* Aby ustawić określony czas rozpoczęcia. Można połączyć je w następujący sposób:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Można łączyć się do 3 filtrów. 

Aby uzyskać więcej informacji, zobacz [tym](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.


##<a name="know-issues-and-limitations"></a>Znasz problemy i ograniczenia

- Dynamiczne manifest działa w GOP dokładność GOP występują ograniczenia (klawisz ramki), w związku z tym przycinania. 
- Można użyć tej samej nazwy filtru dla filtrów lokalnych i globalnych. Uwaga Ten filtr lokalny mają wyższy priorytet i zastąpi filtry globalne.
- Po zaktualizowaniu filtru może potrwać do 2 minut przesyłanie strumieniowe końcowy odświeżyć reguł. Jeśli zawartość została obsługiwane za pomocą niektórych filtrów (i w pamięci podręcznej serwery proxy i CDN pamięci podręcznej), aktualizowanie te filtry może spowodować błędy odtwarzacza. Jest zalecane, aby wyczyścić pamięć podręczną po zaktualizowaniu filtr. Jeśli ta opcja nie jest możliwe, rozważ użycie inny filtr.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zobacz też

[Dostarczania zawartości do przeglądu klientów](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 