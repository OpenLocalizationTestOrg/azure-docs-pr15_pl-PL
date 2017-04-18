<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z TimeOffManager | Microsoft Azure" 
    description="Dowiedz się, jak użyć TimeOffManager z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Samouczek: Azure Active Directory Integracja z TimeOffManager
  
Celem tego samouczka jest pokazanie integracji Azure i TimeOffManager.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   TimeOffManager logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do TimeOffManager będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy TimeOffManager (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla TimeOffManager
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Scenariusz")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>Włączanie integracji aplikacji dla TimeOffManager
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla TimeOffManager.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla TimeOffManager, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **TimeOffManager**.

    ![Galeria aplikacji] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Galeria aplikacji")

7.  W okienku wyników wybierz **TimeOffManager**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania TimeOffManager przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base-64 do Twojej dzierżawy TimeOffManager.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **TimeOffManager** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do TimeOffManager** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL odpowiedź TimeOffManager** wpisz adres URL AssertionConsumerService TimeOffManager (np.: "*przykład: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id = IC34216*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "Skonfigurować adres URL aplikacji")

    Adres URL odpowiedzi można przejść z logowaniu jednokrotnym TimeOffManager na stronie Ustawienia.

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Ustawienia rejestracji jednokrotnej")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w TimeOffManager** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy TimeOffManager jako administrator.

6.  Przejdź do pozycji **konta \> kont opcje \> pojedynczy ustawienia logowania jednokrotnego**.

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Ustawienia rejestracji jednokrotnej")

7.  W sekcji **Ustawienia rejestracji jednokrotnej** wykonaj następujące czynności:

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Ustawienia rejestracji jednokrotnej")

    .  Tworzenie pliku **Base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    b.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wklej cały certyfikat w **Certyfikat X.509** pole tekstowe.
    
    c.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TimeOffManager** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Protokołu Idp wystawcy** .
    
    d.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TimeOffManager** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL punktu końcowego protokołu IdP** .
    
    e.  Jako **Wymuszaj SAML**wybierz opcję **nie**.
    

    f.  Jako **Automatyczne tworzenie użytkowników**wybierz opcję **Tak**.
    
    g.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TimeOffManager** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    
    h.  Kliknij przycisk **Zapisz zmiany**.

8.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TimeOffManager** wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **wykonane**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Konfigurowanie logowania jednokrotnego")

9.  W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Atrybuty")

10. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![atrybuty tokenu SAML] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "atrybuty token SAML")

  	|Nazwa atrybutu|Wartość atrybutu|
  	|---|---|
  	|Adres e-mail|User.mail|
  	|Imię|User.givenName|
  	|Nazwisko|User.surname|

    .  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.

    b.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.

    c.  W polu tekstowym **Wartość atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.

    d.  Kliknij pozycję **pełne**.

11. Kliknij przycisk **Zastosuj zmiany**.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do TimeOffManager, musi być przygotowana do TimeOffManager.  
TimeOffManager obsługuje tylko na czas użytkownika inicjowania obsługi administracyjnej. Nie ma żadnych czynności do wykonania dla Ciebie.  
Użytkownicy są automatycznie dodawane podczas pierwszego logowania się za pomocą rejestracji jednokrotnej na.

>[AZURE.NOTE] Można użyć dowolnego innego TimeOffManager użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez TimeOffManager zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Aby przypisać użytkowników do TimeOffManager, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **TimeOffManager** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
