<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z poczty Zoho | Microsoft Azure" 
    description="Dowiedz się, jak użyć Zoho poczty z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!." 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Samouczek: Azure Active Directory Integracja z Zoho poczty
  
Celem tego samouczka jest pokazanie integracji Azure i Zoho poczty.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Zoho poczty
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do poczty Zoho będą mogli rejestracji jednokrotnej do aplikacji poczty Zoho firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji poczty Zoho
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenariusz")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Włączanie integracji aplikacji poczty Zoho
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji poczty Zoho.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Aby włączyć integrację aplikacji poczty Zoho, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Zoho poczty**.

    ![Galeria aplikacji] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Poczta Zoho**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Poczta Zoho] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Poczta Zoho")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Zoho poczty przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Poczta Zoho** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do poczty Zoho** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Skonfigurować adres URL aplikacji")

    . W polu tekstowym **Zoho poczty logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca:`http://<company name>.ZohoMail.com`

    b. Kliknij przycisk **Dalej**.


4.  Na stronie **Konfigurowanie logowania jednokrotnego w Zoho poczty** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Zoho Poczta jako administrator.

6.  Przejdź do **Panelu sterowania**.

    ![Panel sterowania] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Panel sterowania")

7.  Kliknij kartę **SAML uwierzytelniania** .

    ![Uwierzytelnianie SAML] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "Uwierzytelnianie SAML")

8.  W sekcji **Szczegółów uwierzytelniania SAML** wykonaj następujące czynności:

    ![Szczegóły uwierzytelniania SAML] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Szczegóły uwierzytelniania SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w poczty Zoho** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w poczty Zoho** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w poczty Zoho** skopiuj wartość **Zmień adres URL hasło** , a następnie wklej go w polu tekstowym **Zmień adres URL hasła** .
    4.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **PublicKey** .
    6.  **Algorytm**zaznacz **RSA**.
    7.  Kliknij **przycisk OK**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować do poczty Zoho, musi być przygotowana do Zoho poczty.  
W przypadku poczty Zoho inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Zoho Poczta** jako administrator.

2.  Przejdź do pozycji **Panel sterowania \> Poczta i dokumenty**.

3.  Przejdź do pozycji **Szczegóły użytkownika \> Dodaj użytkownika**.

    ![Dodawanie użytkownika] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Dodawanie użytkownika")

4.  W oknie dialogowym **Dodawanie użytkowników** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Dodawanie użytkownika")

    1.  Wpisz **Imię**, **Nazwisko**, **Identyfikator poczty E-mail**, **hasło** prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij **przycisk OK**.  

        >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomość e-mail z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego poczty Zoho użytkownika konta narzędzia do tworzenia lub interfejsów API przekazanego pocztą Zoho zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Aby przypisać użytkowników do Zoho poczty, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Poczty Zoho **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).