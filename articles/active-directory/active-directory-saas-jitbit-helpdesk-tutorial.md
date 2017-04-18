<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z pomocą techniczną Jitbit | Microsoft Azure" 
    description="Dowiedz się, jak użyć Jitbit działu pomocy technicznej z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Samouczek: Azure Active Directory Integracja z pomocą techniczną Jitbit
  
Celem tego samouczka jest pokazanie integracji Azure i Jitbit działu pomocy technicznej.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Jitbit działu pomocy technicznej
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do działu pomocy technicznej Jitbit będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny pomocy Jitbit (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla działu pomocy technicznej Jitbit
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenariusz")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Włączanie integracji aplikacji dla działu pomocy technicznej Jitbit
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla działu pomocy technicznej Jitbit.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla działu pomocy technicznej Jitbit, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Jitbit działu pomocy technicznej**.

    ![Galeria aplikacji] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Jitbit działu pomocy technicznej**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania zgłoszeń Jitbit przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML. .  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Aby móc skonfigurować rejestracji jednokrotnej na Twojej dzierżawy Jitbit działu pomocy technicznej, należy skontaktować się najpierw techniczną Jitbit działu pomocy technicznej, aby uzyskać ta funkcja jest włączona.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Zgłoszeń Jitbit** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie z Jitbit** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Jitbit zgłoszeń adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*https://\<dzierżawy nazwę\>. Jitbit.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Jitbit działu pomocy technicznej** , aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie jako **c:\\Jitbit Helpdesk.cer**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy zgłoszeń Jitbit jako administrator.

6.  Na pasku narzędzi u góry kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administracja")

7.  Kliknij przycisk **Ustawienia ogólne**.

    ![Użytkownicy, firmy i uprawnienia] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Użytkownicy, firmy i uprawnienia")

8.  W sekcji Konfiguracja **Ustawienia uwierzytelniania** wykonaj następujące czynności:

    ![Ustawienia uwierzytelniania] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Ustawienia uwierzytelniania")

    1.  Wybierz pozycję **Włącz SAML 2.0 pojedynczy logowanie** logowania przy użyciu jednego logowania jednokrotnego (SSO) z **OneLogin**.
    2.  W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w pomocy Jitbit** skopiuj wartość **punktu końcowego inicjowanych przez dostawcę usługi (SP)** , a następnie wklej go w polu tekstowym **Adres URL punktu końcowego** .
    3.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otwórz certyfikatu zakodowany base-64, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **Certyfikat X.509**
    5.  Kliknij przycisk **Zapisz zmiany**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do działu pomocy technicznej Jitbit, musi być przygotowana do działu pomocy technicznej Jitbit.  
W przypadku zgłoszeń Jitbit inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Jitbit działu pomocy technicznej** .

2.  W menu u góry kliknij pozycję **Administracja**.

    ![Administracja] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administracja")

3.  Kliknij przycisk **uprawnienia, firm i użytkowników**.

    ![Użytkownicy, firmach i uprawnienia] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Użytkownicy, firmach i uprawnienia")

4.  Kliknij pozycję **Dodaj użytkownika**.

    ![Dodaj użytkownika] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Dodaj użytkownika")

5.  W sekcji Tworzenie wpisz dane konto Azure AD do świadczenia w następujących polach tekstowych: **Nazwa użytkownika**, **wiadomości E-mail**, **Imię**, **Nazwisko**

    ![Tworzenie] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Tworzenie")

6.  Kliknij przycisk **Utwórz**.

>[AZURE.NOTE] Można użyć dowolnego innego zgłoszeń Jitbit użytkownika konta narzędzia do tworzenia lub interfejsy API dostarczane przez zgłoszeń Jitbit zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Aby przypisać użytkowników do działu pomocy technicznej Jitbit, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **Działu pomocy technicznej Jitbit **aplikacji integracji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).