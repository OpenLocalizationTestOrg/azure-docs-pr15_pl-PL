<properties 
    pageTitle="Omówienie i porównanie Azure na żądanie multimediów kodery | Microsoft Azure" 
    description="Ten temat zawiera omówienie oraz porównanie Azure na żądanie kodery multimediów." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Omówienie i porównanie Azure na żądanie kodery multimediów

##<a name="encoding-overview"></a>Omówienie kodowania

Usługi multimediów Azure udostępnia wiele opcji kodowania multimediów w chmurze.

Podczas uruchamiania z usługami multimediów, należy opis różnic między formatami koderów-dekoderów i plik.
Kodery-dekodery to oprogramowanie, które wykonuje algorytmy kompresji i dekompresji formaty plików są kontenery, które zawierają skompresowane wideo.

Usługi multimediów zawiera dynamiczne opakowań, umożliwiające dostarczanie zawartości adaptacyjne szybkość transmisji bitów MP4 lub gładkie Streaming zakodowany w streaming formaty obsługiwane przez usługi multimediów (ŁĄCZNIKA MPEG, HLS, gładkie Streaming, obr. / min) bez konieczności ponownego pakietów do tych streaming format.

Aby skorzystać z [opakowania dynamiczne](media-services-dynamic-packaging-overview.md), należy wykonać następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu adaptacyjne szybkość transmisji bitów MP4 pliki lub adaptacyjne szybkość transmisji bitów wygładzonymi Streaming (kodowania czynności przedstawiono w dalszej części tego samouczka).
- Uzyskaj co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak na żądanie Streaming zastrzeżone skali](media-services-portal-manage-streaming-endpoints.md).

Usługi multimediów obsługuje następujące kodery żądanie, które zostały opisane w tym artykule:

