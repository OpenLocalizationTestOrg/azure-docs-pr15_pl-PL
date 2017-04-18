<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z Zscaler Beta | Microsoft Azure" 
    description="Dowiedz się, jak użyć Zscaler Beta z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Samouczek: Integracja usługi Azure Active Directory z Zscaler Beta
  
Celem tego samouczka jest pokazanie integracji Azure "i" ZScaler Beta.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   ZScaler Beta logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do ZScaler Beta będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny ZScaler Beta (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla ZScaler w wersji Beta
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie ustawień serwera proxy
4.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
5.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "Scenariusz")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>Włączanie integracji aplikacji dla ZScaler w wersji Beta
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji dla ZScaler w wersji Beta.

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla ZScaler w wersji Beta, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **ZScaler Beta**.

    ![Galeria aplikacji] (./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "Galeria aplikacji")

7.  W okienku wyników wybierz **ZScaler Beta**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ZScaler z nich] (./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "ZScaler z nich")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML ZScaler Beta przy użyciu swojego konta.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do Twojej dzierżawy ZScaler Beta.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **ZScaler Beta** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do wersji ZScaler Beta** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **ZScaler Beta logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji ZScaler Beta, a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "Skonfigurować adres URL aplikacji")

    >[AZURE.NOTE] Uzyskaj wartością rzeczywistą w środowisku usługi z zespołem pomocy technicznej ZScaler Beta jest potrzebny.

4.  Na stronie **Konfigurowanie logowania jednokrotnego w wersji ZScaler Beta** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy ZScaler Beta jako administrator.

6.  W menu u góry kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "Administracja")

7.  W obszarze **Zarządzanie administratorów i ról**kliknij pozycję **Zarządzanie użytkownikami i uwierzytelniania**.

    ![Zarządzanie użytkownikami i uwierzytelniania] (./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "Zarządzanie użytkownikami i uwierzytelniania")

8.  W sekcji **Wybierz opcje uwierzytelniania dla Twojej organizacji** wykonaj następujące czynności:

    ![Uwierzytelnianie] (./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "Uwierzytelnianie")

    1.  Zaznacz **uwierzytelniania przy użyciu SAML rejestracji jednokrotnej**.
    2.  Kliknij przycisk **Konfiguruj SAML pojedynczych logowania jednokrotnego parametrów**.

9.  Na stronie okno dialogowe **Konfigurowanie SAML pojedynczych logowania jednokrotnego parametrów** wykonaj następujące czynności, a następnie kliknij **Gotowe**:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "Rejestracji jednokrotnej")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w wersji ZScaler Beta** skopiuj wartość **Adres URL uwierzytelniania żądanie** , a następnie wklej go w polu tekstowym **adres URL portalu SAML, do której użytkownicy są wysyłane do uwierzytelniania** .
    2.  W polu tekstowym **atrybut zawierającą nazwę logowania** wpisz **NameID**.
    3.  Aby przekazać pobranego certyfikatu, kliknij przycisk **Zscaler pem**.
    4.  Zaznacz opcję **Włącz SAML automatyczne-inicjowania obsługi administracyjnej**.

10. Na stronie okno dialogowe **Konfigurowanie uwierzytelniania użytkowników** wykonaj następujące czynności:

    ![Administracja] (./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "Administracja")

    1.  Kliknij przycisk **Zapisz**.
    2.  Kliknij przycisk **Aktywuj teraz**.

11. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w wersji ZScaler Beta** wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **wykonane**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-proxy-settings"></a>Konfigurowanie ustawień serwera proxy

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Aby skonfigurować ustawienia serwera proxy w programie Internet Explorer

1.  Uruchom **program Internet Explorer**.

2.  W menu **Narzędzia** , aby otworzyć okno dialogowe **Opcje internetowe** , wybierz polecenie **Opcje internetowe** .

    ![Opcje internetowe] (./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Opcje internetowe")

3.  Kliknij kartę **połączenia** .

    ![Połączenia] (./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "Połączenia")

4.  Kliknij przycisk **Ustawienia sieci LAN** , aby otworzyć okno dialogowe **Ustawienia sieci LAN** .

5.  W sekcji serwer Proxy wykonaj następujące czynności:

    ![Serwer proxy] (./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "Serwer proxy")

    1.  Wybierz pozycję Użyj serwera proxy dla sieci LAN.
    2.  W polu tekstowym Adres wpisz **gateway.zscalerBeta.net**.
    3.  W polu tekstowym portu wpisz **80**.
    4.  Zaznacz **Używaj serwera proxy dla adresów lokalnych**.
    5.  Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Ustawienia sieci lokalnej (LAN)** .

6.  Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Opcje internetowe** .

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do ZScaler Beta, musi być przygotowana do wersji ZScaler Beta.  
W przypadku ZScaler Beta inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Zscaler** .

2.  Kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "Administracja")

3.  Kliknij pozycję **Zarządzanie użytkownikami**.

    ![Dodawanie] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Dodawanie")

4.  Na karcie **użytkowników** kliknij przycisk **Dodaj**.

    ![Dodawanie] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Dodawanie")

5.  W sekcji Dodaj użytkownika, wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "Dodawanie użytkownika")

    1.  Wpisz **nazwę użytkownika**, **Wyświetlana nazwa użytkownika**, **hasło**i **Potwierdź hasło**, a następnie wybierz **grupy** i **Dział** działające konto AAD potrzebne do świadczenia.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego ZScaler Beta użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez ZScaler Beta zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>Aby przypisać użytkowników do ZScaler Beta, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **ZScaler Beta** aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).