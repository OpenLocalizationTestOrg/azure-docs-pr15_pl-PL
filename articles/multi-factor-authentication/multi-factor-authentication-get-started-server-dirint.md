<properties 
    pageTitle="Integracja katalogu między uwierzytelnianie wieloskładnikowe Azure i usługi Active Directory"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób zintegrować serwer uwierzytelniania wieloskładnikowego Azure z usługą Active Directory w celu zsynchronizowania katalogów."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Integracja katalogu między Azure MFA Server i usługi Active Directory

W sekcji Integracja katalogu pozwala na serwer Integracja z usługą Active Directory lub innego katalogu LDAP.  Umożliwia konfigurowanie atrybutów pasuje do schematu katalogu i skonfigurować automatycznej synchronizacji użytkowników.

## <a name="settings"></a>Ustawienia
Domyślnie serwer uwierzytelniania wieloskładnikowego Azure jest skonfigurowany do importowania lub zsynchronizować użytkowników z usługi Active Directory.  Karta umożliwia zastępują domyślne zachowanie i powiązać z innego katalogu LDAP, katalogu ADAM lub określonego kontrolera domeny usługi Active Directory.  Jako celu RADIUS, przed uwierzytelniania dla uwierzytelniania usług IIS lub podstawowego uwierzytelniania dla użytkownika portalu danych umożliwia także na potrzeby uwierzytelniania LDAP do serwera proxy LDAP lub do wiązania LDAP.  W poniższej tabeli opisano poszczególne ustawienia.

