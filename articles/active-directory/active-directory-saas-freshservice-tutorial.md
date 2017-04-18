<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z FreshService | Microsoft Azure" 
    description="Dowiedz się, jak użyć FreshService z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Samouczek: Azure Active Directory Integracja z FreshService
  
Celem tego samouczka jest pokazanie integracji Azure i FreshService.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   FreshService logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do FreshService będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla FreshService
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Scenariusz")
##<a name="enabling-the-application-integration-for-freshservice"></a>Włączanie integracji aplikacji dla FreshService
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla FreshService.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla FreshService, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **FreshService**.

    ![Galeria aplikacji] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Galeria aplikacji")

7.  W okienku wyników wybierz **FreshService**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania FreshService przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla FreshService wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **FreshService** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do FreshService** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **FreshService logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Freshdesk (np.: "*http://democompany.freshservice.com/*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w FreshService** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy FreshService jako administrator.

6.  W menu u góry kliknij pozycję **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administrator")

7.  W **Portalu klienta**kliknij pozycję **Zabezpieczenia**.

    ![Zabezpieczenia] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Zabezpieczenia")

8.  W sekcji **Zabezpieczenia** należy wykonać następujące czynności:

    ![Logowaniu jednokrotnym] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Logowaniu jednokrotnym")

    1.  Przełączanie **OnON rejestracji jednokrotnej**.
    2.  Wybierz pozycję **Logowania jednokrotnego SAML**.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w FreshService** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania SAML** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w FreshService** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .
    5.  Skopiuj **wartość** z eksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu zabezpieczeń** .
    
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do FreshService, musi być przygotowana do FreshService.  
W przypadku FreshService inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **FreshService** jako administrator.

2.  W menu u góry kliknij pozycję **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administrator")

3.  W sekcji **Zarządzanie użytkownikami** kliknij pozycję **osoby zleceniodawców**.

    ![Osoby zleceniodawców] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Osoby zleceniodawców")

4.  Kliknij przycisk **Nowy żądającego**.

    ![Nowe osoby zleceniodawców] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Nowe osoby zleceniodawców")

5.  W sekcji **Nowy żądającego** wykonaj następujące czynności:

    ![Nowe żądającego] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Nowe żądającego")

    1.  Wprowadź **Imię** i **poczty E-mail** atrybuty działające konto usługi Azure Active Directory, który ma udzielenia do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

    >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomość e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny

>[AZURE.NOTE] Można użyć dowolnego innego FreshService użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez FreshService zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Aby przypisać użytkowników do FreshService, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **FreshService **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).