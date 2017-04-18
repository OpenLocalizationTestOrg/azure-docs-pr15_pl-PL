<properties
    pageTitle="Samouczek: Azure Active Directory integracji z serwerem Tableau | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory a serwerem Tableau."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Samouczek: Azure Active Directory integracji z serwerem Tableau

Celem tego samouczka jest pokazano, jak zintegrować Tableau serwera z usługą Azure Active Directory (Azure AD).

Integrowanie Tableau serwera z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do serwera Tableau
- Możesz włączyć użytkowników, aby automatycznie uzyskać podpisane na serwerze Tableau (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z serwerem Tableau, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Tableau serwera logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie serwera Tableau z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-tableau-server-from-the-gallery"></a>Dodawanie serwera Tableau z galerii
Aby skonfigurować integrację Azure AD Tableau serwera, musisz dodać serwer Tableau z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać serwer Tableau z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 
 
    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Tableau serwera**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. W okienku wyników zaznacz **Serwer Tableau**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z serwerem Tableau opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi na serwerze Tableau użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi na serwerze Tableau musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w polu Serwer Tableau.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z serwerem Tableau, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie serwera Tableau testowanie użytkownika](#creating-a-tableauserver-test-user)** — aby odpowiednikiem Simona Britta w Tableau serwera, który jest połączony z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji serwera Tableau.

Aplikacja serwera TABLEAU oczekuje potwierdzeń SAML w określonym formacie. Następujące zrzut ekranu przedstawia przykładową to. 

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Aby skonfigurować Azure AD rejestracji jednokrotnej z serwerem Tableau, wykonaj następujące czynności:**


1. W portalu klasyczny Azure, na stronie integracji **Tableau serwera** aplikacji w menu u góry kliknij **atrybutów**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. W oknie dialogowym **atrybuty token SAML** wykonaj następujące czynności:

    

    . Kliknij przycisk **Dodaj atrybut użytkownika** , aby otworzyć okno dialogowe **Dodawanie Attribure użytkownika** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. W polu tekstowym **Nazwa Attrubute** wpisz **nazwę użytkownika**.

    c. Z listy **Wartości atrybutu** selsect **user.displayname**.

    d. Kliknij pozycję **pełne**.  
    



1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 



2. Na stronie **jak chcesz użytkowników do zalogowania się serwerze Tableau** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    . W polu tekstowym **Adres URL logowania** wpisz adres URL serwera Tableau. 

    b. W identyfikator polu Kopiuj 

    c. Kliknij przycisk **Dalej**


4. Na stronie **Konfigurowanie logowania jednokrotnego na serwerze Tableau** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


6. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, musisz logowania jednokrotnego do Twojej dzierżawy Tableau Server jako administrator.

    . W konfiguracji serwera Tableau kliknij kartę **SAML** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Zaznacz pole wyboru **Użyj SAML dla rejestracji jednokrotnej**.

    c. Zlokalizuj plik metadanych Federacji pobrane z portal Azure klasyczny, a następnie przekazanie go w **pliku metadanych protokołu Idp SAML**.

    d. Serwer TABLEAU zwrócić adres URL — adres URL serwera Tableau użytkownicy będą uzyskiwać dostęp do, takich jak http://tableau_server. Używanie http://localhost nie jest zalecane. Za pomocą adresu URL przy użyciu ukośnika (na przykład http://tableau_server/) nie jest obsługiwane. Kopiowanie **serwer Tableau zwrócić adresu URL** i wklejanie go do Azure AD **Logowania na adres URL** w polu tekstowym, jak pokazano w kroku 3

    e. Identyfikator jednostki SAML — instalacją serwera Tableau do protokołu IdP identyfikuje identyfikator jednostki. Możesz wprowadzić adres URL serwera Tableau ponownie, jeśli chcesz, ale nie ma być adres URL serwera Tableau. **Identyfikator jednostki SAML** skopiuj i wklej go do pola tekstowego Azure AD **IDENTYFIKATOREM** , jak przedstawiono w kroku 3.

    f. Kliknij pozycję **Eksportuj plik metadanych** i otwórz go w aplikacji do edycji tekstu. Znajdź adres URL usługi dla klientów indywidualnych potwierdzenia z Http Post i indeksu 0, a następnie skopiuj adres URL. Teraz wkleić go do pola tekstowego Azure AD **Adres URL Odpowiedz** jak przedstawiono w kroku 3. 

    g. Kliknij przycisk **OK** na stronie Tableau Configiuration serwera.

    > [AZURE.NOTE] Jeśli potrzebujesz pomocy w konfigurowaniu SAML na serwerze Tableau następnie można znaleźć w tym artykule [Konfigurowanie SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**. 
 
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.

    b. W polu tekstowym **Nazwa użytkownika** wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-tableau-server-test-user"></a>Tworzenie użytkownika test Tableau serwera

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta Tableau Server. Należy zapewnić wszystkich użytkowników na serwerze Tableau. Pamiętaj również tej nazwy użytkownika użytkownika powinny być zgodne wartości, który skonfigurowano w atrybucie niestandardowe Azure AD **nazwę użytkownika**. Prawidłowe mapowanie Integracja powinna działać [Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z administratorem serwera Tableau w Twojej organizacji.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Tableau Server za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Tableau serwera, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.
 
    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji wybierz **Serwer Tableau**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Tableau serwera w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji serwera Tableau.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
