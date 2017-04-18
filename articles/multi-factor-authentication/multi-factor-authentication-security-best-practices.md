<properties 
    pageTitle="Najważniejsze wskazówki dotyczące zabezpieczeń dotyczące korzystania z Azure MFA"
    description="W tym dokumencie znajdują wskazówkach wokół Azure MFA za pomocą konta Azure"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Najważniejsze wskazówki dotyczące zabezpieczeń dla uwierzytelnianie wieloskładnikowe Azure za pomocą konta Azure AD

Jeśli chodzi o ulepszanie procesu uwierzytelniania, uwierzytelnianie wieloskładnikowe jest preferowany wybór dla większości organizacji. Uwierzytelnianie wieloskładnikowe Azure (MFA) umożliwia firmom wymagań ich zabezpieczenia i zgodność a zapewnia prosty obsługi logowania użytkowników. W tym artykule opisano niektóre najważniejsze wskazówki, które należy wziąć pod uwagę podczas planowania przyjęcia Azure MFA.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Najważniejsze wskazówki dotyczące uwierzytelnianie wieloskładnikowe Azure w chmurze
Aby udostępnić wszystkich użytkowników uwierzytelnianie wieloskładnikowe i wykonać zalety rozszerzone funkcje, które oferuje uwierzytelnianie wieloskładnikowe Azure, należy włączyć uwierzytelnianie wieloskładnikowe Azure dla wszystkich użytkowników.  Jest to osiągnąć przy użyciu jednej z następujących czynności:

- Azure AD Premium lub pakietu mobilności przedsiębiorstwa
- Dostawca uwierzytelniania wieloskładnikowego

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Pakiet mobilności Azure AD Premium przedsiębiorstwa

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Pierwszym krokiem zalecane przyjmując MFA Azure w chmurze za pomocą Azure AD Premium lub Enterprise Suite mobilności jest przypisywanie licencji do użytkowników.  Azure uwierzytelnianie wieloskładnikowe jest częścią te zestawy i jako takie Twoja organizacja nie musi nic więcej, aby rozszerzyć możliwości uwierzytelnianie wieloskładnikowe dla wszystkich użytkowników.

Podczas konfigurowania uwierzytelnianie wieloskładnikowe należy rozważyć następujące czynności:

- Nie musisz utworzyć dostawcę uwierzytelnianie wieloskładnikowe.  Azure AD Premium i Enterprise Suite mobilności zawiera uwierzytelnianie wieloskładnikowe Azure.  Jeśli tworzysz dostawcą uwierzytelniania można uzyskać podwójne zafakturowane.
- Narzędzie Azure AD Connect jest tylko wymagane, jeśli synchronizujesz środowiska usługi Active Directory w lokalnej z katalogu Azure AD. Jeśli używasz wyłącznie katalogu Azure AD, która nie jest synchronizowana z wystąpieniem lokalnej usługi Active Directory, nie potrzebują Azure AD Connect.


### <a name="multi-factor-auth-provider"></a>Dostawca uwierzytelniania wieloskładnikowego

