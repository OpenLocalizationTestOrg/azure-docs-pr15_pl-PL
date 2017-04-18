<properties
    pageTitle="W chmurze odnajdowania aplikacji zabezpieczeń i prywatności zagadnienia | Microsoft Azure"
    description="W tym temacie opisano zagadnienia dotyczące zabezpieczeń i prywatności dotyczące odnajdowania aplikacji chmury."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Odnajdowanie aplikacji zabezpieczeń i prywatności zagadnienia w chmurze

Firma Microsoft poświęca ochroną prywatności i zabezpieczanie danych, dostępne podczas przedstawiania oprogramowania i usług, które ułatwiają zarządzanie zabezpieczeń firmy. <br>
Firma Microsoft uznają, że po powierzyć danych do innych osób, że zaufania wymaga inwestycji konstrukcyjną rygorystyczne zabezpieczeń i kompetencji kopii.
Microsoft opiera się na ściśle zgodności i wskazówki dotyczące zabezpieczeń od praktyk cyklu opracowywania bezpieczny oprogramowanie do pracy z usługą. <br>
Zabezpieczanie i ochrona danych jest priorytetem dla firmy Microsoft.

W tym temacie wyjaśniono, jak dane są zbierane, przetwarzane i zabezpieczone Azure Active Directory chmury aplikacji odnajdowania




##<a name="overview"></a>Omówienie

Odnajdowanie aplikacji chmurze jest funkcją Azure AD i jest obsługiwany w programie Microsoft Azure. <br>
Agent punktu końcowego odnajdowania aplikacji chmurze jest używany do zbierania danych odnajdowania aplikacji z IT zarządzanych komputerów. <br>
Zebrane dane są wysyłane przez zaszyfrowanego kanału bezpieczne do Usługa odnajdowania aplikacji Azure AD chmury. <br>
Dane odnajdowania aplikacji w chmurze dla organizacji będzie widoczny w portalu Azure. <br>


