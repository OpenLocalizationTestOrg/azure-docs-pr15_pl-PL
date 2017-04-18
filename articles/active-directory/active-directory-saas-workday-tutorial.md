<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Workday | Microsoft Azure" 
    description="Dowiedz się, jak użyć pracy z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Samouczek: Azure Active Directory Integracja z pracy
  
Celem tego samouczka jest pokazanie integracji Azure i pracy. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy w pracy
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji do pracy
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

![Scenariusz] (./media/active-directory-saas-workday-tutorial/IC782919.png "Scenariusz")

##<a name="enabling-the-application-integration-for-workday"></a>Włączanie integracji aplikacji do pracy
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla usługi Salesforce.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla dnia roboczego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-workday-tutorial/IC700994.png "Aplikacje")

4.  Aby otworzyć **Galerię aplikacji**, kliknij pozycję **Dodaj aplikację**, a następnie kliknij **Dodawanie aplikacji dla swojej organizacji użyć**.

    ![Co chcesz zrobić?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Co chcesz zrobić?")

5.  W **polu wyszukiwania**wpisz **pracy**.

    ![Dzień roboczy] (./media/active-directory-saas-workday-tutorial/IC701021.png "Dzień roboczy")

6.  W okienku wyników wybierz **pracy**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Dzień roboczy] (./media/active-directory-saas-workday-tutorial/IC701022.png "Dzień roboczy")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do pracy przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane, aby utworzyć certyfikat zakodowany base-64.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Na stronie integracji **Workday** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC782920.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do pracy** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC782921.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-workday-tutorial/IC782957.png "Skonfigurować adres URL aplikacji")

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkownika do zalogowania się do pracy przy użyciu następującego wzorca:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  W polu tekstowym **Adres URL odpowiedź Workday** wpisz adres URL odpowiedź Workday, przy użyciu następującego wzorca:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Adres URL odpowiedź musi mieć poddomenę (np.: "www", wd2, wd3, wd3 impl, wd5, wd5 impl). 
    >Za pomocą podobną do "*http://www.myworkday.com*" działa, ale nie "*http://myworkday.com*". 
 
4.  Na stronie **Konfigurowanie logowania jednokrotnego w pracy** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC782922.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Workday jako administrator.

6.  Przejdź do pozycji **Menu \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Przejdź do **Administrowanie kontem**.

    ![Administrowanie kontem] (./media/active-directory-saas-workday-tutorial/IC782924.png "Administrowanie kontem")

8.  Przejdź do pozycji **Edytuj ustawienia dzierżawy — zabezpieczeń**.

    ![Edytowanie zabezpieczeń dzierżawy] (./media/active-directory-saas-workday-tutorial/IC782925.png "Edytowanie zabezpieczeń dzierżawy")

9.  W sekcji **Przekierowywania adresów URL** wykonaj następujące czynności:

    ![Przekierowywanie adresów URL] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Przekierowywanie adresów URL")

    . Kliknij przycisk **Dodaj wiersz**.

    b. W polu tekstowym **Adres URL przekierowania logowania** i w polu tekstowym **Adres URL przekierowania Mobile** wpisz adres **URL dzierżawy Workday** zostały wprowadzone na stronie **Konfigurowanie adresu URL aplikacji** portalu klasyczny Azure.
    
    c. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pracy** Skopiuj **Adres URL usługi Sign-Out pojedynczej**, a następnie wklej go w polu tekstowym **Adres URL przekierowania Wyloguj** .

    d.  W **środowisku** pole tekstowe wpisz nazwę środowiska.  


    >[AZURE.NOTE] Wartość atrybutu środowiska jest powiązane wartość adres URL dzierżawy:
    >
    >-   Jeśli nazwa domeny dzierżawy Workday adres URL zaczyna się od impl (np.: *https://impl.workday.com/\<dzierżawy\>/login-saml2.htmld*), musi mieć wartość atrybutu **środowiska** wykonania.
    >-   Jeśli nazwa domeny zaczyna się czegoś innego, należy skontaktować się Workday pobrać pasującą wartość **środowiska** .

10. W sekcji **Ustawienia SAML** wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-workday-tutorial/IC782926.png "Ustawienia SAML")

    .  Zaznacz opcję **Włącz uwierzytelnianie SAML**.

    b.  Kliknij przycisk **Dodaj wiersz**.

