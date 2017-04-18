<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z LogicMonitor | Microsoft Azure" 
    description="Dowiedz się, jak użyć LogicMonitor z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Samouczek: Azure Active Directory Integracja z LogicMonitor
  
Celem tego samouczka jest pokazanie integracji Azure i LogicMonitor.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy LogicMonitor
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla LogicMonitor
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenariusz")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Włączanie integracji aplikacji dla LogicMonitor
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla LogicMonitor, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **logicmonitor**.

    ![Galeria aplikacji] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Galeria aplikacji")

7.  W okienku wyników wybierz **LogicMonitor**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania LogicMonitor przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **LogicMonitor **aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do LogicMonitor** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do zarejestrowania się do LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w LogicMonitor** kliknij przycisk **Pobierz metadanych**, a następnie zapisz go na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Konfigurowanie logowania jednokrotnego")

5.  Zaloguj się do witryny firmy **LogicMonitor** jako administrator.

6.  W menu u góry kliknij pozycję **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Ustawienia")

7.  Bat nawigacji po lewej stronie kliknij pozycję **Rejestracji jednokrotnej**

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Rejestracji jednokrotnej")

8.  W sekcji **Ustawienia logowania jednokrotnego (SSO)** wykonaj następujące czynności:

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Ustawienia rejestracji jednokrotnej")

    1.  Zaznacz opcję **Włącz rejestracji jednokrotnej**.
    2.  Jako **Domyślny przypisanie roli**wybierz **tylko do odczytu**.
    3.  Otwórz plik pobrany metadanych w Notatniku, a następnie wklej zawartość pliku w polu tekstowym **Metadanych dostawcy tożsamości** .
    4.  Kliknij przycisk **Zapisz zmiany**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
AAD użytkownikom będzie można się zalogować musi być przygotowana do aplikacji LogicMonitor przy użyciu nazwy użytkowników usługi Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy LogicMonitor jako administrator.

2.  W menu u góry kliknij pozycję **Ustawienia**, a następnie kliknij **role i użytkownicy**.

    ![Role i użytkownicy] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Role i użytkownicy")

3.  Kliknij przycisk **Dodaj**.

4.  W sekcji **Dodawanie konta** wykonaj następujące czynności:

    ![Dodawanie konta] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Dodawanie konta")

    1.  Wpisz **nazwę użytkownika**, **wiadomości E-mail**, **hasło** i **Wpisz ponownie hasło** wartości użytkownika usługi Azure Active Directory, do którego ma zostać należy do powiązanych pól tekstowych.
    2.  Wybierz **role**, **Wyświetlanie uprawnień** i **Stan**.
    3.  Kliknij przycisk **Prześlij**.

>[AZURE.NOTE]Można użyć dowolnego innego LogicMonitor użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez LogicMonitor do świadczenia usługi Azure Active Directory kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Aby przypisać użytkowników do LogicMonitor, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **LogicMonitor** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).




