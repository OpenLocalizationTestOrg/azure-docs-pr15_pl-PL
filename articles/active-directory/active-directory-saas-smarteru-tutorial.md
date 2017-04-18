<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z SmarterU | Microsoft Azure" 
    description="Dowiedz się, jak użyć SmarterU z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Samouczek: Integracja usługi Azure Active Directory z SmarterU
  
Celem tego samouczka jest pokazanie integracji Azure i SmarterU.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy SmarterU
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do SmarterU będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy SmarterU (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla SmarterU
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenariusz")

##<a name="enabling-the-application-integration-for-smarteru"></a>Włączanie integracji aplikacji dla SmarterU
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla SmarterU.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla SmarterU, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **SmarterU**.

    ![Fallery aplikacji] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Fallery aplikacji")

7.  W okienku wyników wybierz **SmarterU**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania SmarterU przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SmarterU** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do SmarterU** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Konfigurowanie logowania jednokrotnego")

3.  Na **Konfigurowanie logowania jednokrotnego w SmarterU** strony, aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie danych lokalnie jako **c:\\SmarterUMetaData.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Konfigurowanie logowania jednokrotnego")

4.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy SmarterU jako administrator.

5.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia kont**.

    ![Ustawienia kont] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Ustawienia kont")

6.  Na stronie Konfiguracja konta wykonaj następujące czynności:

    ![Autoryzacja zewnętrznych] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Autoryzacja zewnętrznych")

    1.  Zaznacz opcję **Włącz autoryzacji zewnętrznych**.
    2.  W sekcji **Kontrola logowania wzorzec** wybierz kartę **SmarterU** .
    3.  W sekcji **Domyślne logowania użytkownika** wybierz kartę **SmarterU** .
    4.  Zaznacz opcję **Włącz Okta**.
    5.  Skopiuj zawartość metadanych pobrany plik, a następnie wklej go w polu tekstowym **Okta metadanych** .
    6.  Kliknij przycisk **Zapisz**.

7.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do SmarterU, musi być przygotowana do SmarterU.  
W przypadku SmarterU inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **SmarterU** .

2.  Przejdź do **użytkowników**.

3.  W sekcji dla użytkownika wykonaj następujące czynności:

    ![Nowy użytkownik] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Nowy użytkownik")

    1.  Kliknij przycisk **+ użytkownika**.
    2.  Wpisz pokrewne atrybuty Azure AD konta użytkownika w następujących polach tekstowych: **Podstawowa poczty E-mail**, **Identyfikator pracownika**, **hasło**, **Potwierdź hasło**, **Imię**, **nazwisko**.
    3.  Kliknij pozycję **aktywne**.
    4.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego SmarterU użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez SmarterU zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Aby przypisać użytkowników do SmarterU, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **SmarterU **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).