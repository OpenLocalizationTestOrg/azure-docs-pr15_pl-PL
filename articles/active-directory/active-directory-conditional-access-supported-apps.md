<properties
    pageTitle="Aplikacje, które używają zasadami dostępu warunkowego w usłudze Azure Active Directory | Microsoft Azure"
    description="Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza w określonych warunkach podczas uwierzytelniania użytkownika oraz umożliwiające dostęp do aplikacji."
    services="active-directory"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Aplikacje, które używają zasadami dostępu warunkowego w usłudze Active Directory platformy Azure

Reguły dostępu warunkowego są obsługiwane w usłudze Azure Active Directory (Azure AD)-połączony aplikacji, wstępnie zintegrowane federacyjnych oprogramowania jako aplikacji usług (władz akredytacji bezpieczeństwa), aplikacje, które używają hasło logowania jednokrotnego (SSO), linia firmie i aplikacje, które używają Azure AD serwer Proxy aplikacji. Szczegółowy wykaz aplikacji, dla których można użyć dostępu warunkowego dla [usługi włączone z dostępem warunkowym](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Dostępu warunkowego działa zarówno przenośnych i stacjonarnych aplikacje, które używają uwierzytelniania nowoczesny. W tym artykule omówiono firma Microsoft jak warunkowe działa dostępu w aplikacjach przenośnych i stacjonarnych.

Za pomocą Azure AD logowania stron w aplikacjach używających nowoczesny uwierzytelniania. Przy użyciu strony logowania użytkownik jest monitowany o uwierzytelnianie wieloskładnikowe. Komunikat jest wyświetlany, jeśli dany użytkownik ma dostęp jest zablokowane. Nowoczesna uwierzytelniania jest wymagane dla urządzenia do uwierzytelniania Azure AD, tak aby zasady opartych na urządzeniach dostępu warunkowego są przetwarzane.

Należy wiedzieć, które aplikacje można użyć reguły dostępu warunkowego i czynności, które należy wykonać, aby bezpiecznego punktów wejścia innych aplikacji.

## <a name="applications-that-use-modern-authentication"></a>Aplikacje, które używają uwierzytelniania nowoczesny

> [AZURE.NOTE] Jeśli masz zasady dostępu warunkowego w Azure AD, który ma odpowiednika w usłudze Office 365, skonfiguruj obie zasady dostępu warunkowego. Ma to zastosowanie, na przykład zasady dostępu warunkowego dla usługi Exchange Online lub usługi SharePoint Online.

Następujące aplikacje obsługują dostępu warunkowego dla Office 365 i inne aplikacje usługi Azure połączony AD:

| Usługa docelowa  | Platformy  | Aplikacji                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Usługa Office 365 Exchange Online | Windows 10|Aplikacji Poczta i kalendarz i osoby, program Outlook 2016, programu Outlook 2013 (z nowoczesny uwierzytelniania), programu Skype dla firm (z uwierzytelnianiem nowoczesny)|
|Usługa Office 365 Exchange Online| Windows 8.1, Windows 7 |Program Outlook 2016, program Outlook 2013 (z nowoczesny uwierzytelniania), program Skype dla firm (z uwierzytelnianie nowoczesny)|
|Usługa Office 365 Exchange Online|iOS i Android|  Program Outlook aplikacji dla urządzeń przenośnych|
|Usługa Office 365 Exchange Online|Mac OS X| Program Outlook 2016 dla uwierzytelnianie wieloskładnikowe i lokalizacji. Obsługa zasad opartych na urządzeniach planowana w przyszłości, w programie Skype dla wsparcia biznesowego planowana w przyszłości|
|Usługa Office 365 SharePoint Online|Windows 10| Aplikacje pakietu Office 2016, aplikacje uniwersalny pakietu Office, Office 2013 (z nowoczesny uwierzytelniania), usługi OneDrive dla aplikacji biznesowych (następnego klienta synchronizacji Generowanie lub NGSC) obsługuje planowana w przyszłości, obsługa grupami usługi Office planowana w przyszłości, obsługa aplikacji SharePoint planowana w przyszłości|
|Usługa Office 365 SharePoint Online|Windows 8.1, Windows 7|Aplikacje pakietu Office 2016, pakiet Office 2013 (z nowoczesny uwierzytelniania), aplikacji OneDrive dla firm (klienta synchronizacji programu Groove)|
|Usługa Office 365 SharePoint Online|iOS i Android|  Aplikacje pakietu Office mobile |
|Usługa Office 365 SharePoint Online|Mac OS X| Aplikacje pakietu Office 2016 uwierzytelnianie wieloskładnikowe i lokalizację. Obsługa zasad opartych na urządzeniach planowana w przyszłości|
|Yammer usługi Office 365|Windows 10, systemów iOS i Android | Aplikacja Yammer pakietu Office|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, systemów iOS i Android | Dynamics CRM aplikacji|
|Usługa PowerBI|Windows 10, Windows 8.1, Windows 7, systemów iOS i Android | Aplikacja PowerBI|
|Azure zdalnego aplikacji usługi|Windows 10, Windows 8.1, Windows 7, iOS, Android i systemu Mac OS X |Aplikacja Remote Azure|
|Wszystkie inne usługi aplikacji Moje aplikacje|IOS i android|Wszystkie inne usługi aplikacji Moje aplikacje |


## <a name="applications-that-do-not-use-modern-authentication"></a>Aplikacje, które nie korzysta z uwierzytelniania nowoczesny

Obecnie możesz zastosować inne metody blokować dostęp do aplikacji, które nie korzysta z uwierzytelniania nowoczesny. Reguły dostępu dla aplikacji, które nie korzystają z uwierzytelniania nowoczesny nie są wymuszane przez dostępu warunkowego. Jest to przede wszystkim uwagę dostępu za pomocą programu Exchange i programu SharePoint. Większość wcześniejszych wersji aplikacji za pomocą starsze protokoły kontroli dostępu.

### <a name="control-access-in-office-365-sharepoint-online"></a>Kontrola dostępu w usłudze Office 365 SharePoint Online
Możesz wyłączyć starsze protokoły dostępu programu SharePoint przy użyciu polecenia cmdlet Set-SPOTenant. Użyj tego polecenia cmdlet, aby uniemożliwić klienci pakietu Office, korzystające z wartością nowoczesny protokoły dostęp do zasobów usługi SharePoint Online.

**Polecenie przykładowe**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Kontrola dostępu w usłudze Office 365 Exchange Online

Exchange oferuje dwie główne kategorie protokołów. Przejrzyj poniższe opcje, a następnie wybierz zasady, które są odpowiednie dla Twojej organizacji.

-   **Program Exchange ActiveSync**. Domyślnie zasady dostępu warunkowego uwierzytelnianie wieloskładnikowe i lokalizację nie są wymuszane dla programu Exchange ActiveSync. Należy chronić dostęp do tych usług, konfigurując zasady programu Exchange ActiveSync bezpośrednio lub blokując programu Exchange ActiveSync za pomocą reguł Active Directory Federation Services (AD FS).
-   **Protokoły starsza wersja**. Można zablokować starsze protokoły z usług AD FS. Ten blokuje dostęp do starszych klientów pakietu Office, takich jak pakiet Office 2013 bez nowoczesnego włączone uwierzytelnianie i starszych wersjach pakietu Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Aby zablokować starsza wersja protokołu za pomocą usług AD FS

Za pomocą następujących reguł przykład w celu zablokowania starsza wersja protokołu dostępu na poziomie usług AD FS. Wybierz z dwóch typowych konfiguracji.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Opcja 1: Umożliwiają programu Exchange ActiveSync i Zezwalaj starszych aplikacji, ale tylko w intranecie

Ruch programu Exchange ActiveSync i przeglądarki i nowoczesnego uwierzytelniania ruchu, stosując następujące trzy reguły do usług AD FS Polegaj zaufania firmy platformy Microsoft Office 365 tożsamości, mają dostęp. Starsze aplikacje są blokowane z ekstranetu.

##### <a name="rule-1"></a>Reguła 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Reguła 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Reguły 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Opcja 2: Zezwalaj na programie Exchange ActiveSync i Zablokuj starsze aplikacje

Ruch programu Exchange ActiveSync i przeglądarki i nowoczesnego uwierzytelniania ruchu, stosując następujące trzy reguły do usług AD FS Polegaj zaufania firmy platformy Microsoft Office 365 tożsamości, mają dostęp. Starsze aplikacje są blokowane z dowolnej lokalizacji.

##### <a name="rule-1"></a>Reguła 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Reguła 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Reguły 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
