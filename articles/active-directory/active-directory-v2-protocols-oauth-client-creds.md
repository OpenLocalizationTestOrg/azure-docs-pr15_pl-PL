
<properties
    pageTitle="Azure AD 2.0 OAuth klienta poświadczenia przepływ | Microsoft Azure"
    description="Tworzenie aplikacje sieci web Azure AD stosowania protokołu uwierzytelniania OAuth 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Protokoły 2.0 — poświadczeń klienta 2.0 OAuth przepływu

[Udzielanie OAuth 2.0 poświadczeń klienta](http://tools.ietf.org/html/rfc6749#section-4.4), nazywane "bokami dwóch OAuth" może służyć dostęp do zasobów sieci web hostowanej przy użyciu tożsamości aplikacji.  Często służy do serwery interakcje, które musi uruchomić w tle bez bezpośredniego precense: użytkownik końcowy.  Aplikacje tego typu są często określane jako **demonów** lub **konta usługi**.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

W bardziej typowe trzy "bokami trzy OAuth," aplikacją kliencką udzielono uprawnień dostępu do zasobu w imieniu określonego użytkownika.  Uprawnienia są **delegowane** przez użytkownika z aplikacją, zwykle podczas procesu [zgodę](active-directory-v2-scopes.md) .  Jednak w procesie poświadczenia klienta są uprawnienia **bezpośrednio** do samej aplikacji.  Gdy aplikacja prezentuje tokenu do zasobu, zasób wymusza, że samej aplikacji ma zezwolenie na wykonywanie określonej akcji — nie, że niektóre użytkownik ma autoryzacji.

## <a name="protocol-diagram"></a>Diagram Protocol (protokół)
Przepływ poświadczeń całego klienta wygląda mniej więcej tak — poszczególne kroki opisane szczegółowo poniżej.

![Przepływ poświadczeń klienta](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Uzyskać bezpośredni 
Aplikacja będzie zazwyczaj otrzymuje bezpośredni zezwolenia na dostęp do zasobu na dwa sposoby: za pomocą listy kontroli dostępu zasób lub za pośrednictwem aplikacji przypisanie uprawnień w Azure AD.  Istnieje kilka sposobów, które zasobu można zezwolić na swoich klientów i każdego serwera zasobów mogą wybierz metodę, która najodpowiedniejszego jej stosowania.  Te dwie metody są najczęściej używane w Azure AD i reccommended dla klientów i zasoby, które chcesz wykonać przepływu poświadczeń klienta.

### <a name="access-control-lists"></a>Listy kontroli dostępu
Z dostawcą danego zasobu może wymusić wyboru autoryzacji opartej na liście aplikacji identyfikatorów zna i udziela pewnego określonego poziomu dostępu.  Kiedy zasobu odbiera tokenu od punktu końcowego 2.0, można dekodowanie tokenu i wyodrębnianie identyfikator aplikacji klienta z `appid` i `iss` roszczeń.  Następnie można porównać, że aplikacje przed niektóre listy kontroli dostępu (ACL) go przechowuje.  Szczegółowości i metoda listy kontroli dostępu różni się znacznie od zasobów do zasobu.

Typowe przypadków użycia dla tych list ACL jest testowanie uczestników dla aplikacji sieci web lub interfejsu api w sieci web.  Sieci web interfejsu api mogą tylko przyznać podzbiór jej uprawnienia Pełna klientom różne.  Aby uruchomić testy kompleksowego api, klient testowy zostanie utworzony, ale uzyskuje tokenów z punkt końcowy 2.0 i wysyła je do interfejsu api.  Interfejs api można następnie ACL identyfikator aplikacji klienckich test dla pełnego dostępu do funkcji całego interfejsu api.  Należy zauważyć, że jeśli masz listę od usługi, należy sprawdzić nie tylko do osoby wywołującej `appid`, ale również sprawdzić, czy `iss` tokenu jest zaufany także.

Zezwolenie na tego typu są często demonów i konta usług, które muszą uzyskiwać dostęp do danych, należącą do użytkowników dla klientów indywidualnych z osobistego konta Microsoft.  Dla danych, należącą do organizacji zaleca się uzyskanie pełnomocnictwo za pośrednictwem aplikacji perimssions.

### <a name="application-permissions"></a>Uprawnienia aplikacji
Zamiast używać ACL, interfejsy API można uwidaczniają zestaw **uprawnienia aplikacji** , które mogą być przyznane do aplikacji.  Uprawnienie aplikacji udzielono aplikacji przez administratora organizacji i można używać tylko dostęp do danych, należącą do organizacji i jego pracownicy.  Na przykład programu Microsoft Graph udostępnia kilka uprawnienia aplikacji:

- Czytanie poczty w wszystkich skrzynek pocztowych
- Czytanie i wysyłanie poczty w wszystkich skrzynek pocztowych
- Wysyłanie poczty jako dowolnego użytkownika
- Odczytywanie danych katalogu
- [+ informacje dodatkowe](https://graph.microsoft.io)

Aby można było korzystać z tych uprawnień w aplikacji, można wykonywać następujące czynności.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Zażądaj uprawnień w portalu rejestracji aplikacji

- Przejdź do aplikacji w [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)lub [Tworzenie aplikacji](active-directory-v2-app-registration.md) , jeśli jeszcze tego nie zrobiono.  Konieczne będzie zapewnienie utworzony w aplikacji co najmniej jeden hasło aplikacji.
- Odszukaj sekcję **Bezpośrednie uprawnienia aplikacji** i Dodaj uprawnienia, które wymagają aplikacji.
- Upewnij się, aby **zapisać** rejestracji aplikacji

#### <a name="recommended-sign-the-user-into-your-app"></a>Zalecane: zalogowanie się użytkownika do aplikacji

Zazwyczaj podczas tworzenia aplikacji, które są stosowane uprawnienia aplikacji, aplikacja muszą mieć strony i widoku, który umożliwia administrator, aby zatwierdzić uprawnienia tej aplikacji.  Ta strona może być częścią aplikacji przepływu zapisów, część ustawień aplikacji lub dedykowane przepływu "łączenie".  W większości przypadków jest to właściwe rozwiązanie dla aplikacji wyświetlić to "łączenie" widoku tylko wtedy, gdy użytkownik jest zalogowany przy użyciu służbowe konto Microsoft.

Logowanie się użytkownika do aplikacji umożliwia identyfikację organziation, do której należy użytkownik przed prosząc o zatwierdzanie uprawnienia dostępu do aplikacji.  Gdy nie niezbędne, ułatwiają tworzenie bardziej intuicyjny obsługi dla użytkowników organizacji.  Aby wylogować użytkownika, wykonaj naszych [samouczki Protocol (protokół) 2.0](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Poproś uprawnienia administratora usługi katalogowej

Po zakończeniu uprawnieniom żądania od administratora firmy, można przekierowywać użytkownika 2.0 — **Administrator zgodę punktu końcowego**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | Dzierżawy katalogu, który ma być zażądaj uprawnień z.  Może być udostępniana w formacie przyjazną nazwę lub identyfikator guid.  Jeśli nie wiesz, które dzierżawy należy użytkownik i chcesz umożliwić jej Zaloguj się przy użyciu dowolnego dzierżawy, za pomocą `common`. |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| redirect_uri | Wymagane | Redirect_uri, w którym chcesz wstawić odpowiedź wysłany w celu aplikacji do obsługi.  Dokładnie musi odpowiadać jeden z redirect_uris, zarejestrowana w portalu, z wyjątkiem musi być zakodowany w adresie url i mogą mieć dodatkowe segmentów. |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Stan jest używany do kodowania informacji na temat stanu użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |

W tym momencie Azure AD wymusić, że administrator dzierżawy zalogować się do wykonania żądania.  Administrator zostanie poproszony o zatwierdzać wszystkie uprawnienia bezpośredniego stosowania, które żądanej aplikacji w portalu rejestracji. 

##### <a name="successful-response"></a>Pomyślną odpowiedź
Jeśli administrator zatwierdzi uprawnienia dla aplikacji, będzie pomyślną odpowiedź:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametr | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Dzierżawy katalogu, która przyznała uprawnienia aplikacji go wymagane, w formacie guid. |
| Województwo | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Stan jest używany do kodowania informacji na temat stanu użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| admin_consent | Zostanie ustawiona na `True`. |


##### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Jeśli administrator nie zatwierdzić uprawnienia dla aplikacji, będzie odpowiedzi nie powiodło się:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Opis |
| ----------------------- | ------------------------------- | --------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu.  |

Po odebraniu odpowiedzi pomyślnego z aplikacji inicjowania obsługi administracyjnej punktu końcowego, aplikacji uzyskanie go wymagane uprawnienia bezpośredniego stosowania.  Teraz można przenieść na żądanie tokenu do żądanego zasobu.

## <a name="get-a-token"></a>Uzyskiwanie tokenu
Po pełnomocnictwo zostało nabyte aplikacji, można kontynuować pobieranie tokeny dostępu dla interfejsów API.  Aby uzyskać tokenu przy użyciu klienta udzielanie poświadczeń, Wyślij żądanie WPIS `/token` punktu końcowego 2.0:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| zakres | Wymagane | Wartość przekazywana `scope` parametr w tym wezwaniu powinien być identyfikator zasobu (aplikacji identyfikator URI) z zasobem, umieszczone z `.default` sufiks.  Aby na przykład programu Microsoft Graph, podane, wartość powinna być `https://graph.microsoft.com/.default`.  Tej wartości informuje punkt końcowy 2.0, że wszystkie bezpośredniego stosowania uprawnień, które zostały skonfigurowane dla aplikacji, jej powinny wydawać token dla nich odnoszących się do żądanego zasobu. |
| client_secret | Wymagane | Tajny aplikacji, który wygenerował w portalu rejestracji dla aplikacji sieci. |
| grant_type | Wymagane | Musi być `client_credentials`. | 

#### <a name="successful-response"></a>Pomyślną odpowiedź
Pomyślną odpowiedź będzie miał postać:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token dostępu wymagane. Aplikacja umożliwia ten token uwierzytelniania zabezpieczone zasobu, takiego jak interfejsu API sieci web. |
| token_type | Wskazuje typ tokenu wartość. Tylko typ, który jest obsługa Azure AD `Bearer`.  |
| expires_in | Jak długo trwa token dostępu jest prawidłowy (w sekundach). |

#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Odpowiedź o błędzie będzie miał postać:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu może służyć do klasyfikowania typów błędów występujących, która umożliwia reagowanie na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |
| error_codes | Lista kodów błędów określonych usługi STS, które mogą ułatwić diagnostyki.  |
| Sygnatura czasowa | Czas, w którym wystąpił błąd. |
| trace_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne.  |
| correlation_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne elementów. |

## <a name="use-a-token"></a>Za pomocą tokenu
Teraz, gdy zostało nabyte tokenu, umożliwia token Tworzenie żądania do zasobu.  Po wygaśnięciu tokenu, po prostu ponownie żądanie `/token` punktu końcowego umożliwia uzyskanie tokenu świeży dostępu.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Przykładowy kod
Aby zobaczyć przykład aplikacji że narzędzi client_credentials udzielić przy użyciu administrator zgodę punktu końcowego, skorzystaj z [Przykładowy kod demon 2.0](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