![Dostawca uwierzytelniania wieloskładnikowego](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Jeśli nie masz Azure AD Premium lub Enterprise Suite mobilności najpierw zalecane dla przyjęcia MFA Azure w chmurze jest utworzenie z dostawcą uwierzytelniania MFA. Mimo że MFA jest domyślnie dostępne dla globalnej administratorów, którzy mają usługi Azure Active Directory w przypadku wdrażania MFA dla swojej organizacji należy rozszerzyć możliwości uwierzytelnianie wieloskładnikowe, aby wszyscy użytkownicy i wykonaj potrzebnych dostawcy uwierzytelnianie wieloskładnikowe.
Podczas wybierania dostawcy uwierzytelniania, musisz wybrać katalog i rozważyć następujące kwestie:

- Katalog Azure AD, aby utworzyć dostawcę uwierzytelnianie wieloskładnikowe nie ma potrzeby.
- Należy skojarzyć dostawcy uwierzytelnianie wieloskładnikowe katalogu Azure AD Jeśli chcesz obejmowały uwierzytelnianie wieloskładnikowe wszystkich użytkowników i/lub być administratorami globalnego, aby można było korzystać z funkcji, takich jak portalu zarządzania, niestandardowe powitania i raportów.
- DirSync lub AAD Sync są wymagane tylko, jeśli synchronizujesz środowiska usługi Active Directory w lokalnej z katalogu Azure AD. Jeśli używasz tylko katalogu Azure AD, która nie jest synchronizowana z wystąpieniem lokalnej usługi Active Directory, nie muszą DirSync lub AAD Sync.
- Jeśli masz Azure AD Premium lub Enterprise Suite mobilności, nie musisz utworzyć dostawcę uwierzytelnianie wieloskładnikowe. Należy przypisać użytkownikowi licencji, a następnie Rozpocznij włączanie MFA dla użytkowników.
- Pamiętaj wybrać model prawo zastosowania dla swojej firmy (na auth lub na włączone użytkownika), po wybraniu modelu zastosowania nie można zmienić go.

### <a name="user-account"></a>Konto użytkownika
Podczas włączania MFA dla użytkowników, w jednym z trzech stanów podstawowych mogą być konta użytkowników: wyłączone, włączony lub wymuszane.
Upewnij się, że korzystasz z najbardziej odpowiednią opcję rozmieszczania za pomocą poniższych wskazówek:

- Gdy stan użytkownika jest ustawiony na wyłączone, użytkownik nie korzysta z uwierzytelnianie wieloskładnikowe. Jest to stan domyślny.
- Stanu użytkowników ma wartość włączone, oznacza, że użytkownik jest włączona, ale nie została ukończona procesu rejestracji. Aby ukończyć proces w następnym logowania zostanie wyświetlony monit. To ustawienie nie wpływa na aplikacji innych niż przeglądarki. Wszystkie aplikacje będą nadal działać, dopóki nie zakończy się procesu rejestracji.
- Gdy stan użytkownika jest równa wymuszoną, oznacza to, że użytkownik może lub nie została ukończona rejestracji. Jeśli zakończeniu procesu rejestracji ta osoba używa uwierzytelnianie wieloskładnikowe. W przeciwnym razie użytkownik zostanie wyświetlony monit, aby ukończyć proces rejestracji na następnej logowania. To ustawienie wpływa na aplikacji innych niż przeglądarki. Te aplikacje nie będzie działać, dopóki hasła aplikacji są tworzone i używane.
- Szablon użytkownika powiadomienie o dostępnych w artykule [wprowadzenie uwierzytelnianie wieloskładnikowe Azure w chmurze](multi-factor-authentication-get-started-cloud.md) do wysyłania wiadomości e-mail dla użytkowników dotyczące wdrażania MFA.

### <a name="supportability"></a>Obsługą

Ponieważ przywykli większość użytkowników za pomocą hasła tylko do uwierzytelnienia, należy pamiętać, że firmy Przesuń Sygnalizacja dostępności dla wszystkich użytkowników dotyczące tego procesu. Ten Sygnalizacja dostępności można zmniejszyć prawdopodobieństwo, że użytkownicy nawiąże połączenie z pomocą techniczną dla drobne problemy związane z MFA.
Istnieją jednak pewne scenariusze, gdy jest konieczne tymczasowe wyłączanie MFA. Opis sposobu obsługi tych scenariuszy za pomocą poniższych wskazówek:

- Upewnij się, że z działem pomocy technicznej są szkolenia scenariuszy uchwyt miejsce, w którym aplikacji dla urządzeń przenośnych lub telefon nie odbiera powiadomienie lub rozmowy telefonicznej i z tego powodu, których użytkownik jest nie można się zalogować. Może udostępnić opcję jednorazowego obejścia, aby zezwolić na uwierzytelnianie pomijając"" uwierzytelnianie wieloskładnikowe tylko raz. Obejście jest tymczasowy i wygasa po określonej liczbie sekund.
- W razie potrzeby możesz wykorzystać możliwości zaufane adresy IP w Azure MFA. Ta funkcja umożliwia administratorom dzierżawy zarządzanej lub federacyjnych możliwość obejścia uwierzytelnianie wieloskładnikowe dla użytkowników, którzy są zalogowaniu się z lokalny intranet firmy. Funkcje są dostępne dla dzierżaw Azure AD, które mają licencje Azure AD Premium, Enterprise mobilności Suite lub uwierzytelnianie wieloskładnikowe Azure.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Najważniejsze wskazówki dotyczące Azure uwierzytelnianie wieloskładnikowe lokalnego
Jeśli firma je wykorzystać własnej infrastruktury MFA, będzie konieczne wdrażanie serwera uwierzytelnianie wieloskładnikowe Azure lokalnego. Składniki serwera MFA są wyświetlane na poniższej ilustracji:

![Dostawca uwierzytelniania wieloskładnikowego](./media/multi-factor-authentication-security-best-practices/server.png)
*nie zainstalowano domyślnie ** zainstalowany, ale nie jest włączony domyślnie


Serwer uwierzytelniania wieloskładnikowego Azure może służyć do bezpiecznego chmurze zasobów i zasobów lokalnych, które są dostępne dla kont Azure AD.  Jednak to może tylko osiągnąć przy użyciu federacji.  Oznacza to, że musisz mieć usług AD FS i go do Federacji dzierżawy usługi Azure AD.
Podczas konfigurowania serwera uwierzytelnianie wieloskładnikowe należy rozważyć następujące czynności:

- Jeśli jesteś zabezpieczanie zasobów Azure AD przy użyciu Active Directory Federation Services, a następnie odbywa się współczynnik 1st uwierzytelniania lokalnego przy użyciu usług AD FS i współczynnik 2 jest wykonywana w lokalnej przy zachowaniu roszczenia.
- Nie jest wymagane serwer uwierzytelniania wieloskładnikowego Azure zainstalowania na serwerze Federacja usług AD FS jednak karta uwierzytelnianie wieloskładnikowe usług AD FS musi być zainstalowany w systemie Windows Server 2012 R2 uruchamiania usług AD FS. Można zainstalować serwer na innym komputerze, o ile jest to obsługiwane wersja i instalowanie karty usług AD FS oddzielnie na serwerze Federacja usług AD FS. Zobacz procedurę poniżej, aby uzyskać instrukcje dotyczące instalowania karty oddzielnie.
- Uwierzytelnianie wieloskładnikowe AD FS karty Kreatora instalacji tworzy grupę zabezpieczeń o nazwie PhoneFactor administratorów w usługi Active Directory, a następnie dodaje konto usługi usług AD FS usługi federacyjne do tej grupy. Zaleca się sprawdzenie kontrolera domeny grupy administratorów PhoneFactor faktycznie zostanie utworzona, a konto usługi usług AD FS jest członkiem tej grupy. W razie potrzeby dodaj konto usługi usług AD FS do grupy Administratorzy PhoneFactor kontrolera domeny ręcznie.

### <a name="user-portal"></a>Portal użytkownika
Ten portal jest uruchamiany w witrynie sieci web Internet Information Server (IIS), dzięki któremu Samoobsługowe funkcje, a zawiera pełny zestaw funkcji Administracja użytkownika. Konfigurowanie tego składnika przy użyciu poniższych wskazówek:

- Wymagany jest program IIS 6 lub nowszego
- ASP.NET v2.0.507207 muszą być zainstalowane i zarejestrowane
- Ten serwer mogą być rozmieszczone w sieci obwód kształtu.



### <a name="app-passwords"></a>Hasła aplikacji
Jeśli Twoja organizacja federacji z Azure AD przy użyciu logowania jednokrotnego i zamierzasz korzystać z Azure MFA, następnie należy pamiętać o następujących za pomocą haseł aplikacji (należy pamiętać, że dotyczy to tylko federacyjnych (SSO) jest używany):

- Hasło aplikacji został zweryfikowany przez Azure AD i w związku z tym pomija federacji. Federacja jest używane tylko podczas konfigurowania hasło aplikacji.
- Federacyjnych (SSO) będą przechowywane hasła użytkowników w polu Identyfikator organizacji. Jeśli użytkownik opuści firmy, tych informacji ma do identyfikator organizacji przy użyciu DirSync w czasie rzeczywistym. Usuwanie Wyłącz konta może potrwać do 3 godzin, aby zsynchronizować, opóźnienia Wyłącz/usuwanie hasła aplikacji w Azure AD.
- Ustawienia kontroli dostępu klienta lokalnego nie są uznane za hasła aplikacji
- Uwierzytelnianie lokalnego rejestrowania / inspekcja funkcja jest dostępna dla hasło aplikacji
- Więcej użytkowników końcowych education jest wymagana dla klienta programu Microsoft Lync 2013.
- Niektóre zaawansowane projektów architektura może wymagać przy użyciu kombinacji organizacji nazwy użytkownika i hasła oraz hasła aplikacji, używając uwierzytelnianie wieloskładnikowe z klientami, w zależności od tego, gdzie uwierzytelniania. Dla klientów, którzy uwierzytelnić infrastrukturę lokalnych należy użyć organizacji nazwy użytkownika i hasła. Dla klientów, którzy uwierzytelnić Azure AD należy użyć hasło aplikacji.
- Domyślnie użytkownicy nie można utworzyć hasła aplikacji, jeśli wymaga Twojej firmy lub jeśli chcesz zezwolić użytkownikom na tworzenie hasła aplikacji w niektórych scenariuszach, upewnij się, że wybrano opcję Zezwalaj użytkownikom na tworzenie hasła aplikacji, aby zalogować się do aplikacji innych niż przeglądarki.

## <a name="additional-considerations"></a>Uwagi dodatkowe
Użyj poniższej listy, aby dowiedzieć się, że niektóre zagadnienia dodatkowe oraz najlepsze rozwiązania dla każdego składnika, który będzie używany w lokalnej:

Metoda|Opis
:------------- | :------------- |
[Usługi federacyjne Active Directory](multi-factor-authentication-get-started-adfs.md)|Informacje na temat konfigurowania uwierzytelnianie wieloskładnikowe Azure z usług AD FS.
[RADIUS Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure o PROMIENIU.
[Uwierzytelnianie usług IIS](multi-factor-authentication-get-started-server-iis.md)|Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą usług IIS.
[Authenticaton systemu Windows](multi-factor-authentication-get-started-server-windows.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą uwierzytelniania systemu Windows.
[Uwierzytelnianie LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą uwierzytelniania LDAP.
[Zdalny pulpit bramy i serwer uwierzytelniania wieloskładnikowego Azure za pomocą RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą bramy usług pulpitu zdalnego przy użyciu usługi RADIUS.
[Synchronizacja z usługą Active Directory systemu Windows Server](multi-factor-authentication-get-started-server-dirint.md)|Informacje dotyczące konfigurowania i Konfigurowanie synchronizacji między usługą Active Directory i serwer Azure MFA.
[Wdrażanie usługi sieci Web dla urządzeń przenośnych aplikacji Server Azure uwierzytelnianie wieloskładnikowe](multi-factor-authentication-get-started-server-webservice.md)|Informacje dotyczące konfigurowania i konfigurowanie usługi sieci web server Azure MFA.
[Konfigurowanie zaawansowanych VPN za pomocą Azure uwierzytelnianie wieloskładnikowe](multi-factor-authentication-advanced-vpn-configurations.md)|Informacje na temat konfigurowania urządzeń firmy Cisco ASA, Citrix Netscaler i Juniper-Pulse bezpiecznego VPN przy użyciu protokołu LDAP lub RADIUS.


## <a name="additional-resources"></a>Dodatkowe zasoby
W tym artykule są wyróżniane niektóre z najważniejszych wskazówek dla Azure MFA, wiążą się inne zasoby, które umożliwia także podczas planowania wdrożenia MFA. Poniższa lista obejmuje niektóre kluczowe artykułów, które mogą pomóc w trakcie tego procesu:

- [Raporty w Azure uwierzytelnianie wieloskładnikowe](multi-factor-authentication-manage-reports.md)
- [Konfigurowanie obsługi uwierzytelniania wieloskładnikowego Azure](multi-factor-authentication-end-user-first-time.md)
- [Uwierzytelnianie wieloskładnikowe Azure — często zadawane pytania](multi-factor-authentication-faq.md)
