<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z cieplarnianych | Microsoft Azure" 
    description="Dowiedz się, jak użyć cieplarnianych z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Samouczek: Azure Active Directory Integracja z cieplarnianych
  
Celem tego samouczka jest pokazanie integracji Azure i cieplarnianych.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Cieplarnianych pojedynczego logowania jednokrotnego subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do cieplarnianych będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny cieplarnianych (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla cieplarnianych
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenariusz")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Włączanie integracji aplikacji dla cieplarnianych
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla cieplarnianych.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla cieplarnianych, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **cieplarnianych**.

    ![Galeria aplikacji] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Galeria aplikacji")

7.  W okienku wyników wybierz **cieplarnianych**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Cieplarnianych] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Cieplarnianych")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML cieplarnianych przy użyciu swojego konta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **cieplarnianych** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do cieplarnianych** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://company.greenhouse.io*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w cieplarnianych** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Konfigurowanie logowania jednokrotnego")

5.  Przesyłanie dalej tego pliku metadanych do zespołu pomocy technicznej cieplarnianych.

    >[AZURE.NOTE] Rejestracji jednokrotnej musi być aktywne przez zespół pomocy technicznej cieplarnianych.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do cieplarnianych, musi być przygotowana do cieplarnianych.  
W przypadku szklarni inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **cieplarnianych** jako administrator.

2.  W menu u góry kliknij przycisk **Konfiguruj**, a następnie kliknij **użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Użytkownicy")

3.  Kliknij pozycję **nowych użytkowników**.

    ![Nowy użytkownik] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Nowy użytkownik")

4.  W sekcji **Dodawanie nowego użytkownika** wykonaj następujące czynności:

    ![Dodawanie nowego użytkownika] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Dodawanie nowego użytkownika")

    1.  W polu tekstowym **Wprowadź użytkownika w wiadomości e-mail** wpisz adres e-mail prawidłowe konta usługi Azure Active Directory, które chcesz świadczenia.
    2.  Kliknij przycisk **Zapisz**.
        
        >[AZURE.NOTE] Posiadacze kont usługi Azure Active Directory otrzymają wiadomość e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego cieplarnianych użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez cieplarnianych zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Aby przypisać użytkowników do cieplarnianych, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **cieplarnianych **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).