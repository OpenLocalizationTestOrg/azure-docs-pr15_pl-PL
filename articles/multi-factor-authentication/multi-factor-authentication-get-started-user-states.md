<properties 
    pageTitle="Stany Microsoft Azure wieloskładnikowe uwierzytelniania użytkownika"
    description="Informacje na temat Państw użytkownika w Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="user-states-in-azure-multi-factor-authentication"></a>Zjednoczone użytkownika w Azure uwierzytelnianie wieloskładnikowe

Konta użytkowników w uwierzytelnianie wieloskładnikowe Azure mają następujące trzy różne stany:

Województwo | Opis |Aplikacje przeglądarki nie dotyczy| Notatki
:-------------: | :-------------: |:-------------: |:-------------: |
Wyłączone | Stan domyślny dla nowego użytkownika nie jest zarejestrowana w uwierzytelnianie wieloskładnikowe.|Brak|Użytkownik nie używa uwierzytelnianie wieloskładnikowe.
Włączone |Użytkownik ma zarejestrowanych w uwierzytelnianie wieloskładnikowe.|Wartość nie.  Ich będą nadal działać, dopóki nie zakończy się procesu rejestracji.|Użytkownik jest włączona, ale nie została ukończona procesu rejestracji. Aby ukończyć proces w następnym logowania zostanie wyświetlony monit.
Wymuszone|Użytkownik ma zarejestrowanych i zakończeniu procesu rejestracji przy użyciu funkcji uwierzytelniania wieloskładnikowego.|Wartość Tak.  Aplikacje wymagają hasła aplikacji. | Użytkownik może lub nie została ukończona rejestracji. Jeśli zakończeniu procesu rejestracji, ta osoba używa uwierzytelnianie wieloskładnikowe. W przeciwnym razie użytkownik zostanie wyświetlony monit, aby ukończyć proces w następnym logowania.

## <a name="changing-a-user-state"></a>Zmienianie stanu użytkownika
Stan użytkowników zmieni się w zależności od tego, czy jej została skonfigurowana do uwierzytelniania MFA i tego, czy użytkownik zakończy procesu.  Po włączeniu MFA dla użytkownika, użytkowników, dla których stan zmieni się z wyłączone, aby włączyć.  Gdy użytkownik, którego stan został zmieniony na włączone, znaki w i kończy proces, ich stanu zmieni się wymuszoną.  

### <a name="to-view-a-users-state"></a>Aby wyświetlić stan użytkownika
--------------------------------------------------------------------------------
1.  Zaloguj się do **portalu klasyczny Azure** jako Administrator.
2.  Po lewej stronie kliknij pozycję **Usługi Active Directory**.
3.  W obszarze **katalogu** kliknij w katalogu dla użytkownika, które chcesz włączyć.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  U góry kliknij pozycję **Użytkownicy**.
5.  U dołu strony kliknij pozycję **Zarządzaj uwierzytelnianie wieloskładnikowe**.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Spowoduje to otwarcie nowej karcie w przeglądarce.  Będzie można wyświetlić stan użytkowników.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Aby zmienić stan z wyłączone do włączony
1.  Zaloguj się do **portalu klasyczny Azure** jako Administrator.
2.  Po lewej stronie kliknij **Usługi Active Directory**.
3.  W obszarze **katalogu** kliknij w katalogu dla użytkownika, które chcesz włączyć.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  U góry kliknij pozycję **Użytkownicy**.
5.  U dołu strony kliknij pozycję **Zarządzaj uwierzytelnianie wieloskładnikowe**.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Spowoduje to otwarcie nowej karcie w przeglądarce.  Znajdź użytkownika, którego chcesz włączyć dla uwierzytelnianie wieloskładnikowe. Może być konieczne zmienianie widoku u góry. Upewnij się, że jest w stanie **wyłączone.** 
 ![Włącz użytkowników](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Sprawdzanie** należy umieścić w polu obok jego nazwy.
7.  Po prawej stronie kliknij polecenie **Włącz**.
![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kliknij opcję **Włącz uwierzytelnianie wieloskładnikowe**.
![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Należy zauważyć, że stan użytkownika zmieniono z **wyłączone** na **włączone**.
![Umożliwianie użytkownikom](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Po włączeniu użytkowników, zaleca się powiadamia je pocztą e-mail.  Należy również informuje ich jak mogą używać swoich aplikacji innych niż przeglądarki w celu uniknięcia jest zablokowane.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Aby zmienić stan z włączoną wymuszane na wyłączone
1.  Zaloguj się do **portalu klasyczny Azure** jako Administrator.
2.  Po lewej stronie kliknij pozycję **Usługi Active Directory**.
3.  W obszarze **katalogu** kliknij w katalogu dla użytkownika, które chcesz włączyć.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  U góry kliknij pozycję **Użytkownicy**.
5.  U dołu strony kliknij pozycję **Zarządzaj uwierzytelnianie wieloskładnikowe**.
![Kliknij pozycję w katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Spowoduje to otwarcie nowej karcie w przeglądarce.  Znajdź użytkownika, którego chcesz wyłączyć. Może być konieczne zmienianie widoku u góry. Upewnij się, że stan jest albo **włączone** lub **wymuszane.**
7.  **Sprawdzanie** należy umieścić w polu obok jego nazwy.
7.  Po prawej stronie kliknij przycisk **Wyłącz**.
![Wyłączanie użytkownika](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Użytkownik zostanie wyświetlony monit o potwierdzenie to.  Kliknij przycisk **Tak**.
![Wyłączanie użytkownika](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Następnie powinna być widoczna, że jej zakończyło się pomyślnie.  Kliknij pozycję **zamknąć.** 
 ![Wyłączanie użytkownika](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
