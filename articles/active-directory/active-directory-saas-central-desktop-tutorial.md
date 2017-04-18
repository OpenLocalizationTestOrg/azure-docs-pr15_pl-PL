<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z centralnego pulpitu | Microsoft Azure" 
    description="Dowiedz się, jak włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych elementów za pomocą pulpitu centralnej z usługą Azure Active Directory!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Samouczek: Azure Active Directory Integracja z centralnego pulpitu

Celem tego samouczka jest pokazanie integracji Azure i centralnego pulpitu. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Centralnego pulpitu rejestracji jednokrotnej dla subskrypcji włączone / dzierżawa centralnego pulpitu

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla centralnego pulpitu
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenariusz")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Włączanie integracji aplikacji dla centralnego pulpitu

Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji dla centralnego pulpitu.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla centralnego pulpitu, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Centralnego pulpitu**.

    ![Galeria aplikacji] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Centralnej pulpit**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Centralnego pulpitu] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Centralnego pulpitu")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do centralnego pulpitu przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do Twojej dzierżawy centralnego pulpitu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie Integracja z **Centralnego pulpitu** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do centralnego pulpitu** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**: 

    -   W polu tekstowym **Centralnego pulpitu logowania w adres URL** wpisz adres URL Twojej dzierżawy centralnego pulpitu (przykład: *http://contoso.centraldesktop.com*).
    -   W polu tekstowym adres URL odpowiedź Central pulpitu lokalizacja z centralnego pulpitu AssertionConsumerService (przykład: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Możesz wyświetlić wartość z centralnego pulpitu metadanych (przykład: *http://contoso.centraldesktop.com*).

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego centralnego pulpitu** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigurowanie logowania jednokrotnego")

5.  Zaloguj się do Twojej dzierżawy **Centralnego pulpitu** .

6.  Przejdź do strony **Ustawienia**, kliknij przycisk **Zaawansowane**, a następnie kliknij **Rejestracji jednokrotnej**.

    ![Instalacja — zaawansowane] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Instalacja — zaawansowane")

7.  Na stronie **Pojedynczy znak na ustawienia** wykonaj następujące czynności:

    ![Pojedynczy znak na ustawienia] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Pojedynczy znak na ustawienia")

    1.  Wybierz pozycję **Włącz SAML w wersji 2 logowaniu jednokrotnym**.
    2.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego centralnego pulpitu** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Adres URL logowania jednokrotnego** .
    3.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego centralnego pulpitu** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania jednokrotnego logowania** .
    4.  W portalu klasyczny Azure, na stronie **Konfigurowanie logowania jednokrotnego centralnego pulpitu** skopiuj wartość **Adres URL usługi pojedynczego Sign-Out** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj logowania jednokrotnego** .

8.  W sekcji **Metoda weryfikacji podpisu wiadomości** wykonaj następujące czynności:

    ![Metodę weryfikacji podpisu wiadomości] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Metodę weryfikacji podpisu wiadomości")

    1.  Wybierz **certyfikat**.
    2.  Na liście **Certyfikat logowania jednokrotnego** zaznacz **RSH SHA256**.
    3.  Tworzenie pliku tekstowego z pobranego certyfikatu, skopiuj zawartość pliku tekstowego, a następnie wklej go w polu **Logowania jednokrotnego certyfikatu** .  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Wybierz pozycję **Wyświetl łącze do strony logowania SAMLv2**.

9.  Kliknij przycisk **Aktualizuj**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

AAD użytkownikom będzie można się zalogować musi być przygotowana do aplikacji centralnego pulpitu. W tej sekcji opisano sposoby tworzenia kont użytkowników AAD w centralnej pulpitu.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Aby zainicjować obsługę kont użytkowników do centralnego pulpitu:

1.  Zaloguj się do Twojej dzierżawy centralnego pulpitu.

2.  Przejdź do pozycji **osób \> członków wewnętrznych**.

3.  Kliknij przycisk **Dodaj członków wewnętrznych**.

    ![Osoby] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Osoby")

4.  W polu tekstowym **Wiadomości E-mail adres z nowych członków** wpisz konta AAD udzielenia, a następnie kliknij przycisk **Dalej**.

    ![Adresy e-mail nowych członków] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Adresy e-mail nowych członków")

5.  Kliknij przycisk **Dodaj wewnętrznych mieli**.

    ![Dodawanie członka wewnętrznych] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Dodawanie członka wewnętrznych")

    >[AZURE.NOTE] Użytkownicy, które zostały dodane będą otrzymywać wiadomości e-mail, która zawiera łącze potwierdzenia, które są potrzebne do kliknij, aby aktywować konto.

>[AZURE.NOTE] Można użyć dowolnego innego centralnego pulpitu użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczane przez centralnego pulpitu zabezpieczanie AAD kont użytkowników

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Aby przypisać użytkowników do centralnego pulpitu, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Centralnej pulpitu** kliknij **przypisać użytkownikowi**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
