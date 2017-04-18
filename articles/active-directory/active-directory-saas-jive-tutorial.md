<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Jive | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Jive."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Samouczek: Azure Active Directory Integracja z Jive

W tym samouczku dowiesz się, jak zintegrować Jive z usługą Azure Active Directory (Azure AD).

Integrowanie Jive z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Jive
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Jive (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Jive, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Jive logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Jive z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-jive-from-the-gallery"></a>Dodawanie Jive z galerii
Aby skonfigurować integracja Jive w usłudze Azure Active Directory, musisz dodać Jive z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Jive z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]
2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Jive**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. W okienku wyników wybierz **Jive**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Jive opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w Jive jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Jive musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Jive.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Jive, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie Jive testowanie użytkownika](#creating-a-jive-test-user)** — aby odpowiednikiem Simona Britta w Jive połączonego z reprezentacją Azure AD jej.
4. **[Konfigurowanie obsługi administracyjnej użytkownika](#configuring-user-provisioning)** — do konspektu, jak włączyć użytkownika inicjowania obsługi administracyjnej usługi Active Directory użytkownika konta do Jive.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
6. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Jive.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Jive, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Jive** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak czy chcesz użytkownikom logowanie do Jive** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji Jive przy użyciu następującego wzorca: **https://\<nazwisko klienta\>. jivecustom.com**.
    
    b. Kliknij przycisk **Dalej**
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w Jive** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Logowania jednokrotnego do Twojej dzierżawy Jive jako administrator.

6. W menu u góry kliknij "**Saml**".

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    . Na karcie **Genaral** , wybierz opcję **włączone** .

    b. Kliknij przycisk "**Zapisz wszystkie ustawienia saml**".

7. Przejdź do karty "**Metadanych protokołu Idp**".

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    . Skopiuj zawartość metadanych pobrany plik XML, a następnie wklej go w polu tekstowym **metadanych dostawcy tożsamości (protokołu IDP)** .

    b. Kliknij przycisk "**Zapisz wszystkie ustawienia saml**". 

8. Przejdź do karty "**Mapowania atrybutu użytkowników**".

    ![Konfigurowanie logowania jednokrotnego na stronie aplikacji](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    . W polu tekstowym **wiadomości E-mail** skopiuj i Wklej nazwę atrybutu wartości **poczty** .

    b. W polu tekstowym **Imię** skopiuj i Wklej nazwę atrybut **Imię** wartości.

    c. W polu tekstowym **Nazwisko** skopiuj i Wklej nazwę atrybutu wartości **nazwisko** .
    
9. W portal Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej i kliknij przycisk **Dalej**.
![Azure AD rejestracji jednokrotnej][10]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.


![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:  ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** , wykonaj następujące czynności: ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



###<a name="creating-a-jive-test-user"></a>Tworzenie użytkownika test Jive

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Jive. We współpracy z zespołem pomocy technicznej Jive, aby dodać użytkowników do platformy Jive.


###<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej konta użytkowników usługi Active Directory w celu Jive użytkownika.  
W ramach tej procedury jest wymagane podanie tokenu zabezpieczającego użytkownika, które należy poprosić Jive.com.
  
Następujące zrzut ekranu przedstawia przykładowe okna dialogowego pokrewne w Azure AD:

![Konfigurowanie użytkowników inicjowania obsługi administracyjnej.] (./media/active-directory-saas-jive-tutorial/IC698794.png "Konfigurowanie użytkowników inicjowania obsługi administracyjnej.")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  W portalu zarządzania Azure na stronie integracji **Jive** aplikacji kliknij pozycję **Konfiguruj przypisywanie użytkowników** , aby otworzyć okno dialogowe **Konfigurowanie przypisywanie użytkowników** .

2.  Na stronie **Wprowadź poświadczenia Jive, aby włączyć automatyczne użytkownika inicjowania obsługi administracyjnej** wprowadź następujące ustawienia konfiguracji:

    1.  W polu tekstowym **Nazwa użytkownika administratora Jive** wpisz nazwę konta Jive zawierającego profilu **Administrator systemu** w Jive.com przydzielone.

    2.  W polu tekstowym **Hasło administratora Jive** wpisz hasło dla tego konta.

    3.  W polu tekstowym **Adres URL dzierżawy Jive** wpisz adres URL dzierżawy Jive.

        >[AZURE.NOTE]Adres URL dzierżawy Jive jest adres URL, który jest używany przez organizację, aby zalogować się do Jive.  
        Zazwyczaj adres URL ma następujący format: **www.\< Organizacja\>. jive.com**.

    4.  Kliknij przycisk **Sprawdzanie poprawności** , aby Sprawdź konfigurację.

    5.  Kliknij przycisk **Dalej** , aby otworzyć stronę **potwierdzenia** .

3.  Na stronie **potwierdzenia** kliknij znacznik wyboru, aby zapisać konfigurację.
  
Możesz teraz utworzyć konto testowe, odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Jive za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Jive, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Jive**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka Jive w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Jive.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
