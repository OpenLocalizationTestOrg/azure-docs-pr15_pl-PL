<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z FM: systemy | Microsoft Azure" 
    description="Dowiedz się, jak używać FM: systemy z usługi Azure Active Directory, aby włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Samouczek: Azure Active Directory Integracja z FM: systemy
  
Celem tego samouczka jest pokazanie integracji Azure i FM:Systems.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   FM:Systems logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do FM:Systems będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy FM:Systems (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla FM:Systems
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scenariusz")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Włączanie integracji aplikacji dla FM:Systems
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla FM:Systems.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla FM:Systems, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **FM:Systems**.

    ![Galeria aplikacji] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Galeria aplikacji")

7.  W okienku wyników wybierz **FM:Systems**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![FM: systemy] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: systemy")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania FM:Systems przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **FM:Systems** integracja aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do FM:Systems** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **FM:Systems logowania na adres URL** wpisz adres FM:Systems **Adres URL odpowiedzi** (przykład: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Możesz wyświetlić tę wartość z Twojej FM: systemy zespołem pomocy technicznej.

    2.  Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w FM:Systems** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie Zapisz metadane na Twoim komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Konfigurowanie logowania jednokrotnego")

5.  Przesyłanie pliku pobranego metadanych do swojego FM: systemy zespołem pomocy technicznej.

    >[AZURE.NOTE] Usługi FM: Zespołu pomocy technicznej ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do FM:Systems, musi być przygotowana do FM:Systems.  
W przypadku FM:Systems inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  W oknie przeglądarki sieci web Zaloguj się do witryny firmy FM:Systems jako administrator.

2.  Przejdź do pozycji **Administracja \> Zarządzaj zabezpieczeniami \> użytkowników \> listy użytkowników**.

    ![Administracja] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Administracja")

3.  Kliknij przycisk **Utwórz nowego użytkownika**.

    ![Utwórz nowego użytkownika] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Utwórz nowego użytkownika")

4.  W sekcji **Tworzenie użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Tworzenie użytkownika")

    1.  Wpisz nazwę użytkownika, hasło i potwierdzenie, adres e-mail i identyfikator pracownika prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Dalej**.

>[AZURE.NOTE] Można użyć dowolnego innego FM:Systems użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez FM:Systems do kont użytkownika AAD.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Aby przypisać użytkowników do FM:Systems, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **FM:Systems **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).