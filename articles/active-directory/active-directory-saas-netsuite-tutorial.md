<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z NetSuite | Microsoft Azure"
    description="Dowiedz się, jak użyć NetSuite z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Samouczek: Jak zintegrować NetSuite z usługi Azure Active Directory

Ten samouczek zostanie wyświetlona jak połączyć środowiska NetSuite z usługi Azure Active Directory (Azure AD). Dowiesz się, jak skonfigurować rejestracji jednokrotnej do NetSuite, jak włączyć automatyczne użytkownika inicjowania obsługi administracyjnej i jak przypisać użytkownicy będą mieli dostęp do NetSuite. 

##<a name="prerequisites"></a>Wymagania wstępne

1. Aby uzyskać dostęp do usługi Azure Active Directory za pośrednictwem [portalu klasyczny Azure](https://manage.windowsazure.com), musisz najpierw włączyć ważna subskrypcja Azure.

2. Musi mieć dostęp administratora do subskrypcji [NetSuite](http://www.netsuite.com/portal/home.shtml) . Można użyć dowolnej usługi bezpłatne konto wersji próbnej.

##<a name="step-1-add-netsuite-to-your-directory"></a>Krok 1: Dodawanie NetSuite do katalogu

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Wybierz pozycję usługi Active Directory w okienku nawigacji po lewej stronie.][0]

2. Na liście **katalogu** zaznacz katalogu, w którym chcesz dodać NetSuite do.

3. W górnym menu wybierz polecenie **aplikacji** .

    ![Polecenie aplikacje.][1]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Kliknij przycisk Dodaj, aby dodać nową aplikację.][2]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Kliknij pozycję Dodaj aplikację z galerii.][3]

6. W **polu wyszukiwania**wpisz **NetSuite**. Następnie wybierz **NetSuite** z wyników, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Dodaj NetSuite.][4]

7. Powinien zostać wyświetlony strony Szybki Start dla NetSuite:

    ![Osoby NetSuite Szybki Start strony w Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Krok 2: Włączanie rejestracji jednokrotnej

1. W Azure AD na stronie Szybki Start dla NetSuite kliknij przycisk **Konfiguruj rejestracji jednokrotnej** .

    ![Pojedynczy znak na przycisk Konfiguruj][6]

2. Okno dialogowe zostanie otwarty i zostanie wyświetlony ekran, z pytaniem "Jak chcesz użytkownikom logowanie do NetSuite?" Zaznacz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Wybierz pozycję Azure AD rejestracji jednokrotnej][7]

    > [AZURE.NOTE] Aby uzyskać więcej informacji o różnych pojedynczego logowania jednokrotnego opcji, [kliknij tutaj](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Na stronie **Konfigurowanie ustawień aplikacji** w polu **Odpowiedz adres URL** wpisz adres URL usługi NetSuite dzierżawy przy użyciu jednego z następujących formatów:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Wpisz adres URL swojej dzierżawy][8]

4. Na stronie **Konfigurowanie logowania jednokrotnego w NetSuite** polecenie **Pobierz metadanych**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Pobierz metadanych.][9]

5. Otwieranie nowej karcie w przeglądarce, a następnie zaloguj się do witryny firmy Netsuite jako administrator.

6. Na pasku narzędzi u góry strony kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Menedżer konfiguracji**.

    ![Przejdź do Menedżera konfiguracji][10]

7. Na liście **Zadań konfiguracyjnych** zaznacz **integracji**.

    ![Przejdź do integracji][11]

8. W sekcji **Zarządzanie uwierzytelnianie** kliknij **SAML rejestracji jednokrotnej**.

    ![Przejdź do pozycji SAML logowania jednokrotnego][12]

9. Na stronie **Konfiguracja SAML** wykonaj następujące czynności:

    - W usłudze Azure Active Directory skopiuj wartość **Adres URL logowania zdalnego** i wkleić go w polu **Strony logowania dostawcy tożsamości** w NetSuite.

        ![Strona konfiguracji SAML.][13]

    - W NetSuite wybierz **Metodę uwierzytelniania podstawowego**.

    - W polu etykietą **Metadanych dostawcy tożsamości SAMLV2**wybierz pozycję **Przekaż plik metadanych protokołu IDP**. Następnie kliknij przycisk **Przeglądaj,** Aby przekazać plik metadanych pobranego w kroku #4.

        ![Przekazywanie metadanych][16]

    - Kliknij przycisk **Prześlij**.

9. W Azure AD zaznacz pole wyboru potwierdzenia konfiguracji rejestracji jednokrotnej umożliwiające certyfikatu, które zostały przekazane do NetSuite. Następnie kliknij przycisk **Dalej**.

    ![Zaznacz pole wyboru potwierdzenia][14]

10. Na ostatniej stronie okna dialogowego Jeśli chcesz otrzymywać powiadomienia e-mail dla błędów i ostrzeżeń związany z zachowaniem tej jednej konfiguracji logowania jednokrotnego wpisz w polu adres e-mail. 

    ![Wpisz swój adres e-mail.][15]

11. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe. Następnie wybierz polecenie **atrybuty** w górnej części strony.

    ![Polecenie atrybuty.][17]

12. Wybierz polecenie **Dodaj atrybut użytkownika**.

    ![Kliknij przycisk Dodaj atrybut użytkownika.][18]

