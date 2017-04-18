<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z @Task| Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i @Task."
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


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Samouczek: Azure Active Directory Integracja z@Task

Celem tego samouczka jest pokazano, jak zintegrować @Task z usługą Azure Active Directory (Azure AD).  
Integrowanie @Task z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do@Task
- Możesz włączyć użytkowników, aby automatycznie pobierać podpisane w @Task (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD Integracja z @Task, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- @Task Logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie @Task z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-task-from-the-gallery"></a>Dodawanie @Task z galerii
Aby skonfigurować integracji @Task do Azure AD, musisz dodać @Task z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać @Task z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1] 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2] 

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3] 

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4] 

6. W polu wyszukiwania wpisz **@Task**.

    ![Aplikacje][5] 

7. W okienku wyników wybierz **@Task**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Aplikacje][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego

Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z @Task opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy, Azure AD musi wiedzieć, jakie użytkownika odpowiednika w @Task do użytkownika w Azure AD jest. Innymi słowy, relacji łącza między użytkownikiem usługi Azure AD i użytkownikowi w @Task musi zostać ustanowione.   
Tej relacji łącze nawiązano przypisując wartość **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w @Task.
 
Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z @Task, , trzeba wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie @Tasktest użytkownika](#creating-a-halogen-software-test-user) ** — ma odpowiednika Simona Britta w @Taskthat jest połączony z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w sieci @Task aplikacji.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z @Task, należy wykonać następujące czynności:**

1. W portalu klasyczny Azure na **@Task** integracji aplikacji kliknij przycisk **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na **jak chcesz użytkowników do zalogowania się @Task ** strony, zaznacz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Azure AD rejestracji jednokrotnej][7] 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie ustawień aplikacji][8] 
 
     . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania do usługi @Task aplikacji (np.:*https://<Tenant name>.attask ondemand.com*).

     b. Kliknij przycisk **Dalej**.

4. Na **Konfigurowanie logowania jednokrotnego w @Task ** strony, kliknij przycisk **Pobierz metadane**, Zapisz plik metadane lokalnie na komputerze i kliknij przycisk **Dalej**.

    ![Co to jest Azure AD Connect][9] 



1. Logowania do usługi @Task firmowej witryny jako administrator.

2. Przejdź do **rejestracji jednokrotnej na konfiguracji**.


1. W oknie **Logowania jednokrotnego** wykonaj następujące czynności

    ![Konfigurowanie logowania jednokrotnego][23]

    . Jako **Typ**wybierz pozycję **SAML 2.0**.

    b. Wybierz **Identyfikator dostawcy usługi**.

    c. W portalu klasyczny Azure Skopiuj **Adres URL logowania zdalnego**, a następnie wklej go w polu tekstowym **Adres URL portalu logowania** .

    d. W portalu klasyczny Azure Skopiuj **Adres URL usługi Sign-Out pojedynczej**, a następnie wklej go w polu tekstowym **Adres URL Sign-Out** .

    e. W portalu klasyczny Azure Skopiuj **Zmień adres URL hasło**, a następnie wklej go w polu tekstowym **Zmień adres URL hasła** .

    f. Kliknij przycisk **Zapisz**.

6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Co to jest Azure AD Connect][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Co to jest Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-an-task-test-user"></a>Tworzenie @Task użytkowników testowych

Celem w tej sekcji jest utworzyć użytkownika o nazwie Simona Britta w @Task.


**Aby utworzyć użytkownika o nazwie Simona Britta w @Task, należy wykonać następujące czynności:**

1. Zaloguj się do swojego @Task firmowej witryny jako administrator.

2. W menu u góry kliknij pozycję **Kontakty**.

3. Kliknij **nową osobę**. 

4. W oknie dialogowym nowej osoby wykonaj następujące czynności:

    ![Tworzenie @Task użytkowników testowych][21] 

    . W polu tekstowym **Imię** wpisz "Britta".

    b. W polu tekstowym **Nazwa** wpisz "Simona".

    c. W polu tekstowym **Adres E-mail** wpisz adres e-mail Simona Britta w usłudze Azure Active Directory.

    d. Kliknij przycisk **Dodaj osoby**.




### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta używanie Azure rejestracji jednokrotnej, aby umożliwić jej dostęp do @Task.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta do @Task, należy wykonać następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **@Task**.

    ![Przypisywanie użytkownika][202] 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu przycisku @Task kafelka w panelu programu Access należy uzyskać automatycznie podpisane w swojej @Task aplikacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






