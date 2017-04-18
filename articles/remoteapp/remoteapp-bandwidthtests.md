<properties 
    pageTitle="Azure RemoteApp — testowanie do wykorzystania przepustowości sieci z kilka typowych scenariuszy | Microsoft Azure"
    description="Dowiedz się, jak typowe scenariusze zastosowania, które mogą ułatwić ustalanie potrzeb przepustowości sieci dla Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp — testowanie do wykorzystania przepustowości sieci z kilka typowych scenariuszy

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jak wspomniano w [RemoteApp Azure Szacowanie wykorzystania przepustowości sieci](remoteapp-bandwidth.md), najlepszym sposobem na ustalanie wpływ Azure RemoteApp do sieci jest uruchamianie niektórych zastosowania testów. Uruchom testy przez ustalony czas i zmierzyć przepustowości potrzebnej dla każdego scenariusza. Jeśli masz możliwości, można również zastosować miary pakietu sieci i utraty zakłócenia sieci zrozumienie wzorców sieci, które zostanie utworzony w określonym środowisku.

    
Podczas obliczania wykorzystania przepustowości, należy pamiętać, że zastosowania różne różnych użytkowników w firmie. Na przykład tekst czytników autorzy zazwyczaj zajmować przepustowości mniejszej niż użytkowników, które współpracują z wideo. Aby uzyskać najlepsze wyniki badaniu potrzeb użytkowników i Utwórz mix następujące scenariusze, który najlepiej odpowiada użytkowników w firmie. Pamiętaj, aby [przejrzeć czynniki, które wykorzystania przepustowości wpływ na obsługę użytkownika](remoteapp-bandwidthexperience.md) — która pomoże zidentyfikować idealny testów.

Najpierw przeczytaj o testów, wybierz swojego mix, a następnie uruchom je. Poniższa tabela umożliwia ułatwia śledzenie wydajności.

>[AZURE.NOTE] Jeśli nie sprawdzenie, czy własnej sieci lub nie masz czasu, aby to zrobić, zapoznaj się z naszą [przepustowości sieci podstawowe szacuje i zalecenia](remoteapp-bandwidthguidelines.md). Przebieg mogą być różne, jednak jeśli *można* uruchomić swoje własne testy, zaleca się.


## <a name="the-usage-tests"></a>Testy zastosowania
Tych testów Uruchom dla różnych ilości czasu i testowania różnych funkcji i funkcji, które zajmują przepustowości sieci. Pamiętaj, aby wybierz kombinacją test, że najlepiej odpowiada użytkowników indywidualnych.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Dyrektorem/skomplikowanych program PowerPoint — uruchom 900 1000 sekund

Użytkownik przedstawia między slajdami zachowaniem wysokiej wierności 45-50 przy użyciu programu Microsoft Office PowerPoint w trybie pełnoekranowym. Slajdy powinny zawierać obrazy, przejścia (z animacjami) i tła z gradientem kolorów, które są typowe dla swojej firmy. Użytkownik powinien przedstawienie co najmniej 20 sekund każdego slajdu.
    
W tym scenariuszu tworzy ruchem seryjnym przejścia slajdu do następnego slajdu w prezentacji.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Prosty program PowerPoint — uruchom sekund ~ 620

Użytkownik przedstawia prosty plik programu PowerPoint z około 30 slajdów za pomocą programu Microsoft Office PowerPoint w trybie pełnoekranowym. Slajdy są bardziej tekstu intensywną niż w scenariuszu dyrektorem/skomplikowanych programu PowerPoint i mają prostsze tła i obrazy (diagramy czarny). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - uruchamianie sekund ~ 250

Użytkownik przegląda sieci web przy użyciu programu Internet Explorer. Użytkownik przegląda i do tekstu, obrazów naturalne i niektórych diagramów schematów przewijania. Przechowywane na dysk lokalny serwer hosta sesji pulpitu zdalnego (hosta sesji pulpitu zdalnego) jako strony sieci web. Plik MHT. Użytkownik przewija Page Up, Page Down, w górę i w dół klawiszy, za pomocą różnych interwałów dla każdego klucza typu przewijania:
    
    - W dół - 250 naciśnięć klawiszy bardzo 500 ms
    - Page Up - 36 naciśnięć klawiszy co 1000 ms
    - W dół - 75 naciśnięć klawiszy co 100 ms
    - Page Down - 20 naciśnięć klawiszy co 500 ms
    - W górę - 120 naciśnięć klawiszy każdej 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>Dokument PDF — proste — Uruchom ~ 610 sekund
Użytkownik odczytuje i wyszukiwanie dokumentu PDF na różne sposoby, za pomocą programu Adobe Acrobat Reader. Dokument powinien się składać z tabel, prostych wykresów i wielu czcionki tekstu. Dokument jest długi stron 35-40. Użytkownik przewija na dwóch różnych stawek, zgodność z poprzednimi i przekazuje je dalej o rozmiarze cztery różne powiększenia (Dopasuj do strony, Dopasuj do szerokości, 100%, a drugi wybrane). Powiększanie gwarantuje, że tekst (czcionki) są renderowane za pomocą o różnych rozmiarach. Przewijanie jest Page Up, Page Down, w górę i w dół klawiszy, za pomocą różnych interwałów dla każdego przewiń w dół.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>Plik PDF dokumentu — mieszane - Uruchom ~ 320 sekund
Użytkownik odczytuje i wyszukiwanie dokumentu PDF na różne sposoby, za pomocą programu Adobe Acrobat Reader. Dokument zawiera obrazy wysokiej jakości (w tym zdjęcia), tabele, prostych wykresów i wielu czcionki tekstu. Użytkownik przewija na dwóch różnych stawek, zgodność z poprzednimi i przekazuje je dalej o rozmiarze cztery różne powiększenia (Dopasuj do strony, Dopasuj do szerokości, 100%, a drugi wybrane). Powiększanie gwarantuje, że tekst (czcionki) są renderowane za pomocą o różnych rozmiarach. Przewijanie jest Page Up, Page Down, w górę i w dół klawiszy, za pomocą różnych interwałów dla każdego przewiń w dół.

