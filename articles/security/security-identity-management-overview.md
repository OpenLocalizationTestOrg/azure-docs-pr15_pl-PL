<properties
   pageTitle="Omówienie zabezpieczeń zarządzania Azure tożsamości | Microsoft Azure"
   description=" Microsoft tożsamości i access rozwiązań Pomoc dotycząca zarządzania IT ochrony dostęp do aplikacji i zasobów w firmowej centrum danych i w chmurze, włączanie poziomy sprawdzania poprawności, takich jak uwierzytelnianie wieloskładnikowe i zasady dostępu warunkowego. W tym artykule omówiono podstawowe funkcje zabezpieczeń Azure, ułatwiające zarządzanie tożsamościami. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Omówienie zabezpieczeń zarządzania Azure tożsamości

Microsoft tożsamości i access rozwiązań Pomoc dotycząca zarządzania IT ochrony dostęp do aplikacji i zasobów w firmowej centrum danych i w chmurze, włączanie poziomy sprawdzania poprawności, takich jak uwierzytelnianie wieloskładnikowe i zasady dostępu warunkowego. Monitorowanie aktywności podejrzanych za pośrednictwem zabezpieczeniami zaawansowanymi raportowania, inspekcji i alertu, że pomaga ograniczyć potencjalne problemy. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) zawiera rejestracji jednokrotnej do tysięcy chmury aplikacji (władz akredytacji bezpieczeństwa) i uzyskać dostęp do aplikacji sieci web uruchomieniem lokalnego.

Korzyści w zakresie zabezpieczeń z Azure Active Directory (AD) mogą być następujące:

- Tworzenie i zarządzanie nimi pojedynczy tożsamości dla każdego użytkownika w przedsiębiorstwie hybrydowych synchronizacja użytkownikami, grupami i urządzeń
- Dostęp pojedynczego logowania w aplikacji, w tym tysiące wstępnie zintegrowane aplikacje władz akredytacji bezpieczeństwa
- Włączanie aplikacji zabezpieczeń programu access przez wymuszanie reguły oparte na uwierzytelnianie wieloskładnikowe dla obu lokalnego i aplikacje w chmurze
- Inicjowanie obsługi bezpieczny dostęp zdalny do lokalnej aplikacji sieci web za pośrednictwem serwera Proxy aplikacji Azure AD

Celem tego artykułu jest zawierają omówienie podstawowych funkcji Azure zabezpieczeń, które pomagają w sposób zarządzania tożsamością. Firma Microsoft udostępnia również łącza do artykułów zawierających szczegółowe informacje o każdej funkcji, aby dowiedzieć się więcej.  

Artykuł poświęcony następujące funkcje zarządzania tożsamości Azure podstawowe:

- Rejestracji jednokrotnej
- Odwracanie serwera proxy
- Uwierzytelnianie wieloskładnikowe
- Monitorowanie zabezpieczeń, alerty i raportów opartych na nauki komputera
- Zarządzanie tożsamości i dostępu dla klientów indywidualnych
- Rejestracja urządzenia
- Zarządzanie tożsamościami uprzywilejowanych
- Ochrona tożsamości
- Zarządzanie tożsamościami hybrydowego

## <a name="single-sign-on"></a>Rejestracji jednokrotnej

Pojedynczy logowania jednokrotnego (SSO) oznacza mogą uzyskiwać dostęp do wszystkich aplikacji i zasobów, które należy wykonać business, logując się tylko raz przy użyciu jednego konta użytkownika. Po zalogowaniu się możesz uzyskać dostęp do wszystkich aplikacji należy bez konieczności uwierzytelniania (na przykład wpisz hasło) po raz drugi.

Wiele organizacji opierają się na oprogramowanie jako aplikacji usług (SaaS) na przykład Office 365, pole i usług Salesforce biurowego użytkownika końcowego. Ze względów historycznych personelem INFORMATYCZNYM, aby pojedynczo tworzenie i aktualizowanie kont użytkowników w każdej aplikacji władz akredytacji bezpieczeństwa, a użytkownicy musiał Zapamiętaj hasło dla każdej aplikacji władz akredytacji bezpieczeństwa.

Azure AD jest rozciągany usługi Active Directory w lokalnej chmurki, umożliwiając użytkownikom za pomocą ich podstawowego konta organizacji nie tylko zalogować się do jego urządzeń domeny i zasobów firmy, ale również wszystkie sieci web i aplikacjach władz akredytacji bezpieczeństwa wymagany dla swojej pracy.

Nie tylko użytkownicy nie trzeba zarządzać wiele zestawów nazwy użytkowników i hasła, dostęp do aplikacji może być automatycznie ustanawianie lub Anuluj ustanawianie na podstawie grup organizacji i ich statusu pracownika. Azure AD wprowadza zabezpieczeń i uzyskiwanie dostępu do formantów zarządzania, które umożliwiają centralne zarządzanie dostępem użytkowników w aplikacjach władz akredytacji bezpieczeństwa.

Dowiedz się więcej:

