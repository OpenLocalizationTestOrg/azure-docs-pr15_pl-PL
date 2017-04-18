<properties 
    pageTitle="Omówienie Live parze przy użyciu usługi multimediów Azure | Microsoft Azure" 
    description="Ten temat zawiera omówienie live parze przy użyciu usługi multimediów Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Omówienie Live parze przy użyciu usługi multimediów Azure

##<a name="overview"></a>Omówienie

W trakcie wydarzeń przesyłanie strumieniowe na żywo z usługi multimediów Azure często obejmuje następujące składniki:

- Kamery, który służy do emisji zdarzenia.
- Live encoder wideo konwertuje sygnały z aparatu fotograficznego strumieni, które są wysyłane do programu live przesyłanie strumieniowe usługi.

    Opcjonalnie wiele czasu live synchronizowane kodery. Dla niektórych krytyczne na żywo wydarzenia, że żądanie bardzo wysokiej dostępności i jakość działania i obsługi, zaleca się zatrudnić kodery zbędne aktywny aktywny synchronizacja czasu uzyskanie Bezproblemowa praca awaryjna bez utraty danych.
- Live usługa Przesyłanie strumieniowe umożliwia wykonaj następujące czynności:
    
    - mogły zjeść tej ostatniej zawartości na żywo za pomocą różnych live protokołów (na przykład RTMP lub gładkie Streaming)
    - (opcjonalnie) kodowanie strumienia do strumienia adaptacyjne szybkość transmisji bitów
    - Wyświetl podgląd swojego strumienia aktywnego,
    - Rejestrowanie i przechowywanie zasysanego zawartości w celu strumieniowe przesyłanie później (usługi wideo na żądanie)
    - przedstawianie zawartości za pomocą typowych protokołów (na przykład MPEG ŁĄCZNIKA, gładkie, HLS, obr. / min) bezpośrednio do klientów lub do zawartości dostawy sieci (CDN) do dalszej dystrybucji.


**Usługi multimediów Azure firmy Microsoft** (AMS) umożliwia mogły zjeść tej ostatniej, kodowanie Podgląd, przechowywanie i dostarczanie live przesyłanie strumieniowe zawartości.

Podczas dostarczania zawartości klientom celem jest dostarczanie wideo o wysokiej jakości różnych urządzeń warunkach innej sieci. Aby osiągnąć ten cel, umożliwia kodowanie strumienia ze strumieniem wideo (adaptacyjne szybkość transmisji bitów) szybkość transmisji bitów wielokrotne kodery live.  Aby zajmować streaming na różnych urządzeniach, umożliwia dynamicznie ponownie pakowanie strumienia do różnych protokołów usługi multimediów [dynamiczne opakowań](media-services-dynamic-packaging-overview.md) . Usługi multimediów obsługuje dostarczenie następujących adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

W usługi multimediów Azure, **kanały**, **programów**i **StreamingEndpoints** obsługiwać wszystkie live przesyłanie strumieniowe funkcje tym ingest, formatowanie, DVR, zabezpieczeń, skalowalność i nadmiarowości.

**Kanał** reprezentuje procesu przetwarzania zawartości przesyłanie strumieniowe na żywo. Kanał może odbierać live wprowadzania strumieni w następujący sposób:

- W lokalnym na żywo wysyła encoder szybkość transmisji bitów wielokrotne **RTMP** lub **Gładkie Streaming** (pofragmentowanych MP4) do kanału, który jest skonfigurowany do dostarczania **przekazującą** . Dostarczenie **przekazującą** jest, gdy zasysanego strumienie przechodzą przez **kanał**s bez dalszego przetwarzania. Możesz użyć następujące kodery live, które wyjściowy szybkość transmisji bitów wielokrotne wygładzonymi Streaming: pierwiastkowej, firmy Envivio, Cisco.  Następujące kodery live wyjściowy RTMP: transcoders programu Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast i Tricaster.  Live encoder możesz też wysyłać strumienia pojedynczy szybkość transmisji bitów do kanału, który nie jest włączona dla kodowanie live, ale nie jest zalecane. Żądanie, usługi multimediów zapewnia strumienia klientom.

    >[AZURE.NOTE] Przekazująca metoda jest najbardziej ekonomicznych sposobem przesyłanie strumieniowe na żywo gdy robią wiele zdarzeń przez dłuższy czas, a następnie można już masz zainwestowanego w kodery lokalnego. Zobacz szczegóły [ceny](/pricing/details/media-services/) .
    
    
