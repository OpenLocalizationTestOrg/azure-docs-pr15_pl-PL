<properties
   pageTitle="Jaki sposób wymusić uwierzytelnianie wieloskładnikowe | Microsoft Azure"
   description="Dowiedz się, jak wymagają uwierzytelnianie wieloskładnikowe (MFA) dla tożsamości uprzywilejowanych z rozszerzeniem Azure Active Directory uprzywilejowanych Zarządzanie tożsamościami."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Jak wymagają MFA w programie Azure AD uprzywilejowanych Zarządzanie tożsamościami

Zalecane jest wymagane uwierzytelnianie wieloskładnikowe (MFA) dla wszystkich administratorów usługi. Zmniejsza zagrożenie atakiem z powodu złamany hasła.

Mogą wymagać, aby wykonać wezwanie MFA się zalogować. Wpis w blogu [MFA dla Office 365 i MFA dla Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) Porównuje zawartość subskrypcji pakietu Office i Azure, przy użyciu funkcji zawarte w oferująca uwierzytelnianie wieloskładnikowe usługi Microsoft Azure.

Możesz również wymagać użytkownik wykonać wezwanie MFA, po ich aktywacji rola w Azure AD PIM. Dzięki temu, jeśli użytkownik nie ukończyć wezwanie MFA po ich zalogowano, zostanie wyświetlony monit to zrobić przez PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Wymaganie MFA w uprawnieniach Azure AD Zarządzanie tożsamościami

Podczas zarządzania tożsamościami w PIM jako administrator uprzywilejowanych roli może zostać wyświetlony alertów, które jest zalecane MFA uprzywilejowanych kont. Kliknij alert zabezpieczeń na pulpicie nawigacyjnym PIM i nowe karta zostanie otwarty z listą konta administratora, które należy wymagają MFA.  Jest wymagane MFA, zaznaczając wiele ról, a następnie kliknij przycisk **Napraw** , lub możesz kliknij wielokropek obok wybranych ról, a następnie kliknij przycisk **Napraw** .

> [AZURE.IMPORTANT] Prawym przyciskiem myszy teraz, Azure MFA działa tylko z pracą lub konta służbowego, nie konta Microsoft (zazwyczaj osobistego konta używanego do logowania się do usług firmy Microsoft, takich jak Skype, Xbox, Outlook.com itp.). Z tego powodu każdy za pomocą konta Microsoft nie można administratorem kwalifikuje się, ponieważ nie mogą używać MFA aktywować ich ról. Jeśli te użytkownicy muszą dalej zarządzać obciążenia za pomocą konta Microsoft, podniesienia ich administratorom trwały teraz.

Ponadto można zmienić wymagania MFA konkretnej roli klikając go w sekcji role PIM pulpitu nawigacyjnego. Następnie kliknij przycisk **Ustawienia** w karta roli, a następnie wybierając **włączyć** w obszarze uwierzytelnianie wieloskładnikowe.

## <a name="how-azure-ad-pim-validates-mfa"></a>Jak Azure AD PIM sprawdza MFA

Dostępne są dwie opcje sprawdzania poprawności MFA, gdy użytkownik aktywuje roli.

Najprostszym rozwiązaniem jest zależne Azure MFA dla użytkowników, którzy aktywowany uprzywilejowanych roli. Aby to zrobić, najpierw sprawdź, czy tych użytkowników jest licencjonowany, jeśli to konieczne i dla Azure MFA zarejestrowano. Więcej informacji na temat wykonywania tego zadania jest [wprowadzenie uwierzytelnianie wieloskładnikowe Azure w chmurze](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Jest to zalecane, ale nie są wymagane, aby skonfigurować Azure AD wymuszenie MFA dla tych użytkowników się zalogować. Jest to spowodowane testy MFA zostaną przez Azure AD PIM, się.

Możesz też uwierzytelnianie użytkowników lokalnych możesz mieć dostawcy tożsamości odpowiedzialne za MFA. Na przykład jeśli skonfigurowano AD Federation Services do wymagania uwierzytelniania opartego na karty inteligentnej przed uzyskaniem dostępu do Azure AD [Zabezpieczanie chmurze zasobów z uwierzytelnianie wieloskładnikowe Azure i usług AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) zawiera instrukcje dotyczące konfigurowania usług AD FS, aby wysłać wniosków Azure AD. Podczas próby aktywowania roli, Azure AD PIM akceptuje, że MFA została już sprawdzona dla użytkownika po odebraniu odpowiednie roszczeń.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
