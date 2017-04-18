<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z zastępca | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i zastępcę."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Samouczek: Azure Active Directory Integracja z zastępca

Celem tego samouczka jest pokazano, jak zintegrować zastępca z usługą Azure Active Directory (Azure AD).

Integrowanie zastępca z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do zastępca
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w zastępca (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z zastępcę, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Zastępca logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie zastępca z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-deputy-from-the-gallery"></a>Dodawanie zastępca z galerii
Aby skonfigurować integrację Azure AD zastępcę, musisz dodać zastępca z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać zastępca z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **zastępca**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. W panelu Wyniki wybierz **zastępcę**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z zastępca opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w zastępca użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w zastępca musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w zastępca.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z zastępcę, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie zastępcę testowanie użytkownika](#creating-a-deputy-test-user)** — aby odpowiednikiem Simona Britta w zastępca połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji zastępca.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z zastępcę, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **zastępca** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do zastępca** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe Jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    . W polu tekstowym **identyfikator** wpisz adres URL przy użyciu następującego wzorca: `https://<your-subdomain>.<region>.deputy.com`.

    b. W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL przy użyciu następującego wzorca: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c. Kliknij przycisk **Dalej**.

4. Jeśli chcesz skonfigurować aplikację w **SP zainicjowane tryb** na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , następnie kliknij polecenie **"Pokaż zaawansowane ustawienia (opcjonalnie)"** i wpisz adres **Logowania na adres URL** i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca: `https://<your-subdomain>.<region>.deputy.com`.

    b. Kliknij przycisk **Dalej**.

    > [AZURE.NOTE] Sufiks region zastępca jest opitional lub jego powinni za pomocą jednej z następujących: au | Brak | Unii Europejskiej | jako | la | af | | au Enterprise | Enterprise — Brak | Unii Europejskiej Enterprise | Enterprise — jako | Enterprise la | af Enterprise | i Enterprise

5. Na stronie **Konfigurowanie logowania jednokrotnego w zastępcę** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    
6. Przejdź do następującego adresu URL: https://(your-subdomain).deputy.com/exec/config-system_config. Przejdź do strony **Ustawienia zabezpieczeń** , a następnie kliknij przycisk **Edytuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. W portalu klasyczny Azure, na konfigurowanie logowania jednokrotnego na stronie zastępca skopiuj adres URL logowania jednokrotnego SAML. 

8. Na tej stronie **Ustawienia zabezpieczeń** wykonaj kroki.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    . Włącz **społecznościowych logowania**.

    b. Otwórz certyfikatu Base64 zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **OpenSSL certyfikatu** .

    c. W polu tekstowym adres URL logowania jednokrotnego SAM wpisz`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. W polu tekstowym adres URL logowania jednokrotnego SAM Zamień `<your subdomain>` z usługi poddomeny.

    e. W polu tekstowym adres URL logowania jednokrotnego SAM Zamień `<saml sso url>` z adresem URL logowania jednokrotnego SAML, które zostały skopiowane z portalu klasyczny Azure.

    f. Kliknij przycisk **Zapisz ustawienia**.

9. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-deputy-test-user"></a>Tworzenie użytkownika test zastępca

Aby umożliwić użytkownikom Azure AD zalogować się do zastępcę, musi być przygotowana do zastępca. W przypadku zastępcę inicjowania obsługi administracyjnej to zadanie ręczne.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkownika, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy zastępca jako administrator.

2.  W okienku górny obszar nawigacji kliknij pozycję **osoby**.

    ![Osoby] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Osoby")

3.  Kliknij przycisk **Dodaj osoby** , a następnie kliknij przycisk **Dodaj jedna osoba**.

    ![Dodawanie osób] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Dodawanie osób")

4.  Wykonaj następujące czynności, a następnie kliknij przycisk **Zapisz i Zaproś**.

    ![Nowy użytkownik] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Nowy użytkownik")

    . W polu tekstowym **Nazwa** wpisz **Britta** i **Simona**.  

    b. W polu tekstowym **Adres E-mail** wpisz adres e-mail konta Azure AD, które ma zostać świadczenia.

    c. W polu tekstowym **pracy na** wpisz nazwę bussniess.

    d. Kliknij przycisk **Zapisz i Zaproś** .

    >[AZURE.NOTE]Właściciel konta AAD otrzymasz wiadomość e-mail i skorzystać z łącza, aby potwierdzić swoje konto, zanim stanie się aktywny. Można użyć dowolnego innego zastępca użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez zastępcę zabezpieczanie AAD kont użytkowników.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej zastępca za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta zastępcę, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **zastępca**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]

### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka zastępca w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji zastępca.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