11. W sekcji dostawcy tożsamości SAML wykonaj następujące czynności:

    ![Dostawcy tożsamości SAML] (./media/active-directory-saas-workday-tutorial/IC7829271.png "Dostawcy tożsamości SAML")

    . W polu tekstowym Nazwa dostawcy tożsamości, wpisz nazwę dostawcy (przykład: *SPInitiatedSSO*).

    b. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pracy** skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **wystawcy** .

    c. Zaznacz opcję **Włącz Workday Wyloguj Initialted**.

    d. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pracy** skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj żądanie** .


    e. Kliknij **Certyfikat klucza publicznego dostawcy tożsamości**, a następnie kliknij przycisk **Utwórz**. 

    ![Tworzenie] (./media/active-directory-saas-workday-tutorial/IC782928.png "Tworzenie")

    f. Kliknij pozycję **Tworzenie x509 klucz publiczny**. 
        
    ![Tworzenie] (./media/active-directory-saas-workday-tutorial/IC782929.png "Tworzenie")


1. W sekcji **Widok x509 klucz publiczny** wykonaj następujące czynności: 

    ![Klucz publiczny widoku x509] (./media/active-directory-saas-workday-tutorial/IC782930.png "Klucz publiczny widoku x509") 

    . W polu tekstowym **Nazwa** wpisz nazwę certyfikatu (np.: *ŚOI\_SP*).
        
    b. W polu tekstowym **Ważny od** wpisz wartość atrybutu certyfikatu obowiązywania.
    
    c.  W polu tekstowym **Ważny do** wpisz prawidłową wartość atrybutu certyfikatu.
        
    >[AZURE.NOTE] Możesz uzyskać prawidłową od daty i prawidłowe daty z pobranego certyfikatu, klikając je dwukrotnie. Daty są wyświetlane w obszarze kartę **Szczegóły** .

    d. Tworzenie pliku **Base 64 zakodowany** z pobranego certyfikatu.  

    >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    e.  Otwórz base-64 certyfikatu zakodowany w Notatniku, a następnie skopiuj zawartość go.
    
    f.  W polu tekstowym **certyfikatu** powoduje wklejenie zawartości Schowka.
    
    g.  Kliknij **przycisk OK**.

12.  Wykonaj następujące czynności: 

    ![Konfiguracja logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "Konfiguracja logowania jednokrotnego")

    .  Włączanie **x509 para klucz prywatny**.

    b.  W polu tekstowym **Identyfikator dostawcy usługi** wpisz **http://www.workday.com**.

    c.  Wybierz pozycję **Włącz SP zainicjowane SAML uwierzytelniania**.

    d.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pracy** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Adres URL usługi rejestracji Jednokrotnej protokołu IdP** .
     
    e. Wybierz pozycję **nie korygowania żądania uwierzytelnienia zainicjowane SP**.

    f. Jako **Podpisu żądanie uwierzytelnienia**wybierz pozycję **SHA256**. 
        
    ![Metoda podpisu żądania uwierzytelniania] (./media/active-directory-saas-workday-tutorial/IC782932.png "Metoda podpisu żądania uwierzytelniania") 
 
    g. Kliknij **przycisk OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w pracy** kliknij przycisk **Dalej**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC782934.png "Konfigurowanie logowania jednokrotnego")

13. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Konfigurowanie logowania jednokrotnego")



##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby uzyskać użytkownika testowego obsługi administracyjnej w pracy, musisz skontaktuj się z zespołem pomocy technicznej pracy.  
Zespół pomocy technicznej Workday utworzy użytkownika dla Ciebie.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Aby przypisać użytkowników do pracy, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Workday **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-workday-tutorial/IC782935.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-workday-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).