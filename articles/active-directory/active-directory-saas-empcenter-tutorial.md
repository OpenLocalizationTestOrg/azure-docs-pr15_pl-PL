<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z EmpCenter | Microsoft Azure" 
    description="Dowiedz się, jak użyć EmpCenter z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Samouczek: Azure Active Directory Integracja z EmpCenter
  
Celem tego samouczka jest pokazanie integracji Azure i EmpCenter.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   EmpCenter logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do EmpCenter będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy EmpCenter (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla EmpCenter
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenariusz")
##<a name="enabling-the-application-integration-for-empcenter"></a>Włączanie integracji aplikacji dla EmpCenter
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla EmpCenter.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla EmpCenter, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **EmpCenter**.

    ![Galeria aplikacji] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Galeria aplikacji")

7.  W okienku wyników wybierz **EmpCenter**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania EmpCenter przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **EmpCenter** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do EmpCenter** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Konfigurowanie ustawień aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji EmpCenter (przykład: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w EmpCenter** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku metadanych pobranych do zespołu pomocy technicznej EmpCenter.

    >[AZURE.NOTE] Zespół pomocy technicznej EmpCenter ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do EmpCenter, musi być przygotowana do EmpCenter.  
W przypadku EmpCenter kont użytkowników muszą zostać utworzone przez zespół pomocy technicznej EmpCenter.

>[AZURE.NOTE] Można użyć dowolnego innego EmpCenter użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez EmpCenter do świadczenia usługi Azure Active Directory kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Aby przypisać użytkowników do EmpCenter, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **EmpCenter **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).