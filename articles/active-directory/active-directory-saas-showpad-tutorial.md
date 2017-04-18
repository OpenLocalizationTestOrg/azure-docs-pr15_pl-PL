<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Showpad | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Showpad."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Samouczek: Azure Active Directory Integracja z Showpad

Celem tego samouczka jest pokazano, jak zintegrować Showpad z usługi Azure Active Directory (Azure AD).

Integrowanie Showpad z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Showpad
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Showpad (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralnej — Portal Azure Active Directory

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Showpad, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Subskrypcja Showpad


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Showpad z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-showpad-from-the-gallery"></a>Dodawanie Showpad z galerii
Aby skonfigurować integrację Azure AD Showpad, musisz dodać Showpad z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Showpad z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Aplikacje][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.
 
    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Showpad**.

    ![Aplikacje](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. W okienku wyników wybierz **Showpad**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Showpad opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w Showpad użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Showpad musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Showpad.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Showpad, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie Showpad testowanie użytkownika](#creating-a-showpad-test-user)** — aby odpowiednikiem Simona Britta w Showpad połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Showpad.



**Aby skonfigurować Azure AD rejestracji jednokrotnej z Showpad, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **Showpad** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do Showpad** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji Showpad przy użyciu następującego wzorca:`https://<company name>.showpad.biz/login`

    b. W polu tekstowym **identyfikator** wpisz adres URL, przy użyciu następującego wzorca:`https://<company name>.showpad.biz`

    c. Kliknij przycisk **Dalej**


4. Na stronie **Konfigurowanie logowania jednokrotnego w Showpad** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Logowania jednokrotnego do Twojej dzierżawy Showpad jako administrator.

6. W menu u góry kliknij przycisk **Ustawienia**.

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Przejdź do "**Logowania jednokrotnego**" i kliknij przycisk "**Włącz**".
 
    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. W oknie dialogowym **Dodawanie usługi SAML 2.0** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    . W polu tekstowym **Nazwa** wpisz nazwę dostawcy identyfikator (przykład: Nazwa firmy).

    b. Jako **Źródło metadanych**zaznacz pole wyboru **XML**.

    c. Skopiuj zawartość metadanych pobrany plik XML, a następnie wklej go w polu tekstowym **XML metadanych** .

    d. Wybierz pozycję **konta automatycznie świadczenia dla nowych użytkowników po zalogowaniu się na**.

    e. Kliknij przycisk **Prześlij**.


10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]


11. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  
    ![Azure AD rejestracji jednokrotnej][11]






### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego Showpad w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    . W polu tekstowym **Nazwa użytkownika** wpisz **BrittaSimon**.

    b. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Jako **roli**wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.


### <a name="creating-a-showpad-test-user"></a>Tworzenie użytkownika test Showpad

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Showpad. 

Showpad obsługuje w czasie inicjowania obsługi administracyjnej. Włączono obsługi administracyjnej w **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**. 

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. 




### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Showpad za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta Showpad, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, w menu u góry kliknij pozycję **aplikacje**.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji kliknij pozycję **Showpad**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]




### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka **Showpad** w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Showpad.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
