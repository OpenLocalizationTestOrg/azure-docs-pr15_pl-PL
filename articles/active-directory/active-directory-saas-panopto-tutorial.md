<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Panopto | Microsoft Azure" 
    description="Dowiedz się, jak użyć Panopto z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Samouczek: Azure Active Directory Integracja z Panopto
  
Celem tego samouczka jest pokazanie integracja Azure i Panopto.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Panopto
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Panopto będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Panopto (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Panopto
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenariusz")
##<a name="enabling-the-application-integration-for-panopto"></a>Włączanie integracji aplikacji dla Panopto
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Panopto, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Panopto**.

    ![Galeria Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Galeria Appkication")

7.  W okienku wyników wybierz **Panopto**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Panopto przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Panopto** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Panopto** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Panopto adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Panopto.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Panopto** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Panopto jako administrator.

6.  Na pasku narzędzi po lewej stronie kliknij pozycję **System**, a następnie kliknij **Dostawcy tożsamości**.

    ![System] (./media/active-directory-saas-panopto-tutorial/IC777670.png "System")

7.  Kliknij przycisk **Dodaj dostawcy**.

    ![Dostawcy tożsamości] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Dostawcy tożsamości")

8.  W sekcji dostawcy SAML wykonaj następujące czynności:

    ![Konfiguracja władz akredytacji bezpieczeństwa] (./media/active-directory-saas-panopto-tutorial/IC777672.png "Konfiguracja władz akredytacji bezpieczeństwa")

    1.  Na liście **Typ dostawcy** zaznacz **SAML20**
    2.  W polu tekstowym **Nazwa wystąpienia** wpisz nazwę dla tego wystąpienia.
    3.  W polu tekstowym **Przyjazny opis** wpisz opis przyjazny.
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Panopto** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Panopto** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres Url strony pojawia się z podskokiem** .
    6.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    7.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **Klucz publiczny**
    8.  Kliknij przycisk **Zapisz**.
        ![Zapisywanie] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Zapisywanie")

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do Panopto użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do Panopto przy użyciu panelu programu access, Panopto sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez Panopto.

>[AZURE.NOTE]Można użyć dowolnego innego Panopto użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez Panopto do zapewniania obsługi Azure AD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Aby przypisać użytkowników do Panopto, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Panopto **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).