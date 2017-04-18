<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Panorama9 | Microsoft Azure" 
    description="Dowiedz się, jak użyć Panorama9 z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Samouczek: Azure Active Directory Integracja z Panorama9
  
Celem tego samouczka jest pokazanie integracja Azure i Panorama9.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Panorama9 logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Panorama9 będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Panorama9 (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Panorama9
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenariusz")
##<a name="enabling-the-application-integration-for-panorama9"></a>Włączanie integracji aplikacji dla Panorama9
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Panorama9.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Panorama9, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Panorama9**.

    ![Galeria aplikacji] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Panorama9**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Panorama9 przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Panorama9 wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Panorama9** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Panorama9** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Panorama9 logowania na adres URL** wpisz adres URL używane przez użytkownika do zalogowania się do Panorama9 (np.: "*https://dashboard.panorama9.com/saml/access/3262*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Panorama9** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz go lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Panorama9 jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Zarządzaj**, a następnie kliknij **rozszerzenia**.

    ![Rozszerzenia] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Rozszerzenia")

7.  W oknie dialogowym **rozszerzenia** kliknij polecenie **Logowania jednokrotnego**.

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Rejestracji jednokrotnej")

8.  W sekcji **Ustawienia** wykonaj następujące czynności:

    ![Ustawienia] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Ustawienia")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Panorama9** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **adres URL dostawcy tożsamości** .
    2.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **odcisku palca certyfikatu** .  

        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    3.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Panorama9, musi być przygotowana do Panorama9.  
W przypadku Panorama9 inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Panorama9** jako administrator.

2.  W menu u góry kliknij pozycję **Zarządzaj**, a następnie kliknij **użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Użytkownicy")

3.  Kliknij pozycję **+**.

4.  W sekcji danych użytkownika wykonaj następujące czynności:

    ![Użytkownicy] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Użytkownicy")

    1.  W polu tekstowym **wiadomości E-mail** wpisz adres e-mail prawidłowym użytkownikiem usługi Azure Active Directory, potrzebne do świadczenia.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE]Można użyć dowolnego innego Panorama9 użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Panorama9 zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Aby przypisać użytkowników do Panorama9, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Panorama9** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).