<center>![Jak działa odnajdowania aplikacji chmury](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


W poniższych sekcjach wykonaj przepływu informacji i opisano, jak zabezpieczone podczas przenoszenia z Twojej organizacji, do usługi w chmurze odnajdowania aplikacji i w końcu do portalu odnajdowania aplikacji chmury.



## <a name="collecting-data-from-your-organization"></a>Zbieranie danych z Twojej organizacji

Aby można było użyć funkcji odnajdowania aplikacji chmury usługi Azure Active Directory uzyskanie wniosków w aplikacjach używane przez pracowników w Twojej organizacji, należy najpierw zainstalować agenta punktu końcowego Azure AD chmury odnajdowania aplikacji na komputerach w organizacji.

Administratorzy dzierżawy usługi Azure Active Directory (lub ich pełnomocnik) mogą pobrać pakiet instalacyjny agenta z portalu Azure. Agent można można ręcznie zainstalowane lub zainstalowana na wielu komputerach w organizacji za pomocą SCCM lub zasad grupy.

Uzyskać odpowiednie instrukcje na temat opcji wdrażania zobacz [Podręcznik wdrażania zasad grupy chmury aplikacji odnajdowania](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Danych zebranych przez agenta

Informacje podane w poniższej listy są zbierane przez agenta po nawiązaniu połączenia do aplikacji sieci Web. Informacje są zbierane przez aplikacje skonfigurowane do odnajdowania administrator. <br>
Można edytować na liście aplikacji chmury, które agent monitoruje za pośrednictwem karta odnajdowania aplikacji chmury w programie Microsoft [Azure portalu](https://portal.azure.com/)w obszarze **Ustawienia**->**Zbierania danych**->**listy kolekcji aplikacji**. Aby uzyskać więcej informacji zobacz [Uzyskiwanie pracę z chmury aplikacji odnajdowanie](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Kategoria informacji**: informacje o użytkowniku <br>
**Opis**: <br>
Nazwa użytkownika systemu Windows procesu żądanie docelowej aplikacji sieci Web (przykład: DOMENA\nazwa_użytkownika) oraz identyfikator zabezpieczeń systemu Windows (SID) użytkownika.


**Informacje o kategorii**: przetwarzanie informacji <br>
**Opis**: <br>
Nazwę procesu, które wprowadzone żądanie docelowej aplikacji sieci Web (np.: "iexplore.exe")

**Informacje o kategorii**: komputer informacji <br>
**Opis**: <br>
Nazwa NetBIOS komputera, na którym jest zainstalowana agenta.

**Kategoria informacji**: informacje o aplikacji ruchu <br>
**Opis**: <br>

Następujące informacje o połączeniu:

- Źródła (komputer lokalny) i docelowe adresy IP i numery portów

- Publiczny adres IP organizacji za pomocą którego żądanie wykracza poza.

- Czas żądania

- Wielkość ruchu wysyłane i otrzymywane

- Wersja IP (4 lub 6)

- Tylko w przypadku TLS połączeń: Nazwa hosta docelowej z rozszerzenia wskazanie nazwa serwera lub certyfikat serwera.

HTTP następujące informacje:

- Metoda (GET, WPIS itp.)

- Protocol (HTTP/1.1 itp.)

- Ciąg agenta użytkownika

- Nazwa hosta

- Docelowy identyfikator URI (z wyłączeniem ciągu kwerendy)

- Informacje o typach zawartości

- Adresy URL odwołania (z wyłączeniem ciągu kwerendy)



> [AZURE.NOTE] Powyższe informacje HTTP są zbierane dla wszystkich połączeń nie są szyfrowane.
W przypadku połączeń TLS te informacje jest tylko zostały przechwycone, gdy opcja "Dokładnej kontroli" jest włączona w portalu. To ustawienie jest domyślnie "Włączone".
Aby uzyskać bardziej szczegółowe informacje, zobacz poniżej i [Wprowadzenie pracę z chmury aplikacji odnajdowania](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Oprócz dane, które agent zbiera aktywności sieci również zbiera anonimowe dane na temat oprogramowania i konfiguracji sprzętowej, raporty o błędach i informacji na temat sposobu używania agenta.

<br><br>
### <a name="how-the-agent-works"></a>Jak działa agenta

Instalacja agenta obejmuje dwa składniki:

- Składnik trybu użytkownika

- Składnik sterownika trybu jądra (sterownika platformy filtrowania systemu Windows)



Podczas pierwszej instalacji agenta przechowuje zaufanego certyfikatu komputera na komputerze, która następnie używa ustanowić bezpiecznego połączenia z usługą odnajdowania aplikacji chmury. <br>
Agent okresowo pobiera konfiguracji zasad z usługi odnajdowania aplikacji chmury nad tym bezpiecznego połączenia. <br>
Zasady zawiera informacje o aplikacji chmury, które mają monitor i czy automatycznego aktualizowania powinna być włączona, między innymi.

Ruchu w sieci Web jest wysyłane i otrzymywane na komputerze z programem Internet Explorer i Chrome, agent odnajdowania aplikacji chmury analizuje ruchu i wyodrębnia odpowiednie metadanych (zobacz sekcję **dane zbierane przez agenta** powyżej). <br>
Co minutę agent spowoduje przekazanie zebrane metadanych do usługi w chmurze odnajdowania aplikacji na zaszyfrowanego kanału.

Składnik sterownika przechwytuje ruchu szyfrowanego i wstawia się w zaszyfrowanej strumienia. Więcej informacji w poniższej sekcji **danych Intercepting z szyfrowanego połączenia (dokładnej kontroli)** .


### <a name="respecting-user-privacy"></a>Przestrzeganie prywatność użytkownika

Celem jest zapewnienie Administratorzy narzędzia, które umożliwiają ustawianie równowagi między szczegółowe optyką do użycia i użytkownika prywatności aplikacji zależnie od potrzeb organizacji. W tym celu firma Microsoft udostępnia następujące pokrętła na stronie Ustawienia w portalu:

- **Zbieranie danych**: Administratorzy mogą wybrać określić aplikacji lub chce uzyskać dane wykrywania na kategorie aplikacji.

- **Dokładnej kontroli**: Administratorzy wybrać Określ, jeśli agent gromadzi ruch HTTP połączenia SSL/TLS (czyli **"dokładnej kontroli"**). Więcej informacji w następnej sekcji.

- **Opcje zgodę**: Administratorzy mogą korzystać z portalu odnajdowania aplikacji w chmurze, o wybranie, czy informować użytkowników o zbierania danych przez agenta i czy użytkownik wyraża zgodę przed uruchomieniem agenta zbieranie danych użytkownika.

Agent punktu końcowego odnajdowania aplikacji chmury tylko gromadzi informacje opisane w sekcji **dane zbierane przez agenta** powyżej.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Przechwycenia danych z połączenia szyfrowanego (dokładnej kontroli)
Jak wspomniano wcześniej, Administratorzy mogą konfigurować agenta monitorowanie danych z szyfrowanego połączenia ("dokładnej kontroli"). TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) jest jednym z najczęściej używanych protokołów w Internecie dzisiaj. Ponieważ do szyfrowania komunikacji z TLS, klient może ustanowić kanału bezpieczeństwo i prywatność komunikacji z serwerem sieci web; TLS zawiera podstawowe ochrony dla przekazywania poświadczeń uwierzytelniania i zapobiec ujawnienia informacji poufnych.

Gdy zakończenia do końca bezpiecznego zaszyfrowanego kanału dostarczony przez TLS umożliwia ważnych zabezpieczeń i ochrony prywatności, protokół jest często użyte do celów złośliwy lub nefarious. Tak dużo tak w rzeczywistości tego TLS jest często nazywane "Protokół uniwersalnej obejścia zapory". Przyczyny problemu to nie może sprawdzić komunikacji TLS, ponieważ warstwy aplikacji dane są szyfrowane za pomocą protokołu SSL większość zapory. Informacje o tym, ataki często wykorzystać TLS do przeprowadzania złośliwy ładunki użytkownikowi pewność, że nawet najbardziej inteligentnego zapory warstwy aplikacji są całkowicie niewidome do protokołu TLS i po prostu powinien przekazywać TLS komunikacji między tabelami hosts. Użytkownicy końcowi często wykorzystać TLS, aby pominąć uzyskiwanie dostępu do formantów nałożonych przez ich firmowej zapory i serwery proxy, jego nawiązywanie połączenia za pomocą publicznej serwery proxy i na protokoły-TLS przez zaporę, która w przeciwnym razie może zostać zablokowana przez zasady tunelowania.

Dokładnej kontroli umożliwia agenta chmury odnajdowania aplikacji jako zaufanej w pośrednik. Po nawiązaniu żądanie klienta dostępu do zasobu HTTPS chroniony sterownik agenta punktu końcowego przechwytuje połączenia i ustala nowe połączenie na serwerze docelowym do pobiera jego certyfikatu SSL w imieniu klienta. Agent następnie sprawdza, czy certyfikat mogą ufać (Sprawdzanie, czy nie został odwołany i wykonywanie innych testy certyfikat), a jeśli te przebiegu agentem punktu końcowego, następnie kopiuje te informacje z certyfikatu serwera i tworzy własny certyfikat serwera — znane jako certyfikat zatrzymania — za pomocą tych informacji. Certyfikat zatrzymania jest podpisanego na bieżąco przez agenta punktu końcowego za pomocą certyfikatu głównego, który jest zainstalowany w magazynie certyfikatów zaufanych systemu Windows. Ten certyfikat z podpisem własnym głównego jest oznaczony jako nie można eksportować i list ACL skorzystania z administratorów. Jest ona przeznaczona do nigdy nie pozostaw na komputerze, na którym został utworzony. Gdy aplikacja klienta użytkownika końcowego odbiera certyfikat zatrzymania, go będą Ufaj ponieważ pomyślnie można sprawdzić poprawność łańcuch certyfikatów Dosunięty do certyfikatu głównego. Ten proces jest głównie przezroczysty z użytkownika końcowego punktu widzenia z kilku ostrzeżenia w sposób opisany poniżej.

Włączając dokładną kontrolę, Agent punktu końcowego odnajdowania aplikacji chmury odszyfrowywanie i Przeprowadź inspekcję TLS szyfrowane komunikacji, umożliwiając usługi w celu zmniejszenia hałasu i podaj więcej informacji o korzystaniu z aplikacji szyfrowanego chmury.

#### <a name="a-word-of-caution"></a>Wyraz Uwaga
Przed włączeniem dokładną kontrolę, zdecydowanie zalecane jest komunikowanie się zamiaru prawnych i działy HR i uzyskać zgodę. Sprawdzanie prywatnej komunikacji szyfrowanego użytkownika końcowego może być tematem poufnych oczywistych powodów. Przed produkcji wdrożenie głębokości inspekcji wprowadź pewność, że zabezpieczeń firmy, a zasady dopuszczalnego używania zostały zaktualizowane oznaczającą będzie poddanych komunikacji zaszyfrowanej. Powiadomienie użytkownika i zwolnienie witryny uznane za poufne (np. bankowego i medycznych witryny) również może być konieczne, jeśli skonfigurowano odnajdowania aplikacji chmury monitorowanie ich. Jak wcześniej wspomniano, Administratorzy umożliwia portalu chmury odnajdowania aplikacji wybierz, czy informować użytkowników o zbierania danych przez agenta i czy wymagać zgody użytkownika przed uruchomieniem agent zbieranie danych użytkownika.

### <a name="known-issues-and-drawbacks"></a>Znane problemy i wad
Istnieje kilka spraw, gdzie zatrzymania TLS mogą mieć wpływ na środowisko użytkownika końcowego:

- Certyfikaty z rozszerzonym sprawdzaniem poprawności (EV) renderowania zielony ma pełnić rolę wizualny sygnał, że są odwiedzania zaufanej witryny sieci web na pasku adresu przeglądarki sieci web. TLS inspekcji nie zduplikowany ww w certyfikat, który problemy do klienta, aby witryn sieci web, które korzystania z certyfikatów EV działa normalnie, ale na pasku adresu nie są wyświetlane zielony.  

- Publicznej klucza przypinanie (również znany jako przypinanie certyfikat) zaprojektowano ułatwiających ochronę komputera przed atakami w pośrednik oraz rozpoczęcie urzędów certyfikacji. Gdy certyfikat główny przypięty witryny nie ma odpowiednika wśród z znanej dobrej urzędu certyfikacji, przeglądarce odrzuci połączenia z powodu błędu. Ponieważ TLS zatrzymania jest w rzeczywistości w pośrednik, te połączenia nie powiedzie się.

- Jeśli użytkownik kliknie przycisk ikona blokady w przeglądarce pasek adresu przeglądarki, aby sprawdzić informacje o witryny, nie zobaczą łańcuch kończący się urząd certyfikacji, używane do podpisania certyfikatu witryny sieci Web, ale zamiast tego łańcucha certyfikatów kończące się literami okna zaufane magazyn certyfikatów.

Aby zmniejszyć liczbę tych problemów, firma Microsoft aktualizowanie informacji o usług w chmurze i aplikacje klienckie znane za pomocą rozszerzonego sprawdzania poprawności lub przypięciu kluczy publicznych i agenta punktu końcowego w celu uniknięcia przechwytywaniu dotkniętej problemem połączenia. Nawet w takich przypadkach jednak odbiorca nadal otrzymuje raporty te aplikacje w chmurze i wielkość przesyłane dane ale ponieważ nie są one głębokości inspekcji, Brak szczegółów informacji na temat używania aplikacji będzie dostępna.


## <a name="sending-data-to-cloud-app-discovery"></a>Wysyłanie danych do chmury odnajdowania aplikacji

Po metadanych zebrane przez agenta, przez jedną minutę lub aż osiągnie zapisujące dane o rozmiarze 5MB jest buforowana na komputerze. Następnie jest skompresowany i przesyłane bezpiecznego połączenia z usługą odnajdowania aplikacji chmury.

W przypadku nie może nawiązać połączenia z usługą odnajdowania aplikacji chmury dowolnego powodu agent zebrane metadane są przechowywane w pamięci podręcznej plik lokalny, który jest możliwy tylko przez użytkowników uprzywilejowanych na komputerze (na przykład do grupy Administratorzy). <br>
Agent próbuje automatycznie ponowne wysyłanie metadane w pamięci podręcznej, dopóki pomyślnie otrzymano przez usługę odnajdowania aplikacji chmury.



## <a name="receiving-the-data-at-the-service-end"></a>Odbieranie danych na końcu usługi

Agentów uwierzytelniania odnajdowania aplikacji chmury usługi przy użyciu certyfikatu uwierzytelniania klienta określonego komputera powyżej i przesyła dane przez zaszyfrowanego kanału. <br>
Potok analizy usługi odnajdowania aplikacji chmury przetwarza metadane dla każdego z klientów oddzielnie logicznie podział na wszystkich etapach procesu analizy.
Metadane analizowanych dysków różnych raportów w portalu.

Nieprzetworzonych metadanych i analizowanych metadanych są przechowywane w 180 dni. Ponadto klienci mogą być przechwytywanie analizowanych metadanych na koncie magazyn obiektów blob platformy Azure ich wybierania.
To jest przydatne w przypadku analiza offline metadanych, a także dłuższy przechowywania danych.

## <a name="accessing-the-data-using-the-azure-portal"></a>Uzyskiwanie dostępu do danych za pomocą portalu Azure

W celu zabezpieczenia metadanych zebrane domyślnie tylko globalnej Administratorzy dzierżawy mają dostęp do funkcji odnajdowania aplikacji w chmurze w portalu Azure. <br>
Jednak administratorzy mogą być pełnomocnictw, aby inni użytkownicy lub grupy.



> [AZURE.NOTE] Aby uzyskać więcej informacji zobacz [Uzyskiwanie pracę z chmury aplikacji odnajdowanie](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Każdy użytkownik dostęp do danych w portalu, musi mieć licencję z licencją Azure AD Premium.



##<a name="additional-resources"></a>Dodatkowe zasoby


* [Jak odkryć chmury unsanctioned aplikacje, które są używane w organizacji](active-directory-cloudappdiscovery-whatis.md)
* [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
