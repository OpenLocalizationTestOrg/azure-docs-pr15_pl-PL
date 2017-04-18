<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Coupa | Microsoft Azure" 
    description="Dowiedz się, jak użyć Coupa z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Samouczek: Azure Active Directory Integracja z Coupa

Celem tego samouczka jest pokazanie integracja Azure i Coupa.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Coupa logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Coupa będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Coupa
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenariusz")
##<a name="enabling-the-application-integration-for-coupa"></a>Włączanie integracji aplikacji dla Coupa

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Coupa.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Coupa, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Coupa**.

    ![Galeria aplikacji] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Coupa**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Coupa przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Coupa wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy Coupa jako administrator.

2.  Przejdź do pozycji **konfiguracji \> kontrolę zabezpieczeń**.

    ![Kontroli bezpieczeństwa] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Kontroli bezpieczeństwa")

3.  Aby pobrać plik metadanych Coupa na komputerze, kliknij **plik do pobrania i importowanie SP metadanych**.

    ![Coupa SP metadanych] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadanych")

4.  W oknie przeglądarki różne logowanie do portalu klasyczny Azure.

5.  Na stronie integracji **Coupa** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurowanie logowania jednokrotnego")

6.  Na stronie **jak chcesz użytkownikom logowanie do Coupa** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurowanie logowania jednokrotnego")

7.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Coupa (np.: "*http://company.Coupa.com*").
    2.  Otwórz pobrany plik metadanych Coupa, a następnie skopiuj **AssertionConsumerService indeks i adres URL**.
    3.  W polu tekstowym **Adres URL odpowiedź Coupa** Wklej wartość **AssertionConsumerService indeks i adres URL** .
    4.  Kliknij przycisk **Dalej**.

8.  Na stronie **Konfigurowanie logowania jednokrotnego w Coupa** Aby pobrać plik metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurowanie logowania jednokrotnego")

9.  W witrynie firmy Coupa, przejdź do **konfiguracji \> kontrolę zabezpieczeń**.

    ![Kontroli bezpieczeństwa] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Kontroli bezpieczeństwa")

10. W sekcji **Zaloguj się przy użyciu poświadczeń Coupa** wykonać następujące czynności:

    ![Zaloguj się przy użyciu poświadczeń Coupa] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Zaloguj się przy użyciu poświadczeń Coupa")

    1.  Wybierz pozycję **Zaloguj się przy użyciu SAML**.
    2.  Kliknij przycisk **Przeglądaj,** Aby przekazać pobrany plik metadanych Azure Active.
    3.  Kliknij przycisk **Zapisz**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Coupa, musi być przygotowana do Coupa.  
W przypadku Coupa inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Coupa** jako administrator.

2.  W menu u góry kliknij pozycję **Ustawienia**, a następnie kliknij **użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Użytkownicy")

3.  Kliknij przycisk **Utwórz**.

    ![Tworzenie użytkowników] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Tworzenie użytkowników")

4.  W sekcji **Tworzenie użytkownika** wykonaj następujące czynności:

    ![Szczegóły użytkownika] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Szczegóły użytkownika")

    1.  Wpisz **identyfikator logowania**, **Imię**, **Nazwisko**, **Identyfikator rejestracji jednokrotnej**, atrybuty **E-mail** prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Utwórz**.

    >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomość e-mail z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego Coupa użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Coupa zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Aby przypisać użytkowników do Coupa, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Coupa **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
