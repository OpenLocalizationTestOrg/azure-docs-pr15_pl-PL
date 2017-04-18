<properties
    pageTitle="Samouczek: Azure Active Directory integracji z usługi sieci Web Amazon (AWS) | Microsoft Azure"
    description="Dowiedz się, jak użyć Amazon usług sieci Web (AWS) z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Samouczek: Azure Active Directory integracji z usługi sieci Web Amazon (AWS)

Celem tego samouczka jest pokazano, jak zintegrować Amazon usługi sieci Web (AWS) z usługą Azure Active Directory (Azure AD).  
Integrowanie Amazon usługi sieci Web (AWS) z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do usług sieci Web Amazon (AWS) 
- Można umożliwić użytkownikom automatyczne pobieranie podpisane na do usługi sieci Web Amazon (AWS) (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować integrację Azure AD przy użyciu usługi sieci Web Amazon (AWS), potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Usługa sieci Web Amazon (AWS) logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Opis scenariuszy
Celem tego samouczka jest ułatwia testowanie Azure AD logowania jednokrotnego w środowisku testowym.  
Scenariusz przedstawionych w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie usługi sieci Web Amazon (AWS) z galerii 
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Dodawanie usługi sieci Web Amazon (AWS) z galerii
Aby skonfigurować integrację z usługi sieci Web Amazon (AWS) w usłudze Azure Active Directory, musisz dodać Amazon usługi sieci Web (AWS) z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Aby dodać Amazon usługi sieci Web (AWS) z galerii, wykonaj następujące czynności:

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1] 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** . 
   
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony. 
   
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**. 
   
    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Amazon usługi sieci Web (AWS)**.
   
    ![Aplikacje][5]

7. W okienku wyników wybierz **Amazon usługi sieci Web (AWS)**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Aplikacje][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
Celem w tej sekcji jest pokazano, jak skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Amazon w sieci Web usługi (AWS) opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi wie, co jest odpowiednikiem użytkownikowi w Amazon usługi sieci Web (AWS) użytkownika w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w Amazon usługi sieci Web (AWS) musi zostać ustanowione.  
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w Amazon usługi sieci Web (AWS).
 
Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej przy użyciu usługi sieci Web Amazon (AWS), należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD pojedynczego logowania jednokrotnego](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie usługi sieci Web Amazon (AWS) testowanie użytkownika](#creating-a-halogen-software-test-user)** — mają odpowiednikiem Simona Britta w Amazon usługi sieci Web (AWS) połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurowanie Azure AD pojedynczego logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w aplikacji usługi sieci Web Amazon (AWS).  
Aplikacja usługi sieci Web Amazon (AWS) oczekuje potwierdzeń SAML w określonym formacie, który należy dodać mapowanie atrybutu niestandardowego do konfiguracji **atrybuty token saml** . Następujące zrzut ekranu przedstawia przykładową to.


![Konfigurowanie logowania jednokrotnego][27]

**Aby skonfigurować Azure AD rejestracji jednokrotnej przy użyciu usługi sieci Web Amazon (AWS), wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji aplikacji **Usługi sieci Web Amazon (AWS)** kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7]

2. Na stronie **jak chcesz użytkownikom logowanie do usługi sieci Web Amazon (AWS)** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego][8]

3. Na stronie **Konfigurowanie ustawień aplikacji** okno kliknij przycisk Dalej. 

    ![Konfigurowanie ustawień aplikacji][9]
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w Amazon usługi sieci Web (AWS)** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego][10]

5. W oknie przeglądarki różnych logowania jednokrotnego do witryny firmy Amazon usługi sieci Web (AWS) jako administrator.

6. Kliknij pozycję **Strona główna konsoli**.

    ![Konfigurowanie logowania jednokrotnego][11]

7. Kliknij pozycję **Zarządzanie tożsamościami i dostępem**. 

    ![Konfigurowanie logowania jednokrotnego][12]

8. Kliknij pozycję **Dostawcy tożsamości**, a następnie kliknij **Tworzenie dostawcy**. 

    ![Konfigurowanie logowania jednokrotnego][13]

9. Na stronie okno dialogowe **Konfigurowanie dostawcy** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][14]

     . Jako **Typ dostawcy**wybierz pozycję **SAML**.

     b. W polu tekstowym **Nazwa dostawcy** , wpisz nazwę dostawcy (np.: *drewna*).

     c. Aby przekazać metadanych pobrany plik, kliknij pozycję **Wybierz plik**.

     d. Kliknij przycisk **Następny krok**.


10. Na stronie okno dialogowe **Sprawdź informacje dostawcy** kliknij przycisk **Utwórz**. 

    ![Konfigurowanie logowania jednokrotnego][15]

11. Kliknij opcję **role**, a następnie kliknij pozycję **Utwórz nową rolę**. 

    ![Konfigurowanie logowania jednokrotnego][16]

12. W oknie dialogowym **Ustawianie Nazwa roli** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][17]

    . W polu tekstowym **Nazwa roli** , wpisz nazwę roli (przykład: *TestUser*).

    b. Kliknij przycisk **Następny krok**.

13. W oknie dialogowym **Wybierz typ roli** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][18]

    . Wybierz **rolę dostępu dostawcy tożsamości**.

    b. W sekcji **dostęp udzielanie Web logowania jednokrotnego (WebSSO) do dostawców SAML** kliknij przycisk **Wybierz**.


