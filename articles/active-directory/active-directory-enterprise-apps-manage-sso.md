<properties
    pageTitle="Pojedynczy Zarządzanie jednokrotnej dla aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Dowiedz się, jak nimi zarządzać rejestracji jednokrotnej dla aplikacji przedsiębiorstwa przy użyciu usługi Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Podgląd: Zarządzanie rejestracji jednokrotnej dla aplikacji przedsiębiorstwa w nowy portal Azure

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Portal Azure klasyczny](active-directory-sso-integrate-saas-apps.md)

Ten artykuł zawiera opis sposobu używania [Azure portal](https://portal.azure.com) do zarządzania ustawieniami Zaloguj się pojedynczego dla aplikacji, szczególnie te, które zostały dodane z [galerii aplikacji usługi Azure Active Directory (Azure AD)](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Środowisko zarządzania Azure AD dla rejestracji jednokrotnej jest obecnie w podglądzie publiczne, a w tym artykule opisano nowe funkcje, a także kilka czasowe ograniczenia, które będą w miejscu tylko w okresie Podgląd. [Co to jest w podglądzie?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Znajdowanie aplikacji w nowego portalu

Od 2016 wrzesień wszystkie aplikacje, które zostały skonfigurowane dla pojedynczego logowania jednokrotnego w katalogu, przez administratora katalogów przy użyciu [galerii aplikacji usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) wewnątrz [portal Azure klasyczny](https://manage.windowsazure.com), można teraz można wyświetlać i zarządzania nimi Azure portal.

Te aplikacje można znaleźć w sekcji **Aplikacje dla przedsiębiorstw** portal Azure, łącze, do której znajduje się na liście **Więcej usług** w [portalu](https://portal.azure.com). Aplikacji przedsiębiorstwa są aplikacje, które zostały wdrożone i są używane przez użytkowników w organizacji.

![Karta aplikacje dla przedsiębiorstw][1]

Zaznacz **wszystkie aplikacje** , aby wyświetlić listę wszystkich aplikacji, które zostały skonfigurowane, łącznie z aplikacjami, które dodano z galerii. Wybieranie aplikacji ładuje karta zasobów dla aplikacji, której raporty mogą być wyświetlane dla tej aplikacji i mogą być zarządzane różne ustawienia.

Aby zarządzać ustawieniami rejestracji jednokrotnej, zaznacz **rejestracji jednokrotnej**.

![Karta zasobu aplikacji][2]


##<a name="single-sign-on-modes"></a>Tryby rejestracji jednokrotnej

Karta **logowania jednokrotnego** zaczyna się od menu **Tryb** , dzięki czemu jednej trybu logowania jednokrotnego, należy skonfigurować. Opcje dostępne są następujące:

* **Znak opartego na** — ta opcja jest dostępna, gdy aplikacja obsługuje pełny federacyjnych rejestracji jednokrotnej z usługi Azure Active Directory przy użyciu protokołu SAML 2.0.

* **Znak na podstawie hasła** — ta opcja jest dostępna, jeśli Azure AD obsługuje formularza hasła, wypełniając dla tej aplikacji.

* **Logowanie połączone** - wcześniej znana pod nazwą "Istniejące rejestracji jednokrotnej", ta opcja umożliwia administratorom umieścić łącze do tej aplikacji w ich użytkownika Panel Azure AD dostępu lub usługi Office 365 uruchamianie aplikacji.

Aby uzyskać więcej informacji na temat trybów, zobacz [jak pojedynczy logowania jednokrotnego z pracą usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>Znak opartego na

Opcja **opartego na protokole SAML logowania na** Wyświetla kartę, która jest podzielony na cztery sekcje:

###<a name="domains-and-urls"></a>Domeny i adresy URL
To miejsce, w którym wszystkie szczegółowe informacje o aplikacji domeny i adresy URL zostaną dodane do katalogu Azure AD. Wszystkie dane wejściowe wymagane do przygotowania pracy rejestracji jednokrotnej aplikacji są wyświetlane bezpośrednio na ekranie, dlatego wszystkie dane wejściowe Opcjonalnie można wyświetlać, zaznaczając pole wyboru **Pokaż zaawansowane ustawienia adresu URL** . Zawiera pełną listę obsługiwanych danych wejściowych:

* **Zaloguj się na adres URL** — miejsce, w którym użytkownik przechodzi do logowania się w tej aplikacji. Jeśli aplikacja jest skonfigurowana do wykonywania usługi inicjowanych przez dostawcę rejestracji jednokrotnej na, a następnie, gdy użytkownik przechodzi do ten adres URL, dostawca usług nie przekierowywania niezbędne do Azure AD do uwierzytelniania i zaloguj się użytkownika. Jeśli to pole jest wypełniane, następnie Azure AD przy użyciu tego adresu URL uruchamianie aplikacji usługi Office 365 i Panel Azure AD dostępu. Jeśli to pole jest pominięty, a następnie Azure AD wykonuje zamiast tego dostawcy tożsamości-znak inicjowane na kiedy aplikacji jest uruchamiany z usługi Office 365, panelu Azure AD dostępu lub Azure AD pojedynczy logowania jednokrotnego adresu URL.

* **Identyfikator** - ten identyfikator URI należy jednoznacznie identyfikować aplikacji, dla których pojedyncze logowanie jest skonfigurowany. Jest to wartość Azure AD wysyła do stosowania SAML token jako parametr odbiorców, a aplikacja ma poprawność. Ta wartość pojawi się także jako identyfikator jednostki w dowolnym metadanych SAML określonej przez aplikację.

* **Adres URL odpowiedzi** — odpowiedź, która jest używany adres URL miejsce, w którym zamierza odbieranie SAML token aplikacji. Jest to także nazywane adres URL usługi dla klientów indywidualnych potwierdzenia (ACS). Po tych zostały wprowadzone, kliknij przycisk Dalej, aby przejść do następnego ekranu. Na tym ekranie zawiera informacje o co musi być skonfigurowane na stronie aplikacji, aby mógł zaakceptować token SAML z usługi Azure Active Directory.

* **Stan przekazywania** - stan przekazywania jest opcjonalny parametr, które mogą być pomocne informujące aplikację, gdzie przekierować użytkownika po zakończeniu uwierzytelniania. Zwykle jest to wartość prawidłowy adres URL w aplikacji, jednak niektóre aplikacje za pomocą inaczej tego pola (patrz aplikacji rejestracji jednokrotnej dotyczące dokumentacji, aby uzyskać szczegółowe informacje). Opcja Ustaw stan przekazywania jest nowej funkcji, który jest unikatowy nowy portal Azure.

###<a name="user-attributes"></a>Atrybuty użytkownika
Jest to miejsce, w którym administratorzy można wyświetlać i edytować atrybuty, które są wysyłane w token SAML Azure AD problemów z aplikacją poszczególnych użytkowników logują się w.

Dla pierwszej wersji preview atrybut tylko edytowalny obsługiwane jest atrybut **Identyfikator użytkownika** . Wartość tego atrybutu jest pole w Azure AD, który musi jednoznacznie identyfikować każdy użytkownik w tej aplikacji. Na przykład jeśli aplikacja została wdrożona przy użyciu "Adres E-mail" jako nazwę użytkownika i unikatowy identyfikator, następnie będzie można wartość pola "user.mail" w Azure AD.

Edytowanie atrybutów dodatkowych będzie obsługiwana w kolejnych preview.

###<a name="saml-signing-certificate"></a>Certyfikat podpisywania SAML
W tej sekcji przedstawiono szczegóły certyfikat Azure AD używany do podpisania tokeny SAML, które zostały wydane do aplikacji zawsze użytkownik uwierzytelnia. Umożliwia właściwości bieżącego certyfikatu, aby sprawdzić, w tym daty wygaśnięcia.

Możliwość najazd certyfikat certyfikatów dodatkowe opcje będą obsługiwane w wersji preview kolejnych i zarządzać nimi. Uwaga pełnego zarządzania certyfikaty nadal mogą być wykonywane w [portal Azure klasyczny](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Konfiguracja aplikacji
Ostateczne sekcja zawiera dokumentacji i/lub formanty wymagane do skonfigurowania aplikacji do korzystania z usługi Azure Active Directory jako dostawcy tożsamości.

Menu wysuwanego **Konfigurowanie aplikacji** udostępnia nowe zwięzły, osadzone instrukcje dotyczące konfigurowania aplikacji. To jest inny nową funkcję unikatowe nowy portal Azure.

> [AZURE.NOTE] Pełny przykład osadzonego dokumentacji Zobacz aplikacja Salesforce.com. Dokumentacji dodatkowych aplikacji jest dodawana ciągłe podczas podglądu.

![Osadzone dokumentów][3]

##<a name="password-based-sign-on"></a>Znak na podstawie hasła
Jeśli obsługiwane w przypadku aplikacji, wybierając tryb logowania jednokrotnego oparte na hasłach a natychmiast pozycję **Zapisz** konfiguruje go w oparciu o hasła logowania jednokrotnego. Aby uzyskać więcej informacji na temat wdrażania opartego na hasło logowania jednokrotnego, zobacz [jak pojedynczy logowania jednokrotnego z pracą usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Znak na podstawie hasła][4]


##<a name="linked-sign-on"></a>Znak połączonych na
Jeśli obsługiwane w przypadku aplikacji, wybierając połączonych tryb logowania jednokrotnego pozwala na wprowadź adres URL, który ma Panel Azure AD dostępu lub usługi Office 365, aby przekierować do po kliknięciu tej aplikacji. Aby uzyskać więcej informacji o połączonych logowania jednokrotnego (wcześniej nazywanego "istniejące logowania jednokrotnego"), zobacz [jak pojedynczy logowania jednokrotnego z pracą usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Połączone logowania jednokrotnego][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
