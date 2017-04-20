# <a name="internet-of-things-security-architecture"></a>Architektura zabezpieczeń Internet rzeczy

Podczas projektowania systemu, jest ważne, aby zrozumieć potencjalnych zagrożeń dla tego systemu, a następnie dodać odpowiednie zabezpieczenia w związku z tym, jak system został zaprojektowany i zaprojektowane pod. Jest to szczególnie ważne do projektowania produktu od początku z myślą o bezpieczeństwie, ponieważ zrozumienie, jak osoba atakująca może być w stanie złamanie zabezpieczeń systemu, pomaga upewnić się, że odpowiednie czynniki ograniczające zagrożenie w miejscu od początku. 

## <a name="security-starts-with-a-threat-model"></a>Bezpieczeństwo zaczyna się od modelu zagrożenia
 
Firma Microsoft dawna używane modele zagrożenie dla jego produktów i udostępniła zagrożenia firmy modelowania procesów publicznie dostępne. Doświadczenie firmy wykaże, że modelowanie ma nieoczekiwany korzyści wykraczające poza bezpośrednim zrozumienia jakie zagrożenia są najbardziej dotyczące. Na przykład tworzy również avenue do otwartej dyskusji z innymi osobami spoza zespołu rozwoju, co może prowadzić do nowych pomysłów i ulepszenia w produkcie.
  
Celem modelowania zagrożenia jest zrozumieć, jak osoba atakująca może być w stanie złamanie zabezpieczeń systemu, a następnie upewnij się, że w miejsce odpowiednich czynników ograniczających zagrożenie. Zagrożenie sił modelowania projektu zespołu rozważenie czynników ograniczających zagrożenie, zgodnie z założeniami systemu, a nie kiedy system jest wdrożona. Fakt ten jest niezwykle ważne, ponieważ modernizacji poziom ochrony na wiele urządzeń, w tym polu jest niemożliwe, podatne i pozostawi klientów na ryzyko.

Wiele zespołów rozwoju nie doskonałą pracę Przechwytywanie wymagania funkcjonalne dla systemu, które korzyści klientom. Jednak określenie sposobów nie jest oczywiste, że ktoś może być niewłaściwe wykorzystanie systemu jest trudniejsze. Modelowania zagrożenia mogą pomóc zrozumieć, co może zrobić osoba atakująca zespoły deweloperów i dlaczego. Modelowania zagrożenia jest procesem strukturalnego, który tworzy decyzje projektowe dyskusji na temat zabezpieczeń w systemie, jak również zmiany w projekcie, że są dokonywane w drodze tego zabezpieczenia wpływu. Model zagrożenie jest po prostu dokumentu, dokumentacja ta stanowi również idealny sposób zapewnienia ciągłości wiedzy, retencji lekcji pokazaliśmy i nowej pomocy na pokładzie zespołu szybko. Wreszcie w wyniku modelowania zagrożenia jest umożliwienie należy wziąć pod uwagę inne aspekty bezpieczeństwa, takie jak jakie zobowiązań w zakresie bezpieczeństwa użytkownik zgadza się udostępnić klientom. Zobowiązania te w połączeniu z modelowania zagrożenia będzie informować i dysków, testowania rozwiązań Internetu rzeczy (IoT).
 

### <a name="when-to-threat-model"></a>Gdy zagrożenie modelu

[Modelowania zagrożenia](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) oferuje największe wartości, jeśli jest włączona w fazie projektowania. Podczas projektowania, możesz skorzystać z największą elastyczność, aby wprowadzić zmiany, aby wyeliminować zagrożenia. Wyeliminowanie zagrożenia zgodnie z projektem jest pożądany efekt. Jest znacznie prostsze niż dodawanie czynników ograniczających zagrożenie, ich testowania i zapewnienie są zawsze aktualne i ponadto takie zniesienie nie zawsze jest możliwe. Coraz trudniej eliminacji zagrożeń, jak produkt staje się bardziej dojrzały, a z kolei ostatecznie wymaga większego nakładu pracy i kompromisy o wiele trudniejsze niż na początku modelowania w rozwoju zagrożenia.

### <a name="what-to-threat-model"></a>Co do modelu zagrożenia

Należy wątku model rozwiązanie jako całości i również koncentrować się na następujących obszarach:

- Funkcje zabezpieczeń i prywatności
- Funkcje, których błędy są odpowiednie zabezpieczenia
- Funkcje, które sąsiadują granicę zaufania 

### <a name="who-threat-models"></a>Kto zagrożenie modele

Modelowania zagrożenia jest procesem, jak każdy inny.  Jest dobrym pomysłem jest traktować dokument modelu zagrożenie, jak i innych części roztworu i sprawdzić jego poprawność. Wiele zespołów rozwoju nie doskonałą pracę Przechwytywanie wymagania funkcjonalne dla systemu, które korzyści klientom. Jednak określenie sposobów nie jest oczywiste, że ktoś może być niewłaściwe wykorzystanie systemu jest trudniejsze. Modelowania zagrożenia mogą pomóc zrozumieć, co może zrobić osoba atakująca zespoły deweloperów i dlaczego.

### <a name="how-to-threat-model"></a>Jak modelu zagrożenia

Zagrożenia modelowania procesów składa się z czterech kroków; kroki są:

