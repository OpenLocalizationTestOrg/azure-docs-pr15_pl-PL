<properties
    pageTitle="Dostępu warunkowego w przypadku aplikacji publikowanych z serwera Proxy aplikacji Azure AD"
    description="Opisano, jak skonfigurować dostępu warunkowego aplikacje, które możesz opublikować można uzyskać dostęp przy użyciu serwera Proxy aplikacji Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Praca z dostępu warunkowego

Możesz skonfigurować reguły dostępu do udzielenia warunkowego dostępu do aplikacji publikowanych przy użyciu serwera Proxy aplikacji. Pozwala na:

- Wymaganie uwierzytelnianie wieloskładnikowe każdej aplikacji
- Wymaganie uwierzytelnianie wieloskładnikowe tylko wtedy, gdy użytkownicy nie są w miejscu pracy
- Uniemożliwić użytkownikom uzyskiwanie dostępu do aplikacji, gdy znajdują się one w miejscu pracy

Mogą być stosowane te reguły, aby wszyscy użytkownicy i grupy, lub tylko określonym użytkownikom i grupom. Domyślnie reguły zostaną zastosowane do wszystkich użytkowników, którzy mają dostęp do aplikacji. Jednak reguły może być ograniczone do użytkowników należących do określonych grup zabezpieczeń.  

Reguły dostępu są sprawdzane, gdy użytkownik uzyskuje dostęp do aplikacji federacyjnych, korzystającego z OAuth 2.0, łączenie OpenID, SAML lub federacyjnych. Ponadto reguły dostępu są wyznaczane z OAuth 2.0 i łączenie OpenID token odświeżania jest używany do uzyskania token dostępu.

## <a name="conditional-access-prerequisites"></a>Wymagania wstępne dotyczące dostępu warunkowego

- Subskrypcja usługi Azure Active Directory Premium
- Federacyjnych lub zarządzanych dzierżawcy usługi Azure Active Directory
- Federacyjnych dzierżaw wymagać włączenia uwierzytelniania wieloskładnikowe (MFA)  
    ![Konfigurowanie reguły dostępu — wymaga uwierzytelniania wieloskładnikowego](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfigurowanie uwierzytelniania wieloskładnikowego każdej aplikacji
1. Zaloguj się jako administrator w portalu klasyczny Azure.
2. Przejdź do usługi Active Directory i wybierz katalog, w którym chcesz włączyć serwer Proxy aplikacji.
3. Kliknij pozycję **Programy** , a następnie przewiń w dół do sekcji **Reguły dostępu** . W sekcji zasady dostępu jest wyświetlana tylko dla aplikacje publikowana przy użyciu serwera Proxy aplikacji, które używają uwierzytelniania federacyjnych.
4. Włącz regułę, wybierając pozycję **Włącz reguły dostępu** **na**.
5. Określanie użytkowników i grup, do których będą stosowane reguły. Wybierz jedną lub więcej grup, do których będzie stosowana reguła dostępu za pomocą przycisku **Dodaj grupę** . To okno dialogowe można także usunąć zaznaczonej grupy.  Po wybraniu opcji zasady mają zastosowanie do grup, reguły dostępu tylko będą wymuszane dla użytkowników, którzy należą do jednej z określonych grup zabezpieczeń.  

  - Aby jawnie wykluczyć grup zabezpieczeń z reguły, zaznacz **z wyjątkiem** i określ jedną lub więcej grup. Użytkownicy, którzy są członkami grupy na liście oprócz nie będą musieli przeprowadzać uwierzytelnianie wieloskładnikowe.  

  - Jeśli użytkownik został skonfigurowany za pomocą funkcji uwierzytelnianie wieloskładnikowe na użytkownika, to ustawienie ma pierwszeństwo przed reguły uwierzytelnianie wieloskładnikowe aplikacji. Oznacza to, że użytkownik, który został skonfigurowany dla uwierzytelnianie wieloskładnikowe użytkownika będzie wymagane przeprowadzać uwierzytelnianie wieloskładnikowe nawet wtedy, gdy masz zostały wyłączone z reguł uwierzytelnianie wieloskładnikowe aplikacji. Dowiedz się więcej na temat [uwierzytelnianie wieloskładnikowe i ustawienia użytkownika](../multi-factor-authentication/multi-factor-authentication.md).

6. Zaznacz regułę programu access, którą chcesz skonfigurować:
    - **Uwierzytelnianie wieloskładnikowe wymagają**: użytkowników, do których stosowane reguły dostępu będzie wymagane do wykonania uwierzytelnianie wieloskładnikowe przed uzyskaniem dostępu do aplikacji, której dotyczy ta reguła.
    - **Uwierzytelnianie wieloskładnikowe wymagają po nie w pracy**: użytkownicy próby uzyskania dostępu do aplikacji z zaufanych adresu IP nie będzie musiał przeprowadzić uwierzytelnianie wieloskładnikowe. Na stronie Ustawienia uwierzytelnianie wieloskładnikowe, można skonfigurować zaufanych zakresy adresów IP.
    - **Zablokuj dostęp, gdy nie w pracy**: próby uzyskania dostępu do aplikacji z poza siecią firmową użytkownicy nie będą mogli uzyskiwać dostęp do aplikacji.


## <a name="configuring-mfa-for-federation-services"></a>Konfigurowanie MFA dla usług federacyjnych
Federacyjnych dzierżaw uwierzytelnianie wieloskładnikowe (MFA) mogą być wykonywane przez usługi Azure Active Directory lub lokalnego serwera usług AD FS. Domyślnie MFA wystąpi na dowolnej stronie obsługiwanych przez usługi Azure Active Directory. Aby skonfigurować MFA lokalnego, uruchom programu Windows PowerShell i właściwość SupportsMFA — umożliwia ustawienie moduł usługi Azure AD.

W poniższym przykładzie pokazano, jak włączyć MFA lokalnego przy użyciu polecenia [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) w dzierżawie contoso.com:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Oprócz ustawienie tej flagi, należy skonfigurować wystąpienie federacyjnych dzierżawy AD FS do uwierzytelniania wieloskładnikowego. Postępuj zgodnie z instrukcjami do [wdrażania platformy Microsoft Azure uwierzytelnianie wieloskładnikowe lokalnego](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Zobacz też

- [Praca z aplikacjami pamiętać o oświadczeniach](active-directory-application-proxy-claims-aware-apps.md)
- [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
