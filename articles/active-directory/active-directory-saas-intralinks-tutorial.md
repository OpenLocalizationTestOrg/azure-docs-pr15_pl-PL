<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Intralinks | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Intralinks."
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


# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Samouczek: Azure Active Directory Integracja z Intralinks

W tym samouczku dowiesz się, jak zintegrować Intralinks z usługą Azure Active Directory (Azure AD).

Integrowanie Intralinks z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Intralinks
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Intralinks (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Intralinks, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Intralinks logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Intralinks z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-intralinks-from-the-gallery"></a>Dodawanie Intralinks z galerii
Aby skonfigurować integrację Azure AD Intralinks, musisz dodać Intralinks z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Intralinks z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Intralinks**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_01.png)

7. W okienku wyników wybierz **Intralinks**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego

W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Intralinks opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w Intralinks jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Intralinks musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Intralinks.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Intralinks, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Intralinks testowanie użytkownika](#creating-an-intralinks-test-user)** — aby odpowiednikiem Simona Britta w Intralinks połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji Intralinks.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z Intralinks, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Intralinks** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do Intralinks** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji Intralinks przy użyciu następującego wzorca: **https://\<nazwy firmy\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<identyfikator Azure AD dzierżawy\>**.

    b. Kliknij przycisk **Dalej**.
    
4. Na stronie **Konfigurowanie logowania jednokrotnego w Intralinks** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_05.png)

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej Intralinks i pocztę e-mail Dołącz metadanych pobrany plik.


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

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:  ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** , wykonaj następujące czynności: ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-intralinks-test-user"></a>Tworzenie użytkownika test Intralinks

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Intralinks. We współpracy z zespołem pomocy technicznej Intralinks, aby dodać użytkowników do platformy Intralinks.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Intralinks za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Intralinks, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Intralinks**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]

### <a name="adding-intralinks-via-or-elite-application"></a>Dodawanie aplikacji Intralinks za pośrednictwem lub Elite
Intralinks używa tej samej platformie pojedynczy znak na tożsamości innych aplikacjach Intralinks wykluczenie węzła transakcji aplikacji. Jeśli zamierzasz użyć dowolnej innej aplikacji Intralinks następnie najpierw trzeba więc Konfigurowanie rejestracji jednokrotnej dla jednej aplikacji Intralinks podstawowy zgodnie z powyższym opisem.

Następnie możesz wykonać poniżej procedurę, aby dodać innej aplikacji Intralinks w Twojej dzierżawy, które mogą korzystać z tej aplikacji podstawowego dla rejestracji jednokrotnej. 

> [AZURE.NOTE] Zapoznaj się z notatki, że ta funkcja jest dostępna tylko dla klientów SKU Azure AD Premium i nie są dostępne bezpłatnie lub podstawowa jednostka SKU klientów.

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]
2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. Kliknij przycisk tabulator lewy na karcie **niestandardowe**

    ![Dodawanie aplikacji Intralinks za pośrednictwem lub Elite](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_51.png)

7. Nadaj nazwę odpowiedniej aplikacji np **Intralinks Elite** i kliknij przycisk Zakończ przycisk.

8. Kliknij przycisk **Konfigurowania rejestracji jednokrotnej**

9. Wybierz opcję **Istniejący rejestracji jednokrotnej**

    ![Dodawanie aplikacji Intralinks za pośrednictwem lub Elite](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_52.png)

10. Uzyskiwanie SP zainicjowane logowania jednokrotnego adres URL Intralinks zespołu stosowania Intralinks i wprowadź je, jak pokazano poniżej. 

    ![Dodawanie aplikacji Intralinks za pośrednictwem lub Elite](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_53.png)

    . W polu tekstowym logowania na adres URL wpisz adres URL używane przez użytkowników do logowania się w aplikacji Intralinks przy użyciu następującego wzorca: **https://\<NazwaFirmy\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<AzureADTenantID\> **


11. Kliknij przycisk **Dalej**.

12. Przypisywanie aplikacji do użytkowników lub grup, jak pokazano w sekcji ** [Przypisywanie użytkowników testowych Azure AD](#assigning-the-azure-ad-test-user)**


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka Intralinks w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Intralinks.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_205.png
