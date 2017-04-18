<properties
    pageTitle="Samouczek: Azure Active Directory integrację ze Qlik sens | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Qlik sens przedsiębiorstwa."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Samouczek: Azure Active Directory Integracja z Qlik właściwe rozwiązanie Enterprise

W tym samouczku dowiesz się, jak integracji przedsiębiorstwa sens Qlik z usługi Azure Active Directory (Azure AD).

Integrowanie Qlik właściwe rozwiązanie Enterprise z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Qlik właściwe rozwiązanie Enterprise
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Qlik właściwe rozwiązanie Enterprise (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Qlik właściwe rozwiązanie Enterprise, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Qlik właściwe rozwiązanie Enterprise logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Qlik właściwe rozwiązanie Enterprise z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Dodawanie Qlik właściwe rozwiązanie Enterprise z galerii
Aby skonfigurować integrację Azure AD Qlik właściwe rozwiązanie Enterprise, musisz dodać Qlik właściwe rozwiązanie Enterprise z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Qlik właściwe rozwiązanie Enterprise z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]
2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Qlik właściwe rozwiązanie Enterprise**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. W okienku wyników wybierz opcję **Qlik właściwe rozwiązanie Enterprise**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z przedsiębiorstwa sens Qlik opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w przedsiębiorstwie sens Qlik jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w przedsiębiorstwie sens Qlik musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w usłudze Qlik właściwe rozwiązanie Enterprise.

