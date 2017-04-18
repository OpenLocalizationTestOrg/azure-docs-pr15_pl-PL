<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z O.C. Napisu Czarnecka - AppreciateHub | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i O.C. Napisu Czarnecka - AppreciateHub."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Samouczek: Azure Active Directory Integracja z O.C. Napisu Czarnecka - AppreciateHub

Celem tego samouczka jest pokazano, jak zintegrować O.C. Napisu Czarnecka - AppreciateHub z Azure Active Directory (Azure AD).  
Integrowanie O.C. Napisu Czarnecka - AppreciateHub z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do O.C. Napisu Czarnecka - AppreciateHub 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w O.C. Napisu Czarnecka - AppreciateHub (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD integracji z O.C. Napisu Czarnecka - AppreciateHub, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- O.C. Napisu Czarnecka - logowania jednokrotnego AppreciateHub enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie O.C. Napisu Czarnecka - AppreciateHub z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>Dodawanie O.C. Napisu Czarnecka - AppreciateHub z galerii
Aby skonfigurować integracja O.C. Napisu Czarnecka - AppreciateHub w usłudze Azure Active Directory, musisz dodać O.C. Napisu Czarnecka - AppreciateHub z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać napisu Czarnecka O.C. - AppreciateHub z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1] 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2] 

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3] 

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4] 

6. W polu wyszukiwania wpisz **Napisu Czarnecka O.C. - AppreciateHub**.

    ![Aplikacje][5] 

7. W okienku wyników wybierz **Napisu Czarnecka O.C. - AppreciateHub**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego

Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z O.C. Napisu Czarnecka - AppreciateHub opartą na użytkowniku test o nazwie "Britta Simona".

Dla rejestracji jednokrotnej do pracy należy poznać użytkownika odpowiednika w O.C. Azure AD Napisu Czarnecka - jest AppreciateHub użytkownikowi w Azure AD. Innymi słowy relacji łącza między użytkownikiem usługi Azure AD i użytkownikowi w O.C. Napisu Czarnecka - AppreciateHub musi zostać ustanowione.  
Łącze relacji nawiązano przypisując wartość **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w O.C. Napisu Czarnecka - AppreciateHub.
 
Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z O.C. Napisu Czarnecka - AppreciateHub, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie O.C. napisu Czarnecka - AppreciateHub test użytkownika](#creating-a-halogen-software-test-user)** — aby odpowiednikiem Simona Britta w O.C. Napisu Czarnecka - AppreciateHub połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w sieci O.C. Napisu Czarnecka - AppreciateHub aplikacji.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z napisu Czarnecka O.C. - AppreciateHub, należy wykonać następujące czynności:**

1. W portalu klasyczny Azure, na stronie **Napisu Czarnecka O.C. - AppreciateHub** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6]

2. Na stronie **jak chcesz użytkownikom logowanie do napisu Czarnecka O.C. - AppreciateHub** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][7]

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie ustawień aplikacji][8]
 
     . Otwórz plik metadanych, korzystając z następującego łącza: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Odszukaj węzeł **md:AssertionConsumerService** . 

     c. Skopiuj wartość atrybutu **lokalizacji** . 

     ![Konfigurowanie ustawień aplikacji][12]
     
     d. W polu tekstowym **Logowania na adres URL** w przeszłości wartość uzyskaniu w poprzednim kroku.

     > [AZURE.NOTE] Jeśli masz problemy expiriencing uzyskiwania adresu URL odpowiedzi z pliku metadanych, skontaktuj się z O.C. Napisu Czarnecka - AppreciateHub zespół pomocy technicznej za pośrednictwem [sso@octanner.com](mailto:sso@octanner.com).

     e. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w napisu Czarnecka O.C. - AppreciateHub** kliknij przycisk **Pobierz metadane**, a następnie zapisz plik metadane lokalnie na komputerze.

    ![Co to jest Azure AD Connect][9]

5. Skontaktuj się z O.C. Napisu Czarnecka - AppreciateHub zespół pomocy technicznej za pośrednictwem xyz, podaj im pliku metadanych, a ich Poinformuj należy pozwalający rejestracji Jednokrotnej dla Ciebie.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Co to jest Azure AD Connect][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Co to jest Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Tworzenie O.C. Napisu Czarnecka - AppreciateHub użytkowników testowych

Celem w tej sekcji jest utworzyć użytkownika o nazwie Simona Britta w O.C. Napisu Czarnecka - AppreciateHub.

**Aby utworzyć użytkownika o nazwie Simona Britta się napisu Czarnecka O.C. - AppreciateHub, wykonaj następujące czynności:**

1. Poproś z zespołem pomocy technicznej napisu OC Czarnecka Utwórz użytkownika, który zawiera w nameID atrybucie tę samą wartość co nazwa użytkownika Simona Britta w Azure AD.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej O.C. za pomocą Azure logowania jednokrotnego Napisu Czarnecka - AppreciateHub.

![Przypisywanie użytkownika][200]

**Aby przypisać Simona Britta do napisu Czarnecka O.C. - AppreciateHub, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201]

2. Na liście aplikacji zaznacz **Napisu Czarnecka O.C. - AppreciateHub**.

    ![Przypisywanie użytkownika][202]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu przycisku O.C. Napisu Czarnecka - AppreciateHub kafelków w panelu programu Access możesz należy uzyskać automatycznie podpisane w Twojej O.C. Napisu Czarnecka - AppreciateHub aplikacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






