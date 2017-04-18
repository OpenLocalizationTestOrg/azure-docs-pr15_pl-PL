<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z SimpleNexus | Microsoft Azure" 
    description="Dowiedz się, jak użyć SimpleNexus z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Samouczek: Azure Active Directory Integracja z SimpleNexus
  
Celem tego samouczka jest pokazanie integracji Azure i SimpleNexus.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   SimpleNexus logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do SimpleNexus będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy SimpleNexus (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla SimpleNexus
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenariusz")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Włączanie integracji aplikacji dla SimpleNexus
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla SimpleNexus.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla SimpleNexus, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **węzła prostego**.

    ![Galeria aplikacji] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Galeria aplikacji")

7.  W okienku wyników wybierz **SimpleNexus**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Prosta węzła] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Prosta węzła")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania SimpleNexus przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SimpleNexus** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do SimpleNexus** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **SimpleNexus adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://simplenexus.com/CompanyName\_logowania*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w SimpleNexus** kliknij przycisk **Pobierz metadanych**, a następnie ustawisz przesyłanie pliku metadanych do zespołu pomocy technicznej SimpleNexus.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Konfigurowanie logowania jednokrotnego")

    >[AZURE.NOTE] Rejestracji jednokrotnej musi być włączona przez zespół pomocy technicznej SimpleNexus.

5.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do SimpleNexus, musi być przygotowana do SimpleNexus.  
W przypadku SimpleNexus inicjowania obsługi administracyjnej jest ręczne zadania wykonywane przez administratora dzierżawy.

>[AZURE.NOTE] Można użyć dowolnego innego SimpleNexus użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez SimpleNexus zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Aby przypisać użytkowników do SimpleNexus, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **SimpleNexus **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).