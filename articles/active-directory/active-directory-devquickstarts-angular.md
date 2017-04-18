<properties
    pageTitle="Azure AD AngularJS wprowadzenie | Microsoft Azure"
    description="Sposoby tworzenia aplikacji kąta JS pojedynczej strony, której można zintegrować z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>Zabezpieczanie AngularJS pojedynczej strony aplikacji z usługą Azure Active Directory

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD ułatwia niezwykle proste umożliwiają dodawanie logowania w Wyloguj się i zabezpieczanie OAuth interfejsu API do aplikacji pojedynczej strony.  Umożliwia dodanie aplikacji do uwierzytelniania użytkowników z kont usługi Active Directory i używanie dowolnego interfejsu API chroniony przez Azure AD, takich jak interfejsów API usługi Office 365 lub interfejsu API Azure w sieci web.

W przypadku języka javascript aplikacje w przeglądarce Azure AD udostępnia Active Directory Authentication Library lub adal.js.  Osoby Adal.js wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikacji AngularJS listy zadań do wykonania którego:

- Zaloguje użytkownika do aplikacji, używając Azure AD jako dostawcy tożsamości.
- Wyświetla niektóre informacje o użytkowniku.
- Bezpieczne nawiąże połączenie z tej aplikacji do Do listy interfejsu API tokeny okaziciela z AAD.
- Podpisuje użytkownika się z aplikacji.

Aby utworzyć pełną aplikację roboczych, musisz:

2. Zarejestruj się w aplikacji z usługą Azure Active Directory.
3. ADAL zainstalować i skonfigurować bezpieczne uwierzytelnianie HASŁA.
5. Za pomocą ADAL bezpiecznego stron w bezpieczne uwierzytelnianie HASŁA.

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Konieczne będzie również dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji.  Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. rejestr aplikacji DirectorySearcher*
Aby włączyć aplikację do uwierzytelniania użytkowników i uzyskać tokenów, najpierw musisz go zarejestrować w dzierżawie usługi Azure AD:

-   Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com)
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych.
    -   **Przekierowywanie Uri** jest lokalizacji, do której AAD zwróci tokeny.  Aby ten przykład lokalizacja domyślna to`https://localhost:44326/`
-   Po zakończeniu rejestracji AAD przypisze aplikacji unikatowy **Identyfikator klienta**.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie **Konfigurowanie** .
- Adal.js używa przepływu niejawne OAuth można komunikować się z usługą Azure Active Directory.  Niejawne przepływu należy włączyć dla aplikacji:
    - Pobierz manifest aplikacji, klikając pozycję **Zarządzaj pojawiają**.
    - Otwórz manifest i odszukaj `oauth2AllowImplicitFlow` właściwości. Ustaw wartość `true`.
    - Zapisywanie i przekaż manifest aplikacji, ponownie klikając **Zarządzanie pojawiają** .

## <a name="2-install-adal--configure-the-spa"></a>*2. ADAL zainstalować i skonfigurować bezpieczne uwierzytelnianie HASŁA*
Teraz, gdy masz aplikację w Azure AD, można zainstalować adal.js i wpisz swój kod dotyczące tożsamości.

-   Rozpocznij, dodając adal.js do projektu TodoSPA przy użyciu konsoli Menedżera pakietów:
  - Pobierz [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) i dodaj go do `App/Scripts/` katalogu projektu.
  - Pobierz [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) i dodaj go do `App/Scripts/` katalogu projektu.
  - Ładowanie każdy skrypt przed upływem okresu `</body>` w `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Bezpieczne uwierzytelnianie HASŁA wewnętrznej bazy danych do Do listy interfejsu API Zaakceptuj tokenów z przeglądarki wewnętrznej bazy danych musi konfiguracji informacji na temat rejestracji aplikacji. W programie project TodoSPA, otwórz `web.config`.  Zamienianie wartości elementy znajdujące się w `<appSettings>` sekcji w celu odzwierciedlenia wartości wejściowych w Azure Portal.  Kod będzie odwoływać się do tych wartości przy każdym użyciu ADAL.
    -   `ida:Tenant` Jest domeną Twojej dzierżawy Azure AD contoso.onmicrosoft.com np.
    -   `ida:Audience` Musi być **Identyfikator klienta** aplikacji skopiowany z portalu.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. za pomocą ADAL bezpiecznego stron w bezpieczne uwierzytelnianie HASŁA*
Adal.js został utworzony do integracji z dostawcami i kierowanie http AngularJS umożliwia bezpieczne poszczególnych widoków w swojej bezpieczne uwierzytelnianie HASŁA.

- W `App/Scripts/app.js`, przesuń w adal.js module:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Teraz można zainicjować `adalProvider` z wartościami konfiguracji rejestracji aplikacji, także w `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Teraz, aby zabezpieczyć `TodoList` wyświetlać w aplikacji, tylko do jednego wiersza kodu konieczne jest – `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Teraz masz aplikację bezpiecznego pojedynczej strony z możliwością logowanie użytkowników i wydać żądania chronionego token okaziciela jego interfejsu API wewnętrznej bazy danych.  Gdy użytkownik kliknie `TodoList` łącza, adal.js automatycznie przekieruje do Azure AD do zalogowania się w razie potrzeby.  Ponadto adal.js automatycznie wstawi access_token do żądań ajax, które są wysyłane do wewnętrznej bazy danych aplikacji.  Powyższe jest od zera minimalne niezbędne do utworzenia bezpieczne uwierzytelnianie HASŁA z adal.js -, ale istnieje wiele innych funkcji, które są przydatne w źródła:

- Wyraźnie problem z logowaniem i wyloguj żądania można zdefiniować funkcje w kontrolerach, które wywołują adal.js.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Można także prezentowanie informacji o użytkownikach w interfejsie użytkownika aplikacji.  Usługa adal została już dodana do `userDataCtrl` kontroler, więc można uzyskać dostęp do `userInfo` obiekt w widoku skojarzonego `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Istnieje wiele scenariuszy, w których można sprawdzić, czy użytkownik jest zalogowany.  Można również użyć `userInfo` obiektu zebrać te informacje.  Na przykład w `index.html` można wyświetlić przycisk "Logowania" albo "Wyloguj" na podstawie stanu uwierzytelniania:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Gratulacje! Usługi Azure AD zintegrowanych aplikacji pojedynczej strony zostało zakończone.  Go można uwierzytelniania użytkowników, bezpieczne połączenie jej wewnętrznej bazy danych przy użyciu OAuth 2.0 i uzyskanie podstawowych informacji o użytkowniku.  Jeśli jeszcze tego nie zrobiono, należy teraz można wypełnić dzierżawy usługi z niektórych użytkowników.  Uruchamianie programu do Do listy bezpieczne uwierzytelnianie HASŁA, a następnie zaloguj się przy użyciu jednego z tych użytkowników.  Dodawanie zadań do użytkowników do listy, wyloguj się i zaloguj się ponownie.

Adal.js ułatwia wdrażanie wszystkich tych typowych funkcji tożsamości do aplikacji.  Trwa istotnych pracy dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych elementów.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Możesz teraz można przejść do dodatkowych scenariuszach.  Warto wypróbować:

[Połączenie CORS interfejsu API sieci Web z bezpieczne uwierzytelnianie HASŁA >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
