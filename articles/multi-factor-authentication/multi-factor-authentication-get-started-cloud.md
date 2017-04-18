<properties
    pageTitle="Uzyskiwanie pracę Azure MFA w chmurze | Microsoft Azure"
    description="Jest to strona uwierzytelniania wieloskładnikowego Microsoft Azure, który opisano, jak rozpocząć pracę z MFA Azure w chmurze."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Wprowadzenie do programu uwierzytelnianie wieloskładnikowe Azure w chmurze
W tym artykule opisano jak rozpocząć pracę przy użyciu funkcji uwierzytelniania wieloskładnikowego Azure w chmurze.

> [AZURE.NOTE]  Poniższą dokumentację zawiera informacje na temat włączania użytkowników za pomocą **Klasycznej Portal Azure**. Jeśli szukasz informacji na temat sposobu konfigurowania uwierzytelnianie wieloskładnikowe Azure dla użytkowników usługi Office 365, zobacz [Konfigurowanie uwierzytelniania wieloskładnikowego dla usługi Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA w chmurze](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Wymagania wstępne
Wymagane są następujące wymagania wstępne, zanim będzie można włączyć uwierzytelnianie wieloskładnikowe Azure dla użytkowników.


1. [Załóż subskrypcji usługi Azure](https://azure.microsoft.com/pricing/free-trial/) — Jeśli nie masz już subskrypcji usługi Azure, musisz zapisów dla jednej. Jeśli po prostu uruchamiania i przy użyciu Azure MFA możesz użyć subskrypcji wersji próbnej
2. [Tworzenie dostawcy uwierzytelnianie wieloskładnikowe](multi-factor-authentication-get-started-auth-provider.md) i przypisać ją do katalogu lub [przypisywać licencji do użytkowników](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licencje są dostępne dla użytkowników, którzy mają Azure MFA, Azure AD Premium lub pakietu mobilności przedsiębiorstwa (EMS).  MFA będzie obejmować Azure AD Premium i EMS. Jeśli masz wystarczającej liczby licencji, nie musisz utworzyć dostawcę uwierzytelniania.


## <a name="turn-on-two-step-verification-for-users"></a>Włączanie Weryfikacja dwuetapowa dla użytkowników
Aby rozpocząć, wymaganie weryfikacji dwóch start na użytkownika, zmienianie stanu użytkownika wyłączonego na włączony.  Uzyskać więcej informacji o stanach użytkownika zobacz [Państw użytkownika w uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-get-started-user-states.md)

Poniższa procedura umożliwia włączenie MFA dla użytkowników.

### <a name="to-turn-on-multi-factor-authentication"></a>Aby włączyć uwierzytelnianie wieloskładnikowe

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com) jako administrator.
2.  Po lewej stronie kliknij pozycję **Usługi Active Directory**.
3.  W obszarze Katalog wybierz katalog dla użytkownika, które chcesz włączyć.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  U góry kliknij pozycję **Użytkownicy**.
5.  U dołu strony kliknij pozycję **Zarządzaj uwierzytelnianie wieloskładnikowe**. Zostanie otwarta na nowej karcie przeglądarki.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Znajdź użytkownika, którego chcesz włączyć na potrzeby weryfikacji dwuetapowej. Może być konieczne zmienianie widoku u góry. Upewnij się, że jest w stanie **wyłączone.** 
 ![Włącz użytkowników](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Umieść **Sprawdź** w polu obok jego nazwy.
7.  Po prawej stronie kliknij polecenie **Włącz**.
![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kliknij opcję **Włącz uwierzytelnianie wieloskładnikowe**.
![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Zwróć uwagę, że stan użytkownika zmieniono z **wyłączone** na **włączone**.
![Umożliwianie użytkownikom](./media/multi-factor-authentication-get-started-cloud/user.png)

Po włączeniu użytkowników, należy je powiadomić pocztą e-mail. Przy następnym próbują się zalogować, ich pojawi się pytanie o zarejestrowanie ich konta na potrzeby weryfikacji dwuetapowej. Po rozpoczęciu przy użyciu Weryfikacja dwuetapowa, również będą musieli się skonfigurować hasła aplikacji, aby uniknąć zablokowane wylogowywanie się z aplikacji innych niż przeglądarki.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Użyj programu PowerShell, aby zautomatyzować zmianę na potrzeby weryfikacji dwuetapowej

Aby zmienić [Stan](multi-factor-authentication-whats-next.md) przy użyciu [Programu AD PowerShell Azure](../powershell-install-configure.md), możesz użyć.  Możesz zmienić `$st.State` równe jeden z następujących stanów:

- Włączone
- Wymuszone
- Wyłączone  

> [AZURE.IMPORTANT]  Firma Microsoft zapobieżenia go przed przenoszenie użytkowników bezpośrednio w stan wyłączenia stan wymuszone. Aplikacje oparte na przeglądarce nie zatrzyma działa, ponieważ użytkownik nie ma przeszli rejestracji MFA i uzyskane w wyniku [hasło aplikacji](multi-factor-authentication-whats-next.md#app-passwords). Jeśli masz nie opartych na przeglądarce aplikacje i włączyć żądanie podania hasła aplikacji, zalecamy przejść ze stanu wyłączone na włączone. Dzięki temu użytkownicy zarejestrować i uzyskać ich hasła aplikacji. Po wykonaniu tej można przenieść je w wymuszone.

Przy użyciu programu PowerShell będzie opcję zbiorcze, umożliwiając użytkownikom. Obecnie jest nie opcję Włącz zbiorcze w portalu Azure i należy wybrać poszczególnych użytkowników pojedynczo. Jeśli wielu użytkowników, może to być bardzo zadania. Tworząc programu PowerShell skryptów za pomocą następujących, można przeglądać listę użytkowników i włączyć je.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Oto przykład:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Aby uzyskać więcej informacji zobacz [Państw użytkownika w uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz skonfigurowane uwierzytelnianie wieloskładnikowe Azure w chmurze, możesz skonfigurować i konfigurowanie wdrożenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie uwierzytelniania wieloskładnikowego Azure](multi-factor-authentication-whats-next.md) .
