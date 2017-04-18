<properties
    pageTitle="Samouczek: Azure Active Directory integracji z oprogramowaniem chlorowców"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i chlorowców oprogramowania."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Samouczek: Azure Active Directory integracji z oprogramowaniem chlorowców

Celem tego samouczka jest pokazano, jak zintegrować chlorowców oprogramowania z usługi Azure Active Directory (Azure AD).

Integrowanie oprogramowanie chlorowców z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do oprogramowania chlorowców 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w oprogramowania chlorowców (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z oprogramowaniem halogenowe, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Oprogramowanie chlorowców logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie oprogramowania chlorowców z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-halogen-software-from-the-gallery"></a>Dodawanie oprogramowania chlorowców z galerii
Aby skonfigurować integrację Azure AD chlorowców oprogramowania, musisz dodać chlorowców oprogramowania z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać chlorowców oprogramowania z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony. 

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **chlorowców oprogramowania**.

    ![Aplikacje][5]

7. W okienku wyników wybierz **Chlorowców oprogramowanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z oprogramowaniem chlorowców opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w oprogramowaniu chlorowców użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w oprogramowaniu chlorowców musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w oprogramowaniu halogenowe.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z oprogramowaniem halogenowe, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD pojedynczego logowania jednokrotnego](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie oprogramowania chlorowców testowanie użytkownika](#creating-a-halogen-software-test-user)** — mają odpowiednikiem Simona Britta w halogenowe oprogramowanie, które jest połączone z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurowanie Azure AD pojedynczego logowania jednokrotnego

Celem w tej sekcji jest włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji chlorowców oprogramowanie.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z oprogramowaniem halogenowe, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracja **Oprogramowania chlorowców** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][8]

2. Na stronie **jak chcesz użytkownikom logowanie do oprogramowania chlorowców** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][9]

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:  ![Konfigurowanie ustawień aplikacji][10]
 
     . w polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji oprogramowania chlorowców przy użyciu następującego wzorca: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w halogenowe oprogramowanie** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.
    
    ![Co to jest Azure AD Connect][11]

5. W oknie przeglądarki różnych logowania jednokrotnego w aplikacji **Oprogramowanie chlorowców** jako administrator.

6. Kliknij kartę **Opcje** . 

    ![Co to jest Azure AD Connect][12]


7. W okienku nawigacji po lewej stronie kliknij pozycję **Konfiguracja SAML**. 

    ![Co to jest Azure AD Connect][13]

8. Na stronie **Konfiguracja SAML** wykonaj następujące czynności:  ![co to jest Azure AD Connect][14]

    . **Unikatowy identyfikator**zaznacz **NameID**.

    b. Jako **Mapy Unikatowy identyfikator do**wybierz **nazwę użytkownika**.

    c. Aby przekazać metadanych pobrany plik, kliknij przycisk **Przeglądaj** , aby wybrać plik, a następnie polecenie **Przekaż plik**.

    d. Aby przetestować konfigurację, kliknij pozycję **Uruchom testowanie**. 

    > [AZURE.NOTE] Trzeba będzie zaczekać komunikatu "*SAML test został zakończony. Zamknij to okno*". Następnie zamknij okno przeglądarki otwarte. Pole wyboru **Włącz SAML** jest dostępne tylko, jeśli wynik testu została wykonana.

    e. Zaznacz opcję **Włącz SAML**.
    
    f. Kliknij przycisk **Zapisz zmiany**. 


9. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** . 

    ![Co to jest Azure AD Connect][15]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Co to jest Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Co to jest Azure AD Connect][100] 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Co to jest Azure AD Connect][101] 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Co to jest Azure AD Connect][102] 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Co to jest Azure AD Connect][103] 
 
    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk Dalej.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Co to jest Azure AD Connect][104] 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** txtbox, typ **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Co to jest Azure AD Connect][105]  

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Co to jest Azure AD Connect][106]   

    . Zanotuj wartość **Nowego hasła**.
    b. Kliknij pozycję **pełne**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Tworzenie użytkownika testowanie oprogramowania chlorowców

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w oprogramowaniu halogenowe.

**Aby utworzyć użytkownika o nazwie Simona Britta w oprogramowaniu halogenowe, wykonaj następujące czynności:**

1. Logowanie się do aplikacji **Oprogramowania chlorowców** jako administrator.

2. Kliknij kartę **Centrum użytkownika** , a następnie kliknij pozycję **Utwórz użytkownika**.

    ![Co to jest Azure AD Connect][300]  

3. Na stronie okno dialogowe **Nowy użytkownik** wykonaj następujące czynności:

    ![Co to jest Azure AD Connect][301]

    . W polu tekstowym **Imię** wpisz **Britta**. 
  
    b. W polu tekstowym **Nazwa** wpisz **Simona**.
  
    c. W polu tekstowym **Nazwa użytkownika** wpisz **nazwę użytkownika Simona Brita w portalu klasyczny Azure**.
  
    d. W polu tekstowym **hasło** wpisz hasło dla Britta.
  
    e. Kliknij przycisk **Zapisz**.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej oprogramowania chlorowców za pomocą Azure rejestracji jednokrotnej.

![Co to jest Azure AD Connect][200]

**Aby przypisać Simona Britta do oprogramowania halogenowe, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Co to jest Azure AD Connect][201]

2. Na liście aplikacji zaznacz **Chlorowców oprogramowania**.

    ![Co to jest Azure AD Connect][202]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Co to jest Azure AD Connect][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

    ![Co to jest Azure AD Connect][204]

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Co to jest Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka chlorowców oprogramowania w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji chlorowców oprogramowania.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png