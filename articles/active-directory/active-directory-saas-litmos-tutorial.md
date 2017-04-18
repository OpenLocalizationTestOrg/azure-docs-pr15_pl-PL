<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Litmos | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Litmos."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Samouczek: Azure Active Directory Integracja z Litmos

Celem tego samouczka jest pokazano, jak zintegrować Litmos z usługi Azure Active Directory (Azure AD).  
Integrowanie Litmos z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do Litmos 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Litmos (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — usługi Azure Active Directory 

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z Litmos, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Litmos logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie Litmos z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-litmos-from-the-gallery"></a>Dodawanie Litmos z galerii
Aby skonfigurować integrację Azure AD Litmos, musisz dodać Litmos z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Litmos z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Litmos**.

    ![Aplikacje][5]

7. W okienku wyników wybierz **Litmos**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Litmos opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w Litmos użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Litmos musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Litmos.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Litmos, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Litmos testowanie użytkownika](#creating-a-halogen-software-test-user)** — aby odpowiednikiem Simona Britta w Litmos połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure AD i konfigurowanie logowania jednokrotnego w aplikacji Litmos.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

W ramach konfiguracji musisz dostosować **Atrybuty tokenu SAML** aplikacji Litmos.  

![Azure AD rejestracji jednokrotnej][17] 

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Litmos, wykonaj następujące czynności:**

1. W portalu klasyczny Azure AD na stronie integracji **Litmos** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do Litmos** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
 
    ![Azure AD rejestracji jednokrotnej][7] 


1. Zaloguj się na Litmos firmowej witryny (przykład: *https://azureapptest.litmos.com/account/Login*) jako administrator.

    ![Azure AD rejestracji jednokrotnej][21] 


1. Na pasku nawigacyjnym po lewej stronie kliknij polecenie **konta**.

    ![Azure AD rejestracji jednokrotnej][22] 


1. Kliknij kartę **integracji** .

    ![Azure AD rejestracji jednokrotnej][23] 


1. Na karcie **integracji** przewiń w dół do **Integracji strona 3**, a następnie kliknij kartę **SAML 2.0** .

    ![Azure AD rejestracji jednokrotnej][24] 

1. Skopiuj wartość w obszarze **jest endoiint SAML dla litmos:**.

    ![Azure AD rejestracji jednokrotnej][26] 


3. W portalu klasyczny Azure na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][8] 
 
    . W polu tekstowym **identyfikator** wpisz adres URL używane przez użytkowników do logowania się w aplikacji Litmos (przykład: *https://azureapptest.litmos.com/account/Login*).
     
    b. W polu tekstowym **Adres URL odpowiedź** Wklej wartość skopiowany z poziomu aplikacji Litmos w poprzednim kroku.

    c. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w Litmos** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][2] 

    . Kliknij przycisk Pobierz certyfikat, a następnie zapisz plik na komputerze.


1. W aplikacji **Litmos** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][25] 

    . Kliknij opcję **Włącz SAML**.

    b. Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

    >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    c. Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **Certyfikat X.509 SAML** .

    d. Kliknij przycisk **Zapisz zmiany**.


6. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  
    ![Azure AD rejestracji jednokrotnej][11]


20. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** . 

    ![Konfigurowanie logowania jednokrotnego][12]


24. W oknie dialogowym **Dodawanie atrybutu użytkownika** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][14]

  	| Nazwa atrybutu | Wartość atrybutu |
  	| ---            | ---             |
  	| Adres e-mail          | User.mail       |
  	| Imię      | User.givenName  |
  	| Nazwisko       | User.surname    |

    Dla każdego wiersza danych w powyższej tabeli wykonaj następujące czynności:
   
    . Kliknij przycisk **Dodaj atrybut użytkownika**. 

    ![Konfigurowanie logowania jednokrotnego][15]


    . W polu tekstowym **Nazwa atrybutu** wpisz **Nazwę atrybutu** wyświetlania dla tego wiersza.

    b. Wybierz **Wartość atrybutu** wyświetlania dla tego wiersza.

    c. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Dodawanie atrybutu użytkownika** .


25. Kliknij przycisk **Zastosuj zmiany**. 

    ![Konfigurowanie logowania jednokrotnego][16]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure clasic**w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Tworzenie użytkownika test Litmos

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Litmos.  
Aplikacja Litmos obsługuje tylko na czas inicjowania obsługi administracyjnej. Oznacza to konto użytkownika jest automatycznie tworzona w razie potrzeby podczas próby uzyskania dostępu do aplikacji przy użyciu panelu programu Access.

**Aby utworzyć użytkownika o nazwie Simona Britta w Litmos, wykonaj następujące czynności:**


1. Zaloguj się na Litmos firmowej witryny (przykład: *https://azureapptest.litmos.com/account/Login*) jako administrator.

    ![Azure AD rejestracji jednokrotnej][21] 


1. Na pasku nawigacyjnym po lewej stronie kliknij polecenie **konta**.

    ![Azure AD rejestracji jednokrotnej][22] 


1. Kliknij kartę **integracji** .

    ![Azure AD rejestracji jednokrotnej][23] 


1. Na karcie **integracji** przewiń w dół do **Integracji strona 3**, a następnie kliknij kartę **SAML 2.0** .

    ![Azure AD rejestracji jednokrotnej][24] 

1. Wybierz pozycję **Automatyczne użytkowników:**.

    ![Azure AD rejestracji jednokrotnej][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej Litmos za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Litmos, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Litmos**.

    ![Przypisywanie użytkownika][202] 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka Litmos w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Litmos.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





