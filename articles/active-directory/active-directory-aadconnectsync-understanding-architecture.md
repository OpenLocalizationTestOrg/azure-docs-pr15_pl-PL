<properties
   pageTitle="Azure AD Connect synchronizacją: omówienie architektury | Microsoft Azure"
   description="W tym temacie opisano architektura Azure AD Connect synchronizacji i wyjaśniono terminy używane."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect synchronizacją: omówienie architektury
W tym temacie opisano podstawowe architektura Azure AD Connect synchronizacji. W wielu aspektach jest podobne do jego poprzedników MIIS 2003, ILM 2007 i FIM 2010. Azure AD Connect synchronizacja jest ewolucji technologii. Użytkownicy zaznajomieni z dowolnego z tych starszych technologii, zawartość tego tematu będzie znane także. Jeśli jesteś nowym użytkownikiem synchronizacji, w tym temacie jest dla Ciebie. Jednak nie jest wymagane do Poznaj szczegóły w tym temacie się pomyślnie dokonując dostosowań do synchronizacji Azure AD Connect (nazywanych aparatu synchronizacji w tym temacie).

## <a name="architecture"></a>Architektura
Aparat synchronizacji tworzy zintegrowany widok obiektów, które są przechowywane w wielu połączonych źródeł danych i zarządza tożsamości informacje w tych źródeł danych. W tym widoku zintegrowane zależy od informacji o tożsamości pobrane z połączonych źródeł danych i zestaw reguł określających sposób przetwarzania te informacje.

### <a name="connected-data-sources-and-connectors"></a>Połączonych źródeł danych i łączników
Aparat synchronizacji przetwarza informacje tożsamości z repozytoria innych danych, takich jak usługi Active Directory lub bazy danych programu SQL Server. Repozytorium wszystkich danych, która porządkuje jej dane w postaci bazy danych i, która zapewnia dostęp do danych standardowe metody jest potencjalny candidate źródła danych aparatu synchronizacji. Repozytoria danych, które są synchronizowane przez aparat synchronizacji są nazywane **połączonych źródeł danych** lub **połączenie katalogów** (CD).

Aparat synchronizacji obejmuje interakcji ze źródłem danych połączonych w module o nazwie **Łącznik**. Każdego typu źródła połączonych danych zawiera określone łącznik. Łącznik tłumaczy operacji wymagane do formatu, który zrozumie podłączonego źródła danych.

Łączniki nawiązywania połączeń interfejsu API do wymiany informacji o tożsamości (zarówno odczytu i zapisu) ze źródłem danych połączonych. Użytkownik może również dodać łącznik niestandardowy przy użyciu struktury extensible łączności. Na poniższej ilustracji przedstawiono sposób łącznik łączenia źródła danych połączonych aparat synchronizacji.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

W dowolnym kierunku można przepływu danych, ale go nie układają się w obu kierunków jednocześnie. Innymi słowy aby umożliwić przepływ z podłączonego źródła danych do aparatu synchronizacji lub aparat synchronizacji ze źródłem danych połączonych danych można skonfigurować łącznika, ale tylko jeden z tych czynności mogą występować w dowolnym momencie dla jednego atrybutów i obiektów. Kierunek może być różne dla różnych obiektów i różne właściwości.

Aby skonfigurować łącznik, możesz określić typy obiektów, które chcesz zsynchronizować. Określanie typów obiektów określa zakres obiektów, które znajdują się w trakcie synchronizacji. Następnym krokiem jest zaznacz atrybuty, aby zsynchronizować, która nosi nazwę Lista dołączania atrybut. Te ustawienia można można zmienić w dowolnym momencie w odpowiedzi na zmiany do reguł biznesowych. Użycie Kreatora instalacji narzędzie Azure AD Connect, te ustawienia są skonfigurowane dla Ciebie.

Aby wyeksportować obiektów do źródła danych połączonych, lista dołączania atrybut musi zawierać co najmniej atrybuty minimalne wymagane do utworzenia określonego typu obiektu w źródle danych połączonego. Na przykład atrybut **sAMAccountName** musi zostać uwzględnione na liście dołączania atrybut, aby wyeksportować obiekt użytkownika do usługi Active Directory, ponieważ wszystkich obiektów użytkowników w usłudze Active Directory musi mieć atrybut **sAMAccountName** zdefiniowane. Ponownie Kreatora instalacji oznacza tej konfiguracji dla Ciebie.

