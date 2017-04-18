<properties
    pageTitle="Azure AD Connect synchronizacją: jak zarządzać konta usługi Azure AD | Microsoft Azure"
    description="W tym temacie opisano sposoby przywracania konta usługi Azure AD."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, jak zresetować hasło do synchronizacji Azure AD Connect konta usługi łącznika"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect synchronizacją: jak zarządzać konta usługi Azure AD
Konto usługi używane przez łącznik Azure AD ma być bezpłatnej usługi. Jeśli chcesz zresetować swoje poświadczenia, w tym temacie jest dla Ciebie. Na przykład jeśli Administrator globalny ma omyłkowo Resetowanie hasła dla konta usługi przy użyciu programu PowerShell.

## <a name="reset-the-credentials"></a>Resetowanie poświadczeń
Jeśli konto usługi zdefiniowane łącznika Azure AD nie może nawiązać Azure AD ze względu na problemy z uwierzytelnianiem, można zresetować hasło.

1. Zaloguj się na serwerze narzędzie Azure AD Connect synchronizacji i zacznij programu PowerShell.
2. Uruchom `Add-ADSyncAADServiceAccount`.  
![Addadsyncaadserviceaccount polecenia cmdlet programu PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Podać poświadczenia administratora globalnego Azure AD.

To polecenie cmdlet pozwala zresetować hasło dla konta usługi i je zaktualizować, zarówno w Azure AD i aparat synchronizacji.

## <a name="known-issues-these-steps-can-solve"></a>Znane problemy dotyczące rozwiązać te kroki
W tej sekcji przedstawiono listę błędów zgłaszane przez klientów, które zostały poprawione poprzez poświadczenia na konto usługi Azure AD zresetowane.

-----------
Zdarzenia 6900  
Serwer napotkał nieoczekiwany błąd podczas przetwarzania powiadomienie o zmianie hasła:  
AADSTS70002: Błąd sprawdzania poprawności poświadczenia. AADSTS50054: Stare hasło jest używany do uwierzytelniania.

----------
Zdarzenia 659  
Błąd podczas pobierania konfiguracji synchronizacji zasad hasła. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Błąd sprawdzania poprawności poświadczenia. AADSTS50054: Stare hasło jest używany do uwierzytelniania.

## <a name="next-steps"></a>Następne kroki

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
