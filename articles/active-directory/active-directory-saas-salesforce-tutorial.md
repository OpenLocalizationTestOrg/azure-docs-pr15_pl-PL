<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z usług Salesforce | Microsoft Azure"
    description="Dowiedz się, jak użyć usług Salesforce z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Samouczek: Jak integracja usług Salesforce z usługi Azure Active Directory

Ten samouczek zostanie wyświetlona jak połączyć środowiska usługi Salesforce usługi Azure Active Directory. Dowiesz się, jak skonfigurować rejestracji jednokrotnej do usług Salesforce, jak włączyć automatyczne użytkownika inicjowania obsługi administracyjnej i jak przypisać użytkownicy będą mieli dostęp do usług Salesforce.

##<a name="prerequisites"></a>Wymagania wstępne

1. Aby uzyskać dostęp do usługi Azure Active Directory za pośrednictwem [portalu klasyczny Azure](https://manage.windowsazure.com), musisz najpierw włączyć ważna subskrypcja Azure.

2. W [Salesforce.com](https://www.salesforce.com/)musi być ważnej dzierżawy.

> [AZURE.IMPORTANT] Jeśli korzystasz z witryny Salesforce.com konto **wersji próbnej** , będzie on mógł skonfigurować automatyczne użytkownika inicjowania obsługi administracyjnej. Konta wersji próbnej bez potrzeby dostępu interfejsu API włączone do czasu zostały one nabyte.
> 
> Możesz uzyskać wokół to ograniczenie przy użyciu [konta dewelopera bezpłatnej](https://developer.salesforce.com/signup) do użycia tego samouczka.

Jeśli używasz środowiska usługi Salesforce w trybie piaskownicy, zobacz [Samouczek Integracja usług Salesforce piaskownicy](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Samouczki wideo

Możesz mogą skorzystać z tego samouczka, za pomocą poniższych wideo.

**Samouczek wideo jedną część: jak włączyć logowania jednokrotnego**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Samouczek wideo część dwa: Jak zautomatyzować, przypisywanie użytkowników**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Krok 1: Dodawanie usług Salesforce do katalogu

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Wybierz pozycję usługi Active Directory w okienku nawigacji po lewej stronie.][0]

2. Na liście **katalogu** zaznacz katalogu, w którym chcesz dodać usług Salesforce do.

3. W górnym menu wybierz polecenie **aplikacji** .

    ![Polecenie aplikacje.][1]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Kliknij przycisk Dodaj, aby dodać nową aplikację.][2]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Kliknij pozycję Dodaj aplikację z galerii.][3]

6. W **polu wyszukiwania**wpisz **usług Salesforce**. Następnie wybierz z listy wyników **usług Salesforce** i kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Dodawanie usług Salesforce.][4]

7. Powinien zostać wyświetlony strony Szybki Start dla usługi Salesforce:

    ![Usługi SalesForce i Szybki Start strony w Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Krok 2: Włączanie rejestracji jednokrotnej

1. Przed skonfigurowaniem rejestracji jednokrotnej, należy skonfigurować i wdrażanie domeny niestandardowej w środowisku usługi Salesforce. Aby uzyskać instrukcje, jak to zrobić zobacz [Ustawianie nazwy domeny](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Strona usługi Salesforce i Szybki Start w Azure AD kliknij przycisk **Konfiguruj rejestracji jednokrotnej** .

    ![Pojedynczy znak na przycisk Konfiguruj][6]

3. Okno dialogowe zostanie otwarty i zostanie wyświetlony ekran, z pytaniem "Jak chcesz użytkownikom logowanie do usługi Salesforce?" Zaznacz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Wybierz pozycję Azure AD rejestracji jednokrotnej][7]

    > [AZURE.NOTE] Aby uzyskać więcej informacji o różnych pojedynczego logowania jednokrotnego opcji, [kliknij tutaj](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. Na stronie **Konfigurowanie ustawień aplikacji** wypełnij **Logowania na adres URL** , wpisując adres URL domeny usługi Salesforce w następującym formacie:
 - Konto organizacji:`https://<domain>.my.salesforce.com`
 - Deweloper konto:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Wpisz informacje logowania na adres URL][8]

5. Na stronie **Konfigurowanie logowania jednokrotnego w usług Salesforce** polecenie **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Pobierz certyfikat][9]

6. Otwórz nową kartę w przeglądarkę i zaloguj się do konta administratora usługi Salesforce.

