<properties
    pageTitle="Azure dostępu warunkowego dla aplikacji władz akredytacji bezpieczeństwa | Microsoft Azure"
    description="Dostępu warunkowego w Azure AD umożliwia konfigurowanie uwierzytelnianie wieloskładnikowe każdej aplikacji programu access reguł i możliwość blokowania dostępu dla użytkowników spoza sieci zaufanych. "
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
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Wprowadzenie do programu Azure Active Directory warunkowego dostępu

Azure Active Directory warunkowe dostępu dla aplikacji [władz akredytacji bezpieczeństwa](https://azure.microsoft.com/overview/what-is-saas/) i Azure AD łączyć aplikacje umożliwia konfigurowanie dostępu warunkowego na podstawie grupy, lokalizację i wrażliwości aplikacji. 

Warunkowe dostęp według charakter aplikacji możesz ustawić reguły dostępu uwierzytelnianie wieloskładnikowe (MFA) dla aplikacji. MFA każdej aplikacji pozwala blokować dostęp dla użytkowników, którzy nie są w zaufanej sieci. Dla wszystkich użytkowników, które są przypisane do aplikacji lub tylko dla użytkowników w określonych grup zabezpieczeń, można zastosować reguły MFA.  Użytkownicy mogą być wyłączone z wymagania MFA, jeśli uzyskują dostęp do aplikacji z adresu IP, który znajduje się w sieci organizacji.

Te funkcje będą dostępne dla klientów, którzy zakupili licencji usługi Azure Active Directory Premium.

## <a name="scenario-prerequisites"></a>Wymagania wstępne dotyczące scenariuszy
* Licencja usługi Azure Active Directory Premium

* Federacyjnych lub zarządzanych dzierżawy usługi Azure Active Directory

* Federacyjnych dzierżaw wymagać włączenia uwierzytelniania wieloskładnikowego.

## <a name="configure-per-application-access-rules"></a>Konfigurowanie reguł dla aplikacji programu access

W tej sekcji opisano, jak skonfigurować reguły dla aplikacji programu access.

1. Zaloguj się do portalu klasyczny Azure przy użyciu konta administratora globalnego dla Azure AD.
2. W okienku po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na karcie Katalog zaznacz katalogu.
4. Wybierz kartę **aplikacje** .
5. Wybierz aplikację, która zostanie ustawiony reguły.
6. Wybierz kartę **Konfiguruj** .
7. Przewiń w dół do sekcji reguły dostępu. Zaznacz regułę, żądany dostęp.
8. Określ użytkowników, których będzie dotyczyć reguła.
9. Włącz zasady, wybierając pozycję **włączone, aby się**.

##<a name="understanding-access-rules"></a>Opis reguł programu access

W tej sekcji przedstawiono szczegółowy opis reguły dostępu obsługiwane w programie Access Azure warunkowe aplikacji.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Określanie użytkowników reguły dostępu stosowanie do

Domyślne zasady zostaną zastosowane do wszystkich użytkowników, którzy mają dostęp do aplikacji. Można jednak również ograniczyć zasad do użytkowników należących do określonych grup zabezpieczeń. Przycisk **Dodaj grupę** jest używany do wybierz jedną lub więcej grup z okna dialogowego wyboru grupy, którego będzie dotyczyć reguła dostępu. To okno dialogowe można także usunąć zaznaczonej grupy. Po wybraniu opcji zasady mają zastosowanie do grup, reguły dostępu tylko będą wymuszane dla użytkowników, którzy należą do jednej z określonych grup zabezpieczeń.

Grupy zabezpieczeń mogą jawnie wykluczone z zasad, wybierając odpowiednią opcję **z wyjątkiem** a określanie jedną lub więcej grup. Nie będzie użytkowników, których jest członkiem grupy na liście **z wyjątkiem** pod warunkiem uwierzytelnianie wieloskładnikowe, nawet jeśli należą do grupy, której dotyczy reguła dostępu.
Reguła dostępu, jak pokazano poniżej wymaga wszystkich użytkowników w grupie Menedżerowie uwierzytelniania wieloskładnikowego podczas uzyskiwania dostępu do aplikacji.

![Ustawianie reguł dostępu warunkowego z MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Reguły dostępu warunkowego z MFA
Jeśli użytkownik został skonfigurowany za pomocą funkcji uwierzytelnianie wieloskładnikowe na użytkownika, to ustawienie na użytkownika połączy się z regułami uwierzytelnianie wieloskładnikowe aplikacji. Oznacza to, że użytkownik, który został skonfigurowany dla poszczególnych użytkowników uwierzytelnianie wieloskładnikowe będą musieli przeprowadzać uwierzytelnianie wieloskładnikowe nawet wtedy, gdy masz zostały wyłączone z reguł uwierzytelnianie wieloskładnikowe aplikacji. Dowiedz się więcej na temat wieloskładnikowe ustawień uwierzytelniania i poszczególnych użytkowników.

### <a name="access-rule-options"></a>Opcje reguły programu Access
Obsługiwane są następujące opcje:

* **Wymaganie uwierzytelnianie wieloskładnikowe**: użytkowników, do których stosowane reguły dostępu, będą wymagane do wykonania uwierzytelnianie wieloskładnikowe przed uzyskaniem dostępu do aplikacji, której dotyczy zasady.

* **Wymaganie uwierzytelnianie wieloskładnikowe, gdy nie pracujący**: użytkownika, które pochodzą od zaufanych adresu IP nie będą musieli wykonać uwierzytelnianie wieloskładnikowe. Na stronie Ustawienia uwierzytelnianie wieloskładnikowe, można skonfigurować zaufanych zakresy adresów IP.

* **Zablokuj dostęp, gdy nie w pracy**: użytkownik, który nie pochodzi z zaufanego adresu IP zostaną zablokowane. Na stronie Ustawienia uwierzytelnianie wieloskładnikowe, można skonfigurować zaufanych zakresy adresów IP.

### <a name="setting-rule-status"></a>Ustawianie stanu reguły
Stan reguły programu Access umożliwia włączanie reguł lub wyłączanie. Gdy reguły dostępu są wyłączone, wymaganie uwierzytelnianie wieloskładnikowe nie są wymuszane.

### <a name="access-rule-evaluation"></a>Oceny reguły dostępu

Reguły dostępu są sprawdzane, gdy użytkownik uzyskuje dostęp do aplikacji federacyjnych, korzystającego z OAuth 2.0, łączenie OpenID, SAML lub federacyjnych. Ponadto reguły dostępu są wyznaczane OAuth 2.0 i łączenie OpenID umożliwia uzyskiwanie token dostępu tokenu odświeżania. Jeśli zasady oceny kończy się niepowodzeniem, gdy token odświeżania jest używany, zostanie zwrócony błąd **invalid_grant** , oznacza to, że użytkownik chce uwierzytelnienie do klienta.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Konfigurowanie usług federacyjnych, aby zapewnić uwierzytelnianie wieloskładnikowe

Federacyjnych dzierżaw MFA mogą być wykonywane przez usługi Azure Active Directory lub lokalnego serwera usług AD FS.

Domyślnie MFA wystąpi na stronie obsługiwanych przez usługi Azure Active Directory. Aby skonfigurować MFA lokalnego, właściwość **— SupportsMFA** musi wynosić **PRAWDA** w usługi Azure Active Directory przy użyciu modułu Azure AD dla programu Windows PowerShell.

W poniższym przykładzie pokazano, jak włączyć MFA lokalnego przy użyciu polecenia [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) w dzierżawie contoso.com:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Oprócz ustawienie tej flagi, należy skonfigurować wystąpienie federacyjnych dzierżawy AD FS do uwierzytelniania wieloskładnikowego. Postępuj zgodnie z instrukcjami do [wdrażania uwierzytelnianie wieloskładnikowe Azure lokalnego](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Artykuły pokrewne

- [Zabezpieczanie dostępu do usługi Office 365 i inne aplikacje połączony z usługi Azure Active Directory](active-directory-conditional-access.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
