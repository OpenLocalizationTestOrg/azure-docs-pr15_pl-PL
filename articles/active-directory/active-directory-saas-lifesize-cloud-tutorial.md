<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z chmury Lifesize | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i chmura Lifesize."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Samouczek: Azure Active Directory Integracja z chmury Lifesize

W tym samouczku dowiesz się, jak integracji chmury Lifesize z usługi Azure Active Directory (Azure AD).

Integrowanie chmury Lifesize z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp w chmurze Lifesize
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w chmurze Lifesize (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji - portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z chmury Lifesize, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Chmura Lifesize logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym.

Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie chmury Lifesize z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Dodawanie chmury Lifesize z galerii
Aby skonfigurować integrację Azure AD chmury Lifesize, musisz dodać chmury Lifesize z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać chmury Lifesize z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory][1]
2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Lifesize chmury**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. W okienku wyników wybierz **Chmury Lifesize**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji można skonfigurować i badania Azure AD logowania jednokrotnego w chmurze Lifesize opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić, użytkownik musi w chmurze Lifesize jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w chmurze Lifesize musi zostać ustanowione.

Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w chmurze Lifesize.

Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z chmury Lifesize, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie w chmurze Lifesize testowanie użytkownika](#creating-a-lifesize-cloud-test-user)** — mają odpowiednikiem Simona Britta w chmurze Lifesize, która jest połączony z reprezentacją Azure AD jej.
4. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

W tej sekcji można włączyć Azure AD logowania jednokrotnego w portalu klasyczny i skonfigurować logowania jednokrotnego w chmurze Lifesize aplikacji.


**Aby skonfigurować Azure AD rejestracji jednokrotnej z chmury Lifesize, wykonaj następujące czynności:**

1. W portalu klasyczny na stronie integracji **Chmury Lifesize** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .
     
    ![Konfigurowanie logowania jednokrotnego][6] 

2. Na stronie **jak chcesz użytkowników do zalogowania się chmurze Lifesize** wybierz **Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    . W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników do logowania się w chmurze Lifesize aplikację za pomocą następującego wzorca: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Kliknij przycisk **Dalej**
 
4. Na stronie **Konfigurowanie logowania jednokrotnego w chmurze Lifesize** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    . Kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik na komputerze.

    b. Kliknij przycisk **Dalej**.


5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, zaloguj się do aplikacji chmury Lifesize z uprawnieniami administratora.

6. W prawym górnym rogu kliknij swoją nazwę, a następnie kliknij na stronie **Ustawienia zaawansowane**

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. W teraz ustawienia zaawansowane, kliknij łącze **Logowania jednokrotnego konfiguracji** . Spowoduje to otwarcie strony konfiguracji rejestracji Jednokrotnej dla wystąpienia.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Teraz można skonfigurować następujące wartości w konfiguracji logowania jednokrotnego interfejsu użytkownika.    

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Skopiuj wartość adres URL wystawcy z Azure AD i wklej tę wartość w polu tekstowym **Wystawcy dostawcy tożsamości** .

    • Skopiuj wartość adres URL logowania zdalnego z Azure AD i wklej tę wartość w polu tekstowym **Adres URL logowania** .

    • Otworzyć pobranego certyfikatu w Notatniku i skopiuj zawartość certyfikatu, bez linii certyfikat rozpoczęcia i zakończenia certyfikat, Wklej to w polu tekstowym **Certyfikat X.509** .

    • Mapowanie SAML atrybut **Imię w** polu tekstowym wprowadź wartość w postaci **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • Mapowanie atrybutu SAML **Nazwisko w** polu tekstowym wprowadź wartość w postaci **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • Mapowanie atrybutu SAML dla pola tekstowego **E-mail** wprowadź wartość w postaci **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Aby sprawdzić konfigurację można kliknij przycisk **Test** .

    > [AZURE.NOTE] Do pomyślnego testowania należy zakończyć działanie Kreatora konfiguracji w Azure AD i zapewniają również dostęp do użytkowników lub grup, które można wykonać test.
    
10. Włącz SSO, zaznaczając pole wyboru przycisk **Włącz logowania jednokrotnego** .

11. Teraz kliknij przycisk **Aktualizuj** , aby wszystkie ustawienia są zapisywane. Spowoduje to wartość RelayState. Skopiuj wartość RelayState, która jest generowany w polu tekstowym. Potrzebujemy tej wartości w następnych kroków.

12. W portalu klasyczny wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

13. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
 
    ![Azure AD rejestracji jednokrotnej][11]

14. Teraz Zaloguj się do portalu zarządzania Azure **https://portal.azure.com** przy użyciu poświadczeń administratora

15. Kliknij łącze **Więcej usług** w lewym okienku nawigacji
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Wyszukiwanie Azure Active Directory i kliknij łącze **Azure Active Directory**
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Spowoduje znalezienie wszystkich aplikacji władz akredytacji bezpieczeństwa pod przyciskiem **Aplikacji przedsiębiorstwa** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Teraz kliknij łącze **Wszystkie aplikacje** w następnym karta
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Wyszukiwanie aplikacji Lifesize, dla której chcesz skonfigurować RelayState. 
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Teraz kliknij łącze **logowania jednokrotnego** w karta

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Pojawi się pole wyboru **Pokaż zaawansowane ustawienia adresu URL** . Kliknij pole wyboru.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Teraz skonfigurować RelayState do zastosowania, który zostanie wyświetlony na stronie Konfiguracja Lifesize aplikacji rejestracji Jednokrotnej. 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Zapisz ustawienia.

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.


![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:  ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** , wykonaj następujące czynności: ![tworzenia użytkowników testowych Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasła tymczasowego** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika testowego Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowego hasła**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Tworzenie użytkownika test Lifesize chmury

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w chmurze Lifesize. Chmura Lifesize obsługuje automatyczne użytkownika inicjowania obsługi administracyjnej. Po pomyślnym uwierzytelnieniu w Azure AD użytkownik zostanie automatycznie przygotowana w aplikacji. 


### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej w chmurze Lifesize za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta w chmurze Lifesize, należy wykonać następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Lifesize chmury**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203]

4. Na liście Użytkownicy zaznacz **Simona Britta**.

5. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji można przetestować Azure AD pojedynczego logowania jednokrotnego konfigurację przy użyciu panelu programu Access.

Po kliknięciu kafelka chmury Lifesize w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Lifesize chmury.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
