<properties
   pageTitle="Lotus Domino łącznik | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować łącznik Domino Lotus firmy Microsoft."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Informacje techniczne programu Lotus Domino łącznika
Ten artykuł zawiera opis łącznika Lotus Domino. Artykuł dotyczy następujących produktów:

- Menedżer tożsamości 2016 (MIM2016)
- Produkt Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Należy użyć poprawki 4.1.3671.0 lub nowszy [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznik jest dostępna do pobrania z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Omówienie łącznika Lotus Domino
Łącznik programu Lotus Domino umożliwia Integracja usługi synchronizacji z serwera firmy IBM Lotus Domino.

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

Funkcja | Pomoc techniczna
--- | ---
Połączone źródło danych | Serwer: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klient:<li>Programu Lotus Notes 9.x</li>
Scenariusze | <li>Zarządzanie cyklu życia obiektu</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li>
Operacje | <li>Pełna i importowania różnicy</li><li>Eksportowanie</li><li>Ustawianie i zmienianie hasła na hasło HTTP</li>
Schematu | <li>Osoby (użytkownika mobilnego kontaktu (osoby nie certyfikatem))</li><li>Grupa</li><li>Zasobów (zasobów, pokoju spotkania Online)</li><li>Poczta w bazie danych</li><li>Dynamiczne odnajdowanie atrybutów dla obsługiwanych obiektów</li>

Łącznik Lotus Domino korzysta z klienta programu Lotus Notes można komunikować się z serwerem programu Lotus Domino. W wyniku tego współzależności obsługiwane klienta notatek programu Lotus musi być zainstalowany na serwerze synchronizacji. Komunikacji między klientem a serwerem jest zaimplementowana za pośrednictwem interfejsu programu Lotus Notes .NET Interop (Interop.domino.dll). Ten interfejs ułatwia komunikacji między platformy Microsoft.NET i klienta programu Lotus Notes i obsługuje dostęp do dokumentów programu Lotus Domino i widoków. Importowanie różnicy również jest możliwe, że (w zależności od metody importu zaznaczonego delta) jest używany interfejs macierzystym C++.

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że masz na serwerze synchronizacji z następujących czynności:

- Program Microsoft .NET 4.5.2 Framework lub nowszy
- Klient programu Lotus Notes musi być zainstalowana na serwerze synchronizacji
- Łącznik programu Lotus Domino wymaga domyślnej programu Lotus Domino LDAP schemat bazy danych (schema.nsf) do znajdować się na serwerze katalogu Domino. Jeśli nie jest dostępna, można ją zainstalować, uruchomiona lub ponowne uruchamianie usługi LDAP na serwerze Domino.

### <a name="connected-data-source-permissions"></a>Uprawnienia połączenia źródła danych
Wykonywanie zadań z obsługiwanych w Łącznik Lotus Domino, musi być członkiem grupy następujące czynności:

- Administratorzy pełnego dostępu
- Administratorzy
- Administratorzy baz danych

W poniższej tabeli przedstawiono uprawnienia, które są wymagane do każdej operacji:

Operacja | Prawa dostępu
--- | ---
Importowanie | <li>Czytanie dokumentów publicznych</li><li> Pełny dostęp administratora (Jeśli jesteś członkiem grupy administratorów pełny dostęp automatycznie masz skutecznego dostępu do w ACL.)</li>
Eksportowanie i ustawić hasło | Skutecznego dostępu: <li>Tworzenie dokumentów</li><li>Usuwanie dokumentów</li><li>Czytanie dokumentów publicznych</li><li>Pisanie dokumentów publicznych</li><li>Skopiowanymi lub kopiowanie dokumentów</li>Dla operacji eksportowania potrzebne są również następujących ról: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Bezpośrednie operacje i AdminP
Operacje albo przejdź bezpośrednio do katalogu Domino lub przez proces AdminP. Następujące tabele obsługiwane listy wszystkich obiektów, operacje i, jeśli to możliwe, metody implementacji pokrewne:

**Książka adresowa podstawowego**

Obiekt | Tworzenie | Aktualizacja | Usuwanie
--- | --- | --- | ---
Osoby | AdminP | Bezpośrednie | AdminP
Grupa | AdminP | Bezpośrednie | AdminP
MailInDB | Bezpośrednie | Bezpośrednie | Bezpośrednie
Zasób | AdminP | Bezpośrednie | AdminP

**Książka adresowa pomocniczej**

Obiekt | Tworzenie | Aktualizacja | Usuwanie
--- | --- | --- | ---
Osoby | N/D! | Bezpośrednie | Bezpośrednie
Grupa | Bezpośrednie | Bezpośrednie | Bezpośrednie
MailInDB | Bezpośrednie | Bezpośrednie | Bezpośrednie
Zasób | N/D! | N/D! | N/D!

Po utworzeniu zasobu jest tworzony dokument notatek. Podobnie po usunięciu zasobu dokumencie Notes zostanie usunięty.

### <a name="ports-and-protocols"></a>Porty i protokoły
IBM Lotus Notes klienta i serwery Domino komunikowanie się za pomocą notatek zdalnego procedury połączenie (NRPC) gdzie NRPC należy używać TCP/IP. Domyślny numer portu jest 1352, ale mogą być zmieniane przez administratora Domino.

### <a name="not-supported"></a>Brak obsługi
Następujące operacje nie są obsługiwane w bieżącej wersji łącznika Lotus Domino:

- Skrzynka pocztowa przechodzenie między serwerami.

## <a name="create-a-new-connector"></a>Tworzenie nowego łącznika

### <a name="client-software-installation-and-configuration"></a>Oprogramowanie instalacja i Konfiguracja klienta
Programu Lotus Notes musi być zainstalowana na serwerze **przed** łącznik jest zainstalowany.

Po zainstalowaniu, upewnij się, że możesz wykonać **Instalacji pojedynczego użytkownika**. Domyślnie **Instalowanie wielu użytkowników** nie działa.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stronie funkcje zainstalować tylko wymagane funkcje programu Lotus Notes i **Logowania jednokrotnego klienta**. Logowania jednokrotnego jest wymagana dla łącznika można było zalogować się do serwera Domino.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Uwaga:** Użytkownik, który znajduje się na tym samym serwerze jako konta, które jest używane jako konto usługi łącznika raz uruchom programu Lotus Notes. Ponadto upewnij się zamknąć klienta programu Lotus Notes na serwerze. To nie może być uruchomiona w tym samym czasie, które łącznik próbuje nawiązać połączenia z serwerem Domino.

### <a name="create-connector"></a>Tworzenie łącznika
Aby utworzyć łącznik Lotus Domino, w **Usługi synchronizacji** wybierz **Agent zarządzania** i **Tworzenie**. Zaznacz łącznik **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Jeśli Twojej wersji usługi synchronizacji oferuje możliwość konfigurowania **Architektura**, upewnij się, że łącznik jest ustawiona na wartość domyślną do uruchamiania w **procesie**.

### <a name="connectivity"></a>Łączność
Na stronie łączność możesz określić nazwę serwera Lotus Domino i wprowadź poświadczenia logowania.  
![Łączność](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Właściwości serwera Domino obsługuje dwa formaty nazwa serwera:

- Nazwa_serwera
- Nazwa_serwera DirectoryName

Format **Nazwa_serwera-DirectoryName** jest w formacie ten atrybut, ponieważ zapewnia szybszą odpowiedzią łącznik kontaktuje się z serwerem Domino.

Podany plik identyfikator użytkownika jest przechowywany w bazie danych konfiguracji usługi synchronizacji.

**Zmiana** importowania są są dostępne następujące opcje:

- **Brak**. Łącznik nie wszystkie operacje importowania różnicy.
- **Dodawanie aktualizacji**. Importowanie różnicy ma łącznik dodawania i aktualizowania operacji. Usuń operację **Pełnego importu** jest wymagane. .Net interop używa tej operacji.
- **Dodaj/Aktualizuj/Usuń**. Importowanie różnicy ma łącznik Dodawanie, aktualizowanie i usuwanie operacji. Operacja jest użycie macierzyste interfejsy C++.

W obszarze **Opcje schematu** masz następujące możliwości:

- **Domyślne schematu**. Łącznik wykrywa schematu z serwerem Domino. To jest domyślna opcja.
- **Schemat DSML**. Używane, jeśli serwer Domino nie są uwidaczniane schematu. Następnie można utworzyć plik DSML ze schematem i zaimportuj go zamiast tego. Aby uzyskać więcej informacji o DSML zobacz [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po kliknięciu przycisku Dalej parametry konfiguracji identyfikator użytkownika i hasło są sprawdzane.

### <a name="global-parameters"></a>Globalne parametry
Na stronie parametry globalne Konfigurowanie strefy czasowej i importowanie i eksportowanie opcji operacji.  
![Globalne parametry](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Strefa czasowa serwera Domino** parametr określa lokalizację serwera Domino.

Ta opcja konfiguracji jest wymagana do obsługi operacji **importowania różnicy** , ponieważ umożliwia synchronizację usługi Określ zmiany między dwie ostatnie.

#### <a name="import-settings-method"></a>Importuj ustawienia metody
**Wykonywanie pełnego importu przez** występują następujące opcje:

- Wyszukiwanie
- Widok (zalecane)

**Wyszukiwanie** jest przy użyciu indeksowania w Domino, ale często, że indeksy nie są aktualizowane w czasie rzeczywistym i dane zwrócone z serwera nie jest zawsze poprawny. System z wielu zmian ta opcja zazwyczaj nie działać prawidłowo i zawiera wartość FAŁSZ usuwa w niektórych sytuacjach. Jednak **wyszukiwania** jest większa niż **widoku**.

**Widok** jest zalecana opcja, ponieważ zapewnia on poprawnego stanu danych. Jest nieco mniejsza niż wyszukiwania.

#### <a name="creation-of-virtual-contact-objects"></a>Tworzenie wirtualnych obiektów kontaktów
**Umożliwiają tworzenie \_obiektu kontaktu** udostępnia następujące opcje:

- Brak
- Wartości-referencyjny
- Wykaz i -referencyjny wartości

W Domino atrybuty odwołanie może zawierać wiele różnych formatów, odwołując się do innych obiektów. Aby można było reprezentują różne odmiany, narzędzia Łącznik \_skontaktuj się z obiektów, nazywane także **Wirtualną kontaktów** (VC). Te obiekty są tworzone, można połączyć istniejące obiekty MV lub planowanego jako nowych obiektów. W ten sposób można zachowywać atrybut odwołania.

Po włączeniu tego ustawienia, a jeśli zawartość atrybutu odwołanie jest format nazwy domeny, \_, zostanie utworzony obiekt kontaktu. Na przykład atrybutem członka grupy może zawierać adresy SMTP. Użytkownik może również być nazwa_skrócona i inne atrybuty, które są zawarte w atrybuty odwołania. W tym scenariuszu wybierz **Wartości inne niż odwołania**. Ta konfiguracja jest najczęściej używane ustawienia dla implementacji Domino.

Po skonfigurowaniu Lotus Domino mają książki adresowe osobnych z innej nazwy wyróżniające reprezentujące tego samego obiektu, to można również utworzyć \_kontakt obiektów dla wszystkich wartości odwołanie, które znajdują się w książce adresowej. W tym scenariuszu wybierz opcję **Odwołanie i wartości inne niż odwołania** .

Jeśli masz wiele wartości atrybutu **Imię i nazwisko** w Domino, następnie chcesz również umożliwiają tworzenie wirtualnych kontaktów, może rozwiązać odwołania. Na przykład ten atrybut może mieć wiele wartości po małżeństwa lub rozwodu. Zaznacz pole wyboru Włącz **... Imię i nazwisko ma wiele wartości** w tym scenariuszu.

Łącząc poprawne atrybuty \_obiekty kontaktów może być dołączone do obiektu MV.

Te obiekty mają VC =\_kontakt dodany do ich nazwy domeny.

#### <a name="import-settings-conflict-object"></a>Importowanie ustawień, wystąpi konflikt obiektu
**Wykluczanie obiektu konflikt**

W dużych implementacji Domino jest możliwe, że samej nazwy domeny z powodu problemów z replikacją wiele obiektów. W tych przypadkach łącznik zobaczy dwa obiekty z różnych UniversalIDs, ale samej nazwy domeny. Ten konflikt spowoduje przejściowych tworzony w obszarze łącznik obiekt. Łącznik można zignorować obiektów, które zostały wybrane w Domino jako ofiar replikacji. Zaleca się pozostaw to pole wyboru zaznaczone.

#### <a name="export-settings"></a>Eksportowanie ustawień
Jeśli nie jest zaznaczona opcja **Użyj AdminP za aktualizowanie odwołań** , eksportowanie atrybutów odwołania, takich jak członka, bezpośrednich połączeń i nie korzysta z procesu AdminP. Użyj tej opcji tylko, gdy AdminP nie został skonfigurowany do obsługi więzy integralności.

#### <a name="routing-information"></a>Informacje o routingu
W Domino jest możliwe, że atrybut odwołanie zawiera informacje o routingu osadzony jako sufiks do nazwy domeny. Na przykład atrybut członka grupy mogą zawierać **CN=example/organization@ABC**. Sufiks @ABC informacji dotyczących routingu. Informacje o routingu służy Domino wysyłanie wiadomości e-mail do prawidłowy system Domino, który może być systemu w innej organizacji. W polu informacji o routingu można określić routing sufiksów używane w obrębie organizacji w zakresie łącznika. Jeśli jedną z następujących wartości zostanie znaleziony jako sufiks atrybut odwołania, informacje o routingu zostanie usunięta z odwołania. Jeśli nie można dopasować routingu sufiks na wartość odwołanie do jednej z tych określonej wartości, \_, zostanie utworzony obiekt kontaktu. Te \_obiekty kontaktów są tworzone przy użyciu ** RO=@ ** wstawione do nazwy domeny. Dla tych \_kontakt obiektów następujące atrybuty są także dodawane umożliwia dołączanie do rzeczywistego obiektu w razie potrzeby: \_routingName, \_przedstawiciel, \_displayName i UniversalID.

#### <a name="additional-address-books"></a>Dodatkowe książki adresowe
Jeśli nie masz **książce telefonicznej** zainstalowany, zawierająca nazwę książki adresowe pomocniczej, można ręcznie wprowadzić te książki adresowej.

#### <a name="multivalued-transformation"></a>Transformacja wielowartościowe
Wiele w Lotus Domino atrybuty wielowartościowe. Odpowiednie atrybuty metaverse są zwykle pojedynczej wartości. Konfigurując importowanie i eksportowanie opcja operacji, należy włączyć łącznik ułatwiające wymagane tłumaczenie atrybutów.

**Eksportowanie**  
Opcja operacji eksportowania obsługuje dwa tryby:

- Dołączanie elementu
- Zamienianie elementu

**Zastąp element** — po wybraniu tej opcji łącznik zawsze usunąć bieżącej wartości atrybutu Domino i zastąpić je podanych wartości. Dołączonym zwracające może być pojedynczej wartości lub wiele wartości.

Przykład: Atrybut Asystenta obiektu osoby występują następujące wartości:

- CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Jeśli nowego Asystenta o nazwie **Alexandrowi David** została przypisana do tego obiektu osoby, wynikiem funkcji jest:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Dołączanie elementu** — po wybraniu tej opcji łącznik zachowuje istniejących wartości atrybutu w Domino i Wstaw nowe wartości u góry listy danych.

Przykład: Atrybut Asystenta obiektu osoby występują następujące wartości:

- CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Jeśli nowego Asystenta o nazwie **Alexandrowi David** została przypisana do tego obiektu osoby, wynikiem funkcji jest:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importowanie**  
Opcja operacji importu obsługuje dwa tryby:

- Domyślne
- Pojedyncza wartość wielowartościowego

**Domyślne** — po wybraniu opcji domyślnych, wszystkie wartości wszystkich atrybutów są importowane.

**Multivalued pojedyncza wartość** — po wybraniu tej opcji wielowartościowy atrybut jest konwertowany na atrybutem pojedynczej wartości. Jeśli istnieje więcej niż jedną wartość, zostanie użyta wartość na początku (Ta wartość zazwyczaj jest również r).

Przykład: Atrybut Asystenta obiektu osoby występują następujące wartości:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
- CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ostatnia aktualizacja do tego atrybutu jest **Alexandrowi David**. Ponieważ opcja operacji importu jest ustawiona na Multivalued pojedyncza wartość, łącznik tylko importuje **Alexandrowi David** do obszaru łącznik.

Logika Aby skonwertować atrybuty wielowartościowe atrybuty pojedynczej wartości nie dotyczy atrybut członka grupy i atrybut imię i nazwisko osoby.

Możliwe także skonfigurować importowanie i eksportowanie reguł transformacja wielowartościowego atrybutów na atrybut, jako wyjątek reguły globalnej. Aby skonfigurować tę opcję, wprowadź [Typ obiektu]. [Nazwa_atrybutu] w polach tekstowych **Lista wykluczeń atrybutów importowanie** i **Eksportowanie listy wykluczeń atrybutów** . Na przykład jeśli wprowadzisz Person.Assistant i flaga global jest ustawiona do zaimportowania wszystkie wartości, dla Asystenta zostanie zaimportowana tylko pierwszą wartość.

#### <a name="certifiers"></a>Wydających świadectwa
Wszystkie jednostki organizacji i organizacji są wyświetlane według łącznik. Aby można było wyeksportować obiekty osobę do książki adresowej podstawowego, wymagane jest certifier z jej hasło.

Jeśli wszystkie wydających świadectwa tego samego hasła, można używać **hasła dla wszystkich Certifers** . Następnie można wprowadzić w tym miejscu i określić tylko plik certifier.

Jeśli tylko zaimportujesz, nie musisz określić dowolną wydających świadectwa.

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie obsługi administracyjnej hierarchii
Po skonfigurowaniu Łącznik Lotus Domino pominąć tę stronę okno dialogowe. Łącznik Lotus Domino nie obsługuje hierarchii inicjowania obsługi administracyjnej.  
![Obsługa administracyjna hierarchii](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurowanie partycje i hierarchie
Po skonfigurowaniu partycje i hierarchie, należy wybrać książkę adresową podstawowego o nazwie NAB=names.nsf. Oprócz książkę adresową podstawowego można wybrać książki adresowe pomocniczej, jeśli istnieją.  
![Partycje](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Zaznacz pole wyboru atrybuty
Po skonfigurowaniu usługi atrybuty po zaznaczeniu wszystkich atrybutów, które są prefiksem ** \_MMS\_**. Te atrybuty są wymagane, gdy dodawać nowe obiekty mają Lotus Domino

![Atrybuty](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Zarządzanie cyklu życia obiektu
W tej sekcji omówiono różne obiekty Domino.

### <a name="person-objects"></a>Obiekty osoby
Obiekt osoby reprezentuje użytkowników w organizacji i jednostkami organizacyjnymi. Oprócz domyślnych atrybutów Domino administrator może dodać atrybutami niestandardowymi do obiektu osoby. Co najmniej obiekt osoby muszą zawierać wszystkie wymagane atrybuty. Aby uzyskać pełną listę atrybuty obowiązkowe zobacz [Programu Lotus Notes właściwości](#lotus-notes-properties). Aby zarejestrować obiektu osoby, muszą być spełnione następujące wymagania:

- Książka adresowa (names.nsf) muszą zdefiniowano i powinna być książkę adresową podstawowego.
- Musi być certifier O-OU Id i hasło, aby zarejestrować określonego użytkownika w organizacji i jednostka organizacyjna.
- Należy ustawić określonego zestawu właściwości programu Lotus Notes dla obiektu osoby. Poniższe właściwości są używane do inicjowania obsługi administracyjnej obiektu osoby. Aby uzyskać więcej informacji zobacz sekcję o nazwie [Programu Lotus Notes właściwości](#lotus-notes-properties) w dalszej części tego dokumentu.
- Początkowe hasło HTTP dla osoby jest atrybut i ustaw podczas inicjowania obsługi administracyjnej.
- Obiekt osoby musi być jedną z następujących trzech obsługiwanych typów:
    1. Normalny użytkownik, który ma pliku poczty i pliku identyfikator użytkownika
    2. Mobilnego dostępu do ustawień użytkownika (normalny użytkownika, zawierającego wszystkie mobilnego pliki bazy danych)
    3. Kontakty (użytkownika z pliku nie identyfikatora)

Osób (z wyjątkiem kontakty) można podzielić na użytkowników z NAMI i międzynarodowe użytkowników dodatkowo określone przez wartość \_MMS\_IDRegType właściwości. Użyj tych osób klienta notatek do uzyskania dostępu do serwerów Lotus Domino mieć identyfikator notatek i dokumentów osoby. Jeśli używają notatki poczty, następnie mają także pliku poczty. Użytkownik musi być zarejestrowany staje się aktywny. Aby uzyskać więcej informacji zobacz:

- [Konfigurowanie użytkowników notatek](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Rejestracja użytkownika](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Zarządzanie użytkownikami](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Zmienianie nazw użytkowników](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Te operacje są wykonywane w Lotus Domino, a następnie zaimportowane do usługi synchronizacji.

### <a name="resources-and-rooms"></a>Zasoby i pomieszczeń
Zasób jest innego typu z bazą danych programu Lotus Domino. Zasoby można sal konferencyjnych z różnymi typami urządzeń, takich jak projektory. Istnieją podtypy zasobów obsługiwanych przez łącznik Lotus Domino, które są definiowane przez atrybut Typ zasobu:

Typ zasobu | Atrybut Typ zasobu
--- | ---
Pokój | 1
Zasobów (inne) | 2
Spotkanie online | 3

Dla tego typu obiektu zasobu do pracy Oto wymagane:

- Bazie danych rezerwacji zasobów powinna już istnieje w połączonych serwera Domino
- Witryna jest już zdefiniowany dla tego zasobu

Baza danych rezerwacji zasobów zawiera trzy typy dokumentów:

- Profil witryny
- Zasób
- Rezerwacja

Aby uzyskać więcej informacji o konfigurowaniu rezerwacji zasobów bazy danych zobacz [Konfigurowanie bazy danych rezerwacji zasobów](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Tworzenie, aktualizowanie i usuwanie zasobów**  
Tworzenie, aktualizowanie i usuwanie operacji wykonywanych przez łącznik Lotus Domino w bazie danych rezerwacji zasobów. Zasoby są tworzone jako dokumenty w Names.nsf (oznacza to, że książka adresowa podstawowego). Aby uzyskać więcej informacji na temat edytowania i usuwania zasobów zobacz [Edytowanie i usuwanie dokumentów zasobu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importowanie i eksportowanie operacji dla zasobów**  
Zasoby można zaimportować do i wyeksportowanego z usługi synchronizacji, takiej jak inny typ obiektu. Wybierz typ obiektu jako zasób podczas konfiguracji. Aby operacji eksportowania pomyślnie powinien zawierać szczegóły dotyczące typ zasobu, konferencji bazy danych i nazwa witryny.

### <a name="mail-in-databases"></a>Poczta w bazach danych
Baza danych poczty elektronicznej w jest bazy danych, która ma odbierać wiadomości. Jest skrzynki pocztowej programu Lotus Domino, która nie jest przypisana do dowolnego określonego konta użytkownika Lotus Domino (oznacza to, że nie ma własny plik ID i hasło). Bazy danych poczty elektronicznej w ma unikatowy identyfikator użytkownika ("krótki nazwa") z nią związane oraz własny adres e-mail.

Jeśli zachodzi potrzeba osobnej skrzynki pocztowej swój własny adres e-mail, który może być udostępniana innym użytkownikom (na przykład group@contoso.com), poczty elektronicznej w baza danych została utworzona. Dostęp do tej skrzynki pocztowej jest kontrolowane przez jego listy kontroli dostępu (ACL), która zawiera nazwy użytkowników notatki, które można otworzyć skrzynki pocztowej.

Aby uzyskać listę wymaganych atrybutów zobacz sekcję o nazwie [Wymagane atrybuty](#mandatory-attributes) w dalszej części tego artykułu.

Gdy bazy danych służy do odbierania wiadomości e-mail, dokumentów poczty elektronicznej w bazie danych zostanie utworzona w Lotus Domino. Ten dokument, musi istnieć w katalogu Domino każdy serwer, na którym jest przechowywana kopia bazy danych. Aby uzyskać szczegółowe informacje o tworzeniu dokumentu korespondencji w bazie danych zobacz [Tworzenie dokumentu w bazie danych](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Przed utworzeniem bazy danych poczty elektronicznej w, baza danych powinna już istnieje (powinna być utworzone przez administratora programu Lotus) na serwerze Domino.

### <a name="group-management"></a>Zarządzanie grupami
Szczegółowe omówienie zarządzania grupy Lotus Domino można uzyskać od następujących zasobów:

- [Korzystanie z grup](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Tworzenie grupy](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Tworzenie i modyfikowanie grupy](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Zarządzanie grupami](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Zmienianie nazwy grupy](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Zarządzanie hasłami
Zarejestrowany użytkownika Lotus Domino istnieją dwa typy haseł:

1. Hasło użytkownika (przechowywane w pliku User.id)
2. Internetowe i hasła HTTP

Łącznik Lotus Domino obsługuje tylko operacje przy użyciu protokołu HTTP hasła.

Aby wykonać zarządzania hasłami, należy włączyć zarządzanie hasłami do łącznika w Projektancie agenta zarządzania. Aby włączyć zarządzanie hasłami, zaznacz opcję **Włącz zarządzanie hasłami** na stronie okno dialogowe **Konfigurowanie rozszerzenia** .  
![Konfigurowanie rozszerzeń](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Obsługa Łącznik Lotus Domino po operacji na Internet hasła:

- Ustawienie hasła: Ustaw hasło Ustawia nowe hasło protokołu HTTP/Internet na użytkownika w Domino. Domyślne konto jest również odblokowane. Flaga unlock są wyświetlane w interfejsie WMI aparatu synchronizacji.
- Zmienianie hasła: W tym scenariuszu użytkownik może być konieczna zmiana hasła lub zostanie wyświetlony monit o Zmienianie hasła po upływie określonego czasu. Dla tej operacji podjęcie umieścić, zarówno (stare i nowe hasło) są wymagane. Po zmianie, nowe hasło jest aktualizowana w Lotus Domino.

Aby uzyskać więcej informacji zobacz:

- [Korzystanie z funkcji blokowania Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Zarządzanie hasła internetowe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Dodatkowe informacje
W tej sekcji przedstawiono przykład opisy atrybutów i atrybut wymagania dotyczące Łącznik Lotus Domino.

### <a name="lotus-notes-properties"></a>Właściwości programu Lotus Notes
Jeśli osoba obiekty są przystosowane do katalogu Lotus Domino, obiekty musi mieć określonego zestawu właściwości z określonymi wartościami wypełnione. Te wartości jest wymagana tylko w przypadku tworzenia operacji.

W poniższej tabeli przedstawiono te właściwości i zawiera ich opis.

Właściwość | Opis
--- | ---
\_MMS_AltFullName | Alternatywny pełną nazwę użytkownika.
\_MMS_AltFullNameLanguage | Język, który ma być używany dla określanie alternatywnego pełną nazwę użytkownika.
\_MMS_CertDaysToExpire | Liczba dni od daty bieżącej przed certyfikat wygaśnie. Jeśli nie zostanie określony, datą domyślną jest dwa lata od daty bieżącej.
\_MMS_Certifier | Właściwość, która zawiera nazwę hierarchii organizacyjnej certifier. Na przykład: OU = OrganizationUnit, O = Org, C = kraju.
\_MMS_IDPath | Jeśli właściwość jest pusta, plik identyfikator użytkownika nie zostanie utworzony lokalnie na serwerze synchronizacji. Jeśli właściwość zawiera nazwę pliku, plik identyfikator użytkownika zostanie utworzony w folderze madata. Właściwość może również zawierać pełną ścieżkę.
\_MMS_IDRegType | Osoby można zaklasyfikować jako kontaktów, nam użytkowników i międzynarodowe użytkowników. W poniższej tabeli przedstawiono możliwe wartości: <li>0 - kontaktu</li><li>1 - US użytkownika</li><li>2 - międzynarodowe użytkownika</li>
\_MMS_IDStoreType | Właściwość wymagane dla Stanów Zjednoczonych i międzynarodowe użytkowników. Właściwość zawiera wartość całkowitą, która określa, czy identyfikator użytkownika są przechowywane jako załącznik w książce adresowej notatki lub w pliku poczty danej osoby. Jeśli plik identyfikator użytkownika jest załącznika w książce adresowej, opcjonalnie można go utworzyć jako plik z \_MMS_IDPath. <li>Opróżnij - pliku identyfikator magazynu w identyfikator magazynu, plik nie identyfikacyjne (używane do kontaktów).</li><li> 1 - załącznik w książce adresowej notatek. \_Należy ustawić właściwość MMS_Password dla plików identyfikator użytkownika, które są załączniki</li><li>2 - identyfikator magazynu w pliku poczty danej osoby. \_MMS_UseAdminP musi być ustawiona na wartość FAŁSZ, aby umożliwić plik poczty, można utworzyć podczas rejestrowania osoby. \_Należy ustawić właściwość MMS_Password dla plików, identyfikator użytkownika.</li>
\_MMS_MailQuotaSizeLimit | Liczba megabajtów przeznaczonych plik bazy danych wiadomości e-mail.
\_MMS_MailQuotaWarningThreshold | Liczba megabajtów przeznaczonych plik bazy danych poczty e-mail przed jest wyświetlane ostrzeżenie.
\_MMS_MailTemplateName | Plik szablonu wiadomości e-mail jest używany do tworzenia pliku e-mail użytkownika. Jeśli szablon jest określony, plik poczty zostanie utworzony przy użyciu określonego szablonu. Jeśli żaden z szablonów jest określony, plik szablonu domyślnego służy do tworzenia pliku.
\_MMS_OU | Opcjonalnie właściwość, która jest nazwą OU w obszarze certifier. Ta właściwość powinna być pusta dla kontaktów.
\_MMS_Password | Właściwość wymagane dla użytkowników. Ta właściwość zawiera hasło do pliku identyfikator obiektu.
\_MMS_UseAdminP | Właściwość powinna być równa PRAWDA, jeśli plik poczty powinien zostać utworzony w procesie AdminP na serwerze Domino (asynchroniczne w procesie eksportowania). Jeśli właściwość jest ustawiona na wartość FAŁSZ, plik poczty jest tworzone przez Domino użytkownika (obraz w procesie eksportowania).

Dla użytkownika z plikiem identyfikacyjny skojarzonego \_właściwość MMS_Password musi zawierać wartość. Aby uzyskać dostęp do poczty elektronicznej za pośrednictwem klienta programu Lotus Notes właściwości MailServer i MailFile użytkownika musi zawierać wartość.

Aby uzyskać dostęp do poczty e-mail za pomocą przeglądarki sieci Web, następujące właściwości musi zawierać wartości:

- MailFile — właściwość wymagane, zawierający ścieżkę na serwerze Lotus Domino, w której jest przechowywany plik poczty.
- MailServer — właściwość wymagane, która zawiera nazwę serwera Lotus Domino. Ta wartość jest nazwę będzie używany podczas tworzenia plików programu Lotus poczty na serwerze Domino.
- HTTPPassword — opcjonalne właściwości, która zawiera hasła dostępu do sieci Web dla obiektu.

Aby uzyskać dostęp do serwera Domino bez możliwości poczty, właściwość HTTPPassword musi zawierać wartość. Właściwości MailFile i MailServer mogą być puste.

Z \_MMS_ IDStoreType = 2 (id magazynu w pliku poczty), właściwość MailSystem NotesRegistrationclass jest równa REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Atrybuty obowiązkowe
Łącznik Lotus Domino głównie obsługuje następujące typy obiektów (typów dokumentów):

- Grupa
- Poczta w bazie danych
- Osoby
- Kontaktu (osoby z nie certifier)
- Zasób

W tej sekcji przedstawiono atrybuty, które są wymagane dla każdego obsługiwanego obiekt do wyeksportowania się z serwerem Domino.

Typ obiektu | Atrybuty obowiązkowe
--- | ---
Grupa | <li>Zdefiniowana</li>
Główny w bazie danych | <li>Imię i nazwisko</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Osoby | <li>Nazwisko</li><li>MailFile</li><li>Nazwa_skrócona</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Kontaktu (osoby z nie certifier) | <li>\_MMS_IDRegType</li>
Zasób | <li>Imię i nazwisko</li><li>Typu zasobu</li><li>ConfDB</li><li>Dyspozycyjność zasobów</li><li>Witryny</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Typowe problemy i pytania

### <a name="schema-detection-does-not-work"></a>Wykrywanie schematu nie działa
Aby można było wykrywanie schematu, jest to konieczne, że plik schema.nsf znajduje się na serwerze Domino. Ten plik jest wyświetlana tylko, jeśli na serwerze LDAP jest zainstalowany. Jeśli schematu nie jest widoczne, sprawdź następujące elementy:

- Schema.nsf plik znajduje się w folderze głównym serwera Domino
- Użytkownik ma uprawnienia do wyświetlania pliku schema.nsf.
- Wymusić ponowne uruchomienie serwera LDAP. Otwieranie **Programu Lotus Domino konsoli** i użyj polecenia **Powiedz ReloadSchema LDAP** , aby ponownie załadować schemat.

### <a name="not-all-secondary-address-books-are-visible"></a>Nie wszystkie książki adresowe pomocniczy są widoczne
Łącznik Domino zależy od funkcji **Książce telefonicznej** będą mogły znaleźć książki adresowe pomocniczą. Jeśli brakuje książki adresowe pomocniczej, sprawdź, jeśli [Książce telefonicznej](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) zostały włączone i skonfigurowane na serwerze Domino.

### <a name="custom-attributes-in-domino"></a>Atrybuty niestandardowe w Domino
Istnieje kilka sposobów w Domino, aby rozszerzyć schemat — będą wyświetlane jako atrybut niestandardowy eksploatacyjnych przez łącznik.

**Metoda 1: Rozszerzanie schematu Lotus Domino**

1. Tworzenie kopii szablonu katalogu Domino {PUBNAMES. NTF} przez następujące [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (należy nie dostosować katalogu IBM Lotus Domino domyślnego szablonu):
2. Otwórz szablon katalogu kopii Domino {CONTOSO. Szablon NTF}, który został utworzony w Projektancie Domino i wykonaj następujące czynności:
    - Kliknij przycisk Elementy udostępnione i rozwijanie podformularzy
    - Kliknij dwukrotnie podformularza InheritableSchema ${nazwa_obiektu} (gdzie {nazwa_obiektu} jest nazwą domyślnej klasy strukturalne obiektu, na przykład: osoby).
    - Nazwa atrybutu, który chcesz dodać do schematu {MyPersonAtrribute} i odpowiadający mu do tego atrybutu. Tworzenie pola, wybierz pozycję Menu **Utwórz** , a następnie wybierz **pole** z menu.
    - W polu dodano ustaw jego właściwości, wybierając jego typ, styl, rozmiar, czcionki i inne pokrewne parametry w oknie właściwości pola.
    - Zachowaj atrybut, wartością domyślną takie same jak nazwa nadana dla tego atrybutu (na przykład jeśli nazwa atrybutu jest MyPersonAttribute, pozostaw wartość domyślna o takiej samej nazwie).
    - Zapisz podformularza InheritableSchema ${nazwa_obiektu} zaktualizowane wartości.
3. Zamień szablon katalogu Domino {PUBNAMES. NTF} nowy szablon niestandardowe {CONTOSO. NTF} przez następujące [następujące kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zamknij Domino administrator i Otwórz konsolę Domino, aby ponownie uruchomić usługi LDAP i ponownie załadować schemat LDAP:
    - W konsoli Domino Wstawianie polecenia w obszarze tekst **Polecenia Domino** zachowano ponowne uruchomienie usługi LDAP - [Ponowne uruchamianie LDAP zadania]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    - Aby ponownie załadować LDAP schematu użyj polecenia określić LDAP - powiedz ReloadSchema LDAP
5. Otwórz administratora Domino i wybierz kartę osoby i grupy, aby sprawdzić, dodane atrybuty jest uwzględniany na domino Dodaj osobę.
6. Otwiera Schema.nsf na karcie **pliki** i dodane atrybuty są uwzględniane w dominoPerson klasy obiektu LDAP.

**Metoda 2: Utwórz auxClass z atrybutem niestandardowych i skojarzyć z klasą obiektu**

1. Tworzenie kopii szablonu katalogu Domino {PUBNAMES. NTF} przez następujące [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nigdy nie Dostosowywanie szablonu domyślnego IBM Lotus Domino katalogu):
2. Otwórz szablon katalogu kopii Domino {CONTOSO. Szablon NTF}, który został utworzony w Projektancie Domino.
3. W okienku po lewej stronie wybierz udostępniony kod, a następnie podformularzy.
4. Kliknij przycisk nowy podformularz
5. Wykonaj poniższe czynności, aby określić właściwości dla nowej podformularza:
    - Nowy podformularz jest otwarty, wybierz projekt — właściwości podformularza
    - Obok pozycji Właściwość Name wprowadź nazwę dla klasy pomocnicze obiektów — na przykład TestSubform.
    - Zachowywanie właściwości opcji "Dołącz do podformularza Wstawianie... okno dialogowe" zaznaczone
    - Usuń zaznaczenie opcji właściwości "renderowania przebiegu za pośrednictwem HTML w notatkach."
    - Inne właściwości pozostaną bez zmian, a następnie zamknij okno właściwości podformularza.
    - Zapisz i zamknij nowy podformularz.
6. Wykonaj poniższe czynności, aby dodać pole w celu zdefiniowania pomocniczy obiektu klasy:
    - Otwórz podformularza, który został utworzony.
    - Wybierz pozycję Tworzenie — pól.
    - Obok nazwy na karcie podstawowe w oknie dialogowym pole, określić dowolną nazwę, na przykład: {MyPersonTestAttribute}.
    - W polu dodano ustaw jego właściwości, wybierając typ, styl, rozmiar, czcionki i powiązanych właściwości.
    - Zachowaj atrybut, wartością domyślną takie same jak nazwa nadana dla tego atrybutu (na przykład jeśli nazwa atrybutu jest MyPersonTestAttribute, pozostaw wartość domyślna o takiej samej nazwie).
    - Zapisz podformularza zaktualizowane wartości i wykonaj następujące czynności:
        - W okienku po lewej stronie wybierz udostępniony kod, a następnie podformularzy
        - Wybierz nowy podformularz, a następnie wybierz projekt — właściwości projektu.
        - Kliknij kartę trzeci od lewej, a następnie wybierz pozycję **Zmień propagowanie zakaz projektu**.
7. Otwórz podformularzu ExtensibleSchema ${nazwa_obiektu} (gdzie {nazwa_obiektu} jest nazwą domyślnej klasy strukturalne obiektu, na przykład — osoby).
8. Wstaw zasób i wybierz pozycję podformularza (utworzone przez Ciebie, na przykład — TestSubform) i zapisywanie podformularza ExtensibleSchema ${nazwa_obiektu}.
9. Zamień szablon katalogu Domino {PUBNAMES. NTF} nowy szablon niestandardowe {CONTOSO. NTF} przez następujące [następujące kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zamknij Domino administrator i Otwórz konsolę Domino, aby ponownie uruchomić usługi LDAP i ponownie załadować schemat LDAP:
    - W konsoli Domino Wstawianie polecenia w obszarze tekst **Polecenia Domino** zachowano ponowne uruchomienie usługi LDAP - [Ponowne uruchamianie LDAP zadania](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    - Aby ponownie załadować LDAP schematu użyj polecenia określić LDAP **Powiedz ReloadSchema LDAP**.
11. Otwórz Domino administrator, a następnie wybierz kartę osoby i grupy, aby zobaczyć dodane atrybuty jest uwzględniany na domino Dodaj osoby (w obszarze inne tabulacji).
12. Otwiera Schema.nsf na karcie **pliki** i dodane atrybuty jest widoczny w obszarze TestSubform LDAP pomocnicze klasy obiektu.

**Metoda 3: Dodawanie atrybutu niestandardowego do klasy ExtensibleObject**

1. Otwórz plik {Schema.nsf} umieszczana w katalogu głównym
2. Wybierz klasy obiektów LDAP z menu po lewej stronie w obszarze **Wszystkie dokumenty schematu** i kliknij przycisk **Dodaj obiekt klasy** :
3. Podaj nazwę LDAP w postaci {zzzExtensibleSchema} (gdzie zzz jest nazwą domyślnej klasy strukturalne obiektu, na przykład osoby). Na przykład aby rozszerzyć schematu dla osoby klasy obiektu, należy podać nazwę LDAP {PersonExtensibleSchema}.
4. Podaj nazwę klasy obiekt nadrzędny, dla którego chcesz wydłużyć w schemacie. Na przykład aby rozszerzyć schematu klasy obiektów osobie, wprowadź nazwy klasa przełożony obiektu {dominoPerson}:
5. Podaj prawidłowy identyfikator OID odpowiadające klasę obiektu.
6. Wybierz atrybuty rozszerzone i niestandardowe w obszarze pola obowiązkowe i opcjonalne typy atrybut zgodnie z wymaganiami:
7. Po dodaniu wymagane atrybuty do ExtensibleObjectClass, kliknij przycisk **Zapisz i zamknij**.
8. ExtensibleObjectClass zostanie utworzona dla odpowiednich domyślne klasy obiektów z atrybuty rozszerzone.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

-   Aby uzyskać informacje dotyczące włączania rejestrowania rozwiązywać łącznik zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
