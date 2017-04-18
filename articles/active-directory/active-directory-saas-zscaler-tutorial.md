<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Zscaler | Microsoft Azure" 
    description="Dowiedz się, jak użyć Zscaler z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Samouczek: Azure Active Directory Integracja z Zscaler
  
Celem tego samouczka jest pokazanie integracji Azure i Zscaler. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy w sekcji Zscaler
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Zscaler
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie ustawień serwera proxy
4.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
5.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenariusz")

##<a name="enabling-the-application-integration-for-zscaler"></a>Włączanie integracji aplikacji dla Zscaler
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Zscaler.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Zscaler, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Zscaler**.

    ![Galeria aplikacji] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Zscaler**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Zscaler przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury musisz przekazać certyfikatu do Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Zscaler** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do Zscaler** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie pojedynczego logowanie] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Konfigurowanie pojedynczego logowanie")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Zscaler adres URL logowania** wpisz informacje logowania masz od Zscaler adres URL, a następnie kliknij przycisk **Dalej**: 

    >[AZURE.NOTE] Skontaktuj się z obsługą Zscaler, jeśli nie wiesz, co to jest adres URL logowania.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Zscaler** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Konfigurowanie logowania jednokrotnego")

    1.  Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Zscaler.cer**.
    2.  Skopiuj adres **URL żądania uwierzytelniania** do Schowka.

5.  Zaloguj się do Twojej dzierżawy Zscaler.

6.  W menu u góry kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Administracja")

7.  W obszarze **Zarządzanie administratorów i ról**kliknij pozycję **Zarządzanie użytkownikami i uwierzytelniania**.

    ![Zarządzaj administratorów i ról] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Zarządzaj administratorów i ról")

8.  W sekcji **Wybierz opcję uwierzytelniania dla Twojej organizacji** wykonaj następujące czynności:

    ![Opcje uwierzytelniania] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Opcje uwierzytelniania")

    1.  Zaznacz **uwierzytelniania przy użyciu SAML rejestracji jednokrotnej**.
    2.  Kliknij przycisk **Konfiguruj SAML pojedynczych logowania jednokrotnego parametrów**.

9.  Na stronie okno dialogowe **Konfigurowanie SAML pojedynczych logowania jednokrotnego parametrów** wykonaj następujące czynności, a następnie kliknij **Gotowe**:

    ![Przekazywanie certyfikatu] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Przekazywanie certyfikatu")

    1.  W polu tekstowym **adres URL portalu SAML, do której użytkownicy są wysyłane do uwierzytelniania** Wklej wartość pola **adresu URL żądania uwierzytelniania** za pomocą portalu klasyczny Azure.
    2.  W polu tekstowym **atrybut zawierającą nazwę logowania** wpisz **NameID**.
    3.  W polu **Przekaż publicznej certyfikat SSL** przekazywanie certyfikatu pobranego z portalu klasyczny Azure.
    4.  Zaznacz opcję **Włącz SAML automatyczne-inicjowania obsługi administracyjnej**.

10. Na stronie okno dialogowe **Konfigurowanie uwierzytelniania użytkowników** wykonaj następujące czynności:

    ![Konfigurowanie uwierzytelniania użytkownika] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Konfigurowanie uwierzytelniania użytkownika")

    1.  Kliknij przycisk **Zapisz**.
    2.  Kliknij przycisk **Aktywuj teraz**.

11. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-proxy-settings"></a>Konfigurowanie ustawień serwera proxy

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Aby skonfigurować ustawienia serwera proxy w programie Internet Explorer

1.  Uruchom **program Internet Explorer**.

2.  W menu **Narzędzia** , aby otworzyć okno dialogowe **Opcje internetowe** , wybierz polecenie **Opcje internetowe** .

    ![Opcje internetowe] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Opcje internetowe")

3.  Kliknij kartę **połączenia** .

    ![Połączenia] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Połączenia")

4.  Kliknij przycisk **Ustawienia sieci LAN** , aby otworzyć okno dialogowe **Ustawienia sieci LAN** .

5.  W sekcji serwer Proxy wykonaj następujące czynności:

    ![Serwer proxy] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Serwer proxy")

    1.  Wybierz pozycję Użyj serwera proxy dla sieci LAN.
    2.  W polu tekstowym Adres wpisz **gateway.zscalertwo.net**.
    3.  W polu tekstowym portu wpisz **80**.
    4.  Zaznacz **Używaj serwera proxy dla adresów lokalnych**.
    5.  Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Ustawienia sieci lokalnej (LAN)** .

6.  Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Opcje internetowe** .

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Zscaler, musi być przygotowana do Zscaler.  
W przypadku Zscaler inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Zscaler** .

2.  Kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Administracja")

3.  Kliknij pozycję **Zarządzanie użytkownikami**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Zarządzanie użytkownikami")

4.  Na karcie **użytkowników** kliknij przycisk **Dodaj**.

    ![Dodawanie] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Dodawanie")

5.  W sekcji Dodaj użytkownika, wykonaj następujące czynności:

    ![Dodawanie użytkownika] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Dodawanie użytkownika")

    1.  Wpisz **nazwę użytkownika**, **Wyświetlana nazwa użytkownika**, **hasło**i **Potwierdź hasło**, a następnie wybierz **grupy** i **Dział** działające konto AAD potrzebne do świadczenia.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego Zscaler użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Zscaler zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Aby przypisać użytkowników do Zscaler, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Zscaler** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