7. W okienku nawigacji **Administrator** kliknij pozycję **Kontroli bezpieczeństwa** , aby rozwinąć sekcję pokrewnych. Następnie kliknij pozycję **Ustawienia rejestracji jednokrotnej**.

    ![Kliknij pozycję Ustawienia pojedynczego logowania jednokrotnego w obszarze kontroli zabezpieczeń][10]

8. Na stronie **Ustawienia rejestracji jednokrotnej** kliknij przycisk **Edytuj** .

    ![Kliknij przycisk Edytuj][11]

    > [AZURE.NOTE] Jeśli nie można włączyć ustawienia rejestracji jednokrotnej dla konta usługi Salesforce, może być konieczne kontakt z pomocą techniczną usługi Salesforce w celu funkcja dla Ciebie dostępne.

9. Zaznacz **SAML włączone**, a następnie kliknij przycisk **Zapisz**.

    ![Wybierz pozycję SAML włączone][12]

10. Aby skonfigurować SAML pojedynczego logowania jednokrotnego ustawienia, kliknij przycisk **Nowy**.

    ![Wybierz pozycję SAML włączone][13]

11. Na stronie **SAML pojedynczego logowania jednokrotnego ustawienie edytowanie** upewnij następujące ustawienia:

    ![Zrzut ekranu przedstawiający konfiguracji, które należy][14]

 - W polu **Nazwa** wpisz przyjazną nazwę dla tej konfiguracji. Dostarczanie wartości dla **nazwy** automatycznie wypełnić pole tekstowe **Nazwa interfejsu API** .

 - W Azure AD skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu **wystawcy** w usług Salesforce.

 - W polu **tekstowym identyfikator jednostki**wpisz nazwę domeny usługi Salesforce przy użyciu następującego wzorca:
     - Konto organizacji:`https://<domain>.my.salesforce.com`
     - Konta dewelopera:`https://<domain>-dev-ed.my.salesforce.com`

 - Kliknij przycisk **Przejdź** lub **Wybierz plik** , aby otworzyć okno dialogowe **Wybieranie pliku do przekazania** , wybrać certyfikat usługi Salesforce i kliknij przycisk **Otwórz** przekazywanie certyfikatu.

 - W obszarze **Typ tożsamości SAML**wybierz **potwierdzenia zawiera nazwę użytkownika salesforce.com użytkownika**.

 - **Lokalizacja tożsamości SAML**wybierz **tożsamości w elemencie NameIdentifier instrukcji tematu**

 - W Azure AD skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej je w polu **Adres URL logowania dostawcy tożsamości** usług Salesforce.

 - Dla **Usługi zainicjowane żądanie wiązanie dostawcy**wybierz **Przekierowania HTTP**.

 - Na koniec kliknij przycisk **Zapisz** , aby zastosować SAML pojedynczego logowania jednokrotnego ustawień.

12. W okienku nawigacji po lewej stronie w usług Salesforce kliknij pozycję **Domain Management** , aby rozwinąć sekcję pokrewne, a następnie kliknij **Moje domeny**.

    ![Kliknij w mojej domenie][15]

13. Przewiń w dół do sekcji **Konfiguracji uwierzytelniania** , a następnie kliknij przycisk **Edytuj** .

    ![Kliknij przycisk Edytuj][16]

14. W sekcji **Usługa uwierzytelniania** wybierz przyjazną nazwę konfiguracji logowania jednokrotnego SAML, a następnie kliknij przycisk **Zapisz**.

    ![Wybierz pozycję konfiguracji logowania jednokrotnego][17]

    > [AZURE.NOTE] Jeśli wybrano więcej niż jedną usługę uwierzytelniania, następnie gdy użytkownicy próbują zainicjować logowania jednokrotnego w środowisku usługi Salesforce zostanie wyświetlony monit Zaznacz która usługa uwierzytelniania, które mają być Zaloguj się przy użyciu. Jeśli nie chcesz, aby tak się stało, następnie należy **pozostawić wszystkich innych usług uwierzytelniania niezaznaczone**.

15. W Azure AD zaznacz pole wyboru potwierdzenia konfiguracji rejestracji jednokrotnej umożliwiające certyfikatu, które zostały przekazane do usługi Salesforce. Następnie kliknij przycisk **Dalej**.

    ![Zaznacz pole wyboru potwierdzenia][18]

