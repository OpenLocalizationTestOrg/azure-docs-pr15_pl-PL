<properties
   pageTitle="Łącznik ogólnego LDAP | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować łącznik LDAP ogólnego firmy Microsoft."
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

# <a name="generic-ldap-connector-technical-reference"></a>Ogólne informacje techniczne łącznik LDAP
Ten artykuł zawiera opis ogólnego łącznik LDAP. Artykuł dotyczy następujących produktów:

- Menedżer tożsamości 2016 (MIM2016)
- Produkt Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Należy użyć poprawki 4.1.3671.0 lub nowszy [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznik jest dostępna do pobrania z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

W odniesieniu do IETF RFC tego dokumentu przy użyciu formatu (RFC [RFC liczba] / [sekcji w dokumencie RFC]), na przykład (RFC 4512-4.3).
Więcej informacji można znaleźć w http://tools.ietf.org/html/rfc4500 (należy zastąpić prawidłowy numer RFC 4500).

## <a name="overview-of-the-generic-ldap-connector"></a>Omówienie łącznik ogólnego LDAP
Rodzajowy łącznik LDAP umożliwia Integracja usługi synchronizacji z serwera LDAP v3.

Niektóre działania i elementy schematu, takie jak kontakty potrzebne do importowania różnicy, nie zostaną określone w dokumentach RFC IETF. Dla tych operacji są obsługiwane tylko katalogów LDAP jawnie określony.

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

Funkcja | Pomoc techniczna
--- | --- |
Połączone źródło danych | Łącznik jest obsługiwana przez wszystkie serwery v3 LDAP (zgodne z 4510 RFC). Przetestowano go z następujących czynności: <li>Usługi LDAP Microsoft Active Directory (AD LDS)</li><li>Microsoft Active Directory globalnej wykazem AD</li><li>Serwer 389 katalogu</li><li>Serwer Apache katalogu</li><li>IBM Tivoli DS</li><li>Katalog Isode</li><li>NetIQ eDirectory</li><li>Serwer katalogowy Novell eDirectory</li><li>Otwieranie DJ</li><li>Otwieranie usługi Katalogowej</li><li>Otwieranie LDAP (openldap.org)</li><li>Oracle (wcześniej n) katalogu Server Enterprise Edition</li><li>Serwera wirtualnego katalogu RadiantOne (VDS)</li><li>Serwer katalogu n z nich</li>**Nieobsługiwane godne uwagi katalogów:** <li>Microsoft usługach domenowych Active Directory (AD DS) [zamiast tego użyj wbudowanej łącznika usługi Active Directory]</li><li>Katalog internetowy Oracle (OID)</li>
Scenariusze   | <li>Zarządzanie cyklu życia obiektu</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li>
Operacje |W katalogów LDAP są obsługiwane następujące operacje: <li>Pełne importowanie</li><li>Eksportowanie</li>W określone katalogi tylko są obsługiwane następujące operacje:<li>Importowanie różnicy</li><li>Ustawianie hasła, zmienianie hasła</li>
Schematu | <li>Schemat zostanie wykryta ze schematu LDAP (RFC3673 i RFC4512-4.2)</li><li>Obsługa klas strukturalnych, aux klas i klasy obiektu extensibleObject (RFC4512-4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Obsługa zarządzania różnicy importu i hasła
Obsługiwane katalogów dla różnicy i zarządzanie hasłami:

- Usługi LDAP Microsoft Active Directory (AD LDS)
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawienie hasła
- Microsoft Active Directory globalnej wykazem AD
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło
- Serwer 389 katalogu
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła
- Serwer Apache katalogu
    - Nie obsługuje importowania różnicy, ponieważ ten katalog ma dziennik trwałych zmian
    - Obsługa ustawić hasło
- IBM Tivoli DS
    - Obsługuje wszystkie operacje importowania delta
    - Obsługa ustawić hasło i zmienianie hasła
- Katalog Isode
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła
- Serwer katalogowy Novell eDirectory i NetIQ eDirectory
    - Obsługuje dodawanie, aktualizowanie i Zmień nazwę operacje importowania różnicy
    - Nie obsługuje operacji usuwania różnicy importowania
    - Obsługa ustawić hasło i zmienianie hasła
- Otwieranie DJ
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła
- Otwieranie usługi Katalogowej
    - Obsługuje wszystkie operacje importowania delta
    - Obsługa ustawić hasło i zmienianie hasła
- Otwieranie LDAP (openldap.org)
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło
    - Nie obsługuje zmienianie hasła
- Oracle (wcześniej n) katalogu Server Enterprise Edition
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła
- Serwera wirtualnego katalogu RadiantOne (VDS)
    - Musi być używana wersja 7.1.1 lub nowszy
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła
-  Serwer katalogu n z nich
    - Obsługuje wszystkie operacje importowania różnicy
    - Obsługa ustawić hasło i zmienianie hasła

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że masz na serwerze synchronizacji z następujących czynności:

- Program Microsoft .NET 4.5.2 Framework lub nowszy

### <a name="detecting-the-ldap-server"></a>Wykrywanie serwera LDAP
Łącznik opiera się różnymi sposobami ich i wykrywanie serwera LDAP. Łącznik jest używana wersja Nazwa dostawcy, DSE główny, a jego sprawdza schematu, aby znaleźć unikatowe obiekty i atrybuty w niektórych serwerach LDAP występują znane. Te dane, jeśli znaleziono, będzie używany do wypełniania wstępnie opcje konfiguracji w łącznik.

### <a name="connected-data-source-permissions"></a>Uprawnienia połączenia źródła danych
Do importowania i eksportowania operacje na obiektach w katalogu połączonego, konto łącznik musi mieć wystarczających uprawnień. Łącznik musi zapisu, aby można było wyeksportować i Odczyt, aby można było zaimportować. Konfiguracja uprawnień odbywa się w obrębie środowiska zarządzania katalogu docelowej się.

### <a name="ports-and-protocols"></a>Porty i protokoły
Łącznik używa numer portu określony w konfiguracji, która domyślnie jest 389 dla protokołu LDAP i 636 dla LDAPS.

Dla LDAPS należy użyć SSL 3.0 i TLS. SSL 2.0 nie jest obsługiwane i nie można aktywować.

### <a name="required-controls-and-features"></a>Wymagane formanty i funkcje
Następujące LDAP formanty i funkcje muszą być dostępne na serwerze LDAP łącznik, aby działał poprawnie:  
`1.3.6.1.4.1.4203.1.5.3`Filtry PRAWDA i FAŁSZ

Filtr PRAWDA i FAŁSZ często jest nie zgłoszone jako obsługiwane przez katalogów LDAP i mogą być wyświetlane na **Globalnej strony** w obszarze **Obowiązkowe funkcji nie można odnaleźć**. Służy do tworzenia **lub** filtrów w kwerendach LDAP, na przykład podczas importowania wielu typów obiektów. Czy można importować więcej niż jeden typ obiektu, serwera LDAP obsługuje tę funkcję.

Jeśli korzystasz z katalogu Unikatowy identyfikator w przypadku zakotwiczenia następujących czynności należy również dostępna (zobacz sekcja [Konfigurowanie zakotwiczenia](#configure-anchors) w dalszej części tego artykułu, aby uzyskać więcej informacji):  
`1.3.6.1.4.1.4203.1.5.1`Wszystkie atrybuty operacyjne

Jeśli katalog ma więcej obiektów niż co można umieścić w jednym połączenia do katalogu, następnie zaleca się używanie nawigacji między stronami. Stronicowanie do pracy, potrzebny jest jeden z następujących opcji:

**Opcja 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Opcja 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Jeśli obie opcje są włączone w konfiguracji łącznika, pagedResultsControl jest używany.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl jest używana tylko z metody importu delta USNChanged, aby można było wyświetlić usunięte obiekty.

Łącznik próbuje wykryć opcji Prezentuj na serwerze. Jeśli nie można wykryć opcji, ostrzeżenie występuje na stronie globalnej właściwości łącznika. Nie wszystkie serwery LDAP Prezentuj wszystkie formanty i funkcje obsługują i nawet jeśli ma to ostrzeżenie, łącznik może działać bez problemów.

### <a name="delta-import"></a>Importowanie różnicy
Importowanie delta jest dostępna tylko w przypadku, gdy wykryto katalogu pomocy technicznej. Obecnie korzystają z następujących metod:

- LDAP Accesslog. Zobacz [Rejestrowanie http://www.openldap.org/doc/admin24/overlays.html#Access](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP Changelog. Zobacz [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Sygnatura czasowa. W przypadku eDirectory Novell/NetIQ łącznik używa Data/godzina ostatniej uzyskanie utworzona i zaktualizowana obiektów. EDirectory Novell/NetIQ nie umożliwia odpowiednik sposoby pobierania usuniętych obiektów. Ta opcja można również w przypadku żadnej innej metody importu różnicy aktywne na serwerze LDAP. Ta opcja nie jest możliwe importowanie usuniętych obiektów.
- USNChanged. Zobacz: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Brak obsługi
Następujące funkcje LDAP nie są obsługiwane:

- Odwołania LDAP między serwerami (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Tworzenie nowego łącznika
Aby utworzyć łącznik ogólnego LDAP, w **Usługi synchronizacji** wybierz **Agent zarządzania** i **Tworzenie**. Zaznacz łącznik **ogólnego LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Łączność
Na stronie Łączność należy określić hosta, Port i powiązania informacje. W zależności od powiązanie jest zaznaczonych, dodatkowe informacje mogą być dostarczane w poniższych sekcjach.

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Ustawienie limitu czasu połączenia służy tylko do pierwszego połączenia z serwerem po wykrywanie w schemacie.
- Jeśli powiązanie jest anonimowy, następnie ani nazwy użytkownika / hasła ani certyfikatu są używane.
- Dla innych powiązań wprowadź informacje albo w nazwy użytkownika i hasła lub Wybieranie certyfikatu.
- Jeśli korzystasz z protokołu Kerberos do uwierzytelniania, następnie zapewniają również obszaru i domenę użytkownika.

Pole tekstowe **aliasy atrybut** jest używana do atrybuty zdefiniowane w schemacie RFC4522 składni. Następujące atrybuty nie mogą być wykryte podczas wykrywania schematu i łącznik musi pomocy do identyfikowania atrybutach. Na przykład należy wprowadzić w polu aliasy atrybut poprawnie zidentyfikować atrybutu certyfikatu użytkownika jako binarne atrybut potrzebne są następujące:

`userCertificate;binary`

Oto przykładowy sposób może wyglądać tej konfiguracji:

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Zaznacz pole wyboru **Dołączanie atrybutów operacyjnych w schemacie** też dołączyć atrybuty utworzone przez serwer. Dotyczy to atrybuty, takie jak Jeśli obiekt został utworzony i godzina ostatniej aktualizacji.

Zaznaczenie **Dołącz extensible atrybuty w schemacie** służą obiekty rozszerzonego (RFC4512-4.3) i włączenie tej opcji umożliwia każdego atrybutu może być używany na wszystkich obiektów. Wybranie tej opcji powoduje, że schemat bardzo duże, chyba że katalogu połączonego używa tej funkcji zaleca się zachować opcja niezaznaczone.

### <a name="global-parameters"></a>Globalne parametry
Na stronie globalne parametry należy skonfigurować nazwy domeny różnicy — dziennik zmian i dodatkowe funkcje LDAP. Strona jest wstępnie wypełnione przy użyciu informacji podanych przez serwer LDAP.

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Górna sekcja zawiera informacji podanych przez samego serwera, takie jak nazwa serwera. Łącznik również sprawdza, czy obowiązkowe kontrolek znajdują się w DSE głównym. W przypadku tych kontrolek nie są widoczne, jest wyświetlone ostrzeżenie. Niektóre katalogów LDAP nie umieszczaj wszystkie funkcje w DSE główny i jest możliwe, że łącznik działa bez problemów, nawet jeśli znajduje się ostrzeżenie.

**Obsługiwane kontrolki** pola wyboru kontrolować zachowanie niektórych działań:

- Z zaznaczoną opcją Usuń drzewa, hierarchii zostanie usunięty z jednego połączenia LDAP. Usuwanie drzewa niezaznaczone łącznik oznacza Usuń cykliczne w razie potrzeby.
- Wyniki stronicowania zaznaczone łącznik działa stronicowania importowanie z podanego rozmiaru na temat wykonywania kroków.
- VLVControl i SortControl jest zamiast pagedResultsControl do odczytywania danych z katalogu LDAP.
- Jeśli wszystkie trzy opcje (pagedResultsControl, VLVControl i SortControl), nie są zaznaczone łącznik importuje wszystkich obiektów w jednej operacji, które może zakończyć się niepowodzeniem, jeśli jest dużego katalogu.
- ShowDeletedControl jest używane tylko w przypadku metody importu Delta USNChanged.

Dziennik zmian nazwy domeny jest używany przez dziennik zmian różnicy, na przykład kontekst nazewnictwa **cn = changelog**. Aby móc wykonać importowanie różnicy należy określić wartość.

Oto lista dziennik zmian domyślne DNs:

Katalogu | Dziennik zmian różnicy
--- | ---
Tych usług firmy Microsoft i AD ° c | Automatycznie wykryty. USNChanged.
Serwer Apache katalogu | Nie jest dostępna.
Katalog 389 | Dziennik zmian. Domyślne wartości użyć: **cn = changelog**
IBM Tivoli DS | Dziennik zmian. Domyślne wartości użyć: **cn = changelog**
Katalog Isode | Dziennik zmian. Domyślne wartości użyć: **cn = changelog**
EDirectory Novell/NetIQ | Nie jest dostępna. Sygnatura czasowa. Zastosowania łącznik Ostatnia aktualizacja Data/Godzina, aby uzyskać Dodawanie i aktualizowanie rekordów.
Otwieranie DJ-DS | Dziennik zmian.  Domyślne wartości użyć: **cn = changelog**
Otwieranie LDAP | Dziennik dostępu. Domyślne wartości użyć: **cn = accesslog**
Oracle DSEE | Dziennik zmian. Domyślne wartości użyć: **cn = changelog**
RadiantOne usługi dysków wirtualnych | Katalog wirtualny. W zależności od katalogu połączony z usługi dysków wirtualnych.
Serwer katalogu n z nich | Dziennik zmian. Domyślne wartości użyć: **cn = changelog**

Atrybut hasło jest nazwą atrybut, który łącznik należy użyć, aby ustawić hasło w Zmień hasło i hasło ustawione operacji.
Wartość ta jest domyślnie ustawiona na **userPassword** , ale można ją zmienić w razie potrzeby dla określonego systemu LDAP.

Na liście dodatkowe partycje jest można dodać dodatkowe przestrzenie nazw automatycznie wykryte. Na przykład to ustawienie umożliwia kilka serwerów składające się na klastrze logiczne powinny być wszystkie importowane w tym samym czasie. Tak, jak usługi Active Directory można mieć wiele domen w jeden las, ale wszystkich domenach udostępnić jeden schemat, można samo symulowane przez wprowadzenie dodatkowych nazw w tym polu. Każdy obszar nazw można zaimportować z różnych serwerów i dodatkowo skonfigurowano na stronie Konfigurowanie partycje i hierarchie. Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz.

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie obsługi administracyjnej hierarchii
Ta strona służy do mapowania składnika nazwy domeny, na przykład OU typ obiektu został zastrzeżony, na przykład jednostka organizacyjna.

![Obsługa administracyjna hierarchii](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfigurując obsługi administracyjnej hierarchii, można skonfigurować łącznik powoduje automatyczne tworzenie struktury w razie potrzeby. Na przykład jeśli istnieje kontrolera domeny nazw = contoso, kontrolera domeny = com i nowe cn obiektu = Jerzy ou = Seattle, c = US, kontrolera domeny = contoso, kontrolera domeny = zainicjowano obsługę administracyjną com, a następnie łącznik można utworzyć obiekt typu kraju dla Stanów Zjednoczonych i jednostka organizacyjna dla Seattle, jeśli te nie znajdujące się w katalogu.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurowanie partycje i hierarchie
Na stronie partycje i hierarchie wybierz wszystkie przestrzenie nazw obiektów, które mają być importowanie i eksportowanie.

![Partycje](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Dla każdego obszaru nazw jest również można skonfigurować ustawienia łączności, zmieniających wartości określonych na ekranie łączności. Jeśli wartości te są od lewej do ich domyślne wartości pustych, informacje na ekranie łączności są używane.

Użytkownik może również wybrać kontenerów i organizacyjnych łącznik należy zaimportować z i eksportowanie.

### <a name="configure-anchors"></a>Konfigurowanie kotwiczenie
Ta strona zawsze mieć wartość wstępnie i nie można zmienić. W przypadku zidentyfikowania dostawcy serwera kotwicy może zostać wypełnione z atrybutem niezmienne, na przykład identyfikator GUID obiektu. Jeśli nie zostało wykryte lub nie ma atrybut niezmienne, łącznik używa nazwy domeny (nazwa wyróżniająca) jako kotwica.

![Kotwiczenie](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Oto lista serwer LDAP i kotwicy używany:

Katalogu | Atrybut zakotwiczenia
--- | ---
Tych usług firmy Microsoft i AD ° c | objectGUID
Serwer 389 katalogu | nazwy domeny
Katalog Apache | nazwy domeny
IBM Tivoli DS | nazwy domeny
Katalog Isode | nazwy domeny
EDirectory Novell/NetIQ | IDENTYFIKATOR GUID
Otwieranie DJ-DS | nazwy domeny
Otwieranie LDAP | nazwy domeny
Oracle ODSEE | nazwy domeny
RadiantOne usługi dysków wirtualnych | nazwy domeny
Serwer katalogu n z nich | nazwy domeny

## <a name="other-notes"></a>Inne uwagi
Ta sekcja zawiera informacje o aspektów, które są specyficzne dla tego łącznika lub innych powodów są ważne.

### <a name="delta-import"></a>Importowanie różnicy
Znak wodny różnicy w otwartej LDAP jest UTC daty/godziny. Z tego powodu należy zsynchronizować zegary między usługą synchronizacji FIM i otwórz LDAP. W przeciwnym razie niektóre wpisy w dzienniku zmian różnicy mogą być pominięte.

Dla serwer katalogowy Novell eDirectory importowanie różnicy nie wykrywa usuwania dowolnego obiektu. Z tego powodu należy uruchomić pełne importowanie okresowo, aby znaleźć wszystkie obiekty usunięte.

Dla katalogów z różnicy dziennik zmian, oparty na Data/Godzina zaleca uruchom pełne importowanie w czasie okresowych. Dzięki temu aparatu synchronizacji, aby znaleźć i nierówności pomiędzy serwera LDAP i co to jest obecnie w obszarze łącznik.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

-   Aby uzyskać informacje dotyczące włączania rejestrowania rozwiązywać łącznik zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
