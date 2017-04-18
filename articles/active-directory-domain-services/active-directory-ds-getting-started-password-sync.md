<properties
    pageTitle="Azure usługach domenowych AD: Włącz synchronizację hasło | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Włączanie synchronizacji haseł do usług domenowych AD Azure
W poprzednim zadania Jeśli włączono usługi Azure AD domen dla dzierżawy usługi Azure AD. Następnego zadania jest umożliwienie mieszania poświadczeń wymaganych do uwierzytelniania NTLM i Kerberos do synchronizacji do usług domenowych AD Azure. Po skonfigurowaniu synchronizacji poświadczeń użytkownicy mogli logować się do zarządzanych domeny przy użyciu poświadczeń firmy.

Etapy różnią się według tego, czy Twoja organizacja ma tylko do chmury Azure AD dzierżawa lub jest ustawiony na synchronizację z katalogu lokalnego przy użyciu Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Dzierżawa Azure AD tylko do chmury](active-directory-ds-getting-started-password-sync.md)
- [Synchronizowane dzierżawy Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Zadanie 5: Włącz synchronizację hasło usługi domeny AAD tylko do chmury Azure AD dzierżawy
Azure usług domenowych AD wymaga poświadczeń mieszania w formacie odpowiednim uwierzytelniania Kerberos i NTLM do uwierzytelniania użytkowników w domenie zarządzane. Jeśli włączysz usług domenowych AAD dla dzierżawy, Azure AD Generowanie lub nie przechowywanie poświadczeń mieszania w formacie wymagane dla uwierzytelniania NTLM lub Kerberos. Ze względów bezpieczeństwa oczywiste Azure AD również nie są zapisywane poświadczeń w postaci zwykłego tekstu. Azure AD nie jest więc umożliwia generowanie poświadczeń mieszania tych NTLM lub Kerberos istniejących poświadczeń użytkowników.

> [AZURE.NOTE] Jeśli Twoja organizacja ma tylko do chmury Azure AD dzierżawy, użytkowników, których będziesz używać usług domenowych AD Azure należy zmieniać swoje hasła.

Ten proces Zmień hasło powoduje, że poświadczenia mieszania wymagane przez usługi Azure AD domeny dla uwierzytelniania Kerberos i NTLM do generowania Azure AD. Można albo wygasania haseł dla wszystkich użytkowników w dzierżawie, którego chcesz używać usług domenowych AD Azure lub poinstruuj tych użytkowników, zmieniać swoje hasła.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Włącz NTLM i Kerberos Generowanie mieszania poświadczeń dla tylko do chmury Azure AD dzierżawy
Poniżej zamieszczono instrukcje, aby zapewnić użytkownikom końcowym, aby mogli oni zmieniać swoje hasła:

1. Przejdź do strony Panel Azure AD dostępu dla Twojej organizacji na [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Wybierz kartę **profilu** na tej stronie.

3. Kliknij Kafelek **Zmienianie hasła** na tej stronie.

    ![Tworzenie wirtualnych sieci usługi Azure AD domeny.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Jeśli opcja **Zmień hasło** na stronie panelu programu Access nie jest widoczna, upewnij się, że Twoja organizacja ma skonfigurowane [Zarządzanie hasłami w Azure AD](../active-directory/active-directory-passwords-getting-started.md).

4. Na stronie **Zmienianie hasła do** istniejącego (starego) hasło wpisz nowe hasło i potwierdź je. Kliknij przycisk **Prześlij**.

    ![Tworzenie wirtualnych sieci usługi Azure AD domeny.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Po zmianie hasła, nowe hasło będzie możliwe w usługach domenowych AD Azure wkrótce. Po kilku minutach (zazwyczaj minut około 20), możesz się zalogować na komputerach dołączony do domeny zarządzane przy użyciu nowo zmiany hasła.

<br>

## <a name="related-content"></a>Zawartość pokrewna

- [Jak zaktualizować swoje hasło](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Wprowadzenie do zarządzania hasłami w Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Włącz synchronizację haseł, aby usługi domenowe AAD zsynchronizowanej Azure AD dzierżawy](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-administer-domain.md)

- [Połączenia maszyny wirtualnej systemu Windows Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-join-windows-vm.md)

- [Dołączanie do maszyny wirtualnej Red funkcję Enterprise Linux w domenie usług domenowych AD Azure zarządzanych](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
