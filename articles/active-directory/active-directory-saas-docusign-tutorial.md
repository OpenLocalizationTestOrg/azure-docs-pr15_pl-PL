<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z DocuSign | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i DocuSign."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Samouczek: Azure Active Directory Integracja z DocuSign

Celem tego samouczka jest pokazanie integracji Azure i DocuSign.
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

- Ważna subskrypcja Azure
- Dzierżawy w sekcji DocuSign



Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. [Włączanie integracji aplikacji dla DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Konfigurowanie logowania jednokrotnego](#configuring-single-sign-on) 


3. [Konfigurowanie konta inicjowania obsługi administracyjnej.](#configuring-account-provisioning) 


4. [Przypisywanie użytkowników](#assigning-users) 

    ![Konfigurowanie logowania jednokrotnego][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Włączanie integracji aplikacji dla DocuSign

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla DocuSign.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla DocuSign, wykonaj następujące czynności:

1. W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Konfigurowanie logowania jednokrotnego][1]

2. Na liście katalogu zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Konfigurowanie logowania jednokrotnego][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. Na stronie co chcesz zrobić okno dialogowe, kliknij przycisk **Dodaj aplikację z galerii**.

    ![Konfigurowanie logowania jednokrotnego][4]


6. W polu wyszukiwania wpisz **DocuSign**.

    ![Konfigurowanie logowania jednokrotnego][5]

7. W okienku wyników wybierz **DocuSign**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Konfigurowanie logowania jednokrotnego][6]


## <a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania DocuSign przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1. W portalu klasyczny Azure, na stronie **DocuSign integracji aplikacji** kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe Konfigurowanie rejestracji jednokrotnej.

    ![Konfigurowanie logowania jednokrotnego][7]

2. Na stronie **jak chcesz użytkownikom logowanie do DocuSign** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk Dalej.

    ![Konfigurowanie logowania jednokrotnego][8]

3. Na stronie **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego][61]

    . W polu tekstowym **zalogować na adres URL** wpisz `https://account.docusign.com/*`.  

    b. W polu tekstowym **identyfikator** wpisz `https://account.docusign.com/*`.  
   
    c. Kliknij przycisk **Dalej**. 


    > [AZURE.TIP] Zaloguj się na adres URL i wartości identyfikatorów są tylko symbole zastępcze. Instrukcje dotyczące pobierania wartości rzeczywiste w środowisku usługi zostały omówione w dalszej części tego tematu.
 

4. Na stronie **Konfigurowanie logowania jednokrotnego w DocuSign** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego][10]


5. W oknie przeglądarki innej witryny sieci web Zaloguj się do sieci **portalu administracyjnego DocuSign** jako administrator.


6. W menu nawigacji po lewej stronie kliknij pozycję **domeny**.

    ![Konfigurowanie logowania jednokrotnego][51]

7. W okienku po prawej stronie kliknij **Domenę roszczeń**.

    ![Konfigurowanie logowania jednokrotnego][52]

8. W oknie dialogowym **roszczeń domeny** , w polu tekstowym **Nazwa domeny** wpisz domenę Twojej firmy, a następnie kliknij **roszczeń**. Upewnij się, że weryfikowanie domeny i stanu jest aktywna.

    ![Konfigurowanie logowania jednokrotnego][53]

9. W menu po lewej stronie kliknij pozycję **Dostawcy tożsamości**  

    ![Konfigurowanie logowania jednokrotnego][54]

10. W okienku po prawej stronie kliknij pozycję **Dodaj dostawcy tożsamości**. 
    
    ![Konfigurowanie logowania jednokrotnego][55]

11. Na stronie **Ustawienia dostawcy tożsamości** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego][56]


    . W polu tekstowym **Nazwa** wpisz unikatową nazwę swojej konfiguracji. Nie używaj spacji.

    b. W portalu klasyczny Azure skopiuj adres URL wystawcy, a następnie wklej go w polu tekstowym **Wystawcy dostawcy tożsamości** .

    c. W portalu klasyczny Azure Skopiuj **Adres URL logowania zdalnego**, a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .

    d. W portalu klasyczny Azure Skopiuj **Adres URL usługi zdalnej Wyloguj się**, a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości** .

    e. Wybierz **Znak poziomach żądanie**.

    f. **Wysyłanie poziomach wniosek**zaznacz **WPIS**.

    g. **Wyślij żądanie Wyloguj**zaznacz **WPIS**. 


