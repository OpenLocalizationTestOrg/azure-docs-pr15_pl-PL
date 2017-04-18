<properties
    pageTitle="Samouczek: Integracja Azure Active Directory przy użyciu aplikacji wideo 23 | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i 23 wideo."
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


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Samouczek: Integracja Azure Active Directory przy użyciu aplikacji wideo 23

Celem tego samouczka jest pokazano, jak zintegrować 23 klip wideo z usługi Azure Active Directory (Azure AD).  
Integrowanie 23 wideo za pomocą Azure AD oferuje następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do 23 wideo 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w 23 wideo (jednokrotną) za pomocą kont Azure AD

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować integrację Azure AD przy użyciu aplikacji wideo 23, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- 23 wideo logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie 23 klipu wideo z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-23-video-from-the-gallery"></a>Dodawanie 23 klipu wideo z galerii
Aby skonfigurować integrację Azure AD 23 klip wideo, musisz dodać 23 klip wideo z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać 23 wideo z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **23 wideo**.

    ![Aplikacje][5]

7. W okienku wyników wybierz pozycję **23 wideo**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej przy użyciu aplikacji wideo 23 opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownika w 23 klip wideo, aby użytkownik w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w 23 wideo musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwa_użytkownika** 23 wideo w usłudze.
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z 23 wideo, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie pliku wideo 23 testowanie użytkownika](#creating-a-23-video-test-user)** — mają odpowiednikiem Simona Britta 23 wideo, które jest połączone z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie logowania jednokrotnego](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w 23 aplikacji wideo.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z 23 wideo, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji aplikacji **23 wideo** kliknij przycisk **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkowników do zalogowania się do 23 klip wideo** wybierz **Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Azure AD jednokrotnej][7] 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][8] 
 
     . W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL używane przez użytkowników do logowania się w witrynie wideo 23 (przykład: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Za pomocą SAML 2.0 Integracja z usługą Active Directory jest dostępna dla wszystkich użytkowników 23 wideo. Skontaktuj się z obsługą na [support@23company.com](mailto:support@23company.com) w razie potrzeby pokrewne metadane.

     b. Kliknij przycisk **Dalej**.
 
4. Na stronie **Konfigurowanie logowania jednokrotnego 23 klip wideo,** wykonaj następujące czynności:

    ![Azure AD rejestracji jednokrotnej][9] 

    . Kliknij przycisk Pobierz certyfikat, a następnie zapisz plik na komputerze.

    b. Skontaktuj się z zespołem pomocy technicznej wideo 23 za pośrednictwem [support@23company.com](mailto:support@23company.com), podaj im pobranego certyfikatu **Wystawcy adresu URL**, **Pojedynczy znak na adres URL usługi**, adres **URL Sign-Out pojedynczej**i następnie poproś o konfiguracji rejestracji Jednokrotnej dla aplikacji sieci 23 wideo. 

    c. Kliknij przycisk **Dalej**.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**. 

    ![Azure AD jednokrotnej][10]

7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
  
    ![Azure AD rejestracji jednokrotnej][11]




### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Na liście **katalogów** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   

  
 
### <a name="creating-a-23-video-test-user"></a>Tworzenie 23 użytkownika test wideo

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta 23 wideo w usłudze.

**Aby utworzyć użytkownika o nazwie Simona Britta 23 wideo, wykonaj następujące czynności:**

1. Logowanie się do witryny firmy 23 wideo jako administrator.

1. Przejdź do **ustawień**.


2. W sekcji **Użytkownicy** kliknij przycisk **Konfiguruj**.

    ![Przypisywanie użytkownika][400]

3. Kliknij przycisk **Dodaj nowego użytkownika**. 

    ![Przypisywanie użytkownika][401]

4. W sekcji **zaprosić inną osobę, aby dołączyć do tej witryny** wykonaj następujące czynności:

    ![Przypisywanie użytkownika][402]

    . W polu tekstowym **adresy E-mail** wpisz adres e-mail Simona Britta w Azure AD.

    b. Kliknij pozycję **Dodaj użytkownika**.   


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej do 23 wideo za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta do 23 wideo, wykonaj następujące czynności:**

1. W portalu klasyczny Azure aby otworzyć widok aplikacji w widoku katalogów, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji wybierz pozycję **23 wideo**.

    ![Przypisywanie użytkownika][202] 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu 23 kafelków klip wideo w panelu programu Access, możesz należy uzyskać automatycznie podpisane w 23 aplikacji wideo.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




