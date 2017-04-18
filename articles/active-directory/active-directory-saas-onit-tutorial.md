<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Onit | Microsoft Azure" 
    description="Dowiedz się, jak użyć Onit z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Samouczek: Azure Active Directory Integracja z Onit
  
Celem tego samouczka jest pokazanie integracji Azure i Onit.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Onit logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Onit będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Onit (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Onit
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-onit-tutorial/IC791166.png "Scenariusz")
##<a name="enabling-the-application-integration-for-onit"></a>Włączanie integracji aplikacji dla Onit
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Onit.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Onit, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-onit-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-onit-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Onit**.

    ![Galeria aplikacji] (./media/active-directory-saas-onit-tutorial/IC791167.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Onit**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Onit przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Onit wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).
  
Aplikacja Onit oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Rejestracji jednokrotnej] (./media/active-directory-saas-onit-tutorial/IC791168.png "Rejestracji jednokrotnej")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Onit** aplikacji w menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-onit-tutorial/IC791169.png "Atrybuty")

2.  Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    
  	|Nazwa atrybutu|Wartość atrybutu|
  	|---|---|
  	|Nazwa|User.userPrincipalName|
  	|Adres e-mail|User.mail|

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    3.  Na liście **Wartości atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.
    4.  Kliknij pozycję **pełne**.

3.  Kliknij przycisk **Zastosuj zmiany**.

4.  W przeglądarce kliknij przycisk **Wstecz** ponownie otworzyć okno dialogowe **Szybkie uruchamianie** .

5.  Kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-onit-tutorial/IC791170.png "Konfigurowanie logowania jednokrotnego")

6.  Na stronie **jak chcesz użytkownikom logowanie do Onit** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-onit-tutorial/IC791171.png "Konfigurowanie logowania jednokrotnego")

7.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Onit logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Onit (np.: "*https://ms-sso-test.onit.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-onit-tutorial/IC791172.png "Skonfigurować adres URL aplikacji")

8.  Na stronie **Konfigurowanie logowania jednokrotnego w Onit** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-onit-tutorial/IC791173.png "Konfigurowanie logowania jednokrotnego")

9.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Onit jako administrator.

10. W menu u góry kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-onit-tutorial/IC791174.png "Administracja")

11. Kliknij przycisk **Edytuj Corporation**.

    ![Edytuj Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Edytuj Corporation")

12. Kliknij kartę **Zabezpieczenia** .

    ![Informacje o firmie edycji] (./media/active-directory-saas-onit-tutorial/IC791176.png "Informacje o firmie edycji")

13. Na karcie **Zabezpieczenia** wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-onit-tutorial/IC791177.png "Rejestracji jednokrotnej")

    1.  Jako **Strategii uwierzytelniania**wybierz **rejestracji jednokrotnej i hasło**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Onit** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Protokołu Idp docelowy adres URL** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Onit** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **adres URL Wyloguj protokołu Idp** .
    4.  Skopiuj **wartość** z wyeksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu protokołu Idp (SHA1)** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    5.  **Typ rejestracji Jednokrotnej**zaznacz **SAML**.
    6.  W polu tekstowym **Tekst przycisku logowania jednokrotnego logowania** wpisz tekst, który chcesz.
    7.  Wybierz pozycję **logowania z logowania jednokrotnego: wymagane dla następujących domeny i użytkowników**, wpisz adres e-mail użytkownika testowego w polu tekstowym powiązanych i kliknij przycisk **Aktualizuj**.
        ![Edytuj Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Edytuj Corporation")

14. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-onit-tutorial/IC791179.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Onit, musi być przygotowana do Onit.  
W przypadku Onit inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy **Onit** jako administrator.

2.  Kliknij pozycję **Dodaj użytkownika**.

    ![Administracja] (./media/active-directory-saas-onit-tutorial/IC791180.png "Administracja")

3.  Na stronie okno dialogowe **Dodaj użytkownika,** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-onit-tutorial/IC791181.png "Dodawanie użytkownika")

    1.  Wpisz **nazwę** i **Adres E-mail** ważnego konta AAD, którą chcesz dodawać do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Utwórz**.  

        >[AZURE.NOTE] Właściciel konta otrzymają wiadomość e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego Onit użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Onit zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Aby przypisać użytkowników do Onit, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Onit **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-onit-tutorial/IC791182.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-onit-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).