<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z MCM | Microsoft Azure" 
    description="Dowiedz się, jak użyć MCM z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Samouczek: Azure Active Directory Integracja z MCM
  
Celem tego samouczka jest pokazano, jak zintegrować MCM z usługą Azure Active Directory (Azure AD).

Integrowanie MCM z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do MCM
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w MCM (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z MCM, potrzebne następujące elementy:

- Ważna subskrypcja Azure
- MCM logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie MCM z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego

## <a name="adding-mcm-from-the-gallery"></a>Dodawanie MCM z galerii
Aby skonfigurować integrację Azure AD MCM, musisz dodać MCM z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać MCM z galerii, wykonaj następujące czynności:**

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **MCM**.

    ![Galeria aplikacji] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Galeria aplikacji")

7.  W okienku wyników wybierz **MCM**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z MCM opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w MCM użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w MCM musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w MCM.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z MCM, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie MCM testowanie użytkownika](#creating-a-mcm-test-user)** — aby odpowiednikiem Simona Britta w MCM połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego
  
W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji MCM.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z MCM, wykonaj następujące czynności:**

1.  W portalu klasyczny Azure, na stronie integracji **MCM** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do MCM** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Microsoft Azure AD jednokrotnej] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD jednokrotnej")

3.  Na stronie okno dialogowe Konfigurowanie ustawień aplikacji wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Skonfigurować adres URL aplikacji")

    . W polu tekstowym **Logowania na adres URL** wpisz: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w MCM** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Konfigurowanie logowania jednokrotnego")

5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej MCM. Dołączanie pliku pobranego metadanych i udostępnij go innym zespołowi MCM Konfigurowanie logowania jednokrotnego na swojej stronie.

6.  W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Konfigurowanie logowania jednokrotnego")

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Konfigurowanie logowania jednokrotnego")


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD

Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   

###<a name="creating-a-mcm-test-user"></a>Tworzenie użytkownika test MCM
  
W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w MCM. We współpracy z zespołem pomocy technicznej MCM, aby dodać użytkowników do platformy MCM.

>[AZURE.NOTE]Można użyć dowolnego innego MCM użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez MCM zabezpieczanie AAD kont użytkowników.


###<a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD
  
Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej MCM za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkowników] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Przypisywanie użytkowników")

**Aby przypisać Simona Britta MCM, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.
    
    ![Przypisywanie użytkowników] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Przypisywanie użytkowników")

2. Na liście aplikacji zaznacz **MCM**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkowników] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Przypisywanie użytkowników")

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.
    
    ![Przypisywanie użytkowników] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Przypisywanie użytkowników")


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.
 
Po kliknięciu kafelka MCM w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji MCM.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)