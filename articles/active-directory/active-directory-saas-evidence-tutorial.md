<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z Evidence.com | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Evidence.com."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Samouczek: Azure Active Directory Integracja z Evidence.com

Celem tego samouczka jest pokazująca, jak skonfigurować rejestracji jednokrotnej między Azure Active Directory (AAD) i Evidence.com. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:
    
* Ważna Subskrypcja Microsoft Azure
* Włączone subskrypcji Evidence.com przy logowaniu jednokrotnym (e-mail earlyaccess@evidence.com Jeśli opartego na protokole SAML logowaniu jednokrotnym nie jest włączona)

Ten samouczek użytkowników AAD, do których przypisano dostęp Evidence.com będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu panelu AAD programu Access.

## <a name="add-evidencecom-to-your-directory"></a>Dodawanie Evidence.com do katalogu

W tej sekcji przedstawiono sposób dodawania Evidence.com jako zintegrowanej aplikacji w usłudze Azure Active Directory.

**Aby włączyć integrację aplikacji dowodów:**

1.  W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

4.  Aby otworzyć galerię aplikacji, kliknij przycisk **Dodaj**, a następnie kliknij **Dodawanie aplikacji z galerii**.

5.  W polu wyszukiwania wpisz **Evidence.com**.

6.  W okienku wyników wybierz **Evidence.com**, a następnie kliknij **Wykonano** w celu dodania aplikacji.


## <a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

W tej sekcji omówiono, jak umożliwić użytkownikom uwierzytelniania Evidence.com przy użyciu swojego konta w Azure Active Directory, używanie federacyjnych na podstawie protokołu SAML.

**Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:**

1.  Po dodaniu Evidence.com w portalu klasyczny Azure, kliknij pozycję **Konfigurowanie logowania jednokrotnego**. 
 
2.  Na następnym ekranie Wybierz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

3.  Na ekranie Konfigurowanie adresu URL aplikacji, wprowadź adres URL miejsce, w którym użytkownicy będą zalogować się do adresu URL dzierżawy Evidence.com (przykład: https://yourtenant.evidence.com, a następnie kliknij przycisk **Dalej**. 

4.  Kliknij łącze **Pobierz certyfikat** , a następnie zapisz go na dysk lokalny. Ten certyfikat i adresy URL metadanych (identyfikator jednostki, logowania jednokrotnego adres URL logowania i zaloguj się adres URL) będą używane do Konfigurowanie logowania jednokrotnego w witrynie Evidence.com. 

5.  W oknie przeglądarki sieci web osobnych, zaloguj się do swojego Evidence.com dzierżawa jako administrator, a następnie przejdź do karty **administratora**
      
6.  Polecenie **logowaniu jednokrotnym agencji**
 
7.  Wybierz pozycję **SAML na podstawie rejestracji jednokrotnej**
 
8.  Skopiuj adres **URL wystawcy** **Rejestracji jednokrotnej** i **Pojedynczy Wyloguj** wartości wyświetlane w portalu klasyczny Azure, a do odpowiednich pól w Evidence.com.

9.  Otwieranie certyfikat pobrany w kroku 4 przy użyciu edytora tekstu, na przykład Notepad.exe, kopiowanie i wklejanie zawartości w polu **Certyfikat zabezpieczeń** . 

10. Zapisywanie konfiguracji w Evidence.com.
 
11. W portalu klasyczny Azure Sprawdź **Potwierdź, który został skonfigurowany logowania jednokrotnego w sposób opisany powyżej**. Sprawdzanie to umożliwi bieżącego certyfikatu rozpocząć pracę dla tego pola wyboru aplikacji.
 
12. Na stronie potwierdzenia rejestracji jednokrotnej kliknij przycisk **Zakończ**.  


## <a name="creating-an-evidencecom-test-user"></a>Tworzenie użytkownika test Evidence.com

Azure AD użytkownikom będzie można się zalogować musi być przygotowana dostępu w aplikacji Evidence.com. W tej sekcji opisano, jak utworzyć konta użytkowników Azure AD wewnątrz Evidence.com

**Inicjowanie obsługi konta użytkownika w Evidence.com:**

1.  W oknie przeglądarki sieci web Zaloguj się do witryny firmy Evidence.com jako administrator.

2.  Przejdź do karty **administratora** .

3.  Wybierz polecenie **Dodaj użytkownika**.

4.  Kliknij przycisk **Dodaj** .

5.  **Adres E-mail** dodano użytkownika musi odpowiadać nazwa_użytkownika użytkowników w Azure AD, który chcesz udzielić dostępu. Jeśli nazwa użytkownika i adres e-mail nie są tej samej wartości w Twojej organizacji, możesz użyć **Evidence.com > atrybuty > logowania jednokrotnego** sekcji Azure portal klasycznego, aby zmienić nameidenitifer wysyłane do Evidence.com być adres e-mail.


## <a name="assigning-users-to-evidencecom"></a>Przypisywanie użytkowników do Evidence.com

Ustanawianie AAD użytkownicy będą mogli zobaczyć Evidence.com na ich Panel dostępu one przypisane dostępu w portalu klasyczny Azure.

**Aby przypisać użytkowników do Evidence.com:**

1.  Na stronie szybki start dla Evidence.com w portalu klasyczny Azure kliknij polecenie **Przypisz użytkownikom Evidence.com**.
 
2.  W menu **Pokaż** Określ, czy chcesz przydzielić Evidence.com użytkownika lub grupy, a następnie kliknij przycisk znacznika wyboru.
 
3.  Na liście **Użytkownicy** Wybierz użytkowników do grupy, do którego chcesz przypisać Evidence.com.
 
4.  W stopce strony kliknij przycisk **Przypisz** .

