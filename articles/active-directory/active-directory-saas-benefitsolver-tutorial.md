<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Benefitsolver | Microsoft Azure"
    description="Dowiedz się, jak użyć Benefitsolver z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Samouczek: Azure Active Directory Integracja z Benefitsolver

Celem tego samouczka jest pokazanie integracja Azure i Benefitsolver.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Benefitsolver logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Benefitsolver będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Benefitsolver
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenariusz")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Włączanie integracji aplikacji dla Benefitsolver

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Benefitsolver, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Benefitsolver**.

    ![Galeria aplikacji] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Benefitsolver**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Benefitsolver przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Aplikacja Benefitsolver oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atrybuty")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Benefitsolver** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Benefitsolver** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Konfigurowanie ustawień aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz **http://azure.benefitsolver.com**.
    2.  W polu tekstowym **Adres URL odpowiedź** wpisz **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Kliknij przycisk **Dalej**.

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Benefitsolver** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku metadanych pobrane z zespołem pomocy technicznej Benefitsolver.

    >[AZURE.NOTE] Zespół pomocy technicznej Benefitsolver ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurowanie logowania jednokrotnego")

7.  W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Atrybuty")

8.  Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![Atrybuty] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atrybuty")

  	|Nazwa atrybutu|Wartość atrybutu|
  	|---|---|
  	|ClientID|Musisz uzyskać tę wartość z zespołem pomocy technicznej Benefitsolver.|
  	|ClientKey|Musisz uzyskać tę wartość z zespołem pomocy technicznej Benefitsolver.|
  	|LogoutURL|Musisz uzyskać tę wartość z zespołem pomocy technicznej Benefitsolver.|
  	|Pole Identyfikator pracownika|Musisz uzyskać tę wartość z zespołem pomocy technicznej Benefitsolver.|

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    3.  W polu tekstowym **Wartość atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.
    4.  Kliknij pozycję **pełne**.

9.  Kliknij przycisk **Zastosuj zmiany**.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Benefitsolver, musi być przygotowana do Benefitsolver.  
W przypadku Benefitsolver dane pracowników znajduje się w aplikacji wypełnione za pośrednictwem pliku spis systemu HRIS (zazwyczaj nightly).  

>[AZURE.NOTE] Można użyć dowolnego innego Benefitsolver użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Benefitsolver zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Aby przypisać użytkowników do Benefitsolver, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Benefitsolver **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
