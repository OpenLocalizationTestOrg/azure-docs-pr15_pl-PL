<properties 
    pageTitle="Samouczek: Integracja Azure Active Directory ze wskazówkami Microsoft | Microsoft Azure" 
    description="Dowiedz się, jak włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych elementów za pomocą wskazówek dotyczących dojazdu na programie Microsoft z usługą Azure Active Directory!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Samouczek: Integracja Azure Active Directory ze wskazówkami firmy Microsoft

Celem tego samouczka jest pokazanie integracji usługi Azure Active Directory i wskazówek dotyczących dojazdu na firmy Microsoft.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Instrukcje dotyczące subskrypcji Microsoft

Jeśli nie masz federacyjnych wskazówek dotyczących dojazdu w subskrypcji Microsoft jeszcze poczty e-mail żądanie "*service@DirectionsOnMicrosoft.com*".

Ten samouczek użytkowników usługi Azure Active Directory, dla których zostały przypisane do wskazówek dotyczących dojazdu na programie Microsoft będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu logowania jednokrotnego.

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji, aby uzyskać instrukcje dotyczące programu Microsoft
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scenariusz")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Włączanie integracji aplikacji, aby uzyskać instrukcje dotyczące programu Microsoft

Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji uzyskać instrukcje dotyczące firmy Microsoft.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Aby włączyć integracji aplikacji, aby uzyskać instrukcje dotyczące programu Microsoft, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **instrukcje dotyczące firmy Microsoft**.

    ![Galeria aplikacji] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galeria aplikacji")

7.  W okienku wyników wybierz **instrukcje dotyczące firmy Microsoft**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Scenariusz] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scenariusz")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania instrukcje dotyczące firmy Microsoft przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Wskazówki dojazdu na programie Microsoft** integracji aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do wskazówek dotyczących dojazdu na programie Microsoft** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Microsoft Azure AD Singel logowania jednokrotnego] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym logowania na adres URL wpisz **https://www.directionsonmicrosoft.com/user/login**, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w kierunkach Microsoft** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku metadanych do wskazówek dotyczących dojazdu na zespół pomocy technicznej firmy Microsoft (*service@DirectionsOnMicrosoft.com*). Aby włączyć instrukcje dotyczące zespołu pomocy technicznej firmy Microsoft zlokalizować uczestnictwo federacyjnych witryny, należy uwzględnić informacji o firmie w wiadomości e-mail.

    >[AZURE.NOTE] Rejestracji jednokrotnej dla kierunków Microsoft musi być włączona przez instrukcje dotyczące zespołu pomocy technicznej firmy Microsoft.
Otrzymasz powiadomienie podczas logowania jednokrotnego został włączony.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Nie ma żadnych czynności do wykonania do konfiguracji użytkownika inicjowania obsługi administracyjnej do wskazówek dotyczących dojazdu na programie Microsoft.  
Jeśli przypisany użytkownik próbuje zalogować się do wskazówek dotyczących dojazdu na firmy Microsoft przy użyciu panelu programu access, instrukcje dotyczące programu Microsoft sprawdza, czy użytkownik istnieje. Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez instrukcje dotyczące firmy Microsoft.
##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Aby przypisać użytkowników do wskazówek dotyczących dojazdu na firmy Microsoft, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **instrukcje dotyczące Microsoft **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Tak")
