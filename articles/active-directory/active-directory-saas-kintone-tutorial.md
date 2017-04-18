<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Kintone | Microsoft Azure" 
    description="Dowiedz się, jak użyć Kintone z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Samouczek: Integracja usługi Azure Active Directory z Kintone
  
Celem tego samouczka jest pokazanie integracji Azure i Kintone.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Kintone logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Kintone będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy Kintone (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Kintone
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenariusz")
##<a name="enabling-the-application-integration-for-kintone"></a>Włączanie integracji aplikacji dla Kintone
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Kintone.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Kintone, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Kintone**.

    ![Galeria aplikacji] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Kintone**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Kintone przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Kintone** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Kintone** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Kintone logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://company.kintone.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Kintone** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **Kintone** jako administrator.

6.  Kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Ustawienia")

7.  Kliknij pozycję **Użytkownicy i Administracja systemu**.

    ![Użytkownicy i Administracja] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Użytkownicy i Administracja")

8.  W obszarze **Administracja \> zabezpieczeń** kliknij przycisk **Zaloguj**.

    ![Logowanie] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Logowanie")

9.  Kliknij pozycję **Włącz SAML uwierzytelniania**.

    ![Uwierzytelnianie SAML] (./media/active-directory-saas-kintone-tutorial/IC785882.png "Uwierzytelnianie SAML")

10. W sekcji uwierzytelnianie SAML wykonaj następujące czynności:

    ![Uwierzytelnianie SAML] (./media/active-directory-saas-kintone-tutorial/IC785883.png "Uwierzytelnianie SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Kintone** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Kintone** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    3.  Kliknij przycisk **Przeglądaj,** Aby przekazać pobrany certyfikatu.
    4.  Kliknij przycisk **Zapisz**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Kintone, musi być przygotowana do Kintone.  
W przypadku Kintone inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Kintone** jako administrator.

2.  Kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Ustawienia")

3.  Kliknij pozycję **Użytkownicy i Administracja systemu**.

    ![Administracja & użytkownika] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Administracja & użytkownika")

4.  W obszarze **Administracja użytkownika**kliknij przycisk **działy i użytkowników**.

    ![Dział i użytkowników] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Dział i użytkowników")

5.  Kliknij pozycję **Nowy użytkownik**.

    ![Nowych użytkowników] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Nowych użytkowników")

6.  W sekcji **Nowego użytkownika** wykonaj następujące czynności:

    ![Nowych użytkowników] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Nowych użytkowników")

    1.  Wpisz **Nazwę wyświetlaną**, **Nazwę logowania**, **Nowe hasło**, **Potwierdź hasło**, **Adres E-mail** i inne szczegóły prawidłowe konta AAD, które chcesz należy do powiązanych texboxes.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego Kintone użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Kintone zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Aby przypisać użytkowników do Kintone, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Kintone **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).