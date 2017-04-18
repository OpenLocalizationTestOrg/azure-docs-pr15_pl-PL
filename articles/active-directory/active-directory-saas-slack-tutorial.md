<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z zapas czasu | Microsoft Azure" 
    description="Dowiedz się, jak użyć zapas czasu przy użyciu usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Samouczek: Azure Active Directory Integracja z zapas czasu
  
Celem tego samouczka jest pokazanie integracji Azure i zapas czasu.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Zapas czasu logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do zapasu czasu będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy zapasu czasu (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla zapas czasu
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-slack-tutorial/IC794980.png "Scenariusz")

##<a name="enabling-the-application-integration-for-slack"></a>Włączanie integracji aplikacji dla zapas czasu
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla zapasu czasu.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla zapas czasu, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-slack-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-slack-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-slack-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Zapas czasu**.

    ![Galeria aplikacji] (./media/active-directory-saas-slack-tutorial/IC794981.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Zapas czasu**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Scenariusz] (./media/active-directory-saas-slack-tutorial/IC796925.png "Scenariusz")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML zapas czasu przy użyciu swojego konta.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji aplikacji **Zapas czasu** kliknij pozycję **Konfiguruj logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-slack-tutorial/IC794982.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak czy chcesz użytkownikom logowanie do zapas czasu** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-slack-tutorial/IC794983.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Zapas czasu logowania na adres URL** wpisz adres URL dzierżawy usługi zapasu czasu (np.: "*https://azuread.slack.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-slack-tutorial/IC794984.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w zapas czasu** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-slack-tutorial/IC794985.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy zapasu czasu jako administrator.

6.  Przejdź do pozycji **do firmy Microsoft Azure AD \> zespołu ustawienia**.

    ![Ustawienia zespołu] (./media/active-directory-saas-slack-tutorial/IC794986.png "Ustawienia zespołu")

7.  W sekcji **Ustawienia zespołu** kliknij kartę **Uwierzytelnianie** , a następnie kliknij pozycję **Zmień ustawienia**.

    ![Ustawienia zespołu] (./media/active-directory-saas-slack-tutorial/IC794987.png "Ustawienia zespołu")

8.  W oknie dialogowym **Ustawienia uwierzytelniania SAML** wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-slack-tutorial/IC794988.png "Ustawienia SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w zapas czasu** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **SAML 2.0 punktu końcowego HTTP ()** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w zapas czasu** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Wystawcy dostawcy tożsamości** .
    3.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.
    
        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Certyfikatu publicznego** .
    5.  Usuń zaznaczenie opcji **Zezwalaj użytkownikom na zmianę jego adres e-mail**.
    6.  Wybierz pozycję **Zezwalaj użytkownikom na wybieranie własną nazwę użytkownika**.
    7.  **Uwierzytelnianie dla zespołu mogą być używane przez**zaznacz **jest opcjonalne**.
    8.  Kliknij przycisk **Zapisz konfiguracji**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-slack-tutorial/IC794989.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do zapas czasu, musi być przygotowana do zapas czasu.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywanie użytkowników do zapasu czasu.  
Jeśli przypisany użytkownik próbuje zalogować się do zapas czasu, konto zapasu czasu są tworzone w razie potrzeby.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Aby przypisać użytkowników do zapas czasu, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Zapas czasu **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-slack-tutorial/IC794990.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-slack-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).