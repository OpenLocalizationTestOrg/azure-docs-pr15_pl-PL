<properties
    pageTitle="Samouczek: Azure Active Directory integracji z oprogramowaniem HR Cezanne | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Cezanne HR oprogramowania."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Samouczek: Azure Active Directory integracji z oprogramowaniem HR Cezanne

Celem tego samouczka jest pokazano, jak zintegrować Cezanne HR oprogramowania z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure Active Directory Cezanne HR oprogramowanie udostępnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do oprogramowania HR Cezanne
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Cezanne oprogramowania HR (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z oprogramowaniem HR Cezanne, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Oprogramowanie HR Cezanne logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie oprogramowania HR Cezanne z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Dodawanie oprogramowania HR Cezanne z galerii
Aby skonfigurować integrację Azure AD Cezanne HR oprogramowanie, musisz dodać oprogramowanie HR Cezanne z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Cezanne HR oprogramowania z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Cezanne HR oprogramowania**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. W panelu Wyniki wybierz **Cezanne HR oprogramowanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z oprogramowaniem HR Cezanne opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w oprogramowaniu HR Cezanne użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w oprogramowaniu HR Cezanne musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w oprogramowaniu HR Cezanne.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z oprogramowaniem HR Cezanne, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie oprogramowania HR Cezanne testowanie użytkownika](#creating-a-cezanne-hr-software-test-user)** — aby odpowiednikiem Simona Britta w Cezanne HR oprogramowanie, które jest połączone z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji Włączanie Azure AD logowania jednokrotnego w portalu klasyczny i konfigurowanie logowania jednokrotnego w HR Cezanne w aplikacji.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z oprogramowaniem HR Cezanne, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Cezanne HR oprogramowania** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do oprogramowania HR Cezanne** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL przy użyciu następującego wzorca: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Pamiętaj, że musisz zaktualizować te wartości rzeczywiste na adres URL logowania i adres URL odpowiedzi. Aby uzyskać te wartości, skontaktuj się z zespołem pomocy technicznej Cezanne HR oprogramowania za pośrednictwem <mailto:info@cezannehr.com>.

4. Na stronie **Konfigurowanie logowania jednokrotnego w Cezanne HR oprogramowania** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

5. W oknie przeglądarki sieci web innej logowania jednokrotnego do Twojej dzierżawy oprogramowanie HR Cezanne jako administrator.

6. W okienku nawigacji po lewej stronie kliknij pozycję **Konfiguracji systemu**. Przejdź do **ustawień zabezpieczeń**. Następnie przejdź do **Konfiguracji rejestracji jednokrotnej**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. **Zezwalaj użytkownikom na Zaloguj się przy użyciu następującej usługi pojedynczego logowania jednokrotnego (SSO)** panel zaznacz pole **SAML 2.0** i wybierz opcję **Konfiguracja zaawansowana** obok niej.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Kliknij przycisk **Dodaj nowy** .

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. W sekcji **Dostawcy tożsamości SAML 2.0** , należy wykonać następujące czynności.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    . Wprowadź nazwę dostawcy tożsamości jako **Nazwa wyświetlana**.

    b. W polu **Identyfikator** pole tekstowe, należy umieścić wartości **Identyfikatora obiektu** z Kreatora konfiguracji aplikacji Azure AD.

    c. Zmień **powiązanie SAML** "POST".

    d. W **Punktu końcowego usługi tokenu zabezpieczeń** pole tekstowe, należy umieścić wartość **Pojedynczy znak na adres URL usługi** z Kreatora konfiguracji aplikacji Azure AD.

    e. Wprowadź "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" w polu **Nazwa atrybutu identyfikator użytkownika**.

    f. Kliknij ikonę **przekazywanie** do przekazania pobranego certyfikatu z usługi Azure Active Directory.

    g. Kliknij przycisk **Ok** . 

10. Kliknij przycisk **Zapisz** .

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

12. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu tekstowym **Nazwa** wpisz **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Tworzenie użytkownika test Cezanne HR oprogramowania

Aby umożliwić użytkownikom Azure AD zalogować się do oprogramowania HR Cezanne, musi być przygotowana do Cezanne HR oprogramowania. W przypadku oprogramowania HR Cezanne inicjowania obsługi administracyjnej to zadanie ręczne.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkownika, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy Cezanne HR oprogramowania jako administrator.

2.  W okienku nawigacji po lewej stronie kliknij pozycję **Konfiguracji systemu**. Przejdź do **zarządzania użytkownikami**. Następnie przejdź do **Dodawania nowego użytkownika**.

    ![Nowy użytkownik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Nowy użytkownik")

3.  W sekcji **Szczegółowe informacje o osobie** wykonaj kroki:

    ![Nowy użytkownik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Nowy użytkownik")

    . Ustawianie **wewnętrznego użytkownika** jako wyłączone.

    b. W polu tekstowym **Imię** wpisz **Britta**.  

    c. W polu tekstowym **Nazwa** wpisz **Simona**.

    d. W polu tekstowym **wiadomości E-mail** wpisz adres e-mail konta Simona Britta.

4.  W sekcji **Informacje o koncie** wykonaj kroki:

    ![Nowy użytkownik] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Nowy użytkownik")

    . W polu tekstowym **Nazwa użytkownika** wpisz adres e-mail Simona Britta.

    b. W polu tekstowym **hasło** wpisz hasło konta Simona Britta.

    c. Wybierz pozycję **HR Professional** rolę **zabezpieczeń**.

    d. Kliknij **przycisk OK**.

5. Przejdź do karty **Logowania jednokrotnego** i wybierz pozycję **Dodaj nowy** w obszarze **SAML 2.0 identyfikatory** .

    ![Użytkownika] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Użytkownika")

6. Wybierz dostawcę tożsamości dla **Dostawcy tożsamości** i w polu tekstowym **Identyfikator użytkownika**, wprowadź adres e-mail konta Simona Britta.

    ![Użytkownika] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Użytkownika")
    
7. Kliknij przycisk **Zapisz** .

    ![Użytkownika] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Użytkownika")


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Cezanne HR oprogramowania za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta Cezanne HR oprogramowania, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **Cezanne HR oprogramowania**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]

### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka Cezanne HR oprogramowania w panelu programu Access, możesz należy uzyskać automatycznie podpisane w HR Cezanne w aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
