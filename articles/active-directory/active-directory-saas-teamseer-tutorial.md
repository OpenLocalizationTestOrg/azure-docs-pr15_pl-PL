<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z TeamSeer | Microsoft Azure" 
    description="Dowiedz się, jak użyć TeamSeer z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Samouczek: Azure Active Directory Integracja z TeamSeer
  
Celem tego samouczka jest pokazanie integracji Azure i TeamSeer.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy TeamSeer
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do TeamSeer będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy TeamSeer (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla TeamSeer
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Scenariusz")

##<a name="enabling-the-application-integration-for-teamseer"></a>Włączanie integracji aplikacji dla TeamSeer
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla TeamSeer.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla TeamSeer, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **TeamSeer**.

    ![Galeria aplikacji] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Galeria aplikacji")

7.  W okienku wyników wybierz **TeamSeer**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania TeamSeer przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **TeamSeer** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do TeamSeer** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **TeamSeer adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*http://www.teamseer.com/companyid*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w TeamSeer** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy TeamSeer jako administrator.

6.  Przejdź do pozycji **Administrator HR**.

    ![Administrator godz.] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "Administrator godz.")

7.  Kliknij przycisk **Ustawienia**.

    ![Konfiguracja] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Konfiguracja")

8.  Kliknij pozycję **Konfiguruj SAML dostawcy szczegóły**.

    ![Ustawienia SAML] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "Ustawienia SAML")

9.  W sekcji szczegółów dostawcy SAML wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "Ustawienia SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w TeamSeer** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **adres URL** .
    2.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Protokołu IdP certyfikatu publicznego** .

10. Aby ukończyć konfigurację dostawcy SAML, wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "Ustawienia SAML")

    1.  W oknie dialogowym **Testowanie adresy E-mail**wpisz adres e-mail użytkownika testowego.
    2.  W polu tekstowym **wystawcy** wpisz adres URL wystawcy usługodawcy.
    3.  Kliknij przycisk **Zapisz**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do TeamSeer, musi być przygotowana do ShiftPlanning.  
W przypadku TeamSeer inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **TeamSeer** jako administrator.

2.  Wykonaj następujące czynności:

    ![Administrator godz.] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "Administrator godz.")

    1.  Przejdź do pozycji **Administrator HR \> użytkowników**.
    2.  Kliknij przycisk **Uruchom Kreatora nowego użytkownika**.

3.  W sekcji **Szczegółów użytkownika** wykonaj następujące czynności:

    ![Szczegóły użytkownika] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Szczegóły użytkownika")

    1.  Wpisz **Imię**, **nazwisko**, **nazwę użytkownika (adres E-mail)** prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Dalej**.

4.  Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie podczas dodawania nowego użytkownika, a następnie kliknij przycisk **Zakończ**.

>[AZURE.NOTE] Można użyć dowolnego innego TeamSeer użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez TeamSeer do zapewniania obsługi Azure AD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Aby przypisać użytkowników do TeamSeer, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **TeamSeer **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).