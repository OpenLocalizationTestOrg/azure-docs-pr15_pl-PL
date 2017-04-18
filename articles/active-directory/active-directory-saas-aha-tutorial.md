<properties 
    pageTitle="Samouczek: Integracja Azure Active Directory z Aha! | Microsoft Azure" 
    description="Dowiedz się, jak używać Aha! z usługi Azure Active Directory, aby włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Samouczek: Integracja Azure Active Directory z Aha!

Celem tego samouczka jest pokazanie integracji Azure i Aha!  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Tak! rejestracji jednokrotnej włączone subskrypcji

Ten samouczek użytkowników Azure AD zostały przypisane do Aha! będą mogli rejestracji jednokrotnej do aplikacji u swojego Aha! Witryna firmy (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Aha!
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-aha-tutorial/IC798944.png "Scenariusz")
##<a name="enabling-the-application-integration-for-aha"></a>Włączanie integracji aplikacji dla Aha!

Celem tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Aha!, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-aha-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-aha-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Aha!**.

    ![Galeria aplikacji] (./media/active-directory-saas-aha-tutorial/IC798945.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Aha!**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Tak!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Tak!")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Aha! przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure na **Aha!** Integracja aplikacji kliknij przycisk **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798946.png "Konfigurowanie logowania jednokrotnego")

2.  Na **jak chcesz użytkownikom logowanie do Aha!** strony, wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798947.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w **Aha! Zaloguj się na adres URL** pole tekstowe, wpisz adres URL używane przez użytkowników do logowania się w swojej Aha! Aplikacja (np.: "*https://company.aha.io/session/new*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-aha-tutorial/IC798948.png "Skonfigurować adres URL aplikacji")

4.  Na **Konfigurowanie logowania jednokrotnego w Aha!** Strona, aby pobrać plik metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798949.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do swojego Aha! Witryna firmy jako administrator.

6.  W menu u góry kliknij pozycję **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-aha-tutorial/IC798950.png "Ustawienia")

7.  Kliknij **konto**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Kliknij pozycję **Zabezpieczenia i logowania jednokrotnego**.

    ![Zabezpieczenia i logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798952.png "Zabezpieczenia i logowania jednokrotnego")

9.  W sekcji **Rejestracji jednokrotnej** jako **Dostawcy tożsamości**, zaznacz **SAML2.0**.

    ![Zabezpieczenia i logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798953.png "Zabezpieczenia i logowania jednokrotnego")

10. Na stronie Konfiguracja **Logowania jednokrotnego** wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-aha-tutorial/IC798954.png "Rejestracji jednokrotnej")

    1.  W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji.
    2.  **Konfigurowanie przy użyciu**wybierz **Plik metadanych**.
    3.  Aby przekazać metadanych pobrany plik, kliknij przycisk **Przeglądaj**.
    4.  Kliknij przycisk **Aktualizuj**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-aha-tutorial/IC798955.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Aha!, musi być przygotowana do Aha!.  
W przypadku Aha!, inicjowania obsługi administracyjnej jest zadanie automatyczne.  
Nie ma żadnych czynności do wykonania dla Ciebie.
  
Użytkownicy są tworzone automatycznie w razie potrzeby podczas pierwszej pojedynczego logowania jednokrotnego próby.

>[AZURE.NOTE] Możesz użyć innych Aha! narzędzia do tworzenia konta użytkownika lub interfejsów API dostarczony przez Aha! Aby zainicjować obsługę AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Aby przypisać użytkowników do Aha!, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na **Tak!** Integracja aplikacji kliknij przycisk **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-aha-tutorial/IC798956.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-aha-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
