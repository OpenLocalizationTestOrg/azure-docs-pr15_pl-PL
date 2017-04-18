<properties
    pageTitle="W obszarze roboczym użytkownika za pomocą urządzeń z systemem Windows 10 | Microsoft Azure"
    description="Udostępnia migawkę funkcje dla użytkowników i IT, kontrastującym różne sposoby urządzenia można obsługi administracyjnej i używane w przedsiębiorstwie z systemu Windows 10."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>W obszarze roboczym użytkownika za pomocą urządzeń z systemem Windows 10

Dotyczy: komputery z systemem Windows 10

Windows 10 oferuje trzy modele dla organizacji, które umożliwiają użytkownikom uzyskiwanie dostępu do zasobów pracy w bezpieczny i wygodny sposób.

- **Dołączanie do usługi Azure Active Directory** (Azure AD połączenie) dla pracowników, którzy uzyskać dostęp do zasobów, takich jak usługi Office 365 przede wszystkim w chmurze. Azure AD jest to samodzielne pracy inicjowania obsługi administracyjnej środowisko, która jest nowa w systemie Windows 10.
- **Dołączanie domeny**, dla organizacji, które mają inwestycji w lokalnej aplikacji i zasobów. Dołączanie domeny oferuje ulepszone środowisko w systemie Windows 10, gdy połączenia Azure AD.
- **Nowe środowisko BYOD uproszczone**, dla użytkowników, którzy chcą dodać konto pracy lub szkole do systemu Windows i łatwo dostęp do urządzeń osobistych zasobów.

W poniższej tabeli przedstawiono migawkę funkcje dla użytkowników i administratorów IT, kontrastującym różne sposoby urządzenia można obsługi administracyjnej i używane w przedsiębiorstwie z systemu Windows 10:

|                                                                                                                                                                 | Dołączanie do domeny     | Azure AD sprzężenia | Osobiste urządzenia |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows urządzenia logowania dla konta służbowego.                                                                                                                      | Tak             | Tak           | Brak              |
| Użytkownik jedno logowania jednokrotnego (SSO) dla aplikacji usługi Office 365 i Azure AD. Logowanie Jednokrotne jest możliwość do zalogowania się tylko raz uzyskać dostęp do zasobów organizacji. | Tak             | Tak           | Tak             |
| Użytkownik logowania jednokrotnego do aplikacji Kerberos i NTLM.                                                                                                                                  | Tak             | Ograniczone       | Tak, przez sieć VPN         |
| Silne autoryzacja i wygodny logowania dla konta służbowego z Microsoft Passport i Witaj systemu Windows.                                                                   | Tak             | Tak           | Tak             |
| Dostęp do wersji enterprise ze Sklepu Windows przy użyciu konta służbowego (nie konto Microsoft).                                                                                    | Tak             | Tak           | Tak             |
| Zgodny z przedsiębiorstwa mobilne ustawień użytkownika na urządzeniach z konta służbowego.                                                                                 | Tak             | Tak           | Tak             |
| Możliwość ograniczenia dostępu do aplikacji organizacji do urządzenia, które są zgodne z zasad organizacji urządzenia.                                                      | Tak             | Tak           | Tak             |
| Użytkownik Samoobsługowe inicjowania obsługi urządzeń do "Praca z dowolnego miejsca".                                                                                                | Brak              | Tak           | Tak             |
| Możliwość zarządzania urządzeniami.                                                                                                                                       | Tak — za pomocą zasad grupy i SCCM | Tak           | Tak             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Użyj urządzenia należące do pracy z Azure AD sprzężenia i domeny dołączanie w systemie Windows 10

Windows 10 oferuje dwie metody należące do pracy urządzenia, aby uzyskać dostęp do zasobów pracy:

- Azure AD sprzężenia
- Dołączanie do domeny

 Oba może być prawidłowe opcje w zależności od potrzeb i wymagań danej organizacji. W niektórych przypadkach organizacji mogą korzystać z włączaniem obie metody wdrażania.

## <a name="when-to-use-azure-active-directory-join"></a>Kiedy należy używać Azure Active Directory dołączanie

