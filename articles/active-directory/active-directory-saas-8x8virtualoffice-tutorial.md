<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z pakietem Office wirtualnych 8 x 8 | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować logowania jednokrotnego między Azure Active Directory i 8 x 8 wirtualnych pakietu Office."
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


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Samouczek: Azure Active Directory integrację z pakietem Office wirtualnych 8 x 8

Celem tego samouczka jest pokazano, jak zintegrować 8 x 8 wirtualnych Office z usługą Azure Active Directory (Azure AD).

Integrowanie 8 x 8 wirtualnych pakietu Office z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do pakietu Office wirtualnych 8 x 8
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Office wirtualnych 8 x 8 (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD Integracja z pakietem Office wirtualnych 8 x 8, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- 8 x 8 wirtualnych Office logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Microsoft Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Office wirtualnych 8 x 8 z galerii
2. Konfigurowanie i testowanie Microsoft Azure AD logowania jednokrotnego


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Dodawanie Office wirtualnych 8 x 8 z galerii
Aby skonfigurować integrację Azure AD Office wirtualnych 8 x 8, musisz dodać Office wirtualnych 8 x 8 z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Office wirtualnych 8 x 8 z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .
    
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Office wirtualnych 8 x 8**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. W okienku wyników wybierz pozycję **Office wirtualnych 8 x 8**, a następnie kliknij **wykonane** w celu dodania aplikacji.

    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Microsoft Azure AD logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Microsoft Azure AD rejestracji jednokrotnej z 8 x 8, Virtual pakietu Office na podstawie których test użytkownika o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w wirtualnej pakiet Office do użytkownika w Azure AD jest 8 x 8. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w 8 x 8 wirtualnych pakietu Office musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w pakiecie Office wirtualnych 8 x 8.

Aby skonfigurować i przetestować Microsoft Azure AD logowania jednokrotnego w pakiecie Office wirtualnych 8 x 8, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie programu Microsoft Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Microsoft Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika test wirtualne Office 8 x 8](#creating-a-8x8-virtual-office-test-user)** - mają odpowiednikiem Simona Britta w pakiecie Office wirtualnych 8 x 8 połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta użyć programu Microsoft Azure AD rejestracji jednokrotnej.
5. **[Testowanie logowania jednokrotnego](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfigurowanie platformy Microsoft Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć Microsoft Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji pakietu Office wirtualnych 8 x 8.

**Aby skonfigurować program Microsoft Azure AD rejestracji jednokrotnej z pakietem Office wirtualnych 8 x 8, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie **Office wirtualnych 8 x 8** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak czy chcesz użytkowników do zalogowania się do pakietu Office wirtualnych 8 x 8** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    . W polu tekstowym **Adres URL odpowiedź** wpisz:`https://sso.8x8.com/saml2`

    b. Kliknij przycisk **Dalej**

4. Na stronie **Konfigurowanie logowania jednokrotnego w biurze wirtualnych 8 x 8** wykonaj następujące czynności, a następnie kliknij przycisk **Dalej**:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.

5. Logowania jednokrotnego do Twojej dzierżawy wirtualnych Office 8 x 8 jako administrator.
6. Wybierz pozycję **Menedżer kont wirtualnych pakietu Office** na panelu aplikacji.

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Wybierz konto **firmowe** zarządzania i kliknij przycisk **Zaloguj** .

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Kliknij kartę **konta** na liście menu.

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Kliknij pozycję **Rejestracji jednokrotnej** na liście kont.

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Zaznacz pole wyboru **Signle jednokrotną** w obszarze metody uwierzytelniania, a następnie kliknij pozycję **SAML**.

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Skopiuj **adres URL logowania jednokrotnego SAML**, **pojedyncze Sing się adres URL usługi** i **adres URL wystawcy** z Azure AD **logować się**do adresu URL, **Wyloguj się adresu URL** i **adres URL wystawcy** w pakiecie Office wirtualnych 8 x 8. 

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Konfigurowanie na stronie aplikacji](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Kliknij przycisk **przeglądarki** , aby przekazać certyfikat, który został pobrany z usługi Azure Active Directory.

13. Kliknij przycisk **Zapisz** .

14. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

15. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny o nazwie Simona Britta.
    
![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Tworzenie użytkownika test wirtualną Office 8 x 8

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w pakiecie Office wirtualnych 8 x 8. 8 x 8 wirtualnych pakiet Office obsługuje w czasie inicjowania obsługi administracyjnej, która jest domyślnie włączona.

Nie ma żadnych czynności do wykonania dla Ciebie w tej sekcji. Nowy użytkownik zostanie utworzony podczas próby uzyskiwać dostęp do pakietu Office wirtualnych 8 x 8, jeśli go jeszcze nie istnieje. 

> [AZURE.NOTE] Jeśli trzeba ręcznie utworzyć użytkownika, musisz skontaktuj się z zespołem pomocy technicznej wirtualnych Office 8 x 8.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej 8 x 8 wirtualnych Office za pomocą Azure rejestracji jednokrotnej.
    
![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta Office wirtualnych 8 x 8, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji wybierz pozycję **Office wirtualnych 8 x 8**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. W menu u góry kliknij pozycję **Użytkownicy**.
    
    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Microsoft Azure AD rejestracji jednokrotnej konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka wirtualnych Office 8 x 8 w panelu programu Access, możesz należy uzyskać automatycznie podpisane na do aplikacji pakietu Office wirtualnych 8 x 8.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
