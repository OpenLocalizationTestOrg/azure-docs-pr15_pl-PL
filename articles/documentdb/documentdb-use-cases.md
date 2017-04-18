<properties 
    pageTitle="DocumentDB typowe przypadki użycia | Microsoft Azure" 
    description="Dowiedz się więcej o góry pięć Używanie przypadków dla DocumentDB: zawartości, rejestrowanie zdarzeń, wykazu danych, dane preferencji użytkownika i Internet czynności (IoT) utworzona przez użytkownika." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Typowe przypadki użycia DocumentDB
W tym artykule omówiono kilka typowych przypadków użycia dla DocumentDB.  Zalecenia w tym artykule służyć jako punktu startowego podczas opracowywania aplikacji przy użyciu DocumentDB.   

Po zapoznaniu się w tym artykule, będziesz mieć możliwość odpowiedzieć na następujące pytania: 
 
- Jakie są typowe przypadki użycia dla DocumentDB?
- Jakie są zalety korzystania DocumentDB jako użytkownik wygenerowane magazynu zawartości?
- Jakie są zalety korzystania DocumentDB jako źródła danych wykazu przechowywać?
- Jakie są zalety używania DocumentDB jako dziennika zdarzeń są przechowywane?
- Jakie są zalety korzystania DocumentDB jako źródła danych preferencje użytkownika są przechowywane?
- Jakie są zalety korzystania DocumentDB przechowywania danych dla systemów Internet czynności (IoT)?

## <a name="common-use-cases-for-documentdb"></a>Typowe przypadki użycia dla DocumentDB
Azure DocumentDB jest to uniwersalny NoSQL baza danych jest używana w szeroką gamę aplikacji i przypadków użycia. Jest dobrym rozwiązaniem dla dowolnej aplikacji, która wymaga czasy niskim odpowiedzi kolejność milisekund i należy szybko skalowanie. Poniżej przedstawiono niektóre atrybuty DocumentDB, które ułatwiają nadają się do aplikacji wysokiej wydajności.

- DocumentDB oryginalnie partycje danych dla wysoką dostępność i skalowalność.
- Osoby DocumentDB ma SSD kopii miejsca do magazynowania z czasem odpowiedzi kolejność milisekund małym opóźnieniem.
- Pomocy technicznej firmy DocumentDB dla poziomów spójności, takich jak ewentualne, sesji i staleness ograniczone umożliwia niski koszt do wydajności współczynnika. 
- DocumentDB ma elastyczne modelu cennik przyjazne dla danych, który gazomierzy miejsca do magazynowania i przepustowość niezależnie.
- Osoby DocumentDB przepustowość zastrzeżone modelu umożliwia Pomyśl pod względem odczytuje i zapisuje zamiast Procesora i pamięci i operacji i/o na sekundę źródłowych sprzętu.
- Umożliwia projektowanie i DocumentDB Skaluj do żądania duże ilości kolejności miliardy żądań dziennie.

Te atrybuty są szczególnie przydatne, gdy nadejdzie w sieci web urządzeń przenośnych, gier i IoT aplikacje, które muszą czasu odpowiedzi niskim i konieczności obsługi dużych ilości Odczyt i zapis. 

## <a name="user-generated-content"></a>Użytkownik wygenerowane zawartości
Typowe przypadków użycia dla DocumentDB jest do przechowywania i utworzona przez użytkownika kwerendy zawartości (UGC) dla sieci web i aplikacji dla urządzeń przenośnych, szczególnie społecznościowych aplikacji.  Przykłady UGC są sesji rozmowy, tweety wpisów w blogu, ocen i komentarze.  Często UGC w aplikacjach społecznościowych jest kombinacją tekst dowolnej postaci, właściwości, znaczników i relacji, które nie są ograniczone przez sztywne struktury.   

Zawartości, takiej jak rozmowy, komentarze i wpisy mogą być przechowywane w DocumentDB bez konieczności przekształcenia lub złożonych obiektu w celu mapowania relacyjnego warstwy.  Właściwości danych można dodać lub modyfikowane łatwo wymagania jak deweloperów iteracyjne kod aplikacji, a więc promowanie szybkiego rozwoju.  

Aplikacje, które integrację z sieciami społecznościowymi różnych musi odpowiadać na Zmiana schematów z tych sieci.  Jak danych jest automatycznie indeksowane domyślnie w DocumentDB, dane są gotowe w dowolnym momencie.  W związku z tym te aplikacje mieć możliwość pobierania prognozy zgodnie z potrzebami odpowiednich.       