Jeśli źródło danych połączonych używa elementów konstrukcyjnych, taki jak partycje lub kontenery do organizowania obiektów, można ograniczyć obszarów w źródle danych połączonych, które są używane dla danego rozwiązania.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Struktura wewnętrzna nazw aparatu synchronizacji
Przestrzeń nazw aparatu synchronizacji całego składa się z dwóch nazw, które przechowywanie informacji o tożsamości. Są dwa nazw:

- Obszar łączników (CS)
- Metaverse (MV)

**Obszar łączników** jest obszaru tymczasowego, zawierającego reprezentujące wyznaczonych obiekty ze źródła danych połączonych i atrybuty znajdujących się na liście włączenia atrybut. Aparat synchronizacji korzysta obszar łączników, aby sprawdzić, co się zmieniło w źródle danych połączonego i etapu przychodzące zmiany. Aparat synchronizacji używa też obszar łączników do etapu wychodzących zmian do eksportowania do tego źródła danych połączonych. Aparat synchronizacji zachowuje obszar odrębnych łączników jako obszaru tymczasowego dla każdego łącznika.

Przy użyciu obszaru tymczasowego, aparatu synchronizacji pozostaje niezależnie od połączonych źródeł danych, a nie dotyczy ich dostępność i ułatwienia dostępu. W wyniku informacji o tożsamości w dowolnym momencie można przetwarzać przy użyciu danych w obszarze tymczasowym. Aparat synchronizacji można zażądać tylko zmiany wprowadzone w źródle danych połączonego od czasu ostatniego sesji komunikacji zamykane lub wypychane tylko zmiany informacje tożsamości, które nie otrzymał jeszcze podłączonego źródła danych, co zmniejsza ruch sieciowy między aparat synchronizacji i podłączonego źródła danych.

Ponadto aparat synchronizacji przechowuje informacje o wszystkich obiektów, że poziomach obszar łączników stanie. Po odebraniu nowych danych aparatu synchronizacji zawsze oblicza, czy dane była już synchronizowana.

**Metaverse** jest obszarem przechowywania, zawierający informacje o tożsamości zagregowane z wielu źródeł danych połączonych, dostarczając pojedynczego widoku globalnego, zintegrowane wszystkie obiekty połączone. Obiekty metaverse są tworzone na podstawie informacji tożsamości pobierane z połączonych źródeł danych i zestaw reguł, które umożliwiają dostosowanie procesu synchronizacji.

Na poniższej ilustracji przedstawiono przestrzeń obszar łączników i nazw metaverse w aparat synchronizacji.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Obiekty tożsamości aparatu synchronizacji
Obiekty aparatu synchronizacji są reprezentacji albo obiektów w źródle danych połączonego lub zintegrowane widok, który synchronizowanie aparat ma tych obiektów. Każdy obiekt aparatu synchronizacji musi mieć unikatowy identyfikator globalny (GUID). Identyfikatory GUID zapewnia integralność danych i express relacje między obiektami.

### <a name="connector-space-objects"></a>Obiekty miejsca łączników
Podczas synchronizacji aparat komunikuje się ze źródłem danych połączonych, odczytuje informacje tożsamości w źródle danych połączonego i informacje użyte do tworzenia hierarchii obiektu tożsamości w obszarze łącznik. Nie można tworzyć ani usuwać te obiekty pojedynczo. Jednak można ręcznie usunąć wszystkie obiekty na obszar łączników.

Wszystkie obiekty w obszarze łącznik mają dwa atrybuty:

- Unikatowy identyfikator globalny (GUID)
- Nazwa wyróżniająca (nazywane także nazwy domeny)

Jeśli źródło danych połączonych przypisuje unikatowy atrybut do obiektu, w obiektów w obszarze łącznika można także używać atrybutu kotwicy. Atrybut kotwicy jednoznacznie identyfikuje obiektu w źródle danych połączonego. Aparat synchronizacji używa zakotwiczenie do lokalizowania odpowiednich reprezentacja ten obiekt w źródle danych połączonego. Aparat synchronizacji przyjęto założenie, że zakotwiczenie obiektu nigdy nie zmienia się w okresie istnienia obiektu.

