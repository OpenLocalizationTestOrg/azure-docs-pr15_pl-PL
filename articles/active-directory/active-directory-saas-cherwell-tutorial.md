<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Cherwell | Microsoft Azure" 
    description="Dowiedz się, jak użyć Cherwell z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Samouczek: Azure Active Directory Integracja z Cherwell

Celem tego samouczka jest pokazanie integracji Azure i Cherwell. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Cherwell logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Cherwell będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Cherwell (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Cherwell
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Scenariusz")
##<a name="enabling-the-application-integration-for-cherwell"></a>Włączanie integracji aplikacji dla Cherwell

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Cherwell.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Cherwell, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  W okienku wyników wybierz **Cherwell**, a następnie kliknij **Wykonano** w celu dodania aplikacji.
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Cherwell przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Cherwell** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Cherwell** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Skonfigurować adres URL aplikacji")

    .  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do swojego **Cherwell** (np.: *https://\<nazwy firmy\>.cherwellondemand.com/cherwellclient*).

    b.  Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Cherwell** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Konfigurowanie logowania jednokrotnego")

    .  Kliknij przycisk **Pobierz certyfikat**, a następnie Zapisz certyfikat lokalnie na komputerze.

    b.  Skopiuj adres **URL dostawcy tożsamości**.

    c.  Skopiuj **adres URL usługi rejestracji jednokrotnej**.

    d.  Kliknij przycisk **Dalej**.

5.  Przesyłanie pobranego certyfikatu, **Adres URL dostawcy tożsamości** i **Pojedynczy znak na adres URL usługi** do zespołu pomocy technicznej Cherwell.

    >[AZURE.NOTE] Zespół pomocy technicznej Cherwell ma wykonywać rzeczywisty konfigurację logowania jednokrotnego.
Otrzymasz powiadomienie po włączeniu rejestracji Jednokrotnej dla subskrypcji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Cherwell, musi być przygotowana do Cherwell.  
W przypadku Cherwell kont użytkowników muszą zostać utworzone przez zespół pomocy technicznej Cherwell.

>[AZURE.NOTE] Można użyć dowolnego innego Cherwell użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez Cherwell do świadczenia usługi Azure Active Directory kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Aby przypisać użytkowników do Cherwell, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Cherwell** aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
