<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z InsideView | Microsoft Azure" 
    description="Dowiedz się, jak użyć InsideView z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Samouczek: Azure Active Directory Integracja z InsideView
  
Celem tego samouczka jest pokazanie integracja Azure i InsideView.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy InsideView
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do InsideView będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy InsideView (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla InsideView
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenariusz")
##<a name="enabling-the-application-integration-for-insideview"></a>Włączanie integracji aplikacji dla InsideView
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla InsideView.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla InsideView, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **InsideView**.

    ![Galeria aplikacji] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Galeria aplikacji")

7.  W okienku wyników wybierz **InsideView**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania InsideView przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **InsideView** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Konfigurowanie logować pojedynczy")

2.  Na stronie **jak chcesz użytkownikom logowanie do InsideView** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Konfigurowanie logować pojedynczy")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL odpowiedź InsideView** wpisz adres URL logowania jednokrotnego InsideView (np.: `https://my.insideview.com/iv/<STS Name>/login.iv`), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w InsideView** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Konfigurowanie logować pojedynczy")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy InsideView jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Administrator**, **SingleSignOn ustawienia**, a następnie kliknij przycisk **Dodaj SAML**.

    ![SAML logowaniu jednokrotnym ustawienia] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML logowaniu jednokrotnym ustawienia")

7.  W sekcji **Dodawanie nowych SAML** wykonaj następujące czynności:

    ![Dodawanie nowego SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Dodawanie nowego SAML")

    1.  W polu tekstowym **Nazwa usługi STS** wpisz nazwę swojej konfiguracji.
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w InsideView** skopiuj wartość **Końcowy inicjowanych przez dostawcę usługi (SP)** , a następnie wklej go w polu tekstowym **SamlP-WS-Fed Unsolicated końcowego** .
    3.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **Certyfikat STS**
    5.  W polu tekstowym **Mapowania identyfikatorów użytkowników Crm** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  W polu tekstowym **Mapowanie E-mail Crm** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  W polu tekstowym **Mapowanie imię Crm** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  W polu tekstowym **Mapowanie nazwisko Crm** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Kliknij przycisk **Zapisz**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Konfigurowanie logować pojedynczy")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do InsideView, musi być przygotowana do InsideView.  
W przypadku InsideView inicjowania obsługi administracyjnej to zadanie ręczne.
  
Aby uzyskać użytkowników lub kontakty utworzone w InsideView, skontaktuj się z menedżerem sukcesu klienta lub wysyłanie wiadomości e-mail do**support@insideview.com**

>[AZURE.NOTE] Można użyć dowolnego innego InsideView użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez InsideView do zapewniania obsługi Azure AD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Aby przypisać użytkowników do InsideView, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **InsideView **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).