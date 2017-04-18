<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z RightAnswers | Microsoft Azure" 
    description="Dowiedz się, jak użyć RightAnswers z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Samouczek: Azure Active Directory Integracja z RightAnswers
  
Celem tego samouczka jest pokazanie integracji Azure i RightAnswers. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   RightAnswers logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do RightAnswers będą mogli pojedynczej Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla RightAnswers
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scenariusz")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Włączanie integracji aplikacji dla RightAnswers
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla RightAnswers.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla RightAnswers, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **RightAnswers**.

    ![Galeria aplikacji] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Galeria aplikacji")

7.  W okienku wyników wybierz **RightAnswers**, a następnie kliknij **Wykonano** w celu dodania aplikacji.
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania RightAnswers przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **RightAnswers** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do RightAnswers** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji RightAnswers (przykład: *https://fortify.rightanswers.com/portal/ss/*), a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Konfigurowanie ustawień aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w RightAnswers** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku metadanych pobrane z zespołem pomocy technicznej RightAnswers.

    >[AZURE.NOTE] Zespół pomocy technicznej RightAnswers ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do RightAnswers, musi być przygotowana do RightAnswers.  
W przypadku RightAnswers inicjowania obsługi administracyjnej to zadanie automatyczne.  
Nie ma żadnych czynności do wykonania dla Ciebie.
  
Użytkownicy są tworzone automatycznie w razie potrzeby podczas pierwszej pojedynczego logowania jednokrotnego próby.

>[AZURE.NOTE]Można użyć dowolnego innego RightAnswers użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez RightAnswers zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Aby przypisać użytkowników do RightAnswers, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **RightAnswers **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).