Znane Unikatowy identyfikator wiele łączników służy do generowania kotwicy automatycznie dla każdego obiektu, gdy są importowane. Na przykład łącznik usługi Active Directory korzysta z atrybutu **objectGUID** dla kotwicy. Połączonych źródeł danych, które nie zawierają jasno zdefiniowane Unikatowy identyfikator można określić generowanie kotwicy w ramach konfiguracji łącznika.

W tym przypadku kotwicy jest tworzony z jedną lub więcej unikatowe atrybuty obiektu wpisz ani zmiany i który jednoznacznie identyfikuje obiekt w obszarze łącznika (na przykład numer pracownika lub Identyfikatora użytkownika).

Obiekt miejsca łącznika może być jedną z następujących czynności:

- Obiekt tymczasowy
- Symbol zastępczy

### <a name="staging-objects"></a>Organizowanie obiektów
Tymczasowy obiekt reprezentuje wystąpienie typy obiektów wyznaczonych ze źródła danych połączonych. Identyfikator GUID i nazwa wyróżniająca tymczasowy obiekt ma zawsze wartość, która wskazuje typ obiektu.

Organizowanie obiektów, które zostały zaimportowane zawsze mieć wartość atrybutu kotwicy. Organizowanie obiektów, które nowo zainicjowano przez aparat synchronizacji i są tworzonych w źródle danych połączonego nie mają wartość atrybutu kotwicy.

Organizowanie obiektów również zawierać bieżące wartości atrybutów firm i operacyjne informacje wymagane przez aparat synchronizacji przeprowadzenie procesu synchronizacji. Informacje operacyjne zawiera flagi wskazujące typ aktualizacji, które są umieszczane na tymczasowy obiekt. Jeśli obiekt tymczasowy otrzymał informacje o nowej tożsamości ze źródła danych połączonego, która jeszcze nie została przetworzona, obiekt jest oznaczony jako **oczekującego importowania**. Jeśli tymczasowy obiekt ma nowe informacje tożsamości, które jeszcze nie zostały wyeksportowane ze źródłem danych połączonych, jest oznaczony jako **Oczekujące eksportu**.

Tymczasowy obiekt może być importowania lub eksportowania obiektu. Aparat synchronizacji tworzy obiekt importu przy użyciu obiektu informacje otrzymane od podłączonego źródła danych. Kiedy aparat synchronizacji odbiera informacje o istnieniu nowy obiekt, który pasuje do jednej typy obiektów zaznaczony w łącznik, tworzy obiekt importu w obszarze łącznika jako reprezentacja obiektu w źródle danych połączonego.

Na poniższej ilustracji przedstawiono importowanie obiekt reprezentujący obiektu w źródle danych połączonego.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Aparat synchronizacji tworzy obiekt eksportu przy użyciu informacji o obiekcie w metaverse. Eksportuj obiekty są eksportowane do źródła danych połączonych podczas następnej sesji komunikacji. Z perspektywy aparat synchronizacji eksportu obiektów nie istnieją w źródle danych połączonego jeszcze. Dlatego atrybut kotwicy eksportowania obiektu nie jest dostępna. Po odebraniu obiekt z aparatu synchronizacji ze źródłem danych połączonych tworzy unikatową wartość atrybutu kotwica obiektu.

Na poniższej ilustracji przedstawiono sposób tworzenia eksportowania obiektu przy użyciu informacji o tożsamości w metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Aparat synchronizacji potwierdza eksportowania obiektu przez ponownie zaimportować obiekt z podłączonego źródła danych. Eksportuj obiekty stają się importowanie obiektów, kiedy aparat synchronizacji odbiera je podczas następnego importowania z tego źródła danych połączonych.

### <a name="placeholders"></a>Symbole zastępcze
Aparat synchronizacji przechowuje prostym nazw obiektów. Jednak niektóre źródła danych połączonych, takich jak usługi Active Directory za pomocą hierarchicznej przestrzeni nazw. Aby informacje z hierarchicznej przestrzeni nazw umożliwia przekształcenie prostym przestrzeń nazw, aparatu synchronizacji zachowanie hierarchii przy użyciu symboli zastępczych.

