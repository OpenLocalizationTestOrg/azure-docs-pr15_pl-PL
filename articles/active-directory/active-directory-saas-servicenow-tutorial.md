<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z ServiceNow i ServiceNow Express | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory a ServiceNow ServiceNow Express."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Samouczek: Integracja Azure Active Directory z ServiceNow i ServiceNow Express.


W tym samouczku dowiesz się, jak zintegrować ServiceNow i ServiceNow Express z usługą Azure Active Directory (Azure AD).

Integracja z usługą Azure Active Directory ServiceNow i ServiceNow Express zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do ServiceNow i ServiceNow Express
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w ServiceNow i ServiceNow Express (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integracji z usługą Azure AD przy użyciu ServiceNow i ServiceNow Express, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- ServiceNow, wystąpienie lub dzierżawy ServiceNow, Calgary wersji lub nowszy
- Express ServiceNow, wystąpienie ServiceNow Express, wersja Helsinkach lub nowszy
- ServiceNow dzierżawy musi mieć [Wiele dostawcy pojedynczy znak na wtyczkę](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) włączone. Można to zrobić po przesłaniu żądania usługi w <https://hi.service-now.com/> 


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie ServiceNow z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego w usłudze ServiceNow lub ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>Dodawanie ServiceNow z galerii
Aby skonfigurować integrację Azure AD ServiceNow lub ServiceNow Express, musisz dodać ServiceNow z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa. 

**Aby dodać ServiceNow z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **ServiceNow**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. W okienku wyników wybierz **ServiceNow**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji można skonfigurować i badanie Azure AD rejestracji jednokrotnej z ServiceNow lub ServiceNow Express opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w ServiceNow jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w ServiceNow musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w ServiceNow. Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ServiceNow, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej dla ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Konfigurowanie Azure AD logowania jednokrotnego ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
3. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie ServiceNow testowanie użytkownika](#creating-a-servicenow-test-user)** — aby odpowiednikiem Simona Britta w ServiceNow połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
6. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

> [AZURE.NOTE] Jeśli chcesz skonfigurować ServiceNow Pomiń krok 2. Analogicznie jeśli chcesz skonfigurować ServiceNow Express Pomiń krok 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurowanie Azure AD rejestracji jednokrotnej dla ServiceNow

1.  W portalu klasyczny Azure AD na stronie integracji **ServiceNow** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do ServiceNow** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Skonfigurować adres URL aplikacji")

    . w polu tekstowym **ServiceNow logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.

    b. W polu tekstowym **identyfikator** wpisz swój adres URL używane przez użytkowników do logowania się w aplikacji ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.

    c. Kliknij przycisk **Dalej**

4.  Aby Azure AD automatycznie skonfigurować ServiceNow uwierzytelniania opartego na protokole SAML, wprowadź nazwę wystąpienia, administrator nazwy użytkownika i hasła administratora usługi ServiceNow w formularzu **Automatyczne konfigurowanie logowania jednokrotnego** i kliknij przycisk *Konfiguruj*. Należy zauważyć, że nazwa użytkownika administratora opisane musi być roli **security_admin** przypisane w ServiceNow to do pracy. W przeciwnym razie Aby ręcznie skonfigurować ServiceNow firmę Azure AD dostawcy tożsamości SAML, kliknij przycisk **Ręczne konfigurowanie aplikacji dla rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej** i należy wykonać następujące czynności.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Skonfigurować adres URL aplikacji")



5.  Na stronie **Konfigurowanie logowania jednokrotnego w ServiceNow** kliknij przycisk **Pobierz certyfikat**, Zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurowanie logowania jednokrotnego")

1. Zaloguj się w aplikacji ServiceNow jako administrator.
2. Aktywować dodatek _integracji - wielu dostawcy pojedynczego logowania jednokrotnego Instalatora_ , wykonując kolejne kroki:

    . W okienku nawigacji po lewej stronie przejdź do sekcji **Definicji systemu** , a następnie kliknij **wtyczki**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Aktywuj wtyczki")
    
    b. Wyszukiwanie _Integracja — wielu dostawcy pojedynczego logowania jednokrotnego Instalatora_.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Aktywuj wtyczki")

    c. Zaznacz tę wtyczkę. Rigth kliknij i wybierz opcję **Uaktywnij-uaktualnienie**.
    
    d. Kliknij przycisk **Aktywuj** .

2. W okienku nawigacji po lewej stronie kliknij pozycję **Właściwości**.  

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Skonfigurować adres URL aplikacji")


3. W oknie dialogowym **Właściwości logowania jednokrotnego wielu dostawcy** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Skonfigurować adres URL aplikacji")

    . Jak **włączyć wiele dostawcy logowania jednokrotnego**wybierz opcję **Tak**.

    b. Jak **włączyć rejestrowanie debugowania masz wiele dostawcy logowania jednokrotnego integracji**wybierz opcję **Tak**.

    c. W polu tekstowym **pola użytkownika tabeli, którą...** wpisz **nazwa_użytkownika**.

    d. Kliknij przycisk **Zapisz**.



1. W okienku nawigacji po lewej stronie kliknij pozycję **Certyfikaty x509**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfigurowanie logowania jednokrotnego")


1. W oknie dialogowym **Certyfikaty X.509** kliknij przycisk **Nowy**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfigurowanie logowania jednokrotnego")


1. W oknie dialogowym **Certyfikaty X.509** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurowanie logowania jednokrotnego")

    . Kliknij przycisk **Nowy**.

    b. W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji (przykład: **TestSAML2.0**).

    c. Zaznaczanie **aktywnej**.

    d. **Formatowanie**zaznacz **PEM**.

    e. Jako **Typ**wybierz pozycję **Zaufania certyfikatu magazynu**.
    
    f. Otwórz certyfikatu Base64 zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **PEM certyfikatu** .

    g. Kliknij przycisk **Aktualizuj**.


1. W okienku nawigacji po lewej stronie kliknij pozycję **Dostawcy tożsamości**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfigurowanie logowania jednokrotnego")

1. W oknie dialogowym **Dostawcy tożsamości** kliknij polecenie **Nowy**:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfigurowanie logowania jednokrotnego")


1. W oknie dialogowym **Dostawcy tożsamości** , kliknij polecenie **SAML2 Update1?**:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfigurowanie logowania jednokrotnego")


1. W oknie dialogowym właściwości Update1 SAML2 wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfigurowanie logowania jednokrotnego")


    . w polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji (np.: **SAML 2.0**).

    b. W polu **Użytkownika** pole tekstowe, wpisz **adres e-mail** lub **user_id**, w zależności od tego, które pole jest używane do jednoznacznie identyfikować użytkownikom we wdrożeniu programu ServiceNow. 
    
    > [AZURE.NOTE] Możesz configue Azure AD na emitowanie identyfikator użytkownika Azure AD (główna nazwa użytkownika) lub adres e-mail jako unikatowy identyfikator tokenu SAML, przechodząc do **ServiceNow > atrybuty > logowania jednokrotnego** sekcji Azure portal klasyczny i mapowania żądanego pola atrybutu **nameidentifier** . Wartość przechowywana dla wybranego atrybutu w Azure AD (np. głównej nazwy użytkownika) musi odpowiadać wartości przechowywanej w ServiceNow wprowadzona pola (na przykład user_id)

    c. W portalu klasyczny Azure AD skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **Adres URL dostawcy tożsamości** .

    d. W portalu klasycznym Azure AD skopiuj wartość **Adres URL uwierzytelniania żądanie** , a następnie wklej go w polu tekstowym **AuthnRequest dostawcy tożsamości** .

    e. W portalu klasyczny Azure AD skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **SingleLogoutRequest dostawcy tożsamości** .

    f. W polu tekstowym **Strona główna ServiceNow** wpisz adres URL strony głównej wystąpienia ServiceNow.

    > [AZURE.NOTE] Na stronie głównej wystąpienia ServiceNow jest łączenie **adresu URL dzierżawy ServieNow** i **/navpage.do** (np.:`https://fabrikam.service-now.com/navpage.do`).
 

    g. W **identyfikator jednostki-wystawcy** pole tekstowe, wpisz adres URL dzierżawy usługi ServiceNow.

    h. W polu tekstowym **Odbiorców adres URL** wpisz adres URL dzierżawy usługi ServiceNow. 

    mogę. W polu tekstowym **Protokół powiązanie protokołu IDP SingleLogoutRequest** wpisz **urn: oasis: nazwy: tc: SAML:2.0:bindings:HTTP-przekierowywać**.

    j. W polu tekstowym zasad NameID wpisz **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: nieokreślony**.

    k. Usuń zaznaczenie opcji **Utwórz AuthnContextClass**.

    l. W przypadku **Metody AuthnContextClassRef**wpisz `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Jest to potrzebne tylko w przypadku organizacji tylko w chmurze. Jeśli korzystasz z lokalnego ADFS lub MFA uwierzytelniania należy nie skonfigurować tę wartość. 

    m. W polu tekstowym **Pochylić zegar** wpisz **60**.

    n. Jako **Pojedynczy znak na skrypt**wybierz pozycję **MultiSSO_SAML2_Update1**.

    o. Jako **x509 certyfikatu**, wybierz certyfikat został utworzony w poprzednim kroku.

    p. Kliknij przycisk **Prześlij**. 



6. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurowanie logowania jednokrotnego")

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.
 
    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurowanie logowania jednokrotnego")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurowanie Azure AD rejestracji jednokrotnej ServiceNow Express

1.  W portalu klasyczny Azure AD na stronie integracji **ServiceNow** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do ServiceNow** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Skonfigurować adres URL aplikacji")

    . w polu tekstowym **ServiceNow logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.

    b. W polu tekstowym **Wystawcy adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji ServiceNow zgodnie ze wzorcem `https://<instance-name>.service-now.com`.

    c. Kliknij przycisk **Dalej**

4.  Kliknij przycisk **Ręczne konfigurowanie aplikacji dla rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej** , a następnie wykonaj następujące czynności.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Skonfigurować adres URL aplikacji")

5.  Na stronie **Konfigurowanie logowania jednokrotnego w ServiceNow** kliknij pozycję **Pobierz certyfikat**, Zapisz plik certyfikatu lokalnie na komputerze i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurowanie logowania jednokrotnego")

6. Zaloguj się w aplikacji ServiceNow Express jako administrator.

7. W okienku nawigacji po lewej stronie kliknij **Rejestracji jednokrotnej**.  

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Skonfigurować adres URL aplikacji")


8. W oknie **Logowania jednokrotnego** kliknij ikonę konfiguracji w prawym górnym rogu i ustawić następujące właściwości:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Skonfigurować adres URL aplikacji")

    . Przełącz **włączyć wiele dostawcy logowania jednokrotnego** w prawo.

    b. Przełącz **Włącz rejestrowanie wielu integracji logowania jednokrotnego dostawcy debugowania** w prawo.

    c. W polu tekstowym **pola użytkownika tabeli, którą...** wpisz **nazwa_użytkownika**.


9. W oknie **Logowania jednokrotnego** kliknij polecenie **Dodaj nowego certyfikatu**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfigurowanie logowania jednokrotnego")



10. W oknie dialogowym **Certyfikaty X.509** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurowanie logowania jednokrotnego")

    . W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji (przykład: **TestSAML2.0**).

    b. Zaznaczanie **aktywnej**.

    c. **Formatowanie**zaznacz **PEM**.

    d. Jako **Typ**wybierz pozycję **Zaufania certyfikatu magazynu**.

    e. Utwórz plik zakodowany Base64 z pobranego certyfikatu.
   
    > [AZURE.NOTE] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Otwórz certyfikatu Base64 zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **PEM certyfikatu** .

    g. Kliknij przycisk **Aktualizuj**.


11. W oknie **Logowania jednokrotnego** kliknij polecenie **Dodaj nowy protokołu IdP**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfigurowanie logowania jednokrotnego")


12. W oknie dialogowym **Dodawanie nowego dostawcy tożsamości** w obszarze **Konfigurowanie dostawcy tożsamości**wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfigurowanie logowania jednokrotnego")


    . W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji (np.: **SAML 2.0**).

    b. W portalu klasyczny Azure AD skopiuj wartość **Identyfikatora dostawcy tożsamości** , a następnie wklej go w polu tekstowym **Adres URL dostawcy tożsamości** .

    c. W portalu klasycznym Azure AD skopiuj wartość **Adres URL uwierzytelniania żądanie** , a następnie wklej go w polu tekstowym **AuthnRequest dostawcy tożsamości** .

    d. W portalu klasyczny Azure AD skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **SingleLogoutRequest dostawcy tożsamości** .

    e. Jako **Certyfikatu dostawcy tożsamości**wybierz certyfikat, który został utworzony w poprzednim kroku.


13. Kliknij przycisk **Ustawienia zaawansowane**, a następnie w obszarze **Dodatkowe właściwości dostawcy tożsamości**, wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfigurowanie logowania jednokrotnego")

    . W polu tekstowym **Protokół powiązanie protokołu IDP SingleLogoutRequest** wpisz **urn: oasis: nazwy: tc: SAML:2.0:bindings:HTTP-przekierowywać**.

    b. W polu tekstowym **Zasad NameID** wpisz **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: nieokreślony**.    

    c. W przypadku **Metody AuthnContextClassRef**wpisz **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Usuń zaznaczenie opcji **Utwórz AuthnContextClass**.

14. W obszarze **Dodatkowe właściwości dostawcy usług**wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfigurowanie logowania jednokrotnego")

    . W polu tekstowym **Strona główna ServiceNow** wpisz adres URL strony głównej wystąpienia ServiceNow.

    > [AZURE.NOTE] Na stronie głównej wystąpienia ServiceNow jest łączenie **adresu URL dzierżawy ServieNow** i **/navpage.do** (np.: `https://fabrikam.service-now.com/navpage.do`).

    b. W **identyfikator jednostki-wystawcy** pole tekstowe, wpisz adres URL dzierżawy usługi ServiceNow.

    c. W polu tekstowym **Identyfikator URI odbiorców** wpisz adres URL dzierżawy usługi ServiceNow. 

    d. W polu tekstowym **Pochylić zegar** wpisz **60**.

    e. W polu **Użytkownika** pole tekstowe, wpisz **adres e-mail** lub **user_id**, w zależności od tego, które pole jest używane do jednoznacznie identyfikować użytkownikom we wdrożeniu programu ServiceNow.
    
    > [AZURE.NOTE] Możesz configue Azure AD na emitowanie identyfikator użytkownika Azure AD (główna nazwa użytkownika) lub adres e-mail jako unikatowy identyfikator tokenu SAML, przechodząc do **ServiceNow > atrybuty > logowania jednokrotnego** sekcji Azure portal klasyczny i mapowania żądanego pola atrybutu **nameidentifier** . Wartość przechowywana dla wybranego atrybutu w Azure AD (np. głównej nazwy użytkownika) musi odpowiadać wartości przechowywanej w ServiceNow wprowadzona pola (na przykład user_id)

    f. Kliknij przycisk **Zapisz**. 


15. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurowanie logowania jednokrotnego")

16. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.
 
    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej konta użytkowników usługi Active Directory w celu ServiceNow użytkownika.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1. W portalu zarządzania Azure klasyczny na stronie integracji **ServiceNow** aplikacji kliknij pozycję **Konfiguruj użytkownika inicjowania obsługi administracyjnej**. 

    ![Przypisywanie użytkowników] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Przypisywanie użytkowników")


2. Na stronie **Wprowadź poświadczenia ServiceNow, aby włączyć automatyczne użytkownika inicjowania obsługi administracyjnej** wprowadź następujące ustawienia konfiguracji: Konfigurowanie przypisywanie użytkowników 

     . W polu tekstowym **Nazwa wystąpienia ServiceNow** wpisz nazwę wystąpienia ServiceNow.

     b. W polu tekstowym **Nazwa użytkownika administratora ServiceNow** wpisz nazwę konta administratora ServiceNow.

     c. W polu tekstowym **Hasło administratora ServiceNow** wpisz hasło dla tego konta.

     d. Kliknij przycisk **Sprawdzanie poprawności** , aby Sprawdź konfigurację.

     e. Kliknij przycisk **Dalej** , aby otworzyć stronę **Następne kroki** .

     f. Aby zainicjować obsługę wszystkich użytkowników do tej aplikacji, wybierz pozycję "**automatycznie obsługi administracyjnej wszystkich kont użytkowników w katalogu do tej aplikacji**". 

    ![Następne kroki] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Następne kroki")

     g. Na stronie **Następne kroki** kliknij **Wykonano** , aby zapisać konfigurację.

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   


### <a name="creating-a-servicenow-test-user"></a>Tworzenie użytkownika test ServiceNow

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w ServiceNow. W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w ServiceNow. Jeśli nie wiesz, jak dodać użytkownika na koncie ServiceNow lub ServiceNow Express, skontaktuj się z zespołem pomocy technicznej ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej ServiceNow za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta ServiceNow, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **ServiceNow**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka ServiceNow w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji ServiceNow.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
