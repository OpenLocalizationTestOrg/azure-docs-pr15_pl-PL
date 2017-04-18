<properties
    pageTitle="Azure AD Cordova wprowadzenie | Microsoft Azure"
    description="Sposoby tworzenia aplikacji Cordova, w której można zintegrować z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integracja Azure AD przy użyciu Apache Cordova aplikacji

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova umożliwia opracowywanie aplikacji HTML5 i JavaScript, które można uruchamiać na urządzeniach przenośnych jako pełnoprawny natywnej aplikacji.
Azure AD można dodawać enterprise ocen uwierzytelniania możliwości aplikacji Cordova. Dzięki wtyczki Cordova zawijanie SDK natywnych Azure AD systemie iOS, Android, ze Sklepu Windows i Windows Phone, można zwiększyć, aplikacja obsługuje znak z kont użytkowników usługi AD, uzyskać dostęp do usługi Office 365 i interfejsu API Azure i nawet ochrony połączenia do własnych niestandardowych interfejs API sieci Web.

W tym samouczku użyjemy dodatku Apache Cordova dla Active Directory uwierzytelniania biblioteki (ADAL) aby poprawić prostych aplikacji następujące funkcje:

-   Wystarczy tylko kilka wierszy kodu uwierzytelnianie użytkownika w usłudze AD i uzyskać tokenu do nawiązywania połączeń z interfejsu API Azure AD wykresu.
-   Umożliwia token wywołania interfejsu API wykres do kwerendy tego katalogu i wyświetlić wyniki  
-   Korzystanie z pamięci podręcznej tokenów ADAL dla zminimalizowania monity o uwierzytelnienie dla użytkownika.

Aby to zrobić, musisz:

2. Zarejestrowanie aplikacji z usługą Azure Active Directory
2. Dodawanie kodu do aplikacji do tokenów żądanie
3. Dodawanie kodu w celu użycia tokenu dla kwerend interfejsu API wykresu i wyświetlić wyniki.
4. Tworzenie projektu wdrażania Cordova ze wszystkich platform, który chcesz przeanalizować i Cordova ADAL wtyczkę i przetestować rozwiązanie w emulatory.

## <a name="0--prerequisites"></a>*0. wymagania wstępne dotyczące*

Aby użyć tego samouczka, które mają być:

- Dzierżawę Azure AD miejsce, w którym mają konta z uprawnieniami opracowywania aplikacji
- Środowisko projektowania, skonfigurowany do używania Apache Cordova  

Jeśli masz zarówno już został skonfigurowany, przejdź bezpośrednio do kroku 1.

Jeśli nie masz dzierżawę Azure AD, możesz znaleźć [instrukcje na temat je uzyskać tutaj](active-directory-howto-tenant.md).

Jeśli nie masz Cordova Apache konfigurowanie na komputerze, zainstaluj następujące czynności:

