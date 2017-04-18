<properties
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z ITRP | Microsoft Azure" 
    description="Dowiedz się, jak użyć ITRP z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Samouczek: Integracja usługi Azure Active Directory z ITRP
  
Celem tego samouczka jest pokazanie integracji Azure i ITRP.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy ITRP
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do ITRP będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy ITRP (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla ITRP
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Scenariusz")
##<a name="enabling-the-application-integration-for-itrp"></a>Włączanie integracji aplikacji dla ITRP
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla ITRP, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **ITRP**.

    ![Galeria aplikacji] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Galeria aplikacji")

7.  W okienku wyników wybierz **ITRP**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania ITRP przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla ITRP wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **ITRP** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do ITRP** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **ITRP adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. ITRP.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w ITRP** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\ITRP.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy ITRP jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Ustawienia**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  W okienku nawigacji po lewej stronie wybierz **Rejestracji jednokrotnej**.

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Rejestracji jednokrotnej")

8.  W sekcji Konfiguracja logowania jednokrotnego wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Rejestracji jednokrotnej")

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Rejestracji jednokrotnej")

    1.  Kliknij przycisk **Włącz**.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w ITRP** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL usługi zdalnej Wyloguj** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w ITRP** skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres URL logowania jednokrotnego SAML** .
    4.  Skopiuj **wartość** z wyeksportowany certyfikat, a następnie wklej go w polu tekstowym **Odcisku palca certyfikatu** .
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI)

    5.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do ITRP, musi być przygotowana do ITRP.  
W przypadku ITRP inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **ITRP** .

2.  Na pasku narzędzi u góry kliknij **rekordów**.

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Administrator")

3.  Z menu podręcznego wybierz pozycję **Kontakty**.

    ![Osoby] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Osoby")

4.  Kliknij przycisk **Dodaj nową osobę** ("+").

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Administrator")

5.  W oknie dialogowym Dodawanie nowej osoby wykonaj następujące czynności:

    ![Użytkownika] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Użytkownika")

    1.  Wpisz **nazwę**, **poczty E-mail** działające konto AAD chcesz świadczenia.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego ITRP użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez ITRP zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Aby przypisać użytkowników do ITRP, należy wykonać następujące czynności:

1.  W portalu Azure AD Utwórz konto testowe.

2.  Na stronie integracji **ITRP **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).