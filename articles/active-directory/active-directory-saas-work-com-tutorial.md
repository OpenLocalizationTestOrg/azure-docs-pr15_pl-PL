<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Work.com | Microsoft Azure" 
    description="Dowiedz się, jak użyć Work.com z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Samouczek: Azure Active Directory Integracja z Work.com
  
Celem tego samouczka jest pokazanie integracji Azure i Work.com.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Work.com logowania jednokrotnego włączone subskrypcji
  
Ten samouczek użytkowników AAD, do których masz przypisać Work.com dostęp będzie mógł pojedynczy znak do aplikacji w witrynie firmy Work.com (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Work.com
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenariusz")

##<a name="enabling-the-application-integration-for-workcom"></a>Włączanie integracji aplikacji dla Work.com
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Work.com.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Work.com, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Work.com**.

    ![Galeria aplikacji] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Work.com**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Work.com przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury musisz przekazać certyfikatu do Work.com.com.

>[AZURE.NOTE] Aby skonfigurować logowania jednokrotnego, musisz skonfigurować niestandardową nazwę domeny Work.com jeszcze. Należy zdefiniować co najmniej nazwę domeny, przetestuj swoją nazwę domeny i wdrożeniu jej w całej organizacji.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy Work.com jako administrator.

2.  Przejdź do **Instalatora**.

    ![Konfiguracja] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Konfiguracja")

3.  W okienku nawigacji po lewej stronie w sekcji **Administrowanie** kliknij **Domain Management** , aby rozwinąć sekcję pokrewne, a następnie kliknij **Moje domeny** , aby otworzyć stronę **Moje domeny** . 

    ![Mojej domeny] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Mojej domeny")

4.  Aby sprawdzić, czy domena została skonfigurowana w poprawnie, upewnij się, że jest "**Krok 4 wdrożony użytkownikom**" i przejrzyj "**Moje ustawienia domeny**".

    ![Domena jest używany do użytkownika] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Domena jest używany do użytkownika")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do portalu klasyczny sieci Azure.

6.  Na stronie integracji **Work.com **aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Konfigurowanie logowania jednokrotnego")

7.  Na stronie **jak chcesz użytkownikom logowanie do Work.com** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Konfigurowanie logowania jednokrotnego")

8.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Work.com logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Work.com (np.: " *http://company.my.salesforce.com*"), a następnie kliknij przycisk **Dalej**: 

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Skonfigurować adres URL aplikacji")

9.  Na stronie **Konfigurowanie logowania jednokrotnego w Work.com** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Konfigurowanie logowania jednokrotnego")

10. Zaloguj się do Twojej dzierżawy Work.com.

11. Przejdź do **Instalatora**.

    ![Konfiguracja] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Konfiguracja")

12. Rozwinięcie menu **Kontroli zabezpieczeń** , a następnie kliknij **Ustawienia rejestracji jednokrotnej**.

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Ustawienia rejestracji jednokrotnej")

13. Na stronie okno dialogowe **Ustawienia rejestracji jednokrotnej** wykonaj następujące czynności:

    ![Włączone SAML] (./media/active-directory-saas-work-com-tutorial/IC781026.png "Włączone SAML")

    1.  Wybierz pozycję **włączone SAML**.
    2.  Kliknij przycisk **Nowy**.

14. W sekcji **SAML pojedynczego logowania jednokrotnego ustawienia** wykonaj następujące czynności:

    ![SAML pojedynczego logowania jednokrotnego ustawienie] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML pojedynczego logowania jednokrotnego ustawienie")

    1.  W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji.  

        >[AZURE.NOTE] Dostarczanie wartości dla **nazwy** automatycznie wypełnij pole tekstowe **Nazwa interfejsu API** .

    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Work.com** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    3.  Aby przekazać pobranego certyfikatu, kliknij przycisk **Przeglądaj**.
    4.  W polu tekstowym **Identyfikator jednostki** wpisz **https://salesforce-work.com**.
    5.  Jako **Typ tożsamości SAML**wybierz pozycję **potwierdzenia zawiera identyfikator federacji z obiektu użytkownika**.
    6.  Jako **Lokalizacji tożsamości SAML**wybierz **tożsamości w elemencie NameIdentfier instrukcji tematu**.
    7.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Work.com** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości** .
    8.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Work.com** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości** .
    9.  Zaznacz **Wpis HTTP** **Usługi zainicjowane żądanie wiązanie dostawcy**.
    10. Kliknij przycisk **Zapisz**.

15. W portalu klasyczny usługi Work.com, w okienku nawigacji po lewej stronie kliknij pozycję **Domain Management** , aby rozwinąć sekcję pokrewne, a następnie kliknij **Moje domeny** , aby otworzyć stronę **Moje domeny** . 

    ![Mojej domeny] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Mojej domeny")

16. Na stronie **Moje domeny** w sekcji **Logowania strony związane ze znakowaniem,** kliknij przycisk **Edytuj**.

    ![Strona logowania znakowanie] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Strona logowania znakowanie")

17. Na stronie **Logowania strony związane ze znakowaniem,** w sekcji **Usługa uwierzytelniania** nazwa **Ustawienia logowania jednokrotnego SAML** jest wyświetlana. Zaznacz go, a następnie kliknij przycisk **Zapisz**.

    ![Strona logowania znakowanie] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Strona logowania znakowanie")

18. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Dla użytkowników usługi Azure Active Directory można było zalogować się musi być przygotowana do Work.com.  
W przypadku Work.com inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy Work.com jako administrator.

2.  Przejdź do **Instalatora**.

    ![Konfiguracja] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Konfiguracja")

3.  Przejdź do pozycji **Zarządzanie użytkownikami \> użytkowników**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Zarządzanie użytkownikami")

4.  Kliknij pozycję **Nowy użytkownik**.

    ![Wszyscy użytkownicy] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Wszyscy użytkownicy")

5.  W sekcji Edytowanie użytkowników wykonaj następujące czynności:

    ![Edytowanie użytkownika] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Edytowanie użytkownika")

    1.  Wpisz **Nazwisko**, **Alias**, **wiadomości E-mail**, **nazwę użytkownika** i atrybuty **pseudonimu** prawidłowe konta usługi Azure Active Directory, które chcesz należy do powiązanych pól tekstowych.
    2.  Wybierz **rolę**, **licencji użytkownika** i **profilu**.
    3.  Kliknij przycisk **Zapisz**.  

        >[AZURE.NOTE] Właściciel konta usługi Azure Active Directory otrzymają wiadomość e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego Work.com użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Work.com zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Aby przypisać użytkowników do Work.com, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji Work.com aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Tak")
  
Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Work.com.com.
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).