Azure AD jest to nowych zadań Samoobsługowe inicjowania obsługi administracyjnej obsługi w systemie Windows 10.  Jest ona przeznaczona dla pracowników, którzy uzyskać dostęp do zasobów pracy, takich jak usługi Office 365 przede wszystkim w chmurze. Jest to najprostsze sposób konfigurowania komputerów, tabletów i telefonów w przedsiębiorstwie. Urządzenia muszą być zarządzane przez zarządzanie urządzeniami przenośnymi za pomocą kontrolek spójne na platformach Windows.

**Użyj Azure AD dołączanie dowolnego z następujących powodów**:

- Chcesz włączyć samoobsługowe inicjowania obsługi urządzeń dla pracowników w podróży.
- Użytkownikom dostarczać należące do pracy urządzeń przenośnych, takich jak tabletów i telefonów.
- Chcesz zarządzać zbiór użytkowników w Azure AD zamiast w usłudze Active Directory, takich jak sezonowy pracowników, wykonawcy lub uczniów lub studentów.
- Chcesz udostępnić pracownikom w zdalnych oddziałów infrastruktury ograniczone lokalnego możliwości łączenia.
- Nie masz usługi Active Directory w lokalnej.

Niektóre organizacje użyje Azure AD dołączanie jako podstawowego sposobem wdrożenia należące do pracy urządzenia, zwłaszcza w przypadku, gdy wykonują migrację większości lub wszystkich zasobów w chmurze. Organizacje hybrydowego z usługi Active Directory i Azure AD także możliwość wdrażanie jednym ze sposobów lub innego w zależności od użytkownika lub działu.

Okręgi szkole i uczelni, na przykład może zarządzać personelu w usłudze Active Directory i studentów w Azure AD. W niektórych firmach usunięcie może być zarządzania zdalnego oddziałów lub sprzedaży działów w Azure AD. W obrębie organizacji hybrydowego można używać zarówno Azure AD sprzężenia, jak i metody dołączania do domeny. Dołącz Azure AD może być doskonałe uzupełnienie Dołączanie domeny do wdrażania urządzeń w środowisku pracy.

**Jeśli jest zwykle do zasobów organizacji za pośrednictwem chmurki, Twoja organizacja może korzystają dodatkowe Jeśli**:

- Możesz usunąć zależności w lokalnej infrastrukturze tożsamości.
- Można uprościć modelu wdrożenia urządzeń, wprowadzenie poza obrazowania rozwiązań, umożliwiając samodzielne konfiguracji.
- Zarządzanie urządzeniami przenośnymi umożliwia zarządzanie wszystkich swoich urządzeniach na różnych platformach.

