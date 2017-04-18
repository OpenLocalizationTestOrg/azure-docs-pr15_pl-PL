<properties
    pageTitle="Samouczek: Integracja usługi Azure Active Directory kanwy LMS | Microsoft Azure" 
    description="Dowiedz się, jak użyć LMS obszaru roboczego z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Samouczek: Integracja usługi Azure Active Directory kanwy LMS

Celem tego samouczka jest pokazanie integracji Azure i obszaru roboczego.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy obszaru roboczego

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do obszaru roboczego będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny obszaru roboczego (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji do obszaru roboczego
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scenariusz")
##<a name="enabling-the-application-integration-for-canvas"></a>Włączanie integracji aplikacji do obszaru roboczego

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla obszaru roboczego.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla obszaru roboczego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **obszaru roboczego**.

    ![Galeria aplikacji] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Galeria aplikacji")

7.  W okienku wyników zaznacz **kanwę**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Kanwa] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Kanwa")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do obszaru roboczego przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla obszaru roboczego wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **kanwy** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do obszaru roboczego** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Kanwa adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca `https://<tenant-name>.instructure.com`, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w witrynie obszaru roboczego** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do firmowej witryny obszaru roboczego jako administrator.

6.  Przejdź do pozycji **Kursy \> kont zarządzanych \> Microsoft**.

    ![Kanwa] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Kanwa")

7.  W okienku nawigacji po lewej stronie wybierz pozycję **uwierzytelniania**, a następnie kliknij **Dodaj nowe konfiguracji SAML**.

    ![Uwierzytelnianie] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Uwierzytelnianie")

8.  Na stronie bieżącej integracji wykonaj następujące czynności:

    ![Integracja z bieżącym] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Integracja z bieżącym")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego na kanwie** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go w polu tekstowym **Identyfikator jednostki protokołu IdP** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego na kanwie** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Na adres URL dziennika** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego na kanwie** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Się adres URL dziennika** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w kanwy** skopiuj wartość **Zmień adres URL hasło** , a następnie wklej go w polu tekstowym **Łącze Zmień hasło** .
    5.  Skopiuj **wartość** z wyeksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    6.  Na liście **Atrybut logowania** zaznacz **NameID**.
    7.  Na liście **Format identyfikator** zaznacz **emailAddress**.
    8.  Kliknij przycisk **Zapisz ustawienia uwierzytelniania**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do obszaru roboczego, musi być przygotowana do obszaru roboczego.  
W przypadku kanwy inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **kanwy** .

2.  Przejdź do pozycji **Kursy \> kont zarządzanych \> Microsoft**.

    ![Kanwa] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Kanwa")

3.  Kliknij pozycję **Użytkownicy**.

    ![Użytkownicy] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Użytkownicy")

4.  Kliknij przycisk **Dodaj nowego użytkownika**.

    ![Użytkownicy] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Użytkownicy")

5.  Na stronie Dodawanie nowego użytkownika okno dialogowe wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Dodawanie użytkownika")

    1.  W polu tekstowym **Imię i nazwisko** wpisz nazwę użytkownika.
    2.  W polu tekstowym **Adres E-mail** wpisz adres e-mail użytkownika.
    3.  W polu tekstowym **logowania** wpisz adres e-mail Azure AD użytkownika.
    4.  Wybierz **wiadomość E-mail dla użytkownika o tym tworzenie konta**.
    5.  Kliknij pozycję **Dodaj użytkownika**.

>[AZURE.NOTE] Można użyć dowolnego innego kanwy użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez kanwy zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Aby przypisać użytkowników do obszaru roboczego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Kanwa **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
