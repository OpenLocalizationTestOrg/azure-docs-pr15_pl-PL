<properties
    pageTitle="Omówienie analizy usługi multimediów Azure | Microsoft Azure"
    description="Usługi multimediów Azure oferuje publicznej Podgląd analizy multimediów Azure, zbiór usług wzroku rozpoznawania mowy i komputera w skali przedsiębiorstwa, zgodności, zabezpieczenia i globalne. Usług Azure analizy multimediów są dostępne za pomocą podstawowe składniki platformy usługi multimediów Azure i w związku z tym są gotowe do obsługi multimediów przetwarzania w skali na jeden dzień. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Omówienie analizy usługi multimediów Azure

##<a name="overview"></a>Omówienie

Więcej organizacji i przedsiębiorstw są obejmującego wideo jako wybranym nośniku przeszkolenie pracowników, prowadzenie swoich klientów i funkcje firm dokumentu. Chmura obliczeniowych ułatwia skutecznych do przechowywania, strumienia i uzyskać dostęp do tych duże pliki multimedialne, ale jak firmy powiększanie ich biblioteki zawartości wideo, muszą mieć jednakowo skuteczny środek się nowe wnioski z wideo w celu utworzenia bardziej zrozumiały spersonalizowanej interakcji z ich odbiorców i trudniejsze zagadnienia dotyczące ich działalności.

Aby rozwiązać ten rosnących potrzeb na rynku, usługi multimediów Azure oferuje analizy multimediów, zbiór składników rozpoznawania mowy i wzroku (w skali przedsiębiorstwa, zgodności, zabezpieczenia i globalne), które ułatwiają organizacje i przedsiębiorstwa do uzyskania sankcji wniosków z ich plików wideo. Usług Azure analizy multimediów są dostępne za pomocą podstawowe składniki platformy usługi multimediów Azure i w związku z tym są gotowe do obsługi multimediów przetwarzania w skali na jeden dzień.

Azure analizy multimediów umożliwia projektantom szybko rozpocząć pracę z możliwości widzenia bądź wideo w ograniczonym zakresie i wyświetlić to zaawansowanych funkcji do aplikacji. Azure analizy multimediów jest wbudowany mają być używane przez środowiskach przedsiębiorstwa przy użyciu skali, zgodności, zabezpieczenia i globalne wymagane w dużych organizacjach.

Na poniższym diagramie przedstawiono **Analizy multimediów** i innych głównych części platformy usługi multimediów. 

![VoD przepływu pracy](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Procesory multimediów analizy multimediów warzywa pliki MP4 lub JSON. Jeżeli procesor multimediów przedstawiony pliku MP4, stopniowo mogą pobrać plik. Jeżeli procesora multimedia przedstawiony pliku JSON, mogą pobrać plik z magazynu obiektów blob platformy Azure. 

## <a name="azure-media-analytics-services"></a>Usług analiz multimediów Azure

- **Indeksowanie** — indeksatora multimediów Azure umożliwia ułatwienia zawartości wyszukiwania, a także wygenerować ścieżki podpisów kodowanych. Usługi multimediów Azure wydana **Azure multimediów indeksatora 2 Preview** w szybszym indeksowania i szerszych obsługę języka. Obsługiwane języki zawierać angielski, hiszpański, francuski, niemiecki, włoski, chiński, portugalski i arabski. Aby uzyskać szczegółowe informacje i przykłady zobacz [proces klipów wideo z 2 indeksatora multimediów Azure](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** — Microsoft Hyperlapse wynika z ponad dwudziestu lat badań wzroku komputera w programie Microsoft Research (MSR), połączenie wideo stabilizacji i czasu wygaśnięcie tworzenie szybki, eksploatacyjnych, atrakcyjnych klipów wideo z formularza długie zawartości. Oprócz tworzenia czasu wygaśnie, umożliwia także Hyperlapse przekształcania obraz wideo przechwycone za pomocą telefonów komórkowych i kamery wideo stabilny. Aby uzyskać szczegółowe informacje i przykłady zobacz [Pliki multimedialne Hyperlapse z Hyperlapse multimediów Azure](media-services-hyperlapse-content.md)
 
- **Wykrywanie ruchu** — ta usługa umożliwia wykrywanie ruchu w klipu wideo z tła papeterii. Jest to idealne rozwiązanie w przypadku klientów, którzy chcesz sprawdzić wyniki fałszywie dodatnie na zdarzeniach ruchu wykrytych przez aparaty nadzoru na źródeł wideo nadzoru. Aby uzyskać szczegółowe informacje i przykłady zobacz [Wykrywanie ruchu analiz multimediów Azure](media-services-motion-detection.md).
 
- **Wykrywanie nominalnej i emocje nominalnej** — przy użyciu tej usługi można wykryć powierzchni innych osób i ich emocje, w tym szczęście, sadness, niespodzianek, gniew, contempt, dostępność, wstręt i indifference neutralność. To jest kilka przydatne branżowe aplikacji, opisane poniżej, także agregacji i analizować reakcji uczestników wydarzenia. Aby uzyskać szczegółowe informacje i przykłady zobacz [nominalnej i wykrywania emocje analizy multimediów Azure](media-services-face-and-emotion-detection.md).
 
- **Klip wideo podsumowania** — wideo podsumowania umożliwia tworzenie podsumowania długie klipów wideo automatycznie wybierając interesujące wstawki z źródła wideo. Jest to przydatne, gdy chcesz podać krótkie omówienie czego można oczekiwać w długich wideo. Aby uzyskać szczegółowe informacje i przykłady zobacz [Używanie Azure multimediów wideo miniatury tworzenie podsumowania klip wideo](media-services-video-summarization.md)

- **Optycznego rozpoznawania znaków** — Azure multimediów analizy optycznego rozpoznawania znaków (OCR) umożliwia konwertowanie tekstu zawartości plików wideo można wyszukiwać, które można edytować tekst cyfrowy. Dzięki temu będzie można zautomatyzować wyodrębnianie metadanych zrozumiałej z sygnału wideo multimediów.
 
- **Skalowalna nominalnej redakcyjny** - **Redactor multimediów Azure** jest CR analizy multimediów Azure, oferująca redakcyjny skalowalna nominalnej w chmurze. Redakcyjny nominalnej umożliwia modyfikowanie ustawień wideo w celu Rozmycie powierzchni wybrane osoby. Warto korzystać z usługi redakcyjny nominalnej w publicznej scenariusze bezpieczeństwa i multimediów wiadomości. Kilka minut materiału zawierającego wiele powierzchni może potrwać godzin Zredaguj ręcznie, ale z tej usługi proces redakcyjny nominalnej wymaga tylko kilku prostych krokach. Aby uzyskać więcej informacji zobacz [ten](media-services-face-redaction.md) artykuł.

 
## <a name="common-scenarios"></a>Typowe scenariusze

Poniżej przedstawiono kilka scenariuszy, gdzie analizy multimediów Azure może pomóc w organizacji i przedsiębiorstw w branży zgromadzonych nowe wnioski z wideo, aby utworzyć bardziej spersonalizowanej odbiorców i pozyskiwaniu pracowników, a także efektywniejsze zarządzanie dużą ilość zawartości wideo:

- **Wyśrodkowanie połączenia** — nawet pojawienie społecznościowych połączenia klienta centra nadal ułatwienia duży procent transakcji usług klienta. Zakodowany w tym dane audio jest wiele informacji o klientach, które można analizować, możesz zwiększyć planów produktu i również zapewnianie szkoleń pracowników Centrum połączenia uzyskanie wyższą zadowolenia klientów. Przy użyciu indeksowania multimediów Azure, klienci będą mogli wyodrębnić tekst i tworzenie indeksu wyszukiwania i pulpitów nawigacyjnych, aby wyodrębnić analizy wokół najczęściej narzeka, narzeka źródła i inne odpowiednie dane.

- **Moderowanie zawartości utworzona przez użytkownika** — z gniazdek multimediów wiadomości do działów policji wiele organizacji ma publicznego portali przeciwległych miejsce, w którym akceptują UGC multimedialne, takie jak klipy wideo i obrazów. Wielkość zawartości może zwiększyć z powodu nieoczekiwanych zdarzeń. W poniższych scenariuszach jest pobliżu niemożliwe prowadzenia skutecznych recenzji ręcznego zawartości dla właściwości. Klienci polega na usługę moderowanie zawartości umożliwia skoncentrowanie się na zawartość, która jest odpowiedni.

- **Nadzór** — wzrostu aparaty IP jest rozłożenie nadzoru klipów wideo. Ręczne przeglądanie wyników nadzoru klip wideo jest intensywnie i podatne na błędu człowieka. Azure analizy multimedia udostępnia kilka składników, takie jak wykrywanie ruchu, wykrywania nominalnej i Hyperlapse ten proces recenzowania oraz tworzenie pochodnych jest łatwiejsze i zarządzania nimi.

## <a name="media-services-analytics-media-processors"></a>Procesory multimediów analizy usługi multimediów 

W tej sekcji przedstawiono wszystkie multimedia usług analizy multimedia procesorów (CR), oraz pokazuje jak używać .NET lub pozostałych uzyskać obiektu CR.

### <a name="mp-names"></a>Nazwy CR


- Podgląd indeksatora 2 multimediów Azure
- Indeksowanie multimediów Azure
- Hyperlapse multimediów Azure
- Wykrywanie nominalnej multimediów Azure
- Wykrywanie ruchu multimediów Azure
- Miniatury wideo multimediów Azure
- OCR multimediów Azure

### <a name="net"></a>.NET

Poniższa funkcja ma jedną z nazwy CR i przywrócić obiekt CR.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>POZOSTAŁE

Żądanie:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Odpowiedź:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Pokazy

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Artykuły pokrewne

[Zawiadomienie o multimediów usług analiz](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
