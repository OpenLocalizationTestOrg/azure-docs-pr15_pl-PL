<properties 
    pageTitle="Media Encoder standardowe formaty oraz koderów-dekoderów" 
    description="W tym temacie przedstawiono omówienie koderów-dekoderów i Media Encoder standardowe formaty." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Standardowe formaty Encoder multimediów oraz koderów-dekoderów


Ten dokument zawiera listę najczęściej używanych importu i eksportu formatów plików, które można używać z Media Encoder standardowy.


##<a name="input-containerfile-formats"></a>Wprowadzanie kontenera-formatach

Formaty plików (rozszerzenia plików)|Obsługiwane
---|---|---|---
FLV (z kodery-dekodery H.264 i AAC) (FLV)          |Tak 
MXF (.mxf)                  |Tak 
GXF (.gxf)                  |Tak 
3GP MPEG2-PS, MPEG2-TS (.ts, PS, 3GP, .3gpp, mpg)   |Tak 
Windows Media Video (WMV) i ASF (.wmv, ASF) |Tak 
AVI (nieskompresowane 8-bitowa-10 bitów) (AVI)|Tak 
MP4 (MP4, M4A, M4V)-ISMV (.isma, .ismv)|Tak 
[Cyfrowe Microsoft Recording(DVR-MS) wideo](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Tak 
Matroska-WebM (.mkv)        |Tak 
WAVE/WAV (wav) |Tak 
QuickTime (mov) |Tak

>[AZURE.NOTE] Brzmi listę częściej wykrytych rozszerzenia nazw plików. Media Encoder Standard obsługuje wiele innych (na przykład: m2ts, .mpeg2video, Qt). Jeśli spróbujesz kodowanie pliku i zostanie wyświetlony komunikat o błędzie dotyczący formatowanie nie są obsługiwane, podaj swoją opinię [w tym miejscu](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Formaty audio w kontenerach wprowadzania danych 

Media Encoder standardowy obsługuje następujące formaty audio w kontenerach wprowadzania wykonywania:

- MXF, GXF i QuickTime pliki, które mają ścieżki audio z przeplotem stereo lub 5.1 próbki

lub

- Plików MXF, GXF i QuickTime, gdy dźwięk jest przeprowadzane jako oddzielne ścieżki PCM, ale wynikają mapowania kanałów (aby stereo lub 5.1) z metadanych pliku

Uwaga obsługujące do mapowania kanałów jawnych i dostarczane przez użytkownika będzie dostępna w najbliższej przyszłości.


##<a name="input-video-codecs"></a>Wprowadzania kodery-dekodery wideo

Wprowadzania kodery-dekodery wideo|Obsługiwane
---|---|---|---
AVC 8-bitową-10-bitową, maksymalnie 4:2:2, łącznie z AVCIntra   |8 bit 4:2:0 i 4:2:2 
Avid DNxHD (w MXF)                                 |Tak 
DVCPro-DVCProHD (w MXF)                            |Tak 
Wideo (DV) (w pliki AVI)                   |Tak
FORMAT JPEG 2000                                           |Tak 
MPEG-2 (maksymalnie 422 profilu oraz wysokiego poziomu, w tym warianty, takie jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|Maksymalnie 422 profilu 
MPEG-1                                              |Tak 
VC-1-WMV9                                           |Tak 
Canopus HQ-HQX                                      |Brak 
MPEG-4, część 2                                       |Tak 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Tak 
YUV420 nieskompresowane lub kodery                   |Tak
Apple ProRes 422                                    |Tak
Apple ProRes 422 LT |Tak
Apple ProRes 422 HQ |Tak
Apple ProRes Proxy|Tak
Apple ProRes 4444 |Tak
Apple ProRes 4444 XQ |Tak



##<a name="input-audio-codecs"></a>Wprowadzania kodery-dekodery Audio

Wprowadzania kodery-dekodery Audio|Obsługiwane
---|---|---|---
AAC (AAC-LC, AAC-HE i AAC-HEv2; maksymalnie 5.1)|Tak 
Warstwy MPEG 2|Tak 
Mp3 (MPEG-1 Audio Layer 3)|Tak 
Windows Media Audio|Tak 
WAV-PCM|Tak 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Tak 
[Dziele](http://go.microsoft.com/fwlink/?LinkId=822667) |Tak 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Tak 
AMR (stopa wielu adaptacyjne)|Tak
AES (SMPTE 331 M i 302 M, AES3 – 2003)        |Brak 
Dolby® E                                    |Brak 
Cyfrowe Dolby® (AC3)                        |Brak 
Cyfrowe Dolby® Plus (E-AC3)                 |Brak 


##<a name="output-formats-and-codecs"></a>Kodery-dekodery i formatów wyjściowych

W poniższej tabeli wymieniono formaty koderów-dekoderów i plików, które są obsługiwane w przypadku eksportowania.


Format pliku|Koder-dekoder wideo|Koder-dekoder audio
---|---|---
MP4 <br/><br/>(w tym kontenerów MP4 szybkość transmisji bitów wielokrotne) |H.264 (wysoki, główny i profile według planu bazowego)|AAC-LC, HE-AAC w wersji 1, HE-AAC w wersji 2 
MPEG2 TS |H.264 (wysoki, główny i profile według planu bazowego)|AAC-LC, HE-AAC w wersji 1, HE-AAC w wersji 2 



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Kodowanie zawartości na żądanie za pomocą usługi multimediów Azure](media-services-encode-asset.md)

[Jak kodowanie z Media Encoder standardowy](media-services-dotnet-encode-with-media-encoder-standard.md)
