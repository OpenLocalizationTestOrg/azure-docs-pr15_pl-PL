<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Innotas | Microsoft Azure"
    description="Dowiedz się, jak użyć Innotas z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-innotas"></a>Samouczek: Azure Active Directory Integracja z Innotas
  
Celem tego samouczka jest pokazanie integracji Azure i Innotas.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Innotas
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Innotas będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Innotas (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Innotas
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-innotas-tutorial/IC777331.png "Scenariusz")
##<a name="enabling-the-application-integration-for-innotas"></a>Włączanie integracji aplikacji dla Innotas
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Innotas.

###<a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Innotas, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-innotas-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-innotas-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-innotas-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-innotas-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Innotas**.

    ![Galeria aplikacji] (./media/active-directory-saas-innotas-tutorial/IC777332.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Innotas**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Innotas] (./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Innotas przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Innotas** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-innotas-tutorial/IC777334.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Innotas** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-innotas-tutorial/IC777335.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Innotas adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Innotas.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-innotas-tutorial/IC777336.png "Skonfigurować adres URL aplikacji")

4.  Na **Konfigurowanie logowania jednokrotnego w Innotas** strony, aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie danych lokalnie jako **c:\\InnotasMetaData.xml**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-innotas-tutorial/IC777337.png "Konfigurowanie logowania jednokrotnego")

5.  Przekazywanie tego pliku metadanych do Innotas zespołem pomocy technicznej. Konfiguruje potrzeb zespołu pomocy technicznej rejestracji jednokrotnej dla Ciebie.

6.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-innotas-tutorial/IC777338.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do Innotas użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do Innotas przy użyciu panelu programu access, Innotas sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez Innotas.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-innotas-perform-the-following-steps"></a>Aby przypisać użytkowników do Innotas, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Innotas **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-innotas-tutorial/IC777339.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-innotas-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).