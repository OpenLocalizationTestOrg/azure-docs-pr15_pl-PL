<properties
    pageTitle="Azure Active Directory B2C: Narzędzia Pomocnik dostosowywania strony interfejsu użytkownika | Microsoft Azure"
    description="Narzędzie Pomocnik używane w celu zademonstrowania funkcji dostosowanie strony interfejsu użytkownika w Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Pomocnik narzędzie do celu zademonstrowania funkcji dostosowania interfejsu użytkownika strony

Ten artykuł dotyczy companion [główny artykuł dostosowania interfejsu użytkownika](active-directory-b2c-reference-ui-customization.md) w B2C usługi Azure Active Directory (Azure AD). W poniższej procedurze opisano sposób wykonywania funkcji dostosowanie strony interfejsu użytkownika przy użyciu zawartości HTML i arkuszy CSS próbki, która udostępniamy.

## <a name="get-an-azure-ad-b2c-tenant"></a>Uzyskiwanie dzierżawy usługi Azure AD B2C

Zanim cokolwiek można dostosować, jeśli nie masz jeszcze jedną będą potrzebne do [uzyskania dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md) .

## <a name="create-a-sign-up-or-sign-in-policy"></a>Tworzenie zasad zapisów lub logowania

Zawartość przykładowa udostępniamy może służyć do customze dwóch stron [zapisów lub logowania zasad](active-directory-b2c-reference-policies.md): [Strona logowania ujednolicony](active-directory-b2c-reference-ui-customization.md) i [samodzielnie potwierdzone atrybutów, strona](active-directory-b2c-reference-ui-customization.md). Podczas [tworzenia zasad zapisów lub logowania](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), Dodawanie konta lokalnego (adres e-mail), Facebook, Google i firmy Microsoft jako **dostawcy tożsamości**. Są to tylko IDPs, które akceptują nasze przykładowe zawartości HTML.  Można również dodać podzestaw te IDPs w razie potrzeby.

## <a name="register-an-application"></a>Zarejestruj się w aplikacji

Konieczne będzie do [rejestrowania aplikacji](active-directory-b2c-app-registration.md) w dzierżawie usługi B2C, który może służyć do wykonywania zasad. Po zarejestrowaniu aplikacji, masz kilka opcji, które umożliwia faktycznie uruchamianie zapisów zasad:

