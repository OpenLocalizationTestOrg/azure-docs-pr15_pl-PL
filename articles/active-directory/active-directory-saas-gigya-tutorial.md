<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Gigya | Microsoft Azure" 
    description="Dowiedz się, jak użyć Gigya z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Samouczek: Azure Active Directory Integracja z Gigya
  
Celem tego samouczka jest pokazanie integracja Azure i Gigya.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Gigya rejestracji jednokrotnej na włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Gigya będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Gigya (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Gigya
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Konfigurowanie logowania jednokrotnego")
##<a name="enabling-the-application-integration-for-gigya"></a>Włączanie integracji aplikacji dla Gigya
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Gigya.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Gigya, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Gigya**.

    ![Galeria aplikacji] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Gigya**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Gigya przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Gigya** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Gigya** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Gigya logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*http://company.gigya.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Gigya** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Gigya jako administrator.

6.  Przejdź do pozycji **Ustawienia \> logowania SAML**, a następnie kliknij przycisk **Dodaj** .

    ![SAML logowania] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML logowania")

7.  W sekcji **SAML logowania** wykonaj następujące czynności:

    ![Konfiguracja SAML] (./media/active-directory-saas-gigya-tutorial/IC789533.png "Konfiguracja SAML")

    1.  W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Gigya** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Gigya** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Pojedynczy znak na adres URL usługi** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Gigya** skopiuj wartość **Format identyfikator nazwy** , a następnie wklej go w polu tekstowym **Nazwa Format Identyfikatora** .
    5.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Certyfikat X.509** .
    7.  Kliknij przycisk **Zapisz ustawienia**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Gigya, musi być przygotowana do Gigya.  
W przypadku Gigya inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Gigya** jako administrator.

2.  Przejdź do pozycji **Administrator \> Zarządzanie użytkownikami**, a następnie kliknij polecenie **Zaproś użytkowników**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Zarządzanie użytkownikami")

3.  W oknie dialogowym zapraszanie użytkowników wykonaj następujące czynności:

    ![Zapraszanie użytkowników] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Zapraszanie użytkowników")

    1.  W polu tekstowym **wiadomości E-mail** wpisz alias e-mail prawidłowe konta usługi Azure Active Directory, które chcesz świadczenia.
    2.  Kliknij polecenie **Zaproś użytkownika**.
    
        >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomości e-mail, która zawiera łącze, aby potwierdzić konto, zanim będzie aktywna.

>[AZURE.NOTE] Można użyć dowolnego innego Gigya użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Gigya zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Aby przypisać użytkowników do Gigya, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Gigya **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).