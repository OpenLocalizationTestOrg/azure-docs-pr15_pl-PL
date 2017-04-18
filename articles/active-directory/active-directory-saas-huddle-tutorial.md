<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z zbijają się | Microsoft Azure" 
    description="Dowiedz się, jak używać zbijają się z usługi Azure Active Directory umożliwiające rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Samouczek: Azure Active Directory Integracja z zbijają się
  
Celem tego samouczka jest pokazanie integracji Azure i zbijają się.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Zbijają się pojedynczego włączone logowanie subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do zbijają się będą mogli pojedynczej Zaloguj się do aplikacji w zbijają się w witrynie firmy (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla zbijają się
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Konfigurowanie logowania jednokrotnego")
##<a name="enabling-the-application-integration-for-huddle"></a>Włączanie integracji aplikacji dla zbijają się
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla zbijają się.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla zbijają się, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **zbijają się**.

    ![Galeria aplikacji] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Galeria aplikacji")

7.  W okienku wyników wybierz **zbijają się**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Zbijają się] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Zbijają się")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania zbijają się przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **zbijają się** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak czy chcesz użytkownikom logowanie do zbijają się** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Zbijają się logowania na adres URL** wpisz adres URL z dzierżawcą zbijają się przy użyciu następującego wzorca "*http://company.huddle.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w zbijają się** wykonywać następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Konfigurowanie logowania jednokrotnego")

    1.  Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.
    2.  Skopiuj wartość **Adres URL wystawcy** , wartość **Adres URL logowania jednokrotnego SAML** i pobranego certyfikatu, a następnie wysłać je do zespołu pomocy technicznej zbijają się.

    >[AZURE.NOTE] Rejestracji jednokrotnej musi być włączona przez zespół pomocy technicznej zbijają się.
Po zakończeniu konfiguracji, otrzymasz powiadomienie.

5.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do zbijają się, musi być przygotowana do zbijają się.  
W przypadku zbijają się inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **zbijają się** jako administrator.

2.  Kliknij **obszar roboczy**.

3.  Kliknij pozycję **osób \> Zaproś osoby**.

    ![Osoby] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Osoby")

4.  W sekcji **Tworzenie nowego zaproszenia** wykonaj następujące czynności:

    ![Nowe zaproszenie] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Nowe zaproszenie")

    1.  Na liście **Wybierz zespołowi zapraszanie osób do** wybierz **zespołu**.
    2.  Wpisz **Adres E-mail** prawidłowych konta AAD, które ma zostać należy w polu tekstowym pokrewnych.
    3.  Kliknij przycisk **Zaproś**.

    >[AZURE.NOTE] Właściciel konta Azure AD otrzymają wiadomość e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego zbijają się użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez zbijają się dostarczaniem AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Aby przypisać użytkowników do zbijają się, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **zbijają się **integracji aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).