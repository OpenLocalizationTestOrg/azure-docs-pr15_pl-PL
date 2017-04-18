<properties 
    pageTitle="Samouczek: Azure Active Directory integracji z oprogramowaniem OfficeSpace | Microsoft Azure" 
    description="Dowiedz się, jak użyć OfficeSpace oprogramowania z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Samouczek: Azure Active Directory integracji z oprogramowaniem OfficeSpace
  
Celem tego samouczka jest pokazanie integracji Azure i OfficeSpace oprogramowania.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Oprogramowanie OfficeSpace logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do oprogramowania OfficeSpace będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny oprogramowania OfficeSpace (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla oprogramowania OfficeSpace
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scenariusz")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Włączanie integracji aplikacji dla oprogramowania OfficeSpace
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla oprogramowania OfficeSpace.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla oprogramowania OfficeSpace, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **OfficeSpace oprogramowania**.

    ![Galeria aplikacji] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Oprogramowanie OfficeSpace**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Oprogramowanie OfficeSpace] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "Oprogramowanie OfficeSpace")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML oprogramowania OfficeSpace przy użyciu swojego konta.  
Konfigurowanie rejestracji jednokrotnej dla oprogramowania OfficeSpace wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracja **Oprogramowania OfficeSpace** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie rejestracji jednokrotnej = na] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Konfigurowanie rejestracji jednokrotnej = na")

2.  Na stronie **jak chcesz użytkownikom logowanie do oprogramowania OfficeSpace** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **OfficeSpace oprogramowania logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji oprogramowania OfficeSpace (np.: "*https://company.officespacesoftware.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w OfficeSpace oprogramowania** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy OfficeSpace oprogramowania jako administrator.

6.  Przejdź do pozycji **Administrator \> łączników**.

    ![Administrator] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Administrator")

7.  Kliknij pozycję **autoryzacji SAML**.

    ![Łączniki] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Łączniki")

8.  W sekcji **Autoryzacji SAML** wykonaj następujące czynności:

    ![Konfiguracja SAML] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "Konfiguracja SAML")

    1.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w oprogramowania OfficeSpace** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **adres url usługodawcy Wyloguj** .
    2.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w oprogramowania OfficeSpace** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **klienta protokołu idp docelowy adres url** .
    3.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **odcisku palca certyfikatu protokołu idp klienta** .  

        >[AZURE.TIP]
        Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    4.  Kliknij przycisk **Zapisz ustawienia**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do oprogramowania OfficeSpace, musi być przygotowana do oprogramowania OfficeSpace. W przypadku oprogramowania OfficeSpace inicjowania obsługi administracyjnej to zadanie automatyczne.  
Nie ma żadnych czynności do wykonania dla Ciebie.  
Użytkownicy są tworzone automatycznie w razie potrzeby podczas pierwszej pojedynczego logowania jednokrotnego próby.

>[AZURE.NOTE]Można użyć dowolnego innego oprogramowania OfficeSpace użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez oprogramowanie OfficeSpace zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Aby przypisać użytkowników do oprogramowania OfficeSpace, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja **Oprogramowania OfficeSpace **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).