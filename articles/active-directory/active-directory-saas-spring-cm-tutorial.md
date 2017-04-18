<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z CM Wiosenna | Microsoft Azure" 
    description="Dowiedz się, jak użyć CM Wiosenna z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Samouczek: Azure Active Directory Integracja z CM Wiosenna
  
Celem tego samouczka jest pokazująca, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i SpringCM.
  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   SpringCM logowania jednokrotnego włączone subskrypcji
  
Ten samouczek użytkowników usługi Azure Active Directory, dla których zostały przypisane do SpringCM będą mogli rejestracji jednokrotnej za pomocą panelu AAD programu Access.

1.  Włączanie integracji aplikacji dla SpringCM
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenariusz")

##<a name="enabling-the-application-integration-for-springcm"></a>Włączanie integracji aplikacji dla SpringCM
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla SpringCM.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla SpringCM, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **SpringCM**.

    ![Galeria aplikacji] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galeria aplikacji")

7.  W okienku wyników wybierz **SpringCM**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
W tej sekcji omówiono, jak umożliwić użytkownikom uwierzytelniania SpringCM przy użyciu swojego konta w Azure Active Directory, używanie federacyjnych na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SpringCM** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do SpringCM** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **SpringCM logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji SpringCM, a następnie kliknij przycisk **Dalej**. 

    Adres URL dzierżawy SpringCM jest używany adres URL aplikacji (przykład: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w SpringCM** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Konfigurowanie logować pojedynczy")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **SpringCM** jako administrator.

6.  W menu u góry kliknij polecenie **Przejdź do**, kliknij polecenie **Preferencje**, a następnie, w sekcji **Preferencji konta** kliknij **Logowania jednokrotnego SAML**.

    ![SAML logowania jednokrotnego] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML logowania jednokrotnego")

7.  W sekcji Konfiguracja dostawcy tożsamości wykonaj następujące czynności:

    ![Konfiguracja dostawcy tożsamości] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Konfiguracja dostawcy tożsamości")

    1.  Aby przekazać pobranego certyfikatu usługi Azure Active Directory, kliknij przycisk **Wybierz wystawcy** lub **Zmień wystawcy certyfikatu**.
    2.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w SpringCM** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    3.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego w SpringCM** skopiuj wartość **Singel logowania jednokrotnego usługi adres URL** , a następnie wklej go w polu tekstowym **Końcowy inicjowanych przez dostawcę usługi (SP)** .
    4.  Jako **Włączone SAML**zaznacz pole wyboru **Włącz**.
    5.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Konfigurowanie logować pojedynczy")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom usługi Azure Active Directory zalogować się do SpringCM, musi być przygotowana do SpringCM.  
W przypadku SpringCM inicjowania obsługi administracyjnej to zadanie ręczne.

>[AZURE.NOTE] Aby uzyskać więcej informacji zobacz [Tworzenie i edytowanie użytkowników SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Aby obsługi administracyjnej konta użytkownika w celu SpringCM, należy wykonać następujące czynności:

1.  Zaloguj się do witryny firmy **SpringCM** jako administrator.

2.  Kliknij przycisk **Przejdź do**, a następnie kliknij polecenie **Książka adresowa**.

    ![Tworzenie użytkownika] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Tworzenie użytkownika")

3.  Kliknij przycisk **Utwórz użytkownika**.

4.  Wybierz **rolę**.

5.  Wybierz pozycję **Wyślij E-mail aktywacji**.

6.  Wpisz imię, nazwisko i adres e-mail prawidłowe konto użytkownika usługi Azure Active Directory, którego chcesz zainicjować obsługę do powiązanych pól tekstowych.

7.  Dodaj użytkownika do **grupy zabezpieczeń**.

8.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego SpringCM użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez SpringCM zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Aby przypisać użytkowników do SpringCM, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **SpringCM** aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).




