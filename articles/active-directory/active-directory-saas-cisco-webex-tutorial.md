<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Cisco Webex | Microsoft Azure" 
    description="Dowiedz się, jak użyć Cisco Webex z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Samouczek: Integracja usługi Azure Active Directory z Cisco Webex

Celem tego samouczka jest pokazanie integracji Azure i Cisco Webex.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Cisco Webex

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Cisco Webex będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Cisco Webex (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Cisco Webex
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenariusz")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Włączanie integracji aplikacji dla Cisco Webex

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Cisco Webex.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla firmy Cisco Webex, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Cisco Webex**.

    ![Galeria aplikacji] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Cisco Webex**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML Cisco Webex przy użyciu swojego konta.  
W ramach tej procedury jest wymagane, aby utworzyć certyfikat zakodowany base-64.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Cisco Webex** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Cisco Webex** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **Sing na adres URL** wpisz adres URL dzierżawy Cisco Webex (przykład: *http://contoso.webex.com*).
    2.  W polu tekstowym **Adres URL odpowiedź Webex Cisco** wpisz adres **Cisco Webex AssertionConsumerService URL** (przykład: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Cisco Webex** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Cisco Webex jako administrator.

6.  W menu u góry kliknij pozycję **Administracja witryny**.

    ![Administracja witryną] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Administracja witryną")

7.  W sekcji **Zarządzanie witryną** kliknij pozycję **Konfiguracja logowania jednokrotnego**.

    ![Konfiguracja logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Konfiguracja logowania jednokrotnego")

8.  W sekcji Federacji Konfiguracja logowania jednokrotnego usługi sieci Web wykonaj następujące czynności:

    ![Federacji konfiguracji logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federacji konfiguracji logowania jednokrotnego")

    1.  Na liście **Protokół Federacji** zaznacz **SAML 2.0**.
    2.  Tworzenie pliku **Base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otwórz base-64 certyfikatu zakodowany w Notatniku, a następnie skopiuj zawartość go.
    4.  Kliknij przycisk **Importuj SAML metadane**, a następnie wklej certyfikatu zakodowany base-64.
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Cisco Webex** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy jako SAML (protokołu IdP ID)** .
    6.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Cisco Webex** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania usługi rejestracji Jednokrotnej klienta** .
    7.  Na liście **NameID Format** wybierz **Adres E-mail**.
    8.  W polu tekstowym **AuthnContextClassRef** wpisz **Urn: oasis: nazwy: tc: SAML:2.0:ac:classes:Password**.
    9.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Cisco Webex** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj usługi rejestracji Jednokrotnej klienta** .
    10. Kliknij przycisk **Aktualizuj**.

9.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Cisco Webex** wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **wykonane**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Cisco Webex, musi być przygotowana do Cisco Webex.  
W przypadku Cisco Webex inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Cisco Webex** .

2.  Przejdź do pozycji **Zarządzanie użytkownikami \> Dodaj użytkownika**.

    ![Dodawanie użytkowników] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Dodawanie użytkowników")

3.  W sekcji Dodaj użytkownika, wykonaj następujące czynności:

    ![Dodaj użytkownika] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Dodaj użytkownika")

    1.  **W obszarze Typ konta**wybierz **hosta**.
    2.  Wpisz informacje istniejącego użytkownika Azure AD w następujących polach tekstowych: ** **imię, nazwisko**, **nazwę użytkownika**, wiadomości E-mail**, **hasło**, **Potwierdź hasło**.
    3.  Kliknij przycisk **Dodaj**.

>[AZURE.NOTE] Można użyć dowolnego innego Cisco Webex użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Cisco Webex zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Aby przypisać użytkowników do Cisco Webex, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Cisco Webex **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