- [Omówienie logowania jednokrotnego](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integracja usługi Azure Active Directory rejestracji jednokrotnej z aplikacjami władz akredytacji bezpieczeństwa](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Odwracanie serwera proxy

Serwer Proxy aplikacji Azure AD umożliwia publikowanie aplikacji lokalnej, takich jak witryny [programu SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) , [Aplikacji Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)i [usług IIS](http://www.iis.net/)-podstawie aplikacje wewnątrz sieci prywatnej i zapewnia bezpieczny dostęp do użytkowników spoza sieci. Serwer Proxy aplikacji zapewnia dostęp zdalny do logowania jednokrotnego (SSO) dla wielu typów aplikacji sieci web w lokalnej z tysiące aplikacji władz akredytacji bezpieczeństwa, które obsługują Azure AD. Pracowników można zalogować się do aplikacji z strona główna na własne urządzenia i służą do uwierzytelniania za pośrednictwem serwera proxy ten opartej na chmurze.

Dowiedz się więcej:

- [Włączanie serwera Proxy aplikacji Azure AD](../active-directory/active-directory-application-proxy-enable.md)
- [Publikowanie aplikacji przy użyciu serwera Proxy aplikacji Azure AD](../active-directory/active-directory-application-proxy-publish.md)
- [Jednokrotnej z serwera Proxy aplikacji](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Praca z dostępu warunkowego](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe
Uwierzytelnianie wieloskładnikowe Azure (MFA) jest to metoda uwierzytelniania, który wymaga korzystania z więcej niż jedną metodę weryfikacji i dodanie krytyczne drugą warstwę zabezpieczeń dodatki logowania użytkownika i transakcje. MFA ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia silne uwierzytelnianie za pomocą różnych opcji Weryfikacja — rozmowy telefonicznej, SMS lub aplikacji dla urządzeń przenośnych powiadomienia lub weryfikacji kod i 3rd firmy OAuth tokeny.

Dowiedz się więcej:

- [Uwierzytelnianie wieloskładnikowe](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Co to jest Azure uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)
- [Jak działa uwierzytelnianie wieloskładnikowe Azure](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Monitorowanie zabezpieczeń, alerty i raportów opartych na nauki komputera

Monitorowanie zabezpieczeń i alertów i oparte na nauki raportów komputera, które identyfikować wzorce niespójne access może ułatwić ochronę firmy. Dostęp do usługi Azure Active Directory i raporty użycia umożliwia uzyskanie wgląd integralności i bezpieczeństwa katalogu organizacji. Przy użyciu tych informacji administratora usługi katalogowej lepiej określić miejsce, w którym może znajdować się możliwe ryzyko związane z zabezpieczeniami, tak aby były odpowiednio można zaplanować ich eliminowania.

W portalu klasyczny Azure raporty są podzielone na następujące sposoby:

- Raporty anomalii — zawierać znak wydarzeń znajdujące się anomalous. Celem jest sprawić, że takie aktywność i umożliwiają mieć możliwość określenia, czy zdarzenie jest podejrzany.
- Zintegrowane raporty aplikacji — zapewniają spostrzeżeń jak aplikacje w chmurze są używane w Twojej organizacji. Azure Active Directory oferuje integrację z tysiące aplikacje w chmurze.
- Raporty o błędach — wskazują błędy, które mogą wystąpić podczas inicjowania obsługi administracyjnej konta do aplikacji zewnętrznych.
- Raporty specyficzne dla użytkownika — wyświetlanie urządzenia i logowania aktywności dane dla określonego użytkownika.
- Dzienniki aktywności — zawierają wszystkie inspekcji zdarzeń w ostatnich 24 godzin, ostatnich 7 dni lub ostatnich 30 dni, a także zmian aktywności grup i resetowanie i rejestracji aktywności hasło.

Dowiedz się więcej:

- [Wyświetlanie raportów dostępu i użytkowania](../active-directory/active-directory-view-access-usage-reports.md)
- [Wprowadzenie do programu Azure Active Directory raportowania](../active-directory/active-directory-reporting-getting-started.md)
- [Przewodnik raportowania Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Zarządzanie tożsamości i dostępu dla klientów indywidualnych

Azure Active Directory B2C jest wysoce dostępne tożsamość globalnego, zarządzania usługą dla aplikacji dla klientów indywidualnych skierowaną która może setki milionów tożsamości. Może zostać zintegrowany przez telefon komórkowy oraz platformy w sieci web. Osób korzystających ze logować się do wszystkich aplikacji za pomocą dostosowania środowiska za pomocą ich istniejącego konta społecznościowych lub tworząc nowe poświadczenia.

W przeszłości deweloperów, którzy przetransponowane mogą rejestrować się i zalogować się konsumentów w aplikacjach czy zostały zapisane własne kodu. I czy jest używane w lokalnej bazy danych lub systemy do przechowywania nazwy użytkowników i hasła. Azure Active Directory B2C oferuje organizacji lepszym sposobem integracji Zarządzanie tożsamościami dla klientów indywidualnych w aplikacjach za pomocą platformy bezpieczne, zgodne ze standardami i dużych zestaw zasad extensible.

Gdy używasz Azure Active Directory B2C osób korzystających ze mogą tworzyć konta dla aplikacji przy użyciu ich istniejącego konta społecznościowych (Facebook, Google, Amazon, LinkedIn) lub tworząc nowe poświadczenia (adres e-mail i hasło, lub nazwy użytkownika i hasła).

Dowiedz się więcej:

- [Co to jest Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Podgląd Azure Active Directory B2C: Zaloguj się w górę i zaloguj się konsumentów w aplikacjach](../active-directory-b2c/active-directory-b2c-overview.md)
- [Podgląd B2C Azure Active Directory: Typów aplikacji](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Rejestracja urządzenia

Rejestracja urządzenia w usłudze Azure AD jest podstawą scenariusze oparte na urządzeniu [dostępie warunkowym](../active-directory/active-directory-conditional-access-on-premises-setup.md) . Po zarejestrowaniu urządzenia Azure Active Directory Rejestracja urządzenia zawiera urządzenia z tożsamości, który jest używany do uwierzytelniania na urządzeniu, gdy użytkownik znaki w. Następnie można uwierzytelnionych urządzenie i atrybuty urządzenia, wymuszanie stosowania zasad dostępu warunkowego dla aplikacji, które są przechowywane w chmurze i lokalnego.

W połączeniu z rozwiązanie do zarządzania (MDM) urządzeń przenośnych, takich jak Intune, atrybuty urządzeń w usłudze Azure Active Directory są aktualizowane dodatkowe informacje o urządzeniu. Umożliwia tworzenie reguł dostępu warunkowego, wymuszające dostęp za pomocą urządzeń standardów dla zabezpieczenia i zgodność.

Dowiedz się więcej:

- [Rozpoczynanie pracy z Azure Active Directory Rejestracja urządzenia](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Aby skonfigurować warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Rejestracja urządzenia automatyczne z urządzeniami domeny Azure Active Directory dla systemu Windows](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Zarządzanie tożsamościami uprzywilejowanych
Azure Active Directory (AD) uprzywilejowanych Zarządzanie tożsamościami pozwala zarządzać, kontrolować i monitorować uprzywilejowanych tożsamości i uzyskać dostęp do zasobów w Azure AD, a także inne usługi online firmy Microsoft, takich jak usługi Office 365 lub Intune firmy Microsoft.

Czasem konieczne przeprowadzania uprzywilejowanych operacji w zasobów Azure lub usługi Office 365 lub inne aplikacje władz akredytacji bezpieczeństwa użytkowników. Często oznacza to, że trzeba nadać im stały dostęp uprzywilejowanych Azure AD organizacji. Jest to zagrożenie bezpieczeństwa rosnących hostowana w chmurze zasobów, ponieważ organizacje wystarczająco nie można monitorować, co robią tych użytkowników z ich uprawnieniami administratora. Ponadto jeśli konto użytkownika z dostępem uprzywilejowanych jest bezpieczne, tego jednego naruszenia mogą mieć wpływ ich ogólne bezpieczeństwo chmury. Zarządzanie tożsamościami w usłudze Azure AD uprawnieniach pozwala rozwiązać ten czynnik ryzyka.

Zarządzanie tożsamościami uprawnieniach AD Azure oferuje następujące możliwości:

- Zobacz użytkowników, którzy są Administratorzy Azure AD
- Włączenie na żądanie "po prostu time" dostęp administracyjny do usługi Online firmy Microsoft, takich jak usługi Office 365 i usługi
- Pobieranie raportów dotyczących historię dostęp administratora i zmiany w przypisaniach administratora
- Otrzymywać alerty o dostęp do roli uprzywilejowanych

Dowiedz się więcej:

- [Zarządzanie tożsamościami uprawnieniach Azure AD](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Role w uprawnieniach Azure AD Zarządzanie tożsamościami](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD uprzywilejowanych Zarządzanie tożsamościami: Jak dodawanie lub usuwanie roli użytkownika](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Ochrona tożsamości
Azure ochronę tożsamości AD to usługa zabezpieczeń, która zapewnia skonsolidowany widok do zdarzenia ryzyka i słabych wpływu tożsamości Twojej organizacji. Ochrona tożsamości wykorzystuje możliwości wykrywania anomalii istniejących Azure usługi Active Directory (dostępne za pośrednictwem Azure AD Anomalous raporty aktywności) i wprowadza nowe typy zdarzeń ryzyka, które może wykryć różnic w odniesieniu w czasie rzeczywistym.

Dowiedz się więcej:

- [Ochrona tożsamości Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanał 9: Azure AD i Pokaż tożsamości: Podgląd ochrony tożsamości](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Zarządzanie tożsamościami hybrydowego

Podejście firmy Microsoft do tożsamości obejmuje lokalnym a chmurą, tworzenie tożsamości jednego użytkownika, dla uwierzytelniania i autoryzacji wszystkie zasoby, niezależnie od lokalizacji.

Dowiedz się więcej:

- [Hybrydowe tożsamości oficjalny dokument](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Blog zespołu usługi Active Directory](https://blogs.technet.microsoft.com/ad/)
