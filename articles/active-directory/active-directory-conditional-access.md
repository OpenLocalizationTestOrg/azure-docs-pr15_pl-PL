<Properties
    pageTitle="Azure Active Directory dostępu warunkowego | Microsoft Azure"  
    description="Użyj kontroli dostępu warunkowego w usłudze Azure Active Directory, aby sprawdzić w określonych warunkach podczas uwierzytelniania w celu uzyskiwania dostępu do aplikacji."  
    services="active-directory"
    keywords="warunkowe dostęp do aplikacji, dostępu warunkowego z usługą Azure Active Directory, bezpiecznego dostępu do zasobów firmy, zasady dostępu warunkowego"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Warunkowego dostępu w usłudze Active Directory platformy Azure   

Funkcje kontroli w programie access warunkowe usługi Azure Active Directory (Azure AD) oferują prostych sposobów bezpiecznego zasobów w chmurze i lokalnego. Zasady dostępu warunkowego, takich jak uwierzytelnianie wieloskładnikowe chronić przed ryzykiem kradzież i poświadczenia phished. Inne zasadami dostępu warunkowego może pomóc w zabezpieczenie danych Twojej organizacji. Na przykład oprócz wymaga poświadczeń, może być zasady tego tylko urządzenia, które zostały zarejestrowane w systemie zarządzania urządzenia przenośnego, takich jak Microsoft Intune można uzyskać dostępu do usług poufnych Twojej organizacji.


## <a name="prerequisites"></a>Wymagania wstępne

