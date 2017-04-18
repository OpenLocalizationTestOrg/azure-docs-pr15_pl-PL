<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Jobscience | Microsoft Azure" 
    description="Dowiedz się, jak użyć Jobscience z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Samouczek: Azure Active Directory Integracja z Jobscience
  
Celem tego samouczka jest pokazanie integracji Azure i Jobscience.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Subskrypcja Jobscience logowania jednokrotnego włączone
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Jobscience będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Jobscience (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Jobscience
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenariusz")
##<a name="enabling-the-application-integration-for-jobscience"></a>Włączanie integracji aplikacji dla Jobscience
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Jobscience.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Jobscience, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **jobscience**.

    ![Galeria aplikacji] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Jobscience**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Jobscience przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla Jobscience wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy Jobscience jako administrator.

2.  Przejdź do **Instalatora**.

    ![Konfiguracja] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Konfiguracja")

3.  W okienku nawigacji po lewej stronie w sekcji **Administrowanie** kliknij **Domain Management** , aby rozwinąć sekcję pokrewne, a następnie kliknij **Moje domeny** , aby otworzyć stronę **Moje domeny** . 

    ![Mojej domeny] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mojej domeny")

4.  Aby sprawdzić, czy domena została skonfigurowana w poprawnie, upewnij się, że jest "**Krok 4 wdrożony użytkownikom**" i przejrzyj "**Moje ustawienia domeny**".

    ![Domena jest używany do użytkownika] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Domena jest używany do użytkownika")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do portalu klasyczny sieci Azure.

6.  Na stronie integracji **Jobscience** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Konfigurowanie logowania jednokrotnego")

7.  Na stronie **jak chcesz użytkownikom logowanie do Jobscience** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Konfigurowanie logowania jednokrotnego")

8.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Jobscience adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*http://company.my.salesforce.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Skonfigurować adres URL aplikacji")

9.  Na stronie **Konfigurowanie logowania jednokrotnego w Jobscience** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Konfigurowanie logowania jednokrotnego")

10. W witrynie firmy Jobscience kliknij **Kontroli zabezpieczeń**, a następnie kliknij pozycję **Ustawienia rejestracji jednokrotnej**.

    ![Kontroli bezpieczeństwa] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Kontroli bezpieczeństwa")

11. W sekcji **Ustawienia rejestracji jednokrotnej** wykonaj następujące czynności:

    ![Ustawienia rejestracji jednokrotnej] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Ustawienia rejestracji jednokrotnej")

    1.  Wybierz pozycję **włączone SAML**.
    2.  Kliknij przycisk **Nowy**.

12. W oknie dialogowym **SAML pojedynczego logowania jednokrotnego ustawienie edytowanie** wykonaj następujące czynności:

    ![SAML pojedynczego logowania jednokrotnego ustawienie] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML pojedynczego logowania jednokrotnego ustawienie")

    1.  W polu tekstowym **Nazwa** wpisz nazwę swojej konfiguracji.
    2.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w Jobscience** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy**
    3.  W polu tekstowym **Identyfikator jednostki** wpisz **https://salesforce-jobscience.com**
    4.  Kliknij przycisk **Przeglądaj,** Aby przekazać certyfikatu Azure AD.
    5.  Jako **Typ tożsamości SAML**wybierz pozycję **potwierdzenia zawiera identyfikator federacji z obiektu użytkownika**.
    6.  Jako **Lokalizacji tożsamości SAML**wybierz **tożsamości w elemencie NameIdentfier instrukcji tematu**.
    7.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w Jobscience** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania dostawcy tożsamości**
    8.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w Jobscience** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy tożsamości**
    9.  Kliknij przycisk **Zapisz**.

13. W okienku nawigacji po lewej stronie w sekcji **Administrowanie** kliknij **Domain Management** , aby rozwinąć sekcję pokrewne, a następnie kliknij **Moje domeny** , aby otworzyć stronę **Moje domeny** . 

    ![Mojej domeny] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mojej domeny")

14. Na stronie **Moje domeny** w sekcji **Logowania strony związane ze znakowaniem,** kliknij przycisk **Edytuj**.

    ![Strona logowania znakowanie] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Strona logowania znakowanie")

15. Na stronie **Logowania strony związane ze znakowaniem,** w sekcji **Usługa uwierzytelniania** nazwa **Ustawienia logowania jednokrotnego SAML** jest wyświetlana. Zaznacz go, a następnie kliknij przycisk **Zapisz**.

    ![Strona logowania znakowanie] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Strona logowania znakowanie")

16. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Konfigurowanie logowania jednokrotnego")
  
Aby uzyskać PS inicjowane rejestracji jednokrotnej na adres URL logowania kliknij na stronie **Ustawienia rejestracji jednokrotnej** w sekcji menu **Kontroli zabezpieczeń** .

![Kontroli bezpieczeństwa] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Kontroli bezpieczeństwa")
  
Kliknij profil logowania jednokrotnego, który został utworzony w poprzednim kroku.  
Ta strona zawiera rejestracji jednokrotnej na adres URL dla swojej firmy (np. *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Jobscience, musi być przygotowana do Jobscience.  
W przypadku Jobscience inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Jobscience** jako administrator.

2.  Przejdź do Instalatora

    ![Konfiguracja] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Konfiguracja")

3.  Przejdź do pozycji **Zarządzanie użytkownikami \> użytkowników**.

    ![Użytkownicy] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Użytkownicy")

4.  Kliknij pozycję **Nowy użytkownik**.

    ![Wszyscy użytkownicy] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Wszyscy użytkownicy")

5.  W oknie dialogowym **Edytowanie użytkowników** wykonaj następujące czynności:

    ![Edytowanie użytkownika] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Edytowanie użytkownika")

    1.  Wpisz imię, nazwisko, alias, wiadomości e-mail, nazwa i pseudonimu właściwości użytkownika użytkownika Azure AD, którego chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

    >[AZURE.NOTE] Właściciel konta Azure AD otrzymają wiadomość e-mail, która zawiera łącza do potwierdzenia konto zostało aktywowane.

>[AZURE.NOTE] Można użyć dowolnego innego Jobscience użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Jobscience zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Aby przypisać użytkowników do Jobscience, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Jobscience **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).