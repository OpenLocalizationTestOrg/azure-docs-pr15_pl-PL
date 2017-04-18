<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z SumoLogic | Microsoft Azure" 
    description="Dowiedz się, jak użyć SumoLogic z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Samouczek: Integracja usługi Azure Active Directory z SumoLogic
  
Celem tego samouczka jest pokazanie integracji Azure i SumoLogic.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy SumoLogic
  
Ten samouczek użytkowników Azure AD przypisane do SumoLogicwill się możliwość pojedynczy znak na aplikację w swojej SumoLogic firmy witryny (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla SumoLogic
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scenariusz")

##<a name="enabling-the-application-integration-for-sumologic"></a>Włączanie integracji aplikacji dla SumoLogic
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla SumoLogic.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla SumoLogic, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **sumologic**.

    ![Galeria aplikacji] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Galeria aplikacji")

7.  W okienku wyników wybierz **SumoLogic**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania SumoLogic przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do swojego SumoLogictenant.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SumoLogic** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do SumoLogic** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **SumoLogic adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. SumoLogic.com*", a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie aoo URL] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfigurowanie aoo URL")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w SumoLogic** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy SumoLogic jako administrator.

6.  Przejdź do pozycji **Zarządzanie \> zabezpieczeń**.

    ![Zarządzanie] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Zarządzanie")

7.  Kliknij pozycję **SAML**.

    ![Ustawienia zabezpieczeń globalnych] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Ustawienia zabezpieczeń globalnych")

8.  Z listy **Wybierz konfiguracji lub Utwórz nową** wybierz pozycję **Azure AD**, a następnie kliknij **Konfiguruj**.

    ![Konfigurowanie SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Konfigurowanie SAML 2.0")

9.  W oknie dialogowym **Konfigurowanie SAML 2.0** wykonaj następujące czynności:

    ![Konfigurowanie SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Konfigurowanie SAML 2.0")

    1.  W polu tekstowym **Nazwa konfiguracji** wpisz **Azure AD**.
    2.  Wybierz **Tryb wykrywania błędów**.
    3.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w SumoLogic** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    4.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w SumoLogic** skopiuj wartość **Adres URL uwierzytelniania żądanie** , a następnie wklej go w polu tekstowym **Adres URL poziomach żądanie** .
    5.  Tworzenie pliku **zakodowany Base-64** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wklej cały certyfikat w **Certyfikat X.509** pole tekstowe.
    7.  Jako **Atrybut poczty E-mail**wybierz **temat SAML użycia**.
    8.  Wybierz pozycję **SP zainicjowane konfiguracji logowania**.
    9.  W polu tekstowym **Ścieżka logowania** wpisz **Azure**.
    10. Kliknij przycisk **Zapisz**.

10. W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w SumoLogic** wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **wykonane**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do SumoLogic, musi być przygotowana do SumoLogic.  
W przypadku SumoLogic inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **SumoLogic** .

2.  Przejdź do pozycji **Zarządzanie \> użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Użytkownicy")

3.  Kliknij przycisk **Dodaj**.

    ![Użytkownicy] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Użytkownicy")

4.  W oknie dialogowym **Nowy użytkownik** wykonaj następujące czynności:

    ![Nowy użytkownik] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Nowy użytkownik")

    1.  Wpisz odpowiednie informacje konta Azure AD, które ma zostać należy do pól tekstowych **Imię**, **Nazwisko** i **Adres E-mail** .
    2.  Wybierz rolę.
    3.  **Stan**wybierz pozycję **aktywne**.
    4.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego SumoLogic użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez SumoLogic zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Aby przypisać użytkowników do SumoLogic, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **SumoLogic** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).