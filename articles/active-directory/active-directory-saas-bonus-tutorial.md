<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Bonus.ly | Microsoft Azure" 
    description="Dowiedz się, jak użyć Bonus.ly z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Samouczek: Integracja usługi Azure Active Directory z Bonus.ly

Celem tego samouczka jest pokazanie integracji Azure i Bonus.ly. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Test dzierżawy w Bonus.ly

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Bonus.ly
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenariusz")
##<a name="enabling-the-application-integration-for-bonusly"></a>Włączanie integracji aplikacji dla Bonus.ly

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Bonus.ly.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Bonus.ly, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Włączanie rejestracji jednokrotnej")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Bonus.ly**.

    ![Galeria aplikacji] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Bonus.ly**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Bonus.ly przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Bonus.ly wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Bonus.ly** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Bonus.ly** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL dzierżawy Bonus.ly** wpisz adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Bonus.LY*", a następnie kliknij przycisk **Dalej**: 

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Bonus.ly** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Bonusly.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki różnych Zaloguj się do Twojej dzierżawy **Bonus.ly** .

6.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**, a następnie wybierz **integracji i aplikacje**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  W obszarze **Logowania jednokrotnego**wybierz **SAML**.

8.  Na stronie okno dialogowe **SAML** wykonaj następujące czynności:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Bonus.ly** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **logowania jednokrotnego protokołu IdP docelowy adres URL** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Bonus.ly** skopiuj wartość **Identyfikator wystawcy** , a następnie wklej go w polu tekstowym **Protokołu IdP wystawcy** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Bonus.ly** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL protokołu IdP logowania** .
    4.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

9.  Kliknij przycisk **Zapisz**.

10. W klasycznym portalu Microsoft Azure wybierz potwierdzenia konfiguracji, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Bonus.ly, musi być przygotowana do Bonus.ly.  
W przypadku Bonus.ly inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  W oknie przeglądarki sieci web Zaloguj się do Twojej dzierżawy Bonus.ly.

2.  Kliknij pozycję **Ustawienia**

    ![Ustawienia] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Ustawienia")

3.  Kliknij kartę **użytkowników i dodatki** .

    ![Użytkownicy i dodatki] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Użytkownicy i dodatki")

4.  Kliknij pozycję **Zarządzanie użytkownikami**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Zarządzanie użytkownikami")

5.  Kliknij pozycję **Dodaj użytkownika**.

    ![Dodawanie użytkownika] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Dodawanie użytkownika")

6.  W oknie dialogowym **Dodaj użytkownika,** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Dodawanie użytkownika")

    1.  Wpisz "**poczty E-mail**, **Imię**, **nazwisko**" prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

    >[AZURE.NOTE] Właściciel konta AAD otrzymają wiadomości e-mail, która zawiera łącze, aby potwierdzić konto, zanim będzie aktywna.

>[AZURE.NOTE] Można użyć dowolnego innego Bonus.ly użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Bonus.ly zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Aby przypisać użytkowników do Bonus.ly, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji Bonus.ly aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
