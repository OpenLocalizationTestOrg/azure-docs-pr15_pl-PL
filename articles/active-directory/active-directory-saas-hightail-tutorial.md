<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Hightail | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Hightail."
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


# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Samouczek: Azure Active Directory Integracja z Hightail

Celem tego samouczka jest pokazano, jak zintegrować Hightail z usługi Azure Active Directory (Azure AD).

Integrowanie Hightail z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Hightail
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Hightail (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Hightail, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Hightail logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Hightail z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-hightail-from-the-gallery"></a>Dodawanie Hightail z galerii
Aby skonfigurować integrację Azure AD Hightail, musisz dodać Hightail z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Hightail z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 
 
    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Hightail**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. W okienku wyników wybierz **Hightail**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Hightail opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w Hightail użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Hightail musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Hightail.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Hightail, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Hightail testowanie użytkownika](#creating-a-hightail-test-user)** — aby odpowiednikiem Simona Britta w Hightail połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Hightail.

Hightail aplikacji zakłada potwierdzeń SAML w określonym formacie. Skonfiguruj następujące oświadczeń dla tej aplikacji. Na karcie **"Atrribute"** aplikacji, możesz zarządzać wartości tych atrybutów. Następujące zrzut ekranu przedstawia przykładową to. 

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png) 

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Hightail, wykonaj następujące czynności:**


1. W portalu klasyczny Azure, na stronie integracji **Hightail** aplikacji w menu u góry kliknij **atrybutów**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png) 


1. W oknie dialogowym **atrybuty tokenu SAML** dla każdego wiersza pokazano w poniższej tabeli, wykonaj następujące czynności:

  	| Nazwa atrybutu | Wartość atrybutu |
  	| --- | --- |    
  	| Imię | User.givenName |
  	| Nazwisko  | User.surname |
  	| Adres e-mail | User.mail |
  	| Tożsamości użytkownika | User.mail |

    . Kliknij przycisk **Dodaj atrybut użytkownika** , aby otworzyć okno dialogowe **Dodawanie Attribure użytkownika** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png) 


    b. W polu tekstowym **Nazwa Attrubute** wpisz nazwę atrybutu wyświetlania dla tego wiersza.

    c. Z listy **Wartości atrybutu** selsect wartość atrybutu wyświetlania dla tego wiersza.

    d. Kliknij pozycję **pełne**.  
    



1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  



2. Na stronie **jak chcesz użytkownikom logowanie do Hightail** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png) 


3. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe Jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png) 



    . W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL w następujący sposób: **"https://www.hightail.com/samlLogin?phi_action=app/samlLogin & podakcję = handleSamlResponse"**

    b. Kliknij przycisk **Dalej**

4. Jeśli chcesz skonfigurować aplikację w **trybie inicjowanych przez SP** na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , następnie kliknij polecenie **"Pokaż zaawansowane ustawienia (opcjonalnie)"** i wpisz adres **Logowania na adres URL** i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png) 

    . W polu tekstowym logowania na adres URL wpisz adres URL używane przez użytkowników do logowania się w aplikacji Hightail przy użyciu następującego wzorca: **https://www.hightail.com/loginSSO**. To jest stronę logowania wspólne dla wszystkich klientów, którzy chcą korzystać logowania jednokrotnego.

    b. Kliknij przycisk **Dalej**

5. Na stronie **Konfigurowanie logowania jednokrotnego w Hightail** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png) 


    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu zakodowany base-64 na Twoim komputerze.

    b. Kliknij przycisk **Dalej**.

    > [AZURE.NOTE] Przed rozpoczęciem konfigurowania rejestracji jednokrotnej w aplikacji Hightail, zapoznaj się z domeną poczty e-mail z Hightail zespołu, aby wszyscy użytkownicy, którzy używają tej domeny mogą korzystać z funkcji rejestracji jednokrotnej listy biały.

6. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, musisz logowania jednokrotnego do Twojej dzierżawy Hightail jako administrator.

    . W menu u góry kliknij kartę **konto** i wybierz **Konfigurowanie SAML**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 


    b. Zaznacz pole wyboru **Włącz uwierzytelnianie SAML**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **SAML certyfikatu podpisywania tokenu** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 


    d. Kopiowanie adresu URL logowania zdalnego z usługi Azure Active Directory **urząd SAML (dostawcy tożsamości)** w Hightail.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Jeśli chcesz skonfigurować aplikację w **protokołu IDP inicjowanych przez tryb** wybierz pozycję **"Zaloguj zainicjowana dostawcy tożsamości (protokołu IdP)"**. Zaznaczenie **SP zainicjowane trybu** **"Zaloguj zainicjowane dostawcy usługi (SP)"**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Skopiuj adres URL dla klientów indywidualnych SAML wystąpienia i wklej je w polu tekstowym **Adres URL odpowiedzi** , jak pokazano w kroku 4. 

    g. Kliknij przycisk **Zapisz**.



6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**. 
 
    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
 
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 


4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png) 

    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.

    b. W polu tekstowym **Nazwa użytkownika** wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png) 


8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-hightail-test-user"></a>Tworzenie użytkownika test Hightail

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Hightail. 

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Hightail obsługuje przypisywanie użytkowników w czasie na podstawie niestandardowego oświadczeń. Jeśli skonfigurowano oświadczeń niestandardowych, jak pokazano w sekcji **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** powyżej, użytkownik zostanie automatycznie utworzona w aplikacji, które nie istnieje. 

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z obsługą Hightail za pośrednictwem [support@hightail.com](mailto:support@hightail.com).


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Hightail za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Hightail, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.
 
    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Hightail**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png) 


1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Hightail w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Hightail.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png