Aby uzyskać więcej informacji na temat Azure AD dołączanie zobacz [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Kiedy należy używać sprzężenia domeny (lub Zachowaj korzystania z niego)

W ostatnim roku 15 wiele organizacji korzystają z Dołączanie domeny do łączenia urządzeń pracy. Umożliwia użytkownikom logowanie się do jego urządzeń z pracą usługi Active Directory lub edukacyjne. Umożliwia dołączanie domeny IT centralnie i w pełni zarządzać tych urządzeń. Organizacje zwykle oparte na obrazowania metodach obsługę urządzeń i często za pomocą Menedżera konfiguracji centrum System (SCCM) lub zasady grupy (zasad grupy) do zarządzania nimi.

**Organizacji należy używać sprzężenia domeny (lub Zachowaj korzystania z niego) z jednego z następujących powodów**:

- Masz aplikacje Win32 wdrożony te urządzenia, które korzystają z NTLM i Kerberos.
- Wymagane zasad grupy lub SCCM i zarządzaniem żądaną konfiguracją zarządzać urządzeniami.
- Chcesz nadal korzystać rozwiązań obrazów do konfigurowania urządzeń dla pracowników.

**Dołączanie domeny w systemie Windows 10 umożliwia także następujące zalety połączenia Azure AD**:

- Silne uwierzytelnianie powiązanych z urządzenia i wygodny logowania dla konta służbowego z Microsoft Passport i Witaj systemu Windows.
- Dostęp do wersji enterprise ze Sklepu Windows dla urządzeń korzystających z pracy lub edukacyjne (nie wymagane konto Microsoft).
- Zgodny z przedsiębiorstwa mobilne ustawień użytkownika na urządzeniach, które za pomocą konta służbowego (nie wymagane konto Microsoft).
- Możliwość ograniczenia dostępu do aplikacji organizacji do urządzenia, które są zgodne z zasad organizacji urządzenia.

Aby uzyskać więcej informacji na temat Azure AD dołączanie zobacz [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Włączanie dołączania osobiście posiadanych urządzeń w pracy lub szkole

Do obsługi BYOD w przedsiębiorstwie, systemu Windows 10 umożliwia użytkownikowi możliwość dodawania konta służbowego do ich komputerze, tablecie lub telefonie. Po użytkownik dodaje konto służbowe, urządzenie jest zarejestrowana z usługą Azure Active Directory i opcjonalnie zarejestrowana w systemie zarządzania urządzenia przenośnego skonfigurowanego przez organizację. Katalog zostanie wyświetlona tych urządzeniach jako "Zarejestrowane" a "Sprzężone Azure AD". IT administraors można stosować różne zasady na podstawie tych informacji, dostarczając jaśniejszy dotykowym na osobiście posiadanych urządzeń niż na urządzeniach należące do pracy w razie potrzeby.

Użytkowników można dodać służbowe konto służbowe do swoich osobistych urządzeń bardzo wygodne. Można to zrobić podczas uzyskiwania dostępu do aplikacji pracy po raz pierwszy lub ich zrobić to ręcznie za pomocą menu Ustawienia. To konto przedstawi logowania jednokrotnego zasobów organizacji.

Aby uzyskać więcej informacji na temat Azure AD dołączanie zobacz [napotkania urządzenia domeny Połącz, aby Azure AD dla systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Włączanie sprzężenia domeny lub Azure AD sprzężenia

Wszystkie opisanych wcześniej metod (Dołączanie domeny, Azure AD sprzężenia i Dodaj Praca konto służbowe) mają punktów wejścia w środowisku użytkownika systemu Windows 10. Jednak wymaga administrator IT włączyć funkcję w infrastrukturze przed środowisko będą działać.

## <a name="requirements-for-deploying-azure-ad-join"></a>Wymagania dotyczące wdrażania Azure AD dołączanie

Aby wdrożyć Azure AD dołączanie dla każdego zbioru użytkowników są potrzebne następujące elementy:

- Subskrypcję usługi Azure AD.
- Subskrypcja Azure AD Premium, takich jak urządzenia przenośnego zarządzania automatyczne rejestrowanie, jeśli jest wymagane więcej funkcji.
- Zarządzanie urządzeniami przenośnymi — na przykład subskrypcji Intune firmy Microsoft, zarządzanie urządzeniami przenośnymi dla usługi Office 365 lub wszystkich dostawców zarządzania urządzenia przenośnego partnerów, którzy Integracja z usługą Azure Active Directory. (Zobacz [sekcja często zadawanych PYTAŃ](#frequently-asked-questions) na końcu tego artykułu, aby uzyskać więcej informacji).

Jeśli ośrodki hybrydowe, zaleca się wdrożenie narzędzie Azure AD Connect rozszerzenie katalogu lokalnego Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Wymagania dotyczące używania Dołączanie domeny z usługą Azure Active Directory

Dołączanie domeny będzie nadal działać jest zawsze dostępne. Jednak aby uzyskać korzyści Azure AD będą potrzebne następujące elementy:

- Subskrypcję usługi Azure AD.
- Rozmieszczanie narzędzie Azure AD Connect rozszerzenie Azure AD katalogu lokalnego.
- Zasady, które umożliwia domeny urządzeń warunkowego dostępu do Azure AD.
- Zasady, które umożliwia uzyskanie dostępu do urządzenia "sprzężone domeny", jeśli chcesz można było ograniczyć dostęp w przypadku niektórych urządzeń.
- Menedżer konfiguracji centrum systemu wersji 1509 dla Technical Preview, aby włączyć zasady wymagające zgodnych urządzeń. (Zobacz dokumentacja TechNet i wpis w blogu).

Aby uzyskać więcej informacji na temat dołączania do domeny w systemie Windows 10 zobacz [napotkania urządzenia domeny Połącz, aby Azure AD dla systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Wymagania dotyczące korzystania z BYOD i "Dodaj konto służbowe"

Aby włączyć "wprowadzić własne urządzenia" (BYOD) przy użyciu konta służbowego, potrzebne następujące czynności:

- Subskrypcję usługi Azure AD.
- Subskrypcja Azure AD Premium, takich jak urządzenia przenośnego zarządzania automatyczne rejestrowanie, jeśli jest wymagane więcej funkcji.

## <a name="requirements-for-using-microsoft-passport"></a>Wymagania dotyczące korzystania z programu Microsoft Passport

Aby włączyć Microsoft Passport, będą potrzebne następujące elementy:

- Infrastruktury publicznych () do obsługi uwierzytelniania opartego na certyfikat, który używa Microsoft Passport.
- Subskrypcja usługi korzystającego z konta służbowego i Microsoft Passport dla Azure AD dołączanie do obsługi uwierzytelniania opartego na certyfikat.
- Menedżer konfiguracji systemu Centrum wersji 1509 dla Technical Preview (zobacz dokumentacja TechNet i wpis w blogu) do obsługi uwierzytelniania opartego na certyfikacie, używającej Microsoft Passport dla domeny sprzężenia.
- Zasady dotyczące włączania Microsoft Passport w organizacji.

Zamiast infrastruktury kluczy publicznych oparte na kluczach Microsoft Passport można włączyć, wykonując następujące czynności:

- Wdrożenie systemu Windows Server 2016 "Produkcji Podgląd 1" DCs (bez potrzeby dla domeny lub las poziomów funkcjonalności; kilka DCs do użycia nadmiarowości, który wystarczy każdej witryny usługi Active Directory).
- Ustawianie zasad, aby włączyć Microsoft Passport w organizacji.

Aby uzyskać więcej informacji na temat Microsoft Passport i Windows Witaj w systemie Windows 10 zobacz < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Często zadawane pytania
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Które produkty zarządzania urządzenia przenośnego partnera Integracja z usługą Azure Active Directory?

Następujące produkty dostawcy Integracja z usługą Azure Active Directory dla rejestrowania ujednolicony i warunkowego dostępu w systemie Windows 10:

- AirWatch przez VMware
- Citrix Xenmobile
- Lightspeed Mobile Manager
- Zarządzanie urządzeniami przenośnymi lokalnego SOTI
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Dołączanie informacje o miejscu pracy w systemie Windows 10?
Dołącz obszar roboczy został użyty w systemie Windows 8.1 umożliwiający BYOD. W systemie Windows 10 BYOD włączono za pośrednictwem "Dodaj służbowe lub szkolne konto" zgodnie z opisem wcześniej w tym dokumencie. Dla organizacji, które nie integracja ich zarządzania urządzeniami przenośnymi z usługą Azure Active Directory, użytkownicy mogą rejestrować urządzenia do zarządzania ręcznie za pomocą **ustawień** > **kont** > **pracy w programie access**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Użytkowników połączyć ze swojego konta Microsoft do konta domeny w systemie Windows 10?
Nie w systemie Windows 10. W systemie Windows 8.1 użytkowników urządzeń domeny może "połączyć" kont Microsoft (na przykład Hotmail, Live, Outlook, Xbox itd.) do swojego konta domeny, aby włączyć niektórych środowiska, takich jak logowania jednokrotnego usługi Live, użyj Sklepu Windows i mobilnego ustawień użytkownika na urządzeniach. W systemie Windows 10 została wycofana konta Microsoft "łączenie" funkcji. Użytkownik może dodawać co najmniej jednego konta Microsoft, jako kolejne konta, aby włączyć rejestracji Jednokrotnej dla klientów indywidualnych usług, takich jak Sklepu Windows. Można to zrobić w **ustawieniach** > **kont** > **konta**.

Użytkownicy, kto jest uaktualniana urządzeń z systemem Windows 8.1 domeny, a kto sprzedał konta Microsoft połączony, automatycznie uzyskuje dodana do listy kolejne konta, które korzystają z połączonego konta Microsoft.


## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
