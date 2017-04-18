<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z konsoli administracyjnej Mimecast | Microsoft Azure" 
    description="Dowiedz się, jak użyć konsoli administracyjnej Mimecast z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Samouczek: Integracja usługi Azure Active Directory z konsoli administracyjnej Mimecast
  
Celem tego samouczka jest pokazanie integracji Azure i Konsola administracyjna Mimecast.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Konsola administracyjna Mimecast logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do konsoli administracyjnej Mimecast będą mogli rejestracji jednokrotnej do aplikacji w konsoli administracyjnej Mimecast firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji do konsoli administracyjnej Mimecast
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenariusz")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Włączanie integracji aplikacji do konsoli administracyjnej Mimecast
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Mimecast konsoli administracyjnej.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla konsoli administracyjnej Mimecast, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Mimecast konsoli administracyjnej**.

    ![Galeria aplikacji] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Konsola administracyjna Mimecast**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Konsola administracyjna Mimecast] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Konsola administracyjna Mimecast")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML Konsola administracyjna Mimecast przy użyciu swojego konta.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Konsola administracyjna Mimecast** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie się do konsoli administracyjnej Mimecast** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Mimecast Administrator konsoli logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji konsoli administracyjnej Mimecast (np.: "https://webmail-uk.mimecast.com" lub "https://webmail-us.mimecast.com"), a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Znak na adres URL to region określonym.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w konsoli administracyjnej Mimecast** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do konsoli administracyjnej usługi Mimecast jako administrator.

6.  Przejdź do pozycji **usług \> aplikacji**.

    ![Usług] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Usług")

7.  Kliknij pozycję **Profile uwierzytelniania**.

    ![Profile uwierzytelniania] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Profile uwierzytelniania")

8.  Kliknij przycisk **Nowy profil uwierzytelniania**.

    ![Nowe profile uwierzytelniania] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nowe profile uwierzytelniania")

9.  W sekcji **Uwierzytelnianie profilu** wykonaj następujące czynności:

    ![Uwierzytelnianie profilu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Uwierzytelnianie profilu")

    1.  W polu tekstowym **Opis** wpisz nazwę swojej konfiguracji.
    2.  Wybierz pozycję **Wymuszaj SAML uwierzytelniania dla konsoli administracyjnej Mimecast**.
    3.  **Dostawca**wybierz **Usługi Azure Active Directory**.
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w konsoli administracyjnej Mimecast** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Adres URL wystawcy** .
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w konsoli administracyjnej Mimecast** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    6.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w konsoli administracyjnej Mimecast** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .  

        >[AZURE.NOTE]Wartość adres URL logowania wartość adresu URL Wyloguj się do konsoli administracyjnej Mimecast tak samo.

    7.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

    8.  Otwieranie base-64 certyfikatu zakodowany w Notatniku usuwanie pierwszego wiersza ("*--*") i ostatni wiersz ("*--*"), skopiuj pozostałej zawartości do Schowka, a następnie wkleić go w polu tekstowym **Certyfikatu dostawcy tożsamości (metadane)** .
    9.  Wybierz opcję **Zezwalaj na rejestracji jednokrotnej**.
    10. Kliknij przycisk **Zapisz**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do konsoli administracyjnej Mimecast, musi być przygotowana do konsoli administracyjnej Mimecast.  
W przypadku konsoli administracyjnej Mimecast inicjowania obsługi administracyjnej to zadanie ręczne.
  
Musisz zarejestrować domenę, aby można było tworzyć użytkowników.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do **Konsoli administracyjnej Mimecast** jako administrator.

2.  Przejdź do pozycji **katalogów \> wewnętrzny**.

    ![Katalogi] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Katalogi")

3.  Kliknij przycisk **Zarejestruj nową domenę**.

    ![Zarejestruj się w nowej domeny] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Zarejestruj się w nowej domeny")

4.  Po utworzeniu nowej domeny kliknij pozycję **Nowy adres**.

    ![Nowy adres] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Nowy adres")

5.  W oknie dialogowym Nowy adres wykonaj następujące czynności:

    ![Zapisywanie] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Zapisywanie")

    1.  Wpisz **Adres E-mail**, **Nazwę globalnej**, **hasło** i **Potwierdź hasło** atrybuty prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE]Za pomocą inne narzędzia do tworzenia konta użytkownika konsoli administracyjnej Mimecast lub interfejsów API dostarczane przez Konsola administracyjna Mimecast zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Aby przypisać użytkowników do konsoli administracyjnej Mimecast, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **Konsola administracyjna Mimecast **aplikacji integracji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).