### <a name="flash-video-playback---run-for-180-seconds"></a>Odtwarzanie wideo Flash — Uruchom ~ 180 sekund
Gdy użytkownik przegląda wideo zakodowany Adobe Flash osadzony na stronie sieci web. Strony sieci web są przechowywane w lokalnym dysku twardym serwera hosta sesji pulpitu zdalnego. Klip wideo zostanie odtworzony w programie Internet Explorer przez osadzonym odtwarzaczem wtyczki.

W tym scenariuszu emuluje użytkowników przeglądających sformatowanego zawartości strony sieci web zawierającej multimediów. Większość danych powinien bo za pośrednictwem VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Wpisywanie zdalnego w programie Word — Uruchom ~ 1800 sekund
Użytkownik wpisze dokumentu sesji RDP. Naciśnięć klawiszy są wysyłane po stronie klienta za pośrednictwem sesji RDP do dokumentu w programie Microsoft Word działa w zdalnej sesji. Stopa pisania jest jeden znak w każdej 250 ms (całkowita liczba znaków 7050). 

Jest to jeden z najbardziej typowe scenariusze dla pracownika wiedzy. W tym scenariuszu testów czas reakcji wpisując w procesor nowoczesny pracy użytkownika. W tym scenariuszu jest wielkość liter na nawet niewielkie zmiany w wykorzystania przepustowości.

## <a name="tracking-the-test-results"></a>Śledzenie wyniki testu

Poniższa tabela umożliwia ocenianie scenariusze w środowisku. Podane poniżej dane tylko dla ilustracji — może być znacznie inne niż możesz obserwować. 

Dla uproszczenia przyjęto założenie, że wszystkie scenariusze jest sprawdzany przy użyciu taką samą rozdzielczość ekranu 1920 x 1080 pikseli i transportu TCP w sieci z czasem oczekiwania (opóźnienie) poniżej 200 ms i sieci funkcja znaku 120 ms + o 1%.

Informacje o tabeli:
- **Średnia możliwości** zawiera przepustowości sieci, w którym wydajność pracy użytkowników nie ma wpływu znacznie, z wyłączeniem nie rzadkie zakłócenia audio lub wideo. System jest można szybko odzyskać wykorzystując logiki dynamiczne. Próba szacuje przepustowości sieci na zagwarantowanie jakości środowiska użytkownika.
 - **Noticeable problemów (punkt podziału)** zawiera przepustowości sieci, w którym użytkownicy mogą wystąpić problemy znaczną w ich obsługi, a ich wydajność dotyczy to mierzone okresów. W tym momencie algorytmy RDP jest klienci i nie daje gwarancji jakości środowiska użytkownika ze względu na niewystarczające przepustowości.
 - **Zalecane** zawiera zalecane dla środowiska dobry lub doskonały przepustowość sieci. Jest zazwyczaj krok większa niż wartość w kolumnie **możliwości średnia** .
 - **Notatki** zawierają uwag i komentarzy.
 
| Test                  | Średnia obsługi | Problemy zauważalne (punkt podziału) | Zalecane przepustowości | Notatki                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Dyrektorem/skomplikowanych w programie PowerPoint | 10 MB/s             | 1 MB/s                           | > 10 MB/s, preferowane 100 MB/s    | 1 MB/s są tracone wiele animacji                                   |
| Prosta w programie PowerPoint            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | 256 KB/s slajdy ładowanie z zauważalne opóźnienia                   |
| Program Internet Explorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, preferowane 100 MB/s    | 1 MB/s klipów wideo w sieci web są rozmyty i pocięty, szybkiego przewijania występują problemy |
| Prosty plik PDF            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | 256 KB/s zajmie trochę czasu ładowania strony                       |
| PDF mieszanym             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | 256 KB/s strony trwa sporo czasu, aby załadować    |
| Odtwarzanie wideo Flash  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, preferowane 100 MB/s    | 1 MB/s klip wideo jest ziarnisty i nie są wyświetlane niektóre ramki           |
| Wpisywanie zdalnego w programie Word    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | 256 KB/s użytkownik może się okazać czasu między naciśnięć klawiszy             |

Aby ocenić przepustowości dla poszczególnych użytkowników, tworzenie kombinację powyższych scenariuszach i odpowiednia część przepustowości sieci wymagane. Wybierz wartość najwyższa wymagane przez usługi scenariuszy. Ponieważ użytkownicy prawie nigdy nie za pomocą samego układu, należy rozważyć, czy niektóre minimalna dla użytkowników, którzy jednocześnie pracować w tej samej sieci.
     
## <a name="learn-more"></a>Dowiedz się więcej
- [Szacowanie wykorzystania przepustowości sieci Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - jak przepustowość sieci i jakość środowiska pracy ze sobą?](remoteapp-bandwidthexperience.md)

- [Azure przepustowość sieci RemoteApp - ogólne wskazówki (Jeśli nie można przetestować własnych)](remoteapp-bandwidthguidelines.md)