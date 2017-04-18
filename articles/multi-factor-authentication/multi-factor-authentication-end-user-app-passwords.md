<properties
    pageTitle="Co to są hasła aplikacji w Azure MFA?"
    description="Ta strona pomoże użytkownikom zrozumieć, co to są hasła aplikacji i co to są używane dla z uwzględnieniem Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Co to są hasła aplikacji w uwierzytelnianie wieloskładnikowe Azure?

Niektórych aplikacji innych niż przeglądarki, takich jak Apple klienta natywnych poczty e-mail, który używa programu Exchange Active Sync nie obsługuje obecnie uwierzytelnianie wieloskładnikowe. Uwierzytelnianie wieloskładnikowe jest włączone dla poszczególnych użytkowników. Oznacza to, że jeśli użytkownik został włączony uwierzytelniania wieloskładnikowego i próbuje za pomocą aplikacji innych niż przeglądarki, będą one zakończyła się. Hasła umożliwia to możliwe.

>[AZURE.NOTE] Nowoczesna uwierzytelniania dla klientów pakietu Office 2013
>
> Klienci pakietu Office 2013 (z programem Outlook) teraz obsługiwać nowe protokoły uwierzytelniania i mogą mieć dostęp do obsługi uwierzytelniania wieloskładnikowego.  Oznacza to, że po włączeniu hasła aplikacji nie są wymagane do użytku z klientami pakietu Office 2013.  Aby uzyskać więcej informacji wyświetlony [Podgląd publicznej nowoczesny uwierzytelniania pakietu Office 2013 ogłoszenia](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Jak za pomocą hasła aplikacji

Oto kilka rzeczy do zapamiętania na temat używania haseł aplikacji.

- Rzeczywisty hasło jest generowane automatycznie i nie został dostarczony przez użytkownika. To jest ponieważ automatycznie wygenerowanego hasło jest trudniejsze intruz odgadnięcie i zabezpieczyć.
- Obecnie nie istnieje ograniczenie liczby 40 haseł dla poszczególnych użytkowników. Jeśli spróbujesz utworzyć po osiągnięto limit pojawi Aby usunąć jeden z istniejących haseł aplikacji w celu utworzenia nowej witryny.
- Zaleca się, że hasła aplikacji można tworzyć na urządzenie, a nie dla aplikacji. Można na przykład utworzyć hasło aplikacji dla komputera przenośnego i używać tego hasła aplikacji dla wszystkich aplikacji na tym komputerze przenośnym.
- Hasła są podane po raz pierwszy należy logowania.  Jeśli potrzebujesz dodatkowych z nich, można je tworzyć.

![Konfiguracja](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Po utworzeniu hasła to zamiast używać oryginalne hasło z tych aplikacji innych niż przeglądarki.  Aby na przykład, jeśli używasz uwierzytelnianie wieloskładnikowe i klienta programu Apple natywnych poczty e-mail na telefonie.  Tak, aby można pominąć uwierzytelnianie wieloskładnikowe i kontynuować pracę przy użyciu hasła aplikacji.

## <a name="creating-and-deleting-app-passwords"></a>Tworzenie i usuwanie hasła aplikacji
Podczas Twojej początkowej logowania są podane hasło aplikacji, którego można używać.  Ponadto możesz również tworzyć i usuwać hasła aplikacji później.  Jak to zrobić, zależy od tego, jak używać uwierzytelnianie wieloskładnikowe.  Wybierz to, że większość warunków.

Jak używać żądanie wieloskładnikowego|Opis
:------------- | :------------- |
[Korzystam z usługi Office 365](#creating-and-deleting-app-passwords-with-office-365)|  Oznacza to, że można tworzyć hasła aplikacji portalu usługi Office 365.
[Nie wiem](#creating-and-deleting-app-passwords-with-myapps-portal)|Oznacza to, że można utworzyć hasła aplikacji za pomocą [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Korzystam z Microsoft Azure](#create-app-passwords-in-the-azure-portal)| Oznacza to, że można utworzyć hasła aplikacji portalu Azure.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Tworzenie i usuwanie hasła aplikacji usługi Office 365

Uwierzytelnianie wieloskładnikowe usługi Office 365 można tworzyć i usuwać hasła aplikacji za pomocą portalu usługi Office 365.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Tworzenie hasła aplikacji w portalu usługi Office 365
--------------------------------------------------------------------------------

1. Zaloguj się do [portalu usługi Office 365](https://login.microsoftonline.com/).
2. W prawym górnym rogu wybierz widżetu i wybierz ustawienia usługi Office 365.
3. Polecenie weryfikacji dodatkowe zabezpieczenia.
4. Po prawej stronie, kliknij łącze informacją, że **aktualizować Moje numery telefonów, używany do zabezpieczenia konta.** 
 ![Konfiguracji](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Spowoduje to przejście do strony, którą będzie można zmienić jego ustawienia.
![Chmury](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. U góry, obok weryfikacji dodatkowe zabezpieczenia kliknij **hasła aplikacji.**
7. Kliknij przycisk **Utwórz**.
![Chmury](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**.
![Tworzenie hasła](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.
![Tworzenie hasła](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Aby usunąć hasła aplikacji przy użyciu portalu usługi Office 365
--------------------------------------------------------------------------------


1. Zaloguj się do [portalu usługi Office 365](https://login.microsoftonline.com/).
2. W prawym górnym rogu wybierz widżetu i wybierz ustawienia usługi Office 365.
3. Polecenie weryfikacji dodatkowe zabezpieczenia.
4. Po prawej stronie, kliknij łącze informacją, że **zaktualizować Moje numery telefonów, używany do zabezpieczenia konta.** 
 ![Konfiguracji](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Spowoduje to przejście do strony, którą będzie można zmienić jego ustawienia.
![Chmury](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. U góry, obok weryfikacji dodatkowe zabezpieczenia kliknij **hasła aplikacji.**
7. W obszarze hasło aplikacji, którą chcesz usunąć kliknij przycisk **Usuń**.
![Usuwanie hasła](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Potwierdź usunięcie, klikając pozycję **Tak**.
![Potwierdzanie usunięcia](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Po usunięciu hasła aplikacji kliknij przycisk **Zamknij**.
![Zamknij](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Tworzenie i usuwanie hasła aplikacji z portalem Moje aplikacje.
Jeśli nie masz pewności, jak używać uwierzytelnianie wieloskładnikowe, następnie możesz zawsze tworzenie i usuwanie hasła aplikacji portalu Moje aplikacje.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Tworzenie hasła za pomocą portalu Moje aplikacje

1. Zaloguj się do [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. U góry wybierz profil.
3. Wybierz pozycję Weryfikacja dodatkowe zabezpieczenia.
![Chmury](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Spowoduje to przejście do strony, którą będzie można zmienić jego ustawienia.
![Konfiguracja](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. U góry, obok weryfikacji dodatkowe zabezpieczenia kliknij **hasła aplikacji.**
6. Kliknij przycisk **Utwórz**.
![Tworzenie hasła](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**.
![Tworzenie hasła](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.
![Tworzenie hasła](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Aby usunąć hasło aplikacji za pomocą portalu Moje aplikacje

1. Zaloguj się do [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. U góry wybierz profil.
3. Wybierz pozycję Weryfikacja dodatkowe zabezpieczenia.
![Chmury](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Spowoduje to przejście do strony, którą będzie można zmienić jego ustawienia.
![Konfiguracja](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. U góry, obok weryfikacji dodatkowe zabezpieczenia kliknij **hasła aplikacji.**
6. W obszarze hasło aplikacji, którą chcesz usunąć kliknij przycisk **Usuń**.
![Usuwanie hasła](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Potwierdź usunięcie, klikając pozycję **Tak**.
![Potwierdzanie usunięcia](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Po usunięciu hasła aplikacji kliknij przycisk **Zamknij**.
![Zamknij](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Tworzenie hasła aplikacji w portalu Azure

Jeśli używasz uwierzytelnianie wieloskładnikowe z Azure można tworzyć hasła aplikacji portalu Azure.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Tworzenie hasła aplikacji w portalu Azure

1. Zaloguj się do portalu zarządzania Azure.
2. U góry kliknij prawym przyciskiem myszy nazwę użytkownika, a następnie zaznacz dodatkowe Weryfikacja zabezpieczeń.
3. Na stronie proofup u góry wybierz pozycję hasła aplikacji
4. Kliknij przycisk **Utwórz**.
5. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**
6. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.
![Chmury](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Aby usunąć hasła aplikacji w portalu Azure

1. Zaloguj się do portalu zarządzania Azure.
2. U góry kliknij prawym przyciskiem myszy nazwę użytkownika, a następnie zaznacz dodatkowe Weryfikacja zabezpieczeń.
3. U góry, obok weryfikacji dodatkowe zabezpieczenia kliknij **hasła aplikacji.**
4. W obszarze hasło aplikacji, którą chcesz usunąć kliknij przycisk **Usuń**.
5. Potwierdź usunięcie, klikając pozycję **Tak**.
6. Po usunięciu hasła aplikacji kliknij przycisk **Zamknij**.
![Zamknij](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
