<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z ImageRelay | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i ImageRelay."
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


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Samouczek: Azure Active Directory Integracja z ImageRelay

Celem tego samouczka jest pokazano, jak zintegrować ImageRelay z usługi Azure Active Directory (Azure AD).  
Integrowanie ImageRelay z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do ImageRelay
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w ImageRelay (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z ImageRelay, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- ImageRelay logowania jednokrotnego włączone subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie ImageRelay z galerii

2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-imagerelay-from-the-gallery"></a>Dodawanie ImageRelay z galerii
Aby skonfigurować integrację Azure AD ImageRelay, musisz dodać ImageRelay z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać ImageRelay z galerii, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **ImageRelay**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. W okienku wyników wybierz **ImageRelay**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ImageRelay opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi reprezentujący użytkownikowi w ImageRelay konta użytkownika.  Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w ImageRelay musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w ImageRelay.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ImageRelay, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie ImageRelay testowanie użytkownika](#creating-a-userecho-test-user)** — aby odpowiednikiem Simona Britta w ImageRelay połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji ImageRelay.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z ImageRelay, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **ImageRelay** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

     ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do ImageRelay** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

     ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji ImageRelay (na przykład: *https://fabrikam.ImageRelay.com/*).

    b. Kliknij przycisk **Dalej**.

4. Na stronie **Konfigurowanie logowania jednokrotnego w ImageRelay** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

5. W oknie przeglądarki innej logowanie się do witryny firmy ImageRelay jako administrator.

1. Na pasku narzędzi u góry kliknij przycisk Obciążenie pracą **Użytkownicy i uprawnienia** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Kliknij przycisk **Utwórz nowe uprawnienie**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. W obciążenie pracą **Pojedynczy znak na ustawienia** zaznacz pole wyboru **Ta grupa można tylko logowania za pomocą rejestracji jednokrotnej** i kliknij przycisk **Zapisz**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Przejdź do **ustawień konta**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Przejdź do obciążenie pracą **Pojedynczy znak na ustawienia** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. W oknie dialogowym **Ustawienia SAML** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    . W portalu klasyczny Azure Skopiuj **Pojedynczy znak na adres URL usługi**, a następnie wklej go w polu tekstowym **Adres URL logowania** .


    b. W portalu klasyczny Azure Skopiuj **Adres URL usługi Sign-Out pojedynczej**, a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .

    c. **Format identyfikatora nazw**, zaznacz **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: emailAddress**.

    
    d. **Wiązanie opcji żądania od dostawcy usługi (przekazywania obrazu)**zaznacz **Wiązanie wpisu**.
   

    e. W obszarze **x.509 certyfikat**kliknij przycisk **Aktualizuj certyfikat**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Otwórz pobrany certyfikatu w Notatniku, skopiuj zawartość, a następnie wklej go w polu tekstowym certyfikat x.509.
  
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. W sekcji **Przypisywanie użytkowników Just-In-Time** wybierz pozycję **Włącz Just-In-Time użytkownika inicjowania obsługi administracyjnej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Wybierz grupę uprawnień, (na przykład **Podstawowe logowania jednokrotnego**) co jest dozwolone do zalogowania się tylko za pośrednictwem logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    mogę. Kliknij przycisk **Zapisz**.

6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.

    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-imagerelay-test-user"></a>Tworzenie użytkownika test ImageRelay

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w ImageRelay.

**Aby utworzyć użytkownika o nazwie Simona Britta w ImageRelay, wykonaj następujące czynności:**

1. Logowania do witryny firmy ImageRelay jako administrator.

1. Przejdź do pozycji **Użytkownicy i uprawnienia** i wybierz polecenie **Utwórz użytkownika logowania jednokrotnego**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Wprowadź **Adres E-mail**, **Imię**, **Nazwisko** i **Firma** użytkownika, który chcesz obsługi administracyjnej i wybierz (na przykład logowania jednokrotnego podstawowych) grupę uprawnień, czyli grupę, do której można zalogować się tylko za pośrednictwem logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Kliknij przycisk **Utwórz**.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej ImageRelay za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta ImageRelay, wykonaj następujące czynności:**

1. W portalu klasyczny Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **ImageRelay**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka ImageRelay w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji ImageRelay.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
