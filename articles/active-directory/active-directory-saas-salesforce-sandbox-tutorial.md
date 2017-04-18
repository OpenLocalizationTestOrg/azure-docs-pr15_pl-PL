<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z usług Salesforce piaskownicy | Microsoft Azure"
    description="Dowiedz się, jak użyć piaskownicy usług Salesforce z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Samouczek: Azure Active Directory Integracja z piaskownicy usług Salesforce
>[AZURE.TIP]W przypadku opinię, kliknij [tutaj](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Celem tego samouczka jest pokazanie integracji Azure i piaskownicy usługi Salesforce.  
Piasecznic umożliwiają możliwość tworzenia wielu kopii organizacji w osobnym środowiskach dla różnych celów, takich jak rozwój, testowanie i szkolenia, bez utraty danych i aplikacji w organizacji produkcji usług Salesforce.  
Aby uzyskać więcej informacji zobacz [Omówienie piaskownicy](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Piaskownicy w Salesforce.com
  
Jeśli nie masz jeszcze prawidłowych piaskownicy w Salesforce.com, należy skontaktować się usług Salesforce.
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla usługi Salesforce piaskownicy
2.  Konfigurowanie logowania jednokrotnego
3.  Włączanie domeny
4.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
5.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenariusz")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Włączanie integracji aplikacji dla usługi Salesforce piaskownicy
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla usługi Salesforce piaskownicy.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla usługi Salesforce w trybie piaskownicy, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Aplikacje")

4.  Aby otworzyć **Galerię aplikacji**, kliknij pozycję **Dodaj aplikację**, a następnie kliknij **Dodawanie aplikacji dla swojej organizacji użyć**.

    ![Co chcesz zrobić?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcesz zrobić?")

5.  W **polu wyszukiwania**wpisz **Piaskownicy usługi Salesforce**.

    ![Galeria aplikacji] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Galeria aplikacji")

6.  W okienku wyników wybierz **Piaskownicy usług Salesforce**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Piaskownicy usług SalesForce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Piaskownicy usług SalesForce")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania usług Salesforce przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracja **Usług Salesforce piaskownicy** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do usługi Salesforce piaskownicy** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Piaskownicy usług SalesForce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Piaskownicy usług SalesForce")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca `http://company.my.salesforce.com`, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Skonfigurować adres URL aplikacji")

4. Jeśli masz już skonfigurowany rejestracji jednokrotnej dla innego wystąpienia usług Salesforce piaskownicy w katalogu, musisz również skonfigurować **identyfikator** mają tę samą wartość co **zalogować na adres URL**. Pole **identyfikator** można znaleźć, zaznaczając pole wyboru **Pokaż ustawienia zaawansowane** na stronie **Konfigurowanie adresu URL aplikacji** okna dialogowego.

4.  Na stronie **Konfigurowanie logowania jednokrotnego w trybie piaskownicy usługi Salesforce** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do usługi piaskownicy usługi Salesforce jako administrator.

6.  W menu u góry kliknij przycisk **Ustawienia**.

    ![Konfiguracja] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Konfiguracja")

7.  W okienku nawigacji po lewej stronie kliknij **Kontroli zabezpieczeń**, a następnie kliknij pozycję **Ustawienia rejestracji jednokrotnej**.

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Ustawienia rejestracji jednokrotnej")

8.  W sekcji Ustawienia rejestracji jednokrotnej wykonaj następujące czynności:

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Ustawienia rejestracji jednokrotnej")

    .  Wybierz pozycję **włączone SAML**.
    
    b.  Kliknij przycisk **Nowy**.

9.  Na SAML pojedynczego logowania w sekcji Ustawienia wykonaj następujące czynności:

    ![SAML pojedynczego logowania jednokrotnego ustawienia] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML pojedynczego logowania jednokrotnego ustawienia")

    .  W polu tekstowym Nazwa wpisz nazwę konfiguracji (np.: *SPSSOWAAD\_Test*).
    
    b.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w trybie piaskownicy usługi Salesforce** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    
    c.  W polu tekstowym **Identyfikator jednostki** wpisz **https://test.salesforce.com** , jeśli jest to pierwsze wystąpienie piaskownicy usług Salesforce, które chcesz dodać do katalogu. Jeśli zostało już dodane wystąpienie z usług Salesforce piaskownicy, a następnie wpisz **Identyfikator jednostki** w **Logowania na adres URL**, który powinien być w następującym formacie:`http://company.my.salesforce.com`
    
    d.  Kliknij przycisk **Przeglądaj,** Aby przekazać pobranego certyfikatu.
    
    e.  Jako **Typ tożsamości SAML**wybierz pozycję **potwierdzenia zawiera identyfikator federacji z obiektu użytkownika**.
    
    f.  Jako **Lokalizacji tożsamości SAML**wybierz **tożsamości w elemencie NameIdentifier instrukcji tematu**.
    
    g.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w trybie piaskownicy usługi Salesforce** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .
    
    h.  SFDC nie obsługuje SAML Wyloguj.  Aby obejść ten problem, Wklej "https://login.windows.net/common/wsfederation?wa=wsignout1.0" ją w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości** .
    
    mogę.  Zaznacz **WPIS HTTP** **Usługi zainicjowane żądanie wiązanie dostawcy**.
    
    j. Kliknij przycisk **Zapisz**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurowanie logowania jednokrotnego")

##<a name="enabling-your-domain"></a>Włączanie domeny
  
W tej sekcji przyjęto założenie, że już utworzono domeny.  
Aby uzyskać więcej informacji zobacz [Definiowanie swoją nazwę domeny](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Aby włączyć domeny, wykonaj następujące czynności:

1.  W okienku nawigacji po lewej stronie kliknij pozycję **Zarządzanie domeny**, a następnie kliknij **My Domain.**

    ![Mojej domeny] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Mojej domeny")

    >[AZURE.NOTE]Upewnij się, że domena został poprawnie skonfigurowany.

2.  W sekcji **Ustawienia strony logowania** kliknij przycisk **Edytuj**, następnie **Usługa uwierzytelniania**, wybierz nazwę SAML pojedynczego logowania jednokrotnego ustawienie z poprzedniej sekcji i na koniec kliknij przycisk **Zapisz**.

    ![Mojej domeny] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Mojej domeny")
  
Jak masz skonfigurowany domeny, użytkowników należy używać adres URL domeny, aby zalogować się do usługi Salesforce piaskownicy.  
Aby uzyskać wartość adres URL, kliknij profil logowania jednokrotnego, który został utworzony w poprzedniej sekcji.
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Celem w tej sekcji jest konspektu, jak włączyć użytkownika inicjowania obsługi administracyjnej usługi Active Directory kont użytkowników do usługi Salesforce piaskownicy.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  W portalu usługi Salesforce na górnym pasku nawigacyjnym wybierz swoją nazwę, aby rozwinąć menu programu użytkownika:

    ![Moje ustawienia] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Moje ustawienia")

2.  Z menu użytkownika wybierz pozycję **Moje ustawienia** , aby otworzyć stronę **Moje ustawienia** .

3.  W okienku po lewej stronie kliknij pozycję **osobiste** , aby rozwinąć sekcję osobistych, a następnie kliknij **Resetuj moje tokenu zabezpieczeń**:

    ![Moje ustawienia] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Moje ustawienia")

4.  Na stronie **Resetowanie Moje tokenu zabezpieczeń** kliknij przycisk **Resetuj tokenu zabezpieczeń** na żądanie wiadomość e-mail zawierającą Salesforce.com token zabezpieczeń.

    ![Nowy Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Nowy Token")

5.  Zaznacz pole wyboru skrzynce odbiorczej poczty e-mail, wiadomości e-mail od Salesforce.com**potwierdzenia zabezpieczeń salesforce.com.com"**jako temat.

6.  Przejrzyj tę wiadomość e-mail, a następnie skopiuj wartość tokenu zabezpieczeń.

7.  W portalu klasyczny Azure, na stronie integracji aplikacji **usług salesforce piaskownicy** kliknij pozycję **Konfiguruj przypisywanie użytkowników** , aby otworzyć okno dialogowe **Konfigurowanie przypisywanie użytkowników** .

    ![Konfigurowanie użytkowników inicjowania obsługi administracyjnej.] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigurowanie użytkowników inicjowania obsługi administracyjnej.")

8.  Na stronie **Wprowadź poświadczenia piaskownicy usług Salesforce, aby włączyć automatyczne użytkownika inicjowania obsługi administracyjnej** wprowadź następujące ustawienia konfiguracji:

    ![Piaskownicy usług SalesForce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Piaskownicy usług SalesForce")

    .  W polu tekstowym **Nazwa użytkownika administratora piaskownicy usługi Salesforce** wpisz nazwę konta piaskownicy usługi Salesforce zawierającego profilu **Administrator systemu** w Salesforce.com przydzielone.

    b.  W polu tekstowym **Hasło administratora usługi Salesforce piaskownicy** wpisz hasło dla tego konta.

    c.  W polu tekstowym **Tokenu zabezpieczeń użytkownika** Wklej wartość tokenu zabezpieczeń.

    d.  Kliknij przycisk **Sprawdzanie poprawności** , aby Sprawdź konfigurację.

    e.  Kliknij przycisk **Dalej** , aby otworzyć stronę **potwierdzenia** .

9.  Na stronie **potwierdzenia** kliknij przycisk **Wykonano** , aby zapisać konfigurację.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Aby przypisać użytkowników do usługi Salesforce w trybie piaskownicy, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Usług Salesforce piaskownicy **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Tak")
  
Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu piaskownicy usługi Salesforce.
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](https://msdn.microsoft.com/library/dn308586).
