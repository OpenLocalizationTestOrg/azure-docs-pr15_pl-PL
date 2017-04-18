<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Zendesk | Microsoft Azure" 
    description="Dowiedz się, jak użyć Zendesk z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Samouczek: Azure Active Directory Integracja z Zendesk
  
Celem tego samouczka jest pokazanie integracja Azure i Zendesk.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Zendesk
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Zendesk będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Zendesk (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Zendesk
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenariusz")

##<a name="enabling-the-application-integration-for-zendesk"></a>Włączanie integracji aplikacji dla Zendesk
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Zendesk, wykonaj następujące czynności:

1.  W portalu zarządzania Azure w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Zendesk**.

    ![Galeria aplikacji] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Zendesk**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Zendesk przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Zendesk wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu Azure AD na stronie integracji **Zendesk** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do Zendesk** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Skonfigurować adres URL aplikacji")
  
    . W polu tekstowym **Zendesk adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca:`https://<tenant-name>.zendesk.com`

    b. Kliknij przycisk **Dalej**.



4.  Na stronie **Konfigurowanie logowania jednokrotnego w Zendesk** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na Twojej compiter.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Zendesk jako administrator.

6.  Kliknij pozycję **Administrator**.

7.  W okienku nawigacji po lewej stronie kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Zabezpieczenia**.

    ![Zabezpieczenia] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Zabezpieczenia")

8.  Na **stronie** kliknij kartę **Administrator i agentów** .

9.  Wybierz pozycję **pojedynczy logowania jednokrotnego (SSO) i SAML**, a następnie wybierz pozycję **SAML**.

10. W portalu Azure AD na stronie **Konfigurowanie logowania jednokrotnego w Zendesk** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres URL logowania jednokrotnego SAML** .

11. W portalu Azure AD na stronie **Konfigurowanie logowania jednokrotnego w Zendesk** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL usługi zdalnej Wyloguj** .

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Rejestracji jednokrotnej")

12. Skopiuj **wartość** z wyeksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .

    >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

13. Kliknij przycisk **Zapisz**.

14. W portalu Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do **Zendesk**, musi być przygotowana do **Zendesk**.  
W przypadku **Zendesk**inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Aby obsługi administracyjnej konta użytkownika w celu Zendesk, należy wykonać następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Zendesk** .

2.  Wybierz kartę **Lista klientów** .

3.  Wybierz kartę **użytkownika** , a następnie kliknij przycisk **Dodaj**.

    ![Dodaj użytkownika] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Dodaj użytkownika")

4.  Wpisz adres e-mail istniejącego konta Azure AD udzielenia, a następnie kliknij przycisk **Zapisz**.

    ![Nowy użytkownik] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Nowy użytkownik")

>[AZURE.NOTE] Można użyć dowolnego innego Zendesk użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Zendesk zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Aby przypisać użytkowników do Zendesk, należy wykonać następujące czynności:

1.  W portalu Azure AD Utwórz konto testowe.

2.  Na stronie integracji **Zendesk **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
