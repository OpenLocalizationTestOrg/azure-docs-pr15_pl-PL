<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z BlueJeans | Microsoft Azure" 
    description="Dowiedz się, jak użyć BlueJeans z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Samouczek: Azure AD Integracja z BlueJeans

Celem tego samouczka jest pokazanie integracji Azure i BlueJeans.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   BlueJeans logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do BlueJeans będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy BlueJeans (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla BlueJeans
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scenariusz")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Włączanie integracji aplikacji dla BlueJeans

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla BlueJeans.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla BlueJeans, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **BlueJeans**.

    ![Galeria aplikacji] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Galeria aplikacji")

7.  W okienku wyników wybierz **BlueJeans**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania BlueJeans przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **BlueJeans** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do BlueJeans** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **BlueJeans logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://company.BlueJeans.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w BlueJeans** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **BlueJeans** jako administrator.

6.  Przejdź do pozycji **Administrator \> ustawienia grupy \> zabezpieczeń**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Administrator")

7.  W sekcji **Zabezpieczenia** należy wykonać następujące czynności:

    ![Logowaniu jednokrotnym SAML] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "Logowaniu jednokrotnym SAML")

    1.  Wybierz pozycję **SAML logowaniu jednokrotnym**.
    2.  Zaznacz opcję **Włącz automatyczne inicjowania obsługi administracyjnej**.

8.  Przechodzenie na poniższe czynności:

    ![Ścieżka certyfikatu] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Ścieżka certyfikatu")

    1.  Kliknij pozycję **Wybierz plik**, a następnie przekaż pobranego certyfikatu.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w BlueJeans** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w BlueJeans** skopiuj wartość **Zmień adres URL hasło** , a następnie wklej go w polu tekstowym **Adres URL Zmień hasło** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w BlueJeans** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .

9.  Przechodzenie na poniższe czynności:

    ![Zapisywanie zmian] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Zapisywanie zmian")

    1.  W polu tekstowym **Nazwa użytkownika** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  W polu tekstowym **wiadomości E-mail** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Kliknij przycisk **Zapisz zmiany**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do BlueJeans, musi być przygotowana do BlueJeans.  
W przypadku BlueJeans inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **BlueJeans** jako administrator.

2.  Przejdź do pozycji **Administrator \> Zarządzanie użytkownikami \> Dodaj użytkownika**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Administrator")

    >[AZURE.IMPORTANT] Na karcie **Dodaj użytkownika** jest dostępna tylko jeśli **kartę Zabezpieczenia** **włączenie automatycznego inicjowania obsługi administracyjnej** jest zaznaczona.

3.  W sekcji **Dodaj użytkownika,** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Dodawanie użytkownika")

    1.  Wpisz **BlueJeans nazwę użytkownika**, **Adres E-mail**, **Identyfikator spotkania BlueJeans**, **Moderatora kod dostępu**, **Imię i nazwisko**, **firmy** działające konto AAD chcesz dodawać do powiązanych pól tekstowych.
    2.  Kliknij pozycję **Dodaj użytkownika**.

>[AZURE.NOTE] Można użyć dowolnego innego BlueJeans użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez BlueJeans zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Aby przypisać użytkowników do BlueJeans, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **BlueJeans **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