14. W oknie dialogowym **Ustanawiania zaufania** należy wykonać następujące czynności:  

    ![Konfigurowanie logowania jednokrotnego][19]
     
     . Jako dostawca SAML wybierz dostawcę SAML utworzono previousley (np.: *drewna*) 

     b. Kliknij przycisk **Następny krok**.


15. W oknie dialogowym **Weryfikowanie zaufania roli** kliknij przycisk **Następny krok**. 

    ![Konfigurowanie logowania jednokrotnego][32]


16. W oknie dialogowym **Dołączanie zasad** kliknij przycisk **Następny krok**.  

    ![Konfigurowanie logowania jednokrotnego][33]


17. W oknie dialogowym **Przeglądanie** wykonaj następujące czynności:   

    ![Konfigurowanie logowania jednokrotnego][34]

     . Skopiuj wartość **Dowiedz roli** .

     b. Skopiuj wartość Dowiedz **Zaufane jednostki** .

     c. Kliknij przycisk **Utwórz roli**. 

18. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Co to jest Azure AD Connect][20]

19. Na stronie **Potwierdzenie rejestracji jednokrotnej** kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Co to jest Azure AD Connect][22]


20. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** . 

    ![Konfigurowanie logowania jednokrotnego][21]

21. Kliknij przycisk **Dodaj atrybut użytkownika**. 

    ![Konfigurowanie logowania jednokrotnego][23]

22. W oknie dialogowym Dodawanie atrybutu użytkownika wykonaj następujące czynności. 

    ![Konfigurowanie logowania jednokrotnego][24] 

    . W polu tekstowym **Nazwa atrybutu** wpisz **https://aws.amazon.com/SAML/Attributes/Role**.

    b. W polu tekstowym **Wartość atrybutu** wpisz **[Dowiedz roli wartość], [wartość zaufane Dowiedz jednostki]**.

    >[AZURE.TIP] Są to wartości, skopiowane w oknie dialogowym Przeglądanie po utworzeniu roli użytkownika. 

    c. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Dodawanie atrybutu użytkownika** .

23. Kliknij przycisk **Dodaj atrybut użytkownika**. 

    ![Konfigurowanie logowania jednokrotnego][23]


24. W oknie dialogowym Dodawanie atrybutu użytkownika wykonaj następujące czynności. 

    ![Konfigurowanie logowania jednokrotnego][25]


     . W polu tekstowym **Nazwa atrybutu** wpisz **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. W polu tekstowym **Wartość atrybutu** wpisz lub wybierz **user.userprincipalname** z listy rozwijanej.
     
    ![Konfigurowanie logowania jednokrotnego][35]
    

     c. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Dodawanie atrybutu użytkownika** .


25. Kliknij przycisk **Zastosuj zmiany**. 

    ![Konfigurowanie logowania jednokrotnego][26]





### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD

Celem w tej sekcji jest utworzenie użytkownika testowego w portalu klasyczny Azure o nazwie Simona Britta.

![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**. 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.
  2. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.
  3. Kliknij przycisk Dalej.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności: 

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** txtbox, typ **Simona**.
  
    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.
  
    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.
  
    b. Kliknij pozycję **pełne**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Tworzenie użytkownika test Amazon usługi sieci Web (AWS)

Celem w tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Amazon usługi sieci Web (AWS).

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Aby utworzyć użytkownika o nazwie Simona Britta w Amazon usługi sieci Web (AWS), wykonaj następujące czynności:

1. Zaloguj się do witryny firmy **Amazon usługi sieci Web (AWS)** jako administrator.

2. Kliknij ikonę **Strona główna konsoli** . 

    ![Konfigurowanie logowania jednokrotnego][11]

3. Kliknij pozycję tożsamości i uzyskać dostęp do zarządzania. 

    ![Konfigurowanie logowania jednokrotnego][28]

4. Na pulpicie nawigacyjnym kliknij pozycję Użytkownicy, a następnie kliknij tworzenie nowych użytkowników. 

    ![Konfigurowanie logowania jednokrotnego][29]

5. W oknie dialogowym Utwórz użytkownika wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego][30]

     . W polach tekstowych **Wpisz nazwy użytkowników** wpisz nazwę użytkownika Simona Brita (userprincipalname) w Azure AD.

     b. Kliknij przycisk **Utwórz**.




### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

Celem w tej sekcji jest włączenie Simona Britta używanie Azure rejestracji jednokrotnej, udzielanie jej dostępu do usług sieci Web Amazon (AWS).

![Przypisywanie użytkownika][31]

**Aby przypisać Simona Britta CloudPassage, wykonaj następujące czynności:**

1. Klasyczny portalu Azure aby otworzyć widok aplikacji w widoku katalogu, kliknij pozycję **aplikacje** w górnym menu.

    ![Przypisywanie użytkownika][26]

2. Na liście aplikacji zaznacz **Amazon usługi sieci Web (AWS)**.

    ![Przypisywanie użytkownika][27]

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][25]

1. Na liście Użytkownicy zaznacz **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][29]

### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.  
Po kliknięciu kafelka Amazon usługi sieci Web (AWS) w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji usługi sieci Web Amazon (AWS).


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















