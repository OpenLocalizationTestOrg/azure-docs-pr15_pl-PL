<properties 
    pageTitle="Samouczek: Azure Active Directory integracji z cyklem SCC | Microsoft Azure" 
    description="Dowiedz się, jak użyć cyklu życia SCC z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Samouczek: Azure Active Directory integracji z cyklem SCC
  
Celem tego samouczka jest pokazanie integracja Azure i SCC cyklu życia.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Cykl życia SCC rejestracji jednokrotnej włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do świadczenia SCC będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny cyklu życia SCC (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla cyklu życia SCC
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenariusz")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Włączanie integracji aplikacji dla cyklu życia SCC
  
Celem w tej sekcji jest w konspekcie, jak włączyć integrację aplikacji dla SCC cyklu życia.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla cyklu życia SCC, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **SCC cyklu życia**.

    ![Galeria aplikacji] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Galeria aplikacji")

7.  W okienku wyników wybierz **SCC cyklu**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Cykl życia SCC] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "Cykl życia SCC")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML cyklu życia SCC przy użyciu swojego konta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SCC cyklu życia** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do świadczenia SCC** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji cyklu życia SCC przy użyciu następującego wzorca "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w cyklu życia SCC** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfigurowanie logowania jednokrotnego")

5.  Przesyłanie dalej tego pliku metadanych do zespołu pomocy technicznej cyklu życia SCC.

    >[AZURE.NOTE]Rejestracji jednokrotnej musi być aktywne przez zespół pomocy technicznej SCC cyklu życia.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
W celu umożliwienia użytkownikom Azure AD zalogować się do cyklu życia SCC, musi być przygotowana do SCC cyklu życia.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywanie użytkowników do SCC cyklu życia.  
Jeśli przypisany użytkownik próbuje zalogować się do cyklu życia SCC, konto cyklu życia SCC są tworzone w razie potrzeby.

>[AZURE.NOTE]Można użyć dowolnego innego cyklu życia SCC użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez cyklu życia SCC zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Aby przypisać użytkowników do świadczenia SCC, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **SCC cyklu **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).