- Model aplikacji
- Wyliczanie zagrożeń
- Zmniejszanie zagrożeń
- Sprawdzanie poprawności czynników ograniczających zagrożenie

#### <a name="the-process-steps"></a>Etapy procesu

Trzy reguły o których należy pamiętać podczas tworzenia modelu zagrożenia:

1. Tworzenie diagramu spośród architektura referencyjna. 
2. Uruchom wszerz. Zawiera omówienie i zrozumieć system jako całość, przed zanurzenie.  Pomaga to zapewnić, że zostanie poszerzony w odpowiednich miejscach.
3. Napęd proces, nie pozwól, aby proces dysku. Jeśli znajdziesz problemu na etapie modelowania i chcesz ją poznać, Idź do niego!  Nie czuć się niewolniczo wykonaj następujące kroki.  

#### <a name="threats"></a>Zagrożenia

Są cztery podstawowe elementy modelu zagrożenia:

- Procesy (usługi sieci web, usługi Win32, * nix demonów itp. Należy zauważyć, że niektóre złożone jednostki (np. pole bram i czujniki) może być przetwarzane jako procesu, gdy techniczne drążenie w dół w tych dziedzinach nie jest możliwe.
- Magazyny danych (tam, gdzie dane są przechowywane, takich jak plik konfiguracyjny lub bazy danych)
- Przepływ danych (gdzie przenoszenia danych między innymi elementami aplikacji)
- Podmioty zewnętrzne (niczego, który współdziała z systemu, ale nie jest pod kontrolą aplikacji, przykłady obejmują użytkowników i satelitarną źródła danych)

Wszystkie elementy w diagram architektury podlegają różne zagrożenia; będziemy używać mnemonik KROKU. Przeczytaj [Zagrożenie modelowania ponownie, POBIEŻNE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) Aby dowiedzieć się więcej o elementy KROKU.

Poszczególne elementy diagramu aplikacji podlegają niektórych zagrożeń KROKU:

- Procesy podlegają KROKU
- Przepływy danych podlegają TID
- Magazyny danych podlegają TID, a czasami R, jeśli pliki dzienników są magazyny danych.
- Podmioty zewnętrzne podlegają SRD

## <a name="security-in-iot"></a>Zabezpieczenia w IoT

Podłączone urządzenia specjalne mają znaczna liczba potencjalnych obszarów powierzchni interakcji i wzorów interakcji, które uznaje się za stworzenie ram dla zabezpieczanie cyfrowego dostępu do tych urządzeń. Termin "digital access" jest używany w tym miejscu odróżnić od wszystkich operacji wykonywanych za pomocą urządzenia bezpośredniej interakcji świadczenia zabezpieczenia dostępu poprzez kontrolę dostępu fizycznego. Na przykład wprowadzenie urządzenia do pomieszczenia z zamkiem na drzwi. Podczas gdy fizycznego dostępu nie można zaprzeczyć, za pomocą sprzętu i oprogramowania, można środki aby zapobiec fizycznemu dostępowi z wiodących na zakłócenia systemu. 

Poszukiwanie wzorów interakcji, przyjrzymy "urządzenia sterowania" i "dane urządzenia" z tego samego poziomu uwagi. "Kontrola urządzeń" może być sklasyfikowany jako wszelkie informacje dostarczone z urządzeniem przez każdą ze stron w celu zmiany lub wpływające na jej zachowanie w stosunku do jego stanu lub stanu jego środowiska. "Dane urządzenia" można uznać za wszelkie informacje, które urządzenie emituje do każdej innej strony o jego stanie i zaobserwowane stan jego środowiska.
   
W celu zoptymalizowania najważniejsze wskazówki dotyczące zabezpieczeń, zaleca się że typowym IoT podzielone na kilka części/strefy jako część modelowania wykonywania zagrożenia. Strefy te są w pełni opisane w tej sekcji i obejmują:

-   Urządzenia,
-   Pole bramy
-   Chmury bramy, i
-   Usługi.

Strefy są szerokie sposób na segmenty rozwiązania; Każda strefa ma często swoje własne wymagania danych i uwierzytelniania i autoryzacji. Stref może również służyć do uszkodzenie izolacji i ograniczyć wpływ stref zaufania low na wyższej strefy zaufania.

Każda strefa jest oddzielona przez granicę zaufania, co jest oznaczany jako czerwona linia kropkowana w poniższym diagramie. Reprezentuje przejście danych/informacji z jednego źródła do innego. Okresie przejściowym dane/informacje mogłyby zostać usterka związana z fałszowaniem, sabotaż, odrzucenie, ujawnienie informacji, "odmowa usługi" oraz podniesienie poziomu uprawnień (krok).

![Strefy zabezpieczeń IoT](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Składniki przedstawione w ramach każdego również są poddawane KROKU, umożliwiając pełne 360 modelowania widoku roztworu zagrożenia. W poniższych sekcjach opracowanie na wszystkich składników i problemów w zakresie ochrony określone i rozwiązań, które powinny być wprowadzone na miejsce.

Sekcje, które następuje omówi standardowe składniki, zwykle w tych strefach.

### <a name="the-device-zone"></a>Strefa urządzenia

Środowisko pracy urządzenia jest natychmiastowe fizycznej przestrzeni wokół urządzenia, gdzie fizycznego dostępu i/lub "sieci lokalnej" peer-to-peer cyfrowy dostęp do urządzenia jest wykonalne. "Sieć lokalną" zakłada się sieci, która jest odrębna i izolowane od – ale potencjalnie zmostkowane z – publicznego Internetu oraz zawiera wszelkie krótkiego zasięgu bezprzewodowej technologię, która zezwala na komunikację peer-to-peer urządzeń. Tak *nie* obejmują żadnych technologii wirtualizacji sieci, tworząc złudzenie sieci lokalnej i również nie ma publicznego operatora sieci, które wymagają dowolnymi dwoma urządzeniami do komunikowania się za pośrednictwem sieci publicznej przestrzeni gdyby wprowadź relacji komunikacji peer-to-peer.

### <a name="the-field-gateway-zone"></a>Strefy bramy pól

Pole brama jest urządzenie/urządzenia lub oprogramowanie komputerowe niektóre serwer ogólnego przeznaczenia, które działa jako włącznik komunikacji, a potencjalnie, jako system sterowania urządzenia i urządzenia centrum przetwarzania danych. Strefa bramy pole obejmuje samej bramki pola i wszystkich urządzeń, które są dołączone do niego. Jak sama nazwa wskazuje, pole bram działać poza dedykowany przetwarzania danych, są zwykle powiązane lokalizacji, potencjalnie podlegają fizycznej ingerencji i będzie mieć ograniczoną operacyjne redundancji. Wszystkie, aby powiedzieć, że brama pole jest powszechnie coś jeden dotykać i sabotażu wiedząc co jest jego funkcji. 

Brama pola różni się od router ruchu jedynie tym, że posiada ona aktywną rolę w zarządzaniu dostępem i przepływu informacji, co oznacza, że jest to aplikacja skierowana encji i połączenie sieciowe lub sesję terminalu. Urządzenie NAT lub zapora, natomiast nie kwalifikują się jako bramy pole ponieważ nie są one wyraźny związek lub terminalach sesji, ale raczej połączenia trasy (lub blok) lub dokonane przez nich sesji. Brama pole ma dwa różne obszary powierzchni. Jeden twarze urządzeń, które są dołączone do niego i stanowi wewnątrz strefy i innych wszystkich stron zewnętrznych i krawędzi strefy.   

### <a name="the-cloud-gateway-zone"></a>Strefa bramy chmury

Brama chmura jest system, który umożliwia komunikacji zdalnej z i do urządzeń lub bram pola z kilku różnych witrynach w sieci publicznej przestrzeni, zwykle w kierunku chmurze kontroli i system analizy danych, Federacji takich systemów. W niektórych przypadkach bramy chmury może natychmiast ułatwienia dostępu do specjalnych urządzeń z terminali, takich jak tabletek lub telefony. W kontekście omawiane tutaj "chmury" ma odwoływać się do systemu przetwarzania danych, który nie jest powiązany z tym samym miejscu co podłączonych urządzeń lub bram pola. Również w strefie chmury, środki operacyjne zapobiec fizycznemu dostępowi ukierunkowane i niekoniecznie nie jest narażony na infrastrukturę "chmury".  

Bramy chmury potencjalnie mogą być mapowane do nakładki wirtualizacji sieci izolować bramy chmury i wszystkich podłączonych urządzeń lub bram pole od innego ruchu sieciowego. Chmura bramy jest ani system kontroli urządzenia ani przetwarzania lub składowania, dane urządzenia; te urządzenia interfejsu z bramy chmury. Strefa bramy chmury obejmuje bramy chmury wraz z wszystkich bram pola i urządzenia bezpośrednio lub pośrednio podłączone do niego. Krawędź strefy jest odrębne obszar powierzchni, gdzie wszystkie strony zewnętrznej komunikują się za pośrednictwem.

### <a name="the-services-zone"></a>Strefy usług

"Usługa" jest zdefiniowany dla tego kontekstu jako części oprogramowania lub moduł, który nawiązuje relacje z urządzenia za pośrednictwem bramy pola lub chmury dla zbierania i analizy danych, jak również do dowodzenia i kontroli.  Usługi są mediatorów. Działa na podstawie swojej tożsamości wobec innych podsystemów i bram, przechowują i analizować dane, autonomicznie wydawania poleceń do urządzeń na podstawie analizowania danych lub harmonogramy i ujawnienia informacji i kontroli możliwości dla autoryzowanych użytkowników końcowych.

### <a name="information-devices-vs-special-purpose-devices"></a>Urządzenia informacyjne a specjalnych urządzeń

PC, telefony i tabletki to przede wszystkim urządzenia interaktywne informacje. Telefony i tablety jawnie są zoptymalizowane wokół maksymalizacji żywotność baterii. Najlepiej wyłączyli częściowo podczas interakcji nie od razu z osobą lub gdy nie dostarcza usług, takich jak odtwarzanie muzyki lub prowadzenie ich właściciela w określonej lokalizacji. Z punktu widzenia systemów tych urządzeń technologii informacyjnej głównie działają jako serwery proxy w stosunku do ludzi. Są one "osób siłowniki" sugerowanie "osób czujniki" zbieranie danych wejściowych i akcje. 

Urządzenia specjalne, z czujników temperatury proste do linii produkcyjnych fabryki złożonych z tysiącami elementów wewnątrz nich, są różne. Urządzenia te są znacznie bardziej zakresu w celu i nawet, jeśli stanowią one niektóre interfejs użytkownika, w dużej mierze do powiązania z zakresu lub być zintegrowane z aktywów w świecie fizycznym. Zmierzyć i sprawozdanie w warunkach środowiskowych, włączyć zawory, kontrolować serw, brzmią alarmy, Przełącz światła i wykonywania wielu innych zadań. Do pracy pomagają, dla których urządzenie informacji jest zbyt ogólne, zbyt drogie, zbyt duży lub zbyt kruche. Konkretnych celów natychmiast decyduje o ich projektu technicznego jako dobrze dostępnego budżetu walutowej ich produkcji i operacji w przewidzianym okresie użytkowania. Połączenie tych dwóch czynników kluczowych ogranicza dostępne operacyjne compute budżetu energii, obszar fizyczny, a tym samym dostępna pamięć i funkcje zabezpieczeń.  

Jeśli coś "pójdzie nie tak" z urządzeniami automatycznego lub zdalnego kontrolowane, na przykład wad fizycznych lub logicznych sterowania wady do umyślnego próby uzyskania nieautoryzowanego dostępu i manipulowania nimi. Partie produkcji mogą zostać zniszczone, budynki mogą być zrabowane lub spalona i osób może być poszkodowana lub nawet die. Jest to oczywiście zupełnie inna klasa szkód niż ktoś przeczytania limit skradzione karty kredytowej. Poziom zabezpieczeń dla urządzeń, które należy przenosić rzeczy, a także do danych z czujników, które ostatecznie prowadzi do poleceń, które powodują rzeczy, które należy przenieść, musi być wyższa niż w handlu elektronicznego lub scenariusz usług bankowych. 


### <a name="device-control-and-device-data-interactions"></a>Sterowanie urządzeniem i interakcje dane urządzenia

Podłączone urządzenia specjalne mają znaczna liczba potencjalnych obszarów powierzchni interakcji i wzorów interakcji, które uznaje się za stworzenie ram dla zabezpieczanie cyfrowego dostępu do tych urządzeń. Termin "digital access" jest używany w tym miejscu odróżnić od wszystkich operacji wykonywanych za pomocą urządzenia bezpośredniej interakcji świadczenia zabezpieczenia dostępu poprzez kontrolę dostępu fizycznego. Na przykład wprowadzenie urządzenia do pomieszczenia z zamkiem na drzwi. Podczas gdy fizycznego dostępu nie można zaprzeczyć, za pomocą sprzętu i oprogramowania, można środki aby zapobiec fizycznemu dostępowi z wiodących na zakłócenia systemu. 
 
Poszukiwanie wzorów interakcji, przyjrzymy "urządzenia sterowania" i "dane urządzenia" z tego samego poziomu uwagi podczas modelowania zagrożenia. "Kontrola urządzeń" może być sklasyfikowany jako wszelkie informacje dostarczone z urządzeniem przez każdą ze stron w celu zmiany lub wpływające na jej zachowanie w stosunku do jego stanu lub stanu jego środowiska. "Dane urządzenia" można uznać za wszelkie informacje, które urządzenie emituje do każdej innej strony o jego stanie i zaobserwowane stan jego środowiska. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Architektura referencyjna Azure IoT modelowania zagrożenia

Firma Microsoft używa ramach opisaną powyżej, zagrożenie modelowania dla Azure IoT. W poniższej sekcji w związku z tym używamy konkretny przykład architektura referencyjna IoT Azure wykazać, jak myśleć o zagrożeniu modelowania dla IoT i jak rozwiązywać rozpoznanych zagrożeń. W naszym przypadku możemy zidentyfikować cztery główne obszary:

-   Urządzenia i źródła danych
-   Transport danych
-   Urządzenia i przetwarzanie zdarzeń i
-   Prezentacja

![Modelowania dla Azure IoT zagrożenia](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Na poniższym diagramie oferuje uproszczony widok architektury IoT firmy Microsoft przy użyciu modelu Diagram przepływu danych, który jest używany przez narzędzie modelowania zagrożenie firmy Microsoft:

![Modelowania dla IoT Azure za pomocą narzędzia do modelowania zagrożenie MS zagrożenia](media/iot-security-architecture/iot-security-architecture-fig3.png)

Należy pamiętać, że architektura oddziela możliwości urządzenia i bramy. Dzięki temu użytkownikowi na korzystanie z urządzeniami bramy, które są lepiej zabezpieczone: posiadają umiejętność komunikowania się z bramy chmury przy użyciu bezpiecznych protokołów, co zwykle wymaga większej przetwarzania, gdy urządzenie macierzyste - takich jak termostat - może zapewnić na własny. W strefie usług Azure zakładamy, że bramy chmury jest reprezentowany przez usługę Centrum IoT Azure.

### <a name="device-and-data-sourcesdata-transport"></a>Transport źródeł danych urządzeń i danych

W tej sekcji bada architekturę opisane powyżej przez obiektyw modelowania zagrożenia i daje zapoznać się z omówieniem sposobu zajmujemy z problemów związanych. Firma Microsoft będą skoncentrowane na podstawowe elementy modelu zagrożenia:

- Procesy (te pod kontrolą naszych, a elementy zewnętrzne)
- Komunikacja (zwane również przepływy danych)
- Magazyn (zwane również magazynów danych)

#### <a name="processes"></a>Procesy

W każdej z kategorii określonych w architekturze Azure IoT, staramy się zmniejszyć liczbę różnych zagrożeń na różnych etapach danych/informacji istnieje w: proces, komunikacji i pamięci masowej. Poniżej podajemy Przegląd najbardziej typowe dla kategorii "proces", a następnie zapoznać się z omówieniem jak te można najlepiej złagodzić: 

**Fałszowanie (S)**: osoba atakująca może wyodrębnić klucze kryptograficzne z urządzenia, albo na poziomie oprogramowania lub sprzętu, i następnie dostęp do systemu za pomocą innego urządzenia fizycznego lub wirtualnego z tożsamością materiału klucza urządzenia zostały podjęte z. Dobry rysunek jest piloty zdalnego sterowania, który może przekształcić dowolny Telewizor i które są prankster popularne narzędzia.

**Odmowa usługi (D)**: urządzenie może być renderowana zdolny do funkcjonowania lub komunikowania się poprzez ograniczanie częstotliwości radiowych lub Cięcie kabli. Na przykład aparat nadzoru, który miał jego zasilania lub połączenie sieciowe celowo odcinane nie zgłosi dane, w ogóle.

**Manipulowanie (T)**: osoba atakująca może częściowo lub całkowicie zastąpić oprogramowania działającego na urządzeniu, potencjalnie umożliwiając zamieniono oprogramowania do dźwigni prawdziwej tożsamości urządzenia, jeśli materiał klucza lub funkcji kryptograficznych holding kluczowych materiałów były dostępne do nielegalnego programu. Na przykład osoba atakująca może wykorzystać wydobytego materiału klucza do przechwycenia i pominąć dane z urządzenia na ścieżkę komunikacyjną i zastąpić go fałszywych danych, który jest uwierzytelniany przy użyciu skradzionego materiału klucza.

**Ujawnienie informacji, (I)**: Jeśli urządzenie jest uruchomione oprogramowanie manipulować, takie oprogramowanie manipulować potencjalnie może wyciekać danych dla osób niepowołanych. Na przykład osoba atakująca może wykorzystać wydobytego materiału klucza może sama wprowadzić do ścieżkę komunikacyjną między urządzenia i brama kontrolera lub pole lub bramy chmury do syfonu informacji.

**Podniesienie poziomu uprawnień (E)**: urządzenie, które wykonuje określoną funkcję może być zmuszony do czegoś innego. Na przykład zawór, który jest tak zaprogramowany, aby otworzyć połowie drogi może być oszukać otworzyć całą drogę.

| **Składnik** | **Zagrożenie** | **Ograniczenie zagrożenia**                                                                                                                                                | **Ryzyko**                                                                                                                                                                                                    | **Implementacja**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Urządzenia        | S          | Przypisywanie tożsamości do urządzenia i uwierzytelnieniu urządzenie                                                                                                | Zastępowanie urządzenie lub część urządzenia inne urządzenie. Jak wiemy, że mówimy do właściwego urządzenia?                                                                                           | Uwierzytelnianie urządzenia, za pomocą zabezpieczeń TLS (Transport Layer) lub protokołu IPSec. Infrastruktury powinien obsługiwać przy użyciu klucza wstępnego (PSK) na tych urządzeniach, które nie może obsłużyć kryptografią asymetryczną pełne. Dźwignia finansowa Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Na przykład zastosowanie do urządzenia odporne na manipulacje mechanizmów przez co bardzo trudne do niemożliwe, aby wyodrębnić klucze i innych materiałów kryptograficznych z urządzenia. | Ryzyko jest, jeśli ktoś jest manipulowanie nimi urządzenia (fizyczne zakłócenia). Jak mamy na pewno tego urządzenia nie została sfałszowana.                                                                                 | Najbardziej efektywne ograniczenie zagrożenia jest możliwości modułu TPM zaufanej platformy, która umożliwia przechowywanie kluczy w specjalne układy-chip, z którego nie można odczytać kluczy, ale należy używać tylko dla operacji kryptograficznych, użyj klawisza, które nigdy nie ujawniać klucza. Szyfrowanie pamięci urządzenia. Zarządzanie kluczami dla urządzenia. Podpisywanie kodu. |
|               | E          | Po kontroli dostępu urządzenia. Schemat autoryzacji.                                                                                                    | Jeśli urządzenie pozwala na poszczególne czynności do wykonania na podstawie poleceń z zewnętrznego źródła lub nawet złamanego czujników, pozwoli to atak do wykonywania operacji w inny sposób dostępny. | Posiadające schematu autoryzacji dla urządzenia                                                                                                                                                                                                                                                                                                             |
| Pole bramy | S          | Uwierzytelniania bramy pola do chmury bramy (certyfikat oparty, PSK, roszczenia oparte,...).                                                                           | Jeśli ktoś może sfałszować bramy pola, następnie może przedstawić się jako dowolnego urządzenia.                                                                                                                               | RSA/PSK, IPSe, TLS [RFC 4279](https://tools.ietf.org/html/rfc4279). Te same biologicznego składowania i atestacji urządzeń w ogóle – Najlepszy przypadek jest używać modułu TPM. 6LowPAN rozszerzenie protokołu IPSec do obsługi bezprzewodowej sieci czujnika (WSN).                                                                                                              |
|               | TRID       | Ochrona bramy pola przed manipulacją (TPM)?                                                                                                            | Fałszowanie atakami podstęp myślenia bramy chmury, które mówi do bramy pola może spowodować ujawnienie informacji i modyfikacją danych                                                             | Pamięć szyfrowania, TPM, uwierzytelniania.                                                                                                                                                                                                                                                                                                              |
|               | E          | Mechanizm kontroli dostępu dla pola bramy                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Oto kilka przykładów zagrożeń w tej kategorii:

Fałszowanie: Osoba atakująca może wyodrębnić klucze kryptograficzne z urządzenia, albo oprogramowanie lub sprzęt poziomie i następnie dostępu, który pochodzi z systemu z innego urządzenia fizycznego lub wirtualnego z tożsamością urządzenia materiału klucza.

**"Odmowa usługi"**: urządzenie może być renderowana zdolny do funkcjonowania lub komunikowania się poprzez ograniczanie częstotliwości radiowych lub Cięcie kabli. Na przykład aparat nadzoru, który miał jego zasilania lub połączenie sieciowe celowo odcinane nie zgłosi dane, w ogóle.

**Sabotaż**: osoba atakująca może częściowo lub całkowicie zastąpić oprogramowania działającego na urządzeniu, potencjalnie umożliwiając zamieniono oprogramowania do dźwigni prawdziwej tożsamości urządzenia, jeśli materiał klucza lub funkcji kryptograficznych holding kluczowych materiałów były dostępne do nielegalnego programu.

**Sabotaż**: zdjęcie takie korytarzu może mieć na celu monitoringu, które jest widoczny obraz widma widzialnego pusty hol. Czujnik dymu lub pożaru może informować o kogoś trzymającego jaśniejszy na jej podstawie. W obu przypadkach urządzenia mogą być technicznie całkowicie godne zaufania do systemu, ale to sprawozdanie manipulować informacji.

**Sabotaż**: osoba atakująca może wykorzystać wydobytego materiału klucza do przechwycenia i pominąć dane z urządzenia na ścieżkę komunikacyjną i zastąpić go fałszywych danych, który jest uwierzytelniany przy użyciu skradzionego materiału klucza.

**Sabotaż**: osoba atakująca może częściowo lub całkowicie zastąpić oprogramowania działającego na urządzeniu, potencjalnie umożliwiając zamieniono oprogramowania do dźwigni prawdziwej tożsamości urządzenia, jeśli materiał klucza lub funkcji kryptograficznych holding kluczowych materiałów były dostępne do nielegalnego programu.
   
**Ujawnienie informacji**: Jeśli urządzenie jest uruchomione oprogramowanie manipulować, takie oprogramowanie manipulować potencjalnie może wyciekać danych dla osób niepowołanych.

**Ujawnienie informacji**: osoba atakująca może wykorzystać wydobytego materiału klucza może sama wprowadzić do ścieżkę komunikacyjną między urządzenia i kontrolera lub pole bramy lub bramy chmury do syfonu informacji.

**"Odmowa usługi"**: urządzenie można wyłączone lub włączone w tryb, gdzie komunikacja jest niemożliwa, (który jest celowe w przypadku wielu komputerów przemysłowych).

**Sabotaż**: urządzenia mogą być skonfigurowane do pracy w stanie Nieznany do systemu kontroli (poza parametrów kalibracyjnych znanych), a zatem mogą udostępniać danych, która może być błędnie zinterpretowana

**Podniesienie uprawnień**: urządzenie, które wykonuje określoną funkcję może być zmuszony do czegoś innego. Na przykład zawór, który jest tak zaprogramowany, aby otworzyć połowie drogi może być oszukać otworzyć całą drogę.

**"Odmowa usługi"**: urządzenie może być przekształcony w Państwie, gdzie komunikacja jest niemożliwa.

**Sabotaż**: urządzenia mogą być skonfigurowane do pracy w stanie Nieznany do systemu kontroli (poza parametrów kalibracyjnych znanych), a zatem mogą udostępniać danych, która może być błędnie zinterpretowana.
 
**Usterka związana z fałszowaniem/sabotaż/odrzucenie**: Jeśli nie zabezpieczone (co zdarza się rzadko z konsumenckich pilotów) osoba atakująca może anonimowo manipulowania stan urządzenia. Dobry rysunek jest piloty zdalnego sterowania, który może przekształcić dowolny Telewizor i które są prankster popularne narzędzia.

#### <a name="communication"></a>Komunikacja

Zagrożenia wokół ścieżki komunikacji między urządzeniami, urządzeń i pole bram i bramy urządzenia i chmury. W poniższej tabeli ma pewne wskazówki wokół otwartych gniazd na urządzenie/VPN:

| **Składnik**               | **Zagrożenie** | **Ograniczenie zagrożenia**                                      | **Ryzyko**                                                                                                      | **Implementacja**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Centrum IoT urządzenia              | IDENTYFIKATOR TID        | (D) TLS (PSK/RSA) do szyfrowania ruchu             | Podsłuchiwanie lub zakłóceń komunikacji między urządzeniem a brama                             | Zabezpieczenia na poziomie protokołu. Przy użyciu protokołów niestandardowych musimy dowiedzieć się, jak chronić je. W większości przypadków komunikacja odbywa się z urządzenia do Centrum IoT (urządzenie inicjuje połączenie).                                                                                                                                                                 |
| Urządzenia urządzenie               | IDENTYFIKATOR TID        | (D) TLS (PSK/RSA) do szyfrowania ruchu.            | Czytanie danych podczas przesyłania między urządzeniami. Manipulowanie danymi. Przeciążenie urządzeń z nowych połączeń | Zabezpieczenia na poziomie protokołu (MQTT/AMQP/HTTP/CoAP. Przy użyciu protokołów niestandardowych musimy dowiedzieć się, jak chronić je. Ograniczenie dotyczące zagrożenia DoS jest równorzędne urządzenia za pośrednictwem bramy chmury lub pola i je tylko akt jako klientom w sieci. Zaglądanie może spowodować bezpośrednie połączenie między komputerami równorzędnymi po posiadające zostały wynegocjowane przez bramę |
| Urządzenie jednostki zewnętrznej      | IDENTYFIKATOR TID        | Silne parowanie jednostki zewnętrznej do urządzenia | Podsłuchiwanie połączenia z urządzeniem. Przeszkadzających komunikacji z urządzeniem                     | Bezpiecznie parowanie jednostki zewnętrznej do urządzenia NFC/Bluetooth LE. Kontrolowanie panelu operacyjnego urządzenia (fiz.)                                                                                                                                                                                                                                                  |
| Pole brama brama chmury | IDENTYFIKATOR TID        | TLS (PSK/RSA) do szyfrowania ruchu.               | Podsłuchiwanie lub zakłóceń komunikacji między urządzeniem a brama                             | Zabezpieczenia na poziomie protokołu (MQTT/AMQP/HTTP/CoAP). Przy użyciu protokołów niestandardowych musimy dowiedzieć się, jak chronić je.                                                                                                                                                                                                                                                       |
| Urządzenie bramy chmury        | IDENTYFIKATOR TID        | TLS (PSK/RSA) do szyfrowania ruchu.               | Podsłuchiwanie lub zakłóceń komunikacji między urządzeniem a brama                             | Zabezpieczenia na poziomie protokołu (MQTT/AMQP/HTTP/CoAP). Przy użyciu protokołów niestandardowych musimy dowiedzieć się, jak chronić je.                                                                                                                                                                                                                                                       |

Oto kilka przykładów zagrożeń w tej kategorii:

**"Odmowa usługi"**: ograniczonego urządzeń są ogólnie pod groźbą DoS gdy aktywnie nasłuchuje połączeń przychodzących lub niechciane datagramów w sieci, ponieważ osoba atakująca może otworzyć wiele połączeń równolegle i nie ich obsługi lub je obsłużyć bardzo powoli lub urządzenie może zostać zalane pomieszczenie niepożądany ruch. W obu przypadkach urządzenie można skutecznie przestać działać w sieci.

**Usterka związana z fałszowaniem, ujawnienie informacji**: ograniczone i specjalne urządzenia często mają jeden dla wszystkich zabezpieczeń udogodnienia, takie jak hasła lub PIN ochrony lub całkowicie zdać się na ufanie sieci, co oznacza będzie przyznania dostępu do informacji, gdy urządzenie jest w tej samej sieci i sieci często tylko jest chroniony za pomocą udostępnionego klucza. Oznacza, że jeżeli zostaną ujawnione wspólnego hasła do urządzenia lub sieci, jest możliwość sterowania urządzeniem lub obserwować dane emitowanych przez urządzenie.  

**Usterka związana z fałszowaniem**: osoba atakująca może przechwycić lub częściowo zastąpić emisji i sfałszowanie inicjatorem (człowiek w środku)

**Sabotaż**: osoba atakująca może przechwycić lub częściowo zastąpić emisji i wysyłać fałszywe informacje 

**Ujawnienie informacji:** osoba atakująca może podsłuchiwać emisji oraz uzyskiwanie informacji bez zgody **"odmowa usługi":** osoba atakująca może jam nadawanego sygnału i Odmów dystrybucji informacji

#### <a name="storage"></a>Magazyn

Każdy bramy urządzenia i pole ma jakąś formę magazynu (tymczasowe dla usługi kolejkowania wiadomości danych, przechowywania obrazów systemu operacyjnego).

| **Składnik**                            | **Zagrożenie** | **Ograniczenie zagrożenia**                       | **Ryzyko**                                                                                                                                                                                                                                                                                                                | **Implementacja**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Urządzenia pamięci masowej                           | TRID       | Szyfrowanie magazynu, podpisywanie dzienniki | Odczytywanie danych z magazynu (dane osobowe), manipulowanie danymi telemetrycznego. Manipulowanie w kolejce lub polecenia sterowania dane w pamięci podręcznej. Wprowadzanie zmian w konfiguracji lub oprogramowania układowego pakiety aktualizacji podczas buforowane lub lokalnie w kolejce może prowadzić do składniki systemu operacyjnego i/lub system zagrożona                                         | Szyfrowanie, kod uwierzytelniania wiadomości (MAC) lub podpis cyfrowy. Jeżeli kontrola dostępu możliwe, silne poprzez dostęp do zasobów kontroli list (kontroli dostępu ACL) lub uprawnienia. |
| System operacyjny urządzenia obrazu                          | TRID       |                                      | Manipulowanie OS / wymiana składników systemu operacyjnego                                                                                                                                                                                                                                                                         | Partycja systemu operacyjnego tylko do odczytu, podpisany obrazu systemu operacyjnego, szyfrowanie                                                                                                                    |
| Pamięć masowa bramy pola (dane usługi kolejkowania wiadomości) | TRID       | Szyfrowanie magazynu, podpisywanie dzienniki | Odczytywanie danych z magazynu (dane osobowe), manipulowanie danymi telemetrycznego ingerencji w kolejce lub polecenia sterowania dane w pamięci podręcznej. Wprowadzanie zmian w konfiguracji lub oprogramowania układowego pakiety aktualizacji (przeznaczone dla urządzeń lub pole bramy) podczas buforowane lub lokalnie w kolejce może prowadzić do składniki systemu operacyjnego i/lub system zagrożona | Funkcja BitLocker                                                                                                                                                              |
| Obraz systemu operacyjnego pole bramy                   | TRID       |                                      | Manipulowanie OS / wymiana składników systemu operacyjnego                                                                                                                                                                                                                                                                          | Partycja systemu operacyjnego tylko do odczytu, podpisany obrazu systemu operacyjnego, szyfrowanie                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Urządzenia i zdarzenia przetwarzania/chmury strefy bramy

Brama chmura jest system, który umożliwia komunikacji zdalnej z i do urządzeń lub bram pola z kilku różnych witrynach w sieci publicznej przestrzeni, zwykle w kierunku chmurze kontroli i system analizy danych, Federacji takich systemów. W niektórych przypadkach bramy chmury może natychmiast ułatwienia dostępu do specjalnych urządzeń z terminali, takich jak tabletek lub telefony. W kontekście omawiane tutaj "chmury" jest przeznaczona do odwoływania się do systemu przetwarzania danych, który nie jest powiązany z tym samym miejscu co podłączonych urządzeń lub bram pola, i gdzie środki operacyjne zapobiec nacelowany fizycznego dostępu, ale nie jest koniecznie do infrastruktury "chmury".  Bramy chmury potencjalnie mogą być mapowane do nakładki wirtualizacji sieci izolować bramy chmury i wszystkich podłączonych urządzeń lub bram pole od innego ruchu sieciowego. Chmura bramy jest ani system kontroli urządzenia ani przetwarzania lub składowania, dane urządzenia; te urządzenia interfejsu z bramy chmury. Strefa bramy chmury obejmuje bramy chmury wraz z wszystkich bram pola i urządzenia bezpośrednio lub pośrednio podłączone do niego.

Bramy chmury jest głównie niestandardowe wbudowane oprogramowanie uruchomione jako usługa z narażonych punktów końcowych, do których pole bramy lub urządzenia łączą. Jako takie muszą być zaprojektowane z myślą o bezpieczeństwie. Wykonaj proces [SDL](http://www.microsoft.com/sdl) do projektowania i budowy tej usługi. 

#### <a name="services-zone"></a>Strefy usług

System kontroli (lub kontroler) jest rozwiązanie programowe interfejsy z urządzeniem, lub pole bramy lub bramy chmury potrzeby kontrolowania jednego lub wielu urządzeń i/lub do zbierania i/lub przechowywania i/lub analizować dane urządzenie dla prezentacji lub celów kolejnych kontroli. Systemy kontroli są tylko podmioty w zakres tej dyskusji, która od razu może ułatwić interakcję z użytkownikami. Wyjątkiem są powierzchnie pośrednich fizycznej kontroli na urządzeniach, takich jak przełącznik, który pozwala osobie, aby wyłączyć urządzenie lub zmienić inne właściwości i dla której nie ma odpowiednika funkcjonalności, które mogą być udostępniane cyfrowo. 

Powierzchnie pośrednich kontroli fizycznej są te, gdzie wszelkiego rodzaju regulujące logiki ogranicza funkcję powierzchnię fizyczną kontrolę, tak, aby równoważne funkcja może zostać zainicjowane zdalnie lub wejściowy jest w konflikcie z zdalnego sterowania można uniknąć – takie przechowywanych przez powierzchnie kontroli pod względem koncepcyjnym są dołączone do systemu miejscowego sterowania, który wykorzystuje tę samą funkcję podstawowej co innego systemu zdalnego sterowania, które urządzenia mogą być dołączone do równolegle. Największe zagrożenia dla cloud computing można przeczytać na stronie [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) .


## <a name="additional-resources"></a>Dodatkowe zasoby

Należy zapoznać się z następującymi artykułami, aby uzyskać dodatkowe informacje:

- [Narzędzie modelowania zagrożenie SDL](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Architektura referencyjna Microsoft Azure IoT](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
