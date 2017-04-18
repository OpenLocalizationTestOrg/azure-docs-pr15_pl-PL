<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z pakietem BPM Questetra | Aure firmy Microsoft"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Questetra BPM pakiet."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Samouczek: Azure Active Directory Integracja z pakietem BPM Questetra

Celem tego samouczka jest pokazano, jak zintegrować zestaw BPM Questetra z usługi Azure Active Directory (Azure AD).  
Integrowanie zestawu BPM Questetra z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do pakietu BPM Questetra 
- Możesz włączyć użytkowników, aby automatycznie uzyskać podpisane w pakiecie BPM Questetra (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD Integracja z pakietem BPM Questetra, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- [Pakiet BPM Questetra](https://senbon-imadegawa-988.questetra.net/) logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie pakietu BPM Questetra z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Dodawanie pakietu BPM Questetra z galerii
Aby skonfigurować integrację Azure AD Questetra BPM pakietu, musisz dodać pakiet BPM Questetra z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać pakiet BPM Questetra z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Questetra BPM pakietu**.

    ![Aplikacje][5]

7. W okienku wyników wybierz **Pakiet BPM Questetra**, a następnie kliknij **Wykonano** w celu dodania aplikacji.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z pakietem BPM Questetra opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi pakietu BPM Questetra użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi pakietu BPM Questetra musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwa_użytkownika** pakietu BPM Questetra.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z pakietem BPM Questetra, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie pakietu BPM Questetra testowanie użytkownika](#creating-a-questetra-bpm-suite-test-user)** — aby odpowiednikiem Simona Britta w pakiecie BPM Questetra połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji pakietu BPM Questetra.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z pakietem BPM Questetra, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **Questetra BPM pakiet** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][8]

2. Na stronie **jak chcesz użytkownikom logowanie w pakiecie BPM Questetra** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][9]


3. W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy **Questetra BPM pakietu** jako administrator.

4. W menu u góry kliknij pozycję **Ustawienia systemowe**. 

    ![Azure AD rejestracji jednokrotnej][10]

5. Aby otworzyć stronę **SingleSignOnSAML** , kliknij pozycję **Logowania jednokrotnego SAML ()**. 

    ![Azure AD rejestracji jednokrotnej][11]


6. W portalu klasyczny Azure na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności: 

    ![Konfigurowanie ustawień aplikacji][13]
 
    . Na możesz **Questetra BPM pakietu** firmowej witryny, w sekcji informacje o SP skopiuj adres **ACS URL**, a następnie wklej go w polu tekstowym **Logowania na adres URL** .

    b. Na możesz **Questetra BPM pakietu** firmowej witryny, w sekcji informacje o SP Skopiuj **Identyfikator jednostki**, a następnie wklej go w polu tekstowym **Adres URL wystawcy** .

    c. Na możesz **Questetra BPM pakietu** firmowej witryny, w sekcji informacje o SP skopiuj adres **ACS URL**, a następnie wklej go w polu tekstowym **Adres URL odpowiedź** .

    d. Kliknij przycisk **Dalej**.

 
7. Na stronie **Konfigurowanie logowania jednokrotnego w pakiecie BPM Questetra** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego][14]


8. Na możesz **Questetra BPM pakietu** firmowej witryny wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][15]

    . Zaznacz opcję **Włącz rejestracji jednokrotnej**.
     
    b. W portalu klasyczny Azure skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Identyfikator jednostki** .

    c. W portalu klasyczny Azure skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **adres URL strony — logowania** .

    d. W portalu klasyczny Azure skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **adres URL strony wyrejestrowywania** .

    e. W polu tekstowym **NameID format** wpisz **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: emailAddress**.


    f. Tworzenie pliku zakodowany base-64 z pobranego certyfikatu. 

    >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

    g. Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wklej go w polu tekstowym **certyfikatu sprawdzania poprawności** . 

    h. Kliknij przycisk **Zapisz**.


9. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Co to jest Azure AD Connect][17]


10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Co to jest Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Utwórz użytkownika test Azure AD][100] 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Utwórz użytkownika test Azure AD][101] 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Utwórz użytkownika test Azure AD][102] 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Utwórz użytkownika test Azure AD][103]
 
    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.
  
    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk Dalej.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Utwórz użytkownika test Azure AD][104] 
  
    . W polu tekstowym **Imię** wpisz **Britta**. 
 
    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Utwórz użytkownika test Azure AD][105]  

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Utwórz użytkownika test Azure AD][106]   
  
    . Zanotuj wartość **Nowego hasła**.
  
    b. Kliknij pozycję **pełne**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Tworzenie użytkownika test Questetra BPM pakietu

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta pakietu BPM Questetra.

**Aby utworzyć użytkownika o nazwie Simona Britta pakietu BPM Questetra, wykonaj następujące czynności:**

1.  Logowania do witryny firmy Questetra BPM pakietu jako administrator.
2.  Przejdź do pozycji **ustawień systemu > listy użytkowników > Nowy użytkownik**. 
3.  W oknie dialogowym Nowy użytkownik wykonaj następujące czynności: 

    ![Tworzenie użytkowników testowych][300] 

    . W polu tekstowym **Nazwa** wpisz nazwę użytkownika i Britta w Azure AD.

    b. W polu tekstowym **wiadomości E-mail** wpisz nazwę użytkownika i Britta w Azure AD.

    c. W polu tekstowym **hasło** wpisz hasło.

4.  Kliknij pozycję **Dodaj nowego użytkownika**.



### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej w pakiecie BPM Questetra za pomocą Azure rejestracji jednokrotnej.

![Co to jest Azure AD Connect][200]

**Aby przypisać Simona Britta Questetra BPM pakietu, należy wykonać następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Co to jest Azure AD Connect][201]

2. Na liście aplikacji wybierz opcję **Questetra BPM Suite**.

    ![Co to jest Azure AD Connect][205]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Co to jest Azure AD Connect][202]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

    ![Co to jest Azure AD Connect][203]

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Co to jest Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka pakietu BPM Questetra w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji pakietu BPM Questetra.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 