<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z przodu | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i z przodu."
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


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Samouczek: Azure Active Directory Integracja z przodu

Celem tego samouczka jest pokazano, jak zintegrować wierzch z usługą Azure Active Directory (Azure AD).

Integrowanie wierzch z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do przodu
- Można umożliwić użytkownikom automatyczne pobieranie podpisane na na wierzch (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z przodu, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Wierzch logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie wierzch z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-front-from-the-gallery"></a>Dodawanie wierzch z galerii
Aby skonfigurować integrację Azure AD wierzch, musisz dodać wierzch z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać wierzch z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **przedniej**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. W panelu Wyniki wybierz **przedniej**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z przodu opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi na wierzchu użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi na wierzchu musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **Nazwa użytkownika** na wierzchu.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z przodu, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie z przodu testowanie użytkownika](#creating-a-front-test-user)** — ma odpowiednika Britta Simona na wierzchu połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji wierzch.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z przodu, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie **przedniej** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak czy chcesz użytkowników, aby zalogować się na pierwszym planie** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe Jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    . W polu tekstowym **identyfikator** wpisz adres URL przy użyciu następującego wzorca:`https://<company name>.frontapp.com`

    b. W polu tekstowym **Adres URL odpowiedź** wpisz adres URL przy użyciu następującego wzorca:`https://<company name>.frontapp.com/sso/saml/callback`

    c. Kliknij przycisk **Dalej**

4. Jeśli chcesz skonfigurować aplikację w **SP zainicjowane tryb** na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , następnie kliknij polecenie **"Pokaż zaawansowane ustawienia (opcjonalnie)"** i wpisz adres **Logowania na adres URL** i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca:`https://<company name>.frontapp.com`

    b. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Pamiętaj, że są one wartości rzeczywistych. Musisz zaktualizować te wartości rzeczywiste logowania na adres URL, identyfikator i adres URL odpowiedź. Aby pobrać te wartości, można odwoływać się **kroku 12** , aby uzyskać szczegółowe informacje lub skontaktuj się z przodu za pośrednictwem [support@frontapp.com](emailTo:support@frontapp.com).

5. Na stronie **Konfigurowanie logowania jednokrotnego przodu** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

6. Logowania jednokrotnego do Twojej dzierżawy wierzch jako administrator.

7. Przejdź do pozycji **Ustawienia (koło zębate ikona u dołu lewy pasek boczny) > Preferencje**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Kliknij łącze **Rejestracji jednokrotnej** .

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Wybierz pozycję **SAML** na liście rozwijanej listy **Rejestracji jednokrotnej**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. **Punkt wejścia** pole tekstowe, należy umieścić wartość **Pojedynczy znak na adres URL usługi** z Kreatora konfiguracji aplikacji Azure AD.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. Skopiuj zawartość pliku pobranego certyfikatu, a następnie wklej go w polu tekstowym **certyfikatu podpisywania** .

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Upewnij się, że te adresy URL zgodne konfiguracji w kroku 3.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

13. Kliknij przycisk **Zapisz** .

14. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

15. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-front-test-user"></a>Tworzenie użytkownika test wierzch

Celem w tej sekcji jest Utwórz użytkownika o nazwie Simona Britta w pracy Front.Please z zespołem pomocy technicznej wierzch, aby dodać użytkowników na koncie wierzch.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej na wierzch za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta do przodu, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **przedniej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka z przodu w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji wierzch.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
