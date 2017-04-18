<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Tangoe polecenie Premium Mobile | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Tangoe polecenie Premium Mobile."
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


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Samouczek: Azure Active Directory Integracja z Tangoe polecenie Premium Mobile

W tym samouczku dowiesz się, jak integracji Mobile Premium polecenie Tangoe z usługi Azure Active Directory (Azure AD).

Integrowanie Tangoe polecenie Premium Mobile z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Tangoe polecenie Premium Mobile
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Tangoe polecenie Premium Mobile (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Tangoe polecenie Premium Mobile, potrzebne następujące elementy:

- Subskrypcję usługi Azure
- Telefon komórkowy Premium polecenie Tangoe logowania jednokrotnego enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Mobile Premium polecenie Tangoe z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Dodawanie Mobile Premium polecenie Tangoe z galerii
Aby skonfigurować integrację Azure AD Tangoe polecenie Premium Mobile, musisz dodać Tangoe polecenie Premium Mobile z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Tangoe polecenie Premium Mobile z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Tangoe polecenie Premium Mobile**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. W okienku wyników wybierz **Tangoe polecenie Premium Mobile**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Tangoe polecenie Premium Mobile opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w Tangoe polecenie Premium Mobile jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Tangoe polecenie Premium Mobile musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Tangoe polecenie Premium Mobile.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Tangoe polecenie Premium Mobile, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Mobile Premium polecenie Tangoe testowanie użytkownika](#creating-an-tangoe-test-user)** — mają odpowiednikiem Simona Britta w Tangoe polecenie Premium Mobile połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji Mobile Premium polecenie Tangoe.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z Tangoe polecenie Premium Mobile, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Tangoe polecenie Premium Mobile** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6]


2. Na stronie **jak chcesz użytkownikom logowanie do Tangoe polecenie Premium Mobile** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji Mobile Premium polecenia Tangoe przy użyciu następującego wzorca: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<wystawcy dzierżawy\>& docelowej =\<docelowy adres URL strony\>"**.

    b. W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL w następujący sposób: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Jeśli nie znasz poprawne wartości dla adresów URL, możesz użyć wartości powyżej jako symbole zastępcze i żądania skojarzyć prawidłowe wartości z usługi Tangoe dział obsługi klienta.


4. Na stronie **Konfigurowanie logowania jednokrotnego telefonem komórkowym Premium polecenie Tangoe** wykonaj następujące czynności:
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5.  Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z Tangoe obsługi klienta skojarzyć i podaj następujące czynności:


    - Plik pobrany metadanych
    - Adres **URL wystawcy**
    - Adres **URL logowania jednokrotnego SAML**
    - **Adres URL usługi wyrejestrowywania pojedynczy**


  
6. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  
    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Tworzenie użytkownika test Tangoe polecenie Premium Mobile

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Tangoe polecenie Premium Mobile. Aplikacji Tangoe polecenie Premium Mobile muszą wszyscy użytkownicy mają być obsługi administracyjnej aplikacji przed wykonaniem rejestracji jednokrotnej. Praca, zapoznaj się z tak z Skojarz obsługi klienta Tangoe do zapewniania obsługi tych użytkowników do aplikacji. 


> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika lub partii użytkowników, należy skontaktować się z obsługą Tangoe polecenie Premium Mobile.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Tangoe polecenie Premium Mobile za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Tangoe polecenie Premium Mobile, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Tangoe polecenie Premium Mobile**.

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka Tangoe polecenie Premium Mobile w panelu programu Access możesz należy uzyskiwanie automatycznie podpisane w aplikacji Mobile Premium polecenie Tangoe.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
