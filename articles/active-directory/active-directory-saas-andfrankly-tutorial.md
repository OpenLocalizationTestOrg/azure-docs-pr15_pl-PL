<properties
    pageTitle="Samouczek: Integracja Azure Active Directory z & frankly | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i & frankly."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Samouczek: Integracja Azure Active Directory z & frankly

Celem tego samouczka jest pokazano, jak zintegrować i frankly z usługą Azure Active Directory (Azure AD).

Integrowanie i frankly z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do & frankly
- Możesz włączyć użytkowników, aby automatycznie pobierać podpisane na do & frankly (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD Integracja z & frankly, możesz potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- A & frankly logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie i frankly z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-frankly-from-the-gallery"></a>Dodawanie i frankly z galerii
Aby skonfigurować integrację z & frankly w usłudze Azure Active Directory, musisz dodać i frankly z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać & frankly z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **& frankly**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. W panelu Wyniki wybierz pozycję **i frankly**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować oraz pojedyncze test Azure AD logowania przy użyciu & frankly oparte na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownika & frankly do użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w & frankly musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w & frankly.

Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z & frankly, możesz, trzeba wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenia i frankly użytkownika testowego](#creating-a-&frankly-test-user)** — ma odpowiednika Simona Britta w i frankly jest połączone z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji Włączanie Azure AD logowania jednokrotnego w portalu klasyczny i konfigurowanie logowania jednokrotnego w sieci i frankly aplikacji.

**Aby skonfigurować Azure AD pojedynczy znak przy użyciu & frankly, wykonaj następujące czynności:**

1. W portalu klasyczny na **& frankly** integracji stronie aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do & frankly** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe Jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    . W polu tekstowym **identyfikator** wpisz adres URL przy użyciu następującego wzorca:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. W polu tekstowym **Adres URL odpowiedź** wpisz adres URL przy użyciu następującego wzorca:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c. Kliknij przycisk **Dalej**

4. Jeśli chcesz skonfigurować aplikację w **SP zainicjowane tryb** na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , następnie kliknij polecenie **"Pokaż zaawansowane ustawienia (opcjonalnie)"** i wpisz adres **Logowania na adres URL** i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Pamiętaj, że są one wartości rzeczywistych. Musisz zaktualizować te wartości rzeczywiste logowania na adres URL, identyfikator i adres URL odpowiedź. Kontakt [help@andfrankly.com](emailTo:help@andfrankly.com) uzyskać te wartości.

5. Na **Konfigurowanie logowania jednokrotnego w & frankly** strony, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

6. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z & frankly zespołu za pośrednictwem pomocy technicznej [help@andfrankly.com](emailTo:help@andfrankly.com). Dołączanie pliku metadanych pobrany, a następnie udostępnij go i frankly członkom zespołu konfigurowanie logowania jednokrotnego na swojej stronie.

7. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-frankly-test-user"></a>Tworzenie i frankly użytkowników testowych

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w & frankly. We współpracy z & frankly zespołu za pośrednictwem pomocy technicznej [help@andfrankly.com](emailTo:help@andfrankly.com) Dodawanie użytkowników w & frankly platformy.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej do i frankly za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta do & frankly, należy wykonać następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji wybierz pozycję **i frankly**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu przycisku & frankly kafelka w panelu programu Access, należy uzyskać automatycznie podpisane w swojej & frankly aplikacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
