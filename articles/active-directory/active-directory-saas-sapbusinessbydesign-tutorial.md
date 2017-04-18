<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z ByDesign firm SAP | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i ByDesign firm SAP."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Samouczek: Azure Active Directory Integracja z ByDesign firm SAP

W tym samouczku dowiesz się, jak integracji ByDesign firm SAP z usługi Azure Active Directory (Azure AD).

Integrowanie SAP firm ByDesign z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do ByDesign firm SAP
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w ByDesign firm SAP (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z ByDesign firm SAP, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- ByDesign firm SAP logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie ByDesign firm SAP z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Dodawanie ByDesign firm SAP z galerii
Aby skonfigurować integrację Azure AD ByDesign firm SAP, musisz dodać ByDesign firm SAP z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać ByDesign firm SAP z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.


3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **ByDesign firm SAP**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. W okienku wyników wybierz **ByDesign firm SAP**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Usługi Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z ByDesign firm SAP opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w ByDesign firm SAP jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w ByDesign firm SAP musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w ByDesign firm SAP.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ByDesign firm SAP, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie ByDesign firm SAP testowanie użytkownika](#creating-an-sap-business-bydesign-test-user)** — aby odpowiednikiem Simona Britta w ByDesign firm SAP połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji ByDesign firm SAP.

Aplikacja ByDesign firm SAP oczekuje potwierdzeń SAML w określonym formacie. Skonfiguruj następujące oświadczeniach dla tej aplikacji. Na karcie **"Atrribute"** aplikacji, możesz zarządzać wartości tych atrybutów. Następujące zrzut ekranu przedstawia przykładową to. 


**Aby skonfigurować Azure AD rejestracji jednokrotnej z ByDesign firm SAP, wykonaj następujące czynności:**


1. W portalu klasyczny Azure, na stronie integracji **ByDesign firm SAP** aplikacji w menu u góry kliknij **atrybutów**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Na liście atrybutów token SAML atrybuty wybierz atrybut nameidentifier, a następnie kliknij **Edytuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. W oknie dialogowym Edytowanie atrybutu użytkownika wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    . Na liście wartości atrybutu zaznacz **ExtractMailPrefix()** fuction

    b. Na liście Poczta wybierz atrybut użytkownika, który ma być używany dla implementacji. 
    Na przykład jeśli chcesz użyć jako identyfikator użytkownika unikatowe pole Identyfikator pracownika, a wartość atrybutu są przechowywane w ExtensionAttribute2, wybierz **user.extensionattribute2**. 

    c. Kliknij pozycję **pełne**. 
    

4. W portalu klasyczny na stronie integracji **ByDesign firm SAP** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

5. Na stronie **jak chcesz użytkownikom logowanie do ByDesign firm SAP** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji ByDesign firm SAP przy użyciu następującego wzorca:`https://<servername>.sapbydesign.com`
    
    b. Kliknij przycisk **Dalej**
 
7. Na stronie **Konfigurowanie logowania jednokrotnego w ByDesign firm SAP** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


8. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, wykonaj następujące czynności:

    . Logowanie się do portalu sieci ByDesign firm SAP z uprawnieniami administratora.

    b. Przejdź do **aplikacji i typowych zadań zarządzania użytkownika** , a następnie kliknij kartę **Dostawcy tożsamości** .

    c. Kliknij przycisk **Nowy dostawcy tożsamości** i wybierz plik XML metadanych, pobranego z portalu klasyczny Azure. Importując metadane, system automatycznie przekaże wymagany podpis certyfikatu i certyfikatu szyfrowania.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Aby dodać **Adres URL usługi dla klientów indywidualnych potwierdzenia** do wezwania SAML, zaznacz **Obejmują potwierdzenia dla klientów indywidualnych adres URL usługi**.

    e. Kliknij przycisk **Aktywuj rejestracji jednokrotnej**.

    f. Zapisz wprowadzone zmiany.

    g. Kliknij kartę **Moje systemu** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Skopiuj adres **URL logowania jednokrotnego** i wklej je w polu tekstowym **Azure AD znak na adres URL** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    mogę. Określ, czy pracownika ręcznie można wybrać zalogowanie się przy użyciu Identyfikatora użytkownika i hasła lub logowania jednokrotnego, wybierając **Zaznaczenie dostawcy tożsamości ręcznie**.

    j. W sekcji **Adres URL logowania jednokrotnego** Określ adres URL, który ma być używany przez pracownika do logowania do systemu. 
    W adres URL wysyłane do listy rozwijanej pracownika można wybrać jedną z następujących opcji:
    
    **Adres URL nie logowania jednokrotnego**
 
    System wyśle odbiorcy adres URL system normalny pracownika. Pracownika nie można zalogować się przy użyciu logowania jednokrotnego, musi za pomocą hasła lub zamiast tego certyfikatu.

    **ADRES URL LOGOWANIA JEDNOKROTNEGO** 

    System wyśle odbiorcy adres URL logowania jednokrotnego pracownika. Pracownika można zalogować się przy użyciu logowania jednokrotnego. Żądanie uwierzytelnienia jest przekierowywane za pośrednictwem protokołu IdP.

    **Wybór automatyczny**
 
    Jeśli logowania jednokrotnego nie jest aktywne, system wyśle odbiorcy pracownika adres URL system normalny. Jeśli logowania jednokrotnego jest aktywne, system sprawdza, czy hasło jest pracownika. Jeśli hasło jest dostępna, zarówno logowania jednokrotnego adresu URL i adresów URL logowania jednokrotnego osoby, które nie są wysyłane do pracownika. Jeśli pracownik nie ma hasła, adres URL logowania jednokrotnego są wysyłane do pracownika.

    k. Zapisz wprowadzone zmiany.

9. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
 
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.


![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:
    
    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Tworzenie użytkownika test ByDesign firm SAP

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w ByDesign firm SAP. Zobacz Praca z ByDesign firm SAP zespół pomocy technicznej, aby dodać użytkowników do platformy ByDesign firm SAP. 

> [AZURE.NOTE] Upewnij się, wartość NameID powinny być zgodne z pola username platformy ByDesign firm SAP.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej ByDesign firm SAP za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta ByDesign firm SAP, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **ByDesign firm SAP**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka ByDesign firm SAP w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji ByDesign firm SAP.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
