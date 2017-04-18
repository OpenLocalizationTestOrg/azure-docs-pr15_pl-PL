<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z nowego Relic | Microsoft Azure" 
    description="Dowiedz się, jak użyć nowej Relic z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Samouczek: Azure Active Directory Integracja z nowego Relic
  
Celem tego samouczka jest pokazująca, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Relic nowy.
  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Nowy Relic logowania jednokrotnego włączone subskrypcji
  
Ten samouczek użytkowników usługi Azure Active Directory, dla których zostały przypisane do nowych Relic będą mogli rejestracji jednokrotnej za pomocą panelu AAD programu Access.

1.  Włączanie integracji aplikacji dla nowego Relic
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scenariusz")
##<a name="enabling-the-application-integration-for-new-relic"></a>Włączanie integracji aplikacji dla nowego Relic
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla nowych Relic.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla nowych Relic, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Nowy Relic**.

    ![Galeria aplikacji] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Relic nowy**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Nowy Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Nowy Relic")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
W tej sekcji omówiono, jak umożliwić użytkownikom uwierzytelniania nowych Relic przy użyciu swojego konta w Azure Active Directory, używanie federacyjnych na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracja **Relic nowej** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do nowego Relic** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Nowy Relic logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Relic nowy, a następnie kliknij przycisk **Dalej**. 

    Adres URL dzierżawy nowe Relic jest używany adres URL aplikacji (przykład: *https://rpm.newrelic.com*):

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w nowej Relic** do pobrania certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **Relic nowe** jako administrator.

6.  W menu u góry kliknij pozycję **Ustawienia kont**.

    ![Ustawienia kont] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Ustawienia kont")

7.  Kliknij kartę **zabezpieczeń i uwierzytelniania** , a następnie kliknij kartę **logowaniu jednokrotnym** .

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Rejestracji jednokrotnej")

8.  Na stronie okno dialogowe SAML wykonaj następujące czynności:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Kliknij pozycję **Wybierz plik** do przekazania pobranego certyfikatu usługi Azure Active Directory.
    2.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w nowej Relic** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **adres URL zdalnego logowania** .
    3.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w nowej Relic** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Wyloguj wyładunku adresu URL** .
    4.  Kliknij przycisk **Zapisz wprowadzone zmiany**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom usługi Azure Active Directory zalogować się do nowego Relic, musi być przygotowana do nowej Relic.  
W przypadku nowych Relic inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Aby obsługi administracyjnej konta użytkownika do nowego Relic, należy wykonać następujące czynności:

1.  Zaloguj się do witryny firmy **Relic nowe** jako administrator.

2.  W menu u góry kliknij pozycję **Ustawienia kont**.

    ![Ustawienia kont] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Ustawienia kont")

3.  W okienku **konta** po lewej stronie kliknij **Podsumowanie**, a następnie kliknij pozycję **Dodaj użytkownika**.

    ![Ustawienia kont] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Ustawienia kont")

4.  W oknie dialogowym **aktywni użytkownicy** wykonaj następujące czynności:

    ![Aktywni użytkownicy] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktywni użytkownicy")

    1.  W polu tekstowym **wiadomości E-mail** wpisz adres e-mail prawidłowym użytkownikiem usługi Azure Active Directory, potrzebne do świadczenia.
    2.  **Rolę** wybierz **użytkownika**.
    3.  Kliknij pozycję **Dodaj użytkownika**.

>[AZURE.NOTE]Można użyć dowolnego innego Relic nowego użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Relic nowych kont użytkowników AAD.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Aby przypisać użytkowników do nowej Relic, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **Nowy Relic** aplikacji integracji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).




