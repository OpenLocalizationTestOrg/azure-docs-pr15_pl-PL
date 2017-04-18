<properties 
    pageTitle="Azure usługi multimediów live pofragmentowane MP4 mogły zjeść tej ostatniej Specyfikacja | Microsoft Azure" 
    description="Ten Specyfikacja opisano protokół i format MP4 pofragmentowane podstawie live spożyciu przesyłanie strumieniowe dla usługi multimediów Azure firmy Microsoft. Usługi multimediów Azure Microsoft dostępna live przesyłanie strumieniowe usługa, która umożliwia klientom strumienia wydarzeń na żywo i emitowanie zawartości w czasie rzeczywistym przy użyciu Microsoft Azure jako platformy chmury. W tym dokumencie omówiono również najważniejsze wskazówki w usłudze live budynku wysoce nadmiarowe i efektywne mechanizmy mogły zjeść tej ostatniej." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure usługi multimediów live pofragmentowane MP4 mogły zjeść tej ostatniej Specyfikacja

Ten Specyfikacja opisano protokół i format MP4 pofragmentowane podstawie live spożyciu przesyłanie strumieniowe dla usługi multimediów Azure firmy Microsoft. Usługi multimediów Azure Microsoft dostępna live przesyłanie strumieniowe usługa, która umożliwia klientom strumienia wydarzeń na żywo i emitowanie zawartości w czasie rzeczywistym przy użyciu Microsoft Azure jako platformy chmury. W tym dokumencie omówiono również najważniejsze wskazówki w usłudze live budynku wysoce nadmiarowe i efektywne mechanizmy mogły zjeść tej ostatniej.


##<a name="1-conformance-notation"></a>1. notacji zgodność

Słowa kluczowe "musi", "Nie", "Wymagane", "SHALL", "Nie jest", "SHOULD", "Nie powinien", "Zalecane", "Może" i "OPCJONALNIE", w tym dokumencie są interpretowane zgodnie z opisem w RFC 2119.

##<a name="2-service-diagram"></a>2. Diagram usługi 

Na poniższym diagramie przedstawiono architektury wysokiego poziomu przesyłanie strumieniowe usługi live w usługi multimediów Azure firmy Microsoft:

1.  Koder Live umieszcza live kanałów kanałami, które są tworzone i obsługi administracyjnej za pośrednictwem usługi multimediów Azure Microsoft SDK.
2.  Kanały, programów i Streaming wszystkie live przesyłanie strumieniowe funkcje ingest, formatowanie, w tym końcowym w uchwyt usługi multimediów Azure firmy Microsoft w chmurze DVR, zabezpieczenia i skalowalność nadmiarowości.
3.  Opcjonalnie klientów można wybrać wdrożenie warstwy CDN między punkt końcowy Streaming i punkty końcowe klienta.
4.  Przesyłaj strumieniowo punkty końcowe klienta z punkt końcowy Streaming za pomocą protokołów HTTP adaptacyjne Streaming (przykład Streaming gładkie, KRESKI, obr. / min lub HLS).

