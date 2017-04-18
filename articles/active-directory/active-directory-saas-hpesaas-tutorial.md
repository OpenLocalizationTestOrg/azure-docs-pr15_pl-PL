<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z władz akredytacji bezpieczeństwa HPE | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i HPE władz akredytacji bezpieczeństwa."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hpe-saas"></a>Samouczek: Azure Active Directory Integracja z HPE władz akredytacji bezpieczeństwa

Celem tego samouczka jest pokazano, jak zintegrować władz akredytacji bezpieczeństwa HPE z usługi Azure Active Directory (Azure AD).  
Integrowanie HPE władz akredytacji bezpieczeństwa z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do HPE władz akredytacji bezpieczeństwa
- Można umożliwić użytkownikom automatyczne pobieranie podpisana w HPE władz akredytacji bezpieczeństwa (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure


Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z HPE władz akredytacji bezpieczeństwa, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- HPE władz akredytacji bezpieczeństwa logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie władz akredytacji bezpieczeństwa HPE z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-hpe-saas-from-the-gallery"></a>Dodawanie władz akredytacji bezpieczeństwa HPE z galerii
Aby skonfigurować integrację Azure AD HPE władz akredytacji bezpieczeństwa, musisz dodać HPE władz akredytacji bezpieczeństwa z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać HPE władz akredytacji bezpieczeństwa z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **HPE władz akredytacji bezpieczeństwa**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_01.png)

7. W okienku wyników wybierz **HPE władz akredytacji bezpieczeństwa**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z władz akredytacji bezpieczeństwa HPE opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w HPE władz akredytacji bezpieczeństwa użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w HPE władz akredytacji bezpieczeństwa musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w HPE władz akredytacji bezpieczeństwa.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z HPE władz akredytacji bezpieczeństwa, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie HPE SaaS testowanie użytkownika](#creating-a-hpesaas-test-user)** — aby odpowiednikiem Simona Britta w HPE władz akredytacji bezpieczeństwa, połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji HPE władz akredytacji bezpieczeństwa.



**Aby skonfigurować Azure AD rejestracji jednokrotnej z HPE władz akredytacji bezpieczeństwa, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **HPE władz akredytacji bezpieczeństwa** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do władz akredytacji bezpieczeństwa HPE** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_04.png) 


    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji HPE władz akredytacji bezpieczeństwa: **"https://login.saas.hpe.com/msg"**. Klientów można również zmienić to do określonego adresu URL aplikacji.

    b. Kliknij przycisk **Dalej**.


4. Na stronie **Konfigurowanie logowania jednokrotnego w HPE władz akredytacji bezpieczeństwa** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_05.png) 

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej HPE władz akredytacji bezpieczeństwa i pocztę e-mail Dołącz metadanych pobrany plik. Tak, aby można je skonfigurować do integracji logowania jednokrotnego.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   




### <a name="creating-a-hpe-saas-test-user"></a>Tworzenie użytkownika test HPE władz akredytacji bezpieczeństwa

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w HPE władz akredytacji bezpieczeństwa. We współpracy z zespołem pomocy technicznej HPE władz akredytacji bezpieczeństwa, aby dodać użytkowników na koncie HPE władz akredytacji bezpieczeństwa. 


> [AZURE.NOTE]Jeśli trzeba ręcznie utworzyć użytkownika, musisz skontaktuj się z zespołem pomocy technicznej HPE władz akredytacji bezpieczeństwa.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej HPE władz akredytacji bezpieczeństwa za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta HPE władz akredytacji bezpieczeństwa, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **HPE władz akredytacji bezpieczeństwa**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka HPE władz akredytacji bezpieczeństwa w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji HPE władz akredytacji bezpieczeństwa.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_205.png
