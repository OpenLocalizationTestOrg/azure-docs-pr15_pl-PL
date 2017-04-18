<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Pagerduty | Microsoft Azure" 
    description="Dowiedz się, jak użyć Pagerduty z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Samouczek: Integracja usługi Azure Active Directory z Pagerduty
  
Celem tego samouczka jest pokazanie integracji Azure i Pagerduty.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Pagerduty
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Pagerduty będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Pagerduty (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Pagerduty
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenariusz")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Włączanie integracji aplikacji dla Pagerduty
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Pagerduty.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Pagerduty, wykonaj następujące czynności:

1.  W portalu zarządzania Azure w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Pagerduty**.

    ![Galeria aplikacji] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Pagerduty**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Pagerduty przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Pagerduty** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Pagerduty** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Pagerduty adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Pagerduty.com*", a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie aplikacji url] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Konfigurowanie aplikacji url")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Pagerduty** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Pagerduty jako administrator.

6.  W menu u góry kliknij pozycję **Ustawienia kont**.

    ![Ustawienia kont] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Ustawienia kont")

7.  Kliknij pozycję **rejestracji jednokrotnej**.

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Rejestracji jednokrotnej")

8.  Na stronie włączyć logowania jednokrotnego (SSO) wykonaj następujące czynności:

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Włączanie rejestracji jednokrotnej")

    1.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wklej go do pola tekstowego **Certyfikat X.509**
    3.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w Pagerduty** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    4.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w Pagerduty** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    5.  Wybierz pozycję **Włącz rejestracji jednokrotnej**.
    6.  Kliknij przycisk **Zapisz zmiany**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Pagerduty, musi być przygotowana do Pagerduty.  
W przypadku Pagerduty inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Pagerduty** .

2.  W menu u góry kliknij pozycję **Użytkownicy**.

3.  Kliknij przycisk **Dodaj użytkowników**.

    ![Dodawanie użytkowników] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Dodawanie użytkowników")

4.  W oknie dialogowym **Zapraszanie zespołu** wpisz **Imię i nazwisko** i adres **E-mail** użytkownika Azure AD, które mają obsługi administracyjnej, kliknij przycisk **Dodaj,**a następnie kliknij przycisk **Wyślij stanowi zaproszenie**.

    ![Zapraszanie zespołu] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Zapraszanie zespołu")

    >[AZURE.NOTE] Wszystkie dodane użytkownicy będą otrzymywać Zaproś, aby utworzyć konto PagerDuty.

>[AZURE.NOTE] Można użyć dowolnego innego Pagerduty użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Pagerduty zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Aby przypisać użytkowników do Pagerduty, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Pagerduty **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).