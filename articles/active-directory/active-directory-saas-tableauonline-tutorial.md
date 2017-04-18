<properties
    pageTitle="Samouczek: Azure Active Directory integracji z Tableau Online | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować logowania jednokrotnego usługi Azure Active Directory i Tableau Online."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Samouczek: Azure Active Directory Integracja z Tableau w trybie Online

W tym samouczku dowiesz się, jak zintegrować Tableau Online z usługą Azure Active Directory (Azure AD).

Integrowanie Tableau Online z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Tableau w trybie Online
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Tableau Online (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Tableau Online, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- **Tableau Online** logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Tableau Online z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-tableau-online-from-the-gallery"></a>Dodawanie Tableau Online z galerii
Aby skonfigurować integrację Azure AD Tableau online, należy dodać do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa Tableau Online z galerii.

**Aby dodać Tableau Online z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Tableau Online**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. W okienku wyników wybierz pozycję **Tableau Online**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Tableau Online opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić, użytkownik musi w Tableau Online jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Tableau Online musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Tableau Online.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Tableau Online, musisz wykonać następujące bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Online Tableau testowanie użytkownika](#creating-a-Tableau-Online-test-user)** — ma odpowiednika Simona Britta w Tableau Online połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Tableau Online.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Tableau Online, wykonaj następujące czynności:**

1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego][6]
2. W portalu klasyczny na stronie **Tableau Online** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7] 

3. Na stronie **jak chcesz użytkownikom logowanie do Tableau Online** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    . W polu tekstowym logowania na adres URL wpisz adres URL przy użyciu następującego wzorca:`https://sso.online.tableau.com`

    c. Kliknij przycisk **Dalej**.

5. Na stronie **Konfigurowanie logowania jednokrotnego w Tableau Online** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]
8. W oknie innej przeglądarki logowania do aplikacji Tableau Online. Przejdź do pozycji **Ustawienia** , a następnie **uwierzytelniania**

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. W sekcji **Typy uwierzytelniania** . Zaznacz pole wyboru **rejestracji jednokrotnej z SAML** , aby włączyć SAML.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Przewiń w dół do sekcji **Importowanie pliku metadanych do Tableau Online** .  Kliknij przycisk Przeglądaj, a następnie zaimportuj plik metadanych pobranego z usługi Azure Active Directory. Następnie kliknij przycisk **Zastosuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. W sekcji **Uwzględnij potwierdzeń** Wstawianie nazwą potwierdzenia z dostawcy tożsamości adresu e-mail, imię i nazwisko. Aby uzyskać te informacje z Azure AD:

    . Wróć do Azure AD. W portalu klasyczny Azure, na stronie **Tableau Online** integracji aplikacji, w menu u góry kliknij **atrybutów**. Kopiowanie nazwę wartości: userprincipalname, imię i nazwisko.
     
    ![Azure AD rejestracji jednokrotnej](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Przełącz się do aplikacji Tableau Online, a następnie ustaw sekcji **Atrybuty w trybie Online Tableau** następująco:
    
    -  E-mail: **Poczta** lub **userprincipalname**
    -  Imię: **Imię**
    -  Nazwisko: **nazwisko**

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-tableau-online-test-user"></a>Tworzenie Tableau Online użytkowników testowych

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Tableau Online.

1. Na **Tableau Online**kliknij **Ustawienia** , a następnie sekcji **uwierzytelniania** . Przewiń w dół do sekcji **Wybieranie użytkowników** . Wybierz polecenie **Dodaj użytkowników** , a następnie **Wprowadź adresy E-mail**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Wybierz pozycję **Dodaj użytkowników dla pojedynczego uwierzytelniania logowania jednokrotnego (SSO)**. W polu tekstowym **Wprowadź adresy E-mail** Dodawaniebritta.simon@contoso.com

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Kliknij przycisk **Utwórz**.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Tableau online za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Tableau Online, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

3. Na liście aplikacji wybierz pozycję **Tableau Online**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

5. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

6. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Tableau Online w panelu programu Access, możesz należy uzyskać automatycznie podpisane w Tableau Online aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
