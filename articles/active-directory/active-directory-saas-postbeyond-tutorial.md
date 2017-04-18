<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z PostBeyond | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i PostBeyond."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a>Samouczek: Azure Active Directory Integracja z PostBeyond

W tym samouczku dowiesz się, jak zintegrować PostBeyond z usługą Azure Active Directory (Azure AD).

Integrowanie PostBeyond z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do PostBeyond
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w PostBeyond (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z PostBeyond, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- **PostBeyond** logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie PostBeyond z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-postbeyond-from-the-gallery"></a>Dodawanie PostBeyond z galerii
Aby skonfigurować integracja PostBeyond w usłudze Azure Active Directory, musisz dodać PostBeyond z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać PostBeyond z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **PostBeyond**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_01.png)

7. W okienku wyników wybierz **PostBeyond**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z PostBeyond opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w PostBeyond jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w PostBeyond musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w PostBeyond.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z PostBeyond, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie PostBeyond testowanie użytkownika](#creating-a-PostBeyond-test-user)** — aby odpowiednikiem Simona Britta w PostBeyond połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji PostBeyond.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z PostBeyond, wykonaj następujące czynności:**

1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego][6]

2. W portalu klasyczny na stronie integracji **PostBeyond** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7] 

3. Na stronie **jak chcesz użytkownikom logowanie do PostBeyond** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_06.png)

4. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_07.png)


    . W polu tekstowym logowania na adres URL wpisz adres URL przy użyciu następującego wzorca: `https://app.postbeyond.com`. 

    b. Kliknij przycisk **Dalej**.

5. Na stronie **Konfigurowanie logowania jednokrotnego w PostBeyond** kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze. Także kopiować wystawcy adres URL, adres URL usługi rejestracji jednokrotnej i wartości adres URL usługi wyrejestrowywania pojedynczej. Konieczne będzie udostępniać te informacje PostBeyond pomocy technicznej, aby uzyskać logowania jednokrotnego skonfigurowane.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_08.png)

6. Uzyskanie logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej PostBeyond w <sso@postbeyond.com>. Pomaga ich właściwego kanału Konfigurowanie logowania jednokrotnego i podaj im następujące czynności: 

    - Pobrany certyfikatu
    - Adres **URL wystawcy**
    - Adres **URL logowania jednokrotnego SAML**
    - **Adres URL usługi wyrejestrowywania pojedynczy**

7. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-postbeyond-test-user"></a>Tworzenie użytkownika PostBeyond test

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w PostBeyond. Jeśli nie wiesz, jak dodawać Simona Britta w PostBeyond, we współpracy z zespołem pomocy technicznej PostBeyond Aby dodać użytkowników testowych i włączyć logowania jednokrotnego. Skontaktuj się z nimi w <sso@postbeyond.com>.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej PostBeyond za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta PostBeyond, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **PostBeyond**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_09.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka PostBeyond w panelu programu Access należy uzyskać do PostBeyond strona logowania. Wybierz polecenie **Zaloguj się przy użyciu usługi Office 365**, wprowadź poświadczenia Azure AD. Następnie możesz powinny być zalogowany do PostBeyond.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_205.png
