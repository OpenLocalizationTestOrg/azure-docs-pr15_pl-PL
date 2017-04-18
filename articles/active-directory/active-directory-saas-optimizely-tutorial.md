<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Optimizely | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Optimizely."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Samouczek: Azure Active Directory Integracja z Optimizely

W tym samouczku dowiesz się, jak zintegrować Optimizely z usługą Azure Active Directory (Azure AD).

Integrowanie Optimizely z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Optimizely
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Optimizely (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Optimizely, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- **Optimizely** logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Optimizely z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-optimizely-from-the-gallery"></a>Dodawanie Optimizely z galerii
Aby skonfigurować integrację Azure AD Optimizely, musisz dodać Optimizely z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Optimizely z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Optimizely**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. W okienku wyników wybierz **Optimizely**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Optimizely opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w Optimizely jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Optimizely musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Optimizely.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Optimizely, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Optimizely testowanie użytkownika](#creating-an-optimizely-test-user)** — aby odpowiednikiem Simona Britta w Optimizely połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Optimizely.

Aplikacja Optimizely oczekuje potwierdzeń SAML zawiera atrybut o nazwie "e-mail". Wartość "e-mail" powinna być wiadomość e-mail Optimizely rozpoznany uzyskiwanie uwierzytelnione przez Azure AD. Skonfiguruj roszczeń "e-mail" dla tej aplikacji. Na karcie **"Atrributes"** aplikacji, możesz zarządzać wartości tych atrybutów. Następujące zrzut ekranu przedstawia przykładową to. 


![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Aby skonfigurować Azure AD rejestracji jednokrotnej z Optimizely, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **Optimizely** aplikacji w menu u góry kliknij **atrybutów**.
     
    ![Konfigurowanie logowania jednokrotnego][5]

2. W oknie dialogowym atrybuty tokenu SAML Dodaj atrybut "e-mail".

    . Kliknij przycisk **Dodaj atrybut użytkownika** , aby otworzyć okno dialogowe **Dodawanie atrybutu użytkownika** . 
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu "e-mail".

    c. Wybierz z listy **Wartość atrybutu** wartość atrybutu "userprincipalname" lub dowolną wartość, która zawiera wiadomości e-mail rozpoznał Azure AD i Optimizely.

    d. Kliknij pozycję **pełne**.
3. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego][6]
4. W portalu klasyczny na stronie integracji **Optimizely** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7] 

5. Na stronie **jak chcesz użytkownikom logowanie do Optimizely** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    . W polu tekstowym **Logowania na adres URL** wpisz:`https://app.optimizely.net/contoso`

    b. W polu tekstowym **identyfikator** wpisz:`urn:auth0:optimizely:contoso`

    c. Kliknij przycisk **Dalej**. 


    > [AZURE.NOTE] Wartości **Na adres URL logowania** i **identyfikator** są tylko symbolami zastępczymi rzeczywistych wartości. Wartości rzeczywiste z Optimizely instrukcje dotyczące aquiring można znaleźć w dalszej części tego samouczka.

7. Na stronie **Konfigurowanie logowania jednokrotnego w Optimizely** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Skopiuj **adres URL usługi rejestracji jednokrotnej**.

8. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z administratorem konta Optimizely i podać następujące informacje:

    - Pobrany certyfikatu 
    - Adres URL usługi rejestracji jednokrotnej
 
    W odpowiedzi na wiadomości e-mail Optimizely umożliwia logowania na adres URL (SSO zainicjowane SP) i wartości Identyfikator usługi dostawcy jednostki.

9. Wróć do strony, okno dialogowe **Konfigurowanie ustawień aplikacji** , a następnie wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    . W polu tekstowym **Logowania na adres URL** wpisz adres **Zainicjowane SP adres URL logowania jednokrotnego** dostarczony przez Optimizely.

    b. W polu tekstowym **identyfikator** wpisz **Identyfikator obiektu dostawcy usługi** dostarczony przez Optimizely.

    c. Kliknij przycisk **Dalej**.

10. Na stronie **Konfigurowanie logowania jednokrotnego w Optimizely** wykonaj następujące czynności:
    
    ![Azure AD rejestracji jednokrotnej][10]

    . Wybierz pozycję potwierdzenia konfiguracji rejestracji jednokrotnej.

    b. Kliknij przycisk **Dalej**.

11. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]

12. W oknie przeglądarki różnych logowania jednokrotnego w aplikacji Optimizely.
13. Kliknij pozycję konta nazwę w prawym górnym rogu, a następnie **Ustawienia kont**.

    ![Azure AD rejestracji jednokrotnej](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Na karcie Konto zaznacz pole **Włącz logowania jednokrotnego** w obszarze rejestracji jednokrotnej w sekcji **Omówienie** .

    ![Azure AD rejestracji jednokrotnej](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.
Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-optimizely-test-user"></a>Tworzenie użytkownika test Optimizely

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Optimizely.

1. Na stronie głównej wybierz kartę **współpracownicy**
2. Kliknij przycisk **Nowy współpracownik** Aby dodać nowy współpracownik do projektu.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Wprowadź adres e-mail i przypisanie ich roli. Kliknij przycisk **Zaproś**.


    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Otrzymają zaproszenie e-mail. Za pomocą adresu e-mail. należy je zalogować się do Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Optimizely za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Optimizely, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Optimizely**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Optimizely w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Optimizely.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
