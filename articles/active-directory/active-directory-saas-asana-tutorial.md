<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Asana | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Asana."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Samouczek: Azure Active Directory Integracja z Asana

W tym samouczku dowiesz się, jak zintegrować Asana z usługą Azure Active Directory (Azure AD).

Integrowanie Asana z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Asana
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Asana (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Asana, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- **Asana** logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Asana z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-asana-from-the-gallery"></a>Dodawanie Asana z galerii
Aby skonfigurować integrację Azure AD Asana, musisz dodać Asana z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Asana z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Asana**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. W okienku wyników wybierz **Asana**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Asana opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w Asana jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Asana musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Asana.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Asana, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Asana testowanie użytkownika](#creating-an-Asana-test-user)** — aby odpowiednikiem Simona Britta w Asana połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD pojedynczy logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji Asana.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Asana, wykonaj następujące czynności:**

1. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego][6]
2. W portalu klasyczny na stronie integracji **Asana** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7] 

3. Na stronie **jak chcesz użytkownikom logowanie do Asana** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    . W polu tekstowym logowania na adres URL wpisz adres URL przy użyciu następującego wzorca:`https://app.asana.com`

    c. Kliknij przycisk **Dalej**.

5. Na stronie **Konfigurowanie logowania jednokrotnego w Asana** kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze. Ponadto skopiuj wartość adres URL logowania jednokrotnego SAML.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Kliknij prawym przyciskiem myszy certyfikat, a następnie otwórz plik certyfikatu przy użyciu Notatnika lub edytora tekstu preferowany. Kopiowanie zawartości między rozpoczęcia i zakończenia tytuł certyfikatu. Jest to certyfikat X.509, że można będzie użyć w Asana w celu skonfigurowania logowania jednokrotnego.

6. W oknie przeglądarki różnych logowania jednokrotnego w aplikacji Asana. Aby skonfigurować logowania jednokrotnego w Asana, uzyskać dostęp do ustawień obszaru roboczego, klikając nazwę obszaru roboczego w prawym górnym rogu ekranu. Następnie kliknij na ** \<swoją nazwę obszaru roboczego\> ustawienia**. 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. W oknie **Ustawienia organizacji** kliknij pozycję **Administracja**. Następnie kliknij przycisk **Członkowie, musisz zalogować się przy użyciu SAML** umożliwiający konfigurację logowania jednokrotnego. Wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    . W texbox **adres URL strony — logowania** Wklej znak SAML na adres URL z usługi Azure Active Directory.

    b. W polu tekstowym **Certyfikat X.509** wklejanie skopiowanych z Azure AD certyfikat X.509.

9. Kliknij przycisk **Zapisz**. Przejdź do [konfigurowania rejestracji Jednokrotnej Przewodnik po Asana](https://asana.com/guide/help/premium/authentication#gl-saml) , jeśli potrzebujesz dodatkowej pomocy.

7. Przejdź do **Konfigurowanie logowania jednokrotnego w Asana** strony Azure AD, wybierz pozycję potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-asana-test-user"></a>Tworzenie użytkownika test Asana

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Asana.

1. Na **Asana**przejdź do sekcji **zespołów** w panelu po lewej stronie. Kliknij przycisk ze znakiem plus. 

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Wpisz wiadomość e-mail britta.simon@contoso.com w polu tekstowym, a następnie wybierz pozycję **Zaproś**.
3. Kliknij przycisk **Wyślij zaproszenie**. Nowy użytkownik otrzymają wiadomość e-mail do swojego konta e-mail. Osoba należy utworzyć i sprawdzania poprawności konta.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Asana za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Asana, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Asana**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem ta sekcja służy do testowania usługi Azure AD rejestracji jednokrotnej.

Przejdź do strony logowania Asana. W wiadomości E-mail, adres w polu tekstowym Wstaw adres e-mail britta.simon@contoso.com. Pozostaw pole tekstowe hasła w puste pole, a następnie kliknij przycisk **Zaloguj**. Nastąpi przekierowanie do strony logowania Azure AD. Wykonywanie poświadczenia Azure AD. Teraz użytkownik jest zalogowany na Asana.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
