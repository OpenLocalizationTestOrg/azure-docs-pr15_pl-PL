<properties 
    pageTitle="Można opracowywać aplikacje odtwarzacza wideo" 
    description="Temat zawiera łącza do odtwarzacza struktury i dodatki plug-in, który służy do tworzenia własnych aplikacji klienta, które mogą zajmować przesyłanie strumieniowe multimediów z usługi multimediów." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Można opracowywać aplikacje odtwarzacza wideo

##<a name="overview"></a>Omówienie

Usługi multimediów Azure udostępnia narzędzia potrzebne do utworzenia klienta bogatych, dynamicznych odtwarzacza aplikacji dla większości platform, w tym: iOS urządzeń, urządzeń z systemem Android, Windows, Windows Phone, konsoli Xbox i dekodera pola. W tym temacie również znajdują się łącza do SDK i struktury odtwarzacza, który służy do tworzenia własnych aplikacji klienta, które mogą zajmować przesyłanie strumieniowe multimediów z usługi multimediów Azure.

##<a name="azure-media-player"></a>Odtwarzacza multimediów Azure

[Azure Media Player](http://aka.ms/ampinfo) to odtwarzacza wideo w sieci web wbudowany odtwarzanie zawartości multimedialnej z usługi multimediów Azure firmy Microsoft na wielu różnych przeglądarek i urządzeń. Azure Media Player wykorzystuje standardami, takich jak HTML5, rozszerzenia źródła multimediów (MSE) i rozszerzenia szyfrowane multimediów (EME), aby zapewnić wzbogaconego adaptacyjne obsługi przesyłanie strumieniowe. Gdy normy te są dostępne na urządzeniu lub w przeglądarce, Azure Media Player używa Flash i Silverlight technologii alternatywnych. Bez względu na to stosowana technologia odtwarzanie deweloperów uzyskuje ujednolicony interfejs JavaScript, aby uzyskać dostęp do interfejsów API. Dzięki temu zawartość obsługiwane przez usługi multimediów Azure ma być odtwarzany w zakresie całego urządzenia i przeglądarki bez dowolny dodatkowy nakład pracy.

Usługi multimediów Azure firmy Microsoft umożliwia zawartości, która jest obsługiwane przy użyciu ŁĄCZNIKA, gładkie Streaming i HLS streaming format do odtwarzania zawartości. Azure Media Player uwzględnia tych różnych formatów i automatycznie odtwarza zalecane łącze możliwości platformy i przeglądarki. Usługi multimediów Azure firmy Microsoft umożliwia również dynamiczne szyfrowanie aktywów przez szyfrowanie PlayReady lub AES 128-bitowego szyfrowania koperty. Azure Media Player umożliwia odszyfrowywanie PlayReady a AES 128-bitowego zaszyfrowanej zawartości, gdy odpowiednio skonfigurowane. 

Aby uzyskać więcej informacji:

- [Odtwarzacza multimediów Azure](http://aka.ms/ampinfo)
- [Dokumentacja odtwarzacza multimediów Azure](http://aka.ms/ampdocs) 
- [Azure odtwarzacz wprowadzenie pracę blogu](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Zarejestruj się, aby być na bieżąco z najnowszymi z Azure Media Player](http://aka.ms/ampsignup)
- [Dodawanie nowych wezwań na funkcji, pomysłów, opinii](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Inne narzędzia do tworzenia aplikacji odtwarzacza

Możesz również użyć dowolnej z SDK następujące:

- [Wygładzanie przesyłanie strumieniowe klienta SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Gładkie przesyłanie strumieniowe aplikacji ze Sklepu Windows](media-services-build-smooth-streaming-apps.md)
- [Platformy Microsoft Media: Struktura odtwarzacza](http://playerframework.codeplex.com/) 
- [HTML5 Dokumentacja Framework odtwarzacza](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Wygładzanie Microsoft Streaming wtyczki dla OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Licencjonowania wygładzonymi klienta przesyłanie strumieniowe Microsoft® przenoszenia Kit](http://aka.ms/sspk) 
- [Tworzenie aplikacji wideo konsoli XBOX](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Reklamę

Usługi multimediów Azure umożliwia wstawianie reklam platformy Windows Media: RAM odtwarzacza. Ramy odtwarzacza z obsługą ad są dostępne dla systemu Windows 8, Silverlight, Windows Phone 8 i iOS urządzeń. Każde z ram odtwarzacza zawiera przykładowy kod, który pokazano, jak wdrażać aplikacja odtwarzacza. Istnieją trzy różne typy reklam, które można wstawiać do multimediów:

Liniowy — reklam pełny ramki Wstrzymaj wideo głównym

Nieliniowa — nakładki reklam, które są wyświetlane podczas odtwarzania wideo głównym, zazwyczaj logo lub inny obraz statyczny umieszczony w odtwarzaczu

Pomocnik — reklam, wyświetlane poza odtwarzacza

Oferty można umieścić w dowolnym miejscu na osi czasu głównym klip wideo. Odtwarzacza należy określić, kiedy odtwarzać ad i które reklam, aby odtworzyć. Można to zrobić przy użyciu zestawu standardowych plików opartym na języku XML: wideo Ad usługi szablonu (VAST), cyfrowego wideo wielu Ad odtwarzania (VMAP), szablon abstrakcyjne harmonogramu w multimediów (KAT) i cyfrowego wideo odtwarzacza Ad interfejsu definicji (VPAID). DUŻYCH plików Określ, jakie reklam, aby wyświetlić. Pliki VMAP Określ, kiedy odtwarzać różne reklam i zawierają OGROMNĄ XML. Pliki kat są reklam sekwencji, które zawierają można również OGROMNĄ XML w inny sposób. Pliki VPAID Definiowanie interfejs między odtwarzacza wideo i ad lub serwer usług ad. Aby uzyskać więcej informacji zobacz [Wstawianie reklam](https://msdn.microsoft.com/library/dn387398.aspx).

Informacje zamknięty obsługi podpisów i reklam w Live strumieniowego przesyłania wideo zobacz [obsługiwane kodowane i standardy wstawiania Ad](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Osadzanie adaptacyjne przesyłanie strumieniowe wideo MPEG-kreska w aplikacji HTML5 z DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[Repozytorium dash.js GitHub](https://github.com/Dash-Industry-Forum/dash.js)
 