16. Na ostatniej stronie okna dialogowego Jeśli chcesz otrzymywać powiadomienia e-mail dla błędów i ostrzeżeń związany z zachowaniem tej jednej konfiguracji logowania jednokrotnego wpisz w polu adres e-mail. 

    ![Wpisz swój adres e-mail.][19]

17. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe. Aby przetestować konfigurację, zobacz poniższą sekcję zatytułowany [Przypisać użytkowników do usługi Salesforce](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Krok 3: Włącz automatyczne przypisywanie użytkowników

1. Na stronie Azure AD Szybki Start dla usług Salesforce kliknij przycisk **Konfiguruj użytkownika inicjowania obsługi administracyjnej** .

    ![Kliknij przycisk Konfigurowanie przypisywanie użytkowników][20]

2. W oknie dialogowym **Konfigurowanie użytkownika inicjowania obsługi administracyjnej** w wpisz nazwę użytkownika administratora usługi Salesforce i hasło.

    ![Wpisz w Twojej administrator nazwy użytkownika i hasła][21]

    > [AZURE.NOTE] Jeśli konfigurujesz środowisku produkcyjnym, najlepiej jest utworzenie nowego konta administratora w usług Salesforce specjalnie dla tego kroku. To konto musi mieć przypisany do niego w usług Salesforce profil **Administratora systemu** .

3. Aby uzyskać tokenu zabezpieczeń usług Salesforce, otwórz nowej karty i zaloguj się do tego samego konta administratora usługi Salesforce. W prawym górnym rogu strony kliknij swoją nazwę, a następnie kliknij pozycję **Moje ustawienia**.

    ![Kliknij swoją nazwę, a następnie kliknij pozycję Moje ustawienia][22]

4. W okienku nawigacji po lewej stronie kliknij **osobistych** aby rozwinąć sekcję pokrewne, a następnie kliknij na **Resetowanie Moje tokenu zabezpieczeń**.

    ![Kliknij swoją nazwę, a następnie kliknij pozycję Moje ustawienia][23]

5. Na stronie **Resetowanie Moje tokenu zabezpieczeń** kliknij przycisk **Resetuj tokenu zabezpieczeń** .

    ![W tym artykule ostrzeżeń.][24]

6. Sprawdzanie skrzynki odbiorczej poczty e-mail skojarzone z tym kontem administratora. Wygląd wiadomości e-mail od Salesforce.com, zawierający nowe tokenu zabezpieczającego.

7. Kopiowanie tokenu, przejdź do okna Azure AD i wkleić go w polu **Tokenu zabezpieczeń użytkownika** . Następnie kliknij przycisk **Dalej**.

    ![Wklej w tokenu zabezpieczającego][25]

8. Na stronie potwierdzenia możesz otrzymywać powiadomienia e-mail dla w przypadku wystąpienia awarii obsługi administracyjnej. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe.

    ![Wpisz swój adres e-mail, aby otrzymywać powiadomienia o][26]

##<a name="step-4-assign-users-to-salesforce"></a>Krok 4: Przypisywanie użytkowników do usługi Salesforce

1. Aby przetestować konfigurację, zacznij od tworzenia nowego konta test w katalogu.

2. Na stronie usługi Salesforce Szybki Start kliknij przycisk **Przypisz użytkowników** .

    ![Kliknij pozycję Przypisz użytkowników][27]

3. Wybierz użytkownika test, a następnie kliknij przycisk **Przypisz** u dołu ekranu:

 - Jeśli jeszcze tego nie włączyć automatyczną użytkownika inicjowania obsługi administracyjnej, a następnie pojawi się następujący monit o potwierdzenie:

        ![Confirm the assignment.][28]

 - Po włączeniu automatycznego przypisywania użytkowników, następnie pojawi się monit, aby zdefiniować, jakiego rodzaju usług Salesforce profilu, użytkownik musi mieć. Nowo ustanawianie użytkownicy będą widoczne w środowisku usługi Salesforce po upływie kilku minut.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Jeśli do środowiska **Deweloper** usług Salesforce są inicjowania obsługi administracyjnej, masz bardzo ograniczoną liczbę dostępnych licencji dostępnych dla każdego profilu. W związku z tym najlepiej do tworzenia użytkowników profilu **Chatter bezpłatne użytkownika** , który ma 4,999 dostępne licencje.

4. Aby przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu na [https://myapps.microsoft.com](https://myapps.microsoft.com/), a następnie zaloguj się do konta testowego i kliknij na **usług Salesforce**.

##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
