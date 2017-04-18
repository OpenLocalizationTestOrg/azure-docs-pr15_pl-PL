<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Thirdlight | Microsoft Azure" 
    description="Dowiedz się, jak użyć Thirdlight z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Samouczek: Integracja usługi Azure Active Directory z Thirdlight
  
Celem tego samouczka jest pokazanie integracja Azure i Thirdlight.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Thirdlight logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Thirdlight będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Thirdlight (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Thirdlight
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Scenariusz")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Włączanie integracji aplikacji dla Thirdlight
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Thirdlight.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Thirdlight, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Thirdlight**.

    ![Galeria aplikacji] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Thirdlight**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Thirdlight przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Thirdlight wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Thirdlight** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Thirdlight** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Thirdlight adres URL logowania** wpisz swój adres URL używane przez użytkowników w celu Logowanie do aplikacji Thirdlight (np.: "*http://azuresso2.thirdlight.com/*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Thirdlight** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Thirdlight jako administrator.

6.  Przejdź do pozycji **konfiguracji \> Administracja**, a następnie kliknij pozycję **SAML2**.

    ![Administracja] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Administracja")

7.  W sekcji Konfiguracja SAML2 wykonaj następujące czynności:

    ![SAML logowania jednokrotnego] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML logowania jednokrotnego")

    1.  Zaznacz opcję **Włącz SAML2 rejestracji jednokrotnej**.
    2.  Jako **źródło dla protokołu IdP metadanych**wybierz pozycję **Załaduj protokołu IdP metadanych z pliku XML**.
    3.  Otwórz plik pobrany metadanych, skopiuj zawartość, a następnie wklej go w polu tekstowym **Protokołu IdP metadanych XML** .
    4.  Kliknij przycisk **Zapisz SAML2 ustawienia**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Thirdlight, musi być przygotowana do Thirdlight.  
W przypadku Thirdlight inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Thirdlight** jako administrator.

2.  Przejdź do karty **użytkowników** .

3.  Wybierz pozycję **Użytkownicy i grupy**.

4.  Kliknij przycisk **Dodaj nowego użytkownika** .

5.  Wprowadź **nazwę użytkownika, nazwę lub opis, wiadomości E-mail, wybierz ustawienia domyślne lub grupie nowych członków** ważnego konta AAD, które mają dostarczaniem.

6.  Kliknij przycisk **Utwórz**.

>[AZURE.NOTE] Można użyć dowolnego innego Thirdlight użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Thirdlight zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Aby przypisać użytkowników do Thirdlight, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Thirdlight **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).