- [Media Encoder standardowe](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium w przepływu pracy](media-services-encode-asset.md#media-encoder-premium-workflow)

Ten artykuł zawiera krótki przegląd na żądanie kodery multimediów i znajdują się łącza do artykułów zawierających szczegółowe informacje. Temat także porównanie kodery.

Należy zauważyć, że domyślnie każdego konta usługi multimediów mogą jednej aktywnego zadania kodowania naraz. Można zarezerwować kodowania jednostek, które umożliwiają wielu zadań kodowania jednocześnie, z jednym dla każdej kodowania jednostki zastrzeżone, które możesz kupić. Aby uzyskać informacje zobacz [Skalowanie kodowanie jednostki](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder standardowe

###<a name="how-to-use"></a>Jak używać

[Jak kodowanie z Media Encoder standardowy](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Formaty

[Kodery-dekodery i formaty](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Ustawienia wstępne

Media Encoder standardowy jest skonfigurowana przy użyciu jednego z kodera ustawień wstępnych opisane [poniżej](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Wejściowe i wyjściowe metadanych

Metadane wprowadzania kodery opisanej [w tym miejscu](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Opisano metadanych dane wyjściowe kodery [tutaj](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Generowanie miniatur

Aby uzyskać informacje zobacz [jak wygenerować miniatury przy użyciu Media Encoder standardowy](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Przycinanie klipów wideo (wycinek)

Aby uzyskać informacje zobacz [jak przycinanie wideo przy użyciu Media Encoder standardowy](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Tworzenie nakładek

Aby uzyskać informacje zobacz [Tworzenie nakładki przy użyciu Media Encoder standardowy](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Zobacz też

[Blog usługi multimediów](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium w przepływu pracy

###<a name="overview"></a>Omówienie

[Wprowadzenie do Premium kodowanie w multimediów Azure usług](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Jak używać

Media Encoder Premium w przepływu pracy jest skonfigurowany za pomocą złożone przepływy pracy. Pliki przepływu pracy może zostać utworzony i zaktualizowane przy użyciu narzędzia [Projektant przepływów pracy](media-services-workflow-designer.md) .

[Jak używać Premium kodowanie w multimediów Azure usług](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Znane problemy

Jeśli wprowadzania wideo nie zawiera podpisy kodowane, wynik zawartości będzie nadal zawierać pusty plik TTML. 


##<a id="compare_encoders"></a>Porównanie kodery

###<a id="billing"></a>Miernik rozliczeń używane przez każdego encoder

Nazwa procesora multimediów|Zastosowanie ceny|Notatki
---|---|---
**Media Encoder standardowe** |ENCODER|Kodowanie zadania jest rozliczana według rozmiaru wynik zawartości w GB stawki określony [poniżej][1], w kolumnie KODERA.
**Media Encoder Premium w przepływu pracy** |KODER PREMIUM|Kodowanie zadania jest rozliczana według rozmiaru wynik zawartości w GB stawki określony [poniżej][1], w kolumnie PREMIUM ENCODER.


W tej sekcji porównano kodowania możliwości **Media Encoder standardowy** i **Media Encoder Premium w przepływu pracy**.


###<a name="input-containerfile-formats"></a>Wprowadzanie kontenera-formatach

Wprowadzanie kontenera-formatach|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
Adobe® Flash® F4V           |Tak|Tak
MXF-SMPTE 377M              |Tak|Tak
GXF                         |Tak|Tak
Strumienie transportu MPEG-2    |Tak|Tak
Strumienie programu MPEG-2      |Tak|Tak
MPEG-4-MP4                  |Tak|Tak
ASF Media systemu Windows           |Tak|Tak
AVI (nieskompresowane 8-bitowa-10 bitów)|Tak|Tak
STANDARD 3GPP-3GPP2                  |Tak|Brak
Format plików Smooth Streaming (PIFF 1.3)|Tak|Brak
[Cyfrowe Microsoft Recording(DVR-MS) wideo](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Tak|Brak
Matroska-WebM               |Tak|Brak
QuickTime (mov) |Tak|Brak

###<a name="input-video-codecs"></a>Wprowadzania kodery-dekodery wideo

Wprowadzania kodery-dekodery wideo|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
AVC 8-bitową-10-bitową, maksymalnie 4:2:2, łącznie z AVCIntra   |8 bit 4:2:0 i 4:2:2|Tak
Avid DNxHD (w MXF)                                 |Tak|Tak
DVCPro-DVCProHD (w MXF)                            |Tak|Tak
JPEG2000                                            |Tak|Tak
MPEG-2 (maksymalnie 422 profilu oraz wysokiego poziomu, w tym warianty, takie jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|Maksymalnie 422 profilu|Tak
MPEG-1                                              |Tak|Tak
Windows Media wideo i VC-1                            |Tak|Tak
Canopus HQ-HQX                                      |Brak|Brak
MPEG-4, część 2                                       |Tak|Brak
[Theora](https://en.wikipedia.org/wiki/Theora)      |Tak|Brak
Apple ProRes 422    |Tak|Brak
Apple ProRes 422 LT |Tak|Brak
Apple ProRes 422 HQ |Tak|Brak
Apple ProRes Proxy|Tak|Brak
Apple ProRes 4444 |Tak|Brak
Apple ProRes 4444 XQ |Tak|Brak

###<a name="input-audio-codecs"></a>Wprowadzania kodery-dekodery Audio

Wprowadzania kodery-dekodery Audio|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
AES (SMPTE 331 M i 302 M, AES3 – 2003)        |Brak|Tak
Dolby® E                                    |Brak|Tak
Cyfrowe Dolby® (AC3)                        |Brak|Tak
Cyfrowe Dolby® Plus (E-AC3)                 |Brak|Tak
AAC (AAC-LC, AAC-HE i AAC-HEv2; maksymalnie 5.1)|Tak|Tak
Warstwy MPEG 2|Tak|Tak
Mp3 (MPEG-1 Audio Layer 3)|Tak|Tak
Windows Media Audio|Tak|Tak
WAV-PCM|Tak|Tak
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Tak|Brak
[Dziele](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Tak|Brak
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Tak|Brak


###<a name="output-containerfile-formats"></a>Plik/kontenera formatów

Plik/kontenera formatów|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
Adobe® Flash® F4V|Brak|Tak
MXF (OP1a, XDCAM i AS02)|Brak|Tak
DPP (w tym AS11)|Brak|Tak
GXF|Brak|Tak
MPEG-4-MP4|Tak|Tak
MPEG-TS|Tak|Tak
ASF Media systemu Windows|Brak|Tak
AVI (nieskompresowane 8-bitowa-10 bitów)|Brak|Tak
Format plików Smooth Streaming (PIFF 1.3)|Brak|Tak

###<a name="output-video-codecs"></a>Kodery-dekodery wideo wyjścia

Kodery-dekodery wideo wyjścia|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
AVC (H.264; 8-bitową, aż do profilu wysoki poziom 5.2; 4 K bardzo HD; Wewnątrz AVC)|Tylko 8 bit 4:2:0|Tak
Avid DNxHD (w MXF)|Brak|Tak
MPEG-2 (maksymalnie 422 profilu oraz wysokiego poziomu, w tym warianty, takie jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|Brak|Tak
MPEG-1|Brak|Tak
Windows Media wideo i VC-1|Brak|Tak
Tworzenie miniatur JPEG|Brak|Tak

###<a name="output-audio-codecs"></a>Kodery-dekodery Audio wyjścia

Kodery-dekodery Audio wyjścia|Media Encoder standardowe|Media Encoder Premium w przepływu pracy
---|---|---
AES (SMPTE 331 M i 302 M, AES3 – 2003)|Brak|Tak
Cyfrowe Dolby® (AC3)|Brak|Tak
Dolby® Digital Plus (E-AC3) do 7.1|Brak|Tak
AAC (AAC-LC, AAC-HE i AAC-HEv2; maksymalnie 5.1)|Tak|Tak
Warstwy MPEG 2|Brak|Tak
Mp3 (MPEG-1 Audio Layer 3)|Brak|Tak
Windows Media Audio|Brak|Tak


##<a name="error-codes"></a>Kody błędów  

Poniższa tabela zawiera listę kodów błędów, które można przywrócić w przypadku, gdy wystąpił błąd podczas kodowania wykonywania zadania.  Aby uzyskać szczegółowe informacje o błędzie w kodzie .NET, użyj klasy [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) . Aby uzyskać szczegółowe informacje o błędzie w kodzie pozostałych, za pomocą interfejsu API usługi REST [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) .

ErrorDetail.Code|Możliwe przyczyny błędów
-----|-----------------------
Nieznany| Nieznany błąd podczas wykonywania zadania
ErrorDownloadingInputAssetMalformedContent|Kategoria błędów zajmującego błędy pobierania wprowadzania środków trwałych, takich jak nazwy uszkodzonych plików, zerowej długości pliki niepoprawne formatuje i tak dalej.
ErrorDownloadingInputAssetServiceFailure|Kategoria błędów obejmuje problemów na stronie usługi — na przykład sieci i magazynu błędów podczas pobierania.
ErrorParsingConfiguration|Kategoria błędy zadania, gdzie <see cref="MediaTask.PrivateData"/> (Konfiguracja) nie jest prawidłowa, na przykład konfiguracji nie jest prawidłowy system wstępnie ustawione lub zawiera nieprawidłowy kod XML.
ErrorExecutingTaskMalformedContent|Kategoria błędy podczas wykonywania zadania, gdzie problemy wewnątrz pliki multimedialne wprowadzania spowodować błąd.
ErrorExecutingTaskUnsupportedFormat|Kategoria błędów, gdzie procesora multimedia nie może przetworzyć pliki dostarczone — format multimediów obsługiwane lub nie pasuje do konfiguracji. Na przykład próby utworzenia tylko audio wynikiem środka trwałego zawierającą tylko wideo
ErrorProcessingTask|Kategoria inne błędy, które procesor multimediów napotka podczas przetwarzania zadania niezwiązane zawartości.
ErrorUploadingOutputAsset|Kategoria błędy podczas przekazywania trwałego wyjścia
ErrorCancelingTask|Kategoria błędów w celu przykrycia awarii podczas próby anulować zadanie
TransientError|Kategoria błędów w celu przykrycia przejściowych problemów (np. tymczasowe problemy sieci z nośnikami Azure)


Aby uzyskać pomoc od zespołu **Usługi multimediów** , otwórz [obsługuje biletów](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Artykuły pokrewne

- [Wykonywanie zaawansowanych zadań kodowania, dostosowując Media Encoder standardowe ustawienia wstępne](media-services-custom-mes-presets-with-dotnet.md)
- [Przydziały i ograniczenia](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
