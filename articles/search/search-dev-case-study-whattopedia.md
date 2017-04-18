<properties 
    pageTitle="Analiza przypadku Deweloper Azure wyszukiwania: Jak WhatToPedia wbudowany infomedia portalu Microsoft Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury" 
    description="Dowiedz się, jak tworzyć informacji portal i meta aparat wyszukiwania za pomocą wyszukiwania Azure, chmury hostowana usługa wyszukiwania dla deweloperów." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Analiza przypadku Deweloper Azure wyszukiwania

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Jak [WhatToPedia.com](http://whattopedia.com/) wbudowany infomedia portalu Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Duży ogólny obraz</font> 


Nasze ogólny obraz jest tworzenie portalu informacje ułatwiające kupujący połączyć się z sprzedawców za pośrednictwem bardzo istotne, zakresu wyszukiwania środowisko, podobnie jak podróży turystów dopasowanie portali w górę z hoteli, restauracji i rozrywka w trybie uncharted terytorium. 

Portal, które możemy myśli będzie przedstawiana wyników wyszukiwania wyjątkowo wysokiej jakości nad danymi u sprzedawcy detalicznego w danym rynku, pomoc kupujący Znajdź Sklepy na podstawie lokalizacji i innych obiektów u sprzedawcy detalicznego zawiera. Firma Microsoft będzie obsługiwał aparat wyszukiwania z początkowego zestawu danych, ale wartość szczegółowego zostanie utworzona w czasie za pomocą subskrybentów u sprzedawcy detalicznego, którzy zawierają informacje dotyczące ich działalności. Promocje, nowe towarów, popularnych marek, usługi specjalne we własnym zakresie-— wszystkie są przykładowe dane, które dodaje wartość do witryny. Te dane są własny zgłoszone i zintegrowany Boże wyszukiwania po u sprzedawcy detalicznego zarejestrowaniu jako. Reklamami oraz subskrypcji, podaj strumienia przychodu dla naszych nowych przedsiębiorstw.

Wyszukiwanie będzie modelu interakcji użytkownika głównym na platformie wyłącznie chmury. W celu skali i koszty niska prawie wszystkie czynności, możliwości portalu do kontrolki źródła będzie widoczny za pośrednictwem usługi online. Za pomocą aparatu wyszukiwania, która zapewnia funkcje, które administrator powinien okno, możemy szybko utworzyć aplikacji wyszukiwania, bez konieczności tworzenia i zarządzania wyszukiwania aparat nad.

## <a name="what-we-built"></a>Firma Microsoft wbudowane

WhatToPedia jest firmę infomedia początkową. Firma Microsoft wbudowany [WhatToPedia.com](http://whattopedia.com/) — - obecnie badań rynku w Europie północnej z datą 2 lutego 2015 live podróży. Nasze bazy klientów jest przede wszystkim sklepach z cegły i moździerz potrzebują przystępnej obecności online, który jest łatwe w zarządzaniu i zachować.

Nasze zadanie jest przyciągania kupujący za pośrednictwem środowiska doskonałe wyszukiwanie w trybie online, zwiększenia wyniki bazujące na miasto lub klubu, marki magazynu nazw, lub typów. Przyciąganie kupujący ma efekt wpływu motywujące sprzedawców subskrybowanie witryny portalu. Subskrypcje są płatnej, stawki przystępnej.

 ![][7] 

Po utworzeniu konta dla subskrypcji sprzedawcy detalicznego przejmuje ich istniejącego profilu (utworzony początkowo przez dane kupowane) aktualizacją wraz z dodatkowymi informacjami o promocje, polecane marek lub ogłoszenia. We własnym zakresie funkcje, takie jak języki używane, walut zaakceptowane, wolnocłowe zakupy, można samodzielnie zgłoszoną lepiej przyciągania kupujący, których szukasz tych obiektów.

## <a name="who-we-are"></a>Kim jesteśmy

Moja nazwa jest Thomas Segato (Microsoft Consulting) i używanej z Tomasz Boelling, prowadzić Deweloper na WhatToPedia zaprojektować rozwiązanie. 

WhatToPedia jest uruchamiania badań marketingowych jej nowej portalu firmy w Szwecja, gdzie znajdują się najczęściej 60 000 sprzedawców Cegła moździerzu małych i średnich przedsiębiorstw (małych i średnich przedsiębiorstw). Ponieważ znają firma Microsoft świadczy wielu języków i zawiera wiele walut osoby zakupy w Europie, możemy utworzyć rozwiązania, które zezwalały shopper wielojęzycznych. Firma Microsoft wymagane i znaleźć aparat wyszukiwania, który obsługuje nasze wymagania wielojęzycznych w wyszukiwaniu Azure.

Azure wyszukiwania było zmieniacz gry dla naszych projektu. Przed dostępność wyszukiwania Azure możemy wydatkowane znaczące energii w kinks tworzenia własnego aparat wyszukiwania. Występuje Azure wyszukiwania usługi online usunięcia największych próg wykluczający technicznych i administracyjnych z naszych rozwiązanie, które są przeznaczone wprowadzenie do obrotu szybszą i bardziej rozbudowany wyników wyszukiwania.  

## <a name="how-we-did-it"></a>Jak NAS

Nasze wzroku było utworzyć pełną infrastrukturę tylko usług w chmurze. Microsoft została wybrana jako strategic platformy, ponieważ dostawca, który oferuje niezbędnych usług (w przypadku zarówno współpracy i rozwoju), skalowanie na żądanie i przystępnej ceny.
 
### <a name="high-level-components"></a>Składniki wysokiego poziomu

Firma Microsoft utworzono business, nie tylko witryny. Pomocnicze całego nakładu wymagane szeroką gamę narzędzi i aplikacji. Firma Microsoft przyjęła Visual Studio i Visual Studio Team Services dla rozwoju, Online zespołu usługi Foundation (TFS) dla kontrolki źródła i zarządzania scrum, usługi Office 365 dla komunikacji i współpracy i oczywiście Microsoft Azure do wszystkich działań związanych z witryny i przechowywania. Przy użyciu programu Visual Studio IDE udostępniana bezpośredni inicjowania obsługi administracyjnej Azure, z integracji TFS online dostarczając zwiększyć produktywność dodatkowe.

Poniższy diagram przedstawia składniki wysokiego poziomu używanych w infrastrukturze WhatToPedia.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Jak firma Microsoft korzysta z Microsoft Azure

Spojrzenie na zielone pola w poprzednim diagramu, zobaczysz, że rozwiązanie WhatToPedia jest oparty na następujących usług:

- [Azure wyszukiwania](https://azure.microsoft.com/services/search/)
- [Azure witryn sieci Web przy użyciu MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs zaplanowane zadania](../app-service-web/websites-webjobs-resources.md)
- [Baza danych SQL Azure](https://azure.microsoft.com/services/sql-database/)
- [Magazyn obiektów BLOB platformy Azure](https://azure.microsoft.com/services/storage/)
- [Dostarczanie poczty E-mail SendGrid](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Bardzo serce rozwiązania są dane i wyszukiwania. Poniżej przedstawiono przepływ danych od dostawcy sprzedawcy do odbiorcy końcowego:

  ![][9]

Podstawowy magazyn danych jest odsprzedawców i danych księgowych w bazie danych SQL Azure. Składa się z początkowego zestawu danych oraz danych u sprzedawcy detalicznego dodane w czasie. Aby opublikować aktualizacje z bazy danych SQL do Boże wyszukiwania do wyszukiwania Azure użyto programu WebJob Azure.

### <a name="presentation-layer"></a>Warstwy prezentacji

Portalu jest Azure witryny sieci Web, realizowane w MVC 4 i [Serwisu Twitter początkowego](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Wybraliśmy MVC, ponieważ obsługuje on wiele oczyszczania podejście do formatu HTML niż rozwoju oparte na formularzach programu ASP.NET. Aby uniknąć do tworzenia aplikacji dla urządzeń z wielu różnych urządzeń przenośnych platformach, serwisu Twitter początkowego została wybrana do obsługi wszystkich urządzeniach i platformach.

### <a name="authentication-permissions-and-sensitive-data"></a>Uwierzytelnianie, uprawnień i ważnych danych

Kupujący anonimowo Przeglądanie witryny. Jako takie nie ma żadnych wymagań logowania dla kupujący ani nie możemy przechowywania danych dla klientów indywidualnych. 

Sprzedawców są różne historii. W tym miejscu przechowywane informacje z profilu publicznej, informacje rozliczeniowe i zawartości multimedialnej, które mają być uwidaczniane w witrynie. Każdy u sprzedawcy detalicznego, które subskrybuje witryny Uzyskaj logowania użytkownika, używane do uwierzytelnienia użytkownika przed wprowadzeniem aktualizacje profilu abonenta.  Wszystkie dane subskrybentów bezpiecznie przechowywane w magazynie obiektów BLOB platformy Azure i bazy danych SQL Azure.
Firma Microsoft przyjął model uwierzytelniania na podstawie uwierzytelniania opartego na formularzach .NET. Wybraliśmy tej metody dla jego uproszczenia; nie jest potrzebna role, obsługa interfejsu użytkownika i inne funkcje zbędne, dostarczane z innych metod. 

W celu zapewnienia, że sprzedawców wyświetlane tylko dane, które należy je, możemy utworzyć sprzedawcy detalicznego identyfikator dla każdego sprzedawcy, następnie używany we wszystkich odczytywanie i zapisywanie operacji dotyczących danych u sprzedawcy detalicznego. Z tej metody znalezionych, że firma Microsoft nie muszą udzielanie uprawnień do bazy danych do poszczególnych sprzedawców. Wszystkie sprzedawców wchodzić w interakcje z układem w obszarze rolę jednej bazie danych o identyfikatorze u sprzedawcy detalicznego, co metoda izolacji naszych danych.

Ponieważ to wszystko, o nasza firma podrzędne efekty (bo więcej firm do sprzedawców, tworzenie zachęcający do ogłaszanie i zasubskrybuj), firma Microsoft można narysować linię na obsługę zakupy w sieci web. Jako takie nie oferuje koszyka w witrynie, co ułatwia nasze wymagania dotyczące zabezpieczeń. 

Inny uproszczenia, możemy wykorzystane było zewnętrzny naszych operacji płatnych kont i rozliczeń. Przez routingu informacji o płatności odbiorcy bezpośrednio do innych firm ([SveaWebPay](http://www.sveawebpay.se/)), firma Microsoft ograniczenia ryzyka kojarzenia z przechowywania i ochrona ważnych danych w naszym magazynów. 

### <a name="search-engine"></a>Aparat wyszukiwania

Podstawową Nasze rozwiązanie jest aparat wyszukiwania oparty na usługi Azure wyszukiwania. Początkowo możemy wbudowane niestandardowe wyszukiwarki, ale w trakcie tego procesu firma Microsoft zrealizowanych złożoności i nakładu był bardzo wysoki faktycznie i który zostanie wyświetlony monit abyśmy rozważ inne alternatywy. 

Podstawowe funkcje, które zostały najważniejszych z nami dostępny:

- Filtry
- Nawigacji aspektowej
- Zwiększenia wyników
- Stronicowanie za pośrednictwem AJAX

Wyszukiwanie w Internecie pozyskano nam wideo następujące podstawą abyśmy Wypróbuj Azure wyszukiwania: [Głębokości Dive u TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Po oglądaniem wideo, możemy są gotowe do utworzenia prototypu według możemy pokazano. Ponieważ mamy już modelu danych w MVC, tworzenia prototypu jest proste, ponieważ dane zawierają wyszukiwania terminów, a firma Microsoft była już określone wymagania dotyczące sposobu trzeba sortować, Faseta i filtrowanie danych. 

Jest to, jak firma Microsoft wbudowany prototypu.

**Konfigurowanie usługi Azure wyszukiwania**

1. Zaloguj się do portalu klasyczny Azure i dodać usługę wyszukiwania do naszych subskrypcji. Użyliśmy udostępniona wersja (bezpłatnie z naszych subskrypcji).
2. Tworzenie indeksu. Dla prototypu użyliśmy portalu interfejsu użytkownika zdefiniować pola wyszukiwania i tworzyć profile wyników. Nasze punktowania profilu na podstawie lokalizacji danych: kraj | miasta | adres (zobacz: Dodawanie profile wyników).
3. Skopiuj adres URL usługi i administrator klucz interfejsu api do plików konfiguracji. Ten klucz znajduje się na stronie usługi wyszukiwania w portalu i jest używany do uwierzytelniania usługi.
    
**Można opracowywać zadanie indeksowanie wyszukiwania — konsoli systemu Windows**

1. Przeczytaj wszystkich odsprzedawców z bazy danych.
2. Interfejs API usługi wyszukiwania Azure odsprzedawców kolejno przekazywania połączeń (zobacz: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Ustawianie właściwości w bazie danych sprzedawcy jest indeksowane przyrostowe indeksowania. Czy firma Microsoft to przez dodanie pola "indeksatora", w którym jest przechowywany stan indeksu dla każdego profilu (indeksowany). 

Zobacz dodatku dla wstawkę kodu, który tworzy zadanie indeksowania.

**Opracowanie portalu wyszukiwania — MVC**

1. Połączenie usługi wyszukiwania Azure uzyskanie wszystkie dokumenty z wyników wyszukiwania (zobacz: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Wyodrębnij zgodnie z odpowiedzi usługa wyszukiwania (przy użyciu json.net http://james.newtonking.com/json)
   - Wyniki
   - Aspekty
   - Liczniki wyników
   - Rozwijanie interfejs użytkownika umożliwiający wyświetlanie wyników wyszukiwania, aspekty i inwentaryzacji (mamy już to).

Jest to kod używanych do uzyskania wyników wyszukiwania Azure:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Zwiększenia według lokalizacji**

Prawdopodobnie najważniejszych wymogu Sprawdź, czy w prototypu uwzględniane, dodając słowo kluczowe wyszukiwania lokalizacji do danej kwerendy. Koniecznie portalu Jeśli użytkownik wprowadzi nazwę miasta w polu Wyszukaj, że kwerenda odsprzedawców w danym miasta czy klasyfikowanie większa niż sprzedawca mający Miasto słów kluczowych w opisie. Tego wymogu użyliśmy wyników profilu pozycji pola Miasto wyższymi niż inne pola.

**Obsługa wielu języków**

Firma Microsoft, aby wyświetlić wyniki wyszukiwania poprawne poprawne i podaj opcję znajdowania ten sam efekt w różnych językach. Były obu stron tego problemu: 

- Wyszukiwanie wyrazów w wielu językach
- Wyświetlanie wyników wyszukiwania w odpowiedniego języka

Firma Microsoft rozwiązać część prezentacji, dodając dokumentu dla każdego języka za pomocą zlokalizowany tekst i właściwości przy użyciu języka. Gdy użytkownik wprowadzi wyszukiwany termin, możemy użytkownika `$filter` wyrażeń, aby odfiltrować język użytkownik wybrał.

Wszystkie dokumenty zawiera ukryte właściwość o nazwie "miast" oparty na typie kolekcji. Ta właściwość przechowuje nazw miast w wszystkich języków, umożliwiając użytkownikom wyszukiwanie w wielu językach.

###<a name="data-storage"></a>Magazynowanie danych

Wszystkie dane (profil, subskrypcji i księgowych) są przechowywane w bazie danych SQL. Wszystkie pliki multimedialne są przechowywane w magazynie obiektów BLOB platformy Azure, w tym obrazów i klipów wideo dostarczony przez u sprzedawcy detalicznego. Przy użyciu oddzielnych magazyn obiektów BLOB izoluje efekty przekazywania plików. pliki nigdy nie są Współtworzenie mingled z witryną sieci Web, więc nie trzeba odbudować witryny, gdy dodajemy plików.

Ważne zaletą naszych projektu miejsca do magazynowania jest wielu deweloperów udostępnić pojedynczy opracowywania miejsca do magazynowania. Jeden z wymagań dotyczących projektu WhatToPedia był aby można było utworzyć środowisko projektowania w ciągu 15 minut, w tym dane sprzedawcy, obrazów i klipów wideo. Uzyskiwanie najnowszych danych z TFS Online, uruchamianie skryptu SQL i wykonywania zadania importu, kompletne środowisko może być umieszczenia w górę w żadnym na wszystkich. Metoda ta zwiększa również procesie przemieszczania.

###<a name="webjobs"></a>WebJobs

Aby zaktualizować dane do indeksu firma Microsoft korzysta z Azure WebJobs. Tworząc zadanie indeksowanie wyszukiwania, indeksowania części jest bardzo łatwa integrowanie Nasze rozwiązanie. Tylko Zmień kod wprowadzono została, aby zezwalały na zadaniu indeksatora było dodać `Indexed` pola do naszego modelu danych do wskazywania stanu indeksu. Gdy nowego profilu została dodana lub zaktualizowana, `Indexed` pola jest ustawiona na wartość false. To samo dotyczy u sprzedawcy detalicznego zmiana jego danych profilu za pośrednictwem portalu.  

Zadanie wyszukuje wszystkie wiersze o `Indexed` ustawiona na wartość false. Po znalezieniu wiersza, dokument jest opublikowany w usłudze Azure wyszukiwania, a następnie `Indexed` pola jest ustawiona na PRAWDA. Nie zostały w celu zaplanowania Dodawanie i aktualizowanie danych, ponieważ usługa wyszukiwania Azure faktycznie odpowiada za wykonywanie to. Jeśli dodasz dokument, który znajduje się już usługę wykona aktualizacji automatycznie.

Wszystkie zadania sieci web opracowano aplikacji konsoli, które mogą być przekazane do Azure witryny sieci web jako pliki ZIP, rozpakowane, a następnie według harmonogramu.

Zadanie zostaje zaplanowane co 5 minut jako zadanie zaplanowane w sieci web. Firma Microsoft obliczeniowych, że usługa przetwarza około trzech minut na przekazywanie dokumentów 3000 będący w naszym wymagania. 

> [AZURE.NOTE] Istnieje funkcja indeksatora prototypu ostatnio wprowadzono w wyszukiwaniu Azure. Ta funkcja została dostarczona zbyt opóźnione abyśmy mogli używać go w naszym natychmiastowego, ale wydaje się, aby rozwiązać ten sam problem, który użyliśmy naszych indeksatora zadania, która jest, aby zautomatyzować operacje ładowania danych.


###<a name="backup-strategy"></a>Strategii wykonywania kopii zapasowych

Firma Microsoft zaprojektowane wielopoziomowymi strategię wykonywania kopii zapasowych odzyskać z zakresu scenariuszy awarii losowych, w dół do odzyskiwania poszczególnych transakcji. Składniki majątku ochrony obejmuje trzy typy danych (witryny sieci web, dane subskrybenta i pliki multimedialne). 

Najpierw, zachowując kodu źródłowego witryny sieci web w TFS Online, możemy wiedzieć, że jeśli witryny ulegnie uszkodzeniu, firma Microsoft może jej odbudowanie publikując z TFS. 

Dane subskrybenta w bazie danych SQL Azure jest najbardziej ważnych elementów zawartości. Firma Microsoft ponownie to przy użyciu wbudowanych funkcji (zobacz [i przywracania kopii zapasowej bazy danych SQL Azure](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Harmonogram kopii zapasowej jest pełnej bazy danych kopii zapasowej raz w tygodniu, różnicy kopie raz dziennie i kopie zapasowe dziennika transakcji co 5 minut.  Określony rozmiar danych, to rozwiązanie jest większe niż odpowiednie dla naszych ilości danych natychmiastowej i przewidywanych.

Trzecie pliki wideo i obrazy przechowywane w magazynie obiektów BLOB platformy Azure. Firma Microsoft nadal ocenia ultimate plan kopii zapasowej tych danych, uwzględniając Explorer Cloudberry dla Azure jako potencjalne rozwiązania. Na obecnym używamy WebJob kopiowania obrazów i klipów wideo do innej lokalizacji.

##<a name="what-we-learned"></a>Firma Microsoft materiału

Ponieważ firma Microsoft już danych było łatwe ustalenie koncepcji. W ciągu godzin było prototypu z aspektami liczniki stronicowanie, sklasyfikowane jako profile, a w wynikach wyszukiwania. Wyniki wyszukiwania są tak dokładne, możemy decyzję o usunięcie niektórych filtrów przedstawione odbiorcy końcowego. 

Największa niespodzianek nam był szybkość może informacje Azure wyszukiwania i ile możemy powrocie. Dosłownie możemy ustanowione koncepcji w ciągu kilku godzin (zobacz poniższą uwagę), zamieniając 500 wierszy kodu 3 wiersze kodu w aplikacji frontonu (oraz nowe WebJob) i uzyskiwania lepszych wyników. 

Wcześniej naszemu kodowi zaimplementowana stronicowanie, liczniki i innych zachowań standardowy do wyszukiwania. W wyszukiwaniu Azure wyniki, które możemy wrócić obejmują trafienia wyszukiwania, aspekty stronicowanie danych, liczniki — wszystkie elementy, możemy potrzebne i zostały konieczności podać nad. On również uwzględniony zwiększenia i wbudowanych nawigacji aspektowej, której nie udostępniamy Nasze rozwiązanie oryginalny.

Największym wyzwaniem w trakcie realizacji był, że został on w wersji Preview i znajdowanie informacji i doświadczeń udostępnionego było trudne. Po połączeniu możemy kilka kropki, znaleziono że przy użyciu usługi wyszukiwania Azure jest bardzo proste, ze względu na jej format danych w interfejsie API usługi REST i JSON. Można nazywamy ramach bezpośrednio z najbardziej otwartych wtyczek źródła, takie jak JQuery JSON.Net, a firma Microsoft można użyć narzędzi, takich jak Fiddler szybkie eksperyment i debugowania. 

> [AZURE.NOTE] Oprócz konieczności danych prepped, ułatwiła te w Stanach Zjednoczonych już tworzenia prototypu zrozumienie jak działa technologia, co nam wydajniej i bardziej appreciative wbudowanych funkcji Wyszukiwanie. Jeśli chcesz rozpocząć wydajną pracę na skonstruować kwerendy wyszukiwania nawigacji aspektowej, filtry itp możesz się spodziewać prototypami wolniej. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Sterowanie aspekty na stronie wyszukiwania prezentacji

Jeden z naszych learnings podczas dowód koncepcja był planowanie aspekty starannie ustalonymi. Po załadowaniu wiele danych do rozwiązania, możemy pokazano, że wielkość znanej ogólnej aspekty jest zbyt duża, aby przedstawić użytkownikom. 

Firma Microsoft to rozwiązać, ograniczając parametr liczba Faseta. Parametr liczba nakłada stały limit liczby aspekty zwracanych użytkownikowi. Łącze, które zawiera omówienie parametru liczba można znaleźć [tutaj](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs planowania zadań

Azure wyszukiwania nie tylko przyjemne niespodzianek us. Firma Microsoft wykryte, że aby zautomatyzować operacje ładowania naszych danych do wyszukiwania Azure za pomocą WebJobs zakończyło się znacznie większą naszych poprzedniego podejście, które tytułu, przy użyciu maszyny dedykowane systemie Windows Scheduler zaplanowanych zadań aktualizowania indeksu wyszukiwania. WebJobs było łatwiej skonfigurować i ułatwia debugowania i oczywiście wielu tańsze niż potrzeby płacenia za dedykowane maszyn wirtualnych.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure Eksplorator magazynu obiektów BLOB aktualizacji obrazy

Odnaleziono, który być bardzo przydatne w zarządzaniu aktualizacje obrazu i wideo do witryny za pomocą [Eksploratora magazynu obiektów BLOB platformy Azure](https://azurestorageexplorer.codeplex.com/) (dostępne w witrynie codeplex). Firma Microsoft korzysta z go jako narzędzie Deweloper ręcznie zaktualizować obrazów i klipów wideo, które są częścią witryny głównej. Możemy znaleźć, aby był bardziej elastyczne niż wdrażanie zmiany do portalu i eliminuje iteracji pełne badanie zawsze, gdy trzeba aktualizacji obrazu. 

##<a name="a-few-final-words"></a>Kilka wyrazów wersja ostateczna

Dzięki doskonałe osób u WhatToPedia umożliwiające us udostępnianie ich opowieści!  

Firma Microsoft Życzymy udało Ci się znaleźć tego analiza przypadku przydatne. Jeśli przejdź do za pomocą funkcji wyszukiwania Azure najlepiej kilka zasobów, aby przyspieszyć możesz wzdłuż:

- [Forum w witrynie MSDN przeznaczonych do wyszukiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [Zdarzeń StackOverflow też ma znacznika](http://stackoverflow.com/questions/tagged/azure-search)
- [Strona dokumentacji w Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure dokumentacji wyszukiwania w witrynie MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Dodatek: WebJob indeksowanie wyszukiwania

Poniższy kod tworzy indeksatora wymienionych w sekcji Tworzenie prototypu.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
