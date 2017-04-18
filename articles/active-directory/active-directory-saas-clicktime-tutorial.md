<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z ClickTime | Microsoft Azure" 
    description="Dowiedz się, jak użyć ClickTime z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Samouczek: Azure Active Directory Integracja z ClickTime

W tym samouczku dowiesz się, jak zintegrować ClickTime z usługą Azure Active Directory (Azure AD).

Integrowanie ClickTime z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do ClickTime
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w ClickTime (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z ClickTime, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- ClickTime logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie ClickTime z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego

##<a name="adding-clicktime-from-the-gallery"></a>Dodawanie ClickTime z galerii

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla ClickTime.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla ClickTime, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **ClickTime**.

    ![Galeria aplikacji] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Galeria aplikacji")

7.  W okienku wyników wybierz **ClickTime**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z ClickTime opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w ClickTime jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w ClickTime musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w ClickTime.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ClickTime, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie ClickTime testowanie użytkownika](#creating-a-clicktime-test-user)** — aby odpowiednikiem Simona Britta w ClickTime połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania ClickTime przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  


>[AZURE.IMPORTANT] Aby móc skonfigurować rejestracji jednokrotnej na Twojej dzierżawy ClickTime, należy skontaktować się najpierw techniczną ClickTime, aby uzyskać ta funkcja jest włączona.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z ClickTime, wykonaj następujące czynności:**

1.  W portalu klasyczny Azure, na stronie integracji **ClickTime** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do ClickTime** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Konfigurowanie logowania jednokrotnego")

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    . W polu tekstowym **IdentifierL** wpisz adres URL przy użyciu następującego wzorca: **https://app.clicktime.com/sp/**
    
    b. W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL przy użyciu następującego wzorca: **https://app.clicktime.com/Login/**

    c. Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w ClickTime** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Konfigurowanie logowania jednokrotnego")

4.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy ClickTime jako administrator.

5.  Na pasku narzędzi u góry kliknij polecenie **Preferencje**, a następnie kliknij pozycję **Ustawienia zabezpieczeń**.

6.  W sekcji Konfiguracja **Preferencji rejestracji jednokrotnej** wykonaj następujące czynności:

    ![Ustawienia zabezpieczeń] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Ustawienia zabezpieczeń")

    .  Wybierz opcję **Zezwalaj** logowania przy użyciu jednego logowania jednokrotnego (SSO) z **usługą Azure Active Directory**.
    
    b.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w ClickTime** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **Punktu końcowego dostawcy tożsamości** .

    c.  Otwórz base-64 certyfikat zakodowany w **Notatniku**, skopiuj zawartość, a następnie wklej go w polu tekstowym **Certyfikat X.509** .
    
    d.  Kliknij przycisk **Zapisz**.

7.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do ClickTime, musi być przygotowana do ClickTime.  
W przypadku ClickTime inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **ClickTime** .

2.  Na pasku narzędzi u góry kliknij **firmy**, a następnie kliknij pozycję **osoby**.

    ![Osoby] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Osoby")

3.  Kliknij przycisk **Dodaj osoby**.

    ![Dodawanie osoby] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Dodawanie osoby")

4.  W sekcji nowej osoby wykonaj następujące czynności:

    ![Osoby] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Osoby")

    .  W polu tekstowym **adres e-mail** wpisz adres e-mail swojego konta Azure AD.
    
    b.  W polu tekstowym **Imię i nazwisko** wpisz nazwę swojego konta Azure AD.  

    >[AZURE.NOTE] Jeśli chcesz, możesz skonfigurować dodatkowe właściwości nowego obiektu osoby.

    c.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można użyć dowolnego innego ClickTime użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczony przez ClickTime do zapewniania obsługi Azure AD kont użytkowników.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej ClickTime za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200]

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

**Aby przypisać Simona Britta ClickTime, wykonaj następujące czynności**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **ClickTime**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]

## <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego
W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka ClickTime w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji ClickTime.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png