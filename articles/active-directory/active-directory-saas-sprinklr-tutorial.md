<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Sprinklr | Microsoft Azure" 
    description="Dowiedz się, jak użyć Sprinklr z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Samouczek: Azure Active Directory Integracja z Sprinklr
  
Celem tego samouczka jest pokazanie integracji Azure i Sprinklr.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Sprinklr
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Sprinklr będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Sprinklr (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Sprinklr
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scenariusz")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Włączanie integracji aplikacji dla Sprinklr
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Sprinklr.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Sprinklr, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Sprinklr**.

    ![Galeria aplikacji] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Sprinklr**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Sprinklr przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Sprinklr** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Sprinklr** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Sprinklr adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. sprinklr.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Sprinklr** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Sprinklr jako administrator.

6.  Przejdź do pozycji **Administracja \> ustawienia**.

    ![Administracja] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administracja")

7.  Przejdź do pozycji **Zarządzanie partnera \> rejestracji jednokrotnej** na w okienku po lewej stronie.

    ![Zarządzanie partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Zarządzanie partnera")

8.  Kliknij przycisk **+ Dodatki rejestracji jednokrotnej**.

    ![Pojedynczy znak dodatków] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Pojedynczy znak dodatków")

9.  Na stronie **Logowaniu jednokrotnym** wykonaj następujące czynności:

    ![Pojedynczy znak dodatków] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Pojedynczy znak dodatków")

    1.  W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji (przykład: *WAADSSOTest*).
    2.  Wybierz pozycję **włączone**.
    3.  Wybierz pozycję **Użyj nowego certyfikatu logowania jednokrotnego**.
    4.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **Certyfikatu dostawcy tożsamości**
    6.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Sprinklr** skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **Identyfikator jednostki** .
    7.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Sprinklr** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .
    8.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Sprinklr** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości** .
    9.  Jako **Typ identyfikator użytkownika SAML**, wybierz pozycję **potwierdzenia zawiera użytkownika "nazwa_użytkownika sprinklr.com s**.
    10. Jako **Lokalizacji identyfikator użytkownika SAML**wybierz **identyfikator użytkownika w elemencie nazwę instrukcji tematu**.
    11. Zamknij **Zapisywanie**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
AAD użytkownikom będzie można się zalogować musi być przygotowana dostępu w aplikacji Sprinklr.  
W tej sekcji opisano sposób tworzenia kont użytkowników AAD wewnątrz Sprinklr.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Aby obsługi administracyjnej konta użytkownika w Sprinklr, należy wykonać następujące czynności:

1.  Zaloguj się do witryny firmy Sprinklr jako administrator.

2.  Przejdź do pozycji **Administracja \> ustawienia**.

    ![Administracja] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administracja")

3.  Przejdź do pozycji **Zarządzanie klienta \> użytkowników** w lewym okienku.

    ![Ustawienia] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Ustawienia")

4.  Kliknij pozycję **Dodaj użytkownika**.

    ![Ustawienia] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Ustawienia")

5.  W oknie dialogowym **Edytowanie użytkownika** wykonaj następujące czynności:

    ![Edytowanie użytkownika] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Edytowanie użytkownika")

    1.  W **wiadomości E-mail**, **Imię** i **Nazwisko** pola tekstowe, wpisz dane konto użytkownika usługi Azure AD chcesz świadczenia.
    2.  Wybierz pozycję **wyłączone hasło**.
    3.  Wybierz **język**.
    4.  Wybierz **Typ użytkownika**.
    5.  Kliknij przycisk **Aktualizuj**.

    >[AZURE.IMPORTANT] Aby umożliwić użytkownikowi zalogować się za pośrednictwem dostawcy tożsamości musi być zaznaczone, co **Hasło wyłączony** .

6.  Przejdź do **roli**, a następnie wykonaj następujące czynności:

    ![Role partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Role partnera")

    1.  Wybierz z listy **Global** **wszystkie\_uprawnienia**.
    2.  Kliknij przycisk **Aktualizuj**.

>[AZURE.NOTE] Można użyć dowolnego innego Sprinklr użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez Sprinklr do zapewniania obsługi Azure AD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Aby przypisać użytkowników do Sprinklr, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Sprinklr **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).