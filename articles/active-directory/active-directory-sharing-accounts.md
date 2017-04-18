<properties
    pageTitle="Udostępnianie konta przy użyciu Azure AD |  Microsoft Azure"
    description="W tym artykule opisano, jak usługi Azure Active Directory umożliwia organizacjom bezpieczne udostępnianie kont dla lokalnego aplikacji i usług w chmurze dla klientów indywidualnych."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Udostępnianie konta z usługą Azure Active Directory

## <a name="overview"></a>Omówienie
Czasami trzeba używać jednej nazwy użytkownika i hasła dla wielu osób organizacji. Dzieje się zazwyczaj w dwóch przypadkach:

- Podczas uzyskiwania dostępu do aplikacji, które wymagają Unikatowy identyfikator logowania i hasło dla każdego użytkownika, czy dla klientów indywidualnych lub lokalnej aplikacji usług w chmurze (np. konta firmowe społecznościowych).
- Podczas tworzenia środowiska wielu użytkowników. Może być jednym, lokalnego konta, które pełnymi uprawnieniami i może służyć podstawowych czynności konfiguracji, administrowanie i odzyskiwania (np. konta lokalne "administratora globalnego" dla usługi Office 365) lub konta administratora w usług Salesforce.

Zazwyczaj te konta może zostać udostępniona przez rozpowszechniania poświadczeń (nazwy użytkownika i hasła) osobom prawo oraz umieszczanie ich w udostępnionej lokalizacji, w której wiele czynników zaufanych można uzyskiwać do nich dostęp.

Tradycyjny model udostępniania zawiera kilka wad:

- Umożliwianie dostępu do nowych aplikacji wymaga rozpowszechnianie poświadczeń dla wszystkich osób, których potrzebuje do niego dostępu.
- Każdy udostępnioną aplikację może wymagać własnej unikatowy zestaw poświadczeń udostępnionego, wymaganie od użytkowników do zapamiętania wiele zestawów poświadczeń. Jeśli użytkownicy mają do zapamiętania wiele poświadczeń, ryzyka zwiększa, że będzie odwołać do praktyk ryzykowna. (przykład zapisywania haseł w dół).
- Nie można sprawdzić, kto ma dostęp do aplikacji.
- Nie można sprawdzić, kto ma *dostęp do* aplikacji.
- Gdy trzeba usunąć dostęp do aplikacji, należy zaktualizować poświadczenia i ponownie dystrybucji ich do wszystkich osób, które wymagają dostępu do tej aplikacji.

## <a name="azure-active-directory-account-sharing"></a>Konta usługi Azure Active Directory i udostępnianie

Azure AD udostępnia nowe podejście do za pomocą udostępnionego konta, aby wyeliminować tych niedogodności.

Azure AD administrator konfiguruje aplikacji, które użytkownik może uzyskać dostęp za pomocą panelu dostępu i wybierając typ najważniejsze rejestracji jednokrotnej dopasowane do tej aplikacji. Jeden z tych typów, *oparte na hasło logowania jednokrotnego na*umożliwia Azure AD pełnić rolę rodzaju "broker" podczas procesu logowania w tej aplikacji.

Użytkownicy, zaloguj się raz za ich konta organizacji. To samo konto, które regularnie używać, aby uzyskać dostęp do ich pulpit lub adresu e-mail. Mogą odnajdowanie i dostęp tylko te aplikacje, które są przypisane do. Z kontami udostępnionych lista aplikacji może zawierać dowolną liczbę udostępnione poświadczenia. Użytkownik nie musi pamiętać lub zanotuj różne konta, które mogą korzystać.

Udostępnione konta nie tylko zwiększanie nadzorem i poprawić użyteczności, również zwiększyć bezpieczeństwo. Użytkownicy z uprawnieniami do używania poświadczeń nie widać wspólne hasło, ale zamiast uzyskać uprawnienia do jako część przepływ uwierzytelniania orchestrated przy użyciu hasła. Ponadto niektórych aplikacji rejestracji Jednokrotnej hasło użytkownik może być Azure AD okresowo przerzucania (Aktualizuj) hasło, za pomocą haseł dużych, złożonych zwiększania zabezpieczeń konta. Administrator może łatwo udzielić lub odebrać dostęp do aplikacji i również wiesz, kto ma dostęp do konta i kto dostępne w przeszłości.

Azure AD obsługuje udostępnionego konta dla dowolnego przedsiębiorstwa mobilności pakietu (EMS), Premium lub podstawowe liczby licencjonowanych użytkowników dla wszystkich rodzajów hasło pojedynczego logowanie w aplikacjach. Można udostępniać konta dla każdego tysiące wstępnie zintegrowane aplikacje w galerii aplikacji i dodać własne uwierzytelniania hasła aplikacji z [niestandardowe aplikacje logowania jednokrotnego](active-directory-sso-integrate-saas-apps.md).

Azure AD funkcje, które umożliwiają udostępnianie konta:

- [Hasło logowania jednokrotnego](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Hasło pojedynczego logowania jednokrotnego agenta
- [Przypisanie grupy](active-directory-accessmanagement-self-service-group-management.md)
- Aplikacje niestandardowe hasła
- [Raporty pulpitu nawigacyjnego zastosowania aplikacji](active-directory-passwords-get-insights.md)
- Portali dostępu użytkownika końcowego
- [Serwer proxy aplikacji](active-directory-application-proxy-get-started.md)
- [Marketplace usługi Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Udostępnianie konta
Aby użyć Azure AD udostępnianie konta, które mają być:

- Dodawanie aplikacji [galerii aplikacji](https://azure.microsoft.com/marketplace/active-directory/) lub [aplikacji niestandardowej](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfigurowanie aplikacji dla hasło pojedynczego logowania jednokrotnego (SSO)
- [Grupa oparta przydział](active-directory-accessmanagement-group-saasapps.md) i wybierz opcję, aby wprowadzić poświadczenia udostępnionych
- Opcjonalnie: w niektórych aplikacjach, takich jak Facebook, Twitter i LinkedIn, można włączyć opcję [przerzucania hasła zautomatyzowanego Azure AD](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Możesz również wykorzystać konta udostępnionej bezpieczniejsze z uwierzytelnianie wieloskładnikowe (MFA) (Dowiedz się więcej na temat [Zabezpieczenia aplikacji z usługą Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started.md)) i możesz delegować możliwość zarządzania, kto ma dostęp do aplikacji przy użyciu [Azure AD Samoobsługowe](active-directory-accessmanagement-self-service-group-management.md) Zarządzanie grupy.

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Ochrona aplikacji programu access warunkowe](active-directory-conditional-access.md)
- [Grupa samodzielne zarządzanie SSAA](active-directory-accessmanagement-self-service-group-management.md)
