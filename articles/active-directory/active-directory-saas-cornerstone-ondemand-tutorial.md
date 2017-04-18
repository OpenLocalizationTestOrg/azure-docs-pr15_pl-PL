<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z na żądanie węgielny | Microsoft Azure" 
    description="Dowiedz się, jak użyć węgielny na żądanie z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Samouczek: Azure Active Directory Integracja z węgielny na żądanie

Celem tego samouczka jest pokazanie integracji Azure i węgielny na żądanie.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Kamień węgielny na żądanie dzierżawy

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do węgielny na żądanie będą mogli rejestracji jednokrotnej do aplikacji na żądanie węgielny firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla węgielny na żądanie
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scenariusz")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Włączanie integracji aplikacji dla węgielny na żądanie

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla węgielny na żądanie.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla węgielny na żądanie, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **węgielny na żądanie**.

    ![Galeria aplikacji] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Węgielny na żądanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Kamień węgielny na żądanie] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Kamień węgielny na żądanie")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania na żądanie węgielny przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Na żądanie węgielny** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do węgielny na żądanie** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Microsoft Azure AD jednokrotnej] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD jednokrotnej")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Węgielny na żądanie adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*http://company.csod.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w węgielny na żądanie** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie o poniższych elementów na żądanie węgielny zespołem pomocy technicznej:

    1.  Pobranego certyfikatu
    2.  Wartość **Zdalny adres URL logowania**
    3.  Wartość **Adres URL usługi zdalnej Wyloguj** .

    >[AZURE.NOTE] Logowania jednokrotnego musi być skonfigurowany przez zespół pomocy technicznej węgielny na żądanie.
Otrzymasz powiadomienie z zespołem pomocy technicznej po zakończeniu konfiguracji.

6.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do węgielny na żądanie, musi być przygotowana do węgielny na żądanie.  
W przypadku węgielny na żądanie inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Wysyłanie informacji (np.: nazwa, Emial) o użytkowniku Azure AD chcesz zapewnienie na żądanie węgielny zespołem pomocy technicznej.

>[AZURE.NOTE] Można użyć dowolnego innego węgielny na żądanie użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez na żądanie węgielny zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Aby przypisać użytkowników do węgielny na żądanie, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Na żądanie węgielny **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Przypisywanie użytkowników")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
