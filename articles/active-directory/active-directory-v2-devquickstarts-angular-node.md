<properties
    pageTitle="Wprowadzenie do AngularJS 2.0 Azure AD | Microsoft Azure"
    description="Jak utworzyć aplikację programu kąta JS pojedynczej strony, która znaki w użytkownicy z obu osobistego konta Microsoft i konta służbowego."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Dodawanie logowania do aplikacji pojedynczej strony AngularJS - NodeJS

W tym artykule dodamy Zaloguj się przy użyciu konta serwera Microsoft włączone do aplikacji AngularJS przy użyciu punktu końcowego usługi Azure Active Directory 2.0. punkt końcowy 2.0 — umożliwiają wykonywanie pojedynczy integracji w aplikacji i uwierzytelniania użytkowników z kontami osobistych i służbowych/szkolnych.

W tym przykładzie jest proste aplikacji pojedynczej strony listy zadań do wykonania, której są przechowywane zadania w wewnętrznej bazie danych interfejsu API usługi REST, napisana NodeJS i chronione za pomocą tokenów okaziciela OAuth z Azure AD.  Aplikacja AngularJS przy użyciu naszych Otwórz źródło biblioteki uwierzytelniania JavaScript [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) obsługiwać całego procesu logowania w i uzyskać tokenów do nawiązywania połączeń z interfejsu API usługi REST.  Takim samym wzorcem mogą być stosowane do uwierzytelniania innych interfejsów API pozostałych, takiego jak [Microsoft Graph](https://graph.microsoft.com) lub za pośrednictwem interfejsów API Menedżera zasobów Azure.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="download"></a>Plik do pobrania

Aby rozpocząć pracę, musisz pobrać i zainstalować [node.js](https://nodejs.org).  Następnie można klonowanie lub [pobrać](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) aplikację szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

Aplikacja szkielet obejmuje cały kod standardowy prostych aplikacji AngularJS, ale brakuje wszystkie części dotyczące tożsamości.  Jeśli nie chcesz, aby wykonać opisane czynności, możesz zamiast tego klonowanie lub [pobrać](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) ukończonej przykładowej.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Zarejestruj się w aplikacji

Najpierw utworzyć aplikację w [Portalu rejestracji aplikacji](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Dodawanie platformy **sieci Web** dla aplikacji.
- Wprowadź prawidłowy **Przekierowywać identyfikatora URI**. Domyślnie w tym przykładzie `http://localhost:8080`.
- Pozostaw pole wyboru **Zezwalaj na niejawne przepływ** włączone. 

Kopiowanie szczegółów **Identyfikator aplikacji** przydzielonego do aplikacji, musisz ją wkrótce. 

## <a name="install-adaljs"></a>Instalowanie adal.js
Aby rozpocząć, przejdź do programu project, możesz pobrać i zainstalować adal.js.  Jeśli masz zainstalowaną [bower](http://bower.io/) można uruchamiać tylko tego polecenia.  Wszelkie niezgodności wersji współzależności po prostu wybierz pozycję nowszej wersji.
```
bower install adal-angular#experimental
```

Ponadto można pobrać ręcznie [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) i [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Dodawanie obu plików `app/lib/adal-angular-experimental/dist` katalogu.

Teraz otworzyć projekt w edytorze tekstu Ulubione i ładowanie adal.js na końcu treści strony:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Konfigurowanie interfejsu API usługi REST

Podczas możemy konfigurowania rzeczy, umożliwia działało interfejsu API usługi REST wewnętrznej bazy danych.  W wierszu polecenia Zainstaluj wszystkie niezbędne opakowania, uruchamiając (Upewnij się, że jesteś w katalogu najwyższego poziomu projektu):

```
npm install
```

Teraz otworzyć `config.js` i zamienić `audience` wartość:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

Interfejsu API usługi REST użyje tej wartości do sprawdzania poprawności tokenów otrzymanej z kąta aplikacji na żądania AJAX.  Uwaga Aby tego prostego interfejsu API usługi REST są przechowywane dane w pamięci — dzięki raz, aby zatrzymać serwer, zostaną utracone wszystkie zadania utworzonego wcześniej.

To wszystko razem, gdy mamy zamiar poświęcają omawiane działania interfejsu API usługi REST.  Zachęcamy do poke w kodzie, ale jeśli chcesz dowiedzieć się więcej o zabezpieczaniu web interfejsy API z Azure AD, zapoznaj się z [tego artykułu](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Logowania użytkowników w
Nadszedł czas na pisanie kodu tożsamości.  Można już zauważyć, że ten adal.js zawiera dostawcy AngularJS, który jest odtwarzany właściwego kąta mechanizmy routingu.  Zacznij od dodania moduł adal aplikacji:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Teraz można zainicjować `adalProvider` przy użyciu swojego Identyfikatora aplikacji:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Doskonałe, adal.js zawiera teraz wszystkie informacje potrzebne do bezpiecznego aplikacji i logowanie użytkowników.  Aby wymusić logowania dla danej trasie w aplikacji, wszystkie, potrzebny jest jeden wiersz kodu:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Teraz, gdy użytkownik kliknie `TodoList` łącza, adal.js automatycznie przekieruje do Azure AD dla logowania w razie potrzeby.  Można także jawnie wysłać żądania logowania i wyrejestrowywania przez adal.js w kontrolerach:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Wyświetl informacje o użytkowniku
Teraz, gdy użytkownik jest zalogowany, prawdopodobnie musisz uzyskać dostęp do danych uwierzytelniania zalogować użytkownika w aplikacji.  Adal.js udostępnia informacje dla Ciebie w `userInfo` obiektu.  Aby uzyskać dostęp do tego obiektu w widoku, najpierw dodać adal.js do zakresu głównego kontrolera odpowiadające im:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Następnie można bezpośrednio rozwiązać `userInfo` obiekt w widoku: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Można również użyć `userInfo` obiektu w celu określenia, jeśli użytkownik jest zalogowany czy nie.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Połączenie interfejsu API usługi REST
Na koniec nadszedł czas, aby uzyskać kilka tokenów i połączeń interfejsu API usługi REST tworzenie, odczytywanie, aktualizowanie i usuwanie zadań.  Dobrze przypuszczenie co?  Nie musisz wykonać *czynność*.  Adal.js zostanie automatycznie zajmować się wprowadzenie, pamięci podręcznej i odświeżanie tokeny.  Go będzie również zajmować się dołączanie tych tokenów do wychodzących żądań AJAX, które wysyłasz do interfejsu API usługi REST.  

Jak dokładnie to działa? To wszystko dzięki magiczną z [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), dzięki czemu adal.js do przekształcenia przychodzących i wychodzących wiadomości http.  Ponadto adal.js przyjęto założenie, że żądań wysyłać do tej samej domeny zgodnie z okna należy używać tokeny przeznaczone do tej samej aplikacji jako aplikacja AngularJS.  Oto Dlaczego użyliśmy tej samej aplikacji w aplikacji kąta i interfejsu API usługi REST NodeJS.  Oczywiście można zmienić to zachowanie i określić adal.js, aby uzyskać tokenów dla innych interfejsów API pozostałych w razie potrzeby —, ale w tym scenariuszu prosty wykonasz ustawień domyślnych.

Oto fragment, który pokazuje, jak łatwo jest Wyślij zapytania z okaziciela tokenów z Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gratulacje!  Azure AD aplikacji zintegrowane pojedynczej strony zostało zakończone.  Zrealizuj, Ci się nagroda.  Go można uwierzytelniania użytkowników, bezpieczne połączenie jej wewnętrznej bazy danych przy użyciu funkcji OpenID połączenie interfejsu API usługi REST i uzyskanie podstawowych informacji o użytkowniku.  Okno obsługuje każdy użytkownik z osobistego Account Microsoft lub konta służbowego/szkolnego z usługi Azure Active Directory.  Wypróbuj aplikację, uruchamiając:

```
node server.js
```

W przeglądarce przejdź do `http://localhost:8080`.  Zaloguj się przy użyciu osobistego konta Microsoft lub konta służbowego/szkolnego.  Dodawanie zadań do listy zadań do wykonania przez użytkownika, a następnie wyloguj się.  Spróbuj zalogować się przy użyciu innego typu konta. W razie potrzeby dzierżawę Azure AD tworzenie użytkowników służbowych/szkolnych [Dowiedz się, jak uzyskać tutaj](active-directory-howto-tenant.md) (jest to bezpłatne).

Kontynuowanie nauki o punkt końcowy 2.0 — powrót do naszego [2.0 — przewodnik dewelopera](active-directory-appmodel-v2-overview.md).  Aby uzyskać dodatkowe zasoby zapoznaj się:

- [Azure próbki GitHub >>](https://github.com/Azure-Samples)
- [Azure AD przy przepełnieniu stos >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD dokumentację dotyczącą [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywać powiadomienia o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
