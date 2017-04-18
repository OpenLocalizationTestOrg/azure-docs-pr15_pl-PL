<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Kontiki | Microsoft Azure" 
    description="Dowiedz się, jak użyć Kontiki z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Samouczek: Azure Active Directory Integracja z Kontiki
  
Celem tego samouczka jest pokazanie integracji Azure i Kontiki.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Kontiki logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Kontiki będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Kontiki (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Kontiki
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Scenariusz")
##<a name="enabling-the-application-integration-for-kontiki"></a>Włączanie integracji aplikacji dla Kontiki
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Kontiki.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Kontiki, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Kontiki**.

    ![Galeria aplikacji] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Kontiki**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Kontiki przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Kontiki** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Konfigurowanie logować pojedynczy")

2.  Na stronie **jak chcesz użytkownikom logowanie do Kontiki** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Konfigurowanie logować pojedynczy")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Kontiki logowania na adres URL** wpisz adres URL używane przez użytkowników do zarejestrowania się do Kontiki (np.: "*https://company.mc.eval.kontiki.com/*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Kontiki** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych na komputerze.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Konfigurowanie logować pojedynczy")

5.  Wyślij metadatafile do zespołu pomocy technicznej Kontiki.

    >[AZURE.NOTE] Pojedynczy Konfiguracja logowania jednokrotnego zawiera mają być wykonywane przez zespół pomocy technicznej Kontiki. Otrzymasz powiadomienie natychmiast po zakończeniu konfiguracji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Konfigurowanie logować pojedynczy")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do Kontiki użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do Kontiki przy użyciu panelu programu access, Kontiki sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez Kontiki.
##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Aby przypisać użytkowników do Kontiki, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Kontiki **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).