<properties 
    pageTitle="Samouczek: Azure Active Directory integracji z oprogramowaniem zebranie | Microsoft Azure" 
    description="Dowiedz się, jak użyć Rally oprogramowania z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Samouczek: Azure Active Directory integracji z oprogramowaniem zebranie
  
Celem tego samouczka jest pokazanie integracja Azure i Rally oprogramowania.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Rally oprogramowania
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji Rally oprogramowania
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenariusz")
##<a name="enabling-the-application-integration-for-rally-software"></a>Włączanie integracji aplikacji Rally oprogramowania
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji Rally oprogramowania.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla oprogramowania Rally, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **zebranie**.

    ![Galeria aplikacji] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Rally oprogramowanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Rally oprogramowania] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally oprogramowania")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML Rally oprogramowania przy użyciu swojego konta.  
W ramach tej procedury musisz przekazać certyfikatu do Rally oprogramowania.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Rally oprogramowania **aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak czy chcesz użytkownikom logowanie do zebranie** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Microsoft Azure AD jednokrotnej] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD jednokrotnej")

3.  Na stronie **Skonfigurować adres URL aplikacji** w polu tekstowym **Rally oprogramowania dzierżawy URL** wpisz adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. rally.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w zebranie** kliknij metadanych pobierania, a następnie zapisz go na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Konfigurowanie logowania jednokrotnego")

5.  Zaloguj się do Twojej dzierżawy **Rally oprogramowania** .

6.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**, a następnie wybierz **subskrypcję**.

    ![Subskrypcji] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Subskrypcji")

7.  Kliknij przycisk **akcji** na pasku narzędzi u góry po prawej stronie, a następnie wybierz pozycję **Edytuj subskrypcji**.

8.  Na stronie okno dialogowe **subskrypcji** wykonaj następujące czynności, a następnie kliknij przycisk **Zapisz i zamknij**:

    ![Uwierzytelnianie] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Uwierzytelnianie")

    1.  Z listy rozwijanej uwierzytelniania wybierz **Uwierzytelnianie zebranie lub logowania jednokrotnego**
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Rally oprogramowania** skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **Adres URL dostawcy tożsamości**
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Rally oprogramowania** skopiuj wartość **Adres URL usługi zdalnej Wyloguj** .

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
AAD użytkownikom będzie można się zalogować musi być przygotowana do Rally aplikacji przy użyciu nazwy użytkowników usługi Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy Rally oprogramowania.

2.  Przejdź do pozycji **konfiguracji \> użytkowników**, a następnie kliknij przycisk **+ Dodaj nowy**.

    ![Użytkownicy] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Użytkownicy")

3.  Wpisz nazwę w polu tekstowym nowego użytkownika, a następnie kliknij przycisk **Dodaj szczegóły**.

4.  W sekcji **Tworzenie użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Tworzenie użytkownika")

    1.  W polu tekstowym **Nazwa użytkownika** wpisz nazwę użytkownika Azure AD, potrzebne do świadczenia.
    2.  W polu tekstowym **Adres E-mail** wpisz adres e-mail użytkownika Azure AD, którego chcesz świadczenia.
    3.  Kliknij przycisk **Zapisz i zamknij**.

>[AZURE.NOTE]Można użyć dowolnego innego oprogramowania Rally użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez oprogramowanie Rally zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Aby przypisać użytkowników do Rally oprogramowania, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja **Oprogramowania Rally** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).




