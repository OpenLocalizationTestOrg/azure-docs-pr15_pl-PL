<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z RunMyProcess | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i RunMyProcess."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Samouczek: Azure Active Directory Integracja z RunMyProcess

Celem tego samouczka jest pokazano, jak zintegrować RunMyProcess z usługi Azure Active Directory (Azure AD).

Integrowanie RunMyProcess z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do RunMyProcess
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w RunMyProcess (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z RunMyProcess, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- RunMyProcess logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie RunMyProcess z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-runmyprocess-from-the-gallery"></a>Dodawanie RunMyProcess z galerii
Aby skonfigurować integrację Azure AD RunMyProcess, musisz dodać RunMyProcess z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać RunMyProcess z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **RunMyProcess**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. W okienku wyników wybierz **RunMyProcess**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z RunMyProcess opartą na użytkowniku test o nazwie "Britta Simona".

W przypadku logowania jednokrotnego w pracy jest Azure AD musi ustalić użytkownika odpowiednika w RunMyProcess jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w RunMyProcess musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w RunMyProcess.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z RunMyProcess, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie RunMyProcess testowanie użytkownika](#creating-a-runmyprocess-test-user)** — aby odpowiednikiem Simona Britta w RunMyProcess połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji RunMyProcess.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z RunMyProcess, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **RunMyProcess** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do RunMyProcess** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Pamiętaj, że musisz zaktualizować wartości z rzeczywisty logowania na adres URL. Aby uzyskać tę wartość, skontaktuj się z zespołem pomocy technicznej RunMyProcess za pośrednictwem <mailto:support@runmyprocess.com>.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w RunMyProcess** kliknij pozycję **Pobierz certyfikat** , a następnie zapisz plik na komputerze:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. W oknie przeglądarki sieci web innej logowania jednokrotnego do Twojej dzierżawy RunMyProcess jako administrator.

6. W panelu nawigacji po lewej stronie kliknij **konto** , a następnie wybierz pozycję **Konfiguracja**.

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Przejdź do sekcji **metody uwierzytelniania** i wykonaj kroki:

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    . Jako **metody**wybierz pozycję **logowania jednokrotnego z Samlv2**.

    b. W **przekierować logowania jednokrotnego** pole tekstowe, należy umieścić wartość **Adres URL logowania jednokrotnego SAML** z Kreatora konfiguracji aplikacji Azure AD.

    c. W **przekierowywać Wyloguj** pole tekstowe, należy umieścić wartość **Adres URL usługi Sign-Out jednego** z Kreatora konfiguracji aplikacji Azure AD.

    d. W **Formacie identyfikator nazwa** pola tekstowego, należy umieścić wartość **Format identyfikator nazwy** z Kreatora konfiguracji aplikacji Azure AD.

    e. Skopiuj zawartość pliku pobranego certyfikatu, a następnie wklej go w polu tekstowym **certyfikatu** . 

    f. Kliknij ikonę **Zapisz** .
    
8. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

9. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
 
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:  ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** , wykonaj następujące czynności: ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

### <a name="creating-a-runmyprocess-test-user"></a>Tworzenie użytkownika test RunMyProcess
  
Aby umożliwić użytkownikom Azure AD zalogować się do RunMyProcess, musi być przygotowana do RunMyProcess. W przypadku RunMyProcess inicjowania obsługi administracyjnej to zadanie ręczne.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy RunMyProcess jako administrator.

2.  Kliknij **konto** , a w panelu nawigacji po lewej stronie wybierz pozycję **Użytkownicy** , a następnie kliknij polecenie **Nowy użytkownik**.

    ![Nowy użytkownik] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Nowy użytkownik")

3.  W sekcji **Ustawienia użytkownika** wykonaj następujące czynności:

    ![Profil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")

    . Wpisz **nazwę** i **poczty E-mail** działające konto AAD chcesz należy do powiązanych pól tekstowych.

    b. Wybierz **język IDE**, **język** i **profilu**.

    c. Wybierz pozycję **Wyślij wiadomość e-mail tworzenia konta do mnie**.

    d. Kliknij przycisk **Zapisz**.

    >[AZURE.NOTE] Można użyć dowolnego innego RunMyProcess użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez RunMyProcess do świadczenia usługi Azure Active Directory kont użytkowników.
    

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej RunMyProcess za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta RunMyProcess, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **RunMyProcess**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. na pasku u dołu, kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka RunMyProcess w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji RunMyProcess.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
