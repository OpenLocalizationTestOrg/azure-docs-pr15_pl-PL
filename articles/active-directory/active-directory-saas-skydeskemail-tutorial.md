<properties
    pageTitle="Samouczek: Azure Active Directory integracji z pocztą E-mail Skydesk | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Skydesk poczty E-mail."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Samouczek: Azure Active Directory Integracja z Skydesk wiadomości E-mail

Celem tego samouczka jest pokazano, jak zintegrować Skydesk wiadomości E-mail z usługi Azure Active Directory (Azure AD).

Integrowanie poczty E-mail Skydesk z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do poczty E-mail Skydesk
- Możesz włączyć użytkowników, aby automatycznie uzyskać podpisany w wiadomości E-mail Skydesk (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — klasyczny portalu usługi Azure Active Directory

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z pocztą E-mail Skydesk, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Wiadomości E-mail Skydesk logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym. 

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Skydesk wiadomości E-mail z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-skydesk-email-from-the-gallery"></a>Dodawanie Skydesk wiadomości E-mail z galerii
Aby skonfigurować integrację Azure AD Skydesk wiadomości E-mail, musisz dodać Skydesk wiadomości E-mail z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Skydesk wiadomości E-mail z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Skydesk wiadomości E-mail**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. W okienku wyników wybierz **Skydesk wiadomości E-mail**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest, aby pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Skydesk pocztą E-mail opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w wiadomości E-mail Skydesk użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w wiadomości E-mail Skydesk musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w wiadomości E-mail Skydesk.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z pocztą E-mail Skydesk, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie wiadomość E-mail z Skydesk testowanie użytkownika](#creating-a-Skydesk-Email-test-user)** — aby odpowiednikiem Simona Britta w Skydesk wiadomości E-mail, która jest połączony z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji pocztowej Skydesk.



**Aby skonfigurować Azure AD rejestracji jednokrotnej z Skydesk pocztą E-mail, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji aplikacji **Skydesk wiadomości E-mail** kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do Skydesk E-mail** wybierz pozycję **Azure AD logowania jednokrotnego**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    . W polu tekstowym logowania na adres URL wpisz adres URL używane przez użytkowników do logowania się w aplikacji pocztowej Skydesk przy użyciu następującego wzorca: **"https://mail.skydesk.jp/portal/\<nazwy firmy\>"**.

    b. Kliknij przycisk **Dalej**.


4. Na stronie **Konfigurowanie logowania jednokrotnego Skydesk poczty e-mail** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby włączyć logowania jednokrotnego w **Skydesk wiadomości E-mail**, wykonaj następujące czynności:
 
    . Logowania do konta E-mail Skydesk jako administrator.

    b. W menu u góry kliknij przycisk Ustawienia i wybierz Org. 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. Kliknij pozycję domeny przy użyciu panelu po lewej stronie.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Kliknij przycisk Dodaj domenę.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Wpisz nazwę domeny, a następnie zweryfikuj domenę.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Wybierz polecenie **Uwierzytelniania SAML** z panelu po lewej stronie

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Na stronie okno dialogowe **Uwierzytelniania SAML** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] Aby SAML podstawie uwierzytelnianie, należy albo użyć **weryfikacji domeny** lub **portal adres URL** konfiguracji. Można ustawić portalu adres URL z unikatową nazwę.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    . W klasycznym portal Azure AD skopiuj wartość **Adres URL logowania jednokrotnego SAML** , a następnie wklej go w polu tekstowym **Adres URL logowania** .

    b. W klasycznym portal Azure AD skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym adres URL **Wyloguj** .

    c. **Zmień hasło adres URL** jest opcjonalny tak pozostaw to pole puste.

    d. Kliknij pozycję na **Uzyskiwanie klucza z pliku** , aby zaznaczyć pobranego certyfikatu Skydesk wiadomości E-mail, a następnie kliknij **Otwórz** przekazywanie certyfikatu.

    e. **Algorytm**zaznacz **RSA**.

    f. Kliknij przycisk **Ok** Aby zapisać zmiany.


7. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  
    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-skydesk-email-test-user"></a>Tworzenie użytkownika test Skydesk wiadomości E-mail

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Skydesk wiadomości E-mail.

. Polecenie **Dostępu użytkowników** za pomocą panelu po lewej stronie w Skydesk wiadomości E-mail, a następnie wprowadź nazwę użytkownika. 

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Jeśli chcesz utworzyć zbiorcza użytkowników, musisz skontaktuj się z zespołem pomocy technicznej Skydesk wiadomości E-mail.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Skydesk wiadomości E-mail za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Skydesk wiadomości E-mail, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.
 
    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Skydesk wiadomości E-mail**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Skydesk poczty E-mail w panelu programu Access możesz należy uzyskać automatycznie podpisane w aplikacji pocztowej Skydesk.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