12. W sekcji **Mapowania atrybut niestandardowy** wybierz pole, które chcesz zamapować z Azure AD roszczeń. W tym przykładzie roszczeń **emailaddress** są mapowane na wartość **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. To jest domyślna nazwa roszczeń z Azure AD dla roszczeń poczty e-mail. 

    > [AZURE.NOTE] Aby zamapować użytkownika z usługi Azure Active Directory do mapowania użytkowników Docusign za pomocą odpowiedni **identyfikator użytkownika** . Zaznacz odpowiednie pole i wprowadź odpowiednią wartość na podstawie ustawień organizacji.

    ![Konfigurowanie logowania jednokrotnego][57]

13. W sekcji **Certyfikatu dostawcy tożsamości** kliknij pozycję **Dodaj certyfikat**, a następnie przekaż certyfikat, który został pobrany z klasyczny portal Azure AD.   

    ![Konfigurowanie logowania jednokrotnego][58]

14. Kliknij przycisk **Zapisz**.

15. W sekcji **Dostawcy tożsamości** kliknij **menu Akcje**, a następnie kliknij **punktów końcowych**.   

    ![Konfigurowanie logowania jednokrotnego][59]



10. W portalu klasyczny Azure wróć na stronę **Konfigurowanie ustawień aplikacji** . 

16. W **portalu administracyjnym DocuSign**w sekcji **Widok SAML 2.0 w punkty końcowe** wykonać, następujące czynności:

    ![Konfigurowanie logowania jednokrotnego][60]

    . Skopiuj **Adres URL wystawcy dostawcy usługi**, a następnie wklej go w polu tekstowym **identyfikator** w portalu klasyczny Azure.

    b. Skopiuj **Adres URL logowania dostawcy usługi**, a następnie wklej w polu tekstowym **Logowania na adres URL** portalu klasyczny Azure.

    c.  Kliknij przycisk **Zamknij**  


10. W portalu klasyczny Azure kliknij przycisk **Dalej**. 


15. W portalu klasyczny Azure wybierz **konfiguracji rejestracji jednokrotnej potwierdzenia**, a następnie kliknij **Dalej**.

    ![Aplikacje][14]

10. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.

    ![Aplikacje][15]
 

## <a name="configuring-account-provisioning"></a>Konfigurowanie konta inicjowania obsługi administracyjnej.

Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej konta użytkowników usługi Active Directory w celu DocuSign użytkownika.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1. W **portalu klasyczny Azure**na stronie **DocuSign integracji aplikacji** kliknij pozycję **Konfiguruj konta inicjowania obsługi administracyjnej** , aby otworzyć okno dialogowe Konfigurowanie przypisywanie użytkowników.

    ![Konfigurowanie konta inicjowania obsługi administracyjnej.][30]

2. Na stronie **Ustawienia i poświadczenia administratora** Aby włączyć automatyczne użytkownika inicjowania obsługi administracyjnej, poświadczenia konta DocuSign z odpowiednich uprawnień, a następnie kliknij **Dalej**. 

    ![Konfigurowanie konta inicjowania obsługi administracyjnej.][31]

3. W oknie dialogowym **Połączenie testowe** kliknij przycisk **Rozpocznij test**, a po pomyślnym teście, kliknij przycisk **Dalej**.

    ![Konfigurowanie konta inicjowania obsługi administracyjnej.][32]

3. Na stronie **potwierdzenia** kliknij przycisk **Zakończ**.

    ![Konfigurowanie konta inicjowania obsługi administracyjnej.][33]
 

## <a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Aby przypisać użytkowników do DocuSign, należy wykonać następujące czynności:

1. W **portalu klasyczny Azure**Utwórz konto test.

2. Na stronie **integracji aplikacji DocuSign** kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników][40]
 

3. Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Przypisywanie użytkowników][41]


Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu DocuSign.

Pierwszym krokiem weryfikacji możesz sprawdzić stan obsługi administracyjnej, klikając pozycję pulpitu nawigacyjnego w D DocuSign aplikacji integracji na stronie portalu klasyczny Azure.

![Przypisywanie użytkowników][42]

Ukończony użytkownika inicjowania obsługi administracyjnej cyklu jest wskazywana przez powiązanych stanu:

![Przypisywanie użytkowników][43]


Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu.

Aby uzyskać więcej informacji na temat Panel dostępu Zobacz wprowadzenie do Panel dostępu.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png