Każdy symbol zastępczy odpowiada (na przykład jednostka organizacyjna) część nazwy hierarchii obiektu, który nie został zaimportowany do aparatu synchronizacji, ale jest wymagane do utworzenia hierarchii nazwę. Wypełniania odstępy utworzone przez odwołania w źródle danych połączonych obiektów, które nie są tymczasowej obiektów w obszarze łącznik.

Aparat synchronizacji również przechowuje symboli zastępczych obiektów odwołania, które nie zostały zaimportowane. Na przykład jeśli synchronizacja jest skonfigurowana do uwzględniania atrybutu menedżera obiektu *Abbie Spencer* i otrzymanej wartości jest obiekt, który nie został jeszcze zaimportowane, takich jak *CN = Sperry Jankowski, CN = Użytkownicy, kontrolera domeny = fabrikam, kontrolera domeny = com*, Menedżer informacje są przechowywane jako symbole zastępcze w obszarze łącznik. Jeśli później zaimportowaniu obiekt menedżera symbol zastępczy obiektu jest zastępowana przez tymczasowy obiekt reprezentujący kierownika.

### <a name="metaverse-objects"></a>Obiekty metaverse
Obiekt metaverse zawiera zagregowany widok tego aparatu synchronizacji występują tymczasowy obiektów w obszarze łącznik. Aparat synchronizacji tworzy metaverse obiekty, korzystając z informacji w Importowanie obiektów. Kilka obiektów miejsca łącznika można połączyć z obiektu metaverse pojedynczego, ale obiekt miejsca łącznika nie można połączyć z więcej niż jednego obiektu metaverse.

Obiekty metaverse nie można ręcznie utworzyć ani usuwać. Aparat synchronizacji automatycznie usuwa metaverse obiektów, które nie mają łącza do dowolnego obiektu miejsca łącznika w obszarze łącznik.

Aby zamapować obiekty źródła danych połączonych do odpowiedniego typu obiektu w obrębie metaverse, aparatu synchronizacji zawiera extensible schematu przy użyciu wstępnie zdefiniowanego zestawu typy obiektów i skojarzonych z nią atrybutów. Można tworzyć nowe typy obiektów i atrybuty metaverse obiektów. Atrybuty mogą być jedno- lub wielowartościowe i typy atrybut można ciągów, odwołania, liczb i wartości logiczne.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relacje między obiektami tymczasowy i metaverse
W obszarze nazw aparatu synchronizacji przepływu danych jest włączone według relacji łącza między obiektami tymczasowy i metaverse. Tymczasowy obiekt, który jest połączony z obiektu metaverse nosi nazwę **sprzężone obiektu** (lub **obiekt łącznika**). Tymczasowy obiekt, który nie jest połączony z obiektu metaverse nosi nazwę **odłączony obiektu** (lub **izolacyjny obiekt**). Terminy sprzężone i odłączony są preferowane nie należy mylić z łącznikami odpowiedzialny za importowania i eksportowania danych z katalogu połączonego.

Symbole zastępcze nigdy nie są połączone z obiektu metaverse

Obiekt połączonych obejmuje tymczasowy obiekt i jego relacji do obiektu metaverse pojedynczy. Sprzężone obiekty są używane do synchronizacji wartości atrybutu między obiektu miejsca łącznik i obiektu metaverse.

Obiekt tymczasowy stał się sprzężonych obiektu podczas synchronizacji, atrybuty może przechodzić między tymczasowy obiektu i obiektu metaverse. Atrybut jest dwukierunkowa i skonfigurowano przy użyciu reguł atrybut importowanie i eksportowanie reguł atrybut.

Obiekt miejsca pojedynczego łącznika można połączyć tylko do jednego obiektu metaverse. Jednak każdy obiekt metaverse można połączyć z kilku obiektów miejsca łącznika w tym samym lub innym łącznik spacje, jak pokazano na poniższej ilustracji.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Połączone powiązanie tymczasowy obiekt z obiektu metaverse jest trwała i może zostać usunięty tylko przez reguły, które można określić.

Obiekt odłączonym jest tymczasowy obiekt, który nie jest połączony z dowolnego obiektu metaverse. Atrybut, który wartości odłączonym obiekt nie są przetwarzane dowolną dodatkowo w metaverse. Wartości atrybutów odpowiedniego obiektu w źródle danych połączonego nie są aktualizowane przez aparat synchronizacji.

