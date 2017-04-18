<properties 
    pageTitle="Samouczek: Azure Active Directory integracji z oprogramowaniem Igloo | Microsoft Azure" 
    description="Dowiedz się, jak użyć Igloo oprogramowania z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Samouczek: Azure Active Directory integracji z oprogramowaniem Igloo
  
Celem tego samouczka jest pokazanie integracji Azure i Igloo oprogramowania.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   [Oprogramowanie Igloo](http://www.igloosoftware.com/) logowaniu jednokrotnym enabled subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do oprogramowania Igloo będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny oprogramowania Igloo (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla oprogramowania Igloo
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenariusz")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Włączanie integracji aplikacji dla oprogramowania Igloo
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla oprogramowania Igloo.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla oprogramowania Igloo, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Igloo oprogramowania**.

    ![Galeria aplikacji] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Oprogramowanie Igloo**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML oprogramowania Igloo przy użyciu swojego konta.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do Twojej dzierżawy centralnego pulpitu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracja **Oprogramowania Igloo** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do oprogramowania Igloo** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Microsoft Azure AD jednokrotnej] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD jednokrotnej")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Igloo oprogramowania adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://company.igloocommunities.com/?signin*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Igloo oprogramowania** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Igloo oprogramowania jako administrator.

6.  Przejdź do **Panelu sterowania**.

    ![Panel sterowania] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Panel sterowania")

7.  Na karcie **członkostwa** kliknij **Znak w ustawienia**.

    ![Zaloguj się w obszarze Ustawienia] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Zaloguj się w obszarze Ustawienia")

8.  W sekcji Konfiguracja SAML kliknij pozycję **Konfigurowanie uwierzytelniania SAML**.

    ![Konfiguracja SAML] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "Konfiguracja SAML")

9.  W sekcji **Konfiguracja ogólne** wykonaj następujące czynności:

    ![Ogólne konfiguracji] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Ogólne konfiguracji")

    1.  W polu tekstowym **Nazwa połączenia** wpisz nazwę niestandardową dla konfiguracji.
    2.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w oprogramowania Igloo** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL protokołu IdP logowania** .
    3.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w oprogramowania Igloo** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj protokołu IdP** .
    4.  **Wyloguj odpowiedź i typ żądania HTTP**zaznacz **WPIS**.
    5.  Tworzenie pliku tekstowego z pobranego certyfikatu.
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    6.  Usuwanie pierwszy wiersz i ostatni wiersz z wersji pliku tekstowego certyfikatu, kopiowanie reszcie tekstu certyfikatu i wkleisz go w polu tekstowym **Certyfikatu publicznego** .

10. W **odpowiedzi i konfiguracji uwierzytelniania**wykonaj następujące czynności:

    ![Odpowiedź i konfiguracji uwierzytelniania] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Odpowiedź i konfiguracji uwierzytelniania")

    1.  **Dostawcy tożsamości**wybierz pozycję **ADFS firmy Microsoft**.
    2.  Jako **Typ identyfikatora**wybierz **Adres E-mail**.
    3.  W polu tekstowym **Atrybut poczty E-mail** wpisz **emailaddress**.
    4.  W polu tekstowym **Atrybut imię** wpisz **Imię**.
    5.  W polu tekstowym **Ostatniego atrybutu nazwy** wpisz **nazwisko**.

11. Preform następujące czynności, aby zakończyć konfigurację:

    ![Tworzenie użytkownika na logowania] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Tworzenie użytkownika na logowania")

    1.  Jak **Tworzenie użytkownika na logowania**wybierz opcję **Utwórz nowego użytkownika w witrynie się zalogować**.
    2.  Jak **zalogować się ustawienia**wybierz **przycisk Użyj SAML na ekranie "Zaloguj"**.
    3.  Kliknij przycisk **Zapisz**.

12. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywanie użytkowników do oprogramowania Igloo.  
Jeśli przypisany użytkownik próbuje zalogować się do oprogramowania Igloo przy użyciu panelu programu access, oprogramowanie Igloo sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez oprogramowanie Igloo.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Aby przypisać użytkowników do oprogramowania Igloo, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja **Oprogramowania Igloo **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).