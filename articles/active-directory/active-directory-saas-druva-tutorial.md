<properties 
    pageTitle="Samouczek: Azure Active Directory integracji Integracja z Druva | Microsoft Azure" 
    description="Dowiedz się, jak użyć Druva z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Samouczek: Azure Active Directory integracji Integracja z Druva

Celem tego samouczka jest pokazanie integracji Azure i Druva.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Druva logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Druva będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Druva (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Druva
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-druva-tutorial/IC795084.png "Scenariusz")
##<a name="enabling-the-application-integration-for-druva"></a>Włączanie integracji aplikacji dla Druva

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Druva.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Druva, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-druva-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-druva-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Druva**.

    ![Galeria aplikacji] (./media/active-directory-saas-druva-tutorial/IC795085.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Druva**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Druva przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

Aplikacja Druva oczekuje potwierdzeń SAML w określonym formacie, który należy dodać atrybut niestandardowy mapowania konfiguracji **atrybuty tokenu saml** .  
Następujące zrzut ekranu przedstawia przykładową to.

![Atrybuty tokenu SAML] (./media/active-directory-saas-druva-tutorial/IC795087.png "Atrybuty tokenu SAML")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Druva** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795027.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Druva** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795088.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Druva logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji Druva (np.: "*https://cloud.druva.com/home/*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-druva-tutorial/IC795089.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Druva** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795090.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Druva jako administrator.

6.  Przejdź do pozycji **Zarządzanie \> ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-druva-tutorial/IC795091.png "Ustawienia")

7.  W oknie dialogowym Ustawienia rejestracji jednokrotnej wykonaj następujące czynności:

    ![Ustawienia ojedynczy logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795092.png "Ustawienia ojedynczy logowania jednokrotnego")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Druva** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL identyfikator dostawcy logowania** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Druva** skopiuj wartość **Adres URL usługi zdalnej Wyloguj się** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj dostawcy identyfikator** .
    3.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **Identyfikator dostawcy certyfikatu**
    5.  Aby otworzyć stronę **Ustawienia** , kliknij przycisk **Zapisz**.

8.  Na stronie **Ustawienia** kliknij pozycję **Generuj Token logowania jednokrotnego**.

    ![Ustawienia] (./media/active-directory-saas-druva-tutorial/IC795093.png "Ustawienia")

9.  W oknie dialogowym **Pojedynczego logowania jednokrotnego uwierzytelniania Token** wykonaj następujące czynności:

    ![Token logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795094.png "Token logowania jednokrotnego")

    1.  Kliknij przycisk **Kopiuj**.
    2.  Kliknij przycisk **Zamknij**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-druva-tutorial/IC795095.png "Konfigurowanie logowania jednokrotnego")

11. W menu u góry kliknij **atrybuty** , aby otworzyć okno dialogowe **Atrybuty tokenu SAML** .

    ![Atrybuty] (./media/active-directory-saas-druva-tutorial/IC795096.png "Atrybuty")

12. Aby dodać mapowania wymagany atrybut, wykonaj następujące czynności:

  	|Nazwa atrybutu|Wartość atrybutu|
  	|---|---|
  	|zsynchronizowana\_auth\_tokenu|<*wartość do Schowka*>|

    1.  Dla każdego wiersza danych w powyższej tabeli kliknij przycisk **Dodaj atrybut użytkownika**.
    2.  W polu tekstowym **Nazwa atrybutu** wpisz nazwę atrybutu wyświetlania dla tego wiersza.
    3.  W polu tekstowym **Wartość atrybutu** wpisz wartość atrybutu wyświetlania dla tego wiersza.
    4.  Kliknij pozycję **pełne**.

13. Kliknij przycisk **Zastosuj zmiany**.
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do Druva, musi być przygotowana do Druva.  
W przypadku Druva inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Druva** jako administrator.

2.  Przejdź do pozycji **Zarządzanie \> użytkowników**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-druva-tutorial/IC795097.png "Zarządzanie użytkownikami")

3.  Kliknij pozycję **Utwórz nowe**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-druva-tutorial/IC795098.png "Zarządzanie użytkownikami")

4.  W oknie dialogowym Tworzenie nowego użytkownika wykonaj następujące czynności:

    ![Tworzenie Nowy_użytkownik] (./media/active-directory-saas-druva-tutorial/IC795099.png "Tworzenie Nowy_użytkownik")

    1.  Wpisz adres e-mail i nazwę ważne konto użytkownika usługi Azure Active Directory chcesz dodawać do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Utwórz użytkownika**.

>[AZURE.NOTE] Można użyć dowolnego innego Druva użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Druva zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Aby przypisać użytkowników do Druva, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Druva **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-druva-tutorial/IC795100.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-druva-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