![Ustawienia](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funkcja | Opis |
| ------- | ----------- |
| Za pomocą usługi Active Directory | Wybierz opcję Użyj usługi Active Directory za pomocą usługi Active Directory do importowania i synchronizacji.  Jest to ustawienie domyślne. <br>Uwaga: Komputer musi być dołączony do domeny i wymagane jest zalogowanie przy użyciu konta domeny dla integracji usługi Active Directory działał poprawnie. |
| Dołączanie zaufanych domen | Pole wyboru zawiera zaufane domeny, aby agenta próba połączenia domeny zaufane bieżącą domenę, innej domeny w las lub domen wyboru zajmujących zaufanie las.  Gdy nie importowania lub synchronizowanie użytkowników z dowolnej domeny zaufanej, wyczyść pole wyboru, aby zwiększyć wydajność.  Domyślnie jest zaznaczone pole wyboru. |
| Używanie określonych konfiguracji LDAP | Wybierz opcję Użyj LDAP za pomocą LDAP ustawieniami do importowania i synchronizacji. Uwaga: Po zaznaczeniu LDAP Użyj interfejsu użytkownika przybierze kształt odwołania z usługi Active Directory LDAP. |
| Obraz przycisku | Przycisk Edytuj umożliwia bieżące ustawienia konfiguracji LDAP do modyfikacji. |
| Używanie atrybutu zakres kwerendy | Wskazuje, czy atrybut zakres kwerendy powinien być używany.  Zezwalaj atrybut zakres kwerendy wyszukiwania w katalogu skutecznego kwalifikowanie rekordów na podstawie wpisów w atrybucie innego rekordu.  Serwer uwierzytelniania wieloskładnikowego Azure wykorzystuje atrybut zakres kwerendy wydajność kwerendy użytkowników, których jest członkiem grupy zabezpieczeń.   <br>Uwaga: Istnieje niektórych przypadkach, gdy atrybut zakres kwerendy są obsługiwane, ale nie powinny być używane.  Na przykład usługi Active Directory mogą mieć problemy z atrybut zakres kwerendy, gdy grupy zabezpieczeń zawiera członków z więcej niż jedną domenę.  W tym przypadku pole wyboru powinny być zaznaczone. |

W poniższej tabeli opisano ustawienia konfiguracji LDAP.

| Funkcja | Opis |
| ------- | ----------- |
| Serwer | Wprowadź nazwa hosta lub adres IP serwera z systemem katalogu LDAP.  Serwer kopii zapasowej mogą również określone, oddzielonych średnikami. <br>Uwaga: Jeśli powiązać typ jest SSL, nazwa hosta w pełni kwalifikowane jest zazwyczaj wymagane. |
| Base nazwy domeny | Wprowadź nazwa wyróżniająca obiektu katalog podstawowy, z której rozpocznie się wszystkie kwerendy katalogu.  Na przykład kontrolera domeny = abc, kontrolera domeny = com. |
| Powiązać typ — kwerendy | Wybierz typ powiązania odpowiednie do użytku podczas wiązania z wyszukiwania w katalogu LDAP.  Ten typ umożliwia importowanie plików, synchronizacji i rozpoznawanie nazwy użytkownika. <br><br>  Anonimowe - anonimowego powiązania zostanie wykonana.  Powiązanie nazwy domeny i powiązać hasła nie będą używane.  To działa tylko, jeśli katalogu LDAP umożliwia powiązanie anonimowe i uprawnienia umożliwiają wyszukiwanie odpowiednich rekordów i atrybuty.  <br><br> Prosty — powiązania nazwy domeny i powiązać hasła będą przekazywane jako zwykły tekst, aby powiązać z katalogu LDAP.  To tylko należy do celów testowych Sprawdź, czy serwer można się z Tobą i czy konto powiązania ma odpowiednie prawa dostępu.  Zaleca się, że SSL użyty po zainstalowaniu odpowiedniej certyfikatu.  <br><br> SSL - powiązanie nazwy domeny i powiązać hasła będą szyfrowane przy użyciu protokołu SSL powiązać z katalogu LDAP.  W tym celu certyfikatu zainstalowania lokalnie czy zaufania katalogu LDAP.  <br><br> Windows — powiązania nazwy użytkownika i hasła powiązać będzie używana aby zapewnić bezpieczne połączenie do kontrolera domeny usługi Active Directory lub ADAM katalogu.  Jeśli powiązać nazwy użytkownika jest puste, konto logowania użytkownika będzie używane powiązać. |
| Typ - uwierzytelnienia wiązania | Wybierz typ powiązania odpowiedni do użycia podczas wykonywania uwierzytelniania powiązania LDAP.  Zobacz powiązania wpisz opisy w obszarze Typ powiązania — zapytania.  Na przykład dzięki dla anonimowego powiązania ma być używany dla zapytania podczas powiązanie SSL jest używany do bezpiecznego uwierzytelniania powiązania LDAP. |
| Powiązania nazwy domeny lub powiązania nazwy użytkownika | Wprowadź nazwę wyróżniająca rekord, aby konto, które będzie używana dla powiązań z katalogiem LDAP.<br><br>Nazwa wyróżniająca powiązania tylko służy powiązać typ jest proste lub SSL.  <br><br>Wprowadź nazwę użytkownika konta systemu Windows, aby używać, gdy wiązanie z katalogu LDAP po powiązać typ systemu Windows.  Jeśli puste, konto logowania użytkownika będzie używane powiązać. |
| Powiązać hasła | Wprowadź hasło powiązania powiązać nazwy domeny lub nazwy użytkownika jest używana do powiązania z katalogu LDAP.  Aby skonfigurować hasło usługi Synchronizacja serwera uwierzytelnianie wieloskładnikowe, musi być włączony synchronizacji i usługi musi być zainstalowany na komputerze lokalnym.  Hasło zostaną zapisane w Windows przechowywane użytkowników i hasła w obszarze konta, które działa synchronizacja Usługa uwierzytelniania wieloskładnikowego jako.  Hasło także zostaną zapisane w obszarze konto, dla którego interfejs użytkownika serwera uwierzytelnianie wieloskładnikowe działa jako oraz konta, dla którego jest uruchomiona usługa serwera uwierzytelnianie wieloskładnikowe, jako.  <br><br> Uwaga: Ponieważ hasła tylko jest przechowywany w lokalnego serwera Windows przechowywane nazwy użytkowników i hasła, ten krok muszą zostać wykonane na wszystkich serwerach uwierzytelniania wieloskładnikowego, który musi mieć dostęp do hasła. |
| Limit rozmiaru kwerendy | Określ limit rozmiaru maksymalna liczba użytkowników, których wyszukiwanie katalogu zwróci.  Ten limit powinny być zgodne konfigurację w katalogu LDAP.  Wyszukiwanie dużych miejsce, w którym Stronicowanie nie jest obsługiwane importowanie i synchronizacja spróbuje pobrać użytkowników.  Jeśli limit rozmiaru określony w tym miejscu jest większy niż limit skonfigurowano w katalogu LDAP, odebrane niektórych użytkowników. |
| Przycisk testu | Kliknij przycisk Testuj, aby przetestować powiązanie z serwera LDAP.  <br><br> Uwaga: Opcja Użyj LDAP nie muszą być zaznaczone, aby przetestować powiązanie.  Dzięki temu powiązanie sprawdzone przed użyciem konfiguracji LDAP. |

## <a name="filters"></a>Filtry
Filtry umożliwiają ustawianie kryteriów w celu kwalifikować się rekordy podczas wyszukiwania w katalogu.  Ustawiając filtr można ograniczyć obiekty, które chcesz zsynchronizować.  

![Filtry](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Uwierzytelnianie wieloskładnikowe Azure dostępne są następujące opcje 3.

- **Filtr kontenera** — używana do kwalifikowania rekordów kontenera podczas wyszukiwania w katalogu kryteria filtru.  Dla usługi Active Directory i ADAM (| () objectClass=organizationalUnit)(objectClass=container)) jest na ogół używany.  Dla innych katalogów LDAP kryteria filtrowania, które kwalifikuje się każdego typu obiektu kontenera powinien być używany w zależności od schematu katalogu.  <br>Uwaga: Jeśli puste, ((objectClass=organizationalUnit)(objectClass=container)) zostanie użyty domyślnie.

- **Filtr grupy zabezpieczeń** — używana do kwalifikowania rekordów grupy zabezpieczeń podczas wyszukiwania w katalogu kryteria filtru.  Dla usługi Active Directory i ADAM (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) jest na ogół używany.  Dla innych katalogów LDAP kryteria filtrowania, które kwalifikuje się każdego typu obiektu grupy zabezpieczeń powinien być używany w zależności od schematu katalogu.  <br>Uwaga: Jeśli puste, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) będzie używany domyślnie.