Wiele aplikacji społecznościowych w skali globalnej i może powodować wystąpienie nieprzewidywalny upodobania.  Elastyczność skalowania magazynu danych jest jako warstwy aplikacji skale zgodnie z zastosowania żądanie.  Dodając partycje dodatkowych danych przy użyciu konta DocumentDB można skalować się.  Ponadto można także tworzyć kolejne konta DocumentDB u wielu regionów. Dostępność DocumentDB usługi region zobacz [Obszary Azure](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Wykaz danych
Scenariusze użycia danych wykazu obejmować przechowywania i wykonywanie zapytań za zestaw atrybutów dla obiektów, takich jak osoby, miejsca i produktów.  Kilka przykładów wykazu danych są konta użytkowników, katalogi produktów, rejestry urządzenia IoT i rachunku systemów materiałów.  Atrybuty tych danych może być różna i mogą się zmieniać w czasie do wymagań aplikacji.  

Należy rozważyć przykład katalogiem produktów dla dostawcy samochodowych. Każdej części może mieć własny atrybuty jak typowe atrybuty, które współużytkują wszystkie elementy.  Ponadto atrybuty dla określonej części można zmienić rok po wydaniu nowego modelu.  Przechowywania dokumentów JSON DocumentDB obsługuje elastyczne schematów i pozwala do przedstawiania danych przy użyciu zagnieżdżonych właściwości, a więc jest odpowiedni do przechowywania danych wykazu produktów.       

## <a name="logging-and-time-series-data"></a>Rejestrowanie i danych szeregu czasowego
Rejestrowanie aplikacji często są wysyłane w dużych zbiorach i mogą mieć różne atrybuty oparte na wersję aplikacji lub składnika rejestrowania zdarzeń.  Dane dziennika nie jest ograniczone przez złożone relacje lub struktur sztywne. Rosnącej złożoności danych dziennika jest umieszczany w formacie JSON, ponieważ JSON jest proste i łatwe ludzi do odczytu.
   
Zazwyczaj są dwie przypadków użycia głównych związane z danymi dziennika zdarzeń.  Pierwszy przypadek użycia ma wykonywać kwerend ad hoc podzestawu danych dotyczących rozwiązywania problemów.  Podczas rozwiązywania problemów, podzestawu danych jest najpierw pobierana z dzienniki, zwykle przez szeregu czasowego.  Następnie rozwijania szczegółów jest wykonywane przez filtrowanie zestawu danych w oknie poziomy błędu lub komunikat o błędzie. Jest to miejsce przechowywania dzienniki zdarzeń w DocumentDB jest korzyści. Dane przechowywane w DocumentDB tabeli jest automatycznie indeksowane domyślnie, a więc po wykonaniu tych czynności można wykonać kwerendy w dowolnym momencie. Ponadto danych dziennika można trwała przez partycje danych jako szeregu czasowego. Dzienniki starszych mogą być wdrażania zimnej miejsce w magazynie na zasad przechowywania.          

Druga przypadków użycia polega na czas wykonywania zadań analizy danych wykonywane w trybie offline w dużej liczby dziennik danych.  Przykładami tym przypadku analizy dostępność serwera, analizy błędów aplikacji i clickstream analizy danych.  Zazwyczaj Hadoop służy do wykonywania następujących typów analiz.  Dzięki łącznika Hadoop dla DocumentDB DocumentDB baz danych działać jako źródła danych i pochłaniacze świnka, gałęzi i Mapa/zmniejszanie zadań. Aby uzyskać szczegółowe informacje na łącznik Hadoop dla DocumentDB zobacz [uruchomić zadanie Hadoop z DocumentDB i HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Gier
Warstwa bazy danych jest kluczowym składnikiem gier aplikacji. Nowoczesne gry przetwarzania graficznego w konsoli/mobile klientach, ale oparte na chmurze do przeprowadzania niestandardowych i spersonalizowanej zawartości, takiej jak statystykę w nożna, integracją z serwisami społecznościowymi i tablice wynik wysoki wyników. Gry wymagają bardzo niskie opóźnienia dla odczytuje i zapisy o podanie atrakcyjnych w gier obsługi i warstwy bazy danych musi obsługiwać sukcesów i porażek uczniów w żądań podczas nowej, konsole uruchomiony i aktualizacji funkcji.

DocumentDB jest używany przez gry ogromną skalę, takich jak [Analizowanie braku: strukturą mężczyzna bez](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) przez [Następnej gry](http://www.nextgames.com/)i [Halo 5: opiekunach](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). W obu przypadkach za pomocą, głównych zalet DocumentDB były następujące:

- DocumentDB umożliwia wydajności skalowania w górę lub w dół elastically. Dzięki temu gry obsługę aktualizowanie profilu oraz statystyki prac spośród wielu miliony jednoczesnego gracze dokonując pojedyncze wywołanie interfejsu API.
- DocumentDB obsługuje milisekund odczytuje i zapisuje w celu uniknięcia dowolnego odchyłki podczas gry.
- Automatyczne indeksowanie w DocumentDB umożliwia filtrowanie dla wielu różnych właściwości w czasie rzeczywistym, np. Znajdź odtwarzacze według ich odtwarzacza wewnętrznych identyfikatorów, lub ich GameCenter, Facebook, identyfikatory Google lub kwerendy na podstawie odtwarzacza członkostwa w podręcznik. Jest to możliwe bez tworzenia złożonych indeksowania lub infrastruktury sharding.
- Funkcje społecznościowe, w tym wiadomości w nożna, członkostwa Podręcznik odtwarzacza problemach wykonane, tablice wyników wynik wysoki i społecznościowych wykresów są łatwiejsze do wdrożenia ze schematem elastyczne.
- DocumentDB jako zarządzanych platformy jako z usługi (PaaS) wymagane minimalnego ustawienia zarządzania służbowy, aby umożliwić szybkie iteracji i skrócenie czasu rynku.


## <a name="user-preferences-data"></a>Dane preferencji użytkownika
Dzisiaj najbardziej nowoczesne sieci web i aplikacji dla urządzeń przenośnych są dostarczane z złożonych widoków i środowiska. Te widoki i środowiska są zwykle dynamiczne, kuchennych preferencji użytkownika lub skojarzone nastroje i znakowanie potrzeb.  W związku z tym aplikacji konieczne będzie można pobrać ustawień spersonalizowanych skutecznie w celu szybkiego wyświetlania elementów interfejsu użytkownika i środowiska. 

JSON jest formatem skutecznych do przedstawiania danych układ interfejsu użytkownika, nie jest tylko proste, ale również można łatwo interpretować JavaScript.  DocumentDB oferuje poziomy ustawiane spójności, umożliwiające szybkie odczytuje z zapisy krótki czas oczekiwania. W związku z tym przechowywania danych układ interfejsu użytkownika tym spersonalizowanych ustawień dokumenty JSON w DocumentDB to skuteczny środek uzyskiwania tych danych w sieci.

## <a name="iot-and-device-sensor-data"></a>Dane czujnika IoT i urządzenia
Przypadków użycia IoT udostępnić często pewne wzorce w sposób one mogły zjeść tej ostatniej, przetwarzania i przechowywania danych.  Najpierw tych systemów Zezwalaj pobrania danych, które można mogły zjeść tej ostatniej seria danych z czujników urządzenia z różnych ustawień regionalnych.  Następnie tych systemów procesu i analizowania danych strumieniowych do uzyskania wniosków w czasie rzeczywistym. I koniec, najlepiej w przeciwnym razie wszystkie dane zostaną ostatecznie grunt w magazynie danych ad hoc analiz podczas badania i w trybie offline.    

Microsoft Azure oferuje zaawansowanych usług, których można użyć dla IoT przypadkami użycia.  Usług Azure IoT są zestawem usług w tym koncentratory zdarzenia Azure, Azure DocumentDB analizy strumieniu Azure, Azure powiadomień Centrum, Azure maszynowego uczenia, usługa Azure HDInsight i PowerBI. 

Seria danych można wchłonięte przez koncentratory zdarzenia Azure, jak zapewnia on wysokiej wydajności spożyciu danych z krótki czas oczekiwania. Dane wchłonięte, które ma zostać przetworzony dla wgląd w czasie rzeczywistym można funneled do analizy strumieniu Azure analiz w czasie rzeczywistym. Dane można załadować do DocumentDB dla kwerend ad hoc. Po załadowaniu danych do DocumentDB, dane są gotowe do zbadania.  Dane w DocumentDB może służyć jako danych źródłowych w ramach analizy w czasie rzeczywistym. Ponadto dane można dodatkowo być Uściślanie i przetwarzanych przez nawiązanie połączenia danych DocumentDB HDInsight świnka lub gałąź mapy/zmniejszanie zadań.  Elegancja następnie załadowaniu danych do DocumentDB dla raportowania.   

Przykładowe rozwiązania IoT przy użyciu DocumentDB, EventHubs i Burza zobacz [repozytorium hdinsight Burza — przykłady w GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Aby uzyskać więcej informacji na Azure oferty dla IoT zobacz [Tworzenie Internet i elementów](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Następne kroki
 
Aby rozpocząć pracę z DocumentDB, można utworzyć [konto](https://azure.microsoft.com/pricing/free-trial/) , a następnie wykonaj naszych [ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/documentdb/) informacje o DocumentDB i znajdowanie informacji, które są potrzebne. 

Lub, jeśli chcesz dowiedzieć się więcej o tym klienci korzystający z DocumentDB, są dostępne następujące artykuły klienta:

- [Następny gry](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Wiadomości Walking: Brak mężczyzna w strukturą gier soars # 1 obsługiwanych przez Azure DocumentDB.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Halo 5 implementacji społecznościowych gry przy użyciu Azure DocumentDB.
- [Galeria analizy Cortana](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Galeria analizy Cortana — witryny społeczności skalowalna oparty na Azure DocumentDB.
- [Błyskawicznie](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Prowadzące Integrator daje wglądu globalnego przedsiębiorstwa międzynarodowej w minutach z technologiami elastyczne chmury.
- [Republika wiadomości](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Dodawanie analizy do wiadomości o podanie informacji w celu obywateli włączone. 
- [Międzynarodowe SGS](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Jednolity kolor na całym świecie głównych marek przejdź do SGS. Oraz SGS powoduje Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globalne znak wiodący Telenor używa chmury, aby przenieść z szybkości uruchamiania. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Magazyn przyszłości działa na wyszukiwanie szybkie i łatwe przepływu danych.
