<properties 
    pageTitle="Raporty Azure uwierzytelnianie wieloskładnikowe"
    description="To w tym artykule opisano sposób zmieniania ustawień użytkownika, takie jak wymuszanie użytkownikom ponownie wykonaj procedurę dowód w górę."
    documentationCenter=""
    services="multi-factor-authentication"
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

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Zarządzanie ustawieniami użytkownika z uwierzytelnianie wieloskładnikowe Azure w chmurze

Jako administrator możesz zarządzać następujące ustawienia użytkownika i urządzenie.  

- [Wymaganie wybranych użytkowników zapewnienie metody kontaktu](#require-selected-users-to-provide-contact-methods-again)
- [Usuwanie użytkowników istniejące hasła aplikacji](#delete-users-existing-app-passwords)
- [Przywracanie MFA na wszystkich urządzeniach zawieszone dla użytkownika](#restore-mfa-on-all-suspended-devices-for-a-user)






Jest to przydatne, jeśli komputer lub urządzenie zostanie utracony lub kradzież lub należy usunąć dostępu użytkowników.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Wymaganie wybranych użytkowników zapewnienie metody kontaktu

Spowoduje to ustawienie wymusza na użytkowniku, aby ukończyć proces rejestracji ponownie, gdy dana osoba znaki w. Należy pamiętać, że aplikacji przeglądarki nie będzie działać, jeśli użytkownik ma hasła aplikacji dla nich.  Hasła użytkowników aplikacji można usunąć, zaznaczając również **usunąć wszystkie istniejące hasła aplikacji wygenerowane przez wybranych użytkowników**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Jak użytkownicy musieli ponownie podać metody kontaktu




1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze katalogu kliknij w katalogu dla użytkownika ma być wymagane zapewnienie ich metody kontaktu.
4. U góry kliknij pozycję Użytkownicy.
5. U dołu strony kliknij pozycję Zarządzaj uwierzytelniania wieloskładnikowego Spowoduje to otwarcie strony uwierzytelnianie wieloskładnikowe.
6. Znajdź użytkownika, który chcesz zaznaczyć pole znajdujące się obok nazwy i zarządzanie nimi. Może być konieczne zmienianie widoku u góry.
7. To powoduje wyświetlenie łącze **Zarządzaj ustawieniami użytkownika** po prawej stronie. Kliknij przycisk.
8. Należy zaznaczyć **wybrany użytkownicy musieli podać ponownie metody kontaktu, z**.
![Podaj metody kontaktu](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Kliknij przycisk Zapisz.
11. Kliknij przycisk Zamknij

## <a name="delete-users-existing-app-passwords"></a>Usuwanie użytkowników istniejące hasła aplikacji

Spowoduje to usunięcie hasła aplikacji utworzonych przez użytkownika. Przeglądarki inne niż aplikacje, które zostały skojarzone z tych hasła aplikacji przestanie działać, dopóki nie zostanie utworzone nowe hasło aplikacji.

### <a name="how-to-delete-users-existing-app-passwords"></a>Jak usunąć użytkowników istniejące hasła aplikacji

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze katalogu kliknij katalog dla użytkownika, którego chcesz usunąć hasła aplikacji.
4. U góry kliknij pozycję Użytkownicy.
5. U dołu strony kliknij pozycję Zarządzaj uwierzytelniania wieloskładnikowego Spowoduje to otwarcie strony uwierzytelnianie wieloskładnikowe.
6. Znajdź użytkownika, który chcesz zaznaczyć pole znajdujące się obok nazwy i zarządzanie nimi. Może być konieczne zmienianie widoku u góry.
7. To powoduje wyświetlenie łącze **Zarządzaj ustawieniami użytkownika** po prawej stronie. Kliknij przycisk.
8. Umieść znacznik wyboru w polu **Usuń wszystkie istniejące hasła aplikacji wygenerowane przez wybranych użytkowników**.
![Usuwanie hasła aplikacji](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Kliknij przycisk Zapisz.
10. Kliknij przycisk Zamknij.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Przywracanie MFA na wszystkich urządzeniach zapamiętane dla użytkownika

Administratorzy mają możliwość przywracania uwierzytelnianie wieloskładnikowe na urządzeniach użytkowników i przeglądarek. W ten sposób Zapamiętaj MFA zostanie usunięty z wszystkich urządzeń użytkownika i przeglądarki i użytkownik będzie wymagane ma być używana w następnym logowaniu MFA.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Jak przywrócić MFA na wszystkich urządzeniach zawieszone dla użytkownika

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij usługi Active Directory.
3. W obszarze katalogu kliknij katalog dla użytkownika, którego chcesz przywrócić mfa na komputerze.
4. U góry kliknij pozycję Użytkownicy.
5. U dołu strony kliknij pozycję Zarządzaj uwierzytelniania wieloskładnikowego Spowoduje to otwarcie strony uwierzytelnianie wieloskładnikowe.
6. Znajdź użytkownika, który chcesz zaznaczyć pole znajdujące się obok nazwy i zarządzanie nimi. Może być konieczne zmienianie widoku u góry.
7. To powoduje wyświetlenie łącze **Zarządzaj ustawieniami użytkownika** po prawej stronie. Kliknij przycisk.
8. Należy zaznaczyć **przywrócić uwierzytelnianie wieloskładnikowe na wszystkich urządzeniach do zapamiętania**
![Usuwanie hasła aplikacji](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Kliknij przycisk Zapisz.
10. Kliknij przycisk Zamknij.
