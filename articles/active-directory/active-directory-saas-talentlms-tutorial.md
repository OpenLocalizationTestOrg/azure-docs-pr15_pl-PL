<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z TalentLMS | Microsoft Azure" 
    description="Dowiedz się, jak użyć TalentLMS z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Samouczek: Integracja usługi Azure Active Directory z TalentLMS
  
Celem tego samouczka jest pokazanie integracji Azure i TalentLMS.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy TalentLMS
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do TalentLMS będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy TalentLMS (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla TalentLMS
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Scenariusz")

##<a name="enabling-the-application-integration-for-talentlms"></a>Włączanie integracji aplikacji dla TalentLMS
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla TalentLMS.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla TalentLMS, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **TalentLMS**.

    ![Galeria aplikacji] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Galeria aplikacji")

7.  W okienku wyników wybierz **TalentLMS**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania TalentLMS przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML. .  
Konfigurowanie rejestracji jednokrotnej dla TalentLMS wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **TalentLMS** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do TalentLMS** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **TalentLMS adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. TalentLMSapp.com*", a następnie kliknij przycisk **Dalej**.

    ![Zaloguj się na adres URL] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Zaloguj się na adres URL")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w TalentLMS** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\TalentLMS.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy TalentLMS jako administrator.

6.  W sekcji **Ustawienia konta i** kliknij kartę **Użytkownicy** .

    ![Konta i ustawienia] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Konta i ustawienia")

7.  Kliknij pozycję **Logowania jednokrotnego (SSO)**,

8.  W sekcji rejestracji jednokrotnej wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Rejestracji jednokrotnej")

    1.  Na liście **Typ integracji logowania jednokrotnego** zaznacz **SAML 2.0**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TalentLMS** skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **dostawcy tożsamości (protokołu IdP)** .
    3.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TalentLMS** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **zdalnego logowania adresu URL** .
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TalentLMS** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **adres URL wyrejestrowywania zdalnej** .
    6.  W polu tekstowym **TargetedID** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  W polu tekstowym **Imię** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  W polu tekstowym **Nazwa** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  W polu tekstowym **wiadomości E-mail** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do TalentLMS, musi być przygotowana do TalentLMS.  
W przypadku TalentLMS inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **TalentLMS** .

2.  Kliknij pozycję **Użytkownicy**, a następnie kliknij pozycję **Dodaj użytkownika**.

3.  Na stronie okno dialogowe **Dodaj użytkownika** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Dodawanie użytkownika")

    1.  Wpisz pokrewne atrybuty Azure AD konta użytkownika w następujących polach tekstowych: **Imię**, **nazwisko**, **Adres E-mail**.
    2.  Kliknij pozycję **Dodaj użytkownika**.

>[AZURE.NOTE] Można użyć dowolnego innego TalentLMS użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez TalentLMS zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Aby przypisać użytkowników do TalentLMS, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **TalentLMS **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).