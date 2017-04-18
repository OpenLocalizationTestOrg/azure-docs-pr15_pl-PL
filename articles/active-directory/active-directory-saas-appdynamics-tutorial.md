<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z AppDynamics | Microsoft Azure" 
    description="Dowiedz się, jak użyć AppDynamics z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Samouczek: Azure Active Directory Integracja z AppDynamics

Celem tego samouczka jest pokazanie integracja Azure i AppDynamics. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   AppDynamics logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do AppDynamics będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy AppDynamics (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla AppDynamics
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scenariusz")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Włączanie integracji aplikacji dla AppDynamics

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla AppDynamics.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla AppDynamics, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **AppDynamics**.

    ![Galeria aplikacji] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Galeria aplikacji")

7.  W okienku wyników wybierz **AppDynamics**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania AppDynamics przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **AppDynamics** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Konfigurowanie logować pojedynczy")

2.  Na stronie **jak chcesz użytkownikom logowanie do AppDynamics** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Konfigurowanie logować pojedynczy")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **AppDynamics logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w AppDynamics ("*https://companyname.saas.appdynamics.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w AppDynamics** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Konfigurowanie logować pojedynczy")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy AppDynamics jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**, a następnie kliknij **administracji**.

    ![Administracja] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Administracja")

7.  Kliknij kartę **Dostawca uwierzytelniania** .

    ![Dostawca uwierzytelniania] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Dostawca uwierzytelniania")

8.  W sekcji **Dostawcy uwierzytelniania** wykonaj następujące czynności:

    ![Konfiguracja SAML] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "Konfiguracja SAML")

    1.  **Dostawca uwierzytelniania**wybierz pozycję **SAML**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w AppDynamics** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w AppDynamics** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    4.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **certyfikatu**
    6.  Kliknij przycisk **Zapisz**.
        ![Zapisywanie] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Zapisywanie")

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Konfigurowanie logować pojedynczy")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do AppDynamics, musi być przygotowana do AppDynamics.  
W przypadku AppDynamics inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy AppDynamics jako administrator.

2.  Przejdź do pozycji **Użytkownicy**, a następnie kliknij pozycję **+** aby otworzyć okno dialogowe **Utwórz użytkownika** .

    ![Użytkownicy] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Użytkownicy")

3.  W sekcji **Tworzenie użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Tworzenie użytkownika")

    1.  Wpisz **nazwę użytkownika**, **nazwę**, **Adres E-mail**, **Nowe hasło**, **Powtórz nowe hasło** działające konto AAD chcesz dodawać do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego AppDynamics użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez AppDynamics do zapewniania obsługi Azure AD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Aby przypisać użytkowników do AppDynamics, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **AppDynamics **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
