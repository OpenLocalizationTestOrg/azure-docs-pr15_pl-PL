<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z zabezpieczeniami Tinfoil | Microsoft Azure"
    description="Dowiedz się, jak użyć zabezpieczeń Tinfoil z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Samouczek: Azure Active Directory Integracja z zabezpieczeniami Tinfoil
  
Celem tego samouczka jest pokazanie integracji Azure i zabezpieczenia Tinfoil.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Zabezpieczenia Tinfoil logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do zabezpieczeń Tinfoil będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny zabezpieczeń Tinfoil (znak zainicjowane dostawcy tożsamości na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji Tinfoil zabezpieczeń
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Konfigurowanie logowania jednokrotnego")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Włączanie integracji aplikacji Tinfoil zabezpieczeń
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Tinfoil zabezpieczeń.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Tinfoil zabezpieczeń, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Tinfoil zabezpieczeń**.

    ![Galeria aplikacji] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Zabezpieczenia Tinfoil**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tinfoil zabezpieczeń] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil zabezpieczeń")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania zabezpieczeń Tinfoil przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie logowania jednokrotnego zabezpieczeń Tinfoil wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Tinfoil zabezpieczeń** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do zabezpieczeń Tinfoil** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL odpowiedź zabezpieczeń Tinfoil** wpisz adres URL Tinfoil zabezpieczeń potwierdzenia dla klientów indywidualnych ACS (Service) (np.: "*https://www.tinfoilsecurity.com/saml/consume*", a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Czy można uzyskać adres URL ACS z metadanych zabezpieczeń Tinfoil (https://www.tinfoilsecurity.com/saml/metadata).

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Tinfoil zabezpieczeń** , aby pobrać certyfikatu, kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Tinfoil Security.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do Tinfoil zabezpieczenia firmowej witryny jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Moje konto**.

    ![Pulpit nawigacyjny] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Pulpit nawigacyjny")

7.  Kliknij pozycję **Zabezpieczenia**.

    ![Zabezpieczenia] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Zabezpieczenia")

8.  Na stronie Konfiguracja **Logowania jednokrotnego** wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Rejestracji jednokrotnej")

    1.  Zaznacz opcję **Włącz SAML**.
    2.  Kliknij pozycję **Konfiguracja ręczna**.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Tinfoil zabezpieczeń** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres URL wpisu SAML** .
    4.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu SAML** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    5.  Skopiuj **identyfikator konta**.
    6.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Konfigurowanie logowania jednokrotnego")

10. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Atrybuty")

11. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![Atrybuty] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Atrybuty")

    1.  Kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz **parametrem accountid**.
    3.  W polu tekstowym **Wartość atrybutu** Wklej wartość Identyfikator konta, skopiowany w poprzedniej sekcji.
    4.  Kliknij pozycję **pełne**.

12. Kliknij przycisk **Zastosuj zmiany**.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do zabezpieczeń Tinfoil, musi być przygotowana do zabezpieczeń Tinfoil.  
W przypadku zabezpieczeń Tinfoil inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Aby uzyskać użytkownika obsługi administracyjnej, wykonaj następujące czynności:

1.  Jeśli użytkownik jest częścią konta organizacji, należy skontaktować się z obsługą zabezpieczeń Tinfoil uzyskać konto użytkownika utworzone.

2.  Jeśli użytkownik jest zwykły użytkownik Tinfoil zabezpieczeń władz akredytacji bezpieczeństwa, użytkownik może dodawać współpracownik do dowolnego użytkownika witryny. Uaktywnia ten proces, aby wysłać zaproszenie na określony adres e-mail, aby utworzyć nowe konto użytkownika zabezpieczeń Tinfoil.

>[AZURE.NOTE] Można użyć dowolnej innych zabezpieczeń Tinfoil użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczane przez zabezpieczeń Tinfoil zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Aby przypisać użytkowników do zabezpieczeń Tinfoil, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Tinfoil zabezpieczeń **kliknij przycisk **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).