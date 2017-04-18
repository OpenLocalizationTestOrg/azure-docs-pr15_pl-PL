<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Lucidchart | Microsoft Azure" 
    description="Dowiedz się, jak użyć Lucidchart z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Samouczek: Azure Active Directory Integracja z Lucidchart
  
Celem tego samouczka jest pokazanie integracja Azure i Lucidchart.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Lucidchart logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Lucidchart będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Lucidchart (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Lucidchart
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scenariusz")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Włączanie integracji aplikacji dla Lucidchart
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Lucidchart.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Lucidchart, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Lucidchart**.

    ![Galeria aplikacji] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Lucidchart**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Lucidchart przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Lucidchart** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Lucidchart** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Lucidchart logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Lucidchart (np.: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Lucidchart** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik danych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Lucidchart jako administrator.

6.  W menu u góry kliknij **zespołu**.

    ![Zespołu] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Zespołu")

7.  Kliknij pozycję **aplikacji \> Zarządzanie SAML**.

    ![Zarządzanie SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Zarządzanie SAML")

8.  Na stronie okno dialogowe **Ustawienia uwierzytelniania SAML** wykonaj następujące czynności:

    1.  Wybierz opcję **Włącz uwierzytelnianie SAML**, a następnie kliknij **opcjonalne**.
        ![Ustawienia uwierzytelniania SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "Ustawienia uwierzytelniania SAML")
    2.  W polu tekstowym **domeny** wpisz nazwę swojej domeny, a następnie kliknij **Certyfikat zmiany**.
        ![Zmienianie certyfikatu] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Zmienianie certyfikatu")
    3.  Otwórz plik pobrany metadanych, skopiuj zawartość, a następnie wklej go w polu tekstowym **Przekazywanie metadanych** .
        ![Przekazywanie metadanych] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Przekazywanie metadanych")
    4.  **Automatyczne dodawanie nowego użytkownika do zespołu**zaznacz, a następnie kliknij przycisk **Zapisz zmiany**.
        ![Zapisywanie zmian] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Zapisywanie zmian")

9.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do Lucidchart użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do Lucidchart przy użyciu panelu programu access, Lucidchart sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez Lucidchart.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Aby przypisać użytkowników do Lucidchart, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Lucidchart **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).