Przy użyciu obiektów odłączonym, można przechowywać informacje o tożsamości aparatu synchronizacji i procesu go później. Zachowanie tymczasowy obiekt jako obiekt odłączonym w obszarze łącznik ma wiele zalet. Ponieważ system jest już zainstalowana wymaganych informacji na temat tego obiektu, nie jest konieczne do utworzenia postać tego obiektu ponownie podczas następnego importowania z podłączonego źródła danych. W ten sposób aparat synchronizacji zawsze ma pełną migawkę podłączonego źródła danych, nawet jeśli istnieje bieżące połączenie ze źródłem danych połączonych. Obiekty odłączonym można przekonwertować na połączonych obiektów i odwrotnie, w zależności od reguły, które można określić.

Obiekt importu jest tworzony jako obiekt odłączonym. Eksportowanie obiektu musi być połączonych obiektów. Logika system wymusza tę regułę i usuwa każdej eksportowania obiektu, który nie jest obiektem połączonych.

## <a name="sync-engine-identity-management-process"></a>Proces zarządzania tożsamości aparatu synchronizacji
Proces zarządzania tożsamości określa sposób aktualizowania informacji o tożsamości między różnymi połączonego źródłami danych. Zarządzanie tożsamościami występuje w trzech procesów:

- Importowanie
- Synchronizacja
- Eksportowanie

Podczas procesu importowania aparatu synchronizacji oblicza informacje dotyczące tożsamości poczty przychodzącej ze źródła danych połączonych. Gdy zmiany zostaną wykryte, jego tworzy nowe obiekty tymczasową albo aktualizacje istniejących tymczasowy obiektów w obszarze łącznik do synchronizacji.

Podczas procesu synchronizacji aparatu synchronizacji aktualizuje metaverse w celu odzwierciedlenia zmian wprowadzonych w obszarze łącznik i aktualizuje miejsca łącznika w celu odzwierciedlenia zmian wprowadzonych w metaverse.

Podczas procesu eksportowania aparatu synchronizacji umieszcza wprowadzonych zmian, które są umieszczane na tymczasowej obiektów i oflagowane jako oczekujące na eksport.

Na poniższej ilustracji pokazano każdego z procesów wykrycie jako tożsamości przepływu informacji z jednego źródła połączonych danych do innego.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Wyniki operacji importowania
Podczas procesu importowania aparatu synchronizacji oblicza aktualizacje informacji tożsamości. Aparat synchronizacji porównuje tożsamości informacje otrzymane od źródła danych połączonych z informacje dotyczące tymczasowy obiekt tożsamości i określa, czy obiekt tymczasowy wymaga aktualizacji. Jeśli jest konieczne zaktualizowanie tymczasowy obiekt z nowymi danymi, tymczasowy obiekt jest oznaczony jako oczekujące importu.

Dzięki obiektów w obszarze łącznik przed synchronizacją, aparatu synchronizacji może przetwarzać informacje tożsamości, która została zmieniona. Ten proces zapewnia następujące korzyści:

- **Wydajność synchronizacji**. Ilość danych przetwarzanych podczas synchronizacji jest zminimalizowane.
- **Ponowne synchronizowanie wydajność**. Możesz zmienić sposób aparat synchronizacji przetwarza informacje tożsamości bez ponownego łączenia się z aparatu synchronizacji ze źródłem danych.
- **Szansa sprzedaży, aby wyświetlić podgląd synchronizacji**. Można wyświetlić podgląd synchronizacji, aby sprawdzić, czy założeń o procesu zarządzania tożsamościami są poprawne.

Dla każdego obiektu określonego w łącznik aparat synchronizacji najpierw próbuje określić lokalizację postać obiektu w obszarze łącznika na łącznik. Aparat synchronizacji sprawdza, czy wszystkie obiekty tymczasowy obszar łączników i próbuje znaleźć odpowiedniego obiekt tymczasowy, który ma odpowiadającego mu atrybut kotwicy. Jeśli nie ma istniejącego obiektu tymczasowy ma odpowiadającego mu atrybut kotwicy, aparatu synchronizacji próbuje znaleźć odpowiedni obiekt tymczasowy o takiej samej nazwie wyróżniająca.

