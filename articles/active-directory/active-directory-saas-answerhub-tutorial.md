<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z AnswerHub | Microsoft Azure" 
    description="Dowiedz się, jak użyć AnswerHub z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Samouczek: Azure Active Directory Integracja z AnswerHub

Celem tego samouczka jest pokazanie integracji Azure i [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software).  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do AnswerHub będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy AnswerHub (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla AnswerHub
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scenariusz")

##<a name="enabling-the-application-integration-for-answerhub"></a>Włączanie integracji aplikacji dla AnswerHub

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla AnswerHub.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla AnswerHub, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **AnswerHub**.

    ![Galeria aplikacji] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galeria aplikacji")

7.  W okienku wyników wybierz **AnswerHub**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania AnswerHub przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **AnswerHub** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do AnswerHub** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **AnswerHub adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://company.answerhub.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w AnswerHub** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy AnswerHub jako administrator.
    >[AZURE.NOTE] Jeśli potrzebujesz pomocy w konfigurowaniu AnswerHub, skontaktuj się z [zespołem pomocy technicznej firmy AnswerHub](mailto:success@answerhub.com. ).








6.  Przejdź do **administracji**.

7.  Kliknij kartę **użytkowników i grup** .

8.  W okienku nawigacji po lewej stronie, w sekcji **Ustawienia społecznościowych** kliknij przycisk **Ustawienia SAML**.

9.  Kliknij kartę **Protokołu IDP konfiguracji** .

10. Na karcie **Konfiguracji protokołu IDP** wykonaj następujące czynności:

    ![Ustawienia SAML] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "Ustawienia SAML")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w AnswerHub** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL protokołu IDP logowania** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w AnswerHub** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj protokołu IDP** .
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w AnswerHub** skopiuj wartość **Format identyfikator nazwy** , a następnie wklej go w polu tekstowym **Format identyfikator nazwy protokołu IDP** .
    4.  Kliknij przycisk **klucze i certyfikaty**.

11. Na karcie klucze i certyfikaty wykonaj następujące czynności:

    ![Certyfikaty i klucze] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Certyfikaty i klucze")

    1.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Protokołu IDP kluczem publicznym (x 509 Format)** .
    3.  Kliknij przycisk **Zapisz**.

12. Na karcie **Protokołu IDP konfiguracji** kliknij przycisk **Zapisz**.

13. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do AnswerHub, musi być przygotowana do AnswerHub.  
W przypadku AnswerHub inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **AnswerHub** jako administrator.

2.  Przejdź do **administracji**.

3.  Kliknij kartę **Użytkownicy i grupy** .

4.  W okienku nawigacji po lewej stronie, w sekcji **Zarządzanie użytkownikami** kliknij pozycję **Utwórz lub zaimportuj użytkowników**.

    ![Użytkownicy i grupy] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Użytkownicy i grupy")

5.  Wpisz **Adres E-mail**, **nazwę użytkownika** i **hasło** prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych, a następnie kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego AnswerHub użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez AnswerHub zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Aby przypisać użytkowników do AnswerHub, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **AnswerHub **aplikacji kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
