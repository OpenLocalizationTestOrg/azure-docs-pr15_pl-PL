<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z PolicyStat | Microsoft Azure" 
    description="Dowiedz się, jak użyć PolicyStat z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Samouczek: Integracja usługi Azure Active Directory z PolicyStat
  
Celem tego samouczka jest pokazanie integracji Azure i PolicyStat.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy PolicyStat
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do PolicyStat będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy PolicyStat (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla PolicyStat
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenariusz")
##<a name="enabling-the-application-integration-for-policystat"></a>Włączanie integracji aplikacji dla PolicyStat
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla PolicyStat.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla PolicyStat, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **PolicyStat**.

    ![Galeria aplikacji] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Galeria aplikacji")

7.  W okienku wyników wybierz **PolicyStat**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania PolicyStat przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Aplikacja PolicyStat oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Atrybuty")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **PolicyStat** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do PolicyStat** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** w polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji PolicyStat adres URL (np.: *"https://demo-azure.policystat.com"*), a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie ustawień aplikacji] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Konfigurowanie ustawień aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w PolicyStat** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy PolicyStat jako administrator.

6.  Kliknij kartę **Administrator** , a następnie kliknij **Konfiguracji rejestracji jednokrotnej** w okienku nawigacji po lewej stronie.

    ![Administrator Menu] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Administrator Menu")

7.  W sekcji **Ustawienia** wybierz pozycję **Włącz pojedynczego logowania jednokrotnego integracji**.

    ![Konfiguracja rejestracji jednokrotnej] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Konfiguracja rejestracji jednokrotnej")

8.  Kliknij pozycję **Konfiguracja atrybutów**, a następnie w sekcji **Konfiguracja atrybutów** , wykonaj następujące czynności:

    ![Konfiguracji rejestracji jednokrotnej] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguracja rejestracji jednokrotnej")

    1.  W polu tekstowym **Nazwa użytkownika atrybut** wpisz **uid**.
    2.  W polu tekstowym **Atrybut imię** wpisz **Imię**.
    3.  W polu tekstowym **Ostatniego atrybutu nazwy** wpisz **nazwisko**.
    4.  W polu tekstowym **Atrybut poczty E-mail** wpisz **emailaddress**.
    5.  Kliknij przycisk **Zapisz zmiany**.

9.  Kliknij **Swój protokołu IDP metadanych**, a następnie w sekcji **I protokołu IDP metadanych** , wykonaj następujące czynności:

    ![Konfiguracja rejestracji jednokrotnej] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguracja rejestracji jednokrotnej")

    1.  Otwórz plik pobrany metadanych, skopiuj zawartość i następnie wklej go w polu tekstowym **I metadanych dostawcy tożsamości**
    2.  Kliknij przycisk **Zapisz zmiany**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Konfigurowanie logowania jednokrotnego")

11. 12. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Atrybuty")

13. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

    ![Atrybuty] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Atrybuty")

    1.  Kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz **uid**.
    3.  W polu tekstowym **Wartość atrybutu** wybierz pozycję **ExtractMailPrefix()**.
    4.  Na liście **Poczta** zaznacz **User.mail**.
    5.  Kliknij pozycję **pełne**.
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do PolicyStat, musi być przygotowana do PolicyStat.  
PolicyStat obsługuje tylko na czas użytkownika inicjowania obsługi administracyjnej. Oznacza to, nie trzeba ręcznie dodać użytkowników do PolicyStat.  
Użytkownicy będą zostać dodane automatycznie podczas jego pierwszego logowania za pośrednictwem rejestracji jednokrotnej na.

>[AZURE.NOTE]Można użyć dowolnego innego PolicyStat użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez PolicyStat zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Aby przypisać użytkowników do PolicyStat, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **PolicyStat **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).