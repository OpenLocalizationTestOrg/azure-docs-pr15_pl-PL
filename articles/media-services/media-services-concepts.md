<properties 
    pageTitle="Pojęcia usługi multimediów Azure | Microsoft Azure" 
    description="Ten temat zawiera omówienie pojęć usługi multimediów Azure" 
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
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

#<a name="azure-media-services-concepts"></a>Pojęcia usługi multimediów Azure 

Ten temat zawiera omówienie najważniejszych pojęć usługi multimediów.

##<a id="assets"></a>Składniki majątku i miejsca do magazynowania

###<a name="assets"></a>Składniki majątku

Środka [trwałego](https://msdn.microsoft.com/library/azure/hh974277.aspx) zawiera cyfrowe pliki (w tym, audio, obrazy, miniatur zbiorów, ścieżek tekstowych pliki wideo i napisy) i metadanych dotyczących tych plików. Po cyfrowe pliki są przekazywane do środka trwałego, może można używać w usługi multimediów kodowanie i przesyłanie strumieniowe przepływy pracy.

Środka trwałego są mapowane do kontenera obiektów blob na koncie magazyn Azure i pliki w tym składniku aktywów są przechowywane jako obiektów blob w danym kontenerze.

Określanie, jaka zawartość multimediów do przekazywania i przechowywać w środka trwałego, następujące kwestie:

- Zasób powinien zawierać tylko jedną, unikatową wystąpienie zawartości multimedialnej. Na przykład pojedynczy Edytuj odcinek telewizyjnego, filmu lub ogłoszeń.
- Zasób nie powinna zawierać wiele wersji lub edycji pliku audiowizualnych. Czy próba przykładem niepoprawne użycie środka trwałego zawierać więcej niż jeden odcinek telewizji, ogłoszeń lub wielu kąt kamery z pojedynczego procesu produkcji wewnątrz środka trwałego. Przechowywanie wielu wersji lub edycji pliku audiowizualnych w środka trwałego może spowodować trudności przesyłania zadań kodowania, przesyłanie strumieniowe i zabezpieczanie dostarczania zawartości w dalszej części przepływu pracy.  

###<a name="asset-file"></a>Plik zawartości 
[AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) reprezentuje rzeczywisty pliku audio lub wideo, który jest przechowywany w kontenerze obiektów blob. Plik zawartości są zawsze skojarzone z środka trwałego i środka trwałego może zawierać jednego lub wielu plików. Zadanie Media Encoder usług nie powiedzie się, czy obiekt plik zawartości nie jest skojarzone z plikiem cyfrowego w kontenerze obiektów blob.

Wystąpienie **AssetFile** i plik multimedialny rzeczywiste są dwa różne obiekty. Wystąpienia AssetFile zawiera metadanych dotyczących plików multimedialnych, gdy plik multimedialny z zawartością rzeczywiste.

Nie należy próbować zmienić zawartość kontenerów obiektów blob, wygenerowane za pomocą usługi multimediów bez użycia interfejsów API usługi multimediów.

###<a name="asset-encryption-options"></a>Opcje szyfrowania zawartości

W zależności od typu zawartości, który chcesz przekazać, przechowywanie i dostarczanie usługi multimediów zapewnia różne opcje szyfrowania, których można wybierać.

**Brak** Szyfrowanie nie jest używana. Jest to wartość domyślna. Należy zauważyć, że podczas korzystania z tej opcji zawartości nie jest chroniony podczas przesyłania lub spoczywa w magazynie.

Jeśli planujesz do przeprowadzania MP4 za pomocą stopniowego pobierania, użyj tę opcję, aby przekazać zawartość.

**StorageEncrypted** — ta opcja służy do szyfrowania Wyczyść zawartość lokalnie, przy użyciu szyfrowania bitowego AES 256, a następnie przekazanie go do magazynu Azure miejsce, w którym jest przechowywany szyfrowane na pozostałych. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć wysokiej jakości plików multimedialnych wprowadzania danych przy użyciu silnego szyfrowania spoczynku na dysku. 

Dostarczenia zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczenia zasobu, aby usługi multimediów wie, jak ma być dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy (na przykład AES, PlayReady lub bez szyfrowania). 

**CommonEncryptionProtected** — tę opcję, jeśli chcesz zaszyfrować (lub przekazać już zaszyfrowane) używanie zawartości z typowych szyfrowania lub DRM PlayReady (na przykład wygładzonymi przesyłanie strumieniowe chronione za pomocą PlayReady DRM).

**EnvelopeEncryptionProtected** — tę opcję, jeśli chcesz chronić (lub przekazać już chroniony) Użyj protokołu HTTP Live Streaming (HLS) zaszyfrowany przy użyciu szyfrowania AES (Advanced Standard). Należy zauważyć, że jeśli przekazujesz HLS już zaszyfrowane z AES go musi zostały zaszyfrowane przez Menedżera Przekształcanie.

###<a name="access-policy"></a>Zasady dostępu 

[AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) określa uprawnienia (na przykład Odczyt, zapis i listy) i czasu trwania dostępu do środka trwałego. Obiekt AccessPolicy będzie zwykle przekazać do locator, który chcesz następnie można uzyskać dostęp do plików przechowywanych w środka trwałego.


###<a name="blob-container"></a>Kontener obiektów blob

Kontener obiektów blob zawiera grupę zestawu obiektów blob. Kontenery obiektów blob są używane w usługi multimediów jako punkt obramowanie kontrola dostępu i Locator udostępnione podpis programu Access (SA) z aktywów. Konta usługi Magazyn Azure mogą zawierać dowolną liczbę kontenerów obiektów blob. Kontener mogą zawierać dowolną liczbę obiektów blob.

>[AZURE.NOTE]Nie należy próbować zmienić zawartość kontenerów obiektów blob, wygenerowane za pomocą usługi multimediów bez użycia interfejsów API usługi multimediów.

###<a id="locators"></a>Locator

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s określić punkt wejścia, aby uzyskać dostęp do plików przechowywanych w środka trwałego. Zasady dostępu służy do definiowania uprawnień i czasu trwania, że klient ma dostęp do danych aktywów. Locator może mieć wiele-do-jednego relacji z zasady dostępu tak, aby różne Locator można udostępnić godziny rozpoczęcia różnych i typy połączeń różnych klientów aplikacją wszystkich same uprawnienia i ustawienia czasu trwania; Jednak ze względu na ograniczenie zasad udostępniania określonych przez usługi Azure magazyn, nie może mieć więcej niż pięciu unikatowych Locator skojarzone z danego składnika majątku w tym samym czasie. 

Usługi multimediów obsługuje dwa typy Locator: Locator OnDemandOrigin, służące do przesyłania strumieniowego (na przykład MPEG KRESKI, HLS lub gładkie Streaming) lub stopniowe pobieranie multimedia i Locator adresu URL skojarzeń zabezpieczeń umożliwia przekazywanie lub pobieranie to\from pliki multimedialne Azure miejsca do magazynowania. 

Należy zauważyć, że uprawnienia listy (AccessPermissions.List) nie może być używana podczas tworzenia locator OrDemandOrigin. 

###<a name="storage-account"></a>Konto miejsca do magazynowania

Dostęp do magazynowania Azure odbywa się przy użyciu konta miejsca do magazynowania. Konto usługi multimediów można skojarzyć z jednego lub wielu kont miejsca do magazynowania. Konta może zawierać dowolną liczbę kontenery, dopóki ich całkowity rozmiar jest w obszarze 500TB na koncie miejsca do magazynowania.  Usługi multimediów udostępnia narzędzia poziomu SDK umożliwia zarządzanie wieloma kontami miejsca do magazynowania i rozkład aktywów podczas przekazywania do tych kont na podstawie metryki lub przypadkowe rozkładu równoważenia obciążenia. Aby uzyskać więcej informacji zobacz Praca z [Nośnikami Azure](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##<a name="jobs-and-tasks"></a>Zadania i zadania

[Zadanie](https://msdn.microsoft.com/library/azure/hh974289.aspx) jest zazwyczaj używany do procesu (na przykład indeks lub kodowanie) jednej prezentacji audio/wideo. W przypadku przetwarzania wiele klipów wideo, utworzyć zadania dla każdego klip wideo, aby być kodowane.

Zadanie zawiera metadane dotyczące przetwarzania do wykonania. Każdego zadania zawiera co najmniej jeden s [zadania](https://msdn.microsoft.com/library/azure/hh974286.aspx), określające, czy zadanie przetwarzania Atomowej, wprowadzania składników majątku wyjściowy aktywów, procesor multimediów i jego skojarzony ustawień. Zadania w ramach danego zadania może być połączonych ze sobą, gdzie trwałego dane wyjściowe jednego zadania jest podawana wprowadzania zawartości do następnego zadania. W ten sposób jedno zadanie może zawierać wszystkie niezbędne do przedstawienia multimediów przetwarzania.

##<a id="encoding"></a>Kodowanie

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

Aby uzyskać informacji na temat obsługiwanych kodery zobacz [kodery](media-services-encode-asset.md).


##<a name="live-streaming"></a>Przesyłanie strumieniowe na żywo

W usługi multimediów Azure kanału reprezentuje procesu przetwarzania zawartości przesyłanie strumieniowe na żywo. Kanał otrzyma wprowadzania strumieni na żywo w jeden z dwóch sposobów:

- Live kodera lokalne wysyła RTMP szybkość transmisji bitów wielokrotne lub gładkie Streaming (pofragmentowane MP4) do kanału. Możesz użyć następujące kodery live, które wyjściowy szybkość transmisji bitów wielokrotne wygładzonymi Streaming: pierwiastkowej, firmy Envivio, Cisco. Następujące kodery live wyjściowy RTMP: transcoders programu Adobe Flash Live, Telestream Wirecast i Tricaster. Strumienie zasysanego przechodzą przez kanały bez dalszego przetwarzania. Żądanie, usługi multimediów zapewnia strumienia klientom.

- Strumień pojedynczy szybkość transmisji bitów (w jednym z następujących formatów: RTP (MPEG-TS)), RTMP lub gładkie Streaming (pofragmentowane MP4)) są wysyłane do kanału, który jest skonfigurowany do przeprowadzania live kodowania z usługi multimediów. Kanał następnie wykonuje live kodowanie przychodzące strumienia pojedynczy szybkość transmisji bitów ze strumieniem wideo wielokrotne-szybkość transmisji bitów (adaptacyjne). Żądanie, usługi multimediów zapewnia strumienia klientom.

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


Aby uzyskać więcej informacji zobacz:

- [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md)
- [Praca z kanałami, które otrzymane szybkość transmisji bitów wielokrotne strumień na żywo z lokalnego kodery](media-services-live-streaming-with-onprem-encoders.md)
- [Przydziały i ograniczenia](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Ochrona zawartości

###<a name="dynamic-encryption"></a>Dynamiczne szyfrowania

Usługi multimediów Azure umożliwia bezpieczne multimediów od momentu opuszczenia tego komputera za pomocą przechowywania, przetwarzania i dostarczania. Usługi multimediów pozwala na przedstawianie zawartości zaszyfrowanych dynamicznie za pomocą szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego) i szyfrowania typowych (CENC) przy użyciu PlayReady i/lub Widevine DRM. Usługi multimediów są także usługą dostarczania kluczy AES i PlayReady licencji na autoryzowanych klientów.

Obecnie możesz szyfrowanie następujących formatów strumieniowych: HLS, MPEG ŁĄCZNIKA i wygładzonymi Streaming. Nie można zaszyfrować obr. / min streaming format lub stopniowe do pobrania.

Usługi multimediów do szyfrowania środka trwałego, należy skojarzyć klucza szyfrowania (CommonEncryption lub EnvelopeEncryption) do zawartości, a także skonfigurować zasady autoryzacji klucza.

Jeśli chcesz przesyłać strumieniowo zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczania zasobu, aby określić sposób przeprowadzania do zawartości.

W przypadku zażądania strumienia przez odtwarzacz, usługi multimediów używa określonego klucza dynamicznie szyfrowanie treści przy użyciu kopert (z AES) lub szyfrowania typowych (z PlayReady lub Widevine). Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi klucza dostarczenia. Określanie, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasady autoryzacji, które określone dla klucza.


###<a name="token-restriction"></a>Ograniczenie tokenu

Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: otwieranie, token ograniczeń ani ograniczeń adresów IP. Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w JSON Token sieci Web (JWT) a formatem prosty tokeny sieci Web (SWT). Usługi multimediów nie umożliwia zabezpieczanie usług tokenu. Można tworzyć niestandardowe STS lub korzystać z usługi Microsoft Azure ACS do tokenów problem. Usługi STS musi być skonfigurowany do tworzenia tokenu zalogowani za pomocą określonego oświadczeniach problem i klucza, określone w konfiguracji token ograniczeń. Dostarczenie klucza usługi multimediów zwróci żądany klucz (lub licencji) do klienta, jeśli tokenu jest prawidłowe i roszczeń w dopasowanie tokenu tych skonfigurowane dla klucz (lub licencji).

Podczas konfigurowania token ograniczony zasad, musisz określić klucz podstawowy weryfikacji, wystawcy i parametrów odbiorców. Klucz podstawowy weryfikacji zawiera klucz token został podpisany przy, wystawcy jest usługę tokenu bezpiecznego problemy tokenu. Grupy odbiorców (nazywane zakresu) w tym artykule opisano przeznaczenie tokenu lub zasób tokenu zezwala na dostęp do. Dostarczenie klucza usługi multimediów sprawdza, czy te wartości tokenu zgodne z wartościami w szablonie.

Aby uzyskać więcej informacji zobacz następujące artykuły:

[Ochrona Przegląd zawartości](media-services-content-protection-overview.md)
[Ochrona z AES-128](media-services-protect-with-aes128.md)
[Ochrona z DRM](media-services-protect-with-drm.md)

##<a name="delivering"></a>Dostarczanie

###<a id="dynamic_packaging"></a>Dynamiczne opakowań

Podczas pracy z usługi multimediów, zaleca się kodowanie plików kodery do adaptacyjne szybkość transmisji bitów MP4 Ustaw, a następnie przekonwertowanie zestawu na wybranym formacie przy użyciu [Dynamiczne opakowań](media-services-dynamic-packaging-overview.md).


###<a name="streaming-endpoint"></a>Przesyłanie strumieniowe punktu końcowego

StreamingEndpoint reprezentuje przesyłanie strumieniowe usługa, która zapewnia zawartości bezpośrednio do aplikacji klienckiej odtwarzacza lub do zawartości dostawy sieci (CDN) do dalszej dystrybucji (usługi multimediów Azure teraz zapewnia integrację Azure CDN). Strumień ruchu wychodzącego z usługi StreamingEndpoint może być strumień na żywo lub klipy wideo na żądanie zawartości na koncie usługi multimediów. Ponadto można kontrolować możliwości usługi StreamingEndpoint obsługę rosnącym wymagania dotyczące przepustowości za pomocą dostosowania skali (nazywane także przesyłanie strumieniowe jednostki). Zalecane jest przydzielić skali jeden lub więcej aplikacji w środowisku produkcyjnym. Skali umożliwiają z pojemnością dedykowane wyjściowym, którego można kupić w przyrostach 200 MB/s i dodatkowe funkcje zawierający opakowań dynamiczne użycia.

Zalecane jest szyfrowania dynamiczne opakowań i\lub dynamiczne. Aby użyć tych funkcji, musisz mieć co najmniej jedną jednostkę przesyłanie strumieniowe dla punktu końcowego, z której ma zostać strumienia. Aby uzyskać więcej informacji zobacz [Skalowanie streaming jednostki](media-services-portal-manage-streaming-endpoints.md).

Domyślnie można mieć maksymalnie 2 streaming punkty końcowe na koncie usługi multimediów. Aby uzyskać wyższy limit, zobacz [przydziałów](media-services-quotas-and-limitations.md).

Dotyczy tylko są rozliczenie, gdy usługi StreamingEndpoint znajduje się w stan uruchomiony.

###<a name="asset-delivery-policy"></a>Zasady dostarczania zawartości

Jeden z kroków w przepływie pracy dostarczania zawartości usługi multimediów konfiguruje [zasady dostarczania trwałych ](https://msdn.microsoft.com/library/azure/dn799055.aspx), które chcesz być strumieniowo. Zasady dostarczania zawartości informuje usługi multimediów sposobu dla swojego trwałego dostarczanych: do których streaming protokołu należy do zasobów dynamicznie umieszczane (na przykład, kreska MPEG, HLS, gładkie Streaming lub wszystkie), czy chcesz zaszyfrować dynamicznie do zawartości i w jaki sposób (kopert lub szyfrowania typowych).

Jeśli masz zasób zaszyfrowanych miejsca do magazynowania, przed można przesłać strumieniowo do zawartości, przesyłanie strumieniowe serwer usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy. Na przykład aby zapewnić usługi trwałego zaszyfrowane kluczem szyfrowania szyfrowania AES (Advanced Standard), Ustaw typ zasad DynamicEnvelopeEncryption. Aby usunąć szyfrowanie miejsca do magazynowania i przesyłać strumieniowo zawartości w Wyczyść, Ustaw typ zasad NoDynamicEncryption.

###<a name="progressive-download"></a>Pobieranie stopniowe

Pobieranie stopniowe umożliwia rozpoczęcie odtwarzania multimediów, zanim pobraniu całego pliku. Tylko stopniowo można pobrać pliku MP4.

Zwróć uwagę, musisz Odszyfrowywanie zaszyfrowanych składników majątku w razie potrzeby do nich dostęp do pobierania progresywnego.

Aby umożliwić użytkownikom z adresami URL stopniowego pobierania, należy najpierw utworzyć locator OnDemandOrigin. Tworzenie wskaźniku, umożliwia ścieżki bazowej do elementu. Następnie należy dołączyć nazwę pliku MP4. Na przykład:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Przesyłanie strumieniowe adresy URL

Strumieniowego przesyłania zawartości do klientów. Aby umożliwić użytkownikom z streaming adresy URL, należy najpierw utworzyć locator OnDemandOrigin. Tworzenie wskaźniku, umożliwia ścieżki bazowej do zawartości, która zawiera zawartość, którą chcesz przesyłać strumieniowo. Jednak aby można było przesyłać strumieniowo tej zawartości należy zmodyfikować następującą ścieżkę dodatkowo. Do utworzenia pełnego adresu URL do pliku manifestu przesyłanie strumieniowe, możesz ZŁĄCZ.teksty wartość ścieżki locator i manifest (filename.ism) nazwę pliku. Następnie dołączyć /Manifest i odpowiedni format (w razie potrzeby) do ścieżki lokalizacji.

Możesz również przesyłać strumieniowo zawartości za pośrednictwem połączenia SSL. Aby to zrobić, upewnij się, że przesyłanie strumieniowe adresami URL początkowych HTTPS.

Zauważ, że możesz tylko przesyłać strumieniowo przez SSL Jeśli przesyłanie strumieniowe punktu końcowego, z której dostarczyć zawartość została utworzona po 10 września 2014 r. Jeśli przesyłanie strumieniowe adresami URL są oparte na przesyłanie strumieniowe punkty końcowe utworzone po 10 września, adres URL zawiera "streaming.mediaservices.windows.net" (nowy format). Przesyłanie strumieniowe adresy URL, które zawierają "origin.mediaservices.windows.net" (stary format) nie obsługują protokołu SSL. Jeśli adres URL jest w starym formacie i chcesz mieć możliwość strumieniowego przesyłania przez SSL, należy utworzyć nowy punkt końcowy przesyłanie strumieniowe. Za pomocą adresy utworzone w oparciu o nowy punkt końcowy przesyłanie strumieniowe przesyłanie strumieniowe zawartości przez SSL.

Poniższa lista zawiera opis różnych formatów strumieniowych oraz przykłady:

- Gładkie strumieniowego przesyłania

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG ŁĄCZNIKA

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=mpd-Time-CSF)



- Apple HTTP Live przesyłanie strumieniowe 4 (HLS)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl)



- Apple HTTP Live przesyłanie strumieniowe V3 (HLS)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3)

- Obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostępu)

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
