<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z HCM równa Dayforce Ceridian | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i HCM równa Dayforce Ceridian."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Samouczek: Azure Active Directory Integracja z HCM równa Dayforce Ceridian

Celem tego samouczka jest pokazano, jak zintegrować HCM równa Dayforce Ceridian z usługi Azure Active Directory (Azure AD).  
Integrowanie HCM równa Dayforce Ceridian z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do HCM równa Dayforce Ceridian
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w HCM równa Dayforce Ceridian (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure


Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z HCM równa Dayforce Ceridian, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- HCM równa Dayforce Ceridian logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie HCM równa Dayforce Ceridian z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Dodawanie HCM równa Dayforce Ceridian z galerii
Aby skonfigurować integrację Azure AD HCM równa Dayforce Ceridian, musisz dodać HCM równa Dayforce Ceridian z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać HCM równa Dayforce Ceridian z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **HCM równa Dayforce Ceridian**.

    ![Aplikacje](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. W okienku wyników wybierz **HCM równa Dayforce Ceridian**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z HCM równa Dayforce Ceridian opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w HCM równa Dayforce Ceridian użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w HCM równa Dayforce Ceridian musi zostać ustanowione.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z HCM równa Dayforce Ceridian, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie HCM równa Ceridian Dayforce testowanie użytkownika](#creating-a-ceridian-dayforce-hcm-test-user)** — aby odpowiednikiem Simona Britta w Ceridian HCM równa Dayforce połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji HCM równa Dayforce Ceridian.

Aplikacja HCM równa Dayforce Ceridian oczekuje potwierdzeń SAML w określonym formacie. Zobacz Praca z zespołu HCM równa Dayforce najpierw do identyfikowania identyfikator odpowiednich użytkowników. Firma Microsoft zaleca używanie atrybutu **"Nazwa"** jako identyfikator użytkownika. Możesz zarządzać wartość tego atrybutu w oknie dialogowym **"Atrribute"** . Następujące zrzut ekranu przedstawia przykładową to. 

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Aby skonfigurować Azure AD rejestracji jednokrotnej z HCM równa Dayforce Ceridian, wykonaj następujące czynności:**




1. W portalu klasyczny Azure, na stronie integracji **HCM równa Dayforce Ceridian** aplikacji w menu u góry kliknij **atrybutów**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Na liście **atrybutów tokenu saml** atrybuty wybierz atrybut nazwy, a następnie kliknij **Edytuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. W oknie dialogowym **Edytowanie atrybutu użytkownika** wykonaj następujące czynności:
 
    . Z listy **Wartość atrybutu** wybierz atrybut użytkownika, który ma być używany dla implementacji.  
    Na przykład jeśli chcesz użyć jako identyfikator użytkownika unikatowe pole Identyfikator pracownika, a wartość atrybutu są przechowywane w ExtensionAttribute2, wybierz **user.extensionattribute2**. 

    b. Kliknij pozycję **pełne**.  
    



1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. W portalu klasyczny Azure, na stronie integracji **HCM równa Dayforce Ceridian** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego**.

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do HCM równa Dayforce Ceridian** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji HCM równa Dayforce Ceridian. W przypadku środowisku produkcyjnym Użyj następującego formatu adresu URL:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Dla jasności zastąpić DayforcehcmNamespace nazw środowiska lub Identyfikatora firmy; na przykład:`https://sso.dayforcehcm.com/contoso`
    
    W środowiskach test należy użyć następującego formatu adresu URL:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. W polu tekstowym **Adres URL odpowiedź** wpisz adres URL używane przez Azure AD ogłaszać odpowiedzi.  
    W przypadku środowisku produkcyjnym Użyj:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    W środowiskach testowych za pomocą:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Na stronie **Konfigurowanie logowania jednokrotnego w HCM równa Dayforce Ceridian** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej HCM równa Dayforce Ceridian za pośrednictwem poczty e-mail i podaj im następujące czynności:

    - Plik certyfikatu pobrany
    - Adres **URL wystawcy**
    - Adres **URL logowania jednokrotnego SAML** 
    - **Pojedynczy Wyloguj adres URL usługi** 


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Tworzenie użytkownika test HCM równa Dayforce Ceridian

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w HCM równa Dayforce Ceridian. 

We współpracy z zespołem pomocy technicznej HCM równa Dayforce Ceridian uzyskanie dodanych użytkowników usługi w aplikacji HCM równa Dayforce Ceridian. 





### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej HCM równa Dayforce Ceridian za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta HCM równa Dayforce Ceridian, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **HCM równa Dayforce Ceridian**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka HCM równa Dayforce Ceridian w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji HCM równa Dayforce Ceridian.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
