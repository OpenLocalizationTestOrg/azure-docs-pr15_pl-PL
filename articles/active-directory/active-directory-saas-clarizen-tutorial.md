<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Clarizen | Microsoft Azure" 
    description="Dowiedz się, jak użyć Clarizen z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Samouczek: Azure Active Directory Integracja z Clarizen

Celem tego samouczka jest pokazanie integracji Azure i Clarizen.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Clarizen logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Clarizen będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Clarizen (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Clarizen
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenariusz")
##<a name="enabling-the-application-integration-for-clarizen"></a>Włączanie integracji aplikacji dla Clarizen

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Clarizen.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Clarizen, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Clarizen**.

    ![Galeria aplikacji] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Clarizen**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Clarizen przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Clarizen** integracja aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Clarizen** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie logowania jednokrotnego w Clarizen** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Konfigurowanie logowania jednokrotnego")

4.  W oknie przeglądarki innej witryny sieci web, zaloguj się do witryny firmy **Clarizen** jako administrator (przykład: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Kliknij swoją nazwę użytkownika, a następnie kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Ustawienia")

6.  Kliknij kartę **Ustawienia globalne** , a następnie obok pozycji **Federacji uwierzytelniania**, kliknij przycisk **Edytuj**.

    ![Ustawienia globalne] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Ustawienia globalne")

7.  W oknie dialogowym **Federacji uwierzytelniania** wykonaj następujące czynności:

    ![Federacji uwierzytelniania] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federacji uwierzytelniania")

    1.  Kliknij przycisk **Przekaż** przekazywania pobranego certyfikatu.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Clarizen** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **adres URL logowania** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie pojedynczej wyrejestrowywania u Clarizen** skopiuj wartość **Adres URL usługi pojedynczej Sign-Out** , a następnie wklej go w polu tekstowym **Adres URL Sign-out** .
    4.  Zaznacz pole wyboru **Użyj wpisu**.
    5.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Clarizen, musi być przygotowana do Clarizen.  
W przypadku Clarizen inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Clarizen** jako administrator.

2.  Kliknij pozycję **Kontakty**.

    ![Osoby] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Osoby")

3.  Kliknij polecenie **Zaproś użytkownika**.

    ![Zapraszanie użytkowników] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Zapraszanie użytkowników")

4.  Na stronie okno dialogowe Zaproś osoby wykonaj następujące czynności:

    ![Zaproś osoby] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Zaproś osoby")

    1.  W polu tekstowym **Adres E-mail** wpisz adres e-mail prawidłowe konta usługi Azure Active Directory, które chcesz świadczenia.
    2.  Kliknij przycisk **Zaproś**.

    >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymasz wiadomość e-mail i skorzystać z łącza, aby potwierdzić swoje konto, zanim stanie się aktywny.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Aby przypisać użytkowników do Clarizen, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Clarizen **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
