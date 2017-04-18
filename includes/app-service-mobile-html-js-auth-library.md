###<a name="server-auth"></a>Jak: uwierzytelniania przez dostawcę (serwer płynu)

Aby aplikacje Mobile Zarządzanie procesem uwierzytelniania w aplikacji, musisz zarejestrować aplikacji u dostawcy tożsamości. W usłudze Azure aplikacji należy skonfigurować identyfikator aplikacji i hasło dostarczony przez dostawcę.
Aby uzyskać więcej informacji zobacz samouczek [Dodaj uwierzytelniania aplikacji].

Po zarejestrowaniu dostawcy tożsamości, po prostu wywołać metodę .login() z nazwą swojego dostawcy. Na przykład, aby zalogować się przy użyciu Facebook poniższy kod.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, zmień wartość przekazywana do powyższej metody logowania do jednej z następujących czynności: `microsoftaccount`, `facebook`, `twitter`, `google`, lub `aad`.

W tym przypadku Azure aplikacji usługi zarządza przepływ uwierzytelniania OAuth 2.0 wyświetlając stronę logowania zaznaczonego dostawcę i generowanie token uwierzytelniania aplikacji usługi po pomyślne logowanie z dostawcą tożsamości. Funkcja logowania po zakończeniu zwraca obiekt JSON (użytkownika), który udostępnia identyfikator użytkownika i aplikacji usługi token uwierzytelniania w polach Nazwa użytkownika i authenticationToken odpowiednio. Ten token można przechowywanych w pamięci podręcznej i ponownego użycia do momentu jej wygaśnięcia.

###<a name="client-auth"></a>Jak: uwierzytelniania przez dostawcę (przepływ klienta)

Aplikacji można również niezależne skontaktuj się z dostawcą tożsamości, a następnie wprowadź token zwracane do usługi aplikacji, uwierzytelniania. Ten przepływ klienta umożliwia zapewnienie pojedynczego obsługi logowania użytkowników lub do pobierania danych użytkowników z dostawcy tożsamości.

#### <a name="social-authentication-basic-example"></a>Przykład podstawowej społecznościowych uwierzytelniania

W tym przykładzie użyto klienta Facebook SDK uwierzytelniania:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
W tym przykładzie przyjęto założenie, że token dostarczony przez dostawcę odpowiednich SDK jest przechowywany w token zmiennej.

#### <a name="microsoft-account-example"></a>Przykład Account Microsoft

W poniższym przykładzie użyto Live SDK, który obsługuje jednokrotnej dla aplikacji ze Sklepu Windows przy użyciu programu Microsoft Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

W tym przykładzie pobiera tokenu z Live połączyć, którego jest dostarczany z usługą aplikacji przez wywoływania funkcji logowania.

###<a name="auth-getinfo"></a>Jak: uzyskiwanie informacji o uwierzytelniony użytkownik

Informacje uwierzytelniania dla bieżącego użytkownika można pobrać z `/.auth/me` metodą AJAX punktu końcowego.  Upewnij się, możesz ustawić `X-ZUMO-AUTH` nagłówek, aby token uwierzytelniania.  Token uwierzytelniania znajduje się w `client.currentUser.mobileServiceAuthenticationToken`.  Na przykład, aby użyć pobierania interfejsu API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Zdalny dostęp jest dostępna jako pakiet npm lub pobrać przeglądarki z CDNJS. Można także użyć jQuery lub inny AJAX interfejsu API do pobierania danych.  Dane zostaną odebrane jako obiekt JSON.
