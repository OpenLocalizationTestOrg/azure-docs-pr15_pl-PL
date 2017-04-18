<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Veracode | Microsoft Azure" 
    description="Dowiedz się, jak użyć Veracode z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Samouczek: Azure Active Directory Integracja z Veracode
  
Celem tego samouczka jest pokazanie integracji Azure i Veracode. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Veracode logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Veracode będą mogli pojedynczej Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Veracode
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Scenariusz")

##<a name="enabling-the-application-integration-for-veracode"></a>Włączanie integracji aplikacji dla Veracode
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Veracode.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Veracode, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Veracode**.

    ![Galeria aplikacji] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Veracode**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Veracode przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Aplikacja Veracode oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atrybuty")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Veracode** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Veracode** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** kliknij przycisk **Dalej**.

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Konfigurowanie ustawień aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Veracode** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Veracode jako administrator.

6.  W menu u góry kliknij pozycję **Ustawienia**, a następnie kliknij **Administrator**.

    ![Administracja] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Administracja")

7.  Kliknij kartę **SAML** .

8.  W sekcji **Ustawienia SAML organizacji** wykonaj następujące czynności:

    ![Administracja] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Administracja")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Veracode** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy**
    2.  Aby przekazać pobrany certyfikatu, kliknij przycisk **Wybierz plik**.
    3.  Zaznacz opcję **Włącz rejestrację automatyczną**.

9.  W sekcji **Ustawienia rejestracji automatycznej** wykonaj następujące czynności, a następnie kliknij **Zapisz**:

    ![Administracja] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Administracja")

    1.  **Aktywacja nowego użytkownika**zaznacz **Wymagany aktywacji nie**.
    2.  **Aktualizacje danych użytkownika**zaznacz **Dane użytkownika Veracode preferencji**.
    3.  **Szczegóły atrybutu SAML**wybierz następujące czynności:
        -   **Role użytkowników**
        -   **Administrator zasad**
        -   **Recenzenta**
        -   **Zabezpieczenia potencjalnego klienta**
        -   **Zarządzający**
        -   **Przesyłające**
        -   **Kreator**
        -   **Wszystkie typy skanowania**
        -   **Członkostwo zespołu**
        -   **Domyślne zespołu**

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Konfigurowanie logowania jednokrotnego")

11. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Atrybuty")

12. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![Atrybuty] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atrybuty")

  	| Nazwa atrybutu | Wartość atrybutu |
  	|:---------------|:----------------|
  	| Imię      | User.givenName  |
  	| nazwisko       | User.surname    |
  	| Adres e-mail          | User.mail       |

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.

    3.  W polu tekstowym **Wartość atrybutu** wybierz wartość atrybutu wyświetlania dla tego wiersza.

    4.  Kliknij pozycję **pełne**.

13. Kliknij przycisk **Zastosuj zmiany**.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Veracode, musi być przygotowana do Veracode.  
W przypadku Veracode inicjowania obsługi administracyjnej to zadanie automatyczne.  
Istnieje nie czynność do wykonania dla Ciebie.
  
Użytkownicy są tworzone automatycznie w razie potrzeby podczas pierwszej pojedynczego logowania jednokrotnego próby.

>[AZURE.NOTE] Można użyć dowolnego innego Veracode użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Veracode zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Aby przypisać użytkowników do Veracode, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Veracode **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).