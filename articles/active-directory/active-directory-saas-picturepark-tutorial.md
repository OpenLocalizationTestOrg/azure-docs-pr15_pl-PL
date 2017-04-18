<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Picturepark | Microsoft Azure" 
    description="Dowiedz się, jak użyć Picturepark z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Samouczek: Integracja usługi Azure Active Directory z Picturepark
  
Celem tego samouczka jest pokazanie integracji Azure i Picturepark.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Picturepark
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Picturepark będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Picturepark (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Picturepark
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenariusz")

##<a name="enabling-the-application-integration-for-picturepark"></a>Włączanie integracji aplikacji dla Picturepark
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Picturepark, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Picturepark**.

    ![Galeria aplikacji] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Picturepark**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Picturepark przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Picturepark wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Picturepark** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Picturepark** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Picturepark logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*http://company.picturepark.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Picturepark** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Picturepark jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Narzędzia administracyjne**, a następnie kliknij **Management Console**.

    ![Konsola zarządzania] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Konsola zarządzania")

7.  Kliknij **uwierzytelniania**, a następnie kliknij pozycję **dostawcy tożsamości**.

    ![Uwierzytelnianie] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Uwierzytelnianie")

8.  W sekcji **Konfiguracja dostawcy tożsamości** wykonaj następujące czynności:

    ![Konfiguracja dostawcy tożsamości] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Konfiguracja dostawcy tożsamości")

    1.  Kliknij przycisk **Dodaj**.
    2.  Wpisz nazwę dla danej konfiguracji.
    3.  Wybierz pozycję **Ustaw jako domyślne**.
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Picturepark** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Identyfikator URI wystawcy** .
    5.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Zaufane wystawcy miniatury** .  

        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    6.  Kliknij pozycję **JoinDefaultUsersGroup**.
    7.  Aby ustawić atrybut **Emailaddress** w polu tekstowym **roszczeń** , wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Konfiguracja] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfiguracja")
    8.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Picturepark, musi być przygotowana do Picturepark.  
W przypadku Picturepark inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Picturepark** .

2.  Na pasku narzędzi u góry kliknij pozycję **Narzędzia administracyjne**, a następnie kliknij **użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Użytkownicy")

3.  Na karcie **Przegląd użytkowników** kliknij przycisk **Nowy**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Zarządzanie użytkownikami")

4.  W oknie dialogowym **Utwórz użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Tworzenie użytkownika")

    1.  Typ: **Adres E-mail**, **hasło**, **Potwierdź hasło**, **Imię**, **nazwisko**, **Firma**, **kraju**, **ZIP**, **Miasto** prawidłowych Azure użytkowników usługi Active Directory, który ma celu należy do powiązanych pól tekstowych.
    2.  Wybierz **język**.
    3.  Kliknij przycisk **Utwórz**.

>[AZURE.NOTE]Można użyć dowolnego innego Picturepark użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Picturepark zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Aby przypisać użytkowników do Picturepark, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Picturepark **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).