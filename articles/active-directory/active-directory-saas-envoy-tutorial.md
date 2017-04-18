<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z wysłannika | Microsoft Azure" 
    description="Dowiedz się, jak użyć wysłannika z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Samouczek: Azure Active Directory Integracja z wysłannika
  
Celem tego samouczka jest pokazanie integracja Azure i wysłannika.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy wysłannika
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do wysłannika będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny wysłannika (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla wysłannika
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenariusz")
##<a name="enabling-the-application-integration-for-envoy"></a>Włączanie integracji aplikacji dla wysłannika
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla wysłannika.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla wysłannika, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **wysłannika**.

    ![Galeria aplikacji] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Galeria aplikacji")

7.  W okienku wyników wybierz **wysłannika**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wysłannika] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Wysłannika")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML wysłannika przy użyciu swojego konta.  
Konfigurowanie rejestracji jednokrotnej dla wysłannika wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **wysłannika** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do wysłannika** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Wysłannika adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Envoy.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w wysłannika** do pobrania certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Envoy.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy wysłannika jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**.

    ![Wysłannika] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Wysłannika")

7.  Kliknij **producenta**.

    ![Firma] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Firma")

8.  Kliknij pozycję **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  W sekcji Konfiguracja **Uwierzytelniania SAML** wykonaj następujące czynności:

    ![Uwierzytelnianie SAML] (./media/active-directory-saas-envoy-tutorial/IC776785.png "Uwierzytelnianie SAML")

    >[AZURE.NOTE] Wartość w polu Identyfikator lokalizacji HQ jest automatycznie wygenerowane przez aplikację.

    1.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **odcisku palca** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w wysłannika** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **URL SAML HTTP dostawcy tożsamości** .
    3.  Kliknij przycisk **Zapisz zmiany**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do wysłannika użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do wysłannika przy użyciu panelu programu access, wysłannika sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez wysłannika.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Aby przypisać użytkowników do wysłannika, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **wysłannika **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).