- [Cyfra](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Polecenie Cordova](https://cordova.apache.org/) (można łatwo zainstalować za pomocą Menedżera pakietów NPM: `npm install -g cordova`)

Należy zauważyć, że te powinna działać zarówno na komputerze PC i Mac.

Wymagania wstępne dotyczące różnych dotyczy wszystkich platform docelowej.

- Tworzenie i uruchamianie komputera typu Tablet systemu Windows lub wersję aplikacji telefonu
    - [Visual Studio 2013 dla systemu Windows z aktualizacji 2 lub nowszy](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express lub inną wersją).
- Aby utworzyć i uruchomić dla systemu iOS
    -   Xcode 5.x lub nowszego. Pobierz go na http://developer.apple.com/downloads lub [Sklepu Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios sim](https://www.npmjs.org/package/ios-sim) — pozwala na uruchamianie aplikacji iOS w systemie iOS Simulator z poziomu wiersza polecenia (można łatwo zainstalować za pośrednictwem terminal: `npm install -g ios-sim`)

- Aby utworzyć i uruchomić aplikację dla systemu Android
    - Instalowanie [Języka Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) lub nowszy. Upewnij się, `JAVA_HOME` (środowiska zmienny) jest ustawiona poprawnie według JDK ścieżka instalacji (na przykład C:\Program Files\Java\jdk1.7.0_75).
    - Zainstaluj [Zestaw SDK systemu Android](http://developer.android.com/sdk/installing/index.html?pkg=tools) i dodawanie `<android-sdk-location>\tools` lokalizacji (na przykład C:\tools\Android\android-sdk\tools), aby usługi `PATH` środowiska zmiennych.
    - Otwórz Menedżera SDK systemu Android (na przykład za pośrednictwem terminal: `android`) i instalowanie
    - Zestaw SDK platformy *systemu android 5.0.1 (interfejsu API 21)*
    - *Narzędzia konstrukcyjne android SDK* wersji 19.1.0 lub nowszym
    - *Repozytorium android pomocy technicznej* (Dodatki)

  Android sdk nie zapewnia dowolne wystąpienie emulatora domyślne. Utwórz nową, uruchamiając `android avd` z terminal, a następnie pozycję *Utwórz...* , jeśli chcesz uruchomić aplikację Android emulatora. Zalecany *Poziom interfejsu Api* jest 19 lub nowszy, zobacz — AVD Manager (http://developer.android.com/tools/help/avd-manager.html) uzyskać więcej informacji o Android opcje emulatora i tworzenie.


## <a name="1--register-an-application-with-azure-ad"></a>*1. rejestrowania aplikacji z usługą Azure Active Directory*

Uwaga: ten __krok jest opcjonalny__. Samouczek opisane wcześniej ustanawianie wartości, które umożliwi Zobacz przykładowe w działaniu bez konieczności wykonywania dowolnej inicjowania obsługi administracyjnej w własnej dzierżawie. Jednak zaleca się wykonać ten krok i zapoznanie się z procesu, jak będzie wymagane, gdy użytkownik utworzy własnych aplikacji.

Azure AD będzie tylko wydać tokeny znane aplikacji. Zanim będzie można używać Azure AD z Twojej aplikacji, należy utworzyć wpis dla niego w dzierżawie.  Aby zarejestrować nowej aplikacji w dzierżawie usługi

-   Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com)
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w którym chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i tworzenie nowej **Natywnej aplikacji klienckiej** (mimo że Cordova aplikacje są HTML podstawie tworzenie aplikacji klientami tutaj tak `Native Client Application` musi być zaznaczona opcja; w przeciwnym razie aplikacja będzie działać).
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   **Przekierowywanie URI** jest identyfikator URI służy do zwracania tokeny do aplikacji. Wprowadź `http://MyDirectorySearcherApp`.

Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz wartość w następnej sekcji: znajdziesz na karcie **Konfigurowanie** aplikacji do nowo utworzonej.

Aby uruchomić `DirSearchClient Sample`, udzielić uprawnienia aplikacji nowo utworzony do kwerendy _API Azure AD wykresu_:
-   Na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji".  Na podstawie "Usługi Azure Active Directory" Dodaj uprawnienia **dostępu katalogu jako użytkownik podpisana w** obszarze **Delegowane uprawnienia**.  Spowoduje to włączenie aplikacji kwerendy API wykres dla użytkowników.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. klonowanie repozytorium aplikacji przykładowych wymagane dla samouczka*

Z powłoki lub wierszu polecenia wpisz następujące polecenie:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Tworzenie aplikacji Cordova*

Istnieje wiele sposobów tworzenia aplikacji Cordova. W tym samouczku użyjemy interfejsu wiersza polecenia Cordova (polecenie).
Z powłoki lub wierszu polecenia wpisz następujące polecenie:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Który spowoduje utworzenie struktura folderów i rusztowania dla projektu Cordova, kopiowaniu zawartości projektu starter w podfolderze ciągu "www".
Przechodzenie do nowego folderu DirSearchClient.

    cd .\DirSearchClient

Dodawanie listy sprawdzonej wtyczki niezbędne do wywoływanie interfejsu API wykresu.

     cordova plugin add cordova-plugin-whitelist

Następnie dodaj wszystkich platform, które mają być obsługiwani. Uzyskanie próbki pracy, należy wykonać co najmniej jedno z poniższych poleceń. Należy zauważyć, że nie będziesz w stanie w celu emulacji pól iOS w systemie Windows lub systemu Windows i Windows Phone na komputerze Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Ponadto możesz dodać ADAL dla wtyczki Cordova do projektu.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. Dodawanie kodu do uwierzytelniania użytkowników i uzyskać tokenów z AAD*

Aplikację, którą tworzysz w tym samouczku zapewni funkcję wyszukiwania katalogu kości b, miejsce, w którym użytkownik końcowy może wpisz alias każdego użytkownika w katalogu i wizualizowanie niektóre podstawowe atrybuty.  Projekt starter zawiera definicję podstawowy interfejs użytkownika aplikacji (w www/index.html) i rusztowania, który druty w górę zdarzenia podstawowe aplikacji cykli powiązań interfejsu użytkownika i powoduje wyświetlanie warunków logicznych (w www/js/index.js). Jedynym elementem osobę jest dodanie logiki wykonania zadań tożsamości.

Pierwszy element, który należy wykonać jest wprowadzenie w kodzie wartości Protocol (protokół), które są używane przez AAD identyfikowanie aplikacji i zasoby, które Określanie. Te wartości posłuży do utworzenia żądania tokenu później. Wstawianie wstawek poniżej na samej górze pliku index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

`redirectUri` i `clientId` wartości powinny być zgodne wartości opisem aplikacji w AAD. Można znaleźć osoby, na karcie Konfiguruj w portalu usługi Azure zgodnie z opisem w kroku 1 wcześniej w tym samouczku.
Uwaga: Jeśli użytkownik przyjął nie rejestruje nowej aplikacji w własnej dzierżawie, można po prostu wkleić powyższych wartości wstępnie skonfigurowane jest — umożliwiające użytkownikom uruchamianie przykładowy, jeśli zawsze należy utworzyć własną pozycję dla aplikacji są przeznaczone dla produkcji.

Następnie należy dodać kod rzeczywiste żądania tokenu. Wstaw poniższy fragment między `search `i `renderdata `definicje.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Przeanalizujmy danej funkcji, dzieląc go w jego dwie główne części.
W tym przykładzie jest przeznaczona do pracy z dowolną dzierżawy w przeciwieństwie do być powiązane jedna. Używa punkt końcowy "/ typowych", dzięki któremu użytkownicy będą wprowadzać żadnego z kont podczas uwierzytelniania, a kieruje żądanie do dzierżawy należy.
To pierwsza część metodę sprawdza ADAL pamięci podręcznej, aby sprawdzić, czy jest już przechowywane token — a jeśli istnieje, używanego dzierżaw, z której pochodzi ponownie inicjowania ADAL. Jest to konieczne uniknąć dodatkowe monity, jak użycie "/ typowych" zawsze powoduje prośbą o wprowadzenie nowego konta.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Druga część metody wykonuje żądanie tokewn pisane z wielkiej litery.
`acquireTokenSilentAsync` Metody z prośbą o ADAL zwraca token dla określonego zasobu bez pokazywania dowolny UX. Który może wystąpić, jeśli pamięć podręczną już tokenu dostępu odpowiednie przechowywane, czy istnieje token odświeżania, których można uzyskać nowy token dostępu bez shwoing dowolnego wiersza.
Jeśli próba która kończy się niepowodzeniem, możemy powrotu `acquireTokenAsync` -który będzie widoczny monity o służą do uwierzytelniania.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Teraz gdy mamy już tokenu, firma Microsoft na koniec wywołania interfejsu API wykresu i wykonywanie kwerendy wyszukiwania, które będą. Wstaw poniższy fragment pod `authenticate` definicji.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Pliki punktu początkowego podanych barebone interfejsu użytkownika do wprowadzania alias użytkownika w polu tekstowym. Ta metoda używa tej wartości, aby skonstruować kwerendę, połączyć go z token dostępu, wyślij go do wykresu i analizowanie wyników. Metoda renderData już w pliku punktu początkowego trwa istotnych mają być wizualizowane wyniki.

## <a name="4-run"></a>*4. Uruchom*
Aplikacja jest na końcu gotowe do uruchomienia! Operacyjnym go jest bardzo proste: po uruchomieniu aplikacji wpisz w polu tekstowym alias użytkownika chcesz wyszukać — a następnie kliknij przycisk. Zostanie wyświetlony monit dla uwierzytelniania. Po pomyślnym uwierzytelnieniu i pomyślnym wyszukiwaniu pojawi się atrybutami wyszukiwane użytkownika. Zostanie uruchomiony kolejnych wyszuka bez pokazywania dowolnego wiersza, dzięki obecności w pamięci podręcznej tokenu wcześniej nabyte.
Konkretne kroki uruchamiania aplikacji zależy od platformy.

####<a name="windows-10"></a>Windows 10:

   Tablet PC:`cordova run windows --archs=x64 -- --appx=uap`

   Komórka (wymaga urządzenie przenośne Windows10 podłączony do komputera):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Uwaga__: przy pierwszym uruchomieniu monit może pojawić się zalogować licencja Deweloper. Aby uzyskać więcej informacji, zobacz [licencji Deweloper](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1 i typu Tablet:

   `cordova run windows`

   __Uwaga__: przy pierwszym uruchomieniu monit może pojawić się zalogować licencji Deweloper. Aby uzyskać więcej informacji, zobacz [licencji Deweloper](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Aby uruchomić na urządzeniami:`cordova run windows --device -- --phone`

   Aby uruchomić emulatora domyślne:`cordova emulate windows -- --phone`

   Używanie `cordova run windows --list -- --phone` Aby wyświetlić wszystkie dostępne cele i `cordova run windows --target=<target_name> -- --phone` na uruchamianie aplikacji w określonego urządzenia lub emulatora (na przykład `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Aby uruchomić na urządzeniami:`cordova run android --device`

   Aby uruchomić emulatora domyślne:`cordova emulate android`

   __Uwaga__: Upewnij się, że został utworzony wystąpienie emulatora za pomocą *Menedżera AVD* jako jest wyświetlana w sekcji *wymagania wstępne* .

   Używanie `cordova run android --list` Aby wyświetlić wszystkie dostępne cele i `cordova run android --target=<target_name>` na uruchamianie aplikacji w określonego urządzenia lub emulatora (na przykład `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Aby uruchomić na urządzeniami:`cordova run ios --device`

   Aby uruchomić emulatora domyślne:`cordova emulate ios`

   __Uwaga__: Upewnij się, `ios-sim` pakiet zainstalowana do uruchamiania emulatora. Zobacz sekcję *wymagania wstępne* , aby uzyskać więcej informacji.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Używanie `cordova run --help` Zobacz Tworzenie dodatkowych i uruchomić opcje.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Możesz teraz można przejść do bardziej zaawansowanych scenariuszy (ok i bardziej interesujące).  Warto wypróbować:

[Zabezpieczanie Node.js interfejsu API sieci Web z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
