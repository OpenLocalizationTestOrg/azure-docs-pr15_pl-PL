<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Freshdesk | Microsoft Azure" 
    description="Dowiedz się, jak użyć Freshdesk z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Samouczek: Integracja usługi Azure Active Directory z Freshdesk
  
Celem tego samouczka jest pokazanie integracji Azure i Freshdesk.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Freshdesk
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Freshdesk będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Freshdesk (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Freshdesk
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Scenariusz")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Włączanie integracji aplikacji dla Freshdesk
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Freshdesk.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Freshdesk, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Freshdesk**.

    ![Galeria aplikacji] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Freshdesk**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Freshdesk przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Freshdesk wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Freshdesk** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Freshdesk** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Freshdesk adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Freshdesk.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Freshdesk** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Freshdesk.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Freshdesk jako administrator.

6.  W menu u góry kliknij pozycję **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Administrator")

7.  Na karcie **Ustawienia ogólne** kliknij pozycję **Zabezpieczenia**.

    ![Zabezpieczenia] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Zabezpieczenia")

8.  W sekcji **Zabezpieczenia** należy wykonać następujące czynności:

    ![Logowaniu jednokrotnym] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Logowaniu jednokrotnym")

    1.  **Pojedynczy znak na (SSO)**wybierz pozycję **na**.
    2.  Wybierz pozycję **Logowania jednokrotnego SAML**.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Freshdesk** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania SAML** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Freshdesk** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    5.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu zabezpieczeń** .  

        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    6.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Freshdesk, musi być przygotowana do Freshdesk.  
W przypadku Freshdesk inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Freshdesk** .

2.  W menu u góry kliknij pozycję **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Administrator")

3.  Na karcie **Ustawienia ogólne** kliknij **agentów**.

    ![Czynniki] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Czynniki")

4.  Kliknij przycisk **Nowy Agent**.

    ![Nowy Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Nowy Agent")

5.  W oknie dialogowym informacje agenta wykonaj następujące czynności:

    ![Informacje agenta] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Informacje agenta")

    1.  W polu tekstowym **Imię i nazwisko** wpisz nazwę konta Azure AD, które ma zostać świadczenia.
    2.  W polu tekstowym **Adres E-mail** wpisz adres e-mail Azure AD konta Azure AD, które ma zostać świadczenia.
    3.  W polu tekstowym **Tytuł** wpisz tytuł konto Azure AD, dla którego chcesz świadczenia.
    4.  Wybierz **rolę czynników**, a następnie kliknij **Przypisz**.
    5.  Kliknij przycisk **Zapisz**.
    
        >[AZURE.NOTE] Właściciel konta Azure AD otrzymają wiadomość e-mail, która zawiera łącza do potwierdzenia konto zostało aktywowane.

>[AZURE.NOTE] Można użyć dowolnego innego Freshdesk użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Freshdesk zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Aby przypisać użytkowników do Freshdesk, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Freshdesk **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).