- Live kodera lokalne wysyła strumienia szybkość transmisji bitów pojedyncze do kanału, który jest skonfigurowany do przeprowadzania live kodowania z usługi multimediów w jednym z następujących formatów: RTMP lub gładkie Streaming (pofragmentowanych MP4). RTP (MPEG-TS) jest również obsługiwane, pod warunkiem, że połączenie dedykowane centrum danych Azure. Następujące kodery live z wyjściowe RTMP wiadomo, że praca z kanałami tego typu: Telestream Wirecast, FMLE. Kanał następnie wykonuje live kodowanie przychodzące strumienia pojedynczy szybkość transmisji bitów ze strumieniem wideo wielokrotne-szybkość transmisji bitów (adaptacyjne). Żądanie, usługi multimediów zapewnia strumienia klientom.


Począwszy od wersji 2.10 usługi multimediów, podczas tworzenia kanału, można określić w jaki sposób chcesz kanału do odbierania strumienia wejściowego i tego, czy chcesz kanału do wykonywania live kodowanie strumienia. Dostępne są dwie opcje:

- **Brak** (przekazywanie) — określ tę wartość, jeśli zamierzasz używać live kodera lokalnego, na którym będzie wyjściowy szybkość transmisji bitów wielokrotne strumienia (strumień przekazująca). W tym przypadku przychodzących strumienia przesłany do wyników bez kodowanie. Jest to zachowanie kanału przed wersji 2.10.  

- **Standardowe** — wybierz wartość tak, jeśli zamierzasz używać usługi multimediów do kodowania strumienia live pojedynczy szybkość transmisji bitów strumieniu szybkość transmisji bitów wielu. Ta metoda jest bardziej ekonomicznych skalowania w górę szybko rzadko zdarzeń. Należy pamiętać, że istnieje rozliczeń wpływ na żywo kodowania i należy pamiętać, że pozostawienie live kanału kodowania w stanie "Uruchomiony" będzie powodowało rozliczeń opłat.  Zaleca się natychmiast Zatrzymaj uruchomionego kanałów po ukończeniu celu uniknięcia opłat za dodatkowe godzinę na żywo przesyłanie strumieniowe wydarzenie. 

##<a name="comparison-of-channel-types"></a>Porównanie typów kanałów

Poniższa tabela zawiera przewodnik do porównywania dwóch typów kanałów obsługiwane w usługach multimediów

Funkcja|Kanał przekazująca|Wzorzec kanał
---|---|---
Wprowadzania pojedynczego szybkość transmisji bitów jest kodowane do wielu bitrates w chmurze|Brak|Tak
Rozdzielczość maksymalna liczba warstw|1080p, 8 warstwy 60 + k/s|720p 6 warstwy, 30 k/s
Protokoły wprowadzania danych|RTMP, gładkie strumieniowego przesyłania|RTMP, gładkie Streaming i RTP
Cena|[Cennik strony](/pricing/details/media-services/) i kliknij kartę "Live Video"|[Cennik strony](/pricing/details/media-services/) 
Maksymalny czas działania|24 x 7|8 godzin
Obsługa Wstawianie kreda|Brak|Tak
Obsługa sygnalizacji ad|Brak|Tak
Przekazywanie CEA 608/708 podpisów|Tak|Tak
Możliwość odzyskiwanie z krótkim wstrzymania w udziale kanału|Tak|Nie (kanału rozpocznie slating sekund 6 + bez wprowadzania danych)
Obsługa-uniform GOPs wprowadzania danych|Tak|Nie — dane wejściowe muszą być ustalone 2 s GOPs
Obsługa ramki zmiennych stawek wprowadzania|Tak|Nie — wprowadzania należy ustalić szybkość odtwarzania.<br/>Niewielkich zmian są dopuszczalne, na przykład podczas ruchu wysoka sceny. Ale encoder nie można usunąć do 10 ramek na sekundę.
Automatyczne zawór kanałów podczas wprowadzania kanału informacyjnego zostaną utracone|Brak|Po 12 godzin, jeśli istnieje nie uruchomiony Program 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Praca z kanałów, które otrzymujesz strumień na żywo szybkość transmisji bitów wielokrotne od kodery lokalnego (przekazywanie)

