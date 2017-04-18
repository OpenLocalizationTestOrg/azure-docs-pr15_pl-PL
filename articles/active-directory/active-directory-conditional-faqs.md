<properties
    pageTitle="Azure Active Directory dostępu warunkowego: często zadawane pytania | Microsoft Azure"
    description="Często zadawane pytania dotyczące dostępu warunkowego "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory dostępu warunkowego — często zadawane pytania

## <a name="which-applications-work-with-conditional-access-policies"></a>Aplikacje, które współpracują z zasady dostępu warunkowego?

**A:** Zobacz temat [warunkowe aplikacje programu access, co są obsługiwane](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Zasady dostępu warunkowego są wymuszane dla gości i współpracą B2B?

**A:** Zasady są wymuszane B2B współpracy użytkowników. Jednak w niektórych przypadkach użytkownik może okazać się niemożliwe spełnia wymagania zasad, jeśli na przykład organizacja nie obsługuje uwierzytelnianie wieloskładnikowe. 

Zasady nie są obecnie wymuszane dla gości programu SharePoint. Relacja gościa jest obsługiwana w programie SharePoint. Zasady użytkowników gości, których konta nie są objęte programu access na serwerze uwierzytelniania. Dostęp gościa można zarządzać w programie SharePoint.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Czy usługi SharePoint Online zasad dotyczy również do usługi OneDrive dla firm?

**A:** Wartość Tak.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Dlaczego nie mogę ustawić zasady na aplikacje klienta, takie jak Word lub Outlook?

**A:** Zasady dostępu warunkowego Ustawia wymagania dotyczące uzyskiwania dostępu do usługi i są wymuszane, gdy uwierzytelnianie odbywa się w tej usłudze. Zasady nie ustawiono bezpośrednio w aplikacji klienckiej; Zamiast tego jest stosowana, gdy go do użytku. Na przykład zasady ustawiony w programie SharePoint dotyczy klientów wywołujących programu SharePoint i zestaw na serwerze Exchange zasad odnosi się do programu Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Zasady dostępu warunkowego dotyczy konta usługi?

**A:** Zasady dostępu warunkowego zastosowane do wszystkich kont użytkowników. Dotyczy to również używane jako konta usługi kont użytkowników. W większości przypadków uruchamianej nienadzorowanego konta usługi nie będzie mógł spełniać zasady. Jest to, na przykład tak, jeśli MFA jest wymagane. W tych przypadkach konta usług mogą być wyłączone z zasady, przy użyciu ustawień zarządzania zasadami dostępu warunkowego. Dowiedz się więcej na temat stosowania zasad do użytkowników w tym miejscu.
