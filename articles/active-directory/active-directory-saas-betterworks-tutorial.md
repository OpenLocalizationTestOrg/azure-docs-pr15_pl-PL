<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z BetterWorks | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i BetterWorks."
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


# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Samouczek: Azure Active Directory Integracja z BetterWorks

Celem tego samouczka jest pokazano, jak zintegrować BetterWorks z usługi Azure Active Directory (Azure AD).

Integrowanie BetterWorks z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do BetterWorks
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w BetterWorks (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z BetterWorks, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- BetterWorks logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie BetterWorks z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-betterworks-from-the-gallery"></a>Dodawanie BetterWorks z galerii
Aby skonfigurować integrację Azure AD BetterWorks, musisz dodać BetterWorks z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać BetterWorks z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **BetterWorks**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_01.png)

7. W okienku wyników wybierz **BetterWorks**, a następnie kliknij **wykonane** , aby dodać aplikację.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z BetterWorks opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w BetterWorks użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w BetterWorks musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w BetterWorks.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z BetterWorks, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie BetterWorks testowanie użytkownika](#creating-a-betterworks-test-user)** — aby odpowiednikiem Simona Britta w BetterWorks połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji BetterWorks.

Aplikacja BetterWorks oczekuje potwierdzeń SAML w określonym formacie. Skonfiguruj następujące oświadczeń dla tej aplikacji. Na karcie "**Atrribute**" aplikacji, możesz zarządzać wartości tych atrybutów. Następujące zrzut ekranu przedstawia przykładową to. 

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_06.png)

**Aby skonfigurować Azure AD rejestracji jednokrotnej z BetterWorks, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **BetterWorks** aplikacji w menu u góry kliknij **atrybutów**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_07.png)

2. W oknie dialogowym **atrybuty tokenu SAML** dla każdego wiersza pokazano w poniższej tabeli, wykonaj następujące czynności:
    

  	| Nazwa atrybutu | Wartość atrybutu |
  	| --- | --- |    
  	| saml_token | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

    . Kliknij przycisk **Dodaj atrybut użytkownika** , aby otworzyć okno dialogowe **Dodawanie Attribure użytkownika** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_12.png)
    
    b. W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    
    c. Na liście **Wartości atrybutu** wpisz identyfikator token saml wyświetlania dla tego wiersza.
    
    d. Kliknij polecenie **ukończone**

3. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_11.png) 

4. Na stronie **jak chcesz użytkownikom logowanie do BetterWorks** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_03.png)

5. Na stronie **Konfigurowanie ustawień aplikacji** okno dialogowe, jeśli chcesz skonfigurować aplikację w **protokołu IDP zainicjowane tryb**, wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_04.png)


    . W polu tekstowym **identyfikator** wpisz adres URL w następujący sposób:`https://app.betterworks.com/saml2/metadata/`


    b. W polu tekstowym **Adres URL odpowiedź** wpisz adres URL w następujący sposób:`https://app.betterworks.com/saml2/acs/`


    c. Kliknij przycisk **Dalej**

6. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , jeśli chcesz skonfigurować aplikację w **SP inicjowanych przez tryb**wykonywać na następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_10.png)

    .  Wybierz pozycję **Pokaż zaawansowane ustawienia (opcjonalnie)**.



    b. W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji BetterWorks przy użyciu następującego wzorca:`https://app.betterworks.com`

    b. Kliknij przycisk **Dalej**.

7. Na stronie **Konfigurowanie logowania jednokrotnego w BetterWorks** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_05.png)

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

8. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej BetterWorks za pośrednictwem <mailto:support@betterworks.com>. Dołączanie pliku pobranego metadanych i udostępnij go innym zespołowi BetterWorks Konfigurowanie logowania jednokrotnego na swojej stronie.

9. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-betterworks-test-user"></a>Tworzenie użytkownika test BetterWorks

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w BetterWorks. 

We współpracy z zespołem pomocy technicznej BetterWorks za pośrednictwem <mailto:support@betterworks.com> Aby dodać użytkowników do platformy BetterWorks.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej BetterWorks za pomocą Azure rejestracji jednokrotnej.
    
   ![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta BetterWorks, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **BetterWorks**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_50.png)

1. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka BetterWorks w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji BetterWorks.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_205.png