13. W polu **Nazwa atrybutu** wpisz w `account`. W polu **Wartość atrybutu** w wpisz swój identyfikator NetSuite konta. Poniżej przedstawiono instrukcje na temat znaleźć identyfikator konta:

    ![Dodawanie identyfikatora konto usługi NetSuite.][19]

    - W NetSuite kliknij **Ustawienia** w górnym menu nawigacji.
    - Następnie kliknij menu nawigacji po lewej stronie, w sekcji **Zadań konfiguracyjnych** , wybierz sekcję **integracji** i wybierz polecenie **Preferencje usług sieci Web**.
    - Identyfikator konta NetSuite skopiuj i wklej je w polu **Wartość atrybutu** Azure AD.

        ![Uzyskaj identyfikator konta][20]

14. W Azure AD kliknij **ukończone** , aby zakończyć dodawanie atrybutu SAML. Następnie kliknij przycisk **Zastosuj zmiany** w menu dołu.

    ![Zapisz wprowadzone zmiany.][21]

15. Zanim użytkownik może wykonać rejestracji jednokrotnej do NetSuite, one należy najpierw przypisane odpowiednie uprawnienia w NetSuite. Postępuj zgodnie z instrukcjami poniżej, aby przypisać uprawnienia.

    - W górnym menu nawigacji kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Menedżer konfiguracji**.

        ![Przejdź do Menedżera konfiguracji][10]

    - W menu nawigacji po lewej stronie wybierz pozycję **Użytkownicy i role**, a następnie kliknij pozycję **Zarządzanie rolami**.

        ![Przejdź do strony Zarządzanie rolami][22]

    - Kliknij pozycję **nowej roli**.

    - Wpisz **nazwę** dla nowej roli, a następnie zaznacz pole wyboru **Pojedynczego logowania jednokrotnego tylko** .

        ![Nazwa nowej roli.][23]

    - Kliknij przycisk **Zapisz**.

    - W menu u góry kliknij przycisk **uprawnienia**. Kliknij pozycję **Ustawienia**.

        ![Przejdź do pozycji uprawnienia][24]

    - Wybierz pozycję **Ustaw w górę SAM rejestracji jednokrotnej**, a następnie kliknij przycisk **Dodaj**.

    - Kliknij przycisk **Zapisz**.

    - W górnym menu nawigacji kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Menedżer konfiguracji**.

        ![Przejdź do Menedżera konfiguracji][10]

    - W menu nawigacji po lewej stronie wybierz pozycję **Użytkownicy i role**, a następnie kliknij pozycję **Zarządzaj użytkownikami**.

        ![Przejdź do strony Zarządzanie użytkownikami][25]

    - Wybierz użytkownika testowego. Następnie kliknij przycisk **Edytuj**.

        ![Przejdź do strony Zarządzanie użytkownikami][26]

    - W oknie dialogowym role wybierz rolę, został utworzony, a następnie kliknij przycisk **Dodaj**.

        ![Przejdź do strony Zarządzanie użytkownikami][27]

    - Kliknij przycisk **Zapisz**.

19. Aby przetestować konfigurację, zobacz poniższą sekcję zatytułowany [Przypisać użytkownikom NetSuite](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Krok 3: Włącz automatyczne przypisywanie użytkowników

> [AZURE.NOTE] Domyślnie ustanawianie użytkowników zostaną dodane do przedstawicielstwo główny środowiska NetSuite.

1. W usługi Azure Active Directory na stronie Szybki Start dla NetSuite, kliknij **Konfiguruj użytkownika inicjowania obsługi administracyjnej**.

    ![Konfigurowanie użytkowników inicjowania obsługi administracyjnej.][28]

2. W oknie dialogowym, które zostanie otwarte wpisz poświadczeń administratora NetSuite, a następnie kliknij przycisk **Dalej**.

    ![Wpisz NetSuite poświadczeń administratora.][29]

3. Na stronie potwierdzenia wpisz adres e-mail, aby otrzymywać powiadomienia o błędy inicjowania obsługi administracyjnej.

    ![Potwierdź.][30]

4. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe.

##<a name="step-4-assign-users-to-netsuite"></a>Krok 4: Przypisywanie użytkowników do NetSuite

1. Aby przetestować konfigurację, mieli możliwość tworzenia nowego konta test w katalogu.

2. Na stronie NetSuite Szybki Start kliknij przycisk **Przypisz użytkowników** .

    ![Kliknij pozycję Przypisz użytkowników][31]

3. Wybierz użytkownika test, a następnie kliknij przycisk **Przypisz** u dołu ekranu:

 - Jeśli jeszcze tego nie włączyć automatyczną użytkownika inicjowania obsługi administracyjnej, a następnie pojawi się następujący monit o potwierdzenie:

        ![Confirm the assignment.][32]

 - Po włączeniu automatycznego przypisywania użytkowników, następnie pojawi się monit, aby zdefiniować, jakiego rodzaju roli, użytkownik musi mieć w NetSuite. Nowo ustanawianie użytkownicy będą widoczne w środowisku NetSuite po upływie kilku minut.

4. Aby przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu na [https://myapps.microsoft.com](https://myapps.microsoft.com/), zaloguj się do konta testowego i kliknij na **NetSuite**.

##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
