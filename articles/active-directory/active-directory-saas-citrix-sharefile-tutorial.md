<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Citrix ShareFile | Microsoft Azure" 
    description="Dowiedz się, jak użyć Citrix ShareFile z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Samouczek: Integracja usługi Azure Active Directory z Citrix ShareFile

Celem tego samouczka jest pokazanie integracji Azure i Citrix ShareFile.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Citrix ShareFile

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Citrix ShareFile będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Citrix ShareFile (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Citrix ShareFile
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenariusz")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Włączanie integracji aplikacji dla Citrix ShareFile

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Citrix ShareFile.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Citrix ShareFile, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Citrix ShareFile**.

    ![Galeria aplikacji] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Citrix ShareFile**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Citrix ShareFile przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Citrix ShareFile** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do Citrix ShareFile** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Citrix ShareFile logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca `https://<tenant-name>.shareFile.com`, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Citrix ShareFile** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![ConfigureSingle logowania jednokrotnego] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **Citrix ShareFile** jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Administrator**.

7.  W okienku nawigacji po lewej stronie wybierz **Konfigurowanie logowania jednokrotnego**.

    ![Administrowanie kontem] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Administrowanie kontem")

8.  Na **logowania jednokrotnego i SAML 2.0 konfiguracji** okno dialogowe strony w obszarze **Ustawienia podstawowe**, wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Rejestracji jednokrotnej")

    1.  Kliknij opcję **Włącz SAML**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Citrix ShareFile** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go do **i wystawcy protokołu IDP-identyfikator jednostki** pole tekstowe.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Citrix ShareFile** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    4.  W portalu klasyczny Azure, strona **Konfigurowanie logowania jednokrotnego w Citrix ShareFile** okno dialogowe skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    5.  Kliknij pozycję **Zmień** obok pola **Certyfikat X.509** , a następnie przekaż certyfikat, który został pobrany z portalem klasyczny Azure AD.
        ![Ustawienia podstawowe] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Ustawienia podstawowe")

9.  W portalu zarządzania Citrix ShareFile, kliknij przycisk **Zapisz** .

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Citrix ShareFile, musi być przygotowana do Citrix ShareFile.  
W przypadku Citrix ShareFile inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Citrix ShareFile** .

2.  Kliknij pozycję **Zarządzanie użytkownikami \> Zarządzanie główne użytkowników \> + Tworzenie pracownika**.

    ![Tworzenie pracownika] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Tworzenie pracownika")

3.  Wprowadź **Adres E-mail**, **Imię** i **nazwisko** z prawidłową Azure AD konta możesz chcesz świadczenia.

    ![Podstawowe informacje] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Podstawowe informacje")

4.  Kliknij pozycję **Dodaj użytkownika**.

    >[AZURE.NOTE] Właściciel konta AAD otrzymasz wiadomość e-mail i skorzystać z łącza, aby potwierdzić swoje konto, zanim stanie się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego Citrix ShareFile użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Citrix ShareFile zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Aby przypisać użytkowników do Citrix ShareFile, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Citrix ShareFile **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
