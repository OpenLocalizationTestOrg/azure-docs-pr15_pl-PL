<properties 
    pageTitle="Formaty Media Encoder Premium w przepływie pracy oraz koderów-dekoderów | Microsoft Azure" 
    description="Ten temat zawiera omówienie formatów Media Encoder Premium przepływu pracy formaty oraz koderów-dekoderów" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Kodery-dekodery i formaty Media Encoder Premium w przepływu pracy


>[AZURE.NOTE]Aby pytania encoder premium poczty e-mail mepd z witryny Microsoft.com.
>
>Procesor multimediów Media Encoder Premium przepływu omówione w tym temacie nie jest dostępna w Chinach. 

Ten dokument zawiera listę wprowadzania i formaty plików dane wyjściowe i koderów-dekoderów obsługiwanych w wersji preview publicznej kodera **Media Encoder Premium w przepływu pracy** .

[Formatów danych wejściowych Worflow Premium Encoder multimediów oraz koderów-dekoderów](#input_formats)

[Formatów wyjściowych Worflow Premium Encoder multimediów oraz koderów-dekoderów](#output_formats)

**Media Encoder Premium** obsługuje kodowane opisane w [tej](#closed_captioning) sekcji. 


##<a id="input_formats"></a>Przepływ pracy programu Media Encoder Premium wprowadzania formaty oraz koderów-dekoderów

Następną sekcję Lista koderów-dekoderów i plik formatów obsługiwanych przez tego procesora multimedia jako danych wejściowych.

###<a name="input-containerfile-formats"></a>Wprowadzanie kontenera-formatach

- Adobe® Flash® F4V
- MXF-SMPTE 377M
- GXF
- Strumienie transportu MPEG-2
- Strumienie programu MPEG-2
- MPEG-4-MP4
- ASF Media systemu Windows
- AVI (nieskompresowane 8-bitowa-10 bitów)

###<a name="input-video-codecs"></a>Wprowadzania kodery-dekodery wideo

- AVC 8-bitową-10-bitową, maksymalnie 4:2:2, łącznie z AVCIntra
- Avid DNxHD (w MXF)
- DVCPro-DVCProHD (w MXF)
- JPEG2000
- MPEG-2 (maksymalnie 422 profilu oraz wysokiego poziomu, w tym warianty, takie jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)
- MPEG-1
- Windows Media wideo i VC-1

###<a name="input-audio-codecs"></a>Wprowadzania kodery-dekodery Audio

- AES (SMPTE 331 M i 302 M, AES3 – 2003)
- Dolby® E
- Cyfrowe Dolby® (AC3)
- AAC (AAC-LC, AAC-HE i AAC-HEv2; maksymalnie 5.1)
- Warstwy MPEG 2
- Mp3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV-PCM
 
##<a id="output_format"></a>Kodery-dekodery i formatów wyjściowych Media Encoder Premium przepływu pracy

Następną sekcję Lista koderów-dekoderów i plik formaty, które są obsługiwane jako dane wyjściowe tego procesora multimediów.

###<a name="output-containerfile-formats"></a>Plik/kontenera formatów

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM i AS02)
- DPP (w tym AS11)
- GXF
- MPEG-4-MP4
- ASF Media systemu Windows
- AVI (nieskompresowane 8-bitowa-10 bitów)
- Format plików Smooth Streaming (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Kodery-dekodery wideo wyjścia

- AVC (H.264; 8-bitową, aż do profilu wysoki poziom 5.2; 4 K bardzo HD; Wewnątrz AVC)
- Avid DNxHD (w MXF)
- DVCPro-DVCProHD (w MXF)
- MPEG-2 (maksymalnie 422 profilu oraz wysokiego poziomu, w tym warianty, takie jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)
- MPEG-1
- Windows Media wideo i VC-1
- Tworzenie miniatur JPEG

###<a name="output-audio-codecs"></a>Kodery-dekodery Audio wyjścia

- AES (SMPTE 331 M i 302 M, AES3 – 2003)
- Cyfrowe Dolby® (AC3)
- Dolby® Digital Plus (E-AC3) do 7.1
- AAC (AAC-LC, AAC-HE i AAC-HEv2; maksymalnie 5.1)
- Warstwy MPEG 2
- Mp3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Obsługa kodowane

Na mogły zjeść tej ostatniej, **Media Encoder Premium** obsługuje:

1. Pliki SCC
1. Pliki SMPTE TT
1. CEA-608/CEA-708 — jako dane użytkownika (wiadomości SEI H.264 strumieni szkół podstawowych, ATSC/53, SCTE20) lub jako dodatkowe dane w plikach MXF-GXF
1. Pliki pomocniczą STL

Produkcja dostępne są następujące opcje:

1. CEA-608 do tłumaczenia CEA 708
1. CEA-608/CEA-708 przekazywanie (osadzony w wiadomości SEI strumieniami H.264 lub przenoszone jako dodatkowe dane w plikach MXF)
1. SCC
1. Tekst Przekroczono SMPTE (ze źródła CEA 608 na SMPTE RP2052; tym tworzenia pliku DFXP)
1. Plik SRT pomocniczą
1. Strumienie napisy DVB

Uwaga: nie wszystkie powyżej formatów wyjściowych są obsługiwane na dostarczenie za pośrednictwem streaming w usługi multimediów Azure.

##<a name="known-issues"></a>Znane problemy

Jeśli wprowadzania wideo nie zawiera podpisy kodowane, wynik zawartości będzie nadal zawierać pusty plik TTML. 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
