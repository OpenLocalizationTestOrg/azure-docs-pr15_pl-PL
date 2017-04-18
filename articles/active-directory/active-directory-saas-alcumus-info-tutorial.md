<properties
    pageTitle="Samouczek: Azure Active Directory integracji z programem Exchange informacje Alcumus | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i wymiany informacji o Alcumus."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Samouczek: Azure Active Directory integracji z programem Exchange informacje Alcumus

Celem tego samouczka jest pokazano, jak zintegrować Alcumus wymiany informacji z usługi Azure Active Directory (Azure AD).  
Integrowanie Alcumus wymiany informacji z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do wymiany informacji o Alcumus 
- Można umożliwić użytkownikom automatyczne pobieranie podpisana w wymiany informacji o Alcumus (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z programem Exchange informacje Alcumus, potrzebne następujące elementy:

- Subskrypcję usługi [Azure AD](https://azure.microsoft.com/)
- [Alcumus Exchange informacje](http://www.alcumusgroup.com/) logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie Alcumus wymiany informacji z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Dodawanie Alcumus wymiany informacji z galerii
Aby skonfigurować integrację Azure AD Alcumus informacje w programie Exchange, musisz dodać Alcumus wymiany informacji z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Alcumus wymiany informacji z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Alcumus wymiany informacji**.

    ![Aplikacje][5]

7. W okienku wyników wybierz pozycję **Exchange informacje Alcumus**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z serwerem Exchange informacje Alcumus opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w usłudze Exchange informacje Alcumus użytkownika w Azure AD. Innymi słowy łącze relacji między użytkownikiem usługi Azure AD i powiązanych użytkownika w usłudze Exchange informacje Alcumus musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w usłudze Exchange informacje Alcumus.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z serwerem Exchange informacje Alcumus, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenia wymiany informacji o Alcumus testowanie użytkownika](#creating-a-alcumus-info-exchange-test-user)** — aby odpowiednikiem Simona Britta w wymiany informacji o Alcumus, który jest połączony z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji programu Exchange informacje Alcumus.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z serwerem Exchange informacje Alcumus, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie Integracja z programem **Exchange informacje Alcumus** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6]

2. Na stronie **jak chcesz użytkownikom logowanie do programu Exchange informacje Alcumus** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][7]

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności: 

    ![Azure AD rejestracji jednokrotnej][8]
 
    . w polu tekstowym **Adres URL odpowiedź** wpisz adres URL dla klientów indywidualnych, który był konfiguracji dla Ciebie przez zespół pomocy technicznej Alcumus wymiany informacji.

    > [AZURE.NOTE] Jeśli nie wiesz, co to jest wartość prawej strony, skontaktuj się z zespołem pomocy technicznej Alcumus wymiany informacji za pośrednictwem [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego po wymiany informacji o Alcumus** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Co to jest Azure AD Connect][9]

5. Skontaktuj się z zespołem pomocy technicznej Alcumus wymiany informacji za pośrednictwem [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), podaj im pliku metadanych i ich Poinformuj należy pozwalający rejestracji Jednokrotnej dla Ciebie.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Co to jest Azure AD Connect][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Co to jest Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.
  
    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.
  
    c. Kliknij przycisk Dalej.



6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    . W polu tekstowym **Imię** wpisz **Britta**.  
  
    b. W polu **Nazwisko** txtbox, typ **Simona**.
  
    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.
  
    d. Na liście **ról** wybierz **użytkownika**.
  
    e. Kliknij przycisk **Dalej**.


7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.
  
    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Tworzenie użytkownika test wymiany informacji o Alcumus

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Alcumus informacje w programie Exchange.

**Aby utworzyć użytkownika o nazwie Simona Britta w Alcumus informacje w programie Exchange, wykonaj następujące czynności:**

1. Skontaktuj się z zespołem pomocy technicznej Alcumus wymiana informacji za pomocą [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Alcumus wymiana informacji za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta do wymiany informacji o Alcumus, wykonaj następujące czynności:**

1. Portal Azure aby otworzyć widok aplikacji w widoku katalogu, wybierz polecenie **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji wybierz pozycję **Exchange informacje Alcumus**.

    ![Przypisywanie użytkownika][202]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka wymiany informacji o Alcumus w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Alcumus wymiany informacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png