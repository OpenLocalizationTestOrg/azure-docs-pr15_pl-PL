<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z KnowBe4 | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i KnowBe4."
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


# <a name="tutorial-azure-active-directory-integration-with-knowbe4"></a>Samouczek: Azure Active Directory Integracja z KnowBe4

Celem tego samouczka jest pokazano, jak zintegrować KnowBe4 z usługi Azure Active Directory (Azure AD).  
Integrowanie KnowBe4 z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do KnowBe4
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w KnowBe4 (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralnej — Portal Azure Active Directory

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z KnowBe4, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- KnowBe4 logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie KnowBe4 z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-knowbe4-from-the-gallery"></a>Dodawanie KnowBe4 z galerii
Aby skonfigurować integrację Azure AD KnowBe4, musisz dodać KnowBe4 z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać KnowBe4 z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **KnowBe4**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_01.png)

7. W okienku wyników wybierz **KnowBe4**, a następnie kliknij **Wykonano** w celu dodania aplikacji.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z KnowBe4 opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w KnowBe4 użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w KnowBe4 musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w KnowBe4.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z KnowBe4, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie KnowBe4 testowanie użytkownika](#creating-a-KnowBe4-test-user)** — aby odpowiednikiem Simona Britta w KnowBe4 połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji KnowBe4.



**Aby skonfigurować Azure AD rejestracji jednokrotnej z KnowBe4, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **KnowBe4** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do KnowBe4** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_04.png) 


    . W polu tekstowym logowania na adres URL wpisz adres URL używane przez użytkowników do logowania się w aplikacji KnowBe4 przy użyciu następującego wzorca: **"https://\<nazwy firmy\>.knowbe4.com/auth/saml/aad168.ccsctp.net)"**.


4. Na stronie **Konfigurowanie logowania jednokrotnego w KnowBe4** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z [zespołem pomocy technicznej KnowBe4](mailto:support@knowbe4.com ). Dołączanie pliku pobranego certyfikatu do poczty i udostępnianie z adresami URL metadanych (identyfikator jednostki, logowania jednokrotnego logowania adresu URL i zaloguj się adres URL) KnowBe4 zespołu, aby skonfigurować logowania jednokrotnego na bok.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-knowbe4-test-user"></a>Tworzenie użytkownika test KnowBe4

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w KnowBe4. KnowBe4 obsługuje w czasie inicjowania obsługi administracyjnej, która jest domyślnie włączona.

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Nowy użytkownik zostanie utworzony podczas próby dostępu KnowBe4, jeśli go jeszcze nie istnieje. 

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z obsługą KnowBe4.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej KnowBe4 za pomocą Azure rejestracji jednokrotnej.

    ![Assign User][200] 

**Aby przypisać Simona Britta KnowBe4, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **KnowBe4**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka KnowBe4 w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji KnowBe4.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_205.png
