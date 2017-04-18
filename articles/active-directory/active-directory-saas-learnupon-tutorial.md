<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Novatus | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i LearnUpon."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Samouczek: Azure Active Directory Integracja z LearnUpon

Celem tego samouczka jest pokazano, jak zintegrować LearnUpon z usługi Azure Active Directory (Azure AD).  
Integrowanie LearnUpon z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do LearnUpon
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w LearnUpon (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - usługi Azure Active Directory klasyczny 


Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z LearnUpon, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- LearnUpon logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie LearnUpon z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-learnupon-from-the-gallery"></a>Dodawanie LearnUpon z galerii
Aby skonfigurować integrację Azure AD LearnUpon, musisz dodać LearnUpon z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać LearnUpon z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **LearnUpon**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. W okienku wyników wybierz **LearnUpon**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z LearnUpon opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w LearnUpon użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w LearnUpon musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w LearnUpon.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z LearnUpon, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie LearnUpon testowanie użytkownika](#creating-a-learnupon-test-user)** — aby odpowiednikiem Simona Britta w LearnUpon połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji LearnUpon.



**Aby skonfigurować Azure AD rejestracji jednokrotnej z LearnUpon, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **LearnUpon** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do LearnUpon** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    . W polu tekstowym **Adres URL odpowiedź** wpisz adres URL usługi dla klientów indywidualnych potwierdzenia przy użyciu następującego wzorca:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Kliknij przycisk **Dalej**. 


4. Na stronie **Konfigurowanie logowania jednokrotnego w LearnUpon** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze. Są wymagane tego certyfikatu i adresy URL metadanych (identyfikator jednostki, logowania jednokrotnego adres URL logowania i zaloguj się adresu URL) Konfigurowanie logowania jednokrotnego na stronie LearnUpon.

    b. Kliknij przycisk **Dalej**.




1. Otwórz innego wystąpienia przeglądarki i zaloguj się do LearnUpon przy użyciu konta administratora. 

1. Kliknij kartę **Ustawienia** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. Kliknij **Rejestracji jednokrotnej - SAML**, a następnie kliknij pozycję **Ustawienia ogólne** , aby skonfigurować ustawienia SAML.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. W sekcji **Ustawienia ogólne** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    . Wybierz pozycję **włączone**.

    b. Jako **wersji**wybierz opcję **2.0**.

    c. Jako **Pomiń warunki**wybierz opcję **nie**.

    d. W polu tekstowym **opublikować tokenu SAML nazwy parametrów** wpisz nazwę parametru wpis w wezwaniu do adresu URL dla klientów indywidualnych SAML powyższym, który zawiera potwierdzenia SAML ma zostać zweryfikowane i uwierzytelniony — na przykład **SAMLResponse**.

    e. W polu tekstowym **Nazwa identyfikator Format** wpisz odpowiednią wartość, która wskazuje, gdzie w swojej potwierdzenia SAML identyfikatorów użytkowników (adres E-mail) znajduje się — na przykład **Nazwa urn: oasis: nazwy: tc: SAML:1.1:nameid-format: emailAddress**.

    f. W polu tekstowym **Lokalizacji zidentyfikowanie dostawcy** wpisz wartość, która wskazuje miejsce, w którym użytkownicy są wysyłane po kliknięciu na ikonie przekazane z ekranu Azure klasyczny logowania portalu.

    g.in portalu klasyczny Azure, skopiuj **Adres URL usługi Sign-Out pojedynczej**, a następnie wklej go w textbos **Wyloguj się adresu URL** .

    h. Kliknij pozycję **Zarządzaj palec umożliwia drukowanie**, a następnie przekazanie odcisk palca pobranego certyfikatu. 


1. Kliknij pozycję **Ustawienia użytkownika**, a następnie wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    . W polu tekstowym **Imię identyfikator Format** wpisz wartość, która wskazuje miejsce, w którym w swojej potwierdzenia SAML imię użytkowników znajduje się — na przykład: **Imię http://schemas.xmlsoap.org/ws/2005/05/identity/claims/**.

    b. W polu tekstowym **Formacie ostatnio identyfikator nazwa** wpisz wartość, która wskazuje miejsce, w którym w swojej potwierdzenia SAML nazwisko użytkowników znajduje się — na przykład: **nazwisko http://schemas.xmlsoap.org/ws/2005/05/identity/claims/**.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-learnupon-test-user"></a>Tworzenie użytkownika test LearnUpon

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w LearnUpon. LearnUpon obsługuje w czasie inicjowania obsługi administracyjnej, która jest domyślnie włączona.

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Nowy użytkownik zostanie utworzony podczas próby dostępu LearnUpon, jeśli go jeszcze nie istnieje. [Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z obsługą LearnUpon.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej LearnUpon za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta LearnUpon, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **LearnUpon**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka LearnUpon w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji LearnUpon.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