Gdy aparat synchronizacji znajdzie tymczasowy obiekt, który odpowiada nazwa wyróżniająca, ale nie kotwicy, występuje następujące szczególne zachowanie:

- Jeśli obiekt znajdujący się w obszarze łącznik ma nie kotwicy, aparatu synchronizacji usuwa ten obiekt z obszar łączników i oznacza obiekt metaverse, który jest połączony jako **Ponów próbę inicjowania obsługi administracyjnej podczas następnej synchronizacji Uruchom**. Następnie tworzy nowy obiekt importu.
- Jeśli obiekt znajdujący się w obszarze łącznik ma kotwicy, aparatu synchronizacji założono, że ten obiekt albo został przeniesiony lub usunięty w katalogu połączonego. Przypisuje tymczasowe, nowe nazwa wyróżniająca obiektu miejsca łącznik tak, aby można umieścić przychodzące obiektu. Stary obiekt staje się **przejściowych**, trwa oczekiwanie na łącznik, aby zaimportować zmiany nazwy lub usunięcia, aby rozwiązać problem.

Jeśli aparat synchronizacji zwraca tymczasowy obiekt, który odpowiada obiektu określonego w łącznik, określa jakiego rodzaju zmiany, aby zastosować. Na przykład aparat synchronizacji może Zmień nazwę lub Usuń obiekt w źródle danych połączonego lub tylko może ją aktualizować atrybuty obiektu.

Organizowanie obiektów z zaktualizowane dane są oznaczone jako oczekujące na importowanie. Dostępne są różne typy oczekujące operacje importowania. W zależności od wynik procesu importowania tymczasowy obiekt w obszarze łącznik ma jedną z następujących oczekujących typów importu:

- **Brak**. Żadne zmiany do dowolnego atrybuty obiektu tymczasowy są dostępne. Aparat synchronizacji nie oznaczał tego typu importu oczekujących.
- **Dodawanie**. Tymczasowy obiekt jest nowy obiekt importu w obszarze łącznik. Aparat synchronizacji flagi tego typu podczas importu do dodatkowych przetwarzania w metaverse oczekujących.
- **Aktualizacja**. Aparat synchronizacji znajduje odpowiedni obiekt tymczasowy w obszar łączników i flagi tego typu jako importu oczekujących, tak aby aktualizacje atrybuty mogą być przetwarzane w metaverse. Aktualizacje obejmują zmiana nazwy obiektu.
- **Usuwanie**. Aparat synchronizacji znajduje odpowiedni obiekt tymczasowy w obszar łączników i flagi tego typu jako importu oczekujących tak, aby połączonych obiektów można usunąć.
- **Dodaj/Usuń**. Aparat synchronizacji umożliwia znalezienie wyrazów odpowiedni obiekt tymczasowy w obszarze łącznika, ale typów obiektów, które nie pasują. W tym przypadku usuń dodawanie modyfikacji jest umieszczane. A Usuń dodawanie modyfikacji wskazuje aparat synchronizacji, że wykonano ponowne synchronizowanie tego obiektu musi przypadać różnych zestawów reguł są stosowane do tego obiektu podczas zmiany typ obiektu.

Ustawiając stan oczekującego importowania obiektu tymczasowy, prawdopodobnie znacznie zmniejszyć ilość danych przetwarzanych podczas synchronizacji, ponieważ spowoduje pozwala system przetwarzania tylko te obiekty, które zostały zaktualizowane dane.

### <a name="synchronization-process"></a>Proces synchronizacji
Synchronizacja składa się z dwóch powiązanych procesów:

- Po zaktualizowaniu zawartości metaverse przy użyciu danych w polu łącznik ruchu przychodzącego synchronizacji.
- Synchronizacja ruchu wychodzącego, gdy zawartość obszaru łącznik zostanie zaktualizowana przy użyciu danych z metaverse.

Przy użyciu informacji umieszczane w obszarze łącznika, proces synchronizacji ruchu przychodzącego tworzy w metaverse zintegrowany widok danych, który jest przechowywany w połączonych źródeł danych. Wszystkie obiekty tymczasowy lub tylko te informacje oczekującego importowania są połączone, w zależności od konfiguracji reguł.

