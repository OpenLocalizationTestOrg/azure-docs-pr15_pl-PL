<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Bambus HR | Microsoft Azure" 
    description="Dowiedz się, jak użyć HR Bambus z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Samouczek: Integracja usługi Azure Active Directory z Bambus godz.

Celem tego samouczka jest pokazanie integracji Azure i BambooHR.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   BambooHR logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do BambooHR będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy BambooHR (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla BambooHR
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenariusz")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Włączanie integracji aplikacji dla BambooHR

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla BambooHR.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla BambooHR, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **BambooHR**.

    ![Galeria aplikacji] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Galeria aplikacji")

7.  W okienku wyników wybierz **BambooHR**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania BambooHR przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **BambooHR** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Scenariusz] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenariusz")

2.  Na stronie **jak chcesz użytkownikom logowanie do BambooHR** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **BambooHR logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji BambooHR (przykład: https://company.bamboohr.com), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w BambooHR** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy BambooHR jako administrator.

6.  Na stronie głównej wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Rejestracji jednokrotnej")

    1.  Kliknij pozycję **aplikacje**.
    2.  W menu aplikacje po lewej stronie kliknij **Rejestracji jednokrotnej**.
    3.  Kliknij pozycję **SAML rejestracji jednokrotnej**.

7.  W sekcji **SAML rejestracji jednokrotnej** wykonaj następujące czynności:

    ![SAML logowania jednokrotnego] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML logowania jednokrotnego")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w BambooHR** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Adres URL logowania jednokrotnego logowania** .
    2.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wklej go do pola tekstowego **Certyfikat X.509**
    4.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do BambooHR, musi być przygotowana do BambooHR.  
W przypadku BambooHR inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny **BambooHR** jako administrator.

2.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**.

    ![Ustawianie] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Ustawianie")

3.  Kliknij przycisk **Podgląd**.

4.  W lewym okienku nawigacji, przejdź do **zabezpieczeń \> użytkowników**.

5.  Wpisz adres e-mail, hasło i nazwa użytkownika prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.

6.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego BambooHR użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez BambooHR zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Aby przypisać użytkowników do BambooHR, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **BambooHR **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