![obraz1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. Format strumienia bitowego — ISO 14496-12 pofragmentowane MP4

Formacie łańcuchowym na żywo streaming mogły zjeść tej ostatniej których został omówiony w tym dokumencie jest oparty na [ISO-14496 – 12]. Odwołują się do [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) , aby uzyskać szczegółowy opis format MP4 pofragmentowany i rozszerzenia dla obu plików usługi wideo na żądanie i live streaming spożyciu.

###<a name="live-ingest-format-definitions"></a>Usługi Live mogły zjeść tej ostatniej definicji formatów 

Poniżej przedstawiono listę specjalny format, który definicje, które dotyczą live mogły zjeść tej ostatniej do usługi multimediów Azure firmy Microsoft:

1. Pole "moov" "ftyp zawiera informacje", LiveServerManifestBox i muszą być wysyłane z każdego żądania (HTTP POST).  MUSI to być przesyłane na początku strumienia i w dowolnym momencie koder ponownie połączyć się Życiorys strumienia mogły zjeść tej ostatniej.  Zajrzyj do sekcji 6 [1] Aby uzyskać więcej informacji.
2. Sekcja 3.3.2 w [1] definiuje pola opcjonalne o nazwie StreamManifestBox na żywo mogły zjeść tej ostatniej. Ze względu na routingu logika równoważenia obciążenia Microsoft Azure zastosowania to pole jest przestarzałe i nie powinien być wtedy, gdy ingesting do usługi multimediów Azure firmy Microsoft. Jeśli to pole jest Prezentuj, usługi multimediów Azure cichym ignoruje ją.
3. Dla każdego fragmentu musi istnieć TrackFragmentExtendedHeaderBox zdefiniowane w 3.2.3.2 w [1].
4. Wersja 2 należy TrackFragmentExtendedHeaderBox można używać w celu wygenerowania segmenty przy użyciu adresu URL identyczne w wielu centrach danych. Pole index fragment jest wymagane dla centrum danych między awaryjnego przeniesienia na podstawie indeksu streaming formaty takich jak Apple HTTP Live Streaming (HLS) i MPEG-kreska na podstawie indeksu.  Aby możliwe było awaryjne przeniesienie centrum danych między, indeks fragment należy zsynchronizować w wielu kodery i zwiększenia przez 1 dla każdego fragmentu kolejnych multimediów nawet przez koder ponownego uruchomienia lub błędy.
5. Sekcja 3.3.6 w [1] definiuje pola o nazwie MovieFragmentRandomAccessBox (mfra), które mogą być przesyłane na końcu live spożyciu oznaczającą EOS (koniec strumienia) do kanału. Ze względu na logiki ingest usługi multimediów Azure zastosowania EOS (koniec strumienia) jest przestarzałe i pole "mfra" live spożyciu nie powinien być wysyłane. Jeśli zostały wysłane, usługi multimediów Azure cichym ignoruje ją. Jest zalecane w celu zresetowania stanu punktu ingest za pomocą [Resetowanie kanału](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) , a także zaleca się używanie [Zatrzymać Program](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) w celu zakończenia prezentacji i strumienia.
6. Czas trwania fragment MP4 powinien być stała, aby zmniejszyć rozmiar manifesty klientów oraz ulepszać heurystykę pobierania klienta przy użyciu znaczników powtarzania.  Czas trwania może zmieniają się w celu wyrównania szybkości odtwarzania nie jest liczbą całkowitą.
7. Czas trwania fragment MP4 powinien być między około 2 i 6 sekund.
8. Sygnatury czasowe fragment MP4 i indeksów (TrackFragmentExtendedHeaderBox fragment_ absolute_ czasu i fragment_index) trwa w kolejności rosnącej.  Mimo że usługi multimediów Azure jest mechanizm do zduplikowanych fragmentów, jest bardzo ograniczone możliwości, aby zmienić kolejność fragmenty zgodnie z osi czasu multimediów.

##<a name="4-protocol-format--http"></a>4. Format protocol — HTTP

ISO MP4 pofragmentowane na żywo mogły zjeść w tej ostatniej standard długotrwałe uruchamianie HTTP POST żądanie przesyłać dane multimediów zakodowany w formacie MP4 pofragmentowane usługi przy użyciu usługi multimediów Azure firmy Microsoft. Każdego wpisu HTTP wysyła pełną pofragmentowane MP4 strumienia bitowego ("strumienia") począwszy od począwszy od pola nagłówka (pole "ftyp zawiera informacje", "Live serwera pojawiają blok" i "moov") i kontynuowanie sekwencji fragmentów (pola "moof" i "mdat"). Zajrzyj do sekcji w 9.2 [1] składni URL żądania HTTP POST. Przykład adresu URL wpisu jest: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Wymagania dotyczące

Poniżej przedstawiono szczegółowe wymagania:

1. Koder powinny zacząć emisji przez wysłanie żądania HTTP POST z pustym "treści" (zerowej długości zawartości) przy użyciu tego samego adresu URL spożyciu. Może to ułatwić szybkie wykrycie Jeśli punkt końcowy spożyciu live jest prawidłowy, a przypadku uwierzytelniania lub innych warunków wymagane. Dla protokołu HTTP serwer będzie mógł wysłać odpowiedź HTTP wstecz do momentu otrzymania żądania całą treść wpisu w tym. Długotrwałych charakter zdarzenia na żywo, bez ten krok koder nie można wykryć jakikolwiek błąd, przed zakończeniem, wysyłając wszystkie dane.
2. Koder musi obsługiwać występują błędy lub wezwania uwierzytelniania wyniku (1). Jeśli (1) zakończyło się powodzeniem z odpowiedzią 200, Kontynuuj.
3. Koder musi rozpoczynać nowe żądanie HTTP POST pofragmentowane strumienia MP4.  Ładunek musi się zaczynać pola nagłówka, a po nim fragmenty.  Należy zauważyć, że pole "ftyp zawiera informacje", "Live serwera pojawiają blok" i "moov" (w kolejności) muszą być wysyłane przy każdym żądaniu nawet wtedy, gdy koder musi ponownie nawiązać połączenie, ponieważ poprzednie żądanie zostało przerwane przed koniec strumienia. 
4. Koder, należy użyć pakietowego Przełącz kodowanie dla przekazywania, ponieważ nie jest możliwe do przewidywania całej zawartości długości zdarzenia na żywo.
5. Po zdarzenia, po wysłaniu ostatniego fragmentu koder musi być bezpiecznie zakończona pakietowego Przełącz kodowanie sekwencja wiadomości (większość stosy klienta HTTP obsługiwać go automatycznie). Koder trzeba poczekać usługi zwracają kod ostatecznej odpowiedzi, a następnie Zakończ połączenie. 
6. Koder nie może zawierać rzeczownikowe Events() zgodnie z opisem w 9.2 w [1] na żywo spożyciu do usługi multimediów Azure firmy Microsoft.
7. Żądanie HTTP POST kończy lub przekroczenie limitu czasu przed końcem strumienia z błędem TCP, koder musi zażądać nowego wpisu przy użyciu nowego połączenia i postępuj zgodnie wymagania powyżej z dodatkowy wymóg, że koder musi ponowne wysłanie poprzedniego dwie części MP4 dla każdej ścieżki w strumieniu i wznowienie bez wprowadzania przerw na osi czasu multimediów.  Ponowne wysyłanie dwie ostatnie fragmenty MP4 dla każdej ścieżki gwarantuje, że jest bez utraty danych.  Innymi słowy jeśli zawiera strumieniu audio i wideo śledzenia, a nie bieżącego żądania WPIS, koder musi ponownie nawiązać połączenie i jej ponowne wysyłanie dwie ostatnie fragmenty ścieżki audio wcześniej wysłano, i dwie ostatnie fragmenty dla ścieżki wideo, które wcześniej wysłano, w celu zapewnienia bez utraty danych.  Koder należy zachować buforu "przekazywania" fragmentów multimediów, które wysyła go ponownie, gdy ponownie.

##<a name="5-timescale"></a>5. Skala czasu 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) opis zastosowania "Skali czasu" SmoothStreamingMedia (sekcja 2.2.2.1), StreamElement (sekcja 2.2.2.3), StreamFragmentElement(2.2.2.6) i LiveSMIL (sekcja 2.2.7.3.1). Jeśli nie ma wartości skali czasu, 10 000 000 (10 MHz) zostanie użyta wartość domyślna. Mimo że wygładzonymi specyfikacja formatu strumieniowego przesyłania nie zablokować zastosowania innych wartości skali czasu w większości zastosowań implementacji encoder tę wartość domyślną (10 MHz), aby wygenerować wygładzonymi Streaming mogły zjeść tej ostatniej danych. Ze względu na funkcji [Pakowanie dynamiczne multimediów Azure](media-services-dynamic-packaging-overview.md) jest zalecane, aby używać 90 kHz skali czasu dla strumieni wideo i 44,1 lub 48.1 kHz strumienie audio. Użycie wartości innej skali czasu dla różnych strumieni muszą być wysyłane strumienia poziomu skali czasu. Zajrzyj do [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. "Strumienia" definicji  

"Strumienia" to podstawowa jednostka operacji na żywo spożyciu tworzenia prezentacji na żywo, obsługa przesyłanie strumieniowe awaryjnego i scenariusze nadmiarowości. "Strumienia" jest zdefiniowany jako jeden unikatowy pofragmentowane MP4 strumienia bitowego zawierające pojedynczą ścieżkę lub wiele ścieżek. Pełne live prezentacji mogą zawierać jeden lub więcej strumieni w zależności od konfiguracji live encoder(s). W przykładach poniżej przedstawiono różne opcje używania stream(s) umożliwia tworzenie prezentacji na żywo pełnego.

**Przykład:** 

Klient chce Tworzenie prezentacji na żywo przesyłanie strumieniowe, która obejmuje następujące bitrates audio/wideo:

Klip wideo — 3000 KB/s, 1500 KB/s, 750 Kb/s

Audio — 128 Kb/s

###<a name="option-1-all-tracks-in-one-stream"></a>Opcja 1: Wszystkie ścieżki w jednym strumieniu

W przypadku tej opcji pojedynczy kodera generuje wszystkie ścieżki audio/wideo i połączyć je do jednego pofragmentowane MP4 strumienia bitowego która następnie przesłane za pośrednictwem jednego połączenia HTTP POST. W tym przykładzie jest tylko jeden strumień dla tej prezentacji na żywo:

![obraz2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Opcja 2: Każdej ścieżki w osobnym strumienia

W przypadku tej opcji encoder(s) trafiać tylko jedną śledzenie do strumienia bitowego każdego fragmentu MP4 i opublikować wszystkie strumienie połączeniach wielu oddzielnych HTTP. Można to zrobić za pomocą kodery jednego lub wielu kodery. Z punktu widzenia live spożyciu tę prezentację na żywo składa się z czterech strumieni.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Opcja 3: Pakietu ścieżki dźwiękowej o najmniejszej Ścieżka wideo szybkość transmisji bitów w jeden strumień

W przypadku tej opcji klienta wybiera pakietu ścieżki dźwiękowej ze ścieżką najniższe szybkość transmisji bitów wideo do jednego fragmentu MP4 strumienia bitowego i pozostaw innych dwoma torami wideo jest jej własnego strumienia. 


![image4][image4]


###<a name="summary"></a>Podsumowanie

Co to jest widoczny ponad nie jest pełna lista wszystkich opcji możliwe spożyciu w tym przykładzie. Jak rzeczy zgrupowania ścieżki do strumieni jest obsługiwana połknięcie live. Odbiorców i dostawców encoder można ich implementacji na podstawie złożoność konstrukcyjną, wydajność encoder i nadmiarowości i zagadnienia pracy awaryjnej. Jednak należy zauważyć, że w większości przypadków jest tylko jeden ścieżki dźwiękowej dla całej prezentacji na żywo jest ważne, aby zapewnić healthiness strumienia ingest, która zawiera ścieżki dźwiękowej. Ten uwagę często powoduje wprowadzanie ścieżki dźwiękowej do własnej strumienia (tak jak opcja 2) i łączenie go ze ścieżką najniższe szybkość transmisji bitów wideo (tak jak opcja 3). Również uzyskać lepszą nadmiarowości i odporność na uszkodzenia, wysyłanie śledzenie samego dźwięku w dwóch różnych strumieni (opcja 2 z zbędne ścieżki audio) lub grupowania, które ścieżki dźwiękowej co najmniej dwa najniższe ścieżki wideo szybkość transmisji bitów (opcja 3 z dźwiękiem połączone w co najmniej dwa strumienie wideo) zaleca się na żywo mogły zjeść tej ostatniej do usługi multimediów Azure firmy Microsoft.

##<a name="7-service-failover"></a>7. pracy awaryjnej usługi 

Podane charakter live streaming Obsługa dobre pracy awaryjnej jest niezbędne do zapewnienia dostępności usługi. Usługi multimediów Azure Microsoft przeznaczony do obsługi różnych typów błędów w tym błędów sieciowych, błędy serwera, problemów związanych z przechowywaniem itp. Gdy używana w połączeniu z logiki pisane z wielkiej litery pracy awaryjnej ze strony live encoder, klienta można uzyskać wysoce niezawodne usługi live przesyłanie strumieniowe z chmury.

W tej sekcji omówimy scenariuszach pracy awaryjnej usługi. W tym przypadku awarii się dzieje w dowolne miejsce w ramach usługi i zniekształcenie błąd sieciowy. Poniżej przedstawiono niektóre zalecenia dotyczące implementacji encoder obsługi pracy awaryjnej usługi:


1. Korzystanie z 10 drugiego limitu czasu do nawiązywania połączenia TCP.  Jeśli próby nawiązania połączenia trwa dłużej niż 10 sekund, przerwać operację i spróbuj ponownie. 
2. Używanie krótki limit czasu dla wysłanie żądania HTTP fragmentów wiadomości.  Jeśli czas trwania fragment docelowej MP4 N sekund, użyj Wyślij przekroczenia limitu czasu między N i 2N sekund; na przykład należy użyć przekroczenie limitu czasu sekund 6-12 czas trwania fragment MP4 6 sekund.  Jeśli to nastąpi, resetowanie połączenia, Otwórz nowe połączenie, a następnie wznowić strumienia mogły zjeść tej ostatniej na nowe połączenie. 
3. Obsługa stopniowe bufor zawierający dwie ostatnie fragmentów, dla każdego toru, wysłane pomyślnie i całkowicie z usługą.  Jeśli żądanie HTTP POST dla strumienia zakończeniem lub przekroczenie limitu czasu przed koniec strumienia, Otwórz nowe połączenie i rozpocząć kolejnego żądania HTTP POST, ponowne wysyłanie nagłówków strumienia, ponowne wysyłanie dwie ostatnie fragmenty dla każdej ścieżki i wznowienie strumienia bez wprowadzania przerwa na osi czasu multimediów.  Zmniejszy prawdopodobieństwo utraty danych.
4. Zaleca się, że koder nie ogranicza liczbę prób, aby nawiązać połączenie Życiorys przesyłanie strumieniowe po wystąpi błąd TCP.
5. Po błąd TCP:
    1. Bieżące połączenie muszą być zamknięte, a dla nowego żądania HTTP POST należy utworzyć nowe połączenie.
    2. Nowe HTTP WPIS adresu URL musi być taka sama jak początkowy adres URL wpisu.
    3. Nowe HTTP WPIS muszą zawierać identyczne nagłówków strumienia nagłówki strumienia (pole "ftyp zawiera informacje", "Live serwera pojawiają blok" i "moov") w pierwszym WPISEM.
    4. Ostatnie dwie części wysłane dla każdej ścieżki należy ponownie wysłać i streaming wznowieniu bez wprowadzania przerwa na osi czasu multimediów.  Sygnatury czasowe fragment MP4 muszą rosnąć, nawet przez żądania HTTP POST.
6. Koder powinien zakończyć żądanie HTTP POST, jeśli dane nie są przesyłane stawki dostosowany do czasu trwania fragment MP4.  Żądanie HTTP POST, które nie wysyła danych uniemożliwia usługi multimediów Azure szybko odłączanie koder w przypadku aktualizacji usługi.  Z tego powodu HTTP POST dla rzadkie ścieżki (ad sygnał) powinien być krótki żyła kończące zaraz po przesłaniu rzadkie fragmentu.

##<a name="8-encoder-failover"></a>8. pracy awaryjnej encoder

Koder pracy awaryjnej jest drugi typ pracy awaryjnej scenariusza, który należy zająć na żywo dostarczenie przesyłanie strumieniowe zakończenia do końca. W tym scenariuszu warunek błędu stało się na stronie kodera. 

![image5][image5]


Poniżej wymieniono oczekiwań od punktu końcowego spożyciu live w przypadku pracy awaryjnej kodera:

1. Aby kontynuować streaming, powinny być tworzone nowego wystąpienia kodera, jak pokazano na powyższym diagramie (strumienia wideo liniami kreskowanymi k 3000).
2. Nowy koder, należy użyć tego samego adresu URL żądania HTTP POST jako wystąpienie nie powiodło się.
3. Koder nowe żądanie POST muszą zawierać samego pola nagłówka pofragmentowane MP4 w jako wystąpienie nie powiodło się.
4. Nowy koder prawidłowo zsynchronizowane z wszystkich innych uruchomionego kodery dla tej samej prezentacji live do generowania zsynchronizowane próbki audio/wideo z wyrównanymi fragment ograniczenia.
5. Nowe strumienia musi być semantycznie równoważne z poprzedniego strumienia i wymiennym na poziomie nagłówka i fragmentu.
6. Nowy koder należy spróbować Minimalizowanie utraty danych.  Od punktu miejsce, w którym ostatnio zatrzymana koder zwiększyć fragment_absolute_time i fragment_index fragmentów multimediów.  Fragment_absolute_time i fragment_index należy zwiększyć w sposób ciągły, ale jest dozwolone wprowadzenie przerwa w razie potrzeby.  Usługi multimediów Azure zignoruje fragmentów, które zawiera już odebrane i przetworzona, więc warto błąd z boku wysłać fragmenty niż Aby wprowadzić przerw na osi czasu multimediów. 

##<a name="9-encoder-redundancy"></a>9. nadmiarowości encoder 

Dla niektórych krytyczne na żywo wydarzenia, że żądanie jeszcze większą dostępność i jakość działania i obsługi, zaleca się zatrudnić aktywny aktywny kodery zbędne uzyskanie Bezproblemowa praca awaryjna bez utraty danych.

![image6][image6]

Jak pokazano na ilustracji, istnieją dwie grupy kodery naciśnięcie dwie kopie każdego strumienia jednocześnie w usłudze live. Taka konfiguracja jest obsługiwana, ponieważ usługi multimediów Azure Microsoft ma możliwość odfiltrować zduplikowane fragmenty oparte na sygnatury czasowej identyfikator i fragment strumienia. Wynikowa strumień na żywo i archiwum będą pojedynczej kopii wszystkich strumieni, która jest zalecane agregacji możliwe z dwóch źródeł. Na przykład w teoretyczna przypadków, ile jest jednym encoder (nie musi być taki sam) działa na dowolnym etapie w czasie dla każdego strumienia, wynikowy strumieniem na żywo z usługi będzie ciągły bez utraty danych. 

Wymagania w tym scenariuszu jest prawie tak samo jak w przypadku pracy awaryjnej Encoder z wyjątkiem drugi zestaw kodery uruchomionych w tym samym czasie jako podstawowego kodery wymagania.

##<a name="10-service-redundancy"></a>10. nadmiarowości usługi  

Wysoce zbędne rozłożenie globalnej czasami konieczne jest kopia zapasowa region krzyżowe do obsługi regionalne awarii. Rozwijanie na topologii "Koder nadmiarowości", klientów można wybrać opcję wdrażania usługi zbędne w innym regionie, które jest połączone z zestawem 2 kodery. Klientów można również działają z dostawcę CDN wdrożenia GTM (globalnym Menedżer ruch) przed wdrożenia usługi dwóch aby bezproblemowo skierować ruch klienta. Wymagania dotyczące kodery to samo jak "Koder nadmiarowości" w przypadku jedynym wyjątkiem, które należy wskazać na inny żywo drugi zestaw kodery mogły zjeść tej ostatniej punkt końcowy. Na poniższym diagramie pokazano tej konfiguracji:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. specjalne typy formatów spożyciu 

W tej sekcji opisano niektóre specjalnego typu live spożyciu formaty, które są przeznaczone do obsługi niektórych określonych scenariuszach.

###<a name="sparse-track"></a>Śledzenie rzadkie

Podczas przedstawiania prezentacji przesyłanie strumieniowe na żywo z środowisko klienta wzbogaconego, należy często przesyłanych czas zsynchronizowany zdarzeń lub sygnały wewnątrzpasmowego danymi głównym multimediów. Przykład jest dynamiczne live wstawiania reklam. Ten rodzaj zdarzenia sygnalizacji różni się od streaming ze względu na jej charakter rzadkie zwykła audio/wideo. Innymi słowy sygnalizowania danych zwykle nie jest stosowane przez cały czas i interwał może być trudne do prognozowania. Pojęcia rzadkie śledzenia został opracowany z myślą mogły zjeść tej ostatniej i emitowanie sygnalizowania danych wewnątrzpasmowych.

Poniżej przedstawiono zalecany implementacji dla ingesting rzadkie śledzenia:

1. Tworzenie osobnych pofragmentowane MP4 strumienia bitowego zawierający tylko zaznaczonych rzadkie bez ścieżek audio/wideo.
2. W polu"Live serwer pojawiają" zgodnie z definicją w sekcji 6 w [1] należy użyć parametru "parentTrackName" Określ nazwę śledzenia nadrzędnej. Zajrzyj do sekcji 4.2.1.2.1.2 w [1], aby uzyskać więcej informacji.
3. W polu"Live serwera pojawiają" manifestOutput musi być ustawiona na wartość "true".
4. Rzadkie charakter sygnalizowania zdarzenia, zalecane jest:
    1. Na początku zdarzenia na żywo, encoder wysyła pola nagłówka początkowej usługi umożliwiające usługę, aby zarejestrować rzadkie śledzenia w manifeście klienta.
    2. Koder powinien zakończyć żądanie HTTP POST, gdy dane nie są przesyłane.  Długotrwałych POST protokołu HTTP, który nie wysyła danych można zapobiec usługi multimediów Azure szybko odłączanie koder w przypadku usługi aktualizacji lub server Uruchom ponownie komputer, zgodnie z serwera multimediów zostaną tymczasowo zablokowane w operacji socket do odbierania. 
    3. W czasie, gdy sygnalizowania danych nie jest dostępny koder SHOULD Zamknij HTTP POST żądanie.  Gdy żądanie POST jest aktywne, koder należy wysyłać dane 
    4. Podczas wysyłania rzadkie fragmenty, kodera można ustawić jawne nagłówka długość zawartości, jeśli jest dostępna.
    5. Podczas wysyłania rzadkie fragment z nowe połączenie, encoder powinny zacząć wysyłania z pola nagłówka następuje nowe fragmenty. Jest uchwyt wtedy, gdy pracy awaryjnej stało się między i nowe połączenie rzadkie ustanowione do nowego serwera, który nie otrzymała rzadkie śledzenia przed.
    6. Fragment rzadkie śledzenia zostaną udostępnione do klienta po odpowiadające im nadrzędne śledzenie fragment zawierającą równa lub większa wartość sygnatury czasowej będą dostępne dla klienta. Na przykład jeśli rzadkie fragment jest sygnatura czasowa t = 1000, jest planowane po klienta widzi sygnatura czasowa 1000 fragment wideo (przy założeniu Nazwa śledzenia nadrzędnej wideo) lub poza, można pobrać rzadkie fragment t = 1000. Pamiętaj, że rzeczywista sygnału dobrze można używać do innej lokalizacji na osi czasu prezentacji wyznaczonych z przeznaczeniem. W powyższym przykładzie, możliwe jest który fragment rzadkie t = 1000 ma ładunku XML, czyli do wstawiania Ad w pozycji, która jest kilka sekund później.
    7. Ładunek fragmentu rzadkie śledzenia może być w różnych formatach (XML lub tekstu lub format binarny, itd.) w zależności od różnych scenariuszach. 


###<a name="redundant-audio-track"></a>Nadmiarowe ścieżki dźwiękowej

Typowe HTTP adaptacyjne Streaming scenariusza (np. wygładzonymi Streaming lub ŁĄCZNIKA) jest zazwyczaj tylko jeden ścieżki dźwiękowej w całej prezentacji. W przeciwieństwie do ścieżki wideo, których wielu poziomów jakości dla klienta do wyboru z błędów ścieżki dźwiękowej może się pojedynczego punktu awarii, gdy spożyciu strumienia, który zawiera ścieżki dźwiękowej zostało przerwane. 

Aby rozwiązać ten problem, usługi multimediów Azure Microsoft obsługuje live spożyciu nadmiarowe ścieżki audio. Koncepcja to, czy samej ścieżki dźwiękowej można przesyłać wielokrotnie w różnych strumieni. Gdy usługa będzie tylko zarejestrować ścieżki dźwiękowej raz w manifeście klienta, będzie mógł Użyj zbędne ścieżki audio jako kopie zapasowe do pobierania fragmenty audio, jeśli podstawowy ścieżki dźwiękowej występują problemy. Aby można było mogły zjeść tej ostatniej zbędne ścieżki audio, koder musi:

1. Tworzenie samej ścieżki dźwiękowej w wielu Fragment MP4 bitowej strumieni. Nadmiarowe ścieżki audio musi być semantycznie równoważne z dokładnie samej fragment sygnatury czasowe i wymiennym na poziomie nagłówka i fragmentu.
2. Upewnij się, że "dźwięk" w Live serwera pojawiają (6 sekcji w [1]) być taka sama dla wszystkich zbędne ścieżki audio.

Oto zalecane wykonania na zbędne ścieżki audio:

1. Wyślij każdej unikatowe ścieżki dźwiękowej w strumieniu samodzielnie.  Też wysyłać zbędne strumienia dla każdego z tych strumieni ścieżki dźwiękowej, gdy 2 strumienia różni się od 1 tylko przez identyfikator w adresie URL wpisu HTTP: {Protokół} :// {adres serwera} / {path}/Streams({identifier}) punkt publikowania.
2. Użyj osobnych strumienie, aby wysłać dwa najniższe bitrates wideo. Każdy z tych strumieni powinien też zawierać kopię każdej unikatowych ścieżki dźwiękowej.  Na przykład jeśli obsługuje wiele języków, strumienie te powinny zawierać ścieżki audio dla każdego języka.
3. Wystąpienia osobny serwer (koder) umożliwia kodowanie i wysyłanie zbędne strumienie wymienionych w (1) i (2). 



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 