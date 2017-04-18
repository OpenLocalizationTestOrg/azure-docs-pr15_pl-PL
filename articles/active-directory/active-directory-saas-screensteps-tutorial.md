<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z ScreenSteps | Microsoft Azure" 
    description="Dowiedz się, jak użyć ScreenSteps z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Samouczek: Azure Active Directory Integracja z ScreenSteps
  
Celem tego samouczka jest pokazanie integracji Azure i ScreenSteps.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy ScreenSteps
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do ScreenSteps będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy ScreenSteps (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla ScreenSteps
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenariusz")
##<a name="enabling-the-application-integration-for-screensteps"></a>Włączanie integracji aplikacji dla ScreenSteps
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla ScreenSteps.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla ScreenSteps, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **ScreenSteps**.

    ![Galeria aplikacji] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Galeria aplikacji")

7.  W okienku wyników wybierz **ScreenSteps**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania ScreenSteps przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **ScreenSteps** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do ScreenSteps** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **ScreenSteps adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. ScreenSteps.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w ScreenSteps** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy ScreenSteps jako administrator.

6.  Kliknij pozycję **Zarządzanie kontami**.

    ![Zarządzanie kontami] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Zarządzanie kontami")

7.  Kliknij pozycję **zdalnego uwierzytelniania**.

    ![Zdalne uwierzytelnianie] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Zdalne uwierzytelnianie")

8.  Kliknij przycisk **Utwórz uwierzytelniania końcowego**.

    ![Zdalne uwierzytelnianie] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Zdalne uwierzytelnianie")

9.  W sekcji **Tworzenie punktu końcowego uwierzytelniania** należy wykonać następujące czynności:

    ![Tworzenie punktu końcowego uwierzytelniania] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Tworzenie punktu końcowego uwierzytelniania")

    1.  W polu tekstowym **Tytuł** wpisz tytuł.
    2.  Na liście **Tryb** zaznacz **SAML**.
    3.  Kliknij przycisk **Utwórz**.

10. W sekcji **Zdalny uwierzytelniania punkt końcowy** należy wykonać następujące czynności:

    ![Punkt końcowy zdalnego uwierzytelniania] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Punkt końcowy zdalnego uwierzytelniania")

    1.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w ScreenSteps** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL zdalnego logowania** .
    2.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w ScreenSteps** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **wylogować adresu URL** .
    3.  Kliknij przycisk **Wybierz plik**, a następnie przekaż pobranego certyfikatu.
    4.  Kliknij przycisk **Aktualizuj**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do **ScreenSteps**, musi być przygotowana do **ScreenSteps**.  
W przypadku **ScreenSteps**inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Aby obsługi administracyjnej konta użytkownika w celu ScreenSteps, należy wykonać następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **ScreenSteps** .

2.  Kliknij pozycję **Zarządzanie kontami**.

    ![Zarządzanie kontami] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Zarządzanie kontami")

3.  Kliknij pozycję **Użytkownicy**.

    ![Użytkownicy] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Użytkownicy")

4.  Kliknij przycisk **Utwórz użytkownika**.

    ![Wszyscy użytkownicy] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Wszyscy użytkownicy")

5.  Na liście **Roli użytkownika** wybierz rolę dla użytkownika.

6.  W sekcji roli użytkownika wpisz**"Imię**, **nazwisko**, **wiadomości E-mail**, **logowania**, **hasło** i **Potwierdzenie hasła"**prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.

    ![Nowy użytkownik] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Nowy użytkownik")

7.  W sekcji grupy wybierz **Grupę uwierzytelniania (SAML)**, a następnie kliknij **Utwórz użytkownika**.

    ![Grupy] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Grupy")

>[AZURE.NOTE]Można użyć dowolnego innego ScreenSteps użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez ScreenSteps zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Aby przypisać użytkowników do ScreenSteps, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **ScreenSteps **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Przypisywanie użytkowników")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).