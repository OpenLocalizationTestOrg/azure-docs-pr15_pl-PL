<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Learningpool | Microsoft Azure" 
    description="Dowiedz się, jak użyć Learningpool z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Samouczek: Azure Active Directory Integracja z Learningpool
  
Celem tego samouczka jest pokazanie integracja Azure i Learningpool.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Learningpool logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Learningpool będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Learningpool (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Learningpool
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scenariusz")
##<a name="enabling-the-application-integration-for-learningpool"></a>Włączanie integracji aplikacji dla Learningpool
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Learningpool.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Learningpool, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Learningpool**.

    ![Galeria aplikacji] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Learningpool**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Learningpool przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.
  
Aplikacja Learningpool oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty tokenu SAML] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "Atrybuty tokenu SAML")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Learningpool** aplikacji w menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Atrybuty")

2.  Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ###  

  	|Nazwa atrybutu                |Wartość atrybutu            |
  	|------------------------------|---------------------------|

     Nazwa urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     Nazwa urn: oid:2.5.4.42|User.givenName   
  	|Nazwa urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|Nazwa urn: oid:2.5.4.4|User.surname

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    3.  Na liście **Wartości atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.
    4.  Kliknij pozycję **pełne**.

3.  Kliknij przycisk **Zastosuj zmiany**.

4.  W przeglądarce kliknij przycisk **Wstecz** ponownie otworzyć okno dialogowe **Szybkie uruchamianie** .

5.  Kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie Singel logowania jednokrotnego] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Konfigurowanie Singel logowania jednokrotnego")

6.  Na stronie **jak chcesz użytkownikom logowanie do Learningpool** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Konfigurowanie logowania jednokrotnego")

7.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Learningpool logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Learningpool (przykład: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Skonfigurować adres URL aplikacji")

8.  Na stronie **Konfigurowanie logowania jednokrotnego w Learningpool** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Konfigurowanie logowania jednokrotnego")

9.  Przesyłanie dalej tego pliku metadanych do zespołu pomocy technicznej Learningpool.

    >[AZURE.NOTE]Logowania jednokrotnego musi być włączony przez zespół pomocy technicznej Learningpool.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Learningpool, musi być przygotowana do Learningpool.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do Learningpool użytkowników.  
Użytkownicy muszą zostać utworzone przez zespół pomocy technicznej Learningpool.

>[AZURE.NOTE]Można użyć dowolnego innego Learningpool użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Learningpool zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Aby przypisać użytkowników do Learningpool, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Learningpool **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).