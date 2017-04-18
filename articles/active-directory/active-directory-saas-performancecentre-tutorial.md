<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z PerformanceCentre | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i PerformanceCentre."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Samouczek: Azure Active Directory Integracja z PerformanceCentre

Celem tego samouczka jest pokazano, jak zintegrować PerformanceCentre z usługi Azure Active Directory (Azure AD).  
Integrowanie PerformanceCentre z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do PerformanceCentre 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w PerformanceCentre (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — klasyczny portalu usługi Azure Active Directory

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z PerformanceCentre, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- PerformanceCentre logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie PerformanceCentre z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-performancecentre-from-the-gallery"></a>Dodawanie PerformanceCentre z galerii
Aby skonfigurować integrację Azure AD PerformanceCentre, musisz dodać PerformanceCentre z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać PerformanceCentre z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **PerformanceCentre**.
 
    ![Aplikacje][5]

7. W okienku wyników wybierz **PerformanceCentre**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z PerformanceCentre opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w PerformanceCentre użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w PerformanceCentre musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w PerformanceCentre.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z PerformanceCentre, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie PerformanceCentre testowanie użytkownika](#creating-a-halogen-software-test-user)** — aby odpowiednikiem Simona Britta w PerformanceCentre połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure AD i konfigurowanie logowania jednokrotnego w aplikacji PerformanceCentre.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z PerformanceCentre, wykonaj następujące czynności:**

1. W portalu klasyczny Azure AD na stronie integracji **PerformanceCentre** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do PerformanceCentre** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][7] 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][8] 
 
     . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w witrynie PerformanceCentre (przykład: *http://companyname.performancecentre.com/saml/SSO*).
 
     b. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w PerformanceCentre** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][9] 

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.



1. Logowania do witryny firmy **PerformanceCentre** jako administrator.

2. Na karcie po lewej stronie kliknij przycisk **Konfiguruj**.

    ![Azure AD rejestracji jednokrotnej][10]

2. Na karcie po lewej stronie kliknij pozycję **inne**, a następnie kliknij **Rejestracji jednokrotnej**.

    ![Azure AD rejestracji jednokrotnej][11]

2. **Protocol (protokół)**zaznacz **SAML**.

    ![Azure AD rejestracji jednokrotnej][12]

2. Otwórz plik pobrany metadanych w Notatniku, skopiuj zawartość, wklej go w polu tekstowym **Metadanych dostawcy tożsamości** i kliknij przycisk **Zapisz**.

    ![Azure AD rejestracji jednokrotnej][13]

2. Upewnij się, że wartości **Jednostek podstawowy adres URL** i **Adres URL identyfikator jednostki** są poprawne.

    ![Azure AD rejestracji jednokrotnej][14]


6. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Azure AD rejestracji jednokrotnej][15]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][16]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_09.png)  

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_05.png)  

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_06.png) 
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-performancecentre-test-user"></a>Tworzenie użytkownika test PerformanceCentre

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w PerformanceCentre.

**Aby utworzyć użytkownika o nazwie Simona Britta w PerformanceCentre, wykonaj następujące czynności:**

1. Logowanie się do witryny firmy PerformanceCentre jako administrator.

2. W menu po lewej stronie kliknij pozycję **Interrelate**, a następnie kliknij **Tworzenie uczestnika**.

    ![Tworzenie użytkownika][400]

4. W oknie dialogowym **Interrelate — tworzenie uczestnika** wykonaj następujące czynności:

    ![Tworzenie użytkownika][401]

    . Wpisz wymagane atrybuty Simona Britta do powiązanych pól tekstowych.
    
    > [AZURE.IMPORTANT] Atrybut nazwy użytkownika i Britta w PerformanceCentre musi być taka sama, jak nazwa użytkownika w Azure AD.


    b. Wybierz pozycję **Administrator klienta** jako **Wybierz rolę**. 

    c. Kliknij przycisk **Zapisz**.   


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej PerformanceCentre za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta PerformanceCentre, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **PerformanceCentre**.

    ![Przypisywanie użytkownika][202]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka PerformanceCentre w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji PerformanceCentre.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_01.png
[500]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_02.png

[6]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_03.png
[8]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_04.png
[9]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_05.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png
[15]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_06.png
[16]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_50.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png
[402]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_402.png



