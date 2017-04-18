<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Bime | Microsoft Azure" 
    description="Dowiedz się, jak użyć Bime z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Samouczek: Integracja usługi Azure Active Directory z Bime

Celem tego samouczka jest pokazanie integracji Azure i Bime.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Bime

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Bime będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Bime (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Bime
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-bime-tutorial/IC775552.png "Scenariusz")
##<a name="enabling-the-application-integration-for-bime"></a>Włączanie integracji aplikacji dla Bime

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Bime.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Bime, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-bime-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-bime-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Bime**.

    ![Galeria aplikacji] (./media/active-directory-saas-bime-tutorial/IC775553.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Bime**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Bime przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Bime wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Bime** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bime-tutorial/IC771709.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Bime** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bime-tutorial/IC775555.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Bime adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Bimeapp.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-bime-tutorial/IC775556.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Bime** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Bime.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bime-tutorial/IC775557.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Bime jako administrator.

6.  Na pasku narzędzi kliknij pozycję **Administrator**, a następnie **konta**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775558.png "Administrator")

7.  Na stronie Konfiguracja konta wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bime-tutorial/IC775559.png "Konfigurowanie logowania jednokrotnego")

    1.  Wybierz opcję **Włącz SAML uwierzytelniania**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Bime** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL zdalnego logowania** .
    3.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    4.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bime-tutorial/IC775560.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Bime, musi być przygotowana do Bime.  
W przypadku Bime inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Bime** .

2.  Na pasku narzędzi kliknij pozycję **Administrator**, a następnie **użytkowników**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775561.png "Administrator")

3.  Z **Listy użytkowników**kliknij pozycję **Dodaj nowego użytkownika** ("+").

    ![Użytkownicy] (./media/active-directory-saas-bime-tutorial/IC775562.png "Użytkownicy")

4.  Na stronie okno dialogowe **Szczegóły użytkownika** wykonaj następujące czynności:

    ![Szczegóły użytkownika] (./media/active-directory-saas-bime-tutorial/IC775563.png "Szczegóły użytkownika")

    1.  Wprowadź imię, nazwisko, logowania, E-mail działające konto AAD potrzebne do świadczenia.
    2.  Kliknij przycisk Zapisz.

>[AZURE.NOTE] Można użyć dowolnego innego Bime użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Bime zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Aby przypisać użytkowników do Bime, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Bime **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-bime-tutorial/IC775564.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-bime-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
