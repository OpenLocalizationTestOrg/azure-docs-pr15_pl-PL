<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Intacct | Microsoft Azure" 
    description="Dowiedz się, jak użyć Intacct z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Samouczek: Azure Active Directory Integracja z Intacct
  
Celem tego samouczka jest pokazanie integracja Azure i Intacct.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy Intacct
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Intacct będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Intacct (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Intacct
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenariusz")
##<a name="enabling-the-application-integration-for-intacct"></a>Włączanie integracji aplikacji dla Intacct
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Intacct, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Intacct**.

    ![Galeria aplikacji] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Intacct**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Intacct przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Intacct** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Intacct** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Intacct logowania na adres URL** wpisz adres URL przy użyciu następującego wzorca "*https://Intacct.com/company*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Intacct** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Intacct jako administrator.

6.  Kliknij kartę **firmy** , a następnie kliknij pozycję **Informacje o firmie**.

    ![Firma] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Firma")

7.  Kliknij kartę **Zabezpieczenia** , a następnie kliknij przycisk **Edytuj**.

    ![Zabezpieczenia] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Zabezpieczenia")

8.  W sekcji **logowaniu jednokrotnym (SSO)** wykonaj następujące czynności:

    ![Logowaniu jednokrotnym] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Logowaniu jednokrotnym")

    1.  Zaznacz opcję **Włącz rejestracji jednokrotnej na**.
    2.  Zaznacz **Typ dostawcy tożsamości** **SAML 2.0**.
    3.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Intacct** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Adres URL wystawcy** .
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Intacct** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    5.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.
        
        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **certyfikatu**
    7.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Intacct, musi być przygotowana do Intacct.  
W przypadku Intacct inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Intacct** .

2.  Kliknij kartę **firmy** , a następnie kliknij pozycję **Użytkownicy**.

    ![Użytkownicy] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Użytkownicy")

3.  Kliknij kartę **Dodaj**

    ![Dodawanie] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Dodawanie")

4.  W sekcji **Informacje o użytkowniku** wykonaj następujące czynności:

    ![Informacje o użytkowniku] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Informacje o użytkowniku")

    1.  Wpisz **Identyfikator użytkownika**, **nazwisko**, **Imię**, **Adres E-mail**, **Tytuł** i **telefonu** Azure AD konta, które ma zostać należy do powiązanych pól tekstowych.
    2.  Wybierz **uprawnienia administratora** Azure AD konta, które ma zostać świadczenia.
    3.  Kliknij przycisk **Zapisz**.
        
        >[AZURE.NOTE] Właściciel konta AAD otrzymasz wiadomość e-mail i skorzystać z łącza, aby potwierdzić swoje konto, zanim stanie się aktywny.

>[AZURE.NOTE] Można użyć dowolnego innego Intacct użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Intacct zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Aby przypisać użytkowników do Intacct, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Intacct **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).