Aktualizacji proces synchronizacji ruchu wychodzącego wyeksportować obiekty po zmianie metaverse obiektów.

Synchronizacja ruchu przychodzącego tworzy zintegrowany widok w metaverse odebrane od źródeł danych połączonych informacje tożsamości. Aparat synchronizacji może przetworzyć informacji o tożsamości w dowolnym momencie przy użyciu najnowszych informacji tożsamości, które ma ze źródła danych połączonych.

**Ruch przychodzący synchronizacji**

Synchronizacja ruchu przychodzącego obejmuje następujące procesy:

- **Obsługa administracyjna** (zwanych również **rzut** jeśli ważne jest, aby odróżnić ten proces od ruchu wychodzącego synchronizacji inicjowania obsługi administracyjnej). Aparat synchronizacji tworzy nowy obiekt metaverse według tymczasowy obiekt i łączy je. Obsługa administracyjna jest operacji na poziomie obiektu.
- **Dołączanie**. Aparat synchronizacji łączy tymczasowy obiekt z istniejącym obiektem metaverse. Sprzężenia jest operacji na poziomie obiektu.
- **Importowanie przepływu atrybut**. Aparat synchronizacji aktualizuje wartości atrybutów o nazwie przepływ atrybut, obiektu w metaverse. Importowanie przepływu atrybut jest operacji na poziomie atrybut wymaga połączenia między obiektu tymczasowy i obiektu metaverse.

Obsługa administracyjna jest tylko procesu, który tworzy obiekty w metaverse. Obsługa administracyjna dotyczy tylko importowanie obiektów, które są odłączonym obiektów. Podczas świadczenia aparatu synchronizacji tworzy obiekt metaverse, który odpowiada typowi obiektu importowanego obiektu i nawiązuje połączenie między obu obiektów, co powoduje utworzenie połączonych obiektów.

Proces sprzężenia również nawiązuje połączenie między Importowanie obiektów i obiektu metaverse. Różnica między sprzężenia i dostarczanie jest, że proces sprzężenia wymaga, że obiekt importu są połączone z istniejącym obiektem metaverse, gdzie proces świadczenia tworzy nowy obiekt metaverse.

Aparat synchronizacji próbuje dołączyć obiektu importu do obiektu metaverse przy użyciu kryteria określone w konfiguracji synchronizacji reguły.

Podczas procesu dostarczania i Dołącz do aparatu synchronizacji łączy odłączonym obiektu do obiektu metaverse, dzięki czemu można je sprzężone. Po zakończeniu tych operacji na poziomie obiektu aparatu synchronizacji można aktualizować atrybuty obiektu metaverse skojarzone. Ten proces jest nazywany przepływu atrybut importowanie.

Importowanie atrybut przepływ występuje na wszystkich Importowanie obiektów, wykonać nowe dane, które są połączone z obiektu metaverse.

**Synchronizacja ruchu wychodzącego**

Aktualizacji synchronizacji ruchu wychodzącego wyeksportować obiektów, gdy obiektu metaverse zmienić, ale nie zostanie usunięty. W celu synchronizacji ruchu wychodzącego jest ocena czy zmiany obiektów metaverse wymagają aktualizacje tymczasowy obiektów w odpowiednich miejscach łącznik. W niektórych przypadkach zmiany mogą wymagać tego tymczasowego obiektów w wszystkie spacje łącznik zostaną zaktualizowane. Organizowanie obiektów, które są zmieniane są oznaczane jako oczekujące Eksportuj, dzięki czemu można je wyeksportować obiekty. Te eksportu obiektów później przesyłanych ze źródłem danych połączonych w procesie eksportowania.

Synchronizacja ruchu wychodzącego występują trzy procesów:

- **Inicjowanie obsługi administracyjnej**
- **Cofanie ubezpieczeń**
- **Eksportowanie przepływu atrybut**

Ubezpieczenia i cofanie ubezpieczeń są operacjami na poziomie obiektu. Cofanie ubezpieczeń zależy od inicjowania obsługi administracyjnej, ponieważ tylko inicjowania obsługi administracyjnej może inicjować go. Cofanie ubezpieczeń zostanie wywołana podczas inicjowania obsługi administracyjnej usuwa łącze między obiektu metaverse i eksportowania obiektu.

