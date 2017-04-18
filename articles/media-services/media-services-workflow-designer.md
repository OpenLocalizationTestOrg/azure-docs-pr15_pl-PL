<properties 
    pageTitle="Tworzenie zaawansowanych kodowania przepływów pracy przy użyciu projektanta przepływów pracy | Microsoft Azure" 
    description="Informacje na temat sposobu tworzenia zaawansowane kodowania przepływów pracy przy użyciu projektanta przepływów pracy." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Tworzenie zaawansowanych kodowania przepływów pracy przy użyciu projektanta przepływów pracy

##<a name="overview"></a>Omówienie

**Projektant przepływów pracy** jest narzędziem pulpitu systemu Windows, które umożliwia projektowanie i tworzenie niestandardowych przepływów pracy dla kodowanie w **Media Encoder Premium w przepływie pracy**.
Korzystając z możliwości narzędzia Projektant przepływów pracy, można projektować i tworzyć złożone przepływy pracy, które będą działać w **Media Encoder Premium**.  

Przepływy pracy mogą zawierać logiczny decyzji klienta i według gałęzi właściwości pliku źródła danych wejściowych. Przepływy pracy można tworzyć za pomocą żaden właściwości i wartości dynamiczne nawet najbardziej złożonych zadań kodowania ułatwia Powtórz i dostosowywanie w chmurze.

Przykład przepływów pracy, które można utworzyć obejmują:

- Decyzja oparta przepływów pracy, które inspekcja zawartości źródła rozdzielczości i kodowanie tylko ścieżki żądany.  To jest helfpul dzięki wyeliminowaniu nieużywanego ścieżki wygenerowanych przez upscaling nieumyślnego zawartości źródła.
- Istnieje wiele plików wejściowych może służyć do obsługi podpisów, nakładki i łączenie zawartości razem. 

To narzędzie może również zmodyfikować naszych [opublikowany przepływy pracy](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Aby uzyskać kopię narzędzia Projektant przepływów pracy, skontaktuj się z mepd@microsoft.com.


Po utworzeniu pliku przepływu pracy można przekazywać jako środka trwałego, a następnie można użyć do kodowania plików multimedialnych. Aby uzyskać informacje na temat kodowanie z **Media Encoder Premium w przepływu pracy** przy użyciu **.NET**zobacz [Zaawansowane kodowanie w Media Encoder Premium w przepływie pracy](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Modyfikowanie istniejącego przepływów pracy

Domyślne [opublikowany przepływy pracy](media-services-workflow-designer.md#existing_workflows) można modyfikować za pomocą projektanta narzędzia. Zostanie wyświetlony domyślny pliki przepływu pracy [w tym miejscu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Folder zawiera również opis tych plików.

Następujące klipy wideo pokazano, jak za pomocą projektanta.

###<a name="day-1--getting-started"></a>Dzień 1 — wprowadzenie

Klip wideo 1 dzień obejmuje:

- Omówienie projektanta
- Podstawowych przepływów pracy — "Witaj świecie"
- Tworzenie wielu wyprowadzania plików MP4 do użytku z streaming usługi multimediów Azure

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Dzień 2

Klip wideo 2 dnia obejmuje:

- Różne scenariusze plik źródłowy — Obsługa audio
- Przepływy pracy z zaawansowanych warunków logicznych
- Etapy wykresu

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>3 dni

Klip wideo 3 dni obejmuje:

- Wykonywanie skryptów wewnątrz przepływów pracy i schematów
- Ograniczenia dotyczące z bieżącego kodera
- Pytania i odpowiedzi
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Jeśli potrzebujesz pomocy technicznej lub masz pytania na temat tworzenia niestandardowych przepływów pracy w narzędziu Projektant przepływów pracy, Wyślij wiadomość e-mail do mepd@microsoft.com.

##<a name="see-also"></a>Zobacz też

[Kursy szkoleniowe projektanta przepływów pracy kodera Azure Premium](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
