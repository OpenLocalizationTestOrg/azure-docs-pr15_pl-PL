<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z książek Overdrive | Microsoft Azure" 
    description="Dowiedz się, jak użyć książki Overdrive z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Samouczek: Azure Active Directory Integracja z Overdrive książek
  
Celem tego samouczka jest pokazanie integracji Azure i OverDrive.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   OverDrive rejestracji jednokrotnej włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do OverDrive będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny OverDrive (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla OverDrive
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scenariusz")
##<a name="enabling-the-application-integration-for-overdrive"></a>Włączanie integracji aplikacji dla OverDrive
  
Celem w tej sekcji jest w konspekcie, jak włączyć integrację aplikacji dla OverDrive.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla OverDrive, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **OverDrive**.

    ![Galeria aplikacji] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Galeria aplikacji")

7.  W okienku wyników wybierz **OverDrive**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML OverDrive przy użyciu swojego konta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **OverDrive** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak czy chcesz użytkownikom logowanie do OverDrive** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **OverDrive adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*http://mslibrarytest.libraryreserve.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Skonfigurować adres URL aplikacji")

4.  Strona **Konfigurowanie logowania jednokrotnego w OverDrive** , aby pobrać plik metadanych, a następnie wyślij go do zespołu pomocy technicznej OverDrive.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Konfigurowanie logowania jednokrotnego")

    >[AZURE.NOTE]Zespół pomocy technicznej OverDrive konfiguruje rejestracji jednokrotnej dla Ciebie i wysyła powiadomienie po zakończeniu konfiguracji.

5.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywanie użytkowników do OverDrive.  
Jeśli przypisany użytkownik próbuje zalogować się do OverDrive, konto OverDrive są tworzone w razie potrzeby.

>[AZURE.NOTE]Można użyć dowolnego innego OverDrive użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez OverDrive zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Aby przypisać użytkowników do OverDrive, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **OverDrive **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).