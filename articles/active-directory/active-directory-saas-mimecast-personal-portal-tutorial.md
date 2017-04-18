<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z portalu osobiste Mimecast | Microsoft Azure" 
    description="Dowiedz się, jak użyć portalu osobistego Mimecast z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Samouczek: Azure Active Directory Integracja z portalu Mimecast osobiste
  
Celem tego samouczka jest pokazanie integracji Azure i Mimecast portalu osobistych.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Portal osobistych Mimecast logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do portalu osobistego Mimecast będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny portalu osobistego Mimecast (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji portalu osobistego Mimecast
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Scenariusz")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Włączanie integracji aplikacji portalu osobistego Mimecast
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji portalu osobistego Mimecast.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Aby włączyć integracji aplikacji portalu osobistego Mimecast, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Mimecast osobistych portalu**.

    ![Galeria aplikacji] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Portal osobistych Mimecast**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Portal Mimecast osobiste] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Portal Mimecast osobiste")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do portalu osobistego Mimecast przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Portalu osobistego Mimecast** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do portalu osobistego Mimecast** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Mimecast osobistych portalu logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji portalu osobistego Mimecast (np.: "https://webmail-uk.mimecast.com" lub "https://webmail-us.mimecast.com"), a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Znak na adres URL to region określonym.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w portalu osobistego Mimecast** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do portalu osobistego sieci Mimecast jako administrator.

6.  Przejdź do pozycji **usług \> aplikacji**.

    ![Aplikacje] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Aplikacje")

7.  Kliknij pozycję **Profile uwierzytelniania**.

    ![Profile uwierzytelniania] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Profile uwierzytelniania")

8.  Kliknij przycisk **Nowy profil uwierzytelniania**.

    ![Nowy Profil uwierzytelniania] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Nowy Profil uwierzytelniania")

9.  W sekcji **Uwierzytelnianie profilu** wykonaj następujące czynności:

    ![Profil uwierzytelniania] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Profil uwierzytelniania")

    1.  W polu tekstowym **Opis** wpisz nazwę swojej konfiguracji.
    2.  Wybierz pozycję **Wymuszaj SAML uwierzytelniania dla portalu Mimecast osobiste**.
    3.  **Dostawca**wybierz **Usługi Azure Active Directory**.
    4.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w portalu osobistego Mimecast** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **Adres URL wystawcy** .
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w portalu osobistego Mimecast** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL logowania** .
    6.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w portalu osobistego Mimecast** skopiuj wartość **Adres URL logowania zdalnego** , a następnie wklej go w polu tekstowym **Adres URL Wyloguj** .  

        >[AZURE.NOTE] Wartość adres URL logowania i wartość adres URL Wyloguj są w w portalu osobistego Mimecast takie same.

    7.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP]Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

    8.  Otwieranie base-64 certyfikatu zakodowany w Notatniku usuwanie pierwszego wiersza ("*--*") i ostatni wiersz ("*--*"), skopiuj pozostałej zawartości do Schowka, a następnie wkleić go w polu tekstowym **Certyfikatu dostawcy tożsamości (metadane)** .
    9.  Wybierz opcję **Zezwalaj na rejestracji jednokrotnej**.
    10. Kliknij przycisk **Zapisz**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do portalu osobistego Mimecast, musi być przygotowana do portalu osobistego Mimecast.  
W przypadku Mimecast osobistych portalu inicjowania obsługi administracyjnej to zadanie ręczne.
  
Musisz zarejestrować domenę, aby można było tworzyć użytkowników.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do sieci **Osobistej portalu Mimecast** jako administrator.

2.  Przejdź do pozycji **katalogów \> wewnętrzny**.

    ![Katalogi] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Katalogi")

3.  Kliknij przycisk **Zarejestruj nową domenę**.

    ![Zarejestruj się w nowej domeny] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Zarejestruj się w nowej domeny")

4.  Po utworzeniu nowej domeny kliknij pozycję **Nowy adres**.

    ![Nowy adres] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Nowy adres")

5.  W oknie dialogowym Nowy adres wykonaj następujące czynności:

    ![Zapisywanie] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Zapisywanie")

    1.  Wpisz **Adres E-mail**, **Nazwa globalnego**, **hasło** i **Potwierdź hasło** atrybuty prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE]Za pomocą innych portalu osobistego Mimecast narzędzia do tworzenia konta użytkownika lub interfejsów API dostarczony przez Portal osobistych Mimecast do kont użytkownika AAD.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Aby przypisać użytkowników do portalu osobistego Mimecast, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **Portalu osobistego Mimecast **aplikacji integracji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).