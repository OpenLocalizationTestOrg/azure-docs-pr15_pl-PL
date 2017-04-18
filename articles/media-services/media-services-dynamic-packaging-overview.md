<properties
    pageTitle="Omówienie dynamiczne opakowania | Microsoft Azure"
    description="Zapewnia tematu i przegląd opakowań dynamiczne."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynamiczne opakowań

##<a name="overview"></a>Omówienie

Usługi multimediów Azure firmy Microsoft mogą być używane do przeprowadzania wiele źródła formaty plików multimedialnych, przesyłanie strumieniowe formatów multimediów i ochrona zawartości formaty do różnych technologii klienta (na przykład iOS konsoli XBOX, Silverlight, Windows 8). Tych klientów opis różnych protokołów, na przykład iOS wymaga formatu 4 HTTP Live Streaming (HLS) i Silverlight i konsoli Xbox wymagają Streaming gładkie. Jeśli masz zestaw adaptacyjne szybkość transmisji bitów (wielokrotne-szybkość transmisji bitów) MP4 plików (Media Base ISO 14496-12) lub zestawu wygładzonymi strumieniowego przesyłania plików adaptacyjne szybkość transmisji bitów, które chcesz służą do klientów, którzy opis KRESKI MPEG, HLS lub gładkie Streaming należy korzystać opakowań dynamiczne usługi multimediów.

Dynamiczne opakowania wystarczy ma utworzyć zasób zawiera zestaw adaptacyjne szybkość transmisji bitów MP4 pliki lub pliki wygładzonymi Streaming adaptacyjne szybkość transmisji bitów. Następnie zgodnie z określonym formatem w wezwaniu manifest lub fragment Streaming na żądanie serwera zapewni odbierać strumień przez protokół, który został wybrany. W wyniku potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta.

Na poniższym diagramie przedstawiono tradycyjne kodowanie i opakowań statyczne przepływu pracy.

![Kodowanie statyczne](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Na poniższym diagramie przedstawiono dynamiczne opakowania przepływu pracy.

![Kodowanie dynamicznych](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Aby skorzystać z dynamicznego opakowania, możesz uzyskać co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak skala usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Typowy scenariusz

1. Przekaż plik wejściowy (nazywanych pliku kodery). Na przykład H.264, MP4 lub WMV (na liście Zobacz formaty obsługiwane [Formaty obsługiwane przez Media Encoder standardowy](media-services-media-encoder-standard-formats.md).

1. Kodowanie pliku kodery do zestawów adaptacyjne szybkość transmisji bitów H.264 MP4.

1. Publikowanie zawartości, która zawiera adaptacyjne szybkość transmisji bitów Ustawianie MP4, tworząc Locator na żądanie.

1. Tworzenie przesyłanie strumieniowe adresów URL, aby uzyskać dostęp do i przesyłać strumieniowo zawartości.


##<a name="preparing-assets-for-dynamic-streaming"></a>Przygotowywanie elementy zawartości dynamicznej strumieniowego przesyłania

Aby przygotować się do zawartości dynamicznej strumieniowego przesyłania masz dwie opcje:

1. [Przekazywanie pliku wzorca](media-services-dotnet-upload-files.md).
2. [Za pomocą Media Encoder standardowy encoder da H.264 MP4 adaptacyjne szybkość transmisji bitów zestawów](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Strumień zawartości](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Formaty, które nie są obsługiwane przez dynamiczne opakowań

Następujące formaty plików źródła nie są obsługiwane przez dynamiczne opakowanie.

- Pliki mp4 cyfrowego Dolby.
- Cyfrowe pliki wygładzonymi Dolby.

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
