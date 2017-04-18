<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z pakietem adaptacyjne | Microsoft Azure"
    description="Dowiedz się, jak użyć adaptacyjne pakiet z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Samouczek: Azure Active Directory Integracja z pakietem adaptacyjne

Celem tego samouczka jest pokazanie integracji Azure i adaptacyjne pakiet.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Adaptacyjne pakietu dzierżawy

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do pakietu adaptacyjne będą mogli rejestracji jednokrotnej do aplikacji w pakiecie adaptacyjne firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji pakietu adaptacyjne
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenariusz")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Włączanie integracji aplikacji pakietu adaptacyjne

Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji pakietu adaptacyjne.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Aby włączyć integrację aplikacji pakietu adaptacyjne, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Adaptacyjne pakietu**.

    ![Galeria aplikacji] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Adaptacyjne pakietu**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Adaptacyjne pakietu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptacyjne pakietu")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia adaptacyjne pakietu przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie logowania jednokrotnego pakietu adaptacyjne wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Adaptacyjne pakiet** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie w pakiecie adaptacyjne** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** w polu tekstowym **Adres URL odpowiedź** , wpisz swój adres URL przy użyciu następującego wzorca "*https://login.adaptiveinsights.com:443-samlsso-RlJFRVRSSUFMMTI3MTE =*", a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Ta wartość możesz przejść ze strony **Ustawienia logowania jednokrotnego SAML** adaptacyjne pakietu.

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Konfigurowanie ustawień aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w pakiecie adaptacyjne** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy adaptacyjne pakietu jako administrator.

6.  Przejdź do pozycji **Administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

7.  W sekcji **Użytkownicy i role** kliknij pozycję **Zarządzaj ustawieniami rejestracji Jednokrotnej SAML**.

    ![Zarządzanie ustawieniami rejestracji Jednokrotnej SAML] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Zarządzanie ustawieniami rejestracji Jednokrotnej SAML")

8.  Na stronie **Ustawienia logowania jednokrotnego SAML** wykonaj następujące czynności:

    ![Ustawienia logowania jednokrotnego SAML] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "Ustawienia logowania jednokrotnego SAML")

    1.  W polu tekstowym **Nazwa dostawcy tożsamości** wpisz nazwę swojej konfiguracji.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pakiecie adaptacyjne** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go w polu tekstowym **dostawcy tożsamości identyfikator jednostki** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pakiecie adaptacyjne** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **adres URL logowania jednokrotnego inni dostawcy tożsamości** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w pakiecie adaptacyjne** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Niestandardowy adres URL Wyloguj** .
    5.  Aby przekazać pobrany certyfikatu, kliknij przycisk **Wybierz plik**.
    6.  Jako **identyfikator użytkownika SAML**wybierz **nazwę użytkownika adaptacyjne wniosków użytkownika**.
    7.  Jako **lokalizacji identyfikator użytkownika SAML**wybierz **identyfikator użytkownika w NameID tematu**.
    8.  **Format SAML NameID**wybierz **Adres E-mail**.
    9.  **Włączanie SAML**zaznaczyć **Zezwalaj logowania jednokrotnego SAML i bezpośredni logowania adaptacyjne wniosków**.
    10. Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do pakietu adaptacyjne, musi być przygotowana w pakiecie adaptacyjne.  
W przypadku pakietu adaptacyjne inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Adaptacyjne pakietu** jako administrator.

2.  Przejdź do pozycji **Administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

3.  W sekcji **Użytkownicy i role** kliknij pozycję **Dodaj użytkownika**.

    ![Dodawanie użytkownika] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Dodawanie użytkownika")

4.  W sekcji **Nowego użytkownika** wykonaj następujące czynności:

    ![Przesyłanie] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Przesyłanie")

    1.  Wpisz **nazwę**, **logowania**, **wiadomości E-mail**, **hasło** prawidłowych użytkownika usługi Azure Active Directory, którego chcesz należy do powiązanych pól tekstowych.
    2.  Wybierz **rolę**.
    3.  Kliknij przycisk **Prześlij**.

>[AZURE.NOTE] Można użyć dowolnego innego adaptacyjne pakietu użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez pakiet adaptacyjne zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Aby przypisać użytkowników do pakietu adaptacyjne, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Adaptacyjne pakiet **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