- **Filtr użytkownika** — używana do kwalifikowania rekordy użytkowników podczas wyszukiwania w katalogu kryteria filtru.  Dla usługi Active Directory i ADAM, (& (objectClass=user)(objectCategory=person)) jest na ogół używany.  Dla innych katalogów LDAP (objectClass = inetOrgPerson) lub podobnej powinny być używane w zależności od schematu katalogu. <br>Uwaga: Jeśli puste, (& (objectCategory=person)(objectClass=user)) będzie używany domyślnie.

## <a name="attributes"></a>Atrybuty
Atrybuty może dostosować stosownie do potrzeb dla określonego katalogu.  Umożliwia dodawanie niestandardowych atrybutów i dostosować synchronizacji tylko te atrybuty, które są potrzebne.  Wartość dla każdego pola atrybut powinna być nazwa atrybutu zgodnie z definicją w schemacie katalogu.  Skorzystaj z tabeli poniżej, aby uzyskać dodatkowe informacje.

![Atrybuty](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funkcja | Opis |
| ------- | ----------- |
| Unikatowy identyfikator | Wprowadź nazwę atrybutu atrybut, który służy jako unikatowy identyfikator kontenera, grupy zabezpieczeń i rekordy użytkowników.  W usłudze Active Directory jest to zazwyczaj objectGUID.  W innych implementacji LDAP może być entryUUID lub podobnej.  Wartość domyślna to objectGUID. |
| ... Przyciski (wybierz atrybut) | Każde pole atrybutu ma przycisku "... obok wyświetlanej w oknie dialogowym wybierz atrybut zezwalania atrybutu należy wybrać z listy". <br><br>Wybierz atrybut okno dialogowe.<br><br>Uwaga: Atrybuty mogą być wprowadzane ręcznie, a nie muszą być zgodne atrybutu na liście atrybutów. |
| Unikatowy identyfikator typu | Wybierz typ atrybutu Unikatowy identyfikator.  W usłudze Active Directory atrybut objectGUID jest typu GUID.  W innych implementacji LDAP może być typu ASCII tablicy lub ciągu.  Wartość domyślna to identyfikator GUID. <br><br>Uwaga: Należy ustawić to poprawnie, ponieważ elementy synchronizacji odwołuje się ich unikatowych identyfikatorów i unikatowy identyfikator typu służy do bezpośrednio znaleźć obiektu w katalogu.  Ustawienie to w ciągu katalogu rzeczywiście zawiera wartość zgodnie z tablicy bajtów znaków ASCII uniemożliwia synchronizację z działa poprawnie. |
| Nazwa wyróżniająca | Wprowadź nazwę atrybutu atrybut, który zawiera nazwa wyróżniająca dla każdego rekordu.  W usłudze Active Directory jest to zazwyczaj distinguishedName.  W innych implementacji LDAP może być entryDN lub podobnej.  Wartość domyślna to distinguishedName. <br><br>Uwaga: Jeśli atrybut zawierający tylko nazwa wyróżniająca nie istnieje, atrybut adspath może być używany.  "LDAP: / /<server>-" część ścieżki zostaną automatycznie usunięte wyłączanie opuszczania tylko nazwa wyróżniająca obiektu. |
| Nazwa kontenera | Wprowadź nazwę atrybutu atrybut, który zawiera nazwę w rekordzie kontener.  Wartość atrybutu ten będzie wyświetlany w hierarchii kontenera importowania danych z usługi Active Directory lub dodawanie elementów synchronizacji.  Wartość domyślna to nazwa. <br><br>Uwaga: Jeśli różne kontenery używać różnych atrybuty ich nazwy, wiele atrybutów nazwy kontenera można określić w rozdzielone średnikami.  Atrybut pierwszej nazwy kontenera znajdujące się na obiekt kontenera będzie używana do wyświetlania jego nazwę. |
| Nazwa grupy zabezpieczeń | Wprowadź nazwę atrybutu atrybut, który zawiera nazwę w rekordzie grupy zabezpieczeń.  Wartość atrybutu ten pojawi się na liście grupy zabezpieczeń podczas importowania danych z usługi Active Directory lub dodawanie elementów synchronizacji.  Wartość domyślna to nazwa. |
| Użytkownicy | Następujące atrybuty są używane do wyszukiwania, wyświetlanie, importowanie i synchronizowanie informacje o użytkowniku z katalogu. |
| Nazwa użytkownika | Wprowadź nazwę atrybutu atrybut, który zawiera nazwy użytkownika w rekordzie użytkownika.  Jako nazwę użytkownika serwera uwierzytelnianie wieloskładnikowe zostanie użyta wartość tego atrybutu.  Druga atrybut można określić jako kopii zapasowej do pierwszej.  Atrybut drugiego można używać tylko jeśli pierwszego atrybutu nie zawiera wartość dla użytkownika.  Ustawienia domyślne są atrybuty sAMAccountName i userPrincipalName. |
| Imię | Wprowadź nazwę atrybutu atrybut, który zawiera nazwę pierwszego rekordu użytkownika.  Wartość domyślna to imię. |
| Nazwisko | Wprowadź nazwę atrybutu atrybut, który zawiera nazwisko w rekordzie użytkownika.  Wartość domyślna to sn. |
| Adres e-mail | Wprowadź nazwę atrybutu atrybut, który zawiera adres e-mail w rekordzie użytkownika.  Adres e-mail będzie używany do wysyłania powitalnej i aktualizowania wiadomości e-mail do użytkownika.  Wartość domyślna to poczty. |
| Grupy użytkowników | Wprowadź nazwę atrybutu atrybut, który zawiera grupy użytkowników w rekordzie użytkownika.  Grupy użytkowników może służyć do filtrowania użytkowników w agent i w raportach w portalu zarządzania serwera uwierzytelniania wieloskładnikowego. |
| Opis | Wprowadź nazwę atrybutu atrybut, który zawiera opis w rekordzie użytkownika.  Opis jest używany tylko do wyszukiwania.  Wartość domyślna to opis. |
| Język połączeń głosowych | Wprowadź nazwę atrybutu atrybut, który zawiera krótką nazwę język używany dla połączeń głosowych dla użytkownika. |
| Język tekstu wiadomości SMS | Wprowadź nazwę atrybutu atrybut, który zawiera krótką nazwę język wiadomości SMS tekstu dla użytkownika. |
| Język aplikacji telefonu | Wprowadź nazwę atrybutu atrybut, który zawiera krótką nazwę język używany dla wiadomości SMS aplikacji telefonu dla użytkownika. |
| Język token PRZYSIĘGĄ | Wprowadź nazwę atrybutu atrybut, który zawiera krótką nazwę język używany dla wiadomości SMS token PRZYSIĘGĄ dla użytkownika. |
| Telefony | Następujące atrybuty są używane do importowania lub synchronizowanie numerów telefonów użytkowników.  Jeśli nie określono nazwy atrybutu dla typów telefonów, typów telefonów nie będą dostępne podczas importowania danych z usługi Active Directory lub dodawanie elementów synchronizacji. |
| Business | Wprowadź nazwę atrybutu atrybut, który zawiera służbowy numer telefonu w rekordzie użytkownika.  Wartość domyślna to telephoneNumber. |
| Narzędzia główne | Wprowadź nazwę atrybutu atrybut, który zawiera numer telefonu domowego w rekordzie użytkownika.  Wartość domyślna to TelefonDomowy. |
| Pager | Wprowadź nazwę atrybutu atrybut, który zawiera numer pager w rekordzie użytkownika.  Wartość domyślna to pager. |
| Telefon komórkowy | Wprowadź nazwę atrybutu atrybut, który zawiera numer telefonu komórkowego w rekordzie użytkownika.  Wartość domyślna to urządzeń przenośnych. |
| Faksu | Wprowadź nazwę atrybutu atrybut, który zawiera numer faksu w rekordzie użytkownika.  Wartość domyślna to facsimileTelephoneNumber. |
| Telefon IP | Wprowadź nazwę atrybutu atrybut, który zawiera numer telefonu IP w rekordzie użytkownika.  Wartość domyślna to ipPhone. |
| Niestandardowe | Wprowadź nazwę atrybutu atrybut, który zawiera numer telefonu niestandardowe w |
|  | rekord użytkownika.  Domyślnie jest puste. |
| Rozszerzenie | Wprowadź nazwę atrybutu atrybut, który zawiera numer wewnętrzny w rekordzie użytkownika.  Rozszerzenie do podstawowego numeru telefonu tylko zostanie użyta wartość pola rozszerzenia.  Domyślnie jest puste. <br><br>Uwaga: Jeśli nie określono atrybutu rozszerzenie, rozszerzenia może być częścią atrybutu telefonu.  Rozszerzenie powinien być poprzedzony z symbolem x, więc można można przeanalizować.  Na przykład 555-123-4567 x890 doprowadziłaby 555-123-4567 jako numer telefonu i 890 jako rozszerzenie. |
| Przywracanie domyślnych ustawień przycisku | Kliknij przycisk Przywróć domyślne, aby przywrócić wszystkie atrybuty wartość domyślna.  Ustawienia domyślne powinna działać poprawnie ze schematem usługi Active Directory i ADAM normalny. |

Aby edytować atrybuty, po prostu kliknij przycisk Edytuj na karcie atrybuty.  To powoduje wyświetlenie okna, które umożliwia edytowanie atrybutów.

![Edytowanie atrybutów](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronizacja
Synchronizacja pozwala bazy danych użytkowników wieloskładnikowe Azure synchronizowana z użytkownikami w usłudze Active Directory lub innego katalogu Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol.  Ten proces jest podobne do ręcznego importowania użytkowników z usługi Active Directory, ale okresowo sprawdza dla użytkownika usługi Active Directory i zmian grup zabezpieczeń procesu.  Umożliwia także wyłączanie lub usuwanie użytkowników usunąć z grupy kontenera lub zabezpieczeń i usuwanie użytkowników zostaną usunięte z usługą Active Directory.

Synchronizacja uwierzytelnianie wieloskładnikowe usługi to usługa Windows wykonującego okresowych ankieta usługi Active Directory.  To nie należy mylić z Azure AD Sync lub narzędzie Azure AD Connect.  Synchronizacja uwierzytelnianie wieloskładnikowe, chociaż utworzoną na podstawie kodu podobne jest specyficzne dla serwera uwierzytelniania wieloskładnikowego Azure.  Jest zainstalowany w stanie zatrzymania, a zostanie uruchomiona przez usługę serwera uwierzytelnianie wieloskładnikowe skonfigurowany do uruchomienia.  Jeśli masz konfiguracji serwera uwierzytelnianie wieloskładnikowe wielu serwerów synchronizacja uwierzytelnianie wieloskładnikowe można uruchomić tylko na serwerze.

Usługa synchronizacja uwierzytelnianie wieloskładnikowe używa rozszerzenie serwera DirSync LDAP dostarczane przez firmę Microsoft do efektywne ankieta zmian.  Ten rozmówcy sterowania DirSync musi zawierać "katalog pobieranie zmian" bezpośrednio i Zasadami replikacji-Get-zmiany rozszerzonego kontrola dostępu w prawo.  Domyślnie tych praw przypisanych do konta administratora i system lokalny na kontrolerach domeny.  Synchronizacja uwierzytelnianie wieloskładnikowe usługi jest skonfigurowany do uruchomienia jako system lokalny domyślnie.  W związku z tym najprościej jest do uruchamiania usługi na kontrolerze domeny.  Usługę można uruchamiać przy użyciu konta z uprawnieniami mniejsze w przypadku konfigurowania zawsze wykonania pełnej synchronizacji.  Jest to mniej wydajne, ale wymaga mniej uprawnień konta.

Jeśli skonfigurowany do używania protokołu LDAP i katalogu LDAP obsługuje sterowanie DirSync, następnie sondowania dla użytkownika i zmian grup zabezpieczeń będzie działać tak samo jak z usługą Active Directory.  Jeśli Usługa LDAP nie obsługuje sterowania DirSync, pełnej synchronizacji zostaną wykonane podczas każdego cyklu.

![Synchronizacja](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Aby uzyskać dodatkowe informacje na temat poszczególnych ustawień na karcie Synchronizacja za pomocą poniższej tabeli.

| Funkcja | Opis |
| ------- | ----------- |
| Włącz synchronizację z usługą Active Directory | Po zaznaczeniu serwera uwierzytelnianie wieloskładnikowe zostanie uruchomiona do okresowego wysyłania usługi Active Directory w poszukiwaniu zmian. <br><br>Uwaga: co najmniej jeden element synchronizacji musi zostać dodana i zsynchronizuj teraz, należy wykonać przed serwera uwierzytelnianie wieloskładnikowe usługa uruchomi się przetwarzanie zmian. |
| Synchronizowanie wszystkich | Określ przedział czasu oczekiwania usługa serwera uwierzytelnianie wieloskładnikowe między sondowania i przetwarzanie zmian. <br><br> Uwaga: Z interwałem określonym jest odstęp czasu między na początku każdego cyklu.  Jeśli podczas przetwarzania zmiany przekracza interwał, usługa zostanie ponownie natychmiast ankieta. |
| Usuwanie użytkowników nie jest już w usłudze Active Directory | Po zaznaczeniu usługi Serwer uwierzytelnianie wieloskładnikowe procesu reliktów użytkownika usługi Active Directory usunięte i usuwanie użytkownikowi serwera uwierzytelnianie wieloskładnikowe. |
| Zawsze wykonania pełnej synchronizacji | Po zaznaczeniu usługi Serwer uwierzytelnianie wieloskładnikowe zawsze wykona pełnej synchronizacji.  Jeśli nie jest zaznaczona, usługa serwera uwierzytelnianie wieloskładnikowe wykona przyrostowe synchronizacji przez tylko badanie użytkowników, które zostały zmienione.  Domyślnie jest zaznaczone. <br><br> Uwaga: Jeśli nie jest zaznaczona, przyrostowe synchronizacji można wykonać tylko po katalogu obsługuje sterowanie DirSync i konto jest używana do tworzenia powiązania z katalogiem ma odpowiednie uprawnienia do wykonywania kwerend przyrostowe DirSync.  Jeśli konta nie dysponuje się odpowiednimi uprawnieniami lub wiele domen wiążą się z synchronizacji, należy wykonać pełne zaleca się synchronizacji. |
| Wymaganie zatwierdzenia przez administratora więcej niż X użytkowników zostaną wyłączone lub usunięte | Aby wyłączyć lub usunąć użytkowników, którzy już członkiem kontener elementu lub grupy zabezpieczeń można skonfigurować elementy synchronizacji.  Ze względów bezpieczeństwa zatwierdzenia przez administratora może być wymagane, gdy liczba użytkowników, aby wyłączyć lub usunąć przekracza progu.  Po zaznaczeniu zatwierdzanie będą wymagane na potrzeby określony próg.  Wartość domyślna to 5, a zakresem są liczby od 1 do 999. <br><br> Zatwierdzanie jest ułatwiać przez pierwszy wysłanie wiadomości e-mail z powiadomieniem dla administratorów. Powiadomienie e-mail zawiera instrukcje dotyczące recenzowanie i zatwierdzanie wyłączanie i usuwanie użytkowników.  Po uruchomieniu interfejsu użytkownika serwera uwierzytelnianie wieloskładnikowe, pojawi się monit o zatwierdzenie. |

Przycisk **Synchronizuj teraz** pozwala na uruchamianie pełnej synchronizacji dla określonych elementów synchronizacji.  Pełna synchronizacja jest wymagana przy każdej synchronizacji elementy zostaną dodane, zmodyfikowane, usunięte lub zamówić ponownie.  Jest ona również wymagana przed usługa synchronizacja uwierzytelnianie wieloskładnikowe będzie operacyjne, ponieważ ustawia punkt początkowy, z którego usługa będzie ankieta przyrostowe zmiany.  Jeśli wprowadzono zmiany do synchronizacji elementów, a nie została wykonana pełnej synchronizacji, możesz wyświetli monit o Synchronizuj teraz gdy Przechodzenie do innej sekcji lub gdy zamknięcia interfejsu użytkownika.

Przycisk **Usuń** umożliwia administratora, aby usunąć jeden lub więcej elementów synchronizacji z listy elementów serwera uwierzytelnianie wieloskładnikowe synchronizacji.

>[AZURE.WARNING]Po usunięciu rekordu elementu synchronizacji nie można odzyskać. Konieczne będzie ponowne dodawanie rekordu elementu synchronizacji, jeśli została usunięta przez pomyłkę.

Synchronizacja element lub elementy synchronizacji zostały usunięte z serwera uwierzytelnianie wieloskładnikowe.  Usługa serwera uwierzytelnianie wieloskładnikowe już nie będzie przetwarzał elementy synchronizacji.

Przyciski Przenieś w górę i Przenieś w dół Zezwalaj administratorem, aby zmienić kolejność elementów synchronizacji.  Kolejność jest ważna, ponieważ użytkownik może być członkiem więcej niż jeden element synchronizacji (przykład kontenera i grupy zabezpieczeń).  Ustawienia zastosowane do użytkownika podczas synchronizacji będą pochodzić z pierwszego elementu synchronizacji na liście, z którym jest skojarzony użytkownik.  W związku z tym elementy synchronizacji należy umieścić w priorytetu.

>[AZURE.TIP]Po usunięciu synchronizacji elementy powinny być wykonywane pełnej synchronizacji.  Pełnej synchronizacji powinny być wykonywane po kolejnością elementy synchronizacji.  Kliknij przycisk Synchronizuj teraz, aby wykonać pełnej synchronizacji.

## <a name="multi-factor-auth-servers"></a>Serwery uwierzytelniania wieloskładnikowego
Dodatkowe serwery uwierzytelnianie wieloskładnikowe może skonfiguruj służyć jako kopii zapasowej proxy RADIUS LDAP proxy lub uwierzytelniania usług IIS. Konfiguracja synchronizacji zostaną udostępnione wszystkim agentów. Jednak tylko jeden z tych czynników może być uruchomiona usługa serwera uwierzytelnianie wieloskładnikowe. Ta karta umożliwia wybierz serwer uwierzytelnianie wieloskładnikowe, który powinien być włączona na potrzeby synchronizacji.

![Serwery wielu wieloskładnikowe uwierzytelniania](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
