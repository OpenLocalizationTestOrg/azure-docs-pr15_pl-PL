<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z hostowanej grafitu | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i hostowana grafitu."
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


# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Samouczek: Azure Active Directory Integracja z hostowanej grafitu

Celem tego samouczka jest pokazano, jak zintegrować hostowanej grafitu z usługą Azure Active Directory (Azure AD).

Integrowanie hostowanej grafitu z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do hostowanej grafitu
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w hostowanej grafitu (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z hostowanej grafitu, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Obsługiwane grafitu logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie hostowanej grafitu z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-hosted-graphite-from-the-gallery"></a>Dodawanie hostowanej grafitu z galerii
Aby skonfigurować integrację Azure AD hostowanej grafitu, musisz dodać hostowanej grafitu z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać hostowanej grafitu z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Hostowanej grafitu**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_01.png)

7. W panelu Wyniki wybierz **Hostowanej grafitu**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z grafitu hostowanej opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownika w hostowanej grafitu użytkownikowi w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w hostowanej grafitu musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w hostowanej grafitu.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z hostowanej grafitu, musisz wykonać następujące bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie grafitu hostowanej testowanie użytkownika](#creating-a-hosted-graphite-test-user)** — aby odpowiednikiem Simona Britta w hostowanej grafitu połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji hostowanej grafitu.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z hostowanej grafitu, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Grafitu hostowanej** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do hostowanej grafitu** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_03.png)

3. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe Jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności i kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_04.png)

    . W polu tekstowym **identyfikator** wpisz adres URL przy użyciu następującego wzorca:`https://www.hostedgraphite.com/metadata/<user id>`

    b. W polu tekstowym **Adres URL odpowiedź** wpisz adres URL przy użyciu następującego wzorca:`https://www.hostedgraphite.com/complete/saml/<user id>`

    c. Kliknij przycisk **Dalej**

4. Jeśli chcesz skonfigurować aplikację w **SP zainicjowane tryb** na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , następnie kliknij polecenie **"Pokaż zaawansowane ustawienia (opcjonalnie)"** i wpisz adres **Logowania na adres URL** i kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca:`https://www.hostedgraphite.com/login/saml/<user id>/`

    b. Kliknij przycisk **Dalej**

    > [AZURE.NOTE] Pamiętaj, że są one wartości rzeczywistych. Musisz zaktualizować te wartości rzeczywiste logowania na adres URL, identyfikator i adres URL odpowiedź. Aby uzyskać te wartości, możesz przejść do programu Access -> Konfiguracja SAML na swojej stronie aplikacji lub skontaktuj się z hostowanej grafitu.

5. Na stronie **Konfigurowanie logowania jednokrotnego w hostowanej grafitu** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_05.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

6. Logowania jednokrotnego do Twojej dzierżawy hostowanej grafitu jako administrator.

7. Przejdź do **strony Konfiguracja SAML** na pasku bocznym (**Access -> Konfiguracja SAML**).

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

8. Upewnij się, że te adresy URL zgodne konfiguracji w kroku 3.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

9. Skopiuj **adres URL wystawcy** oraz **adres URL logowania jednokrotnego SAML** z Azure AD **jednostki lub identyfikator wystawcy** i **adres URL logowania jednokrotnego logowania** w hostowanej grafitu.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_003.png)

9. Wybierz pozycję "**tylko do odczytu**" jako **domyślnej roli użytkownika**.

    ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

10. Skopiuj zawartość pliku pobranego certyfikatu, a następnie wklej go w polu tekstowym **Certyfikat X.509** .

     ![Konfigurowanie strony pojedynczego logowania jednokrotnego w aplikacji](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

11. Kliknij przycisk **Zapisz** .

12. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

13. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-hosted-graphite-test-user"></a>Tworzenie użytkownika test hostowanej grafitu

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w hostowanej grafitu. Obsługiwane grafitu obsługuje w czasie inicjowania obsługi administracyjnej, która jest domyślnie włączona.

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Nowy użytkownik zostanie utworzony podczas próby dostępu hostowanej grafitu, jeśli go jeszcze nie istnieje.

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, musisz skontaktuj się z zespołem pomocy technicznej hostowanej grafitu za pośrednictwem <mailto:help@hostedgraphite.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej hostowanej grafitu za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta hostowanej grafitu, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **Hostowanej grafitu**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_50.png)

1. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka grafitu hostowana w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji hostowanej grafitu.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_205.png
