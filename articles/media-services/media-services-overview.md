<properties 
    pageTitle="Omówienie usługi multimediów Azure i typowych scenariuszy | Microsoft Azure" 
    description="Ten temat zawiera omówienie usługi multimediów Azure" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Omówienie usługi multimediów Azure i typowych scenariuszy

Usługi multimediów Azure Microsoft to extensible opartej na chmurze platforma umożliwiająca programistom tworzenie skalowalna multimediów zarządzania i dostarczanie aplikacji. Usługi multimediów jest oparty na pozostałych interfejsów API, które umożliwiają bezpieczne przekazywanie, przechowywanie kodowanie i pakowanie zawartości audio lub wideo do zarówno na żądanie i rzeczywistych przesyłanie strumieniowe dostarczenia różnych klientów (na przykład telewizji, komputera i urządzeń przenośnych).

Można tworzyć kompleksowe przepływów pracy przy użyciu całkowicie usługi multimediów. Możesz również użyć składników innych firm dla niektórych części przepływu pracy. Na przykład kodowanie za pomocą kodera innej firmy. Następnie przekazać, ochrona, pakowanie, dostarczanie przy użyciu usługi multimediów.

Istnieje możliwość strumienia zawartości live lub dostarczania zawartości na żądanie. W tym temacie przedstawiono typowe scenariusze dostarczania usługi zawartości [na żywo](media-services-overview.md#live_scenarios) lub [na żądanie](media-services-overview.md#vod_scenarios). Temat jest również połączona inne odpowiednie tematy.

## <a name="sdks-and-tools"></a>Narzędzia i SDK

Tworzenie rozwiązań usługi multimediów, umożliwia:

- [Usługi multimediów interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Jedna z dostępnych klienta SDK:
- [Usługi multimediów azure SDK dla środowiska .NET](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK dla języka Java](https://github.com/Azure/azure-sdk-for-java)
- [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
- [Usługi multimediów Azure dla Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jest to wersję firmy Microsoft Node.js SDK. Jego obsługiwany przez społeczności, obecnie nie ma 100% zakresu AMS interfejsy API).
- Istniejące narzędzia:
- [Azure portal](https://portal.azure.com/)
- [Azure-Media--Eksploratora usług](https://github.com/Azure/Azure-Media-Services-Explorer) (Explorer azure usługi multimediów (AMSE) jest Winforms-C# aplikacji dla systemu Windows)

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

Można wyświetlić AMS ścieżek nauki:

- [AMS Live przesyłanie strumieniowe przepływu pracy](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS na żądanie Streaming przepływu pracy](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć korzystanie z usługi multimediów Azure, należy użyć następujących czynności:

3. Konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com).
2. Konto usługi multimediów Azure. Aby utworzyć konto usługi multimediów Azure za pomocą Azure portal, .NET lub interfejsu API usługi REST. Aby uzyskać więcej informacji zobacz [Tworzenie konta](media-services-portal-create-account.md).
3. (Opcjonalnie) Konfigurowanie środowiska projektowego. Wybierz .NET lub interfejsu API usługi REST dla środowiska programowania. Aby uzyskać więcej informacji zobacz [Konfigurowanie środowiska](media-services-dotnet-how-to-use.md).

Ponadto Dowiedz się, jak programowo Połącz [łączy](media-services-dotnet-connect-programmatically.md).
4. (Zalecane) Przydzielić jedną lub więcej jednostek skali. Zalecane jest przydzielić skali jeden lub więcej aplikacji w środowisku produkcyjnym.   Aby uzyskać więcej informacji zobacz [Zarządzanie streaming punktów końcowych](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Omówienie i pojęć

Aby pojęcia usługi multimediów Azure zobacz [pojęcia](media-services-concepts.md).

Instrukcje serii, której przedstawiono główne składniki usługi multimediów Azure zobacz [samouczki krok po kroku Azure multimediów usług](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Tę serię ma doskonałe Omówienie koncepcji i używa narzędzie AMSE celu zademonstrowania AMS zadania. Należy zauważyć, że narzędzie AMSE narzędzie systemu Windows. To narzędzie obsługuje większość zadań, które można uzyskać programowo z [SDK AMS dla środowiska .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK dla języka Java](https://github.com/Azure/azure-sdk-for-java)lub [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

##<a id="vod_scenarios"></a>Dostarczania multimediów na żądanie z usługi multimediów Azure: typowe scenariusze i zadania

W tej sekcji opisano typowe scenariusze i zawiera łącza do odpowiednich tematów. Na poniższym diagramie przedstawiono główne elementy platformy usługi multimediów, uwzględnianych w dostarczania zawartości na żądanie. 

![VoD przepływu pracy](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Ochrona zawartości w magazynie i dostarczania multimediów strumieniowych w programie Wyczyść (innych niż szyfrowane)

1. Przekaż plik kodery wysokiej jakości, do środka trwałego.
    
    Zalecane jest stosowanie opcja szyfrowania magazynowania do Twojej zawartości w celu ochrony zawartości podczas przekazywania oraz w stanie spoczynku w magazynie.
 
1. Kodowanie do zestawu plików adaptacyjne szybkość transmisji bitów MP4. 

    Zalecane jest stosowanie opcja szyfrowania magazynowania do zawartości wyjścia w celu ochrony zawartości spoczynku.
    
1. Konfigurowanie zasad dostarczania zawartości (używane przez dynamiczne opakowania). 
    
    Jeśli do zawartości jest szyfrowane miejsca do magazynowania, **musi** skonfigurować zasady dostarczania zawartości. 

1. Publikowanie elementu, tworząc locator na żądanie.

    Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.

1. Strumień opublikowana zawartość.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Ochrona zawartości w magazynie, przeprowadzania dynamicznie zaszyfrowanych strumieniowego przesyłania multimediów  

Aby można było używać szyfrowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę zastrzeżone przesyłanie strumieniowe przesyłanie strumieniowe końcowego, z której chcesz przesyłać strumieniowo zaszyfrowanej zawartości.

1. Przekaż plik kodery wysokiej jakości, do środka trwałego. Stosowanie opcji szyfrowania miejsca do magazynowania do elementu.
1. Kodowanie do zestawu plików adaptacyjne szybkość transmisji bitów MP4. Stosowanie opcji szyfrowania miejsca do magazynowania trwałym dane wyjściowe.
1. Tworzenie zawartości klucza szyfrowania dla zasobu, który ma zostać dynamicznie szyfrowane podczas odtwarzania.
2. Konfigurowanie zasad autoryzacji klucza zawartości.
1. Konfigurowanie zasad dostarczania zawartości (używane przez dynamiczne opakowań i szyfrowania dynamiczne).
1. Publikowanie elementu, tworząc locator na żądanie.
1. Strumień opublikowana zawartość. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Za pomocą analizy multimediów do uzyskania sankcji wniosków ze swoich klipów wideo 

Multimedia analizy to zbiór składników rozpoznawania mowy i wzroku, które ułatwiają organizacje i przedsiębiorstwa do uzyskania sankcji wniosków z ich plików wideo. Aby uzyskać więcej informacji zobacz [Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md).

1. Przekaż plik kodery wysokiej jakości, do środka trwałego.
2. Użyj jednej z następujących usług analiz multimediów przetwarzania wideo:
    
    - **Indeksowanie** — [proces klipów wideo z 2 indeksatora multimediów Azure](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** — [plików multimedialnych Hyperlapse z Hyperlapse multimediów Azure](media-services-hyperlapse-content.md)
    - **Wykrywanie ruchu** — [Wykrywania ruchu analiz multimediów Azure](media-services-motion-detection.md).
    - **Wykrywanie nominalnej i emocje nominalnej** — [nominalnej i wykrywania emocje analizy multimediów Azure](media-services-face-and-emotion-detection.md).
    - **Klip wideo podsumowania** — [Użyj Azure multimediów wideo miniatury do utworzenia podsumowania wideo](media-services-video-summarization.md)
3. Procesory multimediów analizy multimediów warzywa pliki MP4 lub JSON. Jeżeli procesor multimediów przedstawiony pliku MP4, stopniowo mogą pobrać plik. Jeżeli procesora multimedia przedstawiony pliku JSON, mogą pobrać plik z magazynu obiektów blob platformy Azure. 


###<a name="deliver-progressive-download"></a>Przedstawianie stopniowego pobierania 

1. Przekaż plik kodery wysokiej jakości, do środka trwałego.
1. Kodowanie w jednym pliku MP4.
1. Publikowanie elementu, tworząc locator na żądanie lub skojarzenia zabezpieczeń.

    Przy użyciu locator na żądanie, należy mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której ma zostać stopniowo pobieranie zawartości.

    Jeśli używasz locator skojarzeń zabezpieczeń, zawartość jest pobierana z magazynu obiektów blob platformy Azure. W takim przypadku nie musisz mieć streaming zastrzeżone jednostki.
  
1. Stopniowe pobieranie zawartości.

##<a id="live_scenarios"></a>Dostarczanie Live przesyłanie strumieniowe zdarzenia za pomocą usługi multimediów Azure

Podczas pracy z Live Streaming często obejmuje następujące składniki:

- Kamery, który służy do emisji zdarzenia.
- Live encoder wideo konwertuje sygnały z aparatu fotograficznego strumieni, które są wysyłane do programu live przesyłanie strumieniowe usługi.

Opcjonalnie wielu kodery live synchronizacji czasu. Dla niektórych krytyczne na żywo wydarzenia, że żądanie bardzo wysokiej dostępności i jakość działania i obsługi, zaleca się zatrudnić kodery zbędne aktywny aktywny z godziną synchronizacji Aby osiągnąć Bezproblemowa praca awaryjna bez utraty danych.
- Live usługa Przesyłanie strumieniowe umożliwia wykonaj następujące czynności:

- mogły zjeść tej ostatniej zawartości na żywo za pomocą różnych live protokołów (na przykład RTMP lub gładkie Streaming)
- (opcjonalnie) kodowanie strumienia do strumienia adaptacyjne szybkość transmisji bitów
- Wyświetl podgląd swojego strumienia aktywnego,
- Rejestrowanie i przechowywanie zasysanego zawartości w celu strumieniowe przesyłanie później (usługi wideo na żądanie)
- przedstawianie zawartości za pomocą typowych protokołów (na przykład MPEG ŁĄCZNIKA, gładkie, HLS, obr. / min) bezpośrednio do klientów lub do zawartości dostawy sieci (CDN) do dalszej dystrybucji.


**Usługi multimediów Azure firmy Microsoft** (AMS) umożliwia mogły zjeść tej ostatniej, kodowanie Podgląd, przechowywanie i dostarczanie live przesyłanie strumieniowe zawartości.

Podczas dostarczania zawartości klientom celem jest dostarczanie wideo o wysokiej jakości różnych urządzeń warunkach innej sieci. Aby zajmować warunki jakości i sieci, użyj live kodery do kodowania strumienia do strumienia wideo (adaptacyjne szybkość transmisji bitów) szybkość transmisji bitów wielu.  Aby zajmować streaming na różnych urządzeniach, umożliwia dynamicznie ponownie pakowanie strumienia do różnych protokołów usługi multimediów [dynamiczne opakowań](media-services-dynamic-packaging-overview.md) . Usługi multimediów obsługuje dostarczenie następujących adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

W usługi multimediów Azure, **kanały**, **programów**i **StreamingEndpoints** obsługiwać wszystkie live przesyłanie strumieniowe funkcje tym ingest, formatowanie, DVR, zabezpieczeń, skalowalność i nadmiarowości.

**Kanał** reprezentuje procesu przetwarzania zawartości przesyłanie strumieniowe na żywo. Kanał może odbierać live wprowadzania strumieni w następujący sposób:

- W lokalnym na żywo wysyła encoder szybkość transmisji bitów wielokrotne **RTMP** lub **Gładkie Streaming** (pofragmentowanych MP4) do kanału, który jest skonfigurowany do dostarczania **przekazującą** . Dostarczenie **przekazującą** jest, gdy zasysanego strumienie przechodzą przez **kanał**s bez dalszego przetwarzania. Możesz użyć następujące kodery live, które wyjściowy szybkość transmisji bitów wielokrotne wygładzonymi Streaming: pierwiastkowej, firmy Envivio, Cisco.  Następujące kodery live wyjściowy RTMP: transcoders programu Adobe Flash Live, Telestream Wirecast i Tricaster.  Live encoder możesz też wysyłać strumienia pojedynczy szybkość transmisji bitów do kanału, który nie jest włączona dla kodowanie live, ale nie jest zalecane. Żądanie, usługi multimediów zapewnia strumienia klientom.

>[AZURE.NOTE] Przekazująca metoda jest najbardziej ekonomicznych sposobem live przesyłanie strumieniowe, gdy robią wiele zdarzeń przez dłuższy czas, a następnie można już masz zainwestowanego w kodery lokalnego. Zobacz szczegóły [ceny](/pricing/details/media-services/) .

- Live kodera lokalne wysyła strumienia szybkość transmisji bitów pojedyncze do kanału, który jest skonfigurowany do przeprowadzania live kodowania z usługi multimediów w jednym z następujących formatów: RTP (MPEG-TS), RTMP lub gładkie Streaming (pofragmentowane MP4). Kanał następnie wykonuje live kodowanie przychodzące strumienia pojedynczy szybkość transmisji bitów ze strumieniem wideo wielokrotne-szybkość transmisji bitów (adaptacyjne). Żądanie, usługi multimediów zapewnia strumienia klientom.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Praca z kanałów, które otrzymujesz strumień na żywo szybkość transmisji bitów wielokrotne od kodery lokalnego (przekazywanie)

Na poniższym diagramie przedstawiono główne elementy platformy AMS uwzględnianych w przepływie pracy **przekazującą** .

![Live przepływu pracy][live-overview2]

Aby uzyskać więcej informacji, zobacz [Praca z kanałami tej opcji wielokrotne Odbierz-szybkość transmisji bitów strumień na żywo z lokalnego kodery](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Praca z kanałami, które służą do wykonywania live kodowania z usługi multimediów Azure

Na poniższym diagramie przedstawiono główne elementy platformy AMS uwzględnianych w przepływie pracy Live Streaming, których włączono kanału do wykonywania live kodowania z usługi multimediów.

![Live przepływu pracy][live-overview1]

Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Używające zawartości

Usługi multimediów Azure udostępnia narzędzia potrzebne do utworzenia klienta bogatych, dynamicznych odtwarzacza aplikacji dla większości platform, w tym: iOS urządzeń, urządzeń z systemem Android, Windows, Windows Phone, konsoli Xbox i dekodera pola. Poniższy temat znajdują się łącza do SDK i struktury odtwarzacza, który służy do tworzenia własnych aplikacji klienta, które mogą zajmować przesyłanie strumieniowe multimediów z usługi multimediów.

[Tworzenie aplikacji odtwarzacza wideo](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Włączanie Azure CDN

Usługi multimediów obsługuje integrację z Azure CDN. Aby uzyskać informacje na temat włączania Azure CDN zobacz [jak zarządzać Streaming punkty końcowe na koncie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Skalowanie konto usługi multimediów

Określenie numeru **Streaming zastrzeżone jednostki** i **Kodowanie zastrzeżone jednostki** , który ma swoje konto, aby zarządzać za pomocą używanych można skalować **Usługi multimediów** .

Można również skalować konta usługi multimediów, dodając do niego konta miejsca do magazynowania. Konto każdego miejsca do magazynowania jest ograniczone do 500 TB. Aby rozwinąć magazynie poza domyślne ograniczenia, można dołączyć wiele kont miejsca do magazynowania dla jednego konta usługi multimediów.

[Ten](media-services-portal-scale-streaming-endpoints.md) temat prowadzi do odpowiednich tematów.

##<a name="support"></a>Pomoc techniczna

[Obsługuje Azure](https://azure.microsoft.com/support/options/) udostępnia opcje pomocy technicznej dla Azure, w tym usługi multimediów.

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Umowa dotycząca poziomu usług (SLA)

- Do kodowania usługi multimediów możemy zagwarantować dostępność 99,9% transakcje interfejsu API usługi REST.
- Dla strumieniowych, możemy zostanie pomyślnie żądania obsługi z gwarancję dostępności 99,9% dla istniejących zawartości multimedialnej po co najmniej jedną jednostkę Streaming została zakupiona.
- Dla kanałów Live możemy zagwarantować, uruchomiony kanałów że połączeniami zewnętrznymi 99,9% czasu.
- Dla ochrony zawartości możemy zagwarantować, że zostaną pomyślnie dostarczane żądania klucza co najmniej 99,9% czasu.
- Dla indeksowania, możemy zostanie pomyślnie zadań indeksowania żądania obsługi przetwarzane z zastrzeżony kodowanie 99,9% jednostek czasu.

Aby uzyskać więcej informacji zobacz [Microsoft Azure Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
