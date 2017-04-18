<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z IdeaScale | Microsoft Azure" 
    description="Dowiedz się, jak użyć IdeaScale z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Samouczek: Azure Active Directory Integracja z IdeaScale
  
Celem tego samouczka jest pokazanie integracji Azure i IdeaScale.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   IdeaScale logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do IdeaScale będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla IdeaScale
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenariusz")
##<a name="enabling-the-application-integration-for-ideascale"></a>Włączanie integracji aplikacji dla IdeaScale
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla IdeaScale.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla IdeaScale, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **IdeaScale**.

    ![Galeria aplikacji] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Galeria aplikacji")

7.  W okienku wyników wybierz **IdeaScale**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania IdeaScale przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla IdeaScale wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **IdeaScale** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do IdeaScale** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **IdeaScale logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji IdeaScale (np.: "*https://company.IdeaScale.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w IdeaScale** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy IdeaScale jako administrator.

6.  Przejdź do strony **Ustawienia społeczności**.

    ![Ustawienia społeczności] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Ustawienia społeczności")

7.  Przejdź do pozycji **zabezpieczeń \> pojedynczy logować ustawienia**.

    ![Ustawienia pojedynczego logować] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Ustawienia pojedynczego logować")

8.  Jako **Pojedyncza — typ**wybierz pozycję **SAML 2.0**.

    ![Pojedynczy typ logować] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Pojedynczy typ logować")

9.  W oknie dialogowym **Ustawienia pojedynczego logować** wykonaj następujące czynności:

    ![Ustawienia pojedynczego logować] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Ustawienia pojedynczego logować")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w IdeaScale** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go w polu tekstowym **Identyfikator jednostki protokołu IdP SAML** .
    2.  Skopiuj zawartość metadanych pobrany plik, a następnie wklej go w polu tekstowym **SAML protokołu IdP metadanych** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w IdeaScale** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL sukcesu Wyloguj** .
    4.  Kliknij przycisk **Zapisz zmiany**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do IdeaScale, musi być przygotowana do IdeaScale.  
W przypadku IdeaScale inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **IdeaScale** jako administrator.

2.  Przejdź do strony **Ustawienia społeczności**.

    ![Ustawienia społeczności] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Ustawienia społeczności")

3.  Przejdź do pozycji **podstawowych ustawień \> Zarządzanie członkami grupy**.

4.  Kliknij przycisk **Dodaj członka**.

    ![Zarządzanie członkami grupy] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Zarządzanie członkami grupy")

5.  W sekcji Dodawanie nowego członka wykonaj następujące czynności:

    ![Dodawanie nowego członka] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Dodawanie nowego członka")

    1.  W polu tekstowym **Adres E-mail** wpisz adres e-mail ważnego konta AAD potrzebne do świadczenia.
    2.  Kliknij przycisk **Zapisz zmiany**.

    >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomość e-mail z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego IdeaScale użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez IdeaScale zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Aby przypisać użytkowników do IdeaScale, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **IdeaScale **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).