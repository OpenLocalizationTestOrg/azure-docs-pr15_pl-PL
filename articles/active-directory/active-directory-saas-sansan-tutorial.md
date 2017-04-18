<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z SanSan | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i SanSan."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Samouczek: Azure Active Directory Integracja z SanSan

W tym samouczku dowiesz się, jak zintegrować SanSan z usługą Azure Active Directory (Azure AD).

Integrowanie SanSan z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do SanSan
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w SanSan (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z SanSan, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- SanSan logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Microsoft Microsoft Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie SanSan z galerii
2. Konfigurowanie i testowanie Microsoft Azure AD logowania jednokrotnego


## <a name="adding-sansan-from-the-gallery"></a>Dodawanie SanSan z galerii
Aby skonfigurować integrację Azure AD SanSan, musisz dodać SanSan z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać SanSan z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **SanSan**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. W okienku wyników wybierz **SanSan**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Microsoft Azure AD logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Microsoft Azure AD rejestracji jednokrotnej z SanSan opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w SanSan jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w SanSan musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w SanSan.

Aby skonfigurować i przetestować Microsoft Azure AD rejestracji jednokrotnej z SanSan, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie programu Microsoft Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Microsoft Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie SanSan testowanie użytkownika](#creating-an-sansan-test-user)** — aby odpowiednikiem Simona Britta w SanSan połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta użyć programu Microsoft Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfigurowanie platformy Microsoft Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć Microsoft Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji SanSan.


**Aby skonfigurować program Microsoft Azure AD rejestracji jednokrotnej z SanSan, wykonaj następujące czynności:**


1. W portalu klasyczny Azure na stronie integracji **SanSan** aplikacji kliknij pozycję Konfiguruj rejestracji jednokrotnej aby otworzyć okno dialogowe Konfigurowanie logowania jednokrotnego.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. Na stronie **jak chcesz użytkownikom logowanie do SanSan** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL w następujący sposób:

  	| Środowisko             | ADRES URL |
  	| :--                     | :-- |
  	| Komputer w sieci web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Natywnej aplikacji dla urządzeń przenośnych       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Ustawienia przeglądarki dla urządzeń przenośnych | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. W polu tekstowym **identyfikator** wpisz adres URL w następujący sposób: 

  	| Środowisko             | ADRES URL |
  	| :--                     | :-- |
  	| Komputer w sieci web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Natywnej aplikacji dla urządzeń przenośnych       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Ustawienia przeglądarki dla urządzeń przenośnych | `https://ap.sansan.com/s/saml2/<company name>` |


    c. Kliknij przycisk **Dalej**.

4. Na stronie **Konfigurowanie logowania jednokrotnego w SanSan** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5.  Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, kontaktów zespół pomocy technicznej SanSan i są pomocne w Konfigurowanie logowania jednokrotnego. Podaj im następujące informacje:

    • Pobranego **certyfikatu**

    • **Identyfikator dostawcy tożsamości**

    • Adres **URL SAML logowania jednokrotnego**

    • **Pojedynczy Wyloguj adres URL usługi**

> [AZURE.NOTE] Ustawienia przeglądarki komputera również działać dla aplikacji dla urządzeń przenośnych i przeglądarki dla urządzeń przenośnych wraz z komputera w sieci web. 


6. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD

W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.
Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-sansan-test-user"></a>Tworzenie użytkownika test SanSan

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w SanSan. Aplikacja SanSan potrzebują użytkownika ma zostać zastrzeżona w aplikacji przed wykonaniem logowania jednokrotnego. 


> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika lub wsadowe użytkowników, należy skontaktować się z obsługą SanSan.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej SanSan za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta SanSan, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **SanSan**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Microsoft Azure AD rejestracji jednokrotnej konfigurację przy użyciu panelu programu Access.
Po kliknięciu kafelka SanSan w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji SanSan.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
