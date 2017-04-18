<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Clever | Microsoft Azure" 
    description="Dowiedz się, jak użyć Clever z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Samouczek: Azure Active Directory Integracja z Clever

Celem tego samouczka jest pokazanie integracja Azure i Clever. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Inteligentne dzierżawy

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Clever będą mogli pojedynczy Zaloguj się do aplikacji w witrynie firmy inteligentne (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Clever
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-clever-tutorial/IC798977.png "Scenariusz")
##<a name="enabling-the-application-integration-for-clever"></a>Włączanie integracji aplikacji dla Clever

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Clever.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Clever, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-clever-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-clever-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Clever**.

    ![Galeria aplikacji] (./media/active-directory-saas-clever-tutorial/IC798978.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Clever**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Inteligentne] (./media/active-directory-saas-clever-tutorial/IC798979.png "Inteligentne")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Clever przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Aplikacja inteligentne oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty] (./media/active-directory-saas-clever-tutorial/IC798980.png "Atrybuty")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Clever** integracja aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clever-tutorial/IC784682.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Clever** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clever-tutorial/IC798981.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Inteligentne logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji inteligentne (przykład: *https://clever.com/in/azsandbox*), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-clever-tutorial/IC798982.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Clever** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clever-tutorial/IC798983.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy inteligentne jako administrator.

6.  Na pasku narzędzi kliknij przycisk **Zaloguj błyskawiczne**.

    ![Błyskawiczne logowania] (./media/active-directory-saas-clever-tutorial/IC798984.png "Błyskawiczne logowania")

7.  Na stronie **Logowania błyskawicznych** wykonaj następujące czynności:

    ![Błyskawiczne logowania] (./media/active-directory-saas-clever-tutorial/IC798985.png "Błyskawiczne logowania")

    1.  Wpisz **adres URL logowania**.  

        >[AZURE.NOTE] **Adres URL logowania** jest wartość niestandardową.
Rzeczywista wartość możesz przejść z zespołem pomocy technicznej inteligentne.

    2.  **System tożsamości**zaznacz **ADFS**.
    3.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clever-tutorial/IC798986.png "Konfigurowanie logowania jednokrotnego")

9.  W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-clever-tutorial/IC795920.png "Atrybuty")

10. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![atrybuty tokenu SAML] (./media/active-directory-saas-clever-tutorial/IC795921.png "atrybuty token SAML")

  	|Nazwa atrybutu|Wartość atrybutu|
  	|---|---|
  	|clever.student.Credentials.District\_nazwy użytkownika|User.userPrincipalName|

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    3.  W polu tekstowym **Wartość atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.
    4.  Kliknij pozycję **pełne**.

11. Kliknij przycisk **Zastosuj zmiany**.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Clever, musi być przygotowana do Clever.  
W przypadku Clever inicjowania obsługi administracyjnej jest ręczne zadaniu, które mają być wykonywane przez zespół pomocy technicznej inteligentne.

>[AZURE.NOTE] Można użyć dowolnego innego użytkownika inteligentne konta narzędzia do tworzenia lub interfejsów API dostarczane przez Clever zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Aby przypisać użytkowników do Clever, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Clever **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-clever-tutorial/IC798987.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-clever-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
