<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Keylight | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Keylight."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Samouczek: Azure Active Directory Integracja z Keylight

W tym samouczku dowiesz się, jak zintegrować Keylight z usługą Azure Active Directory (Azure AD).

Integrowanie Keylight z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Keylight
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Keylight (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Keylight, potrzebne następujące elementy:

- Subskrypcję usługi Azure
- Keylight logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Keylight z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-keylight-from-the-gallery"></a>Dodawanie Keylight z galerii
Aby skonfigurować integrację Azure AD Keylight, musisz dodać Keylight z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Keylight z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Keylight**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. W okienku wyników wybierz **Keylight**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Keylight opartą na użytkowniku test o nazwie "Britta Simona".

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Keylight, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Keylight testowanie użytkownika](#creating-a-keylight-test-user)** — aby odpowiednikiem Simona Britta w Keylight połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i skonfigurować logowania jednokrotnego w aplikacji Keylight.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z Keylight, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **Keylight** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 


2. Na stronie **jak chcesz użytkownikom logowanie do Keylight** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    . W polu tekstowym logowania na adres URL wpisz adres URL używane przez użytkowników do logowania się w aplikacji Keylight przy użyciu następującego wzorca: **"https://\<nazwy firmy\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Na stronie **Konfigurowanie logowania jednokrotnego w Keylight** wykonaj następujące czynności:
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby włączyć logowania jednokrotnego w Keylight, wykonaj następujące czynności:
 
    . Logowania do konta Keylight jako administrator.

    b. W menu u góry kliknij **osobę**, a następnie wybierz pozycję **Konfiguracja Keylight**.
       
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. W widoku drzewa po lewej stronie kliknij przycisk **SAML**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. W oknie dialogowym **Ustawienia SAML** kliknij przycisk **Edytuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Na stronie okno dialogowe **Edytuj ustawienia SAML** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/405.png) 

    . Ustawienia **uwierzytelniania SAML** **aktywna**.


    b. W klasycznym portal Azure AD skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .

    c. W klasycznym portal Azure AD skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości** .

    d. Kliknij przycisk **Wybierz plik** , aby wybrać certyfikat Keylight pobrany, a następnie kliknij **Otwórz** przekazywanie certyfikatu.


    e. Ustawianie **lokalizacji SAML identyfikator użytkownika** do **elementu NameIdentifier instrukcji tematu**.
   
    f. Podaj **Keylight usługodawcy przy użyciu następującego wzorca: **https://&lt;nazwy firmy&gt;. keylightgrc.com**.

    g. Ustawianie **automatycznego tworzenia użytkowników** na **aktywny**.

    h. Ustaw **Typ konta automatycznie świadczenia** **Pełny użytkownika**.

    mogę. Jako **roli zabezpieczeń należy automatycznie**wybierz pozycję **Standardowy użytkownika SAML**.
   
    j. **Obsługa administracyjna automatycznej konfiguracji zabezpieczeń**zaznacz **Standardowej konfiguracji użytkownika**.
   
    k. W polu tekstowym atrybut poczty E-mail wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. W polu tekstowym **atrybut imię** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. W polu tekstowym **ostatniego atrybutu nazwy** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Kliknij przycisk **Zapisz**.
   
  
   
  
6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]



**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-keylight-test-user"></a>Tworzenie użytkownika test Keylight

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Keylight. Keylight obsługuje w czasie inicjowania obsługi administracyjnej, która jest domyślnie włączona.

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Uzyskiwanie dostępu do Keylight, jeśli jeszcze nie istnieje użytkownika powoduje utworzenie nowego użytkownika. 

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z obsługą Keylight.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Keylight za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Keylight, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Keylight**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka Keylight w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Keylight.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
