<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Brightspace przez Desire2Learn | Microsoft Azure" 
    description="Dowiedz się, jak użyć Brightspace przez Desire2Learn z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Samouczek: Azure Active Directory Integracja z Brightspace przez Desire2Learn

Celem tego samouczka jest pokazanie integracji Azure i Brightspace przez Desire2Learn.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Brightspace przez Desire2Learn logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Brightspace przez Desire2Learn będą mogli rejestracji jednokrotnej do aplikacji u swojego Brightspace przez Desire2Learn firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Brightspace przez Desire2Learn
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenariusz")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Włączanie integracji aplikacji dla Brightspace przez Desire2Learn

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Brightspace przez Desire2Learn.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Brightspace przez Desire2Learn, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Brightspace przez Desire2Learn**.

    ![Galeria Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Galeria Apllication")

7.  W okienku wyników wybierz **Brightspace przez Desire2Learn**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Brightspace przez Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace przez Desire2Learn")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Brightspace przez Desire2Learn przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Brightspace przez Desire2Learn** integracja aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Brightspace przez Desire2Learn** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do swojego **Brightspace przez Desire2Learn** (przykład: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Brightspace przez Desire2Learn** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie Zapisz metadane na Twoim komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku metadanych pobranych do usługi Brightspace przez zespół pomocy technicznej Desire2Learn.

    >[AZURE.NOTE] Usługi Brightspace przez zespół pomocy technicznej Desire2Learn ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Brightspace przez Desire2Learn, ich musi być przygotowana do Brightspace przez Desire2Learn.  
W przypadku Brightspace przez Desire2Learn kont użytkowników muszą zostać utworzone przez użytkownika Brightspace przez zespół pomocy technicznej Desire2Learn.

>[AZURE.NOTE] Można użyć innych Brightspace Desire2Learn narzędzia do tworzenia konta użytkownika lub interfejsów API udostępniony przez Brightspace przez Desire2Learn do świadczenia usługi Azure Active Directory kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Aby przypisać użytkowników do Brightspace przez Desire2Learn, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Brightspace przez Desire2Learn **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
