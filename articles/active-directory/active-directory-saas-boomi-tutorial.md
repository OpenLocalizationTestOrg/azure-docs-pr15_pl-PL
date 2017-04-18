<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Boomi | Microsoft Azure" 
    description="Dowiedz się, jak użyć Boomi z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Samouczek: Azure Active Directory Integracja z Boomi

Celem tego samouczka jest pokazanie integracji Azure i Boomi.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Boomi logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Boomi będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Boomi (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Boomi
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenariusz")
##<a name="enabling-the-application-integration-for-boomi"></a>Włączanie integracji aplikacji dla Boomi

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Boomi.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Boomi, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Boomi**.

    ![Galeria aplikacji] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Boomi**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Boomi przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Boomi** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Boomi** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL odpowiedź Boomi** wpisz adres **URL logowania AtomSphere Boomi** (np.: "*https://platform.boomi.com/sso/AccountName/saml*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Boomi** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Boomi jako administrator.

6.  Na pasku narzędzi u góry kliknij swojej nazwy firmy, a następnie **konfiguracji**.

    ![Konfiguracja] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Konfiguracja")

7.  Kliknij przycisk **Opcje logowania jednokrotnego**.

    ![Opcje logowania jednokrotnego] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Opcje logowania jednokrotnego")

8.  W sekcji **Opcje rejestracji jednokrotnej** wykonaj następujące czynności:

    ![Opcje rejestracji jednokrotnej] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Opcje rejestracji jednokrotnej")

    1.  Zaznacz opcję **Włącz SAML rejestracji jednokrotnej**.
    2.  Kliknij przycisk **Importuj**przekazywania pobranego certyfikatu.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Boomi** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .
    4.  **Federacja identyfikator lokalizacji**zaznacz **identyfikator federacji jest w elemencie NameID tematu**.
    5.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Boomi, musi być przygotowana do Boomi.  
W przypadku Boomi inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Boomi** jako administrator.

2.  Przejdź do pozycji **Zarządzanie użytkownikami \> użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Użytkownicy")

3.  Kliknij pozycję **Dodaj użytkownika**.

    ![Dodawanie użytkownika] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Dodawanie użytkownika")

4.  Na stronie okno dialogowe **Dodawanie role użytkowników** wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Dodawanie użytkownika")

    1.  Wpisz **Imię**, **Nazwisko** i **poczty E-mail** działające konto AAD chcesz dodawać do powiązanych pól tekstowych.
    2.  Kliknij przycisk OK.

>[AZURE.NOTE] Można użyć dowolnego innego Boomi użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Boomi zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Aby przypisać użytkowników do Boomi, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Boomi **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
