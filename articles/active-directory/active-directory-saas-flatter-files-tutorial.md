<properties
    pageTitle="Samouczek: Azure Active Directory integracji z plikami płaska | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i płaska plików."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Samouczek: Azure Active Directory integracji z plikami płaska

Celem tego samouczka jest pokazano, jak zintegrować płaska pliki z usługi Azure Active Directory (Azure AD).  
Integrowanie płaska pliki z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do plików płaska 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w płaska plików (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralnej — klasyczny portalu usługi Azure Active Directory

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z plikami płaska, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Pliki płaska logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie plików płaska z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-flatter-files-from-the-gallery"></a>Dodawanie plików płaska z galerii
Aby skonfigurować integrację Azure AD płaska plików, musisz dodać płaska pliki z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać pliki płaska z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Płaska plików**.


    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. W okienku wyników wybierz **Płaska plików**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z plikami płaska opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w plikach płaska użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w plikach płaska musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w plikach płaska.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z plikami płaska, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie plików płaska testowanie użytkownika](#creating-a-halogen-software-test-user)** — aby odpowiednikiem Simona Britta w płaska plików, które jest połączone z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure AD i konfigurowanie logowania jednokrotnego w aplikacji płaska plików. W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu. Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

Aby skonfigurować rejestracji jednokrotnej dla plików płaska, potrzebujesz domeny zarejestrowane. Jeśli nie masz domeny zarejestrowane jeszcze, kontakt płaska plików pomocy technicznej zespołu za pomocą [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Aby skonfigurować Azure AD rejestracji jednokrotnej z plikami płaska, wykonaj następujące czynności:**

1. W portalu klasyczny Azure AD na stronie integracji **Płaska plików** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do plików płaska** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
 
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Na stronie **Konfigurowanie ustawień aplikacji** okno kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Pliki płaska używa tego samego adresu URL logowania rejestracji Jednokrotnej dla wszystkich klientów: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Na stronie **Konfigurowanie logowania jednokrotnego płaska pliki** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


1. Zaloguj się w aplikacji płaska pliki jako administrator.

2. Kliknij pozycję pulpit nawigacyjny. 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Kliknij pozycję **Ustawienia**, a następnie na karcie **firmy** należy wykonać następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    . Zaznacz pole wyboru **Użyj SAML 2.0 uwierzytelniania**.

    b. Kliknij przycisk **Konfiguruj SAML**.



2. W oknie dialogowym **Konfiguracja SAML** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    . W polu tekstowym domeny wpisz domenę zarejestrowanych.

    > [AZURE.NOTE] Jeśli nie masz domeny zarejestrowane jeszcze, kontakt płaska plików pomocy technicznej zespołu za pomocą [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. Platformy Azure klasyczny portalu, w konfiguracji pojedynczego logowania jednokrotnego w okno dialogowe płaska plików, copt pojedynczy znak na adres URL usługi, a następnie wklej go w polu tekstowym adres URL dostawcy tożsamości.

    c.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

    >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    d.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka, a następnie wkleić go w polu tekstowym **FlatterFiles certyfikatu dostawcy tożsamości** .

    e. Kliknij przycisk **Aktualizuj**.

6. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Tworzenie użytkownika test płaska plików

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w plikach płaska.

**Aby utworzyć użytkownika o nazwie Simona Britta w plikach płaska, wykonaj następujące czynności:**

1. Logowanie się do witryny firmy **Płaska pliki** jako administrator.

2. W okienku nawigacji po lewej stronie kliknij pozycję **Ustawienia**, a następnie kliknij **kartę**użytkownicy.

    ![Cfreate płaska użytkownika plików](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Kliknij pozycję **Dodaj użytkownika**. 

4. W oknie dialogowym **Dodaj użytkownika,** wykonaj następujące czynności:

    ![Cfreate płaska użytkownika plików](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    . W polu tekstowym **Imię** wpisz **Britta**.

    b. W polu tekstowym **Nazwa** wpisz **Simona**. 

    c. W polu tekstowym **Adres E-mail** wpisz adres e-mail osoby Britta w portalu klasyczny Azure.

    d. Kliknij przycisk **Prześlij**.   


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej płaska plików za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta płaska pliki, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji wybierz pozycję **Pliki płaska**.

    ![Przypisywanie użytkownika](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka płaska plików w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji płaska plików.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






