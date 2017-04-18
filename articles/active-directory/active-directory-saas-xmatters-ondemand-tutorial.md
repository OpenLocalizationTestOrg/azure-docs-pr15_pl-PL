<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z xMatters na żądanie | Microsoft Azure"
    description="Dowiedz się, jak użyć xMatters na żądanie z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Samouczek: Azure Active Directory Integracja z xMatters na żądanie
  
Celem tego samouczka jest pokazanie integracji Azure i xMatters na żądanie. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   XMatters dzierżawy na żądanie
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do xMatters na żądanie będą mogli pojedynczy Zaloguj się do aplikacji w witrynie firmy na żądanie xMatters (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla xMatters na żądanie
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenariusz")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Włączanie integracji aplikacji dla xMatters na żądanie
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla xMatters na żądanie.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla xMatters na żądanie, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **xMatters na żądanie**.

    ![Galeria aplikacji] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galeria aplikacji")

7.  W okienku wyników wybierz **XMatters na żądanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![xMatters na żądanie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters na żądanie")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania na żądanie XMatters przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **XMatters na żądanie** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do XMatters na żądanie** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Skonfigurować adres URL aplikacji")

    . W polu tekstowym **XMatters na żądanie logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Kliknij przycisk **Dalej**.


4.  Na stronie **Konfigurowanie logowania jednokrotnego w XMatters na żądanie** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Należy przesłać dalej certyfikat do zespołu pomocy technicznej xMatters. Certyfikat musi przekazane przez zespół pomocy technicznej xMatters, zanim można zakończyć konfigurację logowania jednej.

    ![Konfigurowanie pojedynczego logowanie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Konfigurowanie pojedynczego logowanie")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy XMatters na żądanie jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Administrator**, a następnie kliknij **Szczegóły firmy** na pasku nawigacyjnym po lewej stronie.

    ![Administrator] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Administrator")

7.  Na stronie **Konfiguracja SAML** wykonaj następujące czynności:

    ![Konfiguracja SAML] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "Konfiguracja SAML")

    1.  Zaznacz opcję **Włącz SAML**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w XMatters na żądanie** skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **Identyfikator dostawcy tożsamości** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w XMatters na żądanie** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Pojedynczy znak na adres URL** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w XMatters na żądanie** skopiuj wartość **Adres URL usługi pojedynczej Sign-Out** , a następnie wklej go w polu tekstowym **Pojedynczej Wyloguj adresu URL** .
    5.  Na stronie Szczegóły firmy u góry kliknij przycisk **Zapisz zmiany**.
        ![Szczegóły firmy] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Szczegóły firmy")

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie pojedynczego logowanie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Konfigurowanie pojedynczego logowanie")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do XMatters na żądanie, musi być przygotowana do XMatters na żądanie.  
W przypadku XMatters na żądanie inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **XMatters na żądanie** .

2.  Kliknij kartę **Użytkownicy** .

3.  Kliknij pozycję **Dodaj użytkownika**.

    ![Użytkownicy] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Użytkownicy")

4.  Zaznaczanie **aktywnej**.

5.  W sekcji **Dodawanie użytkownika** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Dodawanie użytkownika")

    1.  Wprowadź **nazwę użytkownika**, **Imię**, **nazwisko**, **witryny** działające konto AAD potrzebne do świadczenia.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego XMatters na żądanie użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez XMatters na żądanie zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Aby przypisać użytkowników do XMatters na żądanie, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Na żądanie XMatters **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).