Aby skonfigurować i przetestować Azure AD logowania jednokrotnego w usłudze Qlik właściwe rozwiązanie Enterprise, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie przedsiębiorstwa sens Qlik testowanie użytkownika](#creating-a-qliksense-enterprise-test-user)** — mają odpowiednikiem Simona Britta w przedsiębiorstwie sens Qlik połączonego z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w aplikacji Qlik właściwe rozwiązanie Enterprise.


**Aby skonfigurować Azure AD logowania jednokrotnego w usłudze Qlik właściwe rozwiązanie Enterprise, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Przedsiębiorstwa sens Qlik** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do wersji Enterprise sens Qlik** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w aplikacji przedsiębiorstwa sens Qlik przy użyciu następującego wzorca: **https://\<Qlik właściwe rozwiązanie w pełni Qualifed Hostname\>: 443 / < wirtualnych prefiks serwera Proxy\>/samlauthn/**.
    
    > [AZURE.NOTE] Uwaga ukośnika na końcu tego identyfikatora URI.  Wymagane jest.

    b. Kliknij przycisk **Dalej**
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w przedsiębiorstwie sens Qlik** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.  Przygotuj się na edytowanie tego pliku metadanych przed przekazaniem na serwerze sens Qlik.

    b. Kliknij przycisk **Dalej**.

5. Przygotowywanie pliku XML metadanych Federacji tak, aby można przekazać która do serwera sens Qlik.

    > [AZURE.NOTE] Przed przekazaniem metadanych protokołu IdP na serwerze sens Qlik plik musi być edytowane w celu usunięcia informacji, aby zapewnić poprawne działanie między Azure AD i sens Qlik serwera.

    ![QlikSense][qs24]

    . Otwórz plik FederationMetaData.xml pobrane z Azure w edytorze tekstów.

    b. Wyszukiwanie wartości **RoleDescriptor**.  Będzie cztery pozycje (dwóch par otwierających i zamykających znaczniki elementów).

    c. Usuwanie znaczników RoleDescriptor i wszystkich informacji między z pliku.

    d. Zapisz plik i pozostawienie jej pobliżu do użycia w dalszej części tego dokumentu.

6. Przejdź do Qlik sens Qlik Management Console (QMC) jako użytkownik, który można utworzyć konfiguracji wirtualnego serwera proxy.

7. W QMC kliknij element menu wirtualnego serwera Proxy.

    ![QlikSense][qs6] 

8. U dołu ekranu kliknij przycisk Utwórz nowy.

    ![QlikSense][qs7]

9. Zostanie wyświetlony ekran Edytuj wirtualnej serwera proxy.  Po prawej stronie ekranu jest menu umożliwiającego widoczne opcje konfiguracji.

    ![QlikSense][qs9]

10. Korzystając z opcji menu identyfikacyjne zaznaczone pole wyboru Wprowadź informacje identyfikacyjne dla konfiguracji Azure wirtualnego serwera proxy.

    ![QlikSense][qs8]  
    
    . Opis pole jest przyjazną nazwę konfiguracji wirtualnego serwera proxy.  Wprowadź wartość opis.
    
    b. Pole prefiksu identyfikuje punkt końcowy wirtualnego serwera proxy do łączenia się z sens Qlik z Azure AD rejestracji jednokrotnej.  Wprowadź nazwę unikatowy prefiks ten wirtualny serwer proxy.

    c. Limit czasu nieaktywności sesji (w minutach) jest limit czasu dla połączenia za pomocą tego wirtualnego serwera proxy.

    d. Nazwa nagłówka plików cookie sesji jest jego nazwę przechowywania identyfikator sesji dla sesji Qlik właściwe rozwiązanie, które użytkownik otrzymuje po pomyślnym uwierzytelnieniu.  Ta nazwa musi być unikatowa.

11. Kliknij opcję menu uwierzytelniania, aby był widoczny.  Zostanie wyświetlony ekran uwierzytelniania.

    ![QlikSense][qs10]

    . **Tryb dostępu anonimowego** rozwijanej określa użytkownikom anonimowym może udostępniać sens Qlik za pośrednictwem wirtualnych serwera proxy.  Nie użytkownikom anonimowym jest domyślna opcja.

    b. **Metody uwierzytelniania** rozwijanej określa użyje schemat uwierzytelniania wirtualnego serwera proxy.  Z listy rozwijanej wybierz pozycję SAML.  Więcej opcji pojawi się w wyniku.

    c. **Pole Identyfikator URI hosta SAML**wprowadzania że hostname użytkownicy będą mogli wprowadzać dostęp sens Qlik przy użyciu tego serwera proxy do wirtualnego SAML.  Nazwa hosta jest identyfikator uri serwera sens Qlik.

    d. W polu **identyfikator jednostki SAML**wprowadzić tę samą wartość wprowadzona w polu Identyfikator URI SAML hosta.

    e. **Metadane protokołu IdP SAML** jest plikiem edytowane wcześniej w sekcji **Edytowanie metadanych federacji z Azure AD konfiguracji** .  **Przed przekazaniem metadanych protokołu IdP trzeba będzie można edytować plik** , aby usunąć informacje, aby zapewnić poprawne działanie między Azure AD i sens Qlik serwera.  **Skorzystaj z instrukcjami powyżej, jeśli plik jeszcze nie będzie można edytować.**  Jeśli edytowano pliku kliknij przycisk Przeglądaj i wybierz odpowiedni plik edytować metadane, aby przekazać ją do konfiguracji wirtualnych serwera proxy.

    f. Wprowadź nazwę atrybutu lub odwołanie schematu dla atrybutu SAML reprezentującą **identyfikator użytkownika** Azure AD wyśle na serwerze sens Qlik.  Informacje o schemacie odwołanie jest dostępna w konfiguracji wpisu ekrany aplikacji Azure.  Aby użyć atrybutu name **Wprowadź http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.

    g. Wprowadź wartość dla **katalogu użytkownika** zostaną dołączone do użytkowników, gdy uwierzytelniania serwera sens Qlik przez Azure AD.  Wartości ustalony muszą być ujęte w **nawiasy kwadratowe []**.  Aby użyć atrybutu wysłane w potwierdzenia Azure AD SAML, wprowadź nazwę atrybutu w tym tekst pola **bez** nawiasów kwadratowych.

    h. **Algorytm podpisywania SAML** ustawia usługi dostawcy (w tym przypadku sens Qlik serwera) podpisania certyfikatu, konfiguracji wirtualnego serwera proxy.  Jeśli serwer sens Qlik używa zaufanego certyfikatu wygenerowanych przy użyciu programu Microsoft RSA rozszerzony i AES Cryptographic Provider, należy zmienić algorytm podpisywania SAML 256 **Agent kondycji systemu**.

    mogę. W sekcji mapowania atrybutu SAML umożliwia dodatkowych atrybutów, takich jak grupy były wysyłane do sens Qlik do użycia w regułach zabezpieczeń.

12. Kliknij opcję menu, aby był widoczny równoważenia obciążenia.  Zostanie wyświetlony ekran równoważenia obciążenia.

    ![QlikSense][qs11]

13. Kliknij przycisk Dodaj nowy serwera węzeł, wybierz pozycję aparat węzeł lub węzły sens Qlik wysyłanie sesji do celów równoważenia obciążenia i kliknij przycisk Dodaj.

    ![QlikSense][qs12]

14. Kliknij opcję Zaawansowane menu, aby wyświetlić ten plik. Zostanie wyświetlony ekran Zaawansowane.

    ![QlikSense][qs13]

    . Lista biały hosta identyfikuje nazwy hostów, które są akceptowane podczas nawiązywania połączenia z serwerem sens Qlik.  **Wprowadź nazwę hosta, który użytkownicy będą określać podczas łączenia się serwerem sens Qlik.** Nazwa hosta jest tę samą wartość co uri hosta SAML bez https://.

15. Kliknij przycisk Zastosuj.

    ![QlikSense][qs14]

16. Kliknij przycisk OK, aby zaakceptować komunikat ostrzegawczy informujący, zostanie uruchomiony ponownie serwery proxy połączone z wirtualnego serwera proxy.

    ![QlikSense][qs15]

17. Po prawej stronie ekranu zostanie wyświetlone menu elementy skojarzone.  Kliknij opcję menu serwery proxy.

    ![QlikSense][qs16]

18. Zostanie wyświetlony ekran serwera proxy.  Kliknij przycisk Połącz u dołu, aby utworzyć łącze do serwera proxy do wirtualnego serwera proxy.

    ![QlikSense][qs17]

19. Wybierz węzeł serwera proxy, będzie obsługiwać połączenia wirtualnej serwera proxy i kliknij przycisk Połącz.  Po połączeniu, serwer proxy zostaną wyświetlone w obszarze skojarzone serwery proxy.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Po około 5 do 10 sekund zostanie wyświetlony komunikat QMC odświeżania.  Kliknij przycisk Odśwież QMC.

    ![QlikSense][qs20]

21. Po odświeżeniu QMC kliknij element menu wirtualnych serwerów proxy. Nowy wpis wirtualnej serwera proxy SAML znajduje się w tabeli, na ekranie.  Kliknij wpis wirtualnej serwera proxy.

    ![QlikSense][qs51]

22. U dołu ekranu po aktywuje przycisk Pobierz SP metadanych.  Kliknij przycisk metadanych pobieranie programu SharePoint, aby zapisać metadane w pliku.

    ![QlikSense][qs52]

23. Otwórz plik metadanych programu SharePoint.  Obserwować wpis **identyfikator jednostki** oraz wpis **AssertionConsumerService** .  Wartości te są równoważne **identyfikator** i **Zaloguj się na adres URL** w konfiguracji aplikacji Azure AD. Jeśli nie pasują następnie należy zastąpić je w Kreatorze konfiguracji Azure AD aplikacji.

    ![QlikSense][qs53]

24. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

25. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
 
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.


![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:  ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** , wykonaj następujące czynności: ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Tworzenie użytkownika test Qlik właściwe rozwiązanie Enterprise

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w Qlik właściwe rozwiązanie Enterprise. Zobacz Praca z zespołem pomocy technicznej Qlik właściwe rozwiązanie Enterprise, aby dodać użytkowników do platformy Qlik właściwe rozwiązanie Enterprise.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Qlik właściwe rozwiązanie Enterprise za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Qlik właściwe rozwiązanie Enterprise, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji wybierz opcję **Qlik właściwe rozwiązanie Enterprise**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


## <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka Qlik właściwe rozwiązanie Enterprise w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Qlik właściwe rozwiązanie Enterprise.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png