<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z rozpoznawanie | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i rozpoznawanie."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Samouczek: Azure Active Directory Integracja z rozpoznawanie

Celem tego samouczka jest pokazano, jak zintegrować rozpoznawanie z usługą Azure Active Directory (Azure AD).

Integrowanie rozpoznawanie z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do uznania
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w rozpoznawanie (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z rozpoznawanie, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Rozpoznawanie logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie rozpoznawanie z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-recognize-from-the-gallery"></a>Dodawanie rozpoznawanie z galerii
Aby skonfigurować integrację Azure AD rozpoznawanie, musisz dodać rozpoznawanie z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać rozpoznawanie z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Rozpoznawanie**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. W panelu Wyniki wybierz **Rozpoznawanie**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z rozpoznawanie opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w rozpoznawanie użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w rozpoznawanie musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w rozpoznawanie.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z rozpoznawanie, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie rozpoznawanie testowanie użytkownika](#creating-a-recognize-test-user)** — aby odpowiednikiem Simona Britta w rozpoznawanie połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji rozpoznawanie.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z rozpoznawanie, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Rozpoznawanie** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkowników do zalogowania się celu uznania** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. W polu tekstowym **identyfikator** wpisz adres URL przy użyciu następującego wzorca: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Jeśli nie wiesz, że informacje te adresy URL, wpisz adresy URL próbki z wzorcem przykład. Aby pobrać te wartości, można odwoływać się kroku 9, aby uzyskać więcej informacji lub skontaktuj się z zespołem pomocy technicznej uznania za pośrednictwem <mailto:support@recognizeapp.com>.

4. Na stronie **Konfigurowanie logowania jednokrotnego w rozpoznawanie** kliknij pozycję **Pobierz certyfikat** , a następnie zapisz plik na komputerze:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. W oknie przeglądarki sieci web innej logowania jednokrotnego do Twojej dzierżawy rozpoznawanie jako administrator.

6. W prawym górnym rogu kliknij **Menu**. Przejdź do pozycji **administrator firmy**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. W okienku nawigacji po lewej stronie kliknij pozycję **Ustawienia**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. W sekcji **Ustawienia logowania jednokrotnego** , należy wykonać następujące czynności.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    . **Włączanie rejestracji Jednokrotnej**zaznacz **Wł**.

    b. W polu **Identyfikator jednostki protokołu IDP** pole tekstowe, należy umieścić wartość **Adres URL wystawcy** z Kreatora konfiguracji aplikacji Azure AD.

    c. W **logowania jednokrotnego docelowy adres url** pole tekstowe, należy umieścić wartość **Pojedynczy znak na adres URL usługi** z Kreatora konfiguracji aplikacji Azure AD.

    d. W **Slo docelowy adres url** pole tekstowe, należy umieścić wartość **Adres URL usługi Sign-Out jednego** z Kreatora konfiguracji aplikacji Azure AD.

    e. Otwórz plik pobranego certyfikatu w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **certyfikatu** . 

    f. Kliknij przycisk **Zapisz ustawienia** . 

9. Obok sekcji **Ustawienia logowania jednokrotnego** skopiuj adres URL pod **adresem url metadanych dostawcy usługi**.
    
    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Otwórz **metadanych adres URL łącza** w przeglądarce pusty na pobieranie dokumentu metadanych. Następnie użyj wartości EntityDescriptor z podanych rozpoznawanie możesz **identyfikatora** w oknie dialogowym **Konfigurowanie ustawień aplikacji** .

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

12. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu tekstowym **Nazwa** wpisz **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-recognize-test-user"></a>Tworzenie użytkownika test rozpoznawanie

Aby umożliwić użytkownikom Azure AD zalogować się do uznania, musi być przygotowana do uznania. W przypadku uznania inicjowania obsługi administracyjnej to zadanie ręczne.

Ta aplikacja nie obsługuje SCIM inicjowania obsługi administracyjnej, ale nie synchronizacji alternatywny użytkownika, którego przepisy użytkowników. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkownika, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy rozpoznawanie jako administrator.

2.  W prawym górnym rogu kliknij **Menu**. Przejdź do pozycji **administrator firmy**.

3.  W okienku nawigacji po lewej stronie kliknij pozycję **Ustawienia**.

4.  W sekcji **Synchronizacja użytkownika** , należy wykonać następujące czynności.
    
    ![Nowy użytkownik] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Nowy użytkownik")

    . Jako **Włączona synchronizacja**wybierz pozycję **Dalej**.

    b. **Wybierz dostawcę synchronizacji**, wybierz pozycję **firmy Microsoft / Office 365**.

    c. Kliknij polecenie **Uruchom synchronizacja użytkownika**

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej uznania za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta rozpoznawanie, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **Rozpoznawanie**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]

### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka rozpoznawanie w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji rozpoznawanie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