Azure AD dostępu warunkowego to funkcja [Azure Active Directory Premium](http://www.microsoft.com/identity). Każda osoba mająca dostęp do aplikacji, która ma zasady dostępu warunkowego stosowane musi mieć licencję Azure AD Premium. Przedstawiono więcej informacji na temat wymagań licencyjnych w [raporcie licencji użytkownika](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Jak są wymuszane kontrola dostępu warunkowego  

Za pomocą pola Kontrola dostępu warunkowego w miejscu Azure AD sprawdza określonych warunków, można ustawić dla użytkownika uzyskać dostęp do aplikacji. Po access wymagania są spełnione, użytkownik został uwierzytelniony i dostęp do aplikacji.  

![Omówienie warunkowego dostępu](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Warunki

Poniżej przedstawiono warunki, które można dołączać do zasady dostępu warunkowego:

- **Członkostwo w grupach**. Sterowanie dostępem użytkownika na podstawie członkostwa w grupie.

- **Lokalizacja**. Użyj lokalizacji użytkownika uwierzytelnianie wieloskładnikowe wyzwalacz i sterujące blok, gdy użytkownik nie znajduje się w zaufanej sieci.

- **Platformy urządzenia**. Za pomocą platformy urządzenie, takie jak iOS, Android, systemu Windows Mobile lub systemu Windows, jako warunku stosowania zasad.

- **Urządzenie włączone**. Stan urządzenia włączone lub wyłączone, weryfikacji podczas oceny zasad urządzenia. Po wyłączeniu urządzenie utracone lub kradzież w katalogu nie można już spełniają wymagań zasad.

- **Ryzyka logowania i użytkownika**. Umożliwia [ochronę Azure AD tożsamości](active-directory-identityprotection.md) zasady dostępu warunkowego ryzyka. Zasady ryzyka dostępu warunkowego pomoc, nadaj ze względów bezpieczeństwa advance organizacji na podstawie zdarzenia ryzyka i nietypowego działania logowania.


## <a name="controls"></a>Formanty

Oto kontrolek, które można wymusić zasady dostępu warunkowego:

- **Uwierzytelnianie wieloskładnikowe**. Czy wymaga silnego uwierzytelniania za pośrednictwem uwierzytelnianie wieloskładnikowe. Za pomocą uwierzytelnianie wieloskładnikowe uwierzytelnianie wieloskładnikowe Azure lub przy użyciu dostawcy uwierzytelnianie wieloskładnikowe lokalnego używana w połączeniu z usługi Active Directory Federation Services (AD FS). Uwierzytelnianie wieloskładnikowe chroni zasoby przed dostępem nieautoryzowanych użytkowników może być uzyskały dostęp do poświadczeń prawidłowego użytkownika.

- **Blokowanie**. Możesz zastosować warunki, takie jak lokalizacja użytkownika w celu zablokowania dostępu użytkownika. Można na przykład zablokować dostęp, gdy użytkownik nie znajduje się w zaufanej sieci.

- **Zgodny z urządzenia**. Można ustawić zasady warunkowe dostępu na poziomie urządzenia. Można skonfigurować zasady tak, aby tylko komputerach domeny lub urządzeniach przenośnych, które są rejestrowane w aplikacji do zarządzania urządzenia przenośnego, można uzyskać dostęp do zasobów organizacji. Można na przykład umożliwia sprawdzenie zgodności urządzenia Intune i zgłosić go z Azure AD dla wymuszania gdy użytkownik będzie próbował uzyskać dostęp do aplikacji. Szczegółowe wskazówki na temat ochrony aplikacji i danych za pomocą usługi zobacz [Ochrona aplikacji i danych za pomocą usługi firmy Microsoft](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Za pomocą Intune możesz również wymusić ochrona danych dla urządzeń utraconych lub kradzież. Aby uzyskać więcej informacji zobacz [Ochrona danych za pomocą pełnego i selektywnego czyszczenia przy użyciu usługi Microsoft](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Aplikacje

Można wymusić zasady warunkowego dostępu na poziomie aplikacji. Ustawianie poziomów dostępu dla aplikacji i usług w chmurze lub lokalnego. Zasady są stosowane bezpośrednio do witryny sieci Web lub usługi. Zasady są wymuszane uzyskać dostęp do przeglądarki i aplikacje, które dostęp do usługi.


## <a name="device-based-conditional-access"></a>Oparte na urządzeniu dostępie warunkowym

Można ograniczyć dostęp do aplikacji na urządzeniach, które są rejestrowane z usługą Azure Active Directory i które spełniają określone warunki. Oparte na urządzeniu dostępie warunkowym uniemożliwia użytkowników, którzy będą próbować uzyskać dostęp do zasobów z zasobami organizacji:

- Nieznany lub niezarządzanej urządzenia.
- Urządzenia, które nie spełniają zasad zabezpieczeń skonfigurować w swojej organizacji.

Można ustawić zasady dotyczące następujące wymagania:

- **Domeny urządzenia**. Ustawianie zasad ograniczenia dostępu do urządzenia sprzężone w domenie usługi Active Directory w lokalnej, a które są rejestrowane również z usługą Azure Active Directory. Tych zasad dotyczą pulpitów, przenośnych i tabletów enterprise systemu Windows.
Aby uzyskać więcej informacji na temat Konfigurowanie automatycznej rejestracji urządzeń domeny z usługą Azure Active Directory zobacz [Konfigurowanie automatycznej rejestracji systemu Windows urządzeń domeny z usługi Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Zgodny z urządzenia**. Ustawianie zasad ograniczenia dostępu do urządzenia, które mają być oznaczane **zgodności** w katalogu systemu zarządzania. Tych zasad gwarantuje, że tylko urządzenia, które spełniają zasad zabezpieczeń, takich jak wymuszanie szyfrowania plików na urządzeniu mają dostęp. Za pomocą tych zasad ograniczenia dostępu z następujących urządzeń:

    - **Urządzenia z systemem Windows domeny**. Zarządzane przez System Centrum Menedżer konfiguracji (w bieżącym gałąź) wdrożony w konfiguracji hybrydowej.
    - **Praca w systemie Windows 10 Mobile lub urządzeń osobistych**. Zarządzane przez Intune lub systemu zarządzania obsługiwane urządzenia przenośnego innych firm.
    - **iOS i urządzeń z systemem Android**. Zarządza Intune.


Użytkownicy, którzy dostęp do aplikacji, które są chronione oparty na urządzeniach, zasady urzędu certyfikacji musi dostęp do aplikacji z urządzenia, która spełnia wymagania dotyczące tych zasad. Jeśli próba odmowa dostępu na urządzeniu, który nie spełnia wymagań dotyczących zasad.

Aby dowiedzieć się, jak skonfigurować zasady urząd certyfikacji opartych na urządzeniach w Azure AD zobacz [Ustawianie zasad opartych na urządzeniach dostępu warunkowego dla aplikacji związanych z usługi Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Zasoby

Zobacz następujące kategorie zasobów i artykuły, aby dowiedzieć się więcej o ustawianiu dostępu warunkowego dla Twojej organizacji.


### <a name="multi-factor-authentication-and-location-policies"></a>Zasady uwierzytelniania i lokalizacji wieloskładnikowego

- [Wprowadzenie do warunkowego dostępu do Azure AD połączonych aplikacjach w zależności od grupy, lokalizację i zasady uwierzytelnianie wieloskładnikowe](active-directory-conditional-access-azuread-connected-apps.md)
- [Aplikacje, które są obsługiwane](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Oparte na urządzeniu dostępie warunkowym

- [Ustawianie zasad oparte na urządzeniu dostępie warunkowym kontroli dostępu do aplikacji związanych z usługi Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md)
- [Konfigurowanie automatycznej rejestracji systemu Windows urządzeń domeny z usługi Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Rozwiązywanie problemów dostępu usługi Azure Active Directory](active-directory-conditional-access-device-remediation.md)
- [Zabezpieczanie danych na urządzeniach utraconych lub kradzież za pomocą Intune firmy Microsoft](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Ochrona zasobów opartych na logowania ryzyka

-   [Azure AD ochronę tożsamości](active-directory-identityprotection.md)

### <a name="next-steps"></a>Następne kroki

- [Dostępu warunkowego często zadawane pytania](active-directory-conditional-faqs.md)
- [Informacje techniczne](active-directory-conditional-access-technical-reference.md)