Zawsze wywołany inicjowania obsługi administracyjnej, gdy zmiany są stosowane do obiektów w metaverse. Gdy zostaną wprowadzone zmiany do obiektów metaverse, aparatu synchronizacji mogą wykonywać dowolne z następujących czynności w ramach procesu inicjowania obsługi administracyjnej:

- Tworzenie połączonych obiektów, gdzie jest połączony obiekt metaverse do nowo utworzonego Eksportowanie obiektu.
- Zmienianie nazwy obiektu połączonych.
- Odłączyć łącza między obiektem metaverse a przemieszczenia obiekty, tworzenie odłączonym obiektu.

Jeśli inicjowania obsługi administracyjnej wymaga aparat synchronizacji, aby utworzyć nowy obiekt łącznika, tymczasowy obiektu, z którą jest połączony obiekt metaverse jest zawsze eksportowania obiektu, ponieważ obiekt nie istnieje w źródle danych połączonego.

Jeśli inicjowania obsługi administracyjnej wymaga aparatu synchronizacji, aby rozłączyć połączonych obiektów, tworzenia obiektu odłączonym cofanie ubezpieczeń zostanie wywołana. Proces deprovisioning usuwa obiekt.

Podczas cofanie ubezpieczeń, usunięcie eksportowania obiektu nie fizycznie usuń go. Obiekt jest oznaczony jako **usunięte**, co oznacza, że operacja usuwania są umieszczane w obiekcie.

Eksportowanie przepływu atrybut występuje również podczas procesu synchronizacji ruchu wychodzącego, podobnie jak, że przepływ atrybut importowanie występuje podczas synchronizacji przychodzących. Eksportowanie przepływu atrybut odbywa się tylko między metaverse i eksportowanie obiektów, które są połączone.

### <a name="export-process"></a>Proces eksportowania
Podczas procesu eksportowania aparatu synchronizacji sprawdza, czy wszystkie obiekty eksportu, które są oznaczone jako oczekujące na eksport w obszarze łącznik, a następnie po zaakceptowaniu w źródle danych połączonego.

Aparat synchronizacji można określić powodzenia eksportu, ale wystarczająco nie może ustalić, o ukończeniu procesu zarządzania tożsamości. Obiekty w źródle danych połączonego zawsze mogą być zmieniane przez inne procesy. Ponieważ aparat synchronizacji nie ma stałe połączenie ze źródłem danych połączonych, nie jest wystarczające, aby wprowadzić założenia dotyczące właściwości obiektu w źródle danych połączonych opartych tylko na powiadomienie skutecznego eksportu.

Na przykład procesu w źródle danych połączonego może się zmienić atrybuty obiektu powrót do pierwotne wartości (czyli podłączonego źródła danych można zastąpić wartości natychmiast po danych jest przypisany przez aparat synchronizacji i pomyślnie zastosowane w źródle danych połączonego).

Sklepy aparatu synchronizacji eksportowanie oraz importowanie informacje dotyczące każdego obiektu tymczasowy stanu. Jeśli wartości atrybutów, które podano na liście dołączania atrybut zmieniły się od ostatniej operacji eksportowania, przechowywania importowanie i eksportowanie stanu umożliwia synchronizowanie aparat właściwie reagować. Aparat synchronizacji używa procesu importowania o potwierdzenie wartości atrybutu, wyeksportowane ze źródłem danych połączonych. Porównanie zaimportowanego i wyeksportowane informacje, jak pokazano na poniższej ilustracji umożliwia aparat synchronizacji, aby określić, czy eksportowania zakończyło się pomyślnie, lub jeśli musi być powtórzona.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Na przykład jeśli aparat synchronizacji eksportuje atrybut C, który ma wartość 5, ze źródłem danych połączonych, przechowuje C = 5, w pamięci stan eksportu. Każdy dodatkowe eksportowanie na wyników ten obiekt w próba ponownie wyeksportuj C = 5 podłączonego źródła danych, ponieważ aparat synchronizacji przyjęto założenie, że ta wartość nie trwale zastosowano do obiektu (to znaczy, chyba że inną wartość zaimportowano ostatnio ze źródła danych połączonych). Pamięć eksportu jest wyczyszczone, po odebraniu C = 5 podczas operacji importu obiektu.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