- Tworzenie jednej AD B2C Azure aplikacji szybki start wymienionych w sekcji "Wprowadzenie" [Logowanie w górę i zaloguj się konsumentów w aplikacjach](active-directory-b2c-overview.md#getting-started).
- Za pomocą wbudowanych aplikacji [Playground Azure AD B2C](https://aadb2cplayground.azurewebsites.net) . Jeśli zdecydujesz się na użycie playground, musisz rejestrowania aplikacji w dzierżawie B2C przy użyciu **identyfikatora URI przekierowywanie** `https://aadb2cplayground.azurewebsites.net/`.
- Przycisk **Uruchom teraz** na zasad w [Azure portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Dostosowywanie zasad

Aby dostosować wygląd i działanie zasad, musisz najpierw utworzyć pliki HTML i arkuszy CSS przy użyciu określonych konwencji Azure AD B2C. Możesz następnie przekazać zawartość statyczną publicznie dostępne miejsce, aby Azure AD B2C do niego dostęp. Może to być, serwer sieci web dedykowane, magazyn obiektów Blob platformy Azure, sieci dostarczania zawartości Azure lub innych statyczne hostingu zasobów dostawców. Tylko wymagania są, czy zawartość jest dostępny za pomocą protokołu HTTPS i można uzyskać do nich dostęp za pomocą CORS. Po został wystawiony statyczny zawartości w sieci web można edytować zasady, aby wskazywały do tej lokalizacji i prezentowanie zawartości do klientów. [Główny artykuł dostosowania interfejsu użytkownika](active-directory-b2c-reference-ui-customization.md) opisano szczegółowo, jak działa funkcja dostosowywania Azure AD B2C.

Na potrzeby tego samouczka możemy zostały już utworzone części zawartości próbki i hostowana go na magazyn obiektów Blob platformy Azure. Zawartość próbki jest bardzo podstawowe dostosowany w motywie fikcyjnej firmy "Firmy Wingtip zabawki". Aby wypróbować go na własne zasady, wykonaj następujące czynności:

1. Zaloguj się do Twojej dzierżawy w [Azure portal](https://portal.azure.com/) i przejdź do karta funkcje B2C.
2. Kliknij pozycję **zapisów lub logowania zasady** , a następnie kliknij pozycję zasad (na przykład "b2c\_1\_znak\_się\_logowania\_w").
3. Kliknij **dostosowywania strony interfejsu użytkownika** , a następnie **ujednolicony strony zapisów lub logowania**.
4. Przełącz przełącznik **niestandardowej strony** na wartość **Tak**. W polu **strony niestandardowe URI** wprowadź `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Kliknij **przycisk OK**.
5. Kliknij pozycję **Strona Zapisywanie do lokalnego konta**. Przełącz przełącznik **Użyj szablonu niestandardowego** na wartość **Tak**. W polu **strony niestandardowe URI** wprowadź `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Powtórz krok samej **strony zapisów konta społecznościowych**.
 Kliknij przycisk **OK** dwa razy, aby zamknąć karty dostosowania interfejsu użytkownika.
6. Kliknij przycisk **Zapisz**.

Teraz można wypróbować zasad niestandardowych. Można użyć własnej aplikacji lub playground Azure AD B2C, jeśli chcesz, ale możesz też po prostu kliknij polecenie **Uruchom teraz** karta zasady. W polu listy rozwijanej wybierz aplikację i wybierz odpowiednie przekierowania URI. Kliknij przycisk **Uruchom teraz** . Zostanie otwarta na nowej karcie przeglądarki i może zostać uruchomiony za pośrednictwem użytkownikom tworzący konto w aplikacji z nowej zawartości w miejscu!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Przekazać zawartość próbki magazynem obiektów Blob platformy Azure

Jeśli chcesz udostępnić zawartość strony za pomocą magazyn obiektów Blob platformy Azure, można utworzyć konta miejsca do magazynowania i przekaż pliki za pomocą naszego narzędzia Pomocnik B2C.

### <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowe** > **danych + miejsca do magazynowania** > **konta miejsca do magazynowania**. Konieczne będzie Azure subskrypcji, aby utworzyć konto magazyn obiektów Blob platformy Azure. Możesz zalogować się bezpłatną wersję próbną w [Azure witryny sieci Web](https://azure.microsoft.com/pricing/free-trial/).
3. Podaj **nazwę** konta magazynu (na przykład "contoso"), a następnie wybierz odpowiednie opcje dotyczące **ceny warstwa**, **Grupa zasobów** i **subskrypcji**. Upewnij się, że jest zaznaczone pole wyboru opcji **numer Pin, aby Startboard** . Kliknij przycisk **Utwórz**.
4. Wróć do Startboard i kliknij pozycję konto miejsca do magazynowania, która została właśnie utworzona.
5. W sekcji **Podsumowanie** kliknij **kontenery**, a następnie kliknij przycisk **+ Dodaj**.
6. Podaj **nazwę** dla kontenera (na przykład "b2c") i wybierz **Typ dostępu** **Blob** . Kliknij **przycisk OK**.
7. Kontener, w którym została utworzona pojawi się na liście Karta **obiektów blob** . Zanotuj adres URL kontenera; na przykład powinna wyglądać podobnie do `https://contoso.blob.core.windows.net/b2c`. Zamknij karta **obiektów blob** .
8. Na karta konta miejsca do magazynowania kliknij pozycję **kluczy** i zanotuj wartości pola **Klucza podstawowego dostępu** i **Nazwę konta magazynu** .

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowe** > **danych + miejsca do magazynowania** > **konta miejsca do magazynowania**. Konieczne będzie Azure subskrypcji, aby utworzyć konto magazyn obiektów Blob platformy Azure. Możesz zalogować się bezpłatną wersję próbną w [Azure witryny sieci Web](https://azure.microsoft.com/pricing/free-trial/).
3. Wybierz **Magazyn obiektów Blob** w obszarze **Typ konta**i pozostaw innych wartości jako domyślne.  W razie potrzeby możesz edytować grupa zasobów i lokalizacji.  Kliknij przycisk **Utwórz**.
4. Wróć do Startboard i kliknij pozycję konto miejsca do magazynowania, która została właśnie utworzona.
5. W sekcji **Podsumowanie** kliknij przycisk **+ kontener**.
6. Podaj **nazwę** dla kontenera (na przykład "b2c") i wybierz **Typ dostępu** **Blob** . Kliknij **przycisk OK**.
7. Otwórz kontener **Właściwości**i wpisz adres URL kontenera; na przykład powinna wyglądać podobnie do `https://contoso.blob.core.windows.net/b2c`. Zamknij karta kontener.
8. Na karta konta miejsca do magazynowania kliknij **Ikonę klucza** i zanotuj wartości pola **Klucza podstawowego dostępu** i **Nazwę konta magazynu** .

> [AZURE.NOTE]
    **Klucz podstawowy programu Access** jest ważne poświadczenie.

### <a name="download-the-helper-tool-and-sample-files"></a>Pobieranie plików pomocy narzędzia i przykładowe

Można pobrać z [magazynem obiektów Blob platformy Azure pomocnicze narzędzie i przykładowe pliki jako pliku zip](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) lub klonowanie go z GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

To repozytorium zawiera `sample_templates\wingtip` katalogu, który zawiera przykład HTML, CSS i obrazy. Dla tych szablonów, aby odwołać konta magazyn obiektów Blob platformy Azure będzie konieczne edytowania plików HTML. Otwórz `unified.html` i `selfasserted.html` i zamienić wszystkie wystąpienia `https://localhost` z adresem URL własnych kontenera zapisanym w poprzednich krokach. Należy użyć ścieżki plików HTML, ponieważ w tym przypadku kod HTML będzie udostępniania przez Azure AD w domenie `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Przekazywanie plików przykładowych

W tym samym repozytorium Rozpakuj plik `B2CAzureStorageClient.zip` i uruchamianie `B2CAzureStorageClient.exe` plików w. Ten program będzie po prostu przekazać wszystkie pliki w katalogu, w którym można określić do swojego konta miejsca do magazynowania i włączyć CORS dostęp do tych plików. Po wykonaniu powyższych kroków pliki HTML i arkuszy CSS będzie teraz wskazuje konta miejsca do magazynowania. Zauważ, że element, który poprzedza nazwę konta magazynu `blob.core.windows.net`; na przykład `contoso`. Można sprawdzić, czy zawartość została przekazana poprawnie Próbując uzyskać dostęp do `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` w przeglądarce. Aby upewnić się, że ich zawartość jest teraz CORS włączone również użyć [http://test-cors.org/](http://test-cors.org/) . (Poszukaj "stan XHR: 200" w wyniku.)

### <a name="customize-your-policy-again"></a>Dostosowywanie zasad, ponownie

Teraz, gdy zawartość przykładowa zostały przekazane do konta miejsca do magazynowania, możesz edytować zapisów zasad Aby odwołać się do. Powtórz kroki od poprzedniej sekcji ["Dostosowywanie zasad"](#customize-your-policy) , tym razem przy użyciu swojego konta miejsca do magazynowania adresy URL. Na przykład lokalizacji usługi `unified.html` plik będzie `<url-of-your-container>/wingtip/unified.html`.

Teraz można użyć przycisku **Uruchom teraz** lub własnej aplikacji, aby ponownie wykonać zasad. Wynik powinna wyglądać prawie dokładnie tak samo — możesz używać tych samych próbkach, HTML i arkuszy CSS w obu przypadkach. Jednak zasad odwołuje się teraz własne wystąpienie magazyn obiektów Blob platformy Azure i jest można edytować, a następnie przekaż pliki ponownie podczas należy bezpłatnie. Aby uzyskać więcej informacji o dostosowywaniu HTML i arkuszy CSS zapoznaj się z [główny artykuł dostosowania interfejsu użytkownika](active-directory-b2c-reference-ui-customization.md).
