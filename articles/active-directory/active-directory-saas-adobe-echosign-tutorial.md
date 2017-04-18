<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Adobe EchoSign | Microsoft Azure" 
    description="Dowiedz się, jak użyć Adobe EchoSign z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Samouczek: Azure Active Directory Integracja z Adobe EchoSign

Celem tego samouczka jest pokazanie integracji Azure i Adobe EchoSign.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Pojedynczy EchoSign Adobe logowanie enabled subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Adobe EchoSign będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Adobe EchoSign (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla programu Adobe EchoSign
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenariusz")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Włączanie integracji aplikacji dla programu Adobe EchoSign

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla programu Adobe EchoSign.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla programu Adobe EchoSign, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Adobe EchoSign**.

    ![Galeria aplikacji] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Adobe EchoSign**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Adobe EchoSign przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Adobe EchoSign** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do programu Adobe EchoSign** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adobe EchoSign logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://company.echosign.com/*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Adobe EchoSign** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Adobe EchoSign jako administrator.

6.  W menu u góry kliknij **konto**, a następnie w okienku nawigacji po lewej stronie struktury, kliknij pozycję **Ustawienia SAML** w obszarze **Ustawienia kont**.

    ![Konta] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Konta")

7.  W sekcji Ustawienia SAML wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "Ustawienia SAML")

    1.  Jak **Tryb SAML**wybierz pozycję **SAML obowiązkowe**.
    2.  Wybierz pozycję **Zaloguj się przy użyciu poświadczeń EchoSign administratorom konta EchoSign Zezwalaj**.
    3.  **Tworzenie użytkownika**zaznacz **automatycznie dodawać użytkowników uwierzytelnione za pośrednictwem SAML**.

8.  Przenoszenie, wykonując następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "Ustawienia SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Adobe EchoSign** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go w polu tekstowym **Identyfikator jednostki protokołu IdP** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Adobe EchoSign** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL protokołu IdP logowania** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Adobe EchoSign** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj protokołu IdP** .
    4.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Certyfikat protokołu IdP** 6.  Kliknij przycisk **Zapisz zmiany**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do programu Adobe EchoSign, musi być przygotowana do Adobe EchoSign.  
W przypadku Adobe EchoSign inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Adobe EchoSign** jako administrator.

2.  W menu u góry kliknij **konto**, a następnie w okienku nawigacji po lewej stronie struktury, kliknij pozycję **Użytkownicy i grupy**, a następnie kliknij **Utwórz nowego użytkownika**.

    ![Konta] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Konta")

3.  W sekcji **Tworzenie nowego użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Tworzenie użytkownika")

    1.  Wpisz **Adres E-mail**, **Imię** i **Nazwisko** działające konto AAD ma zostać obsługi administracyjnej do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Utwórz użytkownika**.

        >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomości e-mail, która zawiera łącze, aby potwierdzić konto, zanim będzie aktywna.

>[AZURE.NOTE] Można użyć dowolnego innego Adobe EchoSign użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Adobe EchoSign zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Aby przypisać użytkowników do Adobe EchoSign, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Adobe EchoSign **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
