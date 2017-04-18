<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z ADP eTime | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i ADP eTime."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Samouczek: Azure Active Directory Integracja z ADP eTime

Celem tego samouczka jest pokazano, jak zintegrować ADP eTime z usługą Azure Active Directory (Azure AD).  
Integrowanie ADP eTime z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do ADP eTime
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w eTime ADP (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure


Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z ADP eTime, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Plik ADP eTime logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie ADP eTime z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-adp-etime-from-the-gallery"></a>Dodawanie ADP eTime z galerii
Aby skonfigurować integrację Azure AD ADP eTime, musisz dodać ADP eTime z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać plik ADP eTime z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **ADP eTime**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. W okienku wyników wybierz **ADP eTime**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z eTime ADP opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownika w eTime ADP użytkownikowi w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w ADP eTime musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w ADP eTime.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z ADP eTime, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie ADP eTime testowanie użytkownika](#creating-a-adpetime-test-user)** — ma odpowiednika Simona Britta w eTime ADP, połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji eTime ADP.

Aplikacja eTime ADP oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji atrybuty tokenu SAML. Następujące zrzut ekranu przedstawia przykładową to. Nazwa oświadczeń będzie zawsze **"PersonImmutableID"** i wartości, których Pracujemy obecnie ExtensionAttribute2, która zawiera pole Identyfikator pracownika użytkownika. W tym miejscu wierzc Azure AD mapowania użytkownika do ADP eTime zostaną wykonane na pole Identyfikator pracownika, ale można było mapować na inną wartość, również oparte na ustawienia aplikacji. Praca, zapoznaj się z tak zespołowi eTime ADP najpierw za pomocą prawidłowy identyfikator użytkownika i mapowanie tę wartość z roszczeń **"PersonImmutableID"** .  

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Przed skonfigurowaniem potwierdzenia SAML, należy skontaktować się z zespołem pomocy technicznej eTime ADP i poproś wartość atrybutu Unikatowy identyfikator dla dzierżawy. Potrzebujesz tej wartości, aby skonfigurować niestandardowe oświadczeń dla aplikacji.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z ADP eTime, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **ADP eTime** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkownikom logowanie do ADP eTime** wybierz **Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** , wykonaj następujące czynności:.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    . W polu tekstowym **Adres URL odpowiedź** , wpisz adres URL używane przez użytkowników do logowania się w aplikacji eTime ADP przy użyciu następującego wzorca: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Kliknij przycisk **Dalej**.

4. Na stronie **Konfigurowanie logowania jednokrotnego w ADP eTime** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    . Kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej eTime ADP i poczty e-mail Dołącz pobrany plik metadanych, można je skonfigurować do integracji logowania jednokrotnego.

    > [AZURE.NOTE] Po **ADP eTime** zespołu skonfigurować wystąpienie, Pobierz wartość **RelayState** z nich. Wykonaj wymienione poniżej kroki, aby go skonfigurować. Po tej konfiguracji można przetestować integracja. Dlatego Uwaga jest ważne konfiguracji dla tego integracji aplikacji do pracy.

6. Aby skonfigurować wartość RelayState w Azure AD, wykonaj następujące czynności: 
    
    . Zaloguj się do [Portalu zarządzania Azure](https://portal.azure.com) jako administrator.

    b. W okienku nawigacji po lewej stronie kliknij pozycję **Więcej usług**. 
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. W polu tekstowym **Wyszukaj** wpisz **Usługi Azure Active Directory**, a następnie kliknij łącze pokrewnych.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Kliknij przycisk **aplikacje dla przedsiębiorstw**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. W sekcji **Zarządzanie** kliknij pozycję **Wszystkie aplikacje**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. W polu tekstowym **Wyszukaj** wpisz **ADP eTime**, a następnie kliknij łącze pokrewnych. 
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. W sekcji **Zarządzanie** kliknij **rejestracji jednokrotnej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Wybierz pozycję **Pokaż zaawansowane ustawienia adresu URL**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    mogę. W polu tekstowym **Stan przekazywania** wpisz wartość za pomocą następujących wzorów:
    
    - Systemy operacyjne:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Środowisko tymczasowego:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. Zapisz ustawienia.

7. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  

    ![Azure AD rejestracji jednokrotnej][11]



### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.  
Na liście Użytkownicy zaznacz **Simona Britta**.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    . **Typ użytkownika**zaznacz **nowego użytkownika w Twojej organizacji**.

    b. W polu tekstowym **Nazwa użytkownika** wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-adp-etime-test-user"></a>Tworzenie użytkownika test eTime ADP

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w ADP eTime. We współpracy z zespołem pomocy technicznej eTime ADP, aby dodać użytkowników na koncie eTime ADP. 


> [AZURE.NOTE]Jeśli trzeba ręcznie utworzyć użytkownika, musisz skontaktuj się z zespołem pomocy technicznej eTime ADP.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta do udostępnienia jej ADP eTime za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta ADP eTime, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **ADP eTime**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]



### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka eTime ADP w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji eTime ADP.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
