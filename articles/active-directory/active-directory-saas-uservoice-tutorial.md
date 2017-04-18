<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z możliwości | Microsoft Azure" 
    description="Dowiedz się, jak użyć możliwości z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Samouczek: Integracja usługi Azure Active Directory z witryny UserVoice
  
Celem tego samouczka jest pokazanie integracji Azure i możliwości.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy możliwości
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do możliwości będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny możliwości (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla witryny UserVoice
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scenariusz")

##<a name="enabling-the-application-integration-for-uservoice"></a>Włączanie integracji aplikacji dla witryny UserVoice
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla witryny UserVoice.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla witryny UserVoice, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **możliwości**.

    ![Galeria aplikacji] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Galeria aplikacji")

7.  W okienku wyników wybierz **możliwości**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Możliwości] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "Możliwości")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML możliwości przy użyciu swojego konta.  
Konfigurowanie rejestracji jednokrotnej dla możliwości wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **możliwości** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do witryny UserVoice** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Możliwości adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. UserVoice.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w możliwości** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\UserVoice.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy możliwości jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję Ustawienia, a następnie z menu wybierz portalu.

    ![Ustawienia] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Ustawienia")

7.  Na karcie **portalu** w sekcji **Uwierzytelnianie użytkownika** kliknij przycisk **Edytuj** , aby otworzyć stronę okno dialogowe **Edytowanie uwierzytelnianie użytkownika**

    ![Portal sieci Web] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Portal sieci Web")

8.  Na stronie okno dialogowe **Edytowanie uwierzytelniania użytkowników** wykonaj następujące czynności:

    ![Edytowanie uwierzytelniania użytkowników] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Edytowanie uwierzytelniania użytkowników")

    1.  Kliknij przycisk **Logowania jednokrotnego (SSO)**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w możliwości** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Logowania jednokrotnego zdalnego logowania** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w możliwości** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu **tekstowym Sign-Out zdalnego logowania jednokrotnego**.
    4.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **bieżącego odcisku palca SHA1 certyfikatu** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    5.  Kliknij przycisk **Zapisz ustawienia uwierzytelniania**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD do zalogowania się do witryny UserVoice, musi być przygotowana do możliwości.  
W przypadku możliwości inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **możliwości** .

2.  Przejdź do **ustawień**.

    ![Ustawienia] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Ustawienia")

3.  Kliknij pozycję **Ogólne**.

4.  Kliknij przycisk **uprawnienia i agentów**.

    ![Czynniki i uprawnienia] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Czynniki i uprawnienia")

5.  Kliknij przycisk **Dodaj administratorów**.

    ![Dodawanie administratorów] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Dodawanie administratorów")

6.  W oknie dialogowym **Zapraszanie Administratorzy** wykonaj następujące czynności:

    ![Zapraszanie Administratorzy] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Zapraszanie administratorów")

    1.  W texbox wiadomości E-mail wpisz adres e-mail konta do świadczenia, a następnie kliknij przycisk **Dodaj**.
    2.  Kliknij przycisk **Zaproś**.

>[AZURE.NOTE] Można użyć wszelkie inne możliwości użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez możliwości zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Aby przypisać użytkowników do witryny UserVoice, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **możliwości **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).