Na poniższym diagramie przedstawiono główne elementy platformy AMS uwzględnianych w przepływie pracy **przekazującą** .

![Live przepływu pracy](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Aby uzyskać więcej informacji, zobacz [Praca z kanałami tej opcji wielokrotne Odbierz-szybkość transmisji bitów strumień na żywo z lokalnego kodery](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Praca z kanałami, które służą do wykonywania live kodowania z usługi multimediów Azure

Na poniższym diagramie przedstawiono główne elementy platformy AMS uwzględnianych w przepływie pracy Live Streaming, których włączono kanału do wykonywania live kodowania z usługi multimediów.

![Live przepływu pracy](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Opis kanału i ich powiązanych elementów

###<a name="channel"></a>Kanał

W usługach multimediów s [kanału](https://msdn.microsoft.com/library/azure/dn783458.aspx)są odpowiedzialne za przetwarzania zawartości przesyłanie strumieniowe na żywo. Kanał zawiera punktu końcowego wprowadzania (mogły zjeść tej z adresu URL ostatniej) następnie podaj live transcoder. Kanał otrzyma wprowadzania strumieni na żywo z live transcoder i udostępnia do strumieniowego przesyłania przez jeden lub więcej StreamingEndpoints. Kanały zapewniają również końcowy preview (Podgląd URL) umożliwia wyświetlanie podglądu i sprawdź strumienia przed dostarczenia i dalszego przetwarzania.

Po utworzeniu kanału można uzyskać adres URL ingest oraz adres URL podglądu. Aby uzyskać te adresy URL, kanału nie ma być w stanie uruchomienia. Gdy rozpocząć przekazywanie danych z programu live transcoder do tego kanału, musi być uruchomiona kanału. Po uruchomieniu programu live transcoder ingesting danych można wyświetlić podgląd swojego strumienia.

Każdego konta usługi multimediów może zawierać wiele kanałów, wiele programów i wielu StreamingEndpoints. W zależności od potrzeb przepustowości i zabezpieczenia usługi StreamingEndpoint mogą być przeznaczone dla jednego lub kilku kanałów. Dowolny StreamingEndpoint można pobierać z dowolnego kanału.


###<a name="program"></a>Program 

[Program](https://msdn.microsoft.com/library/azure/dn783463.aspx) umożliwia sterowanie publikowania i przechowywania segmentów strumień na żywo. Kanały Zarządzanie programów. Relacja kanału i Program jest bardzo podobne do tradycyjnych multimediów, gdzie kanał zawiera stałą strumienia zawartości i program jest ograniczone do niektórych czasowy zdarzenia, w tym kanale.
Można określić liczbę godzin, aby zachować zarejestrowane zawartości programu przez ustawienie właściwości **ArchiveWindowLength** . Tej wartości można ustawić z co najmniej 5 minut maksymalnie 25 godzin. 

ArchiveWindowLength również decyduje o tym, że maksymalnej ilości czasu klienci mogą Szukanie wstecz od bieżącej pozycji live. Programy mogą być uruchamiane przez określoną ilość czasu, ale zawartość, która znajduje się za długość okna wciąż jest pomijany. Tej wartości tej właściwości określa również, jak długo można powiększać manifesty klienta.

Każdy program jest skojarzony z środka trwałego. Aby opublikować programu możesz utworzyć locator skojarzony element. O tym locator umożliwi tworzenie przesyłanie strumieniowe adres URL, który można utworzyć dla klientów.

Kanał obsługuje maksymalnie trzy jednocześnie uruchomione programy, więc można utworzyć wiele archiwami strumieniu przychodzących. Dzięki temu można opublikować i archiwizowanie różne części zdarzenia, stosownie do potrzeb. Na przykład z wymaganiami firm jest archiwizowania 6 godzin program, ale do emisji tylko ostatnie 10 minut. Aby to zrobić, należy utworzyć dwa jednocześnie uruchomione programy. Jeden program jest ustawiona na archiwizowanie 6 godzin zdarzenia, ale program nie jest opublikowany. Inny program jest ustawiona na archiwizowanie przez 10 minut, a ten program jest opublikowany.


##<a name="billing-implications"></a>Skutki rozliczeń

Kanał rozpoczyna się rozliczenia zaraz za pośrednictwem interfejsu API jest stan przejścia na "Uruchomiony".  

W poniższej tabeli pokazano, jak Zjednoczone kanału mapowanie Zjednoczone rozliczeń w portalu Azure i interfejsu API. Zauważ, że stanach są nieco różnić między UX. portalu i interfejsu API Po zainicjowaniu kanału jest w stanie "Uruchamianie" za pośrednictwem interfejsu API lub w stanie "Gotowe" lub "Strumieniowych" w portalu Azure, rozliczeń będzie aktywna.

Aby zatrzymać kanału z rachunków możesz dodatkowo, należy zatrzymać kanału za pośrednictwem interfejsu API lub w portalu Azure.
Odpowiadają zatrzymywania kanałów po wykonaniu tych czynności z kanałem. Błąd, aby zatrzymać kanale spowoduje dalsze rozliczenia.

>[AZURE.NOTE]Podczas pracy z kanałów standardowych, AMS zostanie automatycznie zawór dowolny kanał, który jeszcze znajduje się w stanie "Uruchomiony" 12 godzin źródło wprowadzania danych jest tracone i nie programów. Jednak będą nadal naliczane po raz kanał był w stanie "Uruchomiony".

###<a id="states"></a>Zjednoczone kanału i sposób mapowania do trybu rozliczeń 

Bieżący stan kanału. Możliwe wartości:

- **Zatrzymano**. Jest to początkowy stan kanału po jego utworzeniu (chyba że autostart została wybrana w portalu). Rozliczenia nie występuje w tym trybie. W tym stanie właściwości kanału można aktualizować, ale streaming nie jest dozwolone.
- **Uruchamianie**. Zostanie uruchomiona kanału. Rozliczenia nie występuje w tym trybie. Żadne aktualizacje lub strumieniowego przesyłania są niedozwolone w tym stanie. Jeśli wystąpi błąd, kanał zwraca do stanu zatrzymania.
- **Uruchamianie**. Kanał jest w stanie przetwarzania strumieni na żywo. Teraz jest on rozliczenia zastosowania. Należy zatrzymać kanału, aby zapobiec dodatkowo rozliczenia. 
- **Zatrzymywanie**. Zatrzymanie kanału. Rozliczenia nie występuje w tym stanie przejściowych. Żadne aktualizacje lub strumieniowego przesyłania są niedozwolone w tym stanie.
- **Usuwanie**. Kanał jest usuwany. Rozliczenia nie występuje w tym stanie przejściowych. Żadne aktualizacje lub strumieniowego przesyłania są niedozwolone w tym stanie.

W poniższej tabeli pokazano, jak Stany kanału mapowanie do trybu rozliczeń. 
 
Stan kanału|Wskaźniki portalu interfejsu użytkownika|To jest karta?
---|---|---
Uruchamianie|Uruchamianie|Nie (stan przejściowych)
Uruchamianie|Gotowe (nie uruchomione programy)<br/>lub<br/>Przesyłanie strumieniowe (co najmniej jeden uruchomiony program)|TAK
Zatrzymywanie|Zatrzymywanie|Nie (stan przejściowych)
Zatrzymano|Zatrzymano|Brak


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Tematy pokrewne

[Usługi multimediów Azure Live pofragmentowane MP4 mogły zjeść tej ostatniej Specyfikacja](media-services-fmp4-live-ingest-overview.md)

[Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md)

[Praca z kanałami, które otrzymane szybkość transmisji bitów wielokrotne strumień na żywo z lokalnego kodery](media-services-live-streaming-with-onprem-encoders.md)

[Przydziały i ograniczenia](media-services-quotas-and-limitations.md).  

[Pojęcia dotyczące usługi multimediów](media-services-concepts.md)
