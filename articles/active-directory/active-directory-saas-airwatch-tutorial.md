<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z AirWatch | Microsoft Azure" 
    description="Dowiedz się, jak użyć AirWatch z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Samouczek: Azure Active Directory Integracja z AirWatch

Celem tego samouczka jest pokazanie integracji Azure i AirWatch.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   AirWatch logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do AirWatch będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy AirWatch (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla AirWatch
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Włączanie integracji aplikacji dla AirWatch

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla AirWatch.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla AirWatch, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **AirWatch**.

    ![Galeria aplikacji] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Galeria aplikacji")

7.  W okienku wyników wybierz **AirWatch**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania AirWatch przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **AirWatch** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do AirWatch** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **AirWatch logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do aplikacji AirWatch (np.: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w AirWatch** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy AirWatch jako administrator.

6.  W okienku nawigacji po lewej stronie kliknij pozycję **konta**, a następnie kliknij **administratorów**.

    ![Administratorzy] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administratorzy")

7.  Rozwinięcie menu **Ustawienia** , a następnie kliknij **Usługami katalogowymi**.

    ![Ustawienia] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Ustawienia")

8.  Kliknij kartę **użytkownika** w pole **Base nazwy domeny** , wpisz nazwę domeny, a następnie kliknij przycisk **Zapisz**.

    ![Użytkownika] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Użytkownika")

9.  Kliknij kartę **serwer** .

    ![Serwer] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Serwer")

10. Wykonaj następujące czynności:

    ![Przekazywanie] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Przekazywanie")

    1.  Jako **Typ katalogu**wybierz opcję **Brak**.
    2.  Zaznacz pole wyboru **Użyj SAML uwierzytelniania**.
    3.  Aby przekazać pobranego certyfikatu, kliknij przycisk **Przekaż**.

11. W sekcji **żądanie** wykonaj następujące czynności:

    ![Żądanie] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Żądanie")

    1.  **Żądanie typu oprawa**zaznacz **WPIS**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Airwatch** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Tożsamości dostawcy pojedynczy znak na adres URL** .
    3.  **NameID Format**wybierz **Adres E-mail**.
    4.  Kliknij przycisk **Zapisz**.

12. Kliknij ponownie kartę **użytkownika** .

    ![Użytkownika] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Użytkownika")

13. W sekcji **atrybut** wykonaj następujące czynności:

    ![Atrybut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Atrybut")

    1.  W polu tekstowym **Identyfikator obiektu** wpisz **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  W polu tekstowym **Nazwa użytkownika** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  W polu tekstowym **Nazwa wyświetlana** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  W polu tekstowym **Imię** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  W polu tekstowym **Nazwa** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  W polu tekstowym **wiadomości E-mail** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Kliknij przycisk **Zapisz**.

14. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do AirWatch, musi być przygotowana do AirWatch.  
W przypadku AirWatch inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **AirWatch** jako administrator.

2.  W okienku nawigacji po lewej stronie kliknij pozycję **konta**, a następnie kliknij pozycję **Użytkownicy**.

    ![Użytkownicy] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Użytkownicy")

3.  W menu **Użytkownicy** kliknij opcję **Widok listy**, a następnie kliknij **Dodaj \> Dodaj użytkownika**.

    ![Dodawanie użytkownika] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Dodawanie użytkownika")

4.  W oknie dialogowym **Dodawanie / Edytowanie użytkownika** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Dodawanie użytkownika")

    1.  Wpisz **nazwę użytkownika**, **hasło**, **Potwierdź hasło**, **Imię**, **Nazwisko**, **Adres E-mail** prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego AirWatch użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez AirWatch zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Aby przypisać użytkowników do AirWatch, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **AirWatch **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
