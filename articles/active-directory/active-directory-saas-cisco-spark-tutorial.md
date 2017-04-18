<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Cisco Spark | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Cisco Spark."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Samouczek: Azure Active Directory Integracja z Cisco Spark

W tym samouczku dowiesz się, jak integracji Cisco Spark z usługi Azure Active Directory (Azure AD).

Integrowanie Cisco Spark z usługą Azure Active Directory zapewnia następujące korzyści:

- Możesz sterować w Azure AD, kto ma dostęp do Cisco Spark
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w iskrowym Cisco (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont w jednym miejscu centralna — portalu klasyczny Azure

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować Azure AD integracji z Cisco Spark, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- **Cisco Spark** logowania jednokrotnego w enabled subskrypcji


> [AZURE.NOTE] Aby przetestować kroki opisane w tym samouczku, zaleca się używanie w środowisku produkcyjnym.


Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariuszy
W tym samouczku przetestować Azure AD logowania jednokrotnego w środowisku testowym. Scenariusz przedstawionych w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie Cisco Spark z galerii
2. Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego


## <a name="adding-cisco-spark-from-the-gallery"></a>Dodawanie Cisco Spark z galerii
Aby skonfigurować integrację Azure AD Cisco Spark, musisz dodać Cisco Spark z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Cisco Spark z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Cisco Spark**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. W okienku wyników wybierz **Cisco Spark**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie Azure AD pojedynczy logowania jednokrotnego
W tej sekcji Konfigurowanie i testowanie Azure AD rejestracji jednokrotnej z Spark Cisco opartą na użytkowniku test o nazwie "Britta Simona".

Rejestracji jednokrotnej do pracy Azure AD musi ustalić użytkownika odpowiednika w iskrowym Cisco jest do użytkownika, w Azure AD. Innymi słowy łącze powiązanie użytkownika w usłudze Azure AD użytkownikowi w iskrowym Cisco musi zostać ustanowione.
Łącze relacji jest określany przez przypisanie wartości **nazwy użytkownika** w Azure AD jako wartość **nazwy użytkownika** w iskrowym Cisco. Aby skonfigurować i przetestować Azure AD rejestracji jednokrotnej z Cisco Spark, należy wykonać następujące czynności bloków konstrukcyjnych:

1. **[Konfigurowanie Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Tworzenie Azure AD testowanie użytkownika](#creating-an-azure-ad-test-user)** — Aby przetestować Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie Cisco Spark testowanie użytkownika](#creating-a-cisco-spark-test-user)** — aby odpowiednikiem Simona Britta w iskrowym Cisco, połączonego z reprezentacją Azure AD jej.
5. **[Przypisywanie Azure AD testowanie użytkownika](#assigning-the-azure-ad-test-user)** — umożliwiające Simona Britta używać Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)** — Aby sprawdzić, czy działa konfiguracji.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD logowania jednokrotnego

Celem w tej sekcji jest, aby włączyć Azure AD logowania jednokrotnego w portalu klasyczny Azure i konfigurowanie logowania jednokrotnego w iskrowym Cisco aplikacji.

Aplikacja Spark Cisco oczekuje potwierdzeń SAML zawiera określonych atrybutów. Skonfiguruj następujące atrybuty dla tej aplikacji. Na karcie **"Atrributes"** aplikacji, możesz zarządzać wartości tych atrybutów. Następujące zrzut ekranu przedstawia przykładową to.

![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Aby skonfigurować Azure AD rejestracji jednokrotnej z Cisco Spark, wykonaj następujące czynności:**

1. W portalu klasyczny Azure, na stronie integracji **Cisco Spark** aplikacji w menu u góry kliknij **atrybutów**.
     
    ![Konfigurowanie logowania jednokrotnego][5]


2. W oknie dialogowym **atrybuty token SAML** wykonaj następujące czynności:

    . Kliknij przycisk **Dodaj atrybut użytkownika** , aby otworzyć okno dialogowe **Dodawanie Attribure użytkownika** .

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. W polu tekstowym **Nazwa atrybutu** wpisz **uid**.
    
    c. Na liście **Wartości atrybutu** zaznacz **user.userprincipal**.
    
    d. Kliknij pozycję **pełne**. Następnie **Zastosuj zmiany** u dołu strony.

3. W menu u góry kliknij **Szybki Start**.

    ![Konfigurowanie logowania jednokrotnego][6]

4. W portalu klasyczny na stronie integracji **Cisco Spark** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie logowania jednokrotnego** .

    ![Konfigurowanie logowania jednokrotnego][7] 

5. Na stronie **jak chcesz użytkowników do zarejestrowania się w iskrowym Cisco** wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.
    
    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Na stronie okno dialogowe **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    . W polu tekstowym logowania na adres URL wpisz adres URL przy użyciu następującego wzorca: `https://web.ciscospark.com/#/signin`.

    b. Kliknij przycisk **Dalej**.


7. Na stronie **Konfigurowanie logowania jednokrotnego w iskrowym Cisco** kliknij **pobierania metadanych**, a następnie zapisz plik na komputerze.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Zaloguj się do [Zarządzania współpracy chmury firmy Cisco](https://admin.ciscospark.com/) przy użyciu poświadczeń administratora pełny.
9. Wybierz pozycję **Ustawienia** , a następnie w sekcji **Uwierzytelnianie** , kliknij przycisk **Modyfikuj**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Wybierz pozycję **Integracja dostawcy tożsamości 3rd firm. (Zaawansowane)** i przejdź do następnego ekranu.
11. Polecenie **Pobierz plik metadane** i Zapisz plik na komputerze.
12. Na stronie **Importowanie protokołu Idp metadanych** przeciągnij i upuść plik metadanych Azure AD na stronę lub za pomocą opcji przeglądarki pliku zlokalizuj i przekaż plik metadanych Azure AD. Następnie wybierz opcję **Wymagaj certyfikat podpisany przez urząd certyfikacji w metadanych (większe bezpieczeństwo)** i kliknij przycisk **Dalej**. 

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Wybierz pozycję **Testuj połączenie logowania jednokrotnego**, a gdy zostanie otwarta na nowej karcie przeglądarki, uwierzytelniania z usługą Azure Active Directory, logując się do.
14. Wróć do karty przeglądarki **Zarządzanie współpracy chmury firmy Cisco** . Jeśli wynik testu zakończyło się pomyślnie, zaznacz **test zakończyło się pomyślnie. Opcja logowania jednokrotnego** i kliknij przycisk **Dalej**.

7. W portalu klasyczny Azure AD wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

8. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.  
    
    ![Azure AD rejestracji jednokrotnej][11]

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika test Azure AD
W tej sekcji możesz utworzyć użytkownika testowego w portalu klasyczny o nazwie Simona Britta.

![Utwórz użytkownika Azure AD][20]

**Aby utworzyć użytkownika testowego w Azure AD, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij pozycję **Użytkownicy**.
    
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Aby otworzyć okno dialogowe **Dodaj użytkownika** , na pasku narzędzi u dołu, kliknij pozycję **Dodaj użytkownika**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Na stronie okno **Powiedz nam o tym użytkowniku,** wykonaj następujące czynności:
 
    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    . Typ użytkownika zaznacz nowego użytkownika w Twojej organizacji.

    b. W polu Nazwa użytkownika **pole tekstowe**wpisz **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na stronie okno dialogowe **Profilu użytkownika** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    . W polu tekstowym **Imię** wpisz **Britta**.  

    b. W polu **Nazwisko** tekstowym typu **Simona**.

    c. W polu tekstowym **Nazwa wyświetlana** wpisz **Simona Britta**.

    d. Na liście **ról** wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na stronie **Pobierz hasło tymczasowe** okno kliknij przycisk **Utwórz**.

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Na stronie okno dialogowe **Pobierz hasło tymczasowe** wykonaj następujące czynności:

    ![Tworzenie użytkownika test Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    . Zanotuj wartość **Nowe hasło**.

    b. Kliknij pozycję **pełne**.   



### <a name="creating-a-cisco-spark-test-user"></a>Tworzenie użytkownika test Cisco Spark

W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w iskrowym Cisco. W tej sekcji możesz utworzyć użytkownika o nazwie Simona Britta w iskrowym Cisco.

1. Przejdź do [Zarządzania współpracy chmury firmy Cisco](https://admin.ciscospark.com/) przy użyciu poświadczeń administratora pełny.
2. Kliknij **użytkowników** , a następnie **Zarządzanie użytkownikami**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. W oknie **Zarządzanie użytkownika** wybierz pozycję **ręcznie dodać lub zmodyfikować użytkowników** , a następnie kliknij przycisk **Dalej**.
4. Wybierz pozycję **nazwy i adresy E-mail**. Następnie wypełnij pole tekstowe, jak wykonać:

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    . W polu tekstowym **Imię** wpisz **Britta**

    b. W polu tekstowym **Nazwa** wpisz **Simona**

    c. W polu tekstowym **Adres E-mail** wpisz**britta.simon@contoso.com**

5. Kliknij znak plus, aby dodać Simona Britta. Następnie kliknij przycisk **Dalej**.
6. W oknie **Dodawanie usług dla użytkowników,** kliknij przycisk **Zapisz** , a następnie **Zakończ**.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkowników testowych Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Cisco Spark za pomocą Azure rejestracji jednokrotnej.

![Przypisywanie użytkownika][200] 

**Aby przypisać Simona Britta Cisco Spark, wykonaj następujące czynności:**

1. W portalu klasyczny aby otworzyć widok aplikacji w widoku katalogu, kliknij **aplikacji** w górnym menu.

    ![Przypisywanie użytkownika][201] 

2. Na liście aplikacji zaznacz **Cisco Spark**.

    ![Konfigurowanie logowania jednokrotnego](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. W menu u góry kliknij pozycję **Użytkownicy**.

    ![Przypisywanie użytkownika][203] 

1. Na liście wszystkich użytkowników wybierz pozycję **Simona Britta**.

2. Na pasku narzędzi u dołu kliknij przycisk **Przypisz**.

    ![Przypisywanie użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

Celem w tej sekcji jest test Azure AD pojedynczego logowania jednokrotnego konfiguracji przy użyciu panelu programu Access.

Po kliknięciu kafelka Cisco Spark w panelu programu Access, możesz należy uzyskać automatycznie podpisane w aplikacji Cisco Spark.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
