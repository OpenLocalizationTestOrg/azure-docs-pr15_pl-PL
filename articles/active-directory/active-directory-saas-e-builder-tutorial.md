<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z konstruktora e | Microsoft Azure" 
    description="Dowiedz się, jak włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych elementów za pomocą konstruktora e z usługą Azure Active Directory!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Samouczek: Azure Active Directory Integracja z konstruktora e
  
Celem tego samouczka jest pokazanie integracji Azure i e konstruktora.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Konstruktor e dzierżawy
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do e Konstruktor będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Konstruktor e (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla konstruktora e
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenariusz")
##<a name="enabling-the-application-integration-for-e-builder"></a>Włączanie integracji aplikacji dla konstruktora e
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla konstruktora e.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla konstruktora e, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **E konstruktora**.

    ![Galeria aplikacji] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Konstruktor e**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Konstruktor e] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "Konstruktor e")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do e konstruktora przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Konstruktor e** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do e konstruktora** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **e Konstruktor Zaloguj adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://\<nazwy dzierżawy\>.e Builder.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Skonfigurować adres URL aplikacji")

4.  Na **Konfigurowanie logowania jednokrotnego w Konstruktorze e** strony, aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie danych lokalnie jako **c:\\e-BuilderMetaData.xml**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Konfigurowanie logowania jednokrotnego")

5.  Przekazywanie tego pliku metadanych do e konstruktora zespołem pomocy technicznej. Konfiguruje potrzeb zespołu pomocy technicznej rejestracji jednokrotnej dla Ciebie.

6.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji użytkownika inicjowania obsługi administracyjnej do e konstruktora.  
Jeśli przypisany użytkownik próbuje zalogować się do e Konstruktor przy użyciu panelu programu access, e Konstruktor sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez konstruktora e.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Aby przypisać użytkowników do konstruktora e, